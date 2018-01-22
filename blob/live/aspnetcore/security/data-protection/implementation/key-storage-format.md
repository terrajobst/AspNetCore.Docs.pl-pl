---
title: Format magazynu kluczy
author: tdykstra
description: "W tym dokumencie opisano szczegóły implementacji format magazynu kluczy ochrony danych platformy ASP.NET Core."
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/implementation/key-storage-format
ms.openlocfilehash: 4b796df19349227ceabad185c539720cee7e7c83
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/19/2018
---
# <a name="key-storage-format"></a><span data-ttu-id="91a11-103">Format magazynu kluczy</span><span class="sxs-lookup"><span data-stu-id="91a11-103">Key Storage Format</span></span>

<a name="data-protection-implementation-key-storage-format"></a>

<span data-ttu-id="91a11-104">Obiekty przechowywane są magazynowane w reprezentacji XML.</span><span class="sxs-lookup"><span data-stu-id="91a11-104">Objects are stored at rest in XML representation.</span></span> <span data-ttu-id="91a11-105">Domyślny katalog do przechowywania klucza wynosi % LOCALAPPDATA%\ASP.NET\DataProtection-Keys\.</span><span class="sxs-lookup"><span data-stu-id="91a11-105">The default directory for key storage is %LOCALAPPDATA%\ASP.NET\DataProtection-Keys\.</span></span>

## <a name="the-key-element"></a><span data-ttu-id="91a11-106">\<Klucza > — element</span><span class="sxs-lookup"><span data-stu-id="91a11-106">The \<key> element</span></span>

<span data-ttu-id="91a11-107">Klucze istnieje jako obiekty najwyższego poziomu w repozytorium klucza.</span><span class="sxs-lookup"><span data-stu-id="91a11-107">Keys exist as top-level objects in the key repository.</span></span> <span data-ttu-id="91a11-108">Konwencja klucze mają nazwę pliku **klucza-{guid} .xml**, gdzie {guid} jest identyfikatora klucza.</span><span class="sxs-lookup"><span data-stu-id="91a11-108">By convention keys have the filename **key-{guid}.xml**, where {guid} is the id of the key.</span></span> <span data-ttu-id="91a11-109">Każdy taki plik zawiera jeden klucz.</span><span class="sxs-lookup"><span data-stu-id="91a11-109">Each such file contains a single key.</span></span> <span data-ttu-id="91a11-110">Format pliku jest w następujący sposób.</span><span class="sxs-lookup"><span data-stu-id="91a11-110">The format of the file is as follows.</span></span>

```xml
<?xml version="1.0" encoding="utf-8"?>
<key id="80732141-ec8f-4b80-af9c-c4d2d1ff8901" version="1">
  <creationDate>2015-03-19T23:32:02.3949887Z</creationDate>
  <activationDate>2015-03-19T23:32:02.3839429Z</activationDate>
  <expirationDate>2015-06-17T23:32:02.3839429Z</expirationDate>
  <descriptor deserializerType="{deserializerType}">
    <descriptor>
      <encryption algorithm="AES_256_CBC" />
      <validation algorithm="HMACSHA256" />
      <enc:encryptedSecret decryptorType="{decryptorType}" xmlns:enc="...">
        <encryptedKey>
          <!-- This key is encrypted with Windows DPAPI. -->
          <value>AQAAANCM...8/zeP8lcwAg==</value>
        </encryptedKey>
      </enc:encryptedSecret>
    </descriptor>
  </descriptor>
</key>
```

<span data-ttu-id="91a11-111">\<Klucza > element zawiera następujące atrybuty i elementy podrzędne:</span><span class="sxs-lookup"><span data-stu-id="91a11-111">The \<key> element contains the following attributes and child elements:</span></span>

* <span data-ttu-id="91a11-112">Identyfikator klucza. Ta wartość jest traktowane jako autorytatywne; Nazwa pliku jest po prostu nicety dla człowieka czytelności.</span><span class="sxs-lookup"><span data-stu-id="91a11-112">The key id. This value is treated as authoritative; the filename is simply a nicety for human readability.</span></span>

* <span data-ttu-id="91a11-113">Wersja \<klucza > element obecnie ustalone na 1.</span><span class="sxs-lookup"><span data-stu-id="91a11-113">The version of the \<key> element, currently fixed at 1.</span></span>

* <span data-ttu-id="91a11-114">Klucz daty utworzenia, aktywacji i wygaśnięcia.</span><span class="sxs-lookup"><span data-stu-id="91a11-114">The key's creation, activation, and expiration dates.</span></span>

