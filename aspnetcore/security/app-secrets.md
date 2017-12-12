---
title: Bezpieczne przechowywanie kluczy tajnych aplikacji w czasie opracowywania w ASP.NET Core
author: rick-anderson
description: "Pokazuje, jak bezpiecznie przechowywać klucze tajne podczas tworzenia"
keywords: Platformy ASP.NET Core
ms.author: riande
manager: wpickett
ms.date: 09/15/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/app-secrets
ms.openlocfilehash: 897d9b360ceeb5fbb0863ff1c1fcec039e1a8b8f
ms.sourcegitcommit: 8f42ab93402c1b8044815e1e48d0bb84c81f8b59
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/29/2017
---
# <a name="safe-storage-of-app-secrets-during-development-in-aspnet-core"></a><span data-ttu-id="76b2f-104">Bezpieczne przechowywanie kluczy tajnych aplikacji w czasie opracowywania w ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="76b2f-104">Safe storage of app secrets during development in ASP.NET Core</span></span>

<span data-ttu-id="76b2f-105">Przez [Rick Anderson](https://twitter.com/RickAndMSFT), [Roth Danielowi](https://github.com/danroth27), i [Scott Addie](https://scottaddie.com)</span><span class="sxs-lookup"><span data-stu-id="76b2f-105">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Daniel Roth](https://github.com/danroth27), and [Scott Addie](https://scottaddie.com)</span></span> 

<span data-ttu-id="76b2f-106">Ten dokument zawiera, jak można użyć narzędzia menedżera klucza tajnego do rozwoju przechowywania kluczy tajnych poza swój kod.</span><span class="sxs-lookup"><span data-stu-id="76b2f-106">This document shows how you can use the Secret Manager tool in development to keep secrets out of your code.</span></span> <span data-ttu-id="76b2f-107">Najważniejsze punkt znajduje się w kodzie źródłowym nigdy nie należy przechowywać haseł i innych poufnych danych i kluczy tajnych w środowisku produkcyjnym nie można używać w trybie projektowania i testowania.</span><span class="sxs-lookup"><span data-stu-id="76b2f-107">The most important point is you should never store passwords or other sensitive data in source code, and you shouldn't use production secrets in development and test mode.</span></span> <span data-ttu-id="76b2f-108">Zamiast tego można użyć [konfiguracji](xref:fundamentals/configuration/index) systemu, aby odczytać te wartości zmiennych środowiskowych lub z wartości przechowywane przy użyciu Menedżera klucz tajny narzędzia.</span><span class="sxs-lookup"><span data-stu-id="76b2f-108">You can instead use the [configuration](xref:fundamentals/configuration/index) system to read these values from environment variables or from values stored using the Secret Manager tool.</span></span> <span data-ttu-id="76b2f-109">Narzędzie Menedżer klucz tajny zapobiega danych poufnych jest wyewidencjonowany do kontroli źródła.</span><span class="sxs-lookup"><span data-stu-id="76b2f-109">The Secret Manager tool helps prevent sensitive data from being checked into source control.</span></span> <span data-ttu-id="76b2f-110">[Konfiguracji](xref:fundamentals/configuration/index) systemu można odczyt kluczy tajnych przechowywane za pomocą narzędzia menedżera klucz tajny opisane w tym artykule.</span><span class="sxs-lookup"><span data-stu-id="76b2f-110">The [configuration](xref:fundamentals/configuration/index) system can read secrets stored with the Secret Manager tool described in this article.</span></span>

<span data-ttu-id="76b2f-111">Narzędzie Menedżer klucz tajny jest używana tylko programowanie.</span><span class="sxs-lookup"><span data-stu-id="76b2f-111">The Secret Manager tool is used only in development.</span></span> <span data-ttu-id="76b2f-112">Można chronić Azure kluczy tajnych testowych i produkcyjnych z [Microsoft Azure Key Vault](https://azure.microsoft.com/services/key-vault/) dostawcę konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="76b2f-112">You can safeguard Azure test and production secrets with the [Microsoft Azure Key Vault](https://azure.microsoft.com/services/key-vault/) configuration provider.</span></span> <span data-ttu-id="76b2f-113">Zobacz [dostawcę konfiguracji usługi Azure Key Vault](https://docs.microsoft.com/aspnet/core/security/key-vault-configuration) Aby uzyskać więcej informacji.</span><span class="sxs-lookup"><span data-stu-id="76b2f-113">See [Azure Key Vault configuration provider](https://docs.microsoft.com/aspnet/core/security/key-vault-configuration) for more information.</span></span>

## <a name="environment-variables"></a><span data-ttu-id="76b2f-114">Zmienne środowiskowe</span><span class="sxs-lookup"><span data-stu-id="76b2f-114">Environment variables</span></span>

<span data-ttu-id="76b2f-115">Aby uniknąć przechowywania klucze tajne aplikacji, kodu lub lokalne pliki konfiguracji, kluczy tajnych są przechowywane w zmiennych środowiskowych.</span><span class="sxs-lookup"><span data-stu-id="76b2f-115">To avoid storing app secrets in code or in local configuration files, you store secrets in environment variables.</span></span> <span data-ttu-id="76b2f-116">Możesz skonfigurować [konfiguracji](xref:fundamentals/configuration/index) framework odczytywać wartości zmiennych środowiskowych przez wywołanie metody `AddEnvironmentVariables`.</span><span class="sxs-lookup"><span data-stu-id="76b2f-116">You can setup the [configuration](xref:fundamentals/configuration/index) framework to read values from environment variables by calling `AddEnvironmentVariables`.</span></span> <span data-ttu-id="76b2f-117">Następnie można używać zmiennych środowiskowych, aby zastąpić wartości konfiguracji dla wszystkich źródeł wcześniej określonej konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="76b2f-117">You can then use environment variables to override configuration values for all previously specified configuration sources.</span></span>

<span data-ttu-id="76b2f-118">Na przykład w przypadku utworzenia nowej aplikacji sieci web platformy ASP.NET Core z indywidualne konta użytkowników, spowoduje to dodanie domyślnego ciągu połączenia do *appsettings.json* plik w projekcie z kluczem `DefaultConnection`.</span><span class="sxs-lookup"><span data-stu-id="76b2f-118">For example, if you create a new ASP.NET Core web app with individual user accounts, it will add a default connection string to the *appsettings.json* file in the project with the key `DefaultConnection`.</span></span> <span data-ttu-id="76b2f-119">Domyślny ciąg połączenia jest skonfigurowana do używania bazy danych LocalDB, która działa w trybie użytkownika i nie wymaga hasła.</span><span class="sxs-lookup"><span data-stu-id="76b2f-119">The default connection string is setup to use LocalDB, which runs in user mode and doesn't require a password.</span></span> <span data-ttu-id="76b2f-120">Podczas wdrażania aplikacji na serwerze testowym lub produkcyjnym można zastąpić `DefaultConnection` wartość klucza z ustawienia zmiennej środowiskowej, zawierający parametry połączenia (potencjalnie o poufnych poświadczeń) w bazie danych testowym lub produkcyjnym serwer.</span><span class="sxs-lookup"><span data-stu-id="76b2f-120">When you deploy your application to a test or production server, you can override the `DefaultConnection` key value with an environment variable setting that contains the connection string (potentially with sensitive credentials) for a test or production database server.</span></span>

>[!WARNING]
> <span data-ttu-id="76b2f-121">Zmienne środowiskowe są zwykle przechowywane w postaci zwykłego tekstu i nie są szyfrowane.</span><span class="sxs-lookup"><span data-stu-id="76b2f-121">Environment variables are generally stored in plain text and are not encrypted.</span></span> <span data-ttu-id="76b2f-122">W przypadku naruszenia zabezpieczeń komputera lub proces, następnie zmienne środowiskowe są dostępne przez niezaufane.</span><span class="sxs-lookup"><span data-stu-id="76b2f-122">If the machine or process is compromised, then environment variables can be accessed by untrusted parties.</span></span> <span data-ttu-id="76b2f-123">Dodatkowe środki, aby zapobiec ujawnieniu kluczy tajnych użytkownik nadal może być wymagane.</span><span class="sxs-lookup"><span data-stu-id="76b2f-123">Additional measures to prevent disclosure of user secrets may still be required.</span></span>

## <a name="secret-manager"></a><span data-ttu-id="76b2f-124">Menedżer klucz tajny</span><span class="sxs-lookup"><span data-stu-id="76b2f-124">Secret Manager</span></span>

<span data-ttu-id="76b2f-125">Narzędzie Menedżer klucz tajny są przechowywane poufne dane w projektach poza drzewa Twojego projektu.</span><span class="sxs-lookup"><span data-stu-id="76b2f-125">The Secret Manager tool stores sensitive data for development work outside of your project tree.</span></span> <span data-ttu-id="76b2f-126">Narzędzie Menedżer klucz tajny jest narzędzie projektu, który może służyć do przechowywania kluczy tajnych dla [.NET Core](https://www.microsoft.com/net/core) projektu w czasie projektowania.</span><span class="sxs-lookup"><span data-stu-id="76b2f-126">The Secret Manager tool is a project tool that can be used to store secrets for a [.NET Core](https://www.microsoft.com/net/core) project during development.</span></span> <span data-ttu-id="76b2f-127">Za pomocą narzędzia menedżera klucz tajny można skojarzyć klucze tajne aplikacji z określonego projektu i udostępniać je w wielu projektach.</span><span class="sxs-lookup"><span data-stu-id="76b2f-127">With the Secret Manager tool, you can associate app secrets with a specific project and share them across multiple projects.</span></span>

>[!WARNING]
> <span data-ttu-id="76b2f-128">Narzędzie Menedżer klucz tajny Szyfruj przechowywane klucze tajne i nie powinien być traktowany jako zaufany magazyn.</span><span class="sxs-lookup"><span data-stu-id="76b2f-128">The Secret Manager tool does not encrypt the stored secrets and should not be treated as a trusted store.</span></span> <span data-ttu-id="76b2f-129">Jest tylko do celów programistycznych.</span><span class="sxs-lookup"><span data-stu-id="76b2f-129">It is for development purposes only.</span></span> <span data-ttu-id="76b2f-130">Klucze i wartości są przechowywane w pliku konfiguracji JSON w katalogu profilu użytkownika.</span><span class="sxs-lookup"><span data-stu-id="76b2f-130">The keys and values are stored in a JSON configuration file in the user profile directory.</span></span>

## <a name="installing-the-secret-manager-tool"></a><span data-ttu-id="76b2f-131">Instalowanie narzędzia menedżera klucz tajny</span><span class="sxs-lookup"><span data-stu-id="76b2f-131">Installing the Secret Manager tool</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="76b2f-132">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="76b2f-132">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="76b2f-133">Kliknij prawym przyciskiem myszy projekt w Eksploratorze rozwiązań i wybierz **Edytuj \<project_name\>.csproj** z menu kontekstowego.</span><span class="sxs-lookup"><span data-stu-id="76b2f-133">Right-click the project in Solution Explorer, and select **Edit \<project_name\>.csproj** from the context menu.</span></span> <span data-ttu-id="76b2f-134">Dodaj wybrany element do *.csproj* pliku, a następnie zapisz do przywrócenia skojarzonego pakietu NuGet:</span><span class="sxs-lookup"><span data-stu-id="76b2f-134">Add the highlighted line to the *.csproj* file, and save to restore the associated NuGet package:</span></span>

[!code-xml[Main](app-secrets/sample/UserSecrets/UserSecrets-before.csproj?highlight=10)]

<span data-ttu-id="76b2f-135">Ponownie kliknij prawym przyciskiem myszy projekt w Eksploratorze rozwiązań i wybierz **Zarządzanie kluczy tajnych użytkownika** z menu kontekstowego.</span><span class="sxs-lookup"><span data-stu-id="76b2f-135">Right-click the project in Solution Explorer again, and select **Manage User Secrets** from the context menu.</span></span> <span data-ttu-id="76b2f-136">Ten gest dodaje nowy `UserSecretsId` węzła w ramach `PropertyGroup` z *.csproj* plików, jak w poniższym przykładzie wyróżniono:</span><span class="sxs-lookup"><span data-stu-id="76b2f-136">This gesture adds a new `UserSecretsId` node within a `PropertyGroup` of the *.csproj* file, as highlighted in the following sample:</span></span>

[!code-xml[Main](app-secrets/sample/UserSecrets/UserSecrets-after.csproj?highlight=4)]

<span data-ttu-id="76b2f-137">Zapisywanie zmodyfikowanych *.csproj* również plik zostanie otwarta `secrets.json` plik w edytorze tekstu.</span><span class="sxs-lookup"><span data-stu-id="76b2f-137">Saving the modified *.csproj* file also opens a `secrets.json` file in the text editor.</span></span> <span data-ttu-id="76b2f-138">Zastąp zawartość `secrets.json` pliku następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="76b2f-138">Replace the contents of the `secrets.json` file with the following code:</span></span>

```json
{
    "MySecret": "ValueOfMySecret"
}
```

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="76b2f-139">Kod Visual Studio</span><span class="sxs-lookup"><span data-stu-id="76b2f-139">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="76b2f-140">Dodaj `Microsoft.Extensions.SecretManager.Tools` do *.csproj* pliku i uruchom `dotnet restore`.</span><span class="sxs-lookup"><span data-stu-id="76b2f-140">Add `Microsoft.Extensions.SecretManager.Tools` to the *.csproj* file and run `dotnet restore`.</span></span> <span data-ttu-id="76b2f-141">Te same kroki można użyć do zainstalowania narzędzia menedżera klucz tajny przy użyciu wiersza polecenia.</span><span class="sxs-lookup"><span data-stu-id="76b2f-141">You can use the same steps to install the Secret Manager Tool using for the command line.</span></span>

[!code-xml[Main](app-secrets/sample/UserSecrets/UserSecrets-before.csproj?highlight=10)]

<span data-ttu-id="76b2f-142">Przetestuj narzędzie Menedżer klucz tajny, uruchamiając następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="76b2f-142">Test the Secret Manager tool by running the following command:</span></span>

```console
dotnet user-secrets -h
```

<span data-ttu-id="76b2f-143">Narzędzie Menedżer klucz tajny wyświetli użycia, opcje i pomoc dla polecenia.</span><span class="sxs-lookup"><span data-stu-id="76b2f-143">The Secret Manager tool will display usage, options and command help.</span></span>

> [!NOTE]
> <span data-ttu-id="76b2f-144">Musi być w tym samym katalogu co *.csproj* uruchamianie narzędzia zdefiniowane w pliku *.csproj* pliku `DotNetCliToolReference` węzłów.</span><span class="sxs-lookup"><span data-stu-id="76b2f-144">You must be in the same directory as the *.csproj* file to run tools defined in the *.csproj* file's `DotNetCliToolReference` nodes.</span></span>

<span data-ttu-id="76b2f-145">Narzędzie Menedżer klucz tajny działa na ustawienia konfiguracji określonego projektu, które są przechowywane w profilu użytkownika.</span><span class="sxs-lookup"><span data-stu-id="76b2f-145">The Secret Manager tool operates on project-specific configuration settings that are stored in your user profile.</span></span> <span data-ttu-id="76b2f-146">Aby użyć hasła użytkownika, należy określić projekt `UserSecretsId` wartości w jego *.csproj* pliku.</span><span class="sxs-lookup"><span data-stu-id="76b2f-146">To use user secrets, the project must specify a `UserSecretsId` value in its *.csproj* file.</span></span> <span data-ttu-id="76b2f-147">Wartość `UserSecretsId` jest określana, ale jest zazwyczaj unikatowa do projektu.</span><span class="sxs-lookup"><span data-stu-id="76b2f-147">The value of `UserSecretsId` is arbitrary, but is generally unique to the project.</span></span> <span data-ttu-id="76b2f-148">Deweloperzy zazwyczaj generowania identyfikatora GUID dla `UserSecretsId`.</span><span class="sxs-lookup"><span data-stu-id="76b2f-148">Developers typically generate a GUID for the `UserSecretsId`.</span></span>

<span data-ttu-id="76b2f-149">Dodaj `UserSecretsId` projektu w *.csproj* pliku:</span><span class="sxs-lookup"><span data-stu-id="76b2f-149">Add a `UserSecretsId` for your project in the *.csproj* file:</span></span>

[!code-xml[Main](app-secrets/sample/UserSecrets/UserSecrets-after.csproj?highlight=4)]

<span data-ttu-id="76b2f-150">Ustaw klucz tajny narzędzie Menedżer klucz tajny.</span><span class="sxs-lookup"><span data-stu-id="76b2f-150">Use the Secret Manager tool to set a secret.</span></span> <span data-ttu-id="76b2f-151">Na przykład w oknie polecenia w katalogu projektu, wprowadź następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="76b2f-151">For example, in a command window from the project directory, enter the following:</span></span>

```console
dotnet user-secrets set MySecret ValueOfMySecret
```

<span data-ttu-id="76b2f-152">Można uruchomić narzędzie Menedżer klucz tajny z innych katalogów, ale należy użyć `--project` opcję, aby przekazać w ścieżce do *.csproj* pliku:</span><span class="sxs-lookup"><span data-stu-id="76b2f-152">You can run the Secret Manager tool from other directories, but you must use the `--project` option to pass in the path to the *.csproj* file:</span></span>
 
```console
dotnet user-secrets set MySecret ValueOfMySecret --project c:\work\WebApp1\src\webapp1
```

<span data-ttu-id="76b2f-153">Narzędzie Menedżer klucz tajny służy również do, Usuń, a następnie wyczyść klucze tajne aplikacji.</span><span class="sxs-lookup"><span data-stu-id="76b2f-153">You can also use the Secret Manager tool to list, remove and clear app secrets.</span></span>

-----

## <a name="accessing-user-secrets-via-configuration"></a><span data-ttu-id="76b2f-154">Uzyskiwanie dostępu do kluczy tajnych użytkownika za pomocą konfiguracji</span><span class="sxs-lookup"><span data-stu-id="76b2f-154">Accessing user secrets via configuration</span></span>

<span data-ttu-id="76b2f-155">Klucz tajny Menedżera kluczy tajnych dostępu za pomocą systemu konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="76b2f-155">You access Secret Manager secrets through the configuration system.</span></span> <span data-ttu-id="76b2f-156">Dodaj `Microsoft.Extensions.Configuration.UserSecrets` pakiet, a następnie uruchom `dotnet restore`.</span><span class="sxs-lookup"><span data-stu-id="76b2f-156">Add the `Microsoft.Extensions.Configuration.UserSecrets` package and run `dotnet restore`.</span></span>

<span data-ttu-id="76b2f-157">Dodaj użytkownika kluczy tajnych konfiguracji źródła `Startup` metody:</span><span class="sxs-lookup"><span data-stu-id="76b2f-157">Add the user secrets configuration source to the `Startup` method:</span></span>

[!code-csharp[Main](app-secrets/sample/UserSecrets/Startup.cs?highlight=16-19)]

<span data-ttu-id="76b2f-158">Aby dostęp do kluczy tajnych użytkownika przez interfejs API konfiguracji:</span><span class="sxs-lookup"><span data-stu-id="76b2f-158">You can access user secrets via the configuration API:</span></span>

[!code-csharp[Main](app-secrets/sample/UserSecrets/Startup.cs?highlight=26-29)]

## <a name="how-the-secret-manager-tool-works"></a><span data-ttu-id="76b2f-159">Jak działa narzędzie Menedżer klucz tajny</span><span class="sxs-lookup"><span data-stu-id="76b2f-159">How the Secret Manager tool works</span></span>

<span data-ttu-id="76b2f-160">Narzędzie Menedżer klucz tajny optymalizacji abstracts szczegóły implementacji, takich jak jak i gdzie są przechowywane wartości.</span><span class="sxs-lookup"><span data-stu-id="76b2f-160">The Secret Manager tool abstracts away the implementation details, such as where and how the values are stored.</span></span> <span data-ttu-id="76b2f-161">Można użyć narzędzia bez uprzedniego uzyskania informacji o tych szczegóły implementacji.</span><span class="sxs-lookup"><span data-stu-id="76b2f-161">You can use the tool without knowing these implementation details.</span></span> <span data-ttu-id="76b2f-162">W bieżącej wersji wartości są przechowywane w [JSON](http://json.org/) pliku konfiguracji w katalogu profilu użytkownika:</span><span class="sxs-lookup"><span data-stu-id="76b2f-162">In the current version, the values are stored in a [JSON](http://json.org/) configuration file in the user profile directory:</span></span>

* <span data-ttu-id="76b2f-163">System Windows:`%APPDATA%\microsoft\UserSecrets\<userSecretsId>\secrets.json`</span><span class="sxs-lookup"><span data-stu-id="76b2f-163">Windows: `%APPDATA%\microsoft\UserSecrets\<userSecretsId>\secrets.json`</span></span>

* <span data-ttu-id="76b2f-164">Linux:`~/.microsoft/usersecrets/<userSecretsId>/secrets.json`</span><span class="sxs-lookup"><span data-stu-id="76b2f-164">Linux: `~/.microsoft/usersecrets/<userSecretsId>/secrets.json`</span></span>

* <span data-ttu-id="76b2f-165">Mac:`~/.microsoft/usersecrets/<userSecretsId>/secrets.json`</span><span class="sxs-lookup"><span data-stu-id="76b2f-165">Mac: `~/.microsoft/usersecrets/<userSecretsId>/secrets.json`</span></span>

<span data-ttu-id="76b2f-166">Wartość `userSecretsId` pochodzi z wartością określoną w *.csproj* pliku.</span><span class="sxs-lookup"><span data-stu-id="76b2f-166">The value of `userSecretsId` comes from the value specified in *.csproj* file.</span></span>

<span data-ttu-id="76b2f-167">Nie należy pisania kodu, który zależy od lokalizacji lub format dane zapisane za pomocą narzędzia menedżera klucz tajny, jak te szczegóły implementacji mogą ulec zmianie.</span><span class="sxs-lookup"><span data-stu-id="76b2f-167">You should not write code that depends on the location or format of the data saved with the Secret Manager tool, as these implementation details might change.</span></span> <span data-ttu-id="76b2f-168">Na przykład tajny wartości są obecnie *nie* dzisiaj zaszyfrowany, ale może być w przyszłości.</span><span class="sxs-lookup"><span data-stu-id="76b2f-168">For example, the secret values are currently *not* encrypted today, but could be someday.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="76b2f-169">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="76b2f-169">Additional Resources</span></span>

* [<span data-ttu-id="76b2f-170">Konfiguracja</span><span class="sxs-lookup"><span data-stu-id="76b2f-170">Configuration</span></span>](xref:fundamentals/configuration/index)
