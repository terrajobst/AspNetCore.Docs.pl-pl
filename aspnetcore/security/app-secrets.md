---
title: Bezpieczne przechowywanie wpisów tajnych aplikacji podczas opracowywania w ASP.NET Core
author: rick-anderson
description: Dowiedz się, jak przechowywać i pobierać poufne informacje jako wpisy tajne aplikacji podczas opracowywania aplikacji ASP.NET Core.
ms.author: scaddie
ms.custom: mvc
ms.date: 12/05/2019
uid: security/app-secrets
ms.openlocfilehash: 9b36ae64fbe277cd81ed22ba7b21b0a035082dbd
ms.sourcegitcommit: c815a9465e7b1bab44ce1643ec345b33e6cf1598
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/02/2020
ms.locfileid: "75606795"
---
# <a name="safe-storage-of-app-secrets-in-development-in-aspnet-core"></a><span data-ttu-id="f4c50-103">Bezpieczne przechowywanie wpisów tajnych aplikacji podczas opracowywania w ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="f4c50-103">Safe storage of app secrets in development in ASP.NET Core</span></span>

<span data-ttu-id="f4c50-104">[Rick Anderson](https://twitter.com/RickAndMSFT), [Daniel Roth](https://github.com/danroth27)i [Scott Addie](https://github.com/scottaddie)</span><span class="sxs-lookup"><span data-stu-id="f4c50-104">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Daniel Roth](https://github.com/danroth27), and [Scott Addie](https://github.com/scottaddie)</span></span>

<span data-ttu-id="f4c50-105">[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/app-secrets/samples) ([sposobu pobierania](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="f4c50-105">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/app-secrets/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="f4c50-106">W tym dokumencie opisano techniki przechowywania i pobierania poufnych danych podczas opracowywania aplikacji ASP.NET Core na komputerze deweloperskim.</span><span class="sxs-lookup"><span data-stu-id="f4c50-106">This document explains techniques for storing and retrieving sensitive data during development of an ASP.NET Core app on a development machine.</span></span> <span data-ttu-id="f4c50-107">Nie należy przechowywać haseł ani innych poufnych danych w kodzie źródłowym.</span><span class="sxs-lookup"><span data-stu-id="f4c50-107">Never store passwords or other sensitive data in source code.</span></span> <span data-ttu-id="f4c50-108">Tajemnice produkcyjne nie powinny być używane do celów deweloperskich i testowych.</span><span class="sxs-lookup"><span data-stu-id="f4c50-108">Production secrets shouldn't be used for development or test.</span></span> <span data-ttu-id="f4c50-109">Wpisy tajne nie powinny być wdrażane przy użyciu aplikacji.</span><span class="sxs-lookup"><span data-stu-id="f4c50-109">Secrets shouldn't be deployed with the app.</span></span> <span data-ttu-id="f4c50-110">Zamiast tego należy udostępnić wpisy tajne w środowisku produkcyjnym za pomocą kontrolowanych środków, takich jak zmienne środowiskowe, Azure Key Vault itd. Za pomocą [dostawcy konfiguracji Azure Key Vault](xref:security/key-vault-configuration)można przechowywać i chronić wpisy tajne środowiska Azure test i produkcyjne.</span><span class="sxs-lookup"><span data-stu-id="f4c50-110">Instead, secrets should be made available in the production environment through a controlled means like environment variables, Azure Key Vault, etc. You can store and protect Azure test and production secrets with the [Azure Key Vault configuration provider](xref:security/key-vault-configuration).</span></span>

## <a name="environment-variables"></a><span data-ttu-id="f4c50-111">Zmienne środowiskowe</span><span class="sxs-lookup"><span data-stu-id="f4c50-111">Environment variables</span></span>

<span data-ttu-id="f4c50-112">Zmienne środowiskowe służą do uniknięcia przechowywania wpisów tajnych aplikacji w kodzie lub w lokalnych plikach konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="f4c50-112">Environment variables are used to avoid storage of app secrets in code or in local configuration files.</span></span> <span data-ttu-id="f4c50-113">Zmienne środowiskowe przesłaniają wartości konfiguracyjne dla wszystkich poprzednio określonych źródeł konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="f4c50-113">Environment variables override configuration values for all previously specified configuration sources.</span></span>

::: moniker range="<= aspnetcore-1.1"

<span data-ttu-id="f4c50-114">Skonfiguruj Odczytywanie wartości zmiennych środowiskowych przez wywołanie <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables%2A> w konstruktorze `Startup`:</span><span class="sxs-lookup"><span data-stu-id="f4c50-114">Configure the reading of environment variable values by calling <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables%2A> in the `Startup` constructor:</span></span>

[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup.cs?name=snippet_StartupConstructor&highlight=8)]

::: moniker-end

<span data-ttu-id="f4c50-115">Rozważ ASP.NET Core aplikacji sieci Web, w której włączono zabezpieczenia **poszczególnych kont użytkowników** .</span><span class="sxs-lookup"><span data-stu-id="f4c50-115">Consider an ASP.NET Core web app in which **Individual User Accounts** security is enabled.</span></span> <span data-ttu-id="f4c50-116">Domyślne parametry połączenia z bazą danych są zawarte w pliku *appSettings. JSON* projektu z kluczem `DefaultConnection`.</span><span class="sxs-lookup"><span data-stu-id="f4c50-116">A default database connection string is included in the project's *appsettings.json* file with the key `DefaultConnection`.</span></span> <span data-ttu-id="f4c50-117">Domyślne parametry połączenia to LocalDB, które są uruchamiane w trybie użytkownika i nie wymagają hasła.</span><span class="sxs-lookup"><span data-stu-id="f4c50-117">The default connection string is for LocalDB, which runs in user mode and doesn't require a password.</span></span> <span data-ttu-id="f4c50-118">Podczas wdrażania aplikacji wartość klucza `DefaultConnection` można zastąpić wartością zmiennej środowiskowej.</span><span class="sxs-lookup"><span data-stu-id="f4c50-118">During app deployment, the `DefaultConnection` key value can be overridden with an environment variable's value.</span></span> <span data-ttu-id="f4c50-119">Zmienna środowiskowa może przechowywać kompletne parametry połączenia z poufnymi poświadczeniami.</span><span class="sxs-lookup"><span data-stu-id="f4c50-119">The environment variable may store the complete connection string with sensitive credentials.</span></span>

> [!WARNING]
> <span data-ttu-id="f4c50-120">Zmienne środowiskowe są zwykle przechowywane w postaci zwykłego, nieszyfrowanego tekstu.</span><span class="sxs-lookup"><span data-stu-id="f4c50-120">Environment variables are generally stored in plain, unencrypted text.</span></span> <span data-ttu-id="f4c50-121">W przypadku naruszenia zabezpieczeń komputera lub procesu zmienne środowiskowe są dostępne dla niezaufanych stron.</span><span class="sxs-lookup"><span data-stu-id="f4c50-121">If the machine or process is compromised, environment variables can be accessed by untrusted parties.</span></span> <span data-ttu-id="f4c50-122">Mogą być wymagane dodatkowe środki, które uniemożliwiają ujawnienie kluczy tajnych użytkownika.</span><span class="sxs-lookup"><span data-stu-id="f4c50-122">Additional measures to prevent disclosure of user secrets may be required.</span></span>

[!INCLUDE[](~/includes/environmentVarableColon.md)]

## <a name="secret-manager"></a><span data-ttu-id="f4c50-123">Menedżer wpisów tajnych</span><span class="sxs-lookup"><span data-stu-id="f4c50-123">Secret Manager</span></span>

<span data-ttu-id="f4c50-124">Narzędzie Secret Manager zapisuje poufne dane podczas opracowywania projektu ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="f4c50-124">The Secret Manager tool stores sensitive data during the development of an ASP.NET Core project.</span></span> <span data-ttu-id="f4c50-125">W tym kontekście dane poufne są tajne dla aplikacji.</span><span class="sxs-lookup"><span data-stu-id="f4c50-125">In this context, a piece of sensitive data is an app secret.</span></span> <span data-ttu-id="f4c50-126">Wpisy tajne aplikacji są przechowywane w oddzielnej lokalizacji w drzewie projektu.</span><span class="sxs-lookup"><span data-stu-id="f4c50-126">App secrets are stored in a separate location from the project tree.</span></span> <span data-ttu-id="f4c50-127">Wpisy tajne aplikacji są skojarzone z określonym projektem lub udostępniane w wielu projektach.</span><span class="sxs-lookup"><span data-stu-id="f4c50-127">The app secrets are associated with a specific project or shared across several projects.</span></span> <span data-ttu-id="f4c50-128">Wpisy tajne aplikacji nie są sprawdzane w kontroli źródła.</span><span class="sxs-lookup"><span data-stu-id="f4c50-128">The app secrets aren't checked into source control.</span></span>

> [!WARNING]
> <span data-ttu-id="f4c50-129">Narzędzie Secret Manager nie szyfruje przechowywanych wpisów tajnych i nie powinna być traktowana jako zaufany magazyn.</span><span class="sxs-lookup"><span data-stu-id="f4c50-129">The Secret Manager tool doesn't encrypt the stored secrets and shouldn't be treated as a trusted store.</span></span> <span data-ttu-id="f4c50-130">Jest przeznaczona tylko do celów programistycznych.</span><span class="sxs-lookup"><span data-stu-id="f4c50-130">It's for development purposes only.</span></span> <span data-ttu-id="f4c50-131">Klucze i wartości są przechowywane w pliku konfiguracji JSON w katalogu profilu użytkownika.</span><span class="sxs-lookup"><span data-stu-id="f4c50-131">The keys and values are stored in a JSON configuration file in the user profile directory.</span></span>

## <a name="how-the-secret-manager-tool-works"></a><span data-ttu-id="f4c50-132">Jak działa narzędzie do zarządzania kluczami tajnymi</span><span class="sxs-lookup"><span data-stu-id="f4c50-132">How the Secret Manager tool works</span></span>

<span data-ttu-id="f4c50-133">Narzędzie tajnego Menedżera wyodrębnia szczegóły implementacji, takie jak miejsce i sposób przechowywania wartości.</span><span class="sxs-lookup"><span data-stu-id="f4c50-133">The Secret Manager tool abstracts away the implementation details, such as where and how the values are stored.</span></span> <span data-ttu-id="f4c50-134">Możesz użyć narzędzia bez znajomości tych szczegółów implementacji.</span><span class="sxs-lookup"><span data-stu-id="f4c50-134">You can use the tool without knowing these implementation details.</span></span> <span data-ttu-id="f4c50-135">Wartości są przechowywane w pliku konfiguracji JSON w folderze profilu użytkownika chronionego przez system na komputerze lokalnym:</span><span class="sxs-lookup"><span data-stu-id="f4c50-135">The values are stored in a JSON configuration file in a system-protected user profile folder on the local machine:</span></span>

# <a name="windowstabwindows"></a>[<span data-ttu-id="f4c50-136">Windows</span><span class="sxs-lookup"><span data-stu-id="f4c50-136">Windows</span></span>](#tab/windows)

<span data-ttu-id="f4c50-137">Ścieżka systemu plików:</span><span class="sxs-lookup"><span data-stu-id="f4c50-137">File system path:</span></span>

`%APPDATA%\Microsoft\UserSecrets\<user_secrets_id>\secrets.json`

# <a name="linux--macostablinuxmacos"></a>[<span data-ttu-id="f4c50-138">Linux/macOS</span><span class="sxs-lookup"><span data-stu-id="f4c50-138">Linux / macOS</span></span>](#tab/linux+macos)

<span data-ttu-id="f4c50-139">Ścieżka systemu plików:</span><span class="sxs-lookup"><span data-stu-id="f4c50-139">File system path:</span></span>

`~/.microsoft/usersecrets/<user_secrets_id>/secrets.json`

---

<span data-ttu-id="f4c50-140">W poprzednich ścieżkach plików Zastąp `<user_secrets_id>` wartością `UserSecretsId` określoną w pliku *. csproj* .</span><span class="sxs-lookup"><span data-stu-id="f4c50-140">In the preceding file paths, replace `<user_secrets_id>` with the `UserSecretsId` value specified in the *.csproj* file.</span></span>

<span data-ttu-id="f4c50-141">Nie pisz kodu, który zależy od lokalizacji lub formatu danych zapisywanych za pomocą narzędzia do zarządzania kluczami tajnymi.</span><span class="sxs-lookup"><span data-stu-id="f4c50-141">Don't write code that depends on the location or format of data saved with the Secret Manager tool.</span></span> <span data-ttu-id="f4c50-142">Te szczegóły implementacji mogą ulec zmianie.</span><span class="sxs-lookup"><span data-stu-id="f4c50-142">These implementation details may change.</span></span> <span data-ttu-id="f4c50-143">Na przykład wartości tajne nie są szyfrowane, ale mogą być w przyszłości.</span><span class="sxs-lookup"><span data-stu-id="f4c50-143">For example, the secret values aren't encrypted, but could be in the future.</span></span>

::: moniker range="<= aspnetcore-2.0"

## <a name="install-the-secret-manager-tool"></a><span data-ttu-id="f4c50-144">Instalowanie narzędzia do zarządzania kluczami tajnymi</span><span class="sxs-lookup"><span data-stu-id="f4c50-144">Install the Secret Manager tool</span></span>

<span data-ttu-id="f4c50-145">Narzędzie Secret Manager jest powiązane z interfejs wiersza polecenia platformy .NET Core w zestaw .NET Core SDK 2.1.300 lub nowszym.</span><span class="sxs-lookup"><span data-stu-id="f4c50-145">The Secret Manager tool is bundled with the .NET Core CLI in .NET Core SDK 2.1.300 or later.</span></span> <span data-ttu-id="f4c50-146">W przypadku wersji zestaw .NET Core SDK przed 2.1.300 należy zainstalować narzędzie.</span><span class="sxs-lookup"><span data-stu-id="f4c50-146">For .NET Core SDK versions before 2.1.300, tool installation is necessary.</span></span>

> [!TIP]
> <span data-ttu-id="f4c50-147">Uruchom `dotnet --version` z powłoki poleceń, aby wyświetlić zainstalowany zestaw .NET Core SDK numer wersji.</span><span class="sxs-lookup"><span data-stu-id="f4c50-147">Run `dotnet --version` from a command shell to see the installed .NET Core SDK version number.</span></span>

<span data-ttu-id="f4c50-148">Ostrzeżenie jest wyświetlane, jeśli używany zestaw .NET Core SDK zawiera narzędzie:</span><span class="sxs-lookup"><span data-stu-id="f4c50-148">A warning is displayed if the .NET Core SDK being used includes the tool:</span></span>

```console
The tool 'Microsoft.Extensions.SecretManager.Tools' is now included in the .NET Core SDK. Information on resolving this warning is available at (https://aka.ms/dotnetclitools-in-box).
```

<span data-ttu-id="f4c50-149">Zainstaluj pakiet NuGet [Microsoft. Extensions. SecretManager. Tools](https://www.nuget.org/packages/Microsoft.Extensions.SecretManager.Tools/) w projekcie ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="f4c50-149">Install the [Microsoft.Extensions.SecretManager.Tools](https://www.nuget.org/packages/Microsoft.Extensions.SecretManager.Tools/) NuGet package in your ASP.NET Core project.</span></span> <span data-ttu-id="f4c50-150">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="f4c50-150">For example:</span></span>

[!code-xml[](app-secrets/samples/1.x/UserSecrets/UserSecrets.csproj?name=snippet_CsprojFile&highlight=15-16)]

<span data-ttu-id="f4c50-151">Aby sprawdzić poprawność instalacji narzędzi, wykonaj następujące polecenie w powłoce poleceń:</span><span class="sxs-lookup"><span data-stu-id="f4c50-151">Execute the following command in a command shell to validate the tool installation:</span></span>

```dotnetcli
dotnet user-secrets -h
```

<span data-ttu-id="f4c50-152">Narzędzie Secret Manager wyświetla przykładowe użycie, opcje i pomoc poleceń:</span><span class="sxs-lookup"><span data-stu-id="f4c50-152">The Secret Manager tool displays sample usage, options, and command help:</span></span>

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
> <span data-ttu-id="f4c50-153">Musisz być w tym samym katalogu, co plik *. csproj* , aby uruchamiać narzędzia zdefiniowane w `DotNetCliToolReference` pliku *. csproj* .</span><span class="sxs-lookup"><span data-stu-id="f4c50-153">You must be in the same directory as the *.csproj* file to run tools defined in the *.csproj* file's `DotNetCliToolReference` elements.</span></span>

::: moniker-end

## <a name="enable-secret-storage"></a><span data-ttu-id="f4c50-154">Włącz magazyn tajny</span><span class="sxs-lookup"><span data-stu-id="f4c50-154">Enable secret storage</span></span>

<span data-ttu-id="f4c50-155">Narzędzie Secret Manager działa na specyficznych dla projektu ustawieniach konfiguracji przechowywanych w profilu użytkownika.</span><span class="sxs-lookup"><span data-stu-id="f4c50-155">The Secret Manager tool operates on project-specific configuration settings stored in your user profile.</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="f4c50-156">Narzędzie Secret Manager zawiera polecenie `init` w zestaw .NET Core SDK 3.0.100 lub nowszym.</span><span class="sxs-lookup"><span data-stu-id="f4c50-156">The Secret Manager tool includes an `init` command in .NET Core SDK 3.0.100 or later.</span></span> <span data-ttu-id="f4c50-157">Aby użyć kluczy tajnych użytkownika, uruchom następujące polecenie w katalogu projektu:</span><span class="sxs-lookup"><span data-stu-id="f4c50-157">To use user secrets, run the following command in the project directory:</span></span>

```dotnetcli
dotnet user-secrets init
```

<span data-ttu-id="f4c50-158">Poprzednie polecenie dodaje element `UserSecretsId` w `PropertyGroup` pliku *. csproj* .</span><span class="sxs-lookup"><span data-stu-id="f4c50-158">The preceding command adds a `UserSecretsId` element within a `PropertyGroup` of the *.csproj* file.</span></span> <span data-ttu-id="f4c50-159">Domyślnie wewnętrznym tekstem `UserSecretsId` jest identyfikator GUID.</span><span class="sxs-lookup"><span data-stu-id="f4c50-159">By default, the inner text of `UserSecretsId` is a GUID.</span></span> <span data-ttu-id="f4c50-160">Wewnętrzny tekst jest dowolny, ale jest unikatowy dla projektu.</span><span class="sxs-lookup"><span data-stu-id="f4c50-160">The inner text is arbitrary, but is unique to the project.</span></span>

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

<span data-ttu-id="f4c50-161">Aby użyć kluczy tajnych użytkownika, zdefiniuj `UserSecretsId` element w `PropertyGroup` pliku *. csproj* .</span><span class="sxs-lookup"><span data-stu-id="f4c50-161">To use user secrets, define a `UserSecretsId` element within a `PropertyGroup` of the *.csproj* file.</span></span> <span data-ttu-id="f4c50-162">Wewnętrzny tekst `UserSecretsId` jest dowolny, ale jest unikatowy dla projektu.</span><span class="sxs-lookup"><span data-stu-id="f4c50-162">The inner text of `UserSecretsId` is arbitrary, but is unique to the project.</span></span> <span data-ttu-id="f4c50-163">Deweloperzy zwykle generują identyfikator GUID dla `UserSecretsId`.</span><span class="sxs-lookup"><span data-stu-id="f4c50-163">Developers typically generate a GUID for the `UserSecretsId`.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

[!code-xml[](app-secrets/samples/2.x/UserSecrets/UserSecrets.csproj?name=snippet_PropertyGroup&highlight=3)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-xml[](app-secrets/samples/1.x/UserSecrets/UserSecrets.csproj?name=snippet_PropertyGroup&highlight=3)]

::: moniker-end

> [!TIP]
> <span data-ttu-id="f4c50-164">W programie Visual Studio kliknij prawym przyciskiem myszy projekt w Eksplorator rozwiązań i wybierz polecenie **Zarządzaj kluczami tajnymi użytkownika** z menu kontekstowego.</span><span class="sxs-lookup"><span data-stu-id="f4c50-164">In Visual Studio, right-click the project in Solution Explorer, and select **Manage User Secrets** from the context menu.</span></span> <span data-ttu-id="f4c50-165">Ten gest dodaje element `UserSecretsId`, wypełniony identyfikatorem GUID do pliku *. csproj* .</span><span class="sxs-lookup"><span data-stu-id="f4c50-165">This gesture adds a `UserSecretsId` element, populated with a GUID, to the *.csproj* file.</span></span>

## <a name="set-a-secret"></a><span data-ttu-id="f4c50-166">Ustaw klucz tajny</span><span class="sxs-lookup"><span data-stu-id="f4c50-166">Set a secret</span></span>

<span data-ttu-id="f4c50-167">Zdefiniuj klucz tajny aplikacji składający się z klucza i jego wartości.</span><span class="sxs-lookup"><span data-stu-id="f4c50-167">Define an app secret consisting of a key and its value.</span></span> <span data-ttu-id="f4c50-168">Wpis tajny jest skojarzony z wartością `UserSecretsId` projektu.</span><span class="sxs-lookup"><span data-stu-id="f4c50-168">The secret is associated with the project's `UserSecretsId` value.</span></span> <span data-ttu-id="f4c50-169">Na przykład uruchom następujące polecenie z katalogu, w którym istnieje plik *. csproj* :</span><span class="sxs-lookup"><span data-stu-id="f4c50-169">For example, run the following command from the directory in which the *.csproj* file exists:</span></span>

```dotnetcli
dotnet user-secrets set "Movies:ServiceApiKey" "12345"
```

<span data-ttu-id="f4c50-170">W poprzednim przykładzie dwukropek oznacza, że `Movies` jest literałem obiektu z właściwością `ServiceApiKey`.</span><span class="sxs-lookup"><span data-stu-id="f4c50-170">In the preceding example, the colon denotes that `Movies` is an object literal with a `ServiceApiKey` property.</span></span>

<span data-ttu-id="f4c50-171">Narzędzia do zarządzania kluczami tajnymi można również użyć z innych katalogów.</span><span class="sxs-lookup"><span data-stu-id="f4c50-171">The Secret Manager tool can be used from other directories too.</span></span> <span data-ttu-id="f4c50-172">Użyj opcji `--project`, aby podać ścieżkę systemu plików, w której istnieje plik *. csproj* .</span><span class="sxs-lookup"><span data-stu-id="f4c50-172">Use the `--project` option to supply the file system path at which the *.csproj* file exists.</span></span> <span data-ttu-id="f4c50-173">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="f4c50-173">For example:</span></span>

```dotnetcli
dotnet user-secrets set "Movies:ServiceApiKey" "12345" --project "C:\apps\WebApp1\src\WebApp1"
```

### <a name="json-structure-flattening-in-visual-studio"></a><span data-ttu-id="f4c50-174">Spłaszczanie struktury JSON w programie Visual Studio</span><span class="sxs-lookup"><span data-stu-id="f4c50-174">JSON structure flattening in Visual Studio</span></span>

<span data-ttu-id="f4c50-175">Gest **Zarządzanie kluczami tajnymi użytkownika** programu Visual Studio otwiera plik Secret *. JSON* w edytorze tekstów.</span><span class="sxs-lookup"><span data-stu-id="f4c50-175">Visual Studio's **Manage User Secrets** gesture opens a *secrets.json* file in the text editor.</span></span> <span data-ttu-id="f4c50-176">Zastąp zawartość Secret *. JSON* wartościami par klucz-wartość, które mają być przechowywane.</span><span class="sxs-lookup"><span data-stu-id="f4c50-176">Replace the contents of *secrets.json* with the key-value pairs to be stored.</span></span> <span data-ttu-id="f4c50-177">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="f4c50-177">For example:</span></span>

```json
{
  "Movies": {
    "ConnectionString": "Server=(localdb)\\mssqllocaldb;Database=Movie-1;Trusted_Connection=True;MultipleActiveResultSets=true",
    "ServiceApiKey": "12345"
  }
}
```

<span data-ttu-id="f4c50-178">Struktura JSON jest spłaszczona po modyfikacji za pośrednictwem `dotnet user-secrets remove` lub `dotnet user-secrets set`.</span><span class="sxs-lookup"><span data-stu-id="f4c50-178">The JSON structure is flattened after modifications via `dotnet user-secrets remove` or `dotnet user-secrets set`.</span></span> <span data-ttu-id="f4c50-179">Na przykład uruchomienie `dotnet user-secrets remove "Movies:ConnectionString"` zwija `Movies` literał obiektu.</span><span class="sxs-lookup"><span data-stu-id="f4c50-179">For example, running `dotnet user-secrets remove "Movies:ConnectionString"` collapses the `Movies` object literal.</span></span> <span data-ttu-id="f4c50-180">Zmodyfikowany plik jest podobny do następującego:</span><span class="sxs-lookup"><span data-stu-id="f4c50-180">The modified file resembles the following:</span></span>

```json
{
  "Movies:ServiceApiKey": "12345"
}
```

## <a name="set-multiple-secrets"></a><span data-ttu-id="f4c50-181">Ustawianie wielu wpisów tajnych</span><span class="sxs-lookup"><span data-stu-id="f4c50-181">Set multiple secrets</span></span>

<span data-ttu-id="f4c50-182">Partia wpisów tajnych można ustawić za pomocą formatu JSON dla polecenia `set`.</span><span class="sxs-lookup"><span data-stu-id="f4c50-182">A batch of secrets can be set by piping JSON to the `set` command.</span></span> <span data-ttu-id="f4c50-183">W poniższym przykładzie zawartość pliku *Input. JSON* jest potoku do polecenia `set`.</span><span class="sxs-lookup"><span data-stu-id="f4c50-183">In the following example, the *input.json* file's contents are piped to the `set` command.</span></span>

# <a name="windowstabwindows"></a>[<span data-ttu-id="f4c50-184">Windows</span><span class="sxs-lookup"><span data-stu-id="f4c50-184">Windows</span></span>](#tab/windows)

<span data-ttu-id="f4c50-185">Otwórz powłokę poleceń i wykonaj następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="f4c50-185">Open a command shell, and execute the following command:</span></span>

  ```dotnetcli
  type .\input.json | dotnet user-secrets set
  ```

# <a name="linux--macostablinuxmacos"></a>[<span data-ttu-id="f4c50-186">Linux/macOS</span><span class="sxs-lookup"><span data-stu-id="f4c50-186">Linux / macOS</span></span>](#tab/linux+macos)

<span data-ttu-id="f4c50-187">Otwórz powłokę poleceń i wykonaj następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="f4c50-187">Open a command shell, and execute the following command:</span></span>

  ```dotnetcli
  cat ./input.json | dotnet user-secrets set
  ```

---

## <a name="access-a-secret"></a><span data-ttu-id="f4c50-188">Dostęp do klucza tajnego</span><span class="sxs-lookup"><span data-stu-id="f4c50-188">Access a secret</span></span>

<span data-ttu-id="f4c50-189">[Interfejs API konfiguracji ASP.NET Core](xref:fundamentals/configuration/index) zapewnia dostęp do wpisów tajnych usługi Secret Manager.</span><span class="sxs-lookup"><span data-stu-id="f4c50-189">The [ASP.NET Core Configuration API](xref:fundamentals/configuration/index) provides access to Secret Manager secrets.</span></span>

::: moniker range=">= aspnetcore-2.0 <= aspnetcore-2.2"

<span data-ttu-id="f4c50-190">Jeśli projekt jest przeznaczony .NET Framework, zainstaluj pakiet NuGet [Microsoft. Extensions. Configuration. UserSecrets](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.UserSecrets) .</span><span class="sxs-lookup"><span data-stu-id="f4c50-190">If your project targets .NET Framework, install the [Microsoft.Extensions.Configuration.UserSecrets](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.UserSecrets) NuGet package.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="f4c50-191">W ASP.NET Core 2,0 lub nowszym Źródło konfiguracji kluczy tajnych użytkownika jest automatycznie dodawane w trybie programistycznym, gdy projekt wywoła <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder%2A>, aby zainicjować nowe wystąpienie hosta ze wstępnie skonfigurowanymi ustawieniami domyślnymi.</span><span class="sxs-lookup"><span data-stu-id="f4c50-191">In ASP.NET Core 2.0 or later, the user secrets configuration source is automatically added in development mode when the project calls <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder%2A> to initialize a new instance of the host with preconfigured defaults.</span></span> <span data-ttu-id="f4c50-192">`CreateDefaultBuilder` wywołań <xref:Microsoft.Extensions.Configuration.UserSecretsConfigurationExtensions.AddUserSecrets%2A>, gdy <xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment.EnvironmentName> jest <xref:Microsoft.AspNetCore.Hosting.EnvironmentName.Development>:</span><span class="sxs-lookup"><span data-stu-id="f4c50-192">`CreateDefaultBuilder` calls <xref:Microsoft.Extensions.Configuration.UserSecretsConfigurationExtensions.AddUserSecrets%2A> when the <xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment.EnvironmentName> is <xref:Microsoft.AspNetCore.Hosting.EnvironmentName.Development>:</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.0 <= aspnetcore-2.2"

[!code-csharp[](app-secrets/samples/2.x/UserSecrets/Program.cs?name=snippet_CreateWebHostBuilder&highlight=2)]

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](app-secrets/samples/3.x/UserSecrets/Program.cs?name=snippet_CreateHostBuilder&highlight=2)]

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="f4c50-193">Gdy `CreateDefaultBuilder` nie zostanie wywołana, Dodaj źródło konfiguracji danych tajnych użytkownika jawnie, wywołując <xref:Microsoft.Extensions.Configuration.UserSecretsConfigurationExtensions.AddUserSecrets%2A> w konstruktorze `Startup`.</span><span class="sxs-lookup"><span data-stu-id="f4c50-193">When `CreateDefaultBuilder` isn't called, add the user secrets configuration source explicitly by calling <xref:Microsoft.Extensions.Configuration.UserSecretsConfigurationExtensions.AddUserSecrets%2A> in the `Startup` constructor.</span></span> <span data-ttu-id="f4c50-194">Wywołaj `AddUserSecrets` tylko wtedy, gdy aplikacja działa w środowisku deweloperskim, jak pokazano w następującym przykładzie:</span><span class="sxs-lookup"><span data-stu-id="f4c50-194">Call `AddUserSecrets` only when the app runs in the Development environment, as shown in the following example:</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.0 <= aspnetcore-2.2"

[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup.cs?name=snippet_StartupConstructor&highlight=12)]

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](app-secrets/samples/3.x/UserSecrets/Startup2.cs?name=snippet_StartupConstructor&highlight=12)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

