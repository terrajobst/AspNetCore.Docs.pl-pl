---
title: Konfigurowanie ochrony danych programu ASP.NET Core
author: rick-anderson
description: Dowiedz się, jak skonfigurować ochronę danych w programie ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 11/13/2018
uid: security/data-protection/configuration/overview
ms.openlocfilehash: 3be220df4b14ed8dbbd1fab70f46578e9408aa26
ms.sourcegitcommit: f202864efca81a72ea7120c0692940c40d9d0630
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/14/2018
ms.locfileid: "51635319"
---
# <a name="configure-aspnet-core-data-protection"></a><span data-ttu-id="feecc-103">Konfigurowanie ochrony danych programu ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="feecc-103">Configure ASP.NET Core Data Protection</span></span>

<span data-ttu-id="feecc-104">Po zainicjowaniu system ochrony danych dotyczy [domyślne ustawienia](xref:security/data-protection/configuration/default-settings) oparte na środowisku operacyjnym.</span><span class="sxs-lookup"><span data-stu-id="feecc-104">When the Data Protection system is initialized, it applies [default settings](xref:security/data-protection/configuration/default-settings) based on the operational environment.</span></span> <span data-ttu-id="feecc-105">Te ustawienia są zwykle odpowiednie dla aplikacji działających na jednym komputerze.</span><span class="sxs-lookup"><span data-stu-id="feecc-105">These settings are generally appropriate for apps running on a single machine.</span></span> <span data-ttu-id="feecc-106">Istnieją przypadki, w których deweloper może być konieczna zmiana ustawień domyślnych:</span><span class="sxs-lookup"><span data-stu-id="feecc-106">There are cases where a developer may want to change the default settings:</span></span>

* <span data-ttu-id="feecc-107">Aplikacja jest rozłożona się między wieloma maszynami.</span><span class="sxs-lookup"><span data-stu-id="feecc-107">The app is spread across multiple machines.</span></span>
* <span data-ttu-id="feecc-108">W celu zachowania zgodności.</span><span class="sxs-lookup"><span data-stu-id="feecc-108">For compliance reasons.</span></span>

<span data-ttu-id="feecc-109">Dla tych scenariuszy system ochrony danych oferuje interfejs API konfiguracji zaawansowanych.</span><span class="sxs-lookup"><span data-stu-id="feecc-109">For these scenarios, the Data Protection system offers a rich configuration API.</span></span>

> [!WARNING]
> <span data-ttu-id="feecc-110">Podobnie jak w plikach konfiguracji, pierścień klucz ochrony danych powinny być chronione przy użyciu odpowiednich uprawnień.</span><span class="sxs-lookup"><span data-stu-id="feecc-110">Similar to configuration files, the data protection key ring should be protected using appropriate permissions.</span></span> <span data-ttu-id="feecc-111">Istnieje możliwość szyfrowania kluczy w spoczynku, ale to nie uniemożliwiają osobom atakującym tworzenia nowych kluczy.</span><span class="sxs-lookup"><span data-stu-id="feecc-111">You can choose to encrypt keys at rest, but this doesn't prevent attackers from creating new keys.</span></span> <span data-ttu-id="feecc-112">W związku z tym ma wpływ na zabezpieczenia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="feecc-112">Consequently, your app's security is impacted.</span></span> <span data-ttu-id="feecc-113">Lokalizacja magazynu skonfigurowaną z ochroną danych powinny mieć jego dostęp ograniczony do aplikacji, podobnie jak będzie chronić pliki konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="feecc-113">The storage location configured with Data Protection should have its access limited to the app itself, similar to the way you would protect configuration files.</span></span> <span data-ttu-id="feecc-114">Na przykład jeśli chcesz przechowywać swoje pierścień klucza na dysku, Użyj uprawnień systemu plików.</span><span class="sxs-lookup"><span data-stu-id="feecc-114">For example, if you choose to store your key ring on disk, use file system permissions.</span></span> <span data-ttu-id="feecc-115">Upewnij się jedynie tożsamość, której działa aplikacja sieci web ma odczytu, zapisu i tworzenia dostęp do tego katalogu.</span><span class="sxs-lookup"><span data-stu-id="feecc-115">Ensure only the identity under which your web app runs has read, write, and create access to that directory.</span></span> <span data-ttu-id="feecc-116">Jeśli używasz usługi Azure Table Storage, w aplikacji sieci web powinien mieć możliwość odczytu, zapisu lub tworzenia nowych wpisów w tabeli store itp.</span><span class="sxs-lookup"><span data-stu-id="feecc-116">If you use Azure Table Storage, only the web app should have the ability to read, write, or create new entries in the table store, etc.</span></span>
>
> <span data-ttu-id="feecc-117">Metoda rozszerzenia [AddDataProtection](/dotnet/api/microsoft.extensions.dependencyinjection.dataprotectionservicecollectionextensions.adddataprotection) zwraca [IDataProtectionBuilder](/dotnet/api/microsoft.aspnetcore.dataprotection.idataprotectionbuilder).</span><span class="sxs-lookup"><span data-stu-id="feecc-117">The extension method [AddDataProtection](/dotnet/api/microsoft.extensions.dependencyinjection.dataprotectionservicecollectionextensions.adddataprotection) returns an [IDataProtectionBuilder](/dotnet/api/microsoft.aspnetcore.dataprotection.idataprotectionbuilder).</span></span> <span data-ttu-id="feecc-118">`IDataProtectionBuilder` udostępnia metody rozszerzenia, czy można połączyć w łańcuch ze sobą, aby skonfigurować ochronę danych opcje.</span><span class="sxs-lookup"><span data-stu-id="feecc-118">`IDataProtectionBuilder` exposes extension methods that you can chain together to configure Data Protection options.</span></span>

::: moniker range=">= aspnetcore-2.1"

## <a name="protectkeyswithazurekeyvault"></a><span data-ttu-id="feecc-119">ProtectKeysWithAzureKeyVault</span><span class="sxs-lookup"><span data-stu-id="feecc-119">ProtectKeysWithAzureKeyVault</span></span>

