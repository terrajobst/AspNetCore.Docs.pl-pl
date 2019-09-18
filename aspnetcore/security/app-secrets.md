---
title: Bezpieczne przechowywanie wpisów tajnych aplikacji podczas opracowywania w ASP.NET Core
author: rick-anderson
description: Dowiedz się, jak przechowywać i pobierać poufne informacje jako wpisy tajne aplikacji podczas opracowywania aplikacji ASP.NET Core.
ms.author: scaddie
ms.custom: mvc
ms.date: 03/13/2019
uid: security/app-secrets
ms.openlocfilehash: 0203a5737caf1af809b739d9e266a6971cd1523b
ms.sourcegitcommit: 215954a638d24124f791024c66fd4fb9109fd380
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/18/2019
ms.locfileid: "71080718"
---
# <a name="safe-storage-of-app-secrets-in-development-in-aspnet-core"></a><span data-ttu-id="eaaad-103">Bezpieczne przechowywanie wpisów tajnych aplikacji podczas opracowywania w ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="eaaad-103">Safe storage of app secrets in development in ASP.NET Core</span></span>

<span data-ttu-id="eaaad-104">[Rick Anderson](https://twitter.com/RickAndMSFT), [Daniel Roth](https://github.com/danroth27)i [Scott Addie](https://github.com/scottaddie)</span><span class="sxs-lookup"><span data-stu-id="eaaad-104">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Daniel Roth](https://github.com/danroth27), and [Scott Addie](https://github.com/scottaddie)</span></span>

<span data-ttu-id="eaaad-105">[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/app-secrets/samples) ([sposobu pobierania](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="eaaad-105">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/app-secrets/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="eaaad-106">W tym dokumencie opisano techniki przechowywania i pobierania poufnych danych podczas opracowywania aplikacji ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="eaaad-106">This document explains techniques for storing and retrieving sensitive data during the development of an ASP.NET Core app.</span></span> <span data-ttu-id="eaaad-107">Nie należy przechowywać haseł ani innych poufnych danych w kodzie źródłowym.</span><span class="sxs-lookup"><span data-stu-id="eaaad-107">Never store passwords or other sensitive data in source code.</span></span> <span data-ttu-id="eaaad-108">Tajemnice produkcyjne nie powinny być używane do celów deweloperskich i testowych.</span><span class="sxs-lookup"><span data-stu-id="eaaad-108">Production secrets shouldn't be used for development or test.</span></span> <span data-ttu-id="eaaad-109">Za pomocą [dostawcy konfiguracji Azure Key Vault](xref:security/key-vault-configuration)można przechowywać i chronić wpisy tajne środowiska Azure test i produkcyjne.</span><span class="sxs-lookup"><span data-stu-id="eaaad-109">You can store and protect Azure test and production secrets with the [Azure Key Vault configuration provider](xref:security/key-vault-configuration).</span></span>

## <a name="environment-variables"></a><span data-ttu-id="eaaad-110">Zmienne środowiskowe</span><span class="sxs-lookup"><span data-stu-id="eaaad-110">Environment variables</span></span>

<span data-ttu-id="eaaad-111">Zmienne środowiskowe służą do uniknięcia przechowywania wpisów tajnych aplikacji w kodzie lub w lokalnych plikach konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="eaaad-111">Environment variables are used to avoid storage of app secrets in code or in local configuration files.</span></span> <span data-ttu-id="eaaad-112">Zmienne środowiskowe przesłaniają wartości konfiguracyjne dla wszystkich poprzednio określonych źródeł konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="eaaad-112">Environment variables override configuration values for all previously specified configuration sources.</span></span>

::: moniker range="<= aspnetcore-1.1"

<span data-ttu-id="eaaad-113">Skonfiguruj Odczytywanie wartości zmiennych środowiskowych przez wywołanie <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*> `Startup` w konstruktorze:</span><span class="sxs-lookup"><span data-stu-id="eaaad-113">Configure the reading of environment variable values by calling <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*> in the `Startup` constructor:</span></span>

[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup.cs?name=snippet_StartupConstructor&highlight=8)]

::: moniker-end

<span data-ttu-id="eaaad-114">Rozważ ASP.NET Core aplikacji sieci Web, w której włączono zabezpieczenia **poszczególnych kont użytkowników** .</span><span class="sxs-lookup"><span data-stu-id="eaaad-114">Consider an ASP.NET Core web app in which **Individual User Accounts** security is enabled.</span></span> <span data-ttu-id="eaaad-115">Domyślne parametry połączenia z bazą danych są zawarte w pliku *appSettings. JSON* projektu z kluczem `DefaultConnection`.</span><span class="sxs-lookup"><span data-stu-id="eaaad-115">A default database connection string is included in the project's *appsettings.json* file with the key `DefaultConnection`.</span></span> <span data-ttu-id="eaaad-116">Domyślne parametry połączenia to LocalDB, które są uruchamiane w trybie użytkownika i nie wymagają hasła.</span><span class="sxs-lookup"><span data-stu-id="eaaad-116">The default connection string is for LocalDB, which runs in user mode and doesn't require a password.</span></span> <span data-ttu-id="eaaad-117">Podczas wdrażania `DefaultConnection` aplikacji wartość klucza może być zastąpiona wartością zmiennej środowiskowej.</span><span class="sxs-lookup"><span data-stu-id="eaaad-117">During app deployment, the `DefaultConnection` key value can be overridden with an environment variable's value.</span></span> <span data-ttu-id="eaaad-118">Zmienna środowiskowa może przechowywać kompletne parametry połączenia z poufnymi poświadczeniami.</span><span class="sxs-lookup"><span data-stu-id="eaaad-118">The environment variable may store the complete connection string with sensitive credentials.</span></span>

> [!WARNING]
> <span data-ttu-id="eaaad-119">Zmienne środowiskowe są zwykle przechowywane w postaci zwykłego, nieszyfrowanego tekstu.</span><span class="sxs-lookup"><span data-stu-id="eaaad-119">Environment variables are generally stored in plain, unencrypted text.</span></span> <span data-ttu-id="eaaad-120">W przypadku naruszenia zabezpieczeń komputera lub procesu zmienne środowiskowe są dostępne dla niezaufanych stron.</span><span class="sxs-lookup"><span data-stu-id="eaaad-120">If the machine or process is compromised, environment variables can be accessed by untrusted parties.</span></span> <span data-ttu-id="eaaad-121">Mogą być wymagane dodatkowe środki, które uniemożliwiają ujawnienie kluczy tajnych użytkownika.</span><span class="sxs-lookup"><span data-stu-id="eaaad-121">Additional measures to prevent disclosure of user secrets may be required.</span></span>

[!INCLUDE[](~/includes/environmentVarableColon.md)]

## <a name="secret-manager"></a><span data-ttu-id="eaaad-122">Menedżer wpisów tajnych</span><span class="sxs-lookup"><span data-stu-id="eaaad-122">Secret Manager</span></span>

<span data-ttu-id="eaaad-123">Narzędzie Secret Manager zapisuje poufne dane podczas opracowywania projektu ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="eaaad-123">The Secret Manager tool stores sensitive data during the development of an ASP.NET Core project.</span></span> <span data-ttu-id="eaaad-124">W tym kontekście dane poufne są tajne dla aplikacji.</span><span class="sxs-lookup"><span data-stu-id="eaaad-124">In this context, a piece of sensitive data is an app secret.</span></span> <span data-ttu-id="eaaad-125">Wpisy tajne aplikacji są przechowywane w oddzielnej lokalizacji w drzewie projektu.</span><span class="sxs-lookup"><span data-stu-id="eaaad-125">App secrets are stored in a separate location from the project tree.</span></span> <span data-ttu-id="eaaad-126">Wpisy tajne aplikacji są skojarzone z określonym projektem lub udostępniane w wielu projektach.</span><span class="sxs-lookup"><span data-stu-id="eaaad-126">The app secrets are associated with a specific project or shared across several projects.</span></span> <span data-ttu-id="eaaad-127">Wpisy tajne aplikacji nie są sprawdzane w kontroli źródła.</span><span class="sxs-lookup"><span data-stu-id="eaaad-127">The app secrets aren't checked into source control.</span></span>

> [!WARNING]
> <span data-ttu-id="eaaad-128">Narzędzie Secret Manager nie szyfruje przechowywanych wpisów tajnych i nie powinna być traktowana jako zaufany magazyn.</span><span class="sxs-lookup"><span data-stu-id="eaaad-128">The Secret Manager tool doesn't encrypt the stored secrets and shouldn't be treated as a trusted store.</span></span> <span data-ttu-id="eaaad-129">Jest przeznaczona tylko do celów programistycznych.</span><span class="sxs-lookup"><span data-stu-id="eaaad-129">It's for development purposes only.</span></span> <span data-ttu-id="eaaad-130">Klucze i wartości są przechowywane w pliku konfiguracji JSON w katalogu profilu użytkownika.</span><span class="sxs-lookup"><span data-stu-id="eaaad-130">The keys and values are stored in a JSON configuration file in the user profile directory.</span></span>

## <a name="how-the-secret-manager-tool-works"></a><span data-ttu-id="eaaad-131">Jak działa narzędzie do zarządzania kluczami tajnymi</span><span class="sxs-lookup"><span data-stu-id="eaaad-131">How the Secret Manager tool works</span></span>

<span data-ttu-id="eaaad-132">Narzędzie tajnego Menedżera wyodrębnia szczegóły implementacji, takie jak miejsce i sposób przechowywania wartości.</span><span class="sxs-lookup"><span data-stu-id="eaaad-132">The Secret Manager tool abstracts away the implementation details, such as where and how the values are stored.</span></span> <span data-ttu-id="eaaad-133">Możesz użyć narzędzia bez znajomości tych szczegółów implementacji.</span><span class="sxs-lookup"><span data-stu-id="eaaad-133">You can use the tool without knowing these implementation details.</span></span> <span data-ttu-id="eaaad-134">Wartości są przechowywane w pliku konfiguracji JSON w folderze profilu użytkownika chronionego przez system na komputerze lokalnym:</span><span class="sxs-lookup"><span data-stu-id="eaaad-134">The values are stored in a JSON configuration file in a system-protected user profile folder on the local machine:</span></span>

# <a name="windowstabwindows"></a>[<span data-ttu-id="eaaad-135">Windows</span><span class="sxs-lookup"><span data-stu-id="eaaad-135">Windows</span></span>](#tab/windows)

<span data-ttu-id="eaaad-136">Ścieżka systemu plików:</span><span class="sxs-lookup"><span data-stu-id="eaaad-136">File system path:</span></span>

`%APPDATA%\Microsoft\UserSecrets\<user_secrets_id>\secrets.json`

# <a name="linux--macostablinuxmacos"></a>[<span data-ttu-id="eaaad-137">Linux/macOS</span><span class="sxs-lookup"><span data-stu-id="eaaad-137">Linux / macOS</span></span>](#tab/linux+macos)

<span data-ttu-id="eaaad-138">Ścieżka systemu plików:</span><span class="sxs-lookup"><span data-stu-id="eaaad-138">File system path:</span></span>

`~/.microsoft/usersecrets/<user_secrets_id>/secrets.json`

---

<span data-ttu-id="eaaad-139">W poprzednich ścieżkach plików Zamień `<user_secrets_id>` `UserSecretsId` na wartość określoną w pliku *. csproj* .</span><span class="sxs-lookup"><span data-stu-id="eaaad-139">In the preceding file paths, replace `<user_secrets_id>` with the `UserSecretsId` value specified in the *.csproj* file.</span></span>

<span data-ttu-id="eaaad-140">Nie pisz kodu, który zależy od lokalizacji lub formatu danych zapisywanych za pomocą narzędzia do zarządzania kluczami tajnymi.</span><span class="sxs-lookup"><span data-stu-id="eaaad-140">Don't write code that depends on the location or format of data saved with the Secret Manager tool.</span></span> <span data-ttu-id="eaaad-141">Te szczegóły implementacji mogą ulec zmianie.</span><span class="sxs-lookup"><span data-stu-id="eaaad-141">These implementation details may change.</span></span> <span data-ttu-id="eaaad-142">Na przykład wartości tajne nie są szyfrowane, ale mogą być w przyszłości.</span><span class="sxs-lookup"><span data-stu-id="eaaad-142">For example, the secret values aren't encrypted, but could be in the future.</span></span>

::: moniker range="<= aspnetcore-2.0"

## <a name="install-the-secret-manager-tool"></a><span data-ttu-id="eaaad-143">Instalowanie narzędzia do zarządzania kluczami tajnymi</span><span class="sxs-lookup"><span data-stu-id="eaaad-143">Install the Secret Manager tool</span></span>

<span data-ttu-id="eaaad-144">Narzędzie Secret Manager jest powiązane z interfejs wiersza polecenia platformy .NET Core w zestaw .NET Core SDK 2.1.300 lub nowszym.</span><span class="sxs-lookup"><span data-stu-id="eaaad-144">The Secret Manager tool is bundled with the .NET Core CLI in .NET Core SDK 2.1.300 or later.</span></span> <span data-ttu-id="eaaad-145">W przypadku wersji zestaw .NET Core SDK przed 2.1.300 należy zainstalować narzędzie.</span><span class="sxs-lookup"><span data-stu-id="eaaad-145">For .NET Core SDK versions before 2.1.300, tool installation is necessary.</span></span>

> [!TIP]
> <span data-ttu-id="eaaad-146">Uruchom `dotnet --version` polecenie w powłoce poleceń, aby zobaczyć zainstalowany zestaw .NET Core SDK numer wersji.</span><span class="sxs-lookup"><span data-stu-id="eaaad-146">Run `dotnet --version` from a command shell to see the installed .NET Core SDK version number.</span></span>

<span data-ttu-id="eaaad-147">Ostrzeżenie jest wyświetlane, jeśli używany zestaw .NET Core SDK zawiera narzędzie:</span><span class="sxs-lookup"><span data-stu-id="eaaad-147">A warning is displayed if the .NET Core SDK being used includes the tool:</span></span>

```console
The tool 'Microsoft.Extensions.SecretManager.Tools' is now included in the .NET Core SDK. Information on resolving this warning is available at (https://aka.ms/dotnetclitools-in-box).
```

<span data-ttu-id="eaaad-148">Zainstaluj pakiet NuGet [Microsoft. Extensions. SecretManager. Tools](https://www.nuget.org/packages/Microsoft.Extensions.SecretManager.Tools/) w projekcie ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="eaaad-148">Install the [Microsoft.Extensions.SecretManager.Tools](https://www.nuget.org/packages/Microsoft.Extensions.SecretManager.Tools/) NuGet package in your ASP.NET Core project.</span></span> <span data-ttu-id="eaaad-149">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="eaaad-149">For example:</span></span>

[!code-xml[](app-secrets/samples/1.x/UserSecrets/UserSecrets.csproj?name=snippet_CsprojFile&highlight=15-16)]

<span data-ttu-id="eaaad-150">Aby sprawdzić poprawność instalacji narzędzi, wykonaj następujące polecenie w powłoce poleceń:</span><span class="sxs-lookup"><span data-stu-id="eaaad-150">Execute the following command in a command shell to validate the tool installation:</span></span>

```dotnetcli
dotnet user-secrets -h
```

<span data-ttu-id="eaaad-151">Narzędzie Secret Manager wyświetla przykładowe użycie, opcje i pomoc poleceń:</span><span class="sxs-lookup"><span data-stu-id="eaaad-151">The Secret Manager tool displays sample usage, options, and command help:</span></span>

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
> <span data-ttu-id="eaaad-152">Musisz być w tym samym katalogu, co plik *. csproj* , aby uruchamiać narzędzia zdefiniowane w `DotNetCliToolReference` elementach *. csproj* pliku.</span><span class="sxs-lookup"><span data-stu-id="eaaad-152">You must be in the same directory as the *.csproj* file to run tools defined in the *.csproj* file's `DotNetCliToolReference` elements.</span></span>

::: moniker-end

## <a name="enable-secret-storage"></a><span data-ttu-id="eaaad-153">Włącz magazyn tajny</span><span class="sxs-lookup"><span data-stu-id="eaaad-153">Enable secret storage</span></span>

<span data-ttu-id="eaaad-154">Narzędzie Secret Manager działa na specyficznych dla projektu ustawieniach konfiguracji przechowywanych w profilu użytkownika.</span><span class="sxs-lookup"><span data-stu-id="eaaad-154">The Secret Manager tool operates on project-specific configuration settings stored in your user profile.</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="eaaad-155">Narzędzie Secret Manager zawiera `init` polecenie w zestaw .NET Core SDK 3.0.100 lub nowszym.</span><span class="sxs-lookup"><span data-stu-id="eaaad-155">The Secret Manager tool includes an `init` command in .NET Core SDK 3.0.100 or later.</span></span> <span data-ttu-id="eaaad-156">Aby użyć kluczy tajnych użytkownika, uruchom następujące polecenie w katalogu projektu:</span><span class="sxs-lookup"><span data-stu-id="eaaad-156">To use user secrets, run the following command in the project directory:</span></span>

```dotnetcli
dotnet user-secrets init
```

<span data-ttu-id="eaaad-157">Poprzednie polecenie dodaje `UserSecretsId` element `PropertyGroup` w pliku *. csproj* .</span><span class="sxs-lookup"><span data-stu-id="eaaad-157">The preceding command adds a `UserSecretsId` element within a `PropertyGroup` of the *.csproj* file.</span></span> <span data-ttu-id="eaaad-158">Domyślnie tekst `UserSecretsId` wewnętrzny jest identyfikatorem GUID.</span><span class="sxs-lookup"><span data-stu-id="eaaad-158">By default, the inner text of `UserSecretsId` is a GUID.</span></span> <span data-ttu-id="eaaad-159">Wewnętrzny tekst jest dowolny, ale jest unikatowy dla projektu.</span><span class="sxs-lookup"><span data-stu-id="eaaad-159">The inner text is arbitrary, but is unique to the project.</span></span>

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

<span data-ttu-id="eaaad-160">Aby użyć kluczy tajnych użytkownika, `UserSecretsId` Zdefiniuj element `PropertyGroup` w pliku *. csproj* .</span><span class="sxs-lookup"><span data-stu-id="eaaad-160">To use user secrets, define a `UserSecretsId` element within a `PropertyGroup` of the *.csproj* file.</span></span> <span data-ttu-id="eaaad-161">Wewnętrzny tekst `UserSecretsId` jest dowolny, ale jest unikatowy dla projektu.</span><span class="sxs-lookup"><span data-stu-id="eaaad-161">The inner text of `UserSecretsId` is arbitrary, but is unique to the project.</span></span> <span data-ttu-id="eaaad-162">Deweloperzy zwykle generują identyfikator GUID dla `UserSecretsId`.</span><span class="sxs-lookup"><span data-stu-id="eaaad-162">Developers typically generate a GUID for the `UserSecretsId`.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

[!code-xml[](app-secrets/samples/2.x/UserSecrets/UserSecrets.csproj?name=snippet_PropertyGroup&highlight=3)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-xml[](app-secrets/samples/1.x/UserSecrets/UserSecrets.csproj?name=snippet_PropertyGroup&highlight=3)]

::: moniker-end

> [!TIP]
> <span data-ttu-id="eaaad-163">W programie Visual Studio kliknij prawym przyciskiem myszy projekt w Eksplorator rozwiązań i wybierz polecenie **Zarządzaj kluczami tajnymi użytkownika** z menu kontekstowego.</span><span class="sxs-lookup"><span data-stu-id="eaaad-163">In Visual Studio, right-click the project in Solution Explorer, and select **Manage User Secrets** from the context menu.</span></span> <span data-ttu-id="eaaad-164">Ten gest dodaje `UserSecretsId` element z identyfikatorem GUID do pliku *. csproj* .</span><span class="sxs-lookup"><span data-stu-id="eaaad-164">This gesture adds a `UserSecretsId` element, populated with a GUID, to the *.csproj* file.</span></span>

## <a name="set-a-secret"></a><span data-ttu-id="eaaad-165">Ustaw klucz tajny</span><span class="sxs-lookup"><span data-stu-id="eaaad-165">Set a secret</span></span>

<span data-ttu-id="eaaad-166">Zdefiniuj klucz tajny aplikacji składający się z klucza i jego wartości.</span><span class="sxs-lookup"><span data-stu-id="eaaad-166">Define an app secret consisting of a key and its value.</span></span> <span data-ttu-id="eaaad-167">Wpis tajny jest skojarzony z `UserSecretsId` wartością projektu.</span><span class="sxs-lookup"><span data-stu-id="eaaad-167">The secret is associated with the project's `UserSecretsId` value.</span></span> <span data-ttu-id="eaaad-168">Na przykład uruchom następujące polecenie z katalogu, w którym istnieje plik *. csproj* :</span><span class="sxs-lookup"><span data-stu-id="eaaad-168">For example, run the following command from the directory in which the *.csproj* file exists:</span></span>

```dotnetcli
dotnet user-secrets set "Movies:ServiceApiKey" "12345"
```

<span data-ttu-id="eaaad-169">W poprzednim przykładzie dwukropek wskazuje, że `Movies` jest to literał obiektu `ServiceApiKey` z właściwością.</span><span class="sxs-lookup"><span data-stu-id="eaaad-169">In the preceding example, the colon denotes that `Movies` is an object literal with a `ServiceApiKey` property.</span></span>

<span data-ttu-id="eaaad-170">Narzędzia do zarządzania kluczami tajnymi można również użyć z innych katalogów.</span><span class="sxs-lookup"><span data-stu-id="eaaad-170">The Secret Manager tool can be used from other directories too.</span></span> <span data-ttu-id="eaaad-171">Użyj opcji, aby podać ścieżkę systemu plików, w której znajduje się plik *. csproj.* `--project`</span><span class="sxs-lookup"><span data-stu-id="eaaad-171">Use the `--project` option to supply the file system path at which the *.csproj* file exists.</span></span> <span data-ttu-id="eaaad-172">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="eaaad-172">For example:</span></span>

```dotnetcli
dotnet user-secrets set "Movies:ServiceApiKey" "12345" --project "C:\apps\WebApp1\src\WebApp1"
```

### <a name="json-structure-flattening-in-visual-studio"></a><span data-ttu-id="eaaad-173">Spłaszczanie struktury JSON w programie Visual Studio</span><span class="sxs-lookup"><span data-stu-id="eaaad-173">JSON structure flattening in Visual Studio</span></span>

<span data-ttu-id="eaaad-174">Gest **Zarządzanie kluczami tajnymi użytkownika** programu Visual Studio otwiera plik Secret *. JSON* w edytorze tekstów.</span><span class="sxs-lookup"><span data-stu-id="eaaad-174">Visual Studio's **Manage User Secrets** gesture opens a *secrets.json* file in the text editor.</span></span> <span data-ttu-id="eaaad-175">Zastąp zawartość Secret *. JSON* wartościami par klucz-wartość, które mają być przechowywane.</span><span class="sxs-lookup"><span data-stu-id="eaaad-175">Replace the contents of *secrets.json* with the key-value pairs to be stored.</span></span> <span data-ttu-id="eaaad-176">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="eaaad-176">For example:</span></span>

```json
{
  "Movies": {
    "ConnectionString": "Server=(localdb)\\mssqllocaldb;Database=Movie-1;Trusted_Connection=True;MultipleActiveResultSets=true",
    "ServiceApiKey": "12345"
  }
}
```

<span data-ttu-id="eaaad-177">Struktura JSON jest spłaszczona po modyfikacji za `dotnet user-secrets remove` pośrednictwem `dotnet user-secrets set`lub.</span><span class="sxs-lookup"><span data-stu-id="eaaad-177">The JSON structure is flattened after modifications via `dotnet user-secrets remove` or `dotnet user-secrets set`.</span></span> <span data-ttu-id="eaaad-178">Na przykład, uruchomione `dotnet user-secrets remove "Movies:ConnectionString"` zwijanie `Movies` literału obiektu.</span><span class="sxs-lookup"><span data-stu-id="eaaad-178">For example, running `dotnet user-secrets remove "Movies:ConnectionString"` collapses the `Movies` object literal.</span></span> <span data-ttu-id="eaaad-179">Zmodyfikowany plik jest podobny do następującego:</span><span class="sxs-lookup"><span data-stu-id="eaaad-179">The modified file resembles the following:</span></span>

```json
{
  "Movies:ServiceApiKey": "12345"
}
```

## <a name="set-multiple-secrets"></a><span data-ttu-id="eaaad-180">Ustawianie wielu wpisów tajnych</span><span class="sxs-lookup"><span data-stu-id="eaaad-180">Set multiple secrets</span></span>

<span data-ttu-id="eaaad-181">Partia wpisów tajnych można ustawić za pomocą formatu JSON dla `set` polecenia.</span><span class="sxs-lookup"><span data-stu-id="eaaad-181">A batch of secrets can be set by piping JSON to the `set` command.</span></span> <span data-ttu-id="eaaad-182">W poniższym przykładzie zawartość pliku *Input. JSON* jest potoku do `set` polecenia.</span><span class="sxs-lookup"><span data-stu-id="eaaad-182">In the following example, the *input.json* file's contents are piped to the `set` command.</span></span>

# <a name="windowstabwindows"></a>[<span data-ttu-id="eaaad-183">Windows</span><span class="sxs-lookup"><span data-stu-id="eaaad-183">Windows</span></span>](#tab/windows)

<span data-ttu-id="eaaad-184">Otwórz powłokę poleceń i wykonaj następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="eaaad-184">Open a command shell, and execute the following command:</span></span>

  ```dotnetcli
  type .\input.json | dotnet user-secrets set
  ```

# <a name="linux--macostablinuxmacos"></a>[<span data-ttu-id="eaaad-185">Linux/macOS</span><span class="sxs-lookup"><span data-stu-id="eaaad-185">Linux / macOS</span></span>](#tab/linux+macos)

<span data-ttu-id="eaaad-186">Otwórz powłokę poleceń i wykonaj następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="eaaad-186">Open a command shell, and execute the following command:</span></span>

  ```dotnetcli
  cat ./input.json | dotnet user-secrets set
  ```

---

## <a name="access-a-secret"></a><span data-ttu-id="eaaad-187">Dostęp do klucza tajnego</span><span class="sxs-lookup"><span data-stu-id="eaaad-187">Access a secret</span></span>

<span data-ttu-id="eaaad-188">[Interfejs API konfiguracji ASP.NET Core](xref:fundamentals/configuration/index) zapewnia dostęp do wpisów tajnych usługi Secret Manager.</span><span class="sxs-lookup"><span data-stu-id="eaaad-188">The [ASP.NET Core Configuration API](xref:fundamentals/configuration/index) provides access to Secret Manager secrets.</span></span>

::: moniker range=">= aspnetcore-2.0 <= aspnetcore-2.2"

<span data-ttu-id="eaaad-189">Jeśli projekt jest przeznaczony .NET Framework, zainstaluj pakiet NuGet [Microsoft. Extensions. Configuration. UserSecrets](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.UserSecrets) .</span><span class="sxs-lookup"><span data-stu-id="eaaad-189">If your project targets .NET Framework, install the [Microsoft.Extensions.Configuration.UserSecrets](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.UserSecrets) NuGet package.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="eaaad-190">W ASP.NET Core 2,0 lub nowszej Źródło konfiguracji użytkownika jest automatycznie dodawane w trybie programistycznym, gdy projekt wywoła <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> nowe wystąpienie hosta ze wstępnie skonfigurowanymi ustawieniami domyślnymi.</span><span class="sxs-lookup"><span data-stu-id="eaaad-190">In ASP.NET Core 2.0 or later, the user secrets configuration source is automatically added in development mode when the project calls <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> to initialize a new instance of the host with preconfigured defaults.</span></span> <span data-ttu-id="eaaad-191">`CreateDefaultBuilder`wywołuje <xref:Microsoft.Extensions.Configuration.UserSecretsConfigurationExtensions.AddUserSecrets*> się, <xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment.EnvironmentName> gdy <xref:Microsoft.AspNetCore.Hosting.EnvironmentName.Development>jest:</span><span class="sxs-lookup"><span data-stu-id="eaaad-191">`CreateDefaultBuilder` calls <xref:Microsoft.Extensions.Configuration.UserSecretsConfigurationExtensions.AddUserSecrets*> when the <xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment.EnvironmentName> is <xref:Microsoft.AspNetCore.Hosting.EnvironmentName.Development>:</span></span>

[!code-csharp[](app-secrets/samples/2.x/UserSecrets/Program.cs?name=snippet_CreateWebHostBuilder&highlight=2)]

<span data-ttu-id="eaaad-192">Gdy `CreateDefaultBuilder` nie jest wywoływana, Dodaj źródło konfiguracji kluczy tajnych użytkownika jawnie <xref:Microsoft.Extensions.Configuration.UserSecretsConfigurationExtensions.AddUserSecrets*> , wywołując `Startup` w konstruktorze.</span><span class="sxs-lookup"><span data-stu-id="eaaad-192">When `CreateDefaultBuilder` isn't called, add the user secrets configuration source explicitly by calling <xref:Microsoft.Extensions.Configuration.UserSecretsConfigurationExtensions.AddUserSecrets*> in the `Startup` constructor.</span></span> <span data-ttu-id="eaaad-193">Wywołanie `AddUserSecrets` tylko wtedy, gdy aplikacja działa w środowisku deweloperskim, jak pokazano w następującym przykładzie:</span><span class="sxs-lookup"><span data-stu-id="eaaad-193">Call `AddUserSecrets` only when the app runs in the Development environment, as shown in the following example:</span></span>

[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup.cs?name=snippet_StartupConstructor&highlight=12)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

<span data-ttu-id="eaaad-194">Zainstaluj pakiet NuGet [Microsoft. Extensions. Configuration. UserSecrets](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.UserSecrets) .</span><span class="sxs-lookup"><span data-stu-id="eaaad-194">Install the [Microsoft.Extensions.Configuration.UserSecrets](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.UserSecrets) NuGet package.</span></span>

<span data-ttu-id="eaaad-195">Dodaj źródło konfiguracji kluczy tajnych użytkownika z wywołaniem <xref:Microsoft.Extensions.Configuration.UserSecretsConfigurationExtensions.AddUserSecrets*> `Startup` w konstruktorze:</span><span class="sxs-lookup"><span data-stu-id="eaaad-195">Add the user secrets configuration source with a call to <xref:Microsoft.Extensions.Configuration.UserSecretsConfigurationExtensions.AddUserSecrets*> in the `Startup` constructor:</span></span>

[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup.cs?name=snippet_StartupConstructor&highlight=12)]

::: moniker-end

<span data-ttu-id="eaaad-196">Wpisy tajne użytkownika można pobrać za `Configuration` pośrednictwem interfejsu API:</span><span class="sxs-lookup"><span data-stu-id="eaaad-196">User secrets can be retrieved via the `Configuration` API:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](app-secrets/samples/2.x/UserSecrets/Startup.cs?name=snippet_StartupClass&highlight=14)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup.cs?name=snippet_StartupClass&highlight=26)]

