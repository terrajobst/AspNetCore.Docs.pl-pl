---
title: Ochrona danych w podstawowej platformy ASP.NET
author: rick-anderson
description: "Ten dokument służy jako spisu treści dla różnych tematów ochrony danych platformy ASP.NET Core."
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/index
ms.openlocfilehash: 7bbd203a67b32032ba2ab82448a5fc9a495b52aa
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/19/2018
---
# <a name="data-protection-in-aspnet-core-consumer-apis-configuration-extensibility-apis-and-implementation"></a>Ochrona danych w ASP.NET Core: interfejsy API konsumenta, konfigurowanie, rozszerzalności interfejsów API i wdrażanie

* [Wprowadzenie do ochrony danych](introduction.md)

* [Wprowadzenie do interfejsów API ochrony danych](using-data-protection.md)

* [Interfejsy API przeznaczone dla klientów](consumer-apis/index.md)

  * [Omówienie interfejsów API przeznaczonych dla klientów](consumer-apis/overview.md)

  * [Ciągi celów](consumer-apis/purpose-strings.md)

  * [Hierarchia celów i obsługa wielu dzierżawców](consumer-apis/purpose-strings-multitenancy.md)

  * [Tworzenia skrótów haseł](consumer-apis/password-hashing.md)

  * [Ograniczanie okresu istnienia ładunków chronionych](consumer-apis/limited-lifetime-payloads.md)

  * [Wyłączanie ochrony ładunków, których klucze zostały odwołane](consumer-apis/dangerous-unprotect.md)

* [Konfiguracja](configuration/index.md)

  * [Konfigurowanie ochrony danych](configuration/overview.md)

  * [Ustawienia domyślne](configuration/default-settings.md)

  * [Zasady dla komputera](configuration/machine-wide-policy.md)

  * [Scenariusze z systemem innym niż Podpisane aware](configuration/non-di-scenarios.md)

* [Interfejsy API rozszerzalności](extensibility/index.md)

  * [Rozszerzalność kryptografii Core](extensibility/core-crypto.md)

  * [Rozszerzalność zarządzania kluczami](extensibility/key-management.md)

  * [Różne interfejsy API](extensibility/misc-apis.md)

* [Implementacja](implementation/index.md)

  * [Szczegóły uwierzytelnionego szyfrowania](implementation/authenticated-encryption-details.md)

  * [Wyprowadzanie podkluczy i uwierzytelnione szyfrowanie](implementation/subkeyderivation.md)

  * [Nagłówki kontekstu](implementation/context-headers.md)

  * [Zarządzanie kluczami](implementation/key-management.md)

  * [Dostawcy magazynu kluczy](implementation/key-storage-providers.md)

  * [Szyfrowanie kluczy podczas magazynowania](implementation/key-encryption-at-rest.md)

  * [Niezmienność kluczy i zmienianie ustawień](implementation/key-immutability.md)

  * [Format magazynu kluczy](implementation/key-storage-format.md)

  * [Dostawcy efemerycznej ochrony danych](implementation/key-storage-ephemeral.md)

* [Zgodność](compatibility/index.md)

  * [Udostępnianie plików cookie między aplikacjami](compatibility/cookie-sharing.md)

  * [Zastępowanie <machineKey> na platformie ASP.NET](compatibility/replacing-machinekey.md)
