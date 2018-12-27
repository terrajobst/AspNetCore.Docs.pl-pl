---
title: Pomocnik tagu kotwicy w programie ASP.NET Core
author: pkellner
description: Dowiedz się, atrybuty Pomocnik tagu kotwicy programu ASP.NET Core i rolę, jaką każdy atrybut jest odtwarzany w rozszerzanie zachowanie HTML tag kotwicy.
ms.author: scaddie
ms.custom: mvc
ms.date: 12/18/2018
uid: mvc/views/tag-helpers/builtin-th/anchor-tag-helper
ms.openlocfilehash: 60fa0c00e40878a8227ca2bc8bdb0bc2bf9f8336
ms.sourcegitcommit: ea215df889e89db44037a6ac2f01baede0450da9
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/19/2018
ms.locfileid: "53595344"
---
# <a name="anchor-tag-helper-in-aspnet-core"></a>Pomocnik tagu kotwicy w programie ASP.NET Core

Przez [Peter Kellner](http://peterkellner.net) i [Scott Addie](https://github.com/scottaddie)

[Pomocnik tagu kotwicy](xref:Microsoft.AspNetCore.Mvc.TagHelpers.AnchorTagHelper) rozszerza standardowe kotwicy HTML (`<a ... ></a>`) tag przez dodanie nowych atrybutów. Według Konwencji nazwy atrybutu mają prefiks `asp-`. Element zakotwiczenia renderowanych `href` wartość atrybutu jest określana przez wartości `asp-` atrybutów.

Aby zapoznać się z omówieniem pomocnicy tagów, zobacz <xref:mvc/views/tag-helpers/intro>.

[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/tag-helpers/built-in/samples) ([sposobu pobierania](xref:index#how-to-download-a-sample))

*SpeakerController* jest używana w przykładach w tym dokumencie:

[!code-csharp[](samples/TagHelpersBuiltIn/Controllers/SpeakerController.cs?name=snippet_SpeakerController)]

Spis `asp-` atrybuty poniżej.

## <a name="asp-controller"></a>Kontroler ASP

[Kontrolera asp](xref:Microsoft.AspNetCore.Mvc.TagHelpers.AnchorTagHelper.Controller*) atrybut przypisuje kontrolera, używany do generowania adresu URL. Następujące znaczniki Wyświetla listę wszystkich prelegentów:

[!code-cshtml[](samples/TagHelpersBuiltIn/Views/Home/Index.cshtml?name=snippet_AspController)]

Wygenerowany kod HTML:

```html
<a href="/Speaker">All Speakers</a>
```

Jeśli `asp-controller` określony atrybut i `asp-action` nie jest domyślnie `asp-action` wartość jest skojarzony z widokiem aktualnie wykonywanej akcji kontrolera. Jeśli `asp-action` zostanie pominięty w poprzednim znaczników, i Pomocnik tagu kotwicy jest używana w *HomeController*firmy *indeksu* widoku (*/Home*), to wygenerowany kod HTML:

```html
<a href="/Home">All Speakers</a>
```

## <a name="asp-action"></a>Akcja ASP

[Akcji asp](xref:Microsoft.AspNetCore.Mvc.TagHelpers.AnchorTagHelper.Action*) wartość atrybutu reprezentuje nazwę akcji kontrolera, które są zawarte w wygenerowanym `href` atrybutu. Następujące znaczniki ustawia wygenerowany `href` wartość atrybutu do strony ocen osoby mówiącej:

[!code-cshtml[](samples/TagHelpersBuiltIn/Views/Home/Index.cshtml?name=snippet_AspAction)]

Wygenerowany kod HTML:

```html
<a href="/Speaker/Evaluations">Speaker Evaluations</a>
```

Jeśli nie `asp-controller` atrybut jest określony, używany jest kontroler domyślne wywoływania wyświetlanie, wykonywanie bieżącego widoku.

Jeśli `asp-action` wartość atrybutu jest `Index`, a następnie żadna akcja jest dołączany do adresu URL prowadzącego do wywołania domyślnych `Index` akcji. Akcja określony (lub przywrócona wartość domyślna), musi istnieć w kontrolerze, do którego odwołuje się `asp-controller`.

## <a name="asp-route-value"></a>asp-route-{value}

[Asp - route-{value}](xref:Microsoft.AspNetCore.Mvc.TagHelpers.AnchorTagHelper.RouteValues*) atrybut umożliwia prefiks trasy symboli wieloznacznych. Wszystkie wartości zajmuje `{value}` symbol zastępczy jest interpretowany jako potencjalne parametru trasy. Jeśli trasa domyślna nie zostanie znalezione, jest dołączany prefiks trasy do wygenerowany `href` atrybutu jako parametru żądania i wartości. W przeciwnym razie zostanie zastąpiony w szablonie trasy.

Należy wziąć pod uwagę następujące akcji kontrolera:

[!code-csharp[](samples/TagHelpersBuiltIn/Controllers/BuiltInTagController.cs?name=snippet_AnchorTagHelperAction)]

Za pomocą domyślnego szablonu trasy zdefiniowane w *Startup.Configure*:

[!code-csharp[](samples/TagHelpersBuiltIn/Startup.cs?name=snippet_UseMvc&highlight=8-10)]

Widok MVC korzysta z modelu, dostarczone przez akcji, w następujący sposób:

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

Trasa domyślna `{id?}` zostały dopasowane do symbolu zastępczego. Wygenerowany kod HTML:

```html
<a href="/Speaker/Detail/12">SpeakerId: 12</a>
```

Załóżmy, że prefiks trasy nie jest częścią zgodnego szablonu routingu, podobnie jak w poniższym widoku MVC:

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

Poniższy kod HTML jest generowany, ponieważ `speakerid` nie został znaleziony w pasującej trasy:

```html
<a href="/Speaker/Detail?speakerid=12">SpeakerId: 12</a>
```

Jeśli `asp-controller` lub `asp-action` nie są określone, a następnie tego samego procesu przetwarzania domyślne występuje, ponieważ jest `asp-route` atrybutu.

## <a name="asp-route"></a>trasy ASP

[Trasy asp](xref:Microsoft.AspNetCore.Mvc.TagHelpers.AnchorTagHelper.Route*) atrybut jest używany do tworzenia adresu URL połączenie bezpośrednio z trasą mającą nazwę. Przy użyciu [atrybuty routingu](xref:mvc/controllers/routing#attribute-routing), może mieć nazwę trasy, jak pokazano na `SpeakerController` i wykorzystywane w jej `Evaluations` akcji:

[!code-csharp[](samples/TagHelpersBuiltIn/Controllers/SpeakerController.cs?range=22-24)]

W niej następujące znaczniki `asp-route` atrybut odwołuje się do nazwanej trasy:

[!code-cshtml[](samples/TagHelpersBuiltIn/Views/Home/Index.cshtml?name=snippet_AspRoute)]

Pomocnik tagu kotwicy generuje trasę bezpośrednio do tej akcji kontrolera, używając adresu URL */osoby mówiącej/ocen*. Wygenerowany kod HTML:

```html
<a href="/Speaker/Evaluations">Speaker Evaluations</a>
```

Jeśli `asp-controller` lub `asp-action` określono oprócz `asp-route`, trasy, generowany jest oczekiwanych. Aby uniknąć konfliktu trasy `asp-route` nie powinny być używane z `asp-controller` i `asp-action` atrybutów.

## <a name="asp-all-route-data"></a>asp-all-route-data

[Asp-all danych trasy](xref:Microsoft.AspNetCore.Mvc.TagHelpers.AnchorTagHelper.RouteValues*) atrybutu obsługuje tworzenie słownik par klucz wartość. Klucz jest nazwa parametru, a wartość jest wartością parametru.

W poniższym przykładzie słownik zostanie zainicjowana i przekazywane do widoku Razor. Alternatywnie można przekazać dane przy użyciu modelu.

[!code-cshtml[](samples/TagHelpersBuiltIn/Views/Home/Index.cshtml?name=snippet_AspAllRouteData)]

Powyższy kod generuje poniższy kod HTML:

```html
<a href="/Speaker/EvaluationsCurrent?speakerId=11&currentYear=true">Speaker Evaluations</a>
```

`asp-all-route-data` Słownika jest spłaszczany, aby utworzyć ciąg zapytania spełnienie wymagań przeciążonej `Evaluations` akcji:

[!code-csharp[](samples/TagHelpersBuiltIn/Controllers/SpeakerController.cs?range=26-30)]

Jeśli jakiekolwiek klucze w słowniku zgodne parametrów trasy, te wartości są zastępowane dla trasy, zgodnie z potrzebami. Niezgodny wartości są generowane jako parametry żądania.

## <a name="asp-fragment"></a>asp-fragment

[Fragmentu asp](xref:Microsoft.AspNetCore.Mvc.TagHelpers.AnchorTagHelper.Fragment*) atrybut definiuje fragmentu adresu URL do dołączenia do adresu URL. Pomocnik tagu kotwicy dodaje znaku numeru (#). Należy wziąć pod uwagę następujące znaczniki:

[!code-cshtml[](samples/TagHelpersBuiltIn/Views/Home/Index.cshtml?name=snippet_AspFragment)]

Wygenerowany kod HTML:

```html
<a href="/Speaker/Evaluations#SpeakerEvaluations">Speaker Evaluations</a>
```

Tagi wyznaczania wartości skrótu są przydatne podczas kompilowania aplikacji po stronie klienta. Umożliwia proste oznaczenie i wyszukiwania w języku JavaScript, na przykład.

## <a name="asp-area"></a>obszar ASP

[Obszaru asp](xref:Microsoft.AspNetCore.Mvc.TagHelpers.AnchorTagHelper.Area*) atrybut ustawia nazwę obszaru używane do ustawiania odpowiednie trasy. Poniższe przykłady przedstawić sposób, w jaki `asp-area` atrybutu powoduje, że ponowne mapowanie trasy.

### <a name="usage-in-razor-pages"></a>Użycie stron Razor

Obszary stron razor są obsługiwane w programie ASP.NET Core 2.1 lub nowszej.

Należy wziąć pod uwagę następujące hierarchii katalogów:

* **{Nazwa projektu}**
  * **wwwroot**
  * **Obszary**
    * **Sesje**
      * **Strony**
        * *\_ViewStart.cshtml*
        * *Index.cshtml*
        * *Index.cshtml.CS*
  * **Strony**

Kod znaczników, aby odwołać się do *sesje* obszaru *indeksu* jest strona Razor:

[!code-cshtml[](samples/TagHelpersBuiltIn/Views/Home/Index.cshtml?name=snippet_AspAreaRazorPages)]

Wygenerowany kod HTML:

```html
<a href="/Sessions">View Sessions</a>
```

> [!TIP]
> Aby zapewnić obsługę obszary w aplikacji stron Razor, wykonaj jedną z następujących czynności w `Startup.ConfigureServices`:
>
> * Ustaw [zgodność wersji](xref:mvc/compatibility-version) do 2.1 lub nowszej.
> * Ustaw [RazorPagesOptions.AllowAreas](xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.AllowAreas*) właściwości `true`:
>
>   [!code-csharp[](samples/TagHelpersBuiltIn/Startup.cs?name=snippet_AllowAreas)]

### <a name="usage-in-mvc"></a>Użycie w MVC

Należy wziąć pod uwagę następujące hierarchii katalogów:

* **{Nazwa projektu}**
  * **wwwroot**
  * **Obszary**
    * **Blogi**
      * **Kontrolery**
        * *HomeController.cs*
      * **Widoki**
        * **Strona główna**
          * *AboutBlog.cshtml*
          * *Index.cshtml*
        * *\_ViewStart.cshtml*
  * **Kontrolery**

Ustawienie `asp-area` "Blogi" prefiksów katalogu *obszarów/blogi* do tras skojarzone kontrolerów i widoków ten tag kotwicy. Kod znaczników, aby odwołać się do *AboutBlog* widok jest:

[!code-cshtml[](samples/TagHelpersBuiltIn/Views/Home/Index.cshtml?name=snippet_AspArea)]

Wygenerowany kod HTML:

```html
<a href="/Blogs/Home/AboutBlog">About Blog</a>
```

> [!TIP]
> Aby zapewnić obsługę obszary w aplikacji MVC, szablon trasy musi zawierać odwołanie do obszaru, jeśli taki istnieje. Ten szablon jest reprezentowany przez drugi parametr `routes.MapRoute` metody wywołania w *Startup.Configure*:
>
> [!code-csharp[](samples/TagHelpersBuiltIn/Startup.cs?name=snippet_UseMvc&highlight=5)]

## <a name="asp-protocol"></a>Protokół ASP

[Protokołu asp](xref:Microsoft.AspNetCore.Mvc.TagHelpers.AnchorTagHelper.Protocol*) atrybut jest określająca protokół (takie jak `https`) w adresie URL. Na przykład:

[!code-cshtml[](samples/TagHelpersBuiltIn/Views/Home/Index.cshtml?name=snippet_AspProtocol)]

Wygenerowany kod HTML:

```html
<a href="https://localhost/Home/About">About</a>
```

W przykładzie nazwa hosta jest localhost. Pomocnik tagu kotwicy używa domeny publicznej witryny sieci Web podczas generowania adresu URL.

## <a name="asp-host"></a>ASP host

[Hosta asp](xref:Microsoft.AspNetCore.Mvc.TagHelpers.AnchorTagHelper.Host*) atrybut służy do określania nazwy hosta w adresie URL. Na przykład:

[!code-cshtml[](samples/TagHelpersBuiltIn/Views/Home/Index.cshtml?name=snippet_AspHost)]

Wygenerowany kod HTML:

```html
<a href="https://microsoft.com/Home/About">About</a>
```

## <a name="asp-page"></a>Strona ASP

[Strona asp](xref:Microsoft.AspNetCore.Mvc.TagHelpers.AnchorTagHelper.Page*) atrybut jest używany ze stronami Razor. Użyj go, aby ustawić tag kotwicy `href` wartość atrybutu do określonej strony. Tworzenie prefiksu nazwy strony, od ukośnika ("/") tworzy adres URL.

Poniższy przykład wskazuje uczestnik strony Razor:

[!code-cshtml[](samples/TagHelpersBuiltIn/Views/Home/Index.cshtml?name=snippet_AspPage)]

Wygenerowany kod HTML:

```html
<a href="/Attendee">All Attendees</a>
```

`asp-page` Atrybutu jest wzajemnie wykluczających się przy użyciu `asp-route`, `asp-controller`, i `asp-action` atrybutów. Jednak `asp-page` mogą być używane z `asp-route-{value}` do sterowania routingiem, tak jak pokazano w następującym:

[!code-cshtml[](samples/TagHelpersBuiltIn/Views/Home/Index.cshtml?name=snippet_AspPageAspRouteId)]

Wygenerowany kod HTML:

```html
<a href="/Attendee?attendeeid=10">View Attendee</a>
```

## <a name="asp-page-handler"></a>asp-page-handler

[Program obsługi stron asp](xref:Microsoft.AspNetCore.Mvc.TagHelpers.AnchorTagHelper.PageHandler*) atrybut jest używany ze stronami Razor. Jest ona przeznaczona do konsolidacji do obsługi określonej strony.

Należy wziąć pod uwagę następujące procedury obsługi strony:

[!code-csharp[](samples/TagHelpersBuiltIn/Pages/Attendee.cshtml.cs?name=snippet_OnGetProfileHandler)]

Skojarzonej z modelem strony łącza znaczników do `OnGetProfile` obsługi strony. Uwaga `On<Verb>` prefiks nazwy metody obsługi strony zostanie pominięty w `asp-page-handler` wartość atrybutu. Jeśli metoda jest asynchroniczna, `Async` sufiks zostanie pominięty, zbyt.

[!code-cshtml[](samples/TagHelpersBuiltIn/Views/Home/Index.cshtml?name=snippet_AspPageHandler)]

Wygenerowany kod HTML:

```html
<a href="/Attendee?attendeeid=12&handler=Profile">Attendee Profile</a>
```

## <a name="additional-resources"></a>Dodatkowe zasoby

* <xref:mvc/controllers/areas>
* <xref:razor-pages/index>
* <xref:mvc/compatibility-version>
