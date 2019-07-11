---
title: Rozszerzalność kryptografii Core w programie ASP.NET Core
author: rick-anderson
description: Więcej informacji na temat IAuthenticatedEncryptor, IAuthenticatedEncryptorDescriptor, IAuthenticatedEncryptorDescriptorDeserializer i fabryki najwyższego poziomu.
ms.author: riande
ms.date: 08/11/2017
uid: security/data-protection/extensibility/core-crypto
ms.openlocfilehash: a5f651e3313cc579b995b45905826a5bffcc241c
ms.sourcegitcommit: 8516b586541e6ba402e57228e356639b85dfb2b9
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/11/2019
ms.locfileid: "67814704"
---
# <a name="core-cryptography-extensibility-in-aspnet-core"></a><span data-ttu-id="080a5-103">Rozszerzalność kryptografii Core w programie ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="080a5-103">Core cryptography extensibility in ASP.NET Core</span></span>

<a name="data-protection-extensibility-core-crypto"></a>

>[!WARNING]
> <span data-ttu-id="080a5-104">Typy, które implementują żadnego z następujących interfejsów powinny być metodą o bezpiecznych wątkach dla wielu obiektów wywołujących.</span><span class="sxs-lookup"><span data-stu-id="080a5-104">Types that implement any of the following interfaces should be thread-safe for multiple callers.</span></span>

<a name="data-protection-extensibility-core-crypto-iauthenticatedencryptor"></a>

## <a name="iauthenticatedencryptor"></a><span data-ttu-id="080a5-105">IAuthenticatedEncryptor</span><span class="sxs-lookup"><span data-stu-id="080a5-105">IAuthenticatedEncryptor</span></span>

<span data-ttu-id="080a5-106">**IAuthenticatedEncryptor** interfejs jest podstawowym budulcem podsystem kryptograficzny.</span><span class="sxs-lookup"><span data-stu-id="080a5-106">The **IAuthenticatedEncryptor** interface is the basic building block of the cryptographic subsystem.</span></span> <span data-ttu-id="080a5-107">Ogólnie jest jeden IAuthenticatedEncryptor na klucz, a wystąpienie IAuthenticatedEncryptor opakowuje wszystkich materiału klucza kryptograficznego i informacje algorytmicznego, które są niezbędne do wykonywania operacji kryptograficznych.</span><span class="sxs-lookup"><span data-stu-id="080a5-107">There's generally one IAuthenticatedEncryptor per key, and the IAuthenticatedEncryptor instance wraps all cryptographic key material and algorithmic information necessary to perform cryptographic operations.</span></span>

<span data-ttu-id="080a5-108">Jak sugeruje nazwa, typ jest odpowiedzialne za świadczenie usług uwierzytelnionego szyfrowania i odszyfrowywania.</span><span class="sxs-lookup"><span data-stu-id="080a5-108">As its name suggests, the type is responsible for providing authenticated encryption and decryption services.</span></span> <span data-ttu-id="080a5-109">Udostępnia ona następujące dwa interfejsy API.</span><span class="sxs-lookup"><span data-stu-id="080a5-109">It exposes the following two APIs.</span></span>

* `Decrypt(ArraySegment<byte> ciphertext, ArraySegment<byte> additionalAuthenticatedData) : byte[]`

* `Encrypt(ArraySegment<byte> plaintext, ArraySegment<byte> additionalAuthenticatedData) : byte[]`

<span data-ttu-id="080a5-110">Metoda szyfrowania zwraca obiekt blob zawiera enciphered zwykłego tekstu i tag uwierzytelniania.</span><span class="sxs-lookup"><span data-stu-id="080a5-110">The Encrypt method returns a blob that includes the enciphered plaintext and an authentication tag.</span></span> <span data-ttu-id="080a5-111">Tag uwierzytelniania musi obejmować dodatkowe uwierzytelnionych danych (AAD), chociaż sama usługi AAD nie muszą być możliwe do odzyskania z ostatnim ładunku.</span><span class="sxs-lookup"><span data-stu-id="080a5-111">The authentication tag must encompass the additional authenticated data (AAD), though the AAD itself need not be recoverable from the final payload.</span></span> <span data-ttu-id="080a5-112">Metoda odszyfrowywania weryfikuje tag uwierzytelniania i zwraca deciphered ładunku.</span><span class="sxs-lookup"><span data-stu-id="080a5-112">The Decrypt method validates the authentication tag and returns the deciphered payload.</span></span> <span data-ttu-id="080a5-113">Wszystkie błędy (z wyjątkiem ArgumentNullException i podobne) powinny być homogenizowane do cryptographicexception —.</span><span class="sxs-lookup"><span data-stu-id="080a5-113">All failures (except ArgumentNullException and similar) should be homogenized to CryptographicException.</span></span>

