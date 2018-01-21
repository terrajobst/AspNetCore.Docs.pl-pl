---
title: "Nagłówki kontekstu"
author: rick-anderson
description: "W tym dokumencie przedstawiono szczegóły implementacji nagłówki kontekstu ochrony danych platformy ASP.NET Core."
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/implementation/context-headers
ms.openlocfilehash: b5ed2e48a55e23d73bccd01a731b35ea68f8944e
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/19/2018
---
# <a name="context-headers"></a><span data-ttu-id="632e2-103">Nagłówki kontekstu</span><span class="sxs-lookup"><span data-stu-id="632e2-103">Context headers</span></span>

<a name="data-protection-implementation-context-headers"></a>

## <a name="background-and-theory"></a><span data-ttu-id="632e2-104">Tło i teorii</span><span class="sxs-lookup"><span data-stu-id="632e2-104">Background and theory</span></span>

<span data-ttu-id="632e2-105">W systemie ochrony danych "klucza" oznacza, że obiekt, który zapewnia uwierzytelniony usługi szyfrowania.</span><span class="sxs-lookup"><span data-stu-id="632e2-105">In the data protection system, a "key" means an object that can provide authenticated encryption services.</span></span> <span data-ttu-id="632e2-106">Każdy klucz jest określana przez unikatowy identyfikator (GUID), a niesie ze sobą algorytmicznego informacje i materiały entropic.</span><span class="sxs-lookup"><span data-stu-id="632e2-106">Each key is identified by a unique id (a GUID), and it carries with it algorithmic information and entropic material.</span></span> <span data-ttu-id="632e2-107">Jest on przeznaczony każdy klucz przenoszenia unikatowy entropii, ale system nie można wymusić, który i musimy także konta dla deweloperów, którzy mogą pierścień klucza ręcznie zmienić, modyfikując algorytmicznego informacji istniejącego klucza w kręgu klucza.</span><span class="sxs-lookup"><span data-stu-id="632e2-107">It is intended that each key carry unique entropy, but the system cannot enforce that, and we also need to account for developers who might change the key ring manually by modifying the algorithmic information of an existing key in the key ring.</span></span> <span data-ttu-id="632e2-108">Do osiągnięcia bieżących wymagań zabezpieczeń podane w tych przypadkach system ochrony danych ma koncepcji [zręczność kryptograficzna](https://www.microsoft.com/en-us/research/publication/cryptographic-agility-and-its-relation-to-circular-encryption/), dzięki czemu przy użyciu pojedynczej wartości entropic wiele algorytmów kryptograficznych.</span><span class="sxs-lookup"><span data-stu-id="632e2-108">To achieve our security requirements given these cases the data protection system has a concept of [cryptographic agility](https://www.microsoft.com/en-us/research/publication/cryptographic-agility-and-its-relation-to-circular-encryption/), which allows securely using a single entropic value across multiple cryptographic algorithms.</span></span>

<span data-ttu-id="632e2-109">Większość systemów, które obsługują zręczność kryptograficzna to zrobić, w tym niektóre informacje identyfikujące algorytm ładunku.</span><span class="sxs-lookup"><span data-stu-id="632e2-109">Most systems which support cryptographic agility do so by including some identifying information about the algorithm inside the payload.</span></span> <span data-ttu-id="632e2-110">Identyfikator OID algorytmu zazwyczaj są odpowiednimi kandydatami tego.</span><span class="sxs-lookup"><span data-stu-id="632e2-110">The algorithm's OID is generally a good candidate for this.</span></span> <span data-ttu-id="632e2-111">Jednak jeden problem, który wystąpił zakłada, że wiele sposobów, aby określić algorytm tego samego: "AES" (CNG) i zarządzanych Aes, AesManaged AesCryptoServiceProvider, AesCng i RijndaelManaged (określone parametry podane) klasy wszystkich faktycznie są takie same element i firma Microsoft musi Obsługa mapowania wszystkich tych na poprawny identyfikator OID.</span><span class="sxs-lookup"><span data-stu-id="632e2-111">However, one problem that we ran into is that there are multiple ways to specify the same algorithm: "AES" (CNG) and the managed Aes, AesManaged, AesCryptoServiceProvider, AesCng, and RijndaelManaged (given specific parameters) classes are all actually the same thing, and we'd need to maintain a mapping of all of these to the correct OID.</span></span> <span data-ttu-id="632e2-112">Deweloper trzeba podać niestandardowy algorytm (lub nawet innej implementacji AES!), zostałyby Powiedz nam jego identyfikator OID.</span><span class="sxs-lookup"><span data-stu-id="632e2-112">If a developer wanted to provide a custom algorithm (or even another implementation of AES!), they'd have to tell us its OID.</span></span> <span data-ttu-id="632e2-113">Tego kroku rejestracji dodatkowe sprawia, że konfiguracja systemu jest szczególnie bolesne.</span><span class="sxs-lookup"><span data-stu-id="632e2-113">This extra registration step makes system configuration particularly painful.</span></span>

<span data-ttu-id="632e2-114">Przechodzenie wstecz, zdecydowaliśmy się, że firma Microsoft zostały zbliża się problem z niewłaściwego kierunek.</span><span class="sxs-lookup"><span data-stu-id="632e2-114">Stepping back, we decided that we were approaching the problem from the wrong direction.</span></span> <span data-ttu-id="632e2-115">Identyfikator OID informuje, co jest algorytm, ale faktycznie nie Dbamy o to.</span><span class="sxs-lookup"><span data-stu-id="632e2-115">An OID tells you what the algorithm is, but we don't actually care about this.</span></span> <span data-ttu-id="632e2-116">Jeśli należy używać jednej wartości entropic w bezpieczny sposób w dwóch różnych algorytmów, nie jest niezbędna dla nam znać, co faktycznie są algorytmy.</span><span class="sxs-lookup"><span data-stu-id="632e2-116">If we need to use a single entropic value securely in two different algorithms, it's not necessary for us to know what the algorithms actually are.</span></span> <span data-ttu-id="632e2-117">Co faktycznie Dbamy o to, ich zachowania.</span><span class="sxs-lookup"><span data-stu-id="632e2-117">What we actually care about is how they behave.</span></span> <span data-ttu-id="632e2-118">Dowolny algorytm szyfrowania zadowalający symetrycznego bloku jest również silne permutacji pseudolosowych (PRP): Usuń dane wejściowe (klucz łańcucha zwykłego tekstu w trybie IV) i dane wyjściowe tekstu szyfrowanego z przeciążając uda się rozpoznać prawdopodobieństwo będą różne od innych szyfrowania symetrycznego bloku Algorytm podane tych samych wejściach.</span><span class="sxs-lookup"><span data-stu-id="632e2-118">Any decent symmetric block cipher algorithm is also a strong pseudorandom permutation (PRP): fix the inputs (key, chaining mode, IV, plaintext) and the ciphertext output will with overwhelming probability be distinct from any other symmetric block cipher algorithm given the same inputs.</span></span> <span data-ttu-id="632e2-119">Podobnie dowolnej funkcji zadowalający kluczem wyznaczania wartości skrótu jest również funkcja pseudolosowych silne (PRF), a podany stały zestaw wejściowy dane wyjściowe przeważnie będą różne od innych funkcji skrótu kluczem.</span><span class="sxs-lookup"><span data-stu-id="632e2-119">Similarly, any decent keyed hash function is also a strong pseudorandom function (PRF), and given a fixed input set its output will overwhelmingly be distinct from any other keyed hash function.</span></span>

<span data-ttu-id="632e2-120">Używamy koncepcji silne PRPs i PRFs do zbudowania nagłówka kontekstu.</span><span class="sxs-lookup"><span data-stu-id="632e2-120">We use this concept of strong PRPs and PRFs to build up a context header.</span></span> <span data-ttu-id="632e2-121">Ten nagłówek kontekstu zasadniczo działa jako stabilny odcisk palca za pośrednictwem algorytmy używane do żadnej operacji danego i zapewnia zręczność kryptograficzna wymaganych przez system ochrony danych.</span><span class="sxs-lookup"><span data-stu-id="632e2-121">This context header essentially acts as a stable thumbprint over the algorithms in use for any given operation, and it provides the cryptographic agility needed by the data protection system.</span></span> <span data-ttu-id="632e2-122">Ten nagłówek zostanie odtworzony i jest później używany w ramach [podkluczy procesu pochodnym](subkeyderivation.md#data-protection-implementation-subkey-derivation).</span><span class="sxs-lookup"><span data-stu-id="632e2-122">This header is reproducible and is used later as part of the [subkey derivation process](subkeyderivation.md#data-protection-implementation-subkey-derivation).</span></span> <span data-ttu-id="632e2-123">Istnieją dwa różne sposoby tworzenia nagłówka kontekstu, w zależności od trybów operacji algorytmów podstawowej.</span><span class="sxs-lookup"><span data-stu-id="632e2-123">There are two different ways to build the context header depending on the modes of operation of the underlying algorithms.</span></span>

## <a name="cbc-mode-encryption--hmac-authentication"></a><span data-ttu-id="632e2-124">Szyfrowanie w trybie CBC + HMAC uwierzytelniania</span><span class="sxs-lookup"><span data-stu-id="632e2-124">CBC-mode encryption + HMAC authentication</span></span>

<a name="data-protection-implementation-context-headers-cbc-components"></a>

<span data-ttu-id="632e2-125">Nagłówek kontekstu składa się z następujących składników:</span><span class="sxs-lookup"><span data-stu-id="632e2-125">The context header consists of the following components:</span></span>

* <span data-ttu-id="632e2-126">[16 bitów] Wartość 00 00, czyli znacznik oznacza "szyfrowania CBC + HMAC uwierzytelniania".</span><span class="sxs-lookup"><span data-stu-id="632e2-126">[16 bits] The value 00 00, which is a marker meaning "CBC encryption + HMAC authentication".</span></span>

* <span data-ttu-id="632e2-127">[32-bitowy] Długość klucza (w bajtach, big-endian) algorytmu szyfrowania symetrycznego bloku.</span><span class="sxs-lookup"><span data-stu-id="632e2-127">[32 bits] The key length (in bytes, big-endian) of the symmetric block cipher algorithm.</span></span>

* <span data-ttu-id="632e2-128">[32-bitowy] Rozmiar bloku (w bajtach, big-endian) algorytmu szyfrowania symetrycznego bloku.</span><span class="sxs-lookup"><span data-stu-id="632e2-128">[32 bits] The block size (in bytes, big-endian) of the symmetric block cipher algorithm.</span></span>

* <span data-ttu-id="632e2-129">[32-bitowy] Długość klucza (w bajtach, big-endian) algorytmu HMAC.</span><span class="sxs-lookup"><span data-stu-id="632e2-129">[32 bits] The key length (in bytes, big-endian) of the HMAC algorithm.</span></span> <span data-ttu-id="632e2-130">(Aktualnie rozmiar klucza zawsze zgodny rozmiar szyfrowanego.)</span><span class="sxs-lookup"><span data-stu-id="632e2-130">(Currently the key size always matches the digest size.)</span></span>

* <span data-ttu-id="632e2-131">[32-bitowy] Skrót rozmiar (w bajtach, big-endian) algorytmu HMAC.</span><span class="sxs-lookup"><span data-stu-id="632e2-131">[32 bits] The digest size (in bytes, big-endian) of the HMAC algorithm.</span></span>

* <span data-ttu-id="632e2-132">EncCBC (K_E, IV, ""), który jest dane wyjściowe algorytmu szyfrowania symetrycznego bloku danych wejściowych pustego ciągu i IV w przypadku wektora samych zer.</span><span class="sxs-lookup"><span data-stu-id="632e2-132">EncCBC(K_E, IV, ""), which is the output of the symmetric block cipher algorithm given an empty string input and where IV is an all-zero vector.</span></span> <span data-ttu-id="632e2-133">Konstrukcja K_E opisano poniżej.</span><span class="sxs-lookup"><span data-stu-id="632e2-133">The construction of K_E is described below.</span></span>

* <span data-ttu-id="632e2-134">MAC (K_H, ""), która jest dane wyjściowe algorytmem HMAC danych wejściowych pusty ciąg.</span><span class="sxs-lookup"><span data-stu-id="632e2-134">MAC(K_H, ""), which is the output of the HMAC algorithm given an empty string input.</span></span> <span data-ttu-id="632e2-135">Konstrukcja K_H opisano poniżej.</span><span class="sxs-lookup"><span data-stu-id="632e2-135">The construction of K_H is described below.</span></span>

<span data-ttu-id="632e2-136">Najlepiej możemy przekazać wektory samych zer, K_E i K_H.</span><span class="sxs-lookup"><span data-stu-id="632e2-136">Ideally, we could pass all-zero vectors for K_E and K_H.</span></span> <span data-ttu-id="632e2-137">Jednak chcemy uniknąć sytuacji, w której algorytm sprawdza istnienie kluczy słabe przed wykonaniem jakichkolwiek operacji (szczególnie DES i 3DES), co wyklucza przy użyciu prostego lub repeatable wzorca takich jak wektorowa samych zer.</span><span class="sxs-lookup"><span data-stu-id="632e2-137">However, we want to avoid the situation where the underlying algorithm checks for the existence of weak keys before performing any operations (notably DES and 3DES), which precludes using a simple or repeatable pattern like an all-zero vector.</span></span>

<span data-ttu-id="632e2-138">Zamiast tego używamy KDF SP800 108 NIST w trybie licznika (zobacz [NIST SP800-108](http://nvlpubs.nist.gov/nistpubs/Legacy/SP/nistspecialpublication800-108.pdf), 5.1 s.) o zerowej długości klucza, etykiety i kontekstu i HMACSHA512 jako podstawowej PRF.</span><span class="sxs-lookup"><span data-stu-id="632e2-138">Instead, we use the NIST SP800-108 KDF in Counter Mode (see [NIST SP800-108](http://nvlpubs.nist.gov/nistpubs/Legacy/SP/nistspecialpublication800-108.pdf), Sec. 5.1) with a zero-length key, label, and context and HMACSHA512 as the underlying PRF.</span></span> <span data-ttu-id="632e2-139">Firma Microsoft pochodzi | K_E | + | K_H | bajtów wyjściowych, następnie Rozłóż wynik na K_E i K_H samodzielnie.</span><span class="sxs-lookup"><span data-stu-id="632e2-139">We derive | K_E | + | K_H | bytes of output, then decompose the result into K_E and K_H themselves.</span></span> <span data-ttu-id="632e2-140">Matematycznie jest reprezentowane w następujący sposób.</span><span class="sxs-lookup"><span data-stu-id="632e2-140">Mathematically, this is represented as follows.</span></span>

<span data-ttu-id="632e2-141">(K_E || K_H) = SP800_108_CTR (prf = HMACSHA512, klucz = "", etykiety = "", kontekst = "")</span><span class="sxs-lookup"><span data-stu-id="632e2-141">( K_E || K_H ) = SP800_108_CTR(prf = HMACSHA512, key = "", label = "", context = "")</span></span>

### <a name="example-aes-192-cbc--hmacsha256"></a><span data-ttu-id="632e2-142">Przykład: AES-192-CBC + HMACSHA256</span><span class="sxs-lookup"><span data-stu-id="632e2-142">Example: AES-192-CBC + HMACSHA256</span></span>

<span data-ttu-id="632e2-143">Na przykład rozważmy przypadek, gdzie algorytmu szyfrowania symetrycznego bloku to AES-192-CBC, a algorytm sprawdzania poprawności jest HMACSHA256.</span><span class="sxs-lookup"><span data-stu-id="632e2-143">As an example, consider the case where the symmetric block cipher algorithm is AES-192-CBC and the validation algorithm is HMACSHA256.</span></span> <span data-ttu-id="632e2-144">System może wygenerować nagłówek kontekstu, wykonując następujące kroki.</span><span class="sxs-lookup"><span data-stu-id="632e2-144">The system would generate the context header using the following steps.</span></span>

<span data-ttu-id="632e2-145">Najpierw należy umożliwić (K_E || K_H) = SP800_108_CTR (prf = HMACSHA512, klucz = "", etykiety = "", kontekst = ""), gdzie | K_E | = bity 192 i | K_H | = 256 bitów na określonym algorytmów.</span><span class="sxs-lookup"><span data-stu-id="632e2-145">First, let ( K_E || K_H ) = SP800_108_CTR(prf = HMACSHA512, key = "", label = "", context = ""), where | K_E | = 192 bits and | K_H | = 256 bits per the specified algorithms.</span></span> <span data-ttu-id="632e2-146">Prowadzi to do K_E = 5BB6... 21DD i K_H = A04A... 00A9 w poniższym przykładzie:</span><span class="sxs-lookup"><span data-stu-id="632e2-146">This leads to K_E = 5BB6..21DD and K_H = A04A..00A9 in the example below:</span></span>

```
5B B6 C9 83 13 78 22 1D 8E 10 73 CA CF 65 8E B0
61 62 42 71 CB 83 21 DD A0 4A 05 00 5B AB C0 A2
49 6F A5 61 E3 E2 49 87 AA 63 55 CD 74 0A DA C4
B7 92 3D BF 59 90 00 A9
```

<span data-ttu-id="632e2-147">Następnie należy obliczyć Enc_CBC (K_E, IV, "") dla AES-192-CBC podane IV = 0 \* i K_E jako powyżej.</span><span class="sxs-lookup"><span data-stu-id="632e2-147">Next, compute Enc_CBC (K_E, IV, "") for AES-192-CBC given IV = 0\* and K_E as above.</span></span>

<span data-ttu-id="632e2-148">result := F474B1872B3B53E4721DE19C0841DB6F</span><span class="sxs-lookup"><span data-stu-id="632e2-148">result := F474B1872B3B53E4721DE19C0841DB6F</span></span>

<span data-ttu-id="632e2-149">Następnie obliczeniowe MAC (K_H, "") dla HMACSHA256 podane K_H jak powyżej.</span><span class="sxs-lookup"><span data-stu-id="632e2-149">Next, compute MAC(K_H, "") for HMACSHA256 given K_H as above.</span></span>

<span data-ttu-id="632e2-150">result := D4791184B996092EE1202F36E8608FA8FBD98ABDFF5402F264B1D7211536220C</span><span class="sxs-lookup"><span data-stu-id="632e2-150">result := D4791184B996092EE1202F36E8608FA8FBD98ABDFF5402F264B1D7211536220C</span></span>

<span data-ttu-id="632e2-151">Daje to nagłówka kontekstu pełnego poniżej:</span><span class="sxs-lookup"><span data-stu-id="632e2-151">This produces the full context header below:</span></span>

```
00 00 00 00 00 18 00 00 00 10 00 00 00 20 00 00
00 20 F4 74 B1 87 2B 3B 53 E4 72 1D E1 9C 08 41
DB 6F D4 79 11 84 B9 96 09 2E E1 20 2F 36 E8 60
8F A8 FB D9 8A BD FF 54 02 F2 64 B1 D7 21 15 36
22 0C
```

<span data-ttu-id="632e2-152">Ten nagłówek kontekstu jest odcisk palca pary algorytmu szyfrowania uwierzytelniony (szyfrowanie AES-192-CBC + weryfikacji HMACSHA256).</span><span class="sxs-lookup"><span data-stu-id="632e2-152">This context header is the thumbprint of the authenticated encryption algorithm pair (AES-192-CBC encryption + HMACSHA256 validation).</span></span> <span data-ttu-id="632e2-153">Składniki, zgodnie z opisem [powyżej](xref:security/data-protection/implementation/context-headers#data-protection-implementation-context-headers-cbc-components) są:</span><span class="sxs-lookup"><span data-stu-id="632e2-153">The components, as described [above](xref:security/data-protection/implementation/context-headers#data-protection-implementation-context-headers-cbc-components) are:</span></span>

* <span data-ttu-id="632e2-154">znacznika (00 00)</span><span class="sxs-lookup"><span data-stu-id="632e2-154">the marker (00 00)</span></span>

* <span data-ttu-id="632e2-155">długość klucza szyfrowania bloku (00 00 00 18)</span><span class="sxs-lookup"><span data-stu-id="632e2-155">the block cipher key length (00 00 00 18)</span></span>

* <span data-ttu-id="632e2-156">rozmiar bloku cipher block (00 00 00 10)</span><span class="sxs-lookup"><span data-stu-id="632e2-156">the block cipher block size (00 00 00 10)</span></span>

* <span data-ttu-id="632e2-157">długość klucza HMAC (00 00 00 20)</span><span class="sxs-lookup"><span data-stu-id="632e2-157">the HMAC key length (00 00 00 20)</span></span>

* <span data-ttu-id="632e2-158">rozmiar skrótu HMAC (00 00 00 20)</span><span class="sxs-lookup"><span data-stu-id="632e2-158">the HMAC digest size (00 00 00 20)</span></span>

* <span data-ttu-id="632e2-159">szyfry blokowe zasady replikacji HASEŁ danych wyjściowych (F4 74 - DB 6 f) i</span><span class="sxs-lookup"><span data-stu-id="632e2-159">the block cipher PRP output (F4 74 - DB 6F) and</span></span>

* <span data-ttu-id="632e2-160">dane wyjściowe HMAC PRF (D4 79 - end).</span><span class="sxs-lookup"><span data-stu-id="632e2-160">the HMAC PRF output (D4 79 - end).</span></span>

> [!NOTE]
> <span data-ttu-id="632e2-161">Szyfrowanie w trybie CBC + HMAC nagłówka kontekstu uwierzytelniania jest oparty ten sam sposób, niezależnie od tego, czy implementacje algorytmów są dostarczane przez Windows CNG lub typy zarządzane SymmetricAlgorithm i KeyedHashAlgorithm.</span><span class="sxs-lookup"><span data-stu-id="632e2-161">The CBC-mode encryption + HMAC authentication context header is built the same way regardless of whether the algorithms implementations are provided by Windows CNG or by managed SymmetricAlgorithm and KeyedHashAlgorithm types.</span></span> <span data-ttu-id="632e2-162">Dzięki temu aplikacje uruchomione w różnych systemach operacyjnych niezawodnie utworzyć tego samego nagłówka kontekstu, mimo że implementacje algorytmów różni się od systemów operacyjnych.</span><span class="sxs-lookup"><span data-stu-id="632e2-162">This allows applications running on different operating systems to reliably produce the same context header even though the implementations of the algorithms differ between OSes.</span></span> <span data-ttu-id="632e2-163">(W praktyce KeyedHashAlgorithm nie musi być odpowiednie HMAC.</span><span class="sxs-lookup"><span data-stu-id="632e2-163">(In practice, the KeyedHashAlgorithm doesn't have to be a proper HMAC.</span></span> <span data-ttu-id="632e2-164">Może być dowolnego typu algorytmu wyznaczania wartości skrótu kluczem.)</span><span class="sxs-lookup"><span data-stu-id="632e2-164">It can be any keyed hash algorithm type.)</span></span>

### <a name="example-3des-192-cbc--hmacsha1"></a><span data-ttu-id="632e2-165">Przykład: 3DES-192-CBC + HMACSHA1</span><span class="sxs-lookup"><span data-stu-id="632e2-165">Example: 3DES-192-CBC + HMACSHA1</span></span>

<span data-ttu-id="632e2-166">Najpierw należy umożliwić (K_E || K_H) = SP800_108_CTR (prf = HMACSHA512, klucz = "", etykiety = "", kontekst = ""), gdzie | K_E | = bity 192 i | K_H | = 160-bitowy określonego algorytmów.</span><span class="sxs-lookup"><span data-stu-id="632e2-166">First, let ( K_E || K_H ) = SP800_108_CTR(prf = HMACSHA512, key = "", label = "", context = ""), where | K_E | = 192 bits and | K_H | = 160 bits per the specified algorithms.</span></span> <span data-ttu-id="632e2-167">Prowadzi to do K_E = A219... E2BB i K_H = DC4A... B464 w poniższym przykładzie:</span><span class="sxs-lookup"><span data-stu-id="632e2-167">This leads to K_E = A219..E2BB and K_H = DC4A..B464 in the example below:</span></span>

```
A2 19 60 2F 83 A9 13 EA B0 61 3A 39 B8 A6 7E 22
61 D9 F8 6C 10 51 E2 BB DC 4A 00 D7 03 A2 48 3E
D1 F7 5A 34 EB 28 3E D7 D4 67 B4 64
```

<span data-ttu-id="632e2-168">Następnie obliczeniowe Enc_CBC (K_E, IV, "") dla algorytmu 3DES-192-CBC podane IV = 0 \* i K_E jako powyżej.</span><span class="sxs-lookup"><span data-stu-id="632e2-168">Next, compute Enc_CBC (K_E, IV, "") for 3DES-192-CBC given IV = 0\* and K_E as above.</span></span>

<span data-ttu-id="632e2-169">result := ABB100F81E53E10E</span><span class="sxs-lookup"><span data-stu-id="632e2-169">result := ABB100F81E53E10E</span></span>

<span data-ttu-id="632e2-170">Następnie obliczeniowe MAC (K_H, "") dla HMACSHA1 podane K_H jak powyżej.</span><span class="sxs-lookup"><span data-stu-id="632e2-170">Next, compute MAC(K_H, "") for HMACSHA1 given K_H as above.</span></span>

<span data-ttu-id="632e2-171">result := 76EB189B35CF03461DDF877CD9F4B1B4D63A7555</span><span class="sxs-lookup"><span data-stu-id="632e2-171">result := 76EB189B35CF03461DDF877CD9F4B1B4D63A7555</span></span>

<span data-ttu-id="632e2-172">Daje to nagłówka kontekstu pełnego, który jest odcisk palca uwierzytelniony szyfrowania algorytmu pary (szyfrowanie 3DES-192-CBC + weryfikacji HMACSHA1), pokazano poniżej:</span><span class="sxs-lookup"><span data-stu-id="632e2-172">This produces the full context header which is a thumbprint of the authenticated encryption algorithm pair (3DES-192-CBC encryption + HMACSHA1 validation), shown below:</span></span>

```
00 00 00 00 00 18 00 00 00 08 00 00 00 14 00 00
00 14 AB B1 00 F8 1E 53 E1 0E 76 EB 18 9B 35 CF
03 46 1D DF 87 7C D9 F4 B1 B4 D6 3A 75 55
```

<span data-ttu-id="632e2-173">Składniki, który podzielić się w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="632e2-173">The components break down as follows:</span></span>

* <span data-ttu-id="632e2-174">znacznika (00 00)</span><span class="sxs-lookup"><span data-stu-id="632e2-174">the marker (00 00)</span></span>

* <span data-ttu-id="632e2-175">długość klucza szyfrowania bloku (00 00 00 18)</span><span class="sxs-lookup"><span data-stu-id="632e2-175">the block cipher key length (00 00 00 18)</span></span>

* <span data-ttu-id="632e2-176">rozmiar bloku cipher block (00 00 00 08)</span><span class="sxs-lookup"><span data-stu-id="632e2-176">the block cipher block size (00 00 00 08)</span></span>

* <span data-ttu-id="632e2-177">długość klucza HMAC (00 00 00 14)</span><span class="sxs-lookup"><span data-stu-id="632e2-177">the HMAC key length (00 00 00 14)</span></span>

* <span data-ttu-id="632e2-178">rozmiar skrótu HMAC (00 00 00 14)</span><span class="sxs-lookup"><span data-stu-id="632e2-178">the HMAC digest size (00 00 00 14)</span></span>

* <span data-ttu-id="632e2-179">szyfry blokowe zasady replikacji HASEŁ danych wyjściowych (AB B1 - E1 0E) i</span><span class="sxs-lookup"><span data-stu-id="632e2-179">the block cipher PRP output (AB B1 - E1 0E) and</span></span>

* <span data-ttu-id="632e2-180">dane wyjściowe HMAC PRF (76 EB - end).</span><span class="sxs-lookup"><span data-stu-id="632e2-180">the HMAC PRF output (76 EB - end).</span></span>

## <a name="galoiscounter-mode-encryption--authentication"></a><span data-ttu-id="632e2-181">Tryb Galois liczników szyfrowania i uwierzytelniania</span><span class="sxs-lookup"><span data-stu-id="632e2-181">Galois/Counter Mode encryption + authentication</span></span>

<span data-ttu-id="632e2-182">Nagłówek kontekstu składa się z następujących składników:</span><span class="sxs-lookup"><span data-stu-id="632e2-182">The context header consists of the following components:</span></span>

* <span data-ttu-id="632e2-183">[16 bitów] Wartość 00 01, który jest znacznik oznacza "szyfrowania GCM + uwierzytelniania".</span><span class="sxs-lookup"><span data-stu-id="632e2-183">[16 bits] The value 00 01, which is a marker meaning "GCM encryption + authentication".</span></span>

* <span data-ttu-id="632e2-184">[32-bitowy] Długość klucza (w bajtach, big-endian) algorytmu szyfrowania symetrycznego bloku.</span><span class="sxs-lookup"><span data-stu-id="632e2-184">[32 bits] The key length (in bytes, big-endian) of the symmetric block cipher algorithm.</span></span>

* <span data-ttu-id="632e2-185">[32-bitowy] Nonce rozmiar (w bajtach, big-endian) używane podczas operacji szyfrowania uwierzytelniony.</span><span class="sxs-lookup"><span data-stu-id="632e2-185">[32 bits] The nonce size (in bytes, big-endian) used during authenticated encryption operations.</span></span> <span data-ttu-id="632e2-186">(Dla naszego systemu jest to stała w rozmiarze nonce = 96 bitów.)</span><span class="sxs-lookup"><span data-stu-id="632e2-186">(For our system, this is fixed at nonce size = 96 bits.)</span></span>

* <span data-ttu-id="632e2-187">[32-bitowy] Rozmiar bloku (w bajtach, big-endian) algorytmu szyfrowania symetrycznego bloku.</span><span class="sxs-lookup"><span data-stu-id="632e2-187">[32 bits] The block size (in bytes, big-endian) of the symmetric block cipher algorithm.</span></span> <span data-ttu-id="632e2-188">(Dla usługi GCM, to jest ustalony na rozmiar bloku = 128 bitów.)</span><span class="sxs-lookup"><span data-stu-id="632e2-188">(For GCM, this is fixed at block size = 128 bits.)</span></span>

* <span data-ttu-id="632e2-189">[32-bitowy] Uwierzytelnianie tag rozmiar (w bajtach, big-endian) utworzonej przez funkcję szyfrowania uwierzytelnionego.</span><span class="sxs-lookup"><span data-stu-id="632e2-189">[32 bits] The authentication tag size (in bytes, big-endian) produced by the authenticated encryption function.</span></span> <span data-ttu-id="632e2-190">(Dla naszego systemu jest to stała w rozmiarze tag = 128 bitów.)</span><span class="sxs-lookup"><span data-stu-id="632e2-190">(For our system, this is fixed at tag size = 128 bits.)</span></span>

* <span data-ttu-id="632e2-191">[128 bitów] Tag Enc_GCM (K_E nonce, ""), czyli dane wyjściowe algorytmu szyfrowania symetrycznego bloku danych wejściowych pusty ciąg, jak i gdzie identyfikator jednorazowy jest wektorem samych zer 96-bitowej.</span><span class="sxs-lookup"><span data-stu-id="632e2-191">[128 bits] The tag of Enc_GCM (K_E, nonce, ""), which is the output of the symmetric block cipher algorithm given an empty string input and where nonce is a 96-bit all-zero vector.</span></span>

<span data-ttu-id="632e2-192">K_E pochodzi ten sam mechanizm, tak jak szyfrowanie CBC + HMAC scenariusz uwierzytelniania.</span><span class="sxs-lookup"><span data-stu-id="632e2-192">K_E is derived using the same mechanism as in the CBC encryption + HMAC authentication scenario.</span></span> <span data-ttu-id="632e2-193">Jednakże, ponieważ w tym miejscu play nie nie K_H, zasadniczo mamy | K_H | = 0, a algorytm zwijany do poniższego formularza.</span><span class="sxs-lookup"><span data-stu-id="632e2-193">However, since there is no K_H in play here, we essentially have | K_H | = 0, and the algorithm collapses to the below form.</span></span>

<span data-ttu-id="632e2-194">K_E = SP800_108_CTR (prf = HMACSHA512, klucz = "", etykiety = "", kontekst = "")</span><span class="sxs-lookup"><span data-stu-id="632e2-194">K_E = SP800_108_CTR(prf = HMACSHA512, key = "", label = "", context = "")</span></span>

### <a name="example-aes-256-gcm"></a><span data-ttu-id="632e2-195">Przykład: AES-256-usługi GCM</span><span class="sxs-lookup"><span data-stu-id="632e2-195">Example: AES-256-GCM</span></span>

<span data-ttu-id="632e2-196">Najpierw należy umożliwić K_E = SP800_108_CTR (prf = HMACSHA512, klucz = "", etykiety = "", kontekst = ""), gdzie | K_E | = 256 bitów.</span><span class="sxs-lookup"><span data-stu-id="632e2-196">First, let K_E = SP800_108_CTR(prf = HMACSHA512, key = "", label = "", context = ""), where | K_E | = 256 bits.</span></span>

<span data-ttu-id="632e2-197">K_E := 22BC6F1B171C08C4AE2F27444AF8FC8B3087A90006CAEA91FDCFB47C1B8733B8</span><span class="sxs-lookup"><span data-stu-id="632e2-197">K_E := 22BC6F1B171C08C4AE2F27444AF8FC8B3087A90006CAEA91FDCFB47C1B8733B8</span></span>

<span data-ttu-id="632e2-198">Następnie obliczeniowe tagu uwierzytelniania Enc_GCM (K_E nonce, "") dla AES-256-GCM podany identyfikator jednorazowy = 096 i K_E jako powyżej.</span><span class="sxs-lookup"><span data-stu-id="632e2-198">Next, compute the authentication tag of Enc_GCM (K_E, nonce, "") for AES-256-GCM given nonce = 096 and K_E as above.</span></span>

<span data-ttu-id="632e2-199">result := E7DCCE66DF855A323A6BB7BD7A59BE45</span><span class="sxs-lookup"><span data-stu-id="632e2-199">result := E7DCCE66DF855A323A6BB7BD7A59BE45</span></span>

<span data-ttu-id="632e2-200">Daje to nagłówka kontekstu pełnego poniżej:</span><span class="sxs-lookup"><span data-stu-id="632e2-200">This produces the full context header below:</span></span>

```
00 01 00 00 00 20 00 00 00 0C 00 00 00 10 00 00
00 10 E7 DC CE 66 DF 85 5A 32 3A 6B B7 BD 7A 59
BE 45
```

<span data-ttu-id="632e2-201">Składniki, który podzielić się w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="632e2-201">The components break down as follows:</span></span>

   * <span data-ttu-id="632e2-202">znacznika (00 01)</span><span class="sxs-lookup"><span data-stu-id="632e2-202">the marker (00 01)</span></span>

   * <span data-ttu-id="632e2-203">długość klucza szyfrowania bloku (00 00 00 20)</span><span class="sxs-lookup"><span data-stu-id="632e2-203">the block cipher key length (00 00 00 20)</span></span>

   * <span data-ttu-id="632e2-204">rozmiar nonce (00 00 00 0 C)</span><span class="sxs-lookup"><span data-stu-id="632e2-204">the nonce size (00 00 00 0C)</span></span>

   * <span data-ttu-id="632e2-205">rozmiar bloku cipher block (00 00 00 10)</span><span class="sxs-lookup"><span data-stu-id="632e2-205">the block cipher block size (00 00 00 10)</span></span>

   * <span data-ttu-id="632e2-206">Rozmiar znacznika uwierzytelniania (00 00 00 10) i</span><span class="sxs-lookup"><span data-stu-id="632e2-206">the authentication tag size (00 00 00 10) and</span></span>

   * <span data-ttu-id="632e2-207">tag uwierzytelniania z systemem szyfrowania bloku (E7 DC - end).</span><span class="sxs-lookup"><span data-stu-id="632e2-207">the authentication tag from running the block cipher (E7 DC - end).</span></span>
