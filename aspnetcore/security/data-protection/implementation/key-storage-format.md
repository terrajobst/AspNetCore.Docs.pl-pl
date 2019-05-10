---
title: Format magazynu kluczy w programie ASP.NET Core
author: rick-anderson
description: Dowiedz się, szczegóły implementacji format magazynu kluczy ochrony danych programu ASP.NET Core.
ms.author: riande
ms.date: 10/14/2016
uid: security/data-protection/implementation/key-storage-format
ms.openlocfilehash: 81df124f3dd0cadf8fd895ab55f66eec6415705f
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/27/2019
ms.locfileid: "64903055"
---
# <a name="key-storage-format-in-aspnet-core"></a><span data-ttu-id="04136-103">Format magazynu kluczy w programie ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="04136-103">Key storage format in ASP.NET Core</span></span>

<a name="data-protection-implementation-key-storage-format"></a>

<span data-ttu-id="04136-104">Obiekty są magazynowane w reprezentacji XML.</span><span class="sxs-lookup"><span data-stu-id="04136-104">Objects are stored at rest in XML representation.</span></span> <span data-ttu-id="04136-105">Domyślny katalog dla magazynu kluczy wynosi % LOCALAPPDATA%\ASP.NET\DataProtection-Keys\.</span><span class="sxs-lookup"><span data-stu-id="04136-105">The default directory for key storage is %LOCALAPPDATA%\ASP.NET\DataProtection-Keys\.</span></span>

## <a name="the-key-element"></a><span data-ttu-id="04136-106">\<Key > elementu</span><span class="sxs-lookup"><span data-stu-id="04136-106">The \<key> element</span></span>

<span data-ttu-id="04136-107">Klucze istnieją jako obiekty najwyższego poziomu w repozytorium klucza.</span><span class="sxs-lookup"><span data-stu-id="04136-107">Keys exist as top-level objects in the key repository.</span></span> <span data-ttu-id="04136-108">Zgodnie z Konwencją klucze mają nazwę pliku **key-{guid} .xml**, gdzie {guid} jest identyfikatora klucza.</span><span class="sxs-lookup"><span data-stu-id="04136-108">By convention keys have the filename **key-{guid}.xml**, where {guid} is the id of the key.</span></span> <span data-ttu-id="04136-109">Każdy taki plik zawiera jeden klucz.</span><span class="sxs-lookup"><span data-stu-id="04136-109">Each such file contains a single key.</span></span> <span data-ttu-id="04136-110">Format pliku jest w następujący sposób.</span><span class="sxs-lookup"><span data-stu-id="04136-110">The format of the file is as follows.</span></span>

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

<span data-ttu-id="04136-111">\<Key > element zawiera następujące atrybuty i elementy podrzędne:</span><span class="sxs-lookup"><span data-stu-id="04136-111">The \<key> element contains the following attributes and child elements:</span></span>

* <span data-ttu-id="04136-112">Identyfikator klucza. Ta wartość jest traktowana jako autorytatywne; Nazwa pliku jest po prostu nicety dla poprawia czytelność dla ludzi.</span><span class="sxs-lookup"><span data-stu-id="04136-112">The key id. This value is treated as authoritative; the filename is simply a nicety for human readability.</span></span>

* <span data-ttu-id="04136-113">Wersja \<key > elementu, obecnie jest ustalony na 1.</span><span class="sxs-lookup"><span data-stu-id="04136-113">The version of the \<key> element, currently fixed at 1.</span></span>

* <span data-ttu-id="04136-114">Klucz daty utworzenia, aktywacji i wygaśnięcia.</span><span class="sxs-lookup"><span data-stu-id="04136-114">The key's creation, activation, and expiration dates.</span></span>

* <span data-ttu-id="04136-115">A \<deskryptora > element, który zawiera informacje na temat wdrażania uwierzytelnione szyfrowanie znajdujących się w ramach tego klucza.</span><span class="sxs-lookup"><span data-stu-id="04136-115">A \<descriptor> element, which contains information on the authenticated encryption implementation contained within this key.</span></span>

