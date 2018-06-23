---
title: Bezpieczne przechowywanie kluczy tajnych aplikacji w rozwoju platformy ASP.NET Core
author: rick-anderson
description: Sposób przechowywania i pobierania informacji poufnych jako klucze tajne aplikacji podczas tworzenia aplikacji platformy ASP.NET Core.
ms.author: scaddie
ms.custom: mvc
ms.date: 06/21/2018
uid: security/app-secrets
ms.openlocfilehash: d3b2de1a17012986ef8dea7aaf8636dd35d10fa1
ms.sourcegitcommit: e22097b84d26a812cd1380a6b2d12c93e522c125
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/22/2018
ms.locfileid: "36314178"
---
# <a name="safe-storage-of-app-secrets-in-development-in-aspnet-core"></a><span data-ttu-id="2bdfb-103">Bezpieczne przechowywanie kluczy tajnych aplikacji w rozwoju platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="2bdfb-103">Safe storage of app secrets in development in ASP.NET Core</span></span>

<span data-ttu-id="2bdfb-104">Przez [Rick Anderson](https://twitter.com/RickAndMSFT), [Roth Danielowi](https://github.com/danroth27), i [Scott Addie](https://github.com/scottaddie)</span><span class="sxs-lookup"><span data-stu-id="2bdfb-104">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Daniel Roth](https://github.com/danroth27), and [Scott Addie](https://github.com/scottaddie)</span></span>

<span data-ttu-id="2bdfb-105">[Wyświetlić lub pobrać przykładowy kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/app-secrets/samples) ([sposobu pobierania](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="2bdfb-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/app-secrets/samples) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="2bdfb-106">W tym dokumencie opisano techniki do przechowywania i pobierania danych poufnych podczas tworzenia aplikacji platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="2bdfb-106">This document explains techniques for storing and retrieving sensitive data during the development of an ASP.NET Core app.</span></span> <span data-ttu-id="2bdfb-107">W kodzie źródłowym nigdy nie należy przechowywać haseł i innych poufnych danych, a nie powinien użyć kluczy tajnych produkcji w rozwoju lub testowania trybu.</span><span class="sxs-lookup"><span data-stu-id="2bdfb-107">You should never store passwords or other sensitive data in source code, and you shouldn't use production secrets in development or test mode.</span></span> <span data-ttu-id="2bdfb-108">Można przechowywać i chronić Azure kluczy tajnych testowych i produkcyjnych z [dostawcę konfiguracji usługi Azure Key Vault](xref:security/key-vault-configuration).</span><span class="sxs-lookup"><span data-stu-id="2bdfb-108">You can store and protect Azure test and production secrets with the [Azure Key Vault configuration provider](xref:security/key-vault-configuration).</span></span>

## <a name="environment-variables"></a><span data-ttu-id="2bdfb-109">Zmienne środowiskowe</span><span class="sxs-lookup"><span data-stu-id="2bdfb-109">Environment variables</span></span>

<span data-ttu-id="2bdfb-110">Zmienne środowiskowe są używane do Rezygnacja z zapisywania haseł aplikacji, kodu lub lokalne pliki konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="2bdfb-110">Environment variables are used to avoid storage of app secrets in code or in local configuration files.</span></span> <span data-ttu-id="2bdfb-111">Zmienne środowiskowe przesłaniają wartości konfiguracji dla wszystkich źródeł wcześniej określonej konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="2bdfb-111">Environment variables override configuration values for all previously specified configuration sources.</span></span>

::: moniker range="<= aspnetcore-1.1"
<span data-ttu-id="2bdfb-112">Konfigurowanie przy odczycie wartości zmiennych środowiskowych, wywołując [AddEnvironmentVariables](/dotnet/api/microsoft.extensions.configuration.environmentvariablesextensions.addenvironmentvariables) w `Startup` konstruktora:</span><span class="sxs-lookup"><span data-stu-id="2bdfb-112">Configure the reading of environment variable values by calling [AddEnvironmentVariables](/dotnet/api/microsoft.extensions.configuration.environmentvariablesextensions.addenvironmentvariables) in the `Startup` constructor:</span></span>

<span data-ttu-id="2bdfb-113">[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup.cs?name=snippet_StartupConstructor&highlight=10)]</span><span class="sxs-lookup"><span data-stu-id="2bdfb-113">[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup.cs?name=snippet_StartupConstructor&highlight=10)]</span></span>
::: moniker-end

<span data-ttu-id="2bdfb-114">Należy wziąć pod uwagę aplikacji sieci web platformy ASP.NET Core, w którym **indywidualnych kont użytkowników** zabezpieczeń jest włączone.</span><span class="sxs-lookup"><span data-stu-id="2bdfb-114">Consider an ASP.NET Core web app in which **Individual User Accounts** security is enabled.</span></span> <span data-ttu-id="2bdfb-115">Domyślny ciąg połączenia bazy danych jest dołączony do projektu *appsettings.json* pliku z kluczem `DefaultConnection`.</span><span class="sxs-lookup"><span data-stu-id="2bdfb-115">A default database connection string is included in the project's *appsettings.json* file with the key `DefaultConnection`.</span></span> <span data-ttu-id="2bdfb-116">Domyślny ciąg połączenia jest dla bazy danych LocalDB, która działa w trybie użytkownika i nie wymaga hasła.</span><span class="sxs-lookup"><span data-stu-id="2bdfb-116">The default connection string is for LocalDB, which runs in user mode and doesn't require a password.</span></span> <span data-ttu-id="2bdfb-117">Podczas wdrażania aplikacji `DefaultConnection` wartość klucza może zostać zastąpiona przez wartość zmiennej środowiskowej.</span><span class="sxs-lookup"><span data-stu-id="2bdfb-117">During app deployment, the `DefaultConnection` key value can be overridden with an environment variable's value.</span></span> <span data-ttu-id="2bdfb-118">Zmienna środowiskowa może przechowywać parametry połączenia ukończone z poświadczeniami poufnych.</span><span class="sxs-lookup"><span data-stu-id="2bdfb-118">The environment variable may store the complete connection string with sensitive credentials.</span></span>

> [!WARNING]
> <span data-ttu-id="2bdfb-119">Zmienne środowiskowe są zazwyczaj przechowywane w zwykły tekst niezaszyfrowane.</span><span class="sxs-lookup"><span data-stu-id="2bdfb-119">Environment variables are generally stored in plain, unencrypted text.</span></span> <span data-ttu-id="2bdfb-120">W przypadku naruszenia zabezpieczeń komputera lub proces, zmienne środowiskowe są dostępne przez niezaufane.</span><span class="sxs-lookup"><span data-stu-id="2bdfb-120">If the machine or process is compromised, environment variables can be accessed by untrusted parties.</span></span> <span data-ttu-id="2bdfb-121">Może być wymagane dodatkowe środki, aby zapobiec ujawnieniu hasła użytkownika.</span><span class="sxs-lookup"><span data-stu-id="2bdfb-121">Additional measures to prevent disclosure of user secrets may be required.</span></span>

## <a name="secret-manager"></a><span data-ttu-id="2bdfb-122">Menedżer klucz tajny</span><span class="sxs-lookup"><span data-stu-id="2bdfb-122">Secret Manager</span></span>

<span data-ttu-id="2bdfb-123">Narzędzie Menedżer klucz tajny są przechowywane poufne dane podczas tworzenia projektu platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="2bdfb-123">The Secret Manager tool stores sensitive data during the development of an ASP.NET Core project.</span></span> <span data-ttu-id="2bdfb-124">W tym kontekście element danych poufnych jest klucz tajny aplikacji.</span><span class="sxs-lookup"><span data-stu-id="2bdfb-124">In this context, a piece of sensitive data is an app secret.</span></span> <span data-ttu-id="2bdfb-125">Klucze tajne aplikacji są przechowywane w innej lokalizacji niż drzewa projektu.</span><span class="sxs-lookup"><span data-stu-id="2bdfb-125">App secrets are stored in a separate location from the project tree.</span></span> <span data-ttu-id="2bdfb-126">Klucze tajne aplikacji są skojarzone z określonego projektu lub współużytkowane przez kilka projektów.</span><span class="sxs-lookup"><span data-stu-id="2bdfb-126">The app secrets are associated with a specific project or shared across several projects.</span></span> <span data-ttu-id="2bdfb-127">Klucze tajne aplikacji nie jest wyewidencjonowany do kontroli źródła.</span><span class="sxs-lookup"><span data-stu-id="2bdfb-127">The app secrets aren't checked into source control.</span></span>

> [!WARNING]
> <span data-ttu-id="2bdfb-128">Narzędzie Menedżer klucz tajny nie Szyfruj przechowywane klucze tajne i nie powinny być traktowane jako zaufanego magazynu.</span><span class="sxs-lookup"><span data-stu-id="2bdfb-128">The Secret Manager tool doesn't encrypt the stored secrets and shouldn't be treated as a trusted store.</span></span> <span data-ttu-id="2bdfb-129">Jest tylko do celów programistycznych.</span><span class="sxs-lookup"><span data-stu-id="2bdfb-129">It's for development purposes only.</span></span> <span data-ttu-id="2bdfb-130">Klucze i wartości są przechowywane w pliku konfiguracji JSON w katalogu profilu użytkownika.</span><span class="sxs-lookup"><span data-stu-id="2bdfb-130">The keys and values are stored in a JSON configuration file in the user profile directory.</span></span>

## <a name="how-the-secret-manager-tool-works"></a><span data-ttu-id="2bdfb-131">Jak działa narzędzie Menedżer klucz tajny</span><span class="sxs-lookup"><span data-stu-id="2bdfb-131">How the Secret Manager tool works</span></span>

<span data-ttu-id="2bdfb-132">Narzędzie Menedżer klucz tajny optymalizacji abstracts szczegóły implementacji, takich jak jak i gdzie są przechowywane wartości.</span><span class="sxs-lookup"><span data-stu-id="2bdfb-132">The Secret Manager tool abstracts away the implementation details, such as where and how the values are stored.</span></span> <span data-ttu-id="2bdfb-133">Można użyć narzędzia bez uprzedniego uzyskania informacji o tych szczegóły implementacji.</span><span class="sxs-lookup"><span data-stu-id="2bdfb-133">You can use the tool without knowing these implementation details.</span></span> <span data-ttu-id="2bdfb-134">Wartości są przechowywane w pliku konfiguracji JSON w folderze profilu użytkownika chronionych przez system na komputerze lokalnym:</span><span class="sxs-lookup"><span data-stu-id="2bdfb-134">The values are stored in a JSON configuration file in a system-protected user profile folder on the local machine:</span></span>

# <a name="windowstabwindows"></a>[<span data-ttu-id="2bdfb-135">Windows</span><span class="sxs-lookup"><span data-stu-id="2bdfb-135">Windows</span></span>](#tab/windows)

<span data-ttu-id="2bdfb-136">Ścieżka systemu plików:</span><span class="sxs-lookup"><span data-stu-id="2bdfb-136">File system path:</span></span>

`%APPDATA%\Microsoft\UserSecrets\<user_secrets_id>\secrets.json`

# <a name="macostabmacos"></a>[<span data-ttu-id="2bdfb-137">macOS</span><span class="sxs-lookup"><span data-stu-id="2bdfb-137">macOS</span></span>](#tab/macos)

<span data-ttu-id="2bdfb-138">Ścieżka systemu plików:</span><span class="sxs-lookup"><span data-stu-id="2bdfb-138">File system path:</span></span>

`~/.microsoft/usersecrets/<user_secrets_id>/secrets.json`

# <a name="linuxtablinux"></a>[<span data-ttu-id="2bdfb-139">Linux</span><span class="sxs-lookup"><span data-stu-id="2bdfb-139">Linux</span></span>](#tab/linux)

<span data-ttu-id="2bdfb-140">Ścieżka systemu plików:</span><span class="sxs-lookup"><span data-stu-id="2bdfb-140">File system path:</span></span>

`~/.microsoft/usersecrets/<user_secrets_id>/secrets.json`

---

<span data-ttu-id="2bdfb-141">W poprzednim ścieżki pliku, Zastąp `<user_secrets_id>` z `UserSecretsId` wartości określonej w *.csproj* pliku.</span><span class="sxs-lookup"><span data-stu-id="2bdfb-141">In the preceding file paths, replace `<user_secrets_id>` with the `UserSecretsId` value specified in the *.csproj* file.</span></span>

<span data-ttu-id="2bdfb-142">Nie pisania kodu, który jest zależny od lokalizacji lub format dane zapisane za pomocą narzędzia menedżera klucz tajny.</span><span class="sxs-lookup"><span data-stu-id="2bdfb-142">Don't write code that depends on the location or format of data saved with the Secret Manager tool.</span></span> <span data-ttu-id="2bdfb-143">Te szczegóły implementacji mogą ulec zmianie.</span><span class="sxs-lookup"><span data-stu-id="2bdfb-143">These implementation details may change.</span></span> <span data-ttu-id="2bdfb-144">Na przykład tajny wartości nie są zaszyfrowane, ale może być w przyszłości.</span><span class="sxs-lookup"><span data-stu-id="2bdfb-144">For example, the secret values aren't encrypted, but could be in the future.</span></span>

::: moniker range="<= aspnetcore-2.0"
## <a name="install-the-secret-manager-tool"></a><span data-ttu-id="2bdfb-145">Zainstaluj narzędzie Menedżer klucz tajny</span><span class="sxs-lookup"><span data-stu-id="2bdfb-145">Install the Secret Manager tool</span></span>

<span data-ttu-id="2bdfb-146">Narzędzie Menedżer klucz tajny jest dołączany .NET Core interfejsu wiersza polecenia począwszy od zestawu SDK programu .NET Core 2.1.300.</span><span class="sxs-lookup"><span data-stu-id="2bdfb-146">The Secret Manager tool is bundled with the .NET Core CLI as of .NET Core SDK 2.1.300.</span></span> <span data-ttu-id="2bdfb-147">W przypadku wersji zestawu SDK programu .NET Core przed 2.1.300 niezbędne jest instalację narzędzia.</span><span class="sxs-lookup"><span data-stu-id="2bdfb-147">For .NET Core SDK versions before 2.1.300, tool installation is necessary.</span></span>

> [!TIP]
> <span data-ttu-id="2bdfb-148">Uruchom `dotnet --version` z powłoki poleceń, aby wyświetlić numer wersji zainstalowanego zestawu SDK programu .NET Core.</span><span class="sxs-lookup"><span data-stu-id="2bdfb-148">Run `dotnet --version` from a command shell to see the installed .NET Core SDK version number.</span></span>

<span data-ttu-id="2bdfb-149">Zostanie wyświetlone ostrzeżenie, jeśli używany zestaw .NET Core SDK zawiera narzędzia:</span><span class="sxs-lookup"><span data-stu-id="2bdfb-149">A warning is displayed if the .NET Core SDK being used includes the tool:</span></span>

```console
The tool 'Microsoft.Extensions.SecretManager.Tools' is now included in the .NET Core SDK. Information on resolving this warning is available at (https://aka.ms/dotnetclitools-in-box).
```

<span data-ttu-id="2bdfb-150">Zainstaluj [Microsoft.Extensions.SecretManager.Tools](https://www.nuget.org/packages/Microsoft.Extensions.SecretManager.Tools/) pakietu NuGet w projekcie platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="2bdfb-150">Install the [Microsoft.Extensions.SecretManager.Tools](https://www.nuget.org/packages/Microsoft.Extensions.SecretManager.Tools/) NuGet package in your ASP.NET Core project.</span></span> <span data-ttu-id="2bdfb-151">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="2bdfb-151">For example:</span></span>

<span data-ttu-id="2bdfb-152">[!code-xml[](app-secrets/samples/1.x/UserSecrets/UserSecrets.csproj?name=snippet_CsprojFile&highlight=13-14)]</span><span class="sxs-lookup"><span data-stu-id="2bdfb-152">[!code-xml[](app-secrets/samples/1.x/UserSecrets/UserSecrets.csproj?name=snippet_CsprojFile&highlight=13-14)]</span></span>

<span data-ttu-id="2bdfb-153">Uruchom następujące polecenie w powłoce poleceń, aby sprawdzić poprawność instalacji narzędzia:</span><span class="sxs-lookup"><span data-stu-id="2bdfb-153">Execute the following command in a command shell to validate the tool installation:</span></span>

```console
dotnet user-secrets -h
```

<span data-ttu-id="2bdfb-154">Narzędzie Menedżer klucz tajny zawiera przykładowe zastosowanie, opcje i pomoc dla polecenia:</span><span class="sxs-lookup"><span data-stu-id="2bdfb-154">The Secret Manager tool displays sample usage, options, and command help:</span></span>

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
> <span data-ttu-id="2bdfb-155">Musi być w tym samym katalogu co *.csproj* uruchamianie narzędzia zdefiniowane w pliku *.csproj* pliku `DotNetCliToolReference` elementów.</span><span class="sxs-lookup"><span data-stu-id="2bdfb-155">You must be in the same directory as the *.csproj* file to run tools defined in the *.csproj* file's `DotNetCliToolReference` elements.</span></span>
::: moniker-end

## <a name="set-a-secret"></a><span data-ttu-id="2bdfb-156">Ustaw klucz tajny</span><span class="sxs-lookup"><span data-stu-id="2bdfb-156">Set a secret</span></span>

<span data-ttu-id="2bdfb-157">Narzędzie Menedżer klucz tajny działa na ustawienia konfiguracji określonego projektu przechowywane w profilu użytkownika.</span><span class="sxs-lookup"><span data-stu-id="2bdfb-157">The Secret Manager tool operates on project-specific configuration settings stored in your user profile.</span></span> <span data-ttu-id="2bdfb-158">Aby użyć hasła użytkownika, należy zdefiniować `UserSecretsId` w elemencie `PropertyGroup` z *.csproj* pliku.</span><span class="sxs-lookup"><span data-stu-id="2bdfb-158">To use user secrets, define a `UserSecretsId` element within a `PropertyGroup` of the *.csproj* file.</span></span> <span data-ttu-id="2bdfb-159">Wartość `UserSecretsId` jest określana, ale jest unikatowy dla projektu.</span><span class="sxs-lookup"><span data-stu-id="2bdfb-159">The value of `UserSecretsId` is arbitrary, but is unique to the project.</span></span> <span data-ttu-id="2bdfb-160">Deweloperzy zazwyczaj generowania identyfikatora GUID dla `UserSecretsId`.</span><span class="sxs-lookup"><span data-stu-id="2bdfb-160">Developers typically generate a GUID for the `UserSecretsId`.</span></span>

::: moniker range="<= aspnetcore-1.1"
<span data-ttu-id="2bdfb-161">[!code-xml[](app-secrets/samples/1.x/UserSecrets/UserSecrets.csproj?name=snippet_PropertyGroup&highlight=3)]</span><span class="sxs-lookup"><span data-stu-id="2bdfb-161">[!code-xml[](app-secrets/samples/1.x/UserSecrets/UserSecrets.csproj?name=snippet_PropertyGroup&highlight=3)]</span></span>
::: moniker-end
::: moniker range=">= aspnetcore-2.0"
<span data-ttu-id="2bdfb-162">[!code-xml[](app-secrets/samples/2.x/UserSecrets/UserSecrets.csproj?name=snippet_PropertyGroup&highlight=3)]</span><span class="sxs-lookup"><span data-stu-id="2bdfb-162">[!code-xml[](app-secrets/samples/2.x/UserSecrets/UserSecrets.csproj?name=snippet_PropertyGroup&highlight=3)]</span></span>
::: moniker-end

> [!TIP]
> <span data-ttu-id="2bdfb-163">W programie Visual Studio, kliknij prawym przyciskiem myszy projekt w Eksploratorze rozwiązań i wybierz **Zarządzanie kluczy tajnych użytkownika** z menu kontekstowego.</span><span class="sxs-lookup"><span data-stu-id="2bdfb-163">In Visual Studio, right-click the project in Solution Explorer, and select **Manage User Secrets** from the context menu.</span></span> <span data-ttu-id="2bdfb-164">Dodaje ten gest `UserSecretsId` element wypełniać za pomocą identyfikatora GUID do *.csproj* pliku.</span><span class="sxs-lookup"><span data-stu-id="2bdfb-164">This gesture adds a `UserSecretsId` element, populated with a GUID, to the *.csproj* file.</span></span> <span data-ttu-id="2bdfb-165">Visual Studio otworzy *secrets.json* plik w edytorze tekstu.</span><span class="sxs-lookup"><span data-stu-id="2bdfb-165">Visual Studio opens a *secrets.json* file in the text editor.</span></span> <span data-ttu-id="2bdfb-166">Zastąp zawartość *secrets.json* z pary klucz wartość do zapisania.</span><span class="sxs-lookup"><span data-stu-id="2bdfb-166">Replace the contents of *secrets.json* with the key-value pairs to be stored.</span></span> <span data-ttu-id="2bdfb-167">Na przykład: [!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file.md)]</span><span class="sxs-lookup"><span data-stu-id="2bdfb-167">For example: [!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file.md)]</span></span>

