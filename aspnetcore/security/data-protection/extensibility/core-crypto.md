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
# <a name="core-cryptography-extensibility-in-aspnet-core"></a>Rozszerzalność kryptografii Core w programie ASP.NET Core

<a name="data-protection-extensibility-core-crypto"></a>

>[!WARNING]
> Typy, które implementują żadnego z następujących interfejsów powinny być metodą o bezpiecznych wątkach dla wielu obiektów wywołujących.

<a name="data-protection-extensibility-core-crypto-iauthenticatedencryptor"></a>

## <a name="iauthenticatedencryptor"></a>IAuthenticatedEncryptor

**IAuthenticatedEncryptor** interfejs jest podstawowym budulcem podsystem kryptograficzny. Ogólnie jest jeden IAuthenticatedEncryptor na klucz, a wystąpienie IAuthenticatedEncryptor opakowuje wszystkich materiału klucza kryptograficznego i informacje algorytmicznego, które są niezbędne do wykonywania operacji kryptograficznych.

Jak sugeruje nazwa, typ jest odpowiedzialne za świadczenie usług uwierzytelnionego szyfrowania i odszyfrowywania. Udostępnia ona następujące dwa interfejsy API.

* `Decrypt(ArraySegment<byte> ciphertext, ArraySegment<byte> additionalAuthenticatedData) : byte[]`

* `Encrypt(ArraySegment<byte> plaintext, ArraySegment<byte> additionalAuthenticatedData) : byte[]`

Metoda szyfrowania zwraca obiekt blob zawiera enciphered zwykłego tekstu i tag uwierzytelniania. Tag uwierzytelniania musi obejmować dodatkowe uwierzytelnionych danych (AAD), chociaż sama usługi AAD nie muszą być możliwe do odzyskania z ostatnim ładunku. Metoda odszyfrowywania weryfikuje tag uwierzytelniania i zwraca deciphered ładunku. Wszystkie błędy (z wyjątkiem ArgumentNullException i podobne) powinny być homogenizowane do cryptographicexception —.

> [!NOTE]
> Samego wystąpienia IAuthenticatedEncryptor faktycznie nie musi zawierać materiał klucza. Na przykład wdrożenia można delegować do modułu HSM dla wszystkich operacji.

<a name="data-protection-extensibility-core-crypto-iauthenticatedencryptorfactory"></a>
<a name="data-protection-extensibility-core-crypto-iauthenticatedencryptordescriptor"></a>

## <a name="how-to-create-an-iauthenticatedencryptor"></a>Jak utworzyć IAuthenticatedEncryptor

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

**IAuthenticatedEncryptorFactory** interfejs reprezentuje typ, który wie, jak utworzyć [IAuthenticatedEncryptor](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptor) wystąpienia. Jego interfejs API jest w następujący sposób.

* CreateEncryptorInstance (klucz IKey): IAuthenticatedEncryptor

W dowolnym danym wystąpieniu klucz Instrumentacji żadnych uwierzytelnionego encryptors tworzone przez metodę jego CreateEncryptorInstance rozważa równoważne, podobnie jak w poniżej przykładowego kodu.

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

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

**IAuthenticatedEncryptorDescriptor** interfejs reprezentuje typ, który wie, jak utworzyć [IAuthenticatedEncryptor](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptor) wystąpienia. Jego interfejs API jest w następujący sposób.

* CreateEncryptorInstance(): IAuthenticatedEncryptor

* ExportToXml(): XmlSerializedDescriptorInfo

Podobnie jak IAuthenticatedEncryptor wystąpienie IAuthenticatedEncryptorDescriptor zakłada, że do opakowania jednego określonego klucza. Oznacza to, że w dowolnym danym wystąpieniu IAuthenticatedEncryptorDescriptor wszelkie uwierzytelnionego encryptors tworzone przez metodę jego CreateEncryptorInstance należy traktować równoważne, podobnie jak w poniżej przykładowego kodu.

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

## <a name="iauthenticatedencryptordescriptor-aspnet-core-2x-only"></a>IAuthenticatedEncryptorDescriptor (ASP.NET Core 2.x tylko)

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

