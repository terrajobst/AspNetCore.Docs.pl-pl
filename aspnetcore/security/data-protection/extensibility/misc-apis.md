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
# <a name="miscellaneous-apis"></a>Dodatkowe interfejsy API

<a name="data-protection-extensibility-mics-apis"></a>

>[!WARNING]
> Typy implementujące żadnego z następujących interfejsów powinny być bezpieczne wątkowo dla wielu wywołań.

## <a name="isecret"></a>ISecret

`ISecret` Interfejsu reprezentuje wartość tajna, takich jak materiał kluczy kryptograficznych. Ten przewodnik zawiera następujące powierzchni interfejsu API:

* `Length`: `int`

* `Dispose()`: `void`

* `WriteSecretIntoBuffer(ArraySegment<byte> buffer)`: `void`

`WriteSecretIntoBuffer` Metoda wypełnia dostarczony bufor o wartości nieprzetworzonej tajny. Przyczyny tego interfejsu API przyjmuje buforu jako parametr zamiast zwracanie `byte[]` bezpośrednio jest możliwość przypiąć obiektu buforu ograniczeniu liczby zagrożeń tajnego do zarządzanego modułu zbierającego elementy bezużyteczne dzięki temu obiekt wywołujący.

`Secret` Typu jest konkretną implementację `ISecret` gdzie poufną wartość jest przechowywana w pamięci w procesie. Na platformach systemu Windows, wartość tajna są szyfrowane za pomocą [CryptProtectMemory](https://msdn.microsoft.com/library/windows/desktop/aa380262(v=vs.85).aspx).