::: moniker-end

## <a name="map-secrets-to-a-poco"></a><span data-ttu-id="eaaad-197">Mapuj wpisy tajne do POCO</span><span class="sxs-lookup"><span data-stu-id="eaaad-197">Map secrets to a POCO</span></span>

<span data-ttu-id="eaaad-198">Mapowanie całego literału obiektu na POCO (prosta Klasa .NET z właściwościami) jest przydatne do agregowania powiązanych właściwości.</span><span class="sxs-lookup"><span data-stu-id="eaaad-198">Mapping an entire object literal to a POCO (a simple .NET class with properties) is useful for aggregating related properties.</span></span>

[!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file-and-text.md)]

<span data-ttu-id="eaaad-199">Aby zmapować poprzednie wpisy tajne do poco, użyj `Configuration` funkcji [powiązania grafu obiektów](xref:fundamentals/configuration/index#bind-to-an-object-graph) interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="eaaad-199">To map the preceding secrets to a POCO, use the `Configuration` API's [object graph binding](xref:fundamentals/configuration/index#bind-to-an-object-graph) feature.</span></span> <span data-ttu-id="eaaad-200">Następujący kod wiąże się z niestandardowym `MovieSettings` POCO i uzyskuje dostęp `ServiceApiKey` do wartości właściwości:</span><span class="sxs-lookup"><span data-stu-id="eaaad-200">The following code binds to a custom `MovieSettings` POCO and accesses the `ServiceApiKey` property value:</span></span>

::: moniker range=">= aspnetcore-1.1"

[!code-csharp[](app-secrets/samples/2.x/UserSecrets/Startup3.cs?name=snippet_BindToObjectGraph)]

::: moniker-end

::: moniker range="= aspnetcore-1.0"

[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup3.cs?name=snippet_BindToObjectGraph)]