> [!NOTE]
> <span data-ttu-id="080a5-114">Samego wystąpienia IAuthenticatedEncryptor faktycznie nie musi zawierać materiał klucza.</span><span class="sxs-lookup"><span data-stu-id="080a5-114">The IAuthenticatedEncryptor instance itself doesn't actually need to contain the key material.</span></span> <span data-ttu-id="080a5-115">Na przykład wdrożenia można delegować do modułu HSM dla wszystkich operacji.</span><span class="sxs-lookup"><span data-stu-id="080a5-115">For example, the implementation could delegate to an HSM for all operations.</span></span>

<a name="data-protection-extensibility-core-crypto-iauthenticatedencryptorfactory"></a>
<a name="data-protection-extensibility-core-crypto-iauthenticatedencryptordescriptor"></a>

## <a name="how-to-create-an-iauthenticatedencryptor"></a><span data-ttu-id="080a5-116">Jak utworzyć IAuthenticatedEncryptor</span><span class="sxs-lookup"><span data-stu-id="080a5-116">How to create an IAuthenticatedEncryptor</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="080a5-117">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="080a5-117">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="080a5-118">**IAuthenticatedEncryptorFactory** interfejs reprezentuje typ, który wie, jak utworzyć [IAuthenticatedEncryptor](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptor) wystąpienia.</span><span class="sxs-lookup"><span data-stu-id="080a5-118">The **IAuthenticatedEncryptorFactory** interface represents a type that knows how to create an [IAuthenticatedEncryptor](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptor) instance.</span></span> <span data-ttu-id="080a5-119">Jego interfejs API jest w następujący sposób.</span><span class="sxs-lookup"><span data-stu-id="080a5-119">Its API is as follows.</span></span>

* <span data-ttu-id="080a5-120">CreateEncryptorInstance (klucz IKey): IAuthenticatedEncryptor</span><span class="sxs-lookup"><span data-stu-id="080a5-120">CreateEncryptorInstance(IKey key) : IAuthenticatedEncryptor</span></span>

<span data-ttu-id="080a5-121">W dowolnym danym wystąpieniu klucz Instrumentacji żadnych uwierzytelnionego encryptors tworzone przez metodę jego CreateEncryptorInstance rozważa równoważne, podobnie jak w poniżej przykładowego kodu.</span><span class="sxs-lookup"><span data-stu-id="080a5-121">For any given IKey instance, any authenticated encryptors created by its CreateEncryptorInstance method should be considered equivalent, as in the below code sample.</span></span>

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

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="080a5-122">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="080a5-122">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="080a5-123">**IAuthenticatedEncryptorDescriptor** interfejs reprezentuje typ, który wie, jak utworzyć [IAuthenticatedEncryptor](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptor) wystąpienia.</span><span class="sxs-lookup"><span data-stu-id="080a5-123">The **IAuthenticatedEncryptorDescriptor** interface represents a type that knows how to create an [IAuthenticatedEncryptor](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptor) instance.</span></span> <span data-ttu-id="080a5-124">Jego interfejs API jest w następujący sposób.</span><span class="sxs-lookup"><span data-stu-id="080a5-124">Its API is as follows.</span></span>

* <span data-ttu-id="080a5-125">CreateEncryptorInstance(): IAuthenticatedEncryptor</span><span class="sxs-lookup"><span data-stu-id="080a5-125">CreateEncryptorInstance() : IAuthenticatedEncryptor</span></span>

* <span data-ttu-id="080a5-126">ExportToXml(): XmlSerializedDescriptorInfo</span><span class="sxs-lookup"><span data-stu-id="080a5-126">ExportToXml() : XmlSerializedDescriptorInfo</span></span>

