---
title: "Rozszerzalność kryptografii Core"
author: rick-anderson
description: "Wyjaśniono IAuthenticatedEncryptor, IAuthenticatedEncryptorDescriptor IAuthenticatedEncryptorDescriptorDeserializer i fabryki najwyższego poziomu."
ms.author: riande
manager: wpickett
ms.date: 8/11/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/extensibility/core-crypto
ms.openlocfilehash: b82c30fe40c4badc74645dafa9f0d13f6ffae031
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/19/2018
---
# <a name="core-cryptography-extensibility"></a><span data-ttu-id="ae6ef-103">Rozszerzalność kryptografii Core</span><span class="sxs-lookup"><span data-stu-id="ae6ef-103">Core cryptography extensibility</span></span>

<a name="data-protection-extensibility-core-crypto"></a>

>[!WARNING]
> <span data-ttu-id="ae6ef-104">Typy implementujące żadnego z następujących interfejsów powinny być bezpieczne wątkowo dla wielu wywołań.</span><span class="sxs-lookup"><span data-stu-id="ae6ef-104">Types that implement any of the following interfaces should be thread-safe for multiple callers.</span></span>

<a name="data-protection-extensibility-core-crypto-iauthenticatedencryptor"></a>

## <a name="iauthenticatedencryptor"></a><span data-ttu-id="ae6ef-105">IAuthenticatedEncryptor</span><span class="sxs-lookup"><span data-stu-id="ae6ef-105">IAuthenticatedEncryptor</span></span>

<span data-ttu-id="ae6ef-106">**IAuthenticatedEncryptor** interfejs jest podstawowym blokiem konstrukcyjnym podsystem kryptograficzny.</span><span class="sxs-lookup"><span data-stu-id="ae6ef-106">The **IAuthenticatedEncryptor** interface is the basic building block of the cryptographic subsystem.</span></span> <span data-ttu-id="ae6ef-107">Zwykle jest jednym IAuthenticatedEncryptor na klucz, a wystąpienia IAuthenticatedEncryptor opakowuje wszystkich materiał kluczy kryptograficznych i algorytmicznego informacje niezbędne do wykonywania operacji kryptograficznych.</span><span class="sxs-lookup"><span data-stu-id="ae6ef-107">There is generally one IAuthenticatedEncryptor per key, and the IAuthenticatedEncryptor instance wraps all cryptographic key material and algorithmic information necessary to perform cryptographic operations.</span></span>

<span data-ttu-id="ae6ef-108">Zgodnie z sugestią, jego nazwa, typ jest odpowiedzialny za zapewnienie uwierzytelnionego usług szyfrowania i odszyfrowywania.</span><span class="sxs-lookup"><span data-stu-id="ae6ef-108">As its name suggests, the type is responsible for providing authenticated encryption and decryption services.</span></span> <span data-ttu-id="ae6ef-109">Udostępnia ona następujące dwa interfejsy API.</span><span class="sxs-lookup"><span data-stu-id="ae6ef-109">It exposes the following two APIs.</span></span>

* <span data-ttu-id="ae6ef-110">Odszyfrowywanie (ArraySegment<byte> tekstu szyfrowanego, ArraySegment<byte> additionalAuthenticatedData): byte]</span><span class="sxs-lookup"><span data-stu-id="ae6ef-110">Decrypt(ArraySegment<byte> ciphertext, ArraySegment<byte> additionalAuthenticatedData) : byte[]</span></span>

* <span data-ttu-id="ae6ef-111">Szyfrowanie (ArraySegment<byte> w postaci zwykłego tekstu, ArraySegment<byte> additionalAuthenticatedData): byte]</span><span class="sxs-lookup"><span data-stu-id="ae6ef-111">Encrypt(ArraySegment<byte> plaintext, ArraySegment<byte> additionalAuthenticatedData) : byte[]</span></span>