<span data-ttu-id="2bdfb-168">Zdefiniuj klucz tajny aplikacji składający się z klucza i wartości.</span><span class="sxs-lookup"><span data-stu-id="2bdfb-168">Define an app secret consisting of a key and its value.</span></span> <span data-ttu-id="2bdfb-169">Klucz tajny jest skojarzony z projektem `UserSecretsId` wartość.</span><span class="sxs-lookup"><span data-stu-id="2bdfb-169">The secret is associated with the project's `UserSecretsId` value.</span></span> <span data-ttu-id="2bdfb-170">Na przykład, uruchom następujące polecenie z katalogu, w którym *.csproj* plik istnieje:</span><span class="sxs-lookup"><span data-stu-id="2bdfb-170">For example, run the following command from the directory in which the *.csproj* file exists:</span></span>

```console
dotnet user-secrets set "Movies:ServiceApiKey" "12345"
```

<span data-ttu-id="2bdfb-171">W poprzednim przykładzie, dwukropek, oznacza, że `Movies` jest obiektem literału z `ServiceApiKey` właściwości.</span><span class="sxs-lookup"><span data-stu-id="2bdfb-171">In the preceding example, the colon denotes that `Movies` is an object literal with a `ServiceApiKey` property.</span></span>

<span data-ttu-id="2bdfb-172">Narzędzie Menedżer klucz tajny może służyć za z innych katalogów.</span><span class="sxs-lookup"><span data-stu-id="2bdfb-172">The Secret Manager tool can be used from other directories too.</span></span> <span data-ttu-id="2bdfb-173">Użyj `--project` opcję, aby podać ścieżka systemu plików, w którym *.csproj* plik istnieje.</span><span class="sxs-lookup"><span data-stu-id="2bdfb-173">Use the `--project` option to supply the file system path at which the *.csproj* file exists.</span></span> <span data-ttu-id="2bdfb-174">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="2bdfb-174">For example:</span></span>

