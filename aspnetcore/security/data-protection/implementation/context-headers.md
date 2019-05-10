---
title: Nagłówki kontekstu w programie ASP.NET Core
author: rick-anderson
description: Dowiedz się, szczegóły implementacji nagłówki kontekstu ochrony danych programu ASP.NET Core.
ms.author: riande
ms.date: 10/14/2016
uid: security/data-protection/implementation/context-headers
ms.openlocfilehash: 2b8fd594672bf623d38bfae90d05a984f92ce6a3
ms.sourcegitcommit: dd9c73db7853d87b566eef136d2162f648a43b85
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/06/2019
ms.locfileid: "65087563"
---
# <a name="context-headers-in-aspnet-core"></a>Nagłówki kontekstu w programie ASP.NET Core

<a name="data-protection-implementation-context-headers"></a>

## <a name="background-and-theory"></a>Tło i teorii

W systemie ochrony danych "key" oznacza, że obiekt, który zapewnia uwierzytelniony usługi szyfrowania. Każdy klucz jest identyfikowane za pomocą unikatowego identyfikatora (GUID), a także niesie ze sobą konsolidatorze informacji i entropic materiałów. Jest ona przeznaczona, każdy klucz przenoszenia entropii unikatowy, ale system nie może wymusić, i musimy również konta dla deweloperów, którzy mogą ulec zmianie pierścień klucz ręcznie, modyfikując konsolidatorze informacje istniejącego klucza w pierścienia klucza. Do osiągnięcia naszych wymagań w zakresie zabezpieczeń podane w tych przypadkach system ochrony danych utworzono według koncepcji [zręczność kryptograficzna](https://www.microsoft.com/en-us/research/publication/cryptographic-agility-and-its-relation-to-circular-encryption/), co pozwala bezpiecznie przy użyciu pojedynczej wartości entropic dla wielu algorytmów kryptograficznych.

Większość systemów, które obsługują zręczność kryptograficzna w tym celu w tym niektóre informacje identyfikujące algorytm wewnątrz ładunku. Identyfikator OID algorytmu ogólnie jest dobrym kandydatem do tego. Jednak jeden problem, który wystąpił jest, że istnieje wiele sposobów, aby określić, to ten sam algorytm: "AES" (CNG) i zarządzanych Aes, AesManaged, AesCryptoServiceProvider, AesCng i RijndaelManaged (biorąc pod uwagę określone parametry) klasy są wszystkie faktycznie ten sam efekt, a firma Microsoft będzie trzeba utrzymywać mapowania wszystkich tych na prawidłowy identyfikator OID. Jeśli Deweloper potrzebowała niestandardowego algorytmu (lub nawet inny implementacji AES!), zostałyby Powiedz nam, jego identyfikator OID. W tym kroku rejestracji dodatkowe sprawia, że konfiguracja systemu jest szczególnie trudne.

Cofanie, zdecydowaliśmy się, że firma Microsoft były zbliża się problem ze złym kierunku. Identyfikator OID informuje, co to jest algorytm, ale faktycznie nie Dbamy o to. Jeśli będziemy potrzebować bezpieczne korzystanie z pojedynczą wartość entropic w dwóch różnych algorytmów, nie jest konieczne nam znać, co faktycznie są algorytmy. Co faktycznie Dbamy o to, jak działają. Dowolny algorytm szyfrowania znośnego symetrycznego bloku jest również silne permutacji pseudolosowych (PRP): napraw (klucz łańcucha zwykłego tekstu w trybie IV) dane wejściowe i dane wyjściowe szyfrowany za pomocą przeciążenia prawdopodobieństwo będą różne od innych szyfrowania symetrycznego bloku Algorytm podane tych samych danych wejściowych. Podobnie dowolnej funkcji znośnego kluczem skrótu jest również pseudolosowych funkcję silne (PRF), a podany stały zestaw danych wejściowych jego dane wyjściowe znaczna będą różne od innych funkcji kluczem skrótu.

Takie podejście do silnych PRPs i PRFs są używane do zbudowania nagłówka kontekstu. Ten nagłówek kontekstu zasadniczo działa jako stabilny odcisk palca za pośrednictwem algorytmy, używany w kontekście danej operacji i zapewnia zręczność kryptograficzna wymagane przez system ochrony danych. Ten nagłówek zostanie odtworzony i jest używany później jako część [podklucza procesu pochodnym](xref:security/data-protection/implementation/subkeyderivation#data-protection-implementation-subkey-derivation). Istnieją dwa różne sposoby kompilacji nagłówka kontekstu, w zależności od tryby działania podstawowej algorytmów.

## <a name="cbc-mode-encryption--hmac-authentication"></a>Trybie CBC szyfrowanie + uwierzytelnianie HMAC

<a name="data-protection-implementation-context-headers-cbc-components"></a>

Nagłówek kontekst składa się z następujących składników:

* [16 bitów] Wartość 00 00, czyli znacznik oznacza "CBC szyfrowanie + uwierzytelnianie HMAC".

* [32-bitowy] Długość klucza (w bajtach, big-endian) algorytmu szyfrowania symetrycznego bloku.

* [32-bitowy] Rozmiar bloku (w bajtach, big-endian) algorytmu szyfrowania symetrycznego bloku.

* [32-bitowy] Długość klucza (w bajtach, big-endian) z algorytmem HMAC. (Obecnie rozmiar klucza zawsze zgodny rozmiar szyfrowanego.)

* [32-bitowy] Podsumowanie rozmiar (w bajtach, big-endian) algorytmem HMAC.

* EncCBC (K_E, IV ""), które znajdują się dane wyjściowe algorytmu szyfrowania symetrycznego bloku podane pusty ciąg wejściowy i gdzie IV jest wektor wszystko od zera. Konstrukcja K_E opisano poniżej.

* MAC (K_H, ""), czyli danych wyjściowych algorytmem HMAC podane dane wejściowe pusty ciąg. Konstrukcja K_H opisano poniżej.

W idealnym przypadku K_E i K_H można przekazywać wektorów wszystko od zera. Jednakże chcemy uniknąć sytuacji, w której algorytm sprawdza istnienie słabe kluczy przed wykonaniem jakiejkolwiek operacji (szczególnie DES i 3DES), co wyklucza przy użyciu prostego lub repeatable wzorca takich jak wektor wszystko od zera.

Zamiast tego stosujemy KDF SP800 108 NIST w trybie licznika (zobacz [NIST SP800-108](http://nvlpubs.nist.gov/nistpubs/Legacy/SP/nistspecialpublication800-108.pdf), s. 5.1) z kluczem o zerowej długości, etykiety i kontekstu i HMACSHA512 jako podstawowej PRF. Pobieramy | K_E | + | K_H | bajty danych wyjściowych, następnie rozłożyć wynik K_E i K_H samodzielnie. Matematycznych jest reprezentowane w następujący sposób.

(K_E || K_H) = SP800_108_CTR (prf = HMACSHA512, key = "", label = "", kontekst = "")

### <a name="example-aes-192-cbc--hmacsha256"></a>Przykład: AES-192-CBC + HMACSHA256

Na przykład należy rozważyć przypadek, gdzie algorytmu szyfrowania symetrycznego bloku jest 192-AES-CBC, a algorytm sprawdzania poprawności jest HMACSHA256. System wygeneruje nagłówka kontekstu, wykonując następujące kroki.

Najpierw należy umożliwić (K_E || K_H) = SP800_108_CTR (prf = HMACSHA512, key = "", label = "", kontekst = ""), gdzie | K_E | = Dzięki 192-bajtowemu i | K_H | = 256 bitów na określonym algorytmów. Prowadzi to do K_E = 5BB6... 21DD i K_H = A04A... 00A9 w poniższym przykładzie:

```
5B B6 C9 83 13 78 22 1D 8E 10 73 CA CF 65 8E B0
61 62 42 71 CB 83 21 DD A0 4A 05 00 5B AB C0 A2
49 6F A5 61 E3 E2 49 87 AA 63 55 CD 74 0A DA C4
B7 92 3D BF 59 90 00 A9
```

Następnie obliczenia Enc_CBC (K_E, IV "") dla AES-192-CBC, biorąc pod uwagę IV = 0 * i K_E opisanych powyżej.

result := F474B1872B3B53E4721DE19C0841DB6F

Następnie obliczenia MAC (K_H, "") dla HMACSHA256, biorąc pod uwagę K_H opisanych powyżej.

result := D4791184B996092EE1202F36E8608FA8FBD98ABDFF5402F264B1D7211536220C

To daje poniżej nagłówka pełnego kontekstu:

```
00 00 00 00 00 18 00 00 00 10 00 00 00 20 00 00
00 20 F4 74 B1 87 2B 3B 53 E4 72 1D E1 9C 08 41
DB 6F D4 79 11 84 B9 96 09 2E E1 20 2F 36 E8 60
8F A8 FB D9 8A BD FF 54 02 F2 64 B1 D7 21 15 36
22 0C
```

Kontekst tego pliku nagłówkowego to odcisk palca pary algorytmu uwierzytelnione szyfrowanie (szyfrowanie AES-192-CBC + weryfikacji HMACSHA256). Składniki, zgodnie z opisem [powyżej](xref:security/data-protection/implementation/context-headers#data-protection-implementation-context-headers-cbc-components) są:

* znacznika (00 00)

* długość klucza szyfrowania bloku (00 00 00 18)

* rozmiar bloku cipher block (00 00 00 10)

* długość klucza HMAC (00 00 00 20)

* rozmiar skrótu HMAC (00 00 00 20)

* szyfrowania bloku danych wyjściowych zasady replikacji HASEŁ (F4 74 - DB 6 f) i

* dane wyjściowe HMAC PRF (D4 79 - end).

> [!NOTE]
> Szyfrowanie w trybie CBC + HMAC nagłówka kontekstu uwierzytelniania zaprojektowano w taki sam sposób niezależnie od tego, czy implementacje algorytmy są dostarczane przez Windows CNG lub typami zarządzanymi SymmetricAlgorithm i KeyedHashAlgorithm. Dzięki temu działających w różnych systemach operacyjnych umożliwiające niezawodne uzyskiwanie ten sam nagłówek kontekstu, nawet jeśli implementacje algorytmy różnią się w systemach operacyjnych. (W praktyce KeyedHashAlgorithm nie musi być odpowiednie HMAC. Może być dowolnego typu algorytmu wyznaczania wartości skrótu w kluczu.)

### <a name="example-3des-192-cbc--hmacsha1"></a>Przykład: 3DES-192-CBC + HMACSHA1

Najpierw należy umożliwić (K_E || K_H) = SP800_108_CTR (prf = HMACSHA512, key = "", label = "", kontekst = ""), gdzie | K_E | = Dzięki 192-bajtowemu i | K_H | = 160-bitowy określonego algorytmów. Prowadzi to do K_E = A219... E2BB i K_H = DC4A... B464 w poniższym przykładzie:

```
A2 19 60 2F 83 A9 13 EA B0 61 3A 39 B8 A6 7E 22
61 D9 F8 6C 10 51 E2 BB DC 4A 00 D7 03 A2 48 3E
D1 F7 5A 34 EB 28 3E D7 D4 67 B4 64
```

Następnie obliczenia Enc_CBC (K_E, IV "") dla algorytmu 3DES-192-CBC podane IV = 0 * i K_E opisanych powyżej.

result := ABB100F81E53E10E

Następnie obliczenia MAC (K_H, "") dla HMACSHA1 podane K_H opisanych powyżej.

result := 76EB189B35CF03461DDF877CD9F4B1B4D63A7555

Daje nagłówka pełny kontekst, który jest odcisk palca uwierzytelnionego szyfrowania algorytm pary (szyfrowania 3DES-192-CBC + weryfikacji HMACSHA1), pokazano poniżej:

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

* szyfrowania bloku danych wyjściowych zasady replikacji HASEŁ (AB B1 - E1 0E) i

* dane wyjściowe HMAC PRF (76 EB - end).

## <a name="galoiscounter-mode-encryption--authentication"></a>Galois/licznik tryb szyfrowania i uwierzytelniania

Nagłówek kontekst składa się z następujących składników:

* [16 bitów] Wartość 00 01, który jest znacznik oznacza "GCM szyfrowanie + uwierzytelnianie".

* [32-bitowy] Długość klucza (w bajtach, big-endian) algorytmu szyfrowania symetrycznego bloku.

* [32-bitowy] Jednorazowy rozmiar (w bajtach, big-endian) używane podczas operacji uwierzytelnionego szyfrowania. (Dla naszego systemu problem ten został rozwiązany w rozmiarze nonce = 96 bitów.)

* [32-bitowy] Rozmiar bloku (w bajtach, big-endian) algorytmu szyfrowania symetrycznego bloku. (Dla usługi GCM, problem ten został rozwiązany w rozmiarze bloku = 128 bitów.)

* [32-bitowy] Uwierzytelnianie tag rozmiar (w bajtach, big-endian) utworzonej przez funkcję uwierzytelnione szyfrowanie. (Dla naszego systemu problem ten został rozwiązany w rozmiarze tag = 128 bitów.)

* [128 bitów] Tag Enc_GCM (K_E jednorazowy, ""), które znajdują się dane wyjściowe algorytmu szyfrowania symetrycznego bloku podane pusty ciąg wejściowy i których identyfikator jednorazowy jest wektorem zero wszystkie 96-bitowym.

K_E jest uzyskiwany za pomocą ten sam mechanizm, tak jak szyfrowanie CBC + HMAC scenariusza uwierzytelniania. Jednakże, ponieważ w tym miejscu play nie K_H, zasadniczo mamy | K_H | = 0, a algorytm jest ustawiana na poniżej formularza.

K_E = SP800_108_CTR (prf = HMACSHA512, key = "", label = "", kontekst = "")

### <a name="example-aes-256-gcm"></a>Przykład: AES-256-GCM

Najpierw należy umożliwić K_E = SP800_108_CTR (prf = HMACSHA512, key = "", etykieta = "", kontekst = ""), gdzie | K_E | = 256 bitów.

K_E := 22BC6F1B171C08C4AE2F27444AF8FC8B3087A90006CAEA91FDCFB47C1B8733B8

Następnie obliczenia tag uwierzytelniania Enc_GCM (K_E jednorazowy, "") dla standardu AES-256-GCM podany identyfikator jednorazowy = 096 i K_E opisanych powyżej.

result := E7DCCE66DF855A323A6BB7BD7A59BE45

To daje poniżej nagłówka pełnego kontekstu:

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