<span data-ttu-id="feecc-120">Do przechowywania kluczy w [usługi Azure Key Vault](https://azure.microsoft.com/services/key-vault/), skonfiguruj system z [ProtectKeysWithAzureKeyVault](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.protectkeyswithazurekeyvault) w `Startup` klasy:</span><span class="sxs-lookup"><span data-stu-id="feecc-120">To store keys in [Azure Key Vault](https://azure.microsoft.com/services/key-vault/), configure the system with [ProtectKeysWithAzureKeyVault](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.protectkeyswithazurekeyvault) in the `Startup` class:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .PersistKeysToAzureBlobStorage(new Uri("<blobUriWithSasToken>"))
        .ProtectKeysWithAzureKeyVault("<keyIdentifier>", "<clientId>", "<clientSecret>");
}
```

<span data-ttu-id="feecc-121">Ustaw lokalizację magazynu pierścień klucza (na przykład [PersistKeysToAzureBlobStorage](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.persistkeystoazureblobstorage)).</span><span class="sxs-lookup"><span data-stu-id="feecc-121">Set the key ring storage location (for example, [PersistKeysToAzureBlobStorage](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.persistkeystoazureblobstorage)).</span></span> <span data-ttu-id="feecc-122">Można ustawić lokalizację, ponieważ wywołanie `ProtectKeysWithAzureKeyVault` implementuje [IXmlEncryptor](/dotnet/api/microsoft.aspnetcore.dataprotection.xmlencryption.ixmlencryptor) wyłączają ustawienia ochrony danych, w tym lokalizacji przechowywania klucza pierścienia.</span><span class="sxs-lookup"><span data-stu-id="feecc-122">The location must be set because calling `ProtectKeysWithAzureKeyVault` implements an [IXmlEncryptor](/dotnet/api/microsoft.aspnetcore.dataprotection.xmlencryption.ixmlencryptor) that disables automatic data protection settings, including the key ring storage location.</span></span> <span data-ttu-id="feecc-123">Poprzedni przykład wykorzystuje usługi Azure Blob Storage, aby utrwalić pierścień klucza.</span><span class="sxs-lookup"><span data-stu-id="feecc-123">The preceding example uses Azure Blob Storage to persist the key ring.</span></span> <span data-ttu-id="feecc-124">Aby uzyskać więcej informacji, zobacz [dostawcy magazynu kluczy: Azure i Redis](xref:security/data-protection/implementation/key-storage-providers#azure-and-redis).</span><span class="sxs-lookup"><span data-stu-id="feecc-124">For more information, see [Key storage providers: Azure and Redis](xref:security/data-protection/implementation/key-storage-providers#azure-and-redis).</span></span> <span data-ttu-id="feecc-125">Można również utrwalić pierścień klucz lokalnie za pomocą [PersistKeysToFileSystem](xref:security/data-protection/implementation/key-storage-providers#file-system).</span><span class="sxs-lookup"><span data-stu-id="feecc-125">You can also persist the key ring locally with [PersistKeysToFileSystem](xref:security/data-protection/implementation/key-storage-providers#file-system).</span></span>

<span data-ttu-id="feecc-126">`keyIdentifier` Jest identyfikator klucza magazynu kluczy, używany do szyfrowania klucza (na przykład `https://contosokeyvault.vault.azure.net/keys/dataprotection/`).</span><span class="sxs-lookup"><span data-stu-id="feecc-126">The `keyIdentifier` is the key vault key identifier used for key encryption (for example, `https://contosokeyvault.vault.azure.net/keys/dataprotection/`).</span></span>

<span data-ttu-id="feecc-127">`ProtectKeysWithAzureKeyVault` przeciążenia:</span><span class="sxs-lookup"><span data-stu-id="feecc-127">`ProtectKeysWithAzureKeyVault` overloads:</span></span>