<span data-ttu-id="080a5-127">Podobnie jak IAuthenticatedEncryptor wystąpienie IAuthenticatedEncryptorDescriptor zakłada, że do opakowania jednego określonego klucza.</span><span class="sxs-lookup"><span data-stu-id="080a5-127">Like IAuthenticatedEncryptor, an instance of IAuthenticatedEncryptorDescriptor is assumed to wrap one specific key.</span></span> <span data-ttu-id="080a5-128">Oznacza to, że w dowolnym danym wystąpieniu IAuthenticatedEncryptorDescriptor wszelkie uwierzytelnionego encryptors tworzone przez metodę jego CreateEncryptorInstance należy traktować równoważne, podobnie jak w poniżej przykładowego kodu.</span><span class="sxs-lookup"><span data-stu-id="080a5-128">This means that for any given IAuthenticatedEncryptorDescriptor instance, any authenticated encryptors created by its CreateEncryptorInstance method should be considered equivalent, as in the below code sample.</span></span>

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

## <a name="iauthenticatedencryptordescriptor-aspnet-core-2x-only"></a><span data-ttu-id="080a5-129">IAuthenticatedEncryptorDescriptor (ASP.NET Core 2.x tylko)</span><span class="sxs-lookup"><span data-stu-id="080a5-129">IAuthenticatedEncryptorDescriptor (ASP.NET Core 2.x only)</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="080a5-130">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="080a5-130">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="080a5-131">**IAuthenticatedEncryptorDescriptor** interfejs reprezentuje typ, który wie, jak sama wyeksportować do pliku XML.</span><span class="sxs-lookup"><span data-stu-id="080a5-131">The **IAuthenticatedEncryptorDescriptor** interface represents a type that knows how to export itself to XML.</span></span> <span data-ttu-id="080a5-132">Jego interfejs API jest w następujący sposób.</span><span class="sxs-lookup"><span data-stu-id="080a5-132">Its API is as follows.</span></span>

* <span data-ttu-id="080a5-133">ExportToXml(): XmlSerializedDescriptorInfo</span><span class="sxs-lookup"><span data-stu-id="080a5-133">ExportToXml() : XmlSerializedDescriptorInfo</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="080a5-134">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="080a5-134">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

---

## <a name="xml-serialization"></a><span data-ttu-id="080a5-135">Serializacji XML</span><span class="sxs-lookup"><span data-stu-id="080a5-135">XML Serialization</span></span>

<span data-ttu-id="080a5-136">Główną różnicą między IAuthenticatedEncryptor i IAuthenticatedEncryptorDescriptor to, że deskryptora wie, jak utworzyć modułu szyfrującego i dostarczyć prawidłowe argumenty.</span><span class="sxs-lookup"><span data-stu-id="080a5-136">The primary difference between IAuthenticatedEncryptor and IAuthenticatedEncryptorDescriptor is that the descriptor knows how to create the encryptor and supply it with valid arguments.</span></span> <span data-ttu-id="080a5-137">Należy wziąć pod uwagę IAuthenticatedEncryptor, którego implementacja jest uzależniona od SymmetricAlgorithm i KeyedHashAlgorithm.</span><span class="sxs-lookup"><span data-stu-id="080a5-137">Consider an IAuthenticatedEncryptor whose implementation relies on SymmetricAlgorithm and KeyedHashAlgorithm.</span></span> <span data-ttu-id="080a5-138">Zadanie modułu szyfrującego jest korzystanie z tych typów, ale go nie zawsze wie, skąd pochodzą te typy, tak naprawdę nie mogą zapisywać się właściwe opis jak odtworzyć sam w przypadku ponownego uruchomienia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="080a5-138">The encryptor's job is to consume these types, but it doesn't necessarily know where these types came from, so it can't really write out a proper description of how to recreate itself if the application restarts.</span></span> <span data-ttu-id="080a5-139">Deskryptor działa jako na podstawie tego wyższego poziomu.</span><span class="sxs-lookup"><span data-stu-id="080a5-139">The descriptor acts as a higher level on top of this.</span></span> <span data-ttu-id="080a5-140">Ponieważ deskryptor wie, jak utworzyć wystąpienie modułu szyfrującego (np. wie, jak utworzyć wymagane algorytmy), może wykonać serializację tę wiedzę w postaci XML, dzięki czemu można odtworzyć wystąpienia modułu szyfrującego po zresetowaniu aplikacji.</span><span class="sxs-lookup"><span data-stu-id="080a5-140">Since the descriptor knows how to create the encryptor instance (e.g., it knows how to create the required algorithms), it can serialize that knowledge in XML form so that the encryptor instance can be recreated after an application reset.</span></span>