<span data-ttu-id="f4c50-195">Zainstaluj pakiet NuGet [Microsoft. Extensions. Configuration. UserSecrets](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.UserSecrets) .</span><span class="sxs-lookup"><span data-stu-id="f4c50-195">Install the [Microsoft.Extensions.Configuration.UserSecrets](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.UserSecrets) NuGet package.</span></span>

<span data-ttu-id="f4c50-196">Dodaj źródło konfiguracji kluczy tajnych użytkownika z wywołaniem do <xref:Microsoft.Extensions.Configuration.UserSecretsConfigurationExtensions.AddUserSecrets%2A> w konstruktorze `Startup`:</span><span class="sxs-lookup"><span data-stu-id="f4c50-196">Add the user secrets configuration source with a call to <xref:Microsoft.Extensions.Configuration.UserSecretsConfigurationExtensions.AddUserSecrets%2A> in the `Startup` constructor:</span></span>

[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup.cs?name=snippet_StartupConstructor&highlight=12)]

::: moniker-end

<span data-ttu-id="f4c50-197">Wpisy tajne użytkownika można pobrać za pośrednictwem interfejsu API `Configuration`:</span><span class="sxs-lookup"><span data-stu-id="f4c50-197">User secrets can be retrieved via the `Configuration` API:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](app-secrets/samples/2.x/UserSecrets/Startup.cs?name=snippet_StartupClass&highlight=14)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup.cs?name=snippet_StartupClass&highlight=26)]

