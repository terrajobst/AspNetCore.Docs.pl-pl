---
title: Nagłówki kontekstu w programie ASP.NET Core
author: rick-anderson
description: Dowiedz się, szczegóły implementacji nagłówki kontekstu ochrony danych programu ASP.NET Core.
ms.author: riande
ms.date: 10/14/2016
uid: security/data-protection/implementation/context-headers
ms.openlocfilehash: 2b8fd594672bf623d38bfae90d05a984f92ce6a3
ms.sourcegitcommit: 5f299daa7c8102d56a63b214b9a34cc4bc87bc42
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/19/2019
ms.locfileid: "58208369"
---
# <a name="context-headers-in-aspnet-core"></a><span data-ttu-id="524f7-103">Nagłówki kontekstu w programie ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="524f7-103">Context headers in ASP.NET Core</span></span>

<a name="data-protection-implementation-context-headers"></a>

## <a name="background-and-theory"></a><span data-ttu-id="524f7-104">Tło i teorii</span><span class="sxs-lookup"><span data-stu-id="524f7-104">Background and theory</span></span>

<span data-ttu-id="524f7-105">W systemie ochrony danych "key" oznacza, że obiekt, który zapewnia uwierzytelniony usługi szyfrowania.</span><span class="sxs-lookup"><span data-stu-id="524f7-105">In the data protection system, a "key" means an object that can provide authenticated encryption services.</span></span> <span data-ttu-id="524f7-106">Każdy klucz jest identyfikowane za pomocą unikatowego identyfikatora (GUID), a także niesie ze sobą konsolidatorze informacji i entropic materiałów.</span><span class="sxs-lookup"><span data-stu-id="524f7-106">Each key is identified by a unique id (a GUID), and it carries with it algorithmic information and entropic material.</span></span> <span data-ttu-id="524f7-107">Jest ona przeznaczona, każdy klucz przenoszenia entropii unikatowy, ale system nie może wymusić, i musimy również konta dla deweloperów, którzy mogą ulec zmianie pierścień klucz ręcznie, modyfikując konsolidatorze informacje istniejącego klucza w pierścienia klucza.</span><span class="sxs-lookup"><span data-stu-id="524f7-107">It's intended that each key carry unique entropy, but the system cannot enforce that, and we also need to account for developers who might change the key ring manually by modifying the algorithmic information of an existing key in the key ring.</span></span> <span data-ttu-id="524f7-108">Do osiągnięcia naszych wymagań w zakresie zabezpieczeń podane w tych przypadkach system ochrony danych utworzono według koncepcji [zręczność kryptograficzna](https://www.microsoft.com/en-us/research/publication/cryptographic-agility-and-its-relation-to-circular-encryption/), co pozwala bezpiecznie przy użyciu pojedynczej wartości entropic dla wielu algorytmów kryptograficznych.</span><span class="sxs-lookup"><span data-stu-id="524f7-108">To achieve our security requirements given these cases the data protection system has a concept of [cryptographic agility](https://www.microsoft.com/en-us/research/publication/cryptographic-agility-and-its-relation-to-circular-encryption/), which allows securely using a single entropic value across multiple cryptographic algorithms.</span></span>

<span data-ttu-id="524f7-109">Większość systemów, które obsługują zręczność kryptograficzna w tym celu w tym niektóre informacje identyfikujące algorytm wewnątrz ładunku.</span><span class="sxs-lookup"><span data-stu-id="524f7-109">Most systems which support cryptographic agility do so by including some identifying information about the algorithm inside the payload.</span></span> <span data-ttu-id="524f7-110">Identyfikator OID algorytmu ogólnie jest dobrym kandydatem do tego.</span><span class="sxs-lookup"><span data-stu-id="524f7-110">The algorithm's OID is generally a good candidate for this.</span></span> <span data-ttu-id="524f7-111">Jednak jeden problem, który wystąpił jest, że istnieje wiele sposobów, aby określić, to ten sam algorytm: "AES" (CNG) i zarządzanych Aes, AesManaged, AesCryptoServiceProvider, AesCng i RijndaelManaged (biorąc pod uwagę określone parametry) klasy są wszystkie faktycznie ten sam efekt, a firma Microsoft będzie trzeba utrzymywać mapowania wszystkich tych na prawidłowy identyfikator OID.</span><span class="sxs-lookup"><span data-stu-id="524f7-111">However, one problem that we ran into is that there are multiple ways to specify the same algorithm: "AES" (CNG) and the managed Aes, AesManaged, AesCryptoServiceProvider, AesCng, and RijndaelManaged (given specific parameters) classes are all actually the same thing, and we'd need to maintain a mapping of all of these to the correct OID.</span></span> <span data-ttu-id="524f7-112">Jeśli Deweloper potrzebowała niestandardowego algorytmu (lub nawet inny implementacji AES!), zostałyby Powiedz nam, jego identyfikator OID.</span><span class="sxs-lookup"><span data-stu-id="524f7-112">If a developer wanted to provide a custom algorithm (or even another implementation of AES!), they'd have to tell us its OID.</span></span> <span data-ttu-id="524f7-113">W tym kroku rejestracji dodatkowe sprawia, że konfiguracja systemu jest szczególnie trudne.</span><span class="sxs-lookup"><span data-stu-id="524f7-113">This extra registration step makes system configuration particularly painful.</span></span>

