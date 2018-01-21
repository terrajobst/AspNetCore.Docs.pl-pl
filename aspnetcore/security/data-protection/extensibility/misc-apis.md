---
title: Dodatkowe interfejsy API
author: rick-anderson
description: W tym dokumencie przedstawiono interfejsu ISecret ochrony danych platformy ASP.NET Core.
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/extensibility/misc-apis
ms.openlocfilehash: 88a08a25abf4c4e1ba0746087b05b1cc8fa13024
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/19/2018
---
# <a name="miscellaneous-apis"></a><span data-ttu-id="c7291-103">Dodatkowe interfejsy API</span><span class="sxs-lookup"><span data-stu-id="c7291-103">Miscellaneous APIs</span></span>

<a name="data-protection-extensibility-mics-apis"></a>

>[!WARNING]
> <span data-ttu-id="c7291-104">Typy implementujące żadnego z następujących interfejsów powinny być bezpieczne wątkowo dla wielu wywołań.</span><span class="sxs-lookup"><span data-stu-id="c7291-104">Types that implement any of the following interfaces should be thread-safe for multiple callers.</span></span>

## <a name="isecret"></a><span data-ttu-id="c7291-105">ISecret</span><span class="sxs-lookup"><span data-stu-id="c7291-105">ISecret</span></span>

<span data-ttu-id="c7291-106">`ISecret` Interfejsu reprezentuje wartość tajna, takich jak materiał kluczy kryptograficznych.</span><span class="sxs-lookup"><span data-stu-id="c7291-106">The `ISecret` interface represents a secret value, such as cryptographic key material.</span></span> <span data-ttu-id="c7291-107">Ten przewodnik zawiera następujące powierzchni interfejsu API:</span><span class="sxs-lookup"><span data-stu-id="c7291-107">It contains the following API surface:</span></span>

* <span data-ttu-id="c7291-108">`Length`: `int`</span><span class="sxs-lookup"><span data-stu-id="c7291-108">`Length`: `int`</span></span>

* <span data-ttu-id="c7291-109">`Dispose()`: `void`</span><span class="sxs-lookup"><span data-stu-id="c7291-109">`Dispose()`: `void`</span></span>

* <span data-ttu-id="c7291-110">`WriteSecretIntoBuffer(ArraySegment<byte> buffer)`: `void`</span><span class="sxs-lookup"><span data-stu-id="c7291-110">`WriteSecretIntoBuffer(ArraySegment<byte> buffer)`: `void`</span></span>

<span data-ttu-id="c7291-111">`WriteSecretIntoBuffer` Metoda wypełnia dostarczony bufor o wartości nieprzetworzonej tajny.</span><span class="sxs-lookup"><span data-stu-id="c7291-111">The `WriteSecretIntoBuffer` method populates the supplied buffer with the raw secret value.</span></span> <span data-ttu-id="c7291-112">Przyczyny tego interfejsu API przyjmuje buforu jako parametr zamiast zwracanie `byte[]` bezpośrednio jest możliwość przypiąć obiektu buforu ograniczeniu liczby zagrożeń tajnego do zarządzanego modułu zbierającego elementy bezużyteczne dzięki temu obiekt wywołujący.</span><span class="sxs-lookup"><span data-stu-id="c7291-112">The reason this API takes the buffer as a parameter rather than returning a `byte[]` directly is that this gives the caller the opportunity to pin the buffer object, limiting secret exposure to the managed garbage collector.</span></span>

<span data-ttu-id="c7291-113">`Secret` Typu jest konkretną implementację `ISecret` gdzie poufną wartość jest przechowywana w pamięci w procesie.</span><span class="sxs-lookup"><span data-stu-id="c7291-113">The `Secret` type is a concrete implementation of `ISecret` where the secret value is stored in in-process memory.</span></span> <span data-ttu-id="c7291-114">Na platformach systemu Windows, wartość tajna są szyfrowane za pomocą [CryptProtectMemory](https://msdn.microsoft.com/library/windows/desktop/aa380262(v=vs.85).aspx).</span><span class="sxs-lookup"><span data-stu-id="c7291-114">On Windows platforms, the secret value is encrypted via [CryptProtectMemory](https://msdn.microsoft.com/library/windows/desktop/aa380262(v=vs.85).aspx).</span></span>