```console
dotnet user-secrets set "Movies:ServiceApiKey" "12345" --project "C:\apps\WebApp1\src\WebApp1"
```

## <a name="set-multiple-secrets"></a><span data-ttu-id="2bdfb-175">Ustawianie wielu kluczy tajnych</span><span class="sxs-lookup"><span data-stu-id="2bdfb-175">Set multiple secrets</span></span>

<span data-ttu-id="2bdfb-176">Można ustawić partii kluczy tajnych przez przekazanie w potoku JSON do `set` polecenia.</span><span class="sxs-lookup"><span data-stu-id="2bdfb-176">A batch of secrets can be set by piping JSON to the `set` command.</span></span> <span data-ttu-id="2bdfb-177">W poniższym przykładzie *input.json* jego zawartość jest przetwarzana potokowo do `set` polecenia.</span><span class="sxs-lookup"><span data-stu-id="2bdfb-177">In the following example, the *input.json* file's contents are piped to the `set` command.</span></span>

# <a name="windowstabwindows"></a>[<span data-ttu-id="2bdfb-178">Windows</span><span class="sxs-lookup"><span data-stu-id="2bdfb-178">Windows</span></span>](#tab/windows)

<span data-ttu-id="2bdfb-179">Otwórz powłokę poleceń, a następnie uruchom następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="2bdfb-179">Open a command shell, and execute the following command:</span></span>

  ```console
  type .\input.json | dotnet user-secrets set
  ```

