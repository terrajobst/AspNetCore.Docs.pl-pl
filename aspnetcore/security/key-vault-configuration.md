---
title: Dostawca konfiguracji usługi Azure Key Vault w programie ASP.NET Core
author: guardrex
description: Dowiedz się, jak skonfigurować aplikację tak, za pomocą pary nazwa wartość ładowane w czasie wykonywania za pomocą dostawcy konfiguracji magazynu kluczy Azure.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 02/25/2019
uid: security/key-vault-configuration
ms.openlocfilehash: 8fd1cca1803d3f1d44d80ec63c5cfc259cbdaf55
ms.sourcegitcommit: 78339e9891c8676db01a6e81e9cb0cdaa280162f
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/17/2019
ms.locfileid: "59012698"
---
# <a name="azure-key-vault-configuration-provider-in-aspnet-core"></a><span data-ttu-id="43b35-103">Dostawca konfiguracji usługi Azure Key Vault w programie ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="43b35-103">Azure Key Vault Configuration Provider in ASP.NET Core</span></span>

<span data-ttu-id="43b35-104">Przez [Luke Latham](https://github.com/guardrex) i [Andrew Stanton pielęgniarki](https://github.com/anurse)</span><span class="sxs-lookup"><span data-stu-id="43b35-104">By [Luke Latham](https://github.com/guardrex) and [Andrew Stanton-Nurse](https://github.com/anurse)</span></span>

<span data-ttu-id="43b35-105">W tym dokumencie wyjaśniono, jak używać [Microsoft Azure Key Vault](https://azure.microsoft.com/services/key-vault/) dostawcę konfiguracji, które można załadować wartości konfiguracji aplikacji z wpisy tajne w usłudze Azure Key Vault.</span><span class="sxs-lookup"><span data-stu-id="43b35-105">This document explains how to use the [Microsoft Azure Key Vault](https://azure.microsoft.com/services/key-vault/) Configuration Provider to load app configuration values from Azure Key Vault secrets.</span></span> <span data-ttu-id="43b35-106">Usługa Azure Key Vault to usługa oparta na chmurze, które pomaga w ochronę kluczy kryptograficznych i wpisów tajnych używanych przez aplikacje i usługi.</span><span class="sxs-lookup"><span data-stu-id="43b35-106">Azure Key Vault is a cloud-based service that assists in safeguarding cryptographic keys and secrets used by apps and services.</span></span> <span data-ttu-id="43b35-107">Typowe scenariusze dotyczące korzystania z usługi Azure Key Vault z aplikacji platformy ASP.NET Core obejmują:</span><span class="sxs-lookup"><span data-stu-id="43b35-107">Common scenarios for using Azure Key Vault with ASP.NET Core apps include:</span></span>

* <span data-ttu-id="43b35-108">Kontrolowanie dostępu do danych poufnych konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="43b35-108">Controlling access to sensitive configuration data.</span></span>
* <span data-ttu-id="43b35-109">Spełniające wymagania FIPS 140-2 Level 2 zweryfikować sprzętowych modułów zabezpieczeń (HSM), podczas zapisywania danych konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="43b35-109">Meeting the requirement for FIPS 140-2 Level 2 validated Hardware Security Modules (HSM's) when storing configuration data.</span></span>

<span data-ttu-id="43b35-110">Ten scenariusz jest dostępna dla aplikacji przeznaczonych dla platformy ASP.NET Core 2.1 lub nowszej.</span><span class="sxs-lookup"><span data-stu-id="43b35-110">This scenario is available for apps that target ASP.NET Core 2.1 or later.</span></span>

<span data-ttu-id="43b35-111">[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/key-vault-configuration/sample) ([sposobu pobierania](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="43b35-111">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/key-vault-configuration/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="packages"></a><span data-ttu-id="43b35-112">Pakiety</span><span class="sxs-lookup"><span data-stu-id="43b35-112">Packages</span></span>

<span data-ttu-id="43b35-113">Aby używać dostawcy konfiguracji magazynu kluczy Azure, Dodaj odwołanie do pakietu [Microsoft.Extensions.Configuration.AzureKeyVault](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.AzureKeyVault/) pakietu.</span><span class="sxs-lookup"><span data-stu-id="43b35-113">To use the Azure Key Vault Configuration Provider, add a package reference to the [Microsoft.Extensions.Configuration.AzureKeyVault](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.AzureKeyVault/) package.</span></span>

<span data-ttu-id="43b35-114">Przyjęcie [zarządzanych tożsamości dla zasobów platformy Azure](/azure/active-directory/managed-identities-azure-resources/overview) scenariusza, Dodaj odwołanie do pakietu [Microsoft.Azure.Services.AppAuthentication](https://www.nuget.org/packages/Microsoft.Azure.Services.AppAuthentication/) pakietu.</span><span class="sxs-lookup"><span data-stu-id="43b35-114">To adopt the [Managed identities for Azure resources](/azure/active-directory/managed-identities-azure-resources/overview) scenario, add a package reference to the [Microsoft.Azure.Services.AppAuthentication](https://www.nuget.org/packages/Microsoft.Azure.Services.AppAuthentication/) package.</span></span>

> [!NOTE]
> <span data-ttu-id="43b35-115">W czasie pisania najnowszą stabilną wersję `Microsoft.Azure.Services.AppAuthentication`, wersja `1.0.3`, zapewnia obsługę [przypisany systemowo zarządzanych tożsamości](/azure/active-directory/managed-identities-azure-resources/overview#how-does-the-managed-identities-for-azure-resources-worka-namehow-does-it-worka).</span><span class="sxs-lookup"><span data-stu-id="43b35-115">At the time of writing, the latest stable release of `Microsoft.Azure.Services.AppAuthentication`, version `1.0.3`, provides support for [system-assigned managed identities](/azure/active-directory/managed-identities-azure-resources/overview#how-does-the-managed-identities-for-azure-resources-worka-namehow-does-it-worka).</span></span> <span data-ttu-id="43b35-116">Obsługa *przypisanych do użytkowników zarządzanych tożsamości* jest dostępna w `1.2.0-preview2` pakietu.</span><span class="sxs-lookup"><span data-stu-id="43b35-116">Support for *user-assigned managed identities* is available in the `1.2.0-preview2` package.</span></span> <span data-ttu-id="43b35-117">W tym temacie przedstawiono użycie tożsamości zarządzanych przez system, a podana Przykładowa aplikacja korzysta z wersji `1.0.3` z `Microsoft.Azure.Services.AppAuthentication` pakietu.</span><span class="sxs-lookup"><span data-stu-id="43b35-117">This topic demonstrates the use of system-managed identities, and the provided sample app uses version `1.0.3` of the `Microsoft.Azure.Services.AppAuthentication` package.</span></span>

## <a name="sample-app"></a><span data-ttu-id="43b35-118">Przykładowa aplikacja</span><span class="sxs-lookup"><span data-stu-id="43b35-118">Sample app</span></span>

<span data-ttu-id="43b35-119">Przykładowa aplikacja jest uruchamiana w jednym z dwóch trybów ustalany na podstawie `#define` instrukcji na górze *Program.cs* pliku:</span><span class="sxs-lookup"><span data-stu-id="43b35-119">The sample app runs in either of two modes determined by the `#define` statement at the top of the *Program.cs* file:</span></span>

* <span data-ttu-id="43b35-120">`Certificate` &ndash; Zademonstrowano użycie certyfikat X.509 i identyfikator klienta systemu Azure klucza magazynu do dostępu do kluczy tajnych przechowywanych w usłudze Azure Key Vault.</span><span class="sxs-lookup"><span data-stu-id="43b35-120">`Certificate` &ndash; Demonstrates the use of an Azure Key Vault Client ID and X.509 certificate to access secrets stored in Azure Key Vault.</span></span> <span data-ttu-id="43b35-121">Ta wersja przykładu można uruchomić z dowolnego miejsca i wdrażać w usłudze Azure App Service lub dowolnego hosta, które umożliwia obsługę aplikacji ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="43b35-121">This version of the sample can be run from any location, deployed to Azure App Service or any host capable of serving an ASP.NET Core app.</span></span>
* <span data-ttu-id="43b35-122">`Managed` &ndash; Pokazuje sposób użycia [zarządzanych tożsamości dla zasobów platformy Azure](/azure/active-directory/managed-identities-azure-resources/overview) można uwierzytelnić aplikację usługi Azure Key Vault przy użyciu uwierzytelniania usługi Azure AD bez poświadczeń przechowywanych w kodzie aplikacji lub konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="43b35-122">`Managed` &ndash; Demonstrates how to use [Managed identities for Azure resources](/azure/active-directory/managed-identities-azure-resources/overview) to authenticate the app to Azure Key Vault with Azure AD authentication without credentials stored in the app's code or configuration.</span></span> <span data-ttu-id="43b35-123">Korzystając z zarządzanych tożsamości do uwierzytelniania, identyfikator aplikacji w usłudze Azure AD i hasło (klucz tajny klienta) nie są wymagane.</span><span class="sxs-lookup"><span data-stu-id="43b35-123">When using managed identities to authenticate, an Azure AD Application ID and Password (Client Secret) aren't required.</span></span> <span data-ttu-id="43b35-124">`Managed` Wersję przykładu, należy wdrożyć na platformie Azure.</span><span class="sxs-lookup"><span data-stu-id="43b35-124">The `Managed` version of the sample must be deployed to Azure.</span></span> <span data-ttu-id="43b35-125">Postępuj zgodnie ze wskazówkami w [używania tożsamości zarządzanych zasobów platformy Azure](#use-managed-identities-for-azure-resources) sekcji.</span><span class="sxs-lookup"><span data-stu-id="43b35-125">Follow the guidance in the [Use the Managed identities for Azure resources](#use-managed-identities-for-azure-resources) section.</span></span>

<span data-ttu-id="43b35-126">Aby uzyskać więcej informacji na temat konfigurowania przykładowej aplikacji za pomocą dyrektywy preprocesora (`#define`), zobacz <xref:index#preprocessor-directives-in-sample-code>.</span><span class="sxs-lookup"><span data-stu-id="43b35-126">For more information on how to configure a sample app using preprocessor directives (`#define`), see <xref:index#preprocessor-directives-in-sample-code>.</span></span>

## <a name="secret-storage-in-the-development-environment"></a><span data-ttu-id="43b35-127">Wpisu tajnego magazynu w środowisku programistycznym</span><span class="sxs-lookup"><span data-stu-id="43b35-127">Secret storage in the Development environment</span></span>

<span data-ttu-id="43b35-128">Ustaw lokalnie przy użyciu kluczy tajnych [narzędzie Menedżer klucz tajny](xref:security/app-secrets).</span><span class="sxs-lookup"><span data-stu-id="43b35-128">Set secrets locally using the [Secret Manager tool](xref:security/app-secrets).</span></span> <span data-ttu-id="43b35-129">Jeśli przykładowa aplikacja jest uruchamiana na komputerze lokalnym w środowisku programistycznym, wpisów tajnych są ładowane z magazynu lokalnego Menedżera klucz tajny.</span><span class="sxs-lookup"><span data-stu-id="43b35-129">When the sample app runs on the local machine in the Development environment, secrets are loaded from the local Secret Manager store.</span></span>

<span data-ttu-id="43b35-130">Narzędzie Menedżer klucz tajny wymaga `<UserSecretsId>` właściwość w pliku projektu aplikacji.</span><span class="sxs-lookup"><span data-stu-id="43b35-130">The Secret Manager tool requires a `<UserSecretsId>` property in the app's project file.</span></span> <span data-ttu-id="43b35-131">Ustaw wartość właściwości (`{GUID}`) do dowolnej Unikatowy identyfikator GUID:</span><span class="sxs-lookup"><span data-stu-id="43b35-131">Set the property value (`{GUID}`) to any unique GUID:</span></span>

```xml
<PropertyGroup>
  <UserSecretsId>{GUID}</UserSecretsId>
</PropertyGroup>
```

<span data-ttu-id="43b35-132">Klucze tajne są tworzone jako pary nazwa wartość.</span><span class="sxs-lookup"><span data-stu-id="43b35-132">Secrets are created as name-value pairs.</span></span> <span data-ttu-id="43b35-133">Użyj wartości hierarchicznych (sekcji konfiguracji) `:` (dwukropek) jako separator w [konfiguracji platformy ASP.NET Core](xref:fundamentals/configuration/index) nazw kluczy.</span><span class="sxs-lookup"><span data-stu-id="43b35-133">Hierarchical values (configuration sections) use a `:` (colon) as a separator in [ASP.NET Core configuration](xref:fundamentals/configuration/index) key names.</span></span>

<span data-ttu-id="43b35-134">Menedżer klucz tajny jest używany z powłoki poleceń, otwarte, aby zawartość katalogu głównego projektu, gdzie `{SECRET NAME}` nazywa się i `{SECRET VALUE}` jest wartością:</span><span class="sxs-lookup"><span data-stu-id="43b35-134">The Secret Manager is used from a command shell opened to the project's content root, where `{SECRET NAME}` is the name and `{SECRET VALUE}` is the value:</span></span>

```console
dotnet user-secrets set "{SECRET NAME}" "{SECRET VALUE}"
```

<span data-ttu-id="43b35-135">W powłoce poleceń z katalogu głównego zawartości projektu, aby ustawić wpisów tajnych dla przykładowej aplikacji, wykonaj następujące polecenia:</span><span class="sxs-lookup"><span data-stu-id="43b35-135">Execute the following commands in a command shell from the project's content root to set the secrets for the sample app:</span></span>

```console
dotnet user-secrets set "SecretName" "secret_value_1_dev"
dotnet user-secrets set "Section:SecretName" "secret_value_2_dev"
```

<span data-ttu-id="43b35-136">Kiedy te klucze tajne są przechowywane w usłudze Azure Key Vault w [wpisu tajnego magazynu w środowisku produkcyjnym za pomocą usługi Azure Key Vault](#secret-storage-in-the-production-environment-with-azure-key-vault) sekcji `_dev` sufiks jest zmieniana na `_prod`.</span><span class="sxs-lookup"><span data-stu-id="43b35-136">When these secrets are stored in Azure Key Vault in the [Secret storage in the Production environment with Azure Key Vault](#secret-storage-in-the-production-environment-with-azure-key-vault) section, the `_dev` suffix is changed to `_prod`.</span></span> <span data-ttu-id="43b35-137">Sufiks oferuje wizualną w danych wyjściowych aplikacji wskazujący źródła wartości konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="43b35-137">The suffix provides a visual cue in the app's output indicating the source of the configuration values.</span></span>

## <a name="secret-storage-in-the-production-environment-with-azure-key-vault"></a><span data-ttu-id="43b35-138">Wpisu tajnego magazynu w środowisku produkcyjnym za pomocą usługi Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="43b35-138">Secret storage in the Production environment with Azure Key Vault</span></span>

<span data-ttu-id="43b35-139">Z instrukcjami wyświetlanymi przez [Szybki Start: Ustawianie i pobieranie wpisu tajnego z usługi Azure Key Vault przy użyciu wiersza polecenia platformy Azure](/azure/key-vault/quick-create-cli) tematu są podsumowywane w tym miejscu służą do tworzenia usługi Azure Key Vault i przechowywania wpisów tajnych używanych przez aplikację przykładową.</span><span class="sxs-lookup"><span data-stu-id="43b35-139">The instructions provided by the [Quickstart: Set and retrieve a secret from Azure Key Vault using Azure CLI](/azure/key-vault/quick-create-cli) topic are summarized here for creating an Azure Key Vault and storing secrets used by the sample app.</span></span> <span data-ttu-id="43b35-140">Zapoznaj się z tematem, aby uzyskać więcej informacji.</span><span class="sxs-lookup"><span data-stu-id="43b35-140">Refer to the topic for further details.</span></span>

1. <span data-ttu-id="43b35-141">Otwórz usługa Azure Cloud shell przy użyciu jednej z następujących metod w [witryny Azure portal](https://portal.azure.com/):</span><span class="sxs-lookup"><span data-stu-id="43b35-141">Open Azure Cloud shell using any one of the following methods in the [Azure portal](https://portal.azure.com/):</span></span>

   * <span data-ttu-id="43b35-142">Wybierz **wypróbuj** w prawym górnym rogu bloku kodu.</span><span class="sxs-lookup"><span data-stu-id="43b35-142">Select **Try It** in the upper-right corner of a code block.</span></span> <span data-ttu-id="43b35-143">W polu tekstowym, należy użyć ciągu "Wiersza polecenia platformy Azure".</span><span class="sxs-lookup"><span data-stu-id="43b35-143">Use the search string "Azure CLI" in the text box.</span></span>
   * <span data-ttu-id="43b35-144">Otwórz usługę Cloud Shell w przeglądarce za pośrednictwem **Launch Cloud Shell** przycisku.</span><span class="sxs-lookup"><span data-stu-id="43b35-144">Open Cloud Shell in your browser with the **Launch Cloud Shell** button.</span></span>
   * <span data-ttu-id="43b35-145">Wybierz **Cloud Shell** przycisk menu w prawym górnym rogu witryny Azure portal.</span><span class="sxs-lookup"><span data-stu-id="43b35-145">Select the **Cloud Shell** button on the menu in the upper-right corner of the Azure portal.</span></span>

   <span data-ttu-id="43b35-146">Aby uzyskać więcej informacji, zobacz [interfejsu wiersza polecenia platformy Azure (CLI)](/cli/azure/) i [Omówienie usługi Azure Cloud Shell](/azure/cloud-shell/overview).</span><span class="sxs-lookup"><span data-stu-id="43b35-146">For more information, see [Azure Command-Line Interface (CLI)](/cli/azure/) and [Overview of Azure Cloud Shell](/azure/cloud-shell/overview).</span></span>

1. <span data-ttu-id="43b35-147">Jeśli nie są już uwierzytelniony, zaloguj się przy użyciu `az login` polecenia.</span><span class="sxs-lookup"><span data-stu-id="43b35-147">If you aren't already authenticated, sign in with the `az login` command.</span></span>

1. <span data-ttu-id="43b35-148">Utwórz grupę zasobów za pomocą następującego polecenia, gdzie `{RESOURCE GROUP NAME}` to nazwa grupy zasobów dla nowej grupy zasobów i `{LOCATION}` to region platformy Azure (datacenter):</span><span class="sxs-lookup"><span data-stu-id="43b35-148">Create a resource group with the following command, where `{RESOURCE GROUP NAME}` is the resource group name for the new resource group and `{LOCATION}` is the Azure region (datacenter):</span></span>

   ```console
   az group create --name "{RESOURCE GROUP NAME}" --location {LOCATION}
   ```

1. <span data-ttu-id="43b35-149">Tworzenie magazynu kluczy w grupie zasobów za pomocą następującego polecenia, gdzie `{KEY VAULT NAME}` to nazwa dla nowego magazynu kluczy i `{LOCATION}` to region platformy Azure (datacenter):</span><span class="sxs-lookup"><span data-stu-id="43b35-149">Create a key vault in the resource group with the following command, where `{KEY VAULT NAME}` is the name for the new key vault and `{LOCATION}` is the Azure region (datacenter):</span></span>

   ```console
   az keyvault create --name "{KEY VAULT NAME}" --resource-group "{RESOURCE GROUP NAME}" --location {LOCATION}
   ```

1. <span data-ttu-id="43b35-150">Tworzenie wpisów tajnych w magazynie kluczy jako pary nazwa wartość.</span><span class="sxs-lookup"><span data-stu-id="43b35-150">Create secrets in the key vault as name-value pairs.</span></span>

   <span data-ttu-id="43b35-151">Usługa Azure Key Vault nazw kluczy tajnych są ograniczone do znaki alfanumeryczne i łączniki.</span><span class="sxs-lookup"><span data-stu-id="43b35-151">Azure Key Vault secret names are limited to alphanumeric characters and dashes.</span></span> <span data-ttu-id="43b35-152">Użyj wartości hierarchicznych (sekcji konfiguracji) `--` (dwa łączniki) jako separatora.</span><span class="sxs-lookup"><span data-stu-id="43b35-152">Hierarchical values (configuration sections) use `--` (two dashes) as a separator.</span></span> <span data-ttu-id="43b35-153">Dwukropki, które są zwykle używane do ograniczania sekcji w podkluczu w [konfiguracji platformy ASP.NET Core](xref:fundamentals/configuration/index), nie są dozwolone w nazwach wpisu tajnego usługi key vault.</span><span class="sxs-lookup"><span data-stu-id="43b35-153">Colons, which are normally used to delimit a section from a subkey in [ASP.NET Core configuration](xref:fundamentals/configuration/index), aren't allowed in key vault secret names.</span></span> <span data-ttu-id="43b35-154">W związku z tym dwa łączniki są używane i zamienione dla dwukropek, gdy wpisy tajne są ładowane do konfiguracji aplikacji.</span><span class="sxs-lookup"><span data-stu-id="43b35-154">Therefore, two dashes are used and swapped for a colon when the secrets are loaded into the app's configuration.</span></span>

   <span data-ttu-id="43b35-155">Następujące klucze tajne są do użytku z przykładowej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="43b35-155">The following secrets are for use with the sample app.</span></span> <span data-ttu-id="43b35-156">Wartości obejmują `_prod` sufiks, aby odróżnić je od `_dev` sufiks wartości załadowane w środowisku programistycznym z wpisami tajnymi użytkowników.</span><span class="sxs-lookup"><span data-stu-id="43b35-156">The values include a `_prod` suffix to distinguish them from the `_dev` suffix values loaded in the Development environment from User Secrets.</span></span> <span data-ttu-id="43b35-157">Zastąp `{KEY VAULT NAME}` nazwą magazynu kluczy, który został utworzony w poprzednim kroku:</span><span class="sxs-lookup"><span data-stu-id="43b35-157">Replace `{KEY VAULT NAME}` with the name of the key vault that you created in the prior step:</span></span>

   ```console
   az keyvault secret set --vault-name "{KEY VAULT NAME}" --name "SecretName" --value "secret_value_1_prod"
   az keyvault secret set --vault-name "{KEY VAULT NAME}" --name "Section--SecretName" --value "secret_value_2_prod"
   ```

## <a name="use-application-id-and-client-secret-for-non-azure-hosted-apps"></a><span data-ttu-id="43b35-158">Użyj Identyfikatora aplikacji i klucz tajny klienta dla aplikacji hostowanej platformy Azure</span><span class="sxs-lookup"><span data-stu-id="43b35-158">Use Application ID and Client Secret for non-Azure-hosted apps</span></span>

<span data-ttu-id="43b35-159">Konfigurowanie usługi Azure AD i usługi Azure Key Vault oraz aplikacji, które można użyć Identyfikatora aplikacji Azure Active Directory i X.509 certyfikatu do uwierzytelniania do magazynu kluczy **gdy aplikacja jest hostowana poza platformą Azure**.</span><span class="sxs-lookup"><span data-stu-id="43b35-159">Configure Azure AD, Azure Key Vault, and the app to use an Azure Active Directory Application ID and X.509 certificate to authenticate to a key vault **when the app is hosted outside of Azure**.</span></span> <span data-ttu-id="43b35-160">Aby uzyskać więcej informacji, zobacz [o kluczy, wpisów tajnych i certyfikatów](/azure/key-vault/about-keys-secrets-and-certificates).</span><span class="sxs-lookup"><span data-stu-id="43b35-160">For more information, see [About keys, secrets, and certificates](/azure/key-vault/about-keys-secrets-and-certificates).</span></span>

> [!NOTE]
> <span data-ttu-id="43b35-161">Przy użyciu Identyfikatora aplikacji i X.509 certyfikatu jest obsługiwana dla aplikacji hostowanych na platformie Azure, zalecamy używanie [zarządzanych tożsamości dla zasobów platformy Azure](#use-managed-identities-for-azure-resources) odnośnie do hostowania aplikacji na platformie Azure.</span><span class="sxs-lookup"><span data-stu-id="43b35-161">Although using an Application ID and X.509 certificate is supported for apps hosted in Azure, we recommend using [Managed identities for Azure resources](#use-managed-identities-for-azure-resources) when hosting an app in Azure.</span></span> <span data-ttu-id="43b35-162">Zarządzanych tożsamości nie wymagają przechowywania certyfikatu w aplikacji lub w środowisku programistycznym.</span><span class="sxs-lookup"><span data-stu-id="43b35-162">Managed identities don't require storing a certificate in the app or in the development environment.</span></span>

<span data-ttu-id="43b35-163">Ta aplikacja używa przykładowych Identyfikatora aplikacji oraz X.509 certyfikatu, kiedy `#define` instrukcji na górze *Program.cs* pliku jest ustawiona na `Certificate`.</span><span class="sxs-lookup"><span data-stu-id="43b35-163">The sample app uses an Application ID and X.509 certificate when the `#define` statement at the top of the *Program.cs* file is set to `Certificate`.</span></span>

1. <span data-ttu-id="43b35-164">Rejestrowanie aplikacji w usłudze Azure AD (**rejestracje aplikacji**).</span><span class="sxs-lookup"><span data-stu-id="43b35-164">Register the app with Azure AD (**App registrations**).</span></span>
1. <span data-ttu-id="43b35-165">Przekaż klucz publiczny:</span><span class="sxs-lookup"><span data-stu-id="43b35-165">Upload the public key:</span></span>
   1. <span data-ttu-id="43b35-166">Wybierz aplikację w usłudze Azure AD.</span><span class="sxs-lookup"><span data-stu-id="43b35-166">Select the app in Azure AD.</span></span>
   1. <span data-ttu-id="43b35-167">Przejdź do **ustawienia** > **klucze**.</span><span class="sxs-lookup"><span data-stu-id="43b35-167">Navigate to **Settings** > **Keys**.</span></span>
   1. <span data-ttu-id="43b35-168">Wybierz **Przekaż klucz publiczny** można przekazać certyfikatu, który zawiera klucz publiczny.</span><span class="sxs-lookup"><span data-stu-id="43b35-168">Select **Upload Public Key** to upload the certificate, which contains the public key.</span></span> <span data-ttu-id="43b35-169">Oprócz używania *cer*, *PEM*, lub *.crt* certyfikatu, *PFX* można przekazać certyfikatu.</span><span class="sxs-lookup"><span data-stu-id="43b35-169">In addition to using a *.cer*, *.pem*, or *.crt* certificate, a *.pfx* certificate can be uploaded.</span></span>
1. <span data-ttu-id="43b35-170">Store nazwa magazynu kluczy oraz Identyfikatora aplikacji w aplikacji *appsettings.json* pliku.</span><span class="sxs-lookup"><span data-stu-id="43b35-170">Store the key vault name and Application ID in the app's *appsettings.json* file.</span></span> <span data-ttu-id="43b35-171">Umieść certyfikat w katalogu głównym aplikacji lub w magazynie certyfikatów aplikacji&dagger;.</span><span class="sxs-lookup"><span data-stu-id="43b35-171">Place the certificate at the root of the app or in the app's certificate store&dagger;.</span></span>
1. <span data-ttu-id="43b35-172">Przejdź do **klucza magazynów** w witrynie Azure portal.</span><span class="sxs-lookup"><span data-stu-id="43b35-172">Navigate to **Key vaults** in the Azure portal.</span></span>
1. <span data-ttu-id="43b35-173">Wybierz magazyn kluczy, który został utworzony w [wpisu tajnego magazynu w środowisku produkcyjnym za pomocą usługi Azure Key Vault](#secret-storage-in-the-production-environment-with-azure-key-vault) sekcji.</span><span class="sxs-lookup"><span data-stu-id="43b35-173">Select the key vault that you created in the [Secret storage in the Production environment with Azure Key Vault](#secret-storage-in-the-production-environment-with-azure-key-vault) section.</span></span>
1. <span data-ttu-id="43b35-174">Wybierz **zasady dostępu**.</span><span class="sxs-lookup"><span data-stu-id="43b35-174">Select **Access policies**.</span></span>
1. <span data-ttu-id="43b35-175">Wybierz **Dodaj nowe**.</span><span class="sxs-lookup"><span data-stu-id="43b35-175">Select **Add new**.</span></span>
1. <span data-ttu-id="43b35-176">Wybierz **Wybierz podmiot zabezpieczeń** i wybierz zarejestrowany aplikację według nazwy.</span><span class="sxs-lookup"><span data-stu-id="43b35-176">Select **Select principal** and select the registered app by name.</span></span> <span data-ttu-id="43b35-177">Wybierz **wybierz** przycisku.</span><span class="sxs-lookup"><span data-stu-id="43b35-177">Select the **Select** button.</span></span>
1. <span data-ttu-id="43b35-178">Otwórz **uprawnienia klucza tajnego** i zapewniają aplikacji za pomocą **uzyskać** i **listy** uprawnienia.</span><span class="sxs-lookup"><span data-stu-id="43b35-178">Open **Secret permissions** and provide the app with **Get** and **List** permissions.</span></span>
1. <span data-ttu-id="43b35-179">Kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="43b35-179">Select **OK**.</span></span>
1. <span data-ttu-id="43b35-180">Wybierz pozycję **Zapisz**.</span><span class="sxs-lookup"><span data-stu-id="43b35-180">Select **Save**.</span></span>
1. <span data-ttu-id="43b35-181">Wdróż aplikację.</span><span class="sxs-lookup"><span data-stu-id="43b35-181">Deploy the app.</span></span>

<span data-ttu-id="43b35-182">&dagger;W przykładowej aplikacji, certyfikat jest używany bezpośrednio z pliku fizycznego certyfikatu w katalogu głównym aplikacji, tworząc nową `X509Certificate2` podczas wywoływania `AddAzureKeyVault`.</span><span class="sxs-lookup"><span data-stu-id="43b35-182">&dagger;In the sample app, the certificate is consumed directly from the physical certificate file in the root of the app by creating a new `X509Certificate2` when calling `AddAzureKeyVault`.</span></span> <span data-ttu-id="43b35-183">Alternatywnym podejściem jest umożliwienie systemu operacyjnego w celu zarządzania certyfikatu.</span><span class="sxs-lookup"><span data-stu-id="43b35-183">An alternative approach is to allow the OS to manage the certificate.</span></span> <span data-ttu-id="43b35-184">Aby uzyskać więcej informacji, zobacz [Zezwalaj systemu operacyjnego w celu zarządzania certyfikatów X.509](#allow-the-os-to-manage-the-x509-certificate) sekcji.</span><span class="sxs-lookup"><span data-stu-id="43b35-184">For more information, see the [Allow the OS to manage the X.509 certificate](#allow-the-os-to-manage-the-x509-certificate) section.</span></span>

<span data-ttu-id="43b35-185">`Certificate` Przykładowa aplikacja uzyskuje jego wartości konfiguracji z `IConfigurationRoot` o takiej samej nazwie jak nazwa wpisu tajnego:</span><span class="sxs-lookup"><span data-stu-id="43b35-185">The `Certificate` sample app obtains its configuration values from `IConfigurationRoot` with the same name as the secret name:</span></span>

* <span data-ttu-id="43b35-186">Wartości inne niż hierarchiczne: Wartość `SecretName` są uzyskiwane z `config["SecretName"]`.</span><span class="sxs-lookup"><span data-stu-id="43b35-186">Non-hierarchical values: The value for `SecretName` is obtained with `config["SecretName"]`.</span></span>
* <span data-ttu-id="43b35-187">Hierarchiczna wartości (sekcje): Użyj `:` notacji (dwukropek) lub `GetSection` — metoda rozszerzenia.</span><span class="sxs-lookup"><span data-stu-id="43b35-187">Hierarchical values (sections): Use `:` (colon) notation or the `GetSection` extension method.</span></span> <span data-ttu-id="43b35-188">Użyj jednej z tych metod można uzyskać wartości konfiguracji:</span><span class="sxs-lookup"><span data-stu-id="43b35-188">Use either of these approaches to obtain the configuration value:</span></span>
  * `config["Section:SecretName"]`
  * `config.GetSection("Section")["SecretName"]`

<span data-ttu-id="43b35-189">Wywołania aplikacji `AddAzureKeyVault` przy użyciu wartości dostarczone przez *appsettings.json* pliku:</span><span class="sxs-lookup"><span data-stu-id="43b35-189">The app calls `AddAzureKeyVault` with values supplied by the *appsettings.json* file:</span></span>

[!code-csharp[](key-vault-configuration/sample/Program.cs?name=snippet1&highlight=12-15)]

<span data-ttu-id="43b35-190">Przykładowe wartości:</span><span class="sxs-lookup"><span data-stu-id="43b35-190">Example values:</span></span>

* <span data-ttu-id="43b35-191">Nazwa magazynu kluczy: `contosovault`</span><span class="sxs-lookup"><span data-stu-id="43b35-191">Key vault name: `contosovault`</span></span>
* <span data-ttu-id="43b35-192">Identyfikator aplikacji: `627e911e-43cc-61d4-992e-12db9c81b413`</span><span class="sxs-lookup"><span data-stu-id="43b35-192">Application ID: `627e911e-43cc-61d4-992e-12db9c81b413`</span></span>

<span data-ttu-id="43b35-193">*appsettings.json*:</span><span class="sxs-lookup"><span data-stu-id="43b35-193">*appsettings.json*:</span></span>

[!code-json[](key-vault-configuration/sample/appsettings.json)]

<span data-ttu-id="43b35-194">Po uruchomieniu aplikacji, strony sieci Web wyświetlane są załadowane wartości klucza tajnego.</span><span class="sxs-lookup"><span data-stu-id="43b35-194">When you run the app, a webpage shows the loaded secret values.</span></span> <span data-ttu-id="43b35-195">W środowisku programistycznym wartościami wpisów tajnych załadować dane przy użyciu `_dev` sufiks.</span><span class="sxs-lookup"><span data-stu-id="43b35-195">In the Development environment, secret values load with the `_dev` suffix.</span></span> <span data-ttu-id="43b35-196">W środowisku produkcyjnym wartości załadować dane przy użyciu `_prod` sufiks.</span><span class="sxs-lookup"><span data-stu-id="43b35-196">In the Production environment, the values load with the `_prod` suffix.</span></span>

## <a name="use-managed-identities-for-azure-resources"></a><span data-ttu-id="43b35-197">Użyj tożsamości zarządzanych zasobów platformy Azure</span><span class="sxs-lookup"><span data-stu-id="43b35-197">Use Managed identities for Azure resources</span></span>

<span data-ttu-id="43b35-198">**Aplikacja wdrożona na platformie Azure** korzystać z zalet [zarządzanych tożsamości dla zasobów platformy Azure](/azure/active-directory/managed-identities-azure-resources/overview), co pozwala aplikacji na uwierzytelnianie za pomocą usługi Azure Key Vault przy użyciu uwierzytelniania usługi Azure AD bez poświadczeń (identyfikator aplikacji i Klucz tajny Password/Client) przechowywanych w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="43b35-198">**An app deployed to Azure** can take advantage of [Managed identities for Azure resources](/azure/active-directory/managed-identities-azure-resources/overview), which allows the app to authenticate with Azure Key Vault using Azure AD authentication without credentials (Application ID and Password/Client Secret) stored in the app.</span></span>

<span data-ttu-id="43b35-199">Przykładowa aplikacja korzysta z tożsamości zarządzanego dla zasobów platformy Azure po `#define` instrukcji na górze *Program.cs* pliku jest ustawiona na `Managed`.</span><span class="sxs-lookup"><span data-stu-id="43b35-199">The sample app uses Managed identities for Azure resources when the `#define` statement at the top of the *Program.cs* file is set to `Managed`.</span></span>

<span data-ttu-id="43b35-200">Wprowadź nazwę magazynu z aplikacją *appsettings.json* pliku.</span><span class="sxs-lookup"><span data-stu-id="43b35-200">Enter the vault name into the app's *appsettings.json* file.</span></span> <span data-ttu-id="43b35-201">Przykładowa aplikacja nie wymaga Identyfikatora aplikacji i hasło (klucz tajny klienta), po ustawieniu `Managed` wersji, dzięki czemu można zignorować te pozycje konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="43b35-201">The sample app doesn't require an Application ID and Password (Client Secret) when set to the `Managed` version, so you can ignore those configuration entries.</span></span> <span data-ttu-id="43b35-202">Aplikacja jest wdrożona na platformie Azure, a platforma Azure uwierzytelnia aplikacji dostępu do usługi Azure Key Vault tylko przy użyciu nazwy magazynu przechowywanych w *appsettings.json* pliku.</span><span class="sxs-lookup"><span data-stu-id="43b35-202">The app is deployed to Azure, and Azure authenticates the app to access Azure Key Vault only using the vault name stored in the *appsettings.json* file.</span></span>

<span data-ttu-id="43b35-203">Wdróż przykładową aplikację w usłudze Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="43b35-203">Deploy the sample app to Azure App Service.</span></span>

<span data-ttu-id="43b35-204">Wdrożenie aplikacji w usłudze Azure App Service jest automatycznie rejestrowane w usłudze Azure AD podczas tworzenia usługi.</span><span class="sxs-lookup"><span data-stu-id="43b35-204">An app deployed to Azure App Service is automatically registered with Azure AD when the service is created.</span></span> <span data-ttu-id="43b35-205">Uzyskaj identyfikator obiektu z wdrożenia do użycia w następującym poleceniu.</span><span class="sxs-lookup"><span data-stu-id="43b35-205">Obtain the Object ID from the deployment for use in the following command.</span></span> <span data-ttu-id="43b35-206">Identyfikator obiektu są wyświetlane w witrynie Azure portal na **tożsamości** panelu usługi App Service.</span><span class="sxs-lookup"><span data-stu-id="43b35-206">The Object ID is shown in the Azure portal on the **Identity** panel of the App Service.</span></span>

<span data-ttu-id="43b35-207">Przy użyciu wiersza polecenia platformy Azure i identyfikator obiektu aplikacji, zapewnić aplikacji za pomocą `list` i `get` uprawnienia do dostępu do magazynu kluczy:</span><span class="sxs-lookup"><span data-stu-id="43b35-207">Using Azure CLI and the app's Object ID, provide the app with `list` and `get` permissions to access the key vault:</span></span>

```console
az keyvault set-policy --name '{KEY VAULT NAME}' --object-id {OBJECT ID} --secret-permissions get list
```

<span data-ttu-id="43b35-208">**Uruchom ponownie aplikację** przy użyciu wiersza polecenia platformy Azure, programu PowerShell lub witryny Azure portal.</span><span class="sxs-lookup"><span data-stu-id="43b35-208">**Restart the app** using Azure CLI, PowerShell, or the Azure portal.</span></span>

<span data-ttu-id="43b35-209">Przykładowa aplikacja:</span><span class="sxs-lookup"><span data-stu-id="43b35-209">The sample app:</span></span>

* <span data-ttu-id="43b35-210">Tworzy wystąpienie `AzureServiceTokenProvider` klasy bez parametrów połączenia.</span><span class="sxs-lookup"><span data-stu-id="43b35-210">Creates an instance of the `AzureServiceTokenProvider` class without a connection string.</span></span> <span data-ttu-id="43b35-211">Gdy parametry połączenia nie jest podany, dostawca próbuje uzyskać token dostępu z tożsamości zarządzanego dla zasobów platformy Azure.</span><span class="sxs-lookup"><span data-stu-id="43b35-211">When a connection string isn't provided, the provider attempts to obtain an access token from Managed identities for Azure resources.</span></span>
* <span data-ttu-id="43b35-212">Nowy `KeyVaultClient` jest tworzona przy użyciu `AzureServiceTokenProvider` wystąpienie tokenu wywołania zwrotnego.</span><span class="sxs-lookup"><span data-stu-id="43b35-212">A new `KeyVaultClient` is created with the `AzureServiceTokenProvider` instance token callback.</span></span>
* <span data-ttu-id="43b35-213">`KeyVaultClient` Wystąpienie jest używane z domyślną implementację elementu `IKeyVaultSecretManager` , ładuje wszystkie wartości klucza tajnego i zastępuje kresek podwójnej precyzji (`--`) z dwukropkiem (`:`) nazwy kluczy.</span><span class="sxs-lookup"><span data-stu-id="43b35-213">The `KeyVaultClient` instance is used with a default implementation of `IKeyVaultSecretManager` that loads all secret values and replaces double-dashes (`--`) with colons (`:`) in key names.</span></span>

[!code-csharp[](key-vault-configuration/sample/Program.cs?name=snippet2&highlight=13-21)]

<span data-ttu-id="43b35-214">Po uruchomieniu aplikacji, strony sieci Web wyświetlane są załadowane wartości klucza tajnego.</span><span class="sxs-lookup"><span data-stu-id="43b35-214">When you run the app, a webpage shows the loaded secret values.</span></span> <span data-ttu-id="43b35-215">W środowisku programistycznym wartościami wpisów tajnych mają `_dev` sufiksu domeny, ponieważ są one udostępniane przez wpisami tajnymi użytkowników.</span><span class="sxs-lookup"><span data-stu-id="43b35-215">In the Development environment, secret values have the `_dev` suffix because they're provided by User Secrets.</span></span> <span data-ttu-id="43b35-216">W środowisku produkcyjnym wartości załadować dane przy użyciu `_prod` sufiksu domeny, ponieważ są one udostępniane przez usługę Azure Key Vault.</span><span class="sxs-lookup"><span data-stu-id="43b35-216">In the Production environment, the values load with the `_prod` suffix because they're provided by Azure Key Vault.</span></span>

<span data-ttu-id="43b35-217">Jeśli zostanie wyświetlony `Access denied` błąd, upewnij się, że aplikacja jest zarejestrowane w usłudze Azure AD i dostępu do magazynu kluczy.</span><span class="sxs-lookup"><span data-stu-id="43b35-217">If you receive an `Access denied` error, confirm that the app is registered with Azure AD and provided access to the key vault.</span></span> <span data-ttu-id="43b35-218">Upewnij się, że został uruchomiony ponownie usługi na platformie Azure.</span><span class="sxs-lookup"><span data-stu-id="43b35-218">Confirm that you've restarted the service in Azure.</span></span>

## <a name="use-a-key-name-prefix"></a><span data-ttu-id="43b35-219">Użyj prefiksu nazwy klucza</span><span class="sxs-lookup"><span data-stu-id="43b35-219">Use a key name prefix</span></span>

<span data-ttu-id="43b35-220">`AddAzureKeyVault` udostępnia przeciążenie, które akceptuje implementację `IKeyVaultSecretManager`, co pozwala na kontrolowanie sposobu klucza magazynu kluczy tajnych są konwertowane na klucze konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="43b35-220">`AddAzureKeyVault` provides an overload that accepts an implementation of `IKeyVaultSecretManager`, which allows you to control how key vault secrets are converted into configuration keys.</span></span> <span data-ttu-id="43b35-221">Na przykład można zaimplementować interfejs służący do ładowania wartościami wpisów tajnych na podstawie wartości prefiks, który podajesz podczas uruchamiania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="43b35-221">For example, you can implement the interface to load secret values based on a prefix value you provide at app startup.</span></span> <span data-ttu-id="43b35-222">Dzięki temu można na przykład, aby ładować wpisy tajne w oparciu o wersję aplikacji.</span><span class="sxs-lookup"><span data-stu-id="43b35-222">This allows you, for example, to load secrets based on the version of the app.</span></span>

> [!WARNING]
> <span data-ttu-id="43b35-223">Nie używaj prefiksy na wpisy tajne usługi key vault, umieść wpisów tajnych dla wielu aplikacji w tym samym magazynie kluczy lub umieść środowiska wpisy tajne (na przykład *rozwoju* a *produkcji* wpisy tajne) w tej samej Magazyn.</span><span class="sxs-lookup"><span data-stu-id="43b35-223">Don't use prefixes on key vault secrets to place secrets for multiple apps into the same key vault or to place environmental secrets (for example, *development* versus *production* secrets) into the same vault.</span></span> <span data-ttu-id="43b35-224">Zalecane jest różnych aplikacji i środowisk tworzenia/produkcyjnych użycie oddzielnych magazynów kluczy do izolowania aplikacji środowiska na najwyższy poziom zabezpieczeń.</span><span class="sxs-lookup"><span data-stu-id="43b35-224">We recommend that different apps and development/production environments use separate key vaults to isolate app environments for the highest level of security.</span></span>

<span data-ttu-id="43b35-225">W poniższym przykładzie wpisu tajnego jest określana w klucza magazynu (i przy użyciu narzędzia menedżera klucz tajny dla środowiska programistycznego) dla `5000-AppSecret` (kropki nie są dozwolone w nazwach wpisu tajnego usługi key vault).</span><span class="sxs-lookup"><span data-stu-id="43b35-225">In the following example, a secret is established in the key vault (and using the Secret Manager tool for the Development environment) for `5000-AppSecret` (periods aren't allowed in key vault secret names).</span></span> <span data-ttu-id="43b35-226">Ten wpis tajny reprezentuje klucz tajny aplikacji dla 5.0.0.0 wersję aplikacji.</span><span class="sxs-lookup"><span data-stu-id="43b35-226">This secret represents an app secret for version 5.0.0.0 of the app.</span></span> <span data-ttu-id="43b35-227">Dla innej wersji aplikacji, 5.1.0.0, klucz tajny jest dodawany do klucza magazynu (i przy użyciu narzędzia menedżera klucz tajny) dla `5100-AppSecret`.</span><span class="sxs-lookup"><span data-stu-id="43b35-227">For another version of the app, 5.1.0.0, a secret is added to the key vault (and using the Secret Manager tool) for `5100-AppSecret`.</span></span> <span data-ttu-id="43b35-228">Każda wersja aplikacji ładuje jego wersjonowany wartość wpisu tajnego do jego konfigurację jako `AppSecret`, oddzielającego off wersji podczas ładowania klucza tajnego.</span><span class="sxs-lookup"><span data-stu-id="43b35-228">Each app version loads its versioned secret value into its configuration as `AppSecret`, stripping off the version as it loads the secret.</span></span>

<span data-ttu-id="43b35-229">`AddAzureKeyVault` jest wywoływana z niestandardowego `IKeyVaultSecretManager`:</span><span class="sxs-lookup"><span data-stu-id="43b35-229">`AddAzureKeyVault` is called with a custom `IKeyVaultSecretManager`:</span></span>

[!code-csharp[](key-vault-configuration/sample_snapshot/Program.cs?name=snippet1&highlight=22)]

<span data-ttu-id="43b35-230">Wartości dla nazwy usługi key vault, identyfikator aplikacji i hasło (klucz tajny klienta) są dostarczane przez *appsettings.json* pliku:</span><span class="sxs-lookup"><span data-stu-id="43b35-230">The values for key vault name, Application ID, and Password (Client Secret) are provided by the *appsettings.json* file:</span></span>

[!code-json[](key-vault-configuration/sample/appsettings.json)]

<span data-ttu-id="43b35-231">Przykładowe wartości:</span><span class="sxs-lookup"><span data-stu-id="43b35-231">Example values:</span></span>

* <span data-ttu-id="43b35-232">Nazwa magazynu kluczy: `contosovault`</span><span class="sxs-lookup"><span data-stu-id="43b35-232">Key vault name: `contosovault`</span></span>
* <span data-ttu-id="43b35-233">Identyfikator aplikacji: `627e911e-43cc-61d4-992e-12db9c81b413`</span><span class="sxs-lookup"><span data-stu-id="43b35-233">Application ID: `627e911e-43cc-61d4-992e-12db9c81b413`</span></span>
* <span data-ttu-id="43b35-234">Hasło: `g58K3dtg59o1Pa+e59v2Tx829w6VxTB2yv9sv/101di=`</span><span class="sxs-lookup"><span data-stu-id="43b35-234">Password: `g58K3dtg59o1Pa+e59v2Tx829w6VxTB2yv9sv/101di=`</span></span>

<span data-ttu-id="43b35-235">`IKeyVaultSecretManager` Implementacji reaguje na prefiksy wersji wpisów tajnych można załadować odpowiedniego wpisu tajnego do konfiguracji:</span><span class="sxs-lookup"><span data-stu-id="43b35-235">The `IKeyVaultSecretManager` implementation reacts to the version prefixes of secrets to load the proper secret into configuration:</span></span>

[!code-csharp[](key-vault-configuration/sample_snapshot/Startup.cs?name=snippet1)]

<span data-ttu-id="43b35-236">`Load` Metoda jest wywoływana przez algorytm dostawcy, który iteruje po magazyn kluczy tajnych wyszukać te, które mają prefiks wersji.</span><span class="sxs-lookup"><span data-stu-id="43b35-236">The `Load` method is called by a provider algorithm that iterates through the vault secrets to find the ones that have the version prefix.</span></span> <span data-ttu-id="43b35-237">Gdy prefiks wersji znajduje się za pomocą `Load`, algorytm używa `GetKey` metodę, aby zwrócić nazwę konfiguracji nazwa wpisu tajnego.</span><span class="sxs-lookup"><span data-stu-id="43b35-237">When a version prefix is found with `Load`, the algorithm uses the `GetKey` method to return the configuration name of the secret name.</span></span> <span data-ttu-id="43b35-238">On paski Wyłącz prefiks wersji od nazwy klucza tajnego i zwraca pozostałe Nazwa wpisu tajnego do załadowania do konfiguracji aplikacji pary nazwa wartość.</span><span class="sxs-lookup"><span data-stu-id="43b35-238">It strips off the version prefix from the secret's name and returns the rest of the secret name for loading into the app's configuration name-value pairs.</span></span>

<span data-ttu-id="43b35-239">Gdy ta metoda jest zaimplementowana:</span><span class="sxs-lookup"><span data-stu-id="43b35-239">When this approach is implemented:</span></span>

1. <span data-ttu-id="43b35-240">Wersja aplikacji określony w pliku projektu aplikacji.</span><span class="sxs-lookup"><span data-stu-id="43b35-240">The app's version specified in the app's project file.</span></span> <span data-ttu-id="43b35-241">W poniższym przykładzie ustawiono wersję aplikacji `5.0.0.0`:</span><span class="sxs-lookup"><span data-stu-id="43b35-241">In the following example, the app's version is set to `5.0.0.0`:</span></span>

   ```xml
   <PropertyGroup>
     <Version>5.0.0.0</Version>
   </PropertyGroup>
   ```

1. <span data-ttu-id="43b35-242">Upewnij się, że `<UserSecretsId>` właściwość znajduje się w pliku projektu aplikacji, gdzie `{GUID}` jest identyfikatorem GUID, dostarczone przez użytkownika:</span><span class="sxs-lookup"><span data-stu-id="43b35-242">Confirm that a `<UserSecretsId>` property is present in the app's project file, where `{GUID}` is a user-supplied GUID:</span></span>

   ```xml
   <PropertyGroup>
     <UserSecretsId>{GUID}</UserSecretsId>
   </PropertyGroup>
   ```

   <span data-ttu-id="43b35-243">Zapisz następujące wpisy tajne lokalnie za pomocą [narzędzie Menedżer klucz tajny](xref:security/app-secrets):</span><span class="sxs-lookup"><span data-stu-id="43b35-243">Save the following secrets locally with the [Secret Manager tool](xref:security/app-secrets):</span></span>

   ```console
   dotnet user-secrets set "5000-AppSecret" "5.0.0.0_secret_value_dev"
   dotnet user-secrets set "5100-AppSecret" "5.1.0.0_secret_value_dev"
   ```

1. <span data-ttu-id="43b35-244">Klucze tajne są zapisywane w usłudze Azure Key Vault przy użyciu następujących poleceń interfejsu wiersza polecenia platformy Azure:</span><span class="sxs-lookup"><span data-stu-id="43b35-244">Secrets are saved in Azure Key Vault using the following Azure CLI commands:</span></span>

   ```console
   az keyvault secret set --vault-name "{KEY VAULT NAME}" --name "5000-AppSecret" --value "5.0.0.0_secret_value_prod"
   az keyvault secret set --vault-name "{KEY VAULT NAME}" --name "5100-AppSecret" --value "5.1.0.0_secret_value_prod"
   ```

1. <span data-ttu-id="43b35-245">Gdy aplikacja jest uruchamiana, wpisy tajne usługi key vault są ładowane.</span><span class="sxs-lookup"><span data-stu-id="43b35-245">When the app is run, the key vault secrets are loaded.</span></span> <span data-ttu-id="43b35-246">Klucz tajny ciągu dla `5000-AppSecret` pasuje do wersji aplikacji określonej w pliku projektu aplikacji (`5.0.0.0`).</span><span class="sxs-lookup"><span data-stu-id="43b35-246">The string secret for `5000-AppSecret` is matched to the app's version specified in the app's project file (`5.0.0.0`).</span></span>

1. <span data-ttu-id="43b35-247">Wersja, `5000` (przy użyciu dash), jest usuwany z nazwą klucza.</span><span class="sxs-lookup"><span data-stu-id="43b35-247">The version, `5000` (with the dash), is stripped from the key name.</span></span> <span data-ttu-id="43b35-248">W całej aplikacji odczytywanie konfiguracji przy użyciu klucza `AppSecret` ładuje wartość wpisu tajnego.</span><span class="sxs-lookup"><span data-stu-id="43b35-248">Throughout the app, reading configuration with the key `AppSecret` loads the secret value.</span></span>

1. <span data-ttu-id="43b35-249">Jeśli wersja aplikacji została zmieniona w pliku projektu do `5.1.0.0` i aplikacja jest uruchamiana ponownie, jest zwracana wartość wpisu tajnego `5.1.0.0_secret_value_dev` w środowisku deweloperskim i `5.1.0.0_secret_value_prod` w środowisku produkcyjnym.</span><span class="sxs-lookup"><span data-stu-id="43b35-249">If the app's version is changed in the project file to `5.1.0.0` and the app is run again, the secret value returned is `5.1.0.0_secret_value_dev` in the Development environment and `5.1.0.0_secret_value_prod` in Production.</span></span>

> [!NOTE]
> <span data-ttu-id="43b35-250">Możesz również podać własne `KeyVaultClient` implementacji `AddAzureKeyVault`.</span><span class="sxs-lookup"><span data-stu-id="43b35-250">You can also provide your own `KeyVaultClient` implementation to `AddAzureKeyVault`.</span></span> <span data-ttu-id="43b35-251">Klient niestandardowy umożliwia udostępnianie jedno wystąpienie klienta w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="43b35-251">A custom client permits sharing a single instance of the client across the app.</span></span>

## <a name="allow-the-os-to-manage-the-x509-certificate"></a><span data-ttu-id="43b35-252">Zezwalaj na systemu operacyjnego w celu zarządzania certyfikatów X.509</span><span class="sxs-lookup"><span data-stu-id="43b35-252">Allow the OS to manage the X.509 certificate</span></span>

<span data-ttu-id="43b35-253">Certyfikat X.509 mogą być zarządzane przez system operacyjny.</span><span class="sxs-lookup"><span data-stu-id="43b35-253">The X.509 certificate can be managed by the OS.</span></span> <span data-ttu-id="43b35-254">W poniższym przykładzie użyto `AddAzureKeyVault` przeciążenie, które akceptuje `X509Certificate2` z magazynu certyfikatów użytkownika bieżącego komputera i odcisk palca certyfikatu pochodzącego z konfiguracji:</span><span class="sxs-lookup"><span data-stu-id="43b35-254">The following example uses the `AddAzureKeyVault` overload that accepts an `X509Certificate2` from the machine's current user certificate store and a certificate thumbprint supplied by configuration:</span></span>

```csharp
// using System.Linq;
// using System.Security.Cryptography.X509Certificates;
// using Microsoft.Extensions.Configuration;

WebHost.CreateDefaultBuilder(args)
    .ConfigureAppConfiguration((context, config) =>
    {
        if (context.HostingEnvironment.IsProduction())
        {
            var builtConfig = config.Build();

            using (var store = new X509Store(StoreName.My, 
                StoreLocation.CurrentUser))
            {
                store.Open(OpenFlags.ReadOnly);
                var certs = store.Certificates
                    .Find(X509FindType.FindByThumbprint, 
                        builtConfig["CertificateThumbprint"], false);

                config.AddAzureKeyVault(
                    builtConfig["KeyVaultName"], 
                    builtConfig["AzureADApplicationId"], 
                    certs.OfType<X509Certificate2>().Single());

                store.Close();
            }
        }
    })
    .UseStartup<Startup>();
```

<span data-ttu-id="43b35-255">Aby uzyskać więcej informacji, zobacz [uwierzytelnianie przy użyciu certyfikatu zamiast wpisu tajnego klienta](/azure/key-vault/key-vault-use-from-web-application#authenticate-with-a-certificate-instead-of-a-client-secret).</span><span class="sxs-lookup"><span data-stu-id="43b35-255">For more information, see [Authenticate with a Certificate instead of a Client Secret](/azure/key-vault/key-vault-use-from-web-application#authenticate-with-a-certificate-instead-of-a-client-secret).</span></span>

## <a name="bind-an-array-to-a-class"></a><span data-ttu-id="43b35-256">Powiąż tablicę do klasy</span><span class="sxs-lookup"><span data-stu-id="43b35-256">Bind an array to a class</span></span>

<span data-ttu-id="43b35-257">Dostawca jest zdolny do odczytywania wartości konfiguracji do tablicy, do powiązania z macierzą POCO.</span><span class="sxs-lookup"><span data-stu-id="43b35-257">The provider is capable of reading configuration values into an array for binding to a POCO array.</span></span>

<span data-ttu-id="43b35-258">Podczas odczytywania ze źródła konfiguracji, która umożliwia klucze, aby zawierać dwukropek (`:`) separatory, liczbowych segment klucza jest używane do odróżniania klucze, które tworzą tablicę (`:0:`, `:1:`,...</span><span class="sxs-lookup"><span data-stu-id="43b35-258">When reading from a configuration source that allows keys to contain colon (`:`) separators, a numeric key segment is used to distinguish the keys that make up an array (`:0:`, `:1:`, …</span></span> <span data-ttu-id="43b35-259">`:{n}:`).</span><span class="sxs-lookup"><span data-stu-id="43b35-259">`:{n}:`).</span></span> <span data-ttu-id="43b35-260">Aby uzyskać więcej informacji, zobacz [konfiguracji: Powiąż tablicę do klasy](xref:fundamentals/configuration/index#bind-an-array-to-a-class).</span><span class="sxs-lookup"><span data-stu-id="43b35-260">For more information, see [Configuration: Bind an array to a class](xref:fundamentals/configuration/index#bind-an-array-to-a-class).</span></span>

<span data-ttu-id="43b35-261">Usługa Azure Key Vault klucze nie można użyć dwukropek jako separatora.</span><span class="sxs-lookup"><span data-stu-id="43b35-261">Azure Key Vault keys can't use a colon as a separator.</span></span> <span data-ttu-id="43b35-262">Podejście opisane w tym temacie używa podwójnego kreski (`--`) jako separatora hierarchiczne wartości (sekcji).</span><span class="sxs-lookup"><span data-stu-id="43b35-262">The approach described in this topic uses double dashes (`--`) as a separator for hierarchical values (sections).</span></span> <span data-ttu-id="43b35-263">Tablicy klucze są przechowywane w usłudze Azure Key Vault, przy użyciu podwójnych kresek i numeryczne kluczowe segmenty (`--0--`, `--1--`, &hellip; `--{n}--`).</span><span class="sxs-lookup"><span data-stu-id="43b35-263">Array keys are stored in Azure Key Vault with double dashes and numeric key segments (`--0--`, `--1--`, &hellip; `--{n}--`).</span></span>

<span data-ttu-id="43b35-264">Sprawdź następujące [Serilog](https://serilog.net/) rejestrowania konfigurację dostawcy udostępniane przez plik w formacie JSON.</span><span class="sxs-lookup"><span data-stu-id="43b35-264">Examine the following [Serilog](https://serilog.net/) logging provider configuration provided by a JSON file.</span></span> <span data-ttu-id="43b35-265">Istnieją dwa obiektu literały zdefiniowane w `WriteTo` tablica, która odzwierciedla dwóch Serilog *wychwytywanie*, opisywać miejsca docelowe danych wyjściowych rejestrowania:</span><span class="sxs-lookup"><span data-stu-id="43b35-265">There are two object literals defined in the `WriteTo` array that reflect two Serilog *sinks*, which describe destinations for logging output:</span></span>

```json
"Serilog": {
  "WriteTo": [
    {
      "Name": "AzureTableStorage",
      "Args": {
        "storageTableName": "logs",
        "connectionString": "DefaultEnd...ountKey=Eby8...GMGw=="
      }
    },
    {
      "Name": "AzureDocumentDB",
      "Args": {
        "endpointUrl": "https://contoso.documents.azure.com:443",
        "authorizationKey": "Eby8...GMGw=="
      }
    }
  ]
}
```

<span data-ttu-id="43b35-266">Konfiguracji przedstawionej w poprzednim pliku JSON jest przechowywany w usłudze Azure Key Vault przy użyciu Podwójna kreska (`--`) notacją i numeryczne segmenty:</span><span class="sxs-lookup"><span data-stu-id="43b35-266">The configuration shown in the preceding JSON file is stored in Azure Key Vault using double dash (`--`) notation and numeric segments:</span></span>

| <span data-ttu-id="43b35-267">Key</span><span class="sxs-lookup"><span data-stu-id="43b35-267">Key</span></span> | <span data-ttu-id="43b35-268">Wartość</span><span class="sxs-lookup"><span data-stu-id="43b35-268">Value</span></span> |
| --- | ----- |
| `Serilog--WriteTo--0--Name` | `AzureTableStorage` |
| `Serilog--WriteTo--0--Args--storageTableName` | `logs` |
| `Serilog--WriteTo--0--Args--connectionString` | `DefaultEnd...ountKey=Eby8...GMGw==` |
| `Serilog--WriteTo--1--Name` | `AzureDocumentDB` |
| `Serilog--WriteTo--1--Args--endpointUrl` | `https://contoso.documents.azure.com:443` |
| `Serilog--WriteTo--1--Args--authorizationKey` | `Eby8...GMGw==` |

## <a name="reload-secrets"></a><span data-ttu-id="43b35-269">Załaduj ponownie wpisy tajne</span><span class="sxs-lookup"><span data-stu-id="43b35-269">Reload secrets</span></span>

<span data-ttu-id="43b35-270">Klucze tajne są buforowane do momentu `IConfigurationRoot.Reload()` jest wywoływana.</span><span class="sxs-lookup"><span data-stu-id="43b35-270">Secrets are cached until `IConfigurationRoot.Reload()` is called.</span></span> <span data-ttu-id="43b35-271">Wygasłe, wyłączona, i zaktualizowanych wpisów tajnych w magazynie kluczy nie są przestrzegane przez aplikację do momentu `Reload` jest wykonywany.</span><span class="sxs-lookup"><span data-stu-id="43b35-271">Expired, disabled, and updated secrets in the key vault are not respected by the app until `Reload` is executed.</span></span>

```csharp
Configuration.Reload();
```

## <a name="disabled-and-expired-secrets"></a><span data-ttu-id="43b35-272">Wyłączone lub wygasłe klucze tajne</span><span class="sxs-lookup"><span data-stu-id="43b35-272">Disabled and expired secrets</span></span>

<span data-ttu-id="43b35-273">Wyłączone lub wygasłe klucze tajne throw `KeyVaultClientException`.</span><span class="sxs-lookup"><span data-stu-id="43b35-273">Disabled and expired secrets throw a `KeyVaultClientException`.</span></span> <span data-ttu-id="43b35-274">Aby zapobiec sytuacji, w której aplikacja zostanie zgłoszony, zastąpić aplikację, lub zaktualizuj wyłączone/wygasły klucz tajny.</span><span class="sxs-lookup"><span data-stu-id="43b35-274">To prevent your app from throwing, replace your app or update the disabled/expired secret.</span></span>

## <a name="troubleshoot"></a><span data-ttu-id="43b35-275">Rozwiązywanie problemów</span><span class="sxs-lookup"><span data-stu-id="43b35-275">Troubleshoot</span></span>

<span data-ttu-id="43b35-276">Gdy aplikacja zakończy się niepowodzeniem, można załadować konfiguracji przy użyciu dostawcy, komunikat o błędzie zostanie zapisany [infrastruktury platformy ASP.NET Core rejestrowania](xref:fundamentals/logging/index).</span><span class="sxs-lookup"><span data-stu-id="43b35-276">When the app fails to load configuration using the provider, an error message is written to the [ASP.NET Core Logging infrastructure](xref:fundamentals/logging/index).</span></span> <span data-ttu-id="43b35-277">Konfiguracja ładowanie uniemożliwia następujące warunki:</span><span class="sxs-lookup"><span data-stu-id="43b35-277">The following conditions will prevent configuration from loading:</span></span>

* <span data-ttu-id="43b35-278">Aplikacja nie jest poprawnie skonfigurowane w usłudze Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="43b35-278">The app isn't configured correctly in Azure Active Directory.</span></span>
* <span data-ttu-id="43b35-279">Magazyn kluczy nie istnieje w usłudze Azure Key Vault.</span><span class="sxs-lookup"><span data-stu-id="43b35-279">The key vault doesn't exist in Azure Key Vault.</span></span>
* <span data-ttu-id="43b35-280">Aplikacji nie jest upoważniony do dostępu do magazynu kluczy.</span><span class="sxs-lookup"><span data-stu-id="43b35-280">The app isn't authorized to access the key vault.</span></span>
* <span data-ttu-id="43b35-281">Zasady dostępu nie zawiera `Get` i `List` uprawnienia.</span><span class="sxs-lookup"><span data-stu-id="43b35-281">The access policy doesn't include `Get` and `List` permissions.</span></span>
* <span data-ttu-id="43b35-282">W usłudze key vault dane konfiguracji (pary nazwa wartość) niepoprawnie nosi nazwę, Brak, wyłączone lub wygasła.</span><span class="sxs-lookup"><span data-stu-id="43b35-282">In the key vault, the configuration data (name-value pair) is incorrectly named, missing, disabled, or expired.</span></span>
* <span data-ttu-id="43b35-283">Aplikacja ma nazwę niewłaściwego magazynu kluczy (`KeyVaultName`), identyfikator aplikacji w usłudze Azure AD (`AzureADApplicationId`), lub haseł usługi Azure AD (klucz tajny klienta) (`AzureADPassword`).</span><span class="sxs-lookup"><span data-stu-id="43b35-283">The app has the wrong key vault name (`KeyVaultName`), Azure AD Application Id (`AzureADApplicationId`), or Azure AD Password (Client Secret) (`AzureADPassword`).</span></span>
* <span data-ttu-id="43b35-284">Hasła usługi Azure AD (klucz tajny klienta) (`AzureADPassword`) wygasł.</span><span class="sxs-lookup"><span data-stu-id="43b35-284">The Azure AD Password (Client Secret) (`AzureADPassword`) is expired.</span></span>
* <span data-ttu-id="43b35-285">Klucz konfiguracji (nazwa) jest niepoprawny w aplikacji dla wartości, które próbujesz załadować.</span><span class="sxs-lookup"><span data-stu-id="43b35-285">The configuration key (name) is incorrect in the app for the value you're trying to load.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="43b35-286">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="43b35-286">Additional resources</span></span>

* <xref:fundamentals/configuration/index>
* [<span data-ttu-id="43b35-287">Microsoft Azure: Magazyn kluczy</span><span class="sxs-lookup"><span data-stu-id="43b35-287">Microsoft Azure: Key Vault</span></span>](https://azure.microsoft.com/services/key-vault/)
* [<span data-ttu-id="43b35-288">Microsoft Azure: Dokumentacja usługi Key Vault</span><span class="sxs-lookup"><span data-stu-id="43b35-288">Microsoft Azure: Key Vault Documentation</span></span>](/azure/key-vault/)
* [<span data-ttu-id="43b35-289">Jak Generowanie i przenoszenie chronionego przez moduł HSM kluczy dla usługi Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="43b35-289">How to generate and transfer HSM-protected keys for Azure Key Vault</span></span>](/azure/key-vault/key-vault-hsm-protected-keys)
* [<span data-ttu-id="43b35-290">Klasa KeyVaultClient</span><span class="sxs-lookup"><span data-stu-id="43b35-290">KeyVaultClient Class</span></span>](/dotnet/api/microsoft.azure.keyvault.keyvaultclient)
* [<span data-ttu-id="43b35-291">Szybki start: Ustawianie i pobieranie wpisu tajnego z usługi Azure Key Vault za pomocą aplikacji internetowej platformy .NET</span><span class="sxs-lookup"><span data-stu-id="43b35-291">Quickstart: Set and retrieve a secret from Azure Key Vault by using a .NET web app</span></span>](/azure/key-vault/quick-create-net)
* [<span data-ttu-id="43b35-292">Samouczek: Jak używać usługi Azure Key Vault za pomocą maszyny wirtualnej Windows Azure na platformie .NET</span><span class="sxs-lookup"><span data-stu-id="43b35-292">Tutorial: How to use Azure Key Vault with Azure Windows Virtual Machine in .NET</span></span>](/azure/key-vault/tutorial-net-windows-virtual-machine)
