---
title: Obszary w programie ASP.NET Core
author: rick-anderson
description: Dowiedz się, jak obszary są funkcją programu ASP.NET MVC, używane do organizowania powiązanych funkcji do grupy jako osobne przestrzeni nazw (w przypadku routingu) i struktury ich folderów (w przypadku widoków).
ms.author: riande
ms.date: 02/14/2017
uid: mvc/controllers/areas
ms.openlocfilehash: b78bb5146f1ab9039fa9ff015471654510718ed6
ms.sourcegitcommit: ecf2cd4e0613569025b28e12de3baa21d86d4258
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/30/2018
ms.locfileid: "43312221"
---
# <a name="areas-in-aspnet-core"></a><span data-ttu-id="2976d-103">Obszary w programie ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="2976d-103">Areas in ASP.NET Core</span></span>

<span data-ttu-id="2976d-104">Przez [Kumara Dhananjay](https://twitter.com/debug_mode) i [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="2976d-104">By [Dhananjay Kumar](https://twitter.com/debug_mode)  and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="2976d-105">Obszary są funkcją programu ASP.NET MVC, używane do organizowania powiązanych funkcji do grupy jako osobne przestrzeni nazw (w przypadku routingu) i struktury ich folderów (w przypadku widoków).</span><span class="sxs-lookup"><span data-stu-id="2976d-105">Areas are an ASP.NET MVC feature used to organize related functionality into a group as a separate namespace (for routing) and folder structure (for views).</span></span> <span data-ttu-id="2976d-106">Za pomocą obszarów tworzą hierarchię na potrzeby routingu, dodając innego parametru trasy, `area`, `controller` i `action`.</span><span class="sxs-lookup"><span data-stu-id="2976d-106">Using areas creates a hierarchy for the purpose of routing by adding another route parameter, `area`, to `controller` and `action`.</span></span>

<span data-ttu-id="2976d-107">Obszary zapewniają sposób dzielenia dużych aplikacji sieci Web platformy ASP.NET Core MVC na mniejsze grupy funkcjonalnej.</span><span class="sxs-lookup"><span data-stu-id="2976d-107">Areas provide a way to partition a large ASP.NET Core MVC Web app into smaller functional groupings.</span></span> <span data-ttu-id="2976d-108">Obszar jest skutecznie strukturę MVC w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="2976d-108">An area is effectively an MVC structure inside an application.</span></span> <span data-ttu-id="2976d-109">W projekcie MVC logiczne składniki, takie jak Model, kontroler i Widok są przechowywane w różnych folderach, i są używane konwencje nazewnictwa do utworzenia relacji między tymi składnikami.</span><span class="sxs-lookup"><span data-stu-id="2976d-109">In an MVC project, logical components like Model, Controller, and View are kept in different folders, and MVC uses naming conventions to create the relationship between these components.</span></span> <span data-ttu-id="2976d-110">W przypadku dużych aplikacji może być korzystne podzielić ją na oddzielnych wysokiego poziomu obszary funkcji.</span><span class="sxs-lookup"><span data-stu-id="2976d-110">For a large app, it may be advantageous to partition the  app into separate high level areas of functionality.</span></span> <span data-ttu-id="2976d-111">Na przykład aplikacja handlu elektronicznego z wielu jednostek biznesowych, takich jak wyszukiwanie itp wyewidencjonowanie i rozliczeniami. Każda z tych jednostek ma swoje własne widoki logiczny składnik, kontrolery i modeli.</span><span class="sxs-lookup"><span data-stu-id="2976d-111">For instance, an e-commerce app with multiple business units, such as checkout, billing, and search etc. Each of these units have their own logical component views, controllers, and models.</span></span> <span data-ttu-id="2976d-112">W tym scenariuszu można użyć obszarów do partycjonowania fizycznie składniki biznesowej, w tym samym projekcie.</span><span class="sxs-lookup"><span data-stu-id="2976d-112">In this scenario, you can use Areas to physically partition the business components in the same project.</span></span>

<span data-ttu-id="2976d-113">Obszar mogą być definiowane jako mniejsze jednostki organizacyjne w projekcie programu ASP.NET Core MVC za pomocą swój własny zestaw kontrolerów, widoki i modele.</span><span class="sxs-lookup"><span data-stu-id="2976d-113">An area can be defined as smaller functional units in an ASP.NET Core MVC project with its own set of controllers, views, and models.</span></span>

<span data-ttu-id="2976d-114">Należy rozważyć użycie obszarów w MVC projektu, gdy:</span><span class="sxs-lookup"><span data-stu-id="2976d-114">Consider using Areas in an MVC project when:</span></span>

* <span data-ttu-id="2976d-115">Aplikacja składa się z wielu wysokiego poziomu funkcjonalności składników, które powinny zostać logicznie oddzielone</span><span class="sxs-lookup"><span data-stu-id="2976d-115">Your application is made of multiple high-level functional components that should be logically separated</span></span>

* <span data-ttu-id="2976d-116">Chcesz podzielić projektu MVC tak, aby każdy obszar funkcjonalny może się opracowaniem niezależnie</span><span class="sxs-lookup"><span data-stu-id="2976d-116">You want to partition your MVC project so that each functional area can be worked on independently</span></span>

<span data-ttu-id="2976d-117">Funkcje obszaru:</span><span class="sxs-lookup"><span data-stu-id="2976d-117">Area features:</span></span>

* <span data-ttu-id="2976d-118">Aplikacja ASP.NET Core MVC może mieć dowolną liczbę obszarów.</span><span class="sxs-lookup"><span data-stu-id="2976d-118">An ASP.NET Core MVC app can have any number of areas.</span></span>

* <span data-ttu-id="2976d-119">Każdy obszar ma swój własny, modeli, widoków i kontrolerów.</span><span class="sxs-lookup"><span data-stu-id="2976d-119">Each area has its own controllers, models, and views.</span></span>

* <span data-ttu-id="2976d-120">Obszary umożliwiają organizowanie dużymi projektami MVC w wielu składników wysokiego poziomu, które mogą być realizowane niezależnie.</span><span class="sxs-lookup"><span data-stu-id="2976d-120">Areas allow you to organize large MVC projects into multiple high-level components that can be worked on independently.</span></span>

* <span data-ttu-id="2976d-121">Obszary obsługuje wiele kontrolerów o takiej samej nazwie, tak długo, jak długo mają różne *obszarów*.</span><span class="sxs-lookup"><span data-stu-id="2976d-121">Areas support multiple controllers with the same name, as long as they have different *areas*.</span></span>

<span data-ttu-id="2976d-122">Spójrzmy na przykład aby zilustrować, jak obszary są tworzone i używane.</span><span class="sxs-lookup"><span data-stu-id="2976d-122">Let's take a look at an example to illustrate how Areas are created and used.</span></span> <span data-ttu-id="2976d-123">Załóżmy, że masz aplikację ze sklepu, która ma dwa oddzielne grupy widoków i kontrolerów: produktów i usług.</span><span class="sxs-lookup"><span data-stu-id="2976d-123">Let's say you have a store app that has two distinct groupings of controllers and views: Products and Services.</span></span> <span data-ttu-id="2976d-124">Typowy folder struktury dla, że przy użyciu obszarów MVC wygląda jak poniżej:</span><span class="sxs-lookup"><span data-stu-id="2976d-124">A typical folder structure for that using MVC areas looks like below:</span></span>

* <span data-ttu-id="2976d-125">Nazwa projektu</span><span class="sxs-lookup"><span data-stu-id="2976d-125">Project name</span></span>

  * <span data-ttu-id="2976d-126">Obszary</span><span class="sxs-lookup"><span data-stu-id="2976d-126">Areas</span></span>

    * <span data-ttu-id="2976d-127">Produkty</span><span class="sxs-lookup"><span data-stu-id="2976d-127">Products</span></span>

      * <span data-ttu-id="2976d-128">Kontrolery</span><span class="sxs-lookup"><span data-stu-id="2976d-128">Controllers</span></span>

        * <span data-ttu-id="2976d-129">HomeController.cs</span><span class="sxs-lookup"><span data-stu-id="2976d-129">HomeController.cs</span></span>

        * <span data-ttu-id="2976d-130">ManageController.cs</span><span class="sxs-lookup"><span data-stu-id="2976d-130">ManageController.cs</span></span>

      * <span data-ttu-id="2976d-131">Widoki</span><span class="sxs-lookup"><span data-stu-id="2976d-131">Views</span></span>

        * <span data-ttu-id="2976d-132">Home</span><span class="sxs-lookup"><span data-stu-id="2976d-132">Home</span></span>

          * <span data-ttu-id="2976d-133">Index.cshtml</span><span class="sxs-lookup"><span data-stu-id="2976d-133">Index.cshtml</span></span>

        * <span data-ttu-id="2976d-134">Zarządzanie</span><span class="sxs-lookup"><span data-stu-id="2976d-134">Manage</span></span>

          * <span data-ttu-id="2976d-135">Index.cshtml</span><span class="sxs-lookup"><span data-stu-id="2976d-135">Index.cshtml</span></span>

    * <span data-ttu-id="2976d-136">Usługi</span><span class="sxs-lookup"><span data-stu-id="2976d-136">Services</span></span>

      * <span data-ttu-id="2976d-137">Kontrolery</span><span class="sxs-lookup"><span data-stu-id="2976d-137">Controllers</span></span>

        * <span data-ttu-id="2976d-138">HomeController.cs</span><span class="sxs-lookup"><span data-stu-id="2976d-138">HomeController.cs</span></span>

      * <span data-ttu-id="2976d-139">Widoki</span><span class="sxs-lookup"><span data-stu-id="2976d-139">Views</span></span>

        * <span data-ttu-id="2976d-140">Home</span><span class="sxs-lookup"><span data-stu-id="2976d-140">Home</span></span>

          * <span data-ttu-id="2976d-141">Index.cshtml</span><span class="sxs-lookup"><span data-stu-id="2976d-141">Index.cshtml</span></span>

<span data-ttu-id="2976d-142">Gdy do renderowania widoku w obszarze domyślnie podejmie próbę MVC, próbuje Szukaj w następujących lokalizacjach:</span><span class="sxs-lookup"><span data-stu-id="2976d-142">When MVC tries to render a view in an Area, by default, it tries to look in the following locations:</span></span>

```text
/Areas/<Area-Name>/Views/<Controller-Name>/<Action-Name>.cshtml
   /Areas/<Area-Name>/Views/Shared/<Action-Name>.cshtml
   /Views/Shared/<Action-Name>.cshtml
   ```

<span data-ttu-id="2976d-143">Są to domyślne lokalizacje, które można zmienić za pośrednictwem `AreaViewLocationFormats` na `Microsoft.AspNetCore.Mvc.Razor.RazorViewEngineOptions`.</span><span class="sxs-lookup"><span data-stu-id="2976d-143">These are the default locations which can be changed via the `AreaViewLocationFormats` on the `Microsoft.AspNetCore.Mvc.Razor.RazorViewEngineOptions`.</span></span>

<span data-ttu-id="2976d-144">Na przykład w poniższego kodu, zamiast nazwy folderu jako "Obszary", został zmieniony na "Kategorie".</span><span class="sxs-lookup"><span data-stu-id="2976d-144">For example, in the below code instead of having the folder name as 'Areas', it has been changed to 'Categories'.</span></span>

```csharp
services.Configure<RazorViewEngineOptions>(options =>
   {
       options.AreaViewLocationFormats.Clear();
       options.AreaViewLocationFormats.Add("/Categories/{2}/Views/{1}/{0}.cshtml");
       options.AreaViewLocationFormats.Add("/Categories/{2}/Views/Shared/{0}.cshtml");
       options.AreaViewLocationFormats.Add("/Views/Shared/{0}.cshtml");
   });
   ```

<span data-ttu-id="2976d-145">Jedno, należy pamiętać, jest strukturą *widoków* folder jest tylko jeden, co jest uznawane za ważne tutaj i zawartość w pozostałej części folderów, takich jak *kontrolerów* i *modeli* jest **nie** znaczenia.</span><span class="sxs-lookup"><span data-stu-id="2976d-145">One thing to note is that the structure of the *Views* folder is the only one which is considered important here and the content of the rest of the folders like *Controllers* and *Models* does **not** matter.</span></span> <span data-ttu-id="2976d-146">Na przykład, użytkownik nie musi mieć *kontrolerów* i *modeli* folderu w ogóle.</span><span class="sxs-lookup"><span data-stu-id="2976d-146">For example, you need not have a *Controllers* and *Models* folder at all.</span></span> <span data-ttu-id="2976d-147">To działa, ponieważ zawartość *kontrolerów* i *modeli* jest po prostu kod, który pobiera skompilowany w dll, gdzie jako zawartość *widoków* nie jest do żądania, Wyświetl zostały wprowadzone.</span><span class="sxs-lookup"><span data-stu-id="2976d-147">This works because the content of *Controllers* and *Models* is just code which gets compiled into a .dll where as the content of the *Views* isn't until a request to that view has been made.</span></span>

<span data-ttu-id="2976d-148">Po zdefiniowaniu hierarchii folderów należy MVC stwierdzić, że każdy kontroler jest skojarzony z obszarem.</span><span class="sxs-lookup"><span data-stu-id="2976d-148">Once you've defined the folder hierarchy, you need to tell MVC that each controller is associated with an area.</span></span> <span data-ttu-id="2976d-149">Można to zrobić, dekoracji nazwy kontrolera, za pomocą `[Area]` atrybutu.</span><span class="sxs-lookup"><span data-stu-id="2976d-149">You do that by decorating the controller name with the `[Area]` attribute.</span></span>

```csharp
...
   namespace MyStore.Areas.Products.Controllers
   {
       [Area("Products")]
       public class HomeController : Controller
       {
           // GET: /Products/Home/Index
           public IActionResult Index()
           {
               return View();
           }

           // GET: /Products/Home/Create
           public IActionResult Create()
           {
               return View();
           }
       }
   }
   ```

<span data-ttu-id="2976d-150">Skonfiguruj definicję trasy, która współdziała z nowo utworzoną obszary.</span><span class="sxs-lookup"><span data-stu-id="2976d-150">Set up a route definition that works with your newly created areas.</span></span> <span data-ttu-id="2976d-151">[Trasy do akcji kontrolera](routing.md) artykułu przechodzi do szczegółowych informacji dotyczących sposobu tworzenia definicji trasy, w tym o korzystaniu z konwencjonalnych trasy w porównaniu z trasami atrybutów.</span><span class="sxs-lookup"><span data-stu-id="2976d-151">The [Route to controller actions](routing.md) article goes into detail about how to create route definitions, including using conventional routes versus attribute routes.</span></span> <span data-ttu-id="2976d-152">W tym przykładzie użyjemy konwencjonalne trasy.</span><span class="sxs-lookup"><span data-stu-id="2976d-152">In this example, we'll use a conventional route.</span></span> <span data-ttu-id="2976d-153">Aby to zrobić, otwórz *Startup.cs* plików i zmodyfikuj go, dodając `areaRoute` o nazwie definicji trasy.</span><span class="sxs-lookup"><span data-stu-id="2976d-153">To do so, open the *Startup.cs* file and modify it by adding the `areaRoute` named route definition below.</span></span>

```csharp
...
   app.UseMvc(routes =>
   {
     routes.MapRoute(
         name: "areaRoute",
         template: "{area:exists}/{controller=Home}/{action=Index}/{id?}");

     routes.MapRoute(
         name: "default",
         template: "{controller=Home}/{action=Index}/{id?}");
   });
   ```

<span data-ttu-id="2976d-154">Przechodzenie do `http://<yourApp>/products`, `Index` metody akcji `HomeController` w `Products` obszar, który zostanie wywołany.</span><span class="sxs-lookup"><span data-stu-id="2976d-154">Browsing to `http://<yourApp>/products`, the `Index` action method of the `HomeController` in the `Products` area will be invoked.</span></span>

## <a name="link-generation"></a><span data-ttu-id="2976d-155">Generowanie konsolidacji</span><span class="sxs-lookup"><span data-stu-id="2976d-155">Link Generation</span></span>

* <span data-ttu-id="2976d-156">Podczas generowania łączy z akcji wewnątrz obszaru na podstawie kontrolera do innej akcji w obrębie tego samego kontrolera.</span><span class="sxs-lookup"><span data-stu-id="2976d-156">Generating links from an action within an area based controller to another action within the same controller.</span></span>

  <span data-ttu-id="2976d-157">Załóżmy, że ścieżka bieżącego żądania jest podobne `/Products/Home/Create`</span><span class="sxs-lookup"><span data-stu-id="2976d-157">Let's say the current request's path is like `/Products/Home/Create`</span></span>

  <span data-ttu-id="2976d-158">Składnia HtmlHelper: `@Html.ActionLink("Go to Product's Home Page", "Index")`</span><span class="sxs-lookup"><span data-stu-id="2976d-158">HtmlHelper syntax: `@Html.ActionLink("Go to Product's Home Page", "Index")`</span></span>

  <span data-ttu-id="2976d-159">Składnia pomocnika tagów: `<a asp-action="Index">Go to Product's Home Page</a>`</span><span class="sxs-lookup"><span data-stu-id="2976d-159">TagHelper syntax: `<a asp-action="Index">Go to Product's Home Page</a>`</span></span>

  <span data-ttu-id="2976d-160">Należy pamiętać, że, firma Microsoft nie musi dostarczać wartości "obszar" i "controller" tutaj, ponieważ są one już dostępne w kontekście bieżącego żądania.</span><span class="sxs-lookup"><span data-stu-id="2976d-160">Note that we need not supply the 'area' and 'controller' values here as they're already available in the context of the current request.</span></span> <span data-ttu-id="2976d-161">Ten rodzaj wartości są nazywane `ambient` wartości.</span><span class="sxs-lookup"><span data-stu-id="2976d-161">These kind of values are called `ambient` values.</span></span>

* <span data-ttu-id="2976d-162">Podczas generowania łączy z akcji wewnątrz obszaru na podstawie kontrolera do kolejnej akcji na innym kontrolerze</span><span class="sxs-lookup"><span data-stu-id="2976d-162">Generating links from an action within an area based controller to another action on a different controller</span></span>

  <span data-ttu-id="2976d-163">Załóżmy, że ścieżka bieżącego żądania jest podobne `/Products/Home/Create`</span><span class="sxs-lookup"><span data-stu-id="2976d-163">Let's say the current request's path is like `/Products/Home/Create`</span></span>

  <span data-ttu-id="2976d-164">Składnia HtmlHelper: `@Html.ActionLink("Go to Manage Products Home Page", "Index", "Manage")`</span><span class="sxs-lookup"><span data-stu-id="2976d-164">HtmlHelper syntax: `@Html.ActionLink("Go to Manage Products Home Page", "Index", "Manage")`</span></span>

  <span data-ttu-id="2976d-165">Składnia pomocnika tagów: `<a asp-controller="Manage" asp-action="Index">Go to Manage Products Home Page</a>`</span><span class="sxs-lookup"><span data-stu-id="2976d-165">TagHelper syntax: `<a asp-controller="Manage" asp-action="Index">Go to Manage Products Home Page</a>`</span></span>

  <span data-ttu-id="2976d-166">Pamiętaj, że w tym miejscu zostanie użyta wartość otoczenia obszaru, ale jawnie określono wartość "controller", powyżej.</span><span class="sxs-lookup"><span data-stu-id="2976d-166">Note that here the ambient value of an 'area' is used but the 'controller' value is specified explicitly above.</span></span>

* <span data-ttu-id="2976d-167">Podczas generowania łączy z akcji w obrębie kontrolera do kolejnej akcji na podstawie innego kontrolera i innego obszaru.</span><span class="sxs-lookup"><span data-stu-id="2976d-167">Generating links from an action within an area based controller to another action on a different controller and a different area.</span></span>

  <span data-ttu-id="2976d-168">Załóżmy, że ścieżka bieżącego żądania jest podobne `/Products/Home/Create`</span><span class="sxs-lookup"><span data-stu-id="2976d-168">Let's say the current request's path is like `/Products/Home/Create`</span></span>

  <span data-ttu-id="2976d-169">Składnia HtmlHelper: `@Html.ActionLink("Go to Services Home Page", "Index", "Home", new { area = "Services" })`</span><span class="sxs-lookup"><span data-stu-id="2976d-169">HtmlHelper syntax: `@Html.ActionLink("Go to Services Home Page", "Index", "Home", new { area = "Services" })`</span></span>

  <span data-ttu-id="2976d-170">Składnia pomocnika tagów: `<a asp-area="Services" asp-controller="Home" asp-action="Index">Go to Services Home Page</a>`</span><span class="sxs-lookup"><span data-stu-id="2976d-170">TagHelper syntax: `<a asp-area="Services" asp-controller="Home" asp-action="Index">Go to Services Home Page</a>`</span></span>

  <span data-ttu-id="2976d-171">Należy pamiętać, że w tym miejscu są używane żadne wartości otoczenia.</span><span class="sxs-lookup"><span data-stu-id="2976d-171">Note that here no ambient values are used.</span></span>

* <span data-ttu-id="2976d-172">Podczas generowania łączy z akcji w kontrolerze obszaru na podstawie innej akcji na innym kontrolerze i **nie** w obszarze.</span><span class="sxs-lookup"><span data-stu-id="2976d-172">Generating links from an action within an area based controller to another action on a different controller and **not** in an area.</span></span>

  <span data-ttu-id="2976d-173">Składnia HtmlHelper: `@Html.ActionLink("Go to Manage Products  Home Page", "Index", "Home", new { area = "" })`</span><span class="sxs-lookup"><span data-stu-id="2976d-173">HtmlHelper syntax: `@Html.ActionLink("Go to Manage Products  Home Page", "Index", "Home", new { area = "" })`</span></span>

  <span data-ttu-id="2976d-174">Składnia pomocnika tagów: `<a asp-area="" asp-controller="Manage" asp-action="Index">Go to Manage Products Home Page</a>`</span><span class="sxs-lookup"><span data-stu-id="2976d-174">TagHelper syntax: `<a asp-area="" asp-controller="Manage" asp-action="Index">Go to Manage Products Home Page</a>`</span></span>

  <span data-ttu-id="2976d-175">Ponieważ chcemy wygenerować łączy do innych obszaru na podstawie akcji kontrolera, możemy pusty otoczenia wartość "obszar" w tym miejscu.</span><span class="sxs-lookup"><span data-stu-id="2976d-175">Since we want to generate links to a non-area based controller action, we empty the ambient value for 'area' here.</span></span>

## <a name="publishing-areas"></a><span data-ttu-id="2976d-176">Obszary publikowania</span><span class="sxs-lookup"><span data-stu-id="2976d-176">Publishing Areas</span></span>

<span data-ttu-id="2976d-177">Wszystkie `*.cshtml` i `wwwroot/**` plików, są publikowane dane wyjściowe, gdy `<Project Sdk="Microsoft.NET.Sdk.Web">` znajduje się w *.csproj* pliku.</span><span class="sxs-lookup"><span data-stu-id="2976d-177">All `*.cshtml` and `wwwroot/**` files are published to output when `<Project Sdk="Microsoft.NET.Sdk.Web">` is included in the *.csproj* file.</span></span>
