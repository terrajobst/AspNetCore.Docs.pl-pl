---
title: Ponowne użycie obiektu za pomocą ObjectPool w ASP.NET Core
author: rick-anderson
description: Porady dotyczące zwiększania wydajności ASP.NET Core aplikacji przy użyciu programu ObjectPool.
monikerRange: '>= aspnetcore-1.1'
ms.author: riande
ms.date: 04/11/2019
uid: performance/ObjectPool
ms.openlocfilehash: 771f19e54a908b8b2cd85ff72f368f16e94a2310
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78666111"
---
# <a name="object-reuse-with-objectpool-in-aspnet-core"></a>Ponowne użycie obiektu za pomocą ObjectPool w ASP.NET Core

[Steve Gordon](https://twitter.com/stevejgordon), [Ryan Nowak](https://github.com/rynowak)i [Rick Anderson](https://twitter.com/RickAndMSFT)

<xref:Microsoft.Extensions.ObjectPool> jest częścią infrastruktury ASP.NET Core, która obsługuje przechowywanie w pamięci grupy obiektów do ponownego użycia, a nie pozwala na wyrzucanie obiektów jako elementów bezużytecznych.

Może być konieczne użycie puli obiektów, jeśli zarządzane obiekty są następujące:

- Kosztowne, aby przydzielić/zainicjować.
- Reprezentuje ograniczony zasób.
- Używane do przewidywania i często.

Na przykład, struktura ASP.NET Core używa puli obiektów w niektórych miejscach do ponownego użycia <xref:System.Text.StringBuilder> wystąpień. `StringBuilder` przydziela własne bufory i zarządza nimi do przechowywania danych znakowych. ASP.NET Core regularnie używa `StringBuilder` do implementowania funkcji, a ich użycie umożliwia korzystanie z zalet wydajności.

Buforowanie obiektów nie zawsze poprawia wydajność:

- Chyba że koszt inicjacji obiektu jest wysoki, zwykle jest wolniejsze Pobieranie obiektu z puli.
- Obiekty zarządzane przez pulę nie są przydzielone do momentu cofnięcia przydziału puli.

Używaj buforowania obiektów tylko po zebraniu danych wydajności przy użyciu realistycznych scenariuszy dla aplikacji lub biblioteki.

**Ostrzeżenie: `ObjectPool` nie implementuje `IDisposable`. Nie zalecamy używania jej z typami, które wymagają usunięcia.**

**Uwaga: ObjectPool nie nakłada limitu liczby obiektów, które zostanie przydzielone, spowoduje ograniczenie liczby obiektów zachowywanych.**

## <a name="concepts"></a>Pojęcia

<xref:Microsoft.Extensions.ObjectPool.ObjectPool`1> — podstawowe streszczenie puli obiektów. Służy do pobierania i zwracania obiektów.

<xref:Microsoft.Extensions.ObjectPool.PooledObjectPolicy%601> — implementuje to, aby dostosować sposób tworzenia obiektu i sposobu jego *resetowania* w przypadku powrotu do puli. Ten element może zostać przesłany do puli obiektów, która została skonstruowana bezpośrednio... ORAZ

<xref:Microsoft.Extensions.ObjectPool.ObjectPoolProvider.Create*> pełni rolę fabryki do tworzenia pul obiektów.
<!-- REview, there is no ObjectPoolProvider<T> -->

ObjectPool może być używana w aplikacji na wiele sposobów:

* Tworzenie wystąpienia puli.
* Rejestrowanie puli w [iniekcji zależności](xref:fundamentals/dependency-injection) (di) jako wystąpienie.
* Rejestrowanie `ObjectPoolProvider<>` w programie DI i używanie go jako fabryki.

## <a name="how-to-use-objectpool"></a>Jak używać ObjectPool

Wywołaj <xref:Microsoft.Extensions.ObjectPool.ObjectPool`1>, aby uzyskać obiekt i <xref:Microsoft.Extensions.ObjectPool.ObjectPool`1.Return*> do zwrócenia obiektu.  Nie jest wymagane, aby zwrócić każdy obiekt. Jeśli nie zwracasz obiektu, zostanie on wyrzucony jako elementy bezużyteczne.

## <a name="objectpool-sample"></a>Przykład ObjectPool

Następujący kod:

* Dodaje `ObjectPoolProvider` do kontenera [iniekcji zależności](xref:fundamentals/dependency-injection) (di).
* Dodaje i konfiguruje `ObjectPool<StringBuilder>` do kontenera DI.
* Dodaje `BirthdayMiddleware`.

[!code-csharp[](ObjectPool/ObjectPoolSample/Startup.cs?name=snippet)]

Poniższy kod implementuje `BirthdayMiddleware`

[!code-csharp[](ObjectPool/ObjectPoolSample/BirthdayMiddleware.cs?name=snippet)]
