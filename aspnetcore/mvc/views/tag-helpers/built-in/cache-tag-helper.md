---
title: Pomocnik tagu w programie ASP.NET Core MVC pamięci podręcznej
author: pkellner
description: Dowiedz się, jak używać Pomocnik tagu pamięci podręcznej.
ms.author: riande
ms.custom: mvc
ms.date: 10/10/2018
uid: mvc/views/tag-helpers/builtin-th/cache-tag-helper
ms.openlocfilehash: fb69584f6e9d4756e175bbd6f3deb1f413b80fc5
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/27/2019
ms.locfileid: "64902629"
---
# <a name="cache-tag-helper-in-aspnet-core-mvc"></a>Pomocnik tagu w programie ASP.NET Core MVC pamięci podręcznej

Przez [Peter Kellner](http://peterkellner.net) i [Luke Latham](https://github.com/guardrex) 

Pomocnik tagu pamięci podręcznej umożliwia poprawę wydajności aplikacji platformy ASP.NET Core przez buforowanie jego zawartości, wewnętrznego dostawcy pamięci podręcznej platformy ASP.NET Core.

Aby zapoznać się z omówieniem pomocnicy tagów, zobacz <xref:mvc/views/tag-helpers/intro>.

Następujący kod Razor buforuje bieżącą datą:

```cshtml
<cache>@DateTime.Now</cache>
```

Pierwsze żądanie strony, która zawiera Pomocnik tagu Wyświetla bieżącą datę. Wartość w pamięci podręcznej umożliwia wyświetlenie dodatkowych żądań do momentu wygaśnięcia pamięci podręcznej (domyślnie 20 minut) lub daty pamięci podręcznej zostanie usunięty z pamięci podręcznej.

## <a name="cache-tag-helper-attributes"></a>Atrybuty pomocnika tagów w pamięci podręcznej

### <a name="enabled"></a>Włączone

| Typ atrybutu  | Przykłady        | Domyślny |
| --------------- | --------------- | ------- |
| Boolean         | `true`, `false` | `true`  |

`enabled` Określa, jeśli zawartość ujęta w Pomocnik tagu pamięci podręcznej są buforowane. Wartość domyślna to `true`. Jeśli ustawiono `false`, jest wyniku renderowania **nie** pamięci podręcznej.

Przykład:

```cshtml
<cache enabled="true">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

### <a name="expires-on"></a>expires-on

| Typ atrybutu   | Przykład                            |
| ---------------- | ---------------------------------- |
| `DateTimeOffset` | `@new DateTime(2025,1,29,17,02,0)` |

`expires-on` Ustawia bezwzględnych wygaśnięcia elementu pamięci podręcznej.

Poniższy przykład zapisuje w pamięci podręcznej zawartość Pomocnik tagu pamięci podręcznej aż do 17:02:00 w dniu 29 stycznia 2025:

```cshtml
<cache expires-on="@new DateTime(2025,1,29,17,02,0)">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

### <a name="expires-after"></a>expires-after

| Typ atrybutu | Przykład                      | Domyślny    |
| -------------- | ---------------------------- | ---------- |
| `TimeSpan`     | `@TimeSpan.FromSeconds(120)` | 20 minut |

`expires-after` Określa długość czasu od pierwszego żądania, aby buforować zawartość.

Przykład:

```cshtml
<cache expires-after="@TimeSpan.FromSeconds(120)">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

Aparat widoku Razor ustawiana domyślnie `expires-after` wartość do 20 minut.

### <a name="expires-sliding"></a>Przedłużanie wygasa

| Typ atrybutu | Przykład                     |
| -------------- | --------------------------- |
| `TimeSpan`     | `@TimeSpan.FromSeconds(60)` |

Ustawia czas, który powinien zostać wykluczony wpisu pamięci podręcznej, jeśli jego wartość nie uzyskano.

Przykład:

```cshtml
<cache expires-sliding="@TimeSpan.FromSeconds(60)">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

### <a name="vary-by-header"></a>różnią się w nagłówku

| Typ atrybutu | Przykłady                                    |
| -------------- | ------------------------------------------- |
| String         | `User-Agent`, `User-Agent,content-encoding` |

`vary-by-header` akceptuje rozdzielana przecinkami lista wartości nagłówka, które wyzwalać odświeżanie pamięci podręcznej po zmianie.

Poniższy przykład monitoruje wartość nagłówka `User-Agent`. Przykład buforuje zawartość dla każdego innego `User-Agent` przesłanym do serwera sieci web:

```cshtml
<cache vary-by-header="User-Agent">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

### <a name="vary-by-query"></a>różnią się przez zapytanie

| Typ atrybutu | Przykłady             |
| -------------- | -------------------- |
| String         | `Make`, `Make,Model` |

`vary-by-query` akceptuje rozdzielaną przecinkami listę <xref:Microsoft.AspNetCore.Http.IQueryCollection.Keys*> w ciągu zapytania (<xref:Microsoft.AspNetCore.Http.HttpRequest.Query*>) który wyzwolić odświeżanie pamięci podręcznej w przypadku wartości dowolnych zmian klucza na liście.

Poniższy przykład monitoruje wartości `Make` i `Model`. Przykład buforuje zawartość dla każdego innego `Make` i `Model` przesłanym do serwera sieci web:

```cshtml
<cache vary-by-query="Make,Model">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

### <a name="vary-by-route"></a>różnią się przez trasy

| Typ atrybutu | Przykłady             |
| -------------- | -------------------- |
| String         | `Make`, `Make,Model` |

`vary-by-route` akceptuje rozdzielaną przecinkami listę nazw parametrów trasy, które mogą powodować odświeżanie pamięci podręcznej po zmianie wartości parametru danych trasy.

Przykład:

*Startup.cs*:

```csharp
routes.MapRoute(
    name: "default",
    template: "{controller=Home}/{action=Index}/{Make?}/{Model?}");
```

*Index.cshtml*:

```cshtml
<cache vary-by-route="Make,Model">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

### <a name="vary-by-cookie"></a>różnią się przez cookie

| Typ atrybutu | Przykłady                                                                         |
| -------------- | -------------------------------------------------------------------------------- |
| String         | `.AspNetCore.Identity.Application`, `.AspNetCore.Identity.Application,HairColor` |

`vary-by-cookie` akceptuje rozdzielana przecinkami lista nazw plików cookie, które wyzwalać odświeżanie pamięci podręcznej po zmianie wartości pliku cookie.

Poniższy przykład monitoruje pliki cookie skojarzone z tożsamości platformy ASP.NET Core. Gdy użytkownik jest uwierzytelniany, zmiany w pliku cookie tożsamości wyzwala odświeżanie pamięci podręcznej:

```cshtml
<cache vary-by-cookie=".AspNetCore.Identity.Application">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

### <a name="vary-by-user"></a>różnią się przez użytkownika

| Typ atrybutu  | Przykłady        | Domyślny |
| --------------- | --------------- | ------- |
| Boolean         | `true`, `false` | `true`  |

`vary-by-user` Określa, czy pamięć podręczna resetuje po zmianie zalogowanego użytkownika (lub w kontekście jednostki). Bieżący użytkownik jest także znana jako jednostki kontekstu żądania i mogą być wyświetlane w widoku Razor, odwołując się do `@User.Identity.Name`.

Poniższy przykład monitoruje bieżącego zalogowanego użytkownika, aby wyzwolić odświeżanie pamięci podręcznej:

```cshtml
<cache vary-by-user="true">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

Za pomocą tego atrybutu przechowuje zawartość w pamięci podręcznej, za pośrednictwem logowania i wylogowania cyklu. Jeśli wartość jest równa `true`, cyklu uwierzytelniania unieważnia zawartość pamięci podręcznej dla tego uwierzytelnionego użytkownika. Pamięć podręczna jest unieważnione, ponieważ nowa wartość unikatowego pliku cookie jest generowany, gdy użytkownik jest uwierzytelniany. Pamięć podręczna jest zachowywana na potrzeby stanu anonimowe pliki cookie nie są obecne lub plik cookie utracił ważność. Jeśli użytkownik jest **nie** uwierzytelniony, pamięci podręcznej jest utrzymywana.

### <a name="vary-by"></a>różnią się przez

| Typ atrybutu | Przykład  |
| -------------- | -------- |
| String         | `@Model` |

`vary-by` Umożliwia dostosowanie jakie dane są buforowane. Gdy zostanie zaktualizowany obiekt odwołuje się ten atrybut ciągu wartości zmiany, zawartość Pomocnik tagu pamięci podręcznej. Często ciągów wartości modelu są przypisane do tego atrybutu. Skutecznie powoduje to scenariusz, w którym aktualizacji do dowolnej wartości łączonych unieważnia zawartość pamięci podręcznej.

W poniższym przykładzie założono metody kontrolera renderowania sum widoku wartość całkowitą dwa parametry trasy `myParam1` i `myParam2`i zwraca sumę jako właściwość pojedynczego modelu. Po zmianie tej sumy zawartość Pomocnik tagu pamięci podręcznej jest renderowana i ponownie buforowany.  

Akcja:

```csharp
public IActionResult Index(string myParam1, string myParam2, string myParam3)
{
    int num1;
    int num2;
    int.TryParse(myParam1, out num1);
    int.TryParse(myParam2, out num2);
    return View(viewName, num1 + num2);
}
```

*Index.cshtml*:

```cshtml
<cache vary-by="@Model">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

### <a name="priority"></a>priority

| Typ atrybutu      | Przykłady                               | Domyślny  |
| ------------------- | -------------------------------------- | -------- |
| `CacheItemPriority` | `High`, `Low`, `NeverRemove`, `Normal` | `Normal` |

`priority` Znajdują się wskazówki eksmisji pamięci podręcznej dostawcy wbudowaną pamięć podręczną. Wyklucza serwer sieci web mogą `Low` najpierw pamięci podręcznej wpisów, po duże wykorzystanie pamięci.

Przykład:

```cshtml
<cache priority="High">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

`priority` Atrybutu nie gwarantuje określony poziom przechowywania w pamięci podręcznej. `CacheItemPriority` jest tylko sugestię. Ustawienie tego atrybutu na `NeverRemove` nie gwarantuje, że elementy pamięci podręcznej są zawsze zachowywane. Zobacz Tematy w [dodatkowe zasoby](#additional-resources) sekcji, aby uzyskać więcej informacji.

Pomocnik tagu pamięci podręcznej jest zależny od [usługa pamięci podręcznej pamięci](xref:performance/caching/memory). Pomocnik tagu pamięci podręcznej dodaje usługę, jeśli nie została dodana.

## <a name="additional-resources"></a>Dodatkowe zasoby

* <xref:performance/caching/memory>
* <xref:security/authentication/identity>
