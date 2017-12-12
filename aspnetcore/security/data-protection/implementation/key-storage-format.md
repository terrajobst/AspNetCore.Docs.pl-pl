---
title: Format magazynu kluczy
author: tdykstra
description: "W tym dokumencie opisano szczegóły implementacji format magazynu kluczy ochrony danych platformy ASP.NET Core."
keywords: Magazynu kluczy platformy ASP.NET Core, ochrony danych
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: e8996478-f7bf-4b58-bab4-7fdb5d8556c5
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/implementation/key-storage-format
ms.openlocfilehash: 4ebad05f7d55e954463ce5e277b419a7d6f773c0
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
# <a name="key-storage-format"></a><span data-ttu-id="36f6c-104">Format magazynu kluczy</span><span class="sxs-lookup"><span data-stu-id="36f6c-104">Key Storage Format</span></span>

<a name="data-protection-implementation-key-storage-format"></a>

<span data-ttu-id="36f6c-105">Obiekty przechowywane są magazynowane w reprezentacji XML.</span><span class="sxs-lookup"><span data-stu-id="36f6c-105">Objects are stored at rest in XML representation.</span></span> <span data-ttu-id="36f6c-106">Domyślny katalog do przechowywania klucza wynosi % LOCALAPPDATA%\ASP.NET\DataProtection-Keys\.</span><span class="sxs-lookup"><span data-stu-id="36f6c-106">The default directory for key storage is %LOCALAPPDATA%\ASP.NET\DataProtection-Keys\.</span></span>

## <a name="the-key-element"></a><span data-ttu-id="36f6c-107">\<Klucza > — element</span><span class="sxs-lookup"><span data-stu-id="36f6c-107">The \<key> element</span></span>

<span data-ttu-id="36f6c-108">Klucze istnieje jako obiekty najwyższego poziomu w repozytorium klucza.</span><span class="sxs-lookup"><span data-stu-id="36f6c-108">Keys exist as top-level objects in the key repository.</span></span> <span data-ttu-id="36f6c-109">Konwencja klucze mają nazwę pliku **klucza-{guid} .xml**, gdzie {guid} jest identyfikatora klucza.</span><span class="sxs-lookup"><span data-stu-id="36f6c-109">By convention keys have the filename **key-{guid}.xml**, where {guid} is the id of the key.</span></span> <span data-ttu-id="36f6c-110">Każdy taki plik zawiera jeden klucz.</span><span class="sxs-lookup"><span data-stu-id="36f6c-110">Each such file contains a single key.</span></span> <span data-ttu-id="36f6c-111">Format pliku jest w następujący sposób.</span><span class="sxs-lookup"><span data-stu-id="36f6c-111">The format of the file is as follows.</span></span>

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

<span data-ttu-id="36f6c-112">\<Klucza > element zawiera następujące atrybuty i elementy podrzędne:</span><span class="sxs-lookup"><span data-stu-id="36f6c-112">The \<key> element contains the following attributes and child elements:</span></span>

* <span data-ttu-id="36f6c-113">Identyfikator klucza. Ta wartość jest traktowane jako autorytatywne; Nazwa pliku jest po prostu nicety dla człowieka czytelności.</span><span class="sxs-lookup"><span data-stu-id="36f6c-113">The key id. This value is treated as authoritative; the filename is simply a nicety for human readability.</span></span>

* <span data-ttu-id="36f6c-114">Wersja \<klucza > element obecnie ustalone na 1.</span><span class="sxs-lookup"><span data-stu-id="36f6c-114">The version of the \<key> element, currently fixed at 1.</span></span>

* <span data-ttu-id="36f6c-115">Klucz daty utworzenia, aktywacji i wygaśnięcia.</span><span class="sxs-lookup"><span data-stu-id="36f6c-115">The key's creation, activation, and expiration dates.</span></span>

* <span data-ttu-id="36f6c-116">A \<deskryptora > element, który zawiera informacje dotyczące wykonania szyfrowania uwierzytelnionego zawarta w tym kluczu.</span><span class="sxs-lookup"><span data-stu-id="36f6c-116">A \<descriptor> element, which contains information on the authenticated encryption implementation contained within this key.</span></span>