<span data-ttu-id="04136-116">W powyższym przykładzie identyfikator klucza jest {80732141-ec8f-4b80-af9c-c4d2d1ff8901}, została utworzona i uaktywniona 19 marca 2015 r. i ma okres istnienia 90 dni.</span><span class="sxs-lookup"><span data-stu-id="04136-116">In the above example, the key's id is {80732141-ec8f-4b80-af9c-c4d2d1ff8901}, it was created and activated on March 19, 2015, and it has a lifetime of 90 days.</span></span> <span data-ttu-id="04136-117">(Czasami Data aktywacji może być nieco przed Data utworzenia, jak w poniższym przykładzie.</span><span class="sxs-lookup"><span data-stu-id="04136-117">(Occasionally the activation date might be slightly before the creation date as in this example.</span></span> <span data-ttu-id="04136-118">Jest to spowodowane nNie, jak działają interfejsy API, i jest nieszkodliwe w praktyce.)</span><span class="sxs-lookup"><span data-stu-id="04136-118">This is due to a nit in how the APIs work and is harmless in practice.)</span></span>

## <a name="the-descriptor-element"></a><span data-ttu-id="04136-119">\<Deskryptora > element</span><span class="sxs-lookup"><span data-stu-id="04136-119">The \<descriptor> element</span></span>

<span data-ttu-id="04136-120">Zewnętrzny \<deskryptora > element zawiera deserializerType atrybutu, która jest nazwą kwalifikowaną dla zestawu typu, która implementuje IAuthenticatedEncryptorDescriptorDeserializer.</span><span class="sxs-lookup"><span data-stu-id="04136-120">The outer \<descriptor> element contains an attribute deserializerType, which is the assembly-qualified name of a type which implements IAuthenticatedEncryptorDescriptorDeserializer.</span></span> <span data-ttu-id="04136-121">Ten typ jest odpowiedzialna za odczytywanie wewnętrzny \<deskryptora > element i analizowania informacji zawartych w.</span><span class="sxs-lookup"><span data-stu-id="04136-121">This type is responsible for reading the inner \<descriptor> element and for parsing the information contained within.</span></span>

<span data-ttu-id="04136-122">Format określonego \<deskryptora > element zależy od implementacji uwierzytelnionego modułu szyfrującego hermetyzowane za pomocą klucza i każdego typu Deserializator oczekuje nieco inny format, w tym.</span><span class="sxs-lookup"><span data-stu-id="04136-122">The particular format of the \<descriptor> element depends on the authenticated encryptor implementation encapsulated by the key, and each deserializer type expects a slightly different format for this.</span></span> <span data-ttu-id="04136-123">Ogólnie rzecz biorąc, jednak ten element będzie zawierać informacje konsolidatorze (nazw, typów, identyfikatory OID, lub podobny) i kluczy tajnych.</span><span class="sxs-lookup"><span data-stu-id="04136-123">In general, though, this element will contain algorithmic information (names, types, OIDs, or similar) and secret key material.</span></span> <span data-ttu-id="04136-124">W powyższym przykładzie deskryptor Określa, że ten klucz ma być zawijany szyfrowania AES-256-CBC + HMACSHA256 sprawdzania poprawności.</span><span class="sxs-lookup"><span data-stu-id="04136-124">In the above example, the descriptor specifies that this key wraps AES-256-CBC encryption + HMACSHA256 validation.</span></span>

## <a name="the-encryptedsecret-element"></a><span data-ttu-id="04136-125">\<EncryptedSecret > element</span><span class="sxs-lookup"><span data-stu-id="04136-125">The \<encryptedSecret> element</span></span>

<span data-ttu-id="04136-126">**&lt;EncryptedSecret&gt;** element, który zawiera zaszyfrowane materiału klucza tajnego klucza mogą występować Jeśli [jest włączone szyfrowanie kluczy tajnych przechowywanych](xref:security/data-protection/implementation/key-encryption-at-rest).</span><span class="sxs-lookup"><span data-stu-id="04136-126">An **&lt;encryptedSecret&gt;** element which contains the encrypted form of the secret key material may be present if [encryption of secrets at rest is enabled](xref:security/data-protection/implementation/key-encryption-at-rest).</span></span> <span data-ttu-id="04136-127">Ten atrybut `decryptorType` jest nazwą kwalifikowaną dla zestawu typu, która implementuje [IXmlDecryptor](/dotnet/api/microsoft.aspnetcore.dataprotection.xmlencryption.ixmldecryptor).</span><span class="sxs-lookup"><span data-stu-id="04136-127">The attribute `decryptorType` is the assembly-qualified name of a type which implements [IXmlDecryptor](/dotnet/api/microsoft.aspnetcore.dataprotection.xmlencryption.ixmldecryptor).</span></span> <span data-ttu-id="04136-128">Ten typ jest odpowiedzialna za odczytywanie wewnętrzny **&lt;encryptedKey&gt;** elementu i odszyfrowuje go, aby odzyskać oryginalnej postaci zwykłego tekstu.</span><span class="sxs-lookup"><span data-stu-id="04136-128">This type is responsible for reading the inner **&lt;encryptedKey&gt;** element and decrypting it to recover the original plaintext.</span></span>

