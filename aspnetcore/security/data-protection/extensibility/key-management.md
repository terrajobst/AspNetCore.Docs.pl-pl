---
title: Zarządzanie kluczami rozszerzalność na platformie ASP.NET Core
author: rick-anderson
description: Więcej informacji na temat ochrony danych platformy ASP.NET Core rozszerzalności zarządzania kluczami.
manager: wpickett
ms.author: riande
ms.date: 11/22/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/data-protection/extensibility/key-management
ms.openlocfilehash: e3042b371cf7be8fa0218c1906042d2810b180e3
ms.sourcegitcommit: 48beecfe749ddac52bc79aa3eb246a2dcdaa1862
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/22/2018
---
# <a name="key-management-extensibility-in-aspnet-core"></a>Zarządzanie kluczami rozszerzalność na platformie ASP.NET Core

<a name="data-protection-extensibility-key-management"></a>

>[!TIP]
> Odczyt [zarządzanie kluczami](xref:security/data-protection/implementation/key-management#data-protection-implementation-key-management) sekcji przed przeczytaniem tej części, jak wyjaśniono niektóre podstawowe pojęcia dotyczące tych interfejsów API.

>[!WARNING]
> Typy implementujące żadnego z następujących interfejsów powinny być bezpieczne wątkowo dla wielu wywołań.

## <a name="key"></a>Key

`IKey` Interfejs jest podstawowe reprezentacja klucza w cryptosystem. Klucz termin jest używany tutaj w tym sensie abstrakcyjne nie znajduje się w rozumieniu literału "materiału klucza kryptograficznego". Klucz ma następujące właściwości:

* Daty aktywacji, tworzenie i wygaśnięcia

* Stan odwołania

* Identyfikator klucza (GUID)

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Ponadto `IKey` przedstawia `CreateEncryptor` metodę, która może służyć do tworzenia [IAuthenticatedEncryptor](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptor) wystąpienia powiązane z danym kluczem.

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Ponadto `IKey` przedstawia `CreateEncryptorInstance` metodę, która może służyć do tworzenia [IAuthenticatedEncryptor](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptor) wystąpienia powiązane z danym kluczem.

---

> [!NOTE]
> Brak żadnego interfejsu API można pobrać raw materiał kryptograficznych z `IKey` wystąpienia.

## <a name="ikeymanager"></a>IKeyManager

`IKeyManager` Interfejsu reprezentuje obiekt odpowiedzialny za ogólne magazynu kluczy, pobieranie i manipulowania nimi. Udostępnia on trzy operacje wysokiego poziomu:

* Utwórz nowy klucz i zachować go do magazynu.

* Pobierz wszystkie klucze z magazynu.

* Odwołaj klucze co najmniej jednego i zachować informacje o odwołaniu do magazynu.

>[!WARNING]
> Zapisywanie `IKeyManager` jest bardzo zaawansowane zadania, a większość deweloperów nie powinny podejmować go. Zamiast tego większość deweloperów należy wykorzystać możliwości oferowane przez [XmlKeyManager](xref:security/data-protection/extensibility/key-management#data-protection-extensibility-key-management-xmlkeymanager) klasy.

<a name="data-protection-extensibility-key-management-xmlkeymanager"></a>

## <a name="xmlkeymanager"></a>XmlKeyManager

`XmlKeyManager` w polu wykonanie jest typu `IKeyManager`. Udostępnia kilka przydatne urządzeń, w tym depozytu kluczy i szyfrowania kluczy w stanie spoczynku. Klucze w tym systemie są reprezentowane jako elementy XML (w szczególności [klasy XElement](https://docs.microsoft.com/dotnet/csharp/programming-guide/concepts/linq/xelement-class-overview)).

`XmlKeyManager` zależy od kilku składników w trakcie wypełnienia swoich zadań:

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

* `AlgorithmConfiguration`, które nakazują algorytmów używanych przez nowych kluczy.

* `IXmlRepository`, które kontrolki, w której klucze są utrwalane w magazynie.

* `IXmlEncryptor` [opcjonalnie], co pozwala na szyfrowanie kluczy w stanie spoczynku.

* `IKeyEscrowSink` [opcjonalnie], która udostępnia usługi kluczy depozytu.

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

* `IXmlRepository`, które kontrolki, w której klucze są utrwalane w magazynie.

* `IXmlEncryptor` [opcjonalnie], co pozwala na szyfrowanie kluczy w stanie spoczynku.

* `IKeyEscrowSink` [opcjonalnie], która udostępnia usługi kluczy depozytu.

---

Poniżej przedstawiono diagramy wysokiego poziomu, które wskazują, jak te składniki są połączone ze sobą w `XmlKeyManager`.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

   ![Tworzenie kluczy](key-management/_static/keycreation2.png)

   *Tworzenie klucza / CreateNewKey*

W implementacji `CreateNewKey`, `AlgorithmConfiguration` składnik jest używany do utworzenia unikatowego `IAuthenticatedEncryptorDescriptor`, która jest następnie zserializowanym w formacie XML. Jeśli występuje zbiornika kluczy depozytu raw XML (niezaszyfrowany) zapewnia sink do długoterminowego przechowywania. Uruchom za pośrednictwem nieszyfrowanego XML `IXmlEncryptor` (jeśli jest to wymagane) do generowania zaszyfrowanego dokumentu XML. Ten dokument zaszyfrowanych jest trwały do magazynu długoterminowego za pośrednictwem `IXmlRepository`. (Jeśli nie `IXmlEncryptor` jest skonfigurowany, niezaszyfrowane dokumentu jest utrwalona w `IXmlRepository`.)

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

   ![Tworzenie kluczy](key-management/_static/keycreation1.png)

   *Tworzenie klucza / CreateNewKey*

W implementacji `CreateNewKey`, `IAuthenticatedEncryptorConfiguration` składnik jest używany do utworzenia unikatowego `IAuthenticatedEncryptorDescriptor`, która jest następnie zserializowanym w formacie XML. Jeśli występuje zbiornika kluczy depozytu raw XML (niezaszyfrowany) zapewnia sink do długoterminowego przechowywania. Uruchom za pośrednictwem nieszyfrowanego XML `IXmlEncryptor` (jeśli jest to wymagane) do generowania zaszyfrowanego dokumentu XML. Ten dokument zaszyfrowanych jest trwały do magazynu długoterminowego za pośrednictwem `IXmlRepository`. (Jeśli nie `IXmlEncryptor` jest skonfigurowany, niezaszyfrowane dokumentu jest utrwalona w `IXmlRepository`.)

---

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

   ![Pobieranie klucza](key-management/_static/keyretrieval2.png)
   
# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

   ![Pobieranie klucza](key-management/_static/keyretrieval1.png)

---

   *Klucz pobierania / GetAllKeys*

W implementacji `GetAllKeys`, reprezentująca klucze dokumentów XML i odwołań są odczytywane z podstawową `IXmlRepository`. Jeśli te dokumenty są szyfrowane, system automatycznie odszyfrować je. `XmlKeyManager` tworzy odpowiednie `IAuthenticatedEncryptorDescriptorDeserializer` wystąpień do deserializacji dokumenty z powrotem do `IAuthenticatedEncryptorDescriptor` wystąpienia, które następnie są ujęte w poszczególnych `IKey` wystąpień. Ta kolekcja `IKey` wystąpień jest zwracany do obiektu wywołującego.

Więcej informacji na temat konkretnego elementów XML znajduje się w [dokumentu w formacie magazynu kluczy](xref:security/data-protection/implementation/key-storage-format#data-protection-implementation-key-storage-format).

## <a name="ixmlrepository"></a>IXmlRepository

`IXmlRepository` Interfejsu reprezentuje typ, który można utrwalić XML i pobrać XML z magazynu zapasowego. Udostępnia ona dwóch interfejsów API:

* GetAllElements(): IReadOnlyCollection<XElement>

* StoreElement (element klasy XElement, friendlyName ciąg)

Implementacje `IXmlRepository` nie ma potrzeby analizy kodu XML przechodzącej przez nich. Powinny traktować jako nieprzezroczyste dokumentów XML i umożliwić wyższych warstw martwić generowania i analizowania dokumentów.

Istnieją dwa typy konkretnych wbudowanych implementujące `IXmlRepository`: `FileSystemXmlRepository` i `RegistryXmlRepository`. Zobacz [dokumentu dostawców magazynu kluczy](xref:security/data-protection/implementation/key-storage-providers#data-protection-implementation-key-storage-providers) Aby uzyskać więcej informacji. Rejestrowanie niestandardowe `IXmlRepository` będzie odpowiedni sposób używania magazynu zapasowego różnych, np. magazyn obiektów Blob Azure.

Aby zmienić domyślne repozytorium całej aplikacji, rejestrowania niestandardowego `IXmlRepository` wystąpienie:

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

   ```csharp
   services.Configure<KeyManagementOptions>(options => options.XmlRepository = new MyCustomXmlRepository());
   ```
   
# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

   ```csharp
   services.AddSingleton<IXmlRepository>(new MyCustomXmlRepository());
   ```

---

<a name="data-protection-extensibility-key-management-ixmlencryptor"></a>

## <a name="ixmlencryptor"></a>IXmlEncryptor

`IXmlEncryptor` Interfejsu reprezentuje typ, który można zaszyfrować element XML w postaci zwykłego tekstu. Udostępnia ona jednego interfejsu API:

* Szyfrowanie (plaintextElement klasy XElement): EncryptedXmlInfo

W przypadku serializacji `IAuthenticatedEncryptorDescriptor` zawiera wszystkie elementy oznaczone jako "wymaga szyfrowania", następnie `XmlKeyManager` uruchomi tych elementów przy użyciu skonfigurowanego `IXmlEncryptor`w `Encrypt` — metoda która zostanie utrzymana enciphered elementu zamiast element w postaci zwykłego tekstu do `IXmlRepository`. Dane wyjściowe `Encrypt` jest metoda `EncryptedXmlInfo` obiektu. Ten obiekt jest otoki, który zawiera wynikowe enciphered `XElement` i typ, który reprezentuje `IXmlDecryptor` które mogą służyć do odszyfrowania odpowiadającego mu elementu.

Istnieją cztery wbudowanych typów konkretnych implementujące `IXmlEncryptor`:
* `CertificateXmlEncryptor`
* `DpapiNGXmlEncryptor`
* `DpapiXmlEncryptor`
* `NullXmlEncryptor`

Zobacz [klucza szyfrowania w dokumencie rest](xref:security/data-protection/implementation/key-encryption-at-rest#data-protection-implementation-key-encryption-at-rest) Aby uzyskać więcej informacji.

Aby zmienić domyślny mechanizm klucza szyfrowania w rest całej aplikacji, rejestrowania niestandardowego `IXmlEncryptor` wystąpienie:

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

   ```csharp
   services.Configure<KeyManagementOptions>(options => options.XmlEncryptor = new MyCustomXmlEncryptor());
   ```
   
# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

   ```csharp
   services.AddSingleton<IXmlEncryptor>(new MyCustomXmlEncryptor());
   ```

---

## <a name="ixmldecryptor"></a>IXmlDecryptor

`IXmlDecryptor` Interfejsu reprezentuje typ, który potrafi do odszyfrowania `XElement` który został enciphered za pośrednictwem `IXmlEncryptor`. Udostępnia ona jednego interfejsu API:

* Odszyfrowywanie (encryptedElement klasy XElement): klasy XElement

`Decrypt` Metody cofa szyfrowania wykonywane przez `IXmlEncryptor.Encrypt`. Ogólnie rzecz biorąc, każdy konkretnych `IXmlEncryptor` implementacji będą miały odpowiednich konkretnych `IXmlDecryptor` implementacji.

Typów implementujących `IXmlDecryptor` powinny mieć jedno z następujących dwóch konstruktorów publicznych:

* .ctor(IServiceProvider)
* .ctor()

> [!NOTE]
> `IServiceProvider` Przekazany do konstruktora może mieć wartości null.

## <a name="ikeyescrowsink"></a>IKeyEscrowSink

`IKeyEscrowSink` Interfejsu reprezentuje typ, który można wykonać depozytu poufne informacje. Odwołaj serializacji deskryptory mogą zawierać poufne informacje (na przykład materiały kryptograficznych), czy jest to, co spowodowało wprowadzenie [IXmlEncryptor](xref:security/data-protection/extensibility/key-management#data-protection-extensibility-key-management-ixmlencryptor) wpisz w pierwszej kolejności. Jednak awarii, a klucz sygnałów mogą zostać usunięte lub uszkodzony.

Interfejs depozytu zapewnia kreskowania ewakuacji, umożliwiający dostęp do nieprzetworzonej serializacji XML przed jest przekształcana przez dowolne skonfigurowane [IXmlEncryptor](xref:security/data-protection/extensibility/key-management#data-protection-extensibility-key-management-ixmlencryptor). Interfejs udostępnia jeden interfejs API:

* Magazyn (identyfikator Guid klucza, element klasy XElement)

Do `IKeyEscrowSink` implementacji do obsługi podany element w bezpieczny sposób zgodne z zasadami firmowymi. Można jedna implementacja możliwe dla odbiorcy depozytu do szyfrowania elementu XML przy użyciu znanych firmowej certyfikatu X.509, gdzie klucz prywatny certyfikatu ma zdeponowane; `CertificateXmlEncryptor` typu może to pomóc. `IKeyEscrowSink` Implementacji również jest odpowiedzialny za odpowiednio utrwalanie podany element.

Mechanizm depozytu jest domyślnie włączona, ale administratorzy serwera mogą [to skonfigurować globalnie](xref:security/data-protection/configuration/machine-wide-policy). Może również być skonfigurowana programowo za pośrednictwem `IDataProtectionBuilder.AddKeyEscrowSink` metody, jak pokazano w poniższym przykładzie. `AddKeyEscrowSink` Dublowany przeciążenia metody `IServiceCollection.AddSingleton` i `IServiceCollection.AddInstance` przeciążenia, jako `IKeyEscrowSink` powinny być pojedynczych wystąpień. Jeśli wiele `IKeyEscrowSink` zarejestrowanych wystąpień, każdy z nich zostanie wywołana podczas generowania kluczy, więc kluczy można zdeponowane w wielu mechanizmów jednocześnie.

Brak żadnego interfejsu API można odczytać materiału z `IKeyEscrowSink` wystąpienia. Jest to zgodne z teorii projektu mechanizmu depozytu: ma zamierza udostępnić materiału klucza do zaufanego urzędu certyfikacji, a ponieważ aplikacja się nie jest zaufany urząd, nie powinien on mieć dostęp do własnej escrowed materiału.

Następujący przykładowy kod przedstawia tworzenie i rejestrowanie `IKeyEscrowSink` gdzie klucze są zdeponowane w taki sposób, aby je odzyskać tylko elementy członkowskie "CONTOSODomain Administratorzy".

> [!NOTE]
> Aby uruchomić ten przykład, użytkownik musi być na przyłączonych do domeny systemu Windows 8 / maszyny z systemem Windows Server 2012, a kontroler domeny musi być systemu Windows Server 2012 lub nowszym.

[!code-csharp[](key-management/samples/key-management-extensibility.cs)]
