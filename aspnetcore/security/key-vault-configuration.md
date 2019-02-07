---
title: Dostawca konfiguracji usługi Azure Key Vault w programie ASP.NET Core
author: guardrex
description: Dowiedz się, jak skonfigurować aplikację tak, za pomocą pary nazwa wartość ładowane w czasie wykonywania za pomocą dostawcy konfiguracji magazynu kluczy Azure.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 01/28/2019
uid: security/key-vault-configuration
ms.openlocfilehash: d255321f6083747ce9b452e1efd4da5bc015bf64
ms.sourcegitcommit: 3c2ba9a0d833d2a096d9d800ba67a1a7f9491af0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/07/2019
ms.locfileid: "55854435"
---
# <a name="azure-key-vault-configuration-provider-in-aspnet-core"></a><span data-ttu-id="02a3c-103">Dostawca konfiguracji usługi Azure Key Vault w programie ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="02a3c-103">Azure Key Vault Configuration Provider in ASP.NET Core</span></span>

<span data-ttu-id="02a3c-104">Przez [Luke Latham](https://github.com/guardrex) i [Andrew Stanton pielęgniarki](https://github.com/anurse)</span><span class="sxs-lookup"><span data-stu-id="02a3c-104">By [Luke Latham](https://github.com/guardrex) and [Andrew Stanton-Nurse](https://github.com/anurse)</span></span>

<span data-ttu-id="02a3c-105">W tym dokumencie wyjaśniono, jak używać [Microsoft Azure Key Vault](https://azure.microsoft.com/services/key-vault/) dostawcę konfiguracji, które można załadować wartości konfiguracji aplikacji z wpisy tajne w usłudze Azure Key Vault.</span><span class="sxs-lookup"><span data-stu-id="02a3c-105">This document explains how to use the [Microsoft Azure Key Vault](https://azure.microsoft.com/services/key-vault/) Configuration Provider to load app configuration values from Azure Key Vault secrets.</span></span> <span data-ttu-id="02a3c-106">Usługa Azure Key Vault to usługa oparta na chmurze, które pomaga w ochronę kluczy kryptograficznych i wpisów tajnych używanych przez aplikacje i usługi.</span><span class="sxs-lookup"><span data-stu-id="02a3c-106">Azure Key Vault is a cloud-based service that assists in safeguarding cryptographic keys and secrets used by apps and services.</span></span> <span data-ttu-id="02a3c-107">Typowe scenariusze dotyczące korzystania z usługi Azure Key Vault z aplikacji platformy ASP.NET Core obejmują:</span><span class="sxs-lookup"><span data-stu-id="02a3c-107">Common scenarios for using Azure Key Vault with ASP.NET Core apps include:</span></span>

* <span data-ttu-id="02a3c-108">Kontrolowanie dostępu do danych poufnych konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="02a3c-108">Controlling access to sensitive configuration data.</span></span>
* <span data-ttu-id="02a3c-109">Spełniające wymagania FIPS 140-2 Level 2 zweryfikować sprzętowych modułów zabezpieczeń (HSM), podczas zapisywania danych konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="02a3c-109">Meeting the requirement for FIPS 140-2 Level 2 validated Hardware Security Modules (HSM's) when storing configuration data.</span></span>

<span data-ttu-id="02a3c-110">Ten scenariusz jest dostępna dla aplikacji przeznaczonych dla platformy ASP.NET Core 2.1 lub nowszej.</span><span class="sxs-lookup"><span data-stu-id="02a3c-110">This scenario is available for apps that target ASP.NET Core 2.1 or later.</span></span>

<span data-ttu-id="02a3c-111">[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/key-vault-configuration/sample) ([sposobu pobierania](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="02a3c-111">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/key-vault-configuration/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="packages"></a><span data-ttu-id="02a3c-112">Pakiety</span><span class="sxs-lookup"><span data-stu-id="02a3c-112">Packages</span></span>

<span data-ttu-id="02a3c-113">Aby używać dostawcy konfiguracji magazynu kluczy Azure, Dodaj odwołanie do pakietu [Microsoft.Extensions.Configuration.AzureKeyVault](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.AzureKeyVault/) pakietu.</span><span class="sxs-lookup"><span data-stu-id="02a3c-113">To use the Azure Key Vault Configuration Provider, add a package reference to the [Microsoft.Extensions.Configuration.AzureKeyVault](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.AzureKeyVault/) package.</span></span>

<span data-ttu-id="02a3c-114">Przyjęcie [zarządzanych tożsamości dla zasobów platformy Azure](/azure/active-directory/managed-identities-azure-resources/overview) scenariusza, Dodaj odwołanie do pakietu [Microsoft.Azure.Services.AppAuthentication](https://www.nuget.org/packages/Microsoft.Azure.Services.AppAuthentication/) pakietu.</span><span class="sxs-lookup"><span data-stu-id="02a3c-114">To adopt the [Managed identities for Azure resources](/azure/active-directory/managed-identities-azure-resources/overview) scenario, add a package reference to the [Microsoft.Azure.Services.AppAuthentication](https://www.nuget.org/packages/Microsoft.Azure.Services.AppAuthentication/) package.</span></span>

> [!NOTE]
> <span data-ttu-id="02a3c-115">W czasie pisania najnowszą stabilną wersję `Microsoft.Azure.Services.AppAuthentication`, wersja `1.0.3`, zapewnia obsługę [przypisany systemowo zarządzanych tożsamości](/azure/active-directory/managed-identities-azure-resources/overview#how-does-the-managed-identities-for-azure-resources-worka-namehow-does-it-worka).</span><span class="sxs-lookup"><span data-stu-id="02a3c-115">At the time of writing, the latest stable release of `Microsoft.Azure.Services.AppAuthentication`, version `1.0.3`, provides support for [system-assigned managed identities](/azure/active-directory/managed-identities-azure-resources/overview#how-does-the-managed-identities-for-azure-resources-worka-namehow-does-it-worka).</span></span> <span data-ttu-id="02a3c-116">Obsługa *przypisanych do użytkowników zarządzanych tożsamości* jest dostępna w `1.0.2-preview` pakietu.</span><span class="sxs-lookup"><span data-stu-id="02a3c-116">Support for *user-assigned managed identities* is available in the `1.0.2-preview` package.</span></span> <span data-ttu-id="02a3c-117">W tym temacie przedstawiono użycie tożsamości zarządzanych przez system, a podana Przykładowa aplikacja korzysta z wersji `1.0.3` z `Microsoft.Azure.Services.AppAuthentication` pakietu.</span><span class="sxs-lookup"><span data-stu-id="02a3c-117">This topic demonstrates the use of system-managed identities, and the provided sample app uses version `1.0.3` of the `Microsoft.Azure.Services.AppAuthentication` package.</span></span>

## <a name="sample-app"></a><span data-ttu-id="02a3c-118">Przykładowa aplikacja</span><span class="sxs-lookup"><span data-stu-id="02a3c-118">Sample app</span></span>

<span data-ttu-id="02a3c-119">Przykładowa aplikacja jest uruchamiana w jednym z dwóch trybów ustalany na podstawie `#define` instrukcji na górze *Program.cs* pliku:</span><span class="sxs-lookup"><span data-stu-id="02a3c-119">The sample app runs in either of two modes determined by the `#define` statement at the top of the *Program.cs* file:</span></span>

* <span data-ttu-id="02a3c-120">`Basic` &ndash; Demonstruje użycie Identyfikatora aplikacji systemu Azure klucza magazynu i hasło (klucz tajny klienta), aby dostęp do danych poufnych przechowywanych w magazynie kluczy.</span><span class="sxs-lookup"><span data-stu-id="02a3c-120">`Basic` &ndash; Demonstrates the use of an Azure Key Vault Application ID and Password (Client Secret) to access secrets stored in the key vault.</span></span> <span data-ttu-id="02a3c-121">Wdrażanie `Basic` wersja przykładu do dowolnego hosta, które umożliwia obsługę aplikacji ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="02a3c-121">Deploy the `Basic` version of the sample to any host capable of serving an ASP.NET Core app.</span></span> <span data-ttu-id="02a3c-122">Postępuj zgodnie ze wskazówkami w [Użyj Identyfikatora aplikacji oraz klucz tajny klienta dla aplikacji hostowanych Azure](#use-application-id-and-client-secret-for-non-azure-hosted-apps) sekcji.</span><span class="sxs-lookup"><span data-stu-id="02a3c-122">Follow the guidance in the [Use Application ID and Client Secret for non-Azure-hosted apps](#use-application-id-and-client-secret-for-non-azure-hosted-apps) section.</span></span>
* <span data-ttu-id="02a3c-123">`Managed` &ndash; Pokazuje sposób użycia [zarządzanych tożsamości dla zasobów platformy Azure](/azure/active-directory/managed-identities-azure-resources/overview) można uwierzytelnić aplikację usługi Azure Key Vault przy użyciu uwierzytelniania usługi Azure AD bez poświadczeń przechowywanych w kodzie aplikacji lub konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="02a3c-123">`Managed` &ndash; Demonstrates how to use [Managed identities for Azure resources](/azure/active-directory/managed-identities-azure-resources/overview) to authenticate the app to Azure Key Vault with Azure AD authentication without credentials stored in the app's code or configuration.</span></span> <span data-ttu-id="02a3c-124">Korzystając z zarządzanych tożsamości do uwierzytelniania, identyfikator aplikacji w usłudze Azure AD i hasło (klucz tajny klienta) nie są wymagane.</span><span class="sxs-lookup"><span data-stu-id="02a3c-124">When using managed identities to authenticate, an Azure AD Application ID and Password (Client Secret) aren't required.</span></span> <span data-ttu-id="02a3c-125">`Managed` Wersję przykładu, należy wdrożyć na platformie Azure.</span><span class="sxs-lookup"><span data-stu-id="02a3c-125">The `Managed` version of the sample must be deployed to Azure.</span></span> <span data-ttu-id="02a3c-126">Postępuj zgodnie ze wskazówkami w [używania tożsamości zarządzanych zasobów platformy Azure](#use-managed-identities-for-azure-resources) sekcji.</span><span class="sxs-lookup"><span data-stu-id="02a3c-126">Follow the guidance in the [Use the Managed identities for Azure resources](#use-managed-identities-for-azure-resources) section.</span></span>

<span data-ttu-id="02a3c-127">Aby uzyskać więcej informacji na temat konfigurowania przykładowej aplikacji za pomocą dyrektywy preprocesora (`#define`), zobacz <xref:index#preprocessor-directives-in-sample-code>.</span><span class="sxs-lookup"><span data-stu-id="02a3c-127">For more information on how to configure a sample app using preprocessor directives (`#define`), see <xref:index#preprocessor-directives-in-sample-code>.</span></span>

## <a name="secret-storage-in-the-development-environment"></a><span data-ttu-id="02a3c-128">Wpisu tajnego magazynu w środowisku programistycznym</span><span class="sxs-lookup"><span data-stu-id="02a3c-128">Secret storage in the Development environment</span></span>

<span data-ttu-id="02a3c-129">Ustaw lokalnie przy użyciu kluczy tajnych [narzędzie Menedżer klucz tajny](xref:security/app-secrets).</span><span class="sxs-lookup"><span data-stu-id="02a3c-129">Set secrets locally using the [Secret Manager tool](xref:security/app-secrets).</span></span> <span data-ttu-id="02a3c-130">Jeśli przykładowa aplikacja jest uruchamiana na komputerze lokalnym w środowisku programistycznym, wpisów tajnych są ładowane z magazynu lokalnego Menedżera klucz tajny.</span><span class="sxs-lookup"><span data-stu-id="02a3c-130">When the sample app runs on the local machine in the Development environment, secrets are loaded from the local Secret Manager store.</span></span>

<span data-ttu-id="02a3c-131">Narzędzie Menedżer klucz tajny wymaga `<UserSecretsId>` właściwość w pliku projektu aplikacji.</span><span class="sxs-lookup"><span data-stu-id="02a3c-131">The Secret Manager tool requires a `<UserSecretsId>` property in the app's project file.</span></span> <span data-ttu-id="02a3c-132">Ustaw wartość właściwości (`{GUID}`) do dowolnej Unikatowy identyfikator GUID:</span><span class="sxs-lookup"><span data-stu-id="02a3c-132">Set the property value (`{GUID}`) to any unique GUID:</span></span>

```xml
<PropertyGroup>
  <UserSecretsId>{GUID}</UserSecretsId>
</PropertyGroup>
```

<span data-ttu-id="02a3c-133">Klucze tajne są tworzone jako pary nazwa wartość.</span><span class="sxs-lookup"><span data-stu-id="02a3c-133">Secrets are created as name-value pairs.</span></span> <span data-ttu-id="02a3c-134">Użyj wartości hierarchicznych (sekcji konfiguracji) `:` (dwukropek) jako separator w [konfiguracji platformy ASP.NET Core](xref:fundamentals/configuration/index) nazw kluczy.</span><span class="sxs-lookup"><span data-stu-id="02a3c-134">Hierarchical values (configuration sections) use a `:` (colon) as a separator in [ASP.NET Core configuration](xref:fundamentals/configuration/index) key names.</span></span>

<span data-ttu-id="02a3c-135">Menedżer klucz tajny jest używany z powłoki poleceń, otwarte, aby zawartość katalogu głównego projektu, gdzie `{SECRET NAME}` nazywa się i `{SECRET VALUE}` jest wartością:</span><span class="sxs-lookup"><span data-stu-id="02a3c-135">The Secret Manager is used from a command shell opened to the project's content root, where `{SECRET NAME}` is the name and `{SECRET VALUE}` is the value:</span></span>

```console
dotnet user-secrets set "{SECRET NAME}" "{SECRET VALUE}"
```

<span data-ttu-id="02a3c-136">W powłoce poleceń z katalogu głównego zawartości projektu, aby ustawić wpisów tajnych dla przykładowej aplikacji, wykonaj następujące polecenia:</span><span class="sxs-lookup"><span data-stu-id="02a3c-136">Execute the following commands in a command shell from the project's content root to set the secrets for the sample app:</span></span>

```console
dotnet user-secrets set "SecretName" "secret_value_1_dev"
dotnet user-secrets set "Section:SecretName" "secret_value_2_dev"
```

<span data-ttu-id="02a3c-137">Kiedy te klucze tajne są przechowywane w usłudze Azure Key Vault w [wpisu tajnego magazynu w środowisku produkcyjnym za pomocą usługi Azure Key Vault](#secret-storage-in-the-production-environment-with-azure-key-vault) sekcji `_dev` sufiks jest zmieniana na `_prod`.</span><span class="sxs-lookup"><span data-stu-id="02a3c-137">When these secrets are stored in Azure Key Vault in the [Secret storage in the Production environment with Azure Key Vault](#secret-storage-in-the-production-environment-with-azure-key-vault) section, the `_dev` suffix is changed to `_prod`.</span></span> <span data-ttu-id="02a3c-138">Sufiks oferuje wizualną w danych wyjściowych aplikacji wskazujący źródła wartości konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="02a3c-138">The suffix provides a visual cue in the app's output indicating the source of the configuration values.</span></span>

## <a name="secret-storage-in-the-production-environment-with-azure-key-vault"></a><span data-ttu-id="02a3c-139">Wpisu tajnego magazynu w środowisku produkcyjnym za pomocą usługi Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="02a3c-139">Secret storage in the Production environment with Azure Key Vault</span></span>

<span data-ttu-id="02a3c-140">Z instrukcjami wyświetlanymi przez [Szybki Start: Ustawianie i pobieranie wpisu tajnego z usługi Azure Key Vault przy użyciu wiersza polecenia platformy Azure](/azure/key-vault/quick-create-cli) tematu są podsumowywane w tym miejscu służą do tworzenia usługi Azure Key Vault i przechowywania wpisów tajnych używanych przez aplikację przykładową.</span><span class="sxs-lookup"><span data-stu-id="02a3c-140">The instructions provided by the [Quickstart: Set and retrieve a secret from Azure Key Vault using Azure CLI](/azure/key-vault/quick-create-cli) topic are summarized here for creating an Azure Key Vault and storing secrets used by the sample app.</span></span> <span data-ttu-id="02a3c-141">Zapoznaj się z tematem, aby uzyskać więcej informacji.</span><span class="sxs-lookup"><span data-stu-id="02a3c-141">Refer to the topic for further details.</span></span>

1. <span data-ttu-id="02a3c-142">Otwórz usługa Azure Cloud shell przy użyciu jednej z następujących metod w [witryny Azure portal](https://portal.azure.com/):</span><span class="sxs-lookup"><span data-stu-id="02a3c-142">Open Azure Cloud shell using any one of the following methods in the [Azure portal](https://portal.azure.com/):</span></span>

   * <span data-ttu-id="02a3c-143">Wybierz **wypróbuj** w prawym górnym rogu bloku kodu.</span><span class="sxs-lookup"><span data-stu-id="02a3c-143">Select **Try It** in the upper-right corner of a code block.</span></span> <span data-ttu-id="02a3c-144">W polu tekstowym, należy użyć ciągu "Wiersza polecenia platformy Azure".</span><span class="sxs-lookup"><span data-stu-id="02a3c-144">Use the search string "Azure CLI" in the text box.</span></span>
   * <span data-ttu-id="02a3c-145">Otwórz usługę Cloud Shell w przeglądarce za pośrednictwem **Launch Cloud Shell** przycisku.</span><span class="sxs-lookup"><span data-stu-id="02a3c-145">Open Cloud Shell in your browser with the **Launch Cloud Shell** button.</span></span>
   * <span data-ttu-id="02a3c-146">Wybierz **Cloud Shell** przycisk menu w prawym górnym rogu witryny Azure portal.</span><span class="sxs-lookup"><span data-stu-id="02a3c-146">Select the **Cloud Shell** button on the menu in the upper-right corner of the Azure portal.</span></span>

   <span data-ttu-id="02a3c-147">Aby uzyskać więcej informacji, zobacz [interfejsu wiersza polecenia platformy Azure (CLI)](/cli/azure/) i [Omówienie usługi Azure Cloud Shell](/azure/cloud-shell/overview).</span><span class="sxs-lookup"><span data-stu-id="02a3c-147">For more information, see [Azure Command-Line Interface (CLI)](/cli/azure/) and [Overview of Azure Cloud Shell](/azure/cloud-shell/overview).</span></span>

1. <span data-ttu-id="02a3c-148">Jeśli nie są już uwierzytelniony, zaloguj się przy użyciu `az login` polecenia.</span><span class="sxs-lookup"><span data-stu-id="02a3c-148">If you aren't already authenticated, sign in with the `az login` command.</span></span>

1. <span data-ttu-id="02a3c-149">Utwórz grupę zasobów za pomocą następującego polecenia, gdzie `{RESOURCE GROUP NAME}` to nazwa grupy zasobów dla nowej grupy zasobów i `{LOCATION}` to region platformy Azure (datacenter):</span><span class="sxs-lookup"><span data-stu-id="02a3c-149">Create a resource group with the following command, where `{RESOURCE GROUP NAME}` is the resource group name for the new resource group and `{LOCATION}` is the Azure region (datacenter):</span></span>

   ```console
   az group create --name "{RESOURCE GROUP NAME}" --location {LOCATION}
   ```

1. <span data-ttu-id="02a3c-150">Tworzenie magazynu kluczy w grupie zasobów za pomocą następującego polecenia, gdzie `{KEY VAULT NAME}` to nazwa dla nowego magazynu kluczy i `{LOCATION}` to region platformy Azure (datacenter):</span><span class="sxs-lookup"><span data-stu-id="02a3c-150">Create a key vault in the resource group with the following command, where `{KEY VAULT NAME}` is the name for the new key vault and `{LOCATION}` is the Azure region (datacenter):</span></span>

   ```console
   az keyvault create --name "{KEY VAULT NAME}" --resource-group "{RESOURCE GROUP NAME}" --location {LOCATION}
   ```

1. <span data-ttu-id="02a3c-151">Tworzenie wpisów tajnych w magazynie kluczy jako pary nazwa wartość.</span><span class="sxs-lookup"><span data-stu-id="02a3c-151">Create secrets in the key vault as name-value pairs.</span></span>

   <span data-ttu-id="02a3c-152">Usługa Azure Key Vault nazw kluczy tajnych są ograniczone do znaki alfanumeryczne i łączniki.</span><span class="sxs-lookup"><span data-stu-id="02a3c-152">Azure Key Vault secret names are limited to alphanumeric characters and dashes.</span></span> <span data-ttu-id="02a3c-153">Użyj wartości hierarchicznych (sekcji konfiguracji) `--` (dwa łączniki) jako separatora.</span><span class="sxs-lookup"><span data-stu-id="02a3c-153">Hierarchical values (configuration sections) use `--` (two dashes) as a separator.</span></span> <span data-ttu-id="02a3c-154">Dwukropki, które są zwykle używane do ograniczania sekcji w podkluczu w [konfiguracji platformy ASP.NET Core](xref:fundamentals/configuration/index), nie są dozwolone w nazwach wpisu tajnego usługi key vault.</span><span class="sxs-lookup"><span data-stu-id="02a3c-154">Colons, which are normally used to delimit a section from a subkey in [ASP.NET Core configuration](xref:fundamentals/configuration/index), aren't allowed in key vault secret names.</span></span> <span data-ttu-id="02a3c-155">W związku z tym dwa łączniki są używane i zamienione dla dwukropek, gdy wpisy tajne są ładowane do konfiguracji aplikacji.</span><span class="sxs-lookup"><span data-stu-id="02a3c-155">Therefore, two dashes are used and swapped for a colon when the secrets are loaded into the app's configuration.</span></span>

   <span data-ttu-id="02a3c-156">Następujące klucze tajne są do użytku z przykładowej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="02a3c-156">The following secrets are for use with the sample app.</span></span> <span data-ttu-id="02a3c-157">Wartości obejmują `_prod` sufiks, aby odróżnić je od `_dev` sufiks wartości załadowane w środowisku programistycznym z wpisami tajnymi użytkowników.</span><span class="sxs-lookup"><span data-stu-id="02a3c-157">The values include a `_prod` suffix to distinguish them from the `_dev` suffix values loaded in the Development environment from User Secrets.</span></span> <span data-ttu-id="02a3c-158">Zastąp `{KEY VAULT NAME}` nazwą magazynu kluczy, który został utworzony w poprzednim kroku:</span><span class="sxs-lookup"><span data-stu-id="02a3c-158">Replace `{KEY VAULT NAME}` with the name of the key vault that you created in the prior step:</span></span>

   ```console
   az keyvault secret set --vault-name "{KEY VAULT NAME}" --name "SecretName" --value "secret_value_1_prod"
   az keyvault secret set --vault-name "{KEY VAULT NAME}" --name "Section--SecretName" --value "secret_value_2_prod"
   ```

## <a name="use-application-id-and-client-secret-for-non-azure-hosted-apps"></a><span data-ttu-id="02a3c-159">Użyj Identyfikatora aplikacji i klucz tajny klienta dla aplikacji hostowanej platformy Azure</span><span class="sxs-lookup"><span data-stu-id="02a3c-159">Use Application ID and Client Secret for non-Azure-hosted apps</span></span>

<span data-ttu-id="02a3c-160">Konfigurowanie usługi Azure AD, usługa Azure Key Vault i aplikacji na używanie aplikacji, identyfikator i hasło (klucz tajny klienta) do magazynu kluczy w celu uwierzytelniania **gdy aplikacja jest hostowana poza platformą Azure**.</span><span class="sxs-lookup"><span data-stu-id="02a3c-160">Configure Azure AD, Azure Key Vault, and the app to use an Application ID and Password (Client Secret) to authenticate to a key vault **when the app is hosted outside of Azure**.</span></span>

> [!NOTE]
> <span data-ttu-id="02a3c-161">Przy użyciu Identyfikatora aplikacji i hasło (klucz tajny klienta) jest obsługiwana dla aplikacji hostowanych na platformie Azure, zalecamy używanie [zarządzanych tożsamości dla zasobów platformy Azure](#use-managed-identities-for-azure-resources) odnośnie do hostowania aplikacji na platformie Azure.</span><span class="sxs-lookup"><span data-stu-id="02a3c-161">Although using an Application ID and Password (Client Secret) is supported for apps hosted in Azure, we recommend using [Managed identities for Azure resources](#use-managed-identities-for-azure-resources) when hosting an app in Azure.</span></span> <span data-ttu-id="02a3c-162">Zarządzanych tożsamości wymaga przechowywania poświadczeń w aplikacji lub jej konfigurację, dzięki czemu jest traktowany jako ogólnie bezpieczniejszym rozwiązaniem.</span><span class="sxs-lookup"><span data-stu-id="02a3c-162">Managed identities require storing credentials in the app or its configuration, so it's regarded as a generally safer approach.</span></span>

<span data-ttu-id="02a3c-163">Przykładowa aplikacja korzysta z aplikacji, identyfikator i hasło (klucz tajny klienta) po `#define` instrukcji na górze *Program.cs* pliku jest ustawiona na `Basic`.</span><span class="sxs-lookup"><span data-stu-id="02a3c-163">The sample app uses an Application ID and Password (Client Secret) when the `#define` statement at the top of the *Program.cs* file is set to `Basic`.</span></span>

1. <span data-ttu-id="02a3c-164">Rejestrowanie aplikacji w usłudze Azure AD i ustanowić hasło (klucz tajny klienta) dla tożsamości aplikacji.</span><span class="sxs-lookup"><span data-stu-id="02a3c-164">Register the app with Azure AD and establish a Password (Client Secret) for the app's identity.</span></span>
1. <span data-ttu-id="02a3c-165">Store nazwa magazynu kluczy, identyfikator aplikacji i hasło/klucz tajny aplikacji *appsettings.json* pliku.</span><span class="sxs-lookup"><span data-stu-id="02a3c-165">Store the key vault name, Application ID, and Password/Client Secret in the app's *appsettings.json* file.</span></span>
1. <span data-ttu-id="02a3c-166">Przejdź do **klucza magazynów** w witrynie Azure portal.</span><span class="sxs-lookup"><span data-stu-id="02a3c-166">Navigate to **Key vaults** in the Azure portal.</span></span>
1. <span data-ttu-id="02a3c-167">Wybierz magazyn kluczy, który został utworzony w [wpisu tajnego magazynu w środowisku produkcyjnym za pomocą usługi Azure Key Vault](#secret-storage-in-the-production-environment-with-azure-key-vault) sekcji.</span><span class="sxs-lookup"><span data-stu-id="02a3c-167">Select the key vault that you created in the [Secret storage in the Production environment with Azure Key Vault](#secret-storage-in-the-production-environment-with-azure-key-vault) section.</span></span>
1. <span data-ttu-id="02a3c-168">Wybierz **zasady dostępu**.</span><span class="sxs-lookup"><span data-stu-id="02a3c-168">Select **Access policies**.</span></span>
1. <span data-ttu-id="02a3c-169">Wybierz **Dodaj nowe**.</span><span class="sxs-lookup"><span data-stu-id="02a3c-169">Select **Add new**.</span></span>
1. <span data-ttu-id="02a3c-170">Wybierz **Wybierz podmiot zabezpieczeń** i wybierz zarejestrowany aplikację według nazwy.</span><span class="sxs-lookup"><span data-stu-id="02a3c-170">Select **Select principal** and select the registered app by name.</span></span> <span data-ttu-id="02a3c-171">Wybierz **wybierz** przycisku.</span><span class="sxs-lookup"><span data-stu-id="02a3c-171">Select the **Select** button.</span></span>
1. <span data-ttu-id="02a3c-172">Otwórz **uprawnienia klucza tajnego** i zapewniają aplikacji za pomocą **uzyskać** i **listy** uprawnienia.</span><span class="sxs-lookup"><span data-stu-id="02a3c-172">Open **Secret permissions** and provide the app with **Get** and **List** permissions.</span></span>
1. <span data-ttu-id="02a3c-173">Kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="02a3c-173">Select **OK**.</span></span>
1. <span data-ttu-id="02a3c-174">Wybierz pozycję **Zapisz**.</span><span class="sxs-lookup"><span data-stu-id="02a3c-174">Select **Save**.</span></span>
1. <span data-ttu-id="02a3c-175">Wdróż aplikację.</span><span class="sxs-lookup"><span data-stu-id="02a3c-175">Deploy the app.</span></span>

<span data-ttu-id="02a3c-176">`Basic` Przykładowa aplikacja uzyskuje jego wartości konfiguracji z `IConfigurationRoot` o takiej samej nazwie jak nazwa wpisu tajnego:</span><span class="sxs-lookup"><span data-stu-id="02a3c-176">The `Basic` sample app obtains its configuration values from `IConfigurationRoot` with the same name as the secret name:</span></span>

* <span data-ttu-id="02a3c-177">Wartości inne niż hierarchiczne: Wartość `SecretName` są uzyskiwane z `config["SecretName"]`.</span><span class="sxs-lookup"><span data-stu-id="02a3c-177">Non-hierarchical values: The value for `SecretName` is obtained with `config["SecretName"]`.</span></span>
* <span data-ttu-id="02a3c-178">Hierarchiczna wartości (sekcje): Użyj `:` notacji (dwukropek) lub `GetSection` — metoda rozszerzenia.</span><span class="sxs-lookup"><span data-stu-id="02a3c-178">Hierarchical values (sections): Use `:` (colon) notation or the `GetSection` extension method.</span></span> <span data-ttu-id="02a3c-179">Użyj jednej z tych metod można uzyskać wartości konfiguracji:</span><span class="sxs-lookup"><span data-stu-id="02a3c-179">Use either of these approaches to obtain the configuration value:</span></span>
  * `config["Section:SecretName"]`
  * `config.GetSection("Section")["SecretName"]`

<span data-ttu-id="02a3c-180">Wywołania aplikacji `AddAzureKeyVault` przy użyciu wartości dostarczone przez *appsettings.json* pliku:</span><span class="sxs-lookup"><span data-stu-id="02a3c-180">The app calls `AddAzureKeyVault` with values supplied by the *appsettings.json* file:</span></span>

[!code-csharp[](key-vault-configuration/sample/Program.cs?name=snippet1&highlight=11-14)]

<span data-ttu-id="02a3c-181">Przykładowe wartości:</span><span class="sxs-lookup"><span data-stu-id="02a3c-181">Example values:</span></span>

* <span data-ttu-id="02a3c-182">Nazwa magazynu kluczy: `contosovault`</span><span class="sxs-lookup"><span data-stu-id="02a3c-182">Key vault name: `contosovault`</span></span>
* <span data-ttu-id="02a3c-183">Identyfikator aplikacji: `627e911e-43cc-61d4-992e-12db9c81b413`</span><span class="sxs-lookup"><span data-stu-id="02a3c-183">Application ID: `627e911e-43cc-61d4-992e-12db9c81b413`</span></span>
* <span data-ttu-id="02a3c-184">Hasło: `g58K3dtg59o1Pa+e59v2Tx829w6VxTB2yv9sv/101di=`</span><span class="sxs-lookup"><span data-stu-id="02a3c-184">Password: `g58K3dtg59o1Pa+e59v2Tx829w6VxTB2yv9sv/101di=`</span></span>

<span data-ttu-id="02a3c-185">*appsettings.json*:</span><span class="sxs-lookup"><span data-stu-id="02a3c-185">*appsettings.json*:</span></span>

[!code-json[](key-vault-configuration/sample/appsettings.json)]

<span data-ttu-id="02a3c-186">Po uruchomieniu aplikacji, strony sieci Web wyświetlane są załadowane wartości klucza tajnego.</span><span class="sxs-lookup"><span data-stu-id="02a3c-186">When you run the app, a webpage shows the loaded secret values.</span></span> <span data-ttu-id="02a3c-187">W środowisku programistycznym wartościami wpisów tajnych załadować dane przy użyciu `_dev` sufiks.</span><span class="sxs-lookup"><span data-stu-id="02a3c-187">In the Development environment, secret values load with the `_dev` suffix.</span></span> <span data-ttu-id="02a3c-188">W środowisku produkcyjnym wartości załadować dane przy użyciu `_prod` sufiks.</span><span class="sxs-lookup"><span data-stu-id="02a3c-188">In the Production environment, the values load with the `_prod` suffix.</span></span>

## <a name="use-managed-identities-for-azure-resources"></a><span data-ttu-id="02a3c-189">Użyj tożsamości zarządzanych zasobów platformy Azure</span><span class="sxs-lookup"><span data-stu-id="02a3c-189">Use Managed identities for Azure resources</span></span>

<span data-ttu-id="02a3c-190">**Aplikacja wdrożona na platformie Azure** korzystać z zalet [zarządzanych tożsamości dla zasobów platformy Azure](/azure/active-directory/managed-identities-azure-resources/overview), co pozwala aplikacji na uwierzytelnianie za pomocą usługi Azure Key Vault przy użyciu uwierzytelniania usługi Azure AD bez poświadczeń (identyfikator aplikacji i Klucz tajny Password/Client) przechowywanych w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="02a3c-190">**An app deployed to Azure** can take advantage of [Managed identities for Azure resources](/azure/active-directory/managed-identities-azure-resources/overview), which allows the app to authenticate with Azure Key Vault using Azure AD authentication without credentials (Application ID and Password/Client Secret) stored in the app.</span></span>

<span data-ttu-id="02a3c-191">Przykładowa aplikacja korzysta z tożsamości zarządzanego dla zasobów platformy Azure po `#define` instrukcji na górze *Program.cs* pliku jest ustawiona na `Managed`.</span><span class="sxs-lookup"><span data-stu-id="02a3c-191">The sample app uses Managed identities for Azure resources when the `#define` statement at the top of the *Program.cs* file is set to `Managed`.</span></span>

<span data-ttu-id="02a3c-192">Wprowadź nazwę magazynu z aplikacją *appsettings.json* pliku.</span><span class="sxs-lookup"><span data-stu-id="02a3c-192">Enter the vault name into the app's *appsettings.json* file.</span></span> <span data-ttu-id="02a3c-193">Przykładowa aplikacja nie wymaga Identyfikatora aplikacji i hasło (klucz tajny klienta), po ustawieniu `Managed` wersji, dzięki czemu można zignorować te pozycje konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="02a3c-193">The sample app doesn't require an Application ID and Password (Client Secret) when set to the `Managed` version, so you can ignore those configuration entries.</span></span> <span data-ttu-id="02a3c-194">Aplikacja jest wdrożona na platformie Azure, a platforma Azure uwierzytelnia aplikacji dostępu do usługi Azure Key Vault tylko przy użyciu nazwy magazynu przechowywanych w *appsettings.json* pliku.</span><span class="sxs-lookup"><span data-stu-id="02a3c-194">The app is deployed to Azure, and Azure authenticates the app to access Azure Key Vault only using the vault name stored in the *appsettings.json* file.</span></span>

<span data-ttu-id="02a3c-195">Wdróż przykładową aplikację w usłudze Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="02a3c-195">Deploy the sample app to Azure App Service.</span></span>

<span data-ttu-id="02a3c-196">Wdrożenie aplikacji w usłudze Azure App Service jest automatycznie rejestrowane w usłudze Azure AD podczas tworzenia usługi.</span><span class="sxs-lookup"><span data-stu-id="02a3c-196">An app deployed to Azure App Service is automatically registered with Azure AD when the service is created.</span></span> <span data-ttu-id="02a3c-197">Uzyskaj identyfikator obiektu z wdrożenia do użycia w następującym poleceniu.</span><span class="sxs-lookup"><span data-stu-id="02a3c-197">Obtain the Object ID from the deployment for use in the following command.</span></span> <span data-ttu-id="02a3c-198">Identyfikator obiektu są wyświetlane w witrynie Azure portal na **tożsamości** panelu usługi App Service.</span><span class="sxs-lookup"><span data-stu-id="02a3c-198">The Object ID is shown in the Azure portal on the **Identity** panel of the App Service.</span></span>

<span data-ttu-id="02a3c-199">Przy użyciu wiersza polecenia platformy Azure i identyfikator obiektu aplikacji, zapewnić aplikacji za pomocą `list` i `get` uprawnienia do dostępu do magazynu kluczy:</span><span class="sxs-lookup"><span data-stu-id="02a3c-199">Using Azure CLI and the app's Object ID, provide the app with `list` and `get` permissions to access the key vault:</span></span>

```console
az keyvault set-policy --name '{KEY VAULT NAME}' --object-id {OBJECT ID} --secret-permissions get list
```

<span data-ttu-id="02a3c-200">**Uruchom ponownie aplikację** przy użyciu wiersza polecenia platformy Azure, programu PowerShell lub witryny Azure portal.</span><span class="sxs-lookup"><span data-stu-id="02a3c-200">**Restart the app** using Azure CLI, PowerShell, or the Azure portal.</span></span>

<span data-ttu-id="02a3c-201">Przykładowa aplikacja:</span><span class="sxs-lookup"><span data-stu-id="02a3c-201">The sample app:</span></span>

* <span data-ttu-id="02a3c-202">Tworzy wystąpienie `AzureServiceTokenProvider` klasy bez parametrów połączenia.</span><span class="sxs-lookup"><span data-stu-id="02a3c-202">Creates an instance of the `AzureServiceTokenProvider` class without a connection string.</span></span> <span data-ttu-id="02a3c-203">Gdy parametry połączenia nie jest podany, dostawca próbuje uzyskać token dostępu z tożsamości zarządzanego dla zasobów platformy Azure.</span><span class="sxs-lookup"><span data-stu-id="02a3c-203">When a connection string isn't provided, the provider attempts to obtain an access token from Managed identities for Azure resources.</span></span>
* <span data-ttu-id="02a3c-204">Nowy `KeyVaultClient` jest tworzona przy użyciu `AzureServiceTokenProvider` wystąpienie tokenu wywołania zwrotnego.</span><span class="sxs-lookup"><span data-stu-id="02a3c-204">A new `KeyVaultClient` is created with the `AzureServiceTokenProvider` instance token callback.</span></span>
* <span data-ttu-id="02a3c-205">`KeyVaultClient` Wystąpienie jest używane z domyślną implementację elementu `IKeyVaultSecretManager` , ładuje wszystkie wartości klucza tajnego i zastępuje kresek podwójnej precyzji (`--`) z dwukropkiem (`:`) nazwy kluczy.</span><span class="sxs-lookup"><span data-stu-id="02a3c-205">The `KeyVaultClient` instance is used with a default implementation of `IKeyVaultSecretManager` that loads all secret values and replaces double-dashes (`--`) with colons (`:`) in key names.</span></span>

[!code-csharp[](key-vault-configuration/sample/Program.cs?name=snippet2&highlight=13-21)]

<span data-ttu-id="02a3c-206">Po uruchomieniu aplikacji, strony sieci Web wyświetlane są załadowane wartości klucza tajnego.</span><span class="sxs-lookup"><span data-stu-id="02a3c-206">When you run the app, a webpage shows the loaded secret values.</span></span> <span data-ttu-id="02a3c-207">W środowisku programistycznym wartościami wpisów tajnych mają `_dev` sufiksu domeny, ponieważ są one udostępniane przez wpisami tajnymi użytkowników.</span><span class="sxs-lookup"><span data-stu-id="02a3c-207">In the Development environment, secret values have the `_dev` suffix because they're provided by User Secrets.</span></span> <span data-ttu-id="02a3c-208">W środowisku produkcyjnym wartości załadować dane przy użyciu `_prod` sufiksu domeny, ponieważ są one udostępniane przez usługę Azure Key Vault.</span><span class="sxs-lookup"><span data-stu-id="02a3c-208">In the Production environment, the values load with the `_prod` suffix because they're provided by Azure Key Vault.</span></span>

<span data-ttu-id="02a3c-209">Jeśli zostanie wyświetlony `Access denied` błąd, upewnij się, że aplikacja jest zarejestrowane w usłudze Azure AD i dostępu do magazynu kluczy.</span><span class="sxs-lookup"><span data-stu-id="02a3c-209">If you receive an `Access denied` error, confirm that the app is registered with Azure AD and provided access to the key vault.</span></span> <span data-ttu-id="02a3c-210">Upewnij się, że został uruchomiony ponownie usługi na platformie Azure.</span><span class="sxs-lookup"><span data-stu-id="02a3c-210">Confirm that you've restarted the service in Azure.</span></span>

## <a name="use-a-key-name-prefix"></a><span data-ttu-id="02a3c-211">Użyj prefiksu nazwy klucza</span><span class="sxs-lookup"><span data-stu-id="02a3c-211">Use a key name prefix</span></span>

<span data-ttu-id="02a3c-212">`AddAzureKeyVault` udostępnia przeciążenie, które akceptuje implementację `IKeyVaultSecretManager`, co pozwala na kontrolowanie sposobu klucza magazynu kluczy tajnych są konwertowane na klucze konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="02a3c-212">`AddAzureKeyVault` provides an overload that accepts an implementation of `IKeyVaultSecretManager`, which allows you to control how key vault secrets are converted into configuration keys.</span></span> <span data-ttu-id="02a3c-213">Na przykład można zaimplementować interfejs służący do ładowania wartościami wpisów tajnych na podstawie wartości prefiks, który podajesz podczas uruchamiania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="02a3c-213">For example, you can implement the interface to load secret values based on a prefix value you provide at app startup.</span></span> <span data-ttu-id="02a3c-214">Dzięki temu można na przykład, aby ładować wpisy tajne w oparciu o wersję aplikacji.</span><span class="sxs-lookup"><span data-stu-id="02a3c-214">This allows you, for example, to load secrets based on the version of the app.</span></span>

> [!WARNING]
> <span data-ttu-id="02a3c-215">Nie używaj prefiksy na wpisy tajne usługi key vault, umieść wpisów tajnych dla wielu aplikacji w tym samym magazynie kluczy lub umieść środowiska wpisy tajne (na przykład *rozwoju* a *produkcji* wpisy tajne) w tej samej Magazyn.</span><span class="sxs-lookup"><span data-stu-id="02a3c-215">Don't use prefixes on key vault secrets to place secrets for multiple apps into the same key vault or to place environmental secrets (for example, *development* versus *production* secrets) into the same vault.</span></span> <span data-ttu-id="02a3c-216">Zalecane jest różnych aplikacji i środowisk tworzenia/produkcyjnych użycie oddzielnych magazynów kluczy do izolowania aplikacji środowiska na najwyższy poziom zabezpieczeń.</span><span class="sxs-lookup"><span data-stu-id="02a3c-216">We recommend that different apps and development/production environments use separate key vaults to isolate app environments for the highest level of security.</span></span>

<span data-ttu-id="02a3c-217">W poniższym przykładzie wpisu tajnego jest określana w klucza magazynu (i przy użyciu narzędzia menedżera klucz tajny dla środowiska programistycznego) dla `5000-AppSecret` (kropki nie są dozwolone w nazwach wpisu tajnego usługi key vault).</span><span class="sxs-lookup"><span data-stu-id="02a3c-217">In the following example, a secret is established in the key vault (and using the Secret Manager tool for the Development environment) for `5000-AppSecret` (periods aren't allowed in key vault secret names).</span></span> <span data-ttu-id="02a3c-218">Ten wpis tajny reprezentuje klucz tajny aplikacji dla 5.0.0.0 wersję aplikacji.</span><span class="sxs-lookup"><span data-stu-id="02a3c-218">This secret represents an app secret for version 5.0.0.0 of the app.</span></span> <span data-ttu-id="02a3c-219">Dla innej wersji aplikacji, 5.1.0.0, klucz tajny jest dodawany do klucza magazynu (i przy użyciu narzędzia menedżera klucz tajny) dla `5100-AppSecret`.</span><span class="sxs-lookup"><span data-stu-id="02a3c-219">For another version of the app, 5.1.0.0, a secret is added to the key vault (and using the Secret Manager tool) for `5100-AppSecret`.</span></span> <span data-ttu-id="02a3c-220">Każda wersja aplikacji ładuje jego wersjonowany wartość wpisu tajnego do jego konfigurację jako `AppSecret`, oddzielającego off wersji podczas ładowania klucza tajnego.</span><span class="sxs-lookup"><span data-stu-id="02a3c-220">Each app version loads its versioned secret value into its configuration as `AppSecret`, stripping off the version as it loads the secret.</span></span>

<span data-ttu-id="02a3c-221">`AddAzureKeyVault` jest wywoływana z niestandardowego `IKeyVaultSecretManager`:</span><span class="sxs-lookup"><span data-stu-id="02a3c-221">`AddAzureKeyVault` is called with a custom `IKeyVaultSecretManager`:</span></span>

[!code-csharp[](key-vault-configuration/sample_snapshot/Program.cs?name=snippet1&highlight=22)]

<span data-ttu-id="02a3c-222">Wartości dla nazwy usługi key vault, identyfikator aplikacji i hasło (klucz tajny klienta) są dostarczane przez *appsettings.json* pliku:</span><span class="sxs-lookup"><span data-stu-id="02a3c-222">The values for key vault name, Application ID, and Password (Client Secret) are provided by the *appsettings.json* file:</span></span>

[!code-json[](key-vault-configuration/sample/appsettings.json)]

<span data-ttu-id="02a3c-223">Przykładowe wartości:</span><span class="sxs-lookup"><span data-stu-id="02a3c-223">Example values:</span></span>

* <span data-ttu-id="02a3c-224">Nazwa magazynu kluczy: `contosovault`</span><span class="sxs-lookup"><span data-stu-id="02a3c-224">Key vault name: `contosovault`</span></span>
* <span data-ttu-id="02a3c-225">Identyfikator aplikacji: `627e911e-43cc-61d4-992e-12db9c81b413`</span><span class="sxs-lookup"><span data-stu-id="02a3c-225">Application ID: `627e911e-43cc-61d4-992e-12db9c81b413`</span></span>
* <span data-ttu-id="02a3c-226">Hasło: `g58K3dtg59o1Pa+e59v2Tx829w6VxTB2yv9sv/101di=`</span><span class="sxs-lookup"><span data-stu-id="02a3c-226">Password: `g58K3dtg59o1Pa+e59v2Tx829w6VxTB2yv9sv/101di=`</span></span>

<span data-ttu-id="02a3c-227">`IKeyVaultSecretManager` Implementacji reaguje na prefiksy wersji wpisów tajnych można załadować odpowiedniego wpisu tajnego do konfiguracji:</span><span class="sxs-lookup"><span data-stu-id="02a3c-227">The `IKeyVaultSecretManager` implementation reacts to the version prefixes of secrets to load the proper secret into configuration:</span></span>

[!code-csharp[](key-vault-configuration/sample_snapshot/Startup.cs?name=snippet1)]

<span data-ttu-id="02a3c-228">`Load` Metoda jest wywoływana przez algorytm dostawcy, który iteruje po magazyn kluczy tajnych wyszukać te, które mają prefiks wersji.</span><span class="sxs-lookup"><span data-stu-id="02a3c-228">The `Load` method is called by a provider algorithm that iterates through the vault secrets to find the ones that have the version prefix.</span></span> <span data-ttu-id="02a3c-229">Gdy prefiks wersji znajduje się za pomocą `Load`, algorytm używa `GetKey` metodę, aby zwrócić nazwę konfiguracji nazwa wpisu tajnego.</span><span class="sxs-lookup"><span data-stu-id="02a3c-229">When a version prefix is found with `Load`, the algorithm uses the `GetKey` method to return the configuration name of the secret name.</span></span> <span data-ttu-id="02a3c-230">On paski Wyłącz prefiks wersji od nazwy klucza tajnego i zwraca pozostałe Nazwa wpisu tajnego do załadowania do konfiguracji aplikacji pary nazwa wartość.</span><span class="sxs-lookup"><span data-stu-id="02a3c-230">It strips off the version prefix from the secret's name and returns the rest of the secret name for loading into the app's configuration name-value pairs.</span></span>

<span data-ttu-id="02a3c-231">Gdy ta metoda jest zaimplementowana:</span><span class="sxs-lookup"><span data-stu-id="02a3c-231">When this approach is implemented:</span></span>

1. <span data-ttu-id="02a3c-232">Wersja aplikacji określony w pliku projektu aplikacji.</span><span class="sxs-lookup"><span data-stu-id="02a3c-232">The app's version specified in the app's project file.</span></span> <span data-ttu-id="02a3c-233">W poniższym przykładzie ustawiono wersję aplikacji `5.0.0.0`:</span><span class="sxs-lookup"><span data-stu-id="02a3c-233">In the following example, the app's version is set to `5.0.0.0`:</span></span>

   ```xml
   <PropertyGroup>
     <Version>5.0.0.0</Version>
   </PropertyGroup>
   ```

1. <span data-ttu-id="02a3c-234">Upewnij się, że `<UserSecretsId>` właściwość znajduje się w pliku projektu aplikacji, gdzie `{GUID}` jest identyfikatorem GUID, dostarczone przez użytkownika:</span><span class="sxs-lookup"><span data-stu-id="02a3c-234">Confirm that a `<UserSecretsId>` property is present in the app's project file, where `{GUID}` is a user-supplied GUID:</span></span>

   ```xml
   <PropertyGroup>
     <UserSecretsId>{GUID}</UserSecretsId>
   </PropertyGroup>
   ```

   <span data-ttu-id="02a3c-235">Zapisz następujące wpisy tajne lokalnie za pomocą [narzędzie Menedżer klucz tajny](xref:security/app-secrets):</span><span class="sxs-lookup"><span data-stu-id="02a3c-235">Save the following secrets locally with the [Secret Manager tool](xref:security/app-secrets):</span></span>

   ```console
   dotnet user-secrets set "5000-AppSecret" "5.0.0.0_secret_value_dev"
   dotnet user-secrets set "5100-AppSecret" "5.1.0.0_secret_value_dev"
   ```

1. <span data-ttu-id="02a3c-236">Klucze tajne są zapisywane w usłudze Azure Key Vault przy użyciu następujących poleceń interfejsu wiersza polecenia platformy Azure:</span><span class="sxs-lookup"><span data-stu-id="02a3c-236">Secrets are saved in Azure Key Vault using the following Azure CLI commands:</span></span>

   ```console
   az keyvault secret set --vault-name "{KEY VAULT NAME}" --name "5000-AppSecret" --value "5.0.0.0_secret_value_prod"
   az keyvault secret set --vault-name "{KEY VAULT NAME}" --name "5100-AppSecret" --value "5.1.0.0_secret_value_prod"
   ```

1. <span data-ttu-id="02a3c-237">Gdy aplikacja jest uruchamiana, wpisy tajne usługi key vault są ładowane.</span><span class="sxs-lookup"><span data-stu-id="02a3c-237">When the app is run, the key vault secrets are loaded.</span></span> <span data-ttu-id="02a3c-238">Klucz tajny ciągu dla `5000-AppSecret` pasuje do wersji aplikacji określonej w pliku projektu aplikacji (`5.0.0.0`).</span><span class="sxs-lookup"><span data-stu-id="02a3c-238">The string secret for `5000-AppSecret` is matched to the app's version specified in the app's project file (`5.0.0.0`).</span></span>

1. <span data-ttu-id="02a3c-239">Wersja, `5000` (przy użyciu dash), jest usuwany z nazwą klucza.</span><span class="sxs-lookup"><span data-stu-id="02a3c-239">The version, `5000` (with the dash), is stripped from the key name.</span></span> <span data-ttu-id="02a3c-240">W całej aplikacji odczytywanie konfiguracji przy użyciu klucza `AppSecret` ładuje wartość wpisu tajnego.</span><span class="sxs-lookup"><span data-stu-id="02a3c-240">Throughout the app, reading configuration with the key `AppSecret` loads the secret value.</span></span>

1. <span data-ttu-id="02a3c-241">Jeśli wersja aplikacji została zmieniona w pliku projektu do `5.1.0.0` i aplikacja jest uruchamiana ponownie, jest zwracana wartość wpisu tajnego `5.1.0.0_secret_value_dev` w środowisku deweloperskim i `5.1.0.0_secret_value_prod` w środowisku produkcyjnym.</span><span class="sxs-lookup"><span data-stu-id="02a3c-241">If the app's version is changed in the project file to `5.1.0.0` and the app is run again, the secret value returned is `5.1.0.0_secret_value_dev` in the Development environment and `5.1.0.0_secret_value_prod` in Production.</span></span>

> [!NOTE]
> <span data-ttu-id="02a3c-242">Możesz również podać własne `KeyVaultClient` implementacji `AddAzureKeyVault`.</span><span class="sxs-lookup"><span data-stu-id="02a3c-242">You can also provide your own `KeyVaultClient` implementation to `AddAzureKeyVault`.</span></span> <span data-ttu-id="02a3c-243">Klient niestandardowy umożliwia udostępnianie jedno wystąpienie klienta w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="02a3c-243">A custom client permits sharing a single instance of the client across the app.</span></span>

## <a name="authenticate-to-azure-key-vault-with-an-x509-certificate"></a><span data-ttu-id="02a3c-244">Uwierzytelnianie usługi Azure Key Vault za pomocą certyfikatu X.509</span><span class="sxs-lookup"><span data-stu-id="02a3c-244">Authenticate to Azure Key Vault with an X.509 certificate</span></span>

<span data-ttu-id="02a3c-245">Podczas tworzenia aplikacji programu .NET Framework w środowisku, które obsługuje certyfikaty, można wybrać metodę uwierzytelniania usługi Azure Key Vault za pomocą certyfikatu X.509.</span><span class="sxs-lookup"><span data-stu-id="02a3c-245">When developing a .NET Framework app in an environment that supports certificates, you can authenticate to Azure Key Vault with an X.509 certificate.</span></span> <span data-ttu-id="02a3c-246">Klucz prywatny certyfikatu X.509 jest zarządzane przez system operacyjny.</span><span class="sxs-lookup"><span data-stu-id="02a3c-246">The X.509 certificate's private key is managed by the OS.</span></span> <span data-ttu-id="02a3c-247">Aby uzyskać więcej informacji, zobacz [uwierzytelnianie przy użyciu certyfikatu zamiast wpisu tajnego klienta](/azure/key-vault/key-vault-use-from-web-application#authenticate-with-a-certificate-instead-of-a-client-secret).</span><span class="sxs-lookup"><span data-stu-id="02a3c-247">For more information, see [Authenticate with a Certificate instead of a Client Secret](/azure/key-vault/key-vault-use-from-web-application#authenticate-with-a-certificate-instead-of-a-client-secret).</span></span> <span data-ttu-id="02a3c-248">Użyj `AddAzureKeyVault` przeciążenie, które akceptuje `X509Certificate2` (`_env` w następującym przykładzie:</span><span class="sxs-lookup"><span data-stu-id="02a3c-248">Use the `AddAzureKeyVault` overload that accepts an `X509Certificate2` (`_env` in the following example :</span></span>

```csharp
var builtConfig = config.Build();

var store = new X509Store(StoreLocation.CurrentUser);
store.Open(OpenFlags.ReadOnly);
var cert = store.Certificates
    .Find(X509FindType.FindByThumbprint, 
        config["CertificateThumbprint"], false);

config.AddAzureKeyVault(
    builtConfig["Vault"],
    builtConfig["ClientId"],
    cert.OfType<X509Certificate2>().Single(),
    new EnvironmentSecretManager(context.HostingEnvironment.ApplicationName));

store.Close();
```

## <a name="bind-an-array-to-a-class"></a><span data-ttu-id="02a3c-249">Powiąż tablicę do klasy</span><span class="sxs-lookup"><span data-stu-id="02a3c-249">Bind an array to a class</span></span>

<span data-ttu-id="02a3c-250">Dostawca jest zdolny do odczytywania wartości konfiguracji do tablicy, do powiązania z macierzą POCO.</span><span class="sxs-lookup"><span data-stu-id="02a3c-250">The provider is capable of reading configuration values into an array for binding to a POCO array.</span></span>

<span data-ttu-id="02a3c-251">Podczas odczytywania ze źródła konfiguracji, która umożliwia klucze, aby zawierać dwukropek (`:`) separatory, liczbowych segment klucza jest używane do odróżniania klucze, które tworzą tablicę (`:0:`, `:1:`,...</span><span class="sxs-lookup"><span data-stu-id="02a3c-251">When reading from a configuration source that allows keys to contain colon (`:`) separators, a numeric key segment is used to distinguish the keys that make up an array (`:0:`, `:1:`, …</span></span> <span data-ttu-id="02a3c-252">`:{n}:`).</span><span class="sxs-lookup"><span data-stu-id="02a3c-252">`:{n}:`).</span></span> <span data-ttu-id="02a3c-253">Aby uzyskać więcej informacji, zobacz [konfiguracji: Powiąż tablicę do klasy](xref:fundamentals/configuration/index#bind-an-array-to-a-class).</span><span class="sxs-lookup"><span data-stu-id="02a3c-253">For more information, see [Configuration: Bind an array to a class](xref:fundamentals/configuration/index#bind-an-array-to-a-class).</span></span>

<span data-ttu-id="02a3c-254">Usługa Azure Key Vault klucze nie można użyć dwukropek jako separatora.</span><span class="sxs-lookup"><span data-stu-id="02a3c-254">Azure Key Vault keys can't use a colon as a separator.</span></span> <span data-ttu-id="02a3c-255">Podejście opisane w tym temacie używa podwójnego kreski (`--`) jako separatora hierarchiczne wartości (sekcji).</span><span class="sxs-lookup"><span data-stu-id="02a3c-255">The approach described in this topic uses double dashes (`--`) as a separator for hierarchical values (sections).</span></span> <span data-ttu-id="02a3c-256">Tablicy klucze są przechowywane w usłudze Azure Key Vault, przy użyciu podwójnych kresek i numeryczne kluczowe segmenty (`--0--`, `--1--`,...</span><span class="sxs-lookup"><span data-stu-id="02a3c-256">Array keys are stored in Azure Key Vault with double dashes and numeric key segments (`--0--`, `--1--`, …</span></span> <span data-ttu-id="02a3c-257">`--{n}--`).</span><span class="sxs-lookup"><span data-stu-id="02a3c-257">`--{n}--`).</span></span>

<span data-ttu-id="02a3c-258">Sprawdź następujące [Serilog](https://serilog.net/) rejestrowania konfigurację dostawcy udostępniane przez plik w formacie JSON.</span><span class="sxs-lookup"><span data-stu-id="02a3c-258">Examine the following [Serilog](https://serilog.net/) logging provider configuration provided by a JSON file.</span></span> <span data-ttu-id="02a3c-259">Istnieją dwa obiektu literały zdefiniowane w `WriteTo` tablica, która odzwierciedla dwóch Serilog *wychwytywanie*, opisywać miejsca docelowe danych wyjściowych rejestrowania:</span><span class="sxs-lookup"><span data-stu-id="02a3c-259">There are two object literals defined in the `WriteTo` array that reflect two Serilog *sinks*, which describe destinations for logging output:</span></span>

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

<span data-ttu-id="02a3c-260">Konfiguracji przedstawionej w poprzednim pliku JSON jest przechowywany w usłudze Azure Key Vault przy użyciu Podwójna kreska (`--`) notacją i numeryczne segmenty:</span><span class="sxs-lookup"><span data-stu-id="02a3c-260">The configuration shown in the preceding JSON file is stored in Azure Key Vault using double dash (`--`) notation and numeric segments:</span></span>

| <span data-ttu-id="02a3c-261">Key</span><span class="sxs-lookup"><span data-stu-id="02a3c-261">Key</span></span> | <span data-ttu-id="02a3c-262">Wartość</span><span class="sxs-lookup"><span data-stu-id="02a3c-262">Value</span></span> |
| --- | ----- |
| `Serilog--WriteTo--0--Name` | `AzureTableStorage` |
| `Serilog--WriteTo--0--Args--storageTableName` | `logs` |
| `Serilog--WriteTo--0--Args--connectionString` | `DefaultEnd...ountKey=Eby8...GMGw==` |
| `Serilog--WriteTo--1--Name` | `AzureDocumentDB` |
| `Serilog--WriteTo--1--Args--endpointUrl` | `https://contoso.documents.azure.com:443` |
| `Serilog--WriteTo--1--Args--authorizationKey` | `Eby8...GMGw==` |

## <a name="reload-secrets"></a><span data-ttu-id="02a3c-263">Załaduj ponownie wpisy tajne</span><span class="sxs-lookup"><span data-stu-id="02a3c-263">Reload secrets</span></span>

<span data-ttu-id="02a3c-264">Klucze tajne są buforowane do momentu `IConfigurationRoot.Reload()` jest wywoływana.</span><span class="sxs-lookup"><span data-stu-id="02a3c-264">Secrets are cached until `IConfigurationRoot.Reload()` is called.</span></span> <span data-ttu-id="02a3c-265">Wygasłe, wyłączona, i zaktualizowanych wpisów tajnych w magazynie kluczy nie są przestrzegane przez aplikację do momentu `Reload` jest wykonywany.</span><span class="sxs-lookup"><span data-stu-id="02a3c-265">Expired, disabled, and updated secrets in the key vault are not respected by the app until `Reload` is executed.</span></span>

```csharp
Configuration.Reload();
```

## <a name="disabled-and-expired-secrets"></a><span data-ttu-id="02a3c-266">Wyłączone lub wygasłe klucze tajne</span><span class="sxs-lookup"><span data-stu-id="02a3c-266">Disabled and expired secrets</span></span>

<span data-ttu-id="02a3c-267">Wyłączone lub wygasłe klucze tajne throw `KeyVaultClientException`.</span><span class="sxs-lookup"><span data-stu-id="02a3c-267">Disabled and expired secrets throw a `KeyVaultClientException`.</span></span> <span data-ttu-id="02a3c-268">Aby zapobiec sytuacji, w której aplikacja zostanie zgłoszony, zastąpić aplikację, lub zaktualizuj wyłączone/wygasły klucz tajny.</span><span class="sxs-lookup"><span data-stu-id="02a3c-268">To prevent your app from throwing, replace your app or update the disabled/expired secret.</span></span>

## <a name="troubleshoot"></a><span data-ttu-id="02a3c-269">Rozwiązywanie problemów</span><span class="sxs-lookup"><span data-stu-id="02a3c-269">Troubleshoot</span></span>

<span data-ttu-id="02a3c-270">Gdy aplikacja zakończy się niepowodzeniem, można załadować konfiguracji przy użyciu dostawcy, komunikat o błędzie zostanie zapisany [infrastruktury platformy ASP.NET Core rejestrowania](xref:fundamentals/logging/index).</span><span class="sxs-lookup"><span data-stu-id="02a3c-270">When the app fails to load configuration using the provider, an error message is written to the [ASP.NET Core Logging infrastructure](xref:fundamentals/logging/index).</span></span> <span data-ttu-id="02a3c-271">Konfiguracja ładowanie uniemożliwia następujące warunki:</span><span class="sxs-lookup"><span data-stu-id="02a3c-271">The following conditions will prevent configuration from loading:</span></span>

* <span data-ttu-id="02a3c-272">Aplikacja nie jest poprawnie skonfigurowane w usłudze Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="02a3c-272">The app isn't configured correctly in Azure Active Directory.</span></span>
* <span data-ttu-id="02a3c-273">Magazyn kluczy nie istnieje w usłudze Azure Key Vault.</span><span class="sxs-lookup"><span data-stu-id="02a3c-273">The key vault doesn't exist in Azure Key Vault.</span></span>
* <span data-ttu-id="02a3c-274">Aplikacji nie jest upoważniony do dostępu do magazynu kluczy.</span><span class="sxs-lookup"><span data-stu-id="02a3c-274">The app isn't authorized to access the key vault.</span></span>
* <span data-ttu-id="02a3c-275">Zasady dostępu nie zawiera `Get` i `List` uprawnienia.</span><span class="sxs-lookup"><span data-stu-id="02a3c-275">The access policy doesn't include `Get` and `List` permissions.</span></span>
* <span data-ttu-id="02a3c-276">W usłudze key vault dane konfiguracji (pary nazwa wartość) niepoprawnie nosi nazwę, Brak, wyłączone lub wygasła.</span><span class="sxs-lookup"><span data-stu-id="02a3c-276">In the key vault, the configuration data (name-value pair) is incorrectly named, missing, disabled, or expired.</span></span>
* <span data-ttu-id="02a3c-277">Aplikacja ma nazwę niewłaściwego magazynu kluczy (`Vault`), identyfikator aplikacji usługi Azure AD (`ClientId`), lub klucza usługi AD Azure (`ClientSecret`).</span><span class="sxs-lookup"><span data-stu-id="02a3c-277">The app has the wrong key vault name (`Vault`), Azure AD App Id (`ClientId`), or Azure AD Key (`ClientSecret`).</span></span>
* <span data-ttu-id="02a3c-278">Klucz usługi Azure AD (`ClientSecret`) wygasł.</span><span class="sxs-lookup"><span data-stu-id="02a3c-278">The Azure AD Key (`ClientSecret`) is expired.</span></span>
* <span data-ttu-id="02a3c-279">Klucz konfiguracji (nazwa) jest niepoprawny w aplikacji dla wartości, które próbujesz załadować.</span><span class="sxs-lookup"><span data-stu-id="02a3c-279">The configuration key (name) is incorrect in the app for the value you're trying to load.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="02a3c-280">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="02a3c-280">Additional resources</span></span>

* <xref:fundamentals/configuration/index>
* [<span data-ttu-id="02a3c-281">Microsoft Azure: Magazyn kluczy</span><span class="sxs-lookup"><span data-stu-id="02a3c-281">Microsoft Azure: Key Vault</span></span>](https://azure.microsoft.com/services/key-vault/)
* [<span data-ttu-id="02a3c-282">Microsoft Azure: Dokumentacja usługi Key Vault</span><span class="sxs-lookup"><span data-stu-id="02a3c-282">Microsoft Azure: Key Vault Documentation</span></span>](/azure/key-vault/)
* [<span data-ttu-id="02a3c-283">Jak Generowanie i przenoszenie chronionego przez moduł HSM kluczy dla usługi Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="02a3c-283">How to generate and transfer HSM-protected keys for Azure Key Vault</span></span>](/azure/key-vault/key-vault-hsm-protected-keys)
* [<span data-ttu-id="02a3c-284">Klasa KeyVaultClient</span><span class="sxs-lookup"><span data-stu-id="02a3c-284">KeyVaultClient Class</span></span>](/dotnet/api/microsoft.azure.keyvault.keyvaultclient)