::: moniker-end

## <a name="map-secrets-to-a-poco"></a><span data-ttu-id="f4c50-198">Mapuj wpisy tajne do POCO</span><span class="sxs-lookup"><span data-stu-id="f4c50-198">Map secrets to a POCO</span></span>

<span data-ttu-id="f4c50-199">Mapowanie całego literału obiektu na POCO (prosta Klasa .NET z właściwościami) jest przydatne do agregowania powiązanych właściwości.</span><span class="sxs-lookup"><span data-stu-id="f4c50-199">Mapping an entire object literal to a POCO (a simple .NET class with properties) is useful for aggregating related properties.</span></span>

[!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file-and-text.md)]

<span data-ttu-id="f4c50-200">Aby zmapować poprzednie wpisy tajne do POCO, użyj funkcji [powiązania grafu obiektów](xref:fundamentals/configuration/index#bind-to-an-object-graph) interfejsu API `Configuration`.</span><span class="sxs-lookup"><span data-stu-id="f4c50-200">To map the preceding secrets to a POCO, use the `Configuration` API's [object graph binding](xref:fundamentals/configuration/index#bind-to-an-object-graph) feature.</span></span> <span data-ttu-id="f4c50-201">Następujący kod wiąże się z niestandardowym `MovieSettings` POCO i uzyskuje dostęp do wartości właściwości `ServiceApiKey`:</span><span class="sxs-lookup"><span data-stu-id="f4c50-201">The following code binds to a custom `MovieSettings` POCO and accesses the `ServiceApiKey` property value:</span></span>

::: moniker range=">= aspnetcore-1.1"

[!code-csharp[](app-secrets/samples/2.x/UserSecrets/Startup3.cs?name=snippet_BindToObjectGraph)]

::: moniker-end

::: moniker range="= aspnetcore-1.0"

[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup3.cs?name=snippet_BindToObjectGraph)]