<span data-ttu-id="524f7-114">Cofanie, zdecydowaliśmy się, że firma Microsoft były zbliża się problem ze złym kierunku.</span><span class="sxs-lookup"><span data-stu-id="524f7-114">Stepping back, we decided that we were approaching the problem from the wrong direction.</span></span> <span data-ttu-id="524f7-115">Identyfikator OID informuje, co to jest algorytm, ale faktycznie nie Dbamy o to.</span><span class="sxs-lookup"><span data-stu-id="524f7-115">An OID tells you what the algorithm is, but we don't actually care about this.</span></span> <span data-ttu-id="524f7-116">Jeśli będziemy potrzebować bezpieczne korzystanie z pojedynczą wartość entropic w dwóch różnych algorytmów, nie jest konieczne nam znać, co faktycznie są algorytmy.</span><span class="sxs-lookup"><span data-stu-id="524f7-116">If we need to use a single entropic value securely in two different algorithms, it's not necessary for us to know what the algorithms actually are.</span></span> <span data-ttu-id="524f7-117">Co faktycznie Dbamy o to, jak działają.</span><span class="sxs-lookup"><span data-stu-id="524f7-117">What we actually care about is how they behave.</span></span> <span data-ttu-id="524f7-118">Dowolny algorytm szyfrowania znośnego symetrycznego bloku jest również silne permutacji pseudolosowych (PRP): napraw (klucz łańcucha zwykłego tekstu w trybie IV) dane wejściowe i dane wyjściowe szyfrowany za pomocą przeciążenia prawdopodobieństwo będą różne od innych szyfrowania symetrycznego bloku Algorytm podane tych samych danych wejściowych.</span><span class="sxs-lookup"><span data-stu-id="524f7-118">Any decent symmetric block cipher algorithm is also a strong pseudorandom permutation (PRP): fix the inputs (key, chaining mode, IV, plaintext) and the ciphertext output will with overwhelming probability be distinct from any other symmetric block cipher algorithm given the same inputs.</span></span> <span data-ttu-id="524f7-119">Podobnie dowolnej funkcji znośnego kluczem skrótu jest również pseudolosowych funkcję silne (PRF), a podany stały zestaw danych wejściowych jego dane wyjściowe znaczna będą różne od innych funkcji kluczem skrótu.</span><span class="sxs-lookup"><span data-stu-id="524f7-119">Similarly, any decent keyed hash function is also a strong pseudorandom function (PRF), and given a fixed input set its output will overwhelmingly be distinct from any other keyed hash function.</span></span>