<a name="data-protection-extensibility-core-crypto-exporttoxml"></a>

<span data-ttu-id="080a5-141">Deskryptor może być Zserializowany za pośrednictwem jego ExportToXml procedury.</span><span class="sxs-lookup"><span data-stu-id="080a5-141">The descriptor can be serialized via its ExportToXml routine.</span></span> <span data-ttu-id="080a5-142">Ta procedura zwraca XmlSerializedDescriptorInfo, który zawiera dwie właściwości: XElement reprezentacja deskryptor i typ, który reprezentuje [IAuthenticatedEncryptorDescriptorDeserializer](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptordescriptordeserializer) może to stanowić używany do operacji przywracania aktywności ten deskryptor, biorąc pod uwagę odpowiedniej klasy XElement.</span><span class="sxs-lookup"><span data-stu-id="080a5-142">This routine returns an XmlSerializedDescriptorInfo which contains two properties: the XElement representation of the descriptor and the Type which represents an [IAuthenticatedEncryptorDescriptorDeserializer](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptordescriptordeserializer) which can be used to resurrect this descriptor given the corresponding XElement.</span></span>

<span data-ttu-id="080a5-143">Zserializowany deskryptora mogą zawierać poufne informacje, takie jak materiału klucza kryptograficznego.</span><span class="sxs-lookup"><span data-stu-id="080a5-143">The serialized descriptor may contain sensitive information such as cryptographic key material.</span></span> <span data-ttu-id="080a5-144">System ochrony danych ma wbudowaną obsługę szyfrowania informacji przed zawiera utrwalone w magazynie.</span><span class="sxs-lookup"><span data-stu-id="080a5-144">The data protection system has built-in support for encrypting information before it's persisted to storage.</span></span> <span data-ttu-id="080a5-145">Aby móc korzystać z tego, deskryptor należy oznaczyć element, który zawiera poufne informacje z nazwy atrybutu "requiresEncryption" (xmlns "<http://schemas.asp.net/2015/03/dataProtection>"), wartość "true".</span><span class="sxs-lookup"><span data-stu-id="080a5-145">To take advantage of this, the descriptor should mark the element which contains sensitive information with the attribute name "requiresEncryption" (xmlns "<http://schemas.asp.net/2015/03/dataProtection>"), value "true".</span></span>

>[!TIP]
> <span data-ttu-id="080a5-146">Brak pomocy interfejsu API dla ustawienie tego atrybutu.</span><span class="sxs-lookup"><span data-stu-id="080a5-146">There's a helper API for setting this attribute.</span></span> <span data-ttu-id="080a5-147">Wywołanie metody rozszerzenia, który XElement.MarkAsRequiresEncryption() znajduje się w przestrzeni nazw Microsoft.AspNetCore.DataProtection.AuthenticatedEncryption.ConfigurationModel.</span><span class="sxs-lookup"><span data-stu-id="080a5-147">Call the extension method XElement.MarkAsRequiresEncryption() located in namespace Microsoft.AspNetCore.DataProtection.AuthenticatedEncryption.ConfigurationModel.</span></span>

