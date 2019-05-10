---
title: Pomocnik tagu pamięci podręcznej w programie ASP.NET Core rozproszonych
author: pkellner
description: Dowiedz się, jak używać Pomocnik tagu rozproszonej pamięci podręcznej.
ms.author: riande
ms.custom: mvc
ms.date: 10/10/2018
uid: mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper
ms.openlocfilehash: a5b33451a763c297c6d7885855a321c43435abb4
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/27/2019
ms.locfileid: "64902626"
---
# <a name="distributed-cache-tag-helper-in-aspnet-core"></a>Pomocnik tagu pamięci podręcznej w programie ASP.NET Core rozproszonych

Przez [Peter Kellner](http://peterkellner.net) i [Luke Latham](https://github.com/guardrex)

Pomocnik tagu usługi rozproszonej pamięci podręcznej umożliwia znacznie zwiększyć wydajność aplikacji platformy ASP.NET Core, buforując zawartość ze źródłem rozproszonej pamięci podręcznej.

Aby zapoznać się z omówieniem pomocnicy tagów, zobacz <xref:mvc/views/tag-helpers/intro>.

Pomocnik tagu rozproszonej pamięci podręcznej dziedziczy z klasy bazowej tego samego jako Pomocnik tagu pamięci podręcznej. Wszystkie [Pomocnik tagu pamięci podręcznej](xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper) atrybuty są dostępne dla Pomocnik tagu rozproszonej.

Pomocnik tagu rozproszonej pamięci podręcznej używa [iniekcji konstruktora](xref:fundamentals/dependency-injection#constructor-injection-behavior). <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> Interfejsu jest przekazywana do konstruktora rozproszonych Pomocnik tagu pamięci podręcznej firmy. Jeśli nie konkretną implementację `IDistributedCache` jest tworzony w `Startup.ConfigureServices` (*Startup.cs*), Pomocnik tagu rozproszonej pamięci podręcznej przy użyciu tego samego dostawcy w pamięci dane w pamięci podręcznej jako [Pomocnik tagu pamięci podręcznej](xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper).

## <a name="distributed-cache-tag-helper-attributes"></a>Rozproszone atrybutów Pomocnik tagu pamięci podręcznej

### <a name="attributes-shared-with-the-cache-tag-helper"></a>Atrybuty udostępnione Pomocnik tagu pamięci podręcznej

* `enabled`
* `expires-on`
* `expires-after`
* `expires-sliding`
* `vary-by-header`
* `vary-by-query`
* `vary-by-route`
* `vary-by-cookie`
* `vary-by-user`
* `vary-by priority`

Pomocnik tagu rozproszonej pamięci podręcznej dziedziczy taka sama klasa co Pomocnik tagu pamięci podręcznej. Aby uzyskać opis tych atrybutów, zobacz [Pomocnik tagu pamięci podręcznej](xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper).

### <a name="name"></a>nazwa

| Typ atrybutu | Przykład                               |
| -------------- | ------------------------------------- |
| String         | `my-distributed-cache-unique-key-101` |

`name` jest wymagany. `name` Atrybut jest używany jako klucz dla każdego wystąpienia przechowywanych w pamięci podręcznej. Inaczej niż w przypadku Pomocnik tagu pamięci podręcznej przypisuje klucz pamięci podręcznej do każdego wystąpienia, na podstawie nazwy strony Razor i lokalizację w stronę Razor, Pomocnik tagu rozproszonej pamięci podręcznej tylko określa jego klucza na na podstawie atrybutu `name`.

Przykład:

```cshtml
<distributed-cache name="my-distributed-cache-unique-key-101">
    Time Inside Cache Tag Helper: @DateTime.Now
</distributed-cache>
```

## <a name="distributed-cache-tag-helper-idistributedcache-implementations"></a>Rozproszone implementacje IDistributedCache Pomocnik tagu pamięci podręcznej

Istnieją dwie implementacje <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> wbudowanych w platformy ASP.NET Core. Jeden opiera się na serwerze SQL Server, a drugi jest oparta na pamięci podręcznej Redis. Szczegóły tych implementacji znajduje się w temacie <xref:performance/caching/distributed>. Obu implementacjach obejmują ustawienia wystąpienie `IDistributedCache` w `Startup`.

Istnieją żadne atrybuty znacznika, w szczególności związanych z użyciem określonych implementacji `IDistributedCache`.

## <a name="additional-resources"></a>Dodatkowe zasoby

* <xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper>
* <xref:fundamentals/dependency-injection>
* <xref:performance/caching/distributed>
* <xref:performance/caching/memory>
* <xref:security/authentication/identity>
