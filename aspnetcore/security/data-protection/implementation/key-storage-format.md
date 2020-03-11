---
title: Format magazynu kluczy w ASP.NET Core
author: rick-anderson
description: Zapoznaj się ze szczegółami implementacji formatu magazynu kluczy ASP.NET Core ochrony danych.
ms.author: riande
ms.date: 10/14/2016
uid: security/data-protection/implementation/key-storage-format
ms.openlocfilehash: 81df124f3dd0cadf8fd895ab55f66eec6415705f
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78667756"
---
# <a name="key-storage-format-in-aspnet-core"></a><span data-ttu-id="3ea8f-103">Format magazynu kluczy w ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="3ea8f-103">Key storage format in ASP.NET Core</span></span>

<a name="data-protection-implementation-key-storage-format"></a>

<span data-ttu-id="3ea8f-104">Obiekty są przechowywane w reprezentacji XML.</span><span class="sxs-lookup"><span data-stu-id="3ea8f-104">Objects are stored at rest in XML representation.</span></span> <span data-ttu-id="3ea8f-105">Domyślnym katalogiem magazynu kluczy jest%LOCALAPPDATA%\ASP.NET\DataProtection-Keys\.</span><span class="sxs-lookup"><span data-stu-id="3ea8f-105">The default directory for key storage is %LOCALAPPDATA%\ASP.NET\DataProtection-Keys\.</span></span>

## <a name="the-key-element"></a><span data-ttu-id="3ea8f-106">> Klucz \<</span><span class="sxs-lookup"><span data-stu-id="3ea8f-106">The \<key> element</span></span>

<span data-ttu-id="3ea8f-107">Klucze istnieją jako obiekty najwyższego poziomu w repozytorium kluczy.</span><span class="sxs-lookup"><span data-stu-id="3ea8f-107">Keys exist as top-level objects in the key repository.</span></span> <span data-ttu-id="3ea8f-108">Klucze konwencji mają **klucz filename-{GUID}. XML**, gdzie {GUID} jest identyfikatorem klucza.</span><span class="sxs-lookup"><span data-stu-id="3ea8f-108">By convention keys have the filename **key-{guid}.xml**, where {guid} is the id of the key.</span></span> <span data-ttu-id="3ea8f-109">Każdy taki plik zawiera jeden klucz.</span><span class="sxs-lookup"><span data-stu-id="3ea8f-109">Each such file contains a single key.</span></span> <span data-ttu-id="3ea8f-110">Format pliku jest następujący.</span><span class="sxs-lookup"><span data-stu-id="3ea8f-110">The format of the file is as follows.</span></span>

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

<span data-ttu-id="3ea8f-111">Element > \<klucz zawiera następujące atrybuty i elementy podrzędne:</span><span class="sxs-lookup"><span data-stu-id="3ea8f-111">The \<key> element contains the following attributes and child elements:</span></span>

* <span data-ttu-id="3ea8f-112">Identyfikator klucza. Ta wartość jest traktowana jako autorytatywna; Nazwa pliku jest po prostu Nicety do czytelności.</span><span class="sxs-lookup"><span data-stu-id="3ea8f-112">The key id. This value is treated as authoritative; the filename is simply a nicety for human readability.</span></span>

* <span data-ttu-id="3ea8f-113">Wersja klucza \<> elementu, obecnie ustalona na 1.</span><span class="sxs-lookup"><span data-stu-id="3ea8f-113">The version of the \<key> element, currently fixed at 1.</span></span>

* <span data-ttu-id="3ea8f-114">Data utworzenia, aktywacji i wygaśnięcia klucza.</span><span class="sxs-lookup"><span data-stu-id="3ea8f-114">The key's creation, activation, and expiration dates.</span></span>

* <span data-ttu-id="3ea8f-115">Deskryptor \<> element, który zawiera informacje dotyczące uwierzytelnionej implementacji szyfrowania zawartej w tym kluczu.</span><span class="sxs-lookup"><span data-stu-id="3ea8f-115">A \<descriptor> element, which contains information on the authenticated encryption implementation contained within this key.</span></span>

