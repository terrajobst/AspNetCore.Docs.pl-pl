---
title: Pochodnym podkluczy i uwierzytelnionego szyfrowania
author: rick-anderson
description: "W tym dokumencie opisano szczegóły implementacji ochrony danych platformy ASP.NET Core podkluczy pochodnym i uwierzytelniony szyfrowania."
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/implementation/subkeyderivation
ms.openlocfilehash: 7db0bb7cd641512e7e5850e1765ad5b3e54e3aca
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/24/2018
---
# <a name="subkey-derivation-and-authenticated-encryption"></a>Pochodnym podkluczy i uwierzytelnionego szyfrowania

<a name="data-protection-implementation-subkey-derivation"></a>

Większość klawiszy w kręgu klucz będzie zawierać jakiegoś entropii i będzie miał informacji algorytmicznego, podając "szyfrowania w trybie CBC + weryfikacji HMAC" lub "szyfrowania GCM + Walidacja". W takich przypadkach nazywamy osadzonych entropii materiał klucza głównego (lub KM) dla tego klucza, a wykonać funkcja wyprowadzania klucza do tworzenia kluczy, które będą używane dla operacje kryptograficzne.

> [!NOTE]
> Klucze są abstrakcyjny, a implementacja niestandardowa może nie działać zgodnie z poniższymi instrukcjami. Jeśli klucz udostępnia własną implementację `IAuthenticatedEncryptor` zamiast przy użyciu jednej z naszych fabryki wbudowanych, mechanizm opisane w tej sekcji nie ma już zastosowania.

<a name="data-protection-implementation-subkey-derivation-aad"></a>

## <a name="additional-authenticated-data-and-subkey-derivation"></a>Dodatkowe dane uwierzytelnionych i podkluczy wyprowadzania

`IAuthenticatedEncryptor` Interfejsu służy jako interfejs core dla wszystkich operacji szyfrowania uwierzytelniony. Jego `Encrypt` metoda przyjmuje dwa buforów: zwykłego tekstu i additionalAuthenticatedData (AAD). Przepływ zawartości w postaci zwykłego tekstu bez zmian wywołanie `IDataProtector.Protect`, ale usługi AAD jest generowany przez system i składa się z trzech składników:

1. Nagłówek magic 32-bitowych 09 F0 C9 F0, która identyfikuje tę wersję systemu ochrony danych.

2. Identyfikator klucza 128-bitowego.

3. Sformatowany ciąg o zmiennej długości z łańcucha cel, który utworzony `IDataProtector` wykonuje tę operację.

Usługi AAD jest unikatowy dla wszystkich trzech komponentów spójnej kolekcji, możemy użyć go do uzyskania nowych kluczy z km, który zamiast km, który sam we wszystkich naszych operacji kryptograficznych. Dla każdego wywołania `IAuthenticatedEncryptor.Encrypt`, odbywa się następujący proces wyprowadzania klucza:

(K_E, K_H) = SP800_108_CTR_HMACSHA512 (contextHeader K_M, AAD, || keyModifier)

