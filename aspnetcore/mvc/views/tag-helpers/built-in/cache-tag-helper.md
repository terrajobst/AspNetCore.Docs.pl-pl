---
title: Pomocnik tagu w programie ASP.NET Core MVC pamięci podręcznej
author: pkellner
description: Pokazuje, jak pracować z Pomocnik tagu pamięci podręcznej
ms.author: riande
ms.date: 02/14/2017
uid: mvc/views/tag-helpers/builtin-th/cache-tag-helper
ms.openlocfilehash: 425d8c2235f0070665bc0c967d2498f2cff2a4a6
ms.sourcegitcommit: d53e0cc71542b92de867bcce51575b054886f529
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/16/2018
ms.locfileid: "41754192"
---
# <a name="cache-tag-helper-in-aspnet-core-mvc"></a>Pomocnik tagu w programie ASP.NET Core MVC pamięci podręcznej

Przez [Peter Kellner](http://peterkellner.net) 

Pomocnik tagu pamięci podręcznej umożliwia znacznie zwiększyć wydajność aplikacji platformy ASP.NET Core, buforując jego zawartości, wewnętrznego dostawcy pamięci podręcznej platformy ASP.NET Core.

Aparat widoku Razor ustawiana domyślnie `expires-after` do 20 minut.

Następujący kod Razor buforuje daty/godziny:

```cshtml
<cache>@DateTime.Now</cache>
```

Pierwsze żądanie strony, która zawiera `CacheTagHelper` spowoduje wyświetlenie bieżącej daty/godziny. Dodatkowe żądania pokaże wartość w pamięci podręcznej, dopóki pamięci podręcznej wygaśnie (domyślnie 20 minut) lub zostaną eksmitowane przez wykorzystanie pamięci.

Można ustawić czas trwania pamięci podręcznej z następującymi atrybutami:

## <a name="cache-tag-helper-attributes"></a>Atrybuty pomocnika tagów w pamięci podręcznej

- - -

### <a name="enabled"></a>Włączone    


| Typ atrybutu    | Prawidłowe wartości      |
|----------------   |----------------   |
| wartość logiczna           | wartość "true" (ustawienie domyślne)  |
|                   | "false"   |


Określa, czy zawartość ujęta w Pomocnik tagu pamięci podręcznej są buforowane. Wartość domyślna to `true`.  Jeśli ustawiono `false` to Pomocnik tagu pamięci podręcznej nie wpłyną buforowania w wyniku renderowania.

Przykład:

```cshtml
<cache enabled="true">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="expires-on"></a>expires-on 

| Typ atrybutu |           Przykładowa wartość            |
|----------------|------------------------------------|
| DateTimeOffset | "@new DateTime(2025,1,29,17,02,0)" |

Ustawia Data bezwzględna wygaśnięcia. Poniższy przykład spowoduje buforowanie zawartości Pomocnik tagu pamięci podręcznej aż do 17:02:00 w dniu 29 stycznia 2025.

Przykład:

```cshtml
<cache expires-on="@new DateTime(2025,1,29,17,02,0)">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="expires-after"></a>expires-after

| Typ atrybutu |        Przykładowa wartość         |
|----------------|------------------------------|
|    Przedział czasu    | "@TimeSpan.FromSeconds(120)" |

Określa długość czasu od pierwszego żądania, aby buforować zawartość. 

Przykład:

```cshtml
<cache expires-after="@TimeSpan.FromSeconds(120)">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="expires-sliding"></a>Przedłużanie wygasa

| Typ atrybutu |        Przykładowa wartość        |
|----------------|-----------------------------|
|    Przedział czasu    | "@TimeSpan.FromSeconds(60)" |

Ustawia czas, który wykluczyć wpisu pamięci podręcznej, jeśli nie uzyska dostępu.

Przykład:

```cshtml
<cache expires-sliding="@TimeSpan.FromSeconds(60)">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="vary-by-header"></a>różnią się w nagłówku

| Typ atrybutu    | Przykładowe wartości                |
|----------------   |----------------               |
| String            | "User-Agent"                  |
|                   | "User-Agent,content-encoding" |

Akceptuje wartość jednego nagłówka, lub rozdzielaną przecinkami listę wartości nagłówka, które mogą powodować odświeżanie pamięci podręcznej, gdy zmienią się one. Poniższy przykład monitoruje wartość nagłówka `User-Agent`. Przykład będzie buforować zawartość dla każdego innego `User-Agent` przesłanym do serwera sieci web.

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

Akceptuje wartość jednego nagłówka, lub rozdzielaną przecinkami listę wartości nagłówka, które wyzwalać odświeżanie pamięci podręcznej po zmianie wartości nagłówka. Poniższy przykład sprawdza wartości `Make` i `Model`.

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

Akceptuje wartość jednego nagłówka, lub rozdzielaną przecinkami listę wartości nagłówka, które mogą powodować odświeżanie pamięci podręcznej, gdy parametr danych trasy wartości zmiany. Przykład:

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

Akceptuje wartość jednego nagłówka, lub rozdzielaną przecinkami listę wartości nagłówka, które mogą powodować odświeżanie pamięci podręcznej, gdy zmiany (s) wartości nagłówka. Poniższy przykład sprawdza plik cookie skojarzone z tożsamości platformy ASP.NET Core. Gdy użytkownik jest uwierzytelniany żądania plik cookie należy ustawić, co powoduje wyzwolenie odświeżania pamięci podręcznej.

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
|                     | "false" (ustawienie domyślne) |

Określa, czy pamięć podręczną należy resetować po zmianie zalogowanego użytkownika (lub w kontekście jednostki). Bieżący użytkownik jest także znana jako jednostki kontekstu żądania i mogą być wyświetlane w widoku Razor, odwołując się do `@User.Identity.Name`.

Poniższy przykład sprawdza aktualnie zalogowanego użytkownika.  

Przykład:

```cshtml
<cache vary-by-user="true">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

Za pomocą tego atrybutu przechowuje zawartość w pamięci podręcznej przez cykl logowania i Wyloguj.  Korzystając z `vary-by-user="true"`, działania logowania i wyloguj unieważnia zawartość pamięci podręcznej dla tego uwierzytelnionego użytkownika.  Pamięć podręczna jest unieważnione, ponieważ nowa wartość unikatowego pliku cookie jest generowany podczas logowania. Pamięć podręczna jest zachowywana na potrzeby stanu anonimowe pliki cookie nie istnieje lub utracił ważność. Oznacza to, jeśli żaden użytkownik nie jest zalogowany, pamięci podręcznej zostaną zachowane.

- - -

### <a name="vary-by"></a>różnią się przez

| Typ atrybutu | Przykładowe wartości |
|----------------|----------------|
|     String     |    "@Model"    |

Umożliwia dostosowanie pobiera buforowane dane. Gdy zostanie zaktualizowany obiekt odwołuje się ten atrybut ciągu wartości zmiany, zawartość Pomocnik tagu pamięci podręcznej. Często ciągów wartości modelu są przypisane do tego atrybutu.  Skutecznie oznacza to, że aktualizacja dowolną z wartości z połączonych unieważnia zawartość pamięci podręcznej.

W poniższym przykładzie założono metody kontrolera renderowania sum widoku wartość całkowitą dwa parametry trasy `myParam1` i `myParam2`i zwraca ją jako właściwość pojedynczego modelu. Po zmianie tej sumy zawartość Pomocnik tagu pamięci podręcznej jest renderowana i ponownie buforowany.  

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
| CacheItemPriority  | "Wysoka"                   |
|                    | "Niska" |
|                    | "NeverRemove" |
|                    | "Normal" |

Znajdują się wskazówki eksmisji pamięci podręcznej dostawcy wbudowaną pamięć podręczną. Wyklucz serwer sieci web `Low` najpierw pamięci podręcznej wpisów, po duże wykorzystanie pamięci.

Przykład:

```cshtml
<cache priority="High">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

`priority` Atrybutu nie gwarantuje określony poziom przechowywania w pamięci podręcznej. `CacheItemPriority` jest tylko sugestię. Ustawienie tego atrybutu na `NeverRemove` nie gwarantuje pamięci podręcznej zawsze zostaną zachowane. Zobacz [dodatkowe zasoby](#additional-resources) Aby uzyskać więcej informacji.

Pomocnik tagu pamięci podręcznej jest zależny od [usługa pamięci podręcznej pamięci](xref:performance/caching/memory). Pomocnik tagu pamięci podręcznej dodaje usługę, jeśli nie został dodany.

## <a name="additional-resources"></a>Dodatkowe zasoby

* [Buforowanie w pamięci](xref:performance/caching/memory)
* [Wprowadzenie do tożsamości](xref:security/authentication/identity)