* <span data-ttu-id="91a11-115">A \<deskryptora > element, który zawiera informacje dotyczące wykonania szyfrowania uwierzytelnionego zawarta w tym kluczu.</span><span class="sxs-lookup"><span data-stu-id="91a11-115">A \<descriptor> element, which contains information on the authenticated encryption implementation contained within this key.</span></span>

<span data-ttu-id="91a11-116">W powyższym przykładzie, identyfikator klucza jest {80732141-ec8f-4b80-af9c-c4d2d1ff8901}, została utworzona, a na 19 marca 2015 aktywowany i zawiera okresu istnienia 90 dni.</span><span class="sxs-lookup"><span data-stu-id="91a11-116">In the above example, the key's id is {80732141-ec8f-4b80-af9c-c4d2d1ff8901}, it was created and activated on March 19, 2015, and it has a lifetime of 90 days.</span></span> <span data-ttu-id="91a11-117">(Czasami Data aktywacji mogą być nieco przed Data utworzenia, jak w poniższym przykładzie.</span><span class="sxs-lookup"><span data-stu-id="91a11-117">(Occasionally the activation date might be slightly before the creation date as in this example.</span></span> <span data-ttu-id="91a11-118">Jest to spowodowane nNie jak interfejsy API działa i jest bezpieczne w praktyce.)</span><span class="sxs-lookup"><span data-stu-id="91a11-118">This is due to a nit in how the APIs work and is harmless in practice.)</span></span>

## <a name="the-descriptor-element"></a><span data-ttu-id="91a11-119">\<Deskryptora > — element</span><span class="sxs-lookup"><span data-stu-id="91a11-119">The \<descriptor> element</span></span>

<span data-ttu-id="91a11-120">Zewnętrznego \<deskryptora > element zawiera deserializerType atrybut, który jest nazwa kwalifikowana zestawu typu, który implementuje IAuthenticatedEncryptorDescriptorDeserializer.</span><span class="sxs-lookup"><span data-stu-id="91a11-120">The outer \<descriptor> element contains an attribute deserializerType, which is the assembly-qualified name of a type which implements IAuthenticatedEncryptorDescriptorDeserializer.</span></span> <span data-ttu-id="91a11-121">Ten typ jest odpowiedzialny za odczytywanie wewnętrzny \<deskryptora > elementu i do analizowania informacji zawartych w.</span><span class="sxs-lookup"><span data-stu-id="91a11-121">This type is responsible for reading the inner \<descriptor> element and for parsing the information contained within.</span></span>

<span data-ttu-id="91a11-122">Format określonego \<deskryptora > elementu zależy od implementacji uwierzytelnionego modułu szyfrującego hermetyzowany za pomocą klucza i każdego typu Deserializator oczekuje nieco inny format tego.</span><span class="sxs-lookup"><span data-stu-id="91a11-122">The particular format of the \<descriptor> element depends on the authenticated encryptor implementation encapsulated by the key, and each deserializer type expects a slightly different format for this.</span></span> <span data-ttu-id="91a11-123">Ogólnie rzecz biorąc, jednak ten element będzie zawierać informacje algorytmicznego (nazwy, typy identyfikatorów OID, lub podobny) i kluczy tajnych.</span><span class="sxs-lookup"><span data-stu-id="91a11-123">In general, though, this element will contain algorithmic information (names, types, OIDs, or similar) and secret key material.</span></span> <span data-ttu-id="91a11-124">W powyższym przykładzie deskryptora Określa, że ten klucz zawijany szyfrowania AES-256-CBC + HMACSHA256 weryfikacji.</span><span class="sxs-lookup"><span data-stu-id="91a11-124">In the above example, the descriptor specifies that this key wraps AES-256-CBC encryption + HMACSHA256 validation.</span></span>

## <a name="the-encryptedsecret-element"></a><span data-ttu-id="91a11-125">\<EncryptedSecret > — element</span><span class="sxs-lookup"><span data-stu-id="91a11-125">The \<encryptedSecret> element</span></span>

