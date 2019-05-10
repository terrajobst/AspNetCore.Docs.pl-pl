---
title: Szyfrowanie kluczy podczas magazynowania w programie ASP.NET Core
author: rick-anderson
description: Dowiedz się, szczegóły implementacji ochrony danych programu ASP.NET Core szyfrowanie kluczy podczas magazynowania.
ms.author: riande
ms.date: 07/16/2018
uid: security/data-protection/implementation/key-encryption-at-rest
ms.openlocfilehash: 52c3137dbe467096364b42430c92aecc7c15e313
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/27/2019
ms.locfileid: "64898774"
---
# <a name="key-encryption-at-rest-in-aspnet-core"></a><span data-ttu-id="27f40-103">Szyfrowanie kluczy podczas magazynowania w programie ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="27f40-103">Key encryption at rest in ASP.NET Core</span></span>

<span data-ttu-id="27f40-104">System ochrony danych [wykorzystuje mechanizm wykrywania domyślnie](xref:security/data-protection/configuration/default-settings) ustalenie, jak kryptograficzne klucze powinny być szyfrowane podczas przechowywania.</span><span class="sxs-lookup"><span data-stu-id="27f40-104">The data protection system [employs a discovery mechanism by default](xref:security/data-protection/configuration/default-settings) to determine how cryptographic keys should be encrypted at rest.</span></span> <span data-ttu-id="27f40-105">Deweloper można zastąpić mechanizm odnajdywania i ręcznie określić, jak klucze powinny być szyfrowane podczas przechowywania.</span><span class="sxs-lookup"><span data-stu-id="27f40-105">The developer can override the discovery mechanism and manually specify how keys should be encrypted at rest.</span></span>

> [!WARNING]
> <span data-ttu-id="27f40-106">Jeśli określisz jawnego [klucza lokalizacji trwałości](xref:security/data-protection/implementation/key-storage-providers), system ochrony danych deregisters domyślne szyfrowanie kluczy podczas mechanizm rest.</span><span class="sxs-lookup"><span data-stu-id="27f40-106">If you specify an explicit [key persistence location](xref:security/data-protection/implementation/key-storage-providers), the data protection system deregisters the default key encryption at rest mechanism.</span></span> <span data-ttu-id="27f40-107">W związku z tym klucze nie są szyfrowane, gdy.</span><span class="sxs-lookup"><span data-stu-id="27f40-107">Consequently, keys are no longer encrypted at rest.</span></span> <span data-ttu-id="27f40-108">Zaleca się, że możesz [mechanizm jawne klucza szyfrowania określony](xref:security/data-protection/implementation/key-encryption-at-rest) we wdrożeniach produkcyjnych.</span><span class="sxs-lookup"><span data-stu-id="27f40-108">We recommend that you [specify an explicit key encryption mechanism](xref:security/data-protection/implementation/key-encryption-at-rest) for production deployments.</span></span> <span data-ttu-id="27f40-109">Opcje mechanizmu szyfrowania podczas spoczynku są opisane w tym temacie.</span><span class="sxs-lookup"><span data-stu-id="27f40-109">The encryption-at-rest mechanism options are described in this topic.</span></span>

::: moniker range=">= aspnetcore-2.1"

## <a name="azure-key-vault"></a><span data-ttu-id="27f40-110">W usłudze Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="27f40-110">Azure Key Vault</span></span>

