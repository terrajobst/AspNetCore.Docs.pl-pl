---
title: "Szczegóły uwierzytelnionego szyfrowania"
author: rick-anderson
description: "Ten dokument zawiera szczegóły implementacji platformy ASP.NET Core ochrony danych uwierzytelniony szyfrowania."
keywords: Szyfrowanie uwierzytelniony platformy ASP.NET Core, ochrony danych
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 826e6d5d-9620-44e6-ad93-3b1d9969b70b
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/implementation/authenticated-encryption-details
ms.openlocfilehash: dc96412f6578e612a39e86ce00e1dc5a20cf84e9
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
# <a name="authenticated-encryption-details"></a><span data-ttu-id="1202f-104">Szczegóły uwierzytelnionego szyfrowania</span><span class="sxs-lookup"><span data-stu-id="1202f-104">Authenticated encryption details</span></span>

<a name="data-protection-implementation-authenticated-encryption-details"></a>

<span data-ttu-id="1202f-105">Wywołania IDataProtector.Protect są operacje szyfrowania uwierzytelniony.</span><span class="sxs-lookup"><span data-stu-id="1202f-105">Calls to IDataProtector.Protect are authenticated encryption operations.</span></span> <span data-ttu-id="1202f-106">Metoda Chroń oferuje zarówno poufności i autentyczności i jest powiązany łańcuch cel, który został użyty do uzyskania tego konkretnego wystąpienia interfejsu IDataProtector od głównego IDataProtectionProvider.</span><span class="sxs-lookup"><span data-stu-id="1202f-106">The Protect method offers both confidentiality and authenticity, and it is tied to the purpose chain that was used to derive this particular IDataProtector instance from its root IDataProtectionProvider.</span></span>

