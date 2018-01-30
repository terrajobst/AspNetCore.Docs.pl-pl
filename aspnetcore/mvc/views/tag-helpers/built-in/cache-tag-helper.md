---
title: "Buforuj pomocnika tagów w podstawowej platformy ASP.NET MVC"
author: pkellner
description: "Pokazuje, jak pracować z pamięci podręcznej pomocnika tagów"
manager: wpickett
ms.author: riande
ms.date: 02/14/2017
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: mvc/views/tag-helpers/builtin-th/cache-tag-helper
ms.openlocfilehash: 51811ee1669a24a0fc4ce9bc67e782b61bff655c
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/30/2018
---
# <a name="cache-tag-helper-in-aspnet-core-mvc"></a>Buforuj pomocnika tagów w podstawowej platformy ASP.NET MVC

Przez [Kellner Peterowi](http://peterkellner.net) 

Pamięć podręczna pomocnika tagów umożliwia znacznie zwiększyć wydajność aplikacji platformy ASP.NET Core buforując zawartość wewnętrzna dostawcy platformy ASP.NET Core w pamięci podręcznej.

Domyślnie ustawia aparat widoku Razor `expires-after` do 20 minut.

Następujący kod Razor buforuje daty/godziny:

```cshtml
<cache>@DateTime.Now</cache>
```

Pierwsze żądanie do strony zawierającej `CacheTagHelper` wyświetli bieżącej daty/godziny. Dodatkowe żądania zostaną wyświetlone wartość w pamięci podręcznej, dopóki bufor wygasa (domyślnie 20 minut) lub jest wykluczony przez wykorzystania pamięci.

Można ustawić czas trwania pamięci podręcznej z następującymi atrybutami:

## <a name="cache-tag-helper-attributes"></a>Buforuj atrybuty pomocnika tagów

- - -

### <a name="enabled"></a>włączone    


| Typ atrybutu    | Prawidłowe wartości      |
|----------------   |----------------   |
| wartość logiczna           | wartość "prawda" (ustawienie domyślne)  |
|                   | "false"   |


Określa, czy zawartość ujęty w pamięci podręcznej pomocnika tagów są buforowane. Wartość domyślna to `true`.  Jeśli ustawiono `false` tego pomocnika tagów pamięci podręcznej nie wpłyną buforowania na renderowanych danych wyjściowych.

Przykład:

```cshtml
<cache enabled="true">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="expires-on"></a>expires-on 

| Typ atrybutu    | Przykładowa wartość     |
|----------------   |----------------   |
| DateTimeOffset    | "@new DateTime(2025,1,29,17,02,0)"    |


Ustawia datę wygaśnięcia bezwzględne. Poniższy przykład będą buforowane zawartość pamięci podręcznej pomocnika tagów do 5:02 PM 29 stycznia 2025.

Przykład:

```cshtml
<cache expires-on="@new DateTime(2025,1,29,17,02,0)">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="expires-after"></a>expires-after

| Typ atrybutu    | Przykładowa wartość     |
|----------------   |----------------   |
| TimeSpan    | "@TimeSpan.FromSeconds(120)"    |


Ustawia czas od pierwszego żądania, aby buforować zawartość. 

Przykład:

```cshtml
<cache expires-after="@TimeSpan.FromSeconds(120)">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="expires-sliding"></a>Przedłużanie wygasa

| Typ atrybutu    | Przykładowa wartość     |
|----------------   |----------------   |
| TimeSpan    | "@TimeSpan.FromSeconds(60)"     |


Ustawia czas, który wykluczyć wpisu pamięci podręcznej, jeśli nie uzyska dostępu.

Przykład:

```cshtml
<cache expires-sliding="@TimeSpan.FromSeconds(60)">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="vary-by-header"></a>vary-by-header

| Typ atrybutu    | Przykładowe wartości                |
|----------------   |----------------               |
| String            | "User-Agent"                  |
|                   | "User-Agent,content-encoding" |

Akceptuje wartości jeden nagłówek lub rozdzielaną przecinkami listę wartości nagłówka, które mogą powodować odświeżenie pamięci podręcznej po zmianie. Poniższy przykład monitoruje wartość nagłówka `User-Agent`. Przykład będzie buforowanie zawartości dla każdego innego `User-Agent` przedstawiony na serwerze sieci web.

Przykład:

```cshtml
<cache vary-by-header="User-Agent">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="vary-by-query"></a>różnią się przez zapytanie

| Typ atrybutu    | Przykładowe wartości                |
|----------------   |----------------               |
| String            | "Przechowuj"                |
|                   | "Make,Model" |

Akceptuje wartości jeden nagłówek lub rozdzielaną przecinkami listę wartości nagłówka, które mogą powodować odświeżenie pamięci podręcznej po zmianie wartości nagłówka. Poniższy przykład analizuje wartości `Make` i `Model`.

Przykład:

```cshtml
<cache vary-by-query="Make,Model">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="vary-by-route"></a>różnią się przez trasy

| Typ atrybutu    | Przykładowe wartości                |
|----------------   |----------------               |
| String            | "Przechowuj"                |
|                   | "Make,Model" |

Akceptuje wartości jeden nagłówek lub rozdzielaną przecinkami listę wartości nagłówka, które mogą powodować odświeżenie pamięci podręcznej podczas zmiany wartości parametru danych trasy. Przykład:

*Startup.cs* 

```csharp
routes.MapRoute(
    name: "default",
    template: "{controller=Home}/{action=Index}/{Make?}/{Model?}");
```
  
*Index.cshtml*

```cshtml
<cache vary-by-route="Make,Model">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="vary-by-cookie"></a>różnią się przez cookie

| Typ atrybutu    | Przykładowe wartości                |
|----------------   |----------------               |
| String            | ".AspNetCore.Identity.Application"                |
|                   | ".AspNetCore.Identity.Application,HairColor" |

Akceptuje wartości jeden nagłówek lub rozdzielaną przecinkami listę wartości nagłówka, które mogą powodować odświeżenie pamięci podręcznej podczas zmiany (s) wartości nagłówka. Poniższy przykład analizuje plik cookie skojarzone z tożsamością ASP.NET. Gdy użytkownik jest uwierzytelniany żądania plik cookie należy ustawić wartość, która wyzwala odświeżania pamięci podręcznej.

Przykład:

```cshtml
<cache vary-by-cookie=".AspNetCore.Identity.Application">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="vary-by-user"></a>różnią się przez użytkownika

| Typ atrybutu    | Przykładowe wartości                |
|----------------   |----------------               |
| Boolean             | wartość "prawda"                  |
|                     | wartość "false" (ustawienie domyślne) |

Określa, czy pamięć podręczna powinni resetować po zmianie zalogowanego użytkownika (lub jej podmiot kontekstu zabezpieczeń). Bieżący użytkownik jest także znana jako podmiot zabezpieczeń kontekstu żądania i mogą być wyświetlane w widoku Razor odwołując `@User.Identity.Name`.

Poniższy przykład analizuje aktualnie zalogowanego użytkownika.  

Przykład:

```cshtml
<cache vary-by-user="true">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

Za pomocą tego atrybutu przechowuje zawartość w pamięci podręcznej za pomocą logowania i wyloguj się cyklu.  Korzystając z `vary-by-user="true"`, działania logowania i wyloguj się unieważnia pamięci podręcznej dla tego uwierzytelnionego użytkownika.  Pamięć podręczna jest unieważniona, ponieważ nowa wartość unikatowy plik cookie jest generowany podczas logowania. Pamięci podręcznej jest utrzymywana dla anonimowego stanu pliki cookie nie istnieje lub utracił ważność. Oznacza to, gdy żaden użytkownik nie jest zalogowany, będzie przechowywany pamięci podręcznej.

- - -

### <a name="vary-by"></a>różnią się przez

| Typ atrybutu    | Przykładowe wartości                |
|----------------   |----------------               |
| String             | "@Model"                 |


Umożliwia dostosowanie pobiera buforowane dane. Gdy obiekt odwołuje się zmian wartości atrybutu ciąg, zawartości pamięci podręcznej pomocnika tagów jest aktualizowana. Często ciągów wartości modelu są przypisane do tego atrybutu.  Efektywne oznacza to, że aktualizacja dowolną z wartości z połączonych unieważnia pamięci podręcznej.

W poniższym przykładzie założono metody renderowania widoku sum wartość całkowita dwóch parametrów trasy, `myParam1` i `myParam2`i zwraca go jako właściwość pojedynczego modelu. Po zmianie tej sumy zawartości pamięci podręcznej pomocnika tagów jest renderowany i ponownie buforowany.  

Przykład:

Akcja:

```csharp
public IActionResult Index(string myParam1,string myParam2,string myParam3)
{
    int num1;
    int num2;
    int.TryParse(myParam1, out num1);
    int.TryParse(myParam2, out num2);
    return View(viewName, num1 + num2);
}
```

*Index.cshtml*

```cshtml
<cache vary-by="@Model"">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="priority"></a>priorytet

| Typ atrybutu    | Przykładowe wartości                |
|----------------   |----------------               |
| CacheItemPriority  | "High"                   |
|                    | "Low" |
|                    | "NeverRemove" |
|                    | "Normal" |

Zawiera wskazówki wykluczenia pamięci podręcznej do dostawcy wbudowanej pamięci podręcznej. Wyklucz serwer sieci web `Low` najpierw buforować wpisów, gdy jest wykorzystanie pamięci.

Przykład:

```cshtml
<cache priority="High">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

`priority` Atrybutu nie gwarantuje określony poziom przechowywania w pamięci podręcznej. `CacheItemPriority`jest tylko sugestię. Ustawienie tego atrybutu na `NeverRemove` nie gwarantuje, że zawsze zachowywania pamięci podręcznej. Zobacz [dodatkowe zasoby](#additional-resources) Aby uzyskać więcej informacji.

Pomocnik Tag pamięci podręcznej jest zależna od [pamięci podręcznej usługi](xref:performance/caching/memory). Pamięci podręcznej pomocnika tagów dodaje usługę, jeśli nie został dodany.

## <a name="additional-resources"></a>Dodatkowe zasoby

* [Buforowanie w pamięci](xref:performance/caching/memory)
* [Wprowadzenie do tożsamości](xref:security/authentication/identity)
