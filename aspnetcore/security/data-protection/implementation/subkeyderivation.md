---
title: Wyprowadzanie podkluczy i uwierzytelnione szyfrowanie w programie ASP.NET Core
author: rick-anderson
description: Dowiedz się, szczegóły implementacji ochrony danych programu ASP.NET Core podklucza pochodnym i uwierzytelnione szyfrowanie.
ms.author: riande
ms.date: 10/14/2016
uid: security/data-protection/implementation/subkeyderivation
ms.openlocfilehash: 37e7b01700e8a6b755b5ed16a9d7d75a9eeb970e
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/27/2019
ms.locfileid: "64898435"
---
# <a name="subkey-derivation-and-authenticated-encryption-in-aspnet-core"></a>Wyprowadzanie podkluczy i uwierzytelnione szyfrowanie w programie ASP.NET Core

<a name="data-protection-implementation-subkey-derivation"></a>

Większość klawiszy pierścienia klucz będzie zawierać jakąś postać entropii i będą zawierać informacje algorytmicznego, podając w "trybie CBC szyfrowanie i weryfikacja HMAC" lub "usługi GCM szyfrowanie i sprawdzanie poprawności". W takich przypadkach nazywamy osadzone entropii materiał klucza głównego (lub KM) dla tego klucza, a następnie wykonamy funkcji wyprowadzania klucza do tworzenia kluczy, które będą używane dla operacje kryptograficzne.

> [!NOTE]
> Klucze są abstrakcyjne, a implementacja niestandardowa mogą nie zachowywać się zgodnie z poniższymi instrukcjami. Jeśli klucz zapewnia własną implementację `IAuthenticatedEncryptor` zamiast przy użyciu jednego z naszych wbudowanych fabryki, mechanizm opisane w tej sekcji nie ma już zastosowania.

<a name="data-protection-implementation-subkey-derivation-aad"></a>

## <a name="additional-authenticated-data-and-subkey-derivation"></a>Dodatkowe dane uwierzytelnionego i wyprowadzanie podkluczy

`IAuthenticatedEncryptor` Interfejsu służy jako interfejs podstawowe dla wszystkich operacji szyfrowania uwierzytelniony. Jego `Encrypt` metoda przyjmuje dwa buforów: zwykłego tekstu i additionalAuthenticatedData (AAD). Przepływ zawartości zwykłego tekstu bez zmian wywołanie `IDataProtector.Protect`, ale usługi AAD jest generowany przez system i składa się z trzech składników:

1. Nagłówek magic 32-bitowych 09 F0 F0 C9, który identyfikuje tę wersję system ochrony danych.

2. Identyfikator klucza 128-bitowego.

3. Ciąg znaków o zmiennej długości utworzone z łańcucha celem utworzenia `IDataProtector` , jest wykonanie tej operacji.

Usługi AAD jest unikatowy dla krotki wszystkie trzy składniki, możemy użyć go do wyprowadzenia nowych kluczy z KM zamiast KM sama we wszystkich naszych operacji kryptograficznych. Dla każdego wywołania `IAuthenticatedEncryptor.Encrypt`, odbywa się następujący proces wyprowadzania klucza:

(K_E, K_H) = SP800_108_CTR_HMACSHA512 (contextHeader K_M, usługi AAD, || keyModifier)

