---
title: Niezmienność kluczy i kluczy ustawień w programie ASP.NET Core
author: rick-anderson
description: Dowiedz się, szczegóły implementacji systemu niezmienność kluczy ochrony danych programu ASP.NET Core interfejsów API.
ms.author: riande
ms.date: 10/14/2016
uid: security/data-protection/implementation/key-immutability
ms.openlocfilehash: 7796cb102c0f6f03809704016fd36b28c7a82438
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/27/2019
ms.locfileid: "64898765"
---
# <a name="key-immutability-and-key-settings-in-aspnet-core"></a>Niezmienność kluczy i kluczy ustawień w programie ASP.NET Core

Gdy obiekt jest utrwalone w magazynie pomocniczym, jego reprezentację w postaci zawsze jest stała. Nowe dane mogą zostać dodane do magazynu zapasowego, ale istniejące dane nigdy nie można zmutować. Głównym celem to zachowanie jest, aby zapobiec uszkodzeniu danych.

W wyniku tego zachowania, jest napisana klucz do magazynu zapasowego niezmienne. Jego daty utworzenia, aktywacji i wygaśnięcia nigdy nie można zmienić, chociaż mogą odwoływane za pomocą `IKeyManager`. Ponadto jego podstawowych informacji algorytmicznego, materiał klucza głównego i szyfrowania we właściwościach rest również są niezmienne.

Jeśli Deweloper zmienia każdego ustawienia, które ma wpływ na trwałość klucza, te zmiany nie zaczną obowiązywać po ponownym wygenerowaniu klucza, za pośrednictwem jawnym wywołaniem `IKeyManager.CreateNewKey` lub za pośrednictwem danych ochrony systemu własnych [automatyczne klucza Generowanie](xref:security/data-protection/implementation/key-management#data-protection-implementation-key-management) zachowanie. Dostępne są następujące ustawienia, które wpływają na trwałość klucza:

* [Domyślny okres istnienia klucza](xref:security/data-protection/implementation/key-management#data-protection-implementation-key-management)

* [Szyfrowanie kluczy podczas mechanizm rest](xref:security/data-protection/implementation/key-encryption-at-rest)

* [Konsolidatorze informacji zawartych w kluczu](xref:security/data-protection/configuration/overview#changing-algorithms-with-usecryptographicalgorithms)

Jeśli potrzebujesz tych ustawień, aby uruchamiać starszych niż kluczowi automatyczne stopniowe czasu, należy wziąć pod uwagę jawnego wywołania do `IKeyManager.CreateNewKey` Aby wymusić utworzenie nowego klucza. Pamiętaj, aby określić datę aktywacji jawne ({teraz + 2 dni} jest regułą aby był czas na której zmiana obejmie) i datę wygaśnięcia w wywołaniu.

>[!TIP]
> Wszystkie aplikacje, dotknięcie repozytorium należy określić te same ustawienia `IDataProtectionBuilder` metody rozszerzenia. W przeciwnym razie właściwości klucza utrwalonych będzie zależny od określonej aplikacji, która wywołała procedury generowania kluczy.
