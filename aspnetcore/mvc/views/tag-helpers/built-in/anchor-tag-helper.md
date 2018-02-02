---
title: Pomocnik Tag kotwicy
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
ms.openlocfilehash: f3b704174c3287edda12725b7973a2464e485bac
ms.sourcegitcommit: f2a11a89037471a77ad68a67533754b7bb8303e2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/01/2018
---
# <a name="anchor-tag-helper"></a><span data-ttu-id="a8b8b-103">Pomocnik Tag kotwicy</span><span class="sxs-lookup"><span data-stu-id="a8b8b-103">Anchor Tag Helper</span></span>

<span data-ttu-id="a8b8b-104">Przez [Kellner Peterowi](http://peterkellner.net) i [Scott Addie](https://github.com/scottaddie)</span><span class="sxs-lookup"><span data-stu-id="a8b8b-104">By [Peter Kellner](http://peterkellner.net) and [Scott Addie](https://github.com/scottaddie)</span></span>

<span data-ttu-id="a8b8b-105">[Wyświetlić lub pobrać przykładowy kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/tag-helpers/built-in/samples/TagHelpersBuiltInAspNetCore) ([sposobu pobierania](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="a8b8b-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/tag-helpers/built-in/samples/TagHelpersBuiltInAspNetCore) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="a8b8b-106">[Pomocnika Tag kotwicy](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper) zwiększa standardowe kotwicy HTML (`<a ... ></a>`) tag przez dodanie nowych atrybutów.</span><span class="sxs-lookup"><span data-stu-id="a8b8b-106">The [Anchor Tag Helper](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper) enhances the standard HTML anchor (`<a ... ></a>`) tag by adding new attributes.</span></span> <span data-ttu-id="a8b8b-107">Konwencja, nazw atrybutów są poprzedzane prefiksem `asp-`.</span><span class="sxs-lookup"><span data-stu-id="a8b8b-107">By convention, the attribute names are prefixed with `asp-`.</span></span> <span data-ttu-id="a8b8b-108">Element zakotwiczenia renderowanych `href` wartość atrybutu jest określany przez wartości `asp-` atrybutów.</span><span class="sxs-lookup"><span data-stu-id="a8b8b-108">The rendered anchor element's `href` attribute value is determined by the values of the `asp-` attributes.</span></span>

<span data-ttu-id="a8b8b-109">*SpeakerController* jest używany w przykłady w tym dokumencie:</span><span class="sxs-lookup"><span data-stu-id="a8b8b-109">*SpeakerController* is used in samples throughout this document:</span></span>

[!code-csharp[](samples/TagHelpersBuiltInAspNetCore/Controllers/SpeakerController.cs?name=snippet_SpeakerController)]

<span data-ttu-id="a8b8b-110">Spis `asp-` następujące atrybuty.</span><span class="sxs-lookup"><span data-stu-id="a8b8b-110">An inventory of the `asp-` attributes follows.</span></span>

## <a name="asp-controller"></a><span data-ttu-id="a8b8b-111">Kontroler ASP</span><span class="sxs-lookup"><span data-stu-id="a8b8b-111">asp-controller</span></span>

<span data-ttu-id="a8b8b-112">[Kontrolera asp](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.controller) atrybut przypisuje kontroler, używany do generowania adresu URL.</span><span class="sxs-lookup"><span data-stu-id="a8b8b-112">The [asp-controller](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.controller) attribute assigns the controller used for generating the URL.</span></span> <span data-ttu-id="a8b8b-113">Następujący kod zawiera listę wszystkich głośników:</span><span class="sxs-lookup"><span data-stu-id="a8b8b-113">The following markup lists all speakers:</span></span>

[!code-cshtml[](samples/TagHelpersBuiltInAspNetCore/Views/Home/Index.cshtml?name=snippet_AspController)]

<span data-ttu-id="a8b8b-114">Wygenerowany kod HTML:</span><span class="sxs-lookup"><span data-stu-id="a8b8b-114">The generated HTML:</span></span>

```html
<a href="/Speaker">All Speakers</a>
```

<span data-ttu-id="a8b8b-115">Jeśli `asp-controller` atrybut został określony i `asp-action` nie jest domyślnie `asp-action` wartość jest skojarzony z widokiem aktualnie wykonywane akcji kontrolera.</span><span class="sxs-lookup"><span data-stu-id="a8b8b-115">If the `asp-controller` attribute is specified and `asp-action` isn't, the default `asp-action` value is the controller action associated with the currently executing view.</span></span> <span data-ttu-id="a8b8b-116">Jeśli `asp-action` pominięto w poprzednim znaczników, i pomocnika Tag kotwicy jest używany w *HomeController*w *indeksu* widoku (*/Home*), jest wygenerowanego kodu HTML:</span><span class="sxs-lookup"><span data-stu-id="a8b8b-116">If `asp-action` is omitted from the preceding markup, and the Anchor Tag Helper is used in *HomeController*'s *Index* view (*/Home*), the generated HTML is:</span></span>

```html
<a href="/Home">All Speakers</a>
```

## <a name="asp-action"></a><span data-ttu-id="a8b8b-117">Akcja ASP</span><span class="sxs-lookup"><span data-stu-id="a8b8b-117">asp-action</span></span>

<span data-ttu-id="a8b8b-118">[Akcji asp](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.action) wartość atrybutu reprezentuje nazwę akcji kontrolera, które są uwzględnione w wygenerowanym `href` atrybutu.</span><span class="sxs-lookup"><span data-stu-id="a8b8b-118">The [asp-action](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.action) attribute value represents the controller action name included in the generated `href` attribute.</span></span> <span data-ttu-id="a8b8b-119">Następujący kod ustawia wygenerowany `href` wartość atrybutu do strony ocen prelegenta:</span><span class="sxs-lookup"><span data-stu-id="a8b8b-119">The following markup sets the generated `href` attribute value to the speaker evaluations page:</span></span>

[!code-cshtml[](samples/TagHelpersBuiltInAspNetCore/Views/Home/Index.cshtml?name=snippet_AspAction)]

<span data-ttu-id="a8b8b-120">Wygenerowany kod HTML:</span><span class="sxs-lookup"><span data-stu-id="a8b8b-120">The generated HTML:</span></span>

```html
<a href="/Speaker/Evaluations">Speaker Evaluations</a>
```

<span data-ttu-id="a8b8b-121">Jeśli nie `asp-controller` atrybut został określony, używany jest kontroler domyślne wywoływania widok wykonywania bieżącego widoku.</span><span class="sxs-lookup"><span data-stu-id="a8b8b-121">If no `asp-controller` attribute is specified, the default controller calling the view executing the current view is used.</span></span>

<span data-ttu-id="a8b8b-122">Jeśli `asp-action` wartość atrybutu jest `Index`, a następnie żadna akcja ze strony jest dołączany do adresu URL, co może prowadzić do wywołania domyślnych `Index` akcji.</span><span class="sxs-lookup"><span data-stu-id="a8b8b-122">If the `asp-action` attribute value is `Index`, then no action is appended to the URL, leading to the invocation of the default `Index` action.</span></span> <span data-ttu-id="a8b8b-123">Akcja określony (lub ustawiana domyślnie), musi istnieć w kontrolerze, do którego odwołuje się `asp-controller`.</span><span class="sxs-lookup"><span data-stu-id="a8b8b-123">The action specified (or defaulted), must exist in the controller referenced in `asp-controller`.</span></span>

## <a name="asp-route-value"></a><span data-ttu-id="a8b8b-124">asp-route-{value}</span><span class="sxs-lookup"><span data-stu-id="a8b8b-124">asp-route-{value}</span></span>

<span data-ttu-id="a8b8b-125">[Asp - route - {wartość value}](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.routevalues) atrybut umożliwia prefiks trasy symboli wieloznacznych.</span><span class="sxs-lookup"><span data-stu-id="a8b8b-125">The [asp-route-{value}](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.routevalues) attribute enables a wildcard route prefix.</span></span> <span data-ttu-id="a8b8b-126">Wszystkie wartości zajmujące `{value}` symbol zastępczy jest interpretowana jako potencjalne parametru trasy.</span><span class="sxs-lookup"><span data-stu-id="a8b8b-126">Any value occupying the `{value}` placeholder is interpreted as a potential route parameter.</span></span> <span data-ttu-id="a8b8b-127">Jeśli trasa domyślna nie zostanie odnaleziony, prefiks trasy jest dołączany do wygenerowanej `href` atrybutu żądania parametr i wartość.</span><span class="sxs-lookup"><span data-stu-id="a8b8b-127">If a default route isn't found, this route prefix is appended to the generated `href` attribute as a request parameter and value.</span></span> <span data-ttu-id="a8b8b-128">W przeciwnym razie zostanie zastąpiony w szablonie trasy.</span><span class="sxs-lookup"><span data-stu-id="a8b8b-128">Otherwise, it's substituted in the route template.</span></span>

<span data-ttu-id="a8b8b-129">Należy wziąć pod uwagę następujące akcji kontrolera:</span><span class="sxs-lookup"><span data-stu-id="a8b8b-129">Consider the following controller action:</span></span>

[!code-csharp[](samples/TagHelpersBuiltInAspNetCore/Controllers/BuiltInTagController.cs?name=snippet_AnchorTagHelperAction)]

<span data-ttu-id="a8b8b-130">Z domyślnego szablonu trasy zdefiniowanej w *Startup.Configure*:</span><span class="sxs-lookup"><span data-stu-id="a8b8b-130">With a default route template defined in *Startup.Configure*:</span></span>

[!code-csharp[](samples/TagHelpersBuiltInAspNetCore/Startup.cs?name=snippet_UseMvc&highlight=8-10)]

<span data-ttu-id="a8b8b-131">Widok MVC korzysta z modelu, pochodzącymi z działania, w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="a8b8b-131">The MVC view uses the model, provided by the action, as follows:</span></span>

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

<span data-ttu-id="a8b8b-132">Trasa domyślna `{id?}` symbol zastępczy został uzyskany.</span><span class="sxs-lookup"><span data-stu-id="a8b8b-132">The default route's `{id?}` placeholder was matched.</span></span> <span data-ttu-id="a8b8b-133">Wygenerowany kod HTML:</span><span class="sxs-lookup"><span data-stu-id="a8b8b-133">The generated HTML:</span></span>

```html
<a href="/Speaker/Detail/12">SpeakerId: 12</a>
```

<span data-ttu-id="a8b8b-134">Przyjęto założenie, że prefiks trasy nie jest częścią zgodnego szablonu routingu, podobnie jak w przypadku następujących widoku MVC:</span><span class="sxs-lookup"><span data-stu-id="a8b8b-134">Assume the route prefix isn't part of the matching routing template, as with the following MVC view:</span></span>

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

<span data-ttu-id="a8b8b-135">Poniższy kod HTML jest generowany `speakerid` nie został znaleziony w pasującej trasy:</span><span class="sxs-lookup"><span data-stu-id="a8b8b-135">The following HTML is generated because `speakerid` wasn't found in the matching route:</span></span>

```html
<a href="/Speaker/Detail?speakerid=12">SpeakerId: 12</a>
```

<span data-ttu-id="a8b8b-136">Jeśli dowolny `asp-controller` lub `asp-action` nie są określone, następnie tego samego przetwarzania domyślny jest kontynuowane, jak jest `asp-route` atrybutu.</span><span class="sxs-lookup"><span data-stu-id="a8b8b-136">If either `asp-controller` or `asp-action` aren't specified, then the same default processing is followed as is in the `asp-route` attribute.</span></span>

## <a name="asp-route"></a><span data-ttu-id="a8b8b-137">trasy ASP</span><span class="sxs-lookup"><span data-stu-id="a8b8b-137">asp-route</span></span>

<span data-ttu-id="a8b8b-138">[Trasy asp](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.route) atrybut służy do tworzenia adresu URL łączenie bezpośrednio do nazwanej trasy.</span><span class="sxs-lookup"><span data-stu-id="a8b8b-138">The [asp-route](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.route) attribute is used for creating a URL linking directly to a named route.</span></span> <span data-ttu-id="a8b8b-139">Przy użyciu [atrybuty routingu](xref:mvc/controllers/routing#attribute-routing), może mieć nazwę trasy, jak pokazano w `SpeakerController` i używany w jego `Evaluations` akcji:</span><span class="sxs-lookup"><span data-stu-id="a8b8b-139">Using [routing attributes](xref:mvc/controllers/routing#attribute-routing), a route can be named as shown in the `SpeakerController` and used in its `Evaluations` action:</span></span>

[!code-cshtml[](samples/TagHelpersBuiltInAspNetCore/Controllers/SpeakerController.cs?range=22-24)]

<span data-ttu-id="a8b8b-140">W następujących znaczników `asp-route` nazwanej trasy odwołuje się do atrybutu:</span><span class="sxs-lookup"><span data-stu-id="a8b8b-140">In the following markup, the `asp-route` attribute references the named route:</span></span>

[!code-cshtml[](samples/TagHelpersBuiltInAspNetCore/Views/Home/Index.cshtml?name=snippet_AspRoute)]

<span data-ttu-id="a8b8b-141">Pomocnik Tag kotwicy generuje trasę bezpośrednio do tej akcji kontrolera, używając adresu URL */prelegenta/oceny*.</span><span class="sxs-lookup"><span data-stu-id="a8b8b-141">The Anchor Tag Helper generates a route directly to that controller action using the URL */Speaker/Evaluations*.</span></span> <span data-ttu-id="a8b8b-142">Wygenerowany kod HTML:</span><span class="sxs-lookup"><span data-stu-id="a8b8b-142">The generated HTML:</span></span>

```html
<a href="/Speaker/Evaluations">Speaker Evaluations</a>
```

<span data-ttu-id="a8b8b-143">Jeśli `asp-controller` lub `asp-action` określono oprócz `asp-route`, generowane trasy nie może być oczekiwań.</span><span class="sxs-lookup"><span data-stu-id="a8b8b-143">If `asp-controller` or `asp-action` is specified in addition to `asp-route`, the route generated may not be what you expect.</span></span> <span data-ttu-id="a8b8b-144">Aby uniknąć konfliktu trasy, `asp-route` nie powinny być używane z `asp-controller` i `asp-action` atrybutów.</span><span class="sxs-lookup"><span data-stu-id="a8b8b-144">To avoid a route conflict, `asp-route` shouldn't be used with the `asp-controller` and `asp-action` attributes.</span></span>

## <a name="asp-all-route-data"></a><span data-ttu-id="a8b8b-145">asp-all-route-data</span><span class="sxs-lookup"><span data-stu-id="a8b8b-145">asp-all-route-data</span></span>

<span data-ttu-id="a8b8b-146">[Asp-all danych trasy](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.routevalues) atrybutu obsługuje tworzenie słownik par klucz wartość.</span><span class="sxs-lookup"><span data-stu-id="a8b8b-146">The [asp-all-route-data](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.routevalues) attribute supports the creation of a dictionary of key-value pairs.</span></span> <span data-ttu-id="a8b8b-147">Nazwa parametru jest klucz i wartość jest wartością parametru.</span><span class="sxs-lookup"><span data-stu-id="a8b8b-147">The key is the parameter name, and the value is the parameter value.</span></span>

<span data-ttu-id="a8b8b-148">W poniższym przykładzie słownik jest zainicjowany i przekazywane do widoku Razor.</span><span class="sxs-lookup"><span data-stu-id="a8b8b-148">In the following example, a dictionary is initialized and passed to a Razor view.</span></span> <span data-ttu-id="a8b8b-149">Alternatywnie dane można otrzymać za pomocą modelu.</span><span class="sxs-lookup"><span data-stu-id="a8b8b-149">Alternatively, the data could be passed in with your model.</span></span>

[!code-cshtml[](samples/TagHelpersBuiltInAspNetCore/Views/Home/Index.cshtml?name=snippet_AspAllRouteData)]

<span data-ttu-id="a8b8b-150">Poprzedni kod generuje poniższy kod HTML:</span><span class="sxs-lookup"><span data-stu-id="a8b8b-150">The preceding code generates the following HTML:</span></span>

```html
<a href="/Speaker/EvaluationsCurrent?speakerId=11&currentYear=true">Speaker Evaluations</a>
```

<span data-ttu-id="a8b8b-151">`asp-all-route-data` Słownika właściwości jest spłaszczona wygenerowało ciąg kwerendy spełniających wymagania przeciążone `Evaluations` akcji:</span><span class="sxs-lookup"><span data-stu-id="a8b8b-151">The `asp-all-route-data` dictionary is flattened to produce a querystring meeting the requirements of the overloaded `Evaluations` action:</span></span>

[!code-csharp[](samples/TagHelpersBuiltInAspNetCore/Controllers/SpeakerController.cs?range=26-30)]

<span data-ttu-id="a8b8b-152">Jeśli klucze w słowniku zgodne parametrów trasy, te wartości są zastępowane w trasie zależnie od potrzeb.</span><span class="sxs-lookup"><span data-stu-id="a8b8b-152">If any keys in the dictionary match route parameters, those values are substituted in the route as appropriate.</span></span> <span data-ttu-id="a8b8b-153">Inne niepasujące wartości są generowane jako parametry żądania.</span><span class="sxs-lookup"><span data-stu-id="a8b8b-153">The other non-matching values are generated as request parameters.</span></span>

## <a name="asp-fragment"></a><span data-ttu-id="a8b8b-154">asp-fragment</span><span class="sxs-lookup"><span data-stu-id="a8b8b-154">asp-fragment</span></span>

<span data-ttu-id="a8b8b-155">[Fragmentu asp](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.fragment) atrybut definiuje fragmentu adresu URL, aby dołączyć do adresu URL.</span><span class="sxs-lookup"><span data-stu-id="a8b8b-155">The [asp-fragment](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.fragment) attribute defines a URL fragment to append to the URL.</span></span> <span data-ttu-id="a8b8b-156">Pomocnika Tag kotwicy dodaje znaku numeru (#).</span><span class="sxs-lookup"><span data-stu-id="a8b8b-156">The Anchor Tag Helper adds the hash character (#).</span></span> <span data-ttu-id="a8b8b-157">Rozważmy następujący kod:</span><span class="sxs-lookup"><span data-stu-id="a8b8b-157">Consider the following markup:</span></span>

[!code-cshtml[](samples/TagHelpersBuiltInAspNetCore/Views/Home/Index.cshtml?name=snippet_AspFragment)]

<span data-ttu-id="a8b8b-158">Wygenerowany kod HTML:</span><span class="sxs-lookup"><span data-stu-id="a8b8b-158">The generated HTML:</span></span>

```html
<a href="/Speaker/Evaluations#SpeakerEvaluations">Speaker Evaluations</a>
```

<span data-ttu-id="a8b8b-159">Skrót tagi są przydatne podczas kompilowania aplikacji po stronie klienta.</span><span class="sxs-lookup"><span data-stu-id="a8b8b-159">Hash tags are useful when building client-side apps.</span></span> <span data-ttu-id="a8b8b-160">Służy do łatwego oznaczenie i wyszukiwanie w języku JavaScript, na przykład.</span><span class="sxs-lookup"><span data-stu-id="a8b8b-160">They can be used for easy marking and searching in JavaScript, for example.</span></span>

## <a name="asp-area"></a><span data-ttu-id="a8b8b-161">asp-area</span><span class="sxs-lookup"><span data-stu-id="a8b8b-161">asp-area</span></span>

<span data-ttu-id="a8b8b-162">[Obszaru asp](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.area) atrybut ustawia nazwę obszaru służy do określania odpowiednich trasy.</span><span class="sxs-lookup"><span data-stu-id="a8b8b-162">The [asp-area](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.area) attribute sets the area name used to set the appropriate route.</span></span> <span data-ttu-id="a8b8b-163">Poniższy przykład przedstawia, jak atrybut obszaru powoduje zmianę mapowania tras.</span><span class="sxs-lookup"><span data-stu-id="a8b8b-163">The following example depicts how the area attribute causes a remapping of routes.</span></span> <span data-ttu-id="a8b8b-164">Ustawienie `asp-area` "Blogach" prefiksy katalogu *obszarów/blogi* do tras skojarzone kontrolery i widoki dla ten tag kotwicy.</span><span class="sxs-lookup"><span data-stu-id="a8b8b-164">Setting `asp-area` to "Blogs" prefixes the directory *Areas/Blogs* to the routes of the associated controllers and views for this anchor tag.</span></span>

* <span data-ttu-id="a8b8b-165">**< Nazwa projektu\>**</span><span class="sxs-lookup"><span data-stu-id="a8b8b-165">**<Project name\>**</span></span>
  * <span data-ttu-id="a8b8b-166">**wwwroot**</span><span class="sxs-lookup"><span data-stu-id="a8b8b-166">**wwwroot**</span></span>
  * <span data-ttu-id="a8b8b-167">**Obszary**</span><span class="sxs-lookup"><span data-stu-id="a8b8b-167">**Areas**</span></span>
    * <span data-ttu-id="a8b8b-168">**Blogi**</span><span class="sxs-lookup"><span data-stu-id="a8b8b-168">**Blogs**</span></span>
      * <span data-ttu-id="a8b8b-169">**Kontrolery**</span><span class="sxs-lookup"><span data-stu-id="a8b8b-169">**Controllers**</span></span>
        * <span data-ttu-id="a8b8b-170">*HomeController.cs*</span><span class="sxs-lookup"><span data-stu-id="a8b8b-170">*HomeController.cs*</span></span>
      * <span data-ttu-id="a8b8b-171">**Widoki**</span><span class="sxs-lookup"><span data-stu-id="a8b8b-171">**Views**</span></span>
        * <span data-ttu-id="a8b8b-172">**Strona główna**</span><span class="sxs-lookup"><span data-stu-id="a8b8b-172">**Home**</span></span>
          * <span data-ttu-id="a8b8b-173">*AboutBlog.cshtml*</span><span class="sxs-lookup"><span data-stu-id="a8b8b-173">*AboutBlog.cshtml*</span></span>
          * <span data-ttu-id="a8b8b-174">*Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="a8b8b-174">*Index.cshtml*</span></span>
        * <span data-ttu-id="a8b8b-175">*_ViewStart.cshtml*</span><span class="sxs-lookup"><span data-stu-id="a8b8b-175">*_ViewStart.cshtml*</span></span>
  * <span data-ttu-id="a8b8b-176">**Kontrolery**</span><span class="sxs-lookup"><span data-stu-id="a8b8b-176">**Controllers**</span></span>

<span data-ttu-id="a8b8b-177">Podane poprzedniego hierarchii katalogów znaczników, aby odwołać *AboutBlog.cshtml* plik jest:</span><span class="sxs-lookup"><span data-stu-id="a8b8b-177">Given the preceding directory hierarchy, the markup to reference the *AboutBlog.cshtml* file is:</span></span>

[!code-cshtml[](samples/TagHelpersBuiltInAspNetCore/Views/Home/Index.cshtml?name=snippet_AspArea)]

<span data-ttu-id="a8b8b-178">Wygenerowany kod HTML:</span><span class="sxs-lookup"><span data-stu-id="a8b8b-178">The generated HTML:</span></span>

```html
<a href="/Blogs/Home/AboutBlog">About Blog</a>
```

> [!TIP]
> <span data-ttu-id="a8b8b-179">Obszarów do pracy w aplikacji MVC szablon trasy musi zawierać odwołanie do obszaru, jeśli istnieje.</span><span class="sxs-lookup"><span data-stu-id="a8b8b-179">For areas to work in an MVC app, the route template must include a reference to the area, if it exists.</span></span> <span data-ttu-id="a8b8b-180">Ten szablon jest reprezentowana przez drugi parametr funkcji `routes.MapRoute` wywołanie metody *Startup.Configure*:[!code-csharp[](samples/TagHelpersBuiltInAspNetCore/Startup.cs?name=snippet_UseMvc&highlight=5)]</span><span class="sxs-lookup"><span data-stu-id="a8b8b-180">That template is represented by the second parameter of the `routes.MapRoute` method call in *Startup.Configure*: [!code-csharp[](samples/TagHelpersBuiltInAspNetCore/Startup.cs?name=snippet_UseMvc&highlight=5)]</span></span>

## <a name="asp-protocol"></a><span data-ttu-id="a8b8b-181">asp-protocol</span><span class="sxs-lookup"><span data-stu-id="a8b8b-181">asp-protocol</span></span>

<span data-ttu-id="a8b8b-182">[Protokołu asp](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.protocol) atrybut służy do określania protokół (takie jak `https`) w adresie URL.</span><span class="sxs-lookup"><span data-stu-id="a8b8b-182">The [asp-protocol](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.protocol) attribute is for specifying a protocol (such as `https`) in your URL.</span></span> <span data-ttu-id="a8b8b-183">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="a8b8b-183">For example:</span></span>

<span data-ttu-id="a8b8b-184">[!code-cshtml[samples/TagHelpersBuiltInAspNetCore/Views/Index.cshtml?name=snippet_AspProtocol]]</span><span class="sxs-lookup"><span data-stu-id="a8b8b-184">[!code-cshtml[samples/TagHelpersBuiltInAspNetCore/Views/Index.cshtml?name=snippet_AspProtocol]]</span></span>

<span data-ttu-id="a8b8b-185">Wygenerowany kod HTML:</span><span class="sxs-lookup"><span data-stu-id="a8b8b-185">The generated HTML:</span></span>

```html
<a href="https://localhost/Home/About">About</a>
```

<span data-ttu-id="a8b8b-186">Nazwa hosta, w tym przykładzie jest localhost, ale pomocnika Tag kotwicy używa domeny publicznej witryny sieci Web podczas generowania adresu URL.</span><span class="sxs-lookup"><span data-stu-id="a8b8b-186">The host name in the example is localhost, but the Anchor Tag Helper uses the website's public domain when generating the URL.</span></span>

## <a name="asp-host"></a><span data-ttu-id="a8b8b-187">ASP host</span><span class="sxs-lookup"><span data-stu-id="a8b8b-187">asp-host</span></span>

<span data-ttu-id="a8b8b-188">[Hosta asp](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.host) atrybut służy do określania nazwy hosta w adresie URL.</span><span class="sxs-lookup"><span data-stu-id="a8b8b-188">The [asp-host](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.host) attribute is for specifying a host name in your URL.</span></span> <span data-ttu-id="a8b8b-189">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="a8b8b-189">For example:</span></span>

[!code-cshtml[](samples/TagHelpersBuiltInAspNetCore/Views/Home/Index.cshtml?name=snippet_AspHost)]

<span data-ttu-id="a8b8b-190">Wygenerowany kod HTML:</span><span class="sxs-lookup"><span data-stu-id="a8b8b-190">The generated HTML:</span></span>

```html
<a href="https://microsoft.com/Home/About">About</a>
```

## <a name="asp-page"></a><span data-ttu-id="a8b8b-191">Strona ASP</span><span class="sxs-lookup"><span data-stu-id="a8b8b-191">asp-page</span></span>

<span data-ttu-id="a8b8b-192">[Strona asp](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.page) atrybut jest używany w przypadku stron Razor.</span><span class="sxs-lookup"><span data-stu-id="a8b8b-192">The [asp-page](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.page) attribute is used with Razor Pages.</span></span> <span data-ttu-id="a8b8b-193">Użyj jej do ustawienia tag kotwicy `href` wartość atrybutu do określonej strony.</span><span class="sxs-lookup"><span data-stu-id="a8b8b-193">Use it to set an anchor tag's `href` attribute value to a specific page.</span></span> <span data-ttu-id="a8b8b-194">Prefiksu nazwy strony się ukośnikiem ("/") tworzy adres URL.</span><span class="sxs-lookup"><span data-stu-id="a8b8b-194">Prefixing the page name with a forward slash ("/") creates the URL.</span></span>

<span data-ttu-id="a8b8b-195">Poniższy przykład punkty do uczestnika Razor strony:</span><span class="sxs-lookup"><span data-stu-id="a8b8b-195">The following sample points to the attendee Razor Page:</span></span>

[!code-cshtml[](samples/TagHelpersBuiltInAspNetCore/Views/Home/Index.cshtml?name=snippet_AspPage)]

<span data-ttu-id="a8b8b-196">Wygenerowany kod HTML:</span><span class="sxs-lookup"><span data-stu-id="a8b8b-196">The generated HTML:</span></span>

```html
<a href="/Attendee">All Attendees</a>
```

<span data-ttu-id="a8b8b-197">`asp-page` Jest wykluczają się wzajemnie z atrybutem `asp-route`, `asp-controller`, i `asp-action` atrybutów.</span><span class="sxs-lookup"><span data-stu-id="a8b8b-197">The `asp-page` attribute is mutually exclusive with the `asp-route`, `asp-controller`, and `asp-action` attributes.</span></span> <span data-ttu-id="a8b8b-198">Jednak `asp-page` mogą być używane z `asp-route-{value}` do kontrolowania routingu, jak pokazano następujący kod:</span><span class="sxs-lookup"><span data-stu-id="a8b8b-198">However, `asp-page` can be used with `asp-route-{value}` to control routing, as the following markup demonstrates:</span></span>

[!code-cshtml[](samples/TagHelpersBuiltInAspNetCore/Views/Home/Index.cshtml?name=snippet_AspPageAspRouteId)]

<span data-ttu-id="a8b8b-199">Wygenerowany kod HTML:</span><span class="sxs-lookup"><span data-stu-id="a8b8b-199">The generated HTML:</span></span>

```html
<a href="/Attendee?attendeeid=10">View Attendee</a>
```

## <a name="asp-page-handler"></a><span data-ttu-id="a8b8b-200">asp-page-handler</span><span class="sxs-lookup"><span data-stu-id="a8b8b-200">asp-page-handler</span></span>

<span data-ttu-id="a8b8b-201">[Program obsługi stron asp](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.pagehandler) atrybut jest używany w przypadku stron Razor.</span><span class="sxs-lookup"><span data-stu-id="a8b8b-201">The [asp-page-handler](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.pagehandler) attribute is used with Razor Pages.</span></span> <span data-ttu-id="a8b8b-202">Przewodnik jest przeznaczony dla połączeń do określonej strony programów obsługi.</span><span class="sxs-lookup"><span data-stu-id="a8b8b-202">It's intended for linking to specific page handlers.</span></span>

<span data-ttu-id="a8b8b-203">Należy wziąć pod uwagę następujące obsługi strony:</span><span class="sxs-lookup"><span data-stu-id="a8b8b-203">Consider the following page handler:</span></span>

[!code-csharp[](samples/TagHelpersBuiltInAspNetCore/Pages/Attendee.cshtml.cs?name=snippet_OnGetProfileHandler)]

<span data-ttu-id="a8b8b-204">Znaczników linki do powiązanych modelu strony `OnGetProfile` obsługi strony.</span><span class="sxs-lookup"><span data-stu-id="a8b8b-204">The page model's associated markup links to the `OnGetProfile` page handler.</span></span> <span data-ttu-id="a8b8b-205">Należy pamiętać, że `On<Verb>` prefiks nazwy metody obsługi strona zostanie pominięty w `asp-page-handler` wartość atrybutu.</span><span class="sxs-lookup"><span data-stu-id="a8b8b-205">Note that the `On<Verb>` prefix of the page handler method name is omitted in the `asp-page-handler` attribute value.</span></span> <span data-ttu-id="a8b8b-206">Jeżeli metodę asynchroniczną `Async` zbyt zostaną pominięte sufiks.</span><span class="sxs-lookup"><span data-stu-id="a8b8b-206">If this were an asynchronous method, the `Async` suffix would be omitted too.</span></span>

[!code-cshtml[](samples/TagHelpersBuiltInAspNetCore/Views/Home/Index.cshtml?name=snippet_AspPageHandler)]

<span data-ttu-id="a8b8b-207">Wygenerowany kod HTML:</span><span class="sxs-lookup"><span data-stu-id="a8b8b-207">The generated HTML:</span></span>

```html
<a href="/Attendee?attendeeid=12&handler=Profile">Attendee Profile</a>
```

## <a name="additional-resources"></a><span data-ttu-id="a8b8b-208">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="a8b8b-208">Additional resources</span></span>

* [<span data-ttu-id="a8b8b-209">Obszary</span><span class="sxs-lookup"><span data-stu-id="a8b8b-209">Areas</span></span>](xref:mvc/controllers/areas)
* [<span data-ttu-id="a8b8b-210">Wprowadzenie do stron Razor</span><span class="sxs-lookup"><span data-stu-id="a8b8b-210">Intro to Razor Pages</span></span>](xref:mvc/razor-pages/index)
