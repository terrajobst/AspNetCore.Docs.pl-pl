---
title: Rozszerzalność zarządzania kluczami w programie ASP.NET Core
author: rick-anderson
description: Więcej informacji na temat ochrony danych programu ASP.NET Core rozszerzalność zarządzania kluczami.
ms.author: riande
ms.custom: mvc, seodec18
ms.date: 10/24/2018
uid: security/data-protection/extensibility/key-management
ms.openlocfilehash: 28932cbef1cc797338980f3e0de8b09caee324c0
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78665880"
---
# <a name="key-management-extensibility-in-aspnet-core"></a>Rozszerzalność zarządzania kluczami w programie ASP.NET Core

> [!TIP]
> Przeczytaj sekcję [Zarządzanie kluczami](xref:security/data-protection/implementation/key-management#data-protection-implementation-key-management) przed przeczytaniem tej sekcji, ponieważ objaśnia ona niektóre podstawowe koncepcje związane z tymi interfejsami API.

> [!WARNING]
> Typy, które implementują żadnego z następujących interfejsów powinny być metodą o bezpiecznych wątkach dla wielu obiektów wywołujących.

## <a name="key"></a>Klucz

Interfejs `IKey` jest podstawową reprezentacją klucza w cryptosystem. Klucz termin jest używany w tym miejscu w sensie abstrakcyjne, nie w sensie literału "materiału klucza kryptograficznego". Klucz ma następujące właściwości:

* Daty aktywacji, tworzenia i wygaśnięcia

* Stan odwołania

* Identyfikator klucza (GUID)

::: moniker range=">= aspnetcore-2.0"

Ponadto `IKey` uwidacznia metodę `CreateEncryptor`, która może służyć do tworzenia wystąpienia [IAuthenticatedEncryptor](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptor) powiązanego z tym kluczem.

::: moniker-end

::: moniker range="< aspnetcore-2.0"

Ponadto `IKey` uwidacznia metodę `CreateEncryptorInstance`, która może służyć do tworzenia wystąpienia [IAuthenticatedEncryptor](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptor) powiązanego z tym kluczem.

::: moniker-end

> [!NOTE]
> Nie istnieje interfejs API umożliwiający pobranie nieprzetworzonego materiału kryptograficznego z wystąpienia `IKey`.

## <a name="ikeymanager"></a>IKeyManager

Interfejs `IKeyManager` reprezentuje obiekt odpowiedzialny za ogólny Magazyn kluczy, pobieranie i manipulowanie. Udostępnia ona trzy operacje wysokiego poziomu:

* Utwórz nowy klucz i jego utrwalać w magazynie.

* Pobierz wszystkie klucze z magazynu.

* Wycofaj klucze co najmniej jeden, i utrwalić informacje o odwołaniach do magazynu.

>[!WARNING]
> Pisanie `IKeyManager` to bardzo zaawansowane zadanie i większość deweloperów nie powinna próbować tego. Zamiast tego większość deweloperów powinna korzystać z udogodnień oferowanych przez klasę [XmlKeyManager](#xmlkeymanager) .

## <a name="xmlkeymanager"></a>XmlKeyManager

Typ `XmlKeyManager` to autonomiczna implementacja `IKeyManager`. Zapewnia kilka przydatne urządzeń, w tym depozytu kluczy i szyfrowania kluczy w stanie spoczynku. Klucze w tym systemie są reprezentowane jako elementy XML (w odniesieniu do [XElement](/dotnet/csharp/programming-guide/concepts/linq/xelement-class-overview)).

`XmlKeyManager` zależy od kilku innych składników w trakcie wykonywania zadań:

::: moniker range=">= aspnetcore-2.0"

* `AlgorithmConfiguration`, który wymusza algorytmy używane przez nowe klucze.

* `IXmlRepository`, która kontroluje, gdzie klucze są utrwalane w magazynie.

* `IXmlEncryptor` [opcjonalny], co umożliwia szyfrowanie kluczy w spoczynku.

* `IKeyEscrowSink` [opcjonalny], który zapewnia usługi Key Escrow.

::: moniker-end

::: moniker range="< aspnetcore-2.0"

* `IXmlRepository`, która kontroluje, gdzie klucze są utrwalane w magazynie.

* `IXmlEncryptor` [opcjonalny], co umożliwia szyfrowanie kluczy w spoczynku.

* `IKeyEscrowSink` [opcjonalny], który zapewnia usługi Key Escrow.

::: moniker-end

Poniżej znajdują się diagramy wysokiego poziomu, które wskazują, jak te składniki są połączone ze sobą w `XmlKeyManager`.

::: moniker range=">= aspnetcore-2.0"

![Tworzenie klucza](key-management/_static/keycreation2.png)

*Tworzenie klucza/CreateNewKey*

W implementacji `CreateNewKey`składnik `AlgorithmConfiguration` służy do tworzenia unikatowego `IAuthenticatedEncryptorDescriptor`, który następnie jest serializowany w formacie XML. Jeśli występuje ujścia kluczy depozytu pierwotne XML (niezaszyfrowany) zapewnia ujścia w celu przechowywania długoterminowego. Niezaszyfrowaną zawartość XML jest następnie uruchamiana za pomocą `IXmlEncryptor` (jeśli jest to wymagane) w celu wygenerowania zaszyfrowanego dokumentu XML. Ten zaszyfrowany dokument jest trwały w magazynie długoterminowym za pośrednictwem `IXmlRepository`. (Jeśli żadna `IXmlEncryptor` nie jest skonfigurowana, niezaszyfrowane dokumenty zostaną utrwalone w `IXmlRepository`).

![Pobieranie klucza](key-management/_static/keyretrieval2.png)

::: moniker-end

::: moniker range="< aspnetcore-2.0"

![Tworzenie klucza](key-management/_static/keycreation1.png)

*Tworzenie klucza/CreateNewKey*

W implementacji `CreateNewKey`składnik `IAuthenticatedEncryptorConfiguration` służy do tworzenia unikatowego `IAuthenticatedEncryptorDescriptor`, który następnie jest serializowany w formacie XML. Jeśli występuje ujścia kluczy depozytu pierwotne XML (niezaszyfrowany) zapewnia ujścia w celu przechowywania długoterminowego. Niezaszyfrowaną zawartość XML jest następnie uruchamiana za pomocą `IXmlEncryptor` (jeśli jest to wymagane) w celu wygenerowania zaszyfrowanego dokumentu XML. Ten zaszyfrowany dokument jest trwały w magazynie długoterminowym za pośrednictwem `IXmlRepository`. (Jeśli żadna `IXmlEncryptor` nie jest skonfigurowana, niezaszyfrowane dokumenty zostaną utrwalone w `IXmlRepository`).

![Pobieranie klucza](key-management/_static/keyretrieval1.png)

::: moniker-end

*Pobieranie klucza/GetAllKeys*

W implementacji `GetAllKeys`dokumenty XML reprezentujące klucze i odwołania są odczytywane z bazowego `IXmlRepository`. Jeśli te dokumenty są zaszyfrowane, system automatycznie odszyfrować je. `XmlKeyManager` tworzy odpowiednie wystąpienia `IAuthenticatedEncryptorDescriptorDeserializer` do deserializacji dokumentów z powrotem do `IAuthenticatedEncryptorDescriptor` wystąpień, które są następnie opakowane w pojedyncze wystąpienia `IKey`. Ta kolekcja wystąpień `IKey` jest zwracana do obiektu wywołującego.

Więcej informacji o poszczególnych elementach XML można znaleźć w [dokumencie format magazynu kluczy](xref:security/data-protection/implementation/key-storage-format#data-protection-implementation-key-storage-format).

## <a name="ixmlrepository"></a>IXmlRepository

Interfejs `IXmlRepository` reprezentuje typ, który może utrwalać XML i pobierać XML z magazynu zapasowego. Udostępnia dwa interfejsy API:

* `GetAllElements`:`IReadOnlyCollection<XElement>`

* `StoreElement(XElement element, string friendlyName)`

Implementacje `IXmlRepository` nie muszą analizować przechodzenia do kodu XML. Powinny traktować dokumentów XML jako nieprzezroczysty i umożliwić wyższe warstwy martwienia się o generowania i analizowania dokumentów.

Istnieją cztery wbudowane typy, które implementują `IXmlRepository`:

::: moniker range=">= aspnetcore-2.2"

* [FileSystemXmlRepository](/dotnet/api/microsoft.aspnetcore.dataprotection.repositories.filesystemxmlrepository)
* [RegistryXmlRepository](/dotnet/api/microsoft.aspnetcore.dataprotection.repositories.registryxmlrepository)
* [AzureStorage. AzureBlobXmlRepository](/dotnet/api/microsoft.aspnetcore.dataprotection.azurestorage.azureblobxmlrepository)
* [RedisXmlRepository](/dotnet/api/microsoft.aspnetcore.dataprotection.stackexchangeredis.redisxmlrepository)

::: moniker-end

::: moniker range="< aspnetcore-2.2"

* [FileSystemXmlRepository](/dotnet/api/microsoft.aspnetcore.dataprotection.repositories.filesystemxmlrepository)
* [RegistryXmlRepository](/dotnet/api/microsoft.aspnetcore.dataprotection.repositories.registryxmlrepository)
* [AzureStorage. AzureBlobXmlRepository](/dotnet/api/microsoft.aspnetcore.dataprotection.azurestorage.azureblobxmlrepository)
* [RedisXmlRepository](/dotnet/api/microsoft.aspnetcore.dataprotection.redisxmlrepository)

::: moniker-end

Aby uzyskać więcej informacji, zobacz dokument dotyczący [dostawców magazynu kluczy](xref:security/data-protection/implementation/key-storage-providers) .

Rejestrowanie niestandardowego `IXmlRepository` jest odpowiednie w przypadku korzystania z innego magazynu zapasowego (na przykład Azure Table Storage).

Aby zmienić domyślne repozytorium dla całej aplikacji, zarejestruj wystąpienie `IXmlRepository` niestandardowego:

::: moniker range=">= aspnetcore-2.0"

```csharp
services.Configure<KeyManagementOptions>(options => options.XmlRepository = new MyCustomXmlRepository());
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
services.AddSingleton<IXmlRepository>(new MyCustomXmlRepository());
```

::: moniker-end

## <a name="ixmlencryptor"></a>IXmlEncryptor

Interfejs `IXmlEncryptor` reprezentuje typ, który może zaszyfrować element XML w postaci zwykłego tekstu. Udostępnia jeden interfejs API:

* Szyfrowanie (XElement plaintextElement): EncryptedXmlInfo

Jeśli serializowany `IAuthenticatedEncryptorDescriptor` zawiera wszystkie elementy oznaczone jako "wymaga szyfrowania", wówczas `XmlKeyManager` uruchomi te elementy za pośrednictwem skonfigurowanej metody `Encrypt` `IXmlEncryptor`i będzie utrzymywać element ENCIPHERED, a nie element w postaci zwykłego tekstu do `IXmlRepository`. Dane wyjściowe metody `Encrypt` są obiektem `EncryptedXmlInfo`. Ten obiekt jest otoką zawierającą zarówno wynikową ENCIPHERED `XElement`, jak i typ, który reprezentuje `IXmlDecryptor`, który może służyć do odszyfrowania odpowiadającego elementu.

Istnieją cztery wbudowane typy, które implementują `IXmlEncryptor`:

* [CertificateXmlEncryptor](/dotnet/api/microsoft.aspnetcore.dataprotection.xmlencryption.certificatexmlencryptor)
* [DpapiNGXmlEncryptor](/dotnet/api/microsoft.aspnetcore.dataprotection.xmlencryption.dpapingxmlencryptor)
* [DpapiXmlEncryptor](/dotnet/api/microsoft.aspnetcore.dataprotection.xmlencryption.dpapixmlencryptor)
* [NullXmlEncryptor](/dotnet/api/microsoft.aspnetcore.dataprotection.xmlencryption.nullxmlencryptor)

Aby uzyskać więcej informacji, zobacz sekcję [szyfrowanie kluczy w dokumencie REST](xref:security/data-protection/implementation/key-encryption-at-rest) .

Aby zmienić domyślny mechanizm szyfrowania klucza — w przypadku aplikacji, należy zarejestrować niestandardowe wystąpienie `IXmlEncryptor`:

::: moniker range=">= aspnetcore-2.0"

```csharp
services.Configure<KeyManagementOptions>(options => options.XmlEncryptor = new MyCustomXmlEncryptor());
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
services.AddSingleton<IXmlEncryptor>(new MyCustomXmlEncryptor());
```

::: moniker-end

## <a name="ixmldecryptor"></a>IXmlDecryptor

Interfejs `IXmlDecryptor` reprezentuje typ, który wie, jak odszyfrować `XElement`, który został ENCIPHERED za pośrednictwem `IXmlEncryptor`. Udostępnia jeden interfejs API:

* Odszyfrowywanie (XElement encryptedElement): XElement

Metoda `Decrypt` cofa szyfrowanie wykonywane przez `IXmlEncryptor.Encrypt`. Ogólnie rzecz biorąc każda konkretna implementacja `IXmlEncryptor` będzie miała odpowiednią konkretną `IXmlDecryptor` implementację.

Typy, które implementują `IXmlDecryptor` powinny mieć jeden z następujących dwóch konstruktorów publicznych:

* .ctor(IServiceProvider)
* .ctor()

> [!NOTE]
> `IServiceProvider` przeniesiona do konstruktora może mieć wartość null.

## <a name="ikeyescrowsink"></a>IKeyEscrowSink

Interfejs `IKeyEscrowSink` reprezentuje typ, który może prowadzić do Escrow informacji poufnych. Odwołaj te serializowane deskryptory mogą zawierać poufne informacje (takie jak materiał kryptograficzny) i to, co doprowadziło do wprowadzenia typu [IXmlEncryptor](#ixmlencryptor) w pierwszym miejscu. Jednak awarii i pierścieni klucz może zostać usunięty lub uszkodzony.

Interfejs Escrow zapewnia awaryjny kreskę ucieczki, umożliwiając dostęp do nieprzetworzonej serializowanej XML, zanim zostanie on przekształcony przez wszystkie skonfigurowane [IXmlEncryptor](#ixmlencryptor). Interfejs udostępnia jeden interfejs API:

* Store (identyfikator Guid klucza, elementu XElement)

Jest to wdrożenie `IKeyEscrowSink` do obsługi dostarczonego elementu w bezpieczny sposób spójny z zasadami biznesowymi. Jedną z możliwych implementacji dla ujścia usługi Escrow jest zaszyfrowanie elementu XML przy użyciu znanego certyfikatu firmowy X. 509, w którym został zgłoszony klucz prywatny certyfikatu. Typ `CertificateXmlEncryptor` może pomóc w tym. Implementacja `IKeyEscrowSink` jest również odpowiedzialna za utrwalanie podanego elementu.

Domyślnie żaden mechanizm Escrow nie jest włączony, jednak Administratorzy serwera mogą [konfigurować to globalnie](xref:security/data-protection/configuration/machine-wide-policy). Można go również skonfigurować programowo za pomocą metody `IDataProtectionBuilder.AddKeyEscrowSink`, jak pokazano w poniższym przykładzie. Przeciążanie metody `AddKeyEscrowSink` powoduje przeciążenia `IServiceCollection.AddSingleton` i `IServiceCollection.AddInstance` przeciążeń, ponieważ `IKeyEscrowSink` wystąpienia są przeznaczone jako pojedyncze. Jeśli zarejestrowano wiele wystąpień `IKeyEscrowSink`, każda z nich zostanie wywołana podczas generowania klucza, dzięki czemu klucze mogą być jednocześnie przełączone do wielu mechanizmów.

Brak interfejsu API do odczytu materiału z wystąpienia `IKeyEscrowSink`. Jest to zgodne z teorii projektowania mechanizmu depozytu: ma zamierza udostępnić materiału klucza dla zaufanego urzędu. Ponadto ponieważ aplikacja sama nie jest zaufany urząd, go nie powinny mieć dostępu do jego własnej escrowed materiałów.

Następujący przykładowy kod demonstruje tworzenie i rejestrowanie `IKeyEscrowSink`, w których klucze są objęte płatnością w taki sposób, że mogą je odzyskać tylko członkowie grupy "Administratorzy CONTOSODomain".

> [!NOTE]
> Aby uruchomić ten przykład, użytkownik musi być na przyłączonym do domeny systemu Windows 8 / machine w systemie Windows Server 2012 i kontrolera domeny musi być Windows Server 2012 lub nowszy.

[!code-csharp[](key-management/samples/key-management-extensibility.cs)]
