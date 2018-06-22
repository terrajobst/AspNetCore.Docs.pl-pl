---
title: Interfejsy API ochrony danych różne platformy ASP.NET Core
author: rick-anderson
description: Więcej informacji na temat interfejsu ISecret ochrony danych podstawowych programu ASP.NET.
ms.author: riande
ms.date: 10/14/2016
uid: security/data-protection/extensibility/misc-apis
ms.openlocfilehash: 114cdd6209970e46b827e403fbe79b95692d0242
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/20/2018
ms.locfileid: "36279158"
---
# <a name="miscellaneous-aspnet-core-data-protection-apis"></a><span data-ttu-id="9aea3-103">Interfejsy API ochrony danych różne platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="9aea3-103">Miscellaneous ASP.NET Core Data Protection APIs</span></span>

<a name="data-protection-extensibility-mics-apis"></a>

>[!WARNING]
> <span data-ttu-id="9aea3-104">Typy implementujące żadnego z następujących interfejsów powinny być bezpieczne wątkowo dla wielu wywołań.</span><span class="sxs-lookup"><span data-stu-id="9aea3-104">Types that implement any of the following interfaces should be thread-safe for multiple callers.</span></span>

## <a name="isecret"></a><span data-ttu-id="9aea3-105">ISecret</span><span class="sxs-lookup"><span data-stu-id="9aea3-105">ISecret</span></span>

<span data-ttu-id="9aea3-106">`ISecret` Interfejsu reprezentuje wartość tajna, takich jak materiał kluczy kryptograficznych.</span><span class="sxs-lookup"><span data-stu-id="9aea3-106">The `ISecret` interface represents a secret value, such as cryptographic key material.</span></span> <span data-ttu-id="9aea3-107">Ten przewodnik zawiera następujące powierzchni interfejsu API:</span><span class="sxs-lookup"><span data-stu-id="9aea3-107">It contains the following API surface:</span></span>

* <span data-ttu-id="9aea3-108">`Length`: `int`</span><span class="sxs-lookup"><span data-stu-id="9aea3-108">`Length`: `int`</span></span>

* <span data-ttu-id="9aea3-109">`Dispose()`: `void`</span><span class="sxs-lookup"><span data-stu-id="9aea3-109">`Dispose()`: `void`</span></span>

* <span data-ttu-id="9aea3-110">`WriteSecretIntoBuffer(ArraySegment<byte> buffer)`: `void`</span><span class="sxs-lookup"><span data-stu-id="9aea3-110">`WriteSecretIntoBuffer(ArraySegment<byte> buffer)`: `void`</span></span>

<span data-ttu-id="9aea3-111">`WriteSecretIntoBuffer` Metoda wypełnia dostarczony bufor o wartości nieprzetworzonej tajny.</span><span class="sxs-lookup"><span data-stu-id="9aea3-111">The `WriteSecretIntoBuffer` method populates the supplied buffer with the raw secret value.</span></span> <span data-ttu-id="9aea3-112">Przyczyny tego interfejsu API przyjmuje buforu jako parametr zamiast zwracanie `byte[]` bezpośrednio jest możliwość przypiąć obiektu buforu ograniczeniu liczby zagrożeń tajnego do zarządzanego modułu zbierającego elementy bezużyteczne dzięki temu obiekt wywołujący.</span><span class="sxs-lookup"><span data-stu-id="9aea3-112">The reason this API takes the buffer as a parameter rather than returning a `byte[]` directly is that this gives the caller the opportunity to pin the buffer object, limiting secret exposure to the managed garbage collector.</span></span>

<span data-ttu-id="9aea3-113">`Secret` Typu jest konkretną implementację `ISecret` gdzie poufną wartość jest przechowywana w pamięci w procesie.</span><span class="sxs-lookup"><span data-stu-id="9aea3-113">The `Secret` type is a concrete implementation of `ISecret` where the secret value is stored in in-process memory.</span></span> <span data-ttu-id="9aea3-114">Na platformach systemu Windows, wartość tajna są szyfrowane za pomocą [CryptProtectMemory](https://msdn.microsoft.com/library/windows/desktop/aa380262(v=vs.85).aspx).</span><span class="sxs-lookup"><span data-stu-id="9aea3-114">On Windows platforms, the secret value is encrypted via [CryptProtectMemory](https://msdn.microsoft.com/library/windows/desktop/aa380262(v=vs.85).aspx).</span></span>
