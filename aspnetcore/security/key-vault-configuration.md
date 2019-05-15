---
title: Dostawca konfiguracji usługi Azure Key Vault w programie ASP.NET Core
author: guardrex
description: Dowiedz się, jak skonfigurować aplikację tak, za pomocą pary nazwa wartość ładowane w czasie wykonywania za pomocą dostawcy konfiguracji magazynu kluczy Azure.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 05/13/2019
uid: security/key-vault-configuration
ms.openlocfilehash: 78c63cf135ca92f0b5f6c6828b2ae34a44a7b36c
ms.sourcegitcommit: 3ee6ee0051c3d2c8d47a58cb17eef1a84a4c46a0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/14/2019
ms.locfileid: "65621016"
---
# <a name="azure-key-vault-configuration-provider-in-aspnet-core"></a><span data-ttu-id="00ccc-103">Dostawca konfiguracji usługi Azure Key Vault w programie ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="00ccc-103">Azure Key Vault Configuration Provider in ASP.NET Core</span></span>

<span data-ttu-id="00ccc-104">Przez [Luke Latham](https://github.com/guardrex) i [Andrew Stanton pielęgniarki](https://github.com/anurse)</span><span class="sxs-lookup"><span data-stu-id="00ccc-104">By [Luke Latham](https://github.com/guardrex) and [Andrew Stanton-Nurse](https://github.com/anurse)</span></span>

<span data-ttu-id="00ccc-105">W tym dokumencie wyjaśniono, jak używać [Microsoft Azure Key Vault](https://azure.microsoft.com/services/key-vault/) dostawcę konfiguracji, które można załadować wartości konfiguracji aplikacji z wpisy tajne w usłudze Azure Key Vault.</span><span class="sxs-lookup"><span data-stu-id="00ccc-105">This document explains how to use the [Microsoft Azure Key Vault](https://azure.microsoft.com/services/key-vault/) Configuration Provider to load app configuration values from Azure Key Vault secrets.</span></span> <span data-ttu-id="00ccc-106">Usługa Azure Key Vault to usługa oparta na chmurze, które pomaga w ochronę kluczy kryptograficznych i wpisów tajnych używanych przez aplikacje i usługi.</span><span class="sxs-lookup"><span data-stu-id="00ccc-106">Azure Key Vault is a cloud-based service that assists in safeguarding cryptographic keys and secrets used by apps and services.</span></span> <span data-ttu-id="00ccc-107">Typowe scenariusze dotyczące korzystania z usługi Azure Key Vault z aplikacji platformy ASP.NET Core obejmują:</span><span class="sxs-lookup"><span data-stu-id="00ccc-107">Common scenarios for using Azure Key Vault with ASP.NET Core apps include:</span></span>

* <span data-ttu-id="00ccc-108">Kontrolowanie dostępu do danych poufnych konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="00ccc-108">Controlling access to sensitive configuration data.</span></span>
* <span data-ttu-id="00ccc-109">Spełniające wymagania FIPS 140-2 Level 2 zweryfikować sprzętowych modułów zabezpieczeń (HSM), podczas zapisywania danych konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="00ccc-109">Meeting the requirement for FIPS 140-2 Level 2 validated Hardware Security Modules (HSM's) when storing configuration data.</span></span>

<span data-ttu-id="00ccc-110">Ten scenariusz jest dostępna dla aplikacji przeznaczonych dla platformy ASP.NET Core 2.1 lub nowszej.</span><span class="sxs-lookup"><span data-stu-id="00ccc-110">This scenario is available for apps that target ASP.NET Core 2.1 or later.</span></span>

<span data-ttu-id="00ccc-111">[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/key-vault-configuration/sample) ([sposobu pobierania](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="00ccc-111">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/key-vault-configuration/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="packages"></a><span data-ttu-id="00ccc-112">Pakiety</span><span class="sxs-lookup"><span data-stu-id="00ccc-112">Packages</span></span>

<span data-ttu-id="00ccc-113">Aby używać dostawcy konfiguracji magazynu kluczy Azure, Dodaj odwołanie do pakietu [Microsoft.Extensions.Configuration.AzureKeyVault](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.AzureKeyVault/) pakietu.</span><span class="sxs-lookup"><span data-stu-id="00ccc-113">To use the Azure Key Vault Configuration Provider, add a package reference to the [Microsoft.Extensions.Configuration.AzureKeyVault](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.AzureKeyVault/) package.</span></span>

<span data-ttu-id="00ccc-114">Przyjęcie [zarządzanych tożsamości dla zasobów platformy Azure](/azure/active-directory/managed-identities-azure-resources/overview) scenariusza, Dodaj odwołanie do pakietu [Microsoft.Azure.Services.AppAuthentication](https://www.nuget.org/packages/Microsoft.Azure.Services.AppAuthentication/) pakietu.</span><span class="sxs-lookup"><span data-stu-id="00ccc-114">To adopt the [Managed identities for Azure resources](/azure/active-directory/managed-identities-azure-resources/overview) scenario, add a package reference to the [Microsoft.Azure.Services.AppAuthentication](https://www.nuget.org/packages/Microsoft.Azure.Services.AppAuthentication/) package.</span></span>

> [!NOTE]
> <span data-ttu-id="00ccc-115">W czasie pisania najnowszą stabilną wersję `Microsoft.Azure.Services.AppAuthentication`, wersja `1.0.3`, zapewnia obsługę [przypisany systemowo zarządzanych tożsamości](/azure/active-directory/managed-identities-azure-resources/overview#how-does-the-managed-identities-for-azure-resources-worka-namehow-does-it-worka).</span><span class="sxs-lookup"><span data-stu-id="00ccc-115">At the time of writing, the latest stable release of `Microsoft.Azure.Services.AppAuthentication`, version `1.0.3`, provides support for [system-assigned managed identities](/azure/active-directory/managed-identities-azure-resources/overview#how-does-the-managed-identities-for-azure-resources-worka-namehow-does-it-worka).</span></span> <span data-ttu-id="00ccc-116">Obsługa *przypisanych do użytkowników zarządzanych tożsamości* jest dostępna w `1.2.0-preview2` pakietu.</span><span class="sxs-lookup"><span data-stu-id="00ccc-116">Support for *user-assigned managed identities* is available in the `1.2.0-preview2` package.</span></span> <span data-ttu-id="00ccc-117">W tym temacie przedstawiono użycie tożsamości zarządzanych przez system, a podana Przykładowa aplikacja korzysta z wersji `1.0.3` z `Microsoft.Azure.Services.AppAuthentication` pakietu.</span><span class="sxs-lookup"><span data-stu-id="00ccc-117">This topic demonstrates the use of system-managed identities, and the provided sample app uses version `1.0.3` of the `Microsoft.Azure.Services.AppAuthentication` package.</span></span>

## <a name="sample-app"></a><span data-ttu-id="00ccc-118">Przykładowa aplikacja</span><span class="sxs-lookup"><span data-stu-id="00ccc-118">Sample app</span></span>

<span data-ttu-id="00ccc-119">Przykładowa aplikacja jest uruchamiana w jednym z dwóch trybów ustalany na podstawie `#define` instrukcji na górze *Program.cs* pliku:</span><span class="sxs-lookup"><span data-stu-id="00ccc-119">The sample app runs in either of two modes determined by the `#define` statement at the top of the *Program.cs* file:</span></span>

* <span data-ttu-id="00ccc-120">`Certificate` &ndash; Zademonstrowano użycie certyfikat X.509 i identyfikator klienta systemu Azure klucza magazynu do dostępu do kluczy tajnych przechowywanych w usłudze Azure Key Vault.</span><span class="sxs-lookup"><span data-stu-id="00ccc-120">`Certificate` &ndash; Demonstrates the use of an Azure Key Vault Client ID and X.509 certificate to access secrets stored in Azure Key Vault.</span></span> <span data-ttu-id="00ccc-121">Ta wersja przykładu można uruchomić z dowolnego miejsca i wdrażać w usłudze Azure App Service lub dowolnego hosta, które umożliwia obsługę aplikacji ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="00ccc-121">This version of the sample can be run from any location, deployed to Azure App Service or any host capable of serving an ASP.NET Core app.</span></span>
* <span data-ttu-id="00ccc-122">`Managed` &ndash; Pokazuje sposób użycia [zarządzanych tożsamości dla zasobów platformy Azure](/azure/active-directory/managed-identities-azure-resources/overview) można uwierzytelnić aplikację usługi Azure Key Vault przy użyciu uwierzytelniania usługi Azure AD bez poświadczeń przechowywanych w kodzie aplikacji lub konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="00ccc-122">`Managed` &ndash; Demonstrates how to use [Managed identities for Azure resources](/azure/active-directory/managed-identities-azure-resources/overview) to authenticate the app to Azure Key Vault with Azure AD authentication without credentials stored in the app's code or configuration.</span></span> <span data-ttu-id="00ccc-123">Korzystając z zarządzanych tożsamości do uwierzytelniania, identyfikator aplikacji w usłudze Azure AD i hasło (klucz tajny klienta) nie są wymagane.</span><span class="sxs-lookup"><span data-stu-id="00ccc-123">When using managed identities to authenticate, an Azure AD Application ID and Password (Client Secret) aren't required.</span></span> <span data-ttu-id="00ccc-124">`Managed` Wersję przykładu, należy wdrożyć na platformie Azure.</span><span class="sxs-lookup"><span data-stu-id="00ccc-124">The `Managed` version of the sample must be deployed to Azure.</span></span> <span data-ttu-id="00ccc-125">Postępuj zgodnie ze wskazówkami w [używania tożsamości zarządzanych zasobów platformy Azure](#use-managed-identities-for-azure-resources) sekcji.</span><span class="sxs-lookup"><span data-stu-id="00ccc-125">Follow the guidance in the [Use the Managed identities for Azure resources](#use-managed-identities-for-azure-resources) section.</span></span>

<span data-ttu-id="00ccc-126">Aby uzyskać więcej informacji na temat konfigurowania przykładowej aplikacji za pomocą dyrektywy preprocesora (`#define`), zobacz <xref:index#preprocessor-directives-in-sample-code>.</span><span class="sxs-lookup"><span data-stu-id="00ccc-126">For more information on how to configure a sample app using preprocessor directives (`#define`), see <xref:index#preprocessor-directives-in-sample-code>.</span></span>

## <a name="secret-storage-in-the-development-environment"></a><span data-ttu-id="00ccc-127">Wpisu tajnego magazynu w środowisku programistycznym</span><span class="sxs-lookup"><span data-stu-id="00ccc-127">Secret storage in the Development environment</span></span>

<span data-ttu-id="00ccc-128">Ustaw lokalnie przy użyciu kluczy tajnych [narzędzie Menedżer klucz tajny](xref:security/app-secrets).</span><span class="sxs-lookup"><span data-stu-id="00ccc-128">Set secrets locally using the [Secret Manager tool](xref:security/app-secrets).</span></span> <span data-ttu-id="00ccc-129">Jeśli przykładowa aplikacja jest uruchamiana na komputerze lokalnym w środowisku programistycznym, wpisów tajnych są ładowane z magazynu lokalnego Menedżera klucz tajny.</span><span class="sxs-lookup"><span data-stu-id="00ccc-129">When the sample app runs on the local machine in the Development environment, secrets are loaded from the local Secret Manager store.</span></span>

<span data-ttu-id="00ccc-130">Narzędzie Menedżer klucz tajny wymaga `<UserSecretsId>` właściwość w pliku projektu aplikacji.</span><span class="sxs-lookup"><span data-stu-id="00ccc-130">The Secret Manager tool requires a `<UserSecretsId>` property in the app's project file.</span></span> <span data-ttu-id="00ccc-131">Ustaw wartość właściwości (`{GUID}`) do dowolnej Unikatowy identyfikator GUID:</span><span class="sxs-lookup"><span data-stu-id="00ccc-131">Set the property value (`{GUID}`) to any unique GUID:</span></span>

```xml
<PropertyGroup>
  <UserSecretsId>{GUID}</UserSecretsId>
</PropertyGroup>
```

<span data-ttu-id="00ccc-132">Klucze tajne są tworzone jako pary nazwa wartość.</span><span class="sxs-lookup"><span data-stu-id="00ccc-132">Secrets are created as name-value pairs.</span></span> <span data-ttu-id="00ccc-133">Użyj wartości hierarchicznych (sekcji konfiguracji) `:` (dwukropek) jako separator w [konfiguracji platformy ASP.NET Core](xref:fundamentals/configuration/index) nazw kluczy.</span><span class="sxs-lookup"><span data-stu-id="00ccc-133">Hierarchical values (configuration sections) use a `:` (colon) as a separator in [ASP.NET Core configuration](xref:fundamentals/configuration/index) key names.</span></span>

<span data-ttu-id="00ccc-134">Menedżer klucz tajny jest używany z powłoki poleceń, otwarte, aby zawartość katalogu głównego projektu, gdzie `{SECRET NAME}` nazywa się i `{SECRET VALUE}` jest wartością:</span><span class="sxs-lookup"><span data-stu-id="00ccc-134">The Secret Manager is used from a command shell opened to the project's content root, where `{SECRET NAME}` is the name and `{SECRET VALUE}` is the value:</span></span>

```console
dotnet user-secrets set "{SECRET NAME}" "{SECRET VALUE}"
```

<span data-ttu-id="00ccc-135">W powłoce poleceń z katalogu głównego zawartości projektu, aby ustawić wpisów tajnych dla przykładowej aplikacji, wykonaj następujące polecenia:</span><span class="sxs-lookup"><span data-stu-id="00ccc-135">Execute the following commands in a command shell from the project's content root to set the secrets for the sample app:</span></span>

```console
dotnet user-secrets set "SecretName" "secret_value_1_dev"
dotnet user-secrets set "Section:SecretName" "secret_value_2_dev"
```

<span data-ttu-id="00ccc-136">Kiedy te klucze tajne są przechowywane w usłudze Azure Key Vault w [wpisu tajnego magazynu w środowisku produkcyjnym za pomocą usługi Azure Key Vault](#secret-storage-in-the-production-environment-with-azure-key-vault) sekcji `_dev` sufiks jest zmieniana na `_prod`.</span><span class="sxs-lookup"><span data-stu-id="00ccc-136">When these secrets are stored in Azure Key Vault in the [Secret storage in the Production environment with Azure Key Vault](#secret-storage-in-the-production-environment-with-azure-key-vault) section, the `_dev` suffix is changed to `_prod`.</span></span> <span data-ttu-id="00ccc-137">Sufiks oferuje wizualną w danych wyjściowych aplikacji wskazujący źródła wartości konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="00ccc-137">The suffix provides a visual cue in the app's output indicating the source of the configuration values.</span></span>

## <a name="secret-storage-in-the-production-environment-with-azure-key-vault"></a><span data-ttu-id="00ccc-138">Wpisu tajnego magazynu w środowisku produkcyjnym za pomocą usługi Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="00ccc-138">Secret storage in the Production environment with Azure Key Vault</span></span>

<span data-ttu-id="00ccc-139">Z instrukcjami wyświetlanymi przez [Szybki Start: Ustawianie i pobieranie wpisu tajnego z usługi Azure Key Vault przy użyciu wiersza polecenia platformy Azure](/azure/key-vault/quick-create-cli) tematu są podsumowywane w tym miejscu służą do tworzenia usługi Azure Key Vault i przechowywania wpisów tajnych używanych przez aplikację przykładową.</span><span class="sxs-lookup"><span data-stu-id="00ccc-139">The instructions provided by the [Quickstart: Set and retrieve a secret from Azure Key Vault using Azure CLI](/azure/key-vault/quick-create-cli) topic are summarized here for creating an Azure Key Vault and storing secrets used by the sample app.</span></span> <span data-ttu-id="00ccc-140">Zapoznaj się z tematem, aby uzyskać więcej informacji.</span><span class="sxs-lookup"><span data-stu-id="00ccc-140">Refer to the topic for further details.</span></span>

1. <span data-ttu-id="00ccc-141">Otwórz usługa Azure Cloud shell przy użyciu jednej z następujących metod w [witryny Azure portal](https://portal.azure.com/):</span><span class="sxs-lookup"><span data-stu-id="00ccc-141">Open Azure Cloud shell using any one of the following methods in the [Azure portal](https://portal.azure.com/):</span></span>

   * <span data-ttu-id="00ccc-142">Wybierz **wypróbuj** w prawym górnym rogu bloku kodu.</span><span class="sxs-lookup"><span data-stu-id="00ccc-142">Select **Try It** in the upper-right corner of a code block.</span></span> <span data-ttu-id="00ccc-143">W polu tekstowym, należy użyć ciągu "Wiersza polecenia platformy Azure".</span><span class="sxs-lookup"><span data-stu-id="00ccc-143">Use the search string "Azure CLI" in the text box.</span></span>
   * <span data-ttu-id="00ccc-144">Otwórz usługę Cloud Shell w przeglądarce za pośrednictwem **Launch Cloud Shell** przycisku.</span><span class="sxs-lookup"><span data-stu-id="00ccc-144">Open Cloud Shell in your browser with the **Launch Cloud Shell** button.</span></span>
   * <span data-ttu-id="00ccc-145">Wybierz **Cloud Shell** przycisk menu w prawym górnym rogu witryny Azure portal.</span><span class="sxs-lookup"><span data-stu-id="00ccc-145">Select the **Cloud Shell** button on the menu in the upper-right corner of the Azure portal.</span></span>

   <span data-ttu-id="00ccc-146">Aby uzyskać więcej informacji, zobacz [interfejsu wiersza polecenia platformy Azure (CLI)](/cli/azure/) i [Omówienie usługi Azure Cloud Shell](/azure/cloud-shell/overview).</span><span class="sxs-lookup"><span data-stu-id="00ccc-146">For more information, see [Azure Command-Line Interface (CLI)](/cli/azure/) and [Overview of Azure Cloud Shell](/azure/cloud-shell/overview).</span></span>

1. <span data-ttu-id="00ccc-147">Jeśli nie są już uwierzytelniony, zaloguj się przy użyciu `az login` polecenia.</span><span class="sxs-lookup"><span data-stu-id="00ccc-147">If you aren't already authenticated, sign in with the `az login` command.</span></span>

1. <span data-ttu-id="00ccc-148">Utwórz grupę zasobów za pomocą następującego polecenia, gdzie `{RESOURCE GROUP NAME}` to nazwa grupy zasobów dla nowej grupy zasobów i `{LOCATION}` to region platformy Azure (datacenter):</span><span class="sxs-lookup"><span data-stu-id="00ccc-148">Create a resource group with the following command, where `{RESOURCE GROUP NAME}` is the resource group name for the new resource group and `{LOCATION}` is the Azure region (datacenter):</span></span>

   ```console
   az group create --name "{RESOURCE GROUP NAME}" --location {LOCATION}
   ```

1. <span data-ttu-id="00ccc-149">Tworzenie magazynu kluczy w grupie zasobów za pomocą następującego polecenia, gdzie `{KEY VAULT NAME}` to nazwa dla nowego magazynu kluczy i `{LOCATION}` to region platformy Azure (datacenter):</span><span class="sxs-lookup"><span data-stu-id="00ccc-149">Create a key vault in the resource group with the following command, where `{KEY VAULT NAME}` is the name for the new key vault and `{LOCATION}` is the Azure region (datacenter):</span></span>

   ```console
   az keyvault create --name "{KEY VAULT NAME}" --resource-group "{RESOURCE GROUP NAME}" --location {LOCATION}
   ```

1. <span data-ttu-id="00ccc-150">Tworzenie wpisów tajnych w magazynie kluczy jako pary nazwa wartość.</span><span class="sxs-lookup"><span data-stu-id="00ccc-150">Create secrets in the key vault as name-value pairs.</span></span>

   <span data-ttu-id="00ccc-151">Usługa Azure Key Vault nazw kluczy tajnych są ograniczone do znaki alfanumeryczne i łączniki.</span><span class="sxs-lookup"><span data-stu-id="00ccc-151">Azure Key Vault secret names are limited to alphanumeric characters and dashes.</span></span> <span data-ttu-id="00ccc-152">Użyj wartości hierarchicznych (sekcji konfiguracji) `--` (dwa łączniki) jako separatora.</span><span class="sxs-lookup"><span data-stu-id="00ccc-152">Hierarchical values (configuration sections) use `--` (two dashes) as a separator.</span></span> <span data-ttu-id="00ccc-153">Dwukropki, które są zwykle używane do ograniczania sekcji w podkluczu w [konfiguracji platformy ASP.NET Core](xref:fundamentals/configuration/index), nie są dozwolone w nazwach wpisu tajnego usługi key vault.</span><span class="sxs-lookup"><span data-stu-id="00ccc-153">Colons, which are normally used to delimit a section from a subkey in [ASP.NET Core configuration](xref:fundamentals/configuration/index), aren't allowed in key vault secret names.</span></span> <span data-ttu-id="00ccc-154">W związku z tym dwa łączniki są używane i zamienione dla dwukropek, gdy wpisy tajne są ładowane do konfiguracji aplikacji.</span><span class="sxs-lookup"><span data-stu-id="00ccc-154">Therefore, two dashes are used and swapped for a colon when the secrets are loaded into the app's configuration.</span></span>

   <span data-ttu-id="00ccc-155">Następujące klucze tajne są do użytku z przykładowej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="00ccc-155">The following secrets are for use with the sample app.</span></span> <span data-ttu-id="00ccc-156">Wartości obejmują `_prod` sufiks, aby odróżnić je od `_dev` sufiks wartości załadowane w środowisku programistycznym z wpisami tajnymi użytkowników.</span><span class="sxs-lookup"><span data-stu-id="00ccc-156">The values include a `_prod` suffix to distinguish them from the `_dev` suffix values loaded in the Development environment from User Secrets.</span></span> <span data-ttu-id="00ccc-157">Zastąp `{KEY VAULT NAME}` nazwą magazynu kluczy, który został utworzony w poprzednim kroku:</span><span class="sxs-lookup"><span data-stu-id="00ccc-157">Replace `{KEY VAULT NAME}` with the name of the key vault that you created in the prior step:</span></span>

   ```console
   az keyvault secret set --vault-name "{KEY VAULT NAME}" --name "SecretName" --value "secret_value_1_prod"
   az keyvault secret set --vault-name "{KEY VAULT NAME}" --name "Section--SecretName" --value "secret_value_2_prod"
   ```

## <a name="use-application-id-and-x509-certificate-for-non-azure-hosted-apps"></a><span data-ttu-id="00ccc-158">Użyj Identyfikatora aplikacji i X.509 certyfikatu dla aplikacji hostowanej platformy Azure</span><span class="sxs-lookup"><span data-stu-id="00ccc-158">Use Application ID and X.509 certificate for non-Azure-hosted apps</span></span>

<span data-ttu-id="00ccc-159">Konfigurowanie usługi Azure AD i usługi Azure Key Vault oraz aplikacji, które można użyć Identyfikatora aplikacji Azure Active Directory i X.509 certyfikatu do uwierzytelniania do magazynu kluczy **gdy aplikacja jest hostowana poza platformą Azure**.</span><span class="sxs-lookup"><span data-stu-id="00ccc-159">Configure Azure AD, Azure Key Vault, and the app to use an Azure Active Directory Application ID and X.509 certificate to authenticate to a key vault **when the app is hosted outside of Azure**.</span></span> <span data-ttu-id="00ccc-160">Aby uzyskać więcej informacji, zobacz [o kluczy, wpisów tajnych i certyfikatów](/azure/key-vault/about-keys-secrets-and-certificates).</span><span class="sxs-lookup"><span data-stu-id="00ccc-160">For more information, see [About keys, secrets, and certificates](/azure/key-vault/about-keys-secrets-and-certificates).</span></span>

> [!NOTE]
> <span data-ttu-id="00ccc-161">Przy użyciu Identyfikatora aplikacji i X.509 certyfikatu jest obsługiwana dla aplikacji hostowanych na platformie Azure, zalecamy używanie [zarządzanych tożsamości dla zasobów platformy Azure](#use-managed-identities-for-azure-resources) odnośnie do hostowania aplikacji na platformie Azure.</span><span class="sxs-lookup"><span data-stu-id="00ccc-161">Although using an Application ID and X.509 certificate is supported for apps hosted in Azure, we recommend using [Managed identities for Azure resources](#use-managed-identities-for-azure-resources) when hosting an app in Azure.</span></span> <span data-ttu-id="00ccc-162">Zarządzanych tożsamości nie wymagają przechowywania certyfikatu w aplikacji lub w środowisku programistycznym.</span><span class="sxs-lookup"><span data-stu-id="00ccc-162">Managed identities don't require storing a certificate in the app or in the development environment.</span></span>

<span data-ttu-id="00ccc-163">Ta aplikacja używa przykładowych Identyfikatora aplikacji oraz X.509 certyfikatu, kiedy `#define` instrukcji na górze *Program.cs* pliku jest ustawiona na `Certificate`.</span><span class="sxs-lookup"><span data-stu-id="00ccc-163">The sample app uses an Application ID and X.509 certificate when the `#define` statement at the top of the *Program.cs* file is set to `Certificate`.</span></span>

1. <span data-ttu-id="00ccc-164">Utwórz archiwum PKCS #12 (*PFX*) certyfikatu.</span><span class="sxs-lookup"><span data-stu-id="00ccc-164">Create a PKCS#12 archive (*.pfx*) certificate.</span></span> <span data-ttu-id="00ccc-165">Opcje tworzenia certyfikatów obejmują [MakeCert w Windows](/windows/desktop/seccrypto/makecert) i [OpenSSL](https://www.openssl.org/).</span><span class="sxs-lookup"><span data-stu-id="00ccc-165">Options for creating certificates include [MakeCert on Windows](/windows/desktop/seccrypto/makecert) and [OpenSSL](https://www.openssl.org/).</span></span>
1. <span data-ttu-id="00ccc-166">Zainstaluj certyfikat w magazynie certyfikatów osobistych bieżącego użytkownika.</span><span class="sxs-lookup"><span data-stu-id="00ccc-166">Install the certificate into the current user's personal certificate store.</span></span> <span data-ttu-id="00ccc-167">Oznaczanie klucz jako eksportowalny jest opcjonalne.</span><span class="sxs-lookup"><span data-stu-id="00ccc-167">Marking the key as exportable is optional.</span></span> <span data-ttu-id="00ccc-168">Należy pamiętać, odcisk palca certyfikatu, który jest używany w dalszej części tego procesu.</span><span class="sxs-lookup"><span data-stu-id="00ccc-168">Note the certificate's thumbprint, which is used later in this process.</span></span>
1. <span data-ttu-id="00ccc-169">Eksportuj archiwum PKCS #12 (*PFX*) certyfikat jako certyfikatu z algorytmem DER (*cer*).</span><span class="sxs-lookup"><span data-stu-id="00ccc-169">Export the PKCS#12 archive (*.pfx*) certificate as a DER-encoded certificate (*.cer*).</span></span>
1. <span data-ttu-id="00ccc-170">Rejestrowanie aplikacji w usłudze Azure AD (**rejestracje aplikacji**).</span><span class="sxs-lookup"><span data-stu-id="00ccc-170">Register the app with Azure AD (**App registrations**).</span></span>
1. <span data-ttu-id="00ccc-171">Przekaż certyfikat algorytmem DER (*cer*) do usługi Azure AD:</span><span class="sxs-lookup"><span data-stu-id="00ccc-171">Upload the DER-encoded certificate (*.cer*) to Azure AD:</span></span>
   1. <span data-ttu-id="00ccc-172">Wybierz aplikację w usłudze Azure AD.</span><span class="sxs-lookup"><span data-stu-id="00ccc-172">Select the app in Azure AD.</span></span>
   1. <span data-ttu-id="00ccc-173">Przejdź do **certyfikaty i klucze tajne**.</span><span class="sxs-lookup"><span data-stu-id="00ccc-173">Navigate to **Certificates & secrets**.</span></span>
   1. <span data-ttu-id="00ccc-174">Wybierz **Przekaż certyfikat** można przekazać certyfikatu, który zawiera klucz publiczny.</span><span class="sxs-lookup"><span data-stu-id="00ccc-174">Select **Upload certificate** to upload the certificate, which contains the public key.</span></span> <span data-ttu-id="00ccc-175">A *cer*, *PEM*, lub *.crt* certyfikatu jest dopuszczalna.</span><span class="sxs-lookup"><span data-stu-id="00ccc-175">A *.cer*, *.pem*, or *.crt* certificate is acceptable.</span></span>
1. <span data-ttu-id="00ccc-176">Store nazwa magazynu kluczy, identyfikator aplikacji i odcisk palca certyfikatu w aplikacji *appsettings.json* pliku.</span><span class="sxs-lookup"><span data-stu-id="00ccc-176">Store the key vault name, Application ID, and certificate thumbprint in the app's *appsettings.json* file.</span></span>
1. <span data-ttu-id="00ccc-177">Przejdź do **klucza magazynów** w witrynie Azure portal.</span><span class="sxs-lookup"><span data-stu-id="00ccc-177">Navigate to **Key vaults** in the Azure portal.</span></span>
1. <span data-ttu-id="00ccc-178">Wybierz magazyn kluczy, który został utworzony w [wpisu tajnego magazynu w środowisku produkcyjnym za pomocą usługi Azure Key Vault](#secret-storage-in-the-production-environment-with-azure-key-vault) sekcji.</span><span class="sxs-lookup"><span data-stu-id="00ccc-178">Select the key vault that you created in the [Secret storage in the Production environment with Azure Key Vault](#secret-storage-in-the-production-environment-with-azure-key-vault) section.</span></span>
1. <span data-ttu-id="00ccc-179">Wybierz **zasady dostępu**.</span><span class="sxs-lookup"><span data-stu-id="00ccc-179">Select **Access policies**.</span></span>
1. <span data-ttu-id="00ccc-180">Wybierz **Dodaj nowe**.</span><span class="sxs-lookup"><span data-stu-id="00ccc-180">Select **Add new**.</span></span>
1. <span data-ttu-id="00ccc-181">Wybierz **Wybierz podmiot zabezpieczeń** i wybierz zarejestrowany aplikację według nazwy.</span><span class="sxs-lookup"><span data-stu-id="00ccc-181">Select **Select principal** and select the registered app by name.</span></span> <span data-ttu-id="00ccc-182">Wybierz **wybierz** przycisku.</span><span class="sxs-lookup"><span data-stu-id="00ccc-182">Select the **Select** button.</span></span>
1. <span data-ttu-id="00ccc-183">Otwórz **uprawnienia klucza tajnego** i zapewniają aplikacji za pomocą **uzyskać** i **listy** uprawnienia.</span><span class="sxs-lookup"><span data-stu-id="00ccc-183">Open **Secret permissions** and provide the app with **Get** and **List** permissions.</span></span>
1. <span data-ttu-id="00ccc-184">Kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="00ccc-184">Select **OK**.</span></span>
1. <span data-ttu-id="00ccc-185">Wybierz pozycję **Zapisz**.</span><span class="sxs-lookup"><span data-stu-id="00ccc-185">Select **Save**.</span></span>
1. <span data-ttu-id="00ccc-186">Wdróż aplikację.</span><span class="sxs-lookup"><span data-stu-id="00ccc-186">Deploy the app.</span></span>

<span data-ttu-id="00ccc-187">`Certificate` Przykładowa aplikacja uzyskuje jego wartości konfiguracji z `IConfigurationRoot` o takiej samej nazwie jak nazwa wpisu tajnego:</span><span class="sxs-lookup"><span data-stu-id="00ccc-187">The `Certificate` sample app obtains its configuration values from `IConfigurationRoot` with the same name as the secret name:</span></span>

* <span data-ttu-id="00ccc-188">Wartości inne niż hierarchiczne: Wartość `SecretName` są uzyskiwane z `config["SecretName"]`.</span><span class="sxs-lookup"><span data-stu-id="00ccc-188">Non-hierarchical values: The value for `SecretName` is obtained with `config["SecretName"]`.</span></span>
* <span data-ttu-id="00ccc-189">Hierarchiczna wartości (sekcje): Użyj `:` notacji (dwukropek) lub `GetSection` — metoda rozszerzenia.</span><span class="sxs-lookup"><span data-stu-id="00ccc-189">Hierarchical values (sections): Use `:` (colon) notation or the `GetSection` extension method.</span></span> <span data-ttu-id="00ccc-190">Użyj jednej z tych metod można uzyskać wartości konfiguracji:</span><span class="sxs-lookup"><span data-stu-id="00ccc-190">Use either of these approaches to obtain the configuration value:</span></span>
  * `config["Section:SecretName"]`
  * `config.GetSection("Section")["SecretName"]`

<span data-ttu-id="00ccc-191">Certyfikat X.509 jest zarządzany przez system operacyjny.</span><span class="sxs-lookup"><span data-stu-id="00ccc-191">The X.509 certificate is managed by the OS.</span></span> <span data-ttu-id="00ccc-192">Wywołania aplikacji `AddAzureKeyVault` przy użyciu wartości dostarczone przez *appsettings.json* pliku:</span><span class="sxs-lookup"><span data-stu-id="00ccc-192">The app calls `AddAzureKeyVault` with values supplied by the *appsettings.json* file:</span></span>

[!code-csharp[](key-vault-configuration/sample/Program.cs?name=snippet1&highlight=20-23)]

<span data-ttu-id="00ccc-193">Przykładowe wartości:</span><span class="sxs-lookup"><span data-stu-id="00ccc-193">Example values:</span></span>

* <span data-ttu-id="00ccc-194">Nazwa magazynu kluczy: `contosovault`</span><span class="sxs-lookup"><span data-stu-id="00ccc-194">Key vault name: `contosovault`</span></span>
* <span data-ttu-id="00ccc-195">Identyfikator aplikacji: `627e911e-43cc-61d4-992e-12db9c81b413`</span><span class="sxs-lookup"><span data-stu-id="00ccc-195">Application ID: `627e911e-43cc-61d4-992e-12db9c81b413`</span></span>
* <span data-ttu-id="00ccc-196">Odcisk palca certyfikatu: `fe14593dd66b2406c5269d742d04b6e1ab03adb1`</span><span class="sxs-lookup"><span data-stu-id="00ccc-196">Certificate thumbprint: `fe14593dd66b2406c5269d742d04b6e1ab03adb1`</span></span>

<span data-ttu-id="00ccc-197">*appsettings.json*:</span><span class="sxs-lookup"><span data-stu-id="00ccc-197">*appsettings.json*:</span></span>

[!code-json[](key-vault-configuration/sample/appsettings.json)]

<span data-ttu-id="00ccc-198">Po uruchomieniu aplikacji, strony sieci Web wyświetlane są załadowane wartości klucza tajnego.</span><span class="sxs-lookup"><span data-stu-id="00ccc-198">When you run the app, a webpage shows the loaded secret values.</span></span> <span data-ttu-id="00ccc-199">W środowisku programistycznym wartościami wpisów tajnych załadować dane przy użyciu `_dev` sufiks.</span><span class="sxs-lookup"><span data-stu-id="00ccc-199">In the Development environment, secret values load with the `_dev` suffix.</span></span> <span data-ttu-id="00ccc-200">W środowisku produkcyjnym wartości załadować dane przy użyciu `_prod` sufiks.</span><span class="sxs-lookup"><span data-stu-id="00ccc-200">In the Production environment, the values load with the `_prod` suffix.</span></span>

## <a name="use-managed-identities-for-azure-resources"></a><span data-ttu-id="00ccc-201">Użyj tożsamości zarządzanych zasobów platformy Azure</span><span class="sxs-lookup"><span data-stu-id="00ccc-201">Use Managed identities for Azure resources</span></span>

<span data-ttu-id="00ccc-202">**Aplikacja wdrożona na platformie Azure** korzystać z zalet [zarządzanych tożsamości dla zasobów platformy Azure](/azure/active-directory/managed-identities-azure-resources/overview), co pozwala aplikacji na uwierzytelnianie za pomocą usługi Azure Key Vault przy użyciu uwierzytelniania usługi Azure AD bez poświadczeń (identyfikator aplikacji i Klucz tajny Password/Client) przechowywanych w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="00ccc-202">**An app deployed to Azure** can take advantage of [Managed identities for Azure resources](/azure/active-directory/managed-identities-azure-resources/overview), which allows the app to authenticate with Azure Key Vault using Azure AD authentication without credentials (Application ID and Password/Client Secret) stored in the app.</span></span>

<span data-ttu-id="00ccc-203">Przykładowa aplikacja korzysta z tożsamości zarządzanego dla zasobów platformy Azure po `#define` instrukcji na górze *Program.cs* pliku jest ustawiona na `Managed`.</span><span class="sxs-lookup"><span data-stu-id="00ccc-203">The sample app uses Managed identities for Azure resources when the `#define` statement at the top of the *Program.cs* file is set to `Managed`.</span></span>

<span data-ttu-id="00ccc-204">Wprowadź nazwę magazynu z aplikacją *appsettings.json* pliku.</span><span class="sxs-lookup"><span data-stu-id="00ccc-204">Enter the vault name into the app's *appsettings.json* file.</span></span> <span data-ttu-id="00ccc-205">Przykładowa aplikacja nie wymaga Identyfikatora aplikacji i hasło (klucz tajny klienta), po ustawieniu `Managed` wersji, dzięki czemu można zignorować te pozycje konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="00ccc-205">The sample app doesn't require an Application ID and Password (Client Secret) when set to the `Managed` version, so you can ignore those configuration entries.</span></span> <span data-ttu-id="00ccc-206">Aplikacja jest wdrożona na platformie Azure, a platforma Azure uwierzytelnia aplikacji dostępu do usługi Azure Key Vault tylko przy użyciu nazwy magazynu przechowywanych w *appsettings.json* pliku.</span><span class="sxs-lookup"><span data-stu-id="00ccc-206">The app is deployed to Azure, and Azure authenticates the app to access Azure Key Vault only using the vault name stored in the *appsettings.json* file.</span></span>

<span data-ttu-id="00ccc-207">Wdróż przykładową aplikację w usłudze Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="00ccc-207">Deploy the sample app to Azure App Service.</span></span>

<span data-ttu-id="00ccc-208">Wdrożenie aplikacji w usłudze Azure App Service jest automatycznie rejestrowane w usłudze Azure AD podczas tworzenia usługi.</span><span class="sxs-lookup"><span data-stu-id="00ccc-208">An app deployed to Azure App Service is automatically registered with Azure AD when the service is created.</span></span> <span data-ttu-id="00ccc-209">Uzyskaj identyfikator obiektu z wdrożenia do użycia w następującym poleceniu.</span><span class="sxs-lookup"><span data-stu-id="00ccc-209">Obtain the Object ID from the deployment for use in the following command.</span></span> <span data-ttu-id="00ccc-210">Identyfikator obiektu są wyświetlane w witrynie Azure portal na **tożsamości** panelu usługi App Service.</span><span class="sxs-lookup"><span data-stu-id="00ccc-210">The Object ID is shown in the Azure portal on the **Identity** panel of the App Service.</span></span>

<span data-ttu-id="00ccc-211">Przy użyciu wiersza polecenia platformy Azure i identyfikator obiektu aplikacji, zapewnić aplikacji za pomocą `list` i `get` uprawnienia do dostępu do magazynu kluczy:</span><span class="sxs-lookup"><span data-stu-id="00ccc-211">Using Azure CLI and the app's Object ID, provide the app with `list` and `get` permissions to access the key vault:</span></span>

```console
az keyvault set-policy --name '{KEY VAULT NAME}' --object-id {OBJECT ID} --secret-permissions get list
```

<span data-ttu-id="00ccc-212">**Uruchom ponownie aplikację** przy użyciu wiersza polecenia platformy Azure, programu PowerShell lub witryny Azure portal.</span><span class="sxs-lookup"><span data-stu-id="00ccc-212">**Restart the app** using Azure CLI, PowerShell, or the Azure portal.</span></span>

<span data-ttu-id="00ccc-213">Przykładowa aplikacja:</span><span class="sxs-lookup"><span data-stu-id="00ccc-213">The sample app:</span></span>

* <span data-ttu-id="00ccc-214">Tworzy wystąpienie `AzureServiceTokenProvider` klasy bez parametrów połączenia.</span><span class="sxs-lookup"><span data-stu-id="00ccc-214">Creates an instance of the `AzureServiceTokenProvider` class without a connection string.</span></span> <span data-ttu-id="00ccc-215">Gdy parametry połączenia nie jest podany, dostawca próbuje uzyskać token dostępu z tożsamości zarządzanego dla zasobów platformy Azure.</span><span class="sxs-lookup"><span data-stu-id="00ccc-215">When a connection string isn't provided, the provider attempts to obtain an access token from Managed identities for Azure resources.</span></span>
* <span data-ttu-id="00ccc-216">Nowy `KeyVaultClient` jest tworzona przy użyciu `AzureServiceTokenProvider` wystąpienie tokenu wywołania zwrotnego.</span><span class="sxs-lookup"><span data-stu-id="00ccc-216">A new `KeyVaultClient` is created with the `AzureServiceTokenProvider` instance token callback.</span></span>
* <span data-ttu-id="00ccc-217">`KeyVaultClient` Wystąpienie jest używane z domyślną implementację elementu `IKeyVaultSecretManager` , ładuje wszystkie wartości klucza tajnego i zastępuje kresek podwójnej precyzji (`--`) z dwukropkiem (`:`) nazwy kluczy.</span><span class="sxs-lookup"><span data-stu-id="00ccc-217">The `KeyVaultClient` instance is used with a default implementation of `IKeyVaultSecretManager` that loads all secret values and replaces double-dashes (`--`) with colons (`:`) in key names.</span></span>

[!code-csharp[](key-vault-configuration/sample/Program.cs?name=snippet2&highlight=13-21)]

<span data-ttu-id="00ccc-218">Po uruchomieniu aplikacji, strony sieci Web wyświetlane są załadowane wartości klucza tajnego.</span><span class="sxs-lookup"><span data-stu-id="00ccc-218">When you run the app, a webpage shows the loaded secret values.</span></span> <span data-ttu-id="00ccc-219">W środowisku programistycznym wartościami wpisów tajnych mają `_dev` sufiksu domeny, ponieważ są one udostępniane przez wpisami tajnymi użytkowników.</span><span class="sxs-lookup"><span data-stu-id="00ccc-219">In the Development environment, secret values have the `_dev` suffix because they're provided by User Secrets.</span></span> <span data-ttu-id="00ccc-220">W środowisku produkcyjnym wartości załadować dane przy użyciu `_prod` sufiksu domeny, ponieważ są one udostępniane przez usługę Azure Key Vault.</span><span class="sxs-lookup"><span data-stu-id="00ccc-220">In the Production environment, the values load with the `_prod` suffix because they're provided by Azure Key Vault.</span></span>

<span data-ttu-id="00ccc-221">Jeśli zostanie wyświetlony `Access denied` błąd, upewnij się, że aplikacja jest zarejestrowane w usłudze Azure AD i dostępu do magazynu kluczy.</span><span class="sxs-lookup"><span data-stu-id="00ccc-221">If you receive an `Access denied` error, confirm that the app is registered with Azure AD and provided access to the key vault.</span></span> <span data-ttu-id="00ccc-222">Upewnij się, że został uruchomiony ponownie usługi na platformie Azure.</span><span class="sxs-lookup"><span data-stu-id="00ccc-222">Confirm that you've restarted the service in Azure.</span></span>

## <a name="use-a-key-name-prefix"></a><span data-ttu-id="00ccc-223">Użyj prefiksu nazwy klucza</span><span class="sxs-lookup"><span data-stu-id="00ccc-223">Use a key name prefix</span></span>

<span data-ttu-id="00ccc-224">`AddAzureKeyVault` udostępnia przeciążenie, które akceptuje implementację `IKeyVaultSecretManager`, co pozwala na kontrolowanie sposobu klucza magazynu kluczy tajnych są konwertowane na klucze konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="00ccc-224">`AddAzureKeyVault` provides an overload that accepts an implementation of `IKeyVaultSecretManager`, which allows you to control how key vault secrets are converted into configuration keys.</span></span> <span data-ttu-id="00ccc-225">Na przykład można zaimplementować interfejs służący do ładowania wartościami wpisów tajnych na podstawie wartości prefiks, który podajesz podczas uruchamiania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="00ccc-225">For example, you can implement the interface to load secret values based on a prefix value you provide at app startup.</span></span> <span data-ttu-id="00ccc-226">Dzięki temu można na przykład, aby ładować wpisy tajne w oparciu o wersję aplikacji.</span><span class="sxs-lookup"><span data-stu-id="00ccc-226">This allows you, for example, to load secrets based on the version of the app.</span></span>

> [!WARNING]
> <span data-ttu-id="00ccc-227">Nie używaj prefiksy na wpisy tajne usługi key vault, umieść wpisów tajnych dla wielu aplikacji w tym samym magazynie kluczy lub umieść środowiska wpisy tajne (na przykład *rozwoju* a *produkcji* wpisy tajne) w tej samej Magazyn.</span><span class="sxs-lookup"><span data-stu-id="00ccc-227">Don't use prefixes on key vault secrets to place secrets for multiple apps into the same key vault or to place environmental secrets (for example, *development* versus *production* secrets) into the same vault.</span></span> <span data-ttu-id="00ccc-228">Zalecane jest różnych aplikacji i środowisk tworzenia/produkcyjnych użycie oddzielnych magazynów kluczy do izolowania aplikacji środowiska na najwyższy poziom zabezpieczeń.</span><span class="sxs-lookup"><span data-stu-id="00ccc-228">We recommend that different apps and development/production environments use separate key vaults to isolate app environments for the highest level of security.</span></span>

<span data-ttu-id="00ccc-229">W poniższym przykładzie wpisu tajnego jest określana w klucza magazynu (i przy użyciu narzędzia menedżera klucz tajny dla środowiska programistycznego) dla `5000-AppSecret` (kropki nie są dozwolone w nazwach wpisu tajnego usługi key vault).</span><span class="sxs-lookup"><span data-stu-id="00ccc-229">In the following example, a secret is established in the key vault (and using the Secret Manager tool for the Development environment) for `5000-AppSecret` (periods aren't allowed in key vault secret names).</span></span> <span data-ttu-id="00ccc-230">Ten wpis tajny reprezentuje klucz tajny aplikacji dla 5.0.0.0 wersję aplikacji.</span><span class="sxs-lookup"><span data-stu-id="00ccc-230">This secret represents an app secret for version 5.0.0.0 of the app.</span></span> <span data-ttu-id="00ccc-231">Dla innej wersji aplikacji, 5.1.0.0, klucz tajny jest dodawany do klucza magazynu (i przy użyciu narzędzia menedżera klucz tajny) dla `5100-AppSecret`.</span><span class="sxs-lookup"><span data-stu-id="00ccc-231">For another version of the app, 5.1.0.0, a secret is added to the key vault (and using the Secret Manager tool) for `5100-AppSecret`.</span></span> <span data-ttu-id="00ccc-232">Każda wersja aplikacji ładuje jego wersjonowany wartość wpisu tajnego do jego konfigurację jako `AppSecret`, oddzielającego off wersji podczas ładowania klucza tajnego.</span><span class="sxs-lookup"><span data-stu-id="00ccc-232">Each app version loads its versioned secret value into its configuration as `AppSecret`, stripping off the version as it loads the secret.</span></span>

<span data-ttu-id="00ccc-233">`AddAzureKeyVault` jest wywoływana z niestandardowego `IKeyVaultSecretManager`:</span><span class="sxs-lookup"><span data-stu-id="00ccc-233">`AddAzureKeyVault` is called with a custom `IKeyVaultSecretManager`:</span></span>

[!code-csharp[](key-vault-configuration/sample_snapshot/Program.cs?highlight=30-34)]

<span data-ttu-id="00ccc-234">`IKeyVaultSecretManager` Implementacji reaguje na prefiksy wersji wpisów tajnych można załadować odpowiedniego wpisu tajnego do konfiguracji:</span><span class="sxs-lookup"><span data-stu-id="00ccc-234">The `IKeyVaultSecretManager` implementation reacts to the version prefixes of secrets to load the proper secret into configuration:</span></span>

[!code-csharp[](key-vault-configuration/sample_snapshot/Startup.cs?name=snippet1)]

<span data-ttu-id="00ccc-235">`Load` Metoda jest wywoływana przez algorytm dostawcy, który iteruje po magazyn kluczy tajnych wyszukać te, które mają prefiks wersji.</span><span class="sxs-lookup"><span data-stu-id="00ccc-235">The `Load` method is called by a provider algorithm that iterates through the vault secrets to find the ones that have the version prefix.</span></span> <span data-ttu-id="00ccc-236">Gdy prefiks wersji znajduje się za pomocą `Load`, algorytm używa `GetKey` metodę, aby zwrócić nazwę konfiguracji nazwa wpisu tajnego.</span><span class="sxs-lookup"><span data-stu-id="00ccc-236">When a version prefix is found with `Load`, the algorithm uses the `GetKey` method to return the configuration name of the secret name.</span></span> <span data-ttu-id="00ccc-237">On paski Wyłącz prefiks wersji od nazwy klucza tajnego i zwraca pozostałe Nazwa wpisu tajnego do załadowania do konfiguracji aplikacji pary nazwa wartość.</span><span class="sxs-lookup"><span data-stu-id="00ccc-237">It strips off the version prefix from the secret's name and returns the rest of the secret name for loading into the app's configuration name-value pairs.</span></span>

<span data-ttu-id="00ccc-238">Gdy ta metoda jest zaimplementowana:</span><span class="sxs-lookup"><span data-stu-id="00ccc-238">When this approach is implemented:</span></span>

1. <span data-ttu-id="00ccc-239">Wersja aplikacji określony w pliku projektu aplikacji.</span><span class="sxs-lookup"><span data-stu-id="00ccc-239">The app's version specified in the app's project file.</span></span> <span data-ttu-id="00ccc-240">W poniższym przykładzie ustawiono wersję aplikacji `5.0.0.0`:</span><span class="sxs-lookup"><span data-stu-id="00ccc-240">In the following example, the app's version is set to `5.0.0.0`:</span></span>

   ```xml
   <PropertyGroup>
     <Version>5.0.0.0</Version>
   </PropertyGroup>
   ```

1. <span data-ttu-id="00ccc-241">Upewnij się, że `<UserSecretsId>` właściwość znajduje się w pliku projektu aplikacji, gdzie `{GUID}` jest identyfikatorem GUID, dostarczone przez użytkownika:</span><span class="sxs-lookup"><span data-stu-id="00ccc-241">Confirm that a `<UserSecretsId>` property is present in the app's project file, where `{GUID}` is a user-supplied GUID:</span></span>

   ```xml
   <PropertyGroup>
     <UserSecretsId>{GUID}</UserSecretsId>
   </PropertyGroup>
   ```

   <span data-ttu-id="00ccc-242">Zapisz następujące wpisy tajne lokalnie za pomocą [narzędzie Menedżer klucz tajny](xref:security/app-secrets):</span><span class="sxs-lookup"><span data-stu-id="00ccc-242">Save the following secrets locally with the [Secret Manager tool](xref:security/app-secrets):</span></span>

   ```console
   dotnet user-secrets set "5000-AppSecret" "5.0.0.0_secret_value_dev"
   dotnet user-secrets set "5100-AppSecret" "5.1.0.0_secret_value_dev"
   ```

1. <span data-ttu-id="00ccc-243">Klucze tajne są zapisywane w usłudze Azure Key Vault przy użyciu następujących poleceń interfejsu wiersza polecenia platformy Azure:</span><span class="sxs-lookup"><span data-stu-id="00ccc-243">Secrets are saved in Azure Key Vault using the following Azure CLI commands:</span></span>

   ```console
   az keyvault secret set --vault-name "{KEY VAULT NAME}" --name "5000-AppSecret" --value "5.0.0.0_secret_value_prod"
   az keyvault secret set --vault-name "{KEY VAULT NAME}" --name "5100-AppSecret" --value "5.1.0.0_secret_value_prod"
   ```

1. <span data-ttu-id="00ccc-244">Gdy aplikacja jest uruchamiana, wpisy tajne usługi key vault są ładowane.</span><span class="sxs-lookup"><span data-stu-id="00ccc-244">When the app is run, the key vault secrets are loaded.</span></span> <span data-ttu-id="00ccc-245">Klucz tajny ciągu dla `5000-AppSecret` pasuje do wersji aplikacji określonej w pliku projektu aplikacji (`5.0.0.0`).</span><span class="sxs-lookup"><span data-stu-id="00ccc-245">The string secret for `5000-AppSecret` is matched to the app's version specified in the app's project file (`5.0.0.0`).</span></span>

1. <span data-ttu-id="00ccc-246">Wersja, `5000` (przy użyciu dash), jest usuwany z nazwą klucza.</span><span class="sxs-lookup"><span data-stu-id="00ccc-246">The version, `5000` (with the dash), is stripped from the key name.</span></span> <span data-ttu-id="00ccc-247">W całej aplikacji odczytywanie konfiguracji przy użyciu klucza `AppSecret` ładuje wartość wpisu tajnego.</span><span class="sxs-lookup"><span data-stu-id="00ccc-247">Throughout the app, reading configuration with the key `AppSecret` loads the secret value.</span></span>

1. <span data-ttu-id="00ccc-248">Jeśli wersja aplikacji została zmieniona w pliku projektu do `5.1.0.0` i aplikacja jest uruchamiana ponownie, jest zwracana wartość wpisu tajnego `5.1.0.0_secret_value_dev` w środowisku deweloperskim i `5.1.0.0_secret_value_prod` w środowisku produkcyjnym.</span><span class="sxs-lookup"><span data-stu-id="00ccc-248">If the app's version is changed in the project file to `5.1.0.0` and the app is run again, the secret value returned is `5.1.0.0_secret_value_dev` in the Development environment and `5.1.0.0_secret_value_prod` in Production.</span></span>

> [!NOTE]
> <span data-ttu-id="00ccc-249">Możesz również podać własne `KeyVaultClient` implementacji `AddAzureKeyVault`.</span><span class="sxs-lookup"><span data-stu-id="00ccc-249">You can also provide your own `KeyVaultClient` implementation to `AddAzureKeyVault`.</span></span> <span data-ttu-id="00ccc-250">Klient niestandardowy umożliwia udostępnianie jedno wystąpienie klienta w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="00ccc-250">A custom client permits sharing a single instance of the client across the app.</span></span>

## <a name="bind-an-array-to-a-class"></a><span data-ttu-id="00ccc-251">Powiąż tablicę do klasy</span><span class="sxs-lookup"><span data-stu-id="00ccc-251">Bind an array to a class</span></span>

<span data-ttu-id="00ccc-252">Dostawca jest zdolny do odczytywania wartości konfiguracji do tablicy, do powiązania z macierzą POCO.</span><span class="sxs-lookup"><span data-stu-id="00ccc-252">The provider is capable of reading configuration values into an array for binding to a POCO array.</span></span>

<span data-ttu-id="00ccc-253">Podczas odczytywania ze źródła konfiguracji, która umożliwia klucze, aby zawierać dwukropek (`:`) separatory, liczbowych segment klucza jest używane do odróżniania klucze, które tworzą tablicę (`:0:`, `:1:`,...</span><span class="sxs-lookup"><span data-stu-id="00ccc-253">When reading from a configuration source that allows keys to contain colon (`:`) separators, a numeric key segment is used to distinguish the keys that make up an array (`:0:`, `:1:`, …</span></span> <span data-ttu-id="00ccc-254">`:{n}:`).</span><span class="sxs-lookup"><span data-stu-id="00ccc-254">`:{n}:`).</span></span> <span data-ttu-id="00ccc-255">Aby uzyskać więcej informacji, zobacz [konfiguracji: Powiąż tablicę do klasy](xref:fundamentals/configuration/index#bind-an-array-to-a-class).</span><span class="sxs-lookup"><span data-stu-id="00ccc-255">For more information, see [Configuration: Bind an array to a class](xref:fundamentals/configuration/index#bind-an-array-to-a-class).</span></span>

<span data-ttu-id="00ccc-256">Usługa Azure Key Vault klucze nie można użyć dwukropek jako separatora.</span><span class="sxs-lookup"><span data-stu-id="00ccc-256">Azure Key Vault keys can't use a colon as a separator.</span></span> <span data-ttu-id="00ccc-257">Podejście opisane w tym temacie używa podwójnego kreski (`--`) jako separatora hierarchiczne wartości (sekcji).</span><span class="sxs-lookup"><span data-stu-id="00ccc-257">The approach described in this topic uses double dashes (`--`) as a separator for hierarchical values (sections).</span></span> <span data-ttu-id="00ccc-258">Tablicy klucze są przechowywane w usłudze Azure Key Vault, przy użyciu podwójnych kresek i numeryczne kluczowe segmenty (`--0--`, `--1--`, &hellip; `--{n}--`).</span><span class="sxs-lookup"><span data-stu-id="00ccc-258">Array keys are stored in Azure Key Vault with double dashes and numeric key segments (`--0--`, `--1--`, &hellip; `--{n}--`).</span></span>

<span data-ttu-id="00ccc-259">Sprawdź następujące [Serilog](https://serilog.net/) rejestrowania konfigurację dostawcy udostępniane przez plik w formacie JSON.</span><span class="sxs-lookup"><span data-stu-id="00ccc-259">Examine the following [Serilog](https://serilog.net/) logging provider configuration provided by a JSON file.</span></span> <span data-ttu-id="00ccc-260">Istnieją dwa obiektu literały zdefiniowane w `WriteTo` tablica, która odzwierciedla dwóch Serilog *wychwytywanie*, opisywać miejsca docelowe danych wyjściowych rejestrowania:</span><span class="sxs-lookup"><span data-stu-id="00ccc-260">There are two object literals defined in the `WriteTo` array that reflect two Serilog *sinks*, which describe destinations for logging output:</span></span>

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

<span data-ttu-id="00ccc-261">Konfiguracji przedstawionej w poprzednim pliku JSON jest przechowywany w usłudze Azure Key Vault przy użyciu Podwójna kreska (`--`) notacją i numeryczne segmenty:</span><span class="sxs-lookup"><span data-stu-id="00ccc-261">The configuration shown in the preceding JSON file is stored in Azure Key Vault using double dash (`--`) notation and numeric segments:</span></span>

| <span data-ttu-id="00ccc-262">Key</span><span class="sxs-lookup"><span data-stu-id="00ccc-262">Key</span></span> | <span data-ttu-id="00ccc-263">Wartość</span><span class="sxs-lookup"><span data-stu-id="00ccc-263">Value</span></span> |
| --- | ----- |
| `Serilog--WriteTo--0--Name` | `AzureTableStorage` |
| `Serilog--WriteTo--0--Args--storageTableName` | `logs` |
| `Serilog--WriteTo--0--Args--connectionString` | `DefaultEnd...ountKey=Eby8...GMGw==` |
| `Serilog--WriteTo--1--Name` | `AzureDocumentDB` |
| `Serilog--WriteTo--1--Args--endpointUrl` | `https://contoso.documents.azure.com:443` |
| `Serilog--WriteTo--1--Args--authorizationKey` | `Eby8...GMGw==` |

## <a name="reload-secrets"></a><span data-ttu-id="00ccc-264">Załaduj ponownie wpisy tajne</span><span class="sxs-lookup"><span data-stu-id="00ccc-264">Reload secrets</span></span>

<span data-ttu-id="00ccc-265">Klucze tajne są buforowane do momentu `IConfigurationRoot.Reload()` jest wywoływana.</span><span class="sxs-lookup"><span data-stu-id="00ccc-265">Secrets are cached until `IConfigurationRoot.Reload()` is called.</span></span> <span data-ttu-id="00ccc-266">Wygasłe, wyłączona, i zaktualizowanych wpisów tajnych w magazynie kluczy nie są przestrzegane przez aplikację do momentu `Reload` jest wykonywany.</span><span class="sxs-lookup"><span data-stu-id="00ccc-266">Expired, disabled, and updated secrets in the key vault are not respected by the app until `Reload` is executed.</span></span>

```csharp
Configuration.Reload();
```

## <a name="disabled-and-expired-secrets"></a><span data-ttu-id="00ccc-267">Wyłączone lub wygasłe klucze tajne</span><span class="sxs-lookup"><span data-stu-id="00ccc-267">Disabled and expired secrets</span></span>

<span data-ttu-id="00ccc-268">Wyłączone lub wygasłe klucze tajne throw `KeyVaultClientException`.</span><span class="sxs-lookup"><span data-stu-id="00ccc-268">Disabled and expired secrets throw a `KeyVaultClientException`.</span></span> <span data-ttu-id="00ccc-269">Aby zapobiec sytuacji, w której aplikacja zostanie zgłoszony, zastąpić aplikację, lub zaktualizuj wyłączone/wygasły klucz tajny.</span><span class="sxs-lookup"><span data-stu-id="00ccc-269">To prevent your app from throwing, replace your app or update the disabled/expired secret.</span></span>

## <a name="troubleshoot"></a><span data-ttu-id="00ccc-270">Rozwiązywanie problemów</span><span class="sxs-lookup"><span data-stu-id="00ccc-270">Troubleshoot</span></span>

<span data-ttu-id="00ccc-271">Gdy aplikacja zakończy się niepowodzeniem, można załadować konfiguracji przy użyciu dostawcy, komunikat o błędzie zostanie zapisany [infrastruktury platformy ASP.NET Core rejestrowania](xref:fundamentals/logging/index).</span><span class="sxs-lookup"><span data-stu-id="00ccc-271">When the app fails to load configuration using the provider, an error message is written to the [ASP.NET Core Logging infrastructure](xref:fundamentals/logging/index).</span></span> <span data-ttu-id="00ccc-272">Konfiguracja ładowanie uniemożliwia następujące warunki:</span><span class="sxs-lookup"><span data-stu-id="00ccc-272">The following conditions will prevent configuration from loading:</span></span>

* <span data-ttu-id="00ccc-273">Aplikacja lub certyfikat nie jest poprawnie skonfigurowany w usłudze Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="00ccc-273">The app or certificate isn't configured correctly in Azure Active Directory.</span></span>
* <span data-ttu-id="00ccc-274">Magazyn kluczy nie istnieje w usłudze Azure Key Vault.</span><span class="sxs-lookup"><span data-stu-id="00ccc-274">The key vault doesn't exist in Azure Key Vault.</span></span>
* <span data-ttu-id="00ccc-275">Aplikacji nie jest upoważniony do dostępu do magazynu kluczy.</span><span class="sxs-lookup"><span data-stu-id="00ccc-275">The app isn't authorized to access the key vault.</span></span>
* <span data-ttu-id="00ccc-276">Zasady dostępu nie zawiera `Get` i `List` uprawnienia.</span><span class="sxs-lookup"><span data-stu-id="00ccc-276">The access policy doesn't include `Get` and `List` permissions.</span></span>
* <span data-ttu-id="00ccc-277">W usłudze key vault dane konfiguracji (pary nazwa wartość) niepoprawnie nosi nazwę, Brak, wyłączone lub wygasła.</span><span class="sxs-lookup"><span data-stu-id="00ccc-277">In the key vault, the configuration data (name-value pair) is incorrectly named, missing, disabled, or expired.</span></span>
* <span data-ttu-id="00ccc-278">Aplikacja ma nazwę niewłaściwego magazynu kluczy (`KeyVaultName`), identyfikator aplikacji w usłudze Azure AD (`AzureADApplicationId`), lub odcisk palca certyfikatu usługi Azure AD (`AzureADCertThumbprint`).</span><span class="sxs-lookup"><span data-stu-id="00ccc-278">The app has the wrong key vault name (`KeyVaultName`), Azure AD Application Id (`AzureADApplicationId`), or Azure AD certificate thumbprint (`AzureADCertThumbprint`).</span></span>
* <span data-ttu-id="00ccc-279">Klucz konfiguracji (nazwa) jest niepoprawny w aplikacji dla wartości, które próbujesz załadować.</span><span class="sxs-lookup"><span data-stu-id="00ccc-279">The configuration key (name) is incorrect in the app for the value you're trying to load.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="00ccc-280">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="00ccc-280">Additional resources</span></span>

* <xref:fundamentals/configuration/index>
* [<span data-ttu-id="00ccc-281">Microsoft Azure: Magazyn kluczy</span><span class="sxs-lookup"><span data-stu-id="00ccc-281">Microsoft Azure: Key Vault</span></span>](https://azure.microsoft.com/services/key-vault/)
* [<span data-ttu-id="00ccc-282">Microsoft Azure: Dokumentacja usługi Key Vault</span><span class="sxs-lookup"><span data-stu-id="00ccc-282">Microsoft Azure: Key Vault Documentation</span></span>](/azure/key-vault/)
* [<span data-ttu-id="00ccc-283">Jak Generowanie i przenoszenie chronionego przez moduł HSM kluczy dla usługi Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="00ccc-283">How to generate and transfer HSM-protected keys for Azure Key Vault</span></span>](/azure/key-vault/key-vault-hsm-protected-keys)
* [<span data-ttu-id="00ccc-284">Klasa KeyVaultClient</span><span class="sxs-lookup"><span data-stu-id="00ccc-284">KeyVaultClient Class</span></span>](/dotnet/api/microsoft.azure.keyvault.keyvaultclient)
* [<span data-ttu-id="00ccc-285">Szybki start: Ustawianie i pobieranie wpisu tajnego z usługi Azure Key Vault za pomocą aplikacji internetowej platformy .NET</span><span class="sxs-lookup"><span data-stu-id="00ccc-285">Quickstart: Set and retrieve a secret from Azure Key Vault by using a .NET web app</span></span>](/azure/key-vault/quick-create-net)
* [<span data-ttu-id="00ccc-286">Samouczek: Jak używać usługi Azure Key Vault za pomocą maszyny wirtualnej Windows Azure na platformie .NET</span><span class="sxs-lookup"><span data-stu-id="00ccc-286">Tutorial: How to use Azure Key Vault with Azure Windows Virtual Machine in .NET</span></span>](/azure/key-vault/tutorial-net-windows-virtual-machine)
