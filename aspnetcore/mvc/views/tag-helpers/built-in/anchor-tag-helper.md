---
title: "Zakotwicz pomocnika tagów | Dokumentacja firmy Microsoft"
author: pkellner
description: "Pokazuje, jak pracować z Pomocnika Tag kotwicy"
keywords: "Platformy ASP.NET Core pomocnika tagów"
ms.author: riande
manager: wpickett
ms.date: 12/20/2017
ms.topic: article
ms.assetid: c045d485-d1dc-4cea-a675-46be83b7a011
ms.technology: aspnet
ms.prod: aspnet-core
uid: mvc/views/tag-helpers/builtin-th/anchor-tag-helper
ms.openlocfilehash: 503ad7c4ce8c4f08b2a06dbe9f985566f54d3ca2
ms.sourcegitcommit: 44a62f59d4db39d685c4487a0345a486be18d7c7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/21/2017
---
# <a name="anchor-tag-helper"></a>Pomocnik Tag kotwicy

Przez [Kellner Peterowi](http://peterkellner.net) 

Pomocnik Tag kotwicy zwiększa kotwicy HTML (`<a ... ></a>`) tag przez dodanie nowych atrybutów. Link wygenerowany (na `href` tagu) jest tworzony przy użyciu nowych atrybutów. Ten adres URL może zawierać protokół opcjonalne, na przykład protokołu https.

Poniżej kontrolera prelegenta jest używany w przykłady w tym dokumencie.

**SpeakerController.cs** 

[!code-csharp[SpeakerController](sample/TagHelpersBuiltInAspNetCore/src/TagHelpersBuiltInAspNetCore/Controllers/SpeakerController.cs)]


## <a name="anchor-tag-helper-attributes"></a>Atrybuty pomocnika Tag kotwicy

### <a name="asp-controller"></a>Kontroler ASP

`asp-controller`Służy do skojarzenia kontrolera, który będzie służyć do generowania adresu URL. Kontrolerów określonych musi istnieć w bieżącym projekcie. Poniższy kod wyświetla listę wszystkich głośników: 

```cshtml
<a asp-controller="Speaker" asp-action="Index">All Speakers</a>
```

Zostanie wygenerowany kod znaczników:

```html
<a href="/Speaker">All Speakers</a>
```

Jeśli `asp-controller` określono i `asp-action` nie jest domyślnie `asp-action` jest domyślną metodą kontrolera widoku aktualnie wykonywane. Czy jest w powyższym przykładzie, jeśli `asp-action` pozostało, i jest generowany tego pomocnika Tag kotwicy na podstawie *HomeController*w `Index` widoku (**/Home**), zostanie wygenerowany kod znaczników:

```html
<a href="/Home">All Speakers</a>
```

### <a name="asp-action"></a>Akcja ASP

`asp-action`Nazwa metody akcji w kontrolerze, który zostanie uwzględniony w wygenerowanym `href`. Na przykład następujący kod ustawić wygenerowany `href` wskaż prelegenta strony szczegółów:

```html
<a asp-controller="Speaker" asp-action="Detail">Speaker Detail</a>
```

Zostanie wygenerowany kod znaczników:

```html
<a href="/Speaker/Detail">Speaker Detail</a>
```

Jeśli nie `asp-controller` atrybut został określony, zostanie użyty domyślny kontroler wywoływania widok wykonywania bieżącego widoku.  
 
Jeśli atrybut `asp-action` jest `Index`, a następnie żadna akcja ze strony jest dołączany do adresu URL, co prowadzi do domyślnej `Index` wywołania metody. Akcja określony (lub ustawiana domyślnie), musi istnieć w kontrolerze, do którego odwołuje się `asp-controller`.

### <a name="asp-page"></a>Strona ASP

Użyj `asp-page` atrybut w tagu zakotwiczenia, aby ustawić adres URL, aby wskazywał określonej strony. Prefiksu nazwy strony, od ukośnika "/" tworzy adres URL. W poniższym przykładzie adres URL wskazuje na stronie "Prelegenta" w bieżącym katalogu.

```cshtml
<a asp-page="/Speakers">All Speakers</a>
```

`asp-page` Atrybutu w poprzednim przykładzie kodu renderuje danych wyjściowych HTML w widoku, który jest podobny do następującego fragmentu kodu:

```html
<a href="/items?page=%2FSpeakers">Speakers</a>
``

The `asp-page` attribute is mutually exclusive with the `asp-route`, `asp-controller`, and `asp-action` attributes. However, `asp-page` can be used with `asp-route-id` to control routing, as the following code sample demonstrates:

```
cshtml<a asp-page="/Speaker" asp-route-id="@speaker.Id">prelegenta widoku</a>
```

The `asp-route-id` produces the following output:

```html
https://localhost:44399/Speakers/Index/2?page=%2FSpeaker
```


### <a name="asp-route-value"></a>ASP - route - {wartość value}

`asp-route-`jest symbol wieloznaczny prefiksu trasy. Wartość, umieszczone po kreska kończąca zostanie potraktowany jako potencjalne parametru trasy. Jeśli trasa domyślna nie zostanie znaleziony, prefiks trasy zostaną dodane do wygenerowanego href żądania parametr i wartość. W przeciwnym razie zostaną zastąpione w szablonie trasy.

Zakładając, że użytkownik ma metody kontrolera zdefiniowane w następujący sposób:

```csharp
public IActionResult AnchorTagHelper(string id)
{
    var speaker = new SpeakerData()
    {
        SpeakerId = 12
    };
    return View(viewName, speaker);
}
```

I mieć domyślnego szablonu trasy zdefiniowanej w Twojej *Startup.cs* w następujący sposób:

```csharp
app.UseMvc(routes =>
{
   routes.MapRoute(
    name: "default",
    template: "{controller=Home}/{action=Index}/{id?}");
});

```

**Cshtml** pliku, który zawiera pomocnika Tag kotwicy koniecznych do używania **prelegenta** przekazany z kontrolera widoku modelu parametr wygląda następująco:

```cshtml
@model SpeakerData
<!DOCTYPE html>
<html><body>
<a asp-controller='Speaker' asp-action='Detail' asp-route-id=@Model.SpeakerId>SpeakerId: @Model.SpeakerId</a>
<body></html>
```

Wygenerowany kod HTML będzie następujący ponieważ **identyfikator** został znaleziony w trasy domyślnej.

```html
<a href='/Speaker/Detail/12'>SpeakerId: 12</a>
```

Jeśli prefiks trasy nie jest częścią routingu znaleziono szablonu, który jest w przypadku następujących **cshtml** pliku:

```cshtml
@model SpeakerData
<!DOCTYPE html>
<html><body>
<a asp-controller='Speaker' asp-action='Detail' asp-route-speakerid=@Model.SpeakerId>SpeakerId: @Model.SpeakerId</a>
<body></html>
```

Wygenerowany kod HTML będzie następujący ponieważ **speakerid** nie został znaleziony w trasie dopasowane:

```html
<a href='/Speaker/Detail?speakerid=12'>SpeakerId: 12</a>
```

Jeśli dowolny `asp-controller` lub `asp-action` nie są określone, następnie tego samego przetwarzania domyślny jest kontynuowane, jak jest `asp-route` atrybutu.

### <a name="asp-route"></a>trasy ASP

`asp-route`zapewnia sposób utworzyć adres URL prowadzący bezpośrednio do nazwanej trasy. Przy użyciu atrybutów routingu, trasy nazwą może być pokazane na `SpeakerController` i używany w jego `Evaluations` metody.

`Name = "speakerevals"`Określa, że Generowanie trasę bezpośrednio do tej metody za pomocą adresu URL pomocnika Tag kotwicy `/Speaker/Evaluations`. Jeśli `asp-controller` lub `asp-action` określono oprócz `asp-route`, generowane trasy nie może być oczekiwań. `asp-route`nie należy używać przy użyciu jednej z wartości atrybutów `asp-controller` lub `asp-action` Aby uniknąć konfliktu trasy.

### <a name="asp-all-route-data"></a>ASP-all danych trasy

`asp-all-route-data`Umożliwia tworzenie słownik par kluczy i wartości gdzie klucz to nazwa parametru, a wartość jest wartością skojarzonego z tym kluczem.

Co w przykładzie poniżej przedstawiono Słownik wbudowany jest tworzony, i dane są przekazywane do widoku razor. Alternatywnie dane można również przekazywane za pomocą modelu.

```cshtml
@{
    var dict =
        new Dictionary<string, string>
        {
            {"speakerId", "11"},
            {"currentYear", "true"}
        };
}
<a asp-route="speakerevalscurrent"
asp-all-route-data="dict">SpeakerEvals</a>
```

Powyższy kod generuje następujący adres URL: http://localhost/Speaker/EvaluationsCurrent?speakerId=11&currentYear=true

Po kliknięciu łącza, metoda kontrolera `EvaluationsCurrent` jest wywoływana. Jest to, ponieważ ten kontroler ma dwa parametry ciągu, które utworzył z `asp-all-route-data` słownika.

Jeśli parametry trasy klucze w słowniku dopasowania, te wartości zostaną zastąpione w trasie zgodnie z potrzebami i inne niepasujące wartości zostaną wygenerowane jako parametry żądania.

### <a name="asp-fragment"></a>ASP fragment

`asp-fragment`Definiuje fragmentu adresu URL, aby dołączyć do adresu URL. Pomocnik Tag kotwicy spowoduje dodanie znaku numeru (#). Jeśli utworzysz tag:

```cshtml
<a asp-action="Evaluations" asp-controller="Speaker"  
   asp-fragment="SpeakerEvaluations">About Speaker Evals</a>
```

Zostanie wygenerowany adres URL: http://localhost/Speaker/Evaluations#SpeakerEvaluations

Skrót tagi są przydatne podczas tworzenia aplikacji po stronie klienta. Służy do łatwego oznaczenie i wyszukiwanie w języku JavaScript, na przykład.

### <a name="asp-area"></a>obszar ASP

`asp-area`Ustawia nazwę obszaru, która korzysta z platformy ASP.NET Core można ustawić odpowiednie trasy. Poniżej przedstawiono przykłady sposobu atrybut obszaru powoduje zmianę mapowania tras. Ustawienie `asp-area` blogach prefiksy katalogu `Areas/Blogs` do tras skojarzone kontrolery i widoki dla ten tag kotwicy.

* Nazwa projektu
  * Wwwroot
  * Obszary
    * Blogi
      * Kontrolery
        * HomeController.cs
      * Widoki
        * Home
          * Index.cshtml
          * AboutBlog.cshtml
  * Kontrolery

Określenie obszaru tag, który jest prawidłowy, takich jak ```area="Blogs"``` podczas odwoływania się do ```AboutBlog.cshtml``` pliku będzie wyglądać podobnie do następującej przy użyciu Pomocnika Tag kotwicy.

```cshtml
<a asp-action="AboutBlog" asp-controller="Home" asp-area="Blogs">Blogs About</a>
```

Wygenerowany kod HTML będzie zawierać segmentu obszarów i będzie wyglądać następująco:

```html
<a href="/Blogs/Home/AboutBlog">Blogs About</a>
```

> [!TIP]
> Obszarów MVC do pracy w aplikacji sieci web szablon trasy musi zawierać odwołanie do obszaru, jeśli istnieje. Szablonu, który jest drugi parametr z `routes.MapRoute` wywołania metody, będą wyświetlane jako:`template: '"{area:exists}/{controller=Home}/{action=Index}"'`

### <a name="asp-protocol"></a>Protokół ASP

`asp-protocol` Służy do określania protokół (takie jak `https`) w adresie URL. Przykład pomocnika Tag kotwicy, zawierającym nazwę protokołu będzie wyglądać w następujący sposób:

```<a asp-protocol="https" asp-action="About" asp-controller="Home">About</a>```

i wygeneruje HTML w następujący sposób:

```<a href="https://localhost/Home/About">About</a>```

Domena, w tym przykładzie jest localhost, ale pomocnika Tag kotwicy używa domeny publicznej witryny sieci Web podczas generowania adresu URL.

## <a name="additional-resources"></a>Dodatkowe zasoby

* [Obszary](xref:mvc/controllers/areas)