<span data-ttu-id="36f6c-117">W powyższym przykładzie, identyfikator klucza jest {80732141-ec8f-4b80-af9c-c4d2d1ff8901}, została utworzona, a na 19 marca 2015 aktywowany i zawiera okresu istnienia 90 dni.</span><span class="sxs-lookup"><span data-stu-id="36f6c-117">In the above example, the key's id is {80732141-ec8f-4b80-af9c-c4d2d1ff8901}, it was created and activated on March 19, 2015, and it has a lifetime of 90 days.</span></span> <span data-ttu-id="36f6c-118">(Czasami Data aktywacji mogą być nieco przed Data utworzenia, jak w poniższym przykładzie.</span><span class="sxs-lookup"><span data-stu-id="36f6c-118">(Occasionally the activation date might be slightly before the creation date as in this example.</span></span> <span data-ttu-id="36f6c-119">Jest to spowodowane nNie jak interfejsy API działa i jest bezpieczne w praktyce.)</span><span class="sxs-lookup"><span data-stu-id="36f6c-119">This is due to a nit in how the APIs work and is harmless in practice.)</span></span>

## <a name="the-descriptor-element"></a><span data-ttu-id="36f6c-120">\<Deskryptora > — element</span><span class="sxs-lookup"><span data-stu-id="36f6c-120">The \<descriptor> element</span></span>

<span data-ttu-id="36f6c-121">Zewnętrznego \<deskryptora > element zawiera deserializerType atrybut, który jest nazwa kwalifikowana zestawu typu, który implementuje IAuthenticatedEncryptorDescriptorDeserializer.</span><span class="sxs-lookup"><span data-stu-id="36f6c-121">The outer \<descriptor> element contains an attribute deserializerType, which is the assembly-qualified name of a type which implements IAuthenticatedEncryptorDescriptorDeserializer.</span></span> <span data-ttu-id="36f6c-122">Ten typ jest odpowiedzialny za odczytywanie wewnętrzny \<deskryptora > elementu i do analizowania informacji zawartych w.</span><span class="sxs-lookup"><span data-stu-id="36f6c-122">This type is responsible for reading the inner \<descriptor> element and for parsing the information contained within.</span></span>

<span data-ttu-id="36f6c-123">Format określonego \<deskryptora > elementu zależy od implementacji uwierzytelnionego modułu szyfrującego hermetyzowany za pomocą klucza i każdego typu Deserializator oczekuje nieco inny format tego.</span><span class="sxs-lookup"><span data-stu-id="36f6c-123">The particular format of the \<descriptor> element depends on the authenticated encryptor implementation encapsulated by the key, and each deserializer type expects a slightly different format for this.</span></span> <span data-ttu-id="36f6c-124">Ogólnie rzecz biorąc, jednak ten element będzie zawierać informacje algorytmicznego (nazwy, typy identyfikatorów OID, lub podobny) i kluczy tajnych.</span><span class="sxs-lookup"><span data-stu-id="36f6c-124">In general, though, this element will contain algorithmic information (names, types, OIDs, or similar) and secret key material.</span></span> <span data-ttu-id="36f6c-125">W powyższym przykładzie deskryptora Określa, że ten klucz zawijany szyfrowania AES-256-CBC + HMACSHA256 weryfikacji.</span><span class="sxs-lookup"><span data-stu-id="36f6c-125">In the above example, the descriptor specifies that this key wraps AES-256-CBC encryption + HMACSHA256 validation.</span></span>

## <a name="the-encryptedsecret-element"></a><span data-ttu-id="36f6c-126">\<EncryptedSecret > — element</span><span class="sxs-lookup"><span data-stu-id="36f6c-126">The \<encryptedSecret> element</span></span>