::: moniker-end

<span data-ttu-id="eaaad-201">Wpisy tajne `Movies:ServiceApiKey`isą mapowane na odpowiednie właściwości w `MovieSettings`programie: `Movies:ConnectionString`</span><span class="sxs-lookup"><span data-stu-id="eaaad-201">The `Movies:ConnectionString` and `Movies:ServiceApiKey` secrets are mapped to the respective properties in `MovieSettings`:</span></span>

[!code-csharp[](app-secrets/samples/2.x/UserSecrets/Models/MovieSettings.cs?name=snippet_MovieSettingsClass)]

## <a name="string-replacement-with-secrets"></a><span data-ttu-id="eaaad-202">Zastępowanie ciągów hasłami tajnymi</span><span class="sxs-lookup"><span data-stu-id="eaaad-202">String replacement with secrets</span></span>

<span data-ttu-id="eaaad-203">Przechowywanie haseł w postaci zwykłego tekstu jest niebezpieczne.</span><span class="sxs-lookup"><span data-stu-id="eaaad-203">Storing passwords in plain text is insecure.</span></span> <span data-ttu-id="eaaad-204">Na przykład parametry połączenia z bazą danych przechowywane w pliku *appSettings. JSON* mogą zawierać hasło dla określonego użytkownika:</span><span class="sxs-lookup"><span data-stu-id="eaaad-204">For example, a database connection string stored in *appsettings.json* may include a password for the specified user:</span></span>