<span data-ttu-id="27f40-111">Do przechowywania kluczy w [usługi Azure Key Vault](https://azure.microsoft.com/services/key-vault/), skonfiguruj system z [ProtectKeysWithAzureKeyVault](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.protectkeyswithazurekeyvault) w `Startup` klasy:</span><span class="sxs-lookup"><span data-stu-id="27f40-111">To store keys in [Azure Key Vault](https://azure.microsoft.com/services/key-vault/), configure the system with [ProtectKeysWithAzureKeyVault](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.protectkeyswithazurekeyvault) in the `Startup` class:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .PersistKeysToAzureBlobStorage(new Uri("<blobUriWithSasToken>"))
        .ProtectKeysWithAzureKeyVault("<keyIdentifier>", "<clientId>", "<clientSecret>");
}
```

<span data-ttu-id="27f40-112">Aby uzyskać więcej informacji, zobacz [konfiguracji ochrony danych platformy ASP.NET Core: ProtectKeysWithAzureKeyVault](xref:security/data-protection/configuration/overview#protectkeyswithazurekeyvault).</span><span class="sxs-lookup"><span data-stu-id="27f40-112">For more information, see [Configure ASP.NET Core Data Protection: ProtectKeysWithAzureKeyVault](xref:security/data-protection/configuration/overview#protectkeyswithazurekeyvault).</span></span>

::: moniker-end

## <a name="windows-dpapi"></a><span data-ttu-id="27f40-113">Windows DPAPI</span><span class="sxs-lookup"><span data-stu-id="27f40-113">Windows DPAPI</span></span>

<span data-ttu-id="27f40-114">**Dotyczy tylko wdrożenia Windows.**</span><span class="sxs-lookup"><span data-stu-id="27f40-114">**Only applies to Windows deployments.**</span></span>

<span data-ttu-id="27f40-115">Gdy jest używany Windows DPAPI, materiału klucza jest szyfrowana za pomocą [CryptProtectData](/windows/desktop/api/dpapi/nf-dpapi-cryptprotectdata) przed są utrwalane w magazynie.</span><span class="sxs-lookup"><span data-stu-id="27f40-115">When Windows DPAPI is used, key material is encrypted with [CryptProtectData](/windows/desktop/api/dpapi/nf-dpapi-cryptprotectdata) before being persisted to storage.</span></span> <span data-ttu-id="27f40-116">DPAPI to mechanizm szyfrowania odpowiednie dla danych, które nie jest nigdy odczytywana poza bieżącej maszyny (choć jest możliwe utworzyć kopię tych kluczy do usługi Active Directory; zobacz [profilów mobilnych i interfejsu DPAPI](https://support.microsoft.com/kb/309408/#6)).</span><span class="sxs-lookup"><span data-stu-id="27f40-116">DPAPI is an appropriate encryption mechanism for data that's never read outside of the current machine (though it's possible to back these keys up to Active Directory; see [DPAPI and Roaming Profiles](https://support.microsoft.com/kb/309408/#6)).</span></span> <span data-ttu-id="27f40-117">Aby skonfigurować szyfrowanie klucza magazynowanych DPAPI, należy wywołać jedną z [ProtectKeysWithDpapi](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.protectkeyswithdpapi) metody rozszerzenia:</span><span class="sxs-lookup"><span data-stu-id="27f40-117">To configure DPAPI key-at-rest encryption, call one of the [ProtectKeysWithDpapi](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.protectkeyswithdpapi) extension methods:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    // Only the local user account can decrypt the keys
    services.AddDataProtection()
        .ProtectKeysWithDpapi();
}
```

<span data-ttu-id="27f40-118">Jeśli `ProtectKeysWithDpapi` jest wywoływana bez parametrów, tylko bieżącego konta użytkownika Windows, może odszyfrować utrwalonych pierścień klucza.</span><span class="sxs-lookup"><span data-stu-id="27f40-118">If `ProtectKeysWithDpapi` is called with no parameters, only the current Windows user account can decipher the persisted key ring.</span></span> <span data-ttu-id="27f40-119">Opcjonalnie możesz określić, że dowolne konto użytkownika na komputerze (nie tylko bieżącego konta użytkownika) można odszyfrować pierścień klucza:</span><span class="sxs-lookup"><span data-stu-id="27f40-119">You can optionally specify that any user account on the machine (not just the current user account) be able to decipher the key ring:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    // All user accounts on the machine can decrypt the keys
    services.AddDataProtection()
        .ProtectKeysWithDpapi(protectToLocalMachine: true);
}
```

::: moniker range=">= aspnetcore-2.0"

## <a name="x509-certificate"></a><span data-ttu-id="27f40-120">Certyfikat X.509</span><span class="sxs-lookup"><span data-stu-id="27f40-120">X.509 certificate</span></span>

<span data-ttu-id="27f40-121">Aplikacja jest rozłożona się między wieloma maszynami, mogą być wygodne rozdystrybuować udostępnionego certyfikat X.509 używany przez maszyny i konfigurowanie aplikacji hostowanych na używanie certyfikatu do szyfrowania kluczy w stanie spoczynku:</span><span class="sxs-lookup"><span data-stu-id="27f40-121">If the app is spread across multiple machines, it may be convenient to distribute a shared X.509 certificate across the machines and configure the hosted apps to use the certificate for encryption of keys at rest:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .ProtectKeysWithCertificate("3BCE558E2AD3E0E34A7743EAB5AEA2A9BD2575A0");
}
```