W tym miejscu Zadzwonimy pod numer telefonu KDF SP800 108 NIST w trybie licznika (zobacz [NIST SP800-108](http://nvlpubs.nist.gov/nistpubs/Legacy/SP/nistspecialpublication800-108.pdf), s. 5.1) z następującymi parametrami:

* Klucz klucza pochodnego (KDK) = K_M

* PRF = HMACSHA512

* Etykieta = additionalAuthenticatedData

* kontekst = contextHeader || keyModifier

Nagłówek kontekstu jest o zmiennej długości i zasadniczo pełni rolę algorytmów, dla których firma Microsoft jest wyprowadzanie K_E i K_H odcisku palca. Modyfikator klucza jest ciągiem 128-bitowego losowo generowany dla każdego wywołania `Encrypt` i służy do zapewnienia przeciążenia prawdopodobieństwo, że KE i KH unikatowy dla tej operacji szyfrowania określonego uwierzytelniania, nawet jeśli wszystkie inne dane wejściowe do KDF jest stałe.

W trybie CBC szyfrowania + HMAC operacji sprawdzania poprawności | K_E | to długość klucza szyfrowania symetrycznego bloku, a | K_H | jest to rozmiar szyfrowanego procedury HMAC. Szyfrowanie usługi GCM i operacje sprawdzania poprawności | K_H | = 0.

## <a name="cbc-mode-encryption--hmac-validation"></a>Trybie CBC szyfrowanie i weryfikacja HMAC

Po wygenerowaniu K_E za pośrednictwem mechanizmu powyższych możemy wektor inicjowania losowe generowanie, a następnie uruchom algorytmu szyfrowania symetrycznego bloku do szyfrowania w formie zwykłego tekstu. Wektor inicjowania i tekstu szyfrowanego są następnie uruchamiane za pomocą procedury HMAC zainicjowane z użyciem klucza K_H do produkcji na komputerze MAC. Ten proces i wartość zwracana jest reprezentowane graficznie poniżej.

![Proces w trybie CBC i wróć](subkeyderivation/_static/cbcprocess.png)

*dane wyjściowe: = keyModifier || IV || E_cbc (dane K_E, iv) || HMAC (K_H, iv || E_cbc (dane K_E, iv))*

> [!NOTE]
> `IDataProtector.Protect` Wdrożenia będzie [dołączana magic nagłówek i identyfikator klucza](xref:security/data-protection/implementation/authenticated-encryption-details) danych wyjściowych przed zwróceniem do obiektu wywołującego. Ponieważ magic nagłówek i identyfikator klucza są niejawnie wchodzi w skład [AAD](xref:security/data-protection/implementation/subkeyderivation#data-protection-implementation-subkey-derivation-aad), a ponieważ modyfikator klucza jest podawany jako dane wejściowe KDF, oznacza to, że każdy jeden bajt końcowy ładunek zwracany jest uwierzytelniany przez komputerów MAC.

## <a name="galoiscounter-mode-encryption--validation"></a>Tryb Galois liczników szyfrowanie i weryfikacja

Po wygenerowaniu K_E za pośrednictwem mechanizmu powyższych możemy Generowanie losowe jednorazowego 96-bitowym, a następnie uruchom algorytmu szyfrowania symetrycznego bloku szyfrowanie w postaci zwykłego tekstu i utworzyć tag uwierzytelniania 128-bitowego.

![Proces trybu GCM i wróć](subkeyderivation/_static/galoisprocess.png)

*dane wyjściowe: = keyModifier || Identyfikator jednorazowy || E_gcm (K_E, jednorazowy, dane) || authTag*

> [!NOTE]
> Mimo że GCM natywnie obsługuje pojęcia usługi AAD, firma Microsoft jest nadal wprowadzając AAD tylko z oryginalnego KDF, aby przekazać pusty ciąg do usługi GCM dla jej parametru usługi AAD. Przyczyna to składa się z dwóch etapów. Po pierwsze, [do obsługi elastyczność](xref:security/data-protection/implementation/context-headers#data-protection-implementation-context-headers) nigdy nie chcemy użyć K_M bezpośrednio jako klucza szyfrowania. Ponadto usługa GCM nakłada unikatowości bardzo ścisłe wymagania dotyczące jego danych wejściowych. Prawdopodobieństwo, że procedura szyfrowania usługi GCM jest nigdy nie są wywoływane na dwóch lub więcej różnych zestawów danych wejściowych o takiej samej (klucz, jednorazowy) pary nie może przekraczać 2 ^ 32. Jeśli firma Microsoft rozwiąże K_E nie można wykonać więcej niż 2 ^ 32 operacji szyfrowania przed możemy uruchomić afoul o 2 ^ ograniczyć -32. To może wydawać się bardzo dużej liczby operacji, ale serwer sieci web o dużym natężeniu ruchu mogą być używane za pośrednictwem 4 miliardów żądań w kilku dni, również w ramach normalnych okres istnienia dla tych kluczy. Aby pozostają zgodne 2 ^ limit prawdopodobieństwo-32 Kontynuujemy przy użyciu 128-bitowego klucza modyfikator i jednorazowego 96-bitowym, która znacząco rozszerza liczbę operacji można używać dla dowolnego danego K_M. Dla uproszczenia projektu udostępniamy ścieżka kodu KDF między operacjami CBC i GCM, a ponieważ usługi AAD już jest uważana za KDF nie ma potrzeby ją przesłać do procedury usługi GCM.