<span data-ttu-id="3ea8f-116">W powyższym przykładzie identyfikator klucza to {80732141-ec8f-4b80-af9c-c4d2d1ff8901}, został utworzony i aktywowany w 19 marca 2015 i ma okres istnienia 90 dni.</span><span class="sxs-lookup"><span data-stu-id="3ea8f-116">In the above example, the key's id is {80732141-ec8f-4b80-af9c-c4d2d1ff8901}, it was created and activated on March 19, 2015, and it has a lifetime of 90 days.</span></span> <span data-ttu-id="3ea8f-117">(Czasami Data aktywacji może być nieco wcześniejsza niż data utworzenia, jak w tym przykładzie.</span><span class="sxs-lookup"><span data-stu-id="3ea8f-117">(Occasionally the activation date might be slightly before the creation date as in this example.</span></span> <span data-ttu-id="3ea8f-118">Wynika to z drobne, w jaki sposób interfejsy API działają i są nieszkodliwe w użyciu.)</span><span class="sxs-lookup"><span data-stu-id="3ea8f-118">This is due to a nit in how the APIs work and is harmless in practice.)</span></span>

## <a name="the-descriptor-element"></a><span data-ttu-id="3ea8f-119">\<deskryptora > elementu</span><span class="sxs-lookup"><span data-stu-id="3ea8f-119">The \<descriptor> element</span></span>

<span data-ttu-id="3ea8f-120">\<deskryptora zewnętrznego > elementu zawiera atrybut deserializatortype, który jest kwalifikowana dla zestawu nazwa typu, który implementuje IAuthenticatedEncryptorDescriptorDeserializer.</span><span class="sxs-lookup"><span data-stu-id="3ea8f-120">The outer \<descriptor> element contains an attribute deserializerType, which is the assembly-qualified name of a type which implements IAuthenticatedEncryptorDescriptorDeserializer.</span></span> <span data-ttu-id="3ea8f-121">Ten typ jest odpowiedzialny za odczytywanie wewnętrznego deskryptora \<> elementu i na potrzeby analizy zawartych w nim informacji.</span><span class="sxs-lookup"><span data-stu-id="3ea8f-121">This type is responsible for reading the inner \<descriptor> element and for parsing the information contained within.</span></span>

<span data-ttu-id="3ea8f-122">Określony format elementu > deskryptora \<zależy od uwierzytelnionej implementacji modułu szyfrującego hermetyzowanego przez klucz, a każdy typ deserializacji oczekuje nieco innego formatu.</span><span class="sxs-lookup"><span data-stu-id="3ea8f-122">The particular format of the \<descriptor> element depends on the authenticated encryptor implementation encapsulated by the key, and each deserializer type expects a slightly different format for this.</span></span> <span data-ttu-id="3ea8f-123">Ogólnie rzecz biorąc, ten element będzie zawierał informacje o algorytmach (nazwy, typy, identyfikatory OID lub podobne) i materiał klucza tajnego.</span><span class="sxs-lookup"><span data-stu-id="3ea8f-123">In general, though, this element will contain algorithmic information (names, types, OIDs, or similar) and secret key material.</span></span> <span data-ttu-id="3ea8f-124">W powyższym przykładzie deskryptor określa, że ten klucz otacza algorytm AES-256-CBC szyfrowanie i HMACSHA256 weryfikację.</span><span class="sxs-lookup"><span data-stu-id="3ea8f-124">In the above example, the descriptor specifies that this key wraps AES-256-CBC encryption + HMACSHA256 validation.</span></span>

## <a name="the-encryptedsecret-element"></a><span data-ttu-id="3ea8f-125">Element \<encryptedSecret ></span><span class="sxs-lookup"><span data-stu-id="3ea8f-125">The \<encryptedSecret> element</span></span>

<span data-ttu-id="3ea8f-126">Element **&lt;encryptedSecret&gt;** , który zawiera zaszyfrowaną formę materiału klucza tajnego, może być obecny, jeśli [włączono szyfrowanie wpisów tajnych w stanie spoczynku](xref:security/data-protection/implementation/key-encryption-at-rest).</span><span class="sxs-lookup"><span data-stu-id="3ea8f-126">An **&lt;encryptedSecret&gt;** element which contains the encrypted form of the secret key material may be present if [encryption of secrets at rest is enabled](xref:security/data-protection/implementation/key-encryption-at-rest).</span></span> <span data-ttu-id="3ea8f-127">Atrybut `decryptorType` to kwalifikowana dla zestawu nazwa typu, który implementuje [IXmlDecryptor](/dotnet/api/microsoft.aspnetcore.dataprotection.xmlencryption.ixmldecryptor).</span><span class="sxs-lookup"><span data-stu-id="3ea8f-127">The attribute `decryptorType` is the assembly-qualified name of a type which implements [IXmlDecryptor](/dotnet/api/microsoft.aspnetcore.dataprotection.xmlencryption.ixmldecryptor).</span></span> <span data-ttu-id="3ea8f-128">Ten typ jest odpowiedzialny za odczytywanie wewnętrznego **&lt;encryptedKey&gt;** elementu i odszyfrowywanie go w celu odzyskania oryginalnego tekstu.</span><span class="sxs-lookup"><span data-stu-id="3ea8f-128">This type is responsible for reading the inner **&lt;encryptedKey&gt;** element and decrypting it to recover the original plaintext.</span></span>