[!code-json[](app-secrets/samples/2.x/UserSecrets/appsettings-unsecure.json?highlight=3)]

<span data-ttu-id="eaaad-205">Bardziej bezpiecznym podejściem jest przechowywanie hasła jako klucza tajnego.</span><span class="sxs-lookup"><span data-stu-id="eaaad-205">A more secure approach is to store the password as a secret.</span></span> <span data-ttu-id="eaaad-206">Przykład:</span><span class="sxs-lookup"><span data-stu-id="eaaad-206">For example:</span></span>

```dotnetcli
dotnet user-secrets set "DbPassword" "pass123"
```

<span data-ttu-id="eaaad-207">Usuń parę klucz-wartość z parametrów połączenia w pliku *appSettings. JSON.* `Password`</span><span class="sxs-lookup"><span data-stu-id="eaaad-207">Remove the `Password` key-value pair from the connection string in *appsettings.json*.</span></span> <span data-ttu-id="eaaad-208">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="eaaad-208">For example:</span></span>

[!code-json[](app-secrets/samples/2.x/UserSecrets/appsettings.json?highlight=3)]

<span data-ttu-id="eaaad-209">Wartość wpisu tajnego można ustawić we <xref:System.Data.SqlClient.SqlConnectionStringBuilder> <xref:System.Data.SqlClient.SqlConnectionStringBuilder.Password*> właściwości obiektu, aby zakończyć parametry połączenia:</span><span class="sxs-lookup"><span data-stu-id="eaaad-209">The secret's value can be set on a <xref:System.Data.SqlClient.SqlConnectionStringBuilder> object's <xref:System.Data.SqlClient.SqlConnectionStringBuilder.Password*> property to complete the connection string:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](app-secrets/samples/2.x/UserSecrets/Startup2.cs?name=snippet_StartupClass&highlight=14-17)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup2.cs?name=snippet_StartupClass&highlight=26-29)]

