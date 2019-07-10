---
title: Obiekt ponownemu użyciu starych z Element ObjectPool w programie ASP.NET Core
author: rick-anderson
description: Porady dotyczące zwiększania wydajności aplikacji platformy ASP.NET Core przy użyciu Element ObjectPool.
monikerRange: '>= aspnetcore-1.1'
ms.author: riande
ms.date: 4/11/2019
uid: performance/ObjectPool
ms.openlocfilehash: 92106d5add7dbda36e451614429baa0db420f0e8
ms.sourcegitcommit: bee530454ae2b3c25dc7ffebf93536f479a14460
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/10/2019
ms.locfileid: "67724857"
---
# <a name="object-reuse-with-objectpool-in-aspnet-core"></a>Obiekt ponownemu użyciu starych z Element ObjectPool w programie ASP.NET Core

Przez [Steve Gordon](https://twitter.com/stevejgordon), [Ryan Nowak](https://github.com/rynowak), i [Rick Anderson](https://twitter.com/RickAndMSFT)

<xref:Microsoft.Extensions.ObjectPool> jest częścią infrastruktury platformy ASP.NET Core, która obsługuje program utrzymywanie grupy obiektów w pamięci do ponownego użycia, a nie zezwolenie obiekty do pamięci zbierane.

Możesz chcieć użyć puli obiektów, jeśli obiekty, które są zarządzane są:

- Kosztowne przydzielić inicjowania.
- Reprezentuje niektórych ograniczonych zasobów.
- Używane przewidywalny i często.

Na przykład platformę ASP.NET Core używa puli obiektów w jednych miejscach do ponownego użycia <xref:System.Text.StringBuilder> wystąpień. `StringBuilder` przydziela i zarządza własną buforów do przechowywania danych znakowych. Platforma ASP.NET Core regularnie używa `StringBuilder` do zaimplementowania funkcji i ponowne użycie ich zapewnia korzyści wydajności.

Buforowanie obiektów nie zawsze poprawi wydajność:

- Chyba że koszt inicjowania obiektu jest wysoka, jest zwykle wolniejsze uzyskać obiekt z puli.
- Obiekty zarządzane przez pulę nie są dezalokowany momentu dezalokowany puli.

Za pomocą buforowanie obiektu dopiero po zbierania danych wydajności przy użyciu realistycznej scenariuszy dla swojej aplikacji lub biblioteki.

**OSTRZEŻENIE: `ObjectPool` Nie implementuje `IDisposable`. Nie zaleca się korzystania z typów, które muszą usuwania.**

**UWAGA: Element ObjectPool nie umieszcza limit liczby obiektów, które rozdzieli, umieszcza limit liczby obiektów, które go zostaną zachowane.**

## <a name="concepts"></a>Pojęcia

<xref:Microsoft.Extensions.ObjectPool.ObjectPool`1> -abstrakcji puli obiektu podstawowego. Umożliwia pobieranie i zwracać obiekty.

<xref:Microsoft.Extensions.ObjectPool.PooledObjectPolicy%601> — Ten element, aby dostosować sposób tworzenia obiektu oraz określić sposób implementacji *resetowania* po zwróceniu do puli. To może być przekazywany do puli obiektów, które możesz utworzyć bezpośrednio... LUB

<xref:Microsoft.Extensions.ObjectPool.ObjectPoolProvider.Create*> działa jako fabrykę do tworzenia pul obiektu.
<!-- REview, there is no ObjectPoolProvider<T> -->

Element ObjectPool może być używany w aplikacji na wiele sposobów:

* Utworzenie wystąpienia puli.
* Rejestrowanie puli [wstrzykiwanie zależności](xref:fundamentals/dependency-injection) (DI) jako wystąpienie.
* Rejestrowanie `ObjectPoolProvider<>` DI i używać go jako fabrykę.

## <a name="how-to-use-objectpool"></a>Jak używać Element ObjectPool

Wywołaj <xref:Microsoft.Extensions.ObjectPool.ObjectPool`1> pobierania obiektu i <xref:Microsoft.Extensions.ObjectPool.ObjectPool`1.Return*> zwracać obiektu.  Nie jest wymagane powrocie każdego obiektu. Jeśli nie zwraca obiektu, będzie bezużyteczne.

## <a name="objectpool-sample"></a>Przykładowy element ObjectPool

Poniższy kod:

* Dodaje `ObjectPoolProvider` do [wstrzykiwanie zależności](xref:fundamentals/dependency-injection) kontenera (DI).
* Dodaje i konfiguruje `ObjectPool<StringBuilder>` do kontenera DI.
* Dodaje `BirthdayMiddleware`.

[!code-csharp[](ObjectPool/ObjectPoolSample/Startup.cs?name=snippet)]

Poniższy kod implementuje `BirthdayMiddleware`

[!code-csharp[](ObjectPool/ObjectPoolSample/BirthdayMiddleware.cs?name=snippet)]
