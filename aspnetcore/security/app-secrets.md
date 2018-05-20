---
title: Bezpieczne przechowywanie kluczy tajnych aplikacji w rozwoju platformy ASP.NET Core
author: rick-anderson
description: Sposób przechowywania i pobierania informacji poufnych jako klucze tajne aplikacji podczas tworzenia aplikacji platformy ASP.NET Core.
manager: wpickett
ms.author: scaddie
ms.custom: mvc
ms.date: 05/16/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/app-secrets
ms.openlocfilehash: 88b4ee9a963543f8cc97cb66271628a14fe657de
ms.sourcegitcommit: 3a893ae05f010656d99d6ddf55e82f1b5b6933bc
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/18/2018
---
# <a name="safe-storage-of-app-secrets-in-development-in-aspnet-core"></a><span data-ttu-id="6b95c-103">Bezpieczne przechowywanie kluczy tajnych aplikacji w rozwoju platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="6b95c-103">Safe storage of app secrets in development in ASP.NET Core</span></span>

<span data-ttu-id="6b95c-104">Przez [Rick Anderson](https://twitter.com/RickAndMSFT), [Roth Danielowi](https://github.com/danroth27), i [Scott Addie](https://github.com/scottaddie)</span><span class="sxs-lookup"><span data-stu-id="6b95c-104">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Daniel Roth](https://github.com/danroth27), and [Scott Addie](https://github.com/scottaddie)</span></span>

::: moniker range="<= aspnetcore-1.1"
<span data-ttu-id="6b95c-105">[Wyświetlić lub pobrać przykładowy kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/app-secrets/samples/1.1) ([sposobu pobierania](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="6b95c-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/app-secrets/samples/1.1) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>
::: moniker-end
::: moniker range=">= aspnetcore-2.0"
<span data-ttu-id="6b95c-106">[Wyświetlić lub pobrać przykładowy kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/app-secrets/samples/2.1) ([sposobu pobierania](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="6b95c-106">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/app-secrets/samples/2.1) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>
::: moniker-end

<span data-ttu-id="6b95c-107">W tym dokumencie opisano techniki do przechowywania i pobierania danych poufnych podczas tworzenia aplikacji platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="6b95c-107">This document explains techniques for storing and retrieving sensitive data during the development of an ASP.NET Core app.</span></span> <span data-ttu-id="6b95c-108">W kodzie źródłowym nigdy nie należy przechowywać haseł i innych poufnych danych, a nie powinien użyć kluczy tajnych produkcji w rozwoju lub testowania trybu.</span><span class="sxs-lookup"><span data-stu-id="6b95c-108">You should never store passwords or other sensitive data in source code, and you shouldn't use production secrets in development or test mode.</span></span> <span data-ttu-id="6b95c-109">Można przechowywać i chronić Azure kluczy tajnych testowych i produkcyjnych z [dostawcę konfiguracji usługi Azure Key Vault](xref:security/key-vault-configuration).</span><span class="sxs-lookup"><span data-stu-id="6b95c-109">You can store and protect Azure test and production secrets with the [Azure Key Vault configuration provider](xref:security/key-vault-configuration).</span></span>

## <a name="environment-variables"></a><span data-ttu-id="6b95c-110">Zmienne środowiskowe</span><span class="sxs-lookup"><span data-stu-id="6b95c-110">Environment variables</span></span>

<span data-ttu-id="6b95c-111">Zmienne środowiskowe są używane do Rezygnacja z zapisywania haseł aplikacji, kodu lub lokalne pliki konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="6b95c-111">Environment variables are used to avoid storage of app secrets in code or in local configuration files.</span></span> <span data-ttu-id="6b95c-112">Zmienne środowiskowe przesłaniają wartości konfiguracji dla wszystkich źródeł wcześniej określonej konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="6b95c-112">Environment variables override configuration values for all previously specified configuration sources.</span></span>

::: moniker range="<= aspnetcore-1.1"
<span data-ttu-id="6b95c-113">Konfigurowanie przy odczycie wartości zmiennych środowiskowych, wywołując [AddEnvironmentVariables](/dotnet/api/microsoft.extensions.configuration.environmentvariablesextensions.addenvironmentvariables) w `Startup` konstruktora:</span><span class="sxs-lookup"><span data-stu-id="6b95c-113">Configure the reading of environment variable values by calling [AddEnvironmentVariables](/dotnet/api/microsoft.extensions.configuration.environmentvariablesextensions.addenvironmentvariables) in the `Startup` constructor:</span></span>

<span data-ttu-id="6b95c-114">[!code-csharp[](app-secrets/samples/1.1/UserSecrets/Startup.cs?name=snippet_StartupConstructor&highlight=10)]</span><span class="sxs-lookup"><span data-stu-id="6b95c-114">[!code-csharp[](app-secrets/samples/1.1/UserSecrets/Startup.cs?name=snippet_StartupConstructor&highlight=10)]</span></span>
::: moniker-end

<span data-ttu-id="6b95c-115">Należy wziąć pod uwagę aplikacji sieci web platformy ASP.NET Core, w którym **indywidualnych kont użytkowników** zabezpieczeń jest włączone.</span><span class="sxs-lookup"><span data-stu-id="6b95c-115">Consider an ASP.NET Core web app in which **Individual User Accounts** security is enabled.</span></span> <span data-ttu-id="6b95c-116">Domyślny ciąg połączenia bazy danych jest dołączony do projektu *appsettings.json* pliku z kluczem `DefaultConnection`.</span><span class="sxs-lookup"><span data-stu-id="6b95c-116">A default database connection string is included in the project's *appsettings.json* file with the key `DefaultConnection`.</span></span> <span data-ttu-id="6b95c-117">Domyślny ciąg połączenia jest dla bazy danych LocalDB, która działa w trybie użytkownika i nie wymaga hasła.</span><span class="sxs-lookup"><span data-stu-id="6b95c-117">The default connection string is for LocalDB, which runs in user mode and doesn't require a password.</span></span> <span data-ttu-id="6b95c-118">Podczas wdrażania aplikacji `DefaultConnection` wartość klucza może zostać zastąpiona przez wartość zmiennej środowiskowej.</span><span class="sxs-lookup"><span data-stu-id="6b95c-118">During app deployment, the `DefaultConnection` key value can be overridden with an environment variable's value.</span></span> <span data-ttu-id="6b95c-119">Zmienna środowiskowa może przechowywać parametry połączenia ukończone z poświadczeniami poufnych.</span><span class="sxs-lookup"><span data-stu-id="6b95c-119">The environment variable may store the complete connection string with sensitive credentials.</span></span>

> [!WARNING]
> <span data-ttu-id="6b95c-120">Zmienne środowiskowe są zazwyczaj przechowywane w zwykły tekst niezaszyfrowane.</span><span class="sxs-lookup"><span data-stu-id="6b95c-120">Environment variables are generally stored in plain, unencrypted text.</span></span> <span data-ttu-id="6b95c-121">W przypadku naruszenia zabezpieczeń komputera lub proces, zmienne środowiskowe są dostępne przez niezaufane.</span><span class="sxs-lookup"><span data-stu-id="6b95c-121">If the machine or process is compromised, environment variables can be accessed by untrusted parties.</span></span> <span data-ttu-id="6b95c-122">Może być wymagane dodatkowe środki, aby zapobiec ujawnieniu hasła użytkownika.</span><span class="sxs-lookup"><span data-stu-id="6b95c-122">Additional measures to prevent disclosure of user secrets may be required.</span></span>

## <a name="secret-manager"></a><span data-ttu-id="6b95c-123">Menedżer klucz tajny</span><span class="sxs-lookup"><span data-stu-id="6b95c-123">Secret Manager</span></span>

<span data-ttu-id="6b95c-124">Narzędzie Menedżer klucz tajny są przechowywane poufne dane podczas tworzenia projektu platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="6b95c-124">The Secret Manager tool stores sensitive data during the development of an ASP.NET Core project.</span></span> <span data-ttu-id="6b95c-125">W tym kontekście element danych poufnych jest klucz tajny aplikacji.</span><span class="sxs-lookup"><span data-stu-id="6b95c-125">In this context, a piece of sensitive data is an app secret.</span></span> <span data-ttu-id="6b95c-126">Klucze tajne aplikacji są przechowywane w innej lokalizacji niż drzewa projektu.</span><span class="sxs-lookup"><span data-stu-id="6b95c-126">App secrets are stored in a separate location from the project tree.</span></span> <span data-ttu-id="6b95c-127">Klucze tajne aplikacji są skojarzone z określonego projektu lub współużytkowane przez kilka projektów.</span><span class="sxs-lookup"><span data-stu-id="6b95c-127">The app secrets are associated with a specific project or shared across several projects.</span></span> <span data-ttu-id="6b95c-128">Klucze tajne aplikacji nie jest wyewidencjonowany do kontroli źródła.</span><span class="sxs-lookup"><span data-stu-id="6b95c-128">The app secrets aren't checked into source control.</span></span>

> [!WARNING]
> <span data-ttu-id="6b95c-129">Narzędzie Menedżer klucz tajny nie Szyfruj przechowywane klucze tajne i nie powinny być traktowane jako zaufanego magazynu.</span><span class="sxs-lookup"><span data-stu-id="6b95c-129">The Secret Manager tool doesn't encrypt the stored secrets and shouldn't be treated as a trusted store.</span></span> <span data-ttu-id="6b95c-130">Jest tylko do celów programistycznych.</span><span class="sxs-lookup"><span data-stu-id="6b95c-130">It's for development purposes only.</span></span> <span data-ttu-id="6b95c-131">Klucze i wartości są przechowywane w pliku konfiguracji JSON w katalogu profilu użytkownika.</span><span class="sxs-lookup"><span data-stu-id="6b95c-131">The keys and values are stored in a JSON configuration file in the user profile directory.</span></span>

## <a name="how-the-secret-manager-tool-works"></a><span data-ttu-id="6b95c-132">Jak działa narzędzie Menedżer klucz tajny</span><span class="sxs-lookup"><span data-stu-id="6b95c-132">How the Secret Manager tool works</span></span>

<span data-ttu-id="6b95c-133">Narzędzie Menedżer klucz tajny optymalizacji abstracts szczegóły implementacji, takich jak jak i gdzie są przechowywane wartości.</span><span class="sxs-lookup"><span data-stu-id="6b95c-133">The Secret Manager tool abstracts away the implementation details, such as where and how the values are stored.</span></span> <span data-ttu-id="6b95c-134">Można użyć narzędzia bez uprzedniego uzyskania informacji o tych szczegóły implementacji.</span><span class="sxs-lookup"><span data-stu-id="6b95c-134">You can use the tool without knowing these implementation details.</span></span> <span data-ttu-id="6b95c-135">Wartości są przechowywane w [JSON](https://json.org/) pliku konfiguracji w folderze profilu użytkownika chronionych przez system na komputerze lokalnym:</span><span class="sxs-lookup"><span data-stu-id="6b95c-135">The values are stored in a [JSON](https://json.org/) configuration file in a system-protected user profile folder on the local machine:</span></span>

# <a name="windowstabwindows"></a>[<span data-ttu-id="6b95c-136">Windows</span><span class="sxs-lookup"><span data-stu-id="6b95c-136">Windows</span></span>](#tab/windows)

<span data-ttu-id="6b95c-137">Ścieżka systemu plików:</span><span class="sxs-lookup"><span data-stu-id="6b95c-137">File system path:</span></span>

`%APPDATA%\Microsoft\UserSecrets\<user_secrets_id>\secrets.json`

# <a name="macostabmacos"></a>[<span data-ttu-id="6b95c-138">macOS</span><span class="sxs-lookup"><span data-stu-id="6b95c-138">macOS</span></span>](#tab/macos)

<span data-ttu-id="6b95c-139">Ścieżka systemu plików:</span><span class="sxs-lookup"><span data-stu-id="6b95c-139">File system path:</span></span>

`~/.microsoft/usersecrets/<user_secrets_id>/secrets.json`

# <a name="linuxtablinux"></a>[<span data-ttu-id="6b95c-140">Linux</span><span class="sxs-lookup"><span data-stu-id="6b95c-140">Linux</span></span>](#tab/linux)

<span data-ttu-id="6b95c-141">Ścieżka systemu plików:</span><span class="sxs-lookup"><span data-stu-id="6b95c-141">File system path:</span></span>

`~/.microsoft/usersecrets/<user_secrets_id>/secrets.json`

---

<span data-ttu-id="6b95c-142">W poprzednim ścieżki pliku, Zastąp `<user_secrets_id>` z `UserSecretsId` wartości określonej w *.csproj* pliku.</span><span class="sxs-lookup"><span data-stu-id="6b95c-142">In the preceding file paths, replace `<user_secrets_id>` with the `UserSecretsId` value specified in the *.csproj* file.</span></span>

<span data-ttu-id="6b95c-143">Nie pisania kodu, który jest zależny od lokalizacji lub format dane zapisane za pomocą narzędzia menedżera klucz tajny.</span><span class="sxs-lookup"><span data-stu-id="6b95c-143">Don't write code that depends on the location or format of data saved with the Secret Manager tool.</span></span> <span data-ttu-id="6b95c-144">Te szczegóły implementacji mogą ulec zmianie.</span><span class="sxs-lookup"><span data-stu-id="6b95c-144">These implementation details may change.</span></span> <span data-ttu-id="6b95c-145">Na przykład tajny wartości nie są zaszyfrowane, ale może być w przyszłości.</span><span class="sxs-lookup"><span data-stu-id="6b95c-145">For example, the secret values aren't encrypted, but could be in the future.</span></span>

::: moniker range="<= aspnetcore-2.0"
## <a name="install-the-secret-manager-tool"></a><span data-ttu-id="6b95c-146">Zainstaluj narzędzie Menedżer klucz tajny</span><span class="sxs-lookup"><span data-stu-id="6b95c-146">Install the Secret Manager tool</span></span>

<span data-ttu-id="6b95c-147">Narzędzie Menedżer klucz tajny jest dołączany .NET Core interfejsu wiersza polecenia w .NET Core SDK 2.1.</span><span class="sxs-lookup"><span data-stu-id="6b95c-147">The Secret Manager tool is bundled with the .NET Core CLI in .NET Core SDK 2.1.</span></span> <span data-ttu-id="6b95c-148">Dla platformy .NET Core SDK 2.0 i starszych wersji niezbędne jest narzędzie instalacji.</span><span class="sxs-lookup"><span data-stu-id="6b95c-148">For .NET Core SDK 2.0 and earlier, tool installation is necessary.</span></span>

<span data-ttu-id="6b95c-149">Zainstaluj [Microsoft.Extensions.SecretManager.Tools](https://www.nuget.org/packages/Microsoft.Extensions.SecretManager.Tools/) pakietu NuGet w projekcie platformy ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="6b95c-149">Install the [Microsoft.Extensions.SecretManager.Tools](https://www.nuget.org/packages/Microsoft.Extensions.SecretManager.Tools/) NuGet package in your ASP.NET Core project:</span></span>

<span data-ttu-id="6b95c-150">[!code-xml[](app-secrets/samples/1.1/UserSecrets/UserSecrets.csproj?name=snippet_CsprojFile&highlight=13-14)]</span><span class="sxs-lookup"><span data-stu-id="6b95c-150">[!code-xml[](app-secrets/samples/1.1/UserSecrets/UserSecrets.csproj?name=snippet_CsprojFile&highlight=13-14)]</span></span>

<span data-ttu-id="6b95c-151">Uruchom następujące polecenie w powłoce poleceń, aby sprawdzić poprawność instalacji narzędzia:</span><span class="sxs-lookup"><span data-stu-id="6b95c-151">Execute the following command in a command shell to validate the tool installation:</span></span>

```console
dotnet user-secrets -h
```

<span data-ttu-id="6b95c-152">Narzędzie Menedżer klucz tajny zawiera przykładowe zastosowanie, opcje i pomoc dla polecenia:</span><span class="sxs-lookup"><span data-stu-id="6b95c-152">The Secret Manager tool displays sample usage, options, and command help:</span></span>

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
> <span data-ttu-id="6b95c-153">Musi być w tym samym katalogu co *.csproj* uruchamianie narzędzia zdefiniowane w pliku *.csproj* pliku `DotNetCliToolReference` elementów.</span><span class="sxs-lookup"><span data-stu-id="6b95c-153">You must be in the same directory as the *.csproj* file to run tools defined in the *.csproj* file's `DotNetCliToolReference` elements.</span></span>
::: moniker-end

## <a name="set-a-secret"></a><span data-ttu-id="6b95c-154">Ustaw klucz tajny</span><span class="sxs-lookup"><span data-stu-id="6b95c-154">Set a secret</span></span>

<span data-ttu-id="6b95c-155">Narzędzie Menedżer klucz tajny działa na ustawienia konfiguracji określonego projektu przechowywane w profilu użytkownika.</span><span class="sxs-lookup"><span data-stu-id="6b95c-155">The Secret Manager tool operates on project-specific configuration settings stored in your user profile.</span></span> <span data-ttu-id="6b95c-156">Aby użyć hasła użytkownika, należy zdefiniować `UserSecretsId` w elemencie `PropertyGroup` z *.csproj* pliku.</span><span class="sxs-lookup"><span data-stu-id="6b95c-156">To use user secrets, define a `UserSecretsId` element within a `PropertyGroup` of the *.csproj* file.</span></span> <span data-ttu-id="6b95c-157">Wartość `UserSecretsId` jest określana, ale jest unikatowy dla projektu.</span><span class="sxs-lookup"><span data-stu-id="6b95c-157">The value of `UserSecretsId` is arbitrary, but is unique to the project.</span></span> <span data-ttu-id="6b95c-158">Deweloperzy zazwyczaj generowania identyfikatora GUID dla `UserSecretsId`.</span><span class="sxs-lookup"><span data-stu-id="6b95c-158">Developers typically generate a GUID for the `UserSecretsId`.</span></span>

::: moniker range="<= aspnetcore-1.1"
<span data-ttu-id="6b95c-159">[!code-xml[](app-secrets/samples/1.1/UserSecrets/UserSecrets.csproj?name=snippet_PropertyGroup&highlight=3)]</span><span class="sxs-lookup"><span data-stu-id="6b95c-159">[!code-xml[](app-secrets/samples/1.1/UserSecrets/UserSecrets.csproj?name=snippet_PropertyGroup&highlight=3)]</span></span>
::: moniker-end
::: moniker range=">= aspnetcore-2.0"
<span data-ttu-id="6b95c-160">[!code-xml[](app-secrets/samples/2.1/UserSecrets/UserSecrets.csproj?name=snippet_PropertyGroup&highlight=3)]</span><span class="sxs-lookup"><span data-stu-id="6b95c-160">[!code-xml[](app-secrets/samples/2.1/UserSecrets/UserSecrets.csproj?name=snippet_PropertyGroup&highlight=3)]</span></span>
::: moniker-end

> [!TIP]
> <span data-ttu-id="6b95c-161">W programie Visual Studio, kliknij prawym przyciskiem myszy projekt w Eksploratorze rozwiązań i wybierz **Zarządzanie kluczy tajnych użytkownika** z menu kontekstowego.</span><span class="sxs-lookup"><span data-stu-id="6b95c-161">In Visual Studio, right-click the project in Solution Explorer, and select **Manage User Secrets** from the context menu.</span></span> <span data-ttu-id="6b95c-162">Dodaje ten gest `UserSecretsId` element wypełniać za pomocą identyfikatora GUID do *.csproj* pliku.</span><span class="sxs-lookup"><span data-stu-id="6b95c-162">This gesture adds a `UserSecretsId` element, populated with a GUID, to the *.csproj* file.</span></span> <span data-ttu-id="6b95c-163">Visual Studio otworzy *secrets.json* plik w edytorze tekstu.</span><span class="sxs-lookup"><span data-stu-id="6b95c-163">Visual Studio opens a *secrets.json* file in the text editor.</span></span> <span data-ttu-id="6b95c-164">Zastąp zawartość *secrets.json* z pary klucz wartość do zapisania.</span><span class="sxs-lookup"><span data-stu-id="6b95c-164">Replace the contents of *secrets.json* with the key-value pairs to be stored.</span></span> <span data-ttu-id="6b95c-165">Na przykład: [!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file.md)]</span><span class="sxs-lookup"><span data-stu-id="6b95c-165">For example: [!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file.md)]</span></span>

<span data-ttu-id="6b95c-166">Zdefiniuj klucz tajny aplikacji składający się z klucza i wartości.</span><span class="sxs-lookup"><span data-stu-id="6b95c-166">Define an app secret consisting of a key and its value.</span></span> <span data-ttu-id="6b95c-167">Klucz tajny jest skojarzony z projektem `UserSecretsId` wartość.</span><span class="sxs-lookup"><span data-stu-id="6b95c-167">The secret is associated with the project's `UserSecretsId` value.</span></span> <span data-ttu-id="6b95c-168">Na przykład, uruchom następujące polecenie z katalogu, w którym *.csproj* plik istnieje:</span><span class="sxs-lookup"><span data-stu-id="6b95c-168">For example, run the following command from the directory in which the *.csproj* file exists:</span></span>

```console
dotnet user-secrets set "Movies:ServiceApiKey" "12345"
```

<span data-ttu-id="6b95c-169">W poprzednim przykładzie, dwukropek, oznacza, że `Movies` jest obiektem literału z `ServiceApiKey` właściwości.</span><span class="sxs-lookup"><span data-stu-id="6b95c-169">In the preceding example, the colon denotes that `Movies` is an object literal with a `ServiceApiKey` property.</span></span>

<span data-ttu-id="6b95c-170">Narzędzie Menedżer klucz tajny może służyć za z innych katalogów.</span><span class="sxs-lookup"><span data-stu-id="6b95c-170">The Secret Manager tool can be used from other directories too.</span></span> <span data-ttu-id="6b95c-171">Użyj `--project` opcję, aby podać ścieżka systemu plików, w którym *.csproj* plik istnieje.</span><span class="sxs-lookup"><span data-stu-id="6b95c-171">Use the `--project` option to supply the file system path at which the *.csproj* file exists.</span></span> <span data-ttu-id="6b95c-172">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="6b95c-172">For example:</span></span>

```console
dotnet user-secrets set "Movies:ServiceApiKey" "12345" --project "C:\apps\WebApp1\src\WebApp1"
```

## <a name="set-multiple-secrets"></a><span data-ttu-id="6b95c-173">Ustawianie wielu kluczy tajnych</span><span class="sxs-lookup"><span data-stu-id="6b95c-173">Set multiple secrets</span></span>

<span data-ttu-id="6b95c-174">Można ustawić partii kluczy tajnych przez przekazanie w potoku JSON do `set` polecenia.</span><span class="sxs-lookup"><span data-stu-id="6b95c-174">A batch of secrets can be set by piping JSON to the `set` command.</span></span> <span data-ttu-id="6b95c-175">W poniższym przykładzie *input.json* jego zawartość jest przetwarzana potokowo do `set` polecenia.</span><span class="sxs-lookup"><span data-stu-id="6b95c-175">In the following example, the *input.json* file's contents are piped to the `set` command.</span></span>

# <a name="windowstabwindows"></a>[<span data-ttu-id="6b95c-176">Windows</span><span class="sxs-lookup"><span data-stu-id="6b95c-176">Windows</span></span>](#tab/windows)

<span data-ttu-id="6b95c-177">Otwórz powłokę poleceń, a następnie uruchom następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="6b95c-177">Open a command shell, and execute the following command:</span></span>

  ```console
  type .\input.json | dotnet user-secrets set
  ```

# <a name="macostabmacos"></a>[<span data-ttu-id="6b95c-178">macOS</span><span class="sxs-lookup"><span data-stu-id="6b95c-178">macOS</span></span>](#tab/macos)

<span data-ttu-id="6b95c-179">Otwórz powłokę poleceń, a następnie uruchom następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="6b95c-179">Open a command shell, and execute the following command:</span></span>

  ```console
  cat ./input.json | dotnet user-secrets set
  ```

# <a name="linuxtablinux"></a>[<span data-ttu-id="6b95c-180">Linux</span><span class="sxs-lookup"><span data-stu-id="6b95c-180">Linux</span></span>](#tab/linux)

<span data-ttu-id="6b95c-181">Otwórz powłokę poleceń, a następnie uruchom następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="6b95c-181">Open a command shell, and execute the following command:</span></span>

  ```console
  cat ./input.json | dotnet user-secrets set
  ```

---

## <a name="access-a-secret"></a><span data-ttu-id="6b95c-182">Dostęp do klucza tajnego</span><span class="sxs-lookup"><span data-stu-id="6b95c-182">Access a secret</span></span>

<span data-ttu-id="6b95c-183">[Interfejsu API platformy ASP.NET Core](xref:fundamentals/configuration/index) zapewnia dostęp do kluczy tajnych Menedżera klucz tajny.</span><span class="sxs-lookup"><span data-stu-id="6b95c-183">The [ASP.NET Core Configuration API](xref:fundamentals/configuration/index) provides access to Secret Manager secrets.</span></span> <span data-ttu-id="6b95c-184">Jeśli przeznaczonych dla platformy .NET Core 1.x lub .NET Framework, zainstaluj [Microsoft.Extensions.Configuration.UserSecrets](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.UserSecrets) pakietu NuGet.</span><span class="sxs-lookup"><span data-stu-id="6b95c-184">If targeting .NET Core 1.x or .NET Framework, install the [Microsoft.Extensions.Configuration.UserSecrets](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.UserSecrets) NuGet package.</span></span>

::: moniker range="<= aspnetcore-1.1"
<span data-ttu-id="6b95c-185">Dodaj użytkownika kluczy tajnych konfiguracji źródła `Startup` konstruktora:</span><span class="sxs-lookup"><span data-stu-id="6b95c-185">Add the user secrets configuration source to the `Startup` constructor:</span></span>

<span data-ttu-id="6b95c-186">[!code-csharp[](app-secrets/samples/1.1/UserSecrets/Startup.cs?name=snippet_StartupConstructor&highlight=5-8)]</span><span class="sxs-lookup"><span data-stu-id="6b95c-186">[!code-csharp[](app-secrets/samples/1.1/UserSecrets/Startup.cs?name=snippet_StartupConstructor&highlight=5-8)]</span></span>
::: moniker-end

<span data-ttu-id="6b95c-187">Klucze tajne użytkownika mogą zostać pobrane za pośrednictwem `Configuration` interfejsu API:</span><span class="sxs-lookup"><span data-stu-id="6b95c-187">User secrets can be retrieved via the `Configuration` API:</span></span>

::: moniker range="<= aspnetcore-1.1"
<span data-ttu-id="6b95c-188">[!code-csharp[](app-secrets/samples/1.1/UserSecrets/Startup.cs?name=snippet_StartupClass&highlight=23)]</span><span class="sxs-lookup"><span data-stu-id="6b95c-188">[!code-csharp[](app-secrets/samples/1.1/UserSecrets/Startup.cs?name=snippet_StartupClass&highlight=23)]</span></span>
::: moniker-end
::: moniker range=">= aspnetcore-2.0"
<span data-ttu-id="6b95c-189">[!code-csharp[](app-secrets/samples/2.1/UserSecrets/Startup.cs?name=snippet_StartupClass&highlight=14)]</span><span class="sxs-lookup"><span data-stu-id="6b95c-189">[!code-csharp[](app-secrets/samples/2.1/UserSecrets/Startup.cs?name=snippet_StartupClass&highlight=14)]</span></span>
::: moniker-end

## <a name="string-replacement-with-secrets"></a><span data-ttu-id="6b95c-190">Ciąg zastępczy z kluczy tajnych</span><span class="sxs-lookup"><span data-stu-id="6b95c-190">String replacement with secrets</span></span>

<span data-ttu-id="6b95c-191">Przechowywanie haseł w postaci zwykłego tekstu jest ryzykowne.</span><span class="sxs-lookup"><span data-stu-id="6b95c-191">Storing passwords in plain text is risky.</span></span> <span data-ttu-id="6b95c-192">Na przykład ciąg połączenia bazy danych są przechowywane w *appsettings.json* może zawierać hasła dla określonego użytkownika:</span><span class="sxs-lookup"><span data-stu-id="6b95c-192">For example, a database connection string stored in *appsettings.json* may include a password for the specified user:</span></span>

[!code-json[](app-secrets/samples/2.1/UserSecrets/appsettings-unsecure.json?highlight=3)]

<span data-ttu-id="6b95c-193">Bardziej bezpieczną metodą jest, aby zapisać hasło jako klucz tajny.</span><span class="sxs-lookup"><span data-stu-id="6b95c-193">A more secure approach is to store the password as a secret.</span></span> <span data-ttu-id="6b95c-194">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="6b95c-194">For example:</span></span>

```console
dotnet user-secrets set "DbPassword" "pass123"
```

<span data-ttu-id="6b95c-195">Zamień hasło w *appsettings.json* symbol zastępczy.</span><span class="sxs-lookup"><span data-stu-id="6b95c-195">Replace the password in *appsettings.json* with a placeholder.</span></span> <span data-ttu-id="6b95c-196">W poniższym przykładzie `{0}` jest używany jako symbol zastępczy do formularza [złożonego ciąg formatu](/dotnet/standard/base-types/composite-formatting#composite-format-string).</span><span class="sxs-lookup"><span data-stu-id="6b95c-196">In the following example, `{0}` is used as the placeholder to form a [Composite Format String](/dotnet/standard/base-types/composite-formatting#composite-format-string).</span></span>

[!code-json[](app-secrets/samples/2.1/UserSecrets/appsettings.json?highlight=3)]

<span data-ttu-id="6b95c-197">Wartość klucza tajnego mogą zostać dodane do symbolu zastępczego, aby ukończyć parametry połączenia:</span><span class="sxs-lookup"><span data-stu-id="6b95c-197">The secret's value can be injected into the placeholder to complete the connection string:</span></span>

::: moniker range="<= aspnetcore-1.1"
<span data-ttu-id="6b95c-198">[!code-csharp[](app-secrets/samples/1.1/UserSecrets/Startup2.cs?name=snippet_StartupClass&highlight=23-25)]</span><span class="sxs-lookup"><span data-stu-id="6b95c-198">[!code-csharp[](app-secrets/samples/1.1/UserSecrets/Startup2.cs?name=snippet_StartupClass&highlight=23-25)]</span></span>
::: moniker-end
::: moniker range=">= aspnetcore-2.0"
<span data-ttu-id="6b95c-199">[!code-csharp[](app-secrets/samples/2.1/UserSecrets/Startup2.cs?name=snippet_StartupClass&highlight=14-16)]</span><span class="sxs-lookup"><span data-stu-id="6b95c-199">[!code-csharp[](app-secrets/samples/2.1/UserSecrets/Startup2.cs?name=snippet_StartupClass&highlight=14-16)]</span></span>
::: moniker-end

## <a name="list-the-secrets"></a><span data-ttu-id="6b95c-200">Lista kluczy tajnych</span><span class="sxs-lookup"><span data-stu-id="6b95c-200">List the secrets</span></span>

[!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file-and-text.md)]

<span data-ttu-id="6b95c-201">Uruchom następujące polecenie z katalogu, w którym *.csproj* plik istnieje:</span><span class="sxs-lookup"><span data-stu-id="6b95c-201">Run the following command from the directory in which the *.csproj* file exists:</span></span>

```console
dotnet user-secrets list
```

<span data-ttu-id="6b95c-202">Zostaną wyświetlone następujące dane wyjściowe:</span><span class="sxs-lookup"><span data-stu-id="6b95c-202">The following output appears:</span></span>

```console
Movies:ServiceApiKey = 12345
Movies:ConnectionString = Server=(localdb)\mssqllocaldb;Database=Movie-1;Trusted_Connection=True;MultipleActiveResultSets=true
```

<span data-ttu-id="6b95c-203">W poprzednim przykładzie, dwukropek w nazwach kluczy oznacza hierarchii obiektów w ramach *secrets.json*.</span><span class="sxs-lookup"><span data-stu-id="6b95c-203">In the preceding example, a colon in the key names denotes the object hierarchy within *secrets.json*.</span></span>

## <a name="remove-a-single-secret"></a><span data-ttu-id="6b95c-204">Usuń jeden klucz tajny</span><span class="sxs-lookup"><span data-stu-id="6b95c-204">Remove a single secret</span></span>

[!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file-and-text.md)]

<span data-ttu-id="6b95c-205">Uruchom następujące polecenie z katalogu, w którym *.csproj* plik istnieje:</span><span class="sxs-lookup"><span data-stu-id="6b95c-205">Run the following command from the directory in which the *.csproj* file exists:</span></span>

```console
dotnet user-secrets remove "Movies:ConnectionString"
```

<span data-ttu-id="6b95c-206">Aplikacji *secrets.json* plik został zmodyfikowany, aby usunąć parę klucz wartość skojarzonych z `MoviesConnectionString` klucza:</span><span class="sxs-lookup"><span data-stu-id="6b95c-206">The app's *secrets.json* file was modified to remove the key-value pair associated with the `MoviesConnectionString` key:</span></span>

```json
{
  "Movies": {
    "ServiceApiKey": "12345"
  }
}
```

<span data-ttu-id="6b95c-207">Uruchomiona `dotnet user-secrets list` wyświetli następujący komunikat:</span><span class="sxs-lookup"><span data-stu-id="6b95c-207">Running `dotnet user-secrets list` displays the following message:</span></span>

```console
Movies:ServiceApiKey = 12345
```

## <a name="remove-all-secrets"></a><span data-ttu-id="6b95c-208">Usunięcie wszystkich kluczy tajnych</span><span class="sxs-lookup"><span data-stu-id="6b95c-208">Remove all secrets</span></span>

[!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file-and-text.md)]

<span data-ttu-id="6b95c-209">Uruchom następujące polecenie z katalogu, w którym *.csproj* plik istnieje:</span><span class="sxs-lookup"><span data-stu-id="6b95c-209">Run the following command from the directory in which the *.csproj* file exists:</span></span>

```console
dotnet user-secrets clear
```

<span data-ttu-id="6b95c-210">Usunięto wszystkie hasła użytkownika dla aplikacji z *secrets.json* pliku:</span><span class="sxs-lookup"><span data-stu-id="6b95c-210">All user secrets for the app have been deleted from the *secrets.json* file:</span></span>

```json
{}
```

<span data-ttu-id="6b95c-211">Uruchomiona `dotnet user-secrets list` wyświetli następujący komunikat:</span><span class="sxs-lookup"><span data-stu-id="6b95c-211">Running `dotnet user-secrets list` displays the following message:</span></span>

```console
No secrets configured for this application.
```

## <a name="additional-resources"></a><span data-ttu-id="6b95c-212">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="6b95c-212">Additional resources</span></span>

* <xref:fundamentals/configuration/index>
* <xref:security/key-vault-configuration>