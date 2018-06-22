---
title: Rozszerzalność kryptografii Core na platformie ASP.NET Core
author: rick-anderson
description: Więcej informacji na temat IAuthenticatedEncryptor, IAuthenticatedEncryptorDescriptor IAuthenticatedEncryptorDescriptorDeserializer i fabryki najwyższego poziomu.
ms.author: riande
ms.date: 8/11/2017
uid: security/data-protection/extensibility/core-crypto
ms.openlocfilehash: 47432cfefe0a52c9f815d717f7269ec68fdb6af3
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/20/2018
ms.locfileid: "36272899"
---
# <a name="core-cryptography-extensibility-in-aspnet-core"></a>Rozszerzalność kryptografii Core na platformie ASP.NET Core

<a name="data-protection-extensibility-core-crypto"></a>

>[!WARNING]
> Typy implementujące żadnego z następujących interfejsów powinny być bezpieczne wątkowo dla wielu wywołań.

<a name="data-protection-extensibility-core-crypto-iauthenticatedencryptor"></a>

## <a name="iauthenticatedencryptor"></a>IAuthenticatedEncryptor

**IAuthenticatedEncryptor** interfejs jest podstawowym blokiem konstrukcyjnym podsystem kryptograficzny. Zwykle jest jednym IAuthenticatedEncryptor na klucz, a wystąpienia IAuthenticatedEncryptor opakowuje wszystkich materiał kluczy kryptograficznych i algorytmicznego informacje niezbędne do wykonywania operacji kryptograficznych.

Zgodnie z sugestią, jego nazwa, typ jest odpowiedzialny za zapewnienie uwierzytelnionego usług szyfrowania i odszyfrowywania. Udostępnia ona następujące dwa interfejsy API.

* Odszyfrowywanie (ArraySegment<byte> tekstu szyfrowanego, ArraySegment<byte> additionalAuthenticatedData): byte]

* Szyfrowanie (ArraySegment<byte> w postaci zwykłego tekstu, ArraySegment<byte> additionalAuthenticatedData): byte]

Metoda szyfrowania zwraca obiektu blob zawierającego enciphered zwykłego tekstu i tag uwierzytelniania. Tag uwierzytelniania musi obejmować dodatkowe uwierzytelnionych danych (AAD), chociaż sama usługi AAD nie muszą być możliwe do odzyskania z ostatnim ładunku. Metoda odszyfrowywania weryfikuje tag uwierzytelniania i zwraca deciphered ładunku. Wszystkie błędy (z wyjątkiem argumentnullexception — i podobne) powinny być homogenizowane do cryptographicexception —.

> [!NOTE]
> Wystąpienie IAuthenticatedEncryptor sam faktycznie nie musi zawierać materiału klucza. Na przykład wdrożenia można delegować do modułu HSM dla wszystkich operacji.

<a name="data-protection-extensibility-core-crypto-iauthenticatedencryptorfactory"></a>
<a name="data-protection-extensibility-core-crypto-iauthenticatedencryptordescriptor"></a>

## <a name="how-to-create-an-iauthenticatedencryptor"></a>Jak utworzyć IAuthenticatedEncryptor

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

**IAuthenticatedEncryptorFactory** interfejsu reprezentuje typ, który umożliwia tworzenie [IAuthenticatedEncryptor](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptor) wystąpienia. Poniżej przedstawiono jej interfejsu API.

* CreateEncryptorInstance (klucz IKey): IAuthenticatedEncryptor

Dla każdego danego wystąpienia IKey żadnych uwierzytelnionego encryptors tworzone przez metodę jego CreateEncryptorInstance należy traktować jako równoważne, podobnie jak w poniżej przykładowy kod.

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

**IAuthenticatedEncryptorDescriptor** interfejsu reprezentuje typ, który umożliwia tworzenie [IAuthenticatedEncryptor](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptor) wystąpienia. Poniżej przedstawiono jej interfejsu API.

* CreateEncryptorInstance(): IAuthenticatedEncryptor

* ExportToXml(): XmlSerializedDescriptorInfo

