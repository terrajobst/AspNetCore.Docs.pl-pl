---
title: Ochrona danych w podstawowej platformy ASP.NET
author: rick-anderson
description: Ten dokument służy jako spisu treści dla różnych tematów ochrony danych platformy ASP.NET Core.
ms.author: riande
ms.date: 10/14/2016
uid: security/data-protection/index
ms.openlocfilehash: 1c38c587956dd99078fa4386c2f4423d784abc60
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/20/2018
ms.locfileid: "36277127"
---
# <a name="data-protection-in-aspnet-core"></a>Ochrona danych w podstawowej platformy ASP.NET

* [Wprowadzenie do ochrony danych](xref:security/data-protection/introduction)

* [Wprowadzenie do interfejsów API ochrony danych](xref:security/data-protection/using-data-protection)

* [Interfejsy API przeznaczone dla klientów](xref:security/data-protection/consumer-apis/index)

  * [Omówienie interfejsów API przeznaczonych dla klientów](xref:security/data-protection/consumer-apis/overview)

  * [Ciągi celów](xref:security/data-protection/consumer-apis/purpose-strings)

  * [Hierarchia celów i obsługa wielu dzierżawców](xref:security/data-protection/consumer-apis/purpose-strings-multitenancy)

  * [Wyznaczanie skrótów haseł](xref:security/data-protection/consumer-apis/password-hashing)

  * [Ograniczanie okresu istnienia ładunków chronionych](xref:security/data-protection/consumer-apis/limited-lifetime-payloads)

  * [Wyłączanie ochrony ładunków, których klucze zostały odwołane](xref:security/data-protection/consumer-apis/dangerous-unprotect)

* [Konfiguracja](xref:security/data-protection/configuration/index)

  * [Konfigurowanie ochrony danych platformy ASP.NET Core](xref:security/data-protection/configuration/overview)

  * [Ustawienia domyślne](xref:security/data-protection/configuration/default-settings)

  * [Zasady dla komputera](xref:security/data-protection/configuration/machine-wide-policy)

  * [Scenariusze z systemem innym niż Podpisane aware](xref:security/data-protection/configuration/non-di-scenarios)

* [Interfejsy API rozszerzalności](xref:security/data-protection/extensibility/index)

  * [Rozszerzalność kryptografii Core](xref:security/data-protection/extensibility/core-crypto)

  * [Rozszerzalność zarządzania kluczami](xref:security/data-protection/extensibility/key-management)

  * [Różne interfejsy API](xref:security/data-protection/extensibility/misc-apis)

* [Implementacja](xref:security/data-protection/implementation/index)

  * [Szczegóły uwierzytelnionego szyfrowania](xref:security/data-protection/implementation/authenticated-encryption-details)

  * [Wyprowadzanie podkluczy i uwierzytelnione szyfrowanie](xref:security/data-protection/implementation/subkeyderivation)

  * [Nagłówki kontekstu](xref:security/data-protection/implementation/context-headers)

  * [Zarządzanie kluczami](xref:security/data-protection/implementation/key-management)

  * [Dostawcy magazynu kluczy](xref:security/data-protection/implementation/key-storage-providers)

  * [Szyfrowanie kluczy podczas magazynowania](xref:security/data-protection/implementation/key-encryption-at-rest)

  * [Ustawienia i niezmienność klucza](xref:security/data-protection/implementation/key-immutability)

  * [Format magazynu kluczy](xref:security/data-protection/implementation/key-storage-format)

  * [Dostawcy efemerycznej ochrony danych](xref:security/data-protection/implementation/key-storage-ephemeral)

* [Zgodność](xref:security/data-protection/compatibility/index)

  * [Zastępowanie ASP.NET <machineKey> w platformy ASP.NET Core](xref:security/data-protection/compatibility/replacing-machinekey)
