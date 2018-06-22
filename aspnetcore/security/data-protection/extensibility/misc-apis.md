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
# <a name="miscellaneous-aspnet-core-data-protection-apis"></a>Interfejsy API ochrony danych różne platformy ASP.NET Core

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