Podobnie jak IAuthenticatedEncryptor wystąpienie IAuthenticatedEncryptorDescriptor zakłada się, że zawijać jednego określonego klucza. Oznacza to, że dla dowolnego danego wystąpienia IAuthenticatedEncryptorDescriptor żadnych uwierzytelnionego encryptors tworzone przez metodę jego CreateEncryptorInstance należy traktować jako równoważne, podobnie jak w poniżej przykładowy kod.

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

## <a name="iauthenticatedencryptordescriptor-aspnet-core-2x-only"></a>IAuthenticatedEncryptorDescriptor (tylko 2.x platformy ASP.NET Core)

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

**IAuthenticatedEncryptorDescriptor** interfejsu reprezentuje typ, który potrafi do samej siebie wyeksportować do pliku XML. Poniżej przedstawiono jej interfejsu API.

* ExportToXml(): XmlSerializedDescriptorInfo

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

---

## <a name="xml-serialization"></a>Serializacji XML

Główną różnicą między IAuthenticatedEncryptor i IAuthenticatedEncryptorDescriptor jest, że deskryptor wie, jak utworzyć modułu szyfrującego i dostarczyć nieprawidłowe argumenty. Należy wziąć pod uwagę IAuthenticatedEncryptor, którego implementacja polega na SymmetricAlgorithm i KeyedHashAlgorithm. Zadanie modułu szyfrującego ma korzystać z tych typów, ale go nie musi wiedzieć, skąd pochodzą te typy, tak naprawdę nie można zapisać się odpowiednie opis sposobu ponownego tworzenia się, jeśli aplikacja zostanie uruchomiony ponownie. Deskryptor działa jako wyższego poziomu na podstawie to. Ponieważ deskryptor potrafi można utworzyć wystąpienia modułu szyfrującego (np. wie jak utworzyć wymagane algorytmy), może wykonać serializację tę wiedzę w postaci XML, tak aby modułu szyfrującego wystąpienie można utworzyć ponownie po zresetowaniu aplikacji.

<a name="data-protection-extensibility-core-crypto-exporttoxml"></a>

Deskryptor może być Zserializowany za pośrednictwem jego ExportToXml procedury. Ta procedura zwraca XmlSerializedDescriptorInfo, który zawiera dwie właściwości: reprezentacja klasy XElement deskryptor i typ, który reprezentuje [IAuthenticatedEncryptorDescriptorDeserializer](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptordescriptordeserializer) może to używane do przywracania aktywności ten deskryptor podane odpowiedniej klasy XElement.

Deskryptor serializacji mogą zawierać poufne informacje, takie jak materiał kluczy kryptograficznych. System ochrony danych ma wbudowaną obsługę szyfrowania informacji przed zostało utrwalone w magazynie. Aby móc korzystać z tego, deskryptora należy zaznaczyć element, który zawiera poufne informacje o nazwie atrybutu "requiresEncryption" (xmlns "<http://schemas.asp.net/2015/03/dataProtection>"), wartość "true".

>[!TIP]
> Brak pomocnik interfejsu API dla ustawienie tego atrybutu. Wywołanie metody rozszerzenia, które XElement.MarkAsRequiresEncryption() znajduje się w przestrzeni nazw Microsoft.AspNetCore.DataProtection.AuthenticatedEncryption.ConfigurationModel.

Można także przypadki, w którym deskryptora serializacji nie zawiera poufne informacje. Rozważmy przykład ponownie klucza kryptograficznego przechowywane w module HSM. Deskryptor nie można zapisać materiału klucza podczas serializowania się, ponieważ moduł HSM nie uwidacznia materiału w formie zwykłego tekstu. Zamiast tego deskryptora może zapisać opakowana klucz wersji klucza (Jeśli moduł HSM umożliwia eksportu w ten sposób) lub własne HSM Unikatowy identyfikator dla klucza.

<a name="data-protection-extensibility-core-crypto-iauthenticatedencryptordescriptordeserializer"></a>

## <a name="iauthenticatedencryptordescriptordeserializer"></a>IAuthenticatedEncryptorDescriptorDeserializer

**IAuthenticatedEncryptorDescriptorDeserializer** interfejsu reprezentuje typ, który potrafi zdeserializować wystąpienia IAuthenticatedEncryptorDescriptor z XElement. Udostępnia ona jedną metodę:

