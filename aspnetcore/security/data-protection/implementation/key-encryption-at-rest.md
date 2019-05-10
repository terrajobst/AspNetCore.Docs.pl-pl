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
# <a name="key-encryption-at-rest-in-aspnet-core"></a>Szyfrowanie kluczy podczas magazynowania w programie ASP.NET Core

System ochrony danych [wykorzystuje mechanizm wykrywania domyślnie](xref:security/data-protection/configuration/default-settings) ustalenie, jak kryptograficzne klucze powinny być szyfrowane podczas przechowywania. Deweloper można zastąpić mechanizm odnajdywania i ręcznie określić, jak klucze powinny być szyfrowane podczas przechowywania.

> [!WARNING]
> Jeśli określisz jawnego [klucza lokalizacji trwałości](xref:security/data-protection/implementation/key-storage-providers), system ochrony danych deregisters domyślne szyfrowanie kluczy podczas mechanizm rest. W związku z tym klucze nie są szyfrowane, gdy. Zaleca się, że możesz [mechanizm jawne klucza szyfrowania określony](xref:security/data-protection/implementation/key-encryption-at-rest) we wdrożeniach produkcyjnych. Opcje mechanizmu szyfrowania podczas spoczynku są opisane w tym temacie.

::: moniker range=">= aspnetcore-2.1"

## <a name="azure-key-vault"></a>W usłudze Azure Key Vault

Do przechowywania kluczy w [usługi Azure Key Vault](https://azure.microsoft.com/services/key-vault/), skonfiguruj system z [ProtectKeysWithAzureKeyVault](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.protectkeyswithazurekeyvault) w `Startup` klasy:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .PersistKeysToAzureBlobStorage(new Uri("<blobUriWithSasToken>"))
        .ProtectKeysWithAzureKeyVault("<keyIdentifier>", "<clientId>", "<clientSecret>");
}
```

Aby uzyskać więcej informacji, zobacz [konfiguracji ochrony danych platformy ASP.NET Core: ProtectKeysWithAzureKeyVault](xref:security/data-protection/configuration/overview#protectkeyswithazurekeyvault).

::: moniker-end

## <a name="windows-dpapi"></a>Windows DPAPI

**Dotyczy tylko wdrożenia Windows.**

Gdy jest używany Windows DPAPI, materiału klucza jest szyfrowana za pomocą [CryptProtectData](/windows/desktop/api/dpapi/nf-dpapi-cryptprotectdata) przed są utrwalane w magazynie. DPAPI to mechanizm szyfrowania odpowiednie dla danych, które nie jest nigdy odczytywana poza bieżącej maszyny (choć jest możliwe utworzyć kopię tych kluczy do usługi Active Directory; zobacz [profilów mobilnych i interfejsu DPAPI](https://support.microsoft.com/kb/309408/#6)). Aby skonfigurować szyfrowanie klucza magazynowanych DPAPI, należy wywołać jedną z [ProtectKeysWithDpapi](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.protectkeyswithdpapi) metody rozszerzenia:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    // Only the local user account can decrypt the keys
    services.AddDataProtection()
        .ProtectKeysWithDpapi();
}
```

Jeśli `ProtectKeysWithDpapi` jest wywoływana bez parametrów, tylko bieżącego konta użytkownika Windows, może odszyfrować utrwalonych pierścień klucza. Opcjonalnie możesz określić, że dowolne konto użytkownika na komputerze (nie tylko bieżącego konta użytkownika) można odszyfrować pierścień klucza:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    // All user accounts on the machine can decrypt the keys
    services.AddDataProtection()
        .ProtectKeysWithDpapi(protectToLocalMachine: true);
}
```

::: moniker range=">= aspnetcore-2.0"

## <a name="x509-certificate"></a>Certyfikat X.509

Aplikacja jest rozłożona się między wieloma maszynami, mogą być wygodne rozdystrybuować udostępnionego certyfikat X.509 używany przez maszyny i konfigurowanie aplikacji hostowanych na używanie certyfikatu do szyfrowania kluczy w stanie spoczynku:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .ProtectKeysWithCertificate("3BCE558E2AD3E0E34A7743EAB5AEA2A9BD2575A0");
}
```

Ze względu na ograniczenia systemu .NET Framework obsługiwane są tylko certyfikatów z kluczami prywatnymi CAPI. Zobacz poniższą zawartość możliwe obejścia tych ograniczeń.

::: moniker-end

## <a name="windows-dpapi-ng"></a>Windows DPAPI-NG

**Ten mechanizm jest dostępna tylko w systemie Windows 8/Windows Server 2012 lub nowszy.**

Począwszy od systemu Windows 8, systemu operacyjnego Windows obsługuje DPAPI-NG (nazywane również CNG DPAPI). Aby uzyskać więcej informacji, zobacz [o DPAPI CNG](/windows/desktop/SecCNG/cng-dpapi).

Podmiot zabezpieczeń jest zakodowane jako regułę ochrony deskryptora. W poniższym przykładzie, który wywołuje [ProtectKeysWithDpapiNG](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.protectkeyswithdpaping)tylko użytkownika przyłączone do domeny przy użyciu określonego identyfikatora SID może odszyfrować pierścień klucza:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    // Uses the descriptor rule "SID=S-1-5-21-..."
    services.AddDataProtection()
        .ProtectKeysWithDpapiNG("SID=S-1-5-21-...",
        flags: DpapiNGProtectionDescriptorFlags.None);
}
```

Istnieje również przeciążenie bez parametrów `ProtectKeysWithDpapiNG`. Określ reguły przy użyciu tej metody jako udogodnienie "identyfikator SID = {CURRENT_ACCOUNT_SID}", gdzie *CURRENT_ACCOUNT_SID* jest identyfikator SID bieżącego konta użytkownika Windows:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    // Use the descriptor rule "SID={current account SID}"
    services.AddDataProtection()
        .ProtectKeysWithDpapiNG();
}
```

W tym scenariuszu kontroler domeny usługi AD jest odpowiedzialna za dystrybucję kluczy szyfrowania używany przez operacje DPAPI NG. Użytkownik docelowy może odszyfrować zaszyfrowanego ładunku z maszyn przyłączonych do domeny (pod warunkiem, że proces jest uruchomiony w ramach ich tożsamości).

## <a name="certificate-based-encryption-with-windows-dpapi-ng"></a>Na podstawie certyfikatu szyfrowania przy użyciu interfejsu DPAPI NG Windows

Jeśli aplikacja jest uruchomiona w systemie Windows 8.1 / Windows Server 2012 R2 lub nowszy, można użyć Windows DPAPI-NG do szyfrowania opartego na certyfikatach. Użyj ciągu deskryptora reguły "certyfikat = HashId:THUMBPRINT", gdzie *odcisk PALCA* jest zakodowany w formacie szesnastkowy SHA1 odcisk palca certyfikatu:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .ProtectKeysWithDpapiNG("CERTIFICATE=HashId:3BCE558E2...B5AEA2A9BD2575A0",
            flags: DpapiNGProtectionDescriptorFlags.None);
}
```

Dowolna aplikacja wskazany w tym repozytorium musi działać na Windows 8.1 / Windows Server 2012 R2 lub nowszej, aby odszyfrować kluczy.

## <a name="custom-key-encryption"></a>Szyfrowanie klucza niestandardowego

Jeśli mechanizmy wewnętrzne nie są odpowiednie, deweloper może określić ich własny mechanizm szyfrowania, podając własne [IXmlEncryptor](/dotnet/api/microsoft.aspnetcore.dataprotection.xmlencryption.ixmlencryptor).