# <a name="macostabmacos"></a>[<span data-ttu-id="2bdfb-180">macOS</span><span class="sxs-lookup"><span data-stu-id="2bdfb-180">macOS</span></span>](#tab/macos)

<span data-ttu-id="2bdfb-181">Otwórz powłokę poleceń, a następnie uruchom następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="2bdfb-181">Open a command shell, and execute the following command:</span></span>

  ```console
  cat ./input.json | dotnet user-secrets set
  ```

# <a name="linuxtablinux"></a>[<span data-ttu-id="2bdfb-182">Linux</span><span class="sxs-lookup"><span data-stu-id="2bdfb-182">Linux</span></span>](#tab/linux)

<span data-ttu-id="2bdfb-183">Otwórz powłokę poleceń, a następnie uruchom następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="2bdfb-183">Open a command shell, and execute the following command:</span></span>

  ```console
  cat ./input.json | dotnet user-secrets set
  ```

---

## <a name="access-a-secret"></a><span data-ttu-id="2bdfb-184">Dostęp do klucza tajnego</span><span class="sxs-lookup"><span data-stu-id="2bdfb-184">Access a secret</span></span>

::: moniker range="<= aspnetcore-1.1"
<span data-ttu-id="2bdfb-185">[Interfejsu API platformy ASP.NET Core](xref:fundamentals/configuration/index) zapewnia dostęp do kluczy tajnych Menedżera klucz tajny.</span><span class="sxs-lookup"><span data-stu-id="2bdfb-185">The [ASP.NET Core Configuration API](xref:fundamentals/configuration/index) provides access to Secret Manager secrets.</span></span> <span data-ttu-id="2bdfb-186">Zainstaluj [Microsoft.Extensions.Configuration.UserSecrets](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.UserSecrets) pakietu NuGet.</span><span class="sxs-lookup"><span data-stu-id="2bdfb-186">Install the [Microsoft.Extensions.Configuration.UserSecrets](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.UserSecrets) NuGet package.</span></span>