::: moniker-end

<span data-ttu-id="f4c50-202">`Movies:ConnectionString` i `Movies:ServiceApiKey` wpisy tajne są mapowane na odpowiednie właściwości w `MovieSettings`:</span><span class="sxs-lookup"><span data-stu-id="f4c50-202">The `Movies:ConnectionString` and `Movies:ServiceApiKey` secrets are mapped to the respective properties in `MovieSettings`:</span></span>

[!code-csharp[](app-secrets/samples/2.x/UserSecrets/Models/MovieSettings.cs?name=snippet_MovieSettingsClass)]

## <a name="string-replacement-with-secrets"></a><span data-ttu-id="f4c50-203">Zastępowanie ciągów hasłami tajnymi</span><span class="sxs-lookup"><span data-stu-id="f4c50-203">String replacement with secrets</span></span>

<span data-ttu-id="f4c50-204">Przechowywanie haseł w postaci zwykłego tekstu jest niebezpieczne.</span><span class="sxs-lookup"><span data-stu-id="f4c50-204">Storing passwords in plain text is insecure.</span></span> <span data-ttu-id="f4c50-205">Na przykład parametry połączenia z bazą danych przechowywane w pliku *appSettings. JSON* mogą zawierać hasło dla określonego użytkownika:</span><span class="sxs-lookup"><span data-stu-id="f4c50-205">For example, a database connection string stored in *appsettings.json* may include a password for the specified user:</span></span>

