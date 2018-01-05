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
ms.openlocfilehash: b2bdf8b2b297a66b08445d99afbc5f43d2e37ef6
ms.sourcegitcommit: 281f0c614543a6c3db565ea4655b70fe49b61d84
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/03/2018
---
# <a name="anchor-tag-helper"></a><span data-ttu-id="9985b-104">Pomocnik Tag kotwicy</span><span class="sxs-lookup"><span data-stu-id="9985b-104">Anchor Tag Helper</span></span>

<span data-ttu-id="9985b-105">Przez [Kellner Peterowi](http://peterkellner.net)</span><span class="sxs-lookup"><span data-stu-id="9985b-105">By [Peter Kellner](http://peterkellner.net)</span></span> 

<span data-ttu-id="9985b-106">Pomocnik Tag kotwicy zwiększa kotwicy HTML (`<a ... ></a>`) tag przez dodanie nowych atrybutów.</span><span class="sxs-lookup"><span data-stu-id="9985b-106">The Anchor Tag Helper enhances the HTML anchor (`<a ... ></a>`) tag by adding new attributes.</span></span> <span data-ttu-id="9985b-107">Link wygenerowany (na `href` tagu) jest tworzony przy użyciu nowych atrybutów.</span><span class="sxs-lookup"><span data-stu-id="9985b-107">The link generated (on the `href` tag) is created using the new attributes.</span></span> <span data-ttu-id="9985b-108">Ten adres URL może zawierać protokół opcjonalne, na przykład protokołu https.</span><span class="sxs-lookup"><span data-stu-id="9985b-108">That URL can include an optional protocol such as https.</span></span>

<span data-ttu-id="9985b-109">Poniżej kontrolera prelegenta jest używany w przykłady w tym dokumencie.</span><span class="sxs-lookup"><span data-stu-id="9985b-109">The speaker controller below is used in samples in this document.</span></span>

<span data-ttu-id="9985b-110">**SpeakerController.cs**</span><span class="sxs-lookup"><span data-stu-id="9985b-110">**SpeakerController.cs**</span></span> 

[!code-csharp[SpeakerController](sample/TagHelpersBuiltInAspNetCore/src/TagHelpersBuiltInAspNetCore/Controllers/SpeakerController.cs)]


## <a name="anchor-tag-helper-attributes"></a><span data-ttu-id="9985b-111">Atrybuty pomocnika Tag kotwicy</span><span class="sxs-lookup"><span data-stu-id="9985b-111">Anchor Tag Helper Attributes</span></span>

### <a name="asp-controller"></a><span data-ttu-id="9985b-112">Kontroler ASP</span><span class="sxs-lookup"><span data-stu-id="9985b-112">asp-controller</span></span>

<span data-ttu-id="9985b-113">`asp-controller`Służy do skojarzenia kontrolera, który będzie służyć do generowania adresu URL.</span><span class="sxs-lookup"><span data-stu-id="9985b-113">`asp-controller` is used to associate which controller will be used to generate the URL.</span></span> <span data-ttu-id="9985b-114">Kontrolerów określonych musi istnieć w bieżącym projekcie.</span><span class="sxs-lookup"><span data-stu-id="9985b-114">The controllers specified must exist in the current project.</span></span> <span data-ttu-id="9985b-115">Poniższy kod wyświetla listę wszystkich głośników:</span><span class="sxs-lookup"><span data-stu-id="9985b-115">The following code lists all speakers:</span></span> 

```cshtml
<a asp-controller="Speaker" asp-action="Index">All Speakers</a>
```

<span data-ttu-id="9985b-116">Zostanie wygenerowany kod znaczników:</span><span class="sxs-lookup"><span data-stu-id="9985b-116">The generated markup will be:</span></span>

```html
<a href="/Speaker">All Speakers</a>
```

<span data-ttu-id="9985b-117">Jeśli `asp-controller` określono i `asp-action` nie jest domyślnie `asp-action` jest domyślną metodą kontrolera widoku aktualnie wykonywane.</span><span class="sxs-lookup"><span data-stu-id="9985b-117">If the `asp-controller` is specified and `asp-action` is not, the default `asp-action` will be the default controller method of the currently executing view.</span></span> <span data-ttu-id="9985b-118">Czy jest w powyższym przykładzie, jeśli `asp-action` pozostało, i jest generowany tego pomocnika Tag kotwicy na podstawie *HomeController*w `Index` widoku (**/Home**), zostanie wygenerowany kod znaczników:</span><span class="sxs-lookup"><span data-stu-id="9985b-118">That is, in the above example, if `asp-action` is left out, and this Anchor Tag Helper is generated from *HomeController*'s `Index` view (**/Home**), the generated markup will be:</span></span>

```html
<a href="/Home">All Speakers</a>
```

### <a name="asp-action"></a><span data-ttu-id="9985b-119">Akcja ASP</span><span class="sxs-lookup"><span data-stu-id="9985b-119">asp-action</span></span>

<span data-ttu-id="9985b-120">`asp-action`Nazwa metody akcji w kontrolerze, który zostanie uwzględniony w wygenerowanym `href`.</span><span class="sxs-lookup"><span data-stu-id="9985b-120">`asp-action` is the name of the action method in the controller that will be included in the generated `href`.</span></span> <span data-ttu-id="9985b-121">Na przykład następujący kod ustawić wygenerowany `href` wskaż prelegenta strony szczegółów:</span><span class="sxs-lookup"><span data-stu-id="9985b-121">For example, the following code set the generated `href` to point to the speaker detail page:</span></span>

```html
<a asp-controller="Speaker" asp-action="Detail">Speaker Detail</a>
```

<span data-ttu-id="9985b-122">Zostanie wygenerowany kod znaczników:</span><span class="sxs-lookup"><span data-stu-id="9985b-122">The generated markup will be:</span></span>

```html
<a href="/Speaker/Detail">Speaker Detail</a>
```

<span data-ttu-id="9985b-123">Jeśli nie `asp-controller` atrybut został określony, zostanie użyty domyślny kontroler wywoływania widok wykonywania bieżącego widoku.</span><span class="sxs-lookup"><span data-stu-id="9985b-123">If no `asp-controller` attribute is specified, the default controller calling the view executing the current view will be used.</span></span>  
 
<span data-ttu-id="9985b-124">Jeśli atrybut `asp-action` jest `Index`, a następnie żadna akcja ze strony jest dołączany do adresu URL, co prowadzi do domyślnej `Index` wywołania metody.</span><span class="sxs-lookup"><span data-stu-id="9985b-124">If the attribute `asp-action` is `Index`, then no action is appended to the URL, leading to the default `Index` method being called.</span></span> <span data-ttu-id="9985b-125">Akcja określony (lub ustawiana domyślnie), musi istnieć w kontrolerze, do którego odwołuje się `asp-controller`.</span><span class="sxs-lookup"><span data-stu-id="9985b-125">The action specified (or defaulted), must exist in the controller referenced in `asp-controller`.</span></span>

### <a name="asp-page"></a><span data-ttu-id="9985b-126">Strona ASP</span><span class="sxs-lookup"><span data-stu-id="9985b-126">asp-page</span></span>

<span data-ttu-id="9985b-127">Użyj `asp-page` atrybut w tagu zakotwiczenia, aby ustawić adres URL, aby wskazywał określonej strony.</span><span class="sxs-lookup"><span data-stu-id="9985b-127">Use the `asp-page` attribute in an anchor tag to set its URL to point to a specific page.</span></span> <span data-ttu-id="9985b-128">Prefiksu nazwy strony, od ukośnika "/" tworzy adres URL.</span><span class="sxs-lookup"><span data-stu-id="9985b-128">Prefixing the page name with a forward slash "/" creates the URL.</span></span> <span data-ttu-id="9985b-129">W poniższym przykładzie adres URL wskazuje na stronie "Prelegenta" w bieżącym katalogu.</span><span class="sxs-lookup"><span data-stu-id="9985b-129">The URL in the sample below points to the "Speaker" page in the current directory.</span></span>

```cshtml
<a asp-page="/Speakers">All Speakers</a>
```

<span data-ttu-id="9985b-130">`asp-page` Atrybutu w poprzednim przykładzie kodu renderuje danych wyjściowych HTML w widoku, który jest podobny do następującego fragmentu kodu:</span><span class="sxs-lookup"><span data-stu-id="9985b-130">The `asp-page` attribute in the previous code sample renders HTML output in the view that is similar to the following snippet:</span></span>

```html
<a href="/items?page=%2FSpeakers">Speakers</a>
``

The `asp-page` attribute is mutually exclusive with the `asp-route`, `asp-controller`, and `asp-action` attributes. However, `asp-page` can be used with `asp-route-id` to control routing, as the following code sample demonstrates:

```cshtml
<a asp-page="/Speaker" asp-route-id="@speaker.Id">View Speaker</a>
```

<span data-ttu-id="9985b-131">`asp-route-id` Następujących danych wyjściowych:</span><span class="sxs-lookup"><span data-stu-id="9985b-131">The `asp-route-id` produces the following output:</span></span>

```html
https://localhost:44399/Speakers/Index/2?page=%2FSpeaker
```

> [!NOTE]
> <span data-ttu-id="9985b-132">Aby użyć `asp-page` atrybutu w stron Razor, adresy URL musi być ścieżką względną, na przykład `"./Speaker"`.</span><span class="sxs-lookup"><span data-stu-id="9985b-132">To use the `asp-page` attribute in Razor Pages, the URLs must be a relative path, for example `"./Speaker"`.</span></span> <span data-ttu-id="9985b-133">Ścieżki względne w `asp-page` atrybutów nie są dostępne w widokach MVC.</span><span class="sxs-lookup"><span data-stu-id="9985b-133">Relative paths in the `asp-page` attribute are not available in MVC views.</span></span> <span data-ttu-id="9985b-134">Zamiast tego użyj składni "/" dla widoków MVC.</span><span class="sxs-lookup"><span data-stu-id="9985b-134">Use the "/" syntax for MVC views instead.</span></span>

### <a name="asp-route-value"></a><span data-ttu-id="9985b-135">ASP - route - {wartość value}</span><span class="sxs-lookup"><span data-stu-id="9985b-135">asp-route-{value}</span></span>

<span data-ttu-id="9985b-136">`asp-route-`jest symbol wieloznaczny prefiksu trasy.</span><span class="sxs-lookup"><span data-stu-id="9985b-136">`asp-route-` is a wild card route prefix.</span></span> <span data-ttu-id="9985b-137">Wartość, umieszczone po kreska kończąca zostanie potraktowany jako potencjalne parametru trasy.</span><span class="sxs-lookup"><span data-stu-id="9985b-137">Any value you put after the trailing dash will be interpreted as a potential route parameter.</span></span> <span data-ttu-id="9985b-138">Jeśli trasa domyślna nie zostanie znaleziony, prefiks trasy zostaną dodane do wygenerowanego href żądania parametr i wartość.</span><span class="sxs-lookup"><span data-stu-id="9985b-138">If a default route is not found, this route prefix will be appended to the generated href as a request parameter and value.</span></span> <span data-ttu-id="9985b-139">W przeciwnym razie zostaną zastąpione w szablonie trasy.</span><span class="sxs-lookup"><span data-stu-id="9985b-139">Otherwise it will be substituted in the route template.</span></span>

<span data-ttu-id="9985b-140">Zakładając, że użytkownik ma metody kontrolera zdefiniowane w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="9985b-140">Assuming you have a controller method defined as follows:</span></span>

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

<span data-ttu-id="9985b-141">I mieć domyślnego szablonu trasy zdefiniowanej w Twojej *Startup.cs* w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="9985b-141">And have the default route template defined in your *Startup.cs* as follows:</span></span>

```csharp
app.UseMvc(routes =>
{
   routes.MapRoute(
    name: "default",
    template: "{controller=Home}/{action=Index}/{id?}");
});

```

<span data-ttu-id="9985b-142">**Cshtml** pliku, który zawiera pomocnika Tag kotwicy koniecznych do używania **prelegenta** przekazany z kontrolera widoku modelu parametr wygląda następująco:</span><span class="sxs-lookup"><span data-stu-id="9985b-142">The **cshtml** file that contains the Anchor Tag Helper necessary to use the **speaker** model parameter passed in from the controller to the view is as follows:</span></span>

```cshtml
@model SpeakerData
<!DOCTYPE html>
<html><body>
<a asp-controller='Speaker' asp-action='Detail' asp-route-id=@Model.SpeakerId>SpeakerId: @Model.SpeakerId</a>
<body></html>
```

<span data-ttu-id="9985b-143">Wygenerowany kod HTML będzie następujący ponieważ **identyfikator** został znaleziony w trasy domyślnej.</span><span class="sxs-lookup"><span data-stu-id="9985b-143">The generated HTML will then be as follows because **id** was found in the default route.</span></span>

```html
<a href='/Speaker/Detail/12'>SpeakerId: 12</a>
```

<span data-ttu-id="9985b-144">Jeśli prefiks trasy nie jest częścią routingu znaleziono szablonu, który jest w przypadku następujących **cshtml** pliku:</span><span class="sxs-lookup"><span data-stu-id="9985b-144">If the route prefix is not part of the routing template found, which is the case with the following **cshtml** file:</span></span>

```cshtml
@model SpeakerData
<!DOCTYPE html>
<html><body>
<a asp-controller='Speaker' asp-action='Detail' asp-route-speakerid=@Model.SpeakerId>SpeakerId: @Model.SpeakerId</a>
<body></html>
```

<span data-ttu-id="9985b-145">Wygenerowany kod HTML będzie następujący ponieważ **speakerid** nie został znaleziony w trasie dopasowane:</span><span class="sxs-lookup"><span data-stu-id="9985b-145">The generated HTML will then be as follows because **speakerid** was not found in the route matched:</span></span>

```html
<a href='/Speaker/Detail?speakerid=12'>SpeakerId: 12</a>
```

<span data-ttu-id="9985b-146">Jeśli dowolny `asp-controller` lub `asp-action` nie są określone, następnie tego samego przetwarzania domyślny jest kontynuowane, jak jest `asp-route` atrybutu.</span><span class="sxs-lookup"><span data-stu-id="9985b-146">If either `asp-controller` or `asp-action` are not specified, then the same default processing is followed as is in the `asp-route` attribute.</span></span>

### <a name="asp-route"></a><span data-ttu-id="9985b-147">trasy ASP</span><span class="sxs-lookup"><span data-stu-id="9985b-147">asp-route</span></span>

<span data-ttu-id="9985b-148">`asp-route`zapewnia sposób utworzyć adres URL prowadzący bezpośrednio do nazwanej trasy.</span><span class="sxs-lookup"><span data-stu-id="9985b-148">`asp-route` provides a way to create a URL that links directly to a named route.</span></span> <span data-ttu-id="9985b-149">Przy użyciu atrybutów routingu, trasy nazwą może być pokazane na `SpeakerController` i używany w jego `Evaluations` metody.</span><span class="sxs-lookup"><span data-stu-id="9985b-149">Using routing attributes, a route can be named as shown in the `SpeakerController` and used in its `Evaluations` method.</span></span>

<span data-ttu-id="9985b-150">`Name = "speakerevals"`Określa, że Generowanie trasę bezpośrednio do tej metody za pomocą adresu URL pomocnika Tag kotwicy `/Speaker/Evaluations`.</span><span class="sxs-lookup"><span data-stu-id="9985b-150">`Name = "speakerevals"` tells the Anchor Tag Helper to generate a route directly to that controller method using the URL `/Speaker/Evaluations`.</span></span> <span data-ttu-id="9985b-151">Jeśli `asp-controller` lub `asp-action` określono oprócz `asp-route`, generowane trasy nie może być oczekiwań.</span><span class="sxs-lookup"><span data-stu-id="9985b-151">If `asp-controller` or `asp-action` is specified in addition to `asp-route`, the route generated may not be what you expect.</span></span> <span data-ttu-id="9985b-152">`asp-route`nie należy używać przy użyciu jednej z wartości atrybutów `asp-controller` lub `asp-action` Aby uniknąć konfliktu trasy.</span><span class="sxs-lookup"><span data-stu-id="9985b-152">`asp-route` should not be used with either of the attributes `asp-controller` or `asp-action` to avoid a route conflict.</span></span>

### <a name="asp-all-route-data"></a><span data-ttu-id="9985b-153">ASP-all danych trasy</span><span class="sxs-lookup"><span data-stu-id="9985b-153">asp-all-route-data</span></span>

<span data-ttu-id="9985b-154">`asp-all-route-data`Umożliwia tworzenie słownik par kluczy i wartości gdzie klucz to nazwa parametru, a wartość jest wartością skojarzonego z tym kluczem.</span><span class="sxs-lookup"><span data-stu-id="9985b-154">`asp-all-route-data` allows creating a dictionary of key value pairs where the key is the parameter name and the value is the value associated with that key.</span></span>

<span data-ttu-id="9985b-155">Co w przykładzie poniżej przedstawiono Słownik wbudowany jest tworzony, i dane są przekazywane do widoku razor.</span><span class="sxs-lookup"><span data-stu-id="9985b-155">As the example below shows, an inline dictionary is created and the data is passed to the razor view.</span></span> <span data-ttu-id="9985b-156">Alternatywnie dane można również przekazywane za pomocą modelu.</span><span class="sxs-lookup"><span data-stu-id="9985b-156">As an alternative, the data could also be passed in with your model.</span></span>

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

<span data-ttu-id="9985b-157">Powyższy kod generuje następujący adres URL: http://localhost/Speaker/EvaluationsCurrent?speakerId=11&currentYear=true</span><span class="sxs-lookup"><span data-stu-id="9985b-157">The code above generates the following URL: http://localhost/Speaker/EvaluationsCurrent?speakerId=11&currentYear=true</span></span>

<span data-ttu-id="9985b-158">Po kliknięciu łącza, metoda kontrolera `EvaluationsCurrent` jest wywoływana.</span><span class="sxs-lookup"><span data-stu-id="9985b-158">When the link is clicked, the controller method `EvaluationsCurrent` is called.</span></span> <span data-ttu-id="9985b-159">Jest to, ponieważ ten kontroler ma dwa parametry ciągu, które utworzył z `asp-all-route-data` słownika.</span><span class="sxs-lookup"><span data-stu-id="9985b-159">It is called because that controller has two string parameters that match what has been created from the `asp-all-route-data` dictionary.</span></span>

<span data-ttu-id="9985b-160">Jeśli parametry trasy klucze w słowniku dopasowania, te wartości zostaną zastąpione w trasie zgodnie z potrzebami i inne niepasujące wartości zostaną wygenerowane jako parametry żądania.</span><span class="sxs-lookup"><span data-stu-id="9985b-160">If any keys in the dictionary match route parameters, those values will be substituted in the route as appropriate and the other non-matching values will be generated as request parameters.</span></span>

### <a name="asp-fragment"></a><span data-ttu-id="9985b-161">ASP fragment</span><span class="sxs-lookup"><span data-stu-id="9985b-161">asp-fragment</span></span>

<span data-ttu-id="9985b-162">`asp-fragment`Definiuje fragmentu adresu URL, aby dołączyć do adresu URL.</span><span class="sxs-lookup"><span data-stu-id="9985b-162">`asp-fragment` defines a URL fragment to append to the URL.</span></span> <span data-ttu-id="9985b-163">Pomocnik Tag kotwicy spowoduje dodanie znaku numeru (#).</span><span class="sxs-lookup"><span data-stu-id="9985b-163">The Anchor Tag Helper will add the hash character (#).</span></span> <span data-ttu-id="9985b-164">Jeśli utworzysz tag:</span><span class="sxs-lookup"><span data-stu-id="9985b-164">If you create a tag:</span></span>

```cshtml
<a asp-action="Evaluations" asp-controller="Speaker"  
   asp-fragment="SpeakerEvaluations">About Speaker Evals</a>
```

<span data-ttu-id="9985b-165">Zostanie wygenerowany adres URL: http://localhost/Speaker/Evaluations#SpeakerEvaluations</span><span class="sxs-lookup"><span data-stu-id="9985b-165">The generated URL will be: http://localhost/Speaker/Evaluations#SpeakerEvaluations</span></span>

<span data-ttu-id="9985b-166">Skrót tagi są przydatne podczas tworzenia aplikacji po stronie klienta.</span><span class="sxs-lookup"><span data-stu-id="9985b-166">Hash tags are useful when building client-side applications.</span></span> <span data-ttu-id="9985b-167">Służy do łatwego oznaczenie i wyszukiwanie w języku JavaScript, na przykład.</span><span class="sxs-lookup"><span data-stu-id="9985b-167">They can be used for easy marking and searching in JavaScript, for example.</span></span>

### <a name="asp-area"></a><span data-ttu-id="9985b-168">obszar ASP</span><span class="sxs-lookup"><span data-stu-id="9985b-168">asp-area</span></span>

<span data-ttu-id="9985b-169">`asp-area`Ustawia nazwę obszaru, która korzysta z platformy ASP.NET Core można ustawić odpowiednie trasy.</span><span class="sxs-lookup"><span data-stu-id="9985b-169">`asp-area` sets the area name that ASP.NET Core uses to set the appropriate route.</span></span> <span data-ttu-id="9985b-170">Poniżej przedstawiono przykłady sposobu atrybut obszaru powoduje zmianę mapowania tras.</span><span class="sxs-lookup"><span data-stu-id="9985b-170">Below are examples of how the area attribute causes a remapping of routes.</span></span> <span data-ttu-id="9985b-171">Ustawienie `asp-area` blogach prefiksy katalogu `Areas/Blogs` do tras skojarzone kontrolery i widoki dla ten tag kotwicy.</span><span class="sxs-lookup"><span data-stu-id="9985b-171">Setting `asp-area` to Blogs prefixes the directory `Areas/Blogs` to the routes of the associated controllers and views for this anchor tag.</span></span>

* <span data-ttu-id="9985b-172">Nazwa projektu</span><span class="sxs-lookup"><span data-stu-id="9985b-172">Project name</span></span>
  * <span data-ttu-id="9985b-173">Wwwroot</span><span class="sxs-lookup"><span data-stu-id="9985b-173">wwwroot</span></span>
  * <span data-ttu-id="9985b-174">Obszary</span><span class="sxs-lookup"><span data-stu-id="9985b-174">Areas</span></span>
    * <span data-ttu-id="9985b-175">Blogi</span><span class="sxs-lookup"><span data-stu-id="9985b-175">Blogs</span></span>
      * <span data-ttu-id="9985b-176">Kontrolery</span><span class="sxs-lookup"><span data-stu-id="9985b-176">Controllers</span></span>
        * <span data-ttu-id="9985b-177">HomeController.cs</span><span class="sxs-lookup"><span data-stu-id="9985b-177">HomeController.cs</span></span>
      * <span data-ttu-id="9985b-178">Widoki</span><span class="sxs-lookup"><span data-stu-id="9985b-178">Views</span></span>
        * <span data-ttu-id="9985b-179">Home</span><span class="sxs-lookup"><span data-stu-id="9985b-179">Home</span></span>
          * <span data-ttu-id="9985b-180">Index.cshtml</span><span class="sxs-lookup"><span data-stu-id="9985b-180">Index.cshtml</span></span>
          * <span data-ttu-id="9985b-181">AboutBlog.cshtml</span><span class="sxs-lookup"><span data-stu-id="9985b-181">AboutBlog.cshtml</span></span>
  * <span data-ttu-id="9985b-182">Kontrolery</span><span class="sxs-lookup"><span data-stu-id="9985b-182">Controllers</span></span>

<span data-ttu-id="9985b-183">Określenie obszaru tag, który jest prawidłowy, takich jak ```area="Blogs"``` podczas odwoływania się do ```AboutBlog.cshtml``` pliku będzie wyglądać podobnie do następującej przy użyciu Pomocnika Tag kotwicy.</span><span class="sxs-lookup"><span data-stu-id="9985b-183">Specifying an area tag that is valid, such as ```area="Blogs"``` when referencing the ```AboutBlog.cshtml``` file will look like the following using the Anchor Tag Helper.</span></span>

```cshtml
<a asp-action="AboutBlog" asp-controller="Home" asp-area="Blogs">Blogs About</a>
```

<span data-ttu-id="9985b-184">Wygenerowany kod HTML będzie zawierać segmentu obszarów i będzie wyglądać następująco:</span><span class="sxs-lookup"><span data-stu-id="9985b-184">The generated HTML will include the areas segment and will be as follows:</span></span>

```html
<a href="/Blogs/Home/AboutBlog">Blogs About</a>
```

> [!TIP]
> <span data-ttu-id="9985b-185">Obszarów MVC do pracy w aplikacji sieci web szablon trasy musi zawierać odwołanie do obszaru, jeśli istnieje.</span><span class="sxs-lookup"><span data-stu-id="9985b-185">For MVC areas to work in a web application, the route template must include a reference to the area if it exists.</span></span> <span data-ttu-id="9985b-186">Szablonu, który jest drugi parametr z `routes.MapRoute` wywołania metody, będą wyświetlane jako:`template: '"{area:exists}/{controller=Home}/{action=Index}"'`</span><span class="sxs-lookup"><span data-stu-id="9985b-186">That template, which is the second parameter of the `routes.MapRoute` method call, will appear as: `template: '"{area:exists}/{controller=Home}/{action=Index}"'`</span></span>

### <a name="asp-protocol"></a><span data-ttu-id="9985b-187">Protokół ASP</span><span class="sxs-lookup"><span data-stu-id="9985b-187">asp-protocol</span></span>

<span data-ttu-id="9985b-188">`asp-protocol` Służy do określania protokół (takie jak `https`) w adresie URL.</span><span class="sxs-lookup"><span data-stu-id="9985b-188">The `asp-protocol` is for specifying a protocol (such as `https`) in your URL.</span></span> <span data-ttu-id="9985b-189">Przykład pomocnika Tag kotwicy, zawierającym nazwę protokołu będzie wyglądać w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="9985b-189">An example Anchor Tag Helper that includes the protocol will look as follows:</span></span>

```<a asp-protocol="https" asp-action="About" asp-controller="Home">About</a>```

<span data-ttu-id="9985b-190">i wygeneruje HTML w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="9985b-190">and will generate HTML as follows:</span></span>

```<a href="https://localhost/Home/About">About</a>```

<span data-ttu-id="9985b-191">Domena, w tym przykładzie jest localhost, ale pomocnika Tag kotwicy używa domeny publicznej witryny sieci Web podczas generowania adresu URL.</span><span class="sxs-lookup"><span data-stu-id="9985b-191">The domain in the example is localhost, but the Anchor Tag Helper uses the website's public domain when generating the URL.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="9985b-192">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="9985b-192">Additional resources</span></span>

* [<span data-ttu-id="9985b-193">Obszary</span><span class="sxs-lookup"><span data-stu-id="9985b-193">Areas</span></span>](xref:mvc/controllers/areas)