<span data-ttu-id="2bdfb-187">Dodaj źródło konfiguracji kluczy tajnych użytkownika z wywołaniem do [AddUserSecrets](/dotnet/api/microsoft.extensions.configuration.usersecretsconfigurationextensions.addusersecrets) w `Startup` konstruktora:</span><span class="sxs-lookup"><span data-stu-id="2bdfb-187">Add the user secrets configuration source with a call to [AddUserSecrets](/dotnet/api/microsoft.extensions.configuration.usersecretsconfigurationextensions.addusersecrets) in the `Startup` constructor:</span></span>

<span data-ttu-id="2bdfb-188">[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup.cs?name=snippet_StartupConstructor&highlight=5-8)]</span><span class="sxs-lookup"><span data-stu-id="2bdfb-188">[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup.cs?name=snippet_StartupConstructor&highlight=5-8)]</span></span>
::: moniker-end
::: moniker range=">= aspnetcore-2.0"
<span data-ttu-id="2bdfb-189">[Interfejsu API platformy ASP.NET Core](xref:fundamentals/configuration/index) zapewnia dostęp do kluczy tajnych Menedżera klucz tajny.</span><span class="sxs-lookup"><span data-stu-id="2bdfb-189">The [ASP.NET Core Configuration API](xref:fundamentals/configuration/index) provides access to Secret Manager secrets.</span></span> <span data-ttu-id="2bdfb-190">Jeśli projekt jest przeznaczony dla programu .NET Framework, zainstaluj [Microsoft.Extensions.Configuration.UserSecrets](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.UserSecrets) pakietu NuGet.</span><span class="sxs-lookup"><span data-stu-id="2bdfb-190">If your project targets the .NET Framework, install the [Microsoft.Extensions.Configuration.UserSecrets](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.UserSecrets) NuGet package.</span></span>

