---
title: Szczegóły uwierzytelnionego szyfrowania w programie ASP.NET Core
author: rick-anderson
description: Szczegóły dotyczące wykonania szyfrowania ochronę danych usługi ASP.NET Core uwierzytelniony.
ms.author: riande
ms.date: 10/14/2016
uid: security/data-protection/implementation/authenticated-encryption-details
ms.openlocfilehash: 9def03e6b27e19fc34a839e923d6152e086889db
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/27/2019
ms.locfileid: "64902662"
---
# <a name="authenticated-encryption-details-in-aspnet-core"></a>Szczegóły uwierzytelnionego szyfrowania w programie ASP.NET Core

<a name="data-protection-implementation-authenticated-encryption-details"></a>

Wywołania IDataProtector.Protect to operacje uwierzytelnione szyfrowanie. Metoda Chroń oferuje poufności i autentyczności i jest powiązany łańcuchem cel, który został użyty do wyprowadzenia tego konkretnego wystąpienia interfejsu IDataProtector od głównego IDataProtectionProvider.

IDataProtector.Protect przyjmuje parametr byte [] zwykłego tekstu i tworzy byte [] chronionych ładunek, którego format został opisany poniżej. (Dostępna jest również przeciążenie metody rozszerzenia, która przyjmuje jako parametr ciągu w postaci zwykłego tekstu i zwraca ładunek chronionych ciągu. Jeśli jest używany ten interfejs API nadal będzie miał format ładunku chronionych poniżej struktury, ale będzie [zakodowane w formacie base64url](https://tools.ietf.org/html/rfc4648#section-5).)

## <a name="protected-payload-format"></a>Format ładunku chronionych

Format ładunku chronionych obejmuje trzy główne składniki:

* Nagłówek magic 32-bitowy, który identyfikuje wersję system ochrony danych.

* 128-bitowego klucza identyfikatora, który identyfikuje klucz używany do ochrony tego konkretnego ładunku.

* Pozostała część chronionej ładunku jest [specyficzne dla modułu szyfrującego zamknięte przez ten klucz](xref:security/data-protection/implementation/subkeyderivation#data-protection-implementation-subkey-derivation). W poniższym przykładzie klawisz reprezentuje AES-256-CBC + modułu szyfrującego HMACSHA256, a ładunek dalej podzielona następująco:
  * 128-bitowego modyfikator klucza.
  * Wektor inicjalizacji 128-bitowego.
  * 48 bajtów danych wyjściowych AES-256-CBC.
  * HMACSHA256 tag uwierzytelniania.

Poniżej przedstawiono przykładowy ładunek chronionych.

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

Z format ładunku powyżej pierwsze 32 bity lub 4 bajty są magic nagłówka, identyfikowanie wersji (09 F0 C9 F0)

Następne 128 bitów lub 16-bajtowy jest identyfikator klucza (80 9C 81 0C 19 66 19 40 95 36 53 F8 AA FF EE 57)

Pozostała zawiera ładunek i zależy od formatu używanego.

> [!WARNING]
> Wszystkich ładunków chronionych przez podany klucz rozpocznie się za pomocą tego samego nagłówka 20-bajtowy (wartość magic, identyfikator klucza). Administratorzy mogą używać tego faktu w celach diagnostycznych zbliżenie podczas generowania ładunku. Na przykład ładunku powyżej odnosi się do klucza {0c819c80-6619-4019-9536-53f8aaffee57}. Jeśli po sprawdzeniu klucza repozytorium możesz znaleźć, Data aktywacji tego określonego klucza to 2015-01-01 i daty wygaśnięcia został 2015-03-01, a następnie jest uzasadnione założył, że ładunek (Jeśli nie naruszony) został wygenerowany w ramach tego okna, zapewniają lub wykonaj mały współczynnik grę fudge po obu stronach.
