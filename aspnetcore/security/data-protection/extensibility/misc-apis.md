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
# <a name="miscellaneous-aspnet-core-data-protection-apis"></a>Różne interfejsy API ochrony danych ASP.NET Core

<a name="data-protection-extensibility-mics-apis"></a>

>[!WARNING]
> Typy, które implementują żadnego z następujących interfejsów powinny być metodą o bezpiecznych wątkach dla wielu obiektów wywołujących.

## <a name="isecret"></a>ISecret

Interfejs `ISecret` reprezentuje wartość tajną, taką jak materiał klucza kryptograficznego. Zawiera następującą powierzchnię interfejsu API:

* `Length`: `int`

* `Dispose()`: `void`

* `WriteSecretIntoBuffer(ArraySegment<byte> buffer)`: `void`

Metoda `WriteSecretIntoBuffer` wypełnia podany bufor wartością surowego klucza tajnego. Dlatego ten interfejs API przyjmuje bufor jako parametr zamiast zwracać `byte[]` bezpośrednio polega na tym, że obiekt wywołujący może przypiąć obiekt buforu, ograniczając tajne narażenie na zarządzane odzyskiwanie pamięci.

Typ `Secret` jest konkretną implementacją `ISecret`, gdzie wartość klucza tajnego jest przechowywana w pamięci w procesie. Na platformach systemu Windows wartość klucza tajnego jest szyfrowana za pośrednictwem [CryptProtectMemory](https://msdn.microsoft.com/library/windows/desktop/aa380262(v=vs.85).aspx).