<span data-ttu-id="2bdfb-191">W programie ASP.NET Core 2.0 lub nowszej, źródło konfiguracji kluczy tajnych użytkownika zostaną automatycznie dodane w tryb programowania, gdy wywołuje projektu [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) zainicjować nowe wystąpienie hosta przy użyciu wstępnie skonfigurowanych ustawień domyślnych.</span><span class="sxs-lookup"><span data-stu-id="2bdfb-191">In ASP.NET Core 2.0 or later, the user secrets configuration source is automatically added in development mode when the project calls [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) to initialize a new instance of the host with preconfigured defaults.</span></span> <span data-ttu-id="2bdfb-192">`CreateDefaultBuilder` wywołania [AddUserSecrets](/dotnet/api/microsoft.extensions.configuration.usersecretsconfigurationextensions.addusersecrets) podczas [EnvironmentName](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.environmentname) jest [programowanie](/dotnet/api/microsoft.aspnetcore.hosting.environmentname.development):</span><span class="sxs-lookup"><span data-stu-id="2bdfb-192">`CreateDefaultBuilder` calls [AddUserSecrets](/dotnet/api/microsoft.extensions.configuration.usersecretsconfigurationextensions.addusersecrets) when the [EnvironmentName](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.environmentname) is [Development](/dotnet/api/microsoft.aspnetcore.hosting.environmentname.development):</span></span>

