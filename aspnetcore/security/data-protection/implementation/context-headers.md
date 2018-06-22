---
title: Nagłówki kontekstu w ASP.NET Core
author: rick-anderson
description: Dowiedz się szczegóły implementacji nagłówków kontekstu ASP.NET Core do ochrony danych.
ms.author: riande
ms.date: 10/14/2016
uid: security/data-protection/implementation/context-headers
ms.openlocfilehash: 2343e59898c024eba420390d7fb0bce2fc82a895
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/20/2018
ms.locfileid: "36274472"
---
# <a name="context-headers-in-aspnet-core"></a>Nagłówki kontekstu w ASP.NET Core

<a name="data-protection-implementation-context-headers"></a>

## <a name="background-and-theory"></a>Tło i teorii

W systemie ochrony danych "klucza" oznacza, że obiekt, który zapewnia uwierzytelniony usługi szyfrowania. Każdy klucz jest określana przez unikatowy identyfikator (GUID), a niesie ze sobą algorytmicznego informacje i materiały entropic. Jest on przeznaczony każdy klucz przenoszenia unikatowy entropii, ale system nie można wymusić, który i musimy także konta dla deweloperów, którzy mogą pierścień klucza ręcznie zmienić, modyfikując algorytmicznego informacji istniejącego klucza w kręgu klucza. Do osiągnięcia bieżących wymagań zabezpieczeń podane w tych przypadkach system ochrony danych ma koncepcji [zręczność kryptograficzna](https://www.microsoft.com/en-us/research/publication/cryptographic-agility-and-its-relation-to-circular-encryption/), dzięki czemu przy użyciu pojedynczej wartości entropic wiele algorytmów kryptograficznych.

Większość systemów, które obsługują zręczność kryptograficzna to zrobić, w tym niektóre informacje identyfikujące algorytm ładunku. Identyfikator OID algorytmu zazwyczaj są odpowiednimi kandydatami tego. Jednak jeden problem, który wystąpił zakłada, że wiele sposobów, aby określić algorytm tego samego: "AES" (CNG) i zarządzanych Aes, AesManaged AesCryptoServiceProvider, AesCng i RijndaelManaged (określone parametry podane) klasy wszystkich faktycznie są takie same element i firma Microsoft musi Obsługa mapowania wszystkich tych na poprawny identyfikator OID. Deweloper trzeba podać niestandardowy algorytm (lub nawet innej implementacji AES!), zostałyby Powiedz nam jego identyfikator OID. Tego kroku rejestracji dodatkowe sprawia, że konfiguracja systemu jest szczególnie bolesne.

Przechodzenie wstecz, zdecydowaliśmy się, że firma Microsoft zostały zbliża się problem z niewłaściwego kierunek. Identyfikator OID informuje, co jest algorytm, ale faktycznie nie Dbamy o to. Jeśli należy używać jednej wartości entropic w bezpieczny sposób w dwóch różnych algorytmów, nie jest niezbędna dla nam znać, co faktycznie są algorytmy. Co faktycznie Dbamy o to, ich zachowania. Dowolny algorytm szyfrowania zadowalający symetrycznego bloku jest również silne permutacji pseudolosowych (PRP): Usuń dane wejściowe (klucz łańcucha zwykłego tekstu w trybie IV) i dane wyjściowe tekstu szyfrowanego z przeciążając uda się rozpoznać prawdopodobieństwo będą różne od innych szyfrowania symetrycznego bloku Algorytm podane tych samych wejściach. Podobnie dowolnej funkcji zadowalający kluczem wyznaczania wartości skrótu jest również funkcja pseudolosowych silne (PRF), a podany stały zestaw wejściowy dane wyjściowe przeważnie będą różne od innych funkcji skrótu kluczem.

Używamy koncepcji silne PRPs i PRFs do zbudowania nagłówka kontekstu. Ten nagłówek kontekstu zasadniczo działa jako stabilny odcisk palca za pośrednictwem algorytmy używane do żadnej operacji danego i zapewnia zręczność kryptograficzna wymaganych przez system ochrony danych. Ten nagłówek zostanie odtworzony i jest później używany w ramach [podkluczy procesu pochodnym](xref:security/data-protection/implementation/subkeyderivation#data-protection-implementation-subkey-derivation). Istnieją dwa różne sposoby tworzenia nagłówka kontekstu, w zależności od trybów operacji algorytmów podstawowej.

## <a name="cbc-mode-encryption--hmac-authentication"></a>Szyfrowanie w trybie CBC + HMAC uwierzytelniania

<a name="data-protection-implementation-context-headers-cbc-components"></a>

Nagłówek kontekstu składa się z następujących składników:

* [16 bitów] Wartość 00 00, czyli znacznik oznacza "szyfrowania CBC + HMAC uwierzytelniania".

* [32-bitowy] Długość klucza (w bajtach, big-endian) algorytmu szyfrowania symetrycznego bloku.

* [32-bitowy] Rozmiar bloku (w bajtach, big-endian) algorytmu szyfrowania symetrycznego bloku.

* [32-bitowy] Długość klucza (w bajtach, big-endian) algorytmu HMAC. (Aktualnie rozmiar klucza zawsze zgodny rozmiar szyfrowanego.)

* [32-bitowy] Skrót rozmiar (w bajtach, big-endian) algorytmu HMAC.

* EncCBC (K_E, IV, ""), który jest dane wyjściowe algorytmu szyfrowania symetrycznego bloku danych wejściowych pustego ciągu i IV w przypadku wektora samych zer. Konstrukcja K_E opisano poniżej.

* MAC (K_H, ""), która jest dane wyjściowe algorytmem HMAC danych wejściowych pusty ciąg. Konstrukcja K_H opisano poniżej.

Najlepiej możemy przekazać wektory samych zer, K_E i K_H. Jednak chcemy uniknąć sytuacji, w której algorytm sprawdza istnienie kluczy słabe przed wykonaniem jakichkolwiek operacji (szczególnie DES i 3DES), co wyklucza przy użyciu prostego lub repeatable wzorca takich jak wektorowa samych zer.

Zamiast tego używamy KDF SP800 108 NIST w trybie licznika (zobacz [NIST SP800-108](http://nvlpubs.nist.gov/nistpubs/Legacy/SP/nistspecialpublication800-108.pdf), 5.1 s.) o zerowej długości klucza, etykiety i kontekstu i HMACSHA512 jako podstawowej PRF. Firma Microsoft pochodzi | K_E | + | K_H | bajtów wyjściowych, następnie Rozłóż wynik na K_E i K_H samodzielnie. Matematycznie jest reprezentowane w następujący sposób.

(K_E || K_H) = SP800_108_CTR (prf = HMACSHA512, klucz = "", etykiety = "", kontekst = "")

### <a name="example-aes-192-cbc--hmacsha256"></a>Przykład: AES-192-CBC + HMACSHA256

Na przykład rozważmy przypadek, gdzie algorytmu szyfrowania symetrycznego bloku to AES-192-CBC, a algorytm sprawdzania poprawności jest HMACSHA256. System może wygenerować nagłówek kontekstu, wykonując następujące kroki.

Najpierw należy umożliwić (K_E || K_H) = SP800_108_CTR (prf = HMACSHA512, klucz = "", etykiety = "", kontekst = ""), gdzie | K_E | = bity 192 i | K_H | = 256 bitów na określonym algorytmów. Prowadzi to do K_E = 5BB6... 21DD i K_H = A04A... 00A9 w poniższym przykładzie:

```
5B B6 C9 83 13 78 22 1D 8E 10 73 CA CF 65 8E B0
61 62 42 71 CB 83 21 DD A0 4A 05 00 5B AB C0 A2
49 6F A5 61 E3 E2 49 87 AA 63 55 CD 74 0A DA C4
B7 92 3D BF 59 90 00 A9
```

Następnie należy obliczyć Enc_CBC (K_E, IV, "") dla AES-192-CBC podane IV = 0 * i K_E jako powyżej.

result := F474B1872B3B53E4721DE19C0841DB6F

Następnie obliczeniowe MAC (K_H, "") dla HMACSHA256 podane K_H jak powyżej.

result := D4791184B996092EE1202F36E8608FA8FBD98ABDFF5402F264B1D7211536220C

Daje to nagłówka kontekstu pełnego poniżej:

```
00 00 00 00 00 18 00 00 00 10 00 00 00 20 00 00
00 20 F4 74 B1 87 2B 3B 53 E4 72 1D E1 9C 08 41
DB 6F D4 79 11 84 B9 96 09 2E E1 20 2F 36 E8 60
8F A8 FB D9 8A BD FF 54 02 F2 64 B1 D7 21 15 36
22 0C
```

Ten nagłówek kontekstu jest odcisk palca pary algorytmu szyfrowania uwierzytelniony (szyfrowanie AES-192-CBC + weryfikacji HMACSHA256). Składniki, zgodnie z opisem [powyżej](xref:security/data-protection/implementation/context-headers#data-protection-implementation-context-headers-cbc-components) są:

* znacznika (00 00)

* długość klucza szyfrowania bloku (00 00 00 18)

* rozmiar bloku cipher block (00 00 00 10)

* długość klucza HMAC (00 00 00 20)

* rozmiar skrótu HMAC (00 00 00 20)

* szyfry blokowe zasady replikacji HASEŁ danych wyjściowych (F4 74 - DB 6 f) i

* dane wyjściowe HMAC PRF (D4 79 - end).

> [!NOTE]
> Szyfrowanie w trybie CBC + HMAC nagłówka kontekstu uwierzytelniania jest oparty ten sam sposób, niezależnie od tego, czy implementacje algorytmów są dostarczane przez Windows CNG lub typy zarządzane SymmetricAlgorithm i KeyedHashAlgorithm. Dzięki temu aplikacje uruchomione w różnych systemach operacyjnych niezawodnie utworzyć tego samego nagłówka kontekstu, mimo że implementacje algorytmów różni się od systemów operacyjnych. (W praktyce KeyedHashAlgorithm nie musi być odpowiednie HMAC. Może być dowolnego typu algorytmu wyznaczania wartości skrótu kluczem.)

### <a name="example-3des-192-cbc--hmacsha1"></a>Przykład: 3DES-192-CBC + HMACSHA1

Najpierw należy umożliwić (K_E || K_H) = SP800_108_CTR (prf = HMACSHA512, klucz = "", etykiety = "", kontekst = ""), gdzie | K_E | = bity 192 i | K_H | = 160-bitowy określonego algorytmów. Prowadzi to do K_E = A219... E2BB i K_H = DC4A... B464 w poniższym przykładzie:

```
A2 19 60 2F 83 A9 13 EA B0 61 3A 39 B8 A6 7E 22
61 D9 F8 6C 10 51 E2 BB DC 4A 00 D7 03 A2 48 3E
D1 F7 5A 34 EB 28 3E D7 D4 67 B4 64
```

Następnie obliczeniowe Enc_CBC (K_E, IV, "") dla algorytmu 3DES-192-CBC podane IV = 0 * i K_E jako powyżej.

result := ABB100F81E53E10E

Następnie obliczeniowe MAC (K_H, "") dla HMACSHA1 podane K_H jak powyżej.

result := 76EB189B35CF03461DDF877CD9F4B1B4D63A7555

Daje to nagłówka kontekstu pełnego, który jest odcisk palca uwierzytelniony szyfrowania algorytmu pary (szyfrowanie 3DES-192-CBC + weryfikacji HMACSHA1), pokazano poniżej:

```
00 00 00 00 00 18 00 00 00 08 00 00 00 14 00 00
00 14 AB B1 00 F8 1E 53 E1 0E 76 EB 18 9B 35 CF
03 46 1D DF 87 7C D9 F4 B1 B4 D6 3A 75 55
```

Składniki, który podzielić się w następujący sposób:

* znacznika (00 00)

* długość klucza szyfrowania bloku (00 00 00 18)

* rozmiar bloku cipher block (00 00 00 08)

* długość klucza HMAC (00 00 00 14)

* rozmiar skrótu HMAC (00 00 00 14)

* szyfry blokowe zasady replikacji HASEŁ danych wyjściowych (AB B1 - E1 0E) i

* dane wyjściowe HMAC PRF (76 EB - end).

## <a name="galoiscounter-mode-encryption--authentication"></a>Tryb Galois liczników szyfrowania i uwierzytelniania

Nagłówek kontekstu składa się z następujących składników:

* [16 bitów] Wartość 00 01, który jest znacznik oznacza "szyfrowania GCM + uwierzytelniania".

* [32-bitowy] Długość klucza (w bajtach, big-endian) algorytmu szyfrowania symetrycznego bloku.

* [32-bitowy] Nonce rozmiar (w bajtach, big-endian) używane podczas operacji szyfrowania uwierzytelniony. (Dla naszego systemu jest to stała w rozmiarze nonce = 96 bitów.)

* [32-bitowy] Rozmiar bloku (w bajtach, big-endian) algorytmu szyfrowania symetrycznego bloku. (Dla usługi GCM, to jest ustalony na rozmiar bloku = 128 bitów.)

* [32-bitowy] Uwierzytelnianie tag rozmiar (w bajtach, big-endian) utworzonej przez funkcję szyfrowania uwierzytelnionego. (Dla naszego systemu jest to stała w rozmiarze tag = 128 bitów.)

* [128 bitów] Tag Enc_GCM (K_E nonce, ""), czyli dane wyjściowe algorytmu szyfrowania symetrycznego bloku danych wejściowych pusty ciąg, jak i gdzie identyfikator jednorazowy jest wektorem samych zer 96-bitowej.

K_E pochodzi ten sam mechanizm, tak jak szyfrowanie CBC + HMAC scenariusz uwierzytelniania. Jednakże, ponieważ w tym miejscu play nie nie K_H, zasadniczo mamy | K_H | = 0, a algorytm zwijany do poniższego formularza.

K_E = SP800_108_CTR (prf = HMACSHA512, klucz = "", etykiety = "", kontekst = "")

### <a name="example-aes-256-gcm"></a>Przykład: AES-256-usługi GCM

Najpierw należy umożliwić K_E = SP800_108_CTR (prf = HMACSHA512, klucz = "", etykiety = "", kontekst = ""), gdzie | K_E | = 256 bitów.

K_E := 22BC6F1B171C08C4AE2F27444AF8FC8B3087A90006CAEA91FDCFB47C1B8733B8

Następnie obliczeniowe tagu uwierzytelniania Enc_GCM (K_E nonce, "") dla AES-256-GCM podany identyfikator jednorazowy = 096 i K_E jako powyżej.

result := E7DCCE66DF855A323A6BB7BD7A59BE45

Daje to nagłówka kontekstu pełnego poniżej:

```
00 01 00 00 00 20 00 00 00 0C 00 00 00 10 00 00
00 10 E7 DC CE 66 DF 85 5A 32 3A 6B B7 BD 7A 59
BE 45
```

Składniki, który podzielić się w następujący sposób:

   * znacznika (00 01)

   * długość klucza szyfrowania bloku (00 00 00 20)

   * rozmiar nonce (00 00 00 0 C)

   * rozmiar bloku cipher block (00 00 00 10)

   * Rozmiar znacznika uwierzytelniania (00 00 00 10) i

   * tag uwierzytelniania z systemem szyfrowania bloku (E7 DC - end).
