---
title: Rozszerzalność zarządzania kluczami w programie ASP.NET Core
author: rick-anderson
description: Więcej informacji na temat ochrony danych programu ASP.NET Core rozszerzalność zarządzania kluczami.
ms.author: riande
ms.custom: mvc, seodec18
ms.date: 10/24/2018
uid: security/data-protection/extensibility/key-management
ms.openlocfilehash: 28932cbef1cc797338980f3e0de8b09caee324c0
ms.sourcegitcommit: b34b25da2ab68e6495b2460ff570468f16a9bf0d
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/12/2018
ms.locfileid: "53284607"
---
# <a name="key-management-extensibility-in-aspnet-core"></a>Rozszerzalność zarządzania kluczami w programie ASP.NET Core

> [!TIP]
> Odczyt [zarządzanie kluczami](xref:security/data-protection/implementation/key-management#data-protection-implementation-key-management) sekcji przed odczytaniem w tej sekcji, zgodnie z jego omówiono podstawowe pojęcia dotyczące tych interfejsów API.

> [!WARNING]
> Typy, które implementują żadnego z następujących interfejsów powinny być metodą o bezpiecznych wątkach dla wielu obiektów wywołujących.

## <a name="key"></a>Key

`IKey` Interfejs jest podstawowe reprezentacja klucza w cryptosystem. Klucz termin jest używany w tym miejscu w sensie abstrakcyjne, nie w sensie literału "materiału klucza kryptograficznego". Klucz ma następujące właściwości:

* Daty aktywacji, tworzenia i wygaśnięcia

* Stan odwołania

* Identyfikator klucza (GUID)

::: moniker range=">= aspnetcore-2.0"

Ponadto `IKey` udostępnia `CreateEncryptor` metody, która może służyć do tworzenia [IAuthenticatedEncryptor](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptor) wystąpienia powiązany do tego klucza.

::: moniker-end

::: moniker range="< aspnetcore-2.0"

Ponadto `IKey` udostępnia `CreateEncryptorInstance` metody, która może służyć do tworzenia [IAuthenticatedEncryptor](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptor) wystąpienia powiązany do tego klucza.

::: moniker-end

> [!NOTE]
> Nie ma żadnych interfejsów API można pobrać pierwotne materiałami kryptograficznymi z `IKey` wystąpienia.

## <a name="ikeymanager"></a>IKeyManager

`IKeyManager` Interfejs reprezentuje odpowiedzialne za ogólne magazynu kluczy, pobieranie i modyfikowanie obiektu. Udostępnia ona trzy operacje wysokiego poziomu:

* Utwórz nowy klucz i jego utrwalać w magazynie.

* Pobierz wszystkie klucze z magazynu.

* Wycofaj klucze co najmniej jeden, i utrwalić informacje o odwołaniach do magazynu.

>[!WARNING]
> Zapisywanie `IKeyManager` jest bardzo zaawansowanym zadaniem, a większość programistów nie należy spróbować go. Zamiast tego większość deweloperów powinna korzystać z zalet możliwości oferowane przez [XmlKeyManager](#xmlkeymanager) klasy.

## <a name="xmlkeymanager"></a>XmlKeyManager

`XmlKeyManager` Typ jest skrzynkach odbiorczych wykonanie `IKeyManager`. Zapewnia kilka przydatne urządzeń, w tym depozytu kluczy i szyfrowania kluczy w stanie spoczynku. Klucze, w tym systemie są reprezentowane jako elementy XML (w szczególności [XElement](/dotnet/csharp/programming-guide/concepts/linq/xelement-class-overview)).

`XmlKeyManager` zależy od kilku składników w trakcie wypełniania jego zadań podrzędnych:

::: moniker range=">= aspnetcore-2.0"

* `AlgorithmConfiguration`, który decyduje o algorytmy, używany przez nowych kluczy.

* `IXmlRepository`, które kontrolki, w którym klucze są utrwalane w magazynie.

* `IXmlEncryptor` [opcjonalnie], co umożliwia szyfrowanie kluczy w stanie spoczynku.

* `IKeyEscrowSink` [opcjonalnie], która umożliwia kluczy depozytu.

::: moniker-end

::: moniker range="< aspnetcore-2.0"

* `IXmlRepository`, które kontrolki, w którym klucze są utrwalane w magazynie.

* `IXmlEncryptor` [opcjonalnie], co umożliwia szyfrowanie kluczy w stanie spoczynku.

* `IKeyEscrowSink` [opcjonalnie], która umożliwia kluczy depozytu.

::: moniker-end

Poniżej przedstawiono ogólny diagramy, które wskazują, jak te składniki są połączone ze sobą w ramach `XmlKeyManager`.

::: moniker range=">= aspnetcore-2.0"

![Tworzenie klucza](key-management/_static/keycreation2.png)

*Tworzenie klucza / CreateNewKey*

W implementacji `CreateNewKey`, `AlgorithmConfiguration` składnik jest używany do tworzenia unikatowej `IAuthenticatedEncryptorDescriptor`, który następnie jest serializowana jako XML. Jeśli występuje ujścia kluczy depozytu pierwotne XML (niezaszyfrowany) zapewnia ujścia w celu przechowywania długoterminowego. Uruchomi za pośrednictwem niezaszyfrowanej XML `IXmlEncryptor` (jeśli jest to wymagane) do generowania zaszyfrowanych dokumentu XML. Ten dokument w zaszyfrowanej jest trwały do magazynu długoterminowego za pośrednictwem `IXmlRepository`. (Jeśli nie `IXmlEncryptor` jest skonfigurowany, niezaszyfrowane dokumentu są utrwalane w `IXmlRepository`.)

![Pobieranie klucza](key-management/_static/keyretrieval2.png)

::: moniker-end

::: moniker range="< aspnetcore-2.0"

![Tworzenie klucza](key-management/_static/keycreation1.png)

*Tworzenie klucza / CreateNewKey*

W implementacji `CreateNewKey`, `IAuthenticatedEncryptorConfiguration` składnik jest używany do tworzenia unikatowej `IAuthenticatedEncryptorDescriptor`, który następnie jest serializowana jako XML. Jeśli występuje ujścia kluczy depozytu pierwotne XML (niezaszyfrowany) zapewnia ujścia w celu przechowywania długoterminowego. Uruchomi za pośrednictwem niezaszyfrowanej XML `IXmlEncryptor` (jeśli jest to wymagane) do generowania zaszyfrowanych dokumentu XML. Ten dokument w zaszyfrowanej jest trwały do magazynu długoterminowego za pośrednictwem `IXmlRepository`. (Jeśli nie `IXmlEncryptor` jest skonfigurowany, niezaszyfrowane dokumentu są utrwalane w `IXmlRepository`.)

![Pobieranie klucza](key-management/_static/keyretrieval1.png)

::: moniker-end

*Pobieranie klucza / GetAllKeys*

W implementacji `GetAllKeys`XML dokumenty reprezentujące kluczy i odwołania są odczytywane z bazowego `IXmlRepository`. Jeśli te dokumenty są zaszyfrowane, system automatycznie odszyfrować je. `XmlKeyManager` tworzy odpowiednie `IAuthenticatedEncryptorDescriptorDeserializer` wystąpień do deserializacji dokumenty z powrotem do `IAuthenticatedEncryptorDescriptor` wystąpień, które następnie są opakowane w poszczególnych `IKey` wystąpień. Ta kolekcja `IKey` wystąpień jest zwracany do obiektu wywołującego.

Więcej informacji na temat konkretne elementy XML można znaleźć w [magazynu kluczy Formatuj dokument](xref:security/data-protection/implementation/key-storage-format#data-protection-implementation-key-storage-format).

## <a name="ixmlrepository"></a>IXmlRepository

`IXmlRepository` Interfejs reprezentuje typ, który można utrwalić XML do aplikacji i pobierać XML z magazynu zapasowego. Udostępnia dwa interfejsy API:

* `GetAllElements` :`IReadOnlyCollection<XElement>`

* `StoreElement(XElement element, string friendlyName)`

Implementacje `IXmlRepository` nie ma potrzeby nemohla analyzovat kód XML przechodzi przez nich. Powinny traktować dokumentów XML jako nieprzezroczysty i umożliwić wyższe warstwy martwienia się o generowania i analizowania dokumentów.

Istnieją cztery wbudowane konkretne typy, które implementują `IXmlRepository`:

::: moniker range=">= aspnetcore-2.2"

* [FileSystemXmlRepository](/dotnet/api/microsoft.aspnetcore.dataprotection.repositories.filesystemxmlrepository)
* [RegistryXmlRepository](/dotnet/api/microsoft.aspnetcore.dataprotection.repositories.registryxmlrepository)
* [AzureStorage.AzureBlobXmlRepository](/dotnet/api/microsoft.aspnetcore.dataprotection.azurestorage.azureblobxmlrepository)
* [RedisXmlRepository](/dotnet/api/microsoft.aspnetcore.dataprotection.stackexchangeredis.redisxmlrepository)

::: moniker-end

::: moniker range="< aspnetcore-2.2"

* [FileSystemXmlRepository](/dotnet/api/microsoft.aspnetcore.dataprotection.repositories.filesystemxmlrepository)
* [RegistryXmlRepository](/dotnet/api/microsoft.aspnetcore.dataprotection.repositories.registryxmlrepository)
* [AzureStorage.AzureBlobXmlRepository](/dotnet/api/microsoft.aspnetcore.dataprotection.azurestorage.azureblobxmlrepository)
* [RedisXmlRepository](/dotnet/api/microsoft.aspnetcore.dataprotection.redisxmlrepository)

::: moniker-end

Zobacz [dokumentu dostawcy magazynu kluczy](xref:security/data-protection/implementation/key-storage-providers) Aby uzyskać więcej informacji.

Rejestrowanie niestandardowego `IXmlRepository` jest odpowiednie w przypadku korzystania z magazynu zapasowego inny (na przykład Azure Table Storage).

Aby zmienić domyślne repozytorium w całej aplikacji, Rejestrowanie niestandardowe `IXmlRepository` wystąpienie:

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

`IXmlEncryptor` Interfejs reprezentuje typ, który można zaszyfrować element XML w postaci zwykłego tekstu. Udostępnia jeden interfejs API:

* Encrypt(XElement plaintextElement): EncryptedXmlInfo

Jeśli jest to Zserializowany obiekt `IAuthenticatedEncryptorDescriptor` zawiera wszystkie elementy oznaczone jako "wymaga szyfrowania", następnie `XmlKeyManager` uruchomi te elementy za pomocą skonfigurowanych `IXmlEncryptor`firmy `Encrypt` metody, a utrwali enciphered elementu, a nie od zwykły tekst elementu `IXmlRepository`. Dane wyjściowe `Encrypt` metodą jest `EncryptedXmlInfo` obiektu. Ten obiekt jest otoki, która zawiera wynikowe enciphered `XElement` i typ, który reprezentuje `IXmlDecryptor` który może służyć do odszyfrowania odpowiadający mu element.

Istnieją cztery wbudowane konkretne typy, które implementują `IXmlEncryptor`:

* [CertificateXmlEncryptor](/dotnet/api/microsoft.aspnetcore.dataprotection.xmlencryption.certificatexmlencryptor)
* [DpapiNGXmlEncryptor](/dotnet/api/microsoft.aspnetcore.dataprotection.xmlencryption.dpapingxmlencryptor)
* [DpapiXmlEncryptor](/dotnet/api/microsoft.aspnetcore.dataprotection.xmlencryption.dpapixmlencryptor)
* [NullXmlEncryptor](/dotnet/api/microsoft.aspnetcore.dataprotection.xmlencryption.nullxmlencryptor)

Zobacz [szyfrowanie kluczy podczas dokumentu rest](xref:security/data-protection/implementation/key-encryption-at-rest) Aby uzyskać więcej informacji.

Aby zmienić klucz szyfrowania podczas spoczynku domyślnego mechanizmu całej aplikacji, Rejestrowanie niestandardowe `IXmlEncryptor` wystąpienie:

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

`IXmlDecryptor` Interfejs reprezentuje typ, który wie, jak można odszyfrować `XElement` , został enciphered za pośrednictwem `IXmlEncryptor`. Udostępnia jeden interfejs API:

* Odszyfrowywanie (XElement encryptedElement): XElement

`Decrypt` Metoda cofa szyfrowania wykonywane przez `IXmlEncryptor.Encrypt`. Ogólnie rzecz biorąc, w konkretny `IXmlEncryptor` wdrożenia będzie miał odpowiedniego konkretny `IXmlDecryptor` implementacji.

Typów implementujących `IXmlDecryptor` powinien mieć jedną z następujących dwóch konstruktorów publicznych:

* .ctor(IServiceProvider)
* .ctor()

> [!NOTE]
> `IServiceProvider` Przekazany do konstruktora może mieć wartości null.

## <a name="ikeyescrowsink"></a>IKeyEscrowSink

`IKeyEscrowSink` Interfejs reprezentuje typ, który można wykonać depozytu poufne informacje. Pamiętaj, że serializacji deskryptory mogą zawierać poufne informacje (na przykład materiałami kryptograficznymi) i jest to, co doprowadziło do wprowadzenia [IXmlEncryptor](#ixmlencryptor) wpisz w pierwszej kolejności. Jednak awarii i pierścieni klucz może zostać usunięty lub uszkodzony.

Interfejs depozytu dostarcza awaryjnym wyjście, zezwalania na dostęp do surowego serializacji XML przed jest przekształcana przez dowolne skonfigurowane [IXmlEncryptor](#ixmlencryptor). Interfejs udostępnia jeden interfejs API:

* Store (identyfikator Guid klucza, elementu XElement)

Do `IKeyEscrowSink` wdrożenia do obsługi podany element w bezpieczny sposób zgodny z zasadami firmy. Jedna implementacja możliwe może być ujścia depozytu można zaszyfrować element XML przy użyciu znanych certyfikat X.509, gdzie klucz prywatny certyfikatu został zdeponowane; `CertificateXmlEncryptor` typu mogą pomóc w tym. `IKeyEscrowSink` Implementacja jest również odpowiedzialne za utrwalanie podany element odpowiednio.

Żaden mechanizm depozytu jest domyślnie włączona, chociaż Administratorzy serwera mogą [skonfigurować globalnie](xref:security/data-protection/configuration/machine-wide-policy). Jego można również skonfigurować programowo za pośrednictwem `IDataProtectionBuilder.AddKeyEscrowSink` metody, jak pokazano w poniższym przykładzie. `AddKeyEscrowSink` Dublowanie przeciążenia metody `IServiceCollection.AddSingleton` i `IServiceCollection.AddInstance` przeciążeń, jako `IKeyEscrowSink` powinny być pojedynczych wystąpień. Jeśli wiele `IKeyEscrowSink` wystąpienia są zarejestrowane, każdy z nich zostanie wywołana podczas generowania kluczy, dzięki czemu klucze mogą być zdeponowane w wiele mechanizmów jednocześnie.

Nie ma żadnych interfejsów API można odczytać materiał z `IKeyEscrowSink` wystąpienia. Jest to zgodne z teorii projektowania mechanizmu depozytu: ma zamierza udostępnić materiału klucza dla zaufanego urzędu. Ponadto ponieważ aplikacja sama nie jest zaufany urząd, go nie powinny mieć dostępu do jego własnej escrowed materiałów.

Następujący przykładowy kod przedstawia tworzenie i rejestrowanie `IKeyEscrowSink` gdzie klucze są zdeponowane w taki sposób, że tylko członkowie "Administratorzy CONTOSODomain" je odzyskać.

> [!NOTE]
> Aby uruchomić ten przykład, użytkownik musi być na przyłączonym do domeny systemu Windows 8 / machine w systemie Windows Server 2012 i kontrolera domeny musi być Windows Server 2012 lub nowszy.

[!code-csharp[](key-management/samples/key-management-extensibility.cs)]
