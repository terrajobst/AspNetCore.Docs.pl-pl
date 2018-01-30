---
title: Ochrona danych w podstawowej platformy ASP.NET
author: rick-anderson
description: "Ten dokument służy jako spisu treści dla różnych tematów ochrony danych platformy ASP.NET Core."
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/data-protection/index
ms.openlocfilehash: b846fb7cb28eeceb8c0bdb47135e1cf014ae08a7
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/30/2018
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

  * [Udostępnianie plików cookie między aplikacjami](xref:security/data-protection/compatibility/cookie-sharing)

  * [Zastępowanie <machineKey> na platformie ASP.NET](xref:security/data-protection/compatibility/replacing-machinekey)