**IAuthenticatedEncryptorDescriptor** interfejs reprezentuje typ, który wie, jak sama wyeksportować do pliku XML. Jego interfejs API jest w następujący sposób.

* ExportToXml(): XmlSerializedDescriptorInfo

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

---

## <a name="xml-serialization"></a>Serializacji XML

Główną różnicą między IAuthenticatedEncryptor i IAuthenticatedEncryptorDescriptor to, że deskryptora wie, jak utworzyć modułu szyfrującego i dostarczyć prawidłowe argumenty. Należy wziąć pod uwagę IAuthenticatedEncryptor, którego implementacja jest uzależniona od SymmetricAlgorithm i KeyedHashAlgorithm. Zadanie modułu szyfrującego jest korzystanie z tych typów, ale go nie zawsze wie, skąd pochodzą te typy, tak naprawdę nie mogą zapisywać się właściwe opis jak odtworzyć sam w przypadku ponownego uruchomienia aplikacji. Deskryptor działa jako na podstawie tego wyższego poziomu. Ponieważ deskryptor wie, jak utworzyć wystąpienie modułu szyfrującego (np. wie, jak utworzyć wymagane algorytmy), może wykonać serializację tę wiedzę w postaci XML, dzięki czemu można odtworzyć wystąpienia modułu szyfrującego po zresetowaniu aplikacji.

<a name="data-protection-extensibility-core-crypto-exporttoxml"></a>

Deskryptor może być Zserializowany za pośrednictwem jego ExportToXml procedury. Ta procedura zwraca XmlSerializedDescriptorInfo, który zawiera dwie właściwości: XElement reprezentacja deskryptor i typ, który reprezentuje [IAuthenticatedEncryptorDescriptorDeserializer](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptordescriptordeserializer) może to stanowić używany do operacji przywracania aktywności ten deskryptor, biorąc pod uwagę odpowiedniej klasy XElement.

Zserializowany deskryptora mogą zawierać poufne informacje, takie jak materiału klucza kryptograficznego. System ochrony danych ma wbudowaną obsługę szyfrowania informacji przed zawiera utrwalone w magazynie. Aby móc korzystać z tego, deskryptor należy oznaczyć element, który zawiera poufne informacje z nazwy atrybutu "requiresEncryption" (xmlns "<http://schemas.asp.net/2015/03/dataProtection>"), wartość "true".

>[!TIP]
> Brak pomocy interfejsu API dla ustawienie tego atrybutu. Wywołanie metody rozszerzenia, który XElement.MarkAsRequiresEncryption() znajduje się w przestrzeni nazw Microsoft.AspNetCore.DataProtection.AuthenticatedEncryption.ConfigurationModel.

Można także przypadki, gdzie deskryptora serializacji nie zawiera poufne informacje. Należy wziąć pod uwagę ponownie w przypadku klucza kryptograficznego, przechowywane w sprzętowym module zabezpieczeń. Deskryptor nie można zapisać materiału klucza podczas serializowania się, ponieważ w ramach sprzętowego modułu zabezpieczeń nie będzie ujawniać materiałów w formie zwykłego tekstu. Zamiast tego należy napisać deskryptor się opakowane klucz wersję klucza (Jeśli moduł HSM umożliwia eksportowanie w ten sposób) lub HSM własny unikatowy identyfikator klucza.

<a name="data-protection-extensibility-core-crypto-iauthenticatedencryptordescriptordeserializer"></a>

## <a name="iauthenticatedencryptordescriptordeserializer"></a>IAuthenticatedEncryptorDescriptorDeserializer

**IAuthenticatedEncryptorDescriptorDeserializer** interfejs reprezentuje typ, który wie, jak wykonać deserializacji wystąpienia IAuthenticatedEncryptorDescriptor z XElement. Udostępnia pojedynczą metodę:

* ImportFromXml (XElement element): IAuthenticatedEncryptorDescriptor

