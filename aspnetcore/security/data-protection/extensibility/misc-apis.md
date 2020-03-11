---
title: Różne interfejsy API ochrony danych ASP.NET Core
author: rick-anderson
description: Dowiedz się więcej o interfejsie ISecret ochrony danych ASP.NET Core.
ms.author: riande
ms.date: 10/14/2016
uid: security/data-protection/extensibility/misc-apis
ms.openlocfilehash: 114cdd6209970e46b827e403fbe79b95692d0242
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78666083"
---
# <a name="miscellaneous-aspnet-core-data-protection-apis"></a><span data-ttu-id="248b4-103">Różne interfejsy API ochrony danych ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="248b4-103">Miscellaneous ASP.NET Core Data Protection APIs</span></span>

<a name="data-protection-extensibility-mics-apis"></a>

>[!WARNING]
> <span data-ttu-id="248b4-104">Typy, które implementują żadnego z następujących interfejsów powinny być metodą o bezpiecznych wątkach dla wielu obiektów wywołujących.</span><span class="sxs-lookup"><span data-stu-id="248b4-104">Types that implement any of the following interfaces should be thread-safe for multiple callers.</span></span>

## <a name="isecret"></a><span data-ttu-id="248b4-105">ISecret</span><span class="sxs-lookup"><span data-stu-id="248b4-105">ISecret</span></span>

<span data-ttu-id="248b4-106">Interfejs `ISecret` reprezentuje wartość tajną, taką jak materiał klucza kryptograficznego.</span><span class="sxs-lookup"><span data-stu-id="248b4-106">The `ISecret` interface represents a secret value, such as cryptographic key material.</span></span> <span data-ttu-id="248b4-107">Zawiera następującą powierzchnię interfejsu API:</span><span class="sxs-lookup"><span data-stu-id="248b4-107">It contains the following API surface:</span></span>

* <span data-ttu-id="248b4-108">`Length`: `int`</span><span class="sxs-lookup"><span data-stu-id="248b4-108">`Length`: `int`</span></span>

* <span data-ttu-id="248b4-109">`Dispose()`: `void`</span><span class="sxs-lookup"><span data-stu-id="248b4-109">`Dispose()`: `void`</span></span>

* <span data-ttu-id="248b4-110">`WriteSecretIntoBuffer(ArraySegment<byte> buffer)`: `void`</span><span class="sxs-lookup"><span data-stu-id="248b4-110">`WriteSecretIntoBuffer(ArraySegment<byte> buffer)`: `void`</span></span>

<span data-ttu-id="248b4-111">Metoda `WriteSecretIntoBuffer` wypełnia podany bufor wartością surowego klucza tajnego.</span><span class="sxs-lookup"><span data-stu-id="248b4-111">The `WriteSecretIntoBuffer` method populates the supplied buffer with the raw secret value.</span></span> <span data-ttu-id="248b4-112">Dlatego ten interfejs API przyjmuje bufor jako parametr zamiast zwracać `byte[]` bezpośrednio polega na tym, że obiekt wywołujący może przypiąć obiekt buforu, ograniczając tajne narażenie na zarządzane odzyskiwanie pamięci.</span><span class="sxs-lookup"><span data-stu-id="248b4-112">The reason this API takes the buffer as a parameter rather than returning a `byte[]` directly is that this gives the caller the opportunity to pin the buffer object, limiting secret exposure to the managed garbage collector.</span></span>

<span data-ttu-id="248b4-113">Typ `Secret` jest konkretną implementacją `ISecret`, gdzie wartość klucza tajnego jest przechowywana w pamięci w procesie.</span><span class="sxs-lookup"><span data-stu-id="248b4-113">The `Secret` type is a concrete implementation of `ISecret` where the secret value is stored in in-process memory.</span></span> <span data-ttu-id="248b4-114">Na platformach systemu Windows wartość klucza tajnego jest szyfrowana za pośrednictwem [CryptProtectMemory](https://msdn.microsoft.com/library/windows/desktop/aa380262(v=vs.85).aspx).</span><span class="sxs-lookup"><span data-stu-id="248b4-114">On Windows platforms, the secret value is encrypted via [CryptProtectMemory](https://msdn.microsoft.com/library/windows/desktop/aa380262(v=vs.85).aspx).</span></span>
