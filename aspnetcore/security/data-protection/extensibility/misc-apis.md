---
title: Interfejsy API ochrony danych różne platformy ASP.NET Core
author: rick-anderson
description: Więcej informacji na temat interfejsu ISecret ochrony danych podstawowych programu ASP.NET.
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/data-protection/extensibility/misc-apis
ms.openlocfilehash: 484c6a0979a10e7cf2b801873655caa99a42532c
ms.sourcegitcommit: 48beecfe749ddac52bc79aa3eb246a2dcdaa1862
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/22/2018
ms.locfileid: "30072767"
---
# <a name="miscellaneous-aspnet-core-data-protection-apis"></a><span data-ttu-id="d20fb-103">Interfejsy API ochrony danych różne platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="d20fb-103">Miscellaneous ASP.NET Core Data Protection APIs</span></span>

<a name="data-protection-extensibility-mics-apis"></a>

>[!WARNING]
> <span data-ttu-id="d20fb-104">Typy implementujące żadnego z następujących interfejsów powinny być bezpieczne wątkowo dla wielu wywołań.</span><span class="sxs-lookup"><span data-stu-id="d20fb-104">Types that implement any of the following interfaces should be thread-safe for multiple callers.</span></span>

## <a name="isecret"></a><span data-ttu-id="d20fb-105">ISecret</span><span class="sxs-lookup"><span data-stu-id="d20fb-105">ISecret</span></span>

<span data-ttu-id="d20fb-106">`ISecret` Interfejsu reprezentuje wartość tajna, takich jak materiał kluczy kryptograficznych.</span><span class="sxs-lookup"><span data-stu-id="d20fb-106">The `ISecret` interface represents a secret value, such as cryptographic key material.</span></span> <span data-ttu-id="d20fb-107">Ten przewodnik zawiera następujące powierzchni interfejsu API:</span><span class="sxs-lookup"><span data-stu-id="d20fb-107">It contains the following API surface:</span></span>

* <span data-ttu-id="d20fb-108">`Length`: `int`</span><span class="sxs-lookup"><span data-stu-id="d20fb-108">`Length`: `int`</span></span>

* <span data-ttu-id="d20fb-109">`Dispose()`: `void`</span><span class="sxs-lookup"><span data-stu-id="d20fb-109">`Dispose()`: `void`</span></span>

* <span data-ttu-id="d20fb-110">`WriteSecretIntoBuffer(ArraySegment<byte> buffer)`: `void`</span><span class="sxs-lookup"><span data-stu-id="d20fb-110">`WriteSecretIntoBuffer(ArraySegment<byte> buffer)`: `void`</span></span>

<span data-ttu-id="d20fb-111">`WriteSecretIntoBuffer` Metoda wypełnia dostarczony bufor o wartości nieprzetworzonej tajny.</span><span class="sxs-lookup"><span data-stu-id="d20fb-111">The `WriteSecretIntoBuffer` method populates the supplied buffer with the raw secret value.</span></span> <span data-ttu-id="d20fb-112">Przyczyny tego interfejsu API przyjmuje buforu jako parametr zamiast zwracanie `byte[]` bezpośrednio jest możliwość przypiąć obiektu buforu ograniczeniu liczby zagrożeń tajnego do zarządzanego modułu zbierającego elementy bezużyteczne dzięki temu obiekt wywołujący.</span><span class="sxs-lookup"><span data-stu-id="d20fb-112">The reason this API takes the buffer as a parameter rather than returning a `byte[]` directly is that this gives the caller the opportunity to pin the buffer object, limiting secret exposure to the managed garbage collector.</span></span>

<span data-ttu-id="d20fb-113">`Secret` Typu jest konkretną implementację `ISecret` gdzie poufną wartość jest przechowywana w pamięci w procesie.</span><span class="sxs-lookup"><span data-stu-id="d20fb-113">The `Secret` type is a concrete implementation of `ISecret` where the secret value is stored in in-process memory.</span></span> <span data-ttu-id="d20fb-114">Na platformach systemu Windows, wartość tajna są szyfrowane za pomocą [CryptProtectMemory](https://msdn.microsoft.com/library/windows/desktop/aa380262(v=vs.85).aspx).</span><span class="sxs-lookup"><span data-stu-id="d20fb-114">On Windows platforms, the secret value is encrypted via [CryptProtectMemory](https://msdn.microsoft.com/library/windows/desktop/aa380262(v=vs.85).aspx).</span></span>
