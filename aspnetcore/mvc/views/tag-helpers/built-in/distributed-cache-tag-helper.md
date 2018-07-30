---
title: Pomocnik tagu pamięci podręcznej w programie ASP.NET Core rozproszonych
author: pkellner
description: Pokazuje, jak pracować z Pomocnik tagu pamięci podręcznej
ms.author: riande
ms.date: 02/14/2017
uid: mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper
ms.openlocfilehash: a35a5795c086273e773c613c483fc6343c694bf2
ms.sourcegitcommit: 927e510d68f269d8335b5a7c8592621219a90965
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/30/2018
ms.locfileid: "39342175"
---
# <a name="distributed-cache-tag-helper-in-aspnet-core"></a>Pomocnik tagu pamięci podręcznej w programie ASP.NET Core rozproszonych

Przez [Peter Kellner](http://peterkellner.net) 

Pomocnik tagu usługi rozproszonej pamięci podręcznej umożliwia znacznie zwiększyć wydajność aplikacji platformy ASP.NET Core, buforując zawartość ze źródłem rozproszonej pamięci podręcznej.

Pomocnik tagu rozproszonej pamięci podręcznej dziedziczy z klasy bazowej tego samego jako Pomocnik tagu pamięci podręcznej. Wszystkie atrybuty skojarzone z Pomocnik tagu pamięci podręcznej będą również działać w Pomocnik tagu rozproszonej.

Pomocnik tagu rozproszonej pamięci podręcznej następuje **jawne zależności zasady** znane jako **iniekcji konstruktora**. W szczególności `IDistributedCache` interfejs kontenera jest przekazywany do konstruktora rozproszonych Pomocnik tagu pamięci podręcznej w. Jeśli nie określonej implementacji konkretnego `IDistributedCache` został utworzony w `ConfigureServices`, zwykle znajduje się w pliku startup.cs, a następnie Pomocnik tagu rozproszonej pamięci podręcznej będzie używać tego samego dostawcy w pamięci do przechowywania danych w pamięci podręcznej jako podstawowego Pomocnik tagu pamięci podręcznej.

## <a name="distributed-cache-tag-helper-attributes"></a>Rozproszone atrybutów Pomocnik tagu pamięci podręcznej

- - -

### <a name="enabled-expires-on-expires-after-expires-sliding-vary-by-header-vary-by-query-vary-by-route-vary-by-cookie-vary-by-user-vary-by-priority"></a>włączone, wygasa w wygasa po wygaśnięciu przedłużanie różnią się w nagłówku różnią się przez zapytanie różnią się przez trasy różnią się przez cookie różnią się przez użytkownika różnią się według priorytetu

Pomocnik tagu pamięci podręcznej znajdują się definicje. Pomocnik tagu usługi rozproszonej pamięci podręcznej dziedziczy taka sama klasa co Pomocnik tagu pamięci podręcznej, dlatego te atrybuty są wspólne w Pomocnik tagu pamięci podręcznej.

- - -

### <a name="name-required"></a>(wymagane)

| Typ atrybutu    | Przykładowa wartość     |
|----------------   |----------------   |
| string    | "my-distributed-cache-unique-key-101"     |

Wymagane `name` atrybut jest używany jako klucz tej pamięci podręcznej przechowywanych dla każdego wystąpienia Pomocnik tagu rozproszonej pamięci podręcznej. W odróżnieniu od podstawowych Pomocnik tagu pamięci podręcznej przypisującej klucza do każdego wystąpienia Pomocnik tagu pamięci podręcznej na podstawie nazwy strony Razor i lokalizacja Pomocnik tagu na stronie razor Pomocnik tagu rozproszonej pamięci podręcznej tylko określa jego klucza na na podstawie atrybutu `name`

Przykład użycia:

```cshtml
<distributed-cache name="my-distributed-cache-unique-key-101">
    Time Inside Cache Tag Helper: @DateTime.Now
</distributed-cache>
```

## <a name="distributed-cache-tag-helper-idistributedcache-implementations"></a>Rozproszone implementacje IDistributedCache Pomocnik tagu pamięci podręcznej

Istnieją dwie implementacje `IDistributedCache` wbudowanych w platformy ASP.NET Core. Jeden opiera się na serwerze SQL Server, a drugi jest oparta na pamięci podręcznej Redis. Szczegóły tych implementacji znajduje się w temacie <xref:performance/caching/distributed>. Obu implementacjach obejmują ustawienia wystąpienie `IDistributedCache` w programie ASP.NET Core *Startup.cs*.

Istnieją żadne atrybuty znacznika, w szczególności związanych z użyciem określonych implementacji `IDistributedCache`.

## <a name="additional-resources"></a>Dodatkowe zasoby

* <xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper>
* <xref:fundamentals/dependency-injection>
* <xref:performance/caching/distributed>
* <xref:performance/caching/memory>
* <xref:security/authentication/identity>
