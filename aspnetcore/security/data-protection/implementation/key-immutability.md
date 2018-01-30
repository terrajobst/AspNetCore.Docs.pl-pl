---
title: "Immutability klucza i zmienianie ustawień"
author: rick-anderson
description: "W tym dokumencie przedstawiono szczegóły implementacji platformy ASP.NET Core danych ochrony klucza immutability interfejsów API."
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/data-protection/implementation/key-immutability
ms.openlocfilehash: 98727c7a0c525edcda4fd8d004e0ac584cf0a5e5
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/30/2018
---
# <a name="key-immutability-and-changing-settings"></a>Klucz Immutability i zmienianie ustawień

Gdy obiekt jest utrwalone w magazynie zapasowy, jego reprezentacja zawsze jest stała. Nowe dane mogą zostać dodane do magazynu zapasowego, ale istniejące dane nigdy nie mogą ulegać mutacjom. Głównym celem to zachowanie jest aby zapobiec uszkodzeniu danych.

W wyniku tego zachowania po klucz są zapisywane jako magazynu zapasowego, jest niezmienialny. Jego daty utworzenia, aktywacji i wygaśnięcia można nigdy zmieniać, chociaż może on odwołany za pomocą `IKeyManager`. Ponadto jego podstawowych informacji algorytmicznego, materiał klucza głównego i szyfrowanie właściwości rest również są niezmienne.

Zmiana każdego ustawienia, które ma wpływ na klucza trwałości, deweloper tych zmian nie zaczynają obowiązywać po ponownym wygenerowaniu klucza, albo za pomocą jawnego wywołania `IKeyManager.CreateNewKey` lub za pośrednictwem danych ochrony systemu własnych [automatyczne klucza Generowanie](key-management.md#data-protection-implementation-key-management) zachowanie. Dostępne są następujące ustawienia, które mają wpływ na trwałości klucza:

* [Domyślny okres istnienia klucza](key-management.md#data-protection-implementation-key-management)

* [Szyfrowanie klucza w mechanizm rest](key-encryption-at-rest.md#data-protection-implementation-key-encryption-at-rest)

* [Algorytmicznego informacje zawarte w kluczu](xref:security/data-protection/configuration/overview#changing-algorithms-with-usecryptographicalgorithms)

Te ustawienia do wcześniejszej niż klawisza Następna automatyczne stopniowych czasu zaczną działać, należy wziąć pod uwagę jawnego wywołania do `IKeyManager.CreateNewKey` wymusić Tworzenie nowego klucza. Pamiętaj, aby określić datę aktywację jawnym ({teraz + 2 dni} jest regułą umożliwia czasu, dla której zmiana obejmie) oraz datę wygaśnięcia w wywołaniu.

>[!TIP]
> Wszystkie aplikacje dotknięcie repozytorium należy określić te same ustawienia `IDataProtectionBuilder` metody rozszerzenia. W przeciwnym razie wartość właściwości klucza utrwalonego będzie zależna od określonej aplikacji, która wywołała procedury generowania kluczy.
