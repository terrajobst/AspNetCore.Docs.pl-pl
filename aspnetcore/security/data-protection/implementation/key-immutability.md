---
title: "Immutability klucza i zmienianie ustawień"
author: rick-anderson
description: "W tym dokumencie przedstawiono szczegóły implementacji platformy ASP.NET Core danych ochrony klucza immutability interfejsów API."
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/implementation/key-immutability
ms.openlocfilehash: 8e46e634266fa5f082c47f3be306009eb54bcbcc
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/19/2018
---
# <a name="key-immutability-and-changing-settings"></a>Klucz Immutability i zmienianie ustawień

Gdy obiekt jest utrwalone w magazynie zapasowy, jego reprezentacja zawsze jest stała. Nowe dane mogą zostać dodane do magazynu zapasowego, ale istniejące dane nigdy nie mogą ulegać mutacjom. Głównym celem to zachowanie jest aby zapobiec uszkodzeniu danych.

W wyniku tego zachowania po klucz są zapisywane jako magazynu zapasowego, jest niezmienialny. Jego daty utworzenia, aktywacji i wygaśnięcia można nigdy zmieniać, chociaż może on odwołany za pomocą `IKeyManager`. Ponadto jego podstawowych informacji algorytmicznego, materiał klucza głównego i szyfrowanie właściwości rest również są niezmienne.

Deweloper zmiana każdego ustawienia, które ma wpływ na trwałości klucza te zmiany nie zostaną zaczynają obowiązywać po ponownym wygenerowaniu klucza, albo za pośrednictwem jawnym wywołaniem `IKeyManager.CreateNewKey` lub za pośrednictwem danych ochrony systemu własnych [automatyczne klucza Generowanie](key-management.md#data-protection-implementation-key-management) zachowanie. Dostępne są następujące ustawienia, które mają wpływ na trwałości klucza:

* [Domyślny okres istnienia klucza](key-management.md#data-protection-implementation-key-management)

* [Szyfrowanie klucza w mechanizm rest](key-encryption-at-rest.md#data-protection-implementation-key-encryption-at-rest)

* [Algorytmicznego informacje zawarte w kluczu](xref:security/data-protection/configuration/overview#changing-algorithms-with-usecryptographicalgorithms)

Te ustawienia do wcześniejszej niż klawisza Następna automatyczne stopniowych czasu zaczną działać, należy wziąć pod uwagę jawnego wywołania do `IKeyManager.CreateNewKey` wymusić Tworzenie nowego klucza. Pamiętaj, aby określić datę aktywację jawnym ({teraz + 2 dni} jest regułą umożliwia czasu, dla której zmiana obejmie) oraz datę wygaśnięcia w wywołaniu.

>[!TIP]
> Wszystkie aplikacje dotknięcie repozytorium należy określić te same ustawienia `IDataProtectionBuilder` metody rozszerzenia. W przeciwnym razie wartość właściwości klucza utrwalonego będzie zależna od określonej aplikacji, która wywołała procedury generowania kluczy.