::: moniker-end

## <a name="list-the-secrets"></a><span data-ttu-id="eaaad-210">Wystaw wpisy tajne</span><span class="sxs-lookup"><span data-stu-id="eaaad-210">List the secrets</span></span>

[!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file-and-text.md)]

<span data-ttu-id="eaaad-211">Uruchom następujące polecenie z katalogu, w którym istnieje plik *. csproj* :</span><span class="sxs-lookup"><span data-stu-id="eaaad-211">Run the following command from the directory in which the *.csproj* file exists:</span></span>

```dotnetcli
dotnet user-secrets list
```

<span data-ttu-id="eaaad-212">Wyświetlane są następujące dane wyjściowe:</span><span class="sxs-lookup"><span data-stu-id="eaaad-212">The following output appears:</span></span>

```console
Movies:ConnectionString = Server=(localdb)\mssqllocaldb;Database=Movie-1;Trusted_Connection=True;MultipleActiveResultSets=true
Movies:ServiceApiKey = 12345
```

<span data-ttu-id="eaaad-213">W poprzednim przykładzie dwukropek w nazwach kluczy oznacza hierarchię obiektów w formacie Secret *. JSON*.</span><span class="sxs-lookup"><span data-stu-id="eaaad-213">In the preceding example, a colon in the key names denotes the object hierarchy within *secrets.json*.</span></span>

