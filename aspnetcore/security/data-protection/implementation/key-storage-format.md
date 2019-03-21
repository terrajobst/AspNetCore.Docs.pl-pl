---
title: Format magazynu kluczy w programie ASP.NET Core
author: rick-anderson
description: Dowiedz się, szczegóły implementacji format magazynu kluczy ochrony danych programu ASP.NET Core.
ms.author: riande
ms.date: 10/14/2016
uid: security/data-protection/implementation/key-storage-format
ms.openlocfilehash: 81df124f3dd0cadf8fd895ab55f66eec6415705f
ms.sourcegitcommit: 5f299daa7c8102d56a63b214b9a34cc4bc87bc42
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/19/2019
ms.locfileid: "58208021"
---
# <a name="key-storage-format-in-aspnet-core"></a>Format magazynu kluczy w programie ASP.NET Core

<a name="data-protection-implementation-key-storage-format"></a>

Obiekty są magazynowane w reprezentacji XML. Domyślny katalog dla magazynu kluczy wynosi % LOCALAPPDATA%\ASP.NET\DataProtection-Keys\.

## <a name="the-key-element"></a>\<Key > elementu

Klucze istnieją jako obiekty najwyższego poziomu w repozytorium klucza. Zgodnie z Konwencją klucze mają nazwę pliku **key-{guid} .xml**, gdzie {guid} jest identyfikatora klucza. Każdy taki plik zawiera jeden klucz. Format pliku jest w następujący sposób.

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

\<Key > element zawiera następujące atrybuty i elementy podrzędne:

* Identyfikator klucza. Ta wartość jest traktowana jako autorytatywne; Nazwa pliku jest po prostu nicety dla poprawia czytelność dla ludzi.

* Wersja \<key > elementu, obecnie jest ustalony na 1.

* Klucz daty utworzenia, aktywacji i wygaśnięcia.

* A \<deskryptora > element, który zawiera informacje na temat wdrażania uwierzytelnione szyfrowanie znajdujących się w ramach tego klucza.

W powyższym przykładzie identyfikator klucza jest {80732141-ec8f-4b80-af9c-c4d2d1ff8901}, została utworzona i uaktywniona 19 marca 2015 r. i ma okres istnienia 90 dni. (Czasami Data aktywacji może być nieco przed Data utworzenia, jak w poniższym przykładzie. Jest to spowodowane nNie, jak działają interfejsy API, i jest nieszkodliwe w praktyce.)

## <a name="the-descriptor-element"></a>\<Deskryptora > element

Zewnętrzny \<deskryptora > element zawiera deserializerType atrybutu, która jest nazwą kwalifikowaną dla zestawu typu, która implementuje IAuthenticatedEncryptorDescriptorDeserializer. Ten typ jest odpowiedzialna za odczytywanie wewnętrzny \<deskryptora > element i analizowania informacji zawartych w.

Format określonego \<deskryptora > element zależy od implementacji uwierzytelnionego modułu szyfrującego hermetyzowane za pomocą klucza i każdego typu Deserializator oczekuje nieco inny format, w tym. Ogólnie rzecz biorąc, jednak ten element będzie zawierać informacje konsolidatorze (nazw, typów, identyfikatory OID, lub podobny) i kluczy tajnych. W powyższym przykładzie deskryptor Określa, że ten klucz ma być zawijany szyfrowania AES-256-CBC + HMACSHA256 sprawdzania poprawności.

## <a name="the-encryptedsecret-element"></a>\<EncryptedSecret > element

**&lt;EncryptedSecret&gt;** element, który zawiera zaszyfrowane materiału klucza tajnego klucza mogą występować Jeśli [jest włączone szyfrowanie kluczy tajnych przechowywanych](xref:security/data-protection/implementation/key-encryption-at-rest). Ten atrybut `decryptorType` jest nazwą kwalifikowaną dla zestawu typu, która implementuje [IXmlDecryptor](/dotnet/api/microsoft.aspnetcore.dataprotection.xmlencryption.ixmldecryptor). Ten typ jest odpowiedzialna za odczytywanie wewnętrzny **&lt;encryptedKey&gt;** elementu i odszyfrowuje go, aby odzyskać oryginalnej postaci zwykłego tekstu.

Podobnie jak w przypadku `<descriptor>`, format określonego `<encryptedSecret>` elementu jest zależna od mechanizm szyfrowania podczas spoczynku w użyciu. W powyższym przykładzie klucz główny są szyfrowane przy użyciu interfejsu DPAPI Windows na komentarz.

## <a name="the-revocation-element"></a>\<Odwołania > element

Odwołania istnieje jako obiekty najwyższego poziomu w repozytorium klucza. Według Konwencji odwołania ma nazwę pliku **odwołania — {sygnatura czasowa} .xml** (na cofnięcie wszystkich kluczy przed określoną datę) lub **odwołania — {guid} .xml** (na potrzeby odwoływanie określonego klucza). Każdy plik zawiera pojedynczy \<odwołania > element.

W przypadku odwołania poszczególnych kluczy, będzie zawartość pliku poniżej.

```xml
<?xml version="1.0" encoding="utf-8"?>
<revocation version="1">
  <revocationDate>2015-03-20T22:45:30.2616742Z</revocationDate>
  <key id="eb4fc299-8808-409d-8a34-23fc83d026c9" />
  <reason>human-readable reason</reason>
</revocation>
```

W tym przypadku została odwołana tylko przy użyciu określonego klucza. Jeśli identyfikator klucza to "*", jednak jak w poniższym przykładzie, wszystkie klucze, w których data utworzenia jest przed datą określonego odwołania zostaną odwołane.

```xml
<?xml version="1.0" encoding="utf-8"?>
<revocation version="1">
  <revocationDate>2015-03-20T15:45:45.7366491-07:00</revocationDate>
  <!-- All keys created before the revocation date are revoked. -->
  <key id="*" />
  <reason>human-readable reason</reason>
</revocation>
```

\<Przyczyna > element nie jest nigdy odczytywana przez system. Jest po prostu wygodne miejsce do przechowywania czytelny dla człowieka przyczynę odwołania.
