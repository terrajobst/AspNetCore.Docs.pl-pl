---
title: Pomocnik Tag kotwicy w platformy ASP.NET Core
author: pkellner
description: "Odnalezienie atrybuty platformy ASP.NET Core zakotwiczenia Tag pomocnika i rolę, jaką odgrywa każdego atrybutu w rozszerzanie zachowanie tag kotwicy HTML."
manager: wpickett
ms.author: scaddie
ms.custom: mvc
ms.date: 01/31/2018
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: mvc/views/tag-helpers/builtin-th/anchor-tag-helper
ms.openlocfilehash: 9aca2d2263285de36efe12e6e267778d54149e9e
ms.sourcegitcommit: 493a215355576cfa481773365de021bcf04bb9c7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/15/2018
---
# <a name="anchor-tag-helper-in-aspnet-core"></a>Pomocnik Tag kotwicy w platformy ASP.NET Core

Przez [Kellner Peterowi](http://peterkellner.net) i [Scott Addie](https://github.com/scottaddie)

[Wyświetlić lub pobrać przykładowy kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/tag-helpers/built-in/samples) ([sposobu pobierania](xref:tutorials/index#how-to-download-a-sample))

[Pomocnika Tag kotwicy](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper) zwiększa standardowe kotwicy HTML (`<a ... ></a>`) tag przez dodanie nowych atrybutów. Konwencja, nazw atrybutów są poprzedzane prefiksem `asp-`. Element zakotwiczenia renderowanych `href` wartość atrybutu jest określany przez wartości `asp-` atrybutów.

*SpeakerController* jest używany w przykłady w tym dokumencie:

[!code-csharp[](samples/TagHelpersBuiltIn/Controllers/SpeakerController.cs?name=snippet_SpeakerController)]

Spis `asp-` następujące atrybuty.

## <a name="asp-controller"></a>Kontroler ASP

[Kontrolera asp](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.controller) atrybut przypisuje kontroler, używany do generowania adresu URL. Następujący kod zawiera listę wszystkich głośników:

[!code-cshtml[](samples/TagHelpersBuiltIn/Views/Home/Index.cshtml?name=snippet_AspController)]

Wygenerowany kod HTML:

```html
<a href="/Speaker">All Speakers</a>
```

Jeśli `asp-controller` atrybut został określony i `asp-action` nie jest domyślnie `asp-action` wartość jest skojarzony z widokiem aktualnie wykonywane akcji kontrolera. Jeśli `asp-action` pominięto w poprzednim znaczników, i pomocnika Tag kotwicy jest używany w *HomeController*w *indeksu* widoku (*/Home*), jest wygenerowanego kodu HTML:

```html
<a href="/Home">All Speakers</a>
```

## <a name="asp-action"></a>Akcja ASP

[Akcji asp](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.action) wartość atrybutu reprezentuje nazwę akcji kontrolera, które są uwzględnione w wygenerowanym `href` atrybutu. Następujący kod ustawia wygenerowany `href` wartość atrybutu do strony ocen prelegenta:

[!code-cshtml[](samples/TagHelpersBuiltIn/Views/Home/Index.cshtml?name=snippet_AspAction)]

Wygenerowany kod HTML:

```html
<a href="/Speaker/Evaluations">Speaker Evaluations</a>
```

Jeśli nie `asp-controller` atrybut został określony, używany jest kontroler domyślne wywoływania widok wykonywania bieżącego widoku.

Jeśli `asp-action` wartość atrybutu jest `Index`, a następnie żadna akcja ze strony jest dołączany do adresu URL, co może prowadzić do wywołania domyślnych `Index` akcji. Akcja określony (lub ustawiana domyślnie), musi istnieć w kontrolerze, do którego odwołuje się `asp-controller`.

## <a name="asp-route-value"></a>asp-route-{value}

[Asp - route - {wartość value}](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.routevalues) atrybut umożliwia prefiks trasy symboli wieloznacznych. Wszystkie wartości zajmujące `{value}` symbol zastępczy jest interpretowana jako potencjalne parametru trasy. Jeśli trasa domyślna nie zostanie odnaleziony, prefiks trasy jest dołączany do wygenerowanej `href` atrybutu żądania parametr i wartość. W przeciwnym razie zostanie zastąpiony w szablonie trasy.

Należy wziąć pod uwagę następujące akcji kontrolera:

[!code-csharp[](samples/TagHelpersBuiltIn/Controllers/BuiltInTagController.cs?name=snippet_AnchorTagHelperAction)]

Z domyślnego szablonu trasy zdefiniowanej w *Startup.Configure*:

[!code-csharp[](samples/TagHelpersBuiltIn/Startup.cs?name=snippet_UseMvc&highlight=8-10)]

Widok MVC korzysta z modelu, pochodzącymi z działania, w następujący sposób:

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

Trasa domyślna `{id?}` symbol zastępczy został uzyskany. Wygenerowany kod HTML:

```html
<a href="/Speaker/Detail/12">SpeakerId: 12</a>
```

Przyjęto założenie, że prefiks trasy nie jest częścią zgodnego szablonu routingu, podobnie jak w przypadku następujących widoku MVC:

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

Poniższy kod HTML jest generowany `speakerid` nie został znaleziony w pasującej trasy:

```html
<a href="/Speaker/Detail?speakerid=12">SpeakerId: 12</a>
```

Jeśli dowolny `asp-controller` lub `asp-action` nie są określone, następnie tego samego przetwarzania domyślny jest kontynuowane, jak jest `asp-route` atrybutu.

## <a name="asp-route"></a>trasy ASP

[Trasy asp](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.route) atrybut służy do tworzenia adresu URL łączenie bezpośrednio do nazwanej trasy. Przy użyciu [atrybuty routingu](xref:mvc/controllers/routing#attribute-routing), może mieć nazwę trasy, jak pokazano w `SpeakerController` i używany w jego `Evaluations` akcji:

[!code-csharp[](samples/TagHelpersBuiltIn/Controllers/SpeakerController.cs?range=22-24)]

W następujących znaczników `asp-route` nazwanej trasy odwołuje się do atrybutu:

[!code-cshtml[](samples/TagHelpersBuiltIn/Views/Home/Index.cshtml?name=snippet_AspRoute)]

Pomocnik Tag kotwicy generuje trasę bezpośrednio do tej akcji kontrolera, używając adresu URL */prelegenta/oceny*. Wygenerowany kod HTML:

```html
<a href="/Speaker/Evaluations">Speaker Evaluations</a>
```

Jeśli `asp-controller` lub `asp-action` określono oprócz `asp-route`, generowane trasy nie może być oczekiwań. Aby uniknąć konfliktu trasy, `asp-route` nie powinny być używane z `asp-controller` i `asp-action` atrybutów.

## <a name="asp-all-route-data"></a>asp-all-route-data

[Asp-all danych trasy](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.routevalues) atrybutu obsługuje tworzenie słownik par klucz wartość. Nazwa parametru jest klucz i wartość jest wartością parametru.

W poniższym przykładzie słownik jest zainicjowany i przekazywane do widoku Razor. Alternatywnie dane można otrzymać za pomocą modelu.

[!code-cshtml[](samples/TagHelpersBuiltIn/Views/Home/Index.cshtml?name=snippet_AspAllRouteData)]

Poprzedni kod generuje poniższy kod HTML:

```html
<a href="/Speaker/EvaluationsCurrent?speakerId=11&currentYear=true">Speaker Evaluations</a>
```

`asp-all-route-data` Słownika właściwości jest spłaszczona wygenerowało ciąg kwerendy spełniających wymagania przeciążone `Evaluations` akcji:

[!code-csharp[](samples/TagHelpersBuiltIn/Controllers/SpeakerController.cs?range=26-30)]

Jeśli klucze w słowniku zgodne parametrów trasy, te wartości są zastępowane w trasie zależnie od potrzeb. Inne niepasujące wartości są generowane jako parametry żądania.

## <a name="asp-fragment"></a>asp-fragment

[Fragmentu asp](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.fragment) atrybut definiuje fragmentu adresu URL, aby dołączyć do adresu URL. Pomocnika Tag kotwicy dodaje znaku numeru (#). Rozważmy następujący kod:

[!code-cshtml[](samples/TagHelpersBuiltIn/Views/Home/Index.cshtml?name=snippet_AspFragment)]

Wygenerowany kod HTML:

```html
<a href="/Speaker/Evaluations#SpeakerEvaluations">Speaker Evaluations</a>
```

Skrót tagi są przydatne podczas kompilowania aplikacji po stronie klienta. Służy do łatwego oznaczenie i wyszukiwanie w języku JavaScript, na przykład.

## <a name="asp-area"></a>asp-area

[Obszaru asp](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.area) atrybut ustawia nazwę obszaru służy do określania odpowiednich trasy. Poniższy przykład przedstawia, jak atrybut obszaru powoduje zmianę mapowania tras. Ustawienie `asp-area` "Blogach" prefiksy katalogu *obszarów/blogi* do tras skojarzone kontrolery i widoki dla ten tag kotwicy.

* **< Nazwa projektu\>**
  * **wwwroot**
  * **Obszary**
    * **Blogi**
      * **Kontrolery**
        * *HomeController.cs*
      * **Widoki**
        * **Strona główna**
          * *AboutBlog.cshtml*
          * *Index.cshtml*
        * *_ViewStart.cshtml*
  * **Kontrolery**

Podane poprzedniego hierarchii katalogów znaczników, aby odwołać *AboutBlog.cshtml* plik jest:

[!code-cshtml[](samples/TagHelpersBuiltIn/Views/Home/Index.cshtml?name=snippet_AspArea)]

Wygenerowany kod HTML:

```html
<a href="/Blogs/Home/AboutBlog">About Blog</a>
```

> [!TIP]
> Obszarów do pracy w aplikacji MVC szablon trasy musi zawierać odwołanie do obszaru, jeśli istnieje. Ten szablon jest reprezentowana przez drugi parametr funkcji `routes.MapRoute` wywołanie metody *Startup.Configure*: [!code-csharp[](samples/TagHelpersBuiltIn/Startup.cs?name=snippet_UseMvc&highlight=5)]

## <a name="asp-protocol"></a>asp-protocol

[Protokołu asp](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.protocol) atrybut służy do określania protokół (takie jak `https`) w adresie URL. Na przykład:

[!code-cshtml[](samples/TagHelpersBuiltIn/Views/Home/Index.cshtml?name=snippet_AspProtocol)]

Wygenerowany kod HTML:

```html
<a href="https://localhost/Home/About">About</a>
```

Nazwa hosta, w tym przykładzie jest localhost, ale pomocnika Tag kotwicy używa domeny publicznej witryny sieci Web podczas generowania adresu URL.

## <a name="asp-host"></a>ASP host

[Hosta asp](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.host) atrybut służy do określania nazwy hosta w adresie URL. Na przykład:

[!code-cshtml[](samples/TagHelpersBuiltIn/Views/Home/Index.cshtml?name=snippet_AspHost)]

Wygenerowany kod HTML:

```html
<a href="https://microsoft.com/Home/About">About</a>
```

## <a name="asp-page"></a>Strona ASP

[Strona asp](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.page) atrybut jest używany w przypadku stron Razor. Użyj jej do ustawienia tag kotwicy `href` wartość atrybutu do określonej strony. Prefiksu nazwy strony się ukośnikiem ("/") tworzy adres URL.

Poniższy przykład punkty do uczestnika Razor strony:

[!code-cshtml[](samples/TagHelpersBuiltIn/Views/Home/Index.cshtml?name=snippet_AspPage)]

Wygenerowany kod HTML:

```html
<a href="/Attendee">All Attendees</a>
```

`asp-page` Jest wykluczają się wzajemnie z atrybutem `asp-route`, `asp-controller`, i `asp-action` atrybutów. Jednak `asp-page` mogą być używane z `asp-route-{value}` do kontrolowania routingu, jak pokazano następujący kod:

[!code-cshtml[](samples/TagHelpersBuiltIn/Views/Home/Index.cshtml?name=snippet_AspPageAspRouteId)]

Wygenerowany kod HTML:

```html
<a href="/Attendee?attendeeid=10">View Attendee</a>
```

## <a name="asp-page-handler"></a>asp-page-handler

[Program obsługi stron asp](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.pagehandler) atrybut jest używany w przypadku stron Razor. Przewodnik jest przeznaczony dla połączeń do określonej strony programów obsługi.

Należy wziąć pod uwagę następujące obsługi strony:

[!code-csharp[](samples/TagHelpersBuiltIn/Pages/Attendee.cshtml.cs?name=snippet_OnGetProfileHandler)]

Znaczników linki do powiązanych modelu strony `OnGetProfile` obsługi strony. Należy pamiętać, że `On<Verb>` prefiks nazwy metody obsługi strona zostanie pominięty w `asp-page-handler` wartość atrybutu. Jeżeli metodę asynchroniczną `Async` zbyt zostaną pominięte sufiks.

[!code-cshtml[](samples/TagHelpersBuiltIn/Views/Home/Index.cshtml?name=snippet_AspPageHandler)]

Wygenerowany kod HTML:

```html
<a href="/Attendee?attendeeid=12&handler=Profile">Attendee Profile</a>
```

## <a name="additional-resources"></a>Dodatkowe zasoby

* [Obszary](xref:mvc/controllers/areas)
* [Wprowadzenie do stron Razor](xref:mvc/razor-pages/index)