[!code-json[](app-secrets/samples/2.x/UserSecrets/appsettings-unsecure.json?highlight=3)]

<span data-ttu-id="f4c50-206">Bardziej bezpiecznym podejściem jest przechowywanie hasła jako klucza tajnego.</span><span class="sxs-lookup"><span data-stu-id="f4c50-206">A more secure approach is to store the password as a secret.</span></span> <span data-ttu-id="f4c50-207">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="f4c50-207">For example:</span></span>

```dotnetcli
dotnet user-secrets set "DbPassword" "pass123"
```

<span data-ttu-id="f4c50-208">Usuń parę klucz-wartość `Password` z parametrów połączenia w pliku *appSettings. JSON*.</span><span class="sxs-lookup"><span data-stu-id="f4c50-208">Remove the `Password` key-value pair from the connection string in *appsettings.json*.</span></span> <span data-ttu-id="f4c50-209">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="f4c50-209">For example:</span></span>

[!code-json[](app-secrets/samples/2.x/UserSecrets/appsettings.json?highlight=3)]

<span data-ttu-id="f4c50-210">Wartość wpisu tajnego można ustawić na właściwości <xref:System.Data.SqlClient.SqlConnectionStringBuilder.Password%2A> obiektu <xref:System.Data.SqlClient.SqlConnectionStringBuilder>, aby zakończyć parametry połączenia:</span><span class="sxs-lookup"><span data-stu-id="f4c50-210">The secret's value can be set on a <xref:System.Data.SqlClient.SqlConnectionStringBuilder> object's <xref:System.Data.SqlClient.SqlConnectionStringBuilder.Password%2A> property to complete the connection string:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](app-secrets/samples/2.x/UserSecrets/Startup2.cs?name=snippet_StartupClass&highlight=14-17)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup2.cs?name=snippet_StartupClass&highlight=26-29)]

