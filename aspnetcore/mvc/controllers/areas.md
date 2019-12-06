---
title: Obszary w ASP.NET Core
author: rick-anderson
description: Dowiedz się, jak obszary są funkcją ASP.NET MVC służącą do organizowania powiązanych funkcji w grupie jako oddzielnej przestrzeni nazw (dla routingu) i struktury folderów (dla widoków).
ms.author: riande
ms.date: 12/05/2019
uid: mvc/controllers/areas
ms.openlocfilehash: 7e02a21361e0e2148b29a3ae0f1ba25e68239e13
ms.sourcegitcommit: c0b72b344dadea835b0e7943c52463f13ab98dd1
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/06/2019
ms.locfileid: "74881116"
---
# <a name="areas-in-aspnet-core"></a><span data-ttu-id="81266-103">Obszary w ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="81266-103">Areas in ASP.NET Core</span></span>

<span data-ttu-id="81266-104">Autorzy [Dhananjay Kumara](https://twitter.com/debug_mode) i [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="81266-104">By [Dhananjay Kumar](https://twitter.com/debug_mode) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="81266-105">Obszary są funkcją ASP.NET używaną do organizowania powiązanych funkcji w grupie jako oddzielnej przestrzeni nazw (dla routingu) i struktury folderów (dla widoków).</span><span class="sxs-lookup"><span data-stu-id="81266-105">Areas are an ASP.NET feature used to organize related functionality into a group as a separate namespace (for routing) and folder structure (for views).</span></span> <span data-ttu-id="81266-106">Za pomocą obszarów tworzy hierarchię do celów routingu przez dodanie innego parametru trasy, `area`, do `controller` i `action` lub `page`strony Razor.</span><span class="sxs-lookup"><span data-stu-id="81266-106">Using areas creates a hierarchy for the purpose of routing by adding another route parameter, `area`, to `controller` and `action` or a Razor Page `page`.</span></span>

<span data-ttu-id="81266-107">Obszary umożliwiają dzielenie aplikacji sieci Web na ASP.NET Core na mniejsze grupy funkcjonalne, z których każdy ma swój własny zestaw Razor Pages, kontrolerów, widoków i modeli.</span><span class="sxs-lookup"><span data-stu-id="81266-107">Areas provide a way to partition an ASP.NET Core Web app into smaller functional groups, each  with its own set of Razor Pages, controllers, views, and models.</span></span> <span data-ttu-id="81266-108">Obszar jest efektywnie strukturą wewnątrz aplikacji.</span><span class="sxs-lookup"><span data-stu-id="81266-108">An area is effectively a structure inside an app.</span></span> <span data-ttu-id="81266-109">W projekcie sieci Web ASP.NET Core składniki logiczne, takie jak Pages, model, Controller i View, są przechowywane w różnych folderach.</span><span class="sxs-lookup"><span data-stu-id="81266-109">In an ASP.NET Core web project, logical components like Pages, Model, Controller, and View are kept in different folders.</span></span> <span data-ttu-id="81266-110">Środowisko uruchomieniowe ASP.NET Core używa konwencji nazewnictwa, aby utworzyć relację między tymi składnikami.</span><span class="sxs-lookup"><span data-stu-id="81266-110">The ASP.NET Core runtime uses naming conventions to create the relationship between these components.</span></span> <span data-ttu-id="81266-111">W przypadku dużej aplikacji warto podzielić aplikację na oddzielne obszary wysokiego poziomu funkcji.</span><span class="sxs-lookup"><span data-stu-id="81266-111">For a large app, it may be advantageous to partition the app into separate high level areas of functionality.</span></span> <span data-ttu-id="81266-112">Na przykład aplikacja handlu elektronicznego z wieloma jednostkami biznesowymi, takimi jak wyewidencjonowywanie, rozliczenia i wyszukiwanie.</span><span class="sxs-lookup"><span data-stu-id="81266-112">For instance, an e-commerce app with multiple business units, such as checkout, billing, and search.</span></span> <span data-ttu-id="81266-113">Każda z tych jednostek ma własny obszar, który zawiera widoki, kontrolery, Razor Pages i modele.</span><span class="sxs-lookup"><span data-stu-id="81266-113">Each of these units have their own area to contain views, controllers, Razor Pages, and models.</span></span>

<span data-ttu-id="81266-114">Rozważ użycie obszarów w projekcie, gdy:</span><span class="sxs-lookup"><span data-stu-id="81266-114">Consider using Areas in a project when:</span></span>

* <span data-ttu-id="81266-115">Aplikacja składa się z wielu składników funkcjonalnych wysokiego poziomu, które można logicznie oddzielić.</span><span class="sxs-lookup"><span data-stu-id="81266-115">The app is made of multiple high-level functional components that can be logically separated.</span></span>
* <span data-ttu-id="81266-116">Chcesz podzielić aplikację na partycje, tak aby każdy obszar funkcjonalny mógł działać niezależnie.</span><span class="sxs-lookup"><span data-stu-id="81266-116">You want to partition the app so that each functional area can be worked on independently.</span></span>

<span data-ttu-id="81266-117">[Wyświetlanie lub Pobieranie przykładowego kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/areas/samples) ([jak pobrać](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="81266-117">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/areas/samples) ([how to download](xref:index#how-to-download-a-sample)).</span></span> <span data-ttu-id="81266-118">Przykład pobierania zawiera podstawową aplikację do testowania obszarów.</span><span class="sxs-lookup"><span data-stu-id="81266-118">The download sample provides a basic app for testing areas.</span></span>

<span data-ttu-id="81266-119">Jeśli używasz Razor Pages, zobacz [obszary z Razor Pages](#areas-with-razor-pages) w tym dokumencie.</span><span class="sxs-lookup"><span data-stu-id="81266-119">If you're using Razor Pages, see [Areas with Razor Pages](#areas-with-razor-pages) in this document.</span></span>

## <a name="areas-for-controllers-with-views"></a><span data-ttu-id="81266-120">Obszary dla kontrolerów z widokami</span><span class="sxs-lookup"><span data-stu-id="81266-120">Areas for controllers with views</span></span>

<span data-ttu-id="81266-121">Typowy ASP.NET Core aplikacja internetowa korzystająca z obszarów, kontrolerów i widoków zawiera następujące elementy:</span><span class="sxs-lookup"><span data-stu-id="81266-121">A typical ASP.NET Core web app using areas, controllers, and views contains the following:</span></span>

* <span data-ttu-id="81266-122">[Struktura folderów obszaru](#area-folder-structure).</span><span class="sxs-lookup"><span data-stu-id="81266-122">An [Area folder structure](#area-folder-structure).</span></span>
* <span data-ttu-id="81266-123">Kontrolery z atrybutem [`[Area]`](#attribute) , aby skojarzyć kontroler z obszarem:</span><span class="sxs-lookup"><span data-stu-id="81266-123">Controllers with the [`[Area]`](#attribute) attribute to associate the controller with the area:</span></span>

  [!code-csharp[](areas/samples/MVCareas/Areas/Products/Controllers/ManageController.cs?name=snippet2)]

* <span data-ttu-id="81266-124">[Trasa obszaru dodana do uruchamiania](#add-area-route):</span><span class="sxs-lookup"><span data-stu-id="81266-124">The [area route added to startup](#add-area-route):</span></span>

  [!code-csharp[](areas/samples/MVCareas/Startup.cs?name=snippet2&highlight=3-6)]

### <a name="area-folder-structure"></a><span data-ttu-id="81266-125">Struktura folderów obszaru</span><span class="sxs-lookup"><span data-stu-id="81266-125">Area folder structure</span></span>

<span data-ttu-id="81266-126">Weź pod uwagę aplikację, która ma dwie grupy logiczne, *produkty* i *usługi*.</span><span class="sxs-lookup"><span data-stu-id="81266-126">Consider an app that has two logical groups, *Products* and *Services*.</span></span> <span data-ttu-id="81266-127">Przy użyciu obszarów struktura folderów będzie wyglądać podobnie do następujących:</span><span class="sxs-lookup"><span data-stu-id="81266-127">Using areas, the folder structure would be similar to the following:</span></span>

* <span data-ttu-id="81266-128">Project name (Nazwa projektu)</span><span class="sxs-lookup"><span data-stu-id="81266-128">Project name</span></span>
  * <span data-ttu-id="81266-129">Obszary</span><span class="sxs-lookup"><span data-stu-id="81266-129">Areas</span></span>
    * <span data-ttu-id="81266-130">Produkty</span><span class="sxs-lookup"><span data-stu-id="81266-130">Products</span></span>
      * <span data-ttu-id="81266-131">Kontrolery</span><span class="sxs-lookup"><span data-stu-id="81266-131">Controllers</span></span>
        * <span data-ttu-id="81266-132">HomeController.cs</span><span class="sxs-lookup"><span data-stu-id="81266-132">HomeController.cs</span></span>
        * <span data-ttu-id="81266-133">ManageController.cs</span><span class="sxs-lookup"><span data-stu-id="81266-133">ManageController.cs</span></span>
      * <span data-ttu-id="81266-134">Widoki</span><span class="sxs-lookup"><span data-stu-id="81266-134">Views</span></span>
        * <span data-ttu-id="81266-135">Strona główna programu</span><span class="sxs-lookup"><span data-stu-id="81266-135">Home</span></span>
          * <span data-ttu-id="81266-136">Index.cshtml</span><span class="sxs-lookup"><span data-stu-id="81266-136">Index.cshtml</span></span>
        * <span data-ttu-id="81266-137">Zarządzanie</span><span class="sxs-lookup"><span data-stu-id="81266-137">Manage</span></span>
          * <span data-ttu-id="81266-138">Index.cshtml</span><span class="sxs-lookup"><span data-stu-id="81266-138">Index.cshtml</span></span>
          * <span data-ttu-id="81266-139">About. cshtml</span><span class="sxs-lookup"><span data-stu-id="81266-139">About.cshtml</span></span>
    * <span data-ttu-id="81266-140">Usługi</span><span class="sxs-lookup"><span data-stu-id="81266-140">Services</span></span>
      * <span data-ttu-id="81266-141">Kontrolery</span><span class="sxs-lookup"><span data-stu-id="81266-141">Controllers</span></span>
        * <span data-ttu-id="81266-142">HomeController.cs</span><span class="sxs-lookup"><span data-stu-id="81266-142">HomeController.cs</span></span>
      * <span data-ttu-id="81266-143">Widoki</span><span class="sxs-lookup"><span data-stu-id="81266-143">Views</span></span>
        * <span data-ttu-id="81266-144">Strona główna programu</span><span class="sxs-lookup"><span data-stu-id="81266-144">Home</span></span>
          * <span data-ttu-id="81266-145">Index.cshtml</span><span class="sxs-lookup"><span data-stu-id="81266-145">Index.cshtml</span></span>

<span data-ttu-id="81266-146">Chociaż poprzedni układ jest typowy w przypadku korzystania z obszarów, do korzystania z tej struktury folderów są wymagane tylko pliki widoku.</span><span class="sxs-lookup"><span data-stu-id="81266-146">While the preceding layout is typical when using Areas, only the view files are required to use this folder structure.</span></span> <span data-ttu-id="81266-147">Wyświetl wyszukiwania odnajdywania dla zgodnego pliku widoku obszaru w następującej kolejności:</span><span class="sxs-lookup"><span data-stu-id="81266-147">View discovery searches for a matching area view file in the following order:</span></span>

```text
/Areas/<Area-Name>/Views/<Controller-Name>/<Action-Name>.cshtml
/Areas/<Area-Name>/Views/Shared/<Action-Name>.cshtml
/Views/Shared/<Action-Name>.cshtml
/Pages/Shared/<Action-Name>.cshtml
```

<a name="attribute"></a>

### <a name="associate-the-controller-with-an-area"></a><span data-ttu-id="81266-148">Skojarz kontroler z obszarem</span><span class="sxs-lookup"><span data-stu-id="81266-148">Associate the controller with an Area</span></span>

<span data-ttu-id="81266-149">Kontrolery obszaru są oznaczone atrybutem [&rbrack;obszaru&lbrack;](xref:Microsoft.AspNetCore.Mvc.AreaAttribute) :</span><span class="sxs-lookup"><span data-stu-id="81266-149">Area controllers are designated with the [&lbrack;Area&rbrack;](xref:Microsoft.AspNetCore.Mvc.AreaAttribute) attribute:</span></span>

[!code-csharp[](areas/samples/MVCareas/Areas/Products/Controllers/ManageController.cs?highlight=5&name=snippet)]

### <a name="add-area-route"></a><span data-ttu-id="81266-150">Dodaj trasę obszaru</span><span class="sxs-lookup"><span data-stu-id="81266-150">Add Area route</span></span>

<span data-ttu-id="81266-151">Trasy obszaru zwykle używają konwencjonalnego routingu, a nie routingu atrybutu.</span><span class="sxs-lookup"><span data-stu-id="81266-151">Area routes typically use conventional routing rather than attribute routing.</span></span> <span data-ttu-id="81266-152">Routowanie konwencjonalne jest zależne od kolejności.</span><span class="sxs-lookup"><span data-stu-id="81266-152">Conventional routing is order-dependent.</span></span> <span data-ttu-id="81266-153">Ogólnie rzecz biorąc, trasy z obszarami należy umieścić wcześniej w tabeli tras, ponieważ są one bardziej specyficzne niż trasy bez obszaru.</span><span class="sxs-lookup"><span data-stu-id="81266-153">In general, routes with areas should be placed earlier in the route table as they're more specific than routes without an area.</span></span>

<span data-ttu-id="81266-154">`{area:...}` może służyć jako token w szablonach tras, jeśli przestrzeń adresów URL jest jednolita dla wszystkich obszarów:</span><span class="sxs-lookup"><span data-stu-id="81266-154">`{area:...}` can be used as a token in route templates if url space is uniform across all areas:</span></span>

[!code-csharp[](areas/samples/MVCareas/Startup.cs?name=snippet&highlight=18-21)]

<span data-ttu-id="81266-155">W poprzednim kodzie `exists` stosuje ograniczenie, które musi być zgodne z obszarem.</span><span class="sxs-lookup"><span data-stu-id="81266-155">In the preceding code, `exists` applies a constraint that the route must match an area.</span></span> <span data-ttu-id="81266-156">Używanie `{area:...}` jest najmniej skomplikowanym mechanizmem do dodawania routingu do obszarów.</span><span class="sxs-lookup"><span data-stu-id="81266-156">Using `{area:...}` is the least complicated mechanism to adding routing to areas.</span></span>

<span data-ttu-id="81266-157">Poniższy kod używa <xref:Microsoft.AspNetCore.Builder.MvcAreaRouteBuilderExtensions.MapAreaRoute*> do tworzenia dwóch nazwanych tras obszaru:</span><span class="sxs-lookup"><span data-stu-id="81266-157">The following code uses <xref:Microsoft.AspNetCore.Builder.MvcAreaRouteBuilderExtensions.MapAreaRoute*> to create two named area routes:</span></span>

[!code-csharp[](areas/samples/MVCareas/StartupMapAreaRoute.cs?name=snippet&highlight=18-27)]

<span data-ttu-id="81266-158">Korzystając z `MapAreaRoute` z ASP.NET Core 2,2, zobacz [ten problem](https://github.com/aspnet/AspNetCore/issues/7772)w usłudze GitHub.</span><span class="sxs-lookup"><span data-stu-id="81266-158">When using `MapAreaRoute` with ASP.NET Core 2.2, see [this GitHub issue](https://github.com/aspnet/AspNetCore/issues/7772).</span></span>

<span data-ttu-id="81266-159">Aby uzyskać więcej informacji, zobacz [Routing obszaru](xref:mvc/controllers/routing#areas).</span><span class="sxs-lookup"><span data-stu-id="81266-159">For more information, see [Area routing](xref:mvc/controllers/routing#areas).</span></span>

### <a name="link-generation-with-mvc-areas"></a><span data-ttu-id="81266-160">Generowanie linków z obszarami MVC</span><span class="sxs-lookup"><span data-stu-id="81266-160">Link generation with MVC areas</span></span>

<span data-ttu-id="81266-161">Poniższy kod z pobranego [przykładu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/areas/samples) pokazuje generowanie linków z określonym obszarem:</span><span class="sxs-lookup"><span data-stu-id="81266-161">The following code from the [sample download](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/areas/samples) shows link generation with the area specified:</span></span>

[!code-cshtml[](areas/samples/MVCareas/Views/Shared/_testLinksPartial.cshtml?name=snippet)]

<span data-ttu-id="81266-162">Linki generowane za pomocą powyższego kodu są prawidłowe w dowolnym miejscu aplikacji.</span><span class="sxs-lookup"><span data-stu-id="81266-162">The links generated with the preceding code are valid anywhere in the app.</span></span>

<span data-ttu-id="81266-163">Pobieranie próbek obejmuje [Widok częściowy](xref:mvc/views/partial) zawierający poprzednie linki i te same linki bez określania obszaru.</span><span class="sxs-lookup"><span data-stu-id="81266-163">The sample download includes a [partial view](xref:mvc/views/partial) that contains the preceding links and the same links without specifying the area.</span></span> <span data-ttu-id="81266-164">Widok częściowy jest przywoływany w [pliku układu](xref:mvc/views/layout), więc każda Strona w aplikacji wyświetla wygenerowane linki.</span><span class="sxs-lookup"><span data-stu-id="81266-164">The partial view is referenced in the [layout file](xref:mvc/views/layout), so every page in the app displays the generated links.</span></span> <span data-ttu-id="81266-165">Linki wygenerowane bez określenia obszaru są prawidłowe tylko wtedy, gdy jest przywoływany ze strony w tym samym obszarze i kontrolerze.</span><span class="sxs-lookup"><span data-stu-id="81266-165">The links generated without specifying the area are only valid when referenced from a page in the same area and controller.</span></span>

<span data-ttu-id="81266-166">Gdy nie określono obszaru lub kontrolera, routing zależy od wartości *otoczenia* .</span><span class="sxs-lookup"><span data-stu-id="81266-166">When the area or controller is not specified, routing depends on the *ambient* values.</span></span> <span data-ttu-id="81266-167">Bieżące wartości trasy bieżącego żądania są uznawane za wartości otoczenia dla generacji łącza.</span><span class="sxs-lookup"><span data-stu-id="81266-167">The current route values of the current request are considered ambient values for link generation.</span></span> <span data-ttu-id="81266-168">W wielu przypadkach dla przykładowej aplikacji korzystanie z wartości otoczenia powoduje wygenerowanie nieprawidłowych linków.</span><span class="sxs-lookup"><span data-stu-id="81266-168">In many cases for the sample app, using the ambient values generates incorrect links.</span></span>

<span data-ttu-id="81266-169">Aby uzyskać więcej informacji, zobacz [routing do kontrolera akcji](xref:mvc/controllers/routing).</span><span class="sxs-lookup"><span data-stu-id="81266-169">For more information, see [Routing to controller actions](xref:mvc/controllers/routing).</span></span>

### <a name="shared-layout-for-areas-using-the-_viewstartcshtml-file"></a><span data-ttu-id="81266-170">Układ współużytkowany dla obszarów korzystających z pliku _ViewStart. cshtml</span><span class="sxs-lookup"><span data-stu-id="81266-170">Shared layout for Areas using the _ViewStart.cshtml file</span></span>

<span data-ttu-id="81266-171">Aby udostępnić wspólny układ całej aplikacji, Przenieś *_ViewStart. cshtml* do folderu głównego aplikacji.</span><span class="sxs-lookup"><span data-stu-id="81266-171">To share a common layout for the entire app, move the *_ViewStart.cshtml* to the application root folder.</span></span>

### <a name="_viewimportscshtml"></a><span data-ttu-id="81266-172">_ViewImports. cshtml</span><span class="sxs-lookup"><span data-stu-id="81266-172">_ViewImports.cshtml</span></span>

<span data-ttu-id="81266-173">W swojej lokalizacji standardowej */Views/_ViewImports. cshtml* nie ma zastosowania do obszarów.</span><span class="sxs-lookup"><span data-stu-id="81266-173">In its standard location, */Views/_ViewImports.cshtml* doesn't apply to areas.</span></span> <span data-ttu-id="81266-174">Aby użyć wspólnych [pomocników tagów](xref:mvc/views/tag-helpers/intro), `@using`lub `@inject` w Twoim regionie, upewnij się, że odpowiedni plik *_ViewImports. cshtml* [ma zastosowanie do widoków obszaru](xref:mvc/views/layout#importing-shared-directives).</span><span class="sxs-lookup"><span data-stu-id="81266-174">To use common [Tag Helpers](xref:mvc/views/tag-helpers/intro), `@using`, or `@inject` in your area, ensure a proper *_ViewImports.cshtml* file [applies to your area views](xref:mvc/views/layout#importing-shared-directives).</span></span> <span data-ttu-id="81266-175">Jeśli chcesz mieć takie samo zachowanie we wszystkich widokach, Przenieś */Views/_ViewImports. cshtml* do katalogu głównego aplikacji.</span><span class="sxs-lookup"><span data-stu-id="81266-175">If you want the same behavior in all your views, move */Views/_ViewImports.cshtml* to the application root.</span></span>

<a name="rename"></a>

### <a name="change-default-area-folder-where-views-are-stored"></a><span data-ttu-id="81266-176">Zmień domyślny folder obszaru, w którym są przechowywane widoki</span><span class="sxs-lookup"><span data-stu-id="81266-176">Change default area folder where views are stored</span></span>

<span data-ttu-id="81266-177">Poniższy kod zmienia domyślny folder obszaru z `"Areas"` na `"MyAreas"`:</span><span class="sxs-lookup"><span data-stu-id="81266-177">The following code changes the default area folder from `"Areas"` to `"MyAreas"`:</span></span>

[!code-csharp[](areas/samples/MVCareas/Startup2.cs?name=snippet)]

<a name="arp"></a>

## <a name="areas-with-razor-pages"></a><span data-ttu-id="81266-178">Obszary z Razor Pages</span><span class="sxs-lookup"><span data-stu-id="81266-178">Areas with Razor Pages</span></span>

<span data-ttu-id="81266-179">Obszary z Razor Pages wymagają folderu *Areas/<area name>/Pages* w katalogu głównym aplikacji.</span><span class="sxs-lookup"><span data-stu-id="81266-179">Areas with Razor Pages require an *Areas/<area name>/Pages* folder in the root of the app.</span></span> <span data-ttu-id="81266-180">Następująca struktura folderów jest używana z [przykładową aplikacją](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/areas/samples):</span><span class="sxs-lookup"><span data-stu-id="81266-180">The following folder structure is used with the [sample app](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/areas/samples):</span></span>

* <span data-ttu-id="81266-181">Project name (Nazwa projektu)</span><span class="sxs-lookup"><span data-stu-id="81266-181">Project name</span></span>
  * <span data-ttu-id="81266-182">Obszary</span><span class="sxs-lookup"><span data-stu-id="81266-182">Areas</span></span>
    * <span data-ttu-id="81266-183">Produkty</span><span class="sxs-lookup"><span data-stu-id="81266-183">Products</span></span>
      * <span data-ttu-id="81266-184">Strony</span><span class="sxs-lookup"><span data-stu-id="81266-184">Pages</span></span>
        * <span data-ttu-id="81266-185">_ViewImports</span><span class="sxs-lookup"><span data-stu-id="81266-185">_ViewImports</span></span>
        * <span data-ttu-id="81266-186">— informacje</span><span class="sxs-lookup"><span data-stu-id="81266-186">About</span></span>
        * <span data-ttu-id="81266-187">Indeks</span><span class="sxs-lookup"><span data-stu-id="81266-187">Index</span></span>
    * <span data-ttu-id="81266-188">Usługi</span><span class="sxs-lookup"><span data-stu-id="81266-188">Services</span></span>
      * <span data-ttu-id="81266-189">Strony</span><span class="sxs-lookup"><span data-stu-id="81266-189">Pages</span></span>
        * <span data-ttu-id="81266-190">Zarządzanie</span><span class="sxs-lookup"><span data-stu-id="81266-190">Manage</span></span>
          * <span data-ttu-id="81266-191">— informacje</span><span class="sxs-lookup"><span data-stu-id="81266-191">About</span></span>
          * <span data-ttu-id="81266-192">Indeks</span><span class="sxs-lookup"><span data-stu-id="81266-192">Index</span></span>

### <a name="link-generation-with-razor-pages-and-areas"></a><span data-ttu-id="81266-193">Generowanie linków przy użyciu Razor Pages i obszarów</span><span class="sxs-lookup"><span data-stu-id="81266-193">Link generation with Razor Pages and areas</span></span>

<span data-ttu-id="81266-194">Poniższy kod z pobranego [przykładu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/areas/samples/RPareas) pokazuje Generowanie łącza z określonym obszarem (na przykład `asp-area="Products"`):</span><span class="sxs-lookup"><span data-stu-id="81266-194">The following code from the [sample download](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/areas/samples/RPareas) shows link generation with the area specified (for example, `asp-area="Products"`):</span></span>

[!code-cshtml[](areas/samples/RPareas/Pages/Shared/_testLinksPartial.cshtml?name=snippet)]

<span data-ttu-id="81266-195">Linki generowane za pomocą powyższego kodu są prawidłowe w dowolnym miejscu aplikacji.</span><span class="sxs-lookup"><span data-stu-id="81266-195">The links generated with the preceding code are valid anywhere in the app.</span></span>

<span data-ttu-id="81266-196">Pobieranie próbek obejmuje [Widok częściowy](xref:mvc/views/partial) zawierający poprzednie linki i te same linki bez określania obszaru.</span><span class="sxs-lookup"><span data-stu-id="81266-196">The sample download includes a [partial view](xref:mvc/views/partial) that contains the preceding links and the same links without specifying the area.</span></span> <span data-ttu-id="81266-197">Widok częściowy jest przywoływany w [pliku układu](xref:mvc/views/layout), więc każda Strona w aplikacji wyświetla wygenerowane linki.</span><span class="sxs-lookup"><span data-stu-id="81266-197">The partial view is referenced in the [layout file](xref:mvc/views/layout), so every page in the app displays the generated links.</span></span> <span data-ttu-id="81266-198">Linki wygenerowane bez określenia obszaru są prawidłowe tylko wtedy, gdy są przywoływane ze strony w tym samym obszarze.</span><span class="sxs-lookup"><span data-stu-id="81266-198">The links generated without specifying the area are only valid when referenced from a page in the same area.</span></span>

<span data-ttu-id="81266-199">Gdy obszar nie zostanie określony, routing zależy od wartości *otoczenia* .</span><span class="sxs-lookup"><span data-stu-id="81266-199">When the area is not specified, routing depends on the *ambient* values.</span></span> <span data-ttu-id="81266-200">Bieżące wartości trasy bieżącego żądania są uznawane za wartości otoczenia dla generacji łącza.</span><span class="sxs-lookup"><span data-stu-id="81266-200">The current route values of the current request are considered ambient values for link generation.</span></span> <span data-ttu-id="81266-201">W wielu przypadkach dla przykładowej aplikacji korzystanie z wartości otoczenia powoduje wygenerowanie nieprawidłowych linków.</span><span class="sxs-lookup"><span data-stu-id="81266-201">In many cases for the sample app, using the ambient values generates incorrect links.</span></span> <span data-ttu-id="81266-202">Rozważmy na przykład linki wygenerowane na podstawie następującego kodu:</span><span class="sxs-lookup"><span data-stu-id="81266-202">For example, consider the links generated from the following code:</span></span>

[!code-cshtml[](areas/samples/RPareas/Pages/Shared/_testLinksPartial.cshtml?name=snippet2)]

<span data-ttu-id="81266-203">Dla poprzedniego kodu:</span><span class="sxs-lookup"><span data-stu-id="81266-203">For the preceding code:</span></span>

* <span data-ttu-id="81266-204">Link wygenerowany z `<a asp-page="/Manage/About">` jest prawidłowy tylko wtedy, gdy ostatnie żądanie dotyczyło strony w obszarze `Services`.</span><span class="sxs-lookup"><span data-stu-id="81266-204">The link generated from `<a asp-page="/Manage/About">` is correct only when the last request was for a page in `Services` area.</span></span> <span data-ttu-id="81266-205">Na przykład `/Services/Manage/`, `/Services/Manage/Index`lub `/Services/Manage/About`.</span><span class="sxs-lookup"><span data-stu-id="81266-205">For example, `/Services/Manage/`, `/Services/Manage/Index`, or `/Services/Manage/About`.</span></span>
* <span data-ttu-id="81266-206">Link wygenerowany z `<a asp-page="/About">` jest prawidłowy tylko wtedy, gdy ostatnie żądanie dotyczyło strony w `/Home`.</span><span class="sxs-lookup"><span data-stu-id="81266-206">The link generated from `<a asp-page="/About">` is correct only when the last request was for a page in `/Home`.</span></span>
* <span data-ttu-id="81266-207">Kod pochodzi z pobranego [przykładu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/areas/samples/RPareas).</span><span class="sxs-lookup"><span data-stu-id="81266-207">The code is from the [sample download](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/areas/samples/RPareas).</span></span>

### <a name="import-namespace-and-tag-helpers-with-_viewimports-file"></a><span data-ttu-id="81266-208">Importowanie przestrzeni nazw i pomocników tagów przy użyciu pliku _ViewImports</span><span class="sxs-lookup"><span data-stu-id="81266-208">Import namespace and Tag Helpers with _ViewImports file</span></span>

<span data-ttu-id="81266-209">Plik *_ViewImports. cshtml* można dodać do każdego folderu *stron* obszaru, aby zaimportować przestrzeń nazw i Tagi pomocników do każdej strony Razor w folderze.</span><span class="sxs-lookup"><span data-stu-id="81266-209">A *_ViewImports.cshtml* file can be added to each area *Pages* folder to import the namespace and Tag Helpers to each Razor Page in the folder.</span></span>

<span data-ttu-id="81266-210">Weź pod uwagę obszar *usług* przykładowego kodu, który nie zawiera pliku *_ViewImports. cshtml* .</span><span class="sxs-lookup"><span data-stu-id="81266-210">Consider the *Services* area of the sample code, which doesn't contain a *_ViewImports.cshtml* file.</span></span> <span data-ttu-id="81266-211">Następujące znaczniki przedstawiają stronę */Services/Manage/about* Razor:</span><span class="sxs-lookup"><span data-stu-id="81266-211">The following markup shows the */Services/Manage/About* Razor Page:</span></span>

[!code-cshtml[](areas/samples/RPareas/Areas/Services/Pages/Manage/About.cshtml)]

<span data-ttu-id="81266-212">W powyższym znaczniku:</span><span class="sxs-lookup"><span data-stu-id="81266-212">In the preceding markup:</span></span>

* <span data-ttu-id="81266-213">W pełni kwalifikowana nazwa domeny musi zostać użyta do określenia modelu (`@model RPareas.Areas.Services.Pages.Manage.AboutModel`).</span><span class="sxs-lookup"><span data-stu-id="81266-213">The fully qualified domain name must be used to specify the model (`@model RPareas.Areas.Services.Pages.Manage.AboutModel`).</span></span>
* <span data-ttu-id="81266-214">[Pomocnicy tagów](xref:mvc/views/tag-helpers/intro) są włączane przez `@addTagHelper *, Microsoft.AspNetCore.Mvc.TagHelpers`</span><span class="sxs-lookup"><span data-stu-id="81266-214">[Tag Helpers](xref:mvc/views/tag-helpers/intro) are enabled by `@addTagHelper *, Microsoft.AspNetCore.Mvc.TagHelpers`</span></span>

<span data-ttu-id="81266-215">W przykładowym pobieranym obszarze produkty znajdują się następujące *_ViewImports. cshtml* :</span><span class="sxs-lookup"><span data-stu-id="81266-215">In the sample download, the Products area contains the following *_ViewImports.cshtml* file:</span></span>

[!code-cshtml[](areas/samples/RPareas/Areas/Products/Pages/_ViewImports.cshtml)]

<span data-ttu-id="81266-216">Następujące znaczniki przedstawiają stronę */Products/about* Razor:</span><span class="sxs-lookup"><span data-stu-id="81266-216">The following markup shows the */Products/About* Razor Page:</span></span>

[!code-cshtml[](areas/samples/RPareas/Areas/Products/Pages/About.cshtml)]

<span data-ttu-id="81266-217">W poprzednim pliku, przestrzeń nazw i dyrektywa `@addTagHelper` są importowane do pliku przez *obszary/produkty/strony/_ViewImports. cshtml* .</span><span class="sxs-lookup"><span data-stu-id="81266-217">In the preceding file, the namespace and `@addTagHelper` directive is imported to the file by the *Areas/Products/Pages/_ViewImports.cshtml* file.</span></span>

<span data-ttu-id="81266-218">Aby uzyskać więcej informacji, zobacz [Zarządzanie zakresem pomocnika tagów](xref:mvc/views/tag-helpers/intro?view=aspnetcore-2.2#managing-tag-helper-scope) i [Importowanie wspólnych dyrektyw](xref:mvc/views/layout#importing-shared-directives).</span><span class="sxs-lookup"><span data-stu-id="81266-218">For more information, see [Managing Tag Helper scope](xref:mvc/views/tag-helpers/intro?view=aspnetcore-2.2#managing-tag-helper-scope) and [Importing Shared Directives](xref:mvc/views/layout#importing-shared-directives).</span></span>

### <a name="shared-layout-for-razor-pages-areas"></a><span data-ttu-id="81266-219">Układ współużytkowany dla obszarów Razor Pages</span><span class="sxs-lookup"><span data-stu-id="81266-219">Shared layout for Razor Pages Areas</span></span>

<span data-ttu-id="81266-220">Aby udostępnić wspólny układ całej aplikacji, Przenieś *_ViewStart. cshtml* do folderu głównego aplikacji.</span><span class="sxs-lookup"><span data-stu-id="81266-220">To share a common layout for the entire app, move the *_ViewStart.cshtml* to the application root folder.</span></span>

### <a name="publishing-areas"></a><span data-ttu-id="81266-221">Publikowanie obszarów</span><span class="sxs-lookup"><span data-stu-id="81266-221">Publishing Areas</span></span>

<span data-ttu-id="81266-222">Wszystkie pliki \*. cshtml i pliki znajdujące się w katalogu *wwwroot* są publikowane w danych wyjściowych, gdy `<Project Sdk="Microsoft.NET.Sdk.Web">` zostanie uwzględniony w pliku \*. csproj.</span><span class="sxs-lookup"><span data-stu-id="81266-222">All \*.cshtml files and files within the *wwwroot* directory are published to output when `<Project Sdk="Microsoft.NET.Sdk.Web">` is included in the \*.csproj file.</span></span>