<span data-ttu-id="2bdfb-193">[!code-csharp[](app-secrets/samples/2.x/UserSecrets/Program.cs?name=snippet_CreateWebHostBuilder&highlight=2)]</span><span class="sxs-lookup"><span data-stu-id="2bdfb-193">[!code-csharp[](app-secrets/samples/2.x/UserSecrets/Program.cs?name=snippet_CreateWebHostBuilder&highlight=2)]</span></span>

<span data-ttu-id="2bdfb-194">Gdy `CreateDefaultBuilder` nie jest wywoływana podczas konstruowania hosta, Dodaj źródło konfiguracji kluczy tajnych użytkownika z wywołaniem do [AddUserSecrets](/dotnet/api/microsoft.extensions.configuration.usersecretsconfigurationextensions.addusersecrets) w `Startup` konstruktora:</span><span class="sxs-lookup"><span data-stu-id="2bdfb-194">When `CreateDefaultBuilder` isn't called during host construction, add the user secrets configuration source with a call to [AddUserSecrets](/dotnet/api/microsoft.extensions.configuration.usersecretsconfigurationextensions.addusersecrets) in the `Startup` constructor:</span></span>

<span data-ttu-id="2bdfb-195">[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup.cs?name=snippet_StartupConstructor&highlight=5-8)]</span><span class="sxs-lookup"><span data-stu-id="2bdfb-195">[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup.cs?name=snippet_StartupConstructor&highlight=5-8)]</span></span>
::: moniker-end

<span data-ttu-id="2bdfb-196">Klucze tajne użytkownika mogą zostać pobrane za pośrednictwem `Configuration` interfejsu API:</span><span class="sxs-lookup"><span data-stu-id="2bdfb-196">User secrets can be retrieved via the `Configuration` API:</span></span>

::: moniker range="<= aspnetcore-1.1"
<span data-ttu-id="2bdfb-197">[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup.cs?name=snippet_StartupClass&highlight=23)]</span><span class="sxs-lookup"><span data-stu-id="2bdfb-197">[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup.cs?name=snippet_StartupClass&highlight=23)]</span></span>
::: moniker-end
::: moniker range=">= aspnetcore-2.0"
<span data-ttu-id="2bdfb-198">[!code-csharp[](app-secrets/samples/2.x/UserSecrets/Startup.cs?name=snippet_StartupClass&highlight=14)]</span><span class="sxs-lookup"><span data-stu-id="2bdfb-198">[!code-csharp[](app-secrets/samples/2.x/UserSecrets/Startup.cs?name=snippet_StartupClass&highlight=14)]</span></span>
::: moniker-end

## <a name="string-replacement-with-secrets"></a><span data-ttu-id="2bdfb-199">Ciąg zastępczy z kluczy tajnych</span><span class="sxs-lookup"><span data-stu-id="2bdfb-199">String replacement with secrets</span></span>

<span data-ttu-id="2bdfb-200">Przechowywanie haseł w postaci zwykłego tekstu jest niebezpieczne.</span><span class="sxs-lookup"><span data-stu-id="2bdfb-200">Storing passwords in plain text is insecure.</span></span> <span data-ttu-id="2bdfb-201">Na przykład ciąg połączenia bazy danych są przechowywane w *appsettings.json* może zawierać hasła dla określonego użytkownika:</span><span class="sxs-lookup"><span data-stu-id="2bdfb-201">For example, a database connection string stored in *appsettings.json* may include a password for the specified user:</span></span>

[!code-json[](app-secrets/samples/2.x/UserSecrets/appsettings-unsecure.json?highlight=3)]

<span data-ttu-id="2bdfb-202">Bardziej bezpieczną metodą jest, aby zapisać hasło jako klucz tajny.</span><span class="sxs-lookup"><span data-stu-id="2bdfb-202">A more secure approach is to store the password as a secret.</span></span> <span data-ttu-id="2bdfb-203">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="2bdfb-203">For example:</span></span>

```console
dotnet user-secrets set "DbPassword" "pass123"
```

<span data-ttu-id="2bdfb-204">Usuń `Password` pary klucz wartość z parametrów połączenia w *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="2bdfb-204">Remove the `Password` key-value pair from the connection string in *appsettings.json*.</span></span> <span data-ttu-id="2bdfb-205">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="2bdfb-205">For example:</span></span>

[!code-json[](app-secrets/samples/2.x/UserSecrets/appsettings.json?highlight=3)]

<span data-ttu-id="2bdfb-206">Można ustawić wartości klucza tajnego [SqlConnectionStringBuilder](/dotnet/api/system.data.sqlclient.sqlconnectionstringbuilder) obiektu [hasło](/dotnet/api/system.data.sqlclient.sqlconnectionstringbuilder.password) właściwości, aby ukończyć parametry połączenia:</span><span class="sxs-lookup"><span data-stu-id="2bdfb-206">The secret's value can be set on a [SqlConnectionStringBuilder](/dotnet/api/system.data.sqlclient.sqlconnectionstringbuilder) object's [Password](/dotnet/api/system.data.sqlclient.sqlconnectionstringbuilder.password) property to complete the connection string:</span></span>

