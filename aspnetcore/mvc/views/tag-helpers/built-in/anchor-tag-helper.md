---
title: Pomocnik tagu kotwicy w ASP.NET Core
author: pkellner
description: Odkryj atrybuty pomocnika ASP.NET Core znacznika i rolę, jaką każdy atrybut odgrywa w rozszerzeniu zachowania tagu kotwicy HTML.
ms.author: scaddie
ms.custom: mvc
ms.date: 10/13/2019
uid: mvc/views/tag-helpers/builtin-th/anchor-tag-helper
ms.openlocfilehash: 6bfbad39115c7823b5677d3c52ca64cfb0683037
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78664004"
---
# <a name="anchor-tag-helper-in-aspnet-core"></a>Pomocnik tagu kotwicy w ASP.NET Core

Według [Peterowi Kellner](https://peterkellner.net) i [Scott Addie](https://github.com/scottaddie)

[Pomocnik tagu kotwicy](xref:Microsoft.AspNetCore.Mvc.TagHelpers.AnchorTagHelper) rozszerza standardowy tag kotwicy HTML (`<a ... ></a>`) przez dodanie nowych atrybutów. Według Konwencji nazwy atrybutów są poprzedzone prefiksem `asp-`. Wartość atrybutu `href` renderowanego elementu zakotwiczenia jest określana na podstawie wartości atrybutów `asp-`.

Aby zapoznać się z omówieniem pomocników tagów, zobacz <xref:mvc/views/tag-helpers/intro>.

[Wyświetl lub pobierz przykładowy kod](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/views/tag-helpers/built-in/samples) ([jak pobrać](xref:index#how-to-download-a-sample))

*SpeakerController* jest używany w przykładach w tym dokumencie:

[!code-csharp[](samples/TagHelpersBuiltIn/Controllers/SpeakerController.cs?name=snippet_SpeakerController)]

## <a name="anchor-tag-helper-attributes"></a>Atrybuty pomocnika tagu kotwicy

### <a name="asp-controller"></a>ASP-Controller

Atrybut [kontrolera ASP](xref:Microsoft.AspNetCore.Mvc.TagHelpers.AnchorTagHelper.Controller*) przypisuje kontroler używany do generowania adresu URL. Następujące znaczniki zawierają listę wszystkich głośników:

[!code-cshtml[](samples/TagHelpersBuiltIn/Views/Home/Index.cshtml?name=snippet_AspController)]

Wygenerowany kod HTML:

```html
<a href="/Speaker">All Speakers</a>
```

Jeśli atrybut `asp-controller` jest określony i nie `asp-action`, domyślną wartością `asp-action` jest akcja kontrolera skojarzona z aktualnie wykonywanym widokiem. Jeśli `asp-action` zostanie pominięty z poprzedniego znacznika, a pomocnik tagu kotwicy jest używany w widoku indeksu *HomeController*( */Home*), wygenerowany kod HTML to:

```html
<a href="/Home">All Speakers</a>
```

### <a name="asp-action"></a>ASP — akcja

Wartość atrybutu [akcji ASP](xref:Microsoft.AspNetCore.Mvc.TagHelpers.AnchorTagHelper.Action*) reprezentuje nazwę akcji kontrolera uwzględnione w wygenerowanym atrybucie `href`. Poniższe znaczniki ustawiają wygenerowaną wartość atrybutu `href` na stronie oceny głośników:

[!code-cshtml[](samples/TagHelpersBuiltIn/Views/Home/Index.cshtml?name=snippet_AspAction)]

Wygenerowany kod HTML:

```html
<a href="/Speaker/Evaluations">Speaker Evaluations</a>
```

Jeśli nie określono atrybutu `asp-controller`, używany jest domyślny kontroler wywołujący widok wykonujący bieżący widok.

Jeśli `asp-action` wartość atrybutu jest `Index`, w adresie URL nie zostanie dołączona żadna akcja, co prowadzi do wywołania domyślnej akcji `Index`. Określona akcja (lub domyślna) musi istnieć na kontrolerze, do którego odwołuje się `asp-controller`.

### <a name="asp-route-value"></a>asp-route-{value}

Atrybut [ASP-Route-{Value}](xref:Microsoft.AspNetCore.Mvc.TagHelpers.AnchorTagHelper.RouteValues*) włącza prefiks trasy wieloznacznej. Wszystkie wartości, które zajmują `{value}` symbol zastępczy, są interpretowane jako potencjalna wartość parametru trasy. Jeśli trasa domyślna nie zostanie znaleziona, ten prefiks trasy jest dołączany do wygenerowanego atrybutu `href` jako parametr i wartość żądania. W przeciwnym razie zostanie zastąpiony w szablonie trasy.

Rozważmy następującą akcję kontrolera:

[!code-csharp[](samples/TagHelpersBuiltIn/Controllers/BuiltInTagController.cs?name=snippet_AnchorTagHelperAction)]

Z domyślnym szablonem trasy zdefiniowanym podczas *uruchamiania. Konfiguracja*:

[!code-csharp[](samples/TagHelpersBuiltIn/Startup.cs?name=snippet_UseMvc&highlight=8-10)]

Widok MVC używa modelu dostarczonego przez akcję w następujący sposób:

```cshtml
@model Speaker
<!DOCTYPE html>
<html>
<body>
    <a asp-controller="Speaker"
       asp-action="Detail" 
       asp-route-id="@Model.SpeakerId">SpeakerId: @Model.SpeakerId</a>
</body>
</html>
```

Dopasowano symbol zastępczy `{id?}` trasy domyślnej. Wygenerowany kod HTML:

```html
<a href="/Speaker/Detail/12">SpeakerId: 12</a>
```

Załóżmy, że prefiks trasy nie jest częścią zgodnego szablonu routingu, tak jak w przypadku następującego widoku MVC:

```cshtml
@model Speaker
<!DOCTYPE html>
<html>
<body>
    <a asp-controller="Speaker"
       asp-action="Detail"
       asp-route-speakerid="@Model.SpeakerId">SpeakerId: @Model.SpeakerId</a>
<body>
</html>
```

Następujący kod HTML został wygenerowany, ponieważ nie znaleziono `speakerid` w zgodnej trasie:

```html
<a href="/Speaker/Detail?speakerid=12">SpeakerId: 12</a>
```

Jeśli `asp-controller` lub `asp-action` nie są określone, to to samo domyślne przetwarzanie zostanie wykonane, tak jak w atrybucie `asp-route`.

### <a name="asp-route"></a>ASP — Route

Atrybut [ASP-Route](xref:Microsoft.AspNetCore.Mvc.TagHelpers.AnchorTagHelper.Route*) służy do tworzenia łączenia adresów URL bezpośrednio z nazwaną trasą. Przy użyciu [atrybutów routingu](xref:mvc/controllers/routing#attribute-routing)trasy mogą być nazwane, jak pokazano w `SpeakerController` i używane w akcji `Evaluations`:

[!code-csharp[](samples/TagHelpersBuiltIn/Controllers/SpeakerController.cs?range=22-24)]

W poniższym znaczniku atrybut `asp-route` odwołuje się do nazwanej trasy:

[!code-cshtml[](samples/TagHelpersBuiltIn/Views/Home/Index.cshtml?name=snippet_AspRoute)]

Pomocnik tagu kotwicy generuje trasę bezpośrednio do tej akcji kontrolera przy użyciu adresu URL */Speaker/Evaluations*. Wygenerowany kod HTML:

```html
<a href="/Speaker/Evaluations">Speaker Evaluations</a>
```

Jeśli oprócz `asp-route`określono `asp-controller` lub `asp-action`, wygenerowana trasa nie jest oczekiwana. Aby uniknąć konfliktu trasy, `asp-route` nie powinien być używany z atrybutami `asp-controller` i `asp-action`.

### <a name="asp-all-route-data"></a>asp-all-route-data

Atrybut [ASP-All-Route-Data](xref:Microsoft.AspNetCore.Mvc.TagHelpers.AnchorTagHelper.RouteValues*) obsługuje tworzenie słownika par klucz-wartość. Klucz jest nazwą parametru, a wartość jest wartością parametru.

W poniższym przykładzie słownik został zainicjowany i przeszedł do widoku Razor. Alternatywnie dane mogły zostać przesłane przy użyciu modelu.

[!code-cshtml[](samples/TagHelpersBuiltIn/Views/Home/Index.cshtml?name=snippet_AspAllRouteData)]

Poprzedni kod generuje następujący HTML:

```html
<a href="/Speaker/EvaluationsCurrent?speakerId=11&currentYear=true">Speaker Evaluations</a>
```

Słownik `asp-all-route-data` jest spłaszczony, aby utworzyć ciąg QueryString spełniający wymagania przeciążonej `Evaluations` akcji:

[!code-csharp[](samples/TagHelpersBuiltIn/Controllers/SpeakerController.cs?range=26-30)]

Jeśli wszystkie klucze w słowniku pasują do parametrów trasy, te wartości są zastępowane w marszrucie, zgodnie z potrzebami. Inne niezgodne wartości są generowane jako parametry żądania.

### <a name="asp-fragment"></a>asp-fragment

Atrybut [ASP-fragment](xref:Microsoft.AspNetCore.Mvc.TagHelpers.AnchorTagHelper.Fragment*) definiuje fragment adresu URL do dołączenia do adresu URL. Pomocnik tagu kotwicy dodaje znak skrótu (#). Rozważ następujące oznakowanie:

[!code-cshtml[](samples/TagHelpersBuiltIn/Views/Home/Index.cshtml?name=snippet_AspFragment)]

Wygenerowany kod HTML:

```html
<a href="/Speaker/Evaluations#SpeakerEvaluations">Speaker Evaluations</a>
```

Tagi skrótów są przydatne podczas tworzenia aplikacji po stronie klienta. Mogą one służyć do łatwego oznaczania i wyszukiwania w języku JavaScript, na przykład.

### <a name="asp-area"></a>obszar ASP

Atrybut [obszaru ASP](xref:Microsoft.AspNetCore.Mvc.TagHelpers.AnchorTagHelper.Area*) ustawia nazwę obszaru używaną do ustawiania odpowiedniej trasy. W poniższych przykładach przedstawiono sposób, w jaki atrybut `asp-area` powoduje ponowne mapowanie tras.

#### <a name="usage-in-razor-pages"></a>Użycie w Razor Pages

Obszary Razor Pages są obsługiwane w ASP.NET Core 2,1 lub nowszych.

Weź pod uwagę następującą hierarchię katalogów:

* **{Nazwa projektu}**
  * **wwwroot**
  * **Obszary**
    * **Obrad**
      * **Strony**
        * *\_ViewStart. cshtml*
        * *Index. cshtml*
        * *Index.cshtml.cs*
  * **Strony**

Znacznikiem odwołującym się do *indeksu* obszaru *sesji* Razor jest:

[!code-cshtml[](samples/TagHelpersBuiltIn/Views/Home/Index.cshtml?name=snippet_AspAreaRazorPages)]

Wygenerowany kod HTML:

```html
<a href="/Sessions">View Sessions</a>
```

> [!TIP]
> Aby obsłużyć obszary w aplikacji Razor Pages, wykonaj jedną z następujących czynności w `Startup.ConfigureServices`:
>
> * Ustaw [wersję zgodności](xref:mvc/compatibility-version) na 2,1 lub nowszą.
> * Ustaw właściwość [RazorPagesOptions. AllowAreas](xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.AllowAreas*) na `true`:
>
>   [!code-csharp[](samples/TagHelpersBuiltIn/Startup.cs?name=snippet_AllowAreas)]

#### <a name="usage-in-mvc"></a>Użycie w MVC

Weź pod uwagę następującą hierarchię katalogów:

* **{Nazwa projektu}**
  * **wwwroot**
  * **Obszary**
    * **Blogi**
      * **Kontrolery**
        * *HomeController.cs*
      * **Widoki**
        * **Mowa**
          * *AboutBlog. cshtml*
          * *Index. cshtml*
        * *\_ViewStart. cshtml*
  * **Kontrolery**

Ustawienie `asp-area` na "Blogi" prefiksuje obszary katalogu */Blogi* do tras skojarzonych kontrolerów i widoków dla tego tagu zakotwiczenia. Znacznikiem odwołującym się do widoku *AboutBlog* jest:

[!code-cshtml[](samples/TagHelpersBuiltIn/Views/Home/Index.cshtml?name=snippet_AspArea)]

Wygenerowany kod HTML:

```html
<a href="/Blogs/Home/AboutBlog">About Blog</a>
```

> [!TIP]
> Aby obsługiwać obszary w aplikacji MVC, szablon trasy musi zawierać odwołanie do obszaru, jeśli istnieje. Ten szablon jest reprezentowany przez drugi parametr wywołania metody `routes.MapRoute` w trakcie *uruchamiania. Skonfiguruj*:
>
> [!code-csharp[](samples/TagHelpersBuiltIn/Startup.cs?name=snippet_UseMvc&highlight=5)]

### <a name="asp-protocol"></a>ASP — protokół

Atrybut [ASP-Protocol](xref:Microsoft.AspNetCore.Mvc.TagHelpers.AnchorTagHelper.Protocol*) służy do określania protokołu (takiego jak `https`) w adresie URL. Na przykład:

[!code-cshtml[](samples/TagHelpersBuiltIn/Views/Home/Index.cshtml?name=snippet_AspProtocol)]

Wygenerowany kod HTML:

```html
<a href="https://localhost/Home/About">About</a>
```

Nazwa hosta w przykładzie jest localhost. Pomocnik tagu kotwicy używa domeny publicznej witryny sieci Web podczas generowania adresu URL.

### <a name="asp-host"></a>ASP — Host

Atrybut [ASP-Host](xref:Microsoft.AspNetCore.Mvc.TagHelpers.AnchorTagHelper.Host*) służy do określania nazwy hosta w adresie URL. Na przykład:

[!code-cshtml[](samples/TagHelpersBuiltIn/Views/Home/Index.cshtml?name=snippet_AspHost)]

Wygenerowany kod HTML:

```html
<a href="https://microsoft.com/Home/About">About</a>
```

### <a name="asp-page"></a>ASP — Strona

Atrybut [ASP-Page](xref:Microsoft.AspNetCore.Mvc.TagHelpers.AnchorTagHelper.Page*) jest używany z Razor Pages. Użyj go, aby ustawić wartość atrybutu `href` znacznika kotwicy na określoną stronę. Prefiks nazwy strony z ukośnikiem ("/") powoduje utworzenie adresu URL.

Poniższy przykład wskazuje na stronę Razor dla uczestnika:

[!code-cshtml[](samples/TagHelpersBuiltIn/Views/Home/Index.cshtml?name=snippet_AspPage)]

Wygenerowany kod HTML:

```html
<a href="/Attendee">All Attendees</a>
```

Atrybut `asp-page` wzajemnie się wykluczają z atrybutów `asp-route`, `asp-controller`i `asp-action`. `asp-page` można jednak używać z `asp-route-{value}` do kontrolowania routingu, ponieważ ilustruje to następujące oznakowanie:

[!code-cshtml[](samples/TagHelpersBuiltIn/Views/Home/Index.cshtml?name=snippet_AspPageAspRouteId)]

Wygenerowany kod HTML:

```html
<a href="/Attendee?attendeeid=10">View Attendee</a>
```

### <a name="asp-page-handler"></a>asp-page-handler

Atrybut [ASP-Page-Handler](xref:Microsoft.AspNetCore.Mvc.TagHelpers.AnchorTagHelper.PageHandler*) jest używany z Razor Pages. Jest on przeznaczony do łączenia z konkretnymi programami obsługi stron.

Weź pod uwagę następujące procedury obsługi stron:

[!code-csharp[](samples/TagHelpersBuiltIn/Pages/Attendee.cshtml.cs?name=snippet_OnGetProfileHandler)]

Skojarzone ze znacznikiem model strony łącza do obsługi stron `OnGetProfile`. Zwróć uwagę, że prefiks `On<Verb>` nazwy metody obsługi stron został pominięty w `asp-page-handler` wartość atrybutu. Gdy metoda jest asynchroniczna, sufiks `Async` jest pomijany.

[!code-cshtml[](samples/TagHelpersBuiltIn/Views/Home/Index.cshtml?name=snippet_AspPageHandler)]

Wygenerowany kod HTML:

```html
<a href="/Attendee?attendeeid=12&handler=Profile">Attendee Profile</a>
```

## <a name="additional-resources"></a>Dodatkowe zasoby

* <xref:mvc/controllers/areas>
* <xref:razor-pages/index>
* <xref:mvc/compatibility-version>