<span data-ttu-id="080a5-148">Można także przypadki, gdzie deskryptora serializacji nie zawiera poufne informacje.</span><span class="sxs-lookup"><span data-stu-id="080a5-148">There can also be cases where the serialized descriptor doesn't contain sensitive information.</span></span> <span data-ttu-id="080a5-149">Należy wziąć pod uwagę ponownie w przypadku klucza kryptograficznego, przechowywane w sprzętowym module zabezpieczeń.</span><span class="sxs-lookup"><span data-stu-id="080a5-149">Consider again the case of a cryptographic key stored in an HSM.</span></span> <span data-ttu-id="080a5-150">Deskryptor nie można zapisać materiału klucza podczas serializowania się, ponieważ w ramach sprzętowego modułu zabezpieczeń nie będzie ujawniać materiałów w formie zwykłego tekstu.</span><span class="sxs-lookup"><span data-stu-id="080a5-150">The descriptor cannot write out the key material when serializing itself since the HSM won't expose the material in plaintext form.</span></span> <span data-ttu-id="080a5-151">Zamiast tego należy napisać deskryptor się opakowane klucz wersję klucza (Jeśli moduł HSM umożliwia eksportowanie w ten sposób) lub HSM własny unikatowy identyfikator klucza.</span><span class="sxs-lookup"><span data-stu-id="080a5-151">Instead, the descriptor might write out the key-wrapped version of the key (if the HSM allows export in this fashion) or the HSM's own unique identifier for the key.</span></span>

<a name="data-protection-extensibility-core-crypto-iauthenticatedencryptordescriptordeserializer"></a>

## <a name="iauthenticatedencryptordescriptordeserializer"></a><span data-ttu-id="080a5-152">IAuthenticatedEncryptorDescriptorDeserializer</span><span class="sxs-lookup"><span data-stu-id="080a5-152">IAuthenticatedEncryptorDescriptorDeserializer</span></span>

<span data-ttu-id="080a5-153">**IAuthenticatedEncryptorDescriptorDeserializer** interfejs reprezentuje typ, który wie, jak wykonać deserializacji wystąpienia IAuthenticatedEncryptorDescriptor z XElement.</span><span class="sxs-lookup"><span data-stu-id="080a5-153">The **IAuthenticatedEncryptorDescriptorDeserializer** interface represents a type that knows how to deserialize an IAuthenticatedEncryptorDescriptor instance from an XElement.</span></span> <span data-ttu-id="080a5-154">Udostępnia pojedynczą metodę:</span><span class="sxs-lookup"><span data-stu-id="080a5-154">It exposes a single method:</span></span>

* <span data-ttu-id="080a5-155">ImportFromXml (XElement element): IAuthenticatedEncryptorDescriptor</span><span class="sxs-lookup"><span data-stu-id="080a5-155">ImportFromXml(XElement element) : IAuthenticatedEncryptorDescriptor</span></span>

<span data-ttu-id="080a5-156">Metoda ImportFromXml przyjmuje XElement, który został zwrócony przez [IAuthenticatedEncryptorDescriptor.ExportToXml](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-exporttoxml) i tworzy odpowiednik IAuthenticatedEncryptorDescriptor oryginalnej.</span><span class="sxs-lookup"><span data-stu-id="080a5-156">The ImportFromXml method takes the XElement that was returned by [IAuthenticatedEncryptorDescriptor.ExportToXml](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-exporttoxml) and creates an equivalent of the original IAuthenticatedEncryptorDescriptor.</span></span>

<span data-ttu-id="080a5-157">Typy, które implementują IAuthenticatedEncryptorDescriptorDeserializer powinien mieć jedną z następujących dwóch konstruktorów publicznych:</span><span class="sxs-lookup"><span data-stu-id="080a5-157">Types which implement IAuthenticatedEncryptorDescriptorDeserializer should have one of the following two public constructors:</span></span>

* <span data-ttu-id="080a5-158">.ctor(IServiceProvider)</span><span class="sxs-lookup"><span data-stu-id="080a5-158">.ctor(IServiceProvider)</span></span>

* <span data-ttu-id="080a5-159">.ctor()</span><span class="sxs-lookup"><span data-stu-id="080a5-159">.ctor()</span></span>

> [!NOTE]
> <span data-ttu-id="080a5-160">Element IServiceProvider przekazany do konstruktora może mieć wartości null.</span><span class="sxs-lookup"><span data-stu-id="080a5-160">The IServiceProvider passed to the constructor may be null.</span></span>

