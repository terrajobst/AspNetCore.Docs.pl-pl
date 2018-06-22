---
title: Szczegóły uwierzytelnionego szyfrowania w ASP.NET Core
author: rick-anderson
description: Szczegóły dotyczące wykonania szyfrowania platformy ASP.NET Core Data Protection uwierzytelniony.
ms.author: riande
ms.date: 10/14/2016
uid: security/data-protection/implementation/authenticated-encryption-details
ms.openlocfilehash: ac650e5c32e7eacc4088225e63f56340f95e1913
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/20/2018
ms.locfileid: "36275687"
---
# <a name="authenticated-encryption-details-in-aspnet-core"></a>Szczegóły uwierzytelnionego szyfrowania w ASP.NET Core

<a name="data-protection-implementation-authenticated-encryption-details"></a>

Wywołania IDataProtector.Protect są operacje szyfrowania uwierzytelniony. Metoda Chroń oferuje zarówno poufności i autentyczności i jest powiązany łańcuch cel, który został użyty do uzyskania tego konkretnego wystąpienia interfejsu IDataProtector od głównego IDataProtectionProvider.

IDataProtector.Protect przyjmuje parametr typu byte [] w postaci zwykłego tekstu i tworzy byte [] chronionych ładunku, których format jest opisane poniżej. (Dostępna jest również przeciążenie metody rozszerzenia, która przyjmuje parametr typu string w postaci zwykłego tekstu i zwraca ciąg ładunku chronionych. Jeśli ten interfejs API jest używany format ładunku chronionych nadal będzie miał poniżej struktury, ale będzie ona [kodowany w formacie base64url](https://tools.ietf.org/html/rfc4648#section-5).)

## <a name="protected-payload-format"></a>Format ładunku chronionych

Format ładunku chronionych obejmuje trzy główne składniki:

* Nagłówek magic 32-bitowy, który identyfikuje wersję systemu ochrony danych.

* 128-bitowego klucza identyfikatora, który identyfikuje klucz używany do ochrony tego konkretnego ładunku.

* W pozostałej części chronionych ładunku jest [specyficzne dla modułu szyfrującego zamknięte przez tego klucza](xref:security/data-protection/implementation/subkeyderivation#data-protection-implementation-subkey-derivation). W poniższym przykładzie reprezentuje klucz AES-256-CBC + modułu szyfrującego HMACSHA256 i ładunków dalszej podzielonych w następujący sposób: * A 128-bitowego klucza modyfikator. * Wektor inicjowania 128-bitowego. * 48 bajtów AES-256-CBC danych wyjściowych. * HMACSHA256 tag uwierzytelniania.

Poniżej przedstawiono przykładowe ładunku chronionych.

```
09 F0 C9 F0 80 9C 81 0C 19 66 19 40 95 36 53 F8
AA FF EE 57 57 2F 40 4C 3F 7F CC 9D CC D9 32 3E
84 17 99 16 EC BA 1F 4A A1 18 45 1F 2D 13 7A 28
79 6B 86 9C F8 B7 84 F9 26 31 FC B1 86 0A F1 56
61 CF 14 58 D3 51 6F CF 36 50 85 82 08 2D 3F 73
5F B0 AD 9E 1A B2 AE 13 57 90 C8 F5 7C 95 4E 6A
8A AA 06 EF 43 CA 19 62 84 7C 11 B2 C8 71 9D AA
52 19 2E 5B 4C 1E 54 F0 55 BE 88 92 12 C1 4B 5E
52 C9 74 A0
```

Z format ładunku powyżej pierwszy 32-bitowy lub 4 bajty mają nagłówek magic identyfikowania wersji (09 F0 C9 F0)

Następne 128 bitów lub 16 bajtów jest identyfikator klucza (80 9C 81 0C 19 66 19 40 95 36 53 F8 AA FF EE 57)

Pozostałe zawiera ładunek i jest specyficzna dla format używany.

>[!WARNING]
> Wszystkie ładunki chroniony przez klucz rozpocznie się z tego samego nagłówka 20-bajtową (wartość Magiczna, identyfikator klucza). Administratorzy mogą używać tego faktu w celach diagnostycznych zbliżenie podczas ładunku został wygenerowany. Na przykład powyżej ładunku odpowiada kluczowi {0c819c80-6619-4019-9536-53f8aaffee57}. Jeśli po sprawdzeniu klucza repozytorium okaże się, że to określonego klucza aktywacji Data 2015-01-01 i jej daty wygaśnięcia został 2015-03-01, a następnie można założyć, że ładunku (Jeśli nie niepowołane) został wygenerowany w ramach tego okna, nadaj lub podjąć małą współczynnik fudge po obu stronach.