<span data-ttu-id="27f40-122">Ze względu na ograniczenia systemu .NET Framework obsługiwane są tylko certyfikatów z kluczami prywatnymi CAPI.</span><span class="sxs-lookup"><span data-stu-id="27f40-122">Due to .NET Framework limitations, only certificates with CAPI private keys are supported.</span></span> <span data-ttu-id="27f40-123">Zobacz poniższą zawartość możliwe obejścia tych ograniczeń.</span><span class="sxs-lookup"><span data-stu-id="27f40-123">See the content below for possible workarounds to these limitations.</span></span>

::: moniker-end

## <a name="windows-dpapi-ng"></a><span data-ttu-id="27f40-124">Windows DPAPI-NG</span><span class="sxs-lookup"><span data-stu-id="27f40-124">Windows DPAPI-NG</span></span>

<span data-ttu-id="27f40-125">**Ten mechanizm jest dostępna tylko w systemie Windows 8/Windows Server 2012 lub nowszy.**</span><span class="sxs-lookup"><span data-stu-id="27f40-125">**This mechanism is available only on Windows 8/Windows Server 2012 or later.**</span></span>

<span data-ttu-id="27f40-126">Począwszy od systemu Windows 8, systemu operacyjnego Windows obsługuje DPAPI-NG (nazywane również CNG DPAPI).</span><span class="sxs-lookup"><span data-stu-id="27f40-126">Beginning with Windows 8, Windows OS supports DPAPI-NG (also called CNG DPAPI).</span></span> <span data-ttu-id="27f40-127">Aby uzyskać więcej informacji, zobacz [o DPAPI CNG](/windows/desktop/SecCNG/cng-dpapi).</span><span class="sxs-lookup"><span data-stu-id="27f40-127">For more information, see [About CNG DPAPI](/windows/desktop/SecCNG/cng-dpapi).</span></span>

<span data-ttu-id="27f40-128">Podmiot zabezpieczeń jest zakodowane jako regułę ochrony deskryptora.</span><span class="sxs-lookup"><span data-stu-id="27f40-128">The principal is encoded as a protection descriptor rule.</span></span> <span data-ttu-id="27f40-129">W poniższym przykładzie, który wywołuje [ProtectKeysWithDpapiNG](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.protectkeyswithdpaping)tylko użytkownika przyłączone do domeny przy użyciu określonego identyfikatora SID może odszyfrować pierścień klucza:</span><span class="sxs-lookup"><span data-stu-id="27f40-129">In the following example that calls [ProtectKeysWithDpapiNG](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.protectkeyswithdpaping), only the domain-joined user with the specified SID can decrypt the key ring:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    // Uses the descriptor rule "SID=S-1-5-21-..."
    services.AddDataProtection()
        .ProtectKeysWithDpapiNG("SID=S-1-5-21-...",
        flags: DpapiNGProtectionDescriptorFlags.None);
}
```

<span data-ttu-id="27f40-130">Istnieje również przeciążenie bez parametrów `ProtectKeysWithDpapiNG`.</span><span class="sxs-lookup"><span data-stu-id="27f40-130">There's also a parameterless overload of `ProtectKeysWithDpapiNG`.</span></span> <span data-ttu-id="27f40-131">Określ reguły przy użyciu tej metody jako udogodnienie "identyfikator SID = {CURRENT_ACCOUNT_SID}", gdzie *CURRENT_ACCOUNT_SID* jest identyfikator SID bieżącego konta użytkownika Windows:</span><span class="sxs-lookup"><span data-stu-id="27f40-131">Use this convenience method to specify the rule "SID={CURRENT_ACCOUNT_SID}", where *CURRENT_ACCOUNT_SID* is the SID of the current Windows user account:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    // Use the descriptor rule "SID={current account SID}"
    services.AddDataProtection()
        .ProtectKeysWithDpapiNG();
}
```

