---
title: "Rozszerzalność zarządzania kluczami"
author: rick-anderson
description: "W tym dokumencie przedstawiono rozszerzalności zarządzania kluczami ochrony danych platformy ASP.NET Core."
ms.author: riande
manager: wpickett
ms.date: 11/22/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/extensibility/key-management
ms.openlocfilehash: d748ee9d3edf9eed4285fab447d5b379dfcd937c
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/24/2018
---
# <a name="key-management-extensibility"></a><span data-ttu-id="ae3d2-103">Rozszerzalność zarządzania kluczami</span><span class="sxs-lookup"><span data-stu-id="ae3d2-103">Key management extensibility</span></span>

<a name="data-protection-extensibility-key-management"></a>

>[!TIP]
> <span data-ttu-id="ae3d2-104">Odczyt [zarządzanie kluczami](../implementation/key-management.md#data-protection-implementation-key-management) sekcji przed przeczytaniem tej części, jak wyjaśniono niektóre podstawowe pojęcia dotyczące tych interfejsów API.</span><span class="sxs-lookup"><span data-stu-id="ae3d2-104">Read the [key management](../implementation/key-management.md#data-protection-implementation-key-management) section before reading this section, as it explains some of the fundamental concepts behind these APIs.</span></span>

>[!WARNING]
> <span data-ttu-id="ae3d2-105">Typy implementujące żadnego z następujących interfejsów powinny być bezpieczne wątkowo dla wielu wywołań.</span><span class="sxs-lookup"><span data-stu-id="ae3d2-105">Types that implement any of the following interfaces should be thread-safe for multiple callers.</span></span>

## <a name="key"></a><span data-ttu-id="ae3d2-106">Key</span><span class="sxs-lookup"><span data-stu-id="ae3d2-106">Key</span></span>

<span data-ttu-id="ae3d2-107">`IKey` Interfejs jest podstawowe reprezentacja klucza w cryptosystem.</span><span class="sxs-lookup"><span data-stu-id="ae3d2-107">The `IKey` interface is the basic representation of a key in cryptosystem.</span></span> <span data-ttu-id="ae3d2-108">Klucz termin jest używany tutaj w tym sensie abstrakcyjne nie znajduje się w rozumieniu literału "materiału klucza kryptograficznego".</span><span class="sxs-lookup"><span data-stu-id="ae3d2-108">The term key is used here in the abstract sense, not in the literal sense of "cryptographic key material".</span></span> <span data-ttu-id="ae3d2-109">Klucz ma następujące właściwości:</span><span class="sxs-lookup"><span data-stu-id="ae3d2-109">A key has the following properties:</span></span>

* <span data-ttu-id="ae3d2-110">Daty aktywacji, tworzenie i wygaśnięcia</span><span class="sxs-lookup"><span data-stu-id="ae3d2-110">Activation, creation, and expiration dates</span></span>

* <span data-ttu-id="ae3d2-111">Stan odwołania</span><span class="sxs-lookup"><span data-stu-id="ae3d2-111">Revocation status</span></span>

* <span data-ttu-id="ae3d2-112">Identyfikator klucza (GUID)</span><span class="sxs-lookup"><span data-stu-id="ae3d2-112">Key identifier (a GUID)</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="ae3d2-113">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="ae3d2-113">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="ae3d2-114">Ponadto `IKey` przedstawia `CreateEncryptor` metodę, która może służyć do tworzenia [IAuthenticatedEncryptor](core-crypto.md#data-protection-extensibility-core-crypto-iauthenticatedencryptor) wystąpienia powiązane z danym kluczem.</span><span class="sxs-lookup"><span data-stu-id="ae3d2-114">Additionally, `IKey` exposes a `CreateEncryptor` method which can be used to create an [IAuthenticatedEncryptor](core-crypto.md#data-protection-extensibility-core-crypto-iauthenticatedencryptor) instance tied to this key.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="ae3d2-115">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="ae3d2-115">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="ae3d2-116">Ponadto `IKey` przedstawia `CreateEncryptorInstance` metodę, która może służyć do tworzenia [IAuthenticatedEncryptor](core-crypto.md#data-protection-extensibility-core-crypto-iauthenticatedencryptor) wystąpienia powiązane z danym kluczem.</span><span class="sxs-lookup"><span data-stu-id="ae3d2-116">Additionally, `IKey` exposes a `CreateEncryptorInstance` method which can be used to create an [IAuthenticatedEncryptor](core-crypto.md#data-protection-extensibility-core-crypto-iauthenticatedencryptor) instance tied to this key.</span></span>

---

> [!NOTE]
> <span data-ttu-id="ae3d2-117">Brak żadnego interfejsu API można pobrać raw materiał kryptograficznych z `IKey` wystąpienia.</span><span class="sxs-lookup"><span data-stu-id="ae3d2-117">There's no API to retrieve the raw cryptographic material from an `IKey` instance.</span></span>

## <a name="ikeymanager"></a><span data-ttu-id="ae3d2-118">IKeyManager</span><span class="sxs-lookup"><span data-stu-id="ae3d2-118">IKeyManager</span></span>

<span data-ttu-id="ae3d2-119">`IKeyManager` Interfejsu reprezentuje obiekt odpowiedzialny za ogólne magazynu kluczy, pobieranie i manipulowania nimi.</span><span class="sxs-lookup"><span data-stu-id="ae3d2-119">The `IKeyManager` interface represents an object responsible for general key storage, retrieval, and manipulation.</span></span> <span data-ttu-id="ae3d2-120">Udostępnia on trzy operacje wysokiego poziomu:</span><span class="sxs-lookup"><span data-stu-id="ae3d2-120">It exposes three high-level operations:</span></span>

* <span data-ttu-id="ae3d2-121">Utwórz nowy klucz i zachować go do magazynu.</span><span class="sxs-lookup"><span data-stu-id="ae3d2-121">Create a new key and persist it to storage.</span></span>

* <span data-ttu-id="ae3d2-122">Pobierz wszystkie klucze z magazynu.</span><span class="sxs-lookup"><span data-stu-id="ae3d2-122">Get all keys from storage.</span></span>

* <span data-ttu-id="ae3d2-123">Odwołaj klucze co najmniej jednego i zachować informacje o odwołaniu do magazynu.</span><span class="sxs-lookup"><span data-stu-id="ae3d2-123">Revoke one or more keys and persist the revocation information to storage.</span></span>

>[!WARNING]
> <span data-ttu-id="ae3d2-124">Zapisywanie `IKeyManager` jest bardzo zaawansowane zadania, a większość deweloperów nie powinny podejmować go.</span><span class="sxs-lookup"><span data-stu-id="ae3d2-124">Writing an `IKeyManager` is a very advanced task, and the majority of developers shouldn't attempt it.</span></span> <span data-ttu-id="ae3d2-125">Zamiast tego większość deweloperów należy wykorzystać możliwości oferowane przez [XmlKeyManager](xref:security/data-protection/extensibility/key-management#data-protection-extensibility-key-management-xmlkeymanager) klasy.</span><span class="sxs-lookup"><span data-stu-id="ae3d2-125">Instead, most developers should take advantage of the facilities offered by the [XmlKeyManager](xref:security/data-protection/extensibility/key-management#data-protection-extensibility-key-management-xmlkeymanager) class.</span></span>

<a name="data-protection-extensibility-key-management-xmlkeymanager"></a>

## <a name="xmlkeymanager"></a><span data-ttu-id="ae3d2-126">XmlKeyManager</span><span class="sxs-lookup"><span data-stu-id="ae3d2-126">XmlKeyManager</span></span>

<span data-ttu-id="ae3d2-127">`XmlKeyManager` w polu wykonanie jest typu `IKeyManager`.</span><span class="sxs-lookup"><span data-stu-id="ae3d2-127">The `XmlKeyManager` type is the in-box concrete implementation of `IKeyManager`.</span></span> <span data-ttu-id="ae3d2-128">Udostępnia kilka przydatne urządzeń, w tym depozytu kluczy i szyfrowania kluczy w stanie spoczynku.</span><span class="sxs-lookup"><span data-stu-id="ae3d2-128">It provides several useful facilities, including key escrow and encryption of keys at rest.</span></span> <span data-ttu-id="ae3d2-129">Klucze w tym systemie są reprezentowane jako elementy XML (w szczególności [klasy XElement](https://docs.microsoft.com/dotnet/csharp/programming-guide/concepts/linq/xelement-class-overview)).</span><span class="sxs-lookup"><span data-stu-id="ae3d2-129">Keys in this system are represented as XML elements (specifically, [XElement](https://docs.microsoft.com/dotnet/csharp/programming-guide/concepts/linq/xelement-class-overview)).</span></span>

<span data-ttu-id="ae3d2-130">`XmlKeyManager`zależy od kilku składników w trakcie wypełnienia swoich zadań:</span><span class="sxs-lookup"><span data-stu-id="ae3d2-130">`XmlKeyManager` depends on several other components in the course of fulfilling its tasks:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="ae3d2-131">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="ae3d2-131">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

* <span data-ttu-id="ae3d2-132">`AlgorithmConfiguration`, które nakazują algorytmów używanych przez nowych kluczy.</span><span class="sxs-lookup"><span data-stu-id="ae3d2-132">`AlgorithmConfiguration`, which dictates the algorithms used by new keys.</span></span>

* <span data-ttu-id="ae3d2-133">`IXmlRepository`, które kontrolki, w której klucze są utrwalane w magazynie.</span><span class="sxs-lookup"><span data-stu-id="ae3d2-133">`IXmlRepository`, which controls where keys are persisted in storage.</span></span>

* <span data-ttu-id="ae3d2-134">`IXmlEncryptor`[opcjonalnie], co pozwala na szyfrowanie kluczy w stanie spoczynku.</span><span class="sxs-lookup"><span data-stu-id="ae3d2-134">`IXmlEncryptor` [optional], which allows encrypting keys at rest.</span></span>

* <span data-ttu-id="ae3d2-135">`IKeyEscrowSink`[opcjonalnie], która udostępnia usługi kluczy depozytu.</span><span class="sxs-lookup"><span data-stu-id="ae3d2-135">`IKeyEscrowSink` [optional], which provides key escrow services.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="ae3d2-136">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="ae3d2-136">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

* <span data-ttu-id="ae3d2-137">`IXmlRepository`, które kontrolki, w której klucze są utrwalane w magazynie.</span><span class="sxs-lookup"><span data-stu-id="ae3d2-137">`IXmlRepository`, which controls where keys are persisted in storage.</span></span>

* <span data-ttu-id="ae3d2-138">`IXmlEncryptor`[opcjonalnie], co pozwala na szyfrowanie kluczy w stanie spoczynku.</span><span class="sxs-lookup"><span data-stu-id="ae3d2-138">`IXmlEncryptor` [optional], which allows encrypting keys at rest.</span></span>

* <span data-ttu-id="ae3d2-139">`IKeyEscrowSink`[opcjonalnie], która udostępnia usługi kluczy depozytu.</span><span class="sxs-lookup"><span data-stu-id="ae3d2-139">`IKeyEscrowSink` [optional], which provides key escrow services.</span></span>

---

<span data-ttu-id="ae3d2-140">Poniżej przedstawiono diagramy wysokiego poziomu, które wskazują, jak te składniki są połączone ze sobą w `XmlKeyManager`.</span><span class="sxs-lookup"><span data-stu-id="ae3d2-140">Below are high-level diagrams which indicate how these components are wired together within `XmlKeyManager`.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="ae3d2-141">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="ae3d2-141">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

   ![Tworzenie kluczy](key-management/_static/keycreation2.png)

   <span data-ttu-id="ae3d2-143">*Tworzenie klucza / CreateNewKey*</span><span class="sxs-lookup"><span data-stu-id="ae3d2-143">*Key Creation / CreateNewKey*</span></span>

<span data-ttu-id="ae3d2-144">W implementacji `CreateNewKey`, `AlgorithmConfiguration` składnik jest używany do utworzenia unikatowego `IAuthenticatedEncryptorDescriptor`, która jest następnie zserializowanym w formacie XML.</span><span class="sxs-lookup"><span data-stu-id="ae3d2-144">In the implementation of `CreateNewKey`, the `AlgorithmConfiguration` component is used to create a unique `IAuthenticatedEncryptorDescriptor`, which is then serialized as XML.</span></span> <span data-ttu-id="ae3d2-145">Jeśli występuje zbiornika kluczy depozytu raw XML (niezaszyfrowany) zapewnia sink do długoterminowego przechowywania.</span><span class="sxs-lookup"><span data-stu-id="ae3d2-145">If a key escrow sink is present, the raw (unencrypted) XML is provided to the sink for long-term storage.</span></span> <span data-ttu-id="ae3d2-146">Uruchom za pośrednictwem nieszyfrowanego XML `IXmlEncryptor` (jeśli jest to wymagane) do generowania zaszyfrowanego dokumentu XML.</span><span class="sxs-lookup"><span data-stu-id="ae3d2-146">The unencrypted XML is then run through an `IXmlEncryptor` (if required) to generate the encrypted XML document.</span></span> <span data-ttu-id="ae3d2-147">Ten dokument zaszyfrowanych jest trwały do magazynu długoterminowego za pośrednictwem `IXmlRepository`.</span><span class="sxs-lookup"><span data-stu-id="ae3d2-147">This encrypted document is persisted to long-term storage via the `IXmlRepository`.</span></span> <span data-ttu-id="ae3d2-148">(Jeśli nie `IXmlEncryptor` jest skonfigurowany, niezaszyfrowane dokumentu jest utrwalona w `IXmlRepository`.)</span><span class="sxs-lookup"><span data-stu-id="ae3d2-148">(If no `IXmlEncryptor` is configured, the unencrypted document is persisted in the `IXmlRepository`.)</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="ae3d2-149">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="ae3d2-149">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

   ![Tworzenie kluczy](key-management/_static/keycreation1.png)

   <span data-ttu-id="ae3d2-151">*Tworzenie klucza / CreateNewKey*</span><span class="sxs-lookup"><span data-stu-id="ae3d2-151">*Key Creation / CreateNewKey*</span></span>

<span data-ttu-id="ae3d2-152">W implementacji `CreateNewKey`, `IAuthenticatedEncryptorConfiguration` składnik jest używany do utworzenia unikatowego `IAuthenticatedEncryptorDescriptor`, która jest następnie zserializowanym w formacie XML.</span><span class="sxs-lookup"><span data-stu-id="ae3d2-152">In the implementation of `CreateNewKey`, the `IAuthenticatedEncryptorConfiguration` component is used to create a unique `IAuthenticatedEncryptorDescriptor`, which is then serialized as XML.</span></span> <span data-ttu-id="ae3d2-153">Jeśli występuje zbiornika kluczy depozytu raw XML (niezaszyfrowany) zapewnia sink do długoterminowego przechowywania.</span><span class="sxs-lookup"><span data-stu-id="ae3d2-153">If a key escrow sink is present, the raw (unencrypted) XML is provided to the sink for long-term storage.</span></span> <span data-ttu-id="ae3d2-154">Uruchom za pośrednictwem nieszyfrowanego XML `IXmlEncryptor` (jeśli jest to wymagane) do generowania zaszyfrowanego dokumentu XML.</span><span class="sxs-lookup"><span data-stu-id="ae3d2-154">The unencrypted XML is then run through an `IXmlEncryptor` (if required) to generate the encrypted XML document.</span></span> <span data-ttu-id="ae3d2-155">Ten dokument zaszyfrowanych jest trwały do magazynu długoterminowego za pośrednictwem `IXmlRepository`.</span><span class="sxs-lookup"><span data-stu-id="ae3d2-155">This encrypted document is persisted to long-term storage via the `IXmlRepository`.</span></span> <span data-ttu-id="ae3d2-156">(Jeśli nie `IXmlEncryptor` jest skonfigurowany, niezaszyfrowane dokumentu jest utrwalona w `IXmlRepository`.)</span><span class="sxs-lookup"><span data-stu-id="ae3d2-156">(If no `IXmlEncryptor` is configured, the unencrypted document is persisted in the `IXmlRepository`.)</span></span>

---

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="ae3d2-157">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="ae3d2-157">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

   ![Pobieranie klucza](key-management/_static/keyretrieval2.png)
   
# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="ae3d2-159">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="ae3d2-159">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

   ![Pobieranie klucza](key-management/_static/keyretrieval1.png)

---

   <span data-ttu-id="ae3d2-161">*Klucz pobierania / GetAllKeys*</span><span class="sxs-lookup"><span data-stu-id="ae3d2-161">*Key Retrieval / GetAllKeys*</span></span>

<span data-ttu-id="ae3d2-162">W implementacji `GetAllKeys`, reprezentująca klucze dokumentów XML i odwołań są odczytywane z podstawową `IXmlRepository`.</span><span class="sxs-lookup"><span data-stu-id="ae3d2-162">In the implementation of `GetAllKeys`, the XML documents representing keys and revocations are read from the underlying `IXmlRepository`.</span></span> <span data-ttu-id="ae3d2-163">Jeśli te dokumenty są szyfrowane, system automatycznie odszyfrować je.</span><span class="sxs-lookup"><span data-stu-id="ae3d2-163">If these documents are encrypted, the system will automatically decrypt them.</span></span> <span data-ttu-id="ae3d2-164">`XmlKeyManager`tworzy odpowiednie `IAuthenticatedEncryptorDescriptorDeserializer` wystąpień do deserializacji dokumenty z powrotem do `IAuthenticatedEncryptorDescriptor` wystąpienia, które następnie są ujęte w poszczególnych `IKey` wystąpień.</span><span class="sxs-lookup"><span data-stu-id="ae3d2-164">`XmlKeyManager` creates the appropriate `IAuthenticatedEncryptorDescriptorDeserializer` instances to deserialize the documents back into `IAuthenticatedEncryptorDescriptor` instances, which are then wrapped in individual `IKey` instances.</span></span> <span data-ttu-id="ae3d2-165">Ta kolekcja `IKey` wystąpień jest zwracany do obiektu wywołującego.</span><span class="sxs-lookup"><span data-stu-id="ae3d2-165">This collection of `IKey` instances is returned to the caller.</span></span>

<span data-ttu-id="ae3d2-166">Więcej informacji na temat konkretnego elementów XML znajduje się w [dokumentu w formacie magazynu kluczy](../implementation/key-storage-format.md#data-protection-implementation-key-storage-format).</span><span class="sxs-lookup"><span data-stu-id="ae3d2-166">Further information on the particular XML elements can be found in the [key storage format document](../implementation/key-storage-format.md#data-protection-implementation-key-storage-format).</span></span>

## <a name="ixmlrepository"></a><span data-ttu-id="ae3d2-167">IXmlRepository</span><span class="sxs-lookup"><span data-stu-id="ae3d2-167">IXmlRepository</span></span>

<span data-ttu-id="ae3d2-168">`IXmlRepository` Interfejsu reprezentuje typ, który można utrwalić XML i pobrać XML z magazynu zapasowego.</span><span class="sxs-lookup"><span data-stu-id="ae3d2-168">The `IXmlRepository` interface represents a type that can persist XML to and retrieve XML from a backing store.</span></span> <span data-ttu-id="ae3d2-169">Udostępnia ona dwóch interfejsów API:</span><span class="sxs-lookup"><span data-stu-id="ae3d2-169">It exposes two APIs:</span></span>

* <span data-ttu-id="ae3d2-170">GetAllElements(): IReadOnlyCollection<XElement></span><span class="sxs-lookup"><span data-stu-id="ae3d2-170">GetAllElements() : IReadOnlyCollection<XElement></span></span>

* <span data-ttu-id="ae3d2-171">StoreElement (element klasy XElement, friendlyName ciąg)</span><span class="sxs-lookup"><span data-stu-id="ae3d2-171">StoreElement(XElement element, string friendlyName)</span></span>

<span data-ttu-id="ae3d2-172">Implementacje `IXmlRepository` nie ma potrzeby analizy kodu XML przechodzącej przez nich.</span><span class="sxs-lookup"><span data-stu-id="ae3d2-172">Implementations of `IXmlRepository` don't need to parse the XML passing through them.</span></span> <span data-ttu-id="ae3d2-173">Powinny traktować jako nieprzezroczyste dokumentów XML i umożliwić wyższych warstw martwić generowania i analizowania dokumentów.</span><span class="sxs-lookup"><span data-stu-id="ae3d2-173">They should treat the XML documents as opaque and let higher layers worry about generating and parsing the documents.</span></span>

<span data-ttu-id="ae3d2-174">Istnieją dwa typy konkretnych wbudowanych implementujące `IXmlRepository`: `FileSystemXmlRepository` i `RegistryXmlRepository`.</span><span class="sxs-lookup"><span data-stu-id="ae3d2-174">There are two built-in concrete types which implement `IXmlRepository`: `FileSystemXmlRepository` and `RegistryXmlRepository`.</span></span> <span data-ttu-id="ae3d2-175">Zobacz [dokumentu dostawców magazynu kluczy](../implementation/key-storage-providers.md#data-protection-implementation-key-storage-providers) Aby uzyskać więcej informacji.</span><span class="sxs-lookup"><span data-stu-id="ae3d2-175">See the [key storage providers document](../implementation/key-storage-providers.md#data-protection-implementation-key-storage-providers) for more information.</span></span> <span data-ttu-id="ae3d2-176">Rejestrowanie niestandardowe `IXmlRepository` będzie odpowiedni sposób używania magazynu zapasowego różnych, np. magazyn obiektów Blob Azure.</span><span class="sxs-lookup"><span data-stu-id="ae3d2-176">Registering a custom `IXmlRepository` would be the appropriate manner to use a different backing store, e.g., Azure Blob Storage.</span></span>

<span data-ttu-id="ae3d2-177">Aby zmienić domyślne repozytorium całej aplikacji, rejestrowania niestandardowego `IXmlRepository` wystąpienie:</span><span class="sxs-lookup"><span data-stu-id="ae3d2-177">To change the default repository application-wide, register a custom `IXmlRepository` instance:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="ae3d2-178">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="ae3d2-178">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

   ```csharp
   services.Configure<KeyManagementOptions>(options => options.XmlRepository = new MyCustomXmlRepository());
   ```
   
# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="ae3d2-179">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="ae3d2-179">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

   ```csharp
   services.AddSingleton<IXmlRepository>(new MyCustomXmlRepository());
   ```

---

<a name="data-protection-extensibility-key-management-ixmlencryptor"></a>

## <a name="ixmlencryptor"></a><span data-ttu-id="ae3d2-180">IXmlEncryptor</span><span class="sxs-lookup"><span data-stu-id="ae3d2-180">IXmlEncryptor</span></span>

<span data-ttu-id="ae3d2-181">`IXmlEncryptor` Interfejsu reprezentuje typ, który można zaszyfrować element XML w postaci zwykłego tekstu.</span><span class="sxs-lookup"><span data-stu-id="ae3d2-181">The `IXmlEncryptor` interface represents a type that can encrypt a plaintext XML element.</span></span> <span data-ttu-id="ae3d2-182">Udostępnia ona jednego interfejsu API:</span><span class="sxs-lookup"><span data-stu-id="ae3d2-182">It exposes a single API:</span></span>

* <span data-ttu-id="ae3d2-183">Szyfrowanie (plaintextElement klasy XElement): EncryptedXmlInfo</span><span class="sxs-lookup"><span data-stu-id="ae3d2-183">Encrypt(XElement plaintextElement) : EncryptedXmlInfo</span></span>

<span data-ttu-id="ae3d2-184">W przypadku serializacji `IAuthenticatedEncryptorDescriptor` zawiera wszystkie elementy oznaczone jako "wymaga szyfrowania", następnie `XmlKeyManager` uruchomi tych elementów przy użyciu skonfigurowanego `IXmlEncryptor`w `Encrypt` — metoda która zostanie utrzymana enciphered elementu zamiast element w postaci zwykłego tekstu do `IXmlRepository`.</span><span class="sxs-lookup"><span data-stu-id="ae3d2-184">If a serialized `IAuthenticatedEncryptorDescriptor` contains any elements marked as "requires encryption", then `XmlKeyManager` will run those elements through the configured `IXmlEncryptor`'s `Encrypt` method, and it will persist the enciphered element rather than the plaintext element to the `IXmlRepository`.</span></span> <span data-ttu-id="ae3d2-185">Dane wyjściowe `Encrypt` jest metoda `EncryptedXmlInfo` obiektu.</span><span class="sxs-lookup"><span data-stu-id="ae3d2-185">The output of the `Encrypt` method is an `EncryptedXmlInfo` object.</span></span> <span data-ttu-id="ae3d2-186">Ten obiekt jest otoki, który zawiera wynikowe enciphered `XElement` i typ, który reprezentuje `IXmlDecryptor` które mogą służyć do odszyfrowania odpowiadającego mu elementu.</span><span class="sxs-lookup"><span data-stu-id="ae3d2-186">This object is a wrapper which contains both the resultant enciphered `XElement` and the Type which represents an `IXmlDecryptor` which can be used to decipher the corresponding element.</span></span>

<span data-ttu-id="ae3d2-187">Istnieją cztery wbudowanych typów konkretnych implementujące `IXmlEncryptor`:</span><span class="sxs-lookup"><span data-stu-id="ae3d2-187">There are four built-in concrete types which implement `IXmlEncryptor`:</span></span>
* `CertificateXmlEncryptor`
* `DpapiNGXmlEncryptor`
* `DpapiXmlEncryptor`
* `NullXmlEncryptor`

<span data-ttu-id="ae3d2-188">Zobacz [klucza szyfrowania w dokumencie rest](../implementation/key-encryption-at-rest.md#data-protection-implementation-key-encryption-at-rest) Aby uzyskać więcej informacji.</span><span class="sxs-lookup"><span data-stu-id="ae3d2-188">See the [key encryption at rest document](../implementation/key-encryption-at-rest.md#data-protection-implementation-key-encryption-at-rest) for more information.</span></span>

<span data-ttu-id="ae3d2-189">Aby zmienić domyślny mechanizm klucza szyfrowania w rest całej aplikacji, rejestrowania niestandardowego `IXmlEncryptor` wystąpienie:</span><span class="sxs-lookup"><span data-stu-id="ae3d2-189">To change the default key-encryption-at-rest mechanism application-wide, register a custom `IXmlEncryptor` instance:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="ae3d2-190">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="ae3d2-190">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

   ```csharp
   services.Configure<KeyManagementOptions>(options => options.XmlEncryptor = new MyCustomXmlEncryptor());
   ```
   
# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="ae3d2-191">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="ae3d2-191">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

   ```csharp
   services.AddSingleton<IXmlEncryptor>(new MyCustomXmlEncryptor());
   ```

---

## <a name="ixmldecryptor"></a><span data-ttu-id="ae3d2-192">IXmlDecryptor</span><span class="sxs-lookup"><span data-stu-id="ae3d2-192">IXmlDecryptor</span></span>

<span data-ttu-id="ae3d2-193">`IXmlDecryptor` Interfejsu reprezentuje typ, który potrafi do odszyfrowania `XElement` który został enciphered za pośrednictwem `IXmlEncryptor`.</span><span class="sxs-lookup"><span data-stu-id="ae3d2-193">The `IXmlDecryptor` interface represents a type that knows how to decrypt an `XElement` that was enciphered via an `IXmlEncryptor`.</span></span> <span data-ttu-id="ae3d2-194">Udostępnia ona jednego interfejsu API:</span><span class="sxs-lookup"><span data-stu-id="ae3d2-194">It exposes a single API:</span></span>

* <span data-ttu-id="ae3d2-195">Odszyfrowywanie (encryptedElement klasy XElement): klasy XElement</span><span class="sxs-lookup"><span data-stu-id="ae3d2-195">Decrypt(XElement encryptedElement) : XElement</span></span>

<span data-ttu-id="ae3d2-196">`Decrypt` Metody cofa szyfrowania wykonywane przez `IXmlEncryptor.Encrypt`.</span><span class="sxs-lookup"><span data-stu-id="ae3d2-196">The `Decrypt` method undoes the encryption performed by `IXmlEncryptor.Encrypt`.</span></span> <span data-ttu-id="ae3d2-197">Ogólnie rzecz biorąc, każdy konkretnych `IXmlEncryptor` implementacji będą miały odpowiednich konkretnych `IXmlDecryptor` implementacji.</span><span class="sxs-lookup"><span data-stu-id="ae3d2-197">Generally, each concrete `IXmlEncryptor` implementation will have a corresponding concrete `IXmlDecryptor` implementation.</span></span>

<span data-ttu-id="ae3d2-198">Typów implementujących `IXmlDecryptor` powinny mieć jedno z następujących dwóch konstruktorów publicznych:</span><span class="sxs-lookup"><span data-stu-id="ae3d2-198">Types which implement `IXmlDecryptor` should have one of the following two public constructors:</span></span>

* <span data-ttu-id="ae3d2-199">.ctor(IServiceProvider)</span><span class="sxs-lookup"><span data-stu-id="ae3d2-199">.ctor(IServiceProvider)</span></span>
* <span data-ttu-id="ae3d2-200">.ctor()</span><span class="sxs-lookup"><span data-stu-id="ae3d2-200">.ctor()</span></span>

> [!NOTE]
> <span data-ttu-id="ae3d2-201">`IServiceProvider` Przekazany do konstruktora może mieć wartości null.</span><span class="sxs-lookup"><span data-stu-id="ae3d2-201">The `IServiceProvider` passed to the constructor may be null.</span></span>

## <a name="ikeyescrowsink"></a><span data-ttu-id="ae3d2-202">IKeyEscrowSink</span><span class="sxs-lookup"><span data-stu-id="ae3d2-202">IKeyEscrowSink</span></span>

<span data-ttu-id="ae3d2-203">`IKeyEscrowSink` Interfejsu reprezentuje typ, który można wykonać depozytu poufne informacje.</span><span class="sxs-lookup"><span data-stu-id="ae3d2-203">The `IKeyEscrowSink` interface represents a type that can perform escrow of sensitive information.</span></span> <span data-ttu-id="ae3d2-204">Odwołaj serializacji deskryptory mogą zawierać poufne informacje (na przykład materiały kryptograficznych), czy jest to, co spowodowało wprowadzenie [IXmlEncryptor](xref:security/data-protection/extensibility/key-management#data-protection-extensibility-key-management-ixmlencryptor) wpisz w pierwszej kolejności.</span><span class="sxs-lookup"><span data-stu-id="ae3d2-204">Recall that serialized descriptors might contain sensitive information (such as cryptographic material), and this is what led to the introduction of the [IXmlEncryptor](xref:security/data-protection/extensibility/key-management#data-protection-extensibility-key-management-ixmlencryptor) type in the first place.</span></span> <span data-ttu-id="ae3d2-205">Jednak awarii, a klucz sygnałów mogą zostać usunięte lub uszkodzony.</span><span class="sxs-lookup"><span data-stu-id="ae3d2-205">However, accidents happen, and key rings can be deleted or become corrupted.</span></span>

<span data-ttu-id="ae3d2-206">Interfejs depozytu zapewnia kreskowania ewakuacji, umożliwiający dostęp do nieprzetworzonej serializacji XML przed jest przekształcana przez dowolne skonfigurowane [IXmlEncryptor](xref:security/data-protection/extensibility/key-management#data-protection-extensibility-key-management-ixmlencryptor).</span><span class="sxs-lookup"><span data-stu-id="ae3d2-206">The escrow interface provides an emergency escape hatch, allowing access to the raw serialized XML before it's transformed by any configured [IXmlEncryptor](xref:security/data-protection/extensibility/key-management#data-protection-extensibility-key-management-ixmlencryptor).</span></span> <span data-ttu-id="ae3d2-207">Interfejs udostępnia jeden interfejs API:</span><span class="sxs-lookup"><span data-stu-id="ae3d2-207">The interface exposes a single API:</span></span>

* <span data-ttu-id="ae3d2-208">Magazyn (identyfikator Guid klucza, element klasy XElement)</span><span class="sxs-lookup"><span data-stu-id="ae3d2-208">Store(Guid keyId, XElement element)</span></span>

<span data-ttu-id="ae3d2-209">Do `IKeyEscrowSink` implementacji do obsługi podany element w bezpieczny sposób zgodne z zasadami firmowymi.</span><span class="sxs-lookup"><span data-stu-id="ae3d2-209">It's up to the `IKeyEscrowSink` implementation to handle the provided element in a secure manner consistent with business policy.</span></span> <span data-ttu-id="ae3d2-210">Można jedna implementacja możliwe dla odbiorcy depozytu do szyfrowania elementu XML przy użyciu znanych firmowej certyfikatu X.509, gdzie klucz prywatny certyfikatu ma zdeponowane; `CertificateXmlEncryptor` typu może to pomóc.</span><span class="sxs-lookup"><span data-stu-id="ae3d2-210">One possible implementation could be for the escrow sink to encrypt the XML element using a known corporate X.509 certificate where the certificate's private key has been escrowed; the `CertificateXmlEncryptor` type can assist with this.</span></span> <span data-ttu-id="ae3d2-211">`IKeyEscrowSink` Implementacji również jest odpowiedzialny za odpowiednio utrwalanie podany element.</span><span class="sxs-lookup"><span data-stu-id="ae3d2-211">The `IKeyEscrowSink` implementation is also responsible for persisting the provided element appropriately.</span></span>

<span data-ttu-id="ae3d2-212">Mechanizm depozytu jest domyślnie włączona, ale administratorzy serwera mogą [to skonfigurować globalnie](xref:security/data-protection/configuration/machine-wide-policy).</span><span class="sxs-lookup"><span data-stu-id="ae3d2-212">By default no escrow mechanism is enabled, though server administrators can [configure this globally](xref:security/data-protection/configuration/machine-wide-policy).</span></span> <span data-ttu-id="ae3d2-213">Może również być skonfigurowana programowo za pośrednictwem `IDataProtectionBuilder.AddKeyEscrowSink` metody, jak pokazano w poniższym przykładzie.</span><span class="sxs-lookup"><span data-stu-id="ae3d2-213">It can also be configured programmatically via the `IDataProtectionBuilder.AddKeyEscrowSink` method as shown in the sample below.</span></span> <span data-ttu-id="ae3d2-214">`AddKeyEscrowSink` Dublowany przeciążenia metody `IServiceCollection.AddSingleton` i `IServiceCollection.AddInstance` przeciążenia, jako `IKeyEscrowSink` powinny być pojedynczych wystąpień.</span><span class="sxs-lookup"><span data-stu-id="ae3d2-214">The `AddKeyEscrowSink` method overloads mirror the `IServiceCollection.AddSingleton` and `IServiceCollection.AddInstance` overloads, as `IKeyEscrowSink` instances are intended to be singletons.</span></span> <span data-ttu-id="ae3d2-215">Jeśli wiele `IKeyEscrowSink` zarejestrowanych wystąpień, każdy z nich zostanie wywołana podczas generowania kluczy, więc kluczy można zdeponowane w wielu mechanizmów jednocześnie.</span><span class="sxs-lookup"><span data-stu-id="ae3d2-215">If multiple `IKeyEscrowSink` instances are registered, each one will be called during key generation, so keys can be escrowed to multiple mechanisms simultaneously.</span></span>

<span data-ttu-id="ae3d2-216">Brak żadnego interfejsu API można odczytać materiału z `IKeyEscrowSink` wystąpienia.</span><span class="sxs-lookup"><span data-stu-id="ae3d2-216">There's no API to read material from an `IKeyEscrowSink` instance.</span></span> <span data-ttu-id="ae3d2-217">Jest to zgodne z teorii projektu mechanizmu depozytu: ma zamierza udostępnić materiału klucza do zaufanego urzędu certyfikacji, a ponieważ aplikacja się nie jest zaufany urząd, nie powinien on mieć dostęp do własnej escrowed materiału.</span><span class="sxs-lookup"><span data-stu-id="ae3d2-217">This is consistent with the design theory of the escrow mechanism: it's intended to make the key material accessible to a trusted authority, and since the application is itself not a trusted authority, it shouldn't have access to its own escrowed material.</span></span>

<span data-ttu-id="ae3d2-218">Następujący przykładowy kod przedstawia tworzenie i rejestrowanie `IKeyEscrowSink` gdzie klucze są zdeponowane w taki sposób, aby je odzyskać tylko elementy członkowskie "CONTOSODomain Administratorzy".</span><span class="sxs-lookup"><span data-stu-id="ae3d2-218">The following sample code demonstrates creating and registering an `IKeyEscrowSink` where keys are escrowed such that only members of "CONTOSODomain Admins" can recover them.</span></span>

> [!NOTE]
> <span data-ttu-id="ae3d2-219">Aby uruchomić ten przykład, użytkownik musi być na przyłączonych do domeny systemu Windows 8 / maszyny z systemem Windows Server 2012, a kontroler domeny musi być systemu Windows Server 2012 lub nowszym.</span><span class="sxs-lookup"><span data-stu-id="ae3d2-219">To run this sample, you must be on a domain-joined Windows 8 / Windows Server 2012 machine, and the domain controller must be Windows Server 2012 or later.</span></span>

[!code-csharp[Main](key-management/samples/key-management-extensibility.cs)]
