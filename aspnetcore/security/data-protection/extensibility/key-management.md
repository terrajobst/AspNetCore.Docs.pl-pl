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
# <a name="key-management-extensibility-in-aspnet-core"></a><span data-ttu-id="f33be-103">Rozszerzalność zarządzania kluczami w programie ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="f33be-103">Key management extensibility in ASP.NET Core</span></span>

> [!TIP]
> <span data-ttu-id="f33be-104">Przeczytaj sekcję [Zarządzanie kluczami](xref:security/data-protection/implementation/key-management#data-protection-implementation-key-management) przed przeczytaniem tej sekcji, ponieważ objaśnia ona niektóre podstawowe koncepcje związane z tymi interfejsami API.</span><span class="sxs-lookup"><span data-stu-id="f33be-104">Read the [key management](xref:security/data-protection/implementation/key-management#data-protection-implementation-key-management) section before reading this section, as it explains some of the fundamental concepts behind these APIs.</span></span>

> [!WARNING]
> <span data-ttu-id="f33be-105">Typy, które implementują żadnego z następujących interfejsów powinny być metodą o bezpiecznych wątkach dla wielu obiektów wywołujących.</span><span class="sxs-lookup"><span data-stu-id="f33be-105">Types that implement any of the following interfaces should be thread-safe for multiple callers.</span></span>

## <a name="key"></a><span data-ttu-id="f33be-106">Klucz</span><span class="sxs-lookup"><span data-stu-id="f33be-106">Key</span></span>

<span data-ttu-id="f33be-107">Interfejs `IKey` jest podstawową reprezentacją klucza w cryptosystem.</span><span class="sxs-lookup"><span data-stu-id="f33be-107">The `IKey` interface is the basic representation of a key in cryptosystem.</span></span> <span data-ttu-id="f33be-108">Klucz termin jest używany w tym miejscu w sensie abstrakcyjne, nie w sensie literału "materiału klucza kryptograficznego".</span><span class="sxs-lookup"><span data-stu-id="f33be-108">The term key is used here in the abstract sense, not in the literal sense of "cryptographic key material".</span></span> <span data-ttu-id="f33be-109">Klucz ma następujące właściwości:</span><span class="sxs-lookup"><span data-stu-id="f33be-109">A key has the following properties:</span></span>

* <span data-ttu-id="f33be-110">Daty aktywacji, tworzenia i wygaśnięcia</span><span class="sxs-lookup"><span data-stu-id="f33be-110">Activation, creation, and expiration dates</span></span>

* <span data-ttu-id="f33be-111">Stan odwołania</span><span class="sxs-lookup"><span data-stu-id="f33be-111">Revocation status</span></span>

* <span data-ttu-id="f33be-112">Identyfikator klucza (GUID)</span><span class="sxs-lookup"><span data-stu-id="f33be-112">Key identifier (a GUID)</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="f33be-113">Ponadto `IKey` uwidacznia metodę `CreateEncryptor`, która może służyć do tworzenia wystąpienia [IAuthenticatedEncryptor](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptor) powiązanego z tym kluczem.</span><span class="sxs-lookup"><span data-stu-id="f33be-113">Additionally, `IKey` exposes a `CreateEncryptor` method which can be used to create an [IAuthenticatedEncryptor](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptor) instance tied to this key.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="f33be-114">Ponadto `IKey` uwidacznia metodę `CreateEncryptorInstance`, która może służyć do tworzenia wystąpienia [IAuthenticatedEncryptor](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptor) powiązanego z tym kluczem.</span><span class="sxs-lookup"><span data-stu-id="f33be-114">Additionally, `IKey` exposes a `CreateEncryptorInstance` method which can be used to create an [IAuthenticatedEncryptor](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptor) instance tied to this key.</span></span>

::: moniker-end

> [!NOTE]
> <span data-ttu-id="f33be-115">Nie istnieje interfejs API umożliwiający pobranie nieprzetworzonego materiału kryptograficznego z wystąpienia `IKey`.</span><span class="sxs-lookup"><span data-stu-id="f33be-115">There's no API to retrieve the raw cryptographic material from an `IKey` instance.</span></span>

## <a name="ikeymanager"></a><span data-ttu-id="f33be-116">IKeyManager</span><span class="sxs-lookup"><span data-stu-id="f33be-116">IKeyManager</span></span>

<span data-ttu-id="f33be-117">Interfejs `IKeyManager` reprezentuje obiekt odpowiedzialny za ogólny Magazyn kluczy, pobieranie i manipulowanie.</span><span class="sxs-lookup"><span data-stu-id="f33be-117">The `IKeyManager` interface represents an object responsible for general key storage, retrieval, and manipulation.</span></span> <span data-ttu-id="f33be-118">Udostępnia ona trzy operacje wysokiego poziomu:</span><span class="sxs-lookup"><span data-stu-id="f33be-118">It exposes three high-level operations:</span></span>

* <span data-ttu-id="f33be-119">Utwórz nowy klucz i jego utrwalać w magazynie.</span><span class="sxs-lookup"><span data-stu-id="f33be-119">Create a new key and persist it to storage.</span></span>

* <span data-ttu-id="f33be-120">Pobierz wszystkie klucze z magazynu.</span><span class="sxs-lookup"><span data-stu-id="f33be-120">Get all keys from storage.</span></span>

* <span data-ttu-id="f33be-121">Wycofaj klucze co najmniej jeden, i utrwalić informacje o odwołaniach do magazynu.</span><span class="sxs-lookup"><span data-stu-id="f33be-121">Revoke one or more keys and persist the revocation information to storage.</span></span>

>[!WARNING]
> <span data-ttu-id="f33be-122">Pisanie `IKeyManager` to bardzo zaawansowane zadanie i większość deweloperów nie powinna próbować tego.</span><span class="sxs-lookup"><span data-stu-id="f33be-122">Writing an `IKeyManager` is a very advanced task, and the majority of developers shouldn't attempt it.</span></span> <span data-ttu-id="f33be-123">Zamiast tego większość deweloperów powinna korzystać z udogodnień oferowanych przez klasę [XmlKeyManager](#xmlkeymanager) .</span><span class="sxs-lookup"><span data-stu-id="f33be-123">Instead, most developers should take advantage of the facilities offered by the [XmlKeyManager](#xmlkeymanager) class.</span></span>

## <a name="xmlkeymanager"></a><span data-ttu-id="f33be-124">XmlKeyManager</span><span class="sxs-lookup"><span data-stu-id="f33be-124">XmlKeyManager</span></span>

<span data-ttu-id="f33be-125">Typ `XmlKeyManager` to autonomiczna implementacja `IKeyManager`.</span><span class="sxs-lookup"><span data-stu-id="f33be-125">The `XmlKeyManager` type is the in-box concrete implementation of `IKeyManager`.</span></span> <span data-ttu-id="f33be-126">Zapewnia kilka przydatne urządzeń, w tym depozytu kluczy i szyfrowania kluczy w stanie spoczynku.</span><span class="sxs-lookup"><span data-stu-id="f33be-126">It provides several useful facilities, including key escrow and encryption of keys at rest.</span></span> <span data-ttu-id="f33be-127">Klucze w tym systemie są reprezentowane jako elementy XML (w odniesieniu do [XElement](/dotnet/csharp/programming-guide/concepts/linq/xelement-class-overview)).</span><span class="sxs-lookup"><span data-stu-id="f33be-127">Keys in this system are represented as XML elements (specifically, [XElement](/dotnet/csharp/programming-guide/concepts/linq/xelement-class-overview)).</span></span>

<span data-ttu-id="f33be-128">`XmlKeyManager` zależy od kilku innych składników w trakcie wykonywania zadań:</span><span class="sxs-lookup"><span data-stu-id="f33be-128">`XmlKeyManager` depends on several other components in the course of fulfilling its tasks:</span></span>

::: moniker range=">= aspnetcore-2.0"

* <span data-ttu-id="f33be-129">`AlgorithmConfiguration`, który wymusza algorytmy używane przez nowe klucze.</span><span class="sxs-lookup"><span data-stu-id="f33be-129">`AlgorithmConfiguration`, which dictates the algorithms used by new keys.</span></span>

* <span data-ttu-id="f33be-130">`IXmlRepository`, która kontroluje, gdzie klucze są utrwalane w magazynie.</span><span class="sxs-lookup"><span data-stu-id="f33be-130">`IXmlRepository`, which controls where keys are persisted in storage.</span></span>

* <span data-ttu-id="f33be-131">`IXmlEncryptor` [opcjonalny], co umożliwia szyfrowanie kluczy w spoczynku.</span><span class="sxs-lookup"><span data-stu-id="f33be-131">`IXmlEncryptor` [optional], which allows encrypting keys at rest.</span></span>

* <span data-ttu-id="f33be-132">`IKeyEscrowSink` [opcjonalny], który zapewnia usługi Key Escrow.</span><span class="sxs-lookup"><span data-stu-id="f33be-132">`IKeyEscrowSink` [optional], which provides key escrow services.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

* <span data-ttu-id="f33be-133">`IXmlRepository`, która kontroluje, gdzie klucze są utrwalane w magazynie.</span><span class="sxs-lookup"><span data-stu-id="f33be-133">`IXmlRepository`, which controls where keys are persisted in storage.</span></span>

* <span data-ttu-id="f33be-134">`IXmlEncryptor` [opcjonalny], co umożliwia szyfrowanie kluczy w spoczynku.</span><span class="sxs-lookup"><span data-stu-id="f33be-134">`IXmlEncryptor` [optional], which allows encrypting keys at rest.</span></span>

* <span data-ttu-id="f33be-135">`IKeyEscrowSink` [opcjonalny], który zapewnia usługi Key Escrow.</span><span class="sxs-lookup"><span data-stu-id="f33be-135">`IKeyEscrowSink` [optional], which provides key escrow services.</span></span>

::: moniker-end

<span data-ttu-id="f33be-136">Poniżej znajdują się diagramy wysokiego poziomu, które wskazują, jak te składniki są połączone ze sobą w `XmlKeyManager`.</span><span class="sxs-lookup"><span data-stu-id="f33be-136">Below are high-level diagrams which indicate how these components are wired together within `XmlKeyManager`.</span></span>

::: moniker range=">= aspnetcore-2.0"

![Tworzenie klucza](key-management/_static/keycreation2.png)

<span data-ttu-id="f33be-138">*Tworzenie klucza/CreateNewKey*</span><span class="sxs-lookup"><span data-stu-id="f33be-138">*Key Creation / CreateNewKey*</span></span>

<span data-ttu-id="f33be-139">W implementacji `CreateNewKey`składnik `AlgorithmConfiguration` służy do tworzenia unikatowego `IAuthenticatedEncryptorDescriptor`, który następnie jest serializowany w formacie XML.</span><span class="sxs-lookup"><span data-stu-id="f33be-139">In the implementation of `CreateNewKey`, the `AlgorithmConfiguration` component is used to create a unique `IAuthenticatedEncryptorDescriptor`, which is then serialized as XML.</span></span> <span data-ttu-id="f33be-140">Jeśli występuje ujścia kluczy depozytu pierwotne XML (niezaszyfrowany) zapewnia ujścia w celu przechowywania długoterminowego.</span><span class="sxs-lookup"><span data-stu-id="f33be-140">If a key escrow sink is present, the raw (unencrypted) XML is provided to the sink for long-term storage.</span></span> <span data-ttu-id="f33be-141">Niezaszyfrowaną zawartość XML jest następnie uruchamiana za pomocą `IXmlEncryptor` (jeśli jest to wymagane) w celu wygenerowania zaszyfrowanego dokumentu XML.</span><span class="sxs-lookup"><span data-stu-id="f33be-141">The unencrypted XML is then run through an `IXmlEncryptor` (if required) to generate the encrypted XML document.</span></span> <span data-ttu-id="f33be-142">Ten zaszyfrowany dokument jest trwały w magazynie długoterminowym za pośrednictwem `IXmlRepository`.</span><span class="sxs-lookup"><span data-stu-id="f33be-142">This encrypted document is persisted to long-term storage via the `IXmlRepository`.</span></span> <span data-ttu-id="f33be-143">(Jeśli żadna `IXmlEncryptor` nie jest skonfigurowana, niezaszyfrowane dokumenty zostaną utrwalone w `IXmlRepository`).</span><span class="sxs-lookup"><span data-stu-id="f33be-143">(If no `IXmlEncryptor` is configured, the unencrypted document is persisted in the `IXmlRepository`.)</span></span>

![Pobieranie klucza](key-management/_static/keyretrieval2.png)

::: moniker-end

::: moniker range="< aspnetcore-2.0"

![Tworzenie klucza](key-management/_static/keycreation1.png)

<span data-ttu-id="f33be-146">*Tworzenie klucza/CreateNewKey*</span><span class="sxs-lookup"><span data-stu-id="f33be-146">*Key Creation / CreateNewKey*</span></span>

<span data-ttu-id="f33be-147">W implementacji `CreateNewKey`składnik `IAuthenticatedEncryptorConfiguration` służy do tworzenia unikatowego `IAuthenticatedEncryptorDescriptor`, który następnie jest serializowany w formacie XML.</span><span class="sxs-lookup"><span data-stu-id="f33be-147">In the implementation of `CreateNewKey`, the `IAuthenticatedEncryptorConfiguration` component is used to create a unique `IAuthenticatedEncryptorDescriptor`, which is then serialized as XML.</span></span> <span data-ttu-id="f33be-148">Jeśli występuje ujścia kluczy depozytu pierwotne XML (niezaszyfrowany) zapewnia ujścia w celu przechowywania długoterminowego.</span><span class="sxs-lookup"><span data-stu-id="f33be-148">If a key escrow sink is present, the raw (unencrypted) XML is provided to the sink for long-term storage.</span></span> <span data-ttu-id="f33be-149">Niezaszyfrowaną zawartość XML jest następnie uruchamiana za pomocą `IXmlEncryptor` (jeśli jest to wymagane) w celu wygenerowania zaszyfrowanego dokumentu XML.</span><span class="sxs-lookup"><span data-stu-id="f33be-149">The unencrypted XML is then run through an `IXmlEncryptor` (if required) to generate the encrypted XML document.</span></span> <span data-ttu-id="f33be-150">Ten zaszyfrowany dokument jest trwały w magazynie długoterminowym za pośrednictwem `IXmlRepository`.</span><span class="sxs-lookup"><span data-stu-id="f33be-150">This encrypted document is persisted to long-term storage via the `IXmlRepository`.</span></span> <span data-ttu-id="f33be-151">(Jeśli żadna `IXmlEncryptor` nie jest skonfigurowana, niezaszyfrowane dokumenty zostaną utrwalone w `IXmlRepository`).</span><span class="sxs-lookup"><span data-stu-id="f33be-151">(If no `IXmlEncryptor` is configured, the unencrypted document is persisted in the `IXmlRepository`.)</span></span>

![Pobieranie klucza](key-management/_static/keyretrieval1.png)

::: moniker-end

<span data-ttu-id="f33be-153">*Pobieranie klucza/GetAllKeys*</span><span class="sxs-lookup"><span data-stu-id="f33be-153">*Key Retrieval / GetAllKeys*</span></span>

<span data-ttu-id="f33be-154">W implementacji `GetAllKeys`dokumenty XML reprezentujące klucze i odwołania są odczytywane z bazowego `IXmlRepository`.</span><span class="sxs-lookup"><span data-stu-id="f33be-154">In the implementation of `GetAllKeys`, the XML documents representing keys and revocations are read from the underlying `IXmlRepository`.</span></span> <span data-ttu-id="f33be-155">Jeśli te dokumenty są zaszyfrowane, system automatycznie odszyfrować je.</span><span class="sxs-lookup"><span data-stu-id="f33be-155">If these documents are encrypted, the system will automatically decrypt them.</span></span> <span data-ttu-id="f33be-156">`XmlKeyManager` tworzy odpowiednie wystąpienia `IAuthenticatedEncryptorDescriptorDeserializer` do deserializacji dokumentów z powrotem do `IAuthenticatedEncryptorDescriptor` wystąpień, które są następnie opakowane w pojedyncze wystąpienia `IKey`.</span><span class="sxs-lookup"><span data-stu-id="f33be-156">`XmlKeyManager` creates the appropriate `IAuthenticatedEncryptorDescriptorDeserializer` instances to deserialize the documents back into `IAuthenticatedEncryptorDescriptor` instances, which are then wrapped in individual `IKey` instances.</span></span> <span data-ttu-id="f33be-157">Ta kolekcja wystąpień `IKey` jest zwracana do obiektu wywołującego.</span><span class="sxs-lookup"><span data-stu-id="f33be-157">This collection of `IKey` instances is returned to the caller.</span></span>

<span data-ttu-id="f33be-158">Więcej informacji o poszczególnych elementach XML można znaleźć w [dokumencie format magazynu kluczy](xref:security/data-protection/implementation/key-storage-format#data-protection-implementation-key-storage-format).</span><span class="sxs-lookup"><span data-stu-id="f33be-158">Further information on the particular XML elements can be found in the [key storage format document](xref:security/data-protection/implementation/key-storage-format#data-protection-implementation-key-storage-format).</span></span>

## <a name="ixmlrepository"></a><span data-ttu-id="f33be-159">IXmlRepository</span><span class="sxs-lookup"><span data-stu-id="f33be-159">IXmlRepository</span></span>

<span data-ttu-id="f33be-160">Interfejs `IXmlRepository` reprezentuje typ, który może utrwalać XML i pobierać XML z magazynu zapasowego.</span><span class="sxs-lookup"><span data-stu-id="f33be-160">The `IXmlRepository` interface represents a type that can persist XML to and retrieve XML from a backing store.</span></span> <span data-ttu-id="f33be-161">Udostępnia dwa interfejsy API:</span><span class="sxs-lookup"><span data-stu-id="f33be-161">It exposes two APIs:</span></span>

* <span data-ttu-id="f33be-162">`GetAllElements`:`IReadOnlyCollection<XElement>`</span><span class="sxs-lookup"><span data-stu-id="f33be-162">`GetAllElements` :`IReadOnlyCollection<XElement>`</span></span>

* `StoreElement(XElement element, string friendlyName)`

<span data-ttu-id="f33be-163">Implementacje `IXmlRepository` nie muszą analizować przechodzenia do kodu XML.</span><span class="sxs-lookup"><span data-stu-id="f33be-163">Implementations of `IXmlRepository` don't need to parse the XML passing through them.</span></span> <span data-ttu-id="f33be-164">Powinny traktować dokumentów XML jako nieprzezroczysty i umożliwić wyższe warstwy martwienia się o generowania i analizowania dokumentów.</span><span class="sxs-lookup"><span data-stu-id="f33be-164">They should treat the XML documents as opaque and let higher layers worry about generating and parsing the documents.</span></span>

<span data-ttu-id="f33be-165">Istnieją cztery wbudowane typy, które implementują `IXmlRepository`:</span><span class="sxs-lookup"><span data-stu-id="f33be-165">There are four built-in concrete types which implement `IXmlRepository`:</span></span>

::: moniker range=">= aspnetcore-2.2"

* [<span data-ttu-id="f33be-166">FileSystemXmlRepository</span><span class="sxs-lookup"><span data-stu-id="f33be-166">FileSystemXmlRepository</span></span>](/dotnet/api/microsoft.aspnetcore.dataprotection.repositories.filesystemxmlrepository)
* [<span data-ttu-id="f33be-167">RegistryXmlRepository</span><span class="sxs-lookup"><span data-stu-id="f33be-167">RegistryXmlRepository</span></span>](/dotnet/api/microsoft.aspnetcore.dataprotection.repositories.registryxmlrepository)
* [<span data-ttu-id="f33be-168">AzureStorage. AzureBlobXmlRepository</span><span class="sxs-lookup"><span data-stu-id="f33be-168">AzureStorage.AzureBlobXmlRepository</span></span>](/dotnet/api/microsoft.aspnetcore.dataprotection.azurestorage.azureblobxmlrepository)
* [<span data-ttu-id="f33be-169">RedisXmlRepository</span><span class="sxs-lookup"><span data-stu-id="f33be-169">RedisXmlRepository</span></span>](/dotnet/api/microsoft.aspnetcore.dataprotection.stackexchangeredis.redisxmlrepository)

::: moniker-end

::: moniker range="< aspnetcore-2.2"

* [<span data-ttu-id="f33be-170">FileSystemXmlRepository</span><span class="sxs-lookup"><span data-stu-id="f33be-170">FileSystemXmlRepository</span></span>](/dotnet/api/microsoft.aspnetcore.dataprotection.repositories.filesystemxmlrepository)
* [<span data-ttu-id="f33be-171">RegistryXmlRepository</span><span class="sxs-lookup"><span data-stu-id="f33be-171">RegistryXmlRepository</span></span>](/dotnet/api/microsoft.aspnetcore.dataprotection.repositories.registryxmlrepository)
* [<span data-ttu-id="f33be-172">AzureStorage. AzureBlobXmlRepository</span><span class="sxs-lookup"><span data-stu-id="f33be-172">AzureStorage.AzureBlobXmlRepository</span></span>](/dotnet/api/microsoft.aspnetcore.dataprotection.azurestorage.azureblobxmlrepository)
* [<span data-ttu-id="f33be-173">RedisXmlRepository</span><span class="sxs-lookup"><span data-stu-id="f33be-173">RedisXmlRepository</span></span>](/dotnet/api/microsoft.aspnetcore.dataprotection.redisxmlrepository)

::: moniker-end

<span data-ttu-id="f33be-174">Aby uzyskać więcej informacji, zobacz dokument dotyczący [dostawców magazynu kluczy](xref:security/data-protection/implementation/key-storage-providers) .</span><span class="sxs-lookup"><span data-stu-id="f33be-174">See the [key storage providers document](xref:security/data-protection/implementation/key-storage-providers) for more information.</span></span>

<span data-ttu-id="f33be-175">Rejestrowanie niestandardowego `IXmlRepository` jest odpowiednie w przypadku korzystania z innego magazynu zapasowego (na przykład Azure Table Storage).</span><span class="sxs-lookup"><span data-stu-id="f33be-175">Registering a custom `IXmlRepository` is appropriate when using a different backing store (for example, Azure Table Storage).</span></span>

<span data-ttu-id="f33be-176">Aby zmienić domyślne repozytorium dla całej aplikacji, zarejestruj wystąpienie `IXmlRepository` niestandardowego:</span><span class="sxs-lookup"><span data-stu-id="f33be-176">To change the default repository application-wide, register a custom `IXmlRepository` instance:</span></span>

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

## <a name="ixmlencryptor"></a><span data-ttu-id="f33be-177">IXmlEncryptor</span><span class="sxs-lookup"><span data-stu-id="f33be-177">IXmlEncryptor</span></span>

<span data-ttu-id="f33be-178">Interfejs `IXmlEncryptor` reprezentuje typ, który może zaszyfrować element XML w postaci zwykłego tekstu.</span><span class="sxs-lookup"><span data-stu-id="f33be-178">The `IXmlEncryptor` interface represents a type that can encrypt a plaintext XML element.</span></span> <span data-ttu-id="f33be-179">Udostępnia jeden interfejs API:</span><span class="sxs-lookup"><span data-stu-id="f33be-179">It exposes a single API:</span></span>

* <span data-ttu-id="f33be-180">Szyfrowanie (XElement plaintextElement): EncryptedXmlInfo</span><span class="sxs-lookup"><span data-stu-id="f33be-180">Encrypt(XElement plaintextElement) : EncryptedXmlInfo</span></span>

<span data-ttu-id="f33be-181">Jeśli serializowany `IAuthenticatedEncryptorDescriptor` zawiera wszystkie elementy oznaczone jako "wymaga szyfrowania", wówczas `XmlKeyManager` uruchomi te elementy za pośrednictwem skonfigurowanej metody `Encrypt` `IXmlEncryptor`i będzie utrzymywać element ENCIPHERED, a nie element w postaci zwykłego tekstu do `IXmlRepository`.</span><span class="sxs-lookup"><span data-stu-id="f33be-181">If a serialized `IAuthenticatedEncryptorDescriptor` contains any elements marked as "requires encryption", then `XmlKeyManager` will run those elements through the configured `IXmlEncryptor`'s `Encrypt` method, and it will persist the enciphered element rather than the plaintext element to the `IXmlRepository`.</span></span> <span data-ttu-id="f33be-182">Dane wyjściowe metody `Encrypt` są obiektem `EncryptedXmlInfo`.</span><span class="sxs-lookup"><span data-stu-id="f33be-182">The output of the `Encrypt` method is an `EncryptedXmlInfo` object.</span></span> <span data-ttu-id="f33be-183">Ten obiekt jest otoką zawierającą zarówno wynikową ENCIPHERED `XElement`, jak i typ, który reprezentuje `IXmlDecryptor`, który może służyć do odszyfrowania odpowiadającego elementu.</span><span class="sxs-lookup"><span data-stu-id="f33be-183">This object is a wrapper which contains both the resultant enciphered `XElement` and the Type which represents an `IXmlDecryptor` which can be used to decipher the corresponding element.</span></span>

<span data-ttu-id="f33be-184">Istnieją cztery wbudowane typy, które implementują `IXmlEncryptor`:</span><span class="sxs-lookup"><span data-stu-id="f33be-184">There are four built-in concrete types which implement `IXmlEncryptor`:</span></span>

* [<span data-ttu-id="f33be-185">CertificateXmlEncryptor</span><span class="sxs-lookup"><span data-stu-id="f33be-185">CertificateXmlEncryptor</span></span>](/dotnet/api/microsoft.aspnetcore.dataprotection.xmlencryption.certificatexmlencryptor)
* [<span data-ttu-id="f33be-186">DpapiNGXmlEncryptor</span><span class="sxs-lookup"><span data-stu-id="f33be-186">DpapiNGXmlEncryptor</span></span>](/dotnet/api/microsoft.aspnetcore.dataprotection.xmlencryption.dpapingxmlencryptor)
* [<span data-ttu-id="f33be-187">DpapiXmlEncryptor</span><span class="sxs-lookup"><span data-stu-id="f33be-187">DpapiXmlEncryptor</span></span>](/dotnet/api/microsoft.aspnetcore.dataprotection.xmlencryption.dpapixmlencryptor)
* [<span data-ttu-id="f33be-188">NullXmlEncryptor</span><span class="sxs-lookup"><span data-stu-id="f33be-188">NullXmlEncryptor</span></span>](/dotnet/api/microsoft.aspnetcore.dataprotection.xmlencryption.nullxmlencryptor)

<span data-ttu-id="f33be-189">Aby uzyskać więcej informacji, zobacz sekcję [szyfrowanie kluczy w dokumencie REST](xref:security/data-protection/implementation/key-encryption-at-rest) .</span><span class="sxs-lookup"><span data-stu-id="f33be-189">See the [key encryption at rest document](xref:security/data-protection/implementation/key-encryption-at-rest) for more information.</span></span>

<span data-ttu-id="f33be-190">Aby zmienić domyślny mechanizm szyfrowania klucza — w przypadku aplikacji, należy zarejestrować niestandardowe wystąpienie `IXmlEncryptor`:</span><span class="sxs-lookup"><span data-stu-id="f33be-190">To change the default key-encryption-at-rest mechanism application-wide, register a custom `IXmlEncryptor` instance:</span></span>

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

## <a name="ixmldecryptor"></a><span data-ttu-id="f33be-191">IXmlDecryptor</span><span class="sxs-lookup"><span data-stu-id="f33be-191">IXmlDecryptor</span></span>

<span data-ttu-id="f33be-192">Interfejs `IXmlDecryptor` reprezentuje typ, który wie, jak odszyfrować `XElement`, który został ENCIPHERED za pośrednictwem `IXmlEncryptor`.</span><span class="sxs-lookup"><span data-stu-id="f33be-192">The `IXmlDecryptor` interface represents a type that knows how to decrypt an `XElement` that was enciphered via an `IXmlEncryptor`.</span></span> <span data-ttu-id="f33be-193">Udostępnia jeden interfejs API:</span><span class="sxs-lookup"><span data-stu-id="f33be-193">It exposes a single API:</span></span>

* <span data-ttu-id="f33be-194">Odszyfrowywanie (XElement encryptedElement): XElement</span><span class="sxs-lookup"><span data-stu-id="f33be-194">Decrypt(XElement encryptedElement) : XElement</span></span>

<span data-ttu-id="f33be-195">Metoda `Decrypt` cofa szyfrowanie wykonywane przez `IXmlEncryptor.Encrypt`.</span><span class="sxs-lookup"><span data-stu-id="f33be-195">The `Decrypt` method undoes the encryption performed by `IXmlEncryptor.Encrypt`.</span></span> <span data-ttu-id="f33be-196">Ogólnie rzecz biorąc każda konkretna implementacja `IXmlEncryptor` będzie miała odpowiednią konkretną `IXmlDecryptor` implementację.</span><span class="sxs-lookup"><span data-stu-id="f33be-196">Generally, each concrete `IXmlEncryptor` implementation will have a corresponding concrete `IXmlDecryptor` implementation.</span></span>

<span data-ttu-id="f33be-197">Typy, które implementują `IXmlDecryptor` powinny mieć jeden z następujących dwóch konstruktorów publicznych:</span><span class="sxs-lookup"><span data-stu-id="f33be-197">Types which implement `IXmlDecryptor` should have one of the following two public constructors:</span></span>

* <span data-ttu-id="f33be-198">.ctor(IServiceProvider)</span><span class="sxs-lookup"><span data-stu-id="f33be-198">.ctor(IServiceProvider)</span></span>
* <span data-ttu-id="f33be-199">.ctor()</span><span class="sxs-lookup"><span data-stu-id="f33be-199">.ctor()</span></span>

> [!NOTE]
> <span data-ttu-id="f33be-200">`IServiceProvider` przeniesiona do konstruktora może mieć wartość null.</span><span class="sxs-lookup"><span data-stu-id="f33be-200">The `IServiceProvider` passed to the constructor may be null.</span></span>

## <a name="ikeyescrowsink"></a><span data-ttu-id="f33be-201">IKeyEscrowSink</span><span class="sxs-lookup"><span data-stu-id="f33be-201">IKeyEscrowSink</span></span>

<span data-ttu-id="f33be-202">Interfejs `IKeyEscrowSink` reprezentuje typ, który może prowadzić do Escrow informacji poufnych.</span><span class="sxs-lookup"><span data-stu-id="f33be-202">The `IKeyEscrowSink` interface represents a type that can perform escrow of sensitive information.</span></span> <span data-ttu-id="f33be-203">Odwołaj te serializowane deskryptory mogą zawierać poufne informacje (takie jak materiał kryptograficzny) i to, co doprowadziło do wprowadzenia typu [IXmlEncryptor](#ixmlencryptor) w pierwszym miejscu.</span><span class="sxs-lookup"><span data-stu-id="f33be-203">Recall that serialized descriptors might contain sensitive information (such as cryptographic material), and this is what led to the introduction of the [IXmlEncryptor](#ixmlencryptor) type in the first place.</span></span> <span data-ttu-id="f33be-204">Jednak awarii i pierścieni klucz może zostać usunięty lub uszkodzony.</span><span class="sxs-lookup"><span data-stu-id="f33be-204">However, accidents happen, and key rings can be deleted or become corrupted.</span></span>

<span data-ttu-id="f33be-205">Interfejs Escrow zapewnia awaryjny kreskę ucieczki, umożliwiając dostęp do nieprzetworzonej serializowanej XML, zanim zostanie on przekształcony przez wszystkie skonfigurowane [IXmlEncryptor](#ixmlencryptor).</span><span class="sxs-lookup"><span data-stu-id="f33be-205">The escrow interface provides an emergency escape hatch, allowing access to the raw serialized XML before it's transformed by any configured [IXmlEncryptor](#ixmlencryptor).</span></span> <span data-ttu-id="f33be-206">Interfejs udostępnia jeden interfejs API:</span><span class="sxs-lookup"><span data-stu-id="f33be-206">The interface exposes a single API:</span></span>

* <span data-ttu-id="f33be-207">Store (identyfikator Guid klucza, elementu XElement)</span><span class="sxs-lookup"><span data-stu-id="f33be-207">Store(Guid keyId, XElement element)</span></span>

<span data-ttu-id="f33be-208">Jest to wdrożenie `IKeyEscrowSink` do obsługi dostarczonego elementu w bezpieczny sposób spójny z zasadami biznesowymi.</span><span class="sxs-lookup"><span data-stu-id="f33be-208">It's up to the `IKeyEscrowSink` implementation to handle the provided element in a secure manner consistent with business policy.</span></span> <span data-ttu-id="f33be-209">Jedną z możliwych implementacji dla ujścia usługi Escrow jest zaszyfrowanie elementu XML przy użyciu znanego certyfikatu firmowy X. 509, w którym został zgłoszony klucz prywatny certyfikatu. Typ `CertificateXmlEncryptor` może pomóc w tym.</span><span class="sxs-lookup"><span data-stu-id="f33be-209">One possible implementation could be for the escrow sink to encrypt the XML element using a known corporate X.509 certificate where the certificate's private key has been escrowed; the `CertificateXmlEncryptor` type can assist with this.</span></span> <span data-ttu-id="f33be-210">Implementacja `IKeyEscrowSink` jest również odpowiedzialna za utrwalanie podanego elementu.</span><span class="sxs-lookup"><span data-stu-id="f33be-210">The `IKeyEscrowSink` implementation is also responsible for persisting the provided element appropriately.</span></span>

<span data-ttu-id="f33be-211">Domyślnie żaden mechanizm Escrow nie jest włączony, jednak Administratorzy serwera mogą [konfigurować to globalnie](xref:security/data-protection/configuration/machine-wide-policy).</span><span class="sxs-lookup"><span data-stu-id="f33be-211">By default no escrow mechanism is enabled, though server administrators can [configure this globally](xref:security/data-protection/configuration/machine-wide-policy).</span></span> <span data-ttu-id="f33be-212">Można go również skonfigurować programowo za pomocą metody `IDataProtectionBuilder.AddKeyEscrowSink`, jak pokazano w poniższym przykładzie.</span><span class="sxs-lookup"><span data-stu-id="f33be-212">It can also be configured programmatically via the `IDataProtectionBuilder.AddKeyEscrowSink` method as shown in the sample below.</span></span> <span data-ttu-id="f33be-213">Przeciążanie metody `AddKeyEscrowSink` powoduje przeciążenia `IServiceCollection.AddSingleton` i `IServiceCollection.AddInstance` przeciążeń, ponieważ `IKeyEscrowSink` wystąpienia są przeznaczone jako pojedyncze.</span><span class="sxs-lookup"><span data-stu-id="f33be-213">The `AddKeyEscrowSink` method overloads mirror the `IServiceCollection.AddSingleton` and `IServiceCollection.AddInstance` overloads, as `IKeyEscrowSink` instances are intended to be singletons.</span></span> <span data-ttu-id="f33be-214">Jeśli zarejestrowano wiele wystąpień `IKeyEscrowSink`, każda z nich zostanie wywołana podczas generowania klucza, dzięki czemu klucze mogą być jednocześnie przełączone do wielu mechanizmów.</span><span class="sxs-lookup"><span data-stu-id="f33be-214">If multiple `IKeyEscrowSink` instances are registered, each one will be called during key generation, so keys can be escrowed to multiple mechanisms simultaneously.</span></span>

<span data-ttu-id="f33be-215">Brak interfejsu API do odczytu materiału z wystąpienia `IKeyEscrowSink`.</span><span class="sxs-lookup"><span data-stu-id="f33be-215">There's no API to read material from an `IKeyEscrowSink` instance.</span></span> <span data-ttu-id="f33be-216">Jest to zgodne z teorii projektowania mechanizmu depozytu: ma zamierza udostępnić materiału klucza dla zaufanego urzędu. Ponadto ponieważ aplikacja sama nie jest zaufany urząd, go nie powinny mieć dostępu do jego własnej escrowed materiałów.</span><span class="sxs-lookup"><span data-stu-id="f33be-216">This is consistent with the design theory of the escrow mechanism: it's intended to make the key material accessible to a trusted authority, and since the application is itself not a trusted authority, it shouldn't have access to its own escrowed material.</span></span>

<span data-ttu-id="f33be-217">Następujący przykładowy kod demonstruje tworzenie i rejestrowanie `IKeyEscrowSink`, w których klucze są objęte płatnością w taki sposób, że mogą je odzyskać tylko członkowie grupy "Administratorzy CONTOSODomain".</span><span class="sxs-lookup"><span data-stu-id="f33be-217">The following sample code demonstrates creating and registering an `IKeyEscrowSink` where keys are escrowed such that only members of "CONTOSODomain Admins" can recover them.</span></span>

> [!NOTE]
> <span data-ttu-id="f33be-218">Aby uruchomić ten przykład, użytkownik musi być na przyłączonym do domeny systemu Windows 8 / machine w systemie Windows Server 2012 i kontrolera domeny musi być Windows Server 2012 lub nowszy.</span><span class="sxs-lookup"><span data-stu-id="f33be-218">To run this sample, you must be on a domain-joined Windows 8 / Windows Server 2012 machine, and the domain controller must be Windows Server 2012 or later.</span></span>

[!code-csharp[](key-management/samples/key-management-extensibility.cs)]