<span data-ttu-id="1202f-107">IDataProtector.Protect przyjmuje parametr typu byte [] w postaci zwykłego tekstu i tworzy byte [] chronionych ładunku, których format jest opisane poniżej.</span><span class="sxs-lookup"><span data-stu-id="1202f-107">IDataProtector.Protect takes a byte[] plaintext parameter and produces a byte[] protected payload, whose format is described below.</span></span> <span data-ttu-id="1202f-108">(Dostępna jest również przeciążenie metody rozszerzenia, która przyjmuje parametr typu string w postaci zwykłego tekstu i zwraca ciąg ładunku chronionych.</span><span class="sxs-lookup"><span data-stu-id="1202f-108">(There is also an extension method overload which takes a string plaintext parameter and returns a string protected payload.</span></span> <span data-ttu-id="1202f-109">Jeśli ten interfejs API jest używany format ładunku chronionych nadal będzie miał poniżej struktury, ale będzie ona [kodowany w formacie base64url](https://tools.ietf.org/html/rfc4648#section-5).)</span><span class="sxs-lookup"><span data-stu-id="1202f-109">If this API is used the protected payload format will still have the below structure, but it will be [base64url-encoded](https://tools.ietf.org/html/rfc4648#section-5).)</span></span>

## <a name="protected-payload-format"></a><span data-ttu-id="1202f-110">Format ładunku chronionych</span><span class="sxs-lookup"><span data-stu-id="1202f-110">Protected payload format</span></span>

<span data-ttu-id="1202f-111">Format ładunku chronionych obejmuje trzy główne składniki:</span><span class="sxs-lookup"><span data-stu-id="1202f-111">The protected payload format consists of three primary components:</span></span>

* <span data-ttu-id="1202f-112">Nagłówek magic 32-bitowy, który identyfikuje wersję systemu ochrony danych.</span><span class="sxs-lookup"><span data-stu-id="1202f-112">A 32-bit magic header that identifies the version of the data protection system.</span></span>

* <span data-ttu-id="1202f-113">128-bitowego klucza identyfikatora, który identyfikuje klucz używany do ochrony tego konkretnego ładunku.</span><span class="sxs-lookup"><span data-stu-id="1202f-113">A 128-bit key id that identifies the key used to protect this particular payload.</span></span>

* <span data-ttu-id="1202f-114">W pozostałej części chronionych ładunku jest [specyficzne dla modułu szyfrującego zamknięte przez tego klucza](subkeyderivation.md#data-protection-implementation-subkey-derivation).</span><span class="sxs-lookup"><span data-stu-id="1202f-114">The remainder of the protected payload is [specific to the encryptor encapsulated by this key](subkeyderivation.md#data-protection-implementation-subkey-derivation).</span></span> <span data-ttu-id="1202f-115">W poniższym przykładzie reprezentuje klucz AES-256-CBC + modułu szyfrującego HMACSHA256 i ładunków dalszej podzielonych w następujący sposób: * A 128-bitowego klucza modyfikator.</span><span class="sxs-lookup"><span data-stu-id="1202f-115">In the example below the key represents an AES-256-CBC + HMACSHA256 encryptor, and the payload is further subdivided as follows: * A 128-bit key modifier.</span></span> <span data-ttu-id="1202f-116">* Wektor inicjowania 128-bitowego.</span><span class="sxs-lookup"><span data-stu-id="1202f-116">* A 128-bit initialization vector.</span></span> <span data-ttu-id="1202f-117">* 48 bajtów AES-256-CBC danych wyjściowych.</span><span class="sxs-lookup"><span data-stu-id="1202f-117">* 48 bytes of AES-256-CBC output.</span></span> <span data-ttu-id="1202f-118">* HMACSHA256 tag uwierzytelniania.</span><span class="sxs-lookup"><span data-stu-id="1202f-118">* An HMACSHA256 authentication tag.</span></span>

<span data-ttu-id="1202f-119">Poniżej przedstawiono przykładowe ładunku chronionych.</span><span class="sxs-lookup"><span data-stu-id="1202f-119">A sample protected payload is illustrated below.</span></span>

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

<span data-ttu-id="1202f-120">Z format ładunku powyżej pierwszy 32-bitowy lub 4 bajty mają nagłówek magic identyfikowania wersji (09 F0 C9 F0)</span><span class="sxs-lookup"><span data-stu-id="1202f-120">From the payload format above the first 32 bits, or 4 bytes are the magic header identifying the version (09 F0 C9 F0)</span></span>

<span data-ttu-id="1202f-121">Następne 128 bitów lub 16 bajtów jest identyfikator klucza (80 9C 81 0C 19 66 19 40 95 36 53 F8 AA FF EE 57)</span><span class="sxs-lookup"><span data-stu-id="1202f-121">The next 128 bits, or 16 bytes is the key identifier (80 9C 81 0C 19 66 19 40 95 36 53 F8 AA FF EE 57)</span></span>

<span data-ttu-id="1202f-122">Pozostałe zawiera ładunek i jest specyficzna dla format używany.</span><span class="sxs-lookup"><span data-stu-id="1202f-122">The remainder contains the payload and is specific to the format used.</span></span>

>[!WARNING]
> <span data-ttu-id="1202f-123">Wszystkie ładunki chroniony przez klucz rozpocznie się z tego samego nagłówka 20-bajtową (wartość Magiczna, identyfikator klucza).</span><span class="sxs-lookup"><span data-stu-id="1202f-123">All payloads protected to a given key will begin with the same 20-byte (magic value, key id) header.</span></span> <span data-ttu-id="1202f-124">Administratorzy mogą używać tego faktu w celach diagnostycznych zbliżenie podczas ładunku został wygenerowany.</span><span class="sxs-lookup"><span data-stu-id="1202f-124">Administrators can use this fact for diagnostic purposes to approximate when a payload was generated.</span></span> <span data-ttu-id="1202f-125">Na przykład powyżej ładunku odpowiada kluczowi {0c819c80-6619-4019-9536-53f8aaffee57}.</span><span class="sxs-lookup"><span data-stu-id="1202f-125">For example, the payload above corresponds to key {0c819c80-6619-4019-9536-53f8aaffee57}.</span></span> <span data-ttu-id="1202f-126">Jeśli po sprawdzeniu klucza repozytorium okaże się, że to określonego klucza aktywacji Data 2015-01-01 i jej daty wygaśnięcia został 2015-03-01, a następnie można założyć, że ładunku (Jeśli nie niepowołane) został wygenerowany w ramach tego okna, nadaj lub podjąć małą współczynnik fudge po obu stronach.</span><span class="sxs-lookup"><span data-stu-id="1202f-126">If after checking the key repository you find that this specific key's activation date was 2015-01-01 and its expiration date was 2015-03-01, then it is reasonable to assume that the payload (if not tampered with) was generated within that window, give or take a small fudge factor on either side.</span></span>