## <a name="remove-a-single-secret"></a><span data-ttu-id="eaaad-214">Usuń pojedynczy klucz tajny</span><span class="sxs-lookup"><span data-stu-id="eaaad-214">Remove a single secret</span></span>

[!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file-and-text.md)]

<span data-ttu-id="eaaad-215">Uruchom następujące polecenie z katalogu, w którym istnieje plik *. csproj* :</span><span class="sxs-lookup"><span data-stu-id="eaaad-215">Run the following command from the directory in which the *.csproj* file exists:</span></span>

```dotnetcli
dotnet user-secrets remove "Movies:ConnectionString"
```

<span data-ttu-id="eaaad-216">Plik Secret *. JSON* aplikacji został zmodyfikowany, aby usunąć parę klucz-wartość skojarzoną z `MoviesConnectionString` kluczem:</span><span class="sxs-lookup"><span data-stu-id="eaaad-216">The app's *secrets.json* file was modified to remove the key-value pair associated with the `MoviesConnectionString` key:</span></span>

```json
{
  "Movies": {
    "ServiceApiKey": "12345"
  }
}
```

<span data-ttu-id="eaaad-217">Uruchomiony `dotnet user-secrets list` program wyświetla następujący komunikat:</span><span class="sxs-lookup"><span data-stu-id="eaaad-217">Running `dotnet user-secrets list` displays the following message:</span></span>