<span data-ttu-id="ae6ef-112">Metoda szyfrowania zwraca obiektu blob zawierającego enciphered zwykłego tekstu i tag uwierzytelniania.</span><span class="sxs-lookup"><span data-stu-id="ae6ef-112">The Encrypt method returns a blob that includes the enciphered plaintext and an authentication tag.</span></span> <span data-ttu-id="ae6ef-113">Tag uwierzytelniania musi obejmować dodatkowe uwierzytelnionych danych (AAD), chociaż sama usługi AAD nie muszą być możliwe do odzyskania z ostatnim ładunku.</span><span class="sxs-lookup"><span data-stu-id="ae6ef-113">The authentication tag must encompass the additional authenticated data (AAD), though the AAD itself need not be recoverable from the final payload.</span></span> <span data-ttu-id="ae6ef-114">Metoda odszyfrowywania weryfikuje tag uwierzytelniania i zwraca deciphered ładunku.</span><span class="sxs-lookup"><span data-stu-id="ae6ef-114">The Decrypt method validates the authentication tag and returns the deciphered payload.</span></span> <span data-ttu-id="ae6ef-115">Wszystkie błędy (z wyjątkiem argumentnullexception — i podobne) powinny być homogenizowane do cryptographicexception —.</span><span class="sxs-lookup"><span data-stu-id="ae6ef-115">All failures (except ArgumentNullException and similar) should be homogenized to CryptographicException.</span></span>

> [!NOTE]
> <span data-ttu-id="ae6ef-116">Wystąpienie IAuthenticatedEncryptor sam faktycznie nie musi zawierać materiału klucza.</span><span class="sxs-lookup"><span data-stu-id="ae6ef-116">The IAuthenticatedEncryptor instance itself doesn't actually need to contain the key material.</span></span> <span data-ttu-id="ae6ef-117">Na przykład wdrożenia można delegować do modułu HSM dla wszystkich operacji.</span><span class="sxs-lookup"><span data-stu-id="ae6ef-117">For example, the implementation could delegate to an HSM for all operations.</span></span>

<a name="data-protection-extensibility-core-crypto-iauthenticatedencryptorfactory"></a>
<a name="data-protection-extensibility-core-crypto-iauthenticatedencryptordescriptor"></a>

## <a name="how-to-create-an-iauthenticatedencryptor"></a><span data-ttu-id="ae6ef-118">Jak utworzyć IAuthenticatedEncryptor</span><span class="sxs-lookup"><span data-stu-id="ae6ef-118">How to create an IAuthenticatedEncryptor</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="ae6ef-119">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="ae6ef-119">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="ae6ef-120">**IAuthenticatedEncryptorFactory** interfejsu reprezentuje typ, który umożliwia tworzenie [IAuthenticatedEncryptor](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptor) wystąpienia.</span><span class="sxs-lookup"><span data-stu-id="ae6ef-120">The **IAuthenticatedEncryptorFactory** interface represents a type that knows how to create an [IAuthenticatedEncryptor](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptor) instance.</span></span> <span data-ttu-id="ae6ef-121">Poniżej przedstawiono jej interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="ae6ef-121">Its API is as follows.</span></span>

* <span data-ttu-id="ae6ef-122">CreateEncryptorInstance (klucz IKey): IAuthenticatedEncryptor</span><span class="sxs-lookup"><span data-stu-id="ae6ef-122">CreateEncryptorInstance(IKey key) : IAuthenticatedEncryptor</span></span>

<span data-ttu-id="ae6ef-123">Dla każdego danego wystąpienia IKey żadnych uwierzytelnionego encryptors tworzone przez metodę jego CreateEncryptorInstance należy traktować jako równoważne, podobnie jak w poniżej przykładowy kod.</span><span class="sxs-lookup"><span data-stu-id="ae6ef-123">For any given IKey instance, any authenticated encryptors created by its CreateEncryptorInstance method should be considered equivalent, as in the below code sample.</span></span>