::: moniker range="<= aspnetcore-1.1"
<span data-ttu-id="2bdfb-207">[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup2.cs?name=snippet_StartupClass&highlight=26-29)]</span><span class="sxs-lookup"><span data-stu-id="2bdfb-207">[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup2.cs?name=snippet_StartupClass&highlight=26-29)]</span></span>
::: moniker-end
::: moniker range=">= aspnetcore-2.0"
<span data-ttu-id="2bdfb-208">[!code-csharp[](app-secrets/samples/2.x/UserSecrets/Startup2.cs?name=snippet_StartupClass&highlight=14-17)]</span><span class="sxs-lookup"><span data-stu-id="2bdfb-208">[!code-csharp[](app-secrets/samples/2.x/UserSecrets/Startup2.cs?name=snippet_StartupClass&highlight=14-17)]</span></span>
::: moniker-end

## <a name="list-the-secrets"></a><span data-ttu-id="2bdfb-209">Lista kluczy tajnych</span><span class="sxs-lookup"><span data-stu-id="2bdfb-209">List the secrets</span></span>

[!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file-and-text.md)]

<span data-ttu-id="2bdfb-210">Uruchom następujące polecenie z katalogu, w którym *.csproj* plik istnieje:</span><span class="sxs-lookup"><span data-stu-id="2bdfb-210">Run the following command from the directory in which the *.csproj* file exists:</span></span>

```console
dotnet user-secrets list
```

<span data-ttu-id="2bdfb-211">Zostaną wyświetlone następujące dane wyjściowe:</span><span class="sxs-lookup"><span data-stu-id="2bdfb-211">The following output appears:</span></span>

```console
Movies:ServiceApiKey = 12345
Movies:ConnectionString = Server=(localdb)\mssqllocaldb;Database=Movie-1;Trusted_Connection=True;MultipleActiveResultSets=true
```

<span data-ttu-id="2bdfb-212">W poprzednim przykładzie, dwukropek w nazwach kluczy oznacza hierarchii obiektów w ramach *secrets.json*.</span><span class="sxs-lookup"><span data-stu-id="2bdfb-212">In the preceding example, a colon in the key names denotes the object hierarchy within *secrets.json*.</span></span>

## <a name="remove-a-single-secret"></a><span data-ttu-id="2bdfb-213">Usuń jeden klucz tajny</span><span class="sxs-lookup"><span data-stu-id="2bdfb-213">Remove a single secret</span></span>

[!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file-and-text.md)]

<span data-ttu-id="2bdfb-214">Uruchom następujące polecenie z katalogu, w którym *.csproj* plik istnieje:</span><span class="sxs-lookup"><span data-stu-id="2bdfb-214">Run the following command from the directory in which the *.csproj* file exists:</span></span>

```console
dotnet user-secrets remove "Movies:ConnectionString"
```

<span data-ttu-id="2bdfb-215">Aplikacji *secrets.json* plik został zmodyfikowany, aby usunąć parę klucz wartość skojarzonych z `MoviesConnectionString` klucza:</span><span class="sxs-lookup"><span data-stu-id="2bdfb-215">The app's *secrets.json* file was modified to remove the key-value pair associated with the `MoviesConnectionString` key:</span></span>

```json
{
  "Movies": {
    "ServiceApiKey": "12345"
  }
}
```

<span data-ttu-id="2bdfb-216">Uruchomiona `dotnet user-secrets list` wyświetli następujący komunikat:</span><span class="sxs-lookup"><span data-stu-id="2bdfb-216">Running `dotnet user-secrets list` displays the following message:</span></span>

```console
Movies:ServiceApiKey = 12345
```

## <a name="remove-all-secrets"></a><span data-ttu-id="2bdfb-217">Usunięcie wszystkich kluczy tajnych</span><span class="sxs-lookup"><span data-stu-id="2bdfb-217">Remove all secrets</span></span>

[!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file-and-text.md)]

<span data-ttu-id="2bdfb-218">Uruchom następujące polecenie z katalogu, w którym *.csproj* plik istnieje:</span><span class="sxs-lookup"><span data-stu-id="2bdfb-218">Run the following command from the directory in which the *.csproj* file exists:</span></span>

```console
dotnet user-secrets clear
```

<span data-ttu-id="2bdfb-219">Usunięto wszystkie hasła użytkownika dla aplikacji z *secrets.json* pliku:</span><span class="sxs-lookup"><span data-stu-id="2bdfb-219">All user secrets for the app have been deleted from the *secrets.json* file:</span></span>

```json
{}
```

<span data-ttu-id="2bdfb-220">Uruchomiona `dotnet user-secrets list` wyświetli następujący komunikat:</span><span class="sxs-lookup"><span data-stu-id="2bdfb-220">Running `dotnet user-secrets list` displays the following message:</span></span>

```console
No secrets configured for this application.
```

## <a name="additional-resources"></a><span data-ttu-id="2bdfb-221">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="2bdfb-221">Additional resources</span></span>

* <xref:fundamentals/configuration/index>
* <xref:security/key-vault-configuration>
