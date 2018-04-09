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
# <a name="key-management-extensibility-in-aspnet-core"></a><span data-ttu-id="ce176-103">Zarządzanie kluczami rozszerzalność na platformie ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="ce176-103">Key management extensibility in ASP.NET Core</span></span>

<a name="data-protection-extensibility-key-management"></a>

>[!TIP]
> <span data-ttu-id="ce176-104">Odczyt [zarządzanie kluczami](xref:security/data-protection/implementation/key-management#data-protection-implementation-key-management) sekcji przed przeczytaniem tej części, jak wyjaśniono niektóre podstawowe pojęcia dotyczące tych interfejsów API.</span><span class="sxs-lookup"><span data-stu-id="ce176-104">Read the [key management](xref:security/data-protection/implementation/key-management#data-protection-implementation-key-management) section before reading this section, as it explains some of the fundamental concepts behind these APIs.</span></span>

>[!WARNING]
> <span data-ttu-id="ce176-105">Typy implementujące żadnego z następujących interfejsów powinny być bezpieczne wątkowo dla wielu wywołań.</span><span class="sxs-lookup"><span data-stu-id="ce176-105">Types that implement any of the following interfaces should be thread-safe for multiple callers.</span></span>

## <a name="key"></a><span data-ttu-id="ce176-106">Key</span><span class="sxs-lookup"><span data-stu-id="ce176-106">Key</span></span>

<span data-ttu-id="ce176-107">`IKey` Interfejs jest podstawowe reprezentacja klucza w cryptosystem.</span><span class="sxs-lookup"><span data-stu-id="ce176-107">The `IKey` interface is the basic representation of a key in cryptosystem.</span></span> <span data-ttu-id="ce176-108">Klucz termin jest używany tutaj w tym sensie abstrakcyjne nie znajduje się w rozumieniu literału "materiału klucza kryptograficznego".</span><span class="sxs-lookup"><span data-stu-id="ce176-108">The term key is used here in the abstract sense, not in the literal sense of "cryptographic key material".</span></span> <span data-ttu-id="ce176-109">Klucz ma następujące właściwości:</span><span class="sxs-lookup"><span data-stu-id="ce176-109">A key has the following properties:</span></span>

* <span data-ttu-id="ce176-110">Daty aktywacji, tworzenie i wygaśnięcia</span><span class="sxs-lookup"><span data-stu-id="ce176-110">Activation, creation, and expiration dates</span></span>

* <span data-ttu-id="ce176-111">Stan odwołania</span><span class="sxs-lookup"><span data-stu-id="ce176-111">Revocation status</span></span>

* <span data-ttu-id="ce176-112">Identyfikator klucza (GUID)</span><span class="sxs-lookup"><span data-stu-id="ce176-112">Key identifier (a GUID)</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="ce176-113">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="ce176-113">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="ce176-114">Ponadto `IKey` przedstawia `CreateEncryptor` metodę, która może służyć do tworzenia [IAuthenticatedEncryptor](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptor) wystąpienia powiązane z danym kluczem.</span><span class="sxs-lookup"><span data-stu-id="ce176-114">Additionally, `IKey` exposes a `CreateEncryptor` method which can be used to create an [IAuthenticatedEncryptor](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptor) instance tied to this key.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="ce176-115">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="ce176-115">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="ce176-116">Ponadto `IKey` przedstawia `CreateEncryptorInstance` metodę, która może służyć do tworzenia [IAuthenticatedEncryptor](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptor) wystąpienia powiązane z danym kluczem.</span><span class="sxs-lookup"><span data-stu-id="ce176-116">Additionally, `IKey` exposes a `CreateEncryptorInstance` method which can be used to create an [IAuthenticatedEncryptor](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptor) instance tied to this key.</span></span>

---

> [!NOTE]
> <span data-ttu-id="ce176-117">Brak żadnego interfejsu API można pobrać raw materiał kryptograficznych z `IKey` wystąpienia.</span><span class="sxs-lookup"><span data-stu-id="ce176-117">There's no API to retrieve the raw cryptographic material from an `IKey` instance.</span></span>

## <a name="ikeymanager"></a><span data-ttu-id="ce176-118">IKeyManager</span><span class="sxs-lookup"><span data-stu-id="ce176-118">IKeyManager</span></span>

<span data-ttu-id="ce176-119">`IKeyManager` Interfejsu reprezentuje obiekt odpowiedzialny za ogólne magazynu kluczy, pobieranie i manipulowania nimi.</span><span class="sxs-lookup"><span data-stu-id="ce176-119">The `IKeyManager` interface represents an object responsible for general key storage, retrieval, and manipulation.</span></span> <span data-ttu-id="ce176-120">Udostępnia on trzy operacje wysokiego poziomu:</span><span class="sxs-lookup"><span data-stu-id="ce176-120">It exposes three high-level operations:</span></span>

* <span data-ttu-id="ce176-121">Utwórz nowy klucz i zachować go do magazynu.</span><span class="sxs-lookup"><span data-stu-id="ce176-121">Create a new key and persist it to storage.</span></span>

* <span data-ttu-id="ce176-122">Pobierz wszystkie klucze z magazynu.</span><span class="sxs-lookup"><span data-stu-id="ce176-122">Get all keys from storage.</span></span>

* <span data-ttu-id="ce176-123">Odwołaj klucze co najmniej jednego i zachować informacje o odwołaniu do magazynu.</span><span class="sxs-lookup"><span data-stu-id="ce176-123">Revoke one or more keys and persist the revocation information to storage.</span></span>

>[!WARNING]
> <span data-ttu-id="ce176-124">Zapisywanie `IKeyManager` jest bardzo zaawansowane zadania, a większość deweloperów nie powinny podejmować go.</span><span class="sxs-lookup"><span data-stu-id="ce176-124">Writing an `IKeyManager` is a very advanced task, and the majority of developers shouldn't attempt it.</span></span> <span data-ttu-id="ce176-125">Zamiast tego większość deweloperów należy wykorzystać możliwości oferowane przez [XmlKeyManager](xref:security/data-protection/extensibility/key-management#data-protection-extensibility-key-management-xmlkeymanager) klasy.</span><span class="sxs-lookup"><span data-stu-id="ce176-125">Instead, most developers should take advantage of the facilities offered by the [XmlKeyManager](xref:security/data-protection/extensibility/key-management#data-protection-extensibility-key-management-xmlkeymanager) class.</span></span>

<a name="data-protection-extensibility-key-management-xmlkeymanager"></a>

## <a name="xmlkeymanager"></a><span data-ttu-id="ce176-126">XmlKeyManager</span><span class="sxs-lookup"><span data-stu-id="ce176-126">XmlKeyManager</span></span>

<span data-ttu-id="ce176-127">`XmlKeyManager` w polu wykonanie jest typu `IKeyManager`.</span><span class="sxs-lookup"><span data-stu-id="ce176-127">The `XmlKeyManager` type is the in-box concrete implementation of `IKeyManager`.</span></span> <span data-ttu-id="ce176-128">Udostępnia kilka przydatne urządzeń, w tym depozytu kluczy i szyfrowania kluczy w stanie spoczynku.</span><span class="sxs-lookup"><span data-stu-id="ce176-128">It provides several useful facilities, including key escrow and encryption of keys at rest.</span></span> <span data-ttu-id="ce176-129">Klucze w tym systemie są reprezentowane jako elementy XML (w szczególności [klasy XElement](https://docs.microsoft.com/dotnet/csharp/programming-guide/concepts/linq/xelement-class-overview)).</span><span class="sxs-lookup"><span data-stu-id="ce176-129">Keys in this system are represented as XML elements (specifically, [XElement](https://docs.microsoft.com/dotnet/csharp/programming-guide/concepts/linq/xelement-class-overview)).</span></span>

<span data-ttu-id="ce176-130">`XmlKeyManager` zależy od kilku składników w trakcie wypełnienia swoich zadań:</span><span class="sxs-lookup"><span data-stu-id="ce176-130">`XmlKeyManager` depends on several other components in the course of fulfilling its tasks:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="ce176-131">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="ce176-131">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

* <span data-ttu-id="ce176-132">`AlgorithmConfiguration`, które nakazują algorytmów używanych przez nowych kluczy.</span><span class="sxs-lookup"><span data-stu-id="ce176-132">`AlgorithmConfiguration`, which dictates the algorithms used by new keys.</span></span>

* <span data-ttu-id="ce176-133">`IXmlRepository`, które kontrolki, w której klucze są utrwalane w magazynie.</span><span class="sxs-lookup"><span data-stu-id="ce176-133">`IXmlRepository`, which controls where keys are persisted in storage.</span></span>

* <span data-ttu-id="ce176-134">`IXmlEncryptor` [opcjonalnie], co pozwala na szyfrowanie kluczy w stanie spoczynku.</span><span class="sxs-lookup"><span data-stu-id="ce176-134">`IXmlEncryptor` [optional], which allows encrypting keys at rest.</span></span>

* <span data-ttu-id="ce176-135">`IKeyEscrowSink` [opcjonalnie], która udostępnia usługi kluczy depozytu.</span><span class="sxs-lookup"><span data-stu-id="ce176-135">`IKeyEscrowSink` [optional], which provides key escrow services.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="ce176-136">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="ce176-136">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

* <span data-ttu-id="ce176-137">`IXmlRepository`, które kontrolki, w której klucze są utrwalane w magazynie.</span><span class="sxs-lookup"><span data-stu-id="ce176-137">`IXmlRepository`, which controls where keys are persisted in storage.</span></span>

* <span data-ttu-id="ce176-138">`IXmlEncryptor` [opcjonalnie], co pozwala na szyfrowanie kluczy w stanie spoczynku.</span><span class="sxs-lookup"><span data-stu-id="ce176-138">`IXmlEncryptor` [optional], which allows encrypting keys at rest.</span></span>

* <span data-ttu-id="ce176-139">`IKeyEscrowSink` [opcjonalnie], która udostępnia usługi kluczy depozytu.</span><span class="sxs-lookup"><span data-stu-id="ce176-139">`IKeyEscrowSink` [optional], which provides key escrow services.</span></span>

---

<span data-ttu-id="ce176-140">Poniżej przedstawiono diagramy wysokiego poziomu, które wskazują, jak te składniki są połączone ze sobą w `XmlKeyManager`.</span><span class="sxs-lookup"><span data-stu-id="ce176-140">Below are high-level diagrams which indicate how these components are wired together within `XmlKeyManager`.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="ce176-141">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="ce176-141">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

   ![Tworzenie kluczy](key-management/_static/keycreation2.png)

   <span data-ttu-id="ce176-143">*Tworzenie klucza / CreateNewKey*</span><span class="sxs-lookup"><span data-stu-id="ce176-143">*Key Creation / CreateNewKey*</span></span>

<span data-ttu-id="ce176-144">W implementacji `CreateNewKey`, `AlgorithmConfiguration` składnik jest używany do utworzenia unikatowego `IAuthenticatedEncryptorDescriptor`, która jest następnie zserializowanym w formacie XML.</span><span class="sxs-lookup"><span data-stu-id="ce176-144">In the implementation of `CreateNewKey`, the `AlgorithmConfiguration` component is used to create a unique `IAuthenticatedEncryptorDescriptor`, which is then serialized as XML.</span></span> <span data-ttu-id="ce176-145">Jeśli występuje zbiornika kluczy depozytu raw XML (niezaszyfrowany) zapewnia sink do długoterminowego przechowywania.</span><span class="sxs-lookup"><span data-stu-id="ce176-145">If a key escrow sink is present, the raw (unencrypted) XML is provided to the sink for long-term storage.</span></span> <span data-ttu-id="ce176-146">Uruchom za pośrednictwem nieszyfrowanego XML `IXmlEncryptor` (jeśli jest to wymagane) do generowania zaszyfrowanego dokumentu XML.</span><span class="sxs-lookup"><span data-stu-id="ce176-146">The unencrypted XML is then run through an `IXmlEncryptor` (if required) to generate the encrypted XML document.</span></span> <span data-ttu-id="ce176-147">Ten dokument zaszyfrowanych jest trwały do magazynu długoterminowego za pośrednictwem `IXmlRepository`.</span><span class="sxs-lookup"><span data-stu-id="ce176-147">This encrypted document is persisted to long-term storage via the `IXmlRepository`.</span></span> <span data-ttu-id="ce176-148">(Jeśli nie `IXmlEncryptor` jest skonfigurowany, niezaszyfrowane dokumentu jest utrwalona w `IXmlRepository`.)</span><span class="sxs-lookup"><span data-stu-id="ce176-148">(If no `IXmlEncryptor` is configured, the unencrypted document is persisted in the `IXmlRepository`.)</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="ce176-149">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="ce176-149">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

   ![Tworzenie kluczy](key-management/_static/keycreation1.png)

   <span data-ttu-id="ce176-151">*Tworzenie klucza / CreateNewKey*</span><span class="sxs-lookup"><span data-stu-id="ce176-151">*Key Creation / CreateNewKey*</span></span>

<span data-ttu-id="ce176-152">W implementacji `CreateNewKey`, `IAuthenticatedEncryptorConfiguration` składnik jest używany do utworzenia unikatowego `IAuthenticatedEncryptorDescriptor`, która jest następnie zserializowanym w formacie XML.</span><span class="sxs-lookup"><span data-stu-id="ce176-152">In the implementation of `CreateNewKey`, the `IAuthenticatedEncryptorConfiguration` component is used to create a unique `IAuthenticatedEncryptorDescriptor`, which is then serialized as XML.</span></span> <span data-ttu-id="ce176-153">Jeśli występuje zbiornika kluczy depozytu raw XML (niezaszyfrowany) zapewnia sink do długoterminowego przechowywania.</span><span class="sxs-lookup"><span data-stu-id="ce176-153">If a key escrow sink is present, the raw (unencrypted) XML is provided to the sink for long-term storage.</span></span> <span data-ttu-id="ce176-154">Uruchom za pośrednictwem nieszyfrowanego XML `IXmlEncryptor` (jeśli jest to wymagane) do generowania zaszyfrowanego dokumentu XML.</span><span class="sxs-lookup"><span data-stu-id="ce176-154">The unencrypted XML is then run through an `IXmlEncryptor` (if required) to generate the encrypted XML document.</span></span> <span data-ttu-id="ce176-155">Ten dokument zaszyfrowanych jest trwały do magazynu długoterminowego za pośrednictwem `IXmlRepository`.</span><span class="sxs-lookup"><span data-stu-id="ce176-155">This encrypted document is persisted to long-term storage via the `IXmlRepository`.</span></span> <span data-ttu-id="ce176-156">(Jeśli nie `IXmlEncryptor` jest skonfigurowany, niezaszyfrowane dokumentu jest utrwalona w `IXmlRepository`.)</span><span class="sxs-lookup"><span data-stu-id="ce176-156">(If no `IXmlEncryptor` is configured, the unencrypted document is persisted in the `IXmlRepository`.)</span></span>

---

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="ce176-157">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="ce176-157">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

   ![Pobieranie klucza](key-management/_static/keyretrieval2.png)
   
# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="ce176-159">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="ce176-159">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

   ![Pobieranie klucza](key-management/_static/keyretrieval1.png)

---

   <span data-ttu-id="ce176-161">*Klucz pobierania / GetAllKeys*</span><span class="sxs-lookup"><span data-stu-id="ce176-161">*Key Retrieval / GetAllKeys*</span></span>

<span data-ttu-id="ce176-162">W implementacji `GetAllKeys`, reprezentująca klucze dokumentów XML i odwołań są odczytywane z podstawową `IXmlRepository`.</span><span class="sxs-lookup"><span data-stu-id="ce176-162">In the implementation of `GetAllKeys`, the XML documents representing keys and revocations are read from the underlying `IXmlRepository`.</span></span> <span data-ttu-id="ce176-163">Jeśli te dokumenty są szyfrowane, system automatycznie odszyfrować je.</span><span class="sxs-lookup"><span data-stu-id="ce176-163">If these documents are encrypted, the system will automatically decrypt them.</span></span> <span data-ttu-id="ce176-164">`XmlKeyManager` tworzy odpowiednie `IAuthenticatedEncryptorDescriptorDeserializer` wystąpień do deserializacji dokumenty z powrotem do `IAuthenticatedEncryptorDescriptor` wystąpienia, które następnie są ujęte w poszczególnych `IKey` wystąpień.</span><span class="sxs-lookup"><span data-stu-id="ce176-164">`XmlKeyManager` creates the appropriate `IAuthenticatedEncryptorDescriptorDeserializer` instances to deserialize the documents back into `IAuthenticatedEncryptorDescriptor` instances, which are then wrapped in individual `IKey` instances.</span></span> <span data-ttu-id="ce176-165">Ta kolekcja `IKey` wystąpień jest zwracany do obiektu wywołującego.</span><span class="sxs-lookup"><span data-stu-id="ce176-165">This collection of `IKey` instances is returned to the caller.</span></span>

<span data-ttu-id="ce176-166">Więcej informacji na temat konkretnego elementów XML znajduje się w [dokumentu w formacie magazynu kluczy](xref:security/data-protection/implementation/key-storage-format#data-protection-implementation-key-storage-format).</span><span class="sxs-lookup"><span data-stu-id="ce176-166">Further information on the particular XML elements can be found in the [key storage format document](xref:security/data-protection/implementation/key-storage-format#data-protection-implementation-key-storage-format).</span></span>

## <a name="ixmlrepository"></a><span data-ttu-id="ce176-167">IXmlRepository</span><span class="sxs-lookup"><span data-stu-id="ce176-167">IXmlRepository</span></span>

<span data-ttu-id="ce176-168">`IXmlRepository` Interfejsu reprezentuje typ, który można utrwalić XML i pobrać XML z magazynu zapasowego.</span><span class="sxs-lookup"><span data-stu-id="ce176-168">The `IXmlRepository` interface represents a type that can persist XML to and retrieve XML from a backing store.</span></span> <span data-ttu-id="ce176-169">Udostępnia ona dwóch interfejsów API:</span><span class="sxs-lookup"><span data-stu-id="ce176-169">It exposes two APIs:</span></span>

* <span data-ttu-id="ce176-170">GetAllElements(): IReadOnlyCollection<XElement></span><span class="sxs-lookup"><span data-stu-id="ce176-170">GetAllElements() : IReadOnlyCollection<XElement></span></span>

* <span data-ttu-id="ce176-171">StoreElement (element klasy XElement, friendlyName ciąg)</span><span class="sxs-lookup"><span data-stu-id="ce176-171">StoreElement(XElement element, string friendlyName)</span></span>

<span data-ttu-id="ce176-172">Implementacje `IXmlRepository` nie ma potrzeby analizy kodu XML przechodzącej przez nich.</span><span class="sxs-lookup"><span data-stu-id="ce176-172">Implementations of `IXmlRepository` don't need to parse the XML passing through them.</span></span> <span data-ttu-id="ce176-173">Powinny traktować jako nieprzezroczyste dokumentów XML i umożliwić wyższych warstw martwić generowania i analizowania dokumentów.</span><span class="sxs-lookup"><span data-stu-id="ce176-173">They should treat the XML documents as opaque and let higher layers worry about generating and parsing the documents.</span></span>

<span data-ttu-id="ce176-174">Istnieją dwa typy konkretnych wbudowanych implementujące `IXmlRepository`: `FileSystemXmlRepository` i `RegistryXmlRepository`.</span><span class="sxs-lookup"><span data-stu-id="ce176-174">There are two built-in concrete types which implement `IXmlRepository`: `FileSystemXmlRepository` and `RegistryXmlRepository`.</span></span> <span data-ttu-id="ce176-175">Zobacz [dokumentu dostawców magazynu kluczy](xref:security/data-protection/implementation/key-storage-providers#data-protection-implementation-key-storage-providers) Aby uzyskać więcej informacji.</span><span class="sxs-lookup"><span data-stu-id="ce176-175">See the [key storage providers document](xref:security/data-protection/implementation/key-storage-providers#data-protection-implementation-key-storage-providers) for more information.</span></span> <span data-ttu-id="ce176-176">Rejestrowanie niestandardowe `IXmlRepository` będzie odpowiedni sposób używania magazynu zapasowego różnych, np. magazyn obiektów Blob Azure.</span><span class="sxs-lookup"><span data-stu-id="ce176-176">Registering a custom `IXmlRepository` would be the appropriate manner to use a different backing store, e.g., Azure Blob Storage.</span></span>

<span data-ttu-id="ce176-177">Aby zmienić domyślne repozytorium całej aplikacji, rejestrowania niestandardowego `IXmlRepository` wystąpienie:</span><span class="sxs-lookup"><span data-stu-id="ce176-177">To change the default repository application-wide, register a custom `IXmlRepository` instance:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="ce176-178">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="ce176-178">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

   ```csharp
   services.Configure<KeyManagementOptions>(options => options.XmlRepository = new MyCustomXmlRepository());
   ```
   
# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="ce176-179">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="ce176-179">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

   ```csharp
   services.AddSingleton<IXmlRepository>(new MyCustomXmlRepository());
   ```

---

<a name="data-protection-extensibility-key-management-ixmlencryptor"></a>

## <a name="ixmlencryptor"></a><span data-ttu-id="ce176-180">IXmlEncryptor</span><span class="sxs-lookup"><span data-stu-id="ce176-180">IXmlEncryptor</span></span>

<span data-ttu-id="ce176-181">`IXmlEncryptor` Interfejsu reprezentuje typ, który można zaszyfrować element XML w postaci zwykłego tekstu.</span><span class="sxs-lookup"><span data-stu-id="ce176-181">The `IXmlEncryptor` interface represents a type that can encrypt a plaintext XML element.</span></span> <span data-ttu-id="ce176-182">Udostępnia ona jednego interfejsu API:</span><span class="sxs-lookup"><span data-stu-id="ce176-182">It exposes a single API:</span></span>

* <span data-ttu-id="ce176-183">Szyfrowanie (plaintextElement klasy XElement): EncryptedXmlInfo</span><span class="sxs-lookup"><span data-stu-id="ce176-183">Encrypt(XElement plaintextElement) : EncryptedXmlInfo</span></span>

<span data-ttu-id="ce176-184">W przypadku serializacji `IAuthenticatedEncryptorDescriptor` zawiera wszystkie elementy oznaczone jako "wymaga szyfrowania", następnie `XmlKeyManager` uruchomi tych elementów przy użyciu skonfigurowanego `IXmlEncryptor`w `Encrypt` — metoda która zostanie utrzymana enciphered elementu zamiast element w postaci zwykłego tekstu do `IXmlRepository`.</span><span class="sxs-lookup"><span data-stu-id="ce176-184">If a serialized `IAuthenticatedEncryptorDescriptor` contains any elements marked as "requires encryption", then `XmlKeyManager` will run those elements through the configured `IXmlEncryptor`'s `Encrypt` method, and it will persist the enciphered element rather than the plaintext element to the `IXmlRepository`.</span></span> <span data-ttu-id="ce176-185">Dane wyjściowe `Encrypt` jest metoda `EncryptedXmlInfo` obiektu.</span><span class="sxs-lookup"><span data-stu-id="ce176-185">The output of the `Encrypt` method is an `EncryptedXmlInfo` object.</span></span> <span data-ttu-id="ce176-186">Ten obiekt jest otoki, który zawiera wynikowe enciphered `XElement` i typ, który reprezentuje `IXmlDecryptor` które mogą służyć do odszyfrowania odpowiadającego mu elementu.</span><span class="sxs-lookup"><span data-stu-id="ce176-186">This object is a wrapper which contains both the resultant enciphered `XElement` and the Type which represents an `IXmlDecryptor` which can be used to decipher the corresponding element.</span></span>

<span data-ttu-id="ce176-187">Istnieją cztery wbudowanych typów konkretnych implementujące `IXmlEncryptor`:</span><span class="sxs-lookup"><span data-stu-id="ce176-187">There are four built-in concrete types which implement `IXmlEncryptor`:</span></span>
* `CertificateXmlEncryptor`
* `DpapiNGXmlEncryptor`
* `DpapiXmlEncryptor`
* `NullXmlEncryptor`

<span data-ttu-id="ce176-188">Zobacz [klucza szyfrowania w dokumencie rest](xref:security/data-protection/implementation/key-encryption-at-rest#data-protection-implementation-key-encryption-at-rest) Aby uzyskać więcej informacji.</span><span class="sxs-lookup"><span data-stu-id="ce176-188">See the [key encryption at rest document](xref:security/data-protection/implementation/key-encryption-at-rest#data-protection-implementation-key-encryption-at-rest) for more information.</span></span>

<span data-ttu-id="ce176-189">Aby zmienić domyślny mechanizm klucza szyfrowania w rest całej aplikacji, rejestrowania niestandardowego `IXmlEncryptor` wystąpienie:</span><span class="sxs-lookup"><span data-stu-id="ce176-189">To change the default key-encryption-at-rest mechanism application-wide, register a custom `IXmlEncryptor` instance:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="ce176-190">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="ce176-190">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

   ```csharp
   services.Configure<KeyManagementOptions>(options => options.XmlEncryptor = new MyCustomXmlEncryptor());
   ```
   
# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="ce176-191">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="ce176-191">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

   ```csharp
   services.AddSingleton<IXmlEncryptor>(new MyCustomXmlEncryptor());
   ```

---

## <a name="ixmldecryptor"></a><span data-ttu-id="ce176-192">IXmlDecryptor</span><span class="sxs-lookup"><span data-stu-id="ce176-192">IXmlDecryptor</span></span>

<span data-ttu-id="ce176-193">`IXmlDecryptor` Interfejsu reprezentuje typ, który potrafi do odszyfrowania `XElement` który został enciphered za pośrednictwem `IXmlEncryptor`.</span><span class="sxs-lookup"><span data-stu-id="ce176-193">The `IXmlDecryptor` interface represents a type that knows how to decrypt an `XElement` that was enciphered via an `IXmlEncryptor`.</span></span> <span data-ttu-id="ce176-194">Udostępnia ona jednego interfejsu API:</span><span class="sxs-lookup"><span data-stu-id="ce176-194">It exposes a single API:</span></span>

* <span data-ttu-id="ce176-195">Odszyfrowywanie (encryptedElement klasy XElement): klasy XElement</span><span class="sxs-lookup"><span data-stu-id="ce176-195">Decrypt(XElement encryptedElement) : XElement</span></span>

<span data-ttu-id="ce176-196">`Decrypt` Metody cofa szyfrowania wykonywane przez `IXmlEncryptor.Encrypt`.</span><span class="sxs-lookup"><span data-stu-id="ce176-196">The `Decrypt` method undoes the encryption performed by `IXmlEncryptor.Encrypt`.</span></span> <span data-ttu-id="ce176-197">Ogólnie rzecz biorąc, każdy konkretnych `IXmlEncryptor` implementacji będą miały odpowiednich konkretnych `IXmlDecryptor` implementacji.</span><span class="sxs-lookup"><span data-stu-id="ce176-197">Generally, each concrete `IXmlEncryptor` implementation will have a corresponding concrete `IXmlDecryptor` implementation.</span></span>

<span data-ttu-id="ce176-198">Typów implementujących `IXmlDecryptor` powinny mieć jedno z następujących dwóch konstruktorów publicznych:</span><span class="sxs-lookup"><span data-stu-id="ce176-198">Types which implement `IXmlDecryptor` should have one of the following two public constructors:</span></span>

* <span data-ttu-id="ce176-199">.ctor(IServiceProvider)</span><span class="sxs-lookup"><span data-stu-id="ce176-199">.ctor(IServiceProvider)</span></span>
* <span data-ttu-id="ce176-200">.ctor()</span><span class="sxs-lookup"><span data-stu-id="ce176-200">.ctor()</span></span>

> [!NOTE]
> <span data-ttu-id="ce176-201">`IServiceProvider` Przekazany do konstruktora może mieć wartości null.</span><span class="sxs-lookup"><span data-stu-id="ce176-201">The `IServiceProvider` passed to the constructor may be null.</span></span>

## <a name="ikeyescrowsink"></a><span data-ttu-id="ce176-202">IKeyEscrowSink</span><span class="sxs-lookup"><span data-stu-id="ce176-202">IKeyEscrowSink</span></span>

<span data-ttu-id="ce176-203">`IKeyEscrowSink` Interfejsu reprezentuje typ, który można wykonać depozytu poufne informacje.</span><span class="sxs-lookup"><span data-stu-id="ce176-203">The `IKeyEscrowSink` interface represents a type that can perform escrow of sensitive information.</span></span> <span data-ttu-id="ce176-204">Odwołaj serializacji deskryptory mogą zawierać poufne informacje (na przykład materiały kryptograficznych), czy jest to, co spowodowało wprowadzenie [IXmlEncryptor](xref:security/data-protection/extensibility/key-management#data-protection-extensibility-key-management-ixmlencryptor) wpisz w pierwszej kolejności.</span><span class="sxs-lookup"><span data-stu-id="ce176-204">Recall that serialized descriptors might contain sensitive information (such as cryptographic material), and this is what led to the introduction of the [IXmlEncryptor](xref:security/data-protection/extensibility/key-management#data-protection-extensibility-key-management-ixmlencryptor) type in the first place.</span></span> <span data-ttu-id="ce176-205">Jednak awarii, a klucz sygnałów mogą zostać usunięte lub uszkodzony.</span><span class="sxs-lookup"><span data-stu-id="ce176-205">However, accidents happen, and key rings can be deleted or become corrupted.</span></span>

<span data-ttu-id="ce176-206">Interfejs depozytu zapewnia kreskowania ewakuacji, umożliwiający dostęp do nieprzetworzonej serializacji XML przed jest przekształcana przez dowolne skonfigurowane [IXmlEncryptor](xref:security/data-protection/extensibility/key-management#data-protection-extensibility-key-management-ixmlencryptor).</span><span class="sxs-lookup"><span data-stu-id="ce176-206">The escrow interface provides an emergency escape hatch, allowing access to the raw serialized XML before it's transformed by any configured [IXmlEncryptor](xref:security/data-protection/extensibility/key-management#data-protection-extensibility-key-management-ixmlencryptor).</span></span> <span data-ttu-id="ce176-207">Interfejs udostępnia jeden interfejs API:</span><span class="sxs-lookup"><span data-stu-id="ce176-207">The interface exposes a single API:</span></span>

* <span data-ttu-id="ce176-208">Magazyn (identyfikator Guid klucza, element klasy XElement)</span><span class="sxs-lookup"><span data-stu-id="ce176-208">Store(Guid keyId, XElement element)</span></span>

<span data-ttu-id="ce176-209">Do `IKeyEscrowSink` implementacji do obsługi podany element w bezpieczny sposób zgodne z zasadami firmowymi.</span><span class="sxs-lookup"><span data-stu-id="ce176-209">It's up to the `IKeyEscrowSink` implementation to handle the provided element in a secure manner consistent with business policy.</span></span> <span data-ttu-id="ce176-210">Można jedna implementacja możliwe dla odbiorcy depozytu do szyfrowania elementu XML przy użyciu znanych firmowej certyfikatu X.509, gdzie klucz prywatny certyfikatu ma zdeponowane; `CertificateXmlEncryptor` typu może to pomóc.</span><span class="sxs-lookup"><span data-stu-id="ce176-210">One possible implementation could be for the escrow sink to encrypt the XML element using a known corporate X.509 certificate where the certificate's private key has been escrowed; the `CertificateXmlEncryptor` type can assist with this.</span></span> <span data-ttu-id="ce176-211">`IKeyEscrowSink` Implementacji również jest odpowiedzialny za odpowiednio utrwalanie podany element.</span><span class="sxs-lookup"><span data-stu-id="ce176-211">The `IKeyEscrowSink` implementation is also responsible for persisting the provided element appropriately.</span></span>

<span data-ttu-id="ce176-212">Mechanizm depozytu jest domyślnie włączona, ale administratorzy serwera mogą [to skonfigurować globalnie](xref:security/data-protection/configuration/machine-wide-policy).</span><span class="sxs-lookup"><span data-stu-id="ce176-212">By default no escrow mechanism is enabled, though server administrators can [configure this globally](xref:security/data-protection/configuration/machine-wide-policy).</span></span> <span data-ttu-id="ce176-213">Może również być skonfigurowana programowo za pośrednictwem `IDataProtectionBuilder.AddKeyEscrowSink` metody, jak pokazano w poniższym przykładzie.</span><span class="sxs-lookup"><span data-stu-id="ce176-213">It can also be configured programmatically via the `IDataProtectionBuilder.AddKeyEscrowSink` method as shown in the sample below.</span></span> <span data-ttu-id="ce176-214">`AddKeyEscrowSink` Dublowany przeciążenia metody `IServiceCollection.AddSingleton` i `IServiceCollection.AddInstance` przeciążenia, jako `IKeyEscrowSink` powinny być pojedynczych wystąpień.</span><span class="sxs-lookup"><span data-stu-id="ce176-214">The `AddKeyEscrowSink` method overloads mirror the `IServiceCollection.AddSingleton` and `IServiceCollection.AddInstance` overloads, as `IKeyEscrowSink` instances are intended to be singletons.</span></span> <span data-ttu-id="ce176-215">Jeśli wiele `IKeyEscrowSink` zarejestrowanych wystąpień, każdy z nich zostanie wywołana podczas generowania kluczy, więc kluczy można zdeponowane w wielu mechanizmów jednocześnie.</span><span class="sxs-lookup"><span data-stu-id="ce176-215">If multiple `IKeyEscrowSink` instances are registered, each one will be called during key generation, so keys can be escrowed to multiple mechanisms simultaneously.</span></span>

<span data-ttu-id="ce176-216">Brak żadnego interfejsu API można odczytać materiału z `IKeyEscrowSink` wystąpienia.</span><span class="sxs-lookup"><span data-stu-id="ce176-216">There's no API to read material from an `IKeyEscrowSink` instance.</span></span> <span data-ttu-id="ce176-217">Jest to zgodne z teorii projektu mechanizmu depozytu: ma zamierza udostępnić materiału klucza do zaufanego urzędu certyfikacji, a ponieważ aplikacja się nie jest zaufany urząd, nie powinien on mieć dostęp do własnej escrowed materiału.</span><span class="sxs-lookup"><span data-stu-id="ce176-217">This is consistent with the design theory of the escrow mechanism: it's intended to make the key material accessible to a trusted authority, and since the application is itself not a trusted authority, it shouldn't have access to its own escrowed material.</span></span>

<span data-ttu-id="ce176-218">Następujący przykładowy kod przedstawia tworzenie i rejestrowanie `IKeyEscrowSink` gdzie klucze są zdeponowane w taki sposób, aby je odzyskać tylko elementy członkowskie "CONTOSODomain Administratorzy".</span><span class="sxs-lookup"><span data-stu-id="ce176-218">The following sample code demonstrates creating and registering an `IKeyEscrowSink` where keys are escrowed such that only members of "CONTOSODomain Admins" can recover them.</span></span>

> [!NOTE]
> <span data-ttu-id="ce176-219">Aby uruchomić ten przykład, użytkownik musi być na przyłączonych do domeny systemu Windows 8 / maszyny z systemem Windows Server 2012, a kontroler domeny musi być systemu Windows Server 2012 lub nowszym.</span><span class="sxs-lookup"><span data-stu-id="ce176-219">To run this sample, you must be on a domain-joined Windows 8 / Windows Server 2012 machine, and the domain controller must be Windows Server 2012 or later.</span></span>

[!code-csharp[](key-management/samples/key-management-extensibility.cs)]