## <a name="the-top-level-factory"></a><span data-ttu-id="080a5-161">Fabryka najwyższego poziomu</span><span class="sxs-lookup"><span data-stu-id="080a5-161">The top-level factory</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="080a5-162">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="080a5-162">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="080a5-163">**AlgorithmConfiguration** klasa reprezentuje typ, który wie, jak utworzyć [IAuthenticatedEncryptorDescriptor](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptordescriptor) wystąpień.</span><span class="sxs-lookup"><span data-stu-id="080a5-163">The **AlgorithmConfiguration** class represents a type which knows how to create [IAuthenticatedEncryptorDescriptor](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptordescriptor) instances.</span></span> <span data-ttu-id="080a5-164">Udostępnia jeden interfejs API.</span><span class="sxs-lookup"><span data-stu-id="080a5-164">It exposes a single API.</span></span>

* <span data-ttu-id="080a5-165">CreateNewDescriptor() : IAuthenticatedEncryptorDescriptor</span><span class="sxs-lookup"><span data-stu-id="080a5-165">CreateNewDescriptor() : IAuthenticatedEncryptorDescriptor</span></span>

<span data-ttu-id="080a5-166">Pomyśl o AlgorithmConfiguration, ponieważ fabryka najwyższego poziomu.</span><span class="sxs-lookup"><span data-stu-id="080a5-166">Think of AlgorithmConfiguration as the top-level factory.</span></span> <span data-ttu-id="080a5-167">Konfiguracja służy jako szablon.</span><span class="sxs-lookup"><span data-stu-id="080a5-167">The configuration serves as a template.</span></span> <span data-ttu-id="080a5-168">Opakowuje konsolidatorze informacji (np. Ta konfiguracja daje deskryptory za pomocą klucza głównego AES-128-GCM), ale jeszcze nie jest skojarzony z określonym kluczem.</span><span class="sxs-lookup"><span data-stu-id="080a5-168">It wraps algorithmic information (e.g., this configuration produces descriptors with an AES-128-GCM master key), but it's not yet associated with a specific key.</span></span>

<span data-ttu-id="080a5-169">CreateNewDescriptor jest wywoływana, świeży materiału klucza jest przeznaczone wyłącznie dla tego wywołania, gdy jest generowany nowy IAuthenticatedEncryptorDescriptor który otacza tego materiału klucza i konsolidatorze informacje wymagane do korzystania z materiałów.</span><span class="sxs-lookup"><span data-stu-id="080a5-169">When CreateNewDescriptor is called, fresh key material is created solely for this call, and a new IAuthenticatedEncryptorDescriptor is produced which wraps this key material and the algorithmic information required to consume the material.</span></span> <span data-ttu-id="080a5-170">Materiał klucza można utworzone w oprogramowaniu (i przechowywane w pamięci), może być tworzone i przechowywane w ramach sprzętowego modułu zabezpieczeń i tak dalej.</span><span class="sxs-lookup"><span data-stu-id="080a5-170">The key material could be created in software (and held in memory), it could be created and held within an HSM, and so on.</span></span> <span data-ttu-id="080a5-171">Kluczowym punktem jest wszelkie wywołania dwóch CreateNewDescriptor powinien nigdy tworzyć równoważne IAuthenticatedEncryptorDescriptor wystąpień.</span><span class="sxs-lookup"><span data-stu-id="080a5-171">The crucial point is that any two calls to CreateNewDescriptor should never create equivalent IAuthenticatedEncryptorDescriptor instances.</span></span>

