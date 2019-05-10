---
title: Interfejsów API ochrony danych różne platformy ASP.NET Core
author: rick-anderson
description: Informacje o interfejsie ASP.NET Core Data Protection ISecret.
ms.author: riande
ms.date: 10/14/2016
uid: security/data-protection/extensibility/misc-apis
ms.openlocfilehash: 114cdd6209970e46b827e403fbe79b95692d0242
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/27/2019
ms.locfileid: "64902659"
---
# <a name="miscellaneous-aspnet-core-data-protection-apis"></a><span data-ttu-id="8d7dd-103">Interfejsów API ochrony danych różne platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="8d7dd-103">Miscellaneous ASP.NET Core Data Protection APIs</span></span>

<a name="data-protection-extensibility-mics-apis"></a>

>[!WARNING]
> <span data-ttu-id="8d7dd-104">Typy, które implementują żadnego z następujących interfejsów powinny być metodą o bezpiecznych wątkach dla wielu obiektów wywołujących.</span><span class="sxs-lookup"><span data-stu-id="8d7dd-104">Types that implement any of the following interfaces should be thread-safe for multiple callers.</span></span>

## <a name="isecret"></a><span data-ttu-id="8d7dd-105">ISecret</span><span class="sxs-lookup"><span data-stu-id="8d7dd-105">ISecret</span></span>

<span data-ttu-id="8d7dd-106">`ISecret` Interfejs reprezentuje wartość wpisu tajnego, takich jak materiału klucza kryptograficznego.</span><span class="sxs-lookup"><span data-stu-id="8d7dd-106">The `ISecret` interface represents a secret value, such as cryptographic key material.</span></span> <span data-ttu-id="8d7dd-107">Zawiera on następujące powierzchni interfejsu API:</span><span class="sxs-lookup"><span data-stu-id="8d7dd-107">It contains the following API surface:</span></span>

* <span data-ttu-id="8d7dd-108">`Length`: `int`</span><span class="sxs-lookup"><span data-stu-id="8d7dd-108">`Length`: `int`</span></span>

* <span data-ttu-id="8d7dd-109">`Dispose()`: `void`</span><span class="sxs-lookup"><span data-stu-id="8d7dd-109">`Dispose()`: `void`</span></span>

* <span data-ttu-id="8d7dd-110">`WriteSecretIntoBuffer(ArraySegment<byte> buffer)`: `void`</span><span class="sxs-lookup"><span data-stu-id="8d7dd-110">`WriteSecretIntoBuffer(ArraySegment<byte> buffer)`: `void`</span></span>

<span data-ttu-id="8d7dd-111">`WriteSecretIntoBuffer` Metoda wypełni dostarczony bufor o pierwotne wartości klucza tajnego.</span><span class="sxs-lookup"><span data-stu-id="8d7dd-111">The `WriteSecretIntoBuffer` method populates the supplied buffer with the raw secret value.</span></span> <span data-ttu-id="8d7dd-112">Przyczyna tego interfejsu API przyjmuje buforu jako parametr zamiast zwracanie `byte[]` bezpośrednio jest możliwość przypiąć obiektu buforu, ograniczenie klucza tajnego narażenia na moduł odśmiecania pamięci zarządzanej dzięki temu obiekt wywołujący.</span><span class="sxs-lookup"><span data-stu-id="8d7dd-112">The reason this API takes the buffer as a parameter rather than returning a `byte[]` directly is that this gives the caller the opportunity to pin the buffer object, limiting secret exposure to the managed garbage collector.</span></span>

<span data-ttu-id="8d7dd-113">`Secret` Typ jest konkretną implementację `ISecret` gdzie wartość wpisu tajnego jest przechowywany w pamięci w procesie.</span><span class="sxs-lookup"><span data-stu-id="8d7dd-113">The `Secret` type is a concrete implementation of `ISecret` where the secret value is stored in in-process memory.</span></span> <span data-ttu-id="8d7dd-114">Na platformach Windows, wartość wpisu tajnego jest szyfrowany za pomocą [CryptProtectMemory](https://msdn.microsoft.com/library/windows/desktop/aa380262(v=vs.85).aspx).</span><span class="sxs-lookup"><span data-stu-id="8d7dd-114">On Windows platforms, the secret value is encrypted via [CryptProtectMemory](https://msdn.microsoft.com/library/windows/desktop/aa380262(v=vs.85).aspx).</span></span>
