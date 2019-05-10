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
# <a name="miscellaneous-aspnet-core-data-protection-apis"></a>Interfejsów API ochrony danych różne platformy ASP.NET Core

<a name="data-protection-extensibility-mics-apis"></a>

>[!WARNING]
> Typy, które implementują żadnego z następujących interfejsów powinny być metodą o bezpiecznych wątkach dla wielu obiektów wywołujących.

## <a name="isecret"></a>ISecret

`ISecret` Interfejs reprezentuje wartość wpisu tajnego, takich jak materiału klucza kryptograficznego. Zawiera on następujące powierzchni interfejsu API:

* `Length`: `int`

* `Dispose()`: `void`

* `WriteSecretIntoBuffer(ArraySegment<byte> buffer)`: `void`

`WriteSecretIntoBuffer` Metoda wypełni dostarczony bufor o pierwotne wartości klucza tajnego. Przyczyna tego interfejsu API przyjmuje buforu jako parametr zamiast zwracanie `byte[]` bezpośrednio jest możliwość przypiąć obiektu buforu, ograniczenie klucza tajnego narażenia na moduł odśmiecania pamięci zarządzanej dzięki temu obiekt wywołujący.

`Secret` Typ jest konkretną implementację `ISecret` gdzie wartość wpisu tajnego jest przechowywany w pamięci w procesie. Na platformach Windows, wartość wpisu tajnego jest szyfrowany za pomocą [CryptProtectMemory](https://msdn.microsoft.com/library/windows/desktop/aa380262(v=vs.85).aspx).