<span data-ttu-id="36f6c-127"><encryptedSecret> Element, który zawiera zaszyfrowane materiału klucza tajnego mogą występować Jeśli [jest włączone szyfrowanie kluczy tajnych magazynowane](key-encryption-at-rest.md#data-protection-implementation-key-encryption-at-rest).</span><span class="sxs-lookup"><span data-stu-id="36f6c-127">An <encryptedSecret> element which contains the encrypted form of the secret key material may be present if [encryption of secrets at rest is enabled](key-encryption-at-rest.md#data-protection-implementation-key-encryption-at-rest).</span></span> <span data-ttu-id="36f6c-128">DecryptorType atrybut będzie kwalifikowaną dla zestawu nazwę typu, która implementuje IXmlDecryptor.</span><span class="sxs-lookup"><span data-stu-id="36f6c-128">The attribute decryptorType will be the assembly-qualified name of a type which implements IXmlDecryptor.</span></span> <span data-ttu-id="36f6c-129">Ten typ jest odpowiedzialny za odczytywanie wewnętrzny <encryptedKey> elementu i odszyfrowywania, aby odzyskać oryginalny zwykły tekst.</span><span class="sxs-lookup"><span data-stu-id="36f6c-129">This type is responsible for reading the inner <encryptedKey> element and decrypting it to recover the original plaintext.</span></span>

<span data-ttu-id="36f6c-130">Jak \<deskryptora >, określonego formatu <encryptedSecret> element jest zależny od mechanizm szyfrowania na rest w użyciu.</span><span class="sxs-lookup"><span data-stu-id="36f6c-130">As with \<descriptor>, the particular format of the <encryptedSecret> element depends on the at-rest encryption mechanism in use.</span></span> <span data-ttu-id="36f6c-131">W powyższym przykładzie klucza głównego są szyfrowane przy użyciu interfejsu DPAPI systemu Windows na komentarz.</span><span class="sxs-lookup"><span data-stu-id="36f6c-131">In the above example, the master key is encrypted using Windows DPAPI per the comment.</span></span>

## <a name="the-revocation-element"></a><span data-ttu-id="36f6c-132">\<Odwołania > — element</span><span class="sxs-lookup"><span data-stu-id="36f6c-132">The \<revocation> element</span></span>

<span data-ttu-id="36f6c-133">Odwołania istnieje jako obiekty najwyższego poziomu w repozytorium klucza.</span><span class="sxs-lookup"><span data-stu-id="36f6c-133">Revocations exist as top-level objects in the key repository.</span></span> <span data-ttu-id="36f6c-134">Konwencja odwołania ma nazwę pliku **odwołania-{sygnatury czasowej} .xml** (dla cofnięcie wszystkich kluczy przed upływem określonego terminu) lub **odwołania-{guid} .xml** (dla odwołania określonego klucza).</span><span class="sxs-lookup"><span data-stu-id="36f6c-134">By convention revocations have the filename **revocation-{timestamp}.xml** (for revoking all keys before a specific date) or **revocation-{guid}.xml** (for revoking a specific key).</span></span> <span data-ttu-id="36f6c-135">Każdy plik zawiera jeden \<odwołania > elementu.</span><span class="sxs-lookup"><span data-stu-id="36f6c-135">Each file contains a single \<revocation> element.</span></span>

<span data-ttu-id="36f6c-136">Dla odwołania poszczególnych kluczy, będzie zawartość pliku jak poniżej.</span><span class="sxs-lookup"><span data-stu-id="36f6c-136">For revocations of individual keys, the file contents will be as below.</span></span>

```xml
<?xml version="1.0" encoding="utf-8"?>
<revocation version="1">
  <revocationDate>2015-03-20T22:45:30.2616742Z</revocationDate>
  <key id="eb4fc299-8808-409d-8a34-23fc83d026c9" />
  <reason>human-readable reason</reason>
</revocation>
```

<span data-ttu-id="36f6c-137">W takim przypadku tylko określony klucz został odwołany.</span><span class="sxs-lookup"><span data-stu-id="36f6c-137">In this case, only the specified key is revoked.</span></span> <span data-ttu-id="36f6c-138">Jeśli identyfikator klucza to "*", jednak podobnie jak w poniższym przykładzie wszystkie klucze, których data utworzenia jest wcześniejsza od daty określonego odwołania są odwołane.</span><span class="sxs-lookup"><span data-stu-id="36f6c-138">If the key id is "*", however, as in the below example, all keys whose creation date is prior to the specified revocation date are revoked.</span></span>

```xml
<?xml version="1.0" encoding="utf-8"?>
<revocation version="1">
  <revocationDate>2015-03-20T15:45:45.7366491-07:00</revocationDate>
  <!-- All keys created before the revocation date are revoked. -->
  <key id="*" />
  <reason>human-readable reason</reason>
</revocation>
```

<span data-ttu-id="36f6c-139">\<Przyczyna > element nigdy nie jest do odczytu przez system.</span><span class="sxs-lookup"><span data-stu-id="36f6c-139">The \<reason> element is never read by the system.</span></span> <span data-ttu-id="36f6c-140">Jest po prostu wygodne miejsce do przechowywania czytelny dla człowieka przyczynę odwołania.</span><span class="sxs-lookup"><span data-stu-id="36f6c-140">It is simply a convenient place to store a human-readable reason for revocation.</span></span>