<span data-ttu-id="080a5-172">Typ AlgorithmConfiguration służy jako punkt wejścia dla procedury tworzenia klucza, takich jak [automatyczne klucz stopniowe](xref:security/data-protection/implementation/key-management#key-expiration-and-rolling).</span><span class="sxs-lookup"><span data-stu-id="080a5-172">The AlgorithmConfiguration type serves as the entry point for key creation routines such as [automatic key rolling](xref:security/data-protection/implementation/key-management#key-expiration-and-rolling).</span></span> <span data-ttu-id="080a5-173">Aby zmienić wdrożenia dla wszystkich przyszłych kluczy, należy ustawić właściwość AuthenticatedEncryptorConfiguration w KeyManagementOptions.</span><span class="sxs-lookup"><span data-stu-id="080a5-173">To change the implementation for all future keys, set the AuthenticatedEncryptorConfiguration property in KeyManagementOptions.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="080a5-174">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="080a5-174">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="080a5-175">**IAuthenticatedEncryptorConfiguration** interfejs reprezentuje typ, który wie, jak utworzyć [IAuthenticatedEncryptorDescriptor](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptordescriptor) wystąpień.</span><span class="sxs-lookup"><span data-stu-id="080a5-175">The **IAuthenticatedEncryptorConfiguration** interface represents a type which knows how to create [IAuthenticatedEncryptorDescriptor](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptordescriptor) instances.</span></span> <span data-ttu-id="080a5-176">Udostępnia jeden interfejs API.</span><span class="sxs-lookup"><span data-stu-id="080a5-176">It exposes a single API.</span></span>

* <span data-ttu-id="080a5-177">CreateNewDescriptor() : IAuthenticatedEncryptorDescriptor</span><span class="sxs-lookup"><span data-stu-id="080a5-177">CreateNewDescriptor() : IAuthenticatedEncryptorDescriptor</span></span>

<span data-ttu-id="080a5-178">Pomyśl o IAuthenticatedEncryptorConfiguration, ponieważ fabryka najwyższego poziomu.</span><span class="sxs-lookup"><span data-stu-id="080a5-178">Think of IAuthenticatedEncryptorConfiguration as the top-level factory.</span></span> <span data-ttu-id="080a5-179">Konfiguracja służy jako szablon.</span><span class="sxs-lookup"><span data-stu-id="080a5-179">The configuration serves as a template.</span></span> <span data-ttu-id="080a5-180">Opakowuje konsolidatorze informacji (np. Ta konfiguracja daje deskryptory za pomocą klucza głównego AES-128-GCM), ale jeszcze nie jest skojarzony z określonym kluczem.</span><span class="sxs-lookup"><span data-stu-id="080a5-180">It wraps algorithmic information (e.g., this configuration produces descriptors with an AES-128-GCM master key), but it's not yet associated with a specific key.</span></span>

<span data-ttu-id="080a5-181">CreateNewDescriptor jest wywoływana, świeży materiału klucza jest przeznaczone wyłącznie dla tego wywołania, gdy jest generowany nowy IAuthenticatedEncryptorDescriptor który otacza tego materiału klucza i konsolidatorze informacje wymagane do korzystania z materiałów.</span><span class="sxs-lookup"><span data-stu-id="080a5-181">When CreateNewDescriptor is called, fresh key material is created solely for this call, and a new IAuthenticatedEncryptorDescriptor is produced which wraps this key material and the algorithmic information required to consume the material.</span></span> <span data-ttu-id="080a5-182">Materiał klucza można utworzone w oprogramowaniu (i przechowywane w pamięci), może być tworzone i przechowywane w ramach sprzętowego modułu zabezpieczeń i tak dalej.</span><span class="sxs-lookup"><span data-stu-id="080a5-182">The key material could be created in software (and held in memory), it could be created and held within an HSM, and so on.</span></span> <span data-ttu-id="080a5-183">Kluczowym punktem jest wszelkie wywołania dwóch CreateNewDescriptor powinien nigdy tworzyć równoważne IAuthenticatedEncryptorDescriptor wystąpień.</span><span class="sxs-lookup"><span data-stu-id="080a5-183">The crucial point is that any two calls to CreateNewDescriptor should never create equivalent IAuthenticatedEncryptorDescriptor instances.</span></span>

<span data-ttu-id="080a5-184">Typ IAuthenticatedEncryptorConfiguration służy jako punkt wejścia dla procedury tworzenia klucza, takich jak [automatyczne klucz stopniowe](xref:security/data-protection/implementation/key-management#key-expiration-and-rolling).</span><span class="sxs-lookup"><span data-stu-id="080a5-184">The IAuthenticatedEncryptorConfiguration type serves as the entry point for key creation routines such as [automatic key rolling](xref:security/data-protection/implementation/key-management#key-expiration-and-rolling).</span></span> <span data-ttu-id="080a5-185">Aby zmienić wdrożenia dla wszystkich przyszłych kluczy, należy zarejestrować pojedyncze IAuthenticatedEncryptorConfiguration w kontenerze usługi.</span><span class="sxs-lookup"><span data-stu-id="080a5-185">To change the implementation for all future keys, register a singleton IAuthenticatedEncryptorConfiguration in the service container.</span></span>

---