::: moniker-end

## <a name="list-the-secrets"></a><span data-ttu-id="f4c50-211">Wystaw wpisy tajne</span><span class="sxs-lookup"><span data-stu-id="f4c50-211">List the secrets</span></span>

[!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file-and-text.md)]

<span data-ttu-id="f4c50-212">Uruchom następujące polecenie z katalogu, w którym istnieje plik *. csproj* :</span><span class="sxs-lookup"><span data-stu-id="f4c50-212">Run the following command from the directory in which the *.csproj* file exists:</span></span>

```dotnetcli
dotnet user-secrets list
```

<span data-ttu-id="f4c50-213">Zostaną wyświetlone następujące dane wyjściowe:</span><span class="sxs-lookup"><span data-stu-id="f4c50-213">The following output appears:</span></span>

```console
Movies:ConnectionString = Server=(localdb)\mssqllocaldb;Database=Movie-1;Trusted_Connection=True;MultipleActiveResultSets=true
Movies:ServiceApiKey = 12345
```

<span data-ttu-id="f4c50-214">W poprzednim przykładzie dwukropek w nazwach kluczy oznacza hierarchię obiektów w formacie Secret *. JSON*.</span><span class="sxs-lookup"><span data-stu-id="f4c50-214">In the preceding example, a colon in the key names denotes the object hierarchy within *secrets.json*.</span></span>

