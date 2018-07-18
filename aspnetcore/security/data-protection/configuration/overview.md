---
title: Konfigurowanie ochrony danych programu ASP.NET Core
author: rick-anderson
description: Dowiedz się, jak skonfigurować ochronę danych w programie ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 07/17/2017
uid: security/data-protection/configuration/overview
ms.openlocfilehash: f3cac3541ffe633886f82cec8180a219272c24d6
ms.sourcegitcommit: 3ca527f27c88cfc9d04688db5499e372fbc2c775
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/17/2018
ms.locfileid: "39095603"
---
# <a name="configure-aspnet-core-data-protection"></a>Konfigurowanie ochrony danych programu ASP.NET Core

Przez [Rick Anderson](https://twitter.com/RickAndMSFT)

Po zainicjowaniu system ochrony danych dotyczy [domyślne ustawienia](xref:security/data-protection/configuration/default-settings) oparte na środowisku operacyjnym. Te ustawienia są zwykle odpowiednie dla aplikacji działających na jednym komputerze. Istnieją przypadki, w których deweloper może być konieczna zmiana ustawień domyślnych:

* Aplikacja jest rozłożona się między wieloma maszynami.
* W celu zachowania zgodności.

Dla tych scenariuszy system ochrony danych oferuje interfejs API konfiguracji zaawansowanych.

> [!WARNING]
> Podobnie jak w plikach konfiguracji, pierścień klucz ochrony danych powinny być chronione przy użyciu odpowiednich uprawnień. Istnieje możliwość szyfrowania kluczy w spoczynku, ale to nie uniemożliwiają osobom atakującym tworzenia nowych kluczy. W związku z tym ma wpływ na zabezpieczenia aplikacji. Lokalizacja magazynu skonfigurowaną z ochroną danych powinny mieć jego dostęp ograniczony do aplikacji, podobnie jak będzie chronić pliki konfiguracji. Na przykład jeśli chcesz przechowywać swoje pierścień klucza na dysku, Użyj uprawnień systemu plików. Upewnij się jedynie tożsamość, której działa aplikacja sieci web ma odczytu, zapisu i tworzenia dostęp do tego katalogu. Jeśli używasz usługi Azure Table Storage, w aplikacji sieci web powinien mieć możliwość odczytu, zapisu lub tworzenia nowych wpisów w tabeli store itp.
>
> Metoda rozszerzenia [AddDataProtection](/dotnet/api/microsoft.extensions.dependencyinjection.dataprotectionservicecollectionextensions.adddataprotection) zwraca [IDataProtectionBuilder](/dotnet/api/microsoft.aspnetcore.dataprotection.idataprotectionbuilder). `IDataProtectionBuilder` udostępnia metody rozszerzenia, czy można połączyć w łańcuch ze sobą, aby skonfigurować ochronę danych opcje.

::: moniker range=">= aspnetcore-2.1"

## <a name="protectkeyswithazurekeyvault"></a>ProtectKeysWithAzureKeyVault

Do przechowywania kluczy w [usługi Azure Key Vault](https://azure.microsoft.com/services/key-vault/), skonfiguruj system z [ProtectKeysWithAzureKeyVault](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.protectkeyswithazurekeyvault) w `Startup` klasy:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .PersistKeysToAzureBlobStorage(new Uri("<blobUriWithSasToken>"))
        .ProtectKeysWithAzureKeyVault("<keyIdentifier>", "<clientId>", "<clientSecret>");
}
```

Ustaw lokalizację magazynu pierścień klucza (na przykład [PersistKeysToAzureBlobStorage](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.persistkeystoazureblobstorage)). Można ustawić lokalizację, ponieważ wywołanie `ProtectKeysWithAzureKeyVault` implementuje [IXmlEncryptor](/dotnet/api/microsoft.aspnetcore.dataprotection.xmlencryption.ixmlencryptor) wyłączają ustawienia ochrony danych, w tym lokalizacji przechowywania klucza pierścienia. Poprzedni przykład wykorzystuje usługi Azure Blob Storage, aby utrwalić pierścień klucza. Aby uzyskać więcej informacji, zobacz [dostawcy magazynu kluczy: Azure i Redis](xref:security/data-protection/implementation/key-storage-providers#azure-and-redis). Można również utrwalić pierścień klucz lokalnie za pomocą [PersistKeysToFileSystem](xref:security/data-protection/implementation/key-storage-providers#file-system).

`keyIdentifier` Jest identyfikator klucza magazynu kluczy, używany do szyfrowania klucza (na przykład `https://contosokeyvault.vault.azure.net/keys/dataprotection/`).

`ProtectKeysWithAzureKeyVault` przeciążenia:

* [ProtectKeysWithAzureKeyVault (IDataProtectionBuilder, KeyVaultClient, String)](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.protectkeyswithazurekeyvault#Microsoft_AspNetCore_DataProtection_AzureDataProtectionBuilderExtensions_ProtectKeysWithAzureKeyVault_Microsoft_AspNetCore_DataProtection_IDataProtectionBuilder_Microsoft_Azure_KeyVault_KeyVaultClient_System_String_) zezwala na używanie [KeyVaultClient](/dotnet/api/microsoft.azure.keyvault.keyvaultclient) umożliwiające systemu ochrony danych, do korzystania z usługi key vault.
* [ProtectKeysWithAzureKeyVault (IDataProtectionBuilder, String, String, X509Certificate2)](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.protectkeyswithazurekeyvault#Microsoft_AspNetCore_DataProtection_AzureDataProtectionBuilderExtensions_ProtectKeysWithAzureKeyVault_Microsoft_AspNetCore_DataProtection_IDataProtectionBuilder_System_String_System_String_System_Security_Cryptography_X509Certificates_X509Certificate2_) zezwala na używanie `ClientId` i [X509Certificate](/dotnet/api/system.security.cryptography.x509certificates.x509certificate2) umożliwiające systemu ochrony danych, do korzystania z usługi key vault.
* [ProtectKeysWithAzureKeyVault (IDataProtectionBuilder, String, String, String)](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.protectkeyswithazurekeyvault#Microsoft_AspNetCore_DataProtection_AzureDataProtectionBuilderExtensions_ProtectKeysWithAzureKeyVault_Microsoft_AspNetCore_DataProtection_IDataProtectionBuilder_System_String_System_String_System_String_) zezwala na używanie `ClientId` i `ClientSecret` umożliwiające systemu ochrony danych, do korzystania z usługi key vault.

::: moniker-end

## <a name="persistkeystofilesystem"></a>PersistKeysToFileSystem

Do przechowywania kluczy w udziale UNC, a nie na *% LOCALAPPDATA %* domyślna lokalizacja, skonfiguruj system [PersistKeysToFileSystem](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.persistkeystofilesystem):

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .PersistKeysToFileSystem(new DirectoryInfo(@"\\server\share\directory\"));
}
```

> [!WARNING]
> W przypadku zmiany lokalizacji trwałość klucza systemu nie są już automatycznie szyfruje kluczy w stanie spoczynku, ponieważ nie wie, czy DPAPI to mechanizm szyfrowania odpowiednie.

## <a name="protectkeyswith"></a>ProtectKeysWith\*

Możesz skonfigurować system do ochrony kluczy w spoczynku, wywołując jedną z [ProtectKeysWith\* ](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions) interfejsy API konfiguracji. Należy wziąć pod uwagę w poniższym przykładzie, klucze są przechowywane w udziale UNC, która szyfruje tych kluczy w stanie spoczynku przy użyciu określonego certyfikatu X.509:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .PersistKeysToFileSystem(new DirectoryInfo(@"\\server\share\directory\"))
        .ProtectKeysWithCertificate("thumbprint");
}
```

Zobacz [klucza szyfrowania na Rest](xref:security/data-protection/implementation/key-encryption-at-rest) więcej przykładów i dyskusji na temat mechanizmów wbudowane szyfrowanie klucza.

## <a name="setdefaultkeylifetime"></a>SetDefaultKeyLifetime

Aby skonfigurować używanie okresu istnienia klucza 14 dni, zamiast domyślnego 90 dni w systemie, użyj [SetDefaultKeyLifetime](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setdefaultkeylifetime):

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .SetDefaultKeyLifetime(TimeSpan.FromDays(14));
}
```

## <a name="setapplicationname"></a>SetApplicationName

Domyślnie system ochrony danych izoluje aplikacje, od siebie nawzajem, nawet wtedy, gdy są one udostępnianie tej samej fizycznej repozytorium klucza. Zapobiega to zrozumienie ładunków chronionych drugiej strony aplikacje. Aby udostępnić ładunków chronionych między dwiema aplikacjami, należy użyć [SetApplicationName](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setapplicationname) z taką samą wartość dla każdej aplikacji:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .SetApplicationName("shared app name");
}
```

## <a name="disableautomatickeygeneration"></a>DisableAutomaticKeyGeneration

Może być scenariusz, w których nie chcesz utworzyć aplikację do automatycznego przenoszenia kluczy (Tworzenie nowych kluczy), zgodnie z ich podejście do wygaśnięcia. Przykładem może być w relacji podstawowy/pomocniczy, gdzie głównej aplikacji jest odpowiedzialny za zarządzanie kluczami problemy i dodatkowej aplikacje mają po prostu widok tylko do odczytu pierścienia klucz aplikacji. Dodatkowej aplikacji można skonfigurować, aby traktować pierścień klucza jako tylko do odczytu przez skonfigurowanie systemu za pomocą [DisableAutomaticKeyGeneration](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.disableautomatickeygeneration):

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .DisableAutomaticKeyGeneration();
}
```

## <a name="per-application-isolation"></a>Na poziomie aplikacji

System ochrony danych jest udostępniane przez hosta platformy ASP.NET Core, jego automatycznie izoluje aplikacje od siebie, nawet jeśli te aplikacje są uruchomione w ten sam proces roboczy i korzystają z tego samego materiał klucza głównego. Przypomina nieco modyfikator IsolateApps z elementu System.Web firmy  **\<machineKey >** elementu.

Mechanizm izolacji działa, biorąc pod uwagę każdej aplikacji na komputerze lokalnym jako dzierżawca unikatowe, dlatego [interfejsu IDataProtector](/dotnet/api/microsoft.aspnetcore.dataprotection.idataprotector) dostęp do konta root dla danej aplikacji automatycznie zawiera identyfikator aplikacji jako dyskryminatora. Unikatowy identyfikator aplikacji pochodzi z jednym z dwóch miejsc:

1. Jeśli aplikacja jest hostowana w usługach IIS, unikatowy identyfikator jest ścieżki konfiguracji aplikacji. Jeśli aplikacja jest wdrażana w środowisku farmy sieci web, ta wartość powinna być stała, przy założeniu, że w środowiskach usług IIS są skonfigurowane w podobny sposób na wszystkich komputerach w farmie sieci web.

2. Jeśli aplikacja nie jest hostowana w usługach IIS, unikatowy identyfikator jest ścieżka fizyczna aplikacji.

Unikatowy identyfikator zaprojektowano w celu przetrwania resetuje &mdash; zarówno poszczególnych aplikacji, jak i samą maszynę.

Ten mechanizm izolacji przyjęto założenie, że aplikacje nie są złośliwe. Złośliwy aplikacji zawsze może wpłynąć na innych aplikacji działających w ramach tego samego konta procesu roboczego. We współużytkowanym środowisku hostingu, gdzie aplikacje są wzajemnie niezaufanych dostawca hostingu powinno zająć kroki w celu zapewnienia izolacji poziomie systemu operacyjnego między aplikacjami, w tym oddzielenie aplikacji podstawowych repozytoriów klucza.

Jeśli system ochrony danych nie jest udostępniane przez hosta platformy ASP.NET Core (na przykład, jeśli go za pomocą wystąpienia `DataProtectionProvider` konkretne typu) Izolacja aplikacji jest domyślnie wyłączona. Izolacja aplikacji jest wyłączone, wszystkie aplikacje objęte tym samym materiału klucza mogą współużytkować ładunków tak długo, jak zapewniają odpowiednią [celów](xref:security/data-protection/consumer-apis/purpose-strings). Do zapewnienia izolacji aplikacji, w tym środowisku, należy wywołać [SetApplicationName](#setapplicationname) metody konfiguracji obiektu oraz podać unikatową nazwę dla każdej aplikacji.

## <a name="changing-algorithms-with-usecryptographicalgorithms"></a>Zmiana algorytmy z UseCryptographicAlgorithms

Stos ochrony danych umożliwia zmianę domyślny algorytm używany przez nowo wygenerowane klucze. Najprostszym sposobem, w tym celu jest wywołanie [UseCryptographicAlgorithms](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.usecryptographicalgorithms) z wywołania zwrotnego konfiguracji:

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```csharp
services.AddDataProtection()
    .UseCryptographicAlgorithms(
        new AuthenticatedEncryptorConfiguration()
    {
        EncryptionAlgorithm = EncryptionAlgorithm.AES_256_CBC,
        ValidationAlgorithm = ValidationAlgorithm.HMACSHA256
    });
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

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

Wartość domyślna usuwają elementy EncryptionAlgorithm to AES-256-CBC, a domyślna ValidationAlgorithm to HMACSHA256. Domyślne zasady można ustawić przez administratora systemu za pośrednictwem [komputera zasad](xref:security/data-protection/configuration/machine-wide-policy), ale jawnym wywołaniem `UseCryptographicAlgorithms` zastępują zasady domyślne.

Wywoływanie `UseCryptographicAlgorithms` służy do określania wybranego algorytmu ze wstępnie zdefiniowanej listy wbudowanych. Nie musisz się martwić o implementację algorytmu. W powyższym scenariuszu system ochrony danych próbuje użyć implementacji CNG AES, jeśli systemem Windows. W przeciwnym razie nastąpi powrót do zarządzanej [System.Security.Cryptography.Aes](/dotnet/api/system.security.cryptography.aes) klasy.

Można ręcznie określić implementację poprzez wywołanie [UseCustomCryptographicAlgorithms](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.usecustomcryptographicalgorithms).

> [!TIP]
> Zmiana algorytmy nie wpływa na istniejące klucze pierścienia klucza. Dotyczy tylko nowo wygenerowane klucze.

### <a name="specifying-custom-managed-algorithms"></a>Określanie niestandardowego algorytmy zarządzanych

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Aby określić niestandardowe zarządzanych algorytmów, tworzyć [ManagedAuthenticatedEncryptorConfiguration](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.configurationmodel.managedauthenticatedencryptorconfiguration) wystąpienia, który wskazuje na typy implementacji:

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

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Aby określić niestandardowe zarządzanych algorytmów, tworzyć [ManagedAuthenticatedEncryptionSettings](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.managedauthenticatedencryptionsettings) wystąpienia, który wskazuje na typy implementacji:

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

Ogólnie \*właściwości typu musi wskazywać na konkretny, tworzone jako wystąpienia (za pośrednictwem publicznego konstruktora bez parametrów) implementacje [SymmetricAlgorithm](/dotnet/api/system.security.cryptography.symmetricalgorithm) i [KeyedHashAlgorithm](/dotnet/api/system.security.cryptography.keyedhashalgorithm), ale przypadki specjalne systemu, takie jak niektóre wartości `typeof(Aes)` dla wygody.

> [!NOTE]
> SymmetricAlgorithm musi mieć klucz o długości 128 bitów ≥ i rozmiar bloku ≥ 64-bitowy, a konieczna jest obsługa szyfrowania trybie CBC przy użyciu dopełnienie PKCS #7. KeyedHashAlgorithm musi mieć rozmiar szyfrowanego > = 128 bitów i sieć VPN musi obsługiwać klucze o długości równej długości szyfrowanego algorytmem wyznaczania wartości skrótu. KeyedHashAlgorithm nie jest bezwzględnie konieczne jako HMAC.

### <a name="specifying-custom-windows-cng-algorithms"></a>Określanie niestandardowego algorytmy Windows CNG

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Określanie niestandardowego algorytmu Windows CNG przy użyciu szyfrowania w trybie CBC przy użyciu HMAC sprawdzania poprawności, należy utworzyć [CngCbcAuthenticatedEncryptorConfiguration](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.configurationmodel.cngcbcauthenticatedencryptorconfiguration) wystąpienia, które zawiera konsolidatorze informacji:

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

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Określanie niestandardowego algorytmu Windows CNG przy użyciu szyfrowania w trybie CBC przy użyciu HMAC sprawdzania poprawności, należy utworzyć [CngCbcAuthenticatedEncryptionSettings](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.cngcbcauthenticatedencryptionsettings) wystąpienia, które zawiera konsolidatorze informacji:

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
> Algorytm szyfrowania symetrycznego bloku musi mieć klucz o długości > = 128 bitów — blok o rozmiarze > = 64-bitowy, i konieczna jest obsługa szyfrowania trybie CBC przy użyciu dopełnienie PKCS #7. Algorytm wyznaczania wartości skrótu musi mieć rozmiar szyfrowanego > = 128 bitów i musi obsługiwać otwierana z BCRYPT\_algorytmu podpisu\_obsługi\_HMAC\_flagi flagi. \*Właściwości dostawcy można ustawić na wartość null, aby użyć domyślnego dostawcy dla określonego algorytmu. Zobacz [BCryptOpenAlgorithmProvider](https://msdn.microsoft.com/library/windows/desktop/aa375479(v=vs.85).aspx) dokumentacji, aby uzyskać więcej informacji.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Określanie niestandardowego algorytmu Windows CNG przy użyciu szyfrowania trybu Galois liczników z weryfikacją, należy utworzyć [CngGcmAuthenticatedEncryptorConfiguration](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.configurationmodel.cnggcmauthenticatedencryptorconfiguration) wystąpienia, które zawiera konsolidatorze informacji:

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

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Określanie niestandardowego algorytmu Windows CNG przy użyciu szyfrowania trybu Galois liczników z weryfikacją, należy utworzyć [CngGcmAuthenticatedEncryptionSettings](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.cnggcmauthenticatedencryptionsettings) wystąpienia, które zawiera konsolidatorze informacji:

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
> Algorytm szyfrowania symetrycznego bloku musi mieć klucz o długości > = 128 bitów — rozmiar bloku dokładnie 128 bitów, i konieczna jest obsługa szyfrowania usługi GCM. Możesz ustawić [EncryptionAlgorithmProvider](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.configurationmodel.cngcbcauthenticatedencryptorconfiguration.encryptionalgorithmprovider) właściwości na wartość null, aby użyć domyślnego dostawcę dla określonego algorytmu. Zobacz [BCryptOpenAlgorithmProvider](https://msdn.microsoft.com/library/windows/desktop/aa375479(v=vs.85).aspx) dokumentacji, aby uzyskać więcej informacji.

### <a name="specifying-other-custom-algorithms"></a>Określanie inne algorytmy niestandardowych

Chociaż nie są widoczne jako najwyższej klasy interfejs API, system ochrony danych jest rozszerzalny, aby umożliwić określenie prawie każdy rodzaj algorytmu. Na przykład użytkownik może zachować wszystkie klucze znajdujących się w ramach sprzętowego modułu zabezpieczeń (HSM) i zapewniają niestandardową implementację podstawową procedury szyfrowania i odszyfrowywania. Zobacz [IAuthenticatedEncryptor](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.iauthenticatedencryptor) w [rozszerzalność kryptografii Core](xref:security/data-protection/extensibility/core-crypto) Aby uzyskać więcej informacji.

## <a name="persisting-keys-when-hosting-in-a-docker-container"></a>Przechowywanie kluczy w przypadku hostowania w kontenerze platformy Docker

W przypadku hostowania w [Docker](/dotnet/standard/microservices-architecture/container-docker-introduction/) kontenera, klucze należy utrzymywać albo:

* Folder, który jest woluminem platformy Docker, która utrwala poza okres istnienia kontenera, takich jak udostępnionego woluminu lub wolumin zainstalowany w hoście.
* Zewnętrznego dostawcy, takich jak [usługi Azure Key Vault](https://azure.microsoft.com/services/key-vault/) lub [Redis](https://redis.io/).

## <a name="see-also"></a>Zobacz także

* <xref:security/data-protection/configuration/non-di-scenarios>
* <xref:security/data-protection/configuration/machine-wide-policy>
* <xref:host-and-deploy/web-farm>