* <span data-ttu-id="feecc-128">[ProtectKeysWithAzureKeyVault (IDataProtectionBuilder, KeyVaultClient, String)](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.protectkeyswithazurekeyvault#Microsoft_AspNetCore_DataProtection_AzureDataProtectionBuilderExtensions_ProtectKeysWithAzureKeyVault_Microsoft_AspNetCore_DataProtection_IDataProtectionBuilder_Microsoft_Azure_KeyVault_KeyVaultClient_System_String_) zezwala na używanie [KeyVaultClient](/dotnet/api/microsoft.azure.keyvault.keyvaultclient) umożliwiające systemu ochrony danych, do korzystania z usługi key vault.</span><span class="sxs-lookup"><span data-stu-id="feecc-128">[ProtectKeysWithAzureKeyVault(IDataProtectionBuilder, KeyVaultClient, String)](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.protectkeyswithazurekeyvault#Microsoft_AspNetCore_DataProtection_AzureDataProtectionBuilderExtensions_ProtectKeysWithAzureKeyVault_Microsoft_AspNetCore_DataProtection_IDataProtectionBuilder_Microsoft_Azure_KeyVault_KeyVaultClient_System_String_) permits the use of a [KeyVaultClient](/dotnet/api/microsoft.azure.keyvault.keyvaultclient) to enable the data protection system to use the key vault.</span></span>
* <span data-ttu-id="feecc-129">[ProtectKeysWithAzureKeyVault (IDataProtectionBuilder, String, String, X509Certificate2)](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.protectkeyswithazurekeyvault#Microsoft_AspNetCore_DataProtection_AzureDataProtectionBuilderExtensions_ProtectKeysWithAzureKeyVault_Microsoft_AspNetCore_DataProtection_IDataProtectionBuilder_System_String_System_String_System_Security_Cryptography_X509Certificates_X509Certificate2_) zezwala na używanie `ClientId` i [X509Certificate](/dotnet/api/system.security.cryptography.x509certificates.x509certificate2) umożliwiające systemu ochrony danych, do korzystania z usługi key vault.</span><span class="sxs-lookup"><span data-stu-id="feecc-129">[ProtectKeysWithAzureKeyVault(IDataProtectionBuilder, String, String, X509Certificate2)](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.protectkeyswithazurekeyvault#Microsoft_AspNetCore_DataProtection_AzureDataProtectionBuilderExtensions_ProtectKeysWithAzureKeyVault_Microsoft_AspNetCore_DataProtection_IDataProtectionBuilder_System_String_System_String_System_Security_Cryptography_X509Certificates_X509Certificate2_) permits the use of a `ClientId` and [X509Certificate](/dotnet/api/system.security.cryptography.x509certificates.x509certificate2) to enable the data protection system to use the key vault.</span></span>
* <span data-ttu-id="feecc-130">[ProtectKeysWithAzureKeyVault (IDataProtectionBuilder, String, String, String)](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.protectkeyswithazurekeyvault#Microsoft_AspNetCore_DataProtection_AzureDataProtectionBuilderExtensions_ProtectKeysWithAzureKeyVault_Microsoft_AspNetCore_DataProtection_IDataProtectionBuilder_System_String_System_String_System_String_) zezwala na używanie `ClientId` i `ClientSecret` umożliwiające systemu ochrony danych, do korzystania z usługi key vault.</span><span class="sxs-lookup"><span data-stu-id="feecc-130">[ProtectKeysWithAzureKeyVault(IDataProtectionBuilder, String, String, String)](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.protectkeyswithazurekeyvault#Microsoft_AspNetCore_DataProtection_AzureDataProtectionBuilderExtensions_ProtectKeysWithAzureKeyVault_Microsoft_AspNetCore_DataProtection_IDataProtectionBuilder_System_String_System_String_System_String_) permits the use of a `ClientId` and `ClientSecret` to enable the data protection system to use the key vault.</span></span>

::: moniker-end

## <a name="persistkeystofilesystem"></a><span data-ttu-id="feecc-131">PersistKeysToFileSystem</span><span class="sxs-lookup"><span data-stu-id="feecc-131">PersistKeysToFileSystem</span></span>

<span data-ttu-id="feecc-132">Do przechowywania kluczy w udziale UNC, a nie na *% LOCALAPPDATA %* domyślna lokalizacja, skonfiguruj system [PersistKeysToFileSystem](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.persistkeystofilesystem):</span><span class="sxs-lookup"><span data-stu-id="feecc-132">To store keys on a UNC share instead of at the *%LOCALAPPDATA%* default location, configure the system with [PersistKeysToFileSystem](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.persistkeystofilesystem):</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .PersistKeysToFileSystem(new DirectoryInfo(@"\\server\share\directory\"));
}
```

> [!WARNING]
> <span data-ttu-id="feecc-133">W przypadku zmiany lokalizacji trwałość klucza systemu nie są już automatycznie szyfruje kluczy w stanie spoczynku, ponieważ nie wie, czy DPAPI to mechanizm szyfrowania odpowiednie.</span><span class="sxs-lookup"><span data-stu-id="feecc-133">If you change the key persistence location, the system no longer automatically encrypts keys at rest, since it doesn't know whether DPAPI is an appropriate encryption mechanism.</span></span>

## <a name="protectkeyswith"></a><span data-ttu-id="feecc-134">ProtectKeysWith\*</span><span class="sxs-lookup"><span data-stu-id="feecc-134">ProtectKeysWith\*</span></span>

<span data-ttu-id="feecc-135">Możesz skonfigurować system do ochrony kluczy w spoczynku, wywołując jedną z [ProtectKeysWith\* ](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions) interfejsy API konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="feecc-135">You can configure the system to protect keys at rest by calling any of the [ProtectKeysWith\*](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions) configuration APIs.</span></span> <span data-ttu-id="feecc-136">Należy wziąć pod uwagę w poniższym przykładzie, klucze są przechowywane w udziale UNC, która szyfruje tych kluczy w stanie spoczynku przy użyciu określonego certyfikatu X.509:</span><span class="sxs-lookup"><span data-stu-id="feecc-136">Consider the example below, which stores keys on a UNC share and encrypts those keys at rest with a specific X.509 certificate:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .PersistKeysToFileSystem(new DirectoryInfo(@"\\server\share\directory\"))
        .ProtectKeysWithCertificate("thumbprint");
}
```

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="feecc-137">W programie ASP.NET Core 2.1 lub nowszej, możesz podać [X509Certificate2](/dotnet/api/system.security.cryptography.x509certificates.x509certificate2) do [ProtectKeysWithCertificate](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.protectkeyswithcertificate), takie jak certyfikatu załadowanego z pliku:</span><span class="sxs-lookup"><span data-stu-id="feecc-137">In ASP.NET Core 2.1 or later, you can provide an [X509Certificate2](/dotnet/api/system.security.cryptography.x509certificates.x509certificate2) to [ProtectKeysWithCertificate](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.protectkeyswithcertificate), such as a certificate loaded from a file:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .PersistKeysToFileSystem(new DirectoryInfo(@"\\server\share\directory\"))
        .ProtectKeysWithCertificate(
            new X509Certificate2("certificate.pfx", "password"));
}
```

::: moniker-end

<span data-ttu-id="feecc-138">Zobacz [klucza szyfrowania na Rest](xref:security/data-protection/implementation/key-encryption-at-rest) więcej przykładów i dyskusji na temat mechanizmów wbudowane szyfrowanie klucza.</span><span class="sxs-lookup"><span data-stu-id="feecc-138">See [Key Encryption At Rest](xref:security/data-protection/implementation/key-encryption-at-rest) for more examples and discussion on the built-in key encryption mechanisms.</span></span>

::: moniker range=">= aspnetcore-2.1"

## <a name="unprotectkeyswithanycertificate"></a><span data-ttu-id="feecc-139">UnprotectKeysWithAnyCertificate</span><span class="sxs-lookup"><span data-stu-id="feecc-139">UnprotectKeysWithAnyCertificate</span></span>

<span data-ttu-id="feecc-140">W programie ASP.NET Core 2.1 lub nowszej, możesz obrócić certyfikaty i odszyfrować kluczy w stanie spoczynku przy użyciu tablicy [X509Certificate2](/dotnet/api/system.security.cryptography.x509certificates.x509certificate2) certyfikatów przy użyciu [UnprotectKeysWithAnyCertificate](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.unprotectkeyswithanycertificate):</span><span class="sxs-lookup"><span data-stu-id="feecc-140">In ASP.NET Core 2.1 or later, you can rotate certificates and decrypt keys at rest using an array of [X509Certificate2](/dotnet/api/system.security.cryptography.x509certificates.x509certificate2) certificates with [UnprotectKeysWithAnyCertificate](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.unprotectkeyswithanycertificate):</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .PersistKeysToFileSystem(new DirectoryInfo(@"\\server\share\directory\"))
        .ProtectKeysWithCertificate(
            new X509Certificate2("certificate.pfx", "password"));
        .UnprotectKeysWithAnyCertificate(
            new X509Certificate2("certificate_old_1.pfx", "password_1"),
            new X509Certificate2("certificate_old_2.pfx", "password_2"));
}
```

::: moniker-end

## <a name="setdefaultkeylifetime"></a><span data-ttu-id="feecc-141">SetDefaultKeyLifetime</span><span class="sxs-lookup"><span data-stu-id="feecc-141">SetDefaultKeyLifetime</span></span>

<span data-ttu-id="feecc-142">Aby skonfigurować używanie okresu istnienia klucza 14 dni, zamiast domyślnego 90 dni w systemie, użyj [SetDefaultKeyLifetime](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setdefaultkeylifetime):</span><span class="sxs-lookup"><span data-stu-id="feecc-142">To configure the system to use a key lifetime of 14 days instead of the default 90 days, use [SetDefaultKeyLifetime](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setdefaultkeylifetime):</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .SetDefaultKeyLifetime(TimeSpan.FromDays(14));
}
```

## <a name="setapplicationname"></a><span data-ttu-id="feecc-143">SetApplicationName</span><span class="sxs-lookup"><span data-stu-id="feecc-143">SetApplicationName</span></span>

<span data-ttu-id="feecc-144">Domyślnie system ochrony danych izoluje aplikacje, od siebie nawzajem, nawet wtedy, gdy są one udostępnianie tej samej fizycznej repozytorium klucza.</span><span class="sxs-lookup"><span data-stu-id="feecc-144">By default, the Data Protection system isolates apps from one another, even if they're sharing the same physical key repository.</span></span> <span data-ttu-id="feecc-145">Zapobiega to zrozumienie ładunków chronionych drugiej strony aplikacje.</span><span class="sxs-lookup"><span data-stu-id="feecc-145">This prevents the apps from understanding each other's protected payloads.</span></span>

<span data-ttu-id="feecc-146">Aby udostępnić chronione ładunków między aplikacjami:</span><span class="sxs-lookup"><span data-stu-id="feecc-146">To share protected payloads among apps:</span></span>

* <span data-ttu-id="feecc-147">Konfigurowanie <xref:Microsoft.AspNetCore.DataProtection.DataProtectionBuilderExtensions.SetApplicationName*> w każdej aplikacji z taką samą wartość.</span><span class="sxs-lookup"><span data-stu-id="feecc-147">Configure <xref:Microsoft.AspNetCore.DataProtection.DataProtectionBuilderExtensions.SetApplicationName*> in each app with the same value.</span></span>
* <span data-ttu-id="feecc-148">Użyj tej samej wersji stosu interfejsu API ochrony danych w aplikacjach.</span><span class="sxs-lookup"><span data-stu-id="feecc-148">Use the same version of the Data Protection API stack across the apps.</span></span> <span data-ttu-id="feecc-149">Wykonaj **albo** z następujących czynności w plikach projektu aplikacji:</span><span class="sxs-lookup"><span data-stu-id="feecc-149">Perform **either** of the following in the apps' project files:</span></span>
  * <span data-ttu-id="feecc-150">Odwoływać się do tej samej wersji framework udostępnione za pośrednictwem [meta Microsoft.aspnetcore.all Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="feecc-150">Reference the same shared framework version via the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>
  * <span data-ttu-id="feecc-151">Takie same odwołania [ochrony danych pakietu](xref:security/data-protection/introduction#package-layout) wersji.</span><span class="sxs-lookup"><span data-stu-id="feecc-151">Reference the same [Data Protection package](xref:security/data-protection/introduction#package-layout) version.</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .SetApplicationName("shared app name");
}
```

## <a name="disableautomatickeygeneration"></a><span data-ttu-id="feecc-152">DisableAutomaticKeyGeneration</span><span class="sxs-lookup"><span data-stu-id="feecc-152">DisableAutomaticKeyGeneration</span></span>

<span data-ttu-id="feecc-153">Może być scenariusz, w których nie chcesz utworzyć aplikację do automatycznego przenoszenia kluczy (Tworzenie nowych kluczy), zgodnie z ich podejście do wygaśnięcia.</span><span class="sxs-lookup"><span data-stu-id="feecc-153">You may have a scenario where you don't want an app to automatically roll keys (create new keys) as they approach expiration.</span></span> <span data-ttu-id="feecc-154">Przykładem może być w relacji podstawowy/pomocniczy, gdzie głównej aplikacji jest odpowiedzialny za zarządzanie kluczami problemy i dodatkowej aplikacje mają po prostu widok tylko do odczytu pierścienia klucz aplikacji.</span><span class="sxs-lookup"><span data-stu-id="feecc-154">One example of this might be apps set up in a primary/secondary relationship, where only the primary app is responsible for key management concerns and secondary apps simply have a read-only view of the key ring.</span></span> <span data-ttu-id="feecc-155">Dodatkowej aplikacji można skonfigurować, aby traktować pierścień klucza jako tylko do odczytu przez skonfigurowanie systemu za pomocą [DisableAutomaticKeyGeneration](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.disableautomatickeygeneration):</span><span class="sxs-lookup"><span data-stu-id="feecc-155">The secondary apps can be configured to treat the key ring as read-only by configuring the system with [DisableAutomaticKeyGeneration](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.disableautomatickeygeneration):</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .DisableAutomaticKeyGeneration();
}
```

## <a name="per-application-isolation"></a><span data-ttu-id="feecc-156">Na poziomie aplikacji</span><span class="sxs-lookup"><span data-stu-id="feecc-156">Per-application isolation</span></span>

<span data-ttu-id="feecc-157">System ochrony danych jest udostępniane przez hosta platformy ASP.NET Core, jego automatycznie izoluje aplikacje od siebie, nawet jeśli te aplikacje są uruchomione w ten sam proces roboczy i korzystają z tego samego materiał klucza głównego.</span><span class="sxs-lookup"><span data-stu-id="feecc-157">When the Data Protection system is provided by an ASP.NET Core host, it automatically isolates apps from one another, even if those apps are running under the same worker process account and are using the same master keying material.</span></span> <span data-ttu-id="feecc-158">Przypomina nieco modyfikator IsolateApps z elementu System.Web firmy  **\<machineKey >** elementu.</span><span class="sxs-lookup"><span data-stu-id="feecc-158">This is somewhat similar to the IsolateApps modifier from System.Web's **\<machineKey>** element.</span></span>

<span data-ttu-id="feecc-159">Mechanizm izolacji działa, biorąc pod uwagę każdej aplikacji na komputerze lokalnym jako dzierżawca unikatowe, dlatego [interfejsu IDataProtector](/dotnet/api/microsoft.aspnetcore.dataprotection.idataprotector) dostęp do konta root dla danej aplikacji automatycznie zawiera identyfikator aplikacji jako dyskryminatora.</span><span class="sxs-lookup"><span data-stu-id="feecc-159">The isolation mechanism works by considering each app on the local machine as a unique tenant, thus the [IDataProtector](/dotnet/api/microsoft.aspnetcore.dataprotection.idataprotector) rooted for any given app automatically includes the app ID as a discriminator.</span></span> <span data-ttu-id="feecc-160">Unikatowy identyfikator aplikacji pochodzi z jednym z dwóch miejsc:</span><span class="sxs-lookup"><span data-stu-id="feecc-160">The app's unique ID comes from one of two places:</span></span>

1. <span data-ttu-id="feecc-161">Jeśli aplikacja jest hostowana w usługach IIS, unikatowy identyfikator jest ścieżki konfiguracji aplikacji.</span><span class="sxs-lookup"><span data-stu-id="feecc-161">If the app is hosted in IIS, the unique identifier is the app's configuration path.</span></span> <span data-ttu-id="feecc-162">Jeśli aplikacja jest wdrażana w środowisku farmy sieci web, ta wartość powinna być stała, przy założeniu, że w środowiskach usług IIS są skonfigurowane w podobny sposób na wszystkich komputerach w farmie sieci web.</span><span class="sxs-lookup"><span data-stu-id="feecc-162">If an app is deployed in a web farm environment, this value should be stable assuming that the IIS environments are configured similarly across all machines in the web farm.</span></span>

2. <span data-ttu-id="feecc-163">Jeśli aplikacja nie jest hostowana w usługach IIS, unikatowy identyfikator jest ścieżka fizyczna aplikacji.</span><span class="sxs-lookup"><span data-stu-id="feecc-163">If the app isn't hosted in IIS, the unique identifier is the physical path of the app.</span></span>

<span data-ttu-id="feecc-164">Unikatowy identyfikator zaprojektowano w celu przetrwania resetuje &mdash; zarówno poszczególnych aplikacji, jak i samą maszynę.</span><span class="sxs-lookup"><span data-stu-id="feecc-164">The unique identifier is designed to survive resets &mdash; both of the individual app and of the machine itself.</span></span>

<span data-ttu-id="feecc-165">Ten mechanizm izolacji przyjęto założenie, że aplikacje nie są złośliwe.</span><span class="sxs-lookup"><span data-stu-id="feecc-165">This isolation mechanism assumes that the apps are not malicious.</span></span> <span data-ttu-id="feecc-166">Złośliwy aplikacji zawsze może wpłynąć na innych aplikacji działających w ramach tego samego konta procesu roboczego.</span><span class="sxs-lookup"><span data-stu-id="feecc-166">A malicious app can always impact any other app running under the same worker process account.</span></span> <span data-ttu-id="feecc-167">We współużytkowanym środowisku hostingu, gdzie aplikacje są wzajemnie niezaufanych dostawca hostingu powinno zająć kroki w celu zapewnienia izolacji poziomie systemu operacyjnego między aplikacjami, w tym oddzielenie aplikacji podstawowych repozytoriów klucza.</span><span class="sxs-lookup"><span data-stu-id="feecc-167">In a shared hosting environment where apps are mutually untrusted, the hosting provider should take steps to ensure OS-level isolation between apps, including separating the apps' underlying key repositories.</span></span>

<span data-ttu-id="feecc-168">Jeśli system ochrony danych nie jest udostępniane przez hosta platformy ASP.NET Core (na przykład, jeśli go za pomocą wystąpienia `DataProtectionProvider` konkretne typu) Izolacja aplikacji jest domyślnie wyłączona.</span><span class="sxs-lookup"><span data-stu-id="feecc-168">If the Data Protection system isn't provided by an ASP.NET Core host (for example, if you instantiate it via the `DataProtectionProvider` concrete type) app isolation is disabled by default.</span></span> <span data-ttu-id="feecc-169">Izolacja aplikacji jest wyłączone, wszystkie aplikacje objęte tym samym materiału klucza mogą współużytkować ładunków tak długo, jak zapewniają odpowiednią [celów](xref:security/data-protection/consumer-apis/purpose-strings).</span><span class="sxs-lookup"><span data-stu-id="feecc-169">When app isolation is disabled, all apps backed by the same keying material can share payloads as long as they provide the appropriate [purposes](xref:security/data-protection/consumer-apis/purpose-strings).</span></span> <span data-ttu-id="feecc-170">Do zapewnienia izolacji aplikacji, w tym środowisku, należy wywołać [SetApplicationName](#setapplicationname) metody konfiguracji obiektu oraz podać unikatową nazwę dla każdej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="feecc-170">To provide app isolation in this environment, call the [SetApplicationName](#setapplicationname) method on the configuration object and provide a unique name for each app.</span></span>

## <a name="changing-algorithms-with-usecryptographicalgorithms"></a><span data-ttu-id="feecc-171">Zmiana algorytmy z UseCryptographicAlgorithms</span><span class="sxs-lookup"><span data-stu-id="feecc-171">Changing algorithms with UseCryptographicAlgorithms</span></span>

<span data-ttu-id="feecc-172">Stos ochrony danych umożliwia zmianę domyślny algorytm używany przez nowo wygenerowane klucze.</span><span class="sxs-lookup"><span data-stu-id="feecc-172">The Data Protection stack allows you to change the default algorithm used by newly-generated keys.</span></span> <span data-ttu-id="feecc-173">Najprostszym sposobem, w tym celu jest wywołanie [UseCryptographicAlgorithms](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.usecryptographicalgorithms) z wywołania zwrotnego konfiguracji:</span><span class="sxs-lookup"><span data-stu-id="feecc-173">The simplest way to do this is to call [UseCryptographicAlgorithms](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.usecryptographicalgorithms) from the configuration callback:</span></span>

::: moniker range=">= aspnetcore-2.0"

```csharp
services.AddDataProtection()
    .UseCryptographicAlgorithms(
        new AuthenticatedEncryptorConfiguration()
    {
        EncryptionAlgorithm = EncryptionAlgorithm.AES_256_CBC,
        ValidationAlgorithm = ValidationAlgorithm.HMACSHA256
    });
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
services.AddDataProtection()
    .UseCryptographicAlgorithms(
        new AuthenticatedEncryptionSettings()
    {
        EncryptionAlgorithm = EncryptionAlgorithm.AES_256_CBC,
        ValidationAlgorithm = ValidationAlgorithm.HMACSHA256
    });
```

::: moniker-end

<span data-ttu-id="feecc-174">Wartość domyślna usuwają elementy EncryptionAlgorithm to AES-256-CBC, a domyślna ValidationAlgorithm to HMACSHA256.</span><span class="sxs-lookup"><span data-stu-id="feecc-174">The default EncryptionAlgorithm is AES-256-CBC, and the default ValidationAlgorithm is HMACSHA256.</span></span> <span data-ttu-id="feecc-175">Domyślne zasady można ustawić przez administratora systemu za pośrednictwem [komputera zasad](xref:security/data-protection/configuration/machine-wide-policy), ale jawnym wywołaniem `UseCryptographicAlgorithms` zastępują zasady domyślne.</span><span class="sxs-lookup"><span data-stu-id="feecc-175">The default policy can be set by a system administrator via a [machine-wide policy](xref:security/data-protection/configuration/machine-wide-policy), but an explicit call to `UseCryptographicAlgorithms` overrides the default policy.</span></span>

<span data-ttu-id="feecc-176">Wywoływanie `UseCryptographicAlgorithms` służy do określania wybranego algorytmu ze wstępnie zdefiniowanej listy wbudowanych.</span><span class="sxs-lookup"><span data-stu-id="feecc-176">Calling `UseCryptographicAlgorithms` allows you to specify the desired algorithm from a predefined built-in list.</span></span> <span data-ttu-id="feecc-177">Nie musisz się martwić o implementację algorytmu.</span><span class="sxs-lookup"><span data-stu-id="feecc-177">You don't need to worry about the implementation of the algorithm.</span></span> <span data-ttu-id="feecc-178">W powyższym scenariuszu system ochrony danych próbuje użyć implementacji CNG AES, jeśli systemem Windows.</span><span class="sxs-lookup"><span data-stu-id="feecc-178">In the scenario above, the Data Protection system attempts to use the CNG implementation of AES if running on Windows.</span></span> <span data-ttu-id="feecc-179">W przeciwnym razie nastąpi powrót do zarządzanej [System.Security.Cryptography.Aes](/dotnet/api/system.security.cryptography.aes) klasy.</span><span class="sxs-lookup"><span data-stu-id="feecc-179">Otherwise, it falls back to the managed [System.Security.Cryptography.Aes](/dotnet/api/system.security.cryptography.aes) class.</span></span>

<span data-ttu-id="feecc-180">Można ręcznie określić implementację poprzez wywołanie [UseCustomCryptographicAlgorithms](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.usecustomcryptographicalgorithms).</span><span class="sxs-lookup"><span data-stu-id="feecc-180">You can manually specify an implementation via a call to [UseCustomCryptographicAlgorithms](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.usecustomcryptographicalgorithms).</span></span>

> [!TIP]
> <span data-ttu-id="feecc-181">Zmiana algorytmy nie wpływa na istniejące klucze pierścienia klucza.</span><span class="sxs-lookup"><span data-stu-id="feecc-181">Changing algorithms doesn't affect existing keys in the key ring.</span></span> <span data-ttu-id="feecc-182">Dotyczy tylko nowo wygenerowane klucze.</span><span class="sxs-lookup"><span data-stu-id="feecc-182">It only affects newly-generated keys.</span></span>

### <a name="specifying-custom-managed-algorithms"></a><span data-ttu-id="feecc-183">Określanie niestandardowego algorytmy zarządzanych</span><span class="sxs-lookup"><span data-stu-id="feecc-183">Specifying custom managed algorithms</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="feecc-184">Aby określić niestandardowe zarządzanych algorytmów, tworzyć [ManagedAuthenticatedEncryptorConfiguration](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.configurationmodel.managedauthenticatedencryptorconfiguration) wystąpienia, który wskazuje na typy implementacji:</span><span class="sxs-lookup"><span data-stu-id="feecc-184">To specify custom managed algorithms, create a [ManagedAuthenticatedEncryptorConfiguration](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.configurationmodel.managedauthenticatedencryptorconfiguration) instance that points to the implementation types:</span></span>

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

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="feecc-185">Aby określić niestandardowe zarządzanych algorytmów, tworzyć [ManagedAuthenticatedEncryptionSettings](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.managedauthenticatedencryptionsettings) wystąpienia, który wskazuje na typy implementacji:</span><span class="sxs-lookup"><span data-stu-id="feecc-185">To specify custom managed algorithms, create a [ManagedAuthenticatedEncryptionSettings](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.managedauthenticatedencryptionsettings) instance that points to the implementation types:</span></span>

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

::: moniker-end

<span data-ttu-id="feecc-186">Ogólnie \*właściwości typu musi wskazywać na konkretny, tworzone jako wystąpienia (za pośrednictwem publicznego konstruktora bez parametrów) implementacje [SymmetricAlgorithm](/dotnet/api/system.security.cryptography.symmetricalgorithm) i [KeyedHashAlgorithm](/dotnet/api/system.security.cryptography.keyedhashalgorithm), ale przypadki specjalne systemu, takie jak niektóre wartości `typeof(Aes)` dla wygody.</span><span class="sxs-lookup"><span data-stu-id="feecc-186">Generally the \*Type properties must point to concrete, instantiable (via a public parameterless ctor) implementations of [SymmetricAlgorithm](/dotnet/api/system.security.cryptography.symmetricalgorithm) and [KeyedHashAlgorithm](/dotnet/api/system.security.cryptography.keyedhashalgorithm), though the system special-cases some values like `typeof(Aes)` for convenience.</span></span>

> [!NOTE]
> <span data-ttu-id="feecc-187">SymmetricAlgorithm musi mieć klucz o długości 128 bitów ≥ i rozmiar bloku ≥ 64-bitowy, a konieczna jest obsługa szyfrowania trybie CBC przy użyciu dopełnienie PKCS #7.</span><span class="sxs-lookup"><span data-stu-id="feecc-187">The SymmetricAlgorithm must have a key length of ≥ 128 bits and a block size of ≥ 64 bits, and it must support CBC-mode encryption with PKCS #7 padding.</span></span> <span data-ttu-id="feecc-188">KeyedHashAlgorithm musi mieć rozmiar szyfrowanego > = 128 bitów i sieć VPN musi obsługiwać klucze o długości równej długości szyfrowanego algorytmem wyznaczania wartości skrótu.</span><span class="sxs-lookup"><span data-stu-id="feecc-188">The KeyedHashAlgorithm must have a digest size of >= 128 bits, and it must support keys of length equal to the hash algorithm's digest length.</span></span> <span data-ttu-id="feecc-189">KeyedHashAlgorithm nie jest bezwzględnie konieczne jako HMAC.</span><span class="sxs-lookup"><span data-stu-id="feecc-189">The KeyedHashAlgorithm isn't strictly required to be HMAC.</span></span>

### <a name="specifying-custom-windows-cng-algorithms"></a><span data-ttu-id="feecc-190">Określanie niestandardowego algorytmy Windows CNG</span><span class="sxs-lookup"><span data-stu-id="feecc-190">Specifying custom Windows CNG algorithms</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="feecc-191">Określanie niestandardowego algorytmu Windows CNG przy użyciu szyfrowania w trybie CBC przy użyciu HMAC sprawdzania poprawności, należy utworzyć [CngCbcAuthenticatedEncryptorConfiguration](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.configurationmodel.cngcbcauthenticatedencryptorconfiguration) wystąpienia, które zawiera konsolidatorze informacji:</span><span class="sxs-lookup"><span data-stu-id="feecc-191">To specify a custom Windows CNG algorithm using CBC-mode encryption with HMAC validation, create a [CngCbcAuthenticatedEncryptorConfiguration](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.configurationmodel.cngcbcauthenticatedencryptorconfiguration) instance that contains the algorithmic information:</span></span>

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

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="feecc-192">Określanie niestandardowego algorytmu Windows CNG przy użyciu szyfrowania w trybie CBC przy użyciu HMAC sprawdzania poprawności, należy utworzyć [CngCbcAuthenticatedEncryptionSettings](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.cngcbcauthenticatedencryptionsettings) wystąpienia, które zawiera konsolidatorze informacji:</span><span class="sxs-lookup"><span data-stu-id="feecc-192">To specify a custom Windows CNG algorithm using CBC-mode encryption with HMAC validation, create a [CngCbcAuthenticatedEncryptionSettings](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.cngcbcauthenticatedencryptionsettings) instance that contains the algorithmic information:</span></span>

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

::: moniker-end

> [!NOTE]
> <span data-ttu-id="feecc-193">Algorytm szyfrowania symetrycznego bloku musi mieć klucz o długości > = 128 bitów — blok o rozmiarze > = 64-bitowy, i konieczna jest obsługa szyfrowania trybie CBC przy użyciu dopełnienie PKCS #7.</span><span class="sxs-lookup"><span data-stu-id="feecc-193">The symmetric block cipher algorithm must have a key length of >= 128 bits, a block size of >= 64 bits, and it must support CBC-mode encryption with PKCS #7 padding.</span></span> <span data-ttu-id="feecc-194">Algorytm wyznaczania wartości skrótu musi mieć rozmiar szyfrowanego > = 128 bitów i musi obsługiwać otwierana z BCRYPT\_algorytmu podpisu\_obsługi\_HMAC\_flagi flagi.</span><span class="sxs-lookup"><span data-stu-id="feecc-194">The hash algorithm must have a digest size of >= 128 bits and must support being opened with the BCRYPT\_ALG\_HANDLE\_HMAC\_FLAG flag.</span></span> <span data-ttu-id="feecc-195">\*Właściwości dostawcy można ustawić na wartość null, aby użyć domyślnego dostawcy dla określonego algorytmu.</span><span class="sxs-lookup"><span data-stu-id="feecc-195">The \*Provider properties can be set to null to use the default provider for the specified algorithm.</span></span> <span data-ttu-id="feecc-196">Zobacz [BCryptOpenAlgorithmProvider](https://msdn.microsoft.com/library/windows/desktop/aa375479(v=vs.85).aspx) dokumentacji, aby uzyskać więcej informacji.</span><span class="sxs-lookup"><span data-stu-id="feecc-196">See the [BCryptOpenAlgorithmProvider](https://msdn.microsoft.com/library/windows/desktop/aa375479(v=vs.85).aspx) documentation for more information.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="feecc-197">Określanie niestandardowego algorytmu Windows CNG przy użyciu szyfrowania trybu Galois liczników z weryfikacją, należy utworzyć [CngGcmAuthenticatedEncryptorConfiguration](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.configurationmodel.cnggcmauthenticatedencryptorconfiguration) wystąpienia, które zawiera konsolidatorze informacji:</span><span class="sxs-lookup"><span data-stu-id="feecc-197">To specify a custom Windows CNG algorithm using Galois/Counter Mode encryption with validation, create a [CngGcmAuthenticatedEncryptorConfiguration](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.configurationmodel.cnggcmauthenticatedencryptorconfiguration) instance that contains the algorithmic information:</span></span>

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

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="feecc-198">Określanie niestandardowego algorytmu Windows CNG przy użyciu szyfrowania trybu Galois liczników z weryfikacją, należy utworzyć [CngGcmAuthenticatedEncryptionSettings](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.cnggcmauthenticatedencryptionsettings) wystąpienia, które zawiera konsolidatorze informacji:</span><span class="sxs-lookup"><span data-stu-id="feecc-198">To specify a custom Windows CNG algorithm using Galois/Counter Mode encryption with validation, create a [CngGcmAuthenticatedEncryptionSettings](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.cnggcmauthenticatedencryptionsettings) instance that contains the algorithmic information:</span></span>

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

::: moniker-end

> [!NOTE]
> <span data-ttu-id="feecc-199">Algorytm szyfrowania symetrycznego bloku musi mieć klucz o długości > = 128 bitów — rozmiar bloku dokładnie 128 bitów, i konieczna jest obsługa szyfrowania usługi GCM.</span><span class="sxs-lookup"><span data-stu-id="feecc-199">The symmetric block cipher algorithm must have a key length of >= 128 bits, a block size of exactly 128 bits, and it must support GCM encryption.</span></span> <span data-ttu-id="feecc-200">Możesz ustawić [EncryptionAlgorithmProvider](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.configurationmodel.cngcbcauthenticatedencryptorconfiguration.encryptionalgorithmprovider) właściwości na wartość null, aby użyć domyślnego dostawcę dla określonego algorytmu.</span><span class="sxs-lookup"><span data-stu-id="feecc-200">You can set the [EncryptionAlgorithmProvider](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.configurationmodel.cngcbcauthenticatedencryptorconfiguration.encryptionalgorithmprovider) property to null to use the default provider for the specified algorithm.</span></span> <span data-ttu-id="feecc-201">Zobacz [BCryptOpenAlgorithmProvider](https://msdn.microsoft.com/library/windows/desktop/aa375479(v=vs.85).aspx) dokumentacji, aby uzyskać więcej informacji.</span><span class="sxs-lookup"><span data-stu-id="feecc-201">See the [BCryptOpenAlgorithmProvider](https://msdn.microsoft.com/library/windows/desktop/aa375479(v=vs.85).aspx) documentation for more information.</span></span>

### <a name="specifying-other-custom-algorithms"></a><span data-ttu-id="feecc-202">Określanie inne algorytmy niestandardowych</span><span class="sxs-lookup"><span data-stu-id="feecc-202">Specifying other custom algorithms</span></span>

<span data-ttu-id="feecc-203">Chociaż nie są widoczne jako najwyższej klasy interfejs API, system ochrony danych jest rozszerzalny, aby umożliwić określenie prawie każdy rodzaj algorytmu.</span><span class="sxs-lookup"><span data-stu-id="feecc-203">Though not exposed as a first-class API, the Data Protection system is extensible enough to allow specifying almost any kind of algorithm.</span></span> <span data-ttu-id="feecc-204">Na przykład użytkownik może zachować wszystkie klucze znajdujących się w ramach sprzętowego modułu zabezpieczeń (HSM) i zapewniają niestandardową implementację podstawową procedury szyfrowania i odszyfrowywania.</span><span class="sxs-lookup"><span data-stu-id="feecc-204">For example, it's possible to keep all keys contained within a Hardware Security Module (HSM) and to provide a custom implementation of the core encryption and decryption routines.</span></span> <span data-ttu-id="feecc-205">Zobacz [IAuthenticatedEncryptor](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.iauthenticatedencryptor) w [rozszerzalność kryptografii Core](xref:security/data-protection/extensibility/core-crypto) Aby uzyskać więcej informacji.</span><span class="sxs-lookup"><span data-stu-id="feecc-205">See [IAuthenticatedEncryptor](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.iauthenticatedencryptor) in [Core cryptography extensibility](xref:security/data-protection/extensibility/core-crypto) for more information.</span></span>

## <a name="persisting-keys-when-hosting-in-a-docker-container"></a><span data-ttu-id="feecc-206">Przechowywanie kluczy w przypadku hostowania w kontenerze platformy Docker</span><span class="sxs-lookup"><span data-stu-id="feecc-206">Persisting keys when hosting in a Docker container</span></span>

<span data-ttu-id="feecc-207">W przypadku hostowania w [Docker](/dotnet/standard/microservices-architecture/container-docker-introduction/) kontenera, klucze należy utrzymywać albo:</span><span class="sxs-lookup"><span data-stu-id="feecc-207">When hosting in a [Docker](/dotnet/standard/microservices-architecture/container-docker-introduction/) container, keys should be maintained in either:</span></span>

* <span data-ttu-id="feecc-208">Folder, który jest woluminem platformy Docker, która utrwala poza okres istnienia kontenera, takich jak udostępnionego woluminu lub wolumin zainstalowany w hoście.</span><span class="sxs-lookup"><span data-stu-id="feecc-208">A folder that's a Docker volume that persists beyond the container's lifetime, such as a shared volume or a host-mounted volume.</span></span>
* <span data-ttu-id="feecc-209">Zewnętrznego dostawcy, takich jak [usługi Azure Key Vault](https://azure.microsoft.com/services/key-vault/) lub [Redis](https://redis.io/).</span><span class="sxs-lookup"><span data-stu-id="feecc-209">An external provider, such as [Azure Key Vault](https://azure.microsoft.com/services/key-vault/) or [Redis](https://redis.io/).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="feecc-210">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="feecc-210">Additional resources</span></span>

* <xref:security/data-protection/configuration/non-di-scenarios>
* <xref:security/data-protection/configuration/machine-wide-policy>
* <xref:host-and-deploy/web-farm>
