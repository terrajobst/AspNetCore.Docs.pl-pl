---
title: Obszary w programie ASP.NET Core
author: rick-anderson
description: Dowiedz się, jak obszary są funkcją programu ASP.NET MVC, używane do organizowania powiązanych funkcji do grupy jako osobne przestrzeni nazw (w przypadku routingu) i struktury ich folderów (w przypadku widoków).
ms.author: riande
ms.date: 02/14/2019
uid: mvc/controllers/areas
ms.openlocfilehash: c21eed04ea68512515da262b6b6895dc1a821039
ms.sourcegitcommit: 2c7ffe349eabdccf2ed748dd303ffd0ba6e1cfe3
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/26/2019
ms.locfileid: "56833530"
---
# <a name="areas-in-aspnet-core"></a><span data-ttu-id="837bb-103">Obszary w programie ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="837bb-103">Areas in ASP.NET Core</span></span>

<span data-ttu-id="837bb-104">Przez [Kumara Dhananjay](https://twitter.com/debug_mode) i [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="837bb-104">By [Dhananjay Kumar](https://twitter.com/debug_mode) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="837bb-105">Obszary są funkcją programu ASP.NET, używane do organizowania powiązanych funkcji do grupy jako osobne przestrzeni nazw (w przypadku routingu) i struktury ich folderów (w przypadku widoków).</span><span class="sxs-lookup"><span data-stu-id="837bb-105">Areas are an ASP.NET feature used to organize related functionality into a group as a separate namespace (for routing) and folder structure (for views).</span></span> <span data-ttu-id="837bb-106">Za pomocą obszarów tworzą hierarchię na potrzeby routingu, dodając innego parametru trasy, `area`, `controller` i `action` lub strony Razor `page`.</span><span class="sxs-lookup"><span data-stu-id="837bb-106">Using areas creates a hierarchy for the purpose of routing by adding another route parameter, `area`, to `controller` and `action` or a Razor Page `page`.</span></span>

<span data-ttu-id="837bb-107">Obszary umożliwiają partycji aplikacji sieci Web platformy ASP.NET Core na mniejsze grupy funkcjonalnej, każdy z swój własny zestaw stron Razor, kontrolerów, widoki i modele.</span><span class="sxs-lookup"><span data-stu-id="837bb-107">Areas provide a way to partition an ASP.NET Core Web app into smaller functional groups, each  with its own set of Razor Pages, controllers, views, and models.</span></span> <span data-ttu-id="837bb-108">Obszar skutecznie to struktura wewnątrz aplikacji.</span><span class="sxs-lookup"><span data-stu-id="837bb-108">An area is effectively a structure inside an app.</span></span> <span data-ttu-id="837bb-109">W projekcie sieci web platformy ASP.NET Core składników logicznych, takich jak strony, modelu, kontroler i Widok są przechowywane w różnych folderach.</span><span class="sxs-lookup"><span data-stu-id="837bb-109">In an ASP.NET Core web project, logical components like Pages, Model, Controller, and View are kept in different folders.</span></span> <span data-ttu-id="837bb-110">Środowisko uruchomieniowe programu ASP.NET Core używa konwencji nazewnictwa do utworzenia relacji między tymi składnikami.</span><span class="sxs-lookup"><span data-stu-id="837bb-110">The ASP.NET Core runtime uses naming conventions to create the relationship between these components.</span></span> <span data-ttu-id="837bb-111">W przypadku dużych aplikacji może być korzystne podzielić ją na oddzielnych wysokiego poziomu obszary funkcji.</span><span class="sxs-lookup"><span data-stu-id="837bb-111">For a large app, it may be advantageous to partition the app into separate high level areas of functionality.</span></span> <span data-ttu-id="837bb-112">Na przykład aplikacja handlu elektronicznego z wielu jednostek biznesowych, takich jak wyewidencjonowania, rozliczeń i wyszukiwania.</span><span class="sxs-lookup"><span data-stu-id="837bb-112">For instance, an e-commerce app with multiple business units, such as checkout, billing, and search.</span></span> <span data-ttu-id="837bb-113">Każda z tych jednostek ma swoje własne obszar zawiera widoki, kontrolery, stronami Razor i modeli.</span><span class="sxs-lookup"><span data-stu-id="837bb-113">Each of these units have their own area to contain views, controllers, Razor Pages, and models.</span></span>

<span data-ttu-id="837bb-114">Należy rozważyć użycie obszarów w projekcie po:</span><span class="sxs-lookup"><span data-stu-id="837bb-114">Consider using Areas in an project when:</span></span>

* <span data-ttu-id="837bb-115">Aplikacja składa się z wielu wysokiego poziomu funkcjonalności składników, które mogą zostać logicznie oddzielone.</span><span class="sxs-lookup"><span data-stu-id="837bb-115">The app is made of multiple high-level functional components that can be logically separated.</span></span>
* <span data-ttu-id="837bb-116">Chcesz podzielić na partycje aplikację tak, aby każdy obszar funkcjonalny może się opracowaniem niezależnie.</span><span class="sxs-lookup"><span data-stu-id="837bb-116">You want to partition the app so that each functional area can be worked on independently.</span></span>

<span data-ttu-id="837bb-117">[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/controllers/areas/samples) ([sposobu pobierania](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="837bb-117">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/controllers/areas/samples) ([how to download](xref:index#how-to-download-a-sample)).</span></span> <span data-ttu-id="837bb-118">Przykład pobierania zawiera podstawową aplikację do testowania obszarów.</span><span class="sxs-lookup"><span data-stu-id="837bb-118">The download sample provides a basic app for testing areas.</span></span>

## <a name="areas-for-controllers-with-views"></a><span data-ttu-id="837bb-119">Obszary dla kontrolerów z widokami</span><span class="sxs-lookup"><span data-stu-id="837bb-119">Areas for controllers with views</span></span>

<span data-ttu-id="837bb-120">Typowa aplikacja internetowa ASP.NET Core przy użyciu obszarów, widoków i kontrolerów zawiera następujące informacje:</span><span class="sxs-lookup"><span data-stu-id="837bb-120">A typical ASP.NET Core web app using areas, controllers, and views contains the following:</span></span>

* <span data-ttu-id="837bb-121">[Strukturę folderów obszaru](#area-folder-structure).</span><span class="sxs-lookup"><span data-stu-id="837bb-121">An [Area folder structure](#area-folder-structure).</span></span>
* <span data-ttu-id="837bb-122">Kontrolery ozdobione [ &lbrack;obszaru&rbrack; ](#attribute) atrybutu, aby skojarzyć kontroler z obszaru: [!code-csharp[](areas/samples/MVCareas/Areas/Products/Controllers/ManageController.cs?name=snippet2)]</span><span class="sxs-lookup"><span data-stu-id="837bb-122">Controllers decorated with the [&lbrack;Area&rbrack;](#attribute) attribute to associate the controller with the area: [!code-csharp[](areas/samples/MVCareas/Areas/Products/Controllers/ManageController.cs?name=snippet2)]</span></span>
* <span data-ttu-id="837bb-123">[Trasa obszaru dodana do uruchamiania](#add-area-route): [!code-csharp[](areas/samples/MVCareas/Startup.cs?name=snippet2&highlight=3-6)]</span><span class="sxs-lookup"><span data-stu-id="837bb-123">The [area route added to startup](#add-area-route): [!code-csharp[](areas/samples/MVCareas/Startup.cs?name=snippet2&highlight=3-6)]</span></span>

## <a name="area-folder-structure"></a><span data-ttu-id="837bb-124">Struktura folderów obszaru</span><span class="sxs-lookup"><span data-stu-id="837bb-124">Area folder structure</span></span>
<span data-ttu-id="837bb-125">Należy wziąć pod uwagę aplikację, która ma dwa grup logicznych *produktów* i *usług*.</span><span class="sxs-lookup"><span data-stu-id="837bb-125">Consider an app that has two logical groups, *Products* and *Services*.</span></span> <span data-ttu-id="837bb-126">Za pomocą obszarów, strukturę folderów będzie podobny do następującego:</span><span class="sxs-lookup"><span data-stu-id="837bb-126">Using areas, the folder structure would be similar to the following:</span></span>

* <span data-ttu-id="837bb-127">Project name (Nazwa projektu)</span><span class="sxs-lookup"><span data-stu-id="837bb-127">Project name</span></span>
  * <span data-ttu-id="837bb-128">Obszary</span><span class="sxs-lookup"><span data-stu-id="837bb-128">Areas</span></span>
    * <span data-ttu-id="837bb-129">Produkty</span><span class="sxs-lookup"><span data-stu-id="837bb-129">Products</span></span>
      * <span data-ttu-id="837bb-130">Kontrolery</span><span class="sxs-lookup"><span data-stu-id="837bb-130">Controllers</span></span>
        * <span data-ttu-id="837bb-131">HomeController.cs</span><span class="sxs-lookup"><span data-stu-id="837bb-131">HomeController.cs</span></span>
        * <span data-ttu-id="837bb-132">ManageController.cs</span><span class="sxs-lookup"><span data-stu-id="837bb-132">ManageController.cs</span></span>
      * <span data-ttu-id="837bb-133">Widoki</span><span class="sxs-lookup"><span data-stu-id="837bb-133">Views</span></span>
        * <span data-ttu-id="837bb-134">Home</span><span class="sxs-lookup"><span data-stu-id="837bb-134">Home</span></span>
          * <span data-ttu-id="837bb-135">Index.cshtml</span><span class="sxs-lookup"><span data-stu-id="837bb-135">Index.cshtml</span></span>
        * <span data-ttu-id="837bb-136">Zarządzanie</span><span class="sxs-lookup"><span data-stu-id="837bb-136">Manage</span></span>
          * <span data-ttu-id="837bb-137">Index.cshtml</span><span class="sxs-lookup"><span data-stu-id="837bb-137">Index.cshtml</span></span>
          * <span data-ttu-id="837bb-138">About.cshtml</span><span class="sxs-lookup"><span data-stu-id="837bb-138">About.cshtml</span></span>
    * <span data-ttu-id="837bb-139">Usługi</span><span class="sxs-lookup"><span data-stu-id="837bb-139">Services</span></span>
      * <span data-ttu-id="837bb-140">Kontrolery</span><span class="sxs-lookup"><span data-stu-id="837bb-140">Controllers</span></span>
        * <span data-ttu-id="837bb-141">HomeController.cs</span><span class="sxs-lookup"><span data-stu-id="837bb-141">HomeController.cs</span></span>
      * <span data-ttu-id="837bb-142">Widoki</span><span class="sxs-lookup"><span data-stu-id="837bb-142">Views</span></span>
        * <span data-ttu-id="837bb-143">Home</span><span class="sxs-lookup"><span data-stu-id="837bb-143">Home</span></span>
          * <span data-ttu-id="837bb-144">Index.cshtml</span><span class="sxs-lookup"><span data-stu-id="837bb-144">Index.cshtml</span></span>

<span data-ttu-id="837bb-145">Podczas poprzedniego układ jest typowe w przypadku, gdy przy użyciu obszarów, Wyświetl pliki, należy użyć tej struktury folderów.</span><span class="sxs-lookup"><span data-stu-id="837bb-145">While the preceding layout is typical when using Areas, only the view files are required to use this folder structure.</span></span> <span data-ttu-id="837bb-146">Widok odnajdywania wyszukuje pasującego pliku widoku obszaru w następującej kolejności:</span><span class="sxs-lookup"><span data-stu-id="837bb-146">View discovery searches for a matching area view file in the following order:</span></span>

```text
/Areas/<Area-Name>/Views/<Controller-Name>/<Action-Name>.cshtml
/Areas/<Area-Name>/Views/Shared/<Action-Name>.cshtml
/Views/Shared/<Action-Name>.cshtml
/Pages/Shared/<Action-Name>.cshtml
   ```

<span data-ttu-id="837bb-147">Lokalizacji-view, folderów, takie jak *kontrolerów* i *modeli* jest **nie** znaczenia.</span><span class="sxs-lookup"><span data-stu-id="837bb-147">The location of non-view folders like *Controllers* and *Models* does **not** matter.</span></span> <span data-ttu-id="837bb-148">Na przykład *kontrolerów* i *modeli* folderu nie są wymagane.</span><span class="sxs-lookup"><span data-stu-id="837bb-148">For example, the *Controllers* and *Models* folder are not required.</span></span> <span data-ttu-id="837bb-149">Zawartość *kontrolerów* i *modeli* jest kod, który zostanie skompilowany w dll.</span><span class="sxs-lookup"><span data-stu-id="837bb-149">The content of *Controllers* and *Models* is code which gets compiled into a .dll.</span></span> <span data-ttu-id="837bb-150">Zawartość *widoków* nie jest kompilowana, dopóki nie wykonano żądania do tego widoku.</span><span class="sxs-lookup"><span data-stu-id="837bb-150">The content of the *Views* isn't compiled until a request to that view has been made.</span></span>

<!-- TODO review:
The content of the *Views* isn't compiled until a request to that view has been made.

What about precompiled views? 
 -->
<a name="attribute"></a>

### <a name="associate-the-controller-with-an-area"></a><span data-ttu-id="837bb-151">Skojarzyć kontroler z obszaru</span><span class="sxs-lookup"><span data-stu-id="837bb-151">Associate the controller with an Area</span></span>

<span data-ttu-id="837bb-152">Obszar kontrolerów zostały oznaczone za pomocą [ &lbrack;obszaru&rbrack; ](xref:Microsoft.AspNetCore.Mvc.AreaAttribute) atrybutu:</span><span class="sxs-lookup"><span data-stu-id="837bb-152">Area controllers are designated with the [&lbrack;Area&rbrack;](xref:Microsoft.AspNetCore.Mvc.AreaAttribute) attribute:</span></span>

[!code-csharp[](areas/samples/MVCareas/Areas/Products/Controllers/ManageController.cs?highlight=5&name=snippet)]

### <a name="add-area-route"></a><span data-ttu-id="837bb-153">Dodaj trasę obszaru</span><span class="sxs-lookup"><span data-stu-id="837bb-153">Add Area route</span></span>

<span data-ttu-id="837bb-154">Obszar trasy używa się zazwyczaj do routingu konwencjonalna zamiast trasowanie atrybutów.</span><span class="sxs-lookup"><span data-stu-id="837bb-154">Area routes typically use conventional routing rather than attribute routing.</span></span> <span data-ttu-id="837bb-155">Tradycyjnie routing jest zależna od kolejności.</span><span class="sxs-lookup"><span data-stu-id="837bb-155">Conventional routing is order-dependent.</span></span> <span data-ttu-id="837bb-156">Ogólnie rzecz biorąc trasy z obszarami należy umieścić we wcześniejszej części tabeli tras, ponieważ są one bardziej szczegółowe niż trasy bez obszaru.</span><span class="sxs-lookup"><span data-stu-id="837bb-156">In general, routes with areas should be placed earlier in the route table as they're more specific than routes without an area.</span></span>

<span data-ttu-id="837bb-157">`{area:...}` może służyć jako token w szablonach tras Jeśli przestrzeń adresów url jest jednolita we wszystkich obszarach:</span><span class="sxs-lookup"><span data-stu-id="837bb-157">`{area:...}` can be used as a token in route templates if url space is uniform across all areas:</span></span>

[!code-csharp[](areas/samples/MVCareas/Startup.cs?name=snippet&highlight=18-21)]

<span data-ttu-id="837bb-158">W poprzednim kodzie `exists` zastosuje ograniczenie, że trasy musi odpowiadać obszar.</span><span class="sxs-lookup"><span data-stu-id="837bb-158">In the preceding code, `exists` applies a constraint that the route must match an area.</span></span> <span data-ttu-id="837bb-159">Za pomocą `{area:...}` jest najmniej skomplikowane mechanizm do dodawania routingu do obszarów.</span><span class="sxs-lookup"><span data-stu-id="837bb-159">Using `{area:...}` is the least complicated mechanism to adding routing to areas.</span></span>

<span data-ttu-id="837bb-160">Poniższy kod używa <xref:Microsoft.AspNetCore.Builder.MvcAreaRouteBuilderExtensions.MapAreaRoute*> utworzyć dwa o nazwie obszaru trasy:</span><span class="sxs-lookup"><span data-stu-id="837bb-160">The following code uses <xref:Microsoft.AspNetCore.Builder.MvcAreaRouteBuilderExtensions.MapAreaRoute*> to create two named area routes:</span></span>

[!code-csharp[](areas/samples/MVCareas/StartupMapAreaRoute.cs?name=snippet&highlight=18-27)]

<span data-ttu-id="837bb-161">Korzystając z `MapAreaRoute` za pomocą platformy ASP.NET Core 2.2, zobacz [problem w usłudze GitHub](https://github.com/aspnet/AspNetCore/issues/7772).</span><span class="sxs-lookup"><span data-stu-id="837bb-161">When using `MapAreaRoute` with ASP.NET Core 2.2, see [this GitHub issue](https://github.com/aspnet/AspNetCore/issues/7772).</span></span>

<span data-ttu-id="837bb-162">Aby uzyskać więcej informacji, zobacz [routingu obszaru](xref:mvc/controllers/routing#areas).</span><span class="sxs-lookup"><span data-stu-id="837bb-162">For more information, see [Area routing](xref:mvc/controllers/routing#areas).</span></span>

### <a name="link-generation-with-areas"></a><span data-ttu-id="837bb-163">Generowanie konsolidacji z obszarami</span><span class="sxs-lookup"><span data-stu-id="837bb-163">Link Generation with Areas</span></span>

<span data-ttu-id="837bb-164">Poniższy kod z [pobrania próbki](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/controllers/areas/samples) pokazuje połączenie generacji z użyciem obszaru określony:</span><span class="sxs-lookup"><span data-stu-id="837bb-164">The following code from the [sample download](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/controllers/areas/samples) shows link generation with the area specified:</span></span>

[!code-cshtml[](areas/samples/MVCareas/Views/Shared/_testLinksPartial.cshtml?name=snippet)]

<span data-ttu-id="837bb-165">Linki wygenerowane z poprzedniego kodu są prawidłowe w dowolnym miejscu aplikacji.</span><span class="sxs-lookup"><span data-stu-id="837bb-165">The links generated with the preceding code are valid anywhere in the app.</span></span>

<span data-ttu-id="837bb-166">Przykładowe do pobrania obejmują programy [widoku częściowego](xref:mvc/views/partial) zawiera poprzednie linki i uruchamia łącze tak samo bez określenia obszaru.</span><span class="sxs-lookup"><span data-stu-id="837bb-166">The sample download includes a [partial view](xref:mvc/views/partial) that contains the preceding links and the same links without specifying the area.</span></span> <span data-ttu-id="837bb-167">Odwołuje się widok częściowy [plik układu](), więc co w aplikacji stronę Wyświetla łącza wygenerowany.</span><span class="sxs-lookup"><span data-stu-id="837bb-167">The partial view is referenced in the [layout file](), so every page in the app displays the generated links.</span></span> <span data-ttu-id="837bb-168">Linki wygenerowany bez określenia obszaru są prawidłowe tylko gdy występuje do ze strony, w tym samym regionie i kontrolera.</span><span class="sxs-lookup"><span data-stu-id="837bb-168">The links generated without specifying the area are only valid when referenced from a page in the same area and controller.</span></span>

<span data-ttu-id="837bb-169">Jeśli nie określono obszaru lub kontrolera, routing zależy od *otoczenia* wartości.</span><span class="sxs-lookup"><span data-stu-id="837bb-169">When the area or controller is not specified, routing depends on the *ambient* values.</span></span> <span data-ttu-id="837bb-170">Bieżące wartości trasy, bieżącego żądania są traktowane jako wartości otoczenia dotyczącymi generowania łączy.</span><span class="sxs-lookup"><span data-stu-id="837bb-170">The current route values of the current request are considered ambient values for link generation.</span></span> <span data-ttu-id="837bb-171">W wielu przypadkach dla przykładowej aplikacji przy użyciu wartości otoczenia generuje nieprawidłowe linki.</span><span class="sxs-lookup"><span data-stu-id="837bb-171">In many cases for the sample app, using the ambient values generates incorrect links.</span></span>

<span data-ttu-id="837bb-172">Aby uzyskać więcej informacji, zobacz [Routing do akcji kontrolera](xref:mvc/controllers/routing).</span><span class="sxs-lookup"><span data-stu-id="837bb-172">For more information, see [Routing to controller actions](xref:mvc/controllers/routing).</span></span>

### <a name="shared-layout-for-areas-using-the-viewstartcshtml-file"></a><span data-ttu-id="837bb-173">Udostępnione układu dla obszarów przy użyciu pliku _ViewStart.cshtml</span><span class="sxs-lookup"><span data-stu-id="837bb-173">Shared layout for Areas using the _ViewStart.cshtml file</span></span>

<span data-ttu-id="837bb-174">Aby udostępnić typowych układu dla całej aplikacji, należy przenieść *_ViewStart.cshtml* do folderu głównego aplikacji.</span><span class="sxs-lookup"><span data-stu-id="837bb-174">To share a common layout for the entire app, move the *_ViewStart.cshtml* to the application root folder.</span></span>

<!-- This section will be completed after https://github.com/aspnet/Docs/pull/10978 is merged.
<a name="arp"></a>

## Areas for Razor Pages
-->
<a name="rename"></a>

### <a name="change-default-area-folder-where-views-are-stored"></a><span data-ttu-id="837bb-175">Zmień domyślny folder obszaru przechowywania widoków</span><span class="sxs-lookup"><span data-stu-id="837bb-175">Change default area folder where views are stored</span></span>

<span data-ttu-id="837bb-176">Poniższy kod zmienia domyślny folder obszaru z `"Areas"` do `"MyAreas"`:</span><span class="sxs-lookup"><span data-stu-id="837bb-176">The following code changes the default area folder from `"Areas"` to `"MyAreas"`:</span></span>

[!code-csharp[](areas/samples/MVCareas/Startup2.cs?name=snippet)]

<!-- TODO review - can we delete this. Areas doesn't change publishing - right? -->
### <a name="publishing-areas"></a><span data-ttu-id="837bb-177">Obszary publikowania</span><span class="sxs-lookup"><span data-stu-id="837bb-177">Publishing Areas</span></span>

<span data-ttu-id="837bb-178">Wszystkie `*.cshtml` i `wwwroot/**` plików, są publikowane dane wyjściowe, gdy `<Project Sdk="Microsoft.NET.Sdk.Web">` znajduje się w *.csproj* pliku.</span><span class="sxs-lookup"><span data-stu-id="837bb-178">All `*.cshtml` and `wwwroot/**` files are published to output when `<Project Sdk="Microsoft.NET.Sdk.Web">` is included in the *.csproj* file.</span></span>