## <a name="remove-a-single-secret"></a><span data-ttu-id="f4c50-215">Usuń pojedynczy klucz tajny</span><span class="sxs-lookup"><span data-stu-id="f4c50-215">Remove a single secret</span></span>

[!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file-and-text.md)]

<span data-ttu-id="f4c50-216">Uruchom następujące polecenie z katalogu, w którym istnieje plik *. csproj* :</span><span class="sxs-lookup"><span data-stu-id="f4c50-216">Run the following command from the directory in which the *.csproj* file exists:</span></span>

```dotnetcli
dotnet user-secrets remove "Movies:ConnectionString"
```

<span data-ttu-id="f4c50-217">Plik Secret *. JSON* aplikacji został zmodyfikowany, aby usunąć parę klucz-wartość skojarzoną z kluczem `MoviesConnectionString`:</span><span class="sxs-lookup"><span data-stu-id="f4c50-217">The app's *secrets.json* file was modified to remove the key-value pair associated with the `MoviesConnectionString` key:</span></span>

```json
{
  "Movies": {
    "ServiceApiKey": "12345"
  }
}
```

<span data-ttu-id="f4c50-218">Uruchomienie `dotnet user-secrets list` wyświetla następujący komunikat:</span><span class="sxs-lookup"><span data-stu-id="f4c50-218">Running `dotnet user-secrets list` displays the following message:</span></span>

