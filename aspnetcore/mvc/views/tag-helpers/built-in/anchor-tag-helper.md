---
title: Pomocnik tagu kotwicy w programie ASP.NET Core
author: pkellner
description: Dowiedz się, atrybuty Pomocnik tagu kotwicy programu ASP.NET Core i rolę, jaką każdy atrybut jest odtwarzany w rozszerzanie zachowanie HTML tag kotwicy.
ms.author: scaddie
ms.custom: mvc
ms.date: 4/18/2019
uid: mvc/views/tag-helpers/builtin-th/anchor-tag-helper
ms.openlocfilehash: de45c99194d4825c1e404aa193b0f076ba659748
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/27/2019
ms.locfileid: "64898576"
---
# <a name="anchor-tag-helper-in-aspnet-core"></a><span data-ttu-id="cd25b-103">Pomocnik tagu kotwicy w programie ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="cd25b-103">Anchor Tag Helper in ASP.NET Core</span></span>

<span data-ttu-id="cd25b-104">Przez [Peter Kellner](http://peterkellner.net) i [Scott Addie](https://github.com/scottaddie)</span><span class="sxs-lookup"><span data-stu-id="cd25b-104">By [Peter Kellner](http://peterkellner.net) and [Scott Addie](https://github.com/scottaddie)</span></span>

<span data-ttu-id="cd25b-105">[Pomocnik tagu kotwicy](xref:Microsoft.AspNetCore.Mvc.TagHelpers.AnchorTagHelper) rozszerza standardowe kotwicy HTML (`<a ... ></a>`) tag przez dodanie nowych atrybutów.</span><span class="sxs-lookup"><span data-stu-id="cd25b-105">The [Anchor Tag Helper](xref:Microsoft.AspNetCore.Mvc.TagHelpers.AnchorTagHelper) enhances the standard HTML anchor (`<a ... ></a>`) tag by adding new attributes.</span></span> <span data-ttu-id="cd25b-106">Według Konwencji nazwy atrybutu mają prefiks `asp-`.</span><span class="sxs-lookup"><span data-stu-id="cd25b-106">By convention, the attribute names are prefixed with `asp-`.</span></span> <span data-ttu-id="cd25b-107">Element zakotwiczenia renderowanych `href` wartość atrybutu jest określana przez wartości `asp-` atrybutów.</span><span class="sxs-lookup"><span data-stu-id="cd25b-107">The rendered anchor element's `href` attribute value is determined by the values of the `asp-` attributes.</span></span>

<span data-ttu-id="cd25b-108">Aby zapoznać się z omówieniem pomocnicy tagów, zobacz <xref:mvc/views/tag-helpers/intro>.</span><span class="sxs-lookup"><span data-stu-id="cd25b-108">For an overview of Tag Helpers, see <xref:mvc/views/tag-helpers/intro>.</span></span>

<span data-ttu-id="cd25b-109">[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/views/tag-helpers/built-in/samples) ([sposobu pobierania](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="cd25b-109">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/views/tag-helpers/built-in/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="cd25b-110">*SpeakerController* jest używana w przykładach w tym dokumencie:</span><span class="sxs-lookup"><span data-stu-id="cd25b-110">*SpeakerController* is used in samples throughout this document:</span></span>

[!code-csharp[](samples/TagHelpersBuiltIn/Controllers/SpeakerController.cs?name=snippet_SpeakerController)]

## <a name="anchor-tag-helper-attributes"></a><span data-ttu-id="cd25b-111">Pomocnik tagu atrybuty zakotwiczenia</span><span class="sxs-lookup"><span data-stu-id="cd25b-111">Anchor Tag Helper attributes</span></span>

### <a name="asp-controller"></a><span data-ttu-id="cd25b-112">Kontroler ASP</span><span class="sxs-lookup"><span data-stu-id="cd25b-112">asp-controller</span></span>

<span data-ttu-id="cd25b-113">[Kontrolera asp](xref:Microsoft.AspNetCore.Mvc.TagHelpers.AnchorTagHelper.Controller*) atrybut przypisuje kontrolera, używany do generowania adresu URL.</span><span class="sxs-lookup"><span data-stu-id="cd25b-113">The [asp-controller](xref:Microsoft.AspNetCore.Mvc.TagHelpers.AnchorTagHelper.Controller*) attribute assigns the controller used for generating the URL.</span></span> <span data-ttu-id="cd25b-114">Następujące znaczniki Wyświetla listę wszystkich prelegentów:</span><span class="sxs-lookup"><span data-stu-id="cd25b-114">The following markup lists all speakers:</span></span>

[!code-cshtml[](samples/TagHelpersBuiltIn/Views/Home/Index.cshtml?name=snippet_AspController)]

<span data-ttu-id="cd25b-115">Wygenerowany kod HTML:</span><span class="sxs-lookup"><span data-stu-id="cd25b-115">The generated HTML:</span></span>

```html
<a href="/Speaker">All Speakers</a>
```

<span data-ttu-id="cd25b-116">Jeśli `asp-controller` określony atrybut i `asp-action` nie jest domyślnie `asp-action` wartość jest skojarzony z widokiem aktualnie wykonywanej akcji kontrolera.</span><span class="sxs-lookup"><span data-stu-id="cd25b-116">If the `asp-controller` attribute is specified and `asp-action` isn't, the default `asp-action` value is the controller action associated with the currently executing view.</span></span> <span data-ttu-id="cd25b-117">Jeśli `asp-action` zostanie pominięty w poprzednim znaczników, i Pomocnik tagu kotwicy jest używana w *HomeController*firmy *indeksu* widoku (*/Home*), to wygenerowany kod HTML:</span><span class="sxs-lookup"><span data-stu-id="cd25b-117">If `asp-action` is omitted from the preceding markup, and the Anchor Tag Helper is used in *HomeController*'s *Index* view (*/Home*), the generated HTML is:</span></span>

```html
<a href="/Home">All Speakers</a>
```

## <a name="asp-action"></a><span data-ttu-id="cd25b-118">Akcja ASP</span><span class="sxs-lookup"><span data-stu-id="cd25b-118">asp-action</span></span>

<span data-ttu-id="cd25b-119">[Akcji asp](xref:Microsoft.AspNetCore.Mvc.TagHelpers.AnchorTagHelper.Action*) wartość atrybutu reprezentuje nazwę akcji kontrolera, które są zawarte w wygenerowanym `href` atrybutu.</span><span class="sxs-lookup"><span data-stu-id="cd25b-119">The [asp-action](xref:Microsoft.AspNetCore.Mvc.TagHelpers.AnchorTagHelper.Action*) attribute value represents the controller action name included in the generated `href` attribute.</span></span> <span data-ttu-id="cd25b-120">Następujące znaczniki ustawia wygenerowany `href` wartość atrybutu do strony ocen osoby mówiącej:</span><span class="sxs-lookup"><span data-stu-id="cd25b-120">The following markup sets the generated `href` attribute value to the speaker evaluations page:</span></span>

[!code-cshtml[](samples/TagHelpersBuiltIn/Views/Home/Index.cshtml?name=snippet_AspAction)]

<span data-ttu-id="cd25b-121">Wygenerowany kod HTML:</span><span class="sxs-lookup"><span data-stu-id="cd25b-121">The generated HTML:</span></span>

```html
<a href="/Speaker/Evaluations">Speaker Evaluations</a>
```

<span data-ttu-id="cd25b-122">Jeśli nie `asp-controller` atrybut jest określony, używany jest kontroler domyślne wywoływania wyświetlanie, wykonywanie bieżącego widoku.</span><span class="sxs-lookup"><span data-stu-id="cd25b-122">If no `asp-controller` attribute is specified, the default controller calling the view executing the current view is used.</span></span>

<span data-ttu-id="cd25b-123">Jeśli `asp-action` wartość atrybutu jest `Index`, a następnie żadna akcja jest dołączany do adresu URL prowadzącego do wywołania domyślnych `Index` akcji.</span><span class="sxs-lookup"><span data-stu-id="cd25b-123">If the `asp-action` attribute value is `Index`, then no action is appended to the URL, leading to the invocation of the default `Index` action.</span></span> <span data-ttu-id="cd25b-124">Akcja określony (lub przywrócona wartość domyślna), musi istnieć w kontrolerze, do którego odwołuje się `asp-controller`.</span><span class="sxs-lookup"><span data-stu-id="cd25b-124">The action specified (or defaulted), must exist in the controller referenced in `asp-controller`.</span></span>

### <a name="asp-route-value"></a><span data-ttu-id="cd25b-125">asp-route-{value}</span><span class="sxs-lookup"><span data-stu-id="cd25b-125">asp-route-{value}</span></span>

<span data-ttu-id="cd25b-126">[Asp - route-{value}](xref:Microsoft.AspNetCore.Mvc.TagHelpers.AnchorTagHelper.RouteValues*) atrybut umożliwia prefiks trasy symboli wieloznacznych.</span><span class="sxs-lookup"><span data-stu-id="cd25b-126">The [asp-route-{value}](xref:Microsoft.AspNetCore.Mvc.TagHelpers.AnchorTagHelper.RouteValues*) attribute enables a wildcard route prefix.</span></span> <span data-ttu-id="cd25b-127">Wszystkie wartości zajmuje `{value}` symbol zastępczy jest interpretowany jako potencjalne parametru trasy.</span><span class="sxs-lookup"><span data-stu-id="cd25b-127">Any value occupying the `{value}` placeholder is interpreted as a potential route parameter.</span></span> <span data-ttu-id="cd25b-128">Jeśli trasa domyślna nie zostanie znalezione, jest dołączany prefiks trasy do wygenerowany `href` atrybutu jako parametru żądania i wartości.</span><span class="sxs-lookup"><span data-stu-id="cd25b-128">If a default route isn't found, this route prefix is appended to the generated `href` attribute as a request parameter and value.</span></span> <span data-ttu-id="cd25b-129">W przeciwnym razie zostanie zastąpiony w szablonie trasy.</span><span class="sxs-lookup"><span data-stu-id="cd25b-129">Otherwise, it's substituted in the route template.</span></span>

<span data-ttu-id="cd25b-130">Należy wziąć pod uwagę następujące akcji kontrolera:</span><span class="sxs-lookup"><span data-stu-id="cd25b-130">Consider the following controller action:</span></span>

[!code-csharp[](samples/TagHelpersBuiltIn/Controllers/BuiltInTagController.cs?name=snippet_AnchorTagHelperAction)]

<span data-ttu-id="cd25b-131">Za pomocą domyślnego szablonu trasy zdefiniowane w *Startup.Configure*:</span><span class="sxs-lookup"><span data-stu-id="cd25b-131">With a default route template defined in *Startup.Configure*:</span></span>

[!code-csharp[](samples/TagHelpersBuiltIn/Startup.cs?name=snippet_UseMvc&highlight=8-10)]

<span data-ttu-id="cd25b-132">Widok MVC korzysta z modelu, dostarczone przez akcji, w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="cd25b-132">The MVC view uses the model, provided by the action, as follows:</span></span>

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

<span data-ttu-id="cd25b-133">Trasa domyślna `{id?}` zostały dopasowane do symbolu zastępczego.</span><span class="sxs-lookup"><span data-stu-id="cd25b-133">The default route's `{id?}` placeholder was matched.</span></span> <span data-ttu-id="cd25b-134">Wygenerowany kod HTML:</span><span class="sxs-lookup"><span data-stu-id="cd25b-134">The generated HTML:</span></span>

```html
<a href="/Speaker/Detail/12">SpeakerId: 12</a>
```

<span data-ttu-id="cd25b-135">Załóżmy, że prefiks trasy nie jest częścią zgodnego szablonu routingu, podobnie jak w poniższym widoku MVC:</span><span class="sxs-lookup"><span data-stu-id="cd25b-135">Assume the route prefix isn't part of the matching routing template, as with the following MVC view:</span></span>

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

<span data-ttu-id="cd25b-136">Poniższy kod HTML jest generowany, ponieważ `speakerid` nie został znaleziony w pasującej trasy:</span><span class="sxs-lookup"><span data-stu-id="cd25b-136">The following HTML is generated because `speakerid` wasn't found in the matching route:</span></span>

```html
<a href="/Speaker/Detail?speakerid=12">SpeakerId: 12</a>
```

<span data-ttu-id="cd25b-137">Jeśli `asp-controller` lub `asp-action` nie są określone, a następnie tego samego procesu przetwarzania domyślne występuje, ponieważ jest `asp-route` atrybutu.</span><span class="sxs-lookup"><span data-stu-id="cd25b-137">If either `asp-controller` or `asp-action` aren't specified, then the same default processing is followed as is in the `asp-route` attribute.</span></span>

### <a name="asp-route"></a><span data-ttu-id="cd25b-138">trasy ASP</span><span class="sxs-lookup"><span data-stu-id="cd25b-138">asp-route</span></span>

<span data-ttu-id="cd25b-139">[Trasy asp](xref:Microsoft.AspNetCore.Mvc.TagHelpers.AnchorTagHelper.Route*) atrybut jest używany do tworzenia adresu URL połączenie bezpośrednio z trasą mającą nazwę.</span><span class="sxs-lookup"><span data-stu-id="cd25b-139">The [asp-route](xref:Microsoft.AspNetCore.Mvc.TagHelpers.AnchorTagHelper.Route*) attribute is used for creating a URL linking directly to a named route.</span></span> <span data-ttu-id="cd25b-140">Przy użyciu [atrybuty routingu](xref:mvc/controllers/routing#attribute-routing), może mieć nazwę trasy, jak pokazano na `SpeakerController` i wykorzystywane w jej `Evaluations` akcji:</span><span class="sxs-lookup"><span data-stu-id="cd25b-140">Using [routing attributes](xref:mvc/controllers/routing#attribute-routing), a route can be named as shown in the `SpeakerController` and used in its `Evaluations` action:</span></span>

[!code-csharp[](samples/TagHelpersBuiltIn/Controllers/SpeakerController.cs?range=22-24)]

<span data-ttu-id="cd25b-141">W niej następujące znaczniki `asp-route` atrybut odwołuje się do nazwanej trasy:</span><span class="sxs-lookup"><span data-stu-id="cd25b-141">In the following markup, the `asp-route` attribute references the named route:</span></span>

[!code-cshtml[](samples/TagHelpersBuiltIn/Views/Home/Index.cshtml?name=snippet_AspRoute)]

<span data-ttu-id="cd25b-142">Pomocnik tagu kotwicy generuje trasę bezpośrednio do tej akcji kontrolera, używając adresu URL */osoby mówiącej/ocen*.</span><span class="sxs-lookup"><span data-stu-id="cd25b-142">The Anchor Tag Helper generates a route directly to that controller action using the URL */Speaker/Evaluations*.</span></span> <span data-ttu-id="cd25b-143">Wygenerowany kod HTML:</span><span class="sxs-lookup"><span data-stu-id="cd25b-143">The generated HTML:</span></span>

```html
<a href="/Speaker/Evaluations">Speaker Evaluations</a>
```

<span data-ttu-id="cd25b-144">Jeśli `asp-controller` lub `asp-action` określono oprócz `asp-route`, trasy, generowany jest oczekiwanych.</span><span class="sxs-lookup"><span data-stu-id="cd25b-144">If `asp-controller` or `asp-action` is specified in addition to `asp-route`, the route generated may not be what you expect.</span></span> <span data-ttu-id="cd25b-145">Aby uniknąć konfliktu trasy `asp-route` nie powinny być używane z `asp-controller` i `asp-action` atrybutów.</span><span class="sxs-lookup"><span data-stu-id="cd25b-145">To avoid a route conflict, `asp-route` shouldn't be used with the `asp-controller` and `asp-action` attributes.</span></span>

## <a name="asp-all-route-data"></a><span data-ttu-id="cd25b-146">asp-all-route-data</span><span class="sxs-lookup"><span data-stu-id="cd25b-146">asp-all-route-data</span></span>

<span data-ttu-id="cd25b-147">[Asp-all danych trasy](xref:Microsoft.AspNetCore.Mvc.TagHelpers.AnchorTagHelper.RouteValues*) atrybutu obsługuje tworzenie słownik par klucz wartość.</span><span class="sxs-lookup"><span data-stu-id="cd25b-147">The [asp-all-route-data](xref:Microsoft.AspNetCore.Mvc.TagHelpers.AnchorTagHelper.RouteValues*) attribute supports the creation of a dictionary of key-value pairs.</span></span> <span data-ttu-id="cd25b-148">Klucz jest nazwa parametru, a wartość jest wartością parametru.</span><span class="sxs-lookup"><span data-stu-id="cd25b-148">The key is the parameter name, and the value is the parameter value.</span></span>

<span data-ttu-id="cd25b-149">W poniższym przykładzie słownik zostanie zainicjowana i przekazywane do widoku Razor.</span><span class="sxs-lookup"><span data-stu-id="cd25b-149">In the following example, a dictionary is initialized and passed to a Razor view.</span></span> <span data-ttu-id="cd25b-150">Alternatywnie można przekazać dane przy użyciu modelu.</span><span class="sxs-lookup"><span data-stu-id="cd25b-150">Alternatively, the data could be passed in with your model.</span></span>

[!code-cshtml[](samples/TagHelpersBuiltIn/Views/Home/Index.cshtml?name=snippet_AspAllRouteData)]

<span data-ttu-id="cd25b-151">Powyższy kod generuje poniższy kod HTML:</span><span class="sxs-lookup"><span data-stu-id="cd25b-151">The preceding code generates the following HTML:</span></span>

```html
<a href="/Speaker/EvaluationsCurrent?speakerId=11&currentYear=true">Speaker Evaluations</a>
```

<span data-ttu-id="cd25b-152">`asp-all-route-data` Słownika jest spłaszczany, aby utworzyć ciąg zapytania spełnienie wymagań przeciążonej `Evaluations` akcji:</span><span class="sxs-lookup"><span data-stu-id="cd25b-152">The `asp-all-route-data` dictionary is flattened to produce a querystring meeting the requirements of the overloaded `Evaluations` action:</span></span>

[!code-csharp[](samples/TagHelpersBuiltIn/Controllers/SpeakerController.cs?range=26-30)]

<span data-ttu-id="cd25b-153">Jeśli jakiekolwiek klucze w słowniku zgodne parametrów trasy, te wartości są zastępowane dla trasy, zgodnie z potrzebami.</span><span class="sxs-lookup"><span data-stu-id="cd25b-153">If any keys in the dictionary match route parameters, those values are substituted in the route as appropriate.</span></span> <span data-ttu-id="cd25b-154">Niezgodny wartości są generowane jako parametry żądania.</span><span class="sxs-lookup"><span data-stu-id="cd25b-154">The other non-matching values are generated as request parameters.</span></span>

### <a name="asp-fragment"></a><span data-ttu-id="cd25b-155">asp-fragment</span><span class="sxs-lookup"><span data-stu-id="cd25b-155">asp-fragment</span></span>

<span data-ttu-id="cd25b-156">[Fragmentu asp](xref:Microsoft.AspNetCore.Mvc.TagHelpers.AnchorTagHelper.Fragment*) atrybut definiuje fragmentu adresu URL do dołączenia do adresu URL.</span><span class="sxs-lookup"><span data-stu-id="cd25b-156">The [asp-fragment](xref:Microsoft.AspNetCore.Mvc.TagHelpers.AnchorTagHelper.Fragment*) attribute defines a URL fragment to append to the URL.</span></span> <span data-ttu-id="cd25b-157">Pomocnik tagu kotwicy dodaje znaku numeru (#).</span><span class="sxs-lookup"><span data-stu-id="cd25b-157">The Anchor Tag Helper adds the hash character (#).</span></span> <span data-ttu-id="cd25b-158">Należy wziąć pod uwagę następujące znaczniki:</span><span class="sxs-lookup"><span data-stu-id="cd25b-158">Consider the following markup:</span></span>

[!code-cshtml[](samples/TagHelpersBuiltIn/Views/Home/Index.cshtml?name=snippet_AspFragment)]

<span data-ttu-id="cd25b-159">Wygenerowany kod HTML:</span><span class="sxs-lookup"><span data-stu-id="cd25b-159">The generated HTML:</span></span>

```html
<a href="/Speaker/Evaluations#SpeakerEvaluations">Speaker Evaluations</a>
```

<span data-ttu-id="cd25b-160">Tagi wyznaczania wartości skrótu są przydatne podczas kompilowania aplikacji po stronie klienta.</span><span class="sxs-lookup"><span data-stu-id="cd25b-160">Hash tags are useful when building client-side apps.</span></span> <span data-ttu-id="cd25b-161">Umożliwia proste oznaczenie i wyszukiwania w języku JavaScript, na przykład.</span><span class="sxs-lookup"><span data-stu-id="cd25b-161">They can be used for easy marking and searching in JavaScript, for example.</span></span>

### <a name="asp-area"></a><span data-ttu-id="cd25b-162">obszar ASP</span><span class="sxs-lookup"><span data-stu-id="cd25b-162">asp-area</span></span>

<span data-ttu-id="cd25b-163">[Obszaru asp](xref:Microsoft.AspNetCore.Mvc.TagHelpers.AnchorTagHelper.Area*) atrybut ustawia nazwę obszaru używane do ustawiania odpowiednie trasy.</span><span class="sxs-lookup"><span data-stu-id="cd25b-163">The [asp-area](xref:Microsoft.AspNetCore.Mvc.TagHelpers.AnchorTagHelper.Area*) attribute sets the area name used to set the appropriate route.</span></span> <span data-ttu-id="cd25b-164">Poniższe przykłady przedstawić sposób, w jaki `asp-area` atrybutu powoduje, że ponowne mapowanie trasy.</span><span class="sxs-lookup"><span data-stu-id="cd25b-164">The following examples depict how the `asp-area` attribute causes a remapping of routes.</span></span>

### <a name="usage-in-razor-pages"></a><span data-ttu-id="cd25b-165">Użycie stron Razor</span><span class="sxs-lookup"><span data-stu-id="cd25b-165">Usage in Razor Pages</span></span>

<span data-ttu-id="cd25b-166">Obszary stron razor są obsługiwane w programie ASP.NET Core 2.1 lub nowszej.</span><span class="sxs-lookup"><span data-stu-id="cd25b-166">Razor Pages areas are supported in ASP.NET Core 2.1 or later.</span></span>

<span data-ttu-id="cd25b-167">Należy wziąć pod uwagę następujące hierarchii katalogów:</span><span class="sxs-lookup"><span data-stu-id="cd25b-167">Consider the following directory hierarchy:</span></span>

* <span data-ttu-id="cd25b-168">**{Project name}**</span><span class="sxs-lookup"><span data-stu-id="cd25b-168">**{Project name}**</span></span>
  * <span data-ttu-id="cd25b-169">**wwwroot**</span><span class="sxs-lookup"><span data-stu-id="cd25b-169">**wwwroot**</span></span>
  * <span data-ttu-id="cd25b-170">**Obszary**</span><span class="sxs-lookup"><span data-stu-id="cd25b-170">**Areas**</span></span>
    * <span data-ttu-id="cd25b-171">**Sesje**</span><span class="sxs-lookup"><span data-stu-id="cd25b-171">**Sessions**</span></span>
      * <span data-ttu-id="cd25b-172">**Strony**</span><span class="sxs-lookup"><span data-stu-id="cd25b-172">**Pages**</span></span>
        * <span data-ttu-id="cd25b-173">*\_ViewStart.cshtml*</span><span class="sxs-lookup"><span data-stu-id="cd25b-173">*\_ViewStart.cshtml*</span></span>
        * <span data-ttu-id="cd25b-174">*Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="cd25b-174">*Index.cshtml*</span></span>
        * <span data-ttu-id="cd25b-175">*Index.cshtml.CS*</span><span class="sxs-lookup"><span data-stu-id="cd25b-175">*Index.cshtml.cs*</span></span>
  * <span data-ttu-id="cd25b-176">**Strony**</span><span class="sxs-lookup"><span data-stu-id="cd25b-176">**Pages**</span></span>

<span data-ttu-id="cd25b-177">Kod znaczników, aby odwołać się do *sesje* obszaru *indeksu* jest strona Razor:</span><span class="sxs-lookup"><span data-stu-id="cd25b-177">The markup to reference the *Sessions* area *Index* Razor Page is:</span></span>

[!code-cshtml[](samples/TagHelpersBuiltIn/Views/Home/Index.cshtml?name=snippet_AspAreaRazorPages)]

<span data-ttu-id="cd25b-178">Wygenerowany kod HTML:</span><span class="sxs-lookup"><span data-stu-id="cd25b-178">The generated HTML:</span></span>

```html
<a href="/Sessions">View Sessions</a>
```

> [!TIP]
> <span data-ttu-id="cd25b-179">Aby zapewnić obsługę obszary w aplikacji stron Razor, wykonaj jedną z następujących czynności w `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="cd25b-179">To support areas in a Razor Pages app, do one of the following in `Startup.ConfigureServices`:</span></span>
>
> * <span data-ttu-id="cd25b-180">Ustaw [zgodność wersji](xref:mvc/compatibility-version) do 2.1 lub nowszej.</span><span class="sxs-lookup"><span data-stu-id="cd25b-180">Set the [compatibility version](xref:mvc/compatibility-version) to 2.1 or later.</span></span>
> * <span data-ttu-id="cd25b-181">Ustaw [RazorPagesOptions.AllowAreas](xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.AllowAreas*) właściwości `true`:</span><span class="sxs-lookup"><span data-stu-id="cd25b-181">Set the [RazorPagesOptions.AllowAreas](xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.AllowAreas*) property to `true`:</span></span>
>
>   [!code-csharp[](samples/TagHelpersBuiltIn/Startup.cs?name=snippet_AllowAreas)]

### <a name="usage-in-mvc"></a><span data-ttu-id="cd25b-182">Użycie w MVC</span><span class="sxs-lookup"><span data-stu-id="cd25b-182">Usage in MVC</span></span>

<span data-ttu-id="cd25b-183">Należy wziąć pod uwagę następujące hierarchii katalogów:</span><span class="sxs-lookup"><span data-stu-id="cd25b-183">Consider the following directory hierarchy:</span></span>

* <span data-ttu-id="cd25b-184">**{Project name}**</span><span class="sxs-lookup"><span data-stu-id="cd25b-184">**{Project name}**</span></span>
  * <span data-ttu-id="cd25b-185">**wwwroot**</span><span class="sxs-lookup"><span data-stu-id="cd25b-185">**wwwroot**</span></span>
  * <span data-ttu-id="cd25b-186">**Obszary**</span><span class="sxs-lookup"><span data-stu-id="cd25b-186">**Areas**</span></span>
    * <span data-ttu-id="cd25b-187">**Blogi**</span><span class="sxs-lookup"><span data-stu-id="cd25b-187">**Blogs**</span></span>
      * <span data-ttu-id="cd25b-188">**Kontrolery**</span><span class="sxs-lookup"><span data-stu-id="cd25b-188">**Controllers**</span></span>
        * <span data-ttu-id="cd25b-189">*HomeController.cs*</span><span class="sxs-lookup"><span data-stu-id="cd25b-189">*HomeController.cs*</span></span>
      * <span data-ttu-id="cd25b-190">**Widoki**</span><span class="sxs-lookup"><span data-stu-id="cd25b-190">**Views**</span></span>
        * <span data-ttu-id="cd25b-191">**Strona główna**</span><span class="sxs-lookup"><span data-stu-id="cd25b-191">**Home**</span></span>
          * <span data-ttu-id="cd25b-192">*AboutBlog.cshtml*</span><span class="sxs-lookup"><span data-stu-id="cd25b-192">*AboutBlog.cshtml*</span></span>
          * <span data-ttu-id="cd25b-193">*Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="cd25b-193">*Index.cshtml*</span></span>
        * <span data-ttu-id="cd25b-194">*\_ViewStart.cshtml*</span><span class="sxs-lookup"><span data-stu-id="cd25b-194">*\_ViewStart.cshtml*</span></span>
  * <span data-ttu-id="cd25b-195">**Kontrolery**</span><span class="sxs-lookup"><span data-stu-id="cd25b-195">**Controllers**</span></span>

<span data-ttu-id="cd25b-196">Ustawienie `asp-area` "Blogi" prefiksów katalogu *obszarów/blogi* do tras skojarzone kontrolerów i widoków ten tag kotwicy.</span><span class="sxs-lookup"><span data-stu-id="cd25b-196">Setting `asp-area` to "Blogs" prefixes the directory *Areas/Blogs* to the routes of the associated controllers and views for this anchor tag.</span></span> <span data-ttu-id="cd25b-197">Kod znaczników, aby odwołać się do *AboutBlog* widok jest:</span><span class="sxs-lookup"><span data-stu-id="cd25b-197">The markup to reference the *AboutBlog* view is:</span></span>

[!code-cshtml[](samples/TagHelpersBuiltIn/Views/Home/Index.cshtml?name=snippet_AspArea)]

<span data-ttu-id="cd25b-198">Wygenerowany kod HTML:</span><span class="sxs-lookup"><span data-stu-id="cd25b-198">The generated HTML:</span></span>

```html
<a href="/Blogs/Home/AboutBlog">About Blog</a>
```

> [!TIP]
> <span data-ttu-id="cd25b-199">Aby zapewnić obsługę obszary w aplikacji MVC, szablon trasy musi zawierać odwołanie do obszaru, jeśli taki istnieje.</span><span class="sxs-lookup"><span data-stu-id="cd25b-199">To support areas in an MVC app, the route template must include a reference to the area, if it exists.</span></span> <span data-ttu-id="cd25b-200">Ten szablon jest reprezentowany przez drugi parametr `routes.MapRoute` metody wywołania w *Startup.Configure*:</span><span class="sxs-lookup"><span data-stu-id="cd25b-200">That template is represented by the second parameter of the `routes.MapRoute` method call in *Startup.Configure*:</span></span>
>
> [!code-csharp[](samples/TagHelpersBuiltIn/Startup.cs?name=snippet_UseMvc&highlight=5)]

### <a name="asp-protocol"></a><span data-ttu-id="cd25b-201">Protokół ASP</span><span class="sxs-lookup"><span data-stu-id="cd25b-201">asp-protocol</span></span>

<span data-ttu-id="cd25b-202">[Protokołu asp](xref:Microsoft.AspNetCore.Mvc.TagHelpers.AnchorTagHelper.Protocol*) atrybut jest określająca protokół (takie jak `https`) w adresie URL.</span><span class="sxs-lookup"><span data-stu-id="cd25b-202">The [asp-protocol](xref:Microsoft.AspNetCore.Mvc.TagHelpers.AnchorTagHelper.Protocol*) attribute is for specifying a protocol (such as `https`) in your URL.</span></span> <span data-ttu-id="cd25b-203">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="cd25b-203">For example:</span></span>

[!code-cshtml[](samples/TagHelpersBuiltIn/Views/Home/Index.cshtml?name=snippet_AspProtocol)]

<span data-ttu-id="cd25b-204">Wygenerowany kod HTML:</span><span class="sxs-lookup"><span data-stu-id="cd25b-204">The generated HTML:</span></span>

```html
<a href="https://localhost/Home/About">About</a>
```

<span data-ttu-id="cd25b-205">W przykładzie nazwa hosta jest localhost.</span><span class="sxs-lookup"><span data-stu-id="cd25b-205">The host name in the example is localhost.</span></span> <span data-ttu-id="cd25b-206">Pomocnik tagu kotwicy używa domeny publicznej witryny sieci Web podczas generowania adresu URL.</span><span class="sxs-lookup"><span data-stu-id="cd25b-206">The Anchor Tag Helper uses the website's public domain when generating the URL.</span></span>

### <a name="asp-host"></a><span data-ttu-id="cd25b-207">asp-host</span><span class="sxs-lookup"><span data-stu-id="cd25b-207">asp-host</span></span>

<span data-ttu-id="cd25b-208">[Hosta asp](xref:Microsoft.AspNetCore.Mvc.TagHelpers.AnchorTagHelper.Host*) atrybut służy do określania nazwy hosta w adresie URL.</span><span class="sxs-lookup"><span data-stu-id="cd25b-208">The [asp-host](xref:Microsoft.AspNetCore.Mvc.TagHelpers.AnchorTagHelper.Host*) attribute is for specifying a host name in your URL.</span></span> <span data-ttu-id="cd25b-209">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="cd25b-209">For example:</span></span>

[!code-cshtml[](samples/TagHelpersBuiltIn/Views/Home/Index.cshtml?name=snippet_AspHost)]

<span data-ttu-id="cd25b-210">Wygenerowany kod HTML:</span><span class="sxs-lookup"><span data-stu-id="cd25b-210">The generated HTML:</span></span>

```html
<a href="https://microsoft.com/Home/About">About</a>
```

### <a name="asp-page"></a><span data-ttu-id="cd25b-211">Strona ASP</span><span class="sxs-lookup"><span data-stu-id="cd25b-211">asp-page</span></span>

<span data-ttu-id="cd25b-212">[Strona asp](xref:Microsoft.AspNetCore.Mvc.TagHelpers.AnchorTagHelper.Page*) atrybut jest używany ze stronami Razor.</span><span class="sxs-lookup"><span data-stu-id="cd25b-212">The [asp-page](xref:Microsoft.AspNetCore.Mvc.TagHelpers.AnchorTagHelper.Page*) attribute is used with Razor Pages.</span></span> <span data-ttu-id="cd25b-213">Użyj go, aby ustawić tag kotwicy `href` wartość atrybutu do określonej strony.</span><span class="sxs-lookup"><span data-stu-id="cd25b-213">Use it to set an anchor tag's `href` attribute value to a specific page.</span></span> <span data-ttu-id="cd25b-214">Tworzenie prefiksu nazwy strony, od ukośnika ("/") tworzy adres URL.</span><span class="sxs-lookup"><span data-stu-id="cd25b-214">Prefixing the page name with a forward slash ("/") creates the URL.</span></span>

<span data-ttu-id="cd25b-215">Poniższy przykład wskazuje uczestnik strony Razor:</span><span class="sxs-lookup"><span data-stu-id="cd25b-215">The following sample points to the attendee Razor Page:</span></span>

[!code-cshtml[](samples/TagHelpersBuiltIn/Views/Home/Index.cshtml?name=snippet_AspPage)]

<span data-ttu-id="cd25b-216">Wygenerowany kod HTML:</span><span class="sxs-lookup"><span data-stu-id="cd25b-216">The generated HTML:</span></span>

```html
<a href="/Attendee">All Attendees</a>
```

<span data-ttu-id="cd25b-217">`asp-page` Atrybutu jest wzajemnie wykluczających się przy użyciu `asp-route`, `asp-controller`, i `asp-action` atrybutów.</span><span class="sxs-lookup"><span data-stu-id="cd25b-217">The `asp-page` attribute is mutually exclusive with the `asp-route`, `asp-controller`, and `asp-action` attributes.</span></span> <span data-ttu-id="cd25b-218">Jednak `asp-page` mogą być używane z `asp-route-{value}` do sterowania routingiem, tak jak pokazano w następującym:</span><span class="sxs-lookup"><span data-stu-id="cd25b-218">However, `asp-page` can be used with `asp-route-{value}` to control routing, as the following markup demonstrates:</span></span>

[!code-cshtml[](samples/TagHelpersBuiltIn/Views/Home/Index.cshtml?name=snippet_AspPageAspRouteId)]

<span data-ttu-id="cd25b-219">Wygenerowany kod HTML:</span><span class="sxs-lookup"><span data-stu-id="cd25b-219">The generated HTML:</span></span>

```html
<a href="/Attendee?attendeeid=10">View Attendee</a>
```

### <a name="asp-page-handler"></a><span data-ttu-id="cd25b-220">asp-page-handler</span><span class="sxs-lookup"><span data-stu-id="cd25b-220">asp-page-handler</span></span>

<span data-ttu-id="cd25b-221">[Program obsługi stron asp](xref:Microsoft.AspNetCore.Mvc.TagHelpers.AnchorTagHelper.PageHandler*) atrybut jest używany ze stronami Razor.</span><span class="sxs-lookup"><span data-stu-id="cd25b-221">The [asp-page-handler](xref:Microsoft.AspNetCore.Mvc.TagHelpers.AnchorTagHelper.PageHandler*) attribute is used with Razor Pages.</span></span> <span data-ttu-id="cd25b-222">Jest ona przeznaczona do konsolidacji do obsługi określonej strony.</span><span class="sxs-lookup"><span data-stu-id="cd25b-222">It's intended for linking to specific page handlers.</span></span>

<span data-ttu-id="cd25b-223">Należy wziąć pod uwagę następujące procedury obsługi strony:</span><span class="sxs-lookup"><span data-stu-id="cd25b-223">Consider the following page handler:</span></span>

[!code-csharp[](samples/TagHelpersBuiltIn/Pages/Attendee.cshtml.cs?name=snippet_OnGetProfileHandler)]

<span data-ttu-id="cd25b-224">Skojarzonej z modelem strony łącza znaczników do `OnGetProfile` obsługi strony.</span><span class="sxs-lookup"><span data-stu-id="cd25b-224">The page model's associated markup links to the `OnGetProfile` page handler.</span></span> <span data-ttu-id="cd25b-225">Uwaga `On<Verb>` prefiks nazwy metody obsługi strony zostanie pominięty w `asp-page-handler` wartość atrybutu.</span><span class="sxs-lookup"><span data-stu-id="cd25b-225">Note the `On<Verb>` prefix of the page handler method name is omitted in the `asp-page-handler` attribute value.</span></span> <span data-ttu-id="cd25b-226">Jeśli metoda jest asynchroniczna, `Async` sufiks zostanie pominięty, zbyt.</span><span class="sxs-lookup"><span data-stu-id="cd25b-226">When the method is asynchronous, the `Async` suffix is omitted, too.</span></span>

[!code-cshtml[](samples/TagHelpersBuiltIn/Views/Home/Index.cshtml?name=snippet_AspPageHandler)]

<span data-ttu-id="cd25b-227">Wygenerowany kod HTML:</span><span class="sxs-lookup"><span data-stu-id="cd25b-227">The generated HTML:</span></span>

```html
<a href="/Attendee?attendeeid=12&handler=Profile">Attendee Profile</a>
```

## <a name="additional-resources"></a><span data-ttu-id="cd25b-228">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="cd25b-228">Additional resources</span></span>

* <xref:mvc/controllers/areas>
* <xref:razor-pages/index>
* <xref:mvc/compatibility-version>