<span data-ttu-id="3ea8f-129">Podobnie jak w przypadku `<descriptor>`, konkretny format elementu `<encryptedSecret>` zależy od mechanizmu szyfrowania w trybie Rest.</span><span class="sxs-lookup"><span data-stu-id="3ea8f-129">As with `<descriptor>`, the particular format of the `<encryptedSecret>` element depends on the at-rest encryption mechanism in use.</span></span> <span data-ttu-id="3ea8f-130">W powyższym przykładzie klucz główny jest szyfrowany przy użyciu funkcji DPAPI systemu Windows na komentarz.</span><span class="sxs-lookup"><span data-stu-id="3ea8f-130">In the above example, the master key is encrypted using Windows DPAPI per the comment.</span></span>

## <a name="the-revocation-element"></a><span data-ttu-id="3ea8f-131">Element > \<odwoływania</span><span class="sxs-lookup"><span data-stu-id="3ea8f-131">The \<revocation> element</span></span>

<span data-ttu-id="3ea8f-132">Odwołania istnieją jako obiekty najwyższego poziomu w repozytorium kluczy.</span><span class="sxs-lookup"><span data-stu-id="3ea8f-132">Revocations exist as top-level objects in the key repository.</span></span> <span data-ttu-id="3ea8f-133">Odwołania do Konwencji mają odwołania do nazwy pliku **-{timestamp}. XML** (do odwoływania wszystkich kluczy przed określoną datą) lub **odwołania-{GUID}. XML** (na potrzeby odwoływania określonego klucza).</span><span class="sxs-lookup"><span data-stu-id="3ea8f-133">By convention revocations have the filename **revocation-{timestamp}.xml** (for revoking all keys before a specific date) or **revocation-{guid}.xml** (for revoking a specific key).</span></span> <span data-ttu-id="3ea8f-134">Każdy plik zawiera jeden \<element > odwołania.</span><span class="sxs-lookup"><span data-stu-id="3ea8f-134">Each file contains a single \<revocation> element.</span></span>

<span data-ttu-id="3ea8f-135">W przypadku odwołań poszczególnych kluczy zawartość pliku będzie następująca.</span><span class="sxs-lookup"><span data-stu-id="3ea8f-135">For revocations of individual keys, the file contents will be as below.</span></span>

```xml
<?xml version="1.0" encoding="utf-8"?>
<revocation version="1">
  <revocationDate>2015-03-20T22:45:30.2616742Z</revocationDate>
  <key id="eb4fc299-8808-409d-8a34-23fc83d026c9" />
  <reason>human-readable reason</reason>
</revocation>
```

<span data-ttu-id="3ea8f-136">W takim przypadku tylko określony klucz jest odwołany.</span><span class="sxs-lookup"><span data-stu-id="3ea8f-136">In this case, only the specified key is revoked.</span></span> <span data-ttu-id="3ea8f-137">Jeśli identyfikator klucza to "\*", jednak jak w poniższym przykładzie, wszystkie klucze, których data utworzenia jest wcześniejsza niż określona data odwołania, zostaną odwołane.</span><span class="sxs-lookup"><span data-stu-id="3ea8f-137">If the key id is "\*", however, as in the below example, all keys whose creation date is prior to the specified revocation date are revoked.</span></span>

```xml
<?xml version="1.0" encoding="utf-8"?>
<revocation version="1">
  <revocationDate>2015-03-20T15:45:45.7366491-07:00</revocationDate>
  <!-- All keys created before the revocation date are revoked. -->
  <key id="*" />
  <reason>human-readable reason</reason>
</revocation>
```

<span data-ttu-id="3ea8f-138">Element > \<przyczyna nie jest nigdy odczytywany przez system.</span><span class="sxs-lookup"><span data-stu-id="3ea8f-138">The \<reason> element is never read by the system.</span></span> <span data-ttu-id="3ea8f-139">Jest to po prostu wygodne miejsce do przechowywania przyczyny odwołania przez człowieka.</span><span class="sxs-lookup"><span data-stu-id="3ea8f-139">It's simply a convenient place to store a human-readable reason for revocation.</span></span>
