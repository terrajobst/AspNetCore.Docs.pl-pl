---
title: "Rozproszonej pamięci podręcznej pomocnika tagów | Dokumentacja firmy Microsoft"
author: pkellner
description: "Pokazuje, jak pracować z pamięci podręcznej pomocnika tagów"
ms.author: riande
manager: wpickett
ms.date: 02/14/2017
ms.topic: article
ms.technology: aspnet
ms.prod: aspnet-core
uid: mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper
ms.openlocfilehash: f5844dade218fdba1169a55fe3ce251a9cc03db2
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/19/2018
---
# <a name="distributed-cache-tag-helper"></a>Pomocnik Tag rozproszonej pamięci podręcznej

Przez [Kellner Peterowi](http://peterkellner.net) 


Rozproszonej pamięci podręcznej pomocnika Tag pozwala na znacznie zwiększyć wydajność aplikacji platformy ASP.NET Core buforując zawartość ze źródłem rozproszonej pamięci podręcznej.

Dziedziczy pomocnika Tag rozproszonej pamięci podręcznej z tej samej klasy podstawowej jako pomocnika tagów pamięci podręcznej.  Wszystkie atrybuty skojarzone z pamięci podręcznej pomocnika tagów również będą działać na pomocnika tagów rozproszonych.


Następuje pomocnika Tag rozproszonej pamięci podręcznej **jawne zależności zasady** znany jako **iniekcji konstruktora**.  W szczególności `IDistributedCache` kontenera interfejs zostanie przekazany do konstruktora rozproszonej pamięci podręcznej Tag Pomocnik firmy.  Jeśli nie określonych konkretną implementację `IDistributedCache` został utworzony w `ConfigureServices`, zwykle znajdują się w pliku startup.cs, a następnie pomocnika Tag rozproszonej pamięci podręcznej będzie używać tego samego dostawcy w pamięci do przechowywania danych z pamięci podręcznej jako podstawowa pomocnika tagów pamięci podręcznej.

## <a name="distributed-cache-tag-helper-attributes"></a>Rozproszonej pamięci podręcznej Tag pomocnika atrybutów

- - -

### <a name="enabled-expires-on-expires-after-expires-sliding-vary-by-header-vary-by-query-vary-by-route-vary-by-cookie-vary-by-user-vary-by-priority"></a>włączone wygasa na wygasa po wygaśnięciu przedłużanie różnią się w nagłówku różnią się przez zapytanie różnią się przez trasy różnią się przez cookie różnią się przez użytkownika różnią się według priorytetu

Zobacz pomocnika tagów pamięci podręcznej dla definicji. Rozproszonej pamięci podręcznej Tag pomocnika dziedziczy z tej samej klasy jako pomocnika tagów pamięci podręcznej, więc te atrybuty są często używane z pamięci podręcznej pomocnika tagów.

- - -

### <a name="name-required"></a>Nazwa (wymagane)

| Typ atrybutu    | Przykładowa wartość     |
|----------------   |----------------   |
| string    | "my-distributed-cache-unique-key-101"     |

Wymagane `name` atrybut jest używany jako klucz tej pamięci podręcznej przechowywanych dla każdego wystąpienia pomocnika Tag rozproszonej pamięci podręcznej.  W przeciwieństwie do podstawowych pomocnika tagów pamięci podręcznej przypisującej klucza do każdego wystąpienia pomocnika tagów pamięci podręcznej na podstawie Razor strony nazwy i lokalizacji pomocnika tagów razor strony pomocnika Tag rozproszonej pamięci podręcznej tylko określa jego klucza na na podstawie atrybutu`name`

Przykład użycia:

```cshtml
<distributed-cache name="my-distributed-cache-unique-key-101">
    Time Inside Cache Tag Helper: @DateTime.Now
</distributed-cache>
```

## <a name="distributed-cache-tag-helper-idistributedcache-implementations"></a>Rozproszonej pamięci podręcznej Tag pomocnika IDistributedCache implementacji

Istnieją dwa implementacje `IDistributedCache` wbudowanych w platformy ASP.NET Core.  Jeden jest oparta na **programu Sql Server** i innych opiera się na **Redis**. Szczegółowe informacje o tych implementacji znajduje się na zasób poniżej o nazwie "Praca z rozproszonej pamięci podręcznej". Zarówno implementacje obejmują ustawianie wystąpienia `IDistributedCache` w ASP.NET Core **startup.cs**.

Istnieją żadne atrybuty tagu w szczególności związanych z użyciem określonych implementacji `IDistributedCache`.



- - -



## <a name="additional-resources"></a>Dodatkowe zasoby

* <xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper>
* <xref:fundamentals/dependency-injection#service-lifetimes-and-registration-options>
* <xref:performance/caching/distributed>
* <xref:performance/caching/memory>
* <xref:security/authentication/identity>
