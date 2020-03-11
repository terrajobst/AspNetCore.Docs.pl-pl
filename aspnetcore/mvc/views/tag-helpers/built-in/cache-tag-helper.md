---
title: Pomocnik tagu pamięci podręcznej w ASP.NET Core MVC
author: pkellner
description: Dowiedz się, jak używać pomocnika tagów pamięci podręcznej.
ms.author: riande
ms.custom: mvc
ms.date: 10/10/2018
uid: mvc/views/tag-helpers/builtin-th/cache-tag-helper
ms.openlocfilehash: db9e1a968588410f11e5f137dfdd4542df505ebc
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78662737"
---
# <a name="cache-tag-helper-in-aspnet-core-mvc"></a>Pomocnik tagu pamięci podręcznej w ASP.NET Core MVC

Według [Peterowi Kellner](https://peterkellner.net)

Pomocnik tagu pamięci podręcznej umożliwia zwiększenie wydajności aplikacji ASP.NET Core przez buforowanie jej zawartości dla dostawcy wewnętrznej ASP.NET Core pamięci podręcznej.

Aby zapoznać się z omówieniem pomocników tagów, zobacz <xref:mvc/views/tag-helpers/intro>.

Następujący znacznik Razor buforuje bieżącą datę:

```cshtml
<cache>@DateTime.Now</cache>
```

Pierwsze żądanie do strony zawierającej pomocnika tagów wyświetla bieżącą datę. Dodatkowe żądania pokazują wartość buforowaną, aż do momentu wygaśnięcia pamięci podręcznej (domyślnie 20 minut) lub do momentu wykluczenia z pamięci podręcznej.

## <a name="cache-tag-helper-attributes"></a>Atrybuty pomocnika tagu pamięci podręcznej

### <a name="enabled"></a>dostępny

| Typ atrybutu  | Przykłady        | Domyślne |
| --------------- | --------------- | ------- |
| Wartość logiczna         | `true`, `false` | `true`  |

`enabled` określa, czy zawartość ujęta w pomocnika znacznika pamięci podręcznej jest buforowana. Wartość domyślna to `true`. W przypadku wybrania wartości `false`renderowane dane wyjściowe **nie** są buforowane.

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

`expires-on` ustawia bezwzględną datę wygaśnięcia dla elementu w pamięci podręcznej.

Poniższy przykład pamięci podręcznej zawartości pomocnika tagów pamięci podręcznej do 5:02 PM dnia 29 stycznia 2025:

```cshtml
<cache expires-on="@new DateTime(2025,1,29,17,02,0)">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

### <a name="expires-after"></a>expires-after

| Typ atrybutu | Przykład                      | Domyślne    |
| -------------- | ---------------------------- | ---------- |
| `TimeSpan`     | `@TimeSpan.FromSeconds(120)` | 20 minut |

`expires-after` Ustawia długość czasu od pierwszego żądania do buforowania zawartości.

Przykład:

```cshtml
<cache expires-after="@TimeSpan.FromSeconds(120)">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

Aparat widoku Razor ustawia wartość domyślną `expires-after` na dwadzieścia minuty.

### <a name="expires-sliding"></a>wygasa — przesuwanie

| Typ atrybutu | Przykład                     |
| -------------- | --------------------------- |
| `TimeSpan`     | `@TimeSpan.FromSeconds(60)` |

Określa czas, w którym wpis pamięci podręcznej powinien zostać wykluczony, jeśli nie uzyskano dostępu do jego wartości.

Przykład:

```cshtml
<cache expires-sliding="@TimeSpan.FromSeconds(60)">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

### <a name="vary-by-header"></a>Zróżnicuj według nagłówka

| Typ atrybutu | Przykłady                                    |
| -------------- | ------------------------------------------- |
| Ciąg         | `User-Agent`, `User-Agent,content-encoding` |

`vary-by-header` akceptuje rozdzielaną przecinkami listę wartości nagłówka, które wyzwalają Odświeżanie pamięci podręcznej, gdy zmienią się.

Poniższy przykład monitoruje wartość nagłówka `User-Agent`. Przykład buforuje zawartość dla każdej innej `User-Agent` prezentowanej na serwerze sieci Web:

```cshtml
<cache vary-by-header="User-Agent">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

### <a name="vary-by-query"></a>Zróżnicuj według zapytania

| Typ atrybutu | Przykłady             |
| -------------- | -------------------- |
| Ciąg         | `Make`, `Make,Model` |

`vary-by-query` akceptuje rozdzieloną przecinkami listę <xref:Microsoft.AspNetCore.Http.IQueryCollection.Keys*> w ciągu zapytania (<xref:Microsoft.AspNetCore.Http.HttpRequest.Query*>), które wyzwalają Odświeżanie pamięci podręcznej, gdy zostanie zmieniona wartość któregokolwiek z wymienionych kluczy.

Poniższy przykład monitoruje wartości `Make` i `Model`. Przykład buforuje zawartość dla każdej różnych `Make` i `Model` przedstawionych na serwerze sieci Web:

```cshtml
<cache vary-by-query="Make,Model">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

### <a name="vary-by-route"></a>Zróżnicuj według trasy

| Typ atrybutu | Przykłady             |
| -------------- | -------------------- |
| Ciąg         | `Make`, `Make,Model` |

`vary-by-route` akceptuje rozdzielaną przecinkami listę nazw parametrów trasy, które wyzwalają Odświeżanie pamięci podręcznej, gdy wartość parametru dane trasy zostanie zmieniona.

Przykład:

*Startup.cs*:

```csharp
routes.MapRoute(
    name: "default",
    template: "{controller=Home}/{action=Index}/{Make?}/{Model?}");
```

*Index. cshtml*:

```cshtml
<cache vary-by-route="Make,Model">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

### <a name="vary-by-cookie"></a>Różne pliki cookie

| Typ atrybutu | Przykłady                                                                         |
| -------------- | -------------------------------------------------------------------------------- |
| Ciąg         | `.AspNetCore.Identity.Application`, `.AspNetCore.Identity.Application,HairColor` |

`vary-by-cookie` akceptuje rozdzielaną przecinkami listę nazw plików cookie, które wyzwalają Odświeżanie pamięci podręcznej, gdy zmieniają się wartości plików cookie.

Poniższy przykład monitoruje plik cookie skojarzony z tożsamością ASP.NET Core. Po uwierzytelnieniu użytkownika zmiana w pliku cookie tożsamości wyzwala odświeżenie pamięci podręcznej:

```cshtml
<cache vary-by-cookie=".AspNetCore.Identity.Application">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

### <a name="vary-by-user"></a>Zróżnicuj według użytkownika

| Typ atrybutu  | Przykłady        | Domyślne |
| --------------- | --------------- | ------- |
| Wartość logiczna         | `true`, `false` | `true`  |

`vary-by-user` określa, czy pamięć podręczna jest resetowana, gdy zostanie zmieniony zalogowany użytkownik (lub podmiot zabezpieczeń kontekstu). Bieżący użytkownik jest również znany jako podmiot zabezpieczeń kontekstu żądania i może być wyświetlany w widoku Razor przez odwołanie `@User.Identity.Name`.

Poniższy przykład monitoruje bieżącego zalogowanego użytkownika, aby wyzwolić Odświeżanie pamięci podręcznej:

```cshtml
<cache vary-by-user="true">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

Użycie tego atrybutu zachowuje zawartość w pamięci podręcznej przez proces logowania i wylogowywania. Gdy wartość jest ustawiona na `true`, cykl uwierzytelniania unieważnia pamięć podręczną dla uwierzytelnionego użytkownika. Pamięć podręczna jest unieważniona, ponieważ podczas uwierzytelniania użytkownika jest generowana Nowa unikatowa wartość cookie. Pamięć podręczna jest utrzymywana dla stanu anonimowego, gdy plik cookie nie istnieje lub plik cookie utracił ważność. Jeśli użytkownik **nie** jest uwierzytelniony, pamięć podręczna jest utrzymywana.

### <a name="vary-by"></a>Zróżnicuj według

| Typ atrybutu | Przykład  |
| -------------- | -------- |
| Ciąg         | `@Model` |

`vary-by` umożliwia dostosowanie danych przechowywanych w pamięci podręcznej. Gdy obiekt, do którego odwołuje się wartość ciągu atrybutu, zmienia się zawartość pomocnika tagu pamięci podręcznej. Często łączenie ciągów wartości modelu jest przypisywane do tego atrybutu. Efektywnie jest to scenariusz, w którym aktualizacja dowolnej z połączonych wartości unieważnia pamięć podręczną.

W poniższym przykładzie przyjęto założenie, że metoda kontrolera renderuje widok sumuje wartość całkowitą dwóch parametrów trasy, `myParam1` i `myParam2`i zwraca sumę jako właściwość pojedynczego modelu. Po zmianie tej sumy zawartość pomocnika tagu pamięci podręcznej jest renderowana i buforowana ponownie.  

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

*Index. cshtml*:

```cshtml
<cache vary-by="@Model">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

### <a name="priority"></a>priority

| Typ atrybutu      | Przykłady                               | Domyślne  |
| ------------------- | -------------------------------------- | -------- |
| `CacheItemPriority` | `High`, `Low`, `NeverRemove`, `Normal` | `Normal` |

`priority` zapewnia wskazówki dotyczące wykluczenia pamięci podręcznej dla wbudowanego dostawcy pamięci podręcznej. Serwer sieci Web najpierw wyklucza `Low` wpisy pamięci podręcznej, gdy jest on w trakcie naciskania pamięci.

Przykład:

```cshtml
<cache priority="High">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

Atrybut `priority` nie gwarantuje określonego poziomu przechowywania pamięci podręcznej. `CacheItemPriority` jest tylko sugestią. Ustawienie tego atrybutu na `NeverRemove` nie gwarantuje, że buforowane elementy są zawsze zachowywane. Aby uzyskać więcej informacji, zobacz tematy w sekcji [dodatkowe zasoby](#additional-resources) .

Pomocnik tagu pamięci podręcznej jest zależny od [usługi pamięci podręcznej](xref:performance/caching/memory). Pomocnik tagu pamięci podręcznej dodaje usługę, jeśli nie została dodana.

## <a name="additional-resources"></a>Dodatkowe zasoby

* <xref:performance/caching/memory>
* <xref:security/authentication/identity>