<span data-ttu-id="524f7-120">Takie podejście do silnych PRPs i PRFs są używane do zbudowania nagłówka kontekstu.</span><span class="sxs-lookup"><span data-stu-id="524f7-120">We use this concept of strong PRPs and PRFs to build up a context header.</span></span> <span data-ttu-id="524f7-121">Ten nagłówek kontekstu zasadniczo działa jako stabilny odcisk palca za pośrednictwem algorytmy, używany w kontekście danej operacji i zapewnia zręczność kryptograficzna wymagane przez system ochrony danych.</span><span class="sxs-lookup"><span data-stu-id="524f7-121">This context header essentially acts as a stable thumbprint over the algorithms in use for any given operation, and it provides the cryptographic agility needed by the data protection system.</span></span> <span data-ttu-id="524f7-122">Ten nagłówek zostanie odtworzony i jest używany później jako część [podklucza procesu pochodnym](xref:security/data-protection/implementation/subkeyderivation#data-protection-implementation-subkey-derivation).</span><span class="sxs-lookup"><span data-stu-id="524f7-122">This header is reproducible and is used later as part of the [subkey derivation process](xref:security/data-protection/implementation/subkeyderivation#data-protection-implementation-subkey-derivation).</span></span> <span data-ttu-id="524f7-123">Istnieją dwa różne sposoby kompilacji nagłówka kontekstu, w zależności od tryby działania podstawowej algorytmów.</span><span class="sxs-lookup"><span data-stu-id="524f7-123">There are two different ways to build the context header depending on the modes of operation of the underlying algorithms.</span></span>

## <a name="cbc-mode-encryption--hmac-authentication"></a><span data-ttu-id="524f7-124">Trybie CBC szyfrowanie + uwierzytelnianie HMAC</span><span class="sxs-lookup"><span data-stu-id="524f7-124">CBC-mode encryption + HMAC authentication</span></span>

<a name="data-protection-implementation-context-headers-cbc-components"></a>

<span data-ttu-id="524f7-125">Nagłówek kontekst składa się z następujących składników:</span><span class="sxs-lookup"><span data-stu-id="524f7-125">The context header consists of the following components:</span></span>

* <span data-ttu-id="524f7-126">[16 bitów] Wartość 00 00, czyli znacznik oznacza "CBC szyfrowanie + uwierzytelnianie HMAC".</span><span class="sxs-lookup"><span data-stu-id="524f7-126">[16 bits] The value 00 00, which is a marker meaning "CBC encryption + HMAC authentication".</span></span>

* <span data-ttu-id="524f7-127">[32-bitowy] Długość klucza (w bajtach, big-endian) algorytmu szyfrowania symetrycznego bloku.</span><span class="sxs-lookup"><span data-stu-id="524f7-127">[32 bits] The key length (in bytes, big-endian) of the symmetric block cipher algorithm.</span></span>

* <span data-ttu-id="524f7-128">[32-bitowy] Rozmiar bloku (w bajtach, big-endian) algorytmu szyfrowania symetrycznego bloku.</span><span class="sxs-lookup"><span data-stu-id="524f7-128">[32 bits] The block size (in bytes, big-endian) of the symmetric block cipher algorithm.</span></span>

* <span data-ttu-id="524f7-129">[32-bitowy] Długość klucza (w bajtach, big-endian) z algorytmem HMAC.</span><span class="sxs-lookup"><span data-stu-id="524f7-129">[32 bits] The key length (in bytes, big-endian) of the HMAC algorithm.</span></span> <span data-ttu-id="524f7-130">(Obecnie rozmiar klucza zawsze zgodny rozmiar szyfrowanego.)</span><span class="sxs-lookup"><span data-stu-id="524f7-130">(Currently the key size always matches the digest size.)</span></span>

* <span data-ttu-id="524f7-131">[32-bitowy] Podsumowanie rozmiar (w bajtach, big-endian) algorytmem HMAC.</span><span class="sxs-lookup"><span data-stu-id="524f7-131">[32 bits] The digest size (in bytes, big-endian) of the HMAC algorithm.</span></span>

* <span data-ttu-id="524f7-132">EncCBC (K_E, IV ""), które znajdują się dane wyjściowe algorytmu szyfrowania symetrycznego bloku podane pusty ciąg wejściowy i gdzie IV jest wektor wszystko od zera.</span><span class="sxs-lookup"><span data-stu-id="524f7-132">EncCBC(K_E, IV, ""), which is the output of the symmetric block cipher algorithm given an empty string input and where IV is an all-zero vector.</span></span> <span data-ttu-id="524f7-133">Konstrukcja K_E opisano poniżej.</span><span class="sxs-lookup"><span data-stu-id="524f7-133">The construction of K_E is described below.</span></span>

* <span data-ttu-id="524f7-134">MAC (K_H, ""), czyli danych wyjściowych algorytmem HMAC podane dane wejściowe pusty ciąg.</span><span class="sxs-lookup"><span data-stu-id="524f7-134">MAC(K_H, ""), which is the output of the HMAC algorithm given an empty string input.</span></span> <span data-ttu-id="524f7-135">Konstrukcja K_H opisano poniżej.</span><span class="sxs-lookup"><span data-stu-id="524f7-135">The construction of K_H is described below.</span></span>

<span data-ttu-id="524f7-136">W idealnym przypadku K_E i K_H można przekazywać wektorów wszystko od zera.</span><span class="sxs-lookup"><span data-stu-id="524f7-136">Ideally, we could pass all-zero vectors for K_E and K_H.</span></span> <span data-ttu-id="524f7-137">Jednakże chcemy uniknąć sytuacji, w której algorytm sprawdza istnienie słabe kluczy przed wykonaniem jakiejkolwiek operacji (szczególnie DES i 3DES), co wyklucza przy użyciu prostego lub repeatable wzorca takich jak wektor wszystko od zera.</span><span class="sxs-lookup"><span data-stu-id="524f7-137">However, we want to avoid the situation where the underlying algorithm checks for the existence of weak keys before performing any operations (notably DES and 3DES), which precludes using a simple or repeatable pattern like an all-zero vector.</span></span>

<span data-ttu-id="524f7-138">Zamiast tego stosujemy KDF SP800 108 NIST w trybie licznika (zobacz [NIST SP800-108](http://nvlpubs.nist.gov/nistpubs/Legacy/SP/nistspecialpublication800-108.pdf), s. 5.1) z kluczem o zerowej długości, etykiety i kontekstu i HMACSHA512 jako podstawowej PRF.</span><span class="sxs-lookup"><span data-stu-id="524f7-138">Instead, we use the NIST SP800-108 KDF in Counter Mode (see [NIST SP800-108](http://nvlpubs.nist.gov/nistpubs/Legacy/SP/nistspecialpublication800-108.pdf), Sec. 5.1) with a zero-length key, label, and context and HMACSHA512 as the underlying PRF.</span></span> <span data-ttu-id="524f7-139">Pobieramy | K_E | + | K_H | bajty danych wyjściowych, następnie rozłożyć wynik K_E i K_H samodzielnie.</span><span class="sxs-lookup"><span data-stu-id="524f7-139">We derive | K_E | + | K_H | bytes of output, then decompose the result into K_E and K_H themselves.</span></span> <span data-ttu-id="524f7-140">Matematycznych jest reprezentowane w następujący sposób.</span><span class="sxs-lookup"><span data-stu-id="524f7-140">Mathematically, this is represented as follows.</span></span>

<span data-ttu-id="524f7-141">(K_E || K_H) = SP800_108_CTR (prf = HMACSHA512, key = "", label = "", kontekst = "")</span><span class="sxs-lookup"><span data-stu-id="524f7-141">( K_E || K_H ) = SP800_108_CTR(prf = HMACSHA512, key = "", label = "", context = "")</span></span>

### <a name="example-aes-192-cbc--hmacsha256"></a><span data-ttu-id="524f7-142">Przykład: AES-192-CBC + HMACSHA256</span><span class="sxs-lookup"><span data-stu-id="524f7-142">Example: AES-192-CBC + HMACSHA256</span></span>

<span data-ttu-id="524f7-143">Na przykład należy rozważyć przypadek, gdzie algorytmu szyfrowania symetrycznego bloku jest 192-AES-CBC, a algorytm sprawdzania poprawności jest HMACSHA256.</span><span class="sxs-lookup"><span data-stu-id="524f7-143">As an example, consider the case where the symmetric block cipher algorithm is AES-192-CBC and the validation algorithm is HMACSHA256.</span></span> <span data-ttu-id="524f7-144">System wygeneruje nagłówka kontekstu, wykonując następujące kroki.</span><span class="sxs-lookup"><span data-stu-id="524f7-144">The system would generate the context header using the following steps.</span></span>

<span data-ttu-id="524f7-145">Najpierw należy umożliwić (K_E || K_H) = SP800_108_CTR (prf = HMACSHA512, key = "", label = "", kontekst = ""), gdzie | K_E | = Dzięki 192-bajtowemu i | K_H | = 256 bitów na określonym algorytmów.</span><span class="sxs-lookup"><span data-stu-id="524f7-145">First, let ( K_E || K_H ) = SP800_108_CTR(prf = HMACSHA512, key = "", label = "", context = ""), where | K_E | = 192 bits and | K_H | = 256 bits per the specified algorithms.</span></span> <span data-ttu-id="524f7-146">Prowadzi to do K_E = 5BB6... 21DD i K_H = A04A... 00A9 w poniższym przykładzie:</span><span class="sxs-lookup"><span data-stu-id="524f7-146">This leads to K_E = 5BB6..21DD and K_H = A04A..00A9 in the example below:</span></span>

```
5B B6 C9 83 13 78 22 1D 8E 10 73 CA CF 65 8E B0
61 62 42 71 CB 83 21 DD A0 4A 05 00 5B AB C0 A2
49 6F A5 61 E3 E2 49 87 AA 63 55 CD 74 0A DA C4
B7 92 3D BF 59 90 00 A9
```

<span data-ttu-id="524f7-147">Następnie obliczenia Enc_CBC (K_E, IV "") dla AES-192-CBC, biorąc pod uwagę IV = 0 \* i K_E opisanych powyżej.</span><span class="sxs-lookup"><span data-stu-id="524f7-147">Next, compute Enc_CBC (K_E, IV, "") for AES-192-CBC given IV = 0\* and K_E as above.</span></span>

<span data-ttu-id="524f7-148">result := F474B1872B3B53E4721DE19C0841DB6F</span><span class="sxs-lookup"><span data-stu-id="524f7-148">result := F474B1872B3B53E4721DE19C0841DB6F</span></span>

<span data-ttu-id="524f7-149">Następnie obliczenia MAC (K_H, "") dla HMACSHA256, biorąc pod uwagę K_H opisanych powyżej.</span><span class="sxs-lookup"><span data-stu-id="524f7-149">Next, compute MAC(K_H, "") for HMACSHA256 given K_H as above.</span></span>

<span data-ttu-id="524f7-150">result := D4791184B996092EE1202F36E8608FA8FBD98ABDFF5402F264B1D7211536220C</span><span class="sxs-lookup"><span data-stu-id="524f7-150">result := D4791184B996092EE1202F36E8608FA8FBD98ABDFF5402F264B1D7211536220C</span></span>

<span data-ttu-id="524f7-151">To daje poniżej nagłówka pełnego kontekstu:</span><span class="sxs-lookup"><span data-stu-id="524f7-151">This produces the full context header below:</span></span>

```
00 00 00 00 00 18 00 00 00 10 00 00 00 20 00 00
00 20 F4 74 B1 87 2B 3B 53 E4 72 1D E1 9C 08 41
DB 6F D4 79 11 84 B9 96 09 2E E1 20 2F 36 E8 60
8F A8 FB D9 8A BD FF 54 02 F2 64 B1 D7 21 15 36
22 0C
```

<span data-ttu-id="524f7-152">Kontekst tego pliku nagłówkowego to odcisk palca pary algorytmu uwierzytelnione szyfrowanie (szyfrowanie AES-192-CBC + weryfikacji HMACSHA256).</span><span class="sxs-lookup"><span data-stu-id="524f7-152">This context header is the thumbprint of the authenticated encryption algorithm pair (AES-192-CBC encryption + HMACSHA256 validation).</span></span> <span data-ttu-id="524f7-153">Składniki, zgodnie z opisem [powyżej](xref:security/data-protection/implementation/context-headers#data-protection-implementation-context-headers-cbc-components) są:</span><span class="sxs-lookup"><span data-stu-id="524f7-153">The components, as described [above](xref:security/data-protection/implementation/context-headers#data-protection-implementation-context-headers-cbc-components) are:</span></span>

* <span data-ttu-id="524f7-154">znacznika (00 00)</span><span class="sxs-lookup"><span data-stu-id="524f7-154">the marker (00 00)</span></span>

* <span data-ttu-id="524f7-155">długość klucza szyfrowania bloku (00 00 00 18)</span><span class="sxs-lookup"><span data-stu-id="524f7-155">the block cipher key length (00 00 00 18)</span></span>

* <span data-ttu-id="524f7-156">rozmiar bloku cipher block (00 00 00 10)</span><span class="sxs-lookup"><span data-stu-id="524f7-156">the block cipher block size (00 00 00 10)</span></span>

* <span data-ttu-id="524f7-157">długość klucza HMAC (00 00 00 20)</span><span class="sxs-lookup"><span data-stu-id="524f7-157">the HMAC key length (00 00 00 20)</span></span>

* <span data-ttu-id="524f7-158">rozmiar skrótu HMAC (00 00 00 20)</span><span class="sxs-lookup"><span data-stu-id="524f7-158">the HMAC digest size (00 00 00 20)</span></span>

* <span data-ttu-id="524f7-159">szyfrowania bloku danych wyjściowych zasady replikacji HASEŁ (F4 74 - DB 6 f) i</span><span class="sxs-lookup"><span data-stu-id="524f7-159">the block cipher PRP output (F4 74 - DB 6F) and</span></span>

* <span data-ttu-id="524f7-160">dane wyjściowe HMAC PRF (D4 79 - end).</span><span class="sxs-lookup"><span data-stu-id="524f7-160">the HMAC PRF output (D4 79 - end).</span></span>

> [!NOTE]
> <span data-ttu-id="524f7-161">Szyfrowanie w trybie CBC + HMAC nagłówka kontekstu uwierzytelniania zaprojektowano w taki sam sposób niezależnie od tego, czy implementacje algorytmy są dostarczane przez Windows CNG lub typami zarządzanymi SymmetricAlgorithm i KeyedHashAlgorithm.</span><span class="sxs-lookup"><span data-stu-id="524f7-161">The CBC-mode encryption + HMAC authentication context header is built the same way regardless of whether the algorithms implementations are provided by Windows CNG or by managed SymmetricAlgorithm and KeyedHashAlgorithm types.</span></span> <span data-ttu-id="524f7-162">Dzięki temu działających w różnych systemach operacyjnych umożliwiające niezawodne uzyskiwanie ten sam nagłówek kontekstu, nawet jeśli implementacje algorytmy różnią się w systemach operacyjnych.</span><span class="sxs-lookup"><span data-stu-id="524f7-162">This allows applications running on different operating systems to reliably produce the same context header even though the implementations of the algorithms differ between OSes.</span></span> <span data-ttu-id="524f7-163">(W praktyce KeyedHashAlgorithm nie musi być odpowiednie HMAC.</span><span class="sxs-lookup"><span data-stu-id="524f7-163">(In practice, the KeyedHashAlgorithm doesn't have to be a proper HMAC.</span></span> <span data-ttu-id="524f7-164">Może być dowolnego typu algorytmu wyznaczania wartości skrótu w kluczu.)</span><span class="sxs-lookup"><span data-stu-id="524f7-164">It can be any keyed hash algorithm type.)</span></span>

### <a name="example-3des-192-cbc--hmacsha1"></a><span data-ttu-id="524f7-165">Przykład: 3DES-192-CBC + HMACSHA1</span><span class="sxs-lookup"><span data-stu-id="524f7-165">Example: 3DES-192-CBC + HMACSHA1</span></span>

<span data-ttu-id="524f7-166">Najpierw należy umożliwić (K_E || K_H) = SP800_108_CTR (prf = HMACSHA512, key = "", label = "", kontekst = ""), gdzie | K_E | = Dzięki 192-bajtowemu i | K_H | = 160-bitowy określonego algorytmów.</span><span class="sxs-lookup"><span data-stu-id="524f7-166">First, let ( K_E || K_H ) = SP800_108_CTR(prf = HMACSHA512, key = "", label = "", context = ""), where | K_E | = 192 bits and | K_H | = 160 bits per the specified algorithms.</span></span> <span data-ttu-id="524f7-167">Prowadzi to do K_E = A219... E2BB i K_H = DC4A... B464 w poniższym przykładzie:</span><span class="sxs-lookup"><span data-stu-id="524f7-167">This leads to K_E = A219..E2BB and K_H = DC4A..B464 in the example below:</span></span>

```
A2 19 60 2F 83 A9 13 EA B0 61 3A 39 B8 A6 7E 22
61 D9 F8 6C 10 51 E2 BB DC 4A 00 D7 03 A2 48 3E
D1 F7 5A 34 EB 28 3E D7 D4 67 B4 64
```

<span data-ttu-id="524f7-168">Następnie obliczenia Enc_CBC (K_E, IV "") dla algorytmu 3DES-192-CBC podane IV = 0 \* i K_E opisanych powyżej.</span><span class="sxs-lookup"><span data-stu-id="524f7-168">Next, compute Enc_CBC (K_E, IV, "") for 3DES-192-CBC given IV = 0\* and K_E as above.</span></span>

<span data-ttu-id="524f7-169">result := ABB100F81E53E10E</span><span class="sxs-lookup"><span data-stu-id="524f7-169">result := ABB100F81E53E10E</span></span>

<span data-ttu-id="524f7-170">Następnie obliczenia MAC (K_H, "") dla HMACSHA1 podane K_H opisanych powyżej.</span><span class="sxs-lookup"><span data-stu-id="524f7-170">Next, compute MAC(K_H, "") for HMACSHA1 given K_H as above.</span></span>

<span data-ttu-id="524f7-171">result := 76EB189B35CF03461DDF877CD9F4B1B4D63A7555</span><span class="sxs-lookup"><span data-stu-id="524f7-171">result := 76EB189B35CF03461DDF877CD9F4B1B4D63A7555</span></span>

<span data-ttu-id="524f7-172">Daje nagłówka pełny kontekst, który jest odcisk palca uwierzytelnionego szyfrowania algorytm pary (szyfrowania 3DES-192-CBC + weryfikacji HMACSHA1), pokazano poniżej:</span><span class="sxs-lookup"><span data-stu-id="524f7-172">This produces the full context header which is a thumbprint of the authenticated encryption algorithm pair (3DES-192-CBC encryption + HMACSHA1 validation), shown below:</span></span>

```
00 00 00 00 00 18 00 00 00 08 00 00 00 14 00 00
00 14 AB B1 00 F8 1E 53 E1 0E 76 EB 18 9B 35 CF
03 46 1D DF 87 7C D9 F4 B1 B4 D6 3A 75 55
```

<span data-ttu-id="524f7-173">Składniki, który podzielić się w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="524f7-173">The components break down as follows:</span></span>

* <span data-ttu-id="524f7-174">znacznika (00 00)</span><span class="sxs-lookup"><span data-stu-id="524f7-174">the marker (00 00)</span></span>

* <span data-ttu-id="524f7-175">długość klucza szyfrowania bloku (00 00 00 18)</span><span class="sxs-lookup"><span data-stu-id="524f7-175">the block cipher key length (00 00 00 18)</span></span>

* <span data-ttu-id="524f7-176">rozmiar bloku cipher block (00 00 00 08)</span><span class="sxs-lookup"><span data-stu-id="524f7-176">the block cipher block size (00 00 00 08)</span></span>

* <span data-ttu-id="524f7-177">długość klucza HMAC (00 00 00 14)</span><span class="sxs-lookup"><span data-stu-id="524f7-177">the HMAC key length (00 00 00 14)</span></span>

* <span data-ttu-id="524f7-178">rozmiar skrótu HMAC (00 00 00 14)</span><span class="sxs-lookup"><span data-stu-id="524f7-178">the HMAC digest size (00 00 00 14)</span></span>

* <span data-ttu-id="524f7-179">szyfrowania bloku danych wyjściowych zasady replikacji HASEŁ (AB B1 - E1 0E) i</span><span class="sxs-lookup"><span data-stu-id="524f7-179">the block cipher PRP output (AB B1 - E1 0E) and</span></span>

* <span data-ttu-id="524f7-180">dane wyjściowe HMAC PRF (76 EB - end).</span><span class="sxs-lookup"><span data-stu-id="524f7-180">the HMAC PRF output (76 EB - end).</span></span>

## <a name="galoiscounter-mode-encryption--authentication"></a><span data-ttu-id="524f7-181">Galois/licznik tryb szyfrowania i uwierzytelniania</span><span class="sxs-lookup"><span data-stu-id="524f7-181">Galois/Counter Mode encryption + authentication</span></span>

<span data-ttu-id="524f7-182">Nagłówek kontekst składa się z następujących składników:</span><span class="sxs-lookup"><span data-stu-id="524f7-182">The context header consists of the following components:</span></span>

* <span data-ttu-id="524f7-183">[16 bitów] Wartość 00 01, który jest znacznik oznacza "GCM szyfrowanie + uwierzytelnianie".</span><span class="sxs-lookup"><span data-stu-id="524f7-183">[16 bits] The value 00 01, which is a marker meaning "GCM encryption + authentication".</span></span>

* <span data-ttu-id="524f7-184">[32-bitowy] Długość klucza (w bajtach, big-endian) algorytmu szyfrowania symetrycznego bloku.</span><span class="sxs-lookup"><span data-stu-id="524f7-184">[32 bits] The key length (in bytes, big-endian) of the symmetric block cipher algorithm.</span></span>

* <span data-ttu-id="524f7-185">[32-bitowy] Jednorazowy rozmiar (w bajtach, big-endian) używane podczas operacji uwierzytelnionego szyfrowania.</span><span class="sxs-lookup"><span data-stu-id="524f7-185">[32 bits] The nonce size (in bytes, big-endian) used during authenticated encryption operations.</span></span> <span data-ttu-id="524f7-186">(Dla naszego systemu problem ten został rozwiązany w rozmiarze nonce = 96 bitów.)</span><span class="sxs-lookup"><span data-stu-id="524f7-186">(For our system, this is fixed at nonce size = 96 bits.)</span></span>

* <span data-ttu-id="524f7-187">[32-bitowy] Rozmiar bloku (w bajtach, big-endian) algorytmu szyfrowania symetrycznego bloku.</span><span class="sxs-lookup"><span data-stu-id="524f7-187">[32 bits] The block size (in bytes, big-endian) of the symmetric block cipher algorithm.</span></span> <span data-ttu-id="524f7-188">(Dla usługi GCM, problem ten został rozwiązany w rozmiarze bloku = 128 bitów.)</span><span class="sxs-lookup"><span data-stu-id="524f7-188">(For GCM, this is fixed at block size = 128 bits.)</span></span>

* <span data-ttu-id="524f7-189">[32-bitowy] Uwierzytelnianie tag rozmiar (w bajtach, big-endian) utworzonej przez funkcję uwierzytelnione szyfrowanie.</span><span class="sxs-lookup"><span data-stu-id="524f7-189">[32 bits] The authentication tag size (in bytes, big-endian) produced by the authenticated encryption function.</span></span> <span data-ttu-id="524f7-190">(Dla naszego systemu problem ten został rozwiązany w rozmiarze tag = 128 bitów.)</span><span class="sxs-lookup"><span data-stu-id="524f7-190">(For our system, this is fixed at tag size = 128 bits.)</span></span>

* <span data-ttu-id="524f7-191">[128 bitów] Tag Enc_GCM (K_E jednorazowy, ""), które znajdują się dane wyjściowe algorytmu szyfrowania symetrycznego bloku podane pusty ciąg wejściowy i których identyfikator jednorazowy jest wektorem zero wszystkie 96-bitowym.</span><span class="sxs-lookup"><span data-stu-id="524f7-191">[128 bits] The tag of Enc_GCM (K_E, nonce, ""), which is the output of the symmetric block cipher algorithm given an empty string input and where nonce is a 96-bit all-zero vector.</span></span>

<span data-ttu-id="524f7-192">K_E jest uzyskiwany za pomocą ten sam mechanizm, tak jak szyfrowanie CBC + HMAC scenariusza uwierzytelniania.</span><span class="sxs-lookup"><span data-stu-id="524f7-192">K_E is derived using the same mechanism as in the CBC encryption + HMAC authentication scenario.</span></span> <span data-ttu-id="524f7-193">Jednakże, ponieważ w tym miejscu play nie K_H, zasadniczo mamy | K_H | = 0, a algorytm jest ustawiana na poniżej formularza.</span><span class="sxs-lookup"><span data-stu-id="524f7-193">However, since there's no K_H in play here, we essentially have | K_H | = 0, and the algorithm collapses to the below form.</span></span>

<span data-ttu-id="524f7-194">K_E = SP800_108_CTR (prf = HMACSHA512, key = "", label = "", kontekst = "")</span><span class="sxs-lookup"><span data-stu-id="524f7-194">K_E = SP800_108_CTR(prf = HMACSHA512, key = "", label = "", context = "")</span></span>

### <a name="example-aes-256-gcm"></a><span data-ttu-id="524f7-195">Przykład: AES-256-GCM</span><span class="sxs-lookup"><span data-stu-id="524f7-195">Example: AES-256-GCM</span></span>

<span data-ttu-id="524f7-196">Najpierw należy umożliwić K_E = SP800_108_CTR (prf = HMACSHA512, key = "", etykieta = "", kontekst = ""), gdzie | K_E | = 256 bitów.</span><span class="sxs-lookup"><span data-stu-id="524f7-196">First, let K_E = SP800_108_CTR(prf = HMACSHA512, key = "", label = "", context = ""), where | K_E | = 256 bits.</span></span>

<span data-ttu-id="524f7-197">K_E := 22BC6F1B171C08C4AE2F27444AF8FC8B3087A90006CAEA91FDCFB47C1B8733B8</span><span class="sxs-lookup"><span data-stu-id="524f7-197">K_E := 22BC6F1B171C08C4AE2F27444AF8FC8B3087A90006CAEA91FDCFB47C1B8733B8</span></span>

<span data-ttu-id="524f7-198">Następnie obliczenia tag uwierzytelniania Enc_GCM (K_E jednorazowy, "") dla standardu AES-256-GCM podany identyfikator jednorazowy = 096 i K_E opisanych powyżej.</span><span class="sxs-lookup"><span data-stu-id="524f7-198">Next, compute the authentication tag of Enc_GCM (K_E, nonce, "") for AES-256-GCM given nonce = 096 and K_E as above.</span></span>

<span data-ttu-id="524f7-199">result := E7DCCE66DF855A323A6BB7BD7A59BE45</span><span class="sxs-lookup"><span data-stu-id="524f7-199">result := E7DCCE66DF855A323A6BB7BD7A59BE45</span></span>

<span data-ttu-id="524f7-200">To daje poniżej nagłówka pełnego kontekstu:</span><span class="sxs-lookup"><span data-stu-id="524f7-200">This produces the full context header below:</span></span>

```
00 01 00 00 00 20 00 00 00 0C 00 00 00 10 00 00
00 10 E7 DC CE 66 DF 85 5A 32 3A 6B B7 BD 7A 59
BE 45
```

<span data-ttu-id="524f7-201">Składniki, który podzielić się w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="524f7-201">The components break down as follows:</span></span>

* <span data-ttu-id="524f7-202">znacznika (00 01)</span><span class="sxs-lookup"><span data-stu-id="524f7-202">the marker (00 01)</span></span>

* <span data-ttu-id="524f7-203">długość klucza szyfrowania bloku (00 00 00 20)</span><span class="sxs-lookup"><span data-stu-id="524f7-203">the block cipher key length (00 00 00 20)</span></span>

* <span data-ttu-id="524f7-204">rozmiar nonce (00 00 00 0 C)</span><span class="sxs-lookup"><span data-stu-id="524f7-204">the nonce size (00 00 00 0C)</span></span>

* <span data-ttu-id="524f7-205">rozmiar bloku cipher block (00 00 00 10)</span><span class="sxs-lookup"><span data-stu-id="524f7-205">the block cipher block size (00 00 00 10)</span></span>

* <span data-ttu-id="524f7-206">Rozmiar znacznika uwierzytelniania (00 00 00 10) i</span><span class="sxs-lookup"><span data-stu-id="524f7-206">the authentication tag size (00 00 00 10) and</span></span>

* <span data-ttu-id="524f7-207">tag uwierzytelniania z systemem szyfrowania bloku (E7 DC - end).</span><span class="sxs-lookup"><span data-stu-id="524f7-207">the authentication tag from running the block cipher (E7 DC - end).</span></span>