Metoda ImportFromXml przyjmuje XElement, który został zwrócony przez [IAuthenticatedEncryptorDescriptor.ExportToXml](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-exporttoxml) i tworzy odpowiednik IAuthenticatedEncryptorDescriptor oryginalnej.

Typy, które implementują IAuthenticatedEncryptorDescriptorDeserializer powinien mieć jedną z następujących dwóch konstruktorów publicznych:

* .ctor(IServiceProvider)

* .ctor()

> [!NOTE]
> Element IServiceProvider przekazany do konstruktora może mieć wartości null.

## <a name="the-top-level-factory"></a>Fabryka najwyższego poziomu

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

**AlgorithmConfiguration** klasa reprezentuje typ, który wie, jak utworzyć [IAuthenticatedEncryptorDescriptor](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptordescriptor) wystąpień. Udostępnia jeden interfejs API.

* CreateNewDescriptor() : IAuthenticatedEncryptorDescriptor

Pomyśl o AlgorithmConfiguration, ponieważ fabryka najwyższego poziomu. Konfiguracja służy jako szablon. Opakowuje konsolidatorze informacji (np. Ta konfiguracja daje deskryptory za pomocą klucza głównego AES-128-GCM), ale jeszcze nie jest skojarzony z określonym kluczem.

CreateNewDescriptor jest wywoływana, świeży materiału klucza jest przeznaczone wyłącznie dla tego wywołania, gdy jest generowany nowy IAuthenticatedEncryptorDescriptor który otacza tego materiału klucza i konsolidatorze informacje wymagane do korzystania z materiałów. Materiał klucza można utworzone w oprogramowaniu (i przechowywane w pamięci), może być tworzone i przechowywane w ramach sprzętowego modułu zabezpieczeń i tak dalej. Kluczowym punktem jest wszelkie wywołania dwóch CreateNewDescriptor powinien nigdy tworzyć równoważne IAuthenticatedEncryptorDescriptor wystąpień.

Typ AlgorithmConfiguration służy jako punkt wejścia dla procedury tworzenia klucza, takich jak [automatyczne klucz stopniowe](xref:security/data-protection/implementation/key-management#key-expiration-and-rolling). Aby zmienić wdrożenia dla wszystkich przyszłych kluczy, należy ustawić właściwość AuthenticatedEncryptorConfiguration w KeyManagementOptions.

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

**IAuthenticatedEncryptorConfiguration** interfejs reprezentuje typ, który wie, jak utworzyć [IAuthenticatedEncryptorDescriptor](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptordescriptor) wystąpień. Udostępnia jeden interfejs API.

* CreateNewDescriptor() : IAuthenticatedEncryptorDescriptor

Pomyśl o IAuthenticatedEncryptorConfiguration, ponieważ fabryka najwyższego poziomu. Konfiguracja służy jako szablon. Opakowuje konsolidatorze informacji (np. Ta konfiguracja daje deskryptory za pomocą klucza głównego AES-128-GCM), ale jeszcze nie jest skojarzony z określonym kluczem.

CreateNewDescriptor jest wywoływana, świeży materiału klucza jest przeznaczone wyłącznie dla tego wywołania, gdy jest generowany nowy IAuthenticatedEncryptorDescriptor który otacza tego materiału klucza i konsolidatorze informacje wymagane do korzystania z materiałów. Materiał klucza można utworzone w oprogramowaniu (i przechowywane w pamięci), może być tworzone i przechowywane w ramach sprzętowego modułu zabezpieczeń i tak dalej. Kluczowym punktem jest wszelkie wywołania dwóch CreateNewDescriptor powinien nigdy tworzyć równoważne IAuthenticatedEncryptorDescriptor wystąpień.

Typ IAuthenticatedEncryptorConfiguration służy jako punkt wejścia dla procedury tworzenia klucza, takich jak [automatyczne klucz stopniowe](xref:security/data-protection/implementation/key-management#key-expiration-and-rolling). Aby zmienić wdrożenia dla wszystkich przyszłych kluczy, należy zarejestrować pojedyncze IAuthenticatedEncryptorConfiguration w kontenerze usługi.

---
