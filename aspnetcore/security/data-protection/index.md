---
title: Ochrona danych w podstawowej platformy ASP.NET
author: rick-anderson
description: "Ten dokument służy jako spisu treści dla różnych tematów ochrony danych platformy ASP.NET Core."
keywords: Platformy ASP.NET Core, ochrony danych
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 1f402da8-1052-4970-9835-9f9f16a02dbc
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/index
ms.openlocfilehash: 8b42e65bb6121355120a6f4fbe8cd4d1fea153de
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
# <a name="data-protection-in-aspnet-core-consumer-apis-configuration-extensibility-apis-and-implementation"></a>Ochrona danych w ASP.NET Core: interfejsy API konsumenta, konfigurowanie, rozszerzalności interfejsów API i wdrażanie

* [Wprowadzenie do ochrony danych](introduction.md)

* [Rozpoczynanie pracy z interfejsami API ochrony danych](using-data-protection.md)

* [Interfejsy API klienta](consumer-apis/index.md)

  * [Omówienie interfejsów API klienta](consumer-apis/overview.md)

  * [Cel ciągów](consumer-apis/purpose-strings.md)

  * [Cel hierarchii i obsługi wielu dzierżawców](consumer-apis/purpose-strings-multitenancy.md)

  * [Tworzenia skrótu hasła](consumer-apis/password-hashing.md)

  * [Ograniczanie okresu istnienia ładunków chronionych](consumer-apis/limited-lifetime-payloads.md)

  * [Wyłączanie ochrony ładunków, której klucze zostały odwołane.](consumer-apis/dangerous-unprotect.md)

* [Konfiguracja](configuration/index.md)

  * [Konfigurowanie ochrony danych](configuration/overview.md)

  * [Ustawienia domyślne](configuration/default-settings.md)

  * [Międzynarodowe zasad komputera](configuration/machine-wide-policy.md)

  * [Scenariusze pamiętać z systemem innym niż Podpisane](configuration/non-di-scenarios.md)

* [Interfejsy API rozszerzalności](extensibility/index.md)

  * [Rozszerzalność kryptografii Core](extensibility/core-crypto.md)

  * [Rozszerzalność zarządzania kluczami](extensibility/key-management.md)

  * [Dodatkowe interfejsy API](extensibility/misc-apis.md)

* [Implementacja](implementation/index.md)

  * [Szczegóły uwierzytelnionego szyfrowania](implementation/authenticated-encryption-details.md)

  * [Pochodnym podkluczy i uwierzytelnionego szyfrowania](implementation/subkeyderivation.md)

  * [Nagłówki kontekstu](implementation/context-headers.md)

  * [Zarządzanie kluczami](implementation/key-management.md)

  * [Dostawcy magazynu kluczy](implementation/key-storage-providers.md)

  * [Szyfrowanie klucza magazynowane](implementation/key-encryption-at-rest.md)

  * [Immutability klucza i zmienianie ustawień](implementation/key-immutability.md)

  * [Format magazynu kluczy](implementation/key-storage-format.md)

  * [Dostawców ochrony danych tymczasowych](implementation/key-storage-ephemeral.md)

* [Zgodność](compatibility/index.md)

  * [Udostępnianie plików cookie między aplikacjami](compatibility/cookie-sharing.md)

  * [Zastępowanie <machineKey> w programie ASP.NET](compatibility/replacing-machinekey.md)
