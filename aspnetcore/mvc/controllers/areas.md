---
title: Obszary w ASP.NET Core
author: rick-anderson
description: Dowiedz się, jak obszary są funkcją ASP.NET MVC służącą do organizowania powiązanych funkcji w grupie jako oddzielnej przestrzeni nazw (dla routingu) i struktury folderów (dla widoków).
ms.author: riande
ms.date: 08/16/2019
uid: mvc/controllers/areas
ms.openlocfilehash: d0af3092776ee09469c879fffd3047c50b1a59b4
ms.sourcegitcommit: 4cb0c7e74355f2e87c60e2a196f842b937247a99
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/16/2019
ms.locfileid: "69545806"
---
# <a name="areas-in-aspnet-core"></a><span data-ttu-id="ca222-103">Obszary w ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="ca222-103">Areas in ASP.NET Core</span></span>

<span data-ttu-id="ca222-104">Autorzy [Dhananjay Kumara](https://twitter.com/debug_mode) i [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="ca222-104">By [Dhananjay Kumar](https://twitter.com/debug_mode) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="ca222-105">Obszary są funkcją ASP.NET używaną do organizowania powiązanych funkcji w grupie jako oddzielnej przestrzeni nazw (dla routingu) i struktury folderów (dla widoków).</span><span class="sxs-lookup"><span data-stu-id="ca222-105">Areas are an ASP.NET feature used to organize related functionality into a group as a separate namespace (for routing) and folder structure (for views).</span></span> <span data-ttu-id="ca222-106">Za pomocą obszarów tworzy hierarchię na potrzeby routingu przez dodanie innego parametru `area`trasy, do `controller` i `action` lub do strony `page`Razor.</span><span class="sxs-lookup"><span data-stu-id="ca222-106">Using areas creates a hierarchy for the purpose of routing by adding another route parameter, `area`, to `controller` and `action` or a Razor Page `page`.</span></span>

<span data-ttu-id="ca222-107">Obszary umożliwiają dzielenie aplikacji sieci Web na ASP.NET Core na mniejsze grupy funkcjonalne, z których każdy ma swój własny zestaw Razor Pages, kontrolerów, widoków i modeli.</span><span class="sxs-lookup"><span data-stu-id="ca222-107">Areas provide a way to partition an ASP.NET Core Web app into smaller functional groups, each  with its own set of Razor Pages, controllers, views, and models.</span></span> <span data-ttu-id="ca222-108">Obszar jest efektywnie strukturą wewnątrz aplikacji.</span><span class="sxs-lookup"><span data-stu-id="ca222-108">An area is effectively a structure inside an app.</span></span> <span data-ttu-id="ca222-109">W projekcie sieci Web ASP.NET Core składniki logiczne, takie jak Pages, model, Controller i View, są przechowywane w różnych folderach.</span><span class="sxs-lookup"><span data-stu-id="ca222-109">In an ASP.NET Core web project, logical components like Pages, Model, Controller, and View are kept in different folders.</span></span> <span data-ttu-id="ca222-110">Środowisko uruchomieniowe ASP.NET Core używa konwencji nazewnictwa, aby utworzyć relację między tymi składnikami.</span><span class="sxs-lookup"><span data-stu-id="ca222-110">The ASP.NET Core runtime uses naming conventions to create the relationship between these components.</span></span> <span data-ttu-id="ca222-111">W przypadku dużej aplikacji warto podzielić aplikację na oddzielne obszary wysokiego poziomu funkcji.</span><span class="sxs-lookup"><span data-stu-id="ca222-111">For a large app, it may be advantageous to partition the app into separate high level areas of functionality.</span></span> <span data-ttu-id="ca222-112">Na przykład aplikacja handlu elektronicznego z wieloma jednostkami biznesowymi, takimi jak wyewidencjonowywanie, rozliczenia i wyszukiwanie.</span><span class="sxs-lookup"><span data-stu-id="ca222-112">For instance, an e-commerce app with multiple business units, such as checkout, billing, and search.</span></span> <span data-ttu-id="ca222-113">Każda z tych jednostek ma własny obszar, który zawiera widoki, kontrolery, Razor Pages i modele.</span><span class="sxs-lookup"><span data-stu-id="ca222-113">Each of these units have their own area to contain views, controllers, Razor Pages, and models.</span></span>

<span data-ttu-id="ca222-114">Rozważ użycie obszarów w projekcie, gdy:</span><span class="sxs-lookup"><span data-stu-id="ca222-114">Consider using Areas in a project when:</span></span>

* <span data-ttu-id="ca222-115">Aplikacja składa się z wielu składników funkcjonalnych wysokiego poziomu, które można logicznie oddzielić.</span><span class="sxs-lookup"><span data-stu-id="ca222-115">The app is made of multiple high-level functional components that can be logically separated.</span></span>
* <span data-ttu-id="ca222-116">Chcesz podzielić aplikację na partycje, tak aby każdy obszar funkcjonalny mógł działać niezależnie.</span><span class="sxs-lookup"><span data-stu-id="ca222-116">You want to partition the app so that each functional area can be worked on independently.</span></span>

<span data-ttu-id="ca222-117">[Wyświetlanie lub Pobieranie przykładowego kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/areas/samples) ([jak pobrać](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="ca222-117">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/areas/samples) ([how to download](xref:index#how-to-download-a-sample)).</span></span> <span data-ttu-id="ca222-118">Przykład pobierania zawiera podstawową aplikację do testowania obszarów.</span><span class="sxs-lookup"><span data-stu-id="ca222-118">The download sample provides a basic app for testing areas.</span></span>

<span data-ttu-id="ca222-119">Jeśli używasz Razor Pages, zobacz [obszary z Razor Pages](#areas-with-razor-pages) w tym dokumencie.</span><span class="sxs-lookup"><span data-stu-id="ca222-119">If you're using Razor Pages, see [Areas with Razor Pages](#areas-with-razor-pages) in this document.</span></span>

## <a name="areas-for-controllers-with-views"></a><span data-ttu-id="ca222-120">Obszary dla kontrolerów z widokami</span><span class="sxs-lookup"><span data-stu-id="ca222-120">Areas for controllers with views</span></span>

<span data-ttu-id="ca222-121">Typowy ASP.NET Core aplikacja internetowa korzystająca z obszarów, kontrolerów i widoków zawiera następujące elementy:</span><span class="sxs-lookup"><span data-stu-id="ca222-121">A typical ASP.NET Core web app using areas, controllers, and views contains the following:</span></span>

* <span data-ttu-id="ca222-122">[Struktura folderów obszaru](#area-folder-structure).</span><span class="sxs-lookup"><span data-stu-id="ca222-122">An [Area folder structure](#area-folder-structure).</span></span>
* <span data-ttu-id="ca222-123">Kontrolery uzupełnione o [ &lbrack;atrybut&rbrack; obszaru](#attribute) , aby skojarzyć kontroler z obszarem:</span><span class="sxs-lookup"><span data-stu-id="ca222-123">Controllers decorated with the [&lbrack;Area&rbrack;](#attribute) attribute to associate the controller with the area:</span></span>

  [!code-csharp[](areas/samples/MVCareas/Areas/Products/Controllers/ManageController.cs?name=snippet2)]

* <span data-ttu-id="ca222-124">[Trasa obszaru dodana do uruchamiania](#add-area-route):</span><span class="sxs-lookup"><span data-stu-id="ca222-124">The [area route added to startup](#add-area-route):</span></span>

  [!code-csharp[](areas/samples/MVCareas/Startup.cs?name=snippet2&highlight=3-6)]

### <a name="area-folder-structure"></a><span data-ttu-id="ca222-125">Struktura folderów obszaru</span><span class="sxs-lookup"><span data-stu-id="ca222-125">Area folder structure</span></span>

<span data-ttu-id="ca222-126">Weź pod uwagę aplikację, która ma dwie grupy logiczne, *produkty* i *usługi*.</span><span class="sxs-lookup"><span data-stu-id="ca222-126">Consider an app that has two logical groups, *Products* and *Services*.</span></span> <span data-ttu-id="ca222-127">Przy użyciu obszarów struktura folderów będzie wyglądać podobnie do następujących:</span><span class="sxs-lookup"><span data-stu-id="ca222-127">Using areas, the folder structure would be similar to the following:</span></span>

* <span data-ttu-id="ca222-128">Project name (Nazwa projektu)</span><span class="sxs-lookup"><span data-stu-id="ca222-128">Project name</span></span>
  * <span data-ttu-id="ca222-129">Obszary</span><span class="sxs-lookup"><span data-stu-id="ca222-129">Areas</span></span>
    * <span data-ttu-id="ca222-130">Produkty</span><span class="sxs-lookup"><span data-stu-id="ca222-130">Products</span></span>
      * <span data-ttu-id="ca222-131">Kontrolery</span><span class="sxs-lookup"><span data-stu-id="ca222-131">Controllers</span></span>
        * <span data-ttu-id="ca222-132">HomeController.cs</span><span class="sxs-lookup"><span data-stu-id="ca222-132">HomeController.cs</span></span>
        * <span data-ttu-id="ca222-133">ManageController.cs</span><span class="sxs-lookup"><span data-stu-id="ca222-133">ManageController.cs</span></span>
      * <span data-ttu-id="ca222-134">Widoki</span><span class="sxs-lookup"><span data-stu-id="ca222-134">Views</span></span>
        * <span data-ttu-id="ca222-135">Home</span><span class="sxs-lookup"><span data-stu-id="ca222-135">Home</span></span>
          * <span data-ttu-id="ca222-136">Index.cshtml</span><span class="sxs-lookup"><span data-stu-id="ca222-136">Index.cshtml</span></span>
        * <span data-ttu-id="ca222-137">Zarządzanie</span><span class="sxs-lookup"><span data-stu-id="ca222-137">Manage</span></span>
          * <span data-ttu-id="ca222-138">Index.cshtml</span><span class="sxs-lookup"><span data-stu-id="ca222-138">Index.cshtml</span></span>
          * <span data-ttu-id="ca222-139">About. cshtml</span><span class="sxs-lookup"><span data-stu-id="ca222-139">About.cshtml</span></span>
    * <span data-ttu-id="ca222-140">Usługi</span><span class="sxs-lookup"><span data-stu-id="ca222-140">Services</span></span>
      * <span data-ttu-id="ca222-141">Kontrolery</span><span class="sxs-lookup"><span data-stu-id="ca222-141">Controllers</span></span>
        * <span data-ttu-id="ca222-142">HomeController.cs</span><span class="sxs-lookup"><span data-stu-id="ca222-142">HomeController.cs</span></span>
      * <span data-ttu-id="ca222-143">Widoki</span><span class="sxs-lookup"><span data-stu-id="ca222-143">Views</span></span>
        * <span data-ttu-id="ca222-144">Home</span><span class="sxs-lookup"><span data-stu-id="ca222-144">Home</span></span>
          * <span data-ttu-id="ca222-145">Index.cshtml</span><span class="sxs-lookup"><span data-stu-id="ca222-145">Index.cshtml</span></span>

<span data-ttu-id="ca222-146">Chociaż poprzedni układ jest typowy w przypadku korzystania z obszarów, do korzystania z tej struktury folderów są wymagane tylko pliki widoku.</span><span class="sxs-lookup"><span data-stu-id="ca222-146">While the preceding layout is typical when using Areas, only the view files are required to use this folder structure.</span></span> <span data-ttu-id="ca222-147">Wyświetl wyszukiwania odnajdywania dla zgodnego pliku widoku obszaru w następującej kolejności:</span><span class="sxs-lookup"><span data-stu-id="ca222-147">View discovery searches for a matching area view file in the following order:</span></span>

```text
/Areas/<Area-Name>/Views/<Controller-Name>/<Action-Name>.cshtml
/Areas/<Area-Name>/Views/Shared/<Action-Name>.cshtml
/Views/Shared/<Action-Name>.cshtml
/Pages/Shared/<Action-Name>.cshtml
   ```

<span data-ttu-id="ca222-148">Lokalizacja folderów niewyświetlanych, takich jak *Kontrolery* i *modele* , nie ma znaczenia.</span><span class="sxs-lookup"><span data-stu-id="ca222-148">The location of non-view folders like *Controllers* and *Models* does **not** matter.</span></span> <span data-ttu-id="ca222-149">Na przykład folder *Kontrolery* i *modele* nie jest wymagany.</span><span class="sxs-lookup"><span data-stu-id="ca222-149">For example, the *Controllers* and *Models* folder are not required.</span></span> <span data-ttu-id="ca222-150">Zawartość *kontrolerów* i *modeli* to kod, który jest kompilowany do pliku dll.</span><span class="sxs-lookup"><span data-stu-id="ca222-150">The content of *Controllers* and *Models* is code which gets compiled into a .dll.</span></span> <span data-ttu-id="ca222-151">Zawartość *widoków* nie zostanie skompilowana, dopóki nie zostanie wysłane żądanie do tego widoku.</span><span class="sxs-lookup"><span data-stu-id="ca222-151">The content of the *Views* isn't compiled until a request to that view has been made.</span></span>

<a name="attribute"></a>

### <a name="associate-the-controller-with-an-area"></a><span data-ttu-id="ca222-152">Skojarz kontroler z obszarem</span><span class="sxs-lookup"><span data-stu-id="ca222-152">Associate the controller with an Area</span></span>

<span data-ttu-id="ca222-153">Kontrolery obszaru są oznaczone [ &lbrack;atrybutem obszaru&rbrack; ](xref:Microsoft.AspNetCore.Mvc.AreaAttribute) :</span><span class="sxs-lookup"><span data-stu-id="ca222-153">Area controllers are designated with the [&lbrack;Area&rbrack;](xref:Microsoft.AspNetCore.Mvc.AreaAttribute) attribute:</span></span>

[!code-csharp[](areas/samples/MVCareas/Areas/Products/Controllers/ManageController.cs?highlight=5&name=snippet)]

### <a name="add-area-route"></a><span data-ttu-id="ca222-154">Dodaj trasę obszaru</span><span class="sxs-lookup"><span data-stu-id="ca222-154">Add Area route</span></span>

<span data-ttu-id="ca222-155">Trasy obszaru zwykle używają konwencjonalnego routingu, a nie routingu atrybutu.</span><span class="sxs-lookup"><span data-stu-id="ca222-155">Area routes typically use conventional routing rather than attribute routing.</span></span> <span data-ttu-id="ca222-156">Routowanie konwencjonalne jest zależne od kolejności.</span><span class="sxs-lookup"><span data-stu-id="ca222-156">Conventional routing is order-dependent.</span></span> <span data-ttu-id="ca222-157">Ogólnie rzecz biorąc, trasy z obszarami należy umieścić wcześniej w tabeli tras, ponieważ są one bardziej specyficzne niż trasy bez obszaru.</span><span class="sxs-lookup"><span data-stu-id="ca222-157">In general, routes with areas should be placed earlier in the route table as they're more specific than routes without an area.</span></span>

<span data-ttu-id="ca222-158">`{area:...}`może służyć jako token w szablonach tras, jeśli przestrzeń adresów URL jest jednolita dla wszystkich obszarów:</span><span class="sxs-lookup"><span data-stu-id="ca222-158">`{area:...}` can be used as a token in route templates if url space is uniform across all areas:</span></span>

[!code-csharp[](areas/samples/MVCareas/Startup.cs?name=snippet&highlight=18-21)]

<span data-ttu-id="ca222-159">W poprzednim kodzie, `exists` stosuje ograniczenie, które musi być zgodne z obszarem.</span><span class="sxs-lookup"><span data-stu-id="ca222-159">In the preceding code, `exists` applies a constraint that the route must match an area.</span></span> <span data-ttu-id="ca222-160">Korzystanie `{area:...}` z programu to najmniej skomplikowany mechanizm dodawania routingu do obszarów.</span><span class="sxs-lookup"><span data-stu-id="ca222-160">Using `{area:...}` is the least complicated mechanism to adding routing to areas.</span></span>

<span data-ttu-id="ca222-161">Poniższy kod używa <xref:Microsoft.AspNetCore.Builder.MvcAreaRouteBuilderExtensions.MapAreaRoute*> do tworzenia dwóch nazwanych tras obszaru:</span><span class="sxs-lookup"><span data-stu-id="ca222-161">The following code uses <xref:Microsoft.AspNetCore.Builder.MvcAreaRouteBuilderExtensions.MapAreaRoute*> to create two named area routes:</span></span>

[!code-csharp[](areas/samples/MVCareas/StartupMapAreaRoute.cs?name=snippet&highlight=18-27)]

<span data-ttu-id="ca222-162">W przypadku `MapAreaRoute` korzystania z programu z ASP.NET Core 2,2, zobacz [ten problem](https://github.com/aspnet/AspNetCore/issues/7772)w usłudze GitHub.</span><span class="sxs-lookup"><span data-stu-id="ca222-162">When using `MapAreaRoute` with ASP.NET Core 2.2, see [this GitHub issue](https://github.com/aspnet/AspNetCore/issues/7772).</span></span>

<span data-ttu-id="ca222-163">Aby uzyskać więcej informacji, zobacz [Routing obszaru](xref:mvc/controllers/routing#areas).</span><span class="sxs-lookup"><span data-stu-id="ca222-163">For more information, see [Area routing](xref:mvc/controllers/routing#areas).</span></span>

### <a name="link-generation-with-mvc-areas"></a><span data-ttu-id="ca222-164">Generowanie linków z obszarami MVC</span><span class="sxs-lookup"><span data-stu-id="ca222-164">Link generation with MVC areas</span></span>

<span data-ttu-id="ca222-165">Poniższy kod z pobranego [przykładu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/areas/samples) pokazuje generowanie linków z określonym obszarem:</span><span class="sxs-lookup"><span data-stu-id="ca222-165">The following code from the [sample download](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/areas/samples) shows link generation with the area specified:</span></span>

[!code-cshtml[](areas/samples/MVCareas/Views/Shared/_testLinksPartial.cshtml?name=snippet)]

<span data-ttu-id="ca222-166">Linki generowane za pomocą powyższego kodu są prawidłowe w dowolnym miejscu aplikacji.</span><span class="sxs-lookup"><span data-stu-id="ca222-166">The links generated with the preceding code are valid anywhere in the app.</span></span>

<span data-ttu-id="ca222-167">Pobieranie próbek obejmuje [Widok częściowy](xref:mvc/views/partial) zawierający poprzednie linki i te same linki bez określania obszaru.</span><span class="sxs-lookup"><span data-stu-id="ca222-167">The sample download includes a [partial view](xref:mvc/views/partial) that contains the preceding links and the same links without specifying the area.</span></span> <span data-ttu-id="ca222-168">Widok częściowy jest przywoływany w [pliku układu](xref:mvc/views/layout), więc każda Strona w aplikacji wyświetla wygenerowane linki.</span><span class="sxs-lookup"><span data-stu-id="ca222-168">The partial view is referenced in the [layout file](xref:mvc/views/layout), so every page in the app displays the generated links.</span></span> <span data-ttu-id="ca222-169">Linki wygenerowane bez określenia obszaru są prawidłowe tylko wtedy, gdy jest przywoływany ze strony w tym samym obszarze i kontrolerze.</span><span class="sxs-lookup"><span data-stu-id="ca222-169">The links generated without specifying the area are only valid when referenced from a page in the same area and controller.</span></span>

<span data-ttu-id="ca222-170">Gdy nie określono obszaru lub kontrolera, routing zależy od wartości *otoczenia* .</span><span class="sxs-lookup"><span data-stu-id="ca222-170">When the area or controller is not specified, routing depends on the *ambient* values.</span></span> <span data-ttu-id="ca222-171">Bieżące wartości trasy bieżącego żądania są uznawane za wartości otoczenia dla generacji łącza.</span><span class="sxs-lookup"><span data-stu-id="ca222-171">The current route values of the current request are considered ambient values for link generation.</span></span> <span data-ttu-id="ca222-172">W wielu przypadkach dla przykładowej aplikacji korzystanie z wartości otoczenia powoduje wygenerowanie nieprawidłowych linków.</span><span class="sxs-lookup"><span data-stu-id="ca222-172">In many cases for the sample app, using the ambient values generates incorrect links.</span></span>

<span data-ttu-id="ca222-173">Aby uzyskać więcej informacji, zobacz [routing do kontrolera akcji](xref:mvc/controllers/routing).</span><span class="sxs-lookup"><span data-stu-id="ca222-173">For more information, see [Routing to controller actions](xref:mvc/controllers/routing).</span></span>

### <a name="shared-layout-for-areas-using-the-_viewstartcshtml-file"></a><span data-ttu-id="ca222-174">Układ współużytkowany dla obszarów korzystających z pliku _ViewStart. cshtml</span><span class="sxs-lookup"><span data-stu-id="ca222-174">Shared layout for Areas using the _ViewStart.cshtml file</span></span>

<span data-ttu-id="ca222-175">Aby udostępnić wspólny układ całej aplikacji, Przenieś *_ViewStart. cshtml* do folderu głównego aplikacji.</span><span class="sxs-lookup"><span data-stu-id="ca222-175">To share a common layout for the entire app, move the *_ViewStart.cshtml* to the application root folder.</span></span>

### <a name="_viewimportscshtml"></a><span data-ttu-id="ca222-176">_ViewImports. cshtml</span><span class="sxs-lookup"><span data-stu-id="ca222-176">_ViewImports.cshtml</span></span>

<span data-ttu-id="ca222-177">W swojej standardowej lokalizacji */views/_ViewImports.cshtml* nie ma zastosowania do obszarów.</span><span class="sxs-lookup"><span data-stu-id="ca222-177">In its standard location, */Views/_ViewImports.cshtml* doesn't apply to areas.</span></span> <span data-ttu-id="ca222-178">Aby użyć wspólnych [pomocników tagów](xref:mvc/views/tag-helpers/intro), `@using`lub `@inject` w Twoim regionie, upewnij się, że odpowiedni plik *_ViewImports. cshtml* [ma zastosowanie do widoków obszaru](xref:mvc/views/layout#importing-shared-directives).</span><span class="sxs-lookup"><span data-stu-id="ca222-178">To use common [Tag Helpers](xref:mvc/views/tag-helpers/intro), `@using`, or `@inject` in your area, ensure a proper *_ViewImports.cshtml* file [applies to your area views](xref:mvc/views/layout#importing-shared-directives).</span></span> <span data-ttu-id="ca222-179">Jeśli chcesz mieć takie samo zachowanie we wszystkich widokach, Przenieś */views/_ViewImports.cshtml* do katalogu głównego aplikacji.</span><span class="sxs-lookup"><span data-stu-id="ca222-179">If you want the same behavior in all your views, move */Views/_ViewImports.cshtml* to the application root.</span></span>

<a name="rename"></a>

### <a name="change-default-area-folder-where-views-are-stored"></a><span data-ttu-id="ca222-180">Zmień domyślny folder obszaru, w którym są przechowywane widoki</span><span class="sxs-lookup"><span data-stu-id="ca222-180">Change default area folder where views are stored</span></span>

<span data-ttu-id="ca222-181">Poniższy kod zmienia domyślny folder obszaru z `"Areas"` na: `"MyAreas"`</span><span class="sxs-lookup"><span data-stu-id="ca222-181">The following code changes the default area folder from `"Areas"` to `"MyAreas"`:</span></span>

[!code-csharp[](areas/samples/MVCareas/Startup2.cs?name=snippet)]

<a name="arp"></a>

## <a name="areas-with-razor-pages"></a><span data-ttu-id="ca222-182">Obszary z Razor Pages</span><span class="sxs-lookup"><span data-stu-id="ca222-182">Areas with Razor Pages</span></span>

<span data-ttu-id="ca222-183">Obszary z Razor Pages wymagają folderu *Areas<area name>//Pages* w katalogu głównym aplikacji.</span><span class="sxs-lookup"><span data-stu-id="ca222-183">Areas with Razor Pages require an *Areas/<area name>/Pages* folder in the root of the app.</span></span> <span data-ttu-id="ca222-184">Następująca struktura folderów jest używana z przykładową [aplikacją](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/areas/samples):</span><span class="sxs-lookup"><span data-stu-id="ca222-184">The following folder structure is used with the [sample app](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/areas/samples):</span></span>

* <span data-ttu-id="ca222-185">Project name (Nazwa projektu)</span><span class="sxs-lookup"><span data-stu-id="ca222-185">Project name</span></span>
  * <span data-ttu-id="ca222-186">Obszary</span><span class="sxs-lookup"><span data-stu-id="ca222-186">Areas</span></span>
    * <span data-ttu-id="ca222-187">Produkty</span><span class="sxs-lookup"><span data-stu-id="ca222-187">Products</span></span>
      * <span data-ttu-id="ca222-188">Strony</span><span class="sxs-lookup"><span data-stu-id="ca222-188">Pages</span></span>
        * <span data-ttu-id="ca222-189">_ViewImports</span><span class="sxs-lookup"><span data-stu-id="ca222-189">_ViewImports</span></span>
        * <span data-ttu-id="ca222-190">Informacje</span><span class="sxs-lookup"><span data-stu-id="ca222-190">About</span></span>
        * <span data-ttu-id="ca222-191">Indeks</span><span class="sxs-lookup"><span data-stu-id="ca222-191">Index</span></span>
    * <span data-ttu-id="ca222-192">Usługi</span><span class="sxs-lookup"><span data-stu-id="ca222-192">Services</span></span>
      * <span data-ttu-id="ca222-193">Strony</span><span class="sxs-lookup"><span data-stu-id="ca222-193">Pages</span></span>
        * <span data-ttu-id="ca222-194">Zarządzanie</span><span class="sxs-lookup"><span data-stu-id="ca222-194">Manage</span></span>
          * <span data-ttu-id="ca222-195">Informacje</span><span class="sxs-lookup"><span data-stu-id="ca222-195">About</span></span>
          * <span data-ttu-id="ca222-196">Indeks</span><span class="sxs-lookup"><span data-stu-id="ca222-196">Index</span></span>

### <a name="link-generation-with-razor-pages-and-areas"></a><span data-ttu-id="ca222-197">Generowanie linków przy użyciu Razor Pages i obszarów</span><span class="sxs-lookup"><span data-stu-id="ca222-197">Link generation with Razor Pages and areas</span></span>

<span data-ttu-id="ca222-198">Poniższy kod z pobranego [przykładu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/areas/samples/RPareas) pokazuje Generowanie łącza z określonym obszarem (na przykład `asp-area="Products"`):</span><span class="sxs-lookup"><span data-stu-id="ca222-198">The following code from the [sample download](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/areas/samples/RPareas) shows link generation with the area specified (for example, `asp-area="Products"`):</span></span>

[!code-cshtml[](areas/samples/RPareas/Pages/Shared/_testLinksPartial.cshtml?name=snippet)]

<span data-ttu-id="ca222-199">Linki generowane za pomocą powyższego kodu są prawidłowe w dowolnym miejscu aplikacji.</span><span class="sxs-lookup"><span data-stu-id="ca222-199">The links generated with the preceding code are valid anywhere in the app.</span></span>

<span data-ttu-id="ca222-200">Pobieranie próbek obejmuje [Widok częściowy](xref:mvc/views/partial) zawierający poprzednie linki i te same linki bez określania obszaru.</span><span class="sxs-lookup"><span data-stu-id="ca222-200">The sample download includes a [partial view](xref:mvc/views/partial) that contains the preceding links and the same links without specifying the area.</span></span> <span data-ttu-id="ca222-201">Widok częściowy jest przywoływany w [pliku układu](xref:mvc/views/layout), więc każda Strona w aplikacji wyświetla wygenerowane linki.</span><span class="sxs-lookup"><span data-stu-id="ca222-201">The partial view is referenced in the [layout file](xref:mvc/views/layout), so every page in the app displays the generated links.</span></span> <span data-ttu-id="ca222-202">Linki wygenerowane bez określenia obszaru są prawidłowe tylko wtedy, gdy są przywoływane ze strony w tym samym obszarze.</span><span class="sxs-lookup"><span data-stu-id="ca222-202">The links generated without specifying the area are only valid when referenced from a page in the same area.</span></span>

<span data-ttu-id="ca222-203">Gdy obszar nie zostanie określony, routing zależy od wartości *otoczenia* .</span><span class="sxs-lookup"><span data-stu-id="ca222-203">When the area is not specified, routing depends on the *ambient* values.</span></span> <span data-ttu-id="ca222-204">Bieżące wartości trasy bieżącego żądania są uznawane za wartości otoczenia dla generacji łącza.</span><span class="sxs-lookup"><span data-stu-id="ca222-204">The current route values of the current request are considered ambient values for link generation.</span></span> <span data-ttu-id="ca222-205">W wielu przypadkach dla przykładowej aplikacji korzystanie z wartości otoczenia powoduje wygenerowanie nieprawidłowych linków.</span><span class="sxs-lookup"><span data-stu-id="ca222-205">In many cases for the sample app, using the ambient values generates incorrect links.</span></span> <span data-ttu-id="ca222-206">Rozważmy na przykład linki wygenerowane na podstawie następującego kodu:</span><span class="sxs-lookup"><span data-stu-id="ca222-206">For example, consider the links generated from the following code:</span></span>

[!code-cshtml[](areas/samples/RPareas/Pages/Shared/_testLinksPartial.cshtml?name=snippet2)]

<span data-ttu-id="ca222-207">Dla poprzedniego kodu:</span><span class="sxs-lookup"><span data-stu-id="ca222-207">For the preceding code:</span></span>

* <span data-ttu-id="ca222-208">Link wygenerowany z `<a asp-page="/Manage/About">` jest prawidłowy tylko wtedy, gdy ostatnie żądanie dotyczyło strony w `Services` obszarze.</span><span class="sxs-lookup"><span data-stu-id="ca222-208">The link generated from `<a asp-page="/Manage/About">` is correct only when the last request was for a page in `Services` area.</span></span> <span data-ttu-id="ca222-209">Na przykład `/Services/Manage/` `/Services/Manage/Index`,, lub `/Services/Manage/About`.</span><span class="sxs-lookup"><span data-stu-id="ca222-209">For example, `/Services/Manage/`, `/Services/Manage/Index`, or `/Services/Manage/About`.</span></span>
* <span data-ttu-id="ca222-210">Link wygenerowany z `<a asp-page="/About">` jest prawidłowy tylko wtedy, gdy ostatnie żądanie dotyczyło strony w `/Home`.</span><span class="sxs-lookup"><span data-stu-id="ca222-210">The link generated from `<a asp-page="/About">` is correct only when the last request was for a page in `/Home`.</span></span>
* <span data-ttu-id="ca222-211">Kod pochodzi z pobranego [przykładu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/areas/samples/RPareas).</span><span class="sxs-lookup"><span data-stu-id="ca222-211">The code is from the [sample download](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/areas/samples/RPareas).</span></span>

### <a name="import-namespace-and-tag-helpers-with-_viewimports-file"></a><span data-ttu-id="ca222-212">Importowanie przestrzeni nazw i pomocników tagów przy użyciu pliku _ViewImports</span><span class="sxs-lookup"><span data-stu-id="ca222-212">Import namespace and Tag Helpers with _ViewImports file</span></span>

<span data-ttu-id="ca222-213">Plik *_ViewImports. cshtml* można dodać do każdego folderu *stron* obszaru, aby zaimportować przestrzeń nazw i Tagi pomocników do każdej strony Razor w folderze.</span><span class="sxs-lookup"><span data-stu-id="ca222-213">A *_ViewImports.cshtml* file can be added to each area *Pages* folder to import the namespace and Tag Helpers to each Razor Page in the folder.</span></span>

<span data-ttu-id="ca222-214">Weź pod uwagę obszar *usług* przykładowego kodu, który nie zawiera pliku *_ViewImports. cshtml* .</span><span class="sxs-lookup"><span data-stu-id="ca222-214">Consider the *Services* area of the sample code, which doesn't contain a *_ViewImports.cshtml* file.</span></span> <span data-ttu-id="ca222-215">Następujące znaczniki przedstawiają stronę */Services/Manage/about* Razor:</span><span class="sxs-lookup"><span data-stu-id="ca222-215">The following markup shows the */Services/Manage/About* Razor Page:</span></span>

[!code-cshtml[](areas/samples/RPareas/Areas/Services/Pages/Manage/About.cshtml)]

<span data-ttu-id="ca222-216">W powyższym znaczniku:</span><span class="sxs-lookup"><span data-stu-id="ca222-216">In the preceding markup:</span></span>

* <span data-ttu-id="ca222-217">W pełni kwalifikowana nazwa domeny musi zostać użyta do określenia modelu (`@model RPareas.Areas.Services.Pages.Manage.AboutModel`).</span><span class="sxs-lookup"><span data-stu-id="ca222-217">The fully qualified domain name must be used to specify the model (`@model RPareas.Areas.Services.Pages.Manage.AboutModel`).</span></span>
* <span data-ttu-id="ca222-218">[Pomocnicy tagów](xref:mvc/views/tag-helpers/intro) są włączani przez`@addTagHelper *, Microsoft.AspNetCore.Mvc.TagHelpers`</span><span class="sxs-lookup"><span data-stu-id="ca222-218">[Tag Helpers](xref:mvc/views/tag-helpers/intro) are enabled by `@addTagHelper *, Microsoft.AspNetCore.Mvc.TagHelpers`</span></span>

<span data-ttu-id="ca222-219">W przykładowym pobieranym obszarze produkty znajduje się następujący plik *_ViewImports. cshtml* :</span><span class="sxs-lookup"><span data-stu-id="ca222-219">In the sample download, the Products area contains the following *_ViewImports.cshtml* file:</span></span>

[!code-cshtml[](areas/samples/RPareas/Areas/Products/Pages/_ViewImports.cshtml)]

<span data-ttu-id="ca222-220">Następujące znaczniki przedstawiają stronę */Products/about* Razor:</span><span class="sxs-lookup"><span data-stu-id="ca222-220">The following markup shows the */Products/About* Razor Page:</span></span>

[!code-cshtml[](areas/samples/RPareas/Areas/Products/Pages/About.cshtml)]

<span data-ttu-id="ca222-221">W poprzednim pliku przestrzeń nazw i `@addTagHelper` dyrektywa są importowane do pliku przez *obszar/produkty/strony/_ViewImports. cshtml* .</span><span class="sxs-lookup"><span data-stu-id="ca222-221">In the preceding file, the namespace and `@addTagHelper` directive is imported to the file by the *Areas/Products/Pages/_ViewImports.cshtml* file.</span></span>

<span data-ttu-id="ca222-222">Aby uzyskać więcej informacji, zobacz [Zarządzanie zakresem pomocnika tagów](xref:mvc/views/tag-helpers/intro?view=aspnetcore-2.2#managing-tag-helper-scope) i [Importowanie wspólnych dyrektyw](xref:mvc/views/layout#importing-shared-directives).</span><span class="sxs-lookup"><span data-stu-id="ca222-222">For more information, see [Managing Tag Helper scope](xref:mvc/views/tag-helpers/intro?view=aspnetcore-2.2#managing-tag-helper-scope) and [Importing Shared Directives](xref:mvc/views/layout#importing-shared-directives).</span></span>

### <a name="shared-layout-for-razor-pages-areas"></a><span data-ttu-id="ca222-223">Układ współużytkowany dla obszarów Razor Pages</span><span class="sxs-lookup"><span data-stu-id="ca222-223">Shared layout for Razor Pages Areas</span></span>

<span data-ttu-id="ca222-224">Aby udostępnić wspólny układ całej aplikacji, Przenieś *_ViewStart. cshtml* do folderu głównego aplikacji.</span><span class="sxs-lookup"><span data-stu-id="ca222-224">To share a common layout for the entire app, move the *_ViewStart.cshtml* to the application root folder.</span></span>

### <a name="publishing-areas"></a><span data-ttu-id="ca222-225">Publikowanie obszarów</span><span class="sxs-lookup"><span data-stu-id="ca222-225">Publishing Areas</span></span>

<span data-ttu-id="ca222-226">Wszystkie pliki \*. cshtml i pliki znajdujące się w katalogu *wwwroot* są publikowane w `<Project Sdk="Microsoft.NET.Sdk.Web">` danych wyjściowych, gdy są zawarte w pliku \*. csproj.</span><span class="sxs-lookup"><span data-stu-id="ca222-226">All \*.cshtml files and files within the *wwwroot* directory are published to output when `<Project Sdk="Microsoft.NET.Sdk.Web">` is included in the \*.csproj file.</span></span>
