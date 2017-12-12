---
title: "Rozszerzalność zarządzania kluczami"
author: rick-anderson
description: "W tym dokumencie przedstawiono rozszerzalności zarządzania kluczami ochrony danych platformy ASP.NET Core."
keywords: "Zarządzanie kluczami platformy ASP.NET Core, ochrony danych"
ms.author: riande
manager: wpickett
ms.date: 11/22/2017
ms.topic: article
ms.assetid: 3606b251-8324-4485-8d52-582a2cd5cffb
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/extensibility/key-management
ms.openlocfilehash: 0702e13163c0208e9d2863e711b02ffb257f6260
ms.sourcegitcommit: e641c5794525f983485621860926d8ab4e7360c8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/23/2017
---
# <a name="key-management-extensibility"></a><span data-ttu-id="fa4be-104">Rozszerzalność zarządzania kluczami</span><span class="sxs-lookup"><span data-stu-id="fa4be-104">Key management extensibility</span></span>

<a name="data-protection-extensibility-key-management"></a>

>[!TIP]
> <span data-ttu-id="fa4be-105">Odczyt [zarządzanie kluczami](../implementation/key-management.md#data-protection-implementation-key-management) sekcji przed przeczytaniem tej części, jak wyjaśniono niektóre podstawowe pojęcia dotyczące tych interfejsów API.</span><span class="sxs-lookup"><span data-stu-id="fa4be-105">Read the [key management](../implementation/key-management.md#data-protection-implementation-key-management) section before reading this section, as it explains some of the fundamental concepts behind these APIs.</span></span>

>[!WARNING]
> <span data-ttu-id="fa4be-106">Typy implementujące żadnego z następujących interfejsów powinny być bezpieczne wątkowo dla wielu wywołań.</span><span class="sxs-lookup"><span data-stu-id="fa4be-106">Types that implement any of the following interfaces should be thread-safe for multiple callers.</span></span>

## <a name="key"></a><span data-ttu-id="fa4be-107">Key</span><span class="sxs-lookup"><span data-stu-id="fa4be-107">Key</span></span>

<span data-ttu-id="fa4be-108">`IKey` Interfejs jest podstawowe reprezentacja klucza w cryptosystem.</span><span class="sxs-lookup"><span data-stu-id="fa4be-108">The `IKey` interface is the basic representation of a key in cryptosystem.</span></span> <span data-ttu-id="fa4be-109">Klucz termin jest używany tutaj w tym sensie abstrakcyjne nie znajduje się w rozumieniu literału "materiału klucza kryptograficznego".</span><span class="sxs-lookup"><span data-stu-id="fa4be-109">The term key is used here in the abstract sense, not in the literal sense of "cryptographic key material".</span></span> <span data-ttu-id="fa4be-110">Klucz ma następujące właściwości:</span><span class="sxs-lookup"><span data-stu-id="fa4be-110">A key has the following properties:</span></span>

* <span data-ttu-id="fa4be-111">Daty aktywacji, tworzenie i wygaśnięcia</span><span class="sxs-lookup"><span data-stu-id="fa4be-111">Activation, creation, and expiration dates</span></span>

* <span data-ttu-id="fa4be-112">Stan odwołania</span><span class="sxs-lookup"><span data-stu-id="fa4be-112">Revocation status</span></span>

* <span data-ttu-id="fa4be-113">Identyfikator klucza (GUID)</span><span class="sxs-lookup"><span data-stu-id="fa4be-113">Key identifier (a GUID)</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="fa4be-114">Program ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="fa4be-114">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="fa4be-115">Ponadto `IKey` przedstawia `CreateEncryptor` metodę, która może służyć do tworzenia [IAuthenticatedEncryptor](core-crypto.md#data-protection-extensibility-core-crypto-iauthenticatedencryptor) wystąpienia powiązane z danym kluczem.</span><span class="sxs-lookup"><span data-stu-id="fa4be-115">Additionally, `IKey` exposes a `CreateEncryptor` method which can be used to create an [IAuthenticatedEncryptor](core-crypto.md#data-protection-extensibility-core-crypto-iauthenticatedencryptor) instance tied to this key.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="fa4be-116">Program ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="fa4be-116">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="fa4be-117">Ponadto `IKey` przedstawia `CreateEncryptorInstance` metodę, która może służyć do tworzenia [IAuthenticatedEncryptor](core-crypto.md#data-protection-extensibility-core-crypto-iauthenticatedencryptor) wystąpienia powiązane z danym kluczem.</span><span class="sxs-lookup"><span data-stu-id="fa4be-117">Additionally, `IKey` exposes a `CreateEncryptorInstance` method which can be used to create an [IAuthenticatedEncryptor](core-crypto.md#data-protection-extensibility-core-crypto-iauthenticatedencryptor) instance tied to this key.</span></span>

---

> [!NOTE]
> <span data-ttu-id="fa4be-118">Brak żadnego interfejsu API można pobrać raw materiał kryptograficznych z `IKey` wystąpienia.</span><span class="sxs-lookup"><span data-stu-id="fa4be-118">There is no API to retrieve the raw cryptographic material from an `IKey` instance.</span></span>

## <a name="ikeymanager"></a><span data-ttu-id="fa4be-119">IKeyManager</span><span class="sxs-lookup"><span data-stu-id="fa4be-119">IKeyManager</span></span>

<span data-ttu-id="fa4be-120">`IKeyManager` Interfejsu reprezentuje obiekt odpowiedzialny za ogólne magazynu kluczy, pobieranie i manipulowania nimi.</span><span class="sxs-lookup"><span data-stu-id="fa4be-120">The `IKeyManager` interface represents an object responsible for general key storage, retrieval, and manipulation.</span></span> <span data-ttu-id="fa4be-121">Udostępnia on trzy operacje wysokiego poziomu:</span><span class="sxs-lookup"><span data-stu-id="fa4be-121">It exposes three high-level operations:</span></span>

* <span data-ttu-id="fa4be-122">Utwórz nowy klucz i zachować go do magazynu.</span><span class="sxs-lookup"><span data-stu-id="fa4be-122">Create a new key and persist it to storage.</span></span>

* <span data-ttu-id="fa4be-123">Pobierz wszystkie klucze z magazynu.</span><span class="sxs-lookup"><span data-stu-id="fa4be-123">Get all keys from storage.</span></span>

* <span data-ttu-id="fa4be-124">Odwołaj klucze co najmniej jednego i zachować informacje o odwołaniu do magazynu.</span><span class="sxs-lookup"><span data-stu-id="fa4be-124">Revoke one or more keys and persist the revocation information to storage.</span></span>

>[!WARNING]
> <span data-ttu-id="fa4be-125">Zapisywanie `IKeyManager` jest bardzo zaawansowane zadania, a większość deweloperów nie powinny podejmować.</span><span class="sxs-lookup"><span data-stu-id="fa4be-125">Writing an `IKeyManager` is a very advanced task, and the majority of developers should not attempt it.</span></span> <span data-ttu-id="fa4be-126">Zamiast tego większość deweloperów należy wykorzystać możliwości oferowane przez [XmlKeyManager](xref:security/data-protection/extensibility/key-management#data-protection-extensibility-key-management-xmlkeymanager) klasy.</span><span class="sxs-lookup"><span data-stu-id="fa4be-126">Instead, most developers should take advantage of the facilities offered by the [XmlKeyManager](xref:security/data-protection/extensibility/key-management#data-protection-extensibility-key-management-xmlkeymanager) class.</span></span>

<a name="data-protection-extensibility-key-management-xmlkeymanager"></a>

## <a name="xmlkeymanager"></a><span data-ttu-id="fa4be-127">XmlKeyManager</span><span class="sxs-lookup"><span data-stu-id="fa4be-127">XmlKeyManager</span></span>

<span data-ttu-id="fa4be-128">`XmlKeyManager` w polu wykonanie jest typu `IKeyManager`.</span><span class="sxs-lookup"><span data-stu-id="fa4be-128">The `XmlKeyManager` type is the in-box concrete implementation of `IKeyManager`.</span></span> <span data-ttu-id="fa4be-129">Udostępnia kilka przydatne urządzeń, w tym depozytu kluczy i szyfrowania kluczy w stanie spoczynku.</span><span class="sxs-lookup"><span data-stu-id="fa4be-129">It provides several useful facilities, including key escrow and encryption of keys at rest.</span></span> <span data-ttu-id="fa4be-130">Klucze w tym systemie są reprezentowane jako elementy XML (w szczególności [klasy XElement](https://docs.microsoft.com/dotnet/csharp/programming-guide/concepts/linq/xelement-class-overview)).</span><span class="sxs-lookup"><span data-stu-id="fa4be-130">Keys in this system are represented as XML elements (specifically, [XElement](https://docs.microsoft.com/dotnet/csharp/programming-guide/concepts/linq/xelement-class-overview)).</span></span>

<span data-ttu-id="fa4be-131">`XmlKeyManager`zależy od kilku składników w trakcie wypełnienia swoich zadań:</span><span class="sxs-lookup"><span data-stu-id="fa4be-131">`XmlKeyManager` depends on several other components in the course of fulfilling its tasks:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="fa4be-132">Program ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="fa4be-132">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

* <span data-ttu-id="fa4be-133">`AlgorithmConfiguration`, które nakazują algorytmów używanych przez nowych kluczy.</span><span class="sxs-lookup"><span data-stu-id="fa4be-133">`AlgorithmConfiguration`, which dictates the algorithms used by new keys.</span></span>

* <span data-ttu-id="fa4be-134">`IXmlRepository`, które kontrolki, w której klucze są utrwalane w magazynie.</span><span class="sxs-lookup"><span data-stu-id="fa4be-134">`IXmlRepository`, which controls where keys are persisted in storage.</span></span>

* <span data-ttu-id="fa4be-135">`IXmlEncryptor`[opcjonalnie], co pozwala na szyfrowanie kluczy w stanie spoczynku.</span><span class="sxs-lookup"><span data-stu-id="fa4be-135">`IXmlEncryptor` [optional], which allows encrypting keys at rest.</span></span>

* <span data-ttu-id="fa4be-136">`IKeyEscrowSink`[opcjonalnie], która udostępnia usługi kluczy depozytu.</span><span class="sxs-lookup"><span data-stu-id="fa4be-136">`IKeyEscrowSink` [optional], which provides key escrow services.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="fa4be-137">Program ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="fa4be-137">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

* <span data-ttu-id="fa4be-138">`IXmlRepository`, które kontrolki, w której klucze są utrwalane w magazynie.</span><span class="sxs-lookup"><span data-stu-id="fa4be-138">`IXmlRepository`, which controls where keys are persisted in storage.</span></span>

* <span data-ttu-id="fa4be-139">`IXmlEncryptor`[opcjonalnie], co pozwala na szyfrowanie kluczy w stanie spoczynku.</span><span class="sxs-lookup"><span data-stu-id="fa4be-139">`IXmlEncryptor` [optional], which allows encrypting keys at rest.</span></span>

* <span data-ttu-id="fa4be-140">`IKeyEscrowSink`[opcjonalnie], która udostępnia usługi kluczy depozytu.</span><span class="sxs-lookup"><span data-stu-id="fa4be-140">`IKeyEscrowSink` [optional], which provides key escrow services.</span></span>

---

<span data-ttu-id="fa4be-141">Poniżej przedstawiono diagramy wysokiego poziomu, które wskazują, jak te składniki są połączone ze sobą w `XmlKeyManager`.</span><span class="sxs-lookup"><span data-stu-id="fa4be-141">Below are high-level diagrams which indicate how these components are wired together within `XmlKeyManager`.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="fa4be-142">Program ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="fa4be-142">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

   ![Tworzenie kluczy](key-management/_static/keycreation2.png)

   <span data-ttu-id="fa4be-144">*Tworzenie klucza / CreateNewKey*</span><span class="sxs-lookup"><span data-stu-id="fa4be-144">*Key Creation / CreateNewKey*</span></span>

<span data-ttu-id="fa4be-145">W implementacji `CreateNewKey`, `AlgorithmConfiguration` składnik jest używany do utworzenia unikatowego `IAuthenticatedEncryptorDescriptor`, która jest następnie zserializowanym w formacie XML.</span><span class="sxs-lookup"><span data-stu-id="fa4be-145">In the implementation of `CreateNewKey`, the `AlgorithmConfiguration` component is used to create a unique `IAuthenticatedEncryptorDescriptor`, which is then serialized as XML.</span></span> <span data-ttu-id="fa4be-146">Jeśli występuje zbiornika kluczy depozytu raw XML (niezaszyfrowany) zapewnia sink do długoterminowego przechowywania.</span><span class="sxs-lookup"><span data-stu-id="fa4be-146">If a key escrow sink is present, the raw (unencrypted) XML is provided to the sink for long-term storage.</span></span> <span data-ttu-id="fa4be-147">Uruchom za pośrednictwem nieszyfrowanego XML `IXmlEncryptor` (jeśli jest to wymagane) do generowania zaszyfrowanego dokumentu XML.</span><span class="sxs-lookup"><span data-stu-id="fa4be-147">The unencrypted XML is then run through an `IXmlEncryptor` (if required) to generate the encrypted XML document.</span></span> <span data-ttu-id="fa4be-148">Ten dokument zaszyfrowanych jest trwały do magazynu długoterminowego za pośrednictwem `IXmlRepository`.</span><span class="sxs-lookup"><span data-stu-id="fa4be-148">This encrypted document is persisted to long-term storage via the `IXmlRepository`.</span></span> <span data-ttu-id="fa4be-149">(Jeśli nie `IXmlEncryptor` jest skonfigurowany, niezaszyfrowane dokumentu jest utrwalona w `IXmlRepository`.)</span><span class="sxs-lookup"><span data-stu-id="fa4be-149">(If no `IXmlEncryptor` is configured, the unencrypted document is persisted in the `IXmlRepository`.)</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="fa4be-150">Program ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="fa4be-150">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

   ![Tworzenie kluczy](key-management/_static/keycreation1.png)

   <span data-ttu-id="fa4be-152">*Tworzenie klucza / CreateNewKey*</span><span class="sxs-lookup"><span data-stu-id="fa4be-152">*Key Creation / CreateNewKey*</span></span>

<span data-ttu-id="fa4be-153">W implementacji `CreateNewKey`, `IAuthenticatedEncryptorConfiguration` składnik jest używany do utworzenia unikatowego `IAuthenticatedEncryptorDescriptor`, która jest następnie zserializowanym w formacie XML.</span><span class="sxs-lookup"><span data-stu-id="fa4be-153">In the implementation of `CreateNewKey`, the `IAuthenticatedEncryptorConfiguration` component is used to create a unique `IAuthenticatedEncryptorDescriptor`, which is then serialized as XML.</span></span> <span data-ttu-id="fa4be-154">Jeśli występuje zbiornika kluczy depozytu raw XML (niezaszyfrowany) zapewnia sink do długoterminowego przechowywania.</span><span class="sxs-lookup"><span data-stu-id="fa4be-154">If a key escrow sink is present, the raw (unencrypted) XML is provided to the sink for long-term storage.</span></span> <span data-ttu-id="fa4be-155">Uruchom za pośrednictwem nieszyfrowanego XML `IXmlEncryptor` (jeśli jest to wymagane) do generowania zaszyfrowanego dokumentu XML.</span><span class="sxs-lookup"><span data-stu-id="fa4be-155">The unencrypted XML is then run through an `IXmlEncryptor` (if required) to generate the encrypted XML document.</span></span> <span data-ttu-id="fa4be-156">Ten dokument zaszyfrowanych jest trwały do magazynu długoterminowego za pośrednictwem `IXmlRepository`.</span><span class="sxs-lookup"><span data-stu-id="fa4be-156">This encrypted document is persisted to long-term storage via the `IXmlRepository`.</span></span> <span data-ttu-id="fa4be-157">(Jeśli nie `IXmlEncryptor` jest skonfigurowany, niezaszyfrowane dokumentu jest utrwalona w `IXmlRepository`.)</span><span class="sxs-lookup"><span data-stu-id="fa4be-157">(If no `IXmlEncryptor` is configured, the unencrypted document is persisted in the `IXmlRepository`.)</span></span>

---

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="fa4be-158">Program ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="fa4be-158">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

   ![Pobieranie klucza](key-management/_static/keyretrieval2.png)
   
# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="fa4be-160">Program ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="fa4be-160">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

   ![Pobieranie klucza](key-management/_static/keyretrieval1.png)

---

   <span data-ttu-id="fa4be-162">*Klucz pobierania / GetAllKeys*</span><span class="sxs-lookup"><span data-stu-id="fa4be-162">*Key Retrieval / GetAllKeys*</span></span>

<span data-ttu-id="fa4be-163">W implementacji `GetAllKeys`, reprezentująca klucze dokumentów XML i odwołań są odczytywane z podstawową `IXmlRepository`.</span><span class="sxs-lookup"><span data-stu-id="fa4be-163">In the implementation of `GetAllKeys`, the XML documents representing keys and revocations are read from the underlying `IXmlRepository`.</span></span> <span data-ttu-id="fa4be-164">Jeśli te dokumenty są szyfrowane, system automatycznie odszyfrować je.</span><span class="sxs-lookup"><span data-stu-id="fa4be-164">If these documents are encrypted, the system will automatically decrypt them.</span></span> <span data-ttu-id="fa4be-165">`XmlKeyManager`tworzy odpowiednie `IAuthenticatedEncryptorDescriptorDeserializer` wystąpień do deserializacji dokumenty z powrotem do `IAuthenticatedEncryptorDescriptor` wystąpienia, które następnie są ujęte w poszczególnych `IKey` wystąpień.</span><span class="sxs-lookup"><span data-stu-id="fa4be-165">`XmlKeyManager` creates the appropriate `IAuthenticatedEncryptorDescriptorDeserializer` instances to deserialize the documents back into `IAuthenticatedEncryptorDescriptor` instances, which are then wrapped in individual `IKey` instances.</span></span> <span data-ttu-id="fa4be-166">Ta kolekcja `IKey` wystąpień jest zwracany do obiektu wywołującego.</span><span class="sxs-lookup"><span data-stu-id="fa4be-166">This collection of `IKey` instances is returned to the caller.</span></span>

<span data-ttu-id="fa4be-167">Więcej informacji na temat konkretnego elementów XML znajduje się w [dokumentu w formacie magazynu kluczy](../implementation/key-storage-format.md#data-protection-implementation-key-storage-format).</span><span class="sxs-lookup"><span data-stu-id="fa4be-167">Further information on the particular XML elements can be found in the [key storage format document](../implementation/key-storage-format.md#data-protection-implementation-key-storage-format).</span></span>

## <a name="ixmlrepository"></a><span data-ttu-id="fa4be-168">IXmlRepository</span><span class="sxs-lookup"><span data-stu-id="fa4be-168">IXmlRepository</span></span>

<span data-ttu-id="fa4be-169">`IXmlRepository` Interfejsu reprezentuje typ, który można utrwalić XML i pobrać XML z magazynu zapasowego.</span><span class="sxs-lookup"><span data-stu-id="fa4be-169">The `IXmlRepository` interface represents a type that can persist XML to and retrieve XML from a backing store.</span></span> <span data-ttu-id="fa4be-170">Udostępnia ona dwóch interfejsów API:</span><span class="sxs-lookup"><span data-stu-id="fa4be-170">It exposes two APIs:</span></span>

* <span data-ttu-id="fa4be-171">GetAllElements(): IReadOnlyCollection<XElement></span><span class="sxs-lookup"><span data-stu-id="fa4be-171">GetAllElements() : IReadOnlyCollection<XElement></span></span>

* <span data-ttu-id="fa4be-172">StoreElement (element klasy XElement, friendlyName ciąg)</span><span class="sxs-lookup"><span data-stu-id="fa4be-172">StoreElement(XElement element, string friendlyName)</span></span>

<span data-ttu-id="fa4be-173">Implementacje `IXmlRepository` nie ma potrzeby analizy kodu XML przechodzącej przez nich.</span><span class="sxs-lookup"><span data-stu-id="fa4be-173">Implementations of `IXmlRepository` don't need to parse the XML passing through them.</span></span> <span data-ttu-id="fa4be-174">Powinny traktować jako nieprzezroczyste dokumentów XML i umożliwić wyższych warstw martwić generowania i analizowania dokumentów.</span><span class="sxs-lookup"><span data-stu-id="fa4be-174">They should treat the XML documents as opaque and let higher layers worry about generating and parsing the documents.</span></span>

<span data-ttu-id="fa4be-175">Istnieją dwa typy konkretnych wbudowanych implementujące `IXmlRepository`: `FileSystemXmlRepository` i `RegistryXmlRepository`.</span><span class="sxs-lookup"><span data-stu-id="fa4be-175">There are two built-in concrete types which implement `IXmlRepository`: `FileSystemXmlRepository` and `RegistryXmlRepository`.</span></span> <span data-ttu-id="fa4be-176">Zobacz [dokumentu dostawców magazynu kluczy](../implementation/key-storage-providers.md#data-protection-implementation-key-storage-providers) Aby uzyskać więcej informacji.</span><span class="sxs-lookup"><span data-stu-id="fa4be-176">See the [key storage providers document](../implementation/key-storage-providers.md#data-protection-implementation-key-storage-providers) for more information.</span></span> <span data-ttu-id="fa4be-177">Rejestrowanie niestandardowe `IXmlRepository` będzie odpowiedni sposób używania magazynu zapasowego różnych, np. magazyn obiektów Blob Azure.</span><span class="sxs-lookup"><span data-stu-id="fa4be-177">Registering a custom `IXmlRepository` would be the appropriate manner to use a different backing store, e.g., Azure Blob Storage.</span></span>

<span data-ttu-id="fa4be-178">Aby zmienić domyślne repozytorium całej aplikacji, rejestrowania niestandardowego `IXmlRepository` wystąpienie:</span><span class="sxs-lookup"><span data-stu-id="fa4be-178">To change the default repository application-wide, register a custom `IXmlRepository` instance:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="fa4be-179">Program ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="fa4be-179">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

   ```csharp
   services.Configure<KeyManagementOptions>(options => options.XmlRepository = new MyCustomXmlRepository());
   ```
   
# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="fa4be-180">Program ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="fa4be-180">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

   ```csharp
   services.AddSingleton<IXmlRepository>(new MyCustomXmlRepository());
   ```

---

<a name="data-protection-extensibility-key-management-ixmlencryptor"></a>

## <a name="ixmlencryptor"></a><span data-ttu-id="fa4be-181">IXmlEncryptor</span><span class="sxs-lookup"><span data-stu-id="fa4be-181">IXmlEncryptor</span></span>

<span data-ttu-id="fa4be-182">`IXmlEncryptor` Interfejsu reprezentuje typ, który można zaszyfrować element XML w postaci zwykłego tekstu.</span><span class="sxs-lookup"><span data-stu-id="fa4be-182">The `IXmlEncryptor` interface represents a type that can encrypt a plaintext XML element.</span></span> <span data-ttu-id="fa4be-183">Udostępnia ona jednego interfejsu API:</span><span class="sxs-lookup"><span data-stu-id="fa4be-183">It exposes a single API:</span></span>

* <span data-ttu-id="fa4be-184">Szyfrowanie (plaintextElement klasy XElement): EncryptedXmlInfo</span><span class="sxs-lookup"><span data-stu-id="fa4be-184">Encrypt(XElement plaintextElement) : EncryptedXmlInfo</span></span>

<span data-ttu-id="fa4be-185">W przypadku serializacji `IAuthenticatedEncryptorDescriptor` zawiera wszystkie elementy oznaczone jako "wymaga szyfrowania", następnie `XmlKeyManager` uruchomi tych elementów przy użyciu skonfigurowanego `IXmlEncryptor`w `Encrypt` — metoda która zostanie utrzymana enciphered elementu zamiast element w postaci zwykłego tekstu do `IXmlRepository`.</span><span class="sxs-lookup"><span data-stu-id="fa4be-185">If a serialized `IAuthenticatedEncryptorDescriptor` contains any elements marked as "requires encryption", then `XmlKeyManager` will run those elements through the configured `IXmlEncryptor`'s `Encrypt` method, and it will persist the enciphered element rather than the plaintext element to the `IXmlRepository`.</span></span> <span data-ttu-id="fa4be-186">Dane wyjściowe `Encrypt` jest metoda `EncryptedXmlInfo` obiektu.</span><span class="sxs-lookup"><span data-stu-id="fa4be-186">The output of the `Encrypt` method is an `EncryptedXmlInfo` object.</span></span> <span data-ttu-id="fa4be-187">Ten obiekt jest otoki, który zawiera wynikowe enciphered `XElement` i typ, który reprezentuje `IXmlDecryptor` które mogą służyć do odszyfrowania odpowiadającego mu elementu.</span><span class="sxs-lookup"><span data-stu-id="fa4be-187">This object is a wrapper which contains both the resultant enciphered `XElement` and the Type which represents an `IXmlDecryptor` which can be used to decipher the corresponding element.</span></span>

<span data-ttu-id="fa4be-188">Istnieją cztery wbudowanych typów konkretnych implementujące `IXmlEncryptor`:</span><span class="sxs-lookup"><span data-stu-id="fa4be-188">There are four built-in concrete types which implement `IXmlEncryptor`:</span></span>
* `CertificateXmlEncryptor`
* `DpapiNGXmlEncryptor`
* `DpapiXmlEncryptor`
* `NullXmlEncryptor`

<span data-ttu-id="fa4be-189">Zobacz [klucza szyfrowania w dokumencie rest](../implementation/key-encryption-at-rest.md#data-protection-implementation-key-encryption-at-rest) Aby uzyskać więcej informacji.</span><span class="sxs-lookup"><span data-stu-id="fa4be-189">See the [key encryption at rest document](../implementation/key-encryption-at-rest.md#data-protection-implementation-key-encryption-at-rest) for more information.</span></span>

<span data-ttu-id="fa4be-190">Aby zmienić domyślny mechanizm klucza szyfrowania w rest całej aplikacji, rejestrowania niestandardowego `IXmlEncryptor` wystąpienie:</span><span class="sxs-lookup"><span data-stu-id="fa4be-190">To change the default key-encryption-at-rest mechanism application-wide, register a custom `IXmlEncryptor` instance:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="fa4be-191">Program ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="fa4be-191">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

   ```csharp
   services.Configure<KeyManagementOptions>(options => options.XmlEncryptor = new MyCustomXmlEncryptor());
   ```
   
# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="fa4be-192">Program ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="fa4be-192">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

   ```csharp
   services.AddSingleton<IXmlEncryptor>(new MyCustomXmlEncryptor());
   ```

---

## <a name="ixmldecryptor"></a><span data-ttu-id="fa4be-193">IXmlDecryptor</span><span class="sxs-lookup"><span data-stu-id="fa4be-193">IXmlDecryptor</span></span>

<span data-ttu-id="fa4be-194">`IXmlDecryptor` Interfejsu reprezentuje typ, który potrafi do odszyfrowania `XElement` który został enciphered za pośrednictwem `IXmlEncryptor`.</span><span class="sxs-lookup"><span data-stu-id="fa4be-194">The `IXmlDecryptor` interface represents a type that knows how to decrypt an `XElement` that was enciphered via an `IXmlEncryptor`.</span></span> <span data-ttu-id="fa4be-195">Udostępnia ona jednego interfejsu API:</span><span class="sxs-lookup"><span data-stu-id="fa4be-195">It exposes a single API:</span></span>

* <span data-ttu-id="fa4be-196">Odszyfrowywanie (encryptedElement klasy XElement): klasy XElement</span><span class="sxs-lookup"><span data-stu-id="fa4be-196">Decrypt(XElement encryptedElement) : XElement</span></span>

<span data-ttu-id="fa4be-197">`Decrypt` Metody cofa szyfrowania wykonywane przez `IXmlEncryptor.Encrypt`.</span><span class="sxs-lookup"><span data-stu-id="fa4be-197">The `Decrypt` method undoes the encryption performed by `IXmlEncryptor.Encrypt`.</span></span> <span data-ttu-id="fa4be-198">Ogólnie rzecz biorąc, każdy konkretnych `IXmlEncryptor` implementacji będą miały odpowiednich konkretnych `IXmlDecryptor` implementacji.</span><span class="sxs-lookup"><span data-stu-id="fa4be-198">Generally, each concrete `IXmlEncryptor` implementation will have a corresponding concrete `IXmlDecryptor` implementation.</span></span>

<span data-ttu-id="fa4be-199">Typów implementujących `IXmlDecryptor` powinny mieć jedno z następujących dwóch konstruktorów publicznych:</span><span class="sxs-lookup"><span data-stu-id="fa4be-199">Types which implement `IXmlDecryptor` should have one of the following two public constructors:</span></span>

* <span data-ttu-id="fa4be-200">.ctor(IServiceProvider)</span><span class="sxs-lookup"><span data-stu-id="fa4be-200">.ctor(IServiceProvider)</span></span>
* <span data-ttu-id="fa4be-201">.ctor()</span><span class="sxs-lookup"><span data-stu-id="fa4be-201">.ctor()</span></span>

> [!NOTE]
> <span data-ttu-id="fa4be-202">`IServiceProvider` Przekazany do konstruktora może mieć wartości null.</span><span class="sxs-lookup"><span data-stu-id="fa4be-202">The `IServiceProvider` passed to the constructor may be null.</span></span>

## <a name="ikeyescrowsink"></a><span data-ttu-id="fa4be-203">IKeyEscrowSink</span><span class="sxs-lookup"><span data-stu-id="fa4be-203">IKeyEscrowSink</span></span>

<span data-ttu-id="fa4be-204">`IKeyEscrowSink` Interfejsu reprezentuje typ, który można wykonać depozytu poufne informacje.</span><span class="sxs-lookup"><span data-stu-id="fa4be-204">The `IKeyEscrowSink` interface represents a type that can perform escrow of sensitive information.</span></span> <span data-ttu-id="fa4be-205">Odwołaj serializacji deskryptory mogą zawierać poufne informacje (na przykład materiały kryptograficznych), czy jest to, co spowodowało wprowadzenie [IXmlEncryptor](xref:security/data-protection/extensibility/key-management#data-protection-extensibility-key-management-ixmlencryptor) wpisz w pierwszej kolejności.</span><span class="sxs-lookup"><span data-stu-id="fa4be-205">Recall that serialized descriptors might contain sensitive information (such as cryptographic material), and this is what led to the introduction of the [IXmlEncryptor](xref:security/data-protection/extensibility/key-management#data-protection-extensibility-key-management-ixmlencryptor) type in the first place.</span></span> <span data-ttu-id="fa4be-206">Jednak awarii, a klucz sygnałów mogą zostać usunięte lub uszkodzony.</span><span class="sxs-lookup"><span data-stu-id="fa4be-206">However, accidents happen, and key rings can be deleted or become corrupted.</span></span>

<span data-ttu-id="fa4be-207">Interfejs depozytu zapewnia kreskowania ewakuacji, umożliwiający dostęp do nieprzetworzonej serializacji XML przed jest przekształcana przez dowolne skonfigurowane [IXmlEncryptor](xref:security/data-protection/extensibility/key-management#data-protection-extensibility-key-management-ixmlencryptor).</span><span class="sxs-lookup"><span data-stu-id="fa4be-207">The escrow interface provides an emergency escape hatch, allowing access to the raw serialized XML before it is transformed by any configured [IXmlEncryptor](xref:security/data-protection/extensibility/key-management#data-protection-extensibility-key-management-ixmlencryptor).</span></span> <span data-ttu-id="fa4be-208">Interfejs udostępnia jeden interfejs API:</span><span class="sxs-lookup"><span data-stu-id="fa4be-208">The interface exposes a single API:</span></span>

* <span data-ttu-id="fa4be-209">Magazyn (identyfikator Guid klucza, element klasy XElement)</span><span class="sxs-lookup"><span data-stu-id="fa4be-209">Store(Guid keyId, XElement element)</span></span>

<span data-ttu-id="fa4be-210">Do `IKeyEscrowSink` implementacji do obsługi podany element w bezpieczny sposób zgodne z zasadami firmowymi.</span><span class="sxs-lookup"><span data-stu-id="fa4be-210">It is up to the `IKeyEscrowSink` implementation to handle the provided element in a secure manner consistent with business policy.</span></span> <span data-ttu-id="fa4be-211">Można jedna implementacja możliwe dla odbiorcy depozytu do szyfrowania elementu XML przy użyciu znanych firmowej certyfikatu X.509, gdzie klucz prywatny certyfikatu ma zdeponowane; `CertificateXmlEncryptor` typu może to pomóc.</span><span class="sxs-lookup"><span data-stu-id="fa4be-211">One possible implementation could be for the escrow sink to encrypt the XML element using a known corporate X.509 certificate where the certificate's private key has been escrowed; the `CertificateXmlEncryptor` type can assist with this.</span></span> <span data-ttu-id="fa4be-212">`IKeyEscrowSink` Implementacji również jest odpowiedzialny za odpowiednio utrwalanie podany element.</span><span class="sxs-lookup"><span data-stu-id="fa4be-212">The `IKeyEscrowSink` implementation is also responsible for persisting the provided element appropriately.</span></span>

<span data-ttu-id="fa4be-213">Mechanizm depozytu jest domyślnie włączona, ale administratorzy serwera mogą [to skonfigurować globalnie](xref:security/data-protection/configuration/machine-wide-policy).</span><span class="sxs-lookup"><span data-stu-id="fa4be-213">By default no escrow mechanism is enabled, though server administrators can [configure this globally](xref:security/data-protection/configuration/machine-wide-policy).</span></span> <span data-ttu-id="fa4be-214">Może również być skonfigurowana programowo za pośrednictwem `IDataProtectionBuilder.AddKeyEscrowSink` metody, jak pokazano w poniższym przykładzie.</span><span class="sxs-lookup"><span data-stu-id="fa4be-214">It can also be configured programmatically via the `IDataProtectionBuilder.AddKeyEscrowSink` method as shown in the sample below.</span></span> <span data-ttu-id="fa4be-215">`AddKeyEscrowSink` Dublowany przeciążenia metody `IServiceCollection.AddSingleton` i `IServiceCollection.AddInstance` przeciążenia, jako `IKeyEscrowSink` powinny być pojedynczych wystąpień.</span><span class="sxs-lookup"><span data-stu-id="fa4be-215">The `AddKeyEscrowSink` method overloads mirror the `IServiceCollection.AddSingleton` and `IServiceCollection.AddInstance` overloads, as `IKeyEscrowSink` instances are intended to be singletons.</span></span> <span data-ttu-id="fa4be-216">Jeśli wiele `IKeyEscrowSink` zarejestrowanych wystąpień, każdy z nich zostanie wywołana podczas generowania kluczy, więc kluczy można zdeponowane w wielu mechanizmów jednocześnie.</span><span class="sxs-lookup"><span data-stu-id="fa4be-216">If multiple `IKeyEscrowSink` instances are registered, each one will be called during key generation, so keys can be escrowed to multiple mechanisms simultaneously.</span></span>

<span data-ttu-id="fa4be-217">Brak żadnego interfejsu API można odczytać materiału z `IKeyEscrowSink` wystąpienia.</span><span class="sxs-lookup"><span data-stu-id="fa4be-217">There is no API to read material from an `IKeyEscrowSink` instance.</span></span> <span data-ttu-id="fa4be-218">Jest to zgodne z teorii projektu mechanizmu depozytu: ma zamierza udostępnić materiału klucza do zaufanego urzędu certyfikacji, a ponieważ aplikacja się nie jest zaufany urząd, nie powinien on mieć dostęp do własnej escrowed materiału.</span><span class="sxs-lookup"><span data-stu-id="fa4be-218">This is consistent with the design theory of the escrow mechanism: it's intended to make the key material accessible to a trusted authority, and since the application is itself not a trusted authority, it shouldn't have access to its own escrowed material.</span></span>

<span data-ttu-id="fa4be-219">Następujący przykładowy kod przedstawia tworzenie i rejestrowanie `IKeyEscrowSink` gdzie klucze są zdeponowane w taki sposób, aby je odzyskać tylko elementy członkowskie "CONTOSODomain Administratorzy".</span><span class="sxs-lookup"><span data-stu-id="fa4be-219">The following sample code demonstrates creating and registering an `IKeyEscrowSink` where keys are escrowed such that only members of "CONTOSODomain Admins" can recover them.</span></span>

> [!NOTE]
> <span data-ttu-id="fa4be-220">Aby uruchomić ten przykład, użytkownik musi być na przyłączonych do domeny systemu Windows 8 / maszyny z systemem Windows Server 2012, a kontroler domeny musi być systemu Windows Server 2012 lub nowszym.</span><span class="sxs-lookup"><span data-stu-id="fa4be-220">To run this sample, you must be on a domain-joined Windows 8 / Windows Server 2012 machine, and the domain controller must be Windows Server 2012 or later.</span></span>

[!code-csharp[Main](key-management/samples/key-management-extensibility.cs)]
