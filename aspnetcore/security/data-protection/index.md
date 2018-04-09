---
title: Ochrona danych w podstawowej platformy ASP.NET
author: rick-anderson
description: Ten dokument służy jako spisu treści dla różnych tematów ochrony danych platformy ASP.NET Core.
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/data-protection/index
ms.openlocfilehash: 83b5bb1e6a4942a4d3e5ec0d445fa6e5a21fb533
ms.sourcegitcommit: 48beecfe749ddac52bc79aa3eb246a2dcdaa1862
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/22/2018
---
# <a name="data-protection-in-aspnet-core"></a>Ochrona danych w podstawowej platformy ASP.NET

* [Wprowadzenie do ochrony danych](xref:security/data-protection/introduction)

* [Wprowadzenie do interfejsów API ochrony danych](xref:security/data-protection/using-data-protection)

* [Interfejsy API przeznaczone dla klientów](xref:security/data-protection/consumer-apis/index)

  * [Omówienie interfejsów API przeznaczonych dla klientów](xref:security/data-protection/consumer-apis/overview)

  * [Ciągi celów](xref:security/data-protection/consumer-apis/purpose-strings)

  * [Hierarchia celów i obsługa wielu dzierżawców](xref:security/data-protection/consumer-apis/purpose-strings-multitenancy)

  * [Skrót hasła](xref:security/data-protection/consumer-apis/password-hashing)

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

  * [Immutability klucza i ustawienia](xref:security/data-protection/implementation/key-immutability)

  * [Format magazynu kluczy](xref:security/data-protection/implementation/key-storage-format)

  * [Dostawcy efemerycznej ochrony danych](xref:security/data-protection/implementation/key-storage-ephemeral)

* [Zgodność](xref:security/data-protection/compatibility/index)

  * [Zastępowanie ASP.NET <machineKey> w platformy ASP.NET Core](xref:security/data-protection/compatibility/replacing-machinekey)