```csharp
// we have an IAuthenticatedEncryptorFactory instance and an IKey instance
IAuthenticatedEncryptorFactory factory = ...;
IKey key = ...;

// get an encryptor instance and perform an authenticated encryption operation
ArraySegment<byte> plaintext = new ArraySegment<byte>(Encoding.UTF8.GetBytes("plaintext"));
ArraySegment<byte> aad = new ArraySegment<byte>(Encoding.UTF8.GetBytes("AAD"));
var encryptor1 = factory.CreateEncryptorInstance(key);
byte[] ciphertext = encryptor1.Encrypt(plaintext, aad);

// get another encryptor instance and perform an authenticated decryption operation
var encryptor2 = factory.CreateEncryptorInstance(key);
byte[] roundTripped = encryptor2.Decrypt(new ArraySegment<byte>(ciphertext), aad);


// the 'roundTripped' and 'plaintext' buffers should be equivalent
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="ae6ef-124">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="ae6ef-124">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="ae6ef-125">**IAuthenticatedEncryptorDescriptor** interfejsu reprezentuje typ, który umożliwia tworzenie [IAuthenticatedEncryptor](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptor) wystąpienia.</span><span class="sxs-lookup"><span data-stu-id="ae6ef-125">The **IAuthenticatedEncryptorDescriptor** interface represents a type that knows how to create an [IAuthenticatedEncryptor](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptor) instance.</span></span> <span data-ttu-id="ae6ef-126">Poniżej przedstawiono jej interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="ae6ef-126">Its API is as follows.</span></span>

* <span data-ttu-id="ae6ef-127">CreateEncryptorInstance(): IAuthenticatedEncryptor</span><span class="sxs-lookup"><span data-stu-id="ae6ef-127">CreateEncryptorInstance() : IAuthenticatedEncryptor</span></span>

* <span data-ttu-id="ae6ef-128">ExportToXml(): XmlSerializedDescriptorInfo</span><span class="sxs-lookup"><span data-stu-id="ae6ef-128">ExportToXml() : XmlSerializedDescriptorInfo</span></span>

<span data-ttu-id="ae6ef-129">Podobnie jak IAuthenticatedEncryptor wystąpienie IAuthenticatedEncryptorDescriptor zakłada się, że zawijać jednego określonego klucza.</span><span class="sxs-lookup"><span data-stu-id="ae6ef-129">Like IAuthenticatedEncryptor, an instance of IAuthenticatedEncryptorDescriptor is assumed to wrap one specific key.</span></span> <span data-ttu-id="ae6ef-130">Oznacza to, że dla dowolnego danego wystąpienia IAuthenticatedEncryptorDescriptor żadnych uwierzytelnionego encryptors tworzone przez metodę jego CreateEncryptorInstance należy traktować jako równoważne, podobnie jak w poniżej przykładowy kod.</span><span class="sxs-lookup"><span data-stu-id="ae6ef-130">This means that for any given IAuthenticatedEncryptorDescriptor instance, any authenticated encryptors created by its CreateEncryptorInstance method should be considered equivalent, as in the below code sample.</span></span>

```csharp
// we have an IAuthenticatedEncryptorDescriptor instance
IAuthenticatedEncryptorDescriptor descriptor = ...;

// get an encryptor instance and perform an authenticated encryption operation
ArraySegment<byte> plaintext = new ArraySegment<byte>(Encoding.UTF8.GetBytes("plaintext"));
ArraySegment<byte> aad = new ArraySegment<byte>(Encoding.UTF8.GetBytes("AAD"));
var encryptor1 = descriptor.CreateEncryptorInstance();
byte[] ciphertext = encryptor1.Encrypt(plaintext, aad);

// get another encryptor instance and perform an authenticated decryption operation
var encryptor2 = descriptor.CreateEncryptorInstance();
byte[] roundTripped = encryptor2.Decrypt(new ArraySegment<byte>(ciphertext), aad);


