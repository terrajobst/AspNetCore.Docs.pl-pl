---
title: Konfigurowanie ochrony danych platformy ASP.NET Core
author: rick-anderson
description: Informacje o sposobie konfigurowania ochrony danych w ASP.NET Core.
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 07/17/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/data-protection/configuration/overview
ms.openlocfilehash: 803b81f5f69496900791ca1d1976f70f8c266f29
ms.sourcegitcommit: 466300d32f8c33e64ee1b419a2cbffe702863cdf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/27/2018
---
# <a name="configure-aspnet-core-data-protection"></a><span data-ttu-id="178ce-103">Konfigurowanie ochrony danych platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="178ce-103">Configure ASP.NET Core Data Protection</span></span>

<span data-ttu-id="178ce-104">przez [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="178ce-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="178ce-105">Po zainicjowaniu systemu ochrony danych dotyczy [ustawienia domyślne](xref:security/data-protection/configuration/default-settings) oparte na środowisku operacyjnym.</span><span class="sxs-lookup"><span data-stu-id="178ce-105">When the Data Protection system is initialized, it applies [default settings](xref:security/data-protection/configuration/default-settings) based on the operational environment.</span></span> <span data-ttu-id="178ce-106">Te ustawienia są zazwyczaj odpowiednie dla aplikacji działających na jednym komputerze.</span><span class="sxs-lookup"><span data-stu-id="178ce-106">These settings are generally appropriate for apps running on a single machine.</span></span> <span data-ttu-id="178ce-107">Istnieją przypadki, w którym deweloper może być konieczna zmiana ustawienia domyślne:</span><span class="sxs-lookup"><span data-stu-id="178ce-107">There are cases where a developer may want to change the default settings:</span></span>

* <span data-ttu-id="178ce-108">Aplikacja zostanie rozmieszczona na wielu komputerach.</span><span class="sxs-lookup"><span data-stu-id="178ce-108">The app is spread across multiple machines.</span></span>
* <span data-ttu-id="178ce-109">Wymaganiami dotyczącymi zgodności.</span><span class="sxs-lookup"><span data-stu-id="178ce-109">For compliance reasons.</span></span>

<span data-ttu-id="178ce-110">W tych sytuacjach systemu ochrony danych oferuje interfejs API konfiguracji zaawansowanych.</span><span class="sxs-lookup"><span data-stu-id="178ce-110">For these scenarios, the Data Protection system offers a rich configuration API.</span></span>

> [!WARNING]
> <span data-ttu-id="178ce-111">Podobnie jak pliki konfiguracji, pierścień klucza ochrony danych powinny być chronione przy użyciu odpowiednich uprawnień.</span><span class="sxs-lookup"><span data-stu-id="178ce-111">Similar to configuration files, the data protection key ring should be protected using appropriate permissions.</span></span> <span data-ttu-id="178ce-112">Można wybrać do szyfrowania kluczy magazynowane, ale to nie uniemożliwiają osobom atakującym tworzenia nowych kluczy.</span><span class="sxs-lookup"><span data-stu-id="178ce-112">You can choose to encrypt keys at rest, but this doesn't prevent attackers from creating new keys.</span></span> <span data-ttu-id="178ce-113">W rezultacie zabezpieczeń aplikacji jest w pełni funkcjonalne.</span><span class="sxs-lookup"><span data-stu-id="178ce-113">Consequently, your app's security is impacted.</span></span> <span data-ttu-id="178ce-114">Lokalizacja magazynu skonfigurowaną do ochrony danych powinien mieć jego dostęp ograniczony do samej siebie, podobnie jak będzie chronić pliki konfiguracji aplikacji.</span><span class="sxs-lookup"><span data-stu-id="178ce-114">The storage location configured with Data Protection should have its access limited to the app itself, similar to the way you would protect configuration files.</span></span> <span data-ttu-id="178ce-115">Na przykład jeśli zdecydujesz się przechowywać pierścień Twojego klucza na dysku, Użyj uprawnień systemu plików.</span><span class="sxs-lookup"><span data-stu-id="178ce-115">For example, if you choose to store your key ring on disk, use file system permissions.</span></span> <span data-ttu-id="178ce-116">Sprawdź tożsamość w której działa aplikacja sieci web ma odczytu, zapisu i tworzenia dostęp do tego katalogu.</span><span class="sxs-lookup"><span data-stu-id="178ce-116">Ensure only the identity under which your web app runs has read, write, and create access to that directory.</span></span> <span data-ttu-id="178ce-117">Jeśli używasz magazynu tabel Azure, tylko aplikacja sieci web powinien mieć możliwość odczytu, zapisu lub tworzenia nowych wpisów w tabeli magazynu itp.</span><span class="sxs-lookup"><span data-stu-id="178ce-117">If you use Azure Table Storage, only the web app should have the ability to read, write, or create new entries in the table store, etc.</span></span>
>
> <span data-ttu-id="178ce-118">Metody rozszerzenia [AddDataProtection](/dotnet/api/microsoft.extensions.dependencyinjection.dataprotectionservicecollectionextensions.adddataprotection) zwraca [IDataProtectionBuilder](/dotnet/api/microsoft.aspnetcore.dataprotection.idataprotectionbuilder).</span><span class="sxs-lookup"><span data-stu-id="178ce-118">The extension method [AddDataProtection](/dotnet/api/microsoft.extensions.dependencyinjection.dataprotectionservicecollectionextensions.adddataprotection) returns an [IDataProtectionBuilder](/dotnet/api/microsoft.aspnetcore.dataprotection.idataprotectionbuilder).</span></span> <span data-ttu-id="178ce-119">`IDataProtectionBuilder` udostępnia metody rozszerzenia, czy użytkownik może łańcuch można skonfigurować ochrony danych opcje.</span><span class="sxs-lookup"><span data-stu-id="178ce-119">`IDataProtectionBuilder` exposes extension methods that you can chain together to configure Data Protection options.</span></span>

::: moniker range=">= aspnetcore-2.1"

## <a name="protectkeyswithazurekeyvault"></a><span data-ttu-id="178ce-120">ProtectKeysWithAzureKeyVault</span><span class="sxs-lookup"><span data-stu-id="178ce-120">ProtectKeysWithAzureKeyVault</span></span>

<span data-ttu-id="178ce-121">Do przechowywania kluczy w [usługi Azure Key Vault](https://azure.microsoft.com/services/key-vault/), skonfigurować system z [ProtectKeysWithAzureKeyVault](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.protectkeyswithazurekeyvault) w `Startup` klasy:</span><span class="sxs-lookup"><span data-stu-id="178ce-121">To store keys in [Azure Key Vault](https://azure.microsoft.com/services/key-vault/), configure the system with [ProtectKeysWithAzureKeyVault](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.protectkeyswithazurekeyvault) in the `Startup` class:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .PersistKeysToAzureBlobStorage(new Uri("<blobUriWithSasToken>"))
        .ProtectKeysWithAzureKeyVault("<keyIdentifier>", "<clientId>", "<clientSecret>");
}
```

<span data-ttu-id="178ce-122">Ustaw lokalizację magazynu pierścień klucza (na przykład [PersistKeysToAzureBlobStorage](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.persistkeystoazureblobstorage)).</span><span class="sxs-lookup"><span data-stu-id="178ce-122">Set the key ring storage location (for example, [PersistKeysToAzureBlobStorage](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.persistkeystoazureblobstorage)).</span></span> <span data-ttu-id="178ce-123">Lokalizacja musi mieć wartość, ponieważ wywołanie `ProtectKeysWithAzureKeyVault` implementuje [IXmlEncryptor](/dotnet/api/microsoft.aspnetcore.dataprotection.xmlencryption.ixmlencryptor) wyłączają ustawienia ochrony danych, w tym klucz pierścień lokalizacji magazynu.</span><span class="sxs-lookup"><span data-stu-id="178ce-123">The location must be set because calling `ProtectKeysWithAzureKeyVault` implements an [IXmlEncryptor](/dotnet/api/microsoft.aspnetcore.dataprotection.xmlencryption.ixmlencryptor) that disables automatic data protection settings, including the key ring storage location.</span></span> <span data-ttu-id="178ce-124">Powyższy przykład używa magazynu obiektów Blob Azure do utrwalenia pierścień klucza.</span><span class="sxs-lookup"><span data-stu-id="178ce-124">The preceding example uses Azure Blob Storage to persist the key ring.</span></span> <span data-ttu-id="178ce-125">Aby uzyskać więcej informacji, zobacz [dostawcy magazynu kluczy: Azure i Redis](xref:security/data-protection/implementation/key-storage-providers#azure-and-redis).</span><span class="sxs-lookup"><span data-stu-id="178ce-125">For more information, see [Key storage providers: Azure and Redis](xref:security/data-protection/implementation/key-storage-providers#azure-and-redis).</span></span> <span data-ttu-id="178ce-126">Można również utrwalić pierścień klucza lokalnie z [PersistKeysToFileSystem](xref:security/data-protection/implementation/key-storage-providers#file-system).</span><span class="sxs-lookup"><span data-stu-id="178ce-126">You can also persist the key ring locally with [PersistKeysToFileSystem](xref:security/data-protection/implementation/key-storage-providers#file-system).</span></span>

<span data-ttu-id="178ce-127">`keyIdentifier` Jest używany do szyfrowania klucza identyfikator klucza magazynu kluczy (na przykład `https://contosokeyvault.vault.azure.net/keys/dataprotection/`).</span><span class="sxs-lookup"><span data-stu-id="178ce-127">The `keyIdentifier` is the key vault key identifier used for key encryption (for example, `https://contosokeyvault.vault.azure.net/keys/dataprotection/`).</span></span>

<span data-ttu-id="178ce-128">`ProtectKeysWithAzureKeyVault` przeciążenia:</span><span class="sxs-lookup"><span data-stu-id="178ce-128">`ProtectKeysWithAzureKeyVault` overloads:</span></span>

* <span data-ttu-id="178ce-129">[ProtectKeysWithAzureKeyVault (IDataProtectionBuilder, KeyVaultClient, ciąg)](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.protectkeyswithazurekeyvault#Microsoft_AspNetCore_DataProtection_AzureDataProtectionBuilderExtensions_ProtectKeysWithAzureKeyVault_Microsoft_AspNetCore_DataProtection_IDataProtectionBuilder_Microsoft_Azure_KeyVault_KeyVaultClient_System_String_) zezwala na używanie [KeyVaultClient](/dotnet/api/microsoft.azure.keyvault.keyvaultclient) Aby włączyć system ochrony danych do użycia magazynu kluczy.</span><span class="sxs-lookup"><span data-stu-id="178ce-129">[ProtectKeysWithAzureKeyVault(IDataProtectionBuilder, KeyVaultClient, String)](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.protectkeyswithazurekeyvault#Microsoft_AspNetCore_DataProtection_AzureDataProtectionBuilderExtensions_ProtectKeysWithAzureKeyVault_Microsoft_AspNetCore_DataProtection_IDataProtectionBuilder_Microsoft_Azure_KeyVault_KeyVaultClient_System_String_) permits the use of a [KeyVaultClient](/dotnet/api/microsoft.azure.keyvault.keyvaultclient) to enable the data protection system to use the key vault.</span></span>
* <span data-ttu-id="178ce-130">[ProtectKeysWithAzureKeyVault (IDataProtectionBuilder, String, String, X509Certificate2)](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.protectkeyswithazurekeyvault#Microsoft_AspNetCore_DataProtection_AzureDataProtectionBuilderExtensions_ProtectKeysWithAzureKeyVault_Microsoft_AspNetCore_DataProtection_IDataProtectionBuilder_System_String_System_String_System_Security_Cryptography_X509Certificates_X509Certificate2_) zezwala na używanie `ClientId` i [X509Certificate](/dotnet/api/system.security.cryptography.x509certificates.x509certificate2) Aby włączyć system ochrony danych do użycia magazynu kluczy.</span><span class="sxs-lookup"><span data-stu-id="178ce-130">[ProtectKeysWithAzureKeyVault(IDataProtectionBuilder, String, String, X509Certificate2)](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.protectkeyswithazurekeyvault#Microsoft_AspNetCore_DataProtection_AzureDataProtectionBuilderExtensions_ProtectKeysWithAzureKeyVault_Microsoft_AspNetCore_DataProtection_IDataProtectionBuilder_System_String_System_String_System_Security_Cryptography_X509Certificates_X509Certificate2_) permits the use of a `ClientId` and [X509Certificate](/dotnet/api/system.security.cryptography.x509certificates.x509certificate2) to enable the data protection system to use the key vault.</span></span>
* <span data-ttu-id="178ce-131">[ProtectKeysWithAzureKeyVault (IDataProtectionBuilder, String, String, String)](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.protectkeyswithazurekeyvault#Microsoft_AspNetCore_DataProtection_AzureDataProtectionBuilderExtensions_ProtectKeysWithAzureKeyVault_Microsoft_AspNetCore_DataProtection_IDataProtectionBuilder_System_String_System_String_System_String_) zezwala na używanie `ClientId` i `ClientSecret` Aby włączyć system ochrony danych do użycia magazynu kluczy.</span><span class="sxs-lookup"><span data-stu-id="178ce-131">[ProtectKeysWithAzureKeyVault(IDataProtectionBuilder, String, String, String)](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.protectkeyswithazurekeyvault#Microsoft_AspNetCore_DataProtection_AzureDataProtectionBuilderExtensions_ProtectKeysWithAzureKeyVault_Microsoft_AspNetCore_DataProtection_IDataProtectionBuilder_System_String_System_String_System_String_) permits the use of a `ClientId` and `ClientSecret` to enable the data protection system to use the key vault.</span></span>

::: moniker-end

## <a name="persistkeystofilesystem"></a><span data-ttu-id="178ce-132">PersistKeysToFileSystem</span><span class="sxs-lookup"><span data-stu-id="178ce-132">PersistKeysToFileSystem</span></span>

<span data-ttu-id="178ce-133">Do przechowywania kluczy w udziale UNC, a nie na *LOCALAPPDATA %* domyślna lokalizacja, skonfigurować system z [PersistKeysToFileSystem](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.persistkeystofilesystem):</span><span class="sxs-lookup"><span data-stu-id="178ce-133">To store keys on a UNC share instead of at the *%LOCALAPPDATA%* default location, configure the system with [PersistKeysToFileSystem](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.persistkeystofilesystem):</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .PersistKeysToFileSystem(new DirectoryInfo(@"\\server\share\directory\"));
}
```

> [!WARNING]
> <span data-ttu-id="178ce-134">Zmiana lokalizacji klucza trwałości, system nie będzie automatycznie szyfruje klucze magazynowane, ponieważ nie wiadomo, czy DPAPI to mechanizm szyfrowania odpowiednie.</span><span class="sxs-lookup"><span data-stu-id="178ce-134">If you change the key persistence location, the system no longer automatically encrypts keys at rest, since it doesn't know whether DPAPI is an appropriate encryption mechanism.</span></span>

## <a name="protectkeyswith"></a><span data-ttu-id="178ce-135">ProtectKeysWith\*</span><span class="sxs-lookup"><span data-stu-id="178ce-135">ProtectKeysWith\*</span></span>

<span data-ttu-id="178ce-136">Możesz skonfigurować system do ochrony kluczy magazynowane wywołując jedną z [ProtectKeysWith\* ](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions) interfejsy API konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="178ce-136">You can configure the system to protect keys at rest by calling any of the [ProtectKeysWith\*](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions) configuration APIs.</span></span> <span data-ttu-id="178ce-137">W poniższym przykładzie, który klucze są przechowywane w udziale UNC i szyfruje te klucze przechowywane przy użyciu określonego certyfikatu X.509 należy wziąć pod uwagę:</span><span class="sxs-lookup"><span data-stu-id="178ce-137">Consider the example below, which stores keys on a UNC share and encrypts those keys at rest with a specific X.509 certificate:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .PersistKeysToFileSystem(new DirectoryInfo(@"\\server\share\directory\"))
        .ProtectKeysWithCertificate("thumbprint");
}
```

<span data-ttu-id="178ce-138">Zobacz [klucza szyfrowania w Rest](xref:security/data-protection/implementation/key-encryption-at-rest) więcej przykładów oraz Omówienie mechanizmów wbudowane szyfrowanie klucza.</span><span class="sxs-lookup"><span data-stu-id="178ce-138">See [Key Encryption At Rest](xref:security/data-protection/implementation/key-encryption-at-rest) for more examples and discussion on the built-in key encryption mechanisms.</span></span>

## <a name="setdefaultkeylifetime"></a><span data-ttu-id="178ce-139">SetDefaultKeyLifetime</span><span class="sxs-lookup"><span data-stu-id="178ce-139">SetDefaultKeyLifetime</span></span>

<span data-ttu-id="178ce-140">Aby skonfigurować system do użycia zamiast domyślnego okresu istnienia klucza 14 dni 90 dni, użyj [SetDefaultKeyLifetime](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setdefaultkeylifetime):</span><span class="sxs-lookup"><span data-stu-id="178ce-140">To configure the system to use a key lifetime of 14 days instead of the default 90 days, use [SetDefaultKeyLifetime](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setdefaultkeylifetime):</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .SetDefaultKeyLifetime(TimeSpan.FromDays(14));
}
```

## <a name="setapplicationname"></a><span data-ttu-id="178ce-141">SetApplicationName</span><span class="sxs-lookup"><span data-stu-id="178ce-141">SetApplicationName</span></span>

<span data-ttu-id="178ce-142">Domyślnie systemu ochrony danych izoluje aplikacje od siebie, nawet jeśli ich współużytkowania tego samego repozytorium klucza fizycznych.</span><span class="sxs-lookup"><span data-stu-id="178ce-142">By default, the Data Protection system isolates apps from one another, even if they're sharing the same physical key repository.</span></span> <span data-ttu-id="178ce-143">Zapobiega to opis ładunków chronionych drugiej strony aplikacje.</span><span class="sxs-lookup"><span data-stu-id="178ce-143">This prevents the apps from understanding each other's protected payloads.</span></span> <span data-ttu-id="178ce-144">Aby udostępnić chronionych ładunków między dwiema aplikacjami, należy użyć [SetApplicationName](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setapplicationname) z taką samą wartość dla każdej aplikacji:</span><span class="sxs-lookup"><span data-stu-id="178ce-144">To share protected payloads between two apps, use [SetApplicationName](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setapplicationname) with the same value for each app:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .SetApplicationName("shared app name");
}
```

## <a name="disableautomatickeygeneration"></a><span data-ttu-id="178ce-145">DisableAutomaticKeyGeneration</span><span class="sxs-lookup"><span data-stu-id="178ce-145">DisableAutomaticKeyGeneration</span></span>

<span data-ttu-id="178ce-146">Scenariusz, w którym nie ma automatycznie wycofanie kluczy (Tworzenie nowych kluczy) zgodnie z ich podejścia wygaśnięcia aplikacji może być.</span><span class="sxs-lookup"><span data-stu-id="178ce-146">You may have a scenario where you don't want an app to automatically roll keys (create new keys) as they approach expiration.</span></span> <span data-ttu-id="178ce-147">Przykładem może być w relacji podstawowe i pomocnicze, gdy tylko głównej aplikacji jest odpowiedzialny za zarządzanie kluczami problemy i dodatkowej aplikacji po prostu ma widoku tylko do odczytu pierścienia klucz aplikacji.</span><span class="sxs-lookup"><span data-stu-id="178ce-147">One example of this might be apps set up in a primary/secondary relationship, where only the primary app is responsible for key management concerns and secondary apps simply have a read-only view of the key ring.</span></span> <span data-ttu-id="178ce-148">Dodatkowej aplikacji można skonfigurować, aby traktować pierścień klucz jako tylko do odczytu przez skonfigurowanie systemu z [DisableAutomaticKeyGeneration](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.disableautomatickeygeneration):</span><span class="sxs-lookup"><span data-stu-id="178ce-148">The secondary apps can be configured to treat the key ring as read-only by configuring the system with [DisableAutomaticKeyGeneration](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.disableautomatickeygeneration):</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .DisableAutomaticKeyGeneration();
}
```

## <a name="per-application-isolation"></a><span data-ttu-id="178ce-149">Na poziomie aplikacji</span><span class="sxs-lookup"><span data-stu-id="178ce-149">Per-application isolation</span></span>

<span data-ttu-id="178ce-150">Gdy systemu ochrony danych jest obsługiwane przez hosta platformy ASP.NET Core, go automatycznie izoluje aplikacje od siebie, nawet jeśli te aplikacje działają w ramach tego samego konta procesu roboczego i korzystają z tej samej materiał klucza głównego.</span><span class="sxs-lookup"><span data-stu-id="178ce-150">When the Data Protection system is provided by an ASP.NET Core host, it automatically isolates apps from one another, even if those apps are running under the same worker process account and are using the same master keying material.</span></span> <span data-ttu-id="178ce-151">Przypomina trochę modyfikator IsolateApps z elementu System.Web w  **\<machineKey >** elementu.</span><span class="sxs-lookup"><span data-stu-id="178ce-151">This is somewhat similar to the IsolateApps modifier from System.Web's **\<machineKey>** element.</span></span>

<span data-ttu-id="178ce-152">Mechanizm izolacji polega na uwzględnieniu każdej aplikacji na komputerze lokalnym jako unikatowy dzierżawy, w związku z tym [interfejsu IDataProtector](/dotnet/api/microsoft.aspnetcore.dataprotection.idataprotector) ścieżką do katalogu głównego dla danej aplikacji obejmują automatycznie Identyfikatora aplikacji jako dyskryminatora.</span><span class="sxs-lookup"><span data-stu-id="178ce-152">The isolation mechanism works by considering each app on the local machine as a unique tenant, thus the [IDataProtector](/dotnet/api/microsoft.aspnetcore.dataprotection.idataprotector) rooted for any given app automatically includes the app ID as a discriminator.</span></span> <span data-ttu-id="178ce-153">Unikatowy identyfikator aplikacji jest dostarczany z jednej z dwóch miejscach:</span><span class="sxs-lookup"><span data-stu-id="178ce-153">The app's unique ID comes from one of two places:</span></span>

1. <span data-ttu-id="178ce-154">Jeśli aplikacja jest obsługiwana w usługach IIS, unikatowy identyfikator jest ścieżki konfiguracji aplikacji.</span><span class="sxs-lookup"><span data-stu-id="178ce-154">If the app is hosted in IIS, the unique identifier is the app's configuration path.</span></span> <span data-ttu-id="178ce-155">Jeśli aplikacja jest wdrażana w środowisku farmy sieci web, ta wartość powinna być stała, przy założeniu, że w środowiskach usług IIS są skonfigurowane podobnie na wszystkich komputerach w farmie sieci web.</span><span class="sxs-lookup"><span data-stu-id="178ce-155">If an app is deployed in a web farm environment, this value should be stable assuming that the IIS environments are configured similarly across all machines in the web farm.</span></span>

2. <span data-ttu-id="178ce-156">Jeśli aplikacja nie jest obsługiwana przez serwer IIS, unikatowy identyfikator jest ścieżkę fizyczną aplikacji.</span><span class="sxs-lookup"><span data-stu-id="178ce-156">If the app isn't hosted in IIS, the unique identifier is the physical path of the app.</span></span>

<span data-ttu-id="178ce-157">Unikatowy identyfikator jest przeznaczona na przetrwanie resetuje &mdash; poszczególnych aplikacji i komputera samego w sobie.</span><span class="sxs-lookup"><span data-stu-id="178ce-157">The unique identifier is designed to survive resets &mdash; both of the individual app and of the machine itself.</span></span>

<span data-ttu-id="178ce-158">Ten mechanizm izolacji przyjęto założenie, aplikacje nie są złośliwe.</span><span class="sxs-lookup"><span data-stu-id="178ce-158">This isolation mechanism assumes that the apps are not malicious.</span></span> <span data-ttu-id="178ce-159">Złośliwe aplikacji zawsze może wpłynąć na inną aplikację do uruchamiania tego samego konta procesu roboczego.</span><span class="sxs-lookup"><span data-stu-id="178ce-159">A malicious app can always impact any other app running under the same worker process account.</span></span> <span data-ttu-id="178ce-160">W udostępnionym środowisku macierzystym których aplikacje są wzajemnie niezaufanych dostawca hostingu powinna przyjmować kroki w celu zapewnienia systemu operacyjnego poziom izolacji między aplikacjami, w tym oddzielanie repozytoria klucza podstawowego w aplikacjach.</span><span class="sxs-lookup"><span data-stu-id="178ce-160">In a shared hosting environment where apps are mutually untrusted, the hosting provider should take steps to ensure OS-level isolation between apps, including separating the apps' underlying key repositories.</span></span>

<span data-ttu-id="178ce-161">Jeśli system ochrony danych nie został podany przez hosta platformy ASP.NET Core (na przykład, jeśli go za pomocą wystąpienia `DataProtectionProvider` specyficzne typu) izolacji aplikacji jest domyślnie wyłączona.</span><span class="sxs-lookup"><span data-stu-id="178ce-161">If the Data Protection system isn't provided by an ASP.NET Core host (for example, if you instantiate it via the `DataProtectionProvider` concrete type) app isolation is disabled by default.</span></span> <span data-ttu-id="178ce-162">Wyłączenie izolacji aplikacji, wszystkie aplikacje obsługiwana przez tego samego materiału klucza można udostępniać ładunków tak długo, jak zapewniają odpowiednią [celów](xref:security/data-protection/consumer-apis/purpose-strings).</span><span class="sxs-lookup"><span data-stu-id="178ce-162">When app isolation is disabled, all apps backed by the same keying material can share payloads as long as they provide the appropriate [purposes](xref:security/data-protection/consumer-apis/purpose-strings).</span></span> <span data-ttu-id="178ce-163">Zapewnienie izolacji aplikacji, w tym środowisku, należy wywołać [SetApplicationName](#setapplicationname) metody konfiguracji obiektu i Podaj unikatową nazwę dla każdej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="178ce-163">To provide app isolation in this environment, call the [SetApplicationName](#setapplicationname) method on the configuration object and provide a unique name for each app.</span></span>

## <a name="changing-algorithms-with-usecryptographicalgorithms"></a><span data-ttu-id="178ce-164">Zmiana algorytmów z UseCryptographicAlgorithms</span><span class="sxs-lookup"><span data-stu-id="178ce-164">Changing algorithms with UseCryptographicAlgorithms</span></span>

<span data-ttu-id="178ce-165">Stos ochrony danych pozwala na zmianę domyślnego algorytmu, używany przez nowo wygenerować kluczy.</span><span class="sxs-lookup"><span data-stu-id="178ce-165">The Data Protection stack allows you to change the default algorithm used by newly-generated keys.</span></span> <span data-ttu-id="178ce-166">Najprostszym sposobem, w tym celu na wywołanie jest [UseCryptographicAlgorithms](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.usecryptographicalgorithms) zwrotne konfiguracji:</span><span class="sxs-lookup"><span data-stu-id="178ce-166">The simplest way to do this is to call [UseCryptographicAlgorithms](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.usecryptographicalgorithms) from the configuration callback:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="178ce-167">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="178ce-167">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
services.AddDataProtection()
    .UseCryptographicAlgorithms(
        new AuthenticatedEncryptorConfiguration()
    {
        EncryptionAlgorithm = EncryptionAlgorithm.AES_256_CBC,
        ValidationAlgorithm = ValidationAlgorithm.HMACSHA256
    });
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="178ce-168">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="178ce-168">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
services.AddDataProtection()
    .UseCryptographicAlgorithms(
        new AuthenticatedEncryptionSettings()
    {
        EncryptionAlgorithm = EncryptionAlgorithm.AES_256_CBC,
        ValidationAlgorithm = ValidationAlgorithm.HMACSHA256
    });
```

---

<span data-ttu-id="178ce-169">Wartość domyślna algorytm szyfrowania to AES-256-CBC, a domyślna ValidationAlgorithm jest HMACSHA256.</span><span class="sxs-lookup"><span data-stu-id="178ce-169">The default EncryptionAlgorithm is AES-256-CBC, and the default ValidationAlgorithm is HMACSHA256.</span></span> <span data-ttu-id="178ce-170">Można ustawić domyślną zasadę przez administratora systemu za pośrednictwem [komputera zasad](xref:security/data-protection/configuration/machine-wide-policy), ale jawnym wywołaniem `UseCryptographicAlgorithms` zastępują zasady domyślne.</span><span class="sxs-lookup"><span data-stu-id="178ce-170">The default policy can be set by a system administrator via a [machine-wide policy](xref:security/data-protection/configuration/machine-wide-policy), but an explicit call to `UseCryptographicAlgorithms` overrides the default policy.</span></span>

<span data-ttu-id="178ce-171">Wywoływanie `UseCryptographicAlgorithms` służy do określania wybranego algorytmu ze wstępnie zdefiniowanej listy wbudowanych.</span><span class="sxs-lookup"><span data-stu-id="178ce-171">Calling `UseCryptographicAlgorithms` allows you to specify the desired algorithm from a predefined built-in list.</span></span> <span data-ttu-id="178ce-172">Nie musisz martwić się o Implementacja algorytmu.</span><span class="sxs-lookup"><span data-stu-id="178ce-172">You don't need to worry about the implementation of the algorithm.</span></span> <span data-ttu-id="178ce-173">W powyższym scenariuszu systemu ochrony danych próbuje użyć implementacji CNG AES, jeśli uruchomiony w systemie Windows.</span><span class="sxs-lookup"><span data-stu-id="178ce-173">In the scenario above, the Data Protection system attempts to use the CNG implementation of AES if running on Windows.</span></span> <span data-ttu-id="178ce-174">W przeciwnym razie go powraca do zarządzanej [System.Security.Cryptography.Aes](/dotnet/api/system.security.cryptography.aes) klasy.</span><span class="sxs-lookup"><span data-stu-id="178ce-174">Otherwise, it falls back to the managed [System.Security.Cryptography.Aes](/dotnet/api/system.security.cryptography.aes) class.</span></span>

<span data-ttu-id="178ce-175">Można ręcznie określić implementację za pośrednictwem wywołania [UseCustomCryptographicAlgorithms](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.usecustomcryptographicalgorithms).</span><span class="sxs-lookup"><span data-stu-id="178ce-175">You can manually specify an implementation via a call to [UseCustomCryptographicAlgorithms](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.usecustomcryptographicalgorithms).</span></span>

> [!TIP]
> <span data-ttu-id="178ce-176">Zmiana algorytmów nie ma wpływu na istniejące klucze w kręgu klucza.</span><span class="sxs-lookup"><span data-stu-id="178ce-176">Changing algorithms doesn't affect existing keys in the key ring.</span></span> <span data-ttu-id="178ce-177">Wpływa tylko na nowo wygenerować kluczy.</span><span class="sxs-lookup"><span data-stu-id="178ce-177">It only affects newly-generated keys.</span></span>

### <a name="specifying-custom-managed-algorithms"></a><span data-ttu-id="178ce-178">Określanie niestandardowych algorytmów zarządzanych</span><span class="sxs-lookup"><span data-stu-id="178ce-178">Specifying custom managed algorithms</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="178ce-179">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="178ce-179">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="178ce-180">Aby określić niestandardowy zarządzany algorytmów, tworzyć [ManagedAuthenticatedEncryptorConfiguration](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.configurationmodel.managedauthenticatedencryptorconfiguration) wystąpienia, który wskazuje typy wdrożenia:</span><span class="sxs-lookup"><span data-stu-id="178ce-180">To specify custom managed algorithms, create a [ManagedAuthenticatedEncryptorConfiguration](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.configurationmodel.managedauthenticatedencryptorconfiguration) instance that points to the implementation types:</span></span>

```csharp
serviceCollection.AddDataProtection()
    .UseCustomCryptographicAlgorithms(
        new ManagedAuthenticatedEncryptorConfiguration()
    {
        // A type that subclasses SymmetricAlgorithm
        EncryptionAlgorithmType = typeof(Aes),

        // Specified in bits
        EncryptionAlgorithmKeySize = 256,

        // A type that subclasses KeyedHashAlgorithm
        ValidationAlgorithmType = typeof(HMACSHA256)
    });
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="178ce-181">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="178ce-181">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="178ce-182">Aby określić niestandardowy zarządzany algorytmów, tworzyć [ManagedAuthenticatedEncryptionSettings](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.managedauthenticatedencryptionsettings) wystąpienia, który wskazuje typy wdrożenia:</span><span class="sxs-lookup"><span data-stu-id="178ce-182">To specify custom managed algorithms, create a [ManagedAuthenticatedEncryptionSettings](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.managedauthenticatedencryptionsettings) instance that points to the implementation types:</span></span>

```csharp
serviceCollection.AddDataProtection()
    .UseCustomCryptographicAlgorithms(
        new ManagedAuthenticatedEncryptionSettings()
    {
        // A type that subclasses SymmetricAlgorithm
        EncryptionAlgorithmType = typeof(Aes),

        // Specified in bits
        EncryptionAlgorithmKeySize = 256,

        // A type that subclasses KeyedHashAlgorithm
        ValidationAlgorithmType = typeof(HMACSHA256)
    });
```

---

<span data-ttu-id="178ce-183">Zazwyczaj \*typ właściwości musi wskazywać na konkretnych, tworzone jako wystąpienia (za pośrednictwem publicznego ctor bez parametrów) implementacje [SymmetricAlgorithm](/dotnet/api/system.security.cryptography.symmetricalgorithm) i [KeyedHashAlgorithm](/dotnet/api/system.security.cryptography.keyedhashalgorithm), ale przypadki specjalne systemu niektórych wartości, takich jak `typeof(Aes)` dla wygody.</span><span class="sxs-lookup"><span data-stu-id="178ce-183">Generally the \*Type properties must point to concrete, instantiable (via a public parameterless ctor) implementations of [SymmetricAlgorithm](/dotnet/api/system.security.cryptography.symmetricalgorithm) and [KeyedHashAlgorithm](/dotnet/api/system.security.cryptography.keyedhashalgorithm), though the system special-cases some values like `typeof(Aes)` for convenience.</span></span>

> [!NOTE]
> <span data-ttu-id="178ce-184">SymmetricAlgorithm musi mieć klucz o długości ≥ 128 bitów i blok o rozmiarze ≥ 64-bitowy, a musi obsługiwać szyfrowania w trybie CBC z dopełnienie PKCS #7.</span><span class="sxs-lookup"><span data-stu-id="178ce-184">The SymmetricAlgorithm must have a key length of ≥ 128 bits and a block size of ≥ 64 bits, and it must support CBC-mode encryption with PKCS #7 padding.</span></span> <span data-ttu-id="178ce-185">KeyedHashAlgorithm musi mieć rozmiar szyfrowanego > = 128 bitów i musi obsługiwać klucze o długości równej długości szyfrowanego algorytmem wyznaczania wartości skrótu.</span><span class="sxs-lookup"><span data-stu-id="178ce-185">The KeyedHashAlgorithm must have a digest size of >= 128 bits, and it must support keys of length equal to the hash algorithm's digest length.</span></span> <span data-ttu-id="178ce-186">KeyedHashAlgorithm nie jest ściśle musi być HMAC.</span><span class="sxs-lookup"><span data-stu-id="178ce-186">The KeyedHashAlgorithm isn't strictly required to be HMAC.</span></span>

### <a name="specifying-custom-windows-cng-algorithms"></a><span data-ttu-id="178ce-187">Określanie niestandardowych algorytmów CNG systemu Windows</span><span class="sxs-lookup"><span data-stu-id="178ce-187">Specifying custom Windows CNG algorithms</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="178ce-188">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="178ce-188">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="178ce-189">Aby określić niestandardowy algorytm CNG systemu Windows przy użyciu szyfrowania w trybie CBC przy weryfikacji HMAC, Utwórz [CngCbcAuthenticatedEncryptorConfiguration](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.configurationmodel.cngcbcauthenticatedencryptorconfiguration) wystąpienia, który zawiera informacje algorytmicznego:</span><span class="sxs-lookup"><span data-stu-id="178ce-189">To specify a custom Windows CNG algorithm using CBC-mode encryption with HMAC validation, create a [CngCbcAuthenticatedEncryptorConfiguration](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.configurationmodel.cngcbcauthenticatedencryptorconfiguration) instance that contains the algorithmic information:</span></span>

```csharp
services.AddDataProtection()
    .UseCustomCryptographicAlgorithms(
        new CngCbcAuthenticatedEncryptorConfiguration()
    {
        // Passed to BCryptOpenAlgorithmProvider
        EncryptionAlgorithm = "AES",
        EncryptionAlgorithmProvider = null,

        // Specified in bits
        EncryptionAlgorithmKeySize = 256,

        // Passed to BCryptOpenAlgorithmProvider
        HashAlgorithm = "SHA256",
        HashAlgorithmProvider = null
    });
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="178ce-190">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="178ce-190">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="178ce-191">Aby określić niestandardowy algorytm CNG systemu Windows przy użyciu szyfrowania w trybie CBC przy weryfikacji HMAC, Utwórz [CngCbcAuthenticatedEncryptionSettings](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.cngcbcauthenticatedencryptionsettings) wystąpienia, który zawiera informacje algorytmicznego:</span><span class="sxs-lookup"><span data-stu-id="178ce-191">To specify a custom Windows CNG algorithm using CBC-mode encryption with HMAC validation, create a [CngCbcAuthenticatedEncryptionSettings](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.cngcbcauthenticatedencryptionsettings) instance that contains the algorithmic information:</span></span>

```csharp
services.AddDataProtection()
    .UseCustomCryptographicAlgorithms(
        new CngCbcAuthenticatedEncryptionSettings()
    {
        // Passed to BCryptOpenAlgorithmProvider
        EncryptionAlgorithm = "AES",
        EncryptionAlgorithmProvider = null,

        // Specified in bits
        EncryptionAlgorithmKeySize = 256,

        // Passed to BCryptOpenAlgorithmProvider
        HashAlgorithm = "SHA256",
        HashAlgorithmProvider = null
    });
```

---

> [!NOTE]
> <span data-ttu-id="178ce-192">Algorytm szyfrowania symetrycznego bloku musi mieć klucz o długości > = 128 bitów — blok o rozmiarze > = 64-bitowy, i musi obsługiwać szyfrowania w trybie CBC z dopełnienie PKCS #7.</span><span class="sxs-lookup"><span data-stu-id="178ce-192">The symmetric block cipher algorithm must have a key length of >= 128 bits, a block size of >= 64 bits, and it must support CBC-mode encryption with PKCS #7 padding.</span></span> <span data-ttu-id="178ce-193">Algorytm wyznaczania wartości skrótu musi mieć rozmiar szyfrowanego > = 128 bitów i musi obsługiwać otwierany z BCRYPT\_ALG\_obsługi\_HMAC\_flagi flagi.</span><span class="sxs-lookup"><span data-stu-id="178ce-193">The hash algorithm must have a digest size of >= 128 bits and must support being opened with the BCRYPT\_ALG\_HANDLE\_HMAC\_FLAG flag.</span></span> <span data-ttu-id="178ce-194">\*Dostawcy właściwości można ustawić wartości null do używania domyślnego dostawcę dla określonego algorytmu.</span><span class="sxs-lookup"><span data-stu-id="178ce-194">The \*Provider properties can be set to null to use the default provider for the specified algorithm.</span></span> <span data-ttu-id="178ce-195">Zobacz [BCryptOpenAlgorithmProvider](https://msdn.microsoft.com/library/windows/desktop/aa375479(v=vs.85).aspx) dokumentacji, aby uzyskać więcej informacji.</span><span class="sxs-lookup"><span data-stu-id="178ce-195">See the [BCryptOpenAlgorithmProvider](https://msdn.microsoft.com/library/windows/desktop/aa375479(v=vs.85).aspx) documentation for more information.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="178ce-196">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="178ce-196">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="178ce-197">Aby określić niestandardowy algorytm CNG systemu Windows przy użyciu szyfrowania tryb Galois liczników ze sprawdzaniem poprawności, Utwórz [CngGcmAuthenticatedEncryptorConfiguration](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.configurationmodel.cnggcmauthenticatedencryptorconfiguration) wystąpienia, który zawiera informacje algorytmicznego:</span><span class="sxs-lookup"><span data-stu-id="178ce-197">To specify a custom Windows CNG algorithm using Galois/Counter Mode encryption with validation, create a [CngGcmAuthenticatedEncryptorConfiguration](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.configurationmodel.cnggcmauthenticatedencryptorconfiguration) instance that contains the algorithmic information:</span></span>

```csharp
services.AddDataProtection()
    .UseCustomCryptographicAlgorithms(
        new CngGcmAuthenticatedEncryptorConfiguration()
    {
        // Passed to BCryptOpenAlgorithmProvider
        EncryptionAlgorithm = "AES",
        EncryptionAlgorithmProvider = null,

        // Specified in bits
        EncryptionAlgorithmKeySize = 256
    });
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="178ce-198">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="178ce-198">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="178ce-199">Aby określić niestandardowy algorytm CNG systemu Windows przy użyciu szyfrowania tryb Galois liczników ze sprawdzaniem poprawności, Utwórz [CngGcmAuthenticatedEncryptionSettings](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.cnggcmauthenticatedencryptionsettings) wystąpienia, który zawiera informacje algorytmicznego:</span><span class="sxs-lookup"><span data-stu-id="178ce-199">To specify a custom Windows CNG algorithm using Galois/Counter Mode encryption with validation, create a [CngGcmAuthenticatedEncryptionSettings](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.cnggcmauthenticatedencryptionsettings) instance that contains the algorithmic information:</span></span>

```csharp
services.AddDataProtection()
    .UseCustomCryptographicAlgorithms(
        new CngGcmAuthenticatedEncryptionSettings()
    {
        // Passed to BCryptOpenAlgorithmProvider
        EncryptionAlgorithm = "AES",
        EncryptionAlgorithmProvider = null,

        // Specified in bits
        EncryptionAlgorithmKeySize = 256
    });
```

---

> [!NOTE]
> <span data-ttu-id="178ce-200">Algorytm szyfrowania symetrycznego bloku musi mieć klucz o długości > = 128 bitów — blok o rozmiarze dokładnie 128 bitów i musi obsługiwać usługi GCM szyfrowania.</span><span class="sxs-lookup"><span data-stu-id="178ce-200">The symmetric block cipher algorithm must have a key length of >= 128 bits, a block size of exactly 128 bits, and it must support GCM encryption.</span></span> <span data-ttu-id="178ce-201">Można ustawić [EncryptionAlgorithmProvider](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.configurationmodel.cngcbcauthenticatedencryptorconfiguration.encryptionalgorithmprovider) właściwości na wartość null, aby użyć domyślnego dostawcy dla określonego algorytmu.</span><span class="sxs-lookup"><span data-stu-id="178ce-201">You can set the [EncryptionAlgorithmProvider](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.configurationmodel.cngcbcauthenticatedencryptorconfiguration.encryptionalgorithmprovider) property to null to use the default provider for the specified algorithm.</span></span> <span data-ttu-id="178ce-202">Zobacz [BCryptOpenAlgorithmProvider](https://msdn.microsoft.com/library/windows/desktop/aa375479(v=vs.85).aspx) dokumentacji, aby uzyskać więcej informacji.</span><span class="sxs-lookup"><span data-stu-id="178ce-202">See the [BCryptOpenAlgorithmProvider](https://msdn.microsoft.com/library/windows/desktop/aa375479(v=vs.85).aspx) documentation for more information.</span></span>

### <a name="specifying-other-custom-algorithms"></a><span data-ttu-id="178ce-203">Określanie inne algorytmy niestandardowych</span><span class="sxs-lookup"><span data-stu-id="178ce-203">Specifying other custom algorithms</span></span>

<span data-ttu-id="178ce-204">Chociaż nie są widoczne jako najwyższej jakości interfejsu API, system ochrony danych jest rozszerzalny, aby umożliwić Określanie niemal każdego rodzaju algorytmu.</span><span class="sxs-lookup"><span data-stu-id="178ce-204">Though not exposed as a first-class API, the Data Protection system is extensible enough to allow specifying almost any kind of algorithm.</span></span> <span data-ttu-id="178ce-205">Na przykład jest to możliwe, aby zachować wszystkich kluczy zawartych w sprzętowego modułu zabezpieczeń (HSM) i do implementacji niestandardowych rdzenia procedury szyfrowania i odszyfrowywania.</span><span class="sxs-lookup"><span data-stu-id="178ce-205">For example, it's possible to keep all keys contained within a Hardware Security Module (HSM) and to provide a custom implementation of the core encryption and decryption routines.</span></span> <span data-ttu-id="178ce-206">Zobacz [IAuthenticatedEncryptor](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.iauthenticatedencryptor) w [podstawowe rozszerzalności kryptografii](xref:security/data-protection/extensibility/core-crypto) Aby uzyskać więcej informacji.</span><span class="sxs-lookup"><span data-stu-id="178ce-206">See [IAuthenticatedEncryptor](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.iauthenticatedencryptor) in [Core cryptography extensibility](xref:security/data-protection/extensibility/core-crypto) for more information.</span></span>

## <a name="persisting-keys-when-hosting-in-a-docker-container"></a><span data-ttu-id="178ce-207">Utrwalanie klucze odnośnie do hostowania w kontenerze Docker</span><span class="sxs-lookup"><span data-stu-id="178ce-207">Persisting keys when hosting in a Docker container</span></span>

<span data-ttu-id="178ce-208">Odnośnie do hostowania w [Docker](/dotnet/standard/microservices-architecture/container-docker-introduction/) kontenera kluczy należy utrzymywać albo:</span><span class="sxs-lookup"><span data-stu-id="178ce-208">When hosting in a [Docker](/dotnet/standard/microservices-architecture/container-docker-introduction/) container, keys should be maintained in either:</span></span>

* <span data-ttu-id="178ce-209">Folder, który jest Docker woluminu, który będzie się powtarzać, poza okres istnienia kontenera, takich jak udostępnionego woluminu lub wolumin zainstalowany w hoście.</span><span class="sxs-lookup"><span data-stu-id="178ce-209">A folder that's a Docker volume that persists beyond the container's lifetime, such as a shared volume or a host-mounted volume.</span></span>
* <span data-ttu-id="178ce-210">Zewnętrznego dostawcy, takich jak [usługi Azure Key Vault](https://azure.microsoft.com/services/key-vault/) lub [Redis](https://redis.io/).</span><span class="sxs-lookup"><span data-stu-id="178ce-210">An external provider, such as [Azure Key Vault](https://azure.microsoft.com/services/key-vault/) or [Redis](https://redis.io/).</span></span>

## <a name="see-also"></a><span data-ttu-id="178ce-211">Zobacz także</span><span class="sxs-lookup"><span data-stu-id="178ce-211">See also</span></span>

* [<span data-ttu-id="178ce-212">Scenariusze pamiętać z systemem innym niż Podpisane</span><span class="sxs-lookup"><span data-stu-id="178ce-212">Non DI Aware Scenarios</span></span>](xref:security/data-protection/configuration/non-di-scenarios)
* [<span data-ttu-id="178ce-213">Międzynarodowe zasad komputera</span><span class="sxs-lookup"><span data-stu-id="178ce-213">Machine Wide Policy</span></span>](xref:security/data-protection/configuration/machine-wide-policy)