<span data-ttu-id="04136-129">Podobnie jak w przypadku `<descriptor>`, format określonego `<encryptedSecret>` elementu jest zależna od mechanizm szyfrowania podczas spoczynku w użyciu.</span><span class="sxs-lookup"><span data-stu-id="04136-129">As with `<descriptor>`, the particular format of the `<encryptedSecret>` element depends on the at-rest encryption mechanism in use.</span></span> <span data-ttu-id="04136-130">W powyższym przykładzie klucz główny są szyfrowane przy użyciu interfejsu DPAPI Windows na komentarz.</span><span class="sxs-lookup"><span data-stu-id="04136-130">In the above example, the master key is encrypted using Windows DPAPI per the comment.</span></span>

## <a name="the-revocation-element"></a><span data-ttu-id="04136-131">\<Odwołania > element</span><span class="sxs-lookup"><span data-stu-id="04136-131">The \<revocation> element</span></span>

<span data-ttu-id="04136-132">Odwołania istnieje jako obiekty najwyższego poziomu w repozytorium klucza.</span><span class="sxs-lookup"><span data-stu-id="04136-132">Revocations exist as top-level objects in the key repository.</span></span> <span data-ttu-id="04136-133">Według Konwencji odwołania ma nazwę pliku **odwołania — {sygnatura czasowa} .xml** (na cofnięcie wszystkich kluczy przed określoną datę) lub **odwołania — {guid} .xml** (na potrzeby odwoływanie określonego klucza).</span><span class="sxs-lookup"><span data-stu-id="04136-133">By convention revocations have the filename **revocation-{timestamp}.xml** (for revoking all keys before a specific date) or **revocation-{guid}.xml** (for revoking a specific key).</span></span> <span data-ttu-id="04136-134">Każdy plik zawiera pojedynczy \<odwołania > element.</span><span class="sxs-lookup"><span data-stu-id="04136-134">Each file contains a single \<revocation> element.</span></span>

<span data-ttu-id="04136-135">W przypadku odwołania poszczególnych kluczy, będzie zawartość pliku poniżej.</span><span class="sxs-lookup"><span data-stu-id="04136-135">For revocations of individual keys, the file contents will be as below.</span></span>

```xml
<?xml version="1.0" encoding="utf-8"?>
<revocation version="1">
  <revocationDate>2015-03-20T22:45:30.2616742Z</revocationDate>
  <key id="eb4fc299-8808-409d-8a34-23fc83d026c9" />
  <reason>human-readable reason</reason>
</revocation>
```

<span data-ttu-id="04136-136">W tym przypadku została odwołana tylko przy użyciu określonego klucza.</span><span class="sxs-lookup"><span data-stu-id="04136-136">In this case, only the specified key is revoked.</span></span> <span data-ttu-id="04136-137">Jeśli identyfikator klucza to "\*", jednak jak w poniższym przykładzie, wszystkie klucze, w których data utworzenia jest przed datą określonego odwołania zostaną odwołane.</span><span class="sxs-lookup"><span data-stu-id="04136-137">If the key id is "\*", however, as in the below example, all keys whose creation date is prior to the specified revocation date are revoked.</span></span>

```xml
<?xml version="1.0" encoding="utf-8"?>
<revocation version="1">
  <revocationDate>2015-03-20T15:45:45.7366491-07:00</revocationDate>
  <!-- All keys created before the revocation date are revoked. -->
  <key id="*" />
  <reason>human-readable reason</reason>
</revocation>
```

<span data-ttu-id="04136-138">\<Przyczyna > element nie jest nigdy odczytywana przez system.</span><span class="sxs-lookup"><span data-stu-id="04136-138">The \<reason> element is never read by the system.</span></span> <span data-ttu-id="04136-139">Jest po prostu wygodne miejsce do przechowywania czytelny dla człowieka przyczynę odwołania.</span><span class="sxs-lookup"><span data-stu-id="04136-139">It's simply a convenient place to store a human-readable reason for revocation.</span></span>