// the 'roundTripped' and 'plaintext' buffers should be equivalent
```

---

<a name="data-protection-extensibility-core-crypto-iauthenticatedencryptordescriptor"></a>

## <a name="iauthenticatedencryptordescriptor-aspnet-core-2x-only"></a><span data-ttu-id="ae6ef-131">IAuthenticatedEncryptorDescriptor (tylko 2.x platformy ASP.NET Core)</span><span class="sxs-lookup"><span data-stu-id="ae6ef-131">IAuthenticatedEncryptorDescriptor (ASP.NET Core 2.x only)</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="ae6ef-132">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="ae6ef-132">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="ae6ef-133">**IAuthenticatedEncryptorDescriptor** interfejsu reprezentuje typ, który potrafi do samej siebie wyeksportować do pliku XML.</span><span class="sxs-lookup"><span data-stu-id="ae6ef-133">The **IAuthenticatedEncryptorDescriptor** interface represents a type that knows how to export itself to XML.</span></span> <span data-ttu-id="ae6ef-134">Poniżej przedstawiono jej interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="ae6ef-134">Its API is as follows.</span></span>

* <span data-ttu-id="ae6ef-135">ExportToXml(): XmlSerializedDescriptorInfo</span><span class="sxs-lookup"><span data-stu-id="ae6ef-135">ExportToXml() : XmlSerializedDescriptorInfo</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="ae6ef-136">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="ae6ef-136">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

---

## <a name="xml-serialization"></a><span data-ttu-id="ae6ef-137">Serializacji XML</span><span class="sxs-lookup"><span data-stu-id="ae6ef-137">XML Serialization</span></span>

<span data-ttu-id="ae6ef-138">Główną różnicą między IAuthenticatedEncryptor i IAuthenticatedEncryptorDescriptor jest, że deskryptor wie, jak utworzyć modułu szyfrującego i dostarczyć nieprawidłowe argumenty.</span><span class="sxs-lookup"><span data-stu-id="ae6ef-138">The primary difference between IAuthenticatedEncryptor and IAuthenticatedEncryptorDescriptor is that the descriptor knows how to create the encryptor and supply it with valid arguments.</span></span> <span data-ttu-id="ae6ef-139">Należy wziąć pod uwagę IAuthenticatedEncryptor, którego implementacja polega na SymmetricAlgorithm i KeyedHashAlgorithm.</span><span class="sxs-lookup"><span data-stu-id="ae6ef-139">Consider an IAuthenticatedEncryptor whose implementation relies on SymmetricAlgorithm and KeyedHashAlgorithm.</span></span> <span data-ttu-id="ae6ef-140">Zadanie modułu szyfrującego ma korzystać z tych typów, ale go nie musi wiedzieć, skąd pochodzą te typy, tak naprawdę nie można zapisać się odpowiednie opis sposobu ponownego tworzenia się, jeśli aplikacja zostanie uruchomiony ponownie.</span><span class="sxs-lookup"><span data-stu-id="ae6ef-140">The encryptor's job is to consume these types, but it doesn't necessarily know where these types came from, so it can't really write out a proper description of how to recreate itself if the application restarts.</span></span> <span data-ttu-id="ae6ef-141">Deskryptor działa jako wyższego poziomu na podstawie to.</span><span class="sxs-lookup"><span data-stu-id="ae6ef-141">The descriptor acts as a higher level on top of this.</span></span> <span data-ttu-id="ae6ef-142">Ponieważ deskryptor potrafi można utworzyć wystąpienia modułu szyfrującego (np. wie jak utworzyć wymagane algorytmy), może wykonać serializację tę wiedzę w postaci XML, tak aby modułu szyfrującego wystąpienie można utworzyć ponownie po zresetowaniu aplikacji.</span><span class="sxs-lookup"><span data-stu-id="ae6ef-142">Since the descriptor knows how to create the encryptor instance (e.g., it knows how to create the required algorithms), it can serialize that knowledge in XML form so that the encryptor instance can be recreated after an application reset.</span></span>

<a name="data-protection-extensibility-core-crypto-exporttoxml"></a>

<span data-ttu-id="ae6ef-143">Deskryptor może być Zserializowany za pośrednictwem jego ExportToXml procedury.</span><span class="sxs-lookup"><span data-stu-id="ae6ef-143">The descriptor can be serialized via its ExportToXml routine.</span></span> <span data-ttu-id="ae6ef-144">Ta procedura zwraca XmlSerializedDescriptorInfo, który zawiera dwie właściwości: reprezentacja klasy XElement deskryptor i typ, który reprezentuje [IAuthenticatedEncryptorDescriptorDeserializer](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptordescriptordeserializer) może to używane do przywracania aktywności ten deskryptor podane odpowiedniej klasy XElement.</span><span class="sxs-lookup"><span data-stu-id="ae6ef-144">This routine returns an XmlSerializedDescriptorInfo which contains two properties: the XElement representation of the descriptor and the Type which represents an [IAuthenticatedEncryptorDescriptorDeserializer](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptordescriptordeserializer) which can be used to resurrect this descriptor given the corresponding XElement.</span></span>

<span data-ttu-id="ae6ef-145">Deskryptor serializacji mogą zawierać poufne informacje, takie jak materiał kluczy kryptograficznych.</span><span class="sxs-lookup"><span data-stu-id="ae6ef-145">The serialized descriptor may contain sensitive information such as cryptographic key material.</span></span> <span data-ttu-id="ae6ef-146">System ochrony danych ma wbudowaną obsługę szyfrowania informacji przed zostało utrwalone w magazynie.</span><span class="sxs-lookup"><span data-stu-id="ae6ef-146">The data protection system has built-in support for encrypting information before it's persisted to storage.</span></span> <span data-ttu-id="ae6ef-147">Aby móc korzystać z tego, deskryptora należy zaznaczyć element, który zawiera poufne informacje o nazwie atrybutu "requiresEncryption" (xmlns "http://schemas.asp.net/2015/03/dataProtection"), wartość "true".</span><span class="sxs-lookup"><span data-stu-id="ae6ef-147">To take advantage of this, the descriptor should mark the element which contains sensitive information with the attribute name "requiresEncryption" (xmlns "http://schemas.asp.net/2015/03/dataProtection"), value "true".</span></span>

>[!TIP]
> <span data-ttu-id="ae6ef-148">Brak pomocnik interfejsu API dla ustawienie tego atrybutu.</span><span class="sxs-lookup"><span data-stu-id="ae6ef-148">There's a helper API for setting this attribute.</span></span> <span data-ttu-id="ae6ef-149">Wywołanie metody rozszerzenia, które XElement.MarkAsRequiresEncryption() znajduje się w przestrzeni nazw Microsoft.AspNetCore.DataProtection.AuthenticatedEncryption.ConfigurationModel.</span><span class="sxs-lookup"><span data-stu-id="ae6ef-149">Call the extension method XElement.MarkAsRequiresEncryption() located in namespace Microsoft.AspNetCore.DataProtection.AuthenticatedEncryption.ConfigurationModel.</span></span>

<span data-ttu-id="ae6ef-150">Można także przypadki, w którym deskryptora serializacji nie zawiera poufne informacje.</span><span class="sxs-lookup"><span data-stu-id="ae6ef-150">There can also be cases where the serialized descriptor doesn't contain sensitive information.</span></span> <span data-ttu-id="ae6ef-151">Rozważmy przykład ponownie klucza kryptograficznego przechowywane w module HSM.</span><span class="sxs-lookup"><span data-stu-id="ae6ef-151">Consider again the case of a cryptographic key stored in an HSM.</span></span> <span data-ttu-id="ae6ef-152">Deskryptor nie można zapisać materiału klucza podczas serializowania się, ponieważ moduł HSM nie powoduje to udostępnienie materiału w formie zwykłego tekstu.</span><span class="sxs-lookup"><span data-stu-id="ae6ef-152">The descriptor cannot write out the key material when serializing itself since the HSM will not expose the material in plaintext form.</span></span> <span data-ttu-id="ae6ef-153">Zamiast tego deskryptora może zapisać opakowana klucz wersji klucza (Jeśli moduł HSM umożliwia eksportu w ten sposób) lub własne HSM Unikatowy identyfikator dla klucza.</span><span class="sxs-lookup"><span data-stu-id="ae6ef-153">Instead, the descriptor might write out the key-wrapped version of the key (if the HSM allows export in this fashion) or the HSM's own unique identifier for the key.</span></span>

<a name="data-protection-extensibility-core-crypto-iauthenticatedencryptordescriptordeserializer"></a>

## <a name="iauthenticatedencryptordescriptordeserializer"></a><span data-ttu-id="ae6ef-154">IAuthenticatedEncryptorDescriptorDeserializer</span><span class="sxs-lookup"><span data-stu-id="ae6ef-154">IAuthenticatedEncryptorDescriptorDeserializer</span></span>

<span data-ttu-id="ae6ef-155">**IAuthenticatedEncryptorDescriptorDeserializer** interfejsu reprezentuje typ, który potrafi zdeserializować wystąpienia IAuthenticatedEncryptorDescriptor z XElement.</span><span class="sxs-lookup"><span data-stu-id="ae6ef-155">The **IAuthenticatedEncryptorDescriptorDeserializer** interface represents a type that knows how to deserialize an IAuthenticatedEncryptorDescriptor instance from an XElement.</span></span> <span data-ttu-id="ae6ef-156">Udostępnia ona jedną metodę:</span><span class="sxs-lookup"><span data-stu-id="ae6ef-156">It exposes a single method:</span></span>

* <span data-ttu-id="ae6ef-157">ImportFromXml (element klasy XElement): IAuthenticatedEncryptorDescriptor</span><span class="sxs-lookup"><span data-stu-id="ae6ef-157">ImportFromXml(XElement element) : IAuthenticatedEncryptorDescriptor</span></span>

<span data-ttu-id="ae6ef-158">Metoda ImportFromXml przyjmuje klasy XElement, który został zwrócony przez [IAuthenticatedEncryptorDescriptor.ExportToXml](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-exporttoxml) i tworzy równoważny IAuthenticatedEncryptorDescriptor oryginalnego.</span><span class="sxs-lookup"><span data-stu-id="ae6ef-158">The ImportFromXml method takes the XElement that was returned by [IAuthenticatedEncryptorDescriptor.ExportToXml](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-exporttoxml) and creates an equivalent of the original IAuthenticatedEncryptorDescriptor.</span></span>

<span data-ttu-id="ae6ef-159">Typy implementujące IAuthenticatedEncryptorDescriptorDeserializer powinny mieć jedno z następujących dwóch konstruktorów publicznych:</span><span class="sxs-lookup"><span data-stu-id="ae6ef-159">Types which implement IAuthenticatedEncryptorDescriptorDeserializer should have one of the following two public constructors:</span></span>

* <span data-ttu-id="ae6ef-160">.ctor(IServiceProvider)</span><span class="sxs-lookup"><span data-stu-id="ae6ef-160">.ctor(IServiceProvider)</span></span>

* <span data-ttu-id="ae6ef-161">.ctor()</span><span class="sxs-lookup"><span data-stu-id="ae6ef-161">.ctor()</span></span>

> [!NOTE]
> <span data-ttu-id="ae6ef-162">IServiceProvider przekazany do konstruktora może mieć wartości null.</span><span class="sxs-lookup"><span data-stu-id="ae6ef-162">The IServiceProvider passed to the constructor may be null.</span></span>

## <a name="the-top-level-factory"></a><span data-ttu-id="ae6ef-163">Fabryka najwyższego poziomu</span><span class="sxs-lookup"><span data-stu-id="ae6ef-163">The top-level factory</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="ae6ef-164">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="ae6ef-164">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="ae6ef-165">**AlgorithmConfiguration** klasa reprezentuje typ, który umożliwia tworzenie [IAuthenticatedEncryptorDescriptor](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptordescriptor) wystąpień.</span><span class="sxs-lookup"><span data-stu-id="ae6ef-165">The **AlgorithmConfiguration** class represents a type which knows how to create [IAuthenticatedEncryptorDescriptor](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptordescriptor) instances.</span></span> <span data-ttu-id="ae6ef-166">Prezentuje ona jednego interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="ae6ef-166">It exposes a single API.</span></span>

* <span data-ttu-id="ae6ef-167">CreateNewDescriptor(): IAuthenticatedEncryptorDescriptor</span><span class="sxs-lookup"><span data-stu-id="ae6ef-167">CreateNewDescriptor() : IAuthenticatedEncryptorDescriptor</span></span>

<span data-ttu-id="ae6ef-168">AlgorithmConfiguration można traktować jako fabryka najwyższego poziomu.</span><span class="sxs-lookup"><span data-stu-id="ae6ef-168">Think of AlgorithmConfiguration as the top-level factory.</span></span> <span data-ttu-id="ae6ef-169">Konfiguracja służy jako szablon.</span><span class="sxs-lookup"><span data-stu-id="ae6ef-169">The configuration serves as a template.</span></span> <span data-ttu-id="ae6ef-170">Jest zawijany algorytmicznego informacji (np. Ta konfiguracja daje deskryptory z kluczem głównym AES-128-GCM), ale nie jest jeszcze skojarzona z określonym kluczem.</span><span class="sxs-lookup"><span data-stu-id="ae6ef-170">It wraps algorithmic information (e.g., this configuration produces descriptors with an AES-128-GCM master key), but it is not yet associated with a specific key.</span></span>

<span data-ttu-id="ae6ef-171">Gdy CreateNewDescriptor jest wywoływana, świeże materiału klucza jest przeznaczone wyłącznie dla tego wywołania i jest generowany nowy IAuthenticatedEncryptorDescriptor który koduje tego materiału klucza i algorytmicznego informacje wymagane użycie materiałów.</span><span class="sxs-lookup"><span data-stu-id="ae6ef-171">When CreateNewDescriptor is called, fresh key material is created solely for this call, and a new IAuthenticatedEncryptorDescriptor is produced which wraps this key material and the algorithmic information required to consume the material.</span></span> <span data-ttu-id="ae6ef-172">Materiału klucza można utworzyć w oprogramowaniu (i przechowywane w pamięci), można utworzyć i przechowywane w module HSM i tak dalej.</span><span class="sxs-lookup"><span data-stu-id="ae6ef-172">The key material could be created in software (and held in memory), it could be created and held within an HSM, and so on.</span></span> <span data-ttu-id="ae6ef-173">Kluczowe punkt znajduje się wszelkie dwóch wywołania CreateNewDescriptor powinien nigdy tworzyć wystąpienia IAuthenticatedEncryptorDescriptor równoważne.</span><span class="sxs-lookup"><span data-stu-id="ae6ef-173">The crucial point is that any two calls to CreateNewDescriptor should never create equivalent IAuthenticatedEncryptorDescriptor instances.</span></span>

<span data-ttu-id="ae6ef-174">Typ AlgorithmConfiguration służy jako punkt wejścia dla procedury tworzenia klucza, takich jak [automatyczne klucza stopniowych](../implementation/key-management.md#key-expiration-and-rolling).</span><span class="sxs-lookup"><span data-stu-id="ae6ef-174">The AlgorithmConfiguration type serves as the entry point for key creation routines such as [automatic key rolling](../implementation/key-management.md#key-expiration-and-rolling).</span></span> <span data-ttu-id="ae6ef-175">Aby zmienić wdrożenia dla wszystkich przyszłych kluczy, należy ustawić właściwość AuthenticatedEncryptorConfiguration w KeyManagementOptions.</span><span class="sxs-lookup"><span data-stu-id="ae6ef-175">To change the implementation for all future keys, set the AuthenticatedEncryptorConfiguration property in KeyManagementOptions.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="ae6ef-176">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="ae6ef-176">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="ae6ef-177">**IAuthenticatedEncryptorConfiguration** interfejsu reprezentuje typ, który umożliwia tworzenie [IAuthenticatedEncryptorDescriptor](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptordescriptor) wystąpień.</span><span class="sxs-lookup"><span data-stu-id="ae6ef-177">The **IAuthenticatedEncryptorConfiguration** interface represents a type which knows how to create [IAuthenticatedEncryptorDescriptor](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptordescriptor) instances.</span></span> <span data-ttu-id="ae6ef-178">Prezentuje ona jednego interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="ae6ef-178">It exposes a single API.</span></span>

* <span data-ttu-id="ae6ef-179">CreateNewDescriptor(): IAuthenticatedEncryptorDescriptor</span><span class="sxs-lookup"><span data-stu-id="ae6ef-179">CreateNewDescriptor() : IAuthenticatedEncryptorDescriptor</span></span>

<span data-ttu-id="ae6ef-180">IAuthenticatedEncryptorConfiguration można traktować jako fabryka najwyższego poziomu.</span><span class="sxs-lookup"><span data-stu-id="ae6ef-180">Think of IAuthenticatedEncryptorConfiguration as the top-level factory.</span></span> <span data-ttu-id="ae6ef-181">Konfiguracja służy jako szablon.</span><span class="sxs-lookup"><span data-stu-id="ae6ef-181">The configuration serves as a template.</span></span> <span data-ttu-id="ae6ef-182">Jest zawijany algorytmicznego informacji (np. Ta konfiguracja daje deskryptory z kluczem głównym AES-128-GCM), ale nie jest jeszcze skojarzona z określonym kluczem.</span><span class="sxs-lookup"><span data-stu-id="ae6ef-182">It wraps algorithmic information (e.g., this configuration produces descriptors with an AES-128-GCM master key), but it is not yet associated with a specific key.</span></span>

<span data-ttu-id="ae6ef-183">Gdy CreateNewDescriptor jest wywoływana, świeże materiału klucza jest przeznaczone wyłącznie dla tego wywołania i jest generowany nowy IAuthenticatedEncryptorDescriptor który koduje tego materiału klucza i algorytmicznego informacje wymagane użycie materiałów.</span><span class="sxs-lookup"><span data-stu-id="ae6ef-183">When CreateNewDescriptor is called, fresh key material is created solely for this call, and a new IAuthenticatedEncryptorDescriptor is produced which wraps this key material and the algorithmic information required to consume the material.</span></span> <span data-ttu-id="ae6ef-184">Materiału klucza można utworzyć w oprogramowaniu (i przechowywane w pamięci), można utworzyć i przechowywane w module HSM i tak dalej.</span><span class="sxs-lookup"><span data-stu-id="ae6ef-184">The key material could be created in software (and held in memory), it could be created and held within an HSM, and so on.</span></span> <span data-ttu-id="ae6ef-185">Kluczowe punkt znajduje się wszelkie dwóch wywołania CreateNewDescriptor powinien nigdy tworzyć wystąpienia IAuthenticatedEncryptorDescriptor równoważne.</span><span class="sxs-lookup"><span data-stu-id="ae6ef-185">The crucial point is that any two calls to CreateNewDescriptor should never create equivalent IAuthenticatedEncryptorDescriptor instances.</span></span>

<span data-ttu-id="ae6ef-186">Typ IAuthenticatedEncryptorConfiguration służy jako punkt wejścia dla procedury tworzenia klucza, takich jak [automatyczne klucza stopniowych](../implementation/key-management.md#key-expiration-and-rolling).</span><span class="sxs-lookup"><span data-stu-id="ae6ef-186">The IAuthenticatedEncryptorConfiguration type serves as the entry point for key creation routines such as [automatic key rolling](../implementation/key-management.md#key-expiration-and-rolling).</span></span> <span data-ttu-id="ae6ef-187">Aby zmienić wdrożenia dla wszystkich przyszłych kluczy, należy zarejestrować pojedynczą IAuthenticatedEncryptorConfiguration w kontenerze usług.</span><span class="sxs-lookup"><span data-stu-id="ae6ef-187">To change the implementation for all future keys, register a singleton IAuthenticatedEncryptorConfiguration in the service container.</span></span>

---