W tym miejscu dzwonimy KDF SP800 108 NIST w trybie licznika (zobacz [NIST SP800-108](http://nvlpubs.nist.gov/nistpubs/Legacy/SP/nistspecialpublication800-108.pdf), 5.1 s.) z następującymi parametrami:

* Klucz klucza pochodnego (KDK) = K_M

* PRF = HMACSHA512

* Etykieta = additionalAuthenticatedData

* kontekst = contextHeader || keyModifier

Nagłówek kontekstu jest o zmiennej długości i zasadniczo służy jako odcisk palca algorytmów, dla których firma Microsoft w przypadku tworzenia klasy pochodnej K_E i K_H. Modyfikator klucza jest ciągiem 128-bitowego, losowo wygenerowany dla każdego wywołania `Encrypt` i służy do zapewnienia przeciążając uda się rozpoznać prawdopodobieństwo, że KE i KH unikatowy dla tej operacji szyfrowania określonego uwierzytelniania, nawet jeśli wszystkie inne dane wejściowe KDF jest stałą.

W trybie CBC szyfrowania + HMAC operacji sprawdzania poprawności | K_E | długość klucza szyfrowania symetrycznego bloku i | K_H | jest rozmiarem skrótu HMAC procedury. Do szyfrowania GCM + operacji sprawdzania poprawności | K_H | = 0.

## <a name="cbc-mode-encryption--hmac-validation"></a>Szyfrowanie w trybie CBC + HMAC weryfikacji

Wygenerowany K_E za pośrednictwem mechanizmu powyżej firma Microsoft wektor inicjowania losowe generowanie i uruchom algorytmu szyfrowania symetrycznego bloku do encipher w formie zwykłego tekstu. Wektor inicjowania i tekstu szyfrowanego następnie uruchom za pomocą procedury HMAC zainicjowany z kluczem K_H w celu utworzenia komputerów MAC. Ten proces i wartość zwracana jest reprezentowane graficznie poniżej.

![Proces w trybie CBC i wróć](subkeyderivation/_static/cbcprocess.png)

*dane wyjściowe: = keyModifier || IV || E_cbc (dane K_E, iv) || Metoda HMAC (K_H, iv || E_cbc (dane K_E, iv))*

> [!NOTE]
> `IDataProtector.Protect` Będzie implementacji [dołączy nagłówku magic i identyfikator klucza](authenticated-encryption-details.md) danych wyjściowych przed zwróceniem jej do obiektu wywołującego. Ponieważ magic nagłówka i identyfikator klucza niejawnie są częścią [AAD](xref:security/data-protection/implementation/subkeyderivation#data-protection-implementation-subkey-derivation-aad), i ponieważ modyfikator klucza jest podawany jako dane wejściowe KDF, oznacza to, że każdy pojedynczy bajt końcowy ładunek zwracane jest uwierzytelniany przez komputerów MAC.

## <a name="galoiscounter-mode-encryption--validation"></a>Tryb Galois liczników szyfrowania + sprawdzania poprawności

Wygenerowany K_E za pośrednictwem mechanizmu powyżej firma Microsoft generowania losowego nonce 96-bitowej, a następnie uruchom algorytmu szyfrowania symetrycznego bloku encipher w postaci zwykłego tekstu i utworzyć tag uwierzytelniania 128-bitowego.

![Proces trybu GCM i wróć](subkeyderivation/_static/galoisprocess.png)

*dane wyjściowe: = keyModifier || Identyfikator jednorazowy || E_gcm (K_E, nonce, dane) || authTag*

> [!NOTE]
> Mimo że GCM natywnie obsługuje pojęcie usługi AAD, firma Microsoft jest nadal zasilania AAD tylko do oryginalnego KDF, aby przekazać pusty ciąg do usługi GCM jej parametru usługi AAD. Przyczyną tego jest dwukrotne. Najpierw [do obsługi elastyczność](context-headers.md#data-protection-implementation-context-headers) nigdy nie chcemy użyć K_M bezpośrednio jako klucza szyfrowania. Ponadto usługi GCM nakłada bardzo rygorystyczny unikatowość wymagania dotyczące jej danych wejściowych. Prawdopodobieństwo, że procedura szyfrowania GCM jest kiedykolwiek wywołana na dwóch lub więcej różne zestawy danych wejściowych o takiej samej (klucz, nonce) pary nie może przekraczać 2 ^ 32. Jeśli firma Microsoft rozwiąże K_E nie można wykonać więcej niż 2 ^ 32 operacji szyfrowania przed możemy uruchomić afoul o 2 ^ ograniczyć -32. To może się wydawać bardzo dużej liczby operacji, ale serwer sieci web o dużym natężeniu ruchu przejść przez kolejne żądania 4 miliardy w zwykłe dni, również w obrębie użytkowania tych kluczy. Aby pozostać zgodne 2 ^ limit prawdopodobieństwo-32 w dalszym ciągu używać 128-bitowego klucza modyfikator i identyfikator jednorazowy 96-bitowej, które znacząco rozszerza liczbę operacji można używać na dowolnym danym K_M. Dla uproszczenia projektowania firma Microsoft udostępnia ścieżkę kodu KDF między operacjami CBC i GCM, a ponieważ AAD jest już uznawana za w KDF nie jest konieczne ją przesłać do procedury usługi GCM.