```console
Movies:ServiceApiKey = 12345
```

## <a name="remove-all-secrets"></a><span data-ttu-id="eaaad-218">Usuń wszystkie wpisy tajne</span><span class="sxs-lookup"><span data-stu-id="eaaad-218">Remove all secrets</span></span>

[!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file-and-text.md)]

<span data-ttu-id="eaaad-219">Uruchom następujące polecenie z katalogu, w którym istnieje plik *. csproj* :</span><span class="sxs-lookup"><span data-stu-id="eaaad-219">Run the following command from the directory in which the *.csproj* file exists:</span></span>

```dotnetcli
dotnet user-secrets clear
```

<span data-ttu-id="eaaad-220">Wszystkie wpisy tajne użytkownika dla aplikacji zostały usunięte z pliku Secret *. JSON* :</span><span class="sxs-lookup"><span data-stu-id="eaaad-220">All user secrets for the app have been deleted from the *secrets.json* file:</span></span>

```json
{}
```

<span data-ttu-id="eaaad-221">Uruchomiony `dotnet user-secrets list` program wyświetla następujący komunikat:</span><span class="sxs-lookup"><span data-stu-id="eaaad-221">Running `dotnet user-secrets list` displays the following message:</span></span>

```console
No secrets configured for this application.
```

## <a name="additional-resources"></a><span data-ttu-id="eaaad-222">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="eaaad-222">Additional resources</span></span>

* <xref:fundamentals/configuration/index>
* <xref:security/key-vault-configuration>
