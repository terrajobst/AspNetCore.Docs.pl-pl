---
title: Konfigurowanie ochrony danych w podstawowej platformy ASP.NET
author: rick-anderson
description: Informacje o sposobie konfigurowania ochrony danych w ASP.NET Core.
ms.author: riande
manager: wpickett
ms.date: 07/17/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/configuration/overview
ms.openlocfilehash: c19b1a9cba89a02420c8792b575827d439bc262b
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/19/2018
---
# <a name="configuring-data-protection-in-aspnet-core"></a>Konfigurowanie ochrony danych w podstawowej platformy ASP.NET

Przez [Rick Anderson](https://twitter.com/RickAndMSFT)

Po zainicjowaniu systemu ochrony danych dotyczy [ustawienia domyślne](xref:security/data-protection/configuration/default-settings) oparte na środowisku operacyjnym. Te ustawienia są zazwyczaj odpowiednie dla aplikacji działających na jednym komputerze. Istnieją przypadki, w którym deweloper może być konieczne może zmienić ustawienia domyślne, ponieważ ich aplikacji zostanie rozmieszczona na wielu komputerach lub wymaganiami dotyczącymi zgodności. W tych sytuacjach systemu ochrony danych oferuje interfejs API konfiguracji zaawansowanych.

Brak metody rozszerzenia [AddDataProtection](/dotnet/api/microsoft.extensions.dependencyinjection.dataprotectionservicecollectionextensions.adddataprotection) zwracającą [IDataProtectionBuilder](/dotnet/api/microsoft.aspnetcore.dataprotection.idataprotectionbuilder). `IDataProtectionBuilder`udostępnia metody rozszerzenia, czy użytkownik może łańcuch można skonfigurować ochrony danych opcje.

## <a name="persistkeystofilesystem"></a>PersistKeysToFileSystem

Do przechowywania kluczy w udziale UNC, a nie na *LOCALAPPDATA %* domyślna lokalizacja, skonfigurować system z [PersistKeysToFileSystem](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.persistkeystofilesystem):

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .PersistKeysToFileSystem(new DirectoryInfo(@"\\server\share\directory\"));
}
```

> [!WARNING]
> Zmiana lokalizacji klucza trwałości, system nie będzie automatycznie szyfruje klucze magazynowane, ponieważ nie wiadomo, czy DPAPI to mechanizm szyfrowania odpowiednie.

## <a name="protectkeyswith"></a>ProtectKeysWith\*

Możesz skonfigurować system do ochrony kluczy magazynowane wywołując jedną z [ProtectKeysWith\* ](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions) interfejsy API konfiguracji. W poniższym przykładzie, który klucze są przechowywane w udziale UNC i szyfruje te klucze przechowywane przy użyciu określonego certyfikatu X.509 należy wziąć pod uwagę:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .PersistKeysToFileSystem(new DirectoryInfo(@"\\server\share\directory\"))
        .ProtectKeysWithCertificate("thumbprint");
}
```

Zobacz [klucza szyfrowania w Rest](xref:security/data-protection/implementation/key-encryption-at-rest) więcej przykładów oraz Omówienie mechanizmów wbudowane szyfrowanie klucza.

## <a name="setdefaultkeylifetime"></a>SetDefaultKeyLifetime

Aby skonfigurować system do użycia zamiast domyślnego okresu istnienia klucza 14 dni 90 dni, użyj [SetDefaultKeyLifetime](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setdefaultkeylifetime):

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .SetDefaultKeyLifetime(TimeSpan.FromDays(14));
}
```

## <a name="setapplicationname"></a>SetApplicationName

Domyślnie systemu ochrony danych izoluje aplikacje od siebie, nawet jeśli ich współużytkowania tego samego repozytorium klucza fizycznych. Zapobiega to opis ładunków chronionych drugiej strony aplikacje. Aby udostępnić chronionych ładunków między dwiema aplikacjami, należy użyć [SetApplicationName](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setapplicationname) z taką samą wartość dla każdej aplikacji:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .SetApplicationName("shared app name");
}
```

## <a name="disableautomatickeygeneration"></a>DisableAutomaticKeyGeneration

Scenariusz, w którym nie ma automatycznie wycofanie kluczy (Tworzenie nowych kluczy) zgodnie z ich podejścia wygaśnięcia aplikacji może być. Przykładem może być w relacji podstawowe i pomocnicze, gdy tylko głównej aplikacji jest odpowiedzialny za zarządzanie kluczami problemy i dodatkowej aplikacji po prostu ma widoku tylko do odczytu pierścienia klucz aplikacji. Dodatkowej aplikacji można skonfigurować, aby traktować pierścień klucz jako tylko do odczytu przez skonfigurowanie systemu z [DisableAutomaticKeyGeneration](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.disableautomatickeygeneration):

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .DisableAutomaticKeyGeneration();
}
```

## <a name="per-application-isolation"></a>Na poziomie aplikacji

Gdy systemu ochrony danych jest obsługiwane przez hosta platformy ASP.NET Core, go automatycznie izoluje aplikacje od siebie, nawet jeśli te aplikacje działają w ramach tego samego konta procesu roboczego i korzystają z tej samej materiał klucza głównego. Przypomina trochę modyfikator IsolateApps z elementu System.Web w  **\<machineKey >** elementu.

Mechanizm izolacji polega na uwzględnieniu każdej aplikacji na komputerze lokalnym jako unikatowy dzierżawy, w związku z tym [interfejsu IDataProtector](/dotnet/api/microsoft.aspnetcore.dataprotection.idataprotector) ścieżką do katalogu głównego dla danej aplikacji obejmują automatycznie Identyfikatora aplikacji jako dyskryminatora. Unikatowy identyfikator aplikacji jest dostarczany z jednej z dwóch miejscach:

1. Jeśli aplikacja jest obsługiwana w usługach IIS, unikatowy identyfikator jest ścieżki konfiguracji aplikacji. Jeśli aplikacja jest wdrażana w środowisku farmy sieci web, ta wartość powinna być stała, przy założeniu, że w środowiskach usług IIS są skonfigurowane podobnie na wszystkich komputerach w farmie sieci web.

2. Jeśli aplikacja nie jest obsługiwana przez serwer IIS, unikatowy identyfikator jest ścieżkę fizyczną aplikacji.

Unikatowy identyfikator jest przeznaczona na przetrwanie resetuje &mdash; poszczególnych aplikacji i komputera samego w sobie.

Ten mechanizm izolacji przyjęto założenie, aplikacje nie są złośliwe. Złośliwe aplikacji zawsze może wpłynąć na inną aplikację do uruchamiania tego samego konta procesu roboczego. W udostępnionym środowisku macierzystym których aplikacje są wzajemnie niezaufanych dostawca hostingu powinna przyjmować kroki w celu zapewnienia systemu operacyjnego poziom izolacji między aplikacjami, w tym oddzielanie repozytoria klucza podstawowego w aplikacjach.

Jeśli system ochrony danych nie został podany przez hosta platformy ASP.NET Core (na przykład, jeśli go za pomocą wystąpienia `DataProtectionProvider` specyficzne typu) izolacji aplikacji jest domyślnie wyłączona. Wyłączenie izolacji aplikacji, wszystkie aplikacje obsługiwana przez tego samego materiału klucza można udostępniać ładunków tak długo, jak zapewniają odpowiednią [celów](xref:security/data-protection/consumer-apis/purpose-strings). Zapewnienie izolacji aplikacji, w tym środowisku, należy wywołać [SetApplicationName](#setapplicationname) metody konfiguracji obiektu i Podaj unikatową nazwę dla każdej aplikacji.

## <a name="changing-algorithms-with-usecryptographicalgorithms"></a>Zmiana algorytmów z UseCryptographicAlgorithms

Stos ochrony danych pozwala na zmianę domyślnego algorytmu, używany przez nowo wygenerować kluczy. Najprostszym sposobem, w tym celu na wywołanie jest [UseCryptographicAlgorithms](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.usecryptographicalgorithms) zwrotne konfiguracji:

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

Wartość domyślna algorytm szyfrowania to AES-256-CBC, a domyślna ValidationAlgorithm jest HMACSHA256. Można ustawić domyślną zasadę przez administratora systemu za pośrednictwem [komputera zasad](xref:security/data-protection/configuration/machine-wide-policy), ale jawnym wywołaniem `UseCryptographicAlgorithms` zastępują zasady domyślne.

Wywoływanie `UseCryptographicAlgorithms` służy do określania wybranego algorytmu ze wstępnie zdefiniowanej listy wbudowanych. Nie musisz martwić się o Implementacja algorytmu. W powyższym scenariuszu systemu ochrony danych próbuje użyć implementacji CNG AES, jeśli uruchomiony w systemie Windows. W przeciwnym razie go powraca do zarządzanej [System.Security.Cryptography.Aes](/dotnet/api/system.security.cryptography.aes) klasy.

Można ręcznie określić implementację za pośrednictwem wywołania [UseCustomCryptographicAlgorithms](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.usecustomcryptographicalgorithms).

> [!TIP]
> Zmiana algorytmów nie ma wpływu na istniejące klucze w kręgu klucza. Wpływa tylko na nowo wygenerować kluczy.

### <a name="specifying-custom-managed-algorithms"></a>Określanie niestandardowych algorytmów zarządzanych

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Aby określić niestandardowy zarządzany algorytmów, tworzyć [ManagedAuthenticatedEncryptorConfiguration](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.configurationmodel.managedauthenticatedencryptorconfiguration) wystąpienia, który wskazuje typy wdrożenia:

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

Aby określić niestandardowy zarządzany algorytmów, tworzyć [ManagedAuthenticatedEncryptionSettings](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.managedauthenticatedencryptionsettings) wystąpienia, który wskazuje typy wdrożenia:

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

Zazwyczaj \*typ właściwości musi wskazywać na konkretnych, tworzone jako wystąpienia (za pośrednictwem publicznego ctor bez parametrów) implementacje [SymmetricAlgorithm](/dotnet/api/system.security.cryptography.symmetricalgorithm) i [KeyedHashAlgorithm](/dotnet/api/system.security.cryptography.keyedhashalgorithm), ale przypadki specjalne systemu niektórych wartości, takich jak `typeof(Aes)` dla wygody.

> [!NOTE]
> SymmetricAlgorithm musi mieć klucz o długości ≥ 128 bitów i blok o rozmiarze ≥ 64-bitowy, a musi obsługiwać szyfrowania w trybie CBC z dopełnienie PKCS #7. KeyedHashAlgorithm musi mieć rozmiar szyfrowanego > = 128 bitów i musi obsługiwać klucze o długości równej długości szyfrowanego algorytmem wyznaczania wartości skrótu. KeyedHashAlgorithm nie jest ściśle musi być HMAC.

### <a name="specifying-custom-windows-cng-algorithms"></a>Określanie niestandardowych algorytmów CNG systemu Windows

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Aby określić niestandardowy algorytm CNG systemu Windows przy użyciu szyfrowania w trybie CBC przy weryfikacji HMAC, Utwórz [CngCbcAuthenticatedEncryptorConfiguration](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.configurationmodel.cngcbcauthenticatedencryptorconfiguration) wystąpienia, który zawiera informacje algorytmicznego:

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

Aby określić niestandardowy algorytm CNG systemu Windows przy użyciu szyfrowania w trybie CBC przy weryfikacji HMAC, Utwórz [CngCbcAuthenticatedEncryptionSettings](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.cngcbcauthenticatedencryptionsettings) wystąpienia, który zawiera informacje algorytmicznego:

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
> Algorytm szyfrowania symetrycznego bloku musi mieć klucz o długości > = 128 bitów — blok o rozmiarze > = 64-bitowy, i musi obsługiwać szyfrowania w trybie CBC z dopełnienie PKCS #7. Algorytm wyznaczania wartości skrótu musi mieć rozmiar szyfrowanego > = 128 bitów i musi obsługiwać otwierany z BCRYPT\_ALG\_obsługi\_HMAC\_flagi flagi. \*Dostawcy właściwości można ustawić wartości null do używania domyślnego dostawcę dla określonego algorytmu. Zobacz [BCryptOpenAlgorithmProvider](https://msdn.microsoft.com/library/windows/desktop/aa375479(v=vs.85).aspx) dokumentacji, aby uzyskać więcej informacji.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Aby określić niestandardowy algorytm CNG systemu Windows przy użyciu szyfrowania tryb Galois liczników ze sprawdzaniem poprawności, Utwórz [CngGcmAuthenticatedEncryptorConfiguration](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.configurationmodel.cnggcmauthenticatedencryptorconfiguration) wystąpienia, który zawiera informacje algorytmicznego:

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

Aby określić niestandardowy algorytm CNG systemu Windows przy użyciu szyfrowania tryb Galois liczników ze sprawdzaniem poprawności, Utwórz [CngGcmAuthenticatedEncryptionSettings](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.cnggcmauthenticatedencryptionsettings) wystąpienia, który zawiera informacje algorytmicznego:

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
> Algorytm szyfrowania symetrycznego bloku musi mieć klucz o długości > = 128 bitów — blok o rozmiarze dokładnie 128 bitów i musi obsługiwać usługi GCM szyfrowania. Można ustawić [EncryptionAlgorithmProvider](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.configurationmodel.cngcbcauthenticatedencryptorconfiguration.encryptionalgorithmprovider) właściwości na wartość null, aby użyć domyślnego dostawcy dla określonego algorytmu. Zobacz [BCryptOpenAlgorithmProvider](https://msdn.microsoft.com/library/windows/desktop/aa375479(v=vs.85).aspx) dokumentacji, aby uzyskać więcej informacji.

### <a name="specifying-other-custom-algorithms"></a>Określanie inne algorytmy niestandardowych

Chociaż nie są widoczne jako najwyższej jakości interfejsu API, system ochrony danych jest rozszerzalny, aby umożliwić Określanie niemal każdego rodzaju algorytmu. Na przykład jest to możliwe, aby zachować wszystkich kluczy zawartych w sprzętowego modułu zabezpieczeń (HSM) i do implementacji niestandardowych rdzenia procedury szyfrowania i odszyfrowywania. Zobacz [IAuthenticatedEncryptor](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.iauthenticatedencryptor) w [podstawowe rozszerzalności kryptografii](xref:security/data-protection/extensibility/core-crypto) Aby uzyskać więcej informacji.

## <a name="persisting-keys-when-hosting-in-a-docker-container"></a>Utrwalanie klucze odnośnie do hostowania w kontenerze Docker

Odnośnie do hostowania w [Docker](/dotnet/standard/microservices-architecture/container-docker-introduction/) kontenera kluczy należy utrzymywać albo:

* Folder, który jest Docker woluminu, który będzie się powtarzać, poza okres istnienia kontenera, takich jak udostępnionego woluminu lub wolumin zainstalowany w hoście.
* Zewnętrznego dostawcy, takich jak [usługi Azure Key Vault](https://azure.microsoft.com/services/key-vault/) lub [Redis](https://redis.io/).

## <a name="see-also"></a>Zobacz także

* [Scenariusze pamiętać z systemem innym niż Podpisane](xref:security/data-protection/configuration/non-di-scenarios)
* [Międzynarodowe zasad komputera](xref:security/data-protection/configuration/machine-wide-policy)
