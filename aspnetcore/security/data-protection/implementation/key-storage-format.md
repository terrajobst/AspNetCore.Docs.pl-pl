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
# <a name="key-storage-format"></a>Format magazynu kluczy

<a name="data-protection-implementation-key-storage-format"></a>

Obiekty przechowywane są magazynowane w reprezentacji XML. Domyślny katalog do przechowywania klucza wynosi % LOCALAPPDATA%\ASP.NET\DataProtection-Keys\.

## <a name="the-key-element"></a>\<Klucza > — element

Klucze istnieje jako obiekty najwyższego poziomu w repozytorium klucza. Konwencja klucze mają nazwę pliku **klucza-{guid} .xml**, gdzie {guid} jest identyfikatora klucza. Każdy taki plik zawiera jeden klucz. Format pliku jest w następujący sposób.

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

\<Klucza > element zawiera następujące atrybuty i elementy podrzędne:

* Identyfikator klucza. Ta wartość jest traktowane jako autorytatywne; Nazwa pliku jest po prostu nicety dla człowieka czytelności.

* Wersja \<klucza > element obecnie ustalone na 1.

* Klucz daty utworzenia, aktywacji i wygaśnięcia.

* A \<deskryptora > element, który zawiera informacje dotyczące wykonania szyfrowania uwierzytelnionego zawarta w tym kluczu.

W powyższym przykładzie, identyfikator klucza jest {80732141-ec8f-4b80-af9c-c4d2d1ff8901}, została utworzona, a na 19 marca 2015 aktywowany i zawiera okresu istnienia 90 dni. (Czasami Data aktywacji mogą być nieco przed Data utworzenia, jak w poniższym przykładzie. Jest to spowodowane nNie jak interfejsy API działa i jest bezpieczne w praktyce.)

## <a name="the-descriptor-element"></a>\<Deskryptora > — element

Zewnętrznego \<deskryptora > element zawiera deserializerType atrybut, który jest nazwa kwalifikowana zestawu typu, który implementuje IAuthenticatedEncryptorDescriptorDeserializer. Ten typ jest odpowiedzialny za odczytywanie wewnętrzny \<deskryptora > elementu i do analizowania informacji zawartych w.

Format określonego \<deskryptora > elementu zależy od implementacji uwierzytelnionego modułu szyfrującego hermetyzowany za pomocą klucza i każdego typu Deserializator oczekuje nieco inny format tego. Ogólnie rzecz biorąc, jednak ten element będzie zawierać informacje algorytmicznego (nazwy, typy identyfikatorów OID, lub podobny) i kluczy tajnych. W powyższym przykładzie deskryptora Określa, że ten klucz zawijany szyfrowania AES-256-CBC + HMACSHA256 weryfikacji.

## <a name="the-encryptedsecret-element"></a>\<EncryptedSecret > — element

<encryptedSecret> Element, który zawiera zaszyfrowane materiału klucza tajnego mogą występować Jeśli [jest włączone szyfrowanie kluczy tajnych magazynowane](key-encryption-at-rest.md#data-protection-implementation-key-encryption-at-rest). DecryptorType atrybut będzie kwalifikowaną dla zestawu nazwę typu, która implementuje IXmlDecryptor. Ten typ jest odpowiedzialny za odczytywanie wewnętrzny <encryptedKey> elementu i odszyfrowywania, aby odzyskać oryginalny zwykły tekst.

Jak \<deskryptora >, określonego formatu <encryptedSecret> element jest zależny od mechanizm szyfrowania na rest w użyciu. W powyższym przykładzie klucza głównego są szyfrowane przy użyciu interfejsu DPAPI systemu Windows na komentarz.

## <a name="the-revocation-element"></a>\<Odwołania > — element

Odwołania istnieje jako obiekty najwyższego poziomu w repozytorium klucza. Konwencja odwołania ma nazwę pliku **odwołania-{sygnatury czasowej} .xml** (dla cofnięcie wszystkich kluczy przed upływem określonego terminu) lub **odwołania-{guid} .xml** (dla odwołania określonego klucza). Każdy plik zawiera jeden \<odwołania > elementu.

Dla odwołania poszczególnych kluczy, będzie zawartość pliku jak poniżej.

```xml
<?xml version="1.0" encoding="utf-8"?>
<revocation version="1">
  <revocationDate>2015-03-20T22:45:30.2616742Z</revocationDate>
  <key id="eb4fc299-8808-409d-8a34-23fc83d026c9" />
  <reason>human-readable reason</reason>
</revocation>
```

W takim przypadku tylko określony klucz został odwołany. Jeśli identyfikator klucza to "*", jednak podobnie jak w poniższym przykładzie wszystkie klucze, których data utworzenia jest wcześniejsza od daty określonego odwołania są odwołane.

```xml
<?xml version="1.0" encoding="utf-8"?>
<revocation version="1">
  <revocationDate>2015-03-20T15:45:45.7366491-07:00</revocationDate>
  <!-- All keys created before the revocation date are revoked. -->
  <key id="*" />
  <reason>human-readable reason</reason>
</revocation>
```

\<Przyczyna > element nigdy nie jest do odczytu przez system. Jest po prostu wygodne miejsce do przechowywania czytelny dla człowieka przyczynę odwołania.