<span data-ttu-id="91a11-126"><encryptedSecret> Element, który zawiera zaszyfrowane materiału klucza tajnego mogą występować Jeśli [jest włączone szyfrowanie kluczy tajnych magazynowane](key-encryption-at-rest.md#data-protection-implementation-key-encryption-at-rest).</span><span class="sxs-lookup"><span data-stu-id="91a11-126">An <encryptedSecret> element which contains the encrypted form of the secret key material may be present if [encryption of secrets at rest is enabled](key-encryption-at-rest.md#data-protection-implementation-key-encryption-at-rest).</span></span> <span data-ttu-id="91a11-127">DecryptorType atrybut będzie kwalifikowaną dla zestawu nazwę typu, która implementuje IXmlDecryptor.</span><span class="sxs-lookup"><span data-stu-id="91a11-127">The attribute decryptorType will be the assembly-qualified name of a type which implements IXmlDecryptor.</span></span> <span data-ttu-id="91a11-128">Ten typ jest odpowiedzialny za odczytywanie wewnętrzny <encryptedKey> elementu i odszyfrowywania, aby odzyskać oryginalny zwykły tekst.</span><span class="sxs-lookup"><span data-stu-id="91a11-128">This type is responsible for reading the inner <encryptedKey> element and decrypting it to recover the original plaintext.</span></span>

<span data-ttu-id="91a11-129">Jak \<deskryptora >, określonego formatu <encryptedSecret> element jest zależny od mechanizm szyfrowania na rest w użyciu.</span><span class="sxs-lookup"><span data-stu-id="91a11-129">As with \<descriptor>, the particular format of the <encryptedSecret> element depends on the at-rest encryption mechanism in use.</span></span> <span data-ttu-id="91a11-130">W powyższym przykładzie klucza głównego są szyfrowane przy użyciu interfejsu DPAPI systemu Windows na komentarz.</span><span class="sxs-lookup"><span data-stu-id="91a11-130">In the above example, the master key is encrypted using Windows DPAPI per the comment.</span></span>

## <a name="the-revocation-element"></a><span data-ttu-id="91a11-131">\<Odwołania > — element</span><span class="sxs-lookup"><span data-stu-id="91a11-131">The \<revocation> element</span></span>

<span data-ttu-id="91a11-132">Odwołania istnieje jako obiekty najwyższego poziomu w repozytorium klucza.</span><span class="sxs-lookup"><span data-stu-id="91a11-132">Revocations exist as top-level objects in the key repository.</span></span> <span data-ttu-id="91a11-133">Konwencja odwołania ma nazwę pliku **odwołania-{sygnatury czasowej} .xml** (dla cofnięcie wszystkich kluczy przed upływem określonego terminu) lub **odwołania-{guid} .xml** (dla odwołania określonego klucza).</span><span class="sxs-lookup"><span data-stu-id="91a11-133">By convention revocations have the filename **revocation-{timestamp}.xml** (for revoking all keys before a specific date) or **revocation-{guid}.xml** (for revoking a specific key).</span></span> <span data-ttu-id="91a11-134">Każdy plik zawiera jeden \<odwołania > elementu.</span><span class="sxs-lookup"><span data-stu-id="91a11-134">Each file contains a single \<revocation> element.</span></span>

<span data-ttu-id="91a11-135">Dla odwołania poszczególnych kluczy, będzie zawartość pliku jak poniżej.</span><span class="sxs-lookup"><span data-stu-id="91a11-135">For revocations of individual keys, the file contents will be as below.</span></span>

```xml
<?xml version="1.0" encoding="utf-8"?>
<revocation version="1">
  <revocationDate>2015-03-20T22:45:30.2616742Z</revocationDate>
  <key id="eb4fc299-8808-409d-8a34-23fc83d026c9" />
  <reason>human-readable reason</reason>
</revocation>
```

<span data-ttu-id="91a11-136">W takim przypadku tylko określony klucz został odwołany.</span><span class="sxs-lookup"><span data-stu-id="91a11-136">In this case, only the specified key is revoked.</span></span> <span data-ttu-id="91a11-137">Jeśli identyfikator klucza to "\*", jednak podobnie jak w poniższym przykładzie wszystkie klucze, których data utworzenia jest wcześniejsza od daty określonego odwołania są odwołane.</span><span class="sxs-lookup"><span data-stu-id="91a11-137">If the key id is "\*", however, as in the below example, all keys whose creation date is prior to the specified revocation date are revoked.</span></span>

```xml
<?xml version="1.0" encoding="utf-8"?>
<revocation version="1">
  <revocationDate>2015-03-20T15:45:45.7366491-07:00</revocationDate>
  <!-- All keys created before the revocation date are revoked. -->
  <key id="*" />
  <reason>human-readable reason</reason>
</revocation>
```

<span data-ttu-id="91a11-138">\<Przyczyna > element nigdy nie jest do odczytu przez system.</span><span class="sxs-lookup"><span data-stu-id="91a11-138">The \<reason> element is never read by the system.</span></span> <span data-ttu-id="91a11-139">Jest po prostu wygodne miejsce do przechowywania czytelny dla człowieka przyczynę odwołania.</span><span class="sxs-lookup"><span data-stu-id="91a11-139">It is simply a convenient place to store a human-readable reason for revocation.</span></span>
