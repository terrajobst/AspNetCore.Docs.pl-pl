---
title: Dodatkowe interfejsy API
author: rick-anderson
description: W tym dokumencie przedstawiono interfejsu ISecret ochrony danych platformy ASP.NET Core.
keywords: Platformy ASP.NET Core ISecret ochrony danych
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 512c6ba7-88ec-47e4-a656-6b30350b34e6
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/extensibility/misc-apis
ms.openlocfilehash: f5d6920f9f229bd480a76c952dab30efb7d9eff5
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
# <a name="miscellaneous-apis"></a><span data-ttu-id="45068-104">Dodatkowe interfejsy API</span><span class="sxs-lookup"><span data-stu-id="45068-104">Miscellaneous APIs</span></span>

<a name="data-protection-extensibility-mics-apis"></a>

>[!WARNING]
> <span data-ttu-id="45068-105">Typy implementujące żadnego z następujących interfejsów powinny być bezpieczne wątkowo dla wielu wywołań.</span><span class="sxs-lookup"><span data-stu-id="45068-105">Types that implement any of the following interfaces should be thread-safe for multiple callers.</span></span>

## <a name="isecret"></a><span data-ttu-id="45068-106">ISecret</span><span class="sxs-lookup"><span data-stu-id="45068-106">ISecret</span></span>

<span data-ttu-id="45068-107">`ISecret` Interfejsu reprezentuje wartość tajna, takich jak materiał kluczy kryptograficznych.</span><span class="sxs-lookup"><span data-stu-id="45068-107">The `ISecret` interface represents a secret value, such as cryptographic key material.</span></span> <span data-ttu-id="45068-108">Ten przewodnik zawiera następujące powierzchni interfejsu API:</span><span class="sxs-lookup"><span data-stu-id="45068-108">It contains the following API surface:</span></span>

* <span data-ttu-id="45068-109">`Length`: `int`</span><span class="sxs-lookup"><span data-stu-id="45068-109">`Length`: `int`</span></span>

* <span data-ttu-id="45068-110">`Dispose()`: `void`</span><span class="sxs-lookup"><span data-stu-id="45068-110">`Dispose()`: `void`</span></span>

* <span data-ttu-id="45068-111">`WriteSecretIntoBuffer(ArraySegment<byte> buffer)`: `void`</span><span class="sxs-lookup"><span data-stu-id="45068-111">`WriteSecretIntoBuffer(ArraySegment<byte> buffer)`: `void`</span></span>

<span data-ttu-id="45068-112">`WriteSecretIntoBuffer` Metoda wypełnia dostarczony bufor o wartości nieprzetworzonej tajny.</span><span class="sxs-lookup"><span data-stu-id="45068-112">The `WriteSecretIntoBuffer` method populates the supplied buffer with the raw secret value.</span></span> <span data-ttu-id="45068-113">Przyczyny tego interfejsu API przyjmuje buforu jako parametr zamiast zwracanie `byte[]` bezpośrednio jest możliwość przypiąć obiektu buforu ograniczeniu liczby zagrożeń tajnego do zarządzanego modułu zbierającego elementy bezużyteczne dzięki temu obiekt wywołujący.</span><span class="sxs-lookup"><span data-stu-id="45068-113">The reason this API takes the buffer as a parameter rather than returning a `byte[]` directly is that this gives the caller the opportunity to pin the buffer object, limiting secret exposure to the managed garbage collector.</span></span>

<span data-ttu-id="45068-114">`Secret` Typu jest konkretną implementację `ISecret` gdzie poufną wartość jest przechowywana w pamięci w procesie.</span><span class="sxs-lookup"><span data-stu-id="45068-114">The `Secret` type is a concrete implementation of `ISecret` where the secret value is stored in in-process memory.</span></span> <span data-ttu-id="45068-115">Na platformach systemu Windows, wartość tajna są szyfrowane za pomocą [CryptProtectMemory](https://msdn.microsoft.com/library/windows/desktop/aa380262(v=vs.85).aspx).</span><span class="sxs-lookup"><span data-stu-id="45068-115">On Windows platforms, the secret value is encrypted via [CryptProtectMemory](https://msdn.microsoft.com/library/windows/desktop/aa380262(v=vs.85).aspx).</span></span>