<span data-ttu-id="27f40-132">W tym scenariuszu kontroler domeny usługi AD jest odpowiedzialna za dystrybucję kluczy szyfrowania używany przez operacje DPAPI NG.</span><span class="sxs-lookup"><span data-stu-id="27f40-132">In this scenario, the AD domain controller is responsible for distributing the encryption keys used by the DPAPI-NG operations.</span></span> <span data-ttu-id="27f40-133">Użytkownik docelowy może odszyfrować zaszyfrowanego ładunku z maszyn przyłączonych do domeny (pod warunkiem, że proces jest uruchomiony w ramach ich tożsamości).</span><span class="sxs-lookup"><span data-stu-id="27f40-133">The target user can decipher the encrypted payload from any domain-joined machine (provided that the process is running under their identity).</span></span>

## <a name="certificate-based-encryption-with-windows-dpapi-ng"></a><span data-ttu-id="27f40-134">Na podstawie certyfikatu szyfrowania przy użyciu interfejsu DPAPI NG Windows</span><span class="sxs-lookup"><span data-stu-id="27f40-134">Certificate-based encryption with Windows DPAPI-NG</span></span>

<span data-ttu-id="27f40-135">Jeśli aplikacja jest uruchomiona w systemie Windows 8.1 / Windows Server 2012 R2 lub nowszy, można użyć Windows DPAPI-NG do szyfrowania opartego na certyfikatach.</span><span class="sxs-lookup"><span data-stu-id="27f40-135">If the app is running on Windows 8.1/Windows Server 2012 R2 or later, you can use Windows DPAPI-NG to perform certificate-based encryption.</span></span> <span data-ttu-id="27f40-136">Użyj ciągu deskryptora reguły "certyfikat = HashId:THUMBPRINT", gdzie *odcisk PALCA* jest zakodowany w formacie szesnastkowy SHA1 odcisk palca certyfikatu:</span><span class="sxs-lookup"><span data-stu-id="27f40-136">Use the rule descriptor string "CERTIFICATE=HashId:THUMBPRINT", where *THUMBPRINT* is the hex-encoded SHA1 thumbprint of the certificate:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .ProtectKeysWithDpapiNG("CERTIFICATE=HashId:3BCE558E2...B5AEA2A9BD2575A0",
            flags: DpapiNGProtectionDescriptorFlags.None);
}
```

<span data-ttu-id="27f40-137">Dowolna aplikacja wskazany w tym repozytorium musi działać na Windows 8.1 / Windows Server 2012 R2 lub nowszej, aby odszyfrować kluczy.</span><span class="sxs-lookup"><span data-stu-id="27f40-137">Any app pointed at this repository must be running on Windows 8.1/Windows Server 2012 R2 or later to decipher the keys.</span></span>

## <a name="custom-key-encryption"></a><span data-ttu-id="27f40-138">Szyfrowanie klucza niestandardowego</span><span class="sxs-lookup"><span data-stu-id="27f40-138">Custom key encryption</span></span>

<span data-ttu-id="27f40-139">Jeśli mechanizmy wewnętrzne nie są odpowiednie, deweloper może określić ich własny mechanizm szyfrowania, podając własne [IXmlEncryptor](/dotnet/api/microsoft.aspnetcore.dataprotection.xmlencryption.ixmlencryptor).</span><span class="sxs-lookup"><span data-stu-id="27f40-139">If the in-box mechanisms aren't appropriate, the developer can specify their own key encryption mechanism by providing a custom [IXmlEncryptor](/dotnet/api/microsoft.aspnetcore.dataprotection.xmlencryption.ixmlencryptor).</span></span>