* ImportFromXml (element klasy XElement): IAuthenticatedEncryptorDescriptor

Metoda ImportFromXml przyjmuje klasy XElement, który został zwrócony przez [IAuthenticatedEncryptorDescriptor.ExportToXml](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-exporttoxml) i tworzy równoważny IAuthenticatedEncryptorDescriptor oryginalnego.

Typy implementujące IAuthenticatedEncryptorDescriptorDeserializer powinny mieć jedno z następujących dwóch konstruktorów publicznych:

* .ctor(IServiceProvider)

* .ctor()

> [!NOTE]
> IServiceProvider przekazany do konstruktora może mieć wartości null.

## <a name="the-top-level-factory"></a>Fabryka najwyższego poziomu

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

**AlgorithmConfiguration** klasa reprezentuje typ, który umożliwia tworzenie [IAuthenticatedEncryptorDescriptor](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptordescriptor) wystąpień. Prezentuje ona jednego interfejsu API.

* CreateNewDescriptor(): IAuthenticatedEncryptorDescriptor

AlgorithmConfiguration można traktować jako fabryka najwyższego poziomu. Konfiguracja służy jako szablon. Jest zawijany algorytmicznego informacji (np. Ta konfiguracja daje deskryptory z kluczem głównym AES-128-GCM), ale nie ma nie został jeszcze skojarzony z określonym kluczem.

Gdy CreateNewDescriptor jest wywoływana, świeże materiału klucza jest przeznaczone wyłącznie dla tego wywołania i jest generowany nowy IAuthenticatedEncryptorDescriptor który koduje tego materiału klucza i algorytmicznego informacje wymagane użycie materiałów. Materiału klucza można utworzyć w oprogramowaniu (i przechowywane w pamięci), można utworzyć i przechowywane w module HSM i tak dalej. Kluczowe punkt znajduje się wszelkie dwóch wywołania CreateNewDescriptor powinien nigdy tworzyć wystąpienia IAuthenticatedEncryptorDescriptor równoważne.

Typ AlgorithmConfiguration służy jako punkt wejścia dla procedury tworzenia klucza, takich jak [automatyczne klucza stopniowych](xref:security/data-protection/implementation/key-management#key-expiration-and-rolling). Aby zmienić wdrożenia dla wszystkich przyszłych kluczy, należy ustawić właściwość AuthenticatedEncryptorConfiguration w KeyManagementOptions.

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

**IAuthenticatedEncryptorConfiguration** interfejsu reprezentuje typ, który umożliwia tworzenie [IAuthenticatedEncryptorDescriptor](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptordescriptor) wystąpień. Prezentuje ona jednego interfejsu API.

* CreateNewDescriptor(): IAuthenticatedEncryptorDescriptor

IAuthenticatedEncryptorConfiguration można traktować jako fabryka najwyższego poziomu. Konfiguracja służy jako szablon. Jest zawijany algorytmicznego informacji (np. Ta konfiguracja daje deskryptory z kluczem głównym AES-128-GCM), ale nie ma nie został jeszcze skojarzony z określonym kluczem.

Gdy CreateNewDescriptor jest wywoływana, świeże materiału klucza jest przeznaczone wyłącznie dla tego wywołania i jest generowany nowy IAuthenticatedEncryptorDescriptor który koduje tego materiału klucza i algorytmicznego informacje wymagane użycie materiałów. Materiału klucza można utworzyć w oprogramowaniu (i przechowywane w pamięci), można utworzyć i przechowywane w module HSM i tak dalej. Kluczowe punkt znajduje się wszelkie dwóch wywołania CreateNewDescriptor powinien nigdy tworzyć wystąpienia IAuthenticatedEncryptorDescriptor równoważne.

Typ IAuthenticatedEncryptorConfiguration służy jako punkt wejścia dla procedury tworzenia klucza, takich jak [automatyczne klucza stopniowych](xref:security/data-protection/implementation/key-management#key-expiration-and-rolling). Aby zmienić wdrożenia dla wszystkich przyszłych kluczy, należy zarejestrować pojedynczą IAuthenticatedEncryptorConfiguration w kontenerze usług.

---
