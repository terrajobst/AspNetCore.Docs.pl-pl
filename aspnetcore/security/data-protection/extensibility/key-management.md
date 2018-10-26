---
title: Rozszerzalność zarządzania kluczami w programie ASP.NET Core
author: rick-anderson
description: Więcej informacji na temat ochrony danych programu ASP.NET Core rozszerzalność zarządzania kluczami.
ms.author: riande
ms.custom: mvc
ms.date: 10/24/2018
uid: security/data-protection/extensibility/key-management
ms.openlocfilehash: 1cf3fc30f72fb872ff9d7f33fc5ffb12a11a982f
ms.sourcegitcommit: 4d74644f11e0dac52b4510048490ae731c691496
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/25/2018
ms.locfileid: "50090618"
---
# <a name="key-management-extensibility-in-aspnet-core"></a><span data-ttu-id="ed7a1-103">Rozszerzalność zarządzania kluczami w programie ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="ed7a1-103">Key management extensibility in ASP.NET Core</span></span>

> [!TIP]
> <span data-ttu-id="ed7a1-104">Odczyt [zarządzanie kluczami](xref:security/data-protection/implementation/key-management#data-protection-implementation-key-management) sekcji przed odczytaniem w tej sekcji, zgodnie z jego omówiono podstawowe pojęcia dotyczące tych interfejsów API.</span><span class="sxs-lookup"><span data-stu-id="ed7a1-104">Read the [key management](xref:security/data-protection/implementation/key-management#data-protection-implementation-key-management) section before reading this section, as it explains some of the fundamental concepts behind these APIs.</span></span>

> [!WARNING]
> <span data-ttu-id="ed7a1-105">Typy, które implementują żadnego z następujących interfejsów powinny być metodą o bezpiecznych wątkach dla wielu obiektów wywołujących.</span><span class="sxs-lookup"><span data-stu-id="ed7a1-105">Types that implement any of the following interfaces should be thread-safe for multiple callers.</span></span>

## <a name="key"></a><span data-ttu-id="ed7a1-106">Key</span><span class="sxs-lookup"><span data-stu-id="ed7a1-106">Key</span></span>

<span data-ttu-id="ed7a1-107">`IKey` Interfejs jest podstawowe reprezentacja klucza w cryptosystem.</span><span class="sxs-lookup"><span data-stu-id="ed7a1-107">The `IKey` interface is the basic representation of a key in cryptosystem.</span></span> <span data-ttu-id="ed7a1-108">Klucz termin jest używany w tym miejscu w sensie abstrakcyjne, nie w sensie literału "materiału klucza kryptograficznego".</span><span class="sxs-lookup"><span data-stu-id="ed7a1-108">The term key is used here in the abstract sense, not in the literal sense of "cryptographic key material".</span></span> <span data-ttu-id="ed7a1-109">Klucz ma następujące właściwości:</span><span class="sxs-lookup"><span data-stu-id="ed7a1-109">A key has the following properties:</span></span>

* <span data-ttu-id="ed7a1-110">Daty aktywacji, tworzenia i wygaśnięcia</span><span class="sxs-lookup"><span data-stu-id="ed7a1-110">Activation, creation, and expiration dates</span></span>

* <span data-ttu-id="ed7a1-111">Stan odwołania</span><span class="sxs-lookup"><span data-stu-id="ed7a1-111">Revocation status</span></span>

* <span data-ttu-id="ed7a1-112">Identyfikator klucza (GUID)</span><span class="sxs-lookup"><span data-stu-id="ed7a1-112">Key identifier (a GUID)</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="ed7a1-113">Ponadto `IKey` udostępnia `CreateEncryptor` metody, która może służyć do tworzenia [IAuthenticatedEncryptor](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptor) wystąpienia powiązany do tego klucza.</span><span class="sxs-lookup"><span data-stu-id="ed7a1-113">Additionally, `IKey` exposes a `CreateEncryptor` method which can be used to create an [IAuthenticatedEncryptor](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptor) instance tied to this key.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="ed7a1-114">Ponadto `IKey` udostępnia `CreateEncryptorInstance` metody, która może służyć do tworzenia [IAuthenticatedEncryptor](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptor) wystąpienia powiązany do tego klucza.</span><span class="sxs-lookup"><span data-stu-id="ed7a1-114">Additionally, `IKey` exposes a `CreateEncryptorInstance` method which can be used to create an [IAuthenticatedEncryptor](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptor) instance tied to this key.</span></span>

::: moniker-end

> [!NOTE]
> <span data-ttu-id="ed7a1-115">Nie ma żadnych interfejsów API można pobrać pierwotne materiałami kryptograficznymi z `IKey` wystąpienia.</span><span class="sxs-lookup"><span data-stu-id="ed7a1-115">There's no API to retrieve the raw cryptographic material from an `IKey` instance.</span></span>

## <a name="ikeymanager"></a><span data-ttu-id="ed7a1-116">IKeyManager</span><span class="sxs-lookup"><span data-stu-id="ed7a1-116">IKeyManager</span></span>

<span data-ttu-id="ed7a1-117">`IKeyManager` Interfejs reprezentuje odpowiedzialne za ogólne magazynu kluczy, pobieranie i modyfikowanie obiektu.</span><span class="sxs-lookup"><span data-stu-id="ed7a1-117">The `IKeyManager` interface represents an object responsible for general key storage, retrieval, and manipulation.</span></span> <span data-ttu-id="ed7a1-118">Udostępnia ona trzy operacje wysokiego poziomu:</span><span class="sxs-lookup"><span data-stu-id="ed7a1-118">It exposes three high-level operations:</span></span>

* <span data-ttu-id="ed7a1-119">Utwórz nowy klucz i jego utrwalać w magazynie.</span><span class="sxs-lookup"><span data-stu-id="ed7a1-119">Create a new key and persist it to storage.</span></span>

* <span data-ttu-id="ed7a1-120">Pobierz wszystkie klucze z magazynu.</span><span class="sxs-lookup"><span data-stu-id="ed7a1-120">Get all keys from storage.</span></span>

* <span data-ttu-id="ed7a1-121">Wycofaj klucze co najmniej jeden, i utrwalić informacje o odwołaniach do magazynu.</span><span class="sxs-lookup"><span data-stu-id="ed7a1-121">Revoke one or more keys and persist the revocation information to storage.</span></span>

>[!WARNING]
> <span data-ttu-id="ed7a1-122">Zapisywanie `IKeyManager` jest bardzo zaawansowanym zadaniem, a większość programistów nie należy spróbować go.</span><span class="sxs-lookup"><span data-stu-id="ed7a1-122">Writing an `IKeyManager` is a very advanced task, and the majority of developers shouldn't attempt it.</span></span> <span data-ttu-id="ed7a1-123">Zamiast tego większość deweloperów powinna korzystać z zalet możliwości oferowane przez [XmlKeyManager](#xmlkeymanager) klasy.</span><span class="sxs-lookup"><span data-stu-id="ed7a1-123">Instead, most developers should take advantage of the facilities offered by the [XmlKeyManager](#xmlkeymanager) class.</span></span>

## <a name="xmlkeymanager"></a><span data-ttu-id="ed7a1-124">XmlKeyManager</span><span class="sxs-lookup"><span data-stu-id="ed7a1-124">XmlKeyManager</span></span>

<span data-ttu-id="ed7a1-125">`XmlKeyManager` Typ jest skrzynkach odbiorczych wykonanie `IKeyManager`.</span><span class="sxs-lookup"><span data-stu-id="ed7a1-125">The `XmlKeyManager` type is the in-box concrete implementation of `IKeyManager`.</span></span> <span data-ttu-id="ed7a1-126">Zapewnia kilka przydatne urządzeń, w tym depozytu kluczy i szyfrowania kluczy w stanie spoczynku.</span><span class="sxs-lookup"><span data-stu-id="ed7a1-126">It provides several useful facilities, including key escrow and encryption of keys at rest.</span></span> <span data-ttu-id="ed7a1-127">Klucze, w tym systemie są reprezentowane jako elementy XML (w szczególności [XElement](/dotnet/csharp/programming-guide/concepts/linq/xelement-class-overview)).</span><span class="sxs-lookup"><span data-stu-id="ed7a1-127">Keys in this system are represented as XML elements (specifically, [XElement](/dotnet/csharp/programming-guide/concepts/linq/xelement-class-overview)).</span></span>

<span data-ttu-id="ed7a1-128">`XmlKeyManager` zależy od kilku składników w trakcie wypełniania jego zadań podrzędnych:</span><span class="sxs-lookup"><span data-stu-id="ed7a1-128">`XmlKeyManager` depends on several other components in the course of fulfilling its tasks:</span></span>

::: moniker range=">= aspnetcore-2.0"

* <span data-ttu-id="ed7a1-129">`AlgorithmConfiguration`, który decyduje o algorytmy, używany przez nowych kluczy.</span><span class="sxs-lookup"><span data-stu-id="ed7a1-129">`AlgorithmConfiguration`, which dictates the algorithms used by new keys.</span></span>

* <span data-ttu-id="ed7a1-130">`IXmlRepository`, które kontrolki, w którym klucze są utrwalane w magazynie.</span><span class="sxs-lookup"><span data-stu-id="ed7a1-130">`IXmlRepository`, which controls where keys are persisted in storage.</span></span>

* <span data-ttu-id="ed7a1-131">`IXmlEncryptor` [opcjonalnie], co umożliwia szyfrowanie kluczy w stanie spoczynku.</span><span class="sxs-lookup"><span data-stu-id="ed7a1-131">`IXmlEncryptor` [optional], which allows encrypting keys at rest.</span></span>

* <span data-ttu-id="ed7a1-132">`IKeyEscrowSink` [opcjonalnie], która umożliwia kluczy depozytu.</span><span class="sxs-lookup"><span data-stu-id="ed7a1-132">`IKeyEscrowSink` [optional], which provides key escrow services.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

* <span data-ttu-id="ed7a1-133">`IXmlRepository`, które kontrolki, w którym klucze są utrwalane w magazynie.</span><span class="sxs-lookup"><span data-stu-id="ed7a1-133">`IXmlRepository`, which controls where keys are persisted in storage.</span></span>

* <span data-ttu-id="ed7a1-134">`IXmlEncryptor` [opcjonalnie], co umożliwia szyfrowanie kluczy w stanie spoczynku.</span><span class="sxs-lookup"><span data-stu-id="ed7a1-134">`IXmlEncryptor` [optional], which allows encrypting keys at rest.</span></span>

* <span data-ttu-id="ed7a1-135">`IKeyEscrowSink` [opcjonalnie], która umożliwia kluczy depozytu.</span><span class="sxs-lookup"><span data-stu-id="ed7a1-135">`IKeyEscrowSink` [optional], which provides key escrow services.</span></span>

::: moniker-end

<span data-ttu-id="ed7a1-136">Poniżej przedstawiono ogólny diagramy, które wskazują, jak te składniki są połączone ze sobą w ramach `XmlKeyManager`.</span><span class="sxs-lookup"><span data-stu-id="ed7a1-136">Below are high-level diagrams which indicate how these components are wired together within `XmlKeyManager`.</span></span>

::: moniker range=">= aspnetcore-2.0"

![Tworzenie klucza](key-management/_static/keycreation2.png)

<span data-ttu-id="ed7a1-138">*Tworzenie klucza / CreateNewKey*</span><span class="sxs-lookup"><span data-stu-id="ed7a1-138">*Key Creation / CreateNewKey*</span></span>

<span data-ttu-id="ed7a1-139">W implementacji `CreateNewKey`, `AlgorithmConfiguration` składnik jest używany do tworzenia unikatowej `IAuthenticatedEncryptorDescriptor`, który następnie jest serializowana jako XML.</span><span class="sxs-lookup"><span data-stu-id="ed7a1-139">In the implementation of `CreateNewKey`, the `AlgorithmConfiguration` component is used to create a unique `IAuthenticatedEncryptorDescriptor`, which is then serialized as XML.</span></span> <span data-ttu-id="ed7a1-140">Jeśli występuje ujścia kluczy depozytu pierwotne XML (niezaszyfrowany) zapewnia ujścia w celu przechowywania długoterminowego.</span><span class="sxs-lookup"><span data-stu-id="ed7a1-140">If a key escrow sink is present, the raw (unencrypted) XML is provided to the sink for long-term storage.</span></span> <span data-ttu-id="ed7a1-141">Uruchomi za pośrednictwem niezaszyfrowanej XML `IXmlEncryptor` (jeśli jest to wymagane) do generowania zaszyfrowanych dokumentu XML.</span><span class="sxs-lookup"><span data-stu-id="ed7a1-141">The unencrypted XML is then run through an `IXmlEncryptor` (if required) to generate the encrypted XML document.</span></span> <span data-ttu-id="ed7a1-142">Ten dokument w zaszyfrowanej jest trwały do magazynu długoterminowego za pośrednictwem `IXmlRepository`.</span><span class="sxs-lookup"><span data-stu-id="ed7a1-142">This encrypted document is persisted to long-term storage via the `IXmlRepository`.</span></span> <span data-ttu-id="ed7a1-143">(Jeśli nie `IXmlEncryptor` jest skonfigurowany, niezaszyfrowane dokumentu są utrwalane w `IXmlRepository`.)</span><span class="sxs-lookup"><span data-stu-id="ed7a1-143">(If no `IXmlEncryptor` is configured, the unencrypted document is persisted in the `IXmlRepository`.)</span></span>

![Pobieranie klucza](key-management/_static/keyretrieval2.png)

::: moniker-end

::: moniker range="< aspnetcore-2.0"

![Tworzenie klucza](key-management/_static/keycreation1.png)

<span data-ttu-id="ed7a1-146">*Tworzenie klucza / CreateNewKey*</span><span class="sxs-lookup"><span data-stu-id="ed7a1-146">*Key Creation / CreateNewKey*</span></span>

<span data-ttu-id="ed7a1-147">W implementacji `CreateNewKey`, `IAuthenticatedEncryptorConfiguration` składnik jest używany do tworzenia unikatowej `IAuthenticatedEncryptorDescriptor`, który następnie jest serializowana jako XML.</span><span class="sxs-lookup"><span data-stu-id="ed7a1-147">In the implementation of `CreateNewKey`, the `IAuthenticatedEncryptorConfiguration` component is used to create a unique `IAuthenticatedEncryptorDescriptor`, which is then serialized as XML.</span></span> <span data-ttu-id="ed7a1-148">Jeśli występuje ujścia kluczy depozytu pierwotne XML (niezaszyfrowany) zapewnia ujścia w celu przechowywania długoterminowego.</span><span class="sxs-lookup"><span data-stu-id="ed7a1-148">If a key escrow sink is present, the raw (unencrypted) XML is provided to the sink for long-term storage.</span></span> <span data-ttu-id="ed7a1-149">Uruchomi za pośrednictwem niezaszyfrowanej XML `IXmlEncryptor` (jeśli jest to wymagane) do generowania zaszyfrowanych dokumentu XML.</span><span class="sxs-lookup"><span data-stu-id="ed7a1-149">The unencrypted XML is then run through an `IXmlEncryptor` (if required) to generate the encrypted XML document.</span></span> <span data-ttu-id="ed7a1-150">Ten dokument w zaszyfrowanej jest trwały do magazynu długoterminowego za pośrednictwem `IXmlRepository`.</span><span class="sxs-lookup"><span data-stu-id="ed7a1-150">This encrypted document is persisted to long-term storage via the `IXmlRepository`.</span></span> <span data-ttu-id="ed7a1-151">(Jeśli nie `IXmlEncryptor` jest skonfigurowany, niezaszyfrowane dokumentu są utrwalane w `IXmlRepository`.)</span><span class="sxs-lookup"><span data-stu-id="ed7a1-151">(If no `IXmlEncryptor` is configured, the unencrypted document is persisted in the `IXmlRepository`.)</span></span>

![Pobieranie klucza](key-management/_static/keyretrieval1.png)

::: moniker-end

<span data-ttu-id="ed7a1-153">*Pobieranie klucza / GetAllKeys*</span><span class="sxs-lookup"><span data-stu-id="ed7a1-153">*Key Retrieval / GetAllKeys*</span></span>

<span data-ttu-id="ed7a1-154">W implementacji `GetAllKeys`XML dokumenty reprezentujące kluczy i odwołania są odczytywane z bazowego `IXmlRepository`.</span><span class="sxs-lookup"><span data-stu-id="ed7a1-154">In the implementation of `GetAllKeys`, the XML documents representing keys and revocations are read from the underlying `IXmlRepository`.</span></span> <span data-ttu-id="ed7a1-155">Jeśli te dokumenty są zaszyfrowane, system automatycznie odszyfrować je.</span><span class="sxs-lookup"><span data-stu-id="ed7a1-155">If these documents are encrypted, the system will automatically decrypt them.</span></span> <span data-ttu-id="ed7a1-156">`XmlKeyManager` tworzy odpowiednie `IAuthenticatedEncryptorDescriptorDeserializer` wystąpień do deserializacji dokumenty z powrotem do `IAuthenticatedEncryptorDescriptor` wystąpień, które następnie są opakowane w poszczególnych `IKey` wystąpień.</span><span class="sxs-lookup"><span data-stu-id="ed7a1-156">`XmlKeyManager` creates the appropriate `IAuthenticatedEncryptorDescriptorDeserializer` instances to deserialize the documents back into `IAuthenticatedEncryptorDescriptor` instances, which are then wrapped in individual `IKey` instances.</span></span> <span data-ttu-id="ed7a1-157">Ta kolekcja `IKey` wystąpień jest zwracany do obiektu wywołującego.</span><span class="sxs-lookup"><span data-stu-id="ed7a1-157">This collection of `IKey` instances is returned to the caller.</span></span>

<span data-ttu-id="ed7a1-158">Więcej informacji na temat konkretne elementy XML można znaleźć w [magazynu kluczy Formatuj dokument](xref:security/data-protection/implementation/key-storage-format#data-protection-implementation-key-storage-format).</span><span class="sxs-lookup"><span data-stu-id="ed7a1-158">Further information on the particular XML elements can be found in the [key storage format document](xref:security/data-protection/implementation/key-storage-format#data-protection-implementation-key-storage-format).</span></span>

## <a name="ixmlrepository"></a><span data-ttu-id="ed7a1-159">IXmlRepository</span><span class="sxs-lookup"><span data-stu-id="ed7a1-159">IXmlRepository</span></span>

<span data-ttu-id="ed7a1-160">`IXmlRepository` Interfejs reprezentuje typ, który można utrwalić XML do aplikacji i pobierać XML z magazynu zapasowego.</span><span class="sxs-lookup"><span data-stu-id="ed7a1-160">The `IXmlRepository` interface represents a type that can persist XML to and retrieve XML from a backing store.</span></span> <span data-ttu-id="ed7a1-161">Udostępnia dwa interfejsy API:</span><span class="sxs-lookup"><span data-stu-id="ed7a1-161">It exposes two APIs:</span></span>

* <span data-ttu-id="ed7a1-162">`GetAllElements` :`IReadOnlyCollection<XElement>`</span><span class="sxs-lookup"><span data-stu-id="ed7a1-162">`GetAllElements` :`IReadOnlyCollection<XElement>`</span></span>

* `StoreElement(XElement element, string friendlyName)`

<span data-ttu-id="ed7a1-163">Implementacje `IXmlRepository` nie ma potrzeby nemohla analyzovat kód XML przechodzi przez nich.</span><span class="sxs-lookup"><span data-stu-id="ed7a1-163">Implementations of `IXmlRepository` don't need to parse the XML passing through them.</span></span> <span data-ttu-id="ed7a1-164">Powinny traktować dokumentów XML jako nieprzezroczysty i umożliwić wyższe warstwy martwienia się o generowania i analizowania dokumentów.</span><span class="sxs-lookup"><span data-stu-id="ed7a1-164">They should treat the XML documents as opaque and let higher layers worry about generating and parsing the documents.</span></span>

<span data-ttu-id="ed7a1-165">Istnieją cztery wbudowane konkretne typy, które implementują `IXmlRepository`:</span><span class="sxs-lookup"><span data-stu-id="ed7a1-165">There are four built-in concrete types which implement `IXmlRepository`:</span></span>

::: moniker range=">= aspnetcore-2.2"

* [<span data-ttu-id="ed7a1-166">FileSystemXmlRepository</span><span class="sxs-lookup"><span data-stu-id="ed7a1-166">FileSystemXmlRepository</span></span>](/dotnet/api/microsoft.aspnetcore.dataprotection.repositories.filesystemxmlrepository)
* [<span data-ttu-id="ed7a1-167">RegistryXmlRepository</span><span class="sxs-lookup"><span data-stu-id="ed7a1-167">RegistryXmlRepository</span></span>](/dotnet/api/microsoft.aspnetcore.dataprotection.repositories.registryxmlrepository)
* [<span data-ttu-id="ed7a1-168">AzureStorage.AzureBlobXmlRepository</span><span class="sxs-lookup"><span data-stu-id="ed7a1-168">AzureStorage.AzureBlobXmlRepository</span></span>](/dotnet/api/microsoft.aspnetcore.dataprotection.azurestorage.azureblobxmlrepository)
* [<span data-ttu-id="ed7a1-169">RedisXmlRepository</span><span class="sxs-lookup"><span data-stu-id="ed7a1-169">RedisXmlRepository</span></span>](/dotnet/api/microsoft.aspnetcore.dataprotection.stackexchangeredis.redisxmlrepository)

::: moniker-end

::: moniker range="< aspnetcore-2.2"

* [<span data-ttu-id="ed7a1-170">FileSystemXmlRepository</span><span class="sxs-lookup"><span data-stu-id="ed7a1-170">FileSystemXmlRepository</span></span>](/dotnet/api/microsoft.aspnetcore.dataprotection.repositories.filesystemxmlrepository)
* [<span data-ttu-id="ed7a1-171">RegistryXmlRepository</span><span class="sxs-lookup"><span data-stu-id="ed7a1-171">RegistryXmlRepository</span></span>](/dotnet/api/microsoft.aspnetcore.dataprotection.repositories.registryxmlrepository)
* [<span data-ttu-id="ed7a1-172">AzureStorage.AzureBlobXmlRepository</span><span class="sxs-lookup"><span data-stu-id="ed7a1-172">AzureStorage.AzureBlobXmlRepository</span></span>](/dotnet/api/microsoft.aspnetcore.dataprotection.azurestorage.azureblobxmlrepository)
* [<span data-ttu-id="ed7a1-173">RedisXmlRepository</span><span class="sxs-lookup"><span data-stu-id="ed7a1-173">RedisXmlRepository</span></span>](/dotnet/api/microsoft.aspnetcore.dataprotection.redisxmlrepository)

::: moniker-end

<span data-ttu-id="ed7a1-174">Zobacz [dokumentu dostawcy magazynu kluczy](xref:security/data-protection/implementation/key-storage-providers) Aby uzyskać więcej informacji.</span><span class="sxs-lookup"><span data-stu-id="ed7a1-174">See the [key storage providers document](xref:security/data-protection/implementation/key-storage-providers) for more information.</span></span>

<span data-ttu-id="ed7a1-175">Rejestrowanie niestandardowego `IXmlRepository` jest odpowiednie w przypadku korzystania z magazynu zapasowego inny (na przykład Azure Table Storage).</span><span class="sxs-lookup"><span data-stu-id="ed7a1-175">Registering a custom `IXmlRepository` is appropriate when using a different backing store (for example, Azure Table Storage).</span></span>

<span data-ttu-id="ed7a1-176">Aby zmienić domyślne repozytorium w całej aplikacji, Rejestrowanie niestandardowe `IXmlRepository` wystąpienie:</span><span class="sxs-lookup"><span data-stu-id="ed7a1-176">To change the default repository application-wide, register a custom `IXmlRepository` instance:</span></span>

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

## <a name="ixmlencryptor"></a><span data-ttu-id="ed7a1-177">IXmlEncryptor</span><span class="sxs-lookup"><span data-stu-id="ed7a1-177">IXmlEncryptor</span></span>

<span data-ttu-id="ed7a1-178">`IXmlEncryptor` Interfejs reprezentuje typ, który można zaszyfrować element XML w postaci zwykłego tekstu.</span><span class="sxs-lookup"><span data-stu-id="ed7a1-178">The `IXmlEncryptor` interface represents a type that can encrypt a plaintext XML element.</span></span> <span data-ttu-id="ed7a1-179">Udostępnia jeden interfejs API:</span><span class="sxs-lookup"><span data-stu-id="ed7a1-179">It exposes a single API:</span></span>

* <span data-ttu-id="ed7a1-180">Szyfrowanie (XElement plaintextElement): EncryptedXmlInfo</span><span class="sxs-lookup"><span data-stu-id="ed7a1-180">Encrypt(XElement plaintextElement) : EncryptedXmlInfo</span></span>

<span data-ttu-id="ed7a1-181">Jeśli jest to Zserializowany obiekt `IAuthenticatedEncryptorDescriptor` zawiera wszystkie elementy oznaczone jako "wymaga szyfrowania", następnie `XmlKeyManager` uruchomi te elementy za pomocą skonfigurowanych `IXmlEncryptor`firmy `Encrypt` metody, a utrwali enciphered elementu, a nie od zwykły tekst elementu `IXmlRepository`.</span><span class="sxs-lookup"><span data-stu-id="ed7a1-181">If a serialized `IAuthenticatedEncryptorDescriptor` contains any elements marked as "requires encryption", then `XmlKeyManager` will run those elements through the configured `IXmlEncryptor`'s `Encrypt` method, and it will persist the enciphered element rather than the plaintext element to the `IXmlRepository`.</span></span> <span data-ttu-id="ed7a1-182">Dane wyjściowe `Encrypt` metodą jest `EncryptedXmlInfo` obiektu.</span><span class="sxs-lookup"><span data-stu-id="ed7a1-182">The output of the `Encrypt` method is an `EncryptedXmlInfo` object.</span></span> <span data-ttu-id="ed7a1-183">Ten obiekt jest otoki, która zawiera wynikowe enciphered `XElement` i typ, który reprezentuje `IXmlDecryptor` który może służyć do odszyfrowania odpowiadający mu element.</span><span class="sxs-lookup"><span data-stu-id="ed7a1-183">This object is a wrapper which contains both the resultant enciphered `XElement` and the Type which represents an `IXmlDecryptor` which can be used to decipher the corresponding element.</span></span>

<span data-ttu-id="ed7a1-184">Istnieją cztery wbudowane konkretne typy, które implementują `IXmlEncryptor`:</span><span class="sxs-lookup"><span data-stu-id="ed7a1-184">There are four built-in concrete types which implement `IXmlEncryptor`:</span></span>

* [<span data-ttu-id="ed7a1-185">CertificateXmlEncryptor</span><span class="sxs-lookup"><span data-stu-id="ed7a1-185">CertificateXmlEncryptor</span></span>](/dotnet/api/microsoft.aspnetcore.dataprotection.xmlencryption.certificatexmlencryptor)
* [<span data-ttu-id="ed7a1-186">DpapiNGXmlEncryptor</span><span class="sxs-lookup"><span data-stu-id="ed7a1-186">DpapiNGXmlEncryptor</span></span>](/dotnet/api/microsoft.aspnetcore.dataprotection.xmlencryption.dpapingxmlencryptor)
* [<span data-ttu-id="ed7a1-187">DpapiXmlEncryptor</span><span class="sxs-lookup"><span data-stu-id="ed7a1-187">DpapiXmlEncryptor</span></span>](/dotnet/api/microsoft.aspnetcore.dataprotection.xmlencryption.dpapixmlencryptor)
* [<span data-ttu-id="ed7a1-188">NullXmlEncryptor</span><span class="sxs-lookup"><span data-stu-id="ed7a1-188">NullXmlEncryptor</span></span>](/dotnet/api/microsoft.aspnetcore.dataprotection.xmlencryption.nullxmlencryptor)

<span data-ttu-id="ed7a1-189">Zobacz [szyfrowanie kluczy podczas dokumentu rest](xref:security/data-protection/implementation/key-encryption-at-rest) Aby uzyskać więcej informacji.</span><span class="sxs-lookup"><span data-stu-id="ed7a1-189">See the [key encryption at rest document](xref:security/data-protection/implementation/key-encryption-at-rest) for more information.</span></span>

<span data-ttu-id="ed7a1-190">Aby zmienić klucz szyfrowania podczas spoczynku domyślnego mechanizmu całej aplikacji, Rejestrowanie niestandardowe `IXmlEncryptor` wystąpienie:</span><span class="sxs-lookup"><span data-stu-id="ed7a1-190">To change the default key-encryption-at-rest mechanism application-wide, register a custom `IXmlEncryptor` instance:</span></span>

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

## <a name="ixmldecryptor"></a><span data-ttu-id="ed7a1-191">IXmlDecryptor</span><span class="sxs-lookup"><span data-stu-id="ed7a1-191">IXmlDecryptor</span></span>

<span data-ttu-id="ed7a1-192">`IXmlDecryptor` Interfejs reprezentuje typ, który wie, jak można odszyfrować `XElement` , został enciphered za pośrednictwem `IXmlEncryptor`.</span><span class="sxs-lookup"><span data-stu-id="ed7a1-192">The `IXmlDecryptor` interface represents a type that knows how to decrypt an `XElement` that was enciphered via an `IXmlEncryptor`.</span></span> <span data-ttu-id="ed7a1-193">Udostępnia jeden interfejs API:</span><span class="sxs-lookup"><span data-stu-id="ed7a1-193">It exposes a single API:</span></span>

* <span data-ttu-id="ed7a1-194">Odszyfrowywanie (XElement encryptedElement): XElement</span><span class="sxs-lookup"><span data-stu-id="ed7a1-194">Decrypt(XElement encryptedElement) : XElement</span></span>

<span data-ttu-id="ed7a1-195">`Decrypt` Metoda cofa szyfrowania wykonywane przez `IXmlEncryptor.Encrypt`.</span><span class="sxs-lookup"><span data-stu-id="ed7a1-195">The `Decrypt` method undoes the encryption performed by `IXmlEncryptor.Encrypt`.</span></span> <span data-ttu-id="ed7a1-196">Ogólnie rzecz biorąc, w konkretny `IXmlEncryptor` wdrożenia będzie miał odpowiedniego konkretny `IXmlDecryptor` implementacji.</span><span class="sxs-lookup"><span data-stu-id="ed7a1-196">Generally, each concrete `IXmlEncryptor` implementation will have a corresponding concrete `IXmlDecryptor` implementation.</span></span>

<span data-ttu-id="ed7a1-197">Typów implementujących `IXmlDecryptor` powinien mieć jedną z następujących dwóch konstruktorów publicznych:</span><span class="sxs-lookup"><span data-stu-id="ed7a1-197">Types which implement `IXmlDecryptor` should have one of the following two public constructors:</span></span>

* <span data-ttu-id="ed7a1-198">.ctor(IServiceProvider)</span><span class="sxs-lookup"><span data-stu-id="ed7a1-198">.ctor(IServiceProvider)</span></span>
* <span data-ttu-id="ed7a1-199">.ctor()</span><span class="sxs-lookup"><span data-stu-id="ed7a1-199">.ctor()</span></span>

> [!NOTE]
> <span data-ttu-id="ed7a1-200">`IServiceProvider` Przekazany do konstruktora może mieć wartości null.</span><span class="sxs-lookup"><span data-stu-id="ed7a1-200">The `IServiceProvider` passed to the constructor may be null.</span></span>

## <a name="ikeyescrowsink"></a><span data-ttu-id="ed7a1-201">IKeyEscrowSink</span><span class="sxs-lookup"><span data-stu-id="ed7a1-201">IKeyEscrowSink</span></span>

<span data-ttu-id="ed7a1-202">`IKeyEscrowSink` Interfejs reprezentuje typ, który można wykonać depozytu poufne informacje.</span><span class="sxs-lookup"><span data-stu-id="ed7a1-202">The `IKeyEscrowSink` interface represents a type that can perform escrow of sensitive information.</span></span> <span data-ttu-id="ed7a1-203">Pamiętaj, że serializacji deskryptory mogą zawierać poufne informacje (na przykład materiałami kryptograficznymi) i jest to, co doprowadziło do wprowadzenia [IXmlEncryptor](#ixmlencryptor) wpisz w pierwszej kolejności.</span><span class="sxs-lookup"><span data-stu-id="ed7a1-203">Recall that serialized descriptors might contain sensitive information (such as cryptographic material), and this is what led to the introduction of the [IXmlEncryptor](#ixmlencryptor) type in the first place.</span></span> <span data-ttu-id="ed7a1-204">Jednak awarii i pierścieni klucz może zostać usunięty lub uszkodzony.</span><span class="sxs-lookup"><span data-stu-id="ed7a1-204">However, accidents happen, and key rings can be deleted or become corrupted.</span></span>

<span data-ttu-id="ed7a1-205">Interfejs depozytu dostarcza awaryjnym wyjście, zezwalania na dostęp do surowego serializacji XML przed jest przekształcana przez dowolne skonfigurowane [IXmlEncryptor](#ixmlencryptor).</span><span class="sxs-lookup"><span data-stu-id="ed7a1-205">The escrow interface provides an emergency escape hatch, allowing access to the raw serialized XML before it's transformed by any configured [IXmlEncryptor](#ixmlencryptor).</span></span> <span data-ttu-id="ed7a1-206">Interfejs udostępnia jeden interfejs API:</span><span class="sxs-lookup"><span data-stu-id="ed7a1-206">The interface exposes a single API:</span></span>

* <span data-ttu-id="ed7a1-207">Store (identyfikator Guid klucza, elementu XElement)</span><span class="sxs-lookup"><span data-stu-id="ed7a1-207">Store(Guid keyId, XElement element)</span></span>

<span data-ttu-id="ed7a1-208">Do `IKeyEscrowSink` wdrożenia do obsługi podany element w bezpieczny sposób zgodny z zasadami firmy.</span><span class="sxs-lookup"><span data-stu-id="ed7a1-208">It's up to the `IKeyEscrowSink` implementation to handle the provided element in a secure manner consistent with business policy.</span></span> <span data-ttu-id="ed7a1-209">Jedna implementacja możliwe może być ujścia depozytu można zaszyfrować element XML przy użyciu znanych certyfikat X.509, gdzie klucz prywatny certyfikatu został zdeponowane; `CertificateXmlEncryptor` typu mogą pomóc w tym.</span><span class="sxs-lookup"><span data-stu-id="ed7a1-209">One possible implementation could be for the escrow sink to encrypt the XML element using a known corporate X.509 certificate where the certificate's private key has been escrowed; the `CertificateXmlEncryptor` type can assist with this.</span></span> <span data-ttu-id="ed7a1-210">`IKeyEscrowSink` Implementacja jest również odpowiedzialne za utrwalanie podany element odpowiednio.</span><span class="sxs-lookup"><span data-stu-id="ed7a1-210">The `IKeyEscrowSink` implementation is also responsible for persisting the provided element appropriately.</span></span>

<span data-ttu-id="ed7a1-211">Żaden mechanizm depozytu jest domyślnie włączona, chociaż Administratorzy serwera mogą [skonfigurować globalnie](xref:security/data-protection/configuration/machine-wide-policy).</span><span class="sxs-lookup"><span data-stu-id="ed7a1-211">By default no escrow mechanism is enabled, though server administrators can [configure this globally](xref:security/data-protection/configuration/machine-wide-policy).</span></span> <span data-ttu-id="ed7a1-212">Jego można również skonfigurować programowo za pośrednictwem `IDataProtectionBuilder.AddKeyEscrowSink` metody, jak pokazano w poniższym przykładzie.</span><span class="sxs-lookup"><span data-stu-id="ed7a1-212">It can also be configured programmatically via the `IDataProtectionBuilder.AddKeyEscrowSink` method as shown in the sample below.</span></span> <span data-ttu-id="ed7a1-213">`AddKeyEscrowSink` Dublowanie przeciążenia metody `IServiceCollection.AddSingleton` i `IServiceCollection.AddInstance` przeciążeń, jako `IKeyEscrowSink` powinny być pojedynczych wystąpień.</span><span class="sxs-lookup"><span data-stu-id="ed7a1-213">The `AddKeyEscrowSink` method overloads mirror the `IServiceCollection.AddSingleton` and `IServiceCollection.AddInstance` overloads, as `IKeyEscrowSink` instances are intended to be singletons.</span></span> <span data-ttu-id="ed7a1-214">Jeśli wiele `IKeyEscrowSink` wystąpienia są zarejestrowane, każdy z nich zostanie wywołana podczas generowania kluczy, dzięki czemu klucze mogą być zdeponowane w wiele mechanizmów jednocześnie.</span><span class="sxs-lookup"><span data-stu-id="ed7a1-214">If multiple `IKeyEscrowSink` instances are registered, each one will be called during key generation, so keys can be escrowed to multiple mechanisms simultaneously.</span></span>

<span data-ttu-id="ed7a1-215">Nie ma żadnych interfejsów API można odczytać materiał z `IKeyEscrowSink` wystąpienia.</span><span class="sxs-lookup"><span data-stu-id="ed7a1-215">There's no API to read material from an `IKeyEscrowSink` instance.</span></span> <span data-ttu-id="ed7a1-216">Jest to zgodne z teorii projektowania mechanizmu depozytu: ma zamierza udostępnić materiału klucza dla zaufanego urzędu. Ponadto ponieważ aplikacja sama nie jest zaufany urząd, go nie powinny mieć dostępu do jego własnej escrowed materiałów.</span><span class="sxs-lookup"><span data-stu-id="ed7a1-216">This is consistent with the design theory of the escrow mechanism: it's intended to make the key material accessible to a trusted authority, and since the application is itself not a trusted authority, it shouldn't have access to its own escrowed material.</span></span>

<span data-ttu-id="ed7a1-217">Następujący przykładowy kod przedstawia tworzenie i rejestrowanie `IKeyEscrowSink` gdzie klucze są zdeponowane w taki sposób, że tylko członkowie "Administratorzy CONTOSODomain" je odzyskać.</span><span class="sxs-lookup"><span data-stu-id="ed7a1-217">The following sample code demonstrates creating and registering an `IKeyEscrowSink` where keys are escrowed such that only members of "CONTOSODomain Admins" can recover them.</span></span>

> [!NOTE]
> <span data-ttu-id="ed7a1-218">Aby uruchomić ten przykład, użytkownik musi być na przyłączonym do domeny systemu Windows 8 / machine w systemie Windows Server 2012 i kontrolera domeny musi być Windows Server 2012 lub nowszy.</span><span class="sxs-lookup"><span data-stu-id="ed7a1-218">To run this sample, you must be on a domain-joined Windows 8 / Windows Server 2012 machine, and the domain controller must be Windows Server 2012 or later.</span></span>

[!code-csharp[](key-management/samples/key-management-extensibility.cs)]