```console
Movies:ServiceApiKey = 12345
```

## <a name="remove-all-secrets"></a><span data-ttu-id="f4c50-219">Usuń wszystkie wpisy tajne</span><span class="sxs-lookup"><span data-stu-id="f4c50-219">Remove all secrets</span></span>

[!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file-and-text.md)]

<span data-ttu-id="f4c50-220">Uruchom następujące polecenie z katalogu, w którym istnieje plik *. csproj* :</span><span class="sxs-lookup"><span data-stu-id="f4c50-220">Run the following command from the directory in which the *.csproj* file exists:</span></span>

```dotnetcli
dotnet user-secrets clear
```

<span data-ttu-id="f4c50-221">Wszystkie wpisy tajne użytkownika dla aplikacji zostały usunięte z pliku Secret *. JSON* :</span><span class="sxs-lookup"><span data-stu-id="f4c50-221">All user secrets for the app have been deleted from the *secrets.json* file:</span></span>

```json
{}
```

<span data-ttu-id="f4c50-222">Uruchomienie `dotnet user-secrets list` wyświetla następujący komunikat:</span><span class="sxs-lookup"><span data-stu-id="f4c50-222">Running `dotnet user-secrets list` displays the following message:</span></span>

```console
No secrets configured for this application.
```

## <a name="additional-resources"></a><span data-ttu-id="f4c50-223">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="f4c50-223">Additional resources</span></span>

* <span data-ttu-id="f4c50-224">Zobacz [ten problem](https://github.com/aspnet/AspNetCore.Docs/issues/16328) , aby uzyskać informacje na temat uzyskiwania dostępu do Menedżera kluczy tajnych z usług IIS.</span><span class="sxs-lookup"><span data-stu-id="f4c50-224">See [this issue](https://github.com/aspnet/AspNetCore.Docs/issues/16328) for information on accessing Secret Manager from IIS.</span></span>
* <xref:fundamentals/configuration/index>
* <xref:security/key-vault-configuration>
