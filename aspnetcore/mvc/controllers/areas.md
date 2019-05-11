---
title: Obszary w programie ASP.NET Core
author: rick-anderson
description: Dowiedz się, jak obszary są funkcją programu ASP.NET MVC, używane do organizowania powiązanych funkcji do grupy jako osobne przestrzeni nazw (w przypadku routingu) i struktury ich folderów (w przypadku widoków).
ms.author: riande
ms.date: 05/10/2019
uid: mvc/controllers/areas
ms.openlocfilehash: f3a75bc307a206e43241b421f448b09011868d08
ms.sourcegitcommit: ffe3ed7921ec6c7c70abaac1d10703ec9a43374c
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/11/2019
ms.locfileid: "65535969"
---
# <a name="areas-in-aspnet-core"></a><span data-ttu-id="33f4b-103">Obszary w programie ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="33f4b-103">Areas in ASP.NET Core</span></span>

<span data-ttu-id="33f4b-104">Przez [Kumara Dhananjay](https://twitter.com/debug_mode) i [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="33f4b-104">By [Dhananjay Kumar](https://twitter.com/debug_mode) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="33f4b-105">Obszary są funkcją programu ASP.NET, używane do organizowania powiązanych funkcji do grupy jako osobne przestrzeni nazw (w przypadku routingu) i struktury ich folderów (w przypadku widoków).</span><span class="sxs-lookup"><span data-stu-id="33f4b-105">Areas are an ASP.NET feature used to organize related functionality into a group as a separate namespace (for routing) and folder structure (for views).</span></span> <span data-ttu-id="33f4b-106">Za pomocą obszarów tworzą hierarchię na potrzeby routingu, dodając innego parametru trasy, `area`, `controller` i `action` lub strony Razor `page`.</span><span class="sxs-lookup"><span data-stu-id="33f4b-106">Using areas creates a hierarchy for the purpose of routing by adding another route parameter, `area`, to `controller` and `action` or a Razor Page `page`.</span></span>

<span data-ttu-id="33f4b-107">Obszary umożliwiają partycji aplikacji sieci Web platformy ASP.NET Core na mniejsze grupy funkcjonalnej, każdy z swój własny zestaw stron Razor, kontrolerów, widoki i modele.</span><span class="sxs-lookup"><span data-stu-id="33f4b-107">Areas provide a way to partition an ASP.NET Core Web app into smaller functional groups, each  with its own set of Razor Pages, controllers, views, and models.</span></span> <span data-ttu-id="33f4b-108">Obszar skutecznie to struktura wewnątrz aplikacji.</span><span class="sxs-lookup"><span data-stu-id="33f4b-108">An area is effectively a structure inside an app.</span></span> <span data-ttu-id="33f4b-109">W projekcie sieci web platformy ASP.NET Core składników logicznych, takich jak strony, modelu, kontroler i Widok są przechowywane w różnych folderach.</span><span class="sxs-lookup"><span data-stu-id="33f4b-109">In an ASP.NET Core web project, logical components like Pages, Model, Controller, and View are kept in different folders.</span></span> <span data-ttu-id="33f4b-110">Środowisko uruchomieniowe programu ASP.NET Core używa konwencji nazewnictwa do utworzenia relacji między tymi składnikami.</span><span class="sxs-lookup"><span data-stu-id="33f4b-110">The ASP.NET Core runtime uses naming conventions to create the relationship between these components.</span></span> <span data-ttu-id="33f4b-111">W przypadku dużych aplikacji może być korzystne podzielić ją na oddzielnych wysokiego poziomu obszary funkcji.</span><span class="sxs-lookup"><span data-stu-id="33f4b-111">For a large app, it may be advantageous to partition the app into separate high level areas of functionality.</span></span> <span data-ttu-id="33f4b-112">Na przykład aplikacja handlu elektronicznego z wielu jednostek biznesowych, takich jak wyewidencjonowania, rozliczeń i wyszukiwania.</span><span class="sxs-lookup"><span data-stu-id="33f4b-112">For instance, an e-commerce app with multiple business units, such as checkout, billing, and search.</span></span> <span data-ttu-id="33f4b-113">Każda z tych jednostek ma swoje własne obszar zawiera widoki, kontrolery, stronami Razor i modeli.</span><span class="sxs-lookup"><span data-stu-id="33f4b-113">Each of these units have their own area to contain views, controllers, Razor Pages, and models.</span></span>

<span data-ttu-id="33f4b-114">Należy rozważyć użycie obszarów w projekcie po:</span><span class="sxs-lookup"><span data-stu-id="33f4b-114">Consider using Areas in a project when:</span></span>

* <span data-ttu-id="33f4b-115">Aplikacja składa się z wielu wysokiego poziomu funkcjonalności składników, które mogą zostać logicznie oddzielone.</span><span class="sxs-lookup"><span data-stu-id="33f4b-115">The app is made of multiple high-level functional components that can be logically separated.</span></span>
* <span data-ttu-id="33f4b-116">Chcesz podzielić na partycje aplikację tak, aby każdy obszar funkcjonalny może się opracowaniem niezależnie.</span><span class="sxs-lookup"><span data-stu-id="33f4b-116">You want to partition the app so that each functional area can be worked on independently.</span></span>

<span data-ttu-id="33f4b-117">[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/areas/samples) ([sposobu pobierania](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="33f4b-117">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/areas/samples) ([how to download](xref:index#how-to-download-a-sample)).</span></span> <span data-ttu-id="33f4b-118">Przykład pobierania zawiera podstawową aplikację do testowania obszarów.</span><span class="sxs-lookup"><span data-stu-id="33f4b-118">The download sample provides a basic app for testing areas.</span></span>

<span data-ttu-id="33f4b-119">Jeśli używasz stron Razor, zobacz [obszarów ze stronami Razor](#areas-with-razor-pages) w tym dokumencie.</span><span class="sxs-lookup"><span data-stu-id="33f4b-119">If you're using Razor Pages, see [Areas with Razor Pages](#areas-with-razor-pages) in this document.</span></span>

## <a name="areas-for-controllers-with-views"></a><span data-ttu-id="33f4b-120">Obszary dla kontrolerów z widokami</span><span class="sxs-lookup"><span data-stu-id="33f4b-120">Areas for controllers with views</span></span>

<span data-ttu-id="33f4b-121">Typowa aplikacja internetowa ASP.NET Core przy użyciu obszarów, widoków i kontrolerów zawiera następujące informacje:</span><span class="sxs-lookup"><span data-stu-id="33f4b-121">A typical ASP.NET Core web app using areas, controllers, and views contains the following:</span></span>

* <span data-ttu-id="33f4b-122">[Strukturę folderów obszaru](#area-folder-structure).</span><span class="sxs-lookup"><span data-stu-id="33f4b-122">An [Area folder structure](#area-folder-structure).</span></span>
* <span data-ttu-id="33f4b-123">Kontrolery ozdobione [ &lbrack;obszaru&rbrack; ](#attribute) atrybutu, aby skojarzyć kontroler z obszaru:</span><span class="sxs-lookup"><span data-stu-id="33f4b-123">Controllers decorated with the [&lbrack;Area&rbrack;](#attribute) attribute to associate the controller with the area:</span></span>

  [!code-csharp[](areas/samples/MVCareas/Areas/Products/Controllers/ManageController.cs?name=snippet2)]

* <span data-ttu-id="33f4b-124">[Trasa obszaru dodana do uruchamiania](#add-area-route):</span><span class="sxs-lookup"><span data-stu-id="33f4b-124">The [area route added to startup](#add-area-route):</span></span>

  [!code-csharp[](areas/samples/MVCareas/Startup.cs?name=snippet2&highlight=3-6)]

### <a name="area-folder-structure"></a><span data-ttu-id="33f4b-125">Struktura folderów obszaru</span><span class="sxs-lookup"><span data-stu-id="33f4b-125">Area folder structure</span></span>

<span data-ttu-id="33f4b-126">Należy wziąć pod uwagę aplikację, która ma dwa grup logicznych *produktów* i *usług*.</span><span class="sxs-lookup"><span data-stu-id="33f4b-126">Consider an app that has two logical groups, *Products* and *Services*.</span></span> <span data-ttu-id="33f4b-127">Za pomocą obszarów, strukturę folderów będzie podobny do następującego:</span><span class="sxs-lookup"><span data-stu-id="33f4b-127">Using areas, the folder structure would be similar to the following:</span></span>

* <span data-ttu-id="33f4b-128">Project name (Nazwa projektu)</span><span class="sxs-lookup"><span data-stu-id="33f4b-128">Project name</span></span>
  * <span data-ttu-id="33f4b-129">Obszary</span><span class="sxs-lookup"><span data-stu-id="33f4b-129">Areas</span></span>
    * <span data-ttu-id="33f4b-130">Produkty</span><span class="sxs-lookup"><span data-stu-id="33f4b-130">Products</span></span>
      * <span data-ttu-id="33f4b-131">Kontrolery</span><span class="sxs-lookup"><span data-stu-id="33f4b-131">Controllers</span></span>
        * <span data-ttu-id="33f4b-132">HomeController.cs</span><span class="sxs-lookup"><span data-stu-id="33f4b-132">HomeController.cs</span></span>
        * <span data-ttu-id="33f4b-133">ManageController.cs</span><span class="sxs-lookup"><span data-stu-id="33f4b-133">ManageController.cs</span></span>
      * <span data-ttu-id="33f4b-134">Widoki</span><span class="sxs-lookup"><span data-stu-id="33f4b-134">Views</span></span>
        * <span data-ttu-id="33f4b-135">Home</span><span class="sxs-lookup"><span data-stu-id="33f4b-135">Home</span></span>
          * <span data-ttu-id="33f4b-136">Index.cshtml</span><span class="sxs-lookup"><span data-stu-id="33f4b-136">Index.cshtml</span></span>
        * <span data-ttu-id="33f4b-137">Zarządzanie</span><span class="sxs-lookup"><span data-stu-id="33f4b-137">Manage</span></span>
          * <span data-ttu-id="33f4b-138">Index.cshtml</span><span class="sxs-lookup"><span data-stu-id="33f4b-138">Index.cshtml</span></span>
          * <span data-ttu-id="33f4b-139">About.cshtml</span><span class="sxs-lookup"><span data-stu-id="33f4b-139">About.cshtml</span></span>
    * <span data-ttu-id="33f4b-140">Usługi</span><span class="sxs-lookup"><span data-stu-id="33f4b-140">Services</span></span>
      * <span data-ttu-id="33f4b-141">Kontrolery</span><span class="sxs-lookup"><span data-stu-id="33f4b-141">Controllers</span></span>
        * <span data-ttu-id="33f4b-142">HomeController.cs</span><span class="sxs-lookup"><span data-stu-id="33f4b-142">HomeController.cs</span></span>
      * <span data-ttu-id="33f4b-143">Widoki</span><span class="sxs-lookup"><span data-stu-id="33f4b-143">Views</span></span>
        * <span data-ttu-id="33f4b-144">Home</span><span class="sxs-lookup"><span data-stu-id="33f4b-144">Home</span></span>
          * <span data-ttu-id="33f4b-145">Index.cshtml</span><span class="sxs-lookup"><span data-stu-id="33f4b-145">Index.cshtml</span></span>

<span data-ttu-id="33f4b-146">Podczas poprzedniego układ jest typowe w przypadku, gdy przy użyciu obszarów, Wyświetl pliki, należy użyć tej struktury folderów.</span><span class="sxs-lookup"><span data-stu-id="33f4b-146">While the preceding layout is typical when using Areas, only the view files are required to use this folder structure.</span></span> <span data-ttu-id="33f4b-147">Widok odnajdywania wyszukuje pasującego pliku widoku obszaru w następującej kolejności:</span><span class="sxs-lookup"><span data-stu-id="33f4b-147">View discovery searches for a matching area view file in the following order:</span></span>

```text
/Areas/<Area-Name>/Views/<Controller-Name>/<Action-Name>.cshtml
/Areas/<Area-Name>/Views/Shared/<Action-Name>.cshtml
/Views/Shared/<Action-Name>.cshtml
/Pages/Shared/<Action-Name>.cshtml
   ```

<span data-ttu-id="33f4b-148">Lokalizacji-view, folderów, takie jak *kontrolerów* i *modeli* jest **nie** znaczenia.</span><span class="sxs-lookup"><span data-stu-id="33f4b-148">The location of non-view folders like *Controllers* and *Models* does **not** matter.</span></span> <span data-ttu-id="33f4b-149">Na przykład *kontrolerów* i *modeli* folderu nie są wymagane.</span><span class="sxs-lookup"><span data-stu-id="33f4b-149">For example, the *Controllers* and *Models* folder are not required.</span></span> <span data-ttu-id="33f4b-150">Zawartość *kontrolerów* i *modeli* jest kod, który zostanie skompilowany w dll.</span><span class="sxs-lookup"><span data-stu-id="33f4b-150">The content of *Controllers* and *Models* is code which gets compiled into a .dll.</span></span> <span data-ttu-id="33f4b-151">Zawartość *widoków* nie jest kompilowana, dopóki nie wykonano żądania do tego widoku.</span><span class="sxs-lookup"><span data-stu-id="33f4b-151">The content of the *Views* isn't compiled until a request to that view has been made.</span></span>

<a name="attribute"></a>

### <a name="associate-the-controller-with-an-area"></a><span data-ttu-id="33f4b-152">Skojarzyć kontroler z obszaru</span><span class="sxs-lookup"><span data-stu-id="33f4b-152">Associate the controller with an Area</span></span>

<span data-ttu-id="33f4b-153">Obszar kontrolerów zostały oznaczone za pomocą [ &lbrack;obszaru&rbrack; ](xref:Microsoft.AspNetCore.Mvc.AreaAttribute) atrybutu:</span><span class="sxs-lookup"><span data-stu-id="33f4b-153">Area controllers are designated with the [&lbrack;Area&rbrack;](xref:Microsoft.AspNetCore.Mvc.AreaAttribute) attribute:</span></span>

[!code-csharp[](areas/samples/MVCareas/Areas/Products/Controllers/ManageController.cs?highlight=5&name=snippet)]

### <a name="add-area-route"></a><span data-ttu-id="33f4b-154">Dodaj trasę obszaru</span><span class="sxs-lookup"><span data-stu-id="33f4b-154">Add Area route</span></span>

<span data-ttu-id="33f4b-155">Obszar trasy używa się zazwyczaj do routingu konwencjonalna zamiast trasowanie atrybutów.</span><span class="sxs-lookup"><span data-stu-id="33f4b-155">Area routes typically use conventional routing rather than attribute routing.</span></span> <span data-ttu-id="33f4b-156">Tradycyjnie routing jest zależna od kolejności.</span><span class="sxs-lookup"><span data-stu-id="33f4b-156">Conventional routing is order-dependent.</span></span> <span data-ttu-id="33f4b-157">Ogólnie rzecz biorąc trasy z obszarami należy umieścić we wcześniejszej części tabeli tras, ponieważ są one bardziej szczegółowe niż trasy bez obszaru.</span><span class="sxs-lookup"><span data-stu-id="33f4b-157">In general, routes with areas should be placed earlier in the route table as they're more specific than routes without an area.</span></span>

<span data-ttu-id="33f4b-158">`{area:...}` może służyć jako token w szablonach tras Jeśli przestrzeń adresów url jest jednolita we wszystkich obszarach:</span><span class="sxs-lookup"><span data-stu-id="33f4b-158">`{area:...}` can be used as a token in route templates if url space is uniform across all areas:</span></span>

[!code-csharp[](areas/samples/MVCareas/Startup.cs?name=snippet&highlight=18-21)]

<span data-ttu-id="33f4b-159">W poprzednim kodzie `exists` zastosuje ograniczenie, że trasy musi odpowiadać obszar.</span><span class="sxs-lookup"><span data-stu-id="33f4b-159">In the preceding code, `exists` applies a constraint that the route must match an area.</span></span> <span data-ttu-id="33f4b-160">Za pomocą `{area:...}` jest najmniej skomplikowane mechanizm do dodawania routingu do obszarów.</span><span class="sxs-lookup"><span data-stu-id="33f4b-160">Using `{area:...}` is the least complicated mechanism to adding routing to areas.</span></span>

<span data-ttu-id="33f4b-161">Poniższy kod używa <xref:Microsoft.AspNetCore.Builder.MvcAreaRouteBuilderExtensions.MapAreaRoute*> utworzyć dwa o nazwie obszaru trasy:</span><span class="sxs-lookup"><span data-stu-id="33f4b-161">The following code uses <xref:Microsoft.AspNetCore.Builder.MvcAreaRouteBuilderExtensions.MapAreaRoute*> to create two named area routes:</span></span>

[!code-csharp[](areas/samples/MVCareas/StartupMapAreaRoute.cs?name=snippet&highlight=18-27)]

<span data-ttu-id="33f4b-162">Korzystając z `MapAreaRoute` za pomocą platformy ASP.NET Core 2.2, zobacz [problem w usłudze GitHub](https://github.com/aspnet/AspNetCore/issues/7772).</span><span class="sxs-lookup"><span data-stu-id="33f4b-162">When using `MapAreaRoute` with ASP.NET Core 2.2, see [this GitHub issue](https://github.com/aspnet/AspNetCore/issues/7772).</span></span>

<span data-ttu-id="33f4b-163">Aby uzyskać więcej informacji, zobacz [routingu obszaru](xref:mvc/controllers/routing#areas).</span><span class="sxs-lookup"><span data-stu-id="33f4b-163">For more information, see [Area routing](xref:mvc/controllers/routing#areas).</span></span>

### <a name="link-generation-with-mvc-areas"></a><span data-ttu-id="33f4b-164">Generowanie konsolidacji z obszarami MVC</span><span class="sxs-lookup"><span data-stu-id="33f4b-164">Link generation with MVC areas</span></span>

<span data-ttu-id="33f4b-165">Poniższy kod z [pobrania próbki](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/areas/samples) pokazuje połączenie generacji z użyciem obszaru określony:</span><span class="sxs-lookup"><span data-stu-id="33f4b-165">The following code from the [sample download](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/areas/samples) shows link generation with the area specified:</span></span>

[!code-cshtml[](areas/samples/MVCareas/Views/Shared/_testLinksPartial.cshtml?name=snippet)]

<span data-ttu-id="33f4b-166">Linki wygenerowane z poprzedniego kodu są prawidłowe w dowolnym miejscu aplikacji.</span><span class="sxs-lookup"><span data-stu-id="33f4b-166">The links generated with the preceding code are valid anywhere in the app.</span></span>

<span data-ttu-id="33f4b-167">Przykładowe do pobrania obejmują programy [widoku częściowego](xref:mvc/views/partial) zawiera poprzednie linki i uruchamia łącze tak samo bez określenia obszaru.</span><span class="sxs-lookup"><span data-stu-id="33f4b-167">The sample download includes a [partial view](xref:mvc/views/partial) that contains the preceding links and the same links without specifying the area.</span></span> <span data-ttu-id="33f4b-168">Odwołuje się widok częściowy [plik układu](xref:mvc/views/layout), więc co w aplikacji stronę Wyświetla łącza wygenerowany.</span><span class="sxs-lookup"><span data-stu-id="33f4b-168">The partial view is referenced in the [layout file](xref:mvc/views/layout), so every page in the app displays the generated links.</span></span> <span data-ttu-id="33f4b-169">Linki wygenerowany bez określenia obszaru są prawidłowe tylko gdy występuje do ze strony, w tym samym regionie i kontrolera.</span><span class="sxs-lookup"><span data-stu-id="33f4b-169">The links generated without specifying the area are only valid when referenced from a page in the same area and controller.</span></span>

<span data-ttu-id="33f4b-170">Jeśli nie określono obszaru lub kontrolera, routing zależy od *otoczenia* wartości.</span><span class="sxs-lookup"><span data-stu-id="33f4b-170">When the area or controller is not specified, routing depends on the *ambient* values.</span></span> <span data-ttu-id="33f4b-171">Bieżące wartości trasy, bieżącego żądania są traktowane jako wartości otoczenia dotyczącymi generowania łączy.</span><span class="sxs-lookup"><span data-stu-id="33f4b-171">The current route values of the current request are considered ambient values for link generation.</span></span> <span data-ttu-id="33f4b-172">W wielu przypadkach dla przykładowej aplikacji przy użyciu wartości otoczenia generuje nieprawidłowe linki.</span><span class="sxs-lookup"><span data-stu-id="33f4b-172">In many cases for the sample app, using the ambient values generates incorrect links.</span></span>

<span data-ttu-id="33f4b-173">Aby uzyskać więcej informacji, zobacz [Routing do akcji kontrolera](xref:mvc/controllers/routing).</span><span class="sxs-lookup"><span data-stu-id="33f4b-173">For more information, see [Routing to controller actions](xref:mvc/controllers/routing).</span></span>

### <a name="shared-layout-for-areas-using-the-viewstartcshtml-file"></a><span data-ttu-id="33f4b-174">Udostępnione układu dla obszarów przy użyciu pliku _ViewStart.cshtml</span><span class="sxs-lookup"><span data-stu-id="33f4b-174">Shared layout for Areas using the _ViewStart.cshtml file</span></span>

<span data-ttu-id="33f4b-175">Aby udostępnić typowych układu dla całej aplikacji, należy przenieść *_ViewStart.cshtml* do folderu głównego aplikacji.</span><span class="sxs-lookup"><span data-stu-id="33f4b-175">To share a common layout for the entire app, move the *_ViewStart.cshtml* to the application root folder.</span></span>

<a name="rename"></a>

### <a name="change-default-area-folder-where-views-are-stored"></a><span data-ttu-id="33f4b-176">Zmień domyślny folder obszaru przechowywania widoków</span><span class="sxs-lookup"><span data-stu-id="33f4b-176">Change default area folder where views are stored</span></span>

<span data-ttu-id="33f4b-177">Poniższy kod zmienia domyślny folder obszaru z `"Areas"` do `"MyAreas"`:</span><span class="sxs-lookup"><span data-stu-id="33f4b-177">The following code changes the default area folder from `"Areas"` to `"MyAreas"`:</span></span>

[!code-csharp[](areas/samples/MVCareas/Startup2.cs?name=snippet)]

<a name="arp"></a>

## <a name="areas-with-razor-pages"></a><span data-ttu-id="33f4b-178">Obszary ze stron Razor</span><span class="sxs-lookup"><span data-stu-id="33f4b-178">Areas with Razor Pages</span></span>

<span data-ttu-id="33f4b-179">Wymagaj obszarów ze stronami Razor i *obszarów /&lt;nazwy obszaru&gt;/strony* folder w katalogu głównym aplikacji.</span><span class="sxs-lookup"><span data-stu-id="33f4b-179">Areas with Razor Pages require and *Areas/&lt;area name&gt;/Pages* folder in the root of the app.</span></span> <span data-ttu-id="33f4b-180">Następującą strukturę folderów jest używana z [pobrania próbki](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/areas/samples)</span><span class="sxs-lookup"><span data-stu-id="33f4b-180">The following folder structure is used with the [sample download](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/areas/samples)</span></span>

* <span data-ttu-id="33f4b-181">Project name (Nazwa projektu)</span><span class="sxs-lookup"><span data-stu-id="33f4b-181">Project name</span></span>
  * <span data-ttu-id="33f4b-182">Obszary</span><span class="sxs-lookup"><span data-stu-id="33f4b-182">Areas</span></span>
    * <span data-ttu-id="33f4b-183">Produkty</span><span class="sxs-lookup"><span data-stu-id="33f4b-183">Products</span></span>
      * <span data-ttu-id="33f4b-184">Strony</span><span class="sxs-lookup"><span data-stu-id="33f4b-184">Pages</span></span>
        * <span data-ttu-id="33f4b-185">_ViewImports</span><span class="sxs-lookup"><span data-stu-id="33f4b-185">_ViewImports</span></span>
        * <span data-ttu-id="33f4b-186">Informacje</span><span class="sxs-lookup"><span data-stu-id="33f4b-186">About</span></span>
        * <span data-ttu-id="33f4b-187">Indeks</span><span class="sxs-lookup"><span data-stu-id="33f4b-187">Index</span></span>
    * <span data-ttu-id="33f4b-188">Usługi</span><span class="sxs-lookup"><span data-stu-id="33f4b-188">Services</span></span>
      * <span data-ttu-id="33f4b-189">Strony</span><span class="sxs-lookup"><span data-stu-id="33f4b-189">Pages</span></span>
        * <span data-ttu-id="33f4b-190">Zarządzanie</span><span class="sxs-lookup"><span data-stu-id="33f4b-190">Manage</span></span>
          * <span data-ttu-id="33f4b-191">Informacje</span><span class="sxs-lookup"><span data-stu-id="33f4b-191">About</span></span>
          * <span data-ttu-id="33f4b-192">Indeks</span><span class="sxs-lookup"><span data-stu-id="33f4b-192">Index</span></span>

### <a name="link-generation-with-razor-pages-and-areas"></a><span data-ttu-id="33f4b-193">Generowanie konsolidacji ze stronami Razor i obszary</span><span class="sxs-lookup"><span data-stu-id="33f4b-193">Link generation with Razor Pages and areas</span></span>

<span data-ttu-id="33f4b-194">Poniższy kod z [pobrania próbki](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/areas/samples/RPareas) pokazuje połączenie generacji z użyciem obszaru określony (na przykład `asp-area="Products"`):</span><span class="sxs-lookup"><span data-stu-id="33f4b-194">The following code from the [sample download](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/areas/samples/RPareas) shows link generation with the area specified (for example, `asp-area="Products"`):</span></span>

[!code-cshtml[](areas/samples/RPareas/Pages/Shared/_testLinksPartial.cshtml?name=snippet)]

<span data-ttu-id="33f4b-195">Linki wygenerowane z poprzedniego kodu są prawidłowe w dowolnym miejscu aplikacji.</span><span class="sxs-lookup"><span data-stu-id="33f4b-195">The links generated with the preceding code are valid anywhere in the app.</span></span>

<span data-ttu-id="33f4b-196">Przykładowe do pobrania obejmują programy [widoku częściowego](xref:mvc/views/partial) zawiera poprzednie linki i uruchamia łącze tak samo bez określenia obszaru.</span><span class="sxs-lookup"><span data-stu-id="33f4b-196">The sample download includes a [partial view](xref:mvc/views/partial) that contains the preceding links and the same links without specifying the area.</span></span> <span data-ttu-id="33f4b-197">Odwołuje się widok częściowy [plik układu](xref:mvc/views/layout), więc co w aplikacji stronę Wyświetla łącza wygenerowany.</span><span class="sxs-lookup"><span data-stu-id="33f4b-197">The partial view is referenced in the [layout file](xref:mvc/views/layout), so every page in the app displays the generated links.</span></span> <span data-ttu-id="33f4b-198">Linki wygenerowany bez określenia obszaru są prawidłowe tylko gdy występuje do ze strony, w tym samym regionie.</span><span class="sxs-lookup"><span data-stu-id="33f4b-198">The links generated without specifying the area are only valid when referenced from a page in the same area.</span></span>

<span data-ttu-id="33f4b-199">Jeśli nie określono obszaru, routing jest zależna od *otoczenia* wartości.</span><span class="sxs-lookup"><span data-stu-id="33f4b-199">When the area is not specified, routing depends on the *ambient* values.</span></span> <span data-ttu-id="33f4b-200">Bieżące wartości trasy, bieżącego żądania są traktowane jako wartości otoczenia dotyczącymi generowania łączy.</span><span class="sxs-lookup"><span data-stu-id="33f4b-200">The current route values of the current request are considered ambient values for link generation.</span></span> <span data-ttu-id="33f4b-201">W wielu przypadkach dla przykładowej aplikacji przy użyciu wartości otoczenia generuje nieprawidłowe linki.</span><span class="sxs-lookup"><span data-stu-id="33f4b-201">In many cases for the sample app, using the ambient values generates incorrect links.</span></span> <span data-ttu-id="33f4b-202">Na przykład należy wziąć pod uwagę łącza wygenerowany na podstawie następującego kodu:</span><span class="sxs-lookup"><span data-stu-id="33f4b-202">For example, consider the links generated from the following code:</span></span>

[!code-cshtml[](areas/samples/RPareas/Pages/Shared/_testLinksPartial.cshtml?name=snippet2)]

<span data-ttu-id="33f4b-203">Dla poprzedniego kodu:</span><span class="sxs-lookup"><span data-stu-id="33f4b-203">For the preceding code:</span></span>

* <span data-ttu-id="33f4b-204">Link wygenerowany na podstawie `<a asp-page="/Manage/About">` jest prawidłowy tylko podczas ostatniego żądania został dla strony w `Services` obszaru.</span><span class="sxs-lookup"><span data-stu-id="33f4b-204">The link generated from `<a asp-page="/Manage/About">` is correct only when the last request was for a page in `Services` area.</span></span> <span data-ttu-id="33f4b-205">Na przykład `/Services/Manage/`, `/Services/Manage/Index`, lub `/Services/Manage/About`.</span><span class="sxs-lookup"><span data-stu-id="33f4b-205">For example, `/Services/Manage/`, `/Services/Manage/Index`, or `/Services/Manage/About`.</span></span>
* <span data-ttu-id="33f4b-206">Link wygenerowany na podstawie `<a asp-page="/About">` jest prawidłowy tylko podczas ostatniego żądania został dla strony w `/Home`.</span><span class="sxs-lookup"><span data-stu-id="33f4b-206">The link generated from `<a asp-page="/About">` is correct only when the last request was for a page in `/Home`.</span></span>
* <span data-ttu-id="33f4b-207">Kod pochodzi z [pobrania próbki](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/areas/samples/RPareas).</span><span class="sxs-lookup"><span data-stu-id="33f4b-207">The code is from the [sample download](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/areas/samples/RPareas).</span></span>

### <a name="import-namespace-and-tag-helpers-with-viewimports-file"></a><span data-ttu-id="33f4b-208">Importowanie przy użyciu pliku _ViewImports przestrzeni nazw i pomocnicy tagów</span><span class="sxs-lookup"><span data-stu-id="33f4b-208">Import namespace and Tag Helpers with _ViewImports file</span></span>

<span data-ttu-id="33f4b-209">A *_ViewImports.cshtml* plik może zostać dodany do każdego obszaru *stron* folderu importowania przestrzeni nazw i pomocnicy tagów do każdej strony Razor, w tym folderze.</span><span class="sxs-lookup"><span data-stu-id="33f4b-209">A *_ViewImports.cshtml* file can be added to each area *Pages* folder to import the namespace and Tag Helpers to each Razor Page in the folder.</span></span>

<span data-ttu-id="33f4b-210">Należy wziąć pod uwagę *usług* obszaru przykładowy kod, który nie zawiera *_ViewImports.cshtml* pliku.</span><span class="sxs-lookup"><span data-stu-id="33f4b-210">Consider the *Services* area of the sample code, which doesn't contain a *_ViewImports.cshtml* file.</span></span> <span data-ttu-id="33f4b-211">Ilustruje poniższy kod znaczników */Services/Manage/About* strona Razor:</span><span class="sxs-lookup"><span data-stu-id="33f4b-211">The following markup shows the */Services/Manage/About* Razor Page:</span></span>

[!code-cshtml[](areas/samples/RPareas/Areas/Services/Pages/Manage/About.cshtml)]

<span data-ttu-id="33f4b-212">W poprzednim znaczników:</span><span class="sxs-lookup"><span data-stu-id="33f4b-212">In the preceding markup:</span></span>

* <span data-ttu-id="33f4b-213">W pełni kwalifikowana nazwa domeny może służyć do określania modelu (`@model RPareas.Areas.Services.Pages.Manage.AboutModel`).</span><span class="sxs-lookup"><span data-stu-id="33f4b-213">The fully qualified domain name must be used to specify the model (`@model RPareas.Areas.Services.Pages.Manage.AboutModel`).</span></span>
* <span data-ttu-id="33f4b-214">[Pomocników tagów](xref:mvc/views/tag-helpers/intro) są włączane przez `@addTagHelper *, Microsoft.AspNetCore.Mvc.TagHelpers`</span><span class="sxs-lookup"><span data-stu-id="33f4b-214">[Tag Helpers](xref:mvc/views/tag-helpers/intro) are enabled by `@addTagHelper *, Microsoft.AspNetCore.Mvc.TagHelpers`</span></span>

<span data-ttu-id="33f4b-215">Do pobrania próbki, w obszarze produktów zawiera następujące *_ViewImports.cshtml* pliku:</span><span class="sxs-lookup"><span data-stu-id="33f4b-215">In the sample download, the Products area contains the following *_ViewImports.cshtml* file:</span></span>

[!code-cshtml[](areas/samples/RPareas/Areas/Products/Pages/_ViewImports.cshtml)]

<span data-ttu-id="33f4b-216">Ilustruje poniższy kod znaczników */produkty/o* strona Razor:</span><span class="sxs-lookup"><span data-stu-id="33f4b-216">The following markup shows the */Products/About* Razor Page:</span></span>

[!code-cshtml[](areas/samples/RPareas/Areas/Products/Pages/About.cshtml)]

<span data-ttu-id="33f4b-217">W poprzednim pliku przestrzeni nazw i `@addTagHelper` dyrektywy są importowane w pliku przez *Areas/Products/Pages/_ViewImports.cshtml* pliku.</span><span class="sxs-lookup"><span data-stu-id="33f4b-217">In the preceding file, the namespace and `@addTagHelper` directive is imported to the file by the *Areas/Products/Pages/_ViewImports.cshtml* file.</span></span>

<span data-ttu-id="33f4b-218">Aby uzyskać więcej informacji, zobacz [Zarządzanie Pomocnik tagu zakresu](xref:mvc/views/tag-helpers/intro?view=aspnetcore-2.2#managing-tag-helper-scope) i [importowania dyrektywy udostępnione](xref:mvc/views/layout#importing-shared-directives).</span><span class="sxs-lookup"><span data-stu-id="33f4b-218">For more information, see [Managing Tag Helper scope](xref:mvc/views/tag-helpers/intro?view=aspnetcore-2.2#managing-tag-helper-scope) and [Importing Shared Directives](xref:mvc/views/layout#importing-shared-directives).</span></span>

### <a name="shared-layout-for-razor-pages-areas"></a><span data-ttu-id="33f4b-219">Układ udostępnionych obszarów stron Razor</span><span class="sxs-lookup"><span data-stu-id="33f4b-219">Shared layout for Razor Pages Areas</span></span>

<span data-ttu-id="33f4b-220">Aby udostępnić typowych układu dla całej aplikacji, należy przenieść *_ViewStart.cshtml* do folderu głównego aplikacji.</span><span class="sxs-lookup"><span data-stu-id="33f4b-220">To share a common layout for the entire app, move the *_ViewStart.cshtml* to the application root folder.</span></span>

### <a name="publishing-areas"></a><span data-ttu-id="33f4b-221">Obszary publikowania</span><span class="sxs-lookup"><span data-stu-id="33f4b-221">Publishing Areas</span></span>

<span data-ttu-id="33f4b-222">\*.Cshtml wszystkie pliki i pliki w obrębie *wwwroot* katalogu są publikowane dane wyjściowe, gdy `<Project Sdk="Microsoft.NET.Sdk.Web">` znajdują się w pliku \*.csproj.</span><span class="sxs-lookup"><span data-stu-id="33f4b-222">All \*.cshtml files and files within the *wwwroot* directory are published to output when `<Project Sdk="Microsoft.NET.Sdk.Web">` is included in the \*.csproj file.</span></span>
