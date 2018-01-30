---
title: Obszary
author: rick-anderson
description: "Pokazuje, jak pracować z obszarów."
manager: wpickett
ms.author: riande
ms.date: 02/14/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: mvc/controllers/areas
ms.openlocfilehash: 1ade49de3f6c58edc4ea7b06bc593b3db797081c
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/30/2018
---
# <a name="areas"></a><span data-ttu-id="5a4ec-103">Obszary</span><span class="sxs-lookup"><span data-stu-id="5a4ec-103">Areas</span></span>

<span data-ttu-id="5a4ec-104">Przez [Dhananjay Kumar](https://twitter.com/debug_mode) i [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="5a4ec-104">By [Dhananjay Kumar](https://twitter.com/debug_mode)  and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="5a4ec-105">Kwestie funkcji ASP.NET MVC, używane do organizowania funkcje w grupie jako osobne w obszarze nazw (routing) i struktury folderów (dla widoków).</span><span class="sxs-lookup"><span data-stu-id="5a4ec-105">Areas are an ASP.NET MVC feature used to organize related functionality into a group as a separate namespace (for routing) and folder structure (for views).</span></span> <span data-ttu-id="5a4ec-106">Za pomocą obszarów tworzy hierarchię na potrzeby routingu, dodając inny parametru trasy, `area`, do `controller` i `action`.</span><span class="sxs-lookup"><span data-stu-id="5a4ec-106">Using areas creates a hierarchy for the purpose of routing by adding another route parameter, `area`, to `controller` and `action`.</span></span>

<span data-ttu-id="5a4ec-107">Obszary zapewniają sposób podziału dużych aplikacji sieci Web platformy ASP.NET Core MVC na mniejsze funkcjonalności grupowania.</span><span class="sxs-lookup"><span data-stu-id="5a4ec-107">Areas provide a way to partition a large ASP.NET Core MVC Web app into smaller functional groupings.</span></span> <span data-ttu-id="5a4ec-108">Obszar skutecznie to struktura MVC wewnątrz aplikacji.</span><span class="sxs-lookup"><span data-stu-id="5a4ec-108">An area is effectively an MVC structure inside an application.</span></span> <span data-ttu-id="5a4ec-109">W projekcie MVC składników logicznych, takich jak Model, kontrolera i widoku są przechowywane w różnych folderach i MVC używa konwencji nazewnictwa do utworzenia relacji między tymi składnikami.</span><span class="sxs-lookup"><span data-stu-id="5a4ec-109">In an MVC project, logical components like Model, Controller, and View are kept in different folders, and MVC uses naming conventions to create the relationship between these components.</span></span> <span data-ttu-id="5a4ec-110">W przypadku dużych aplikacji może być korzystne partycji aplikacji na oddzielnych wysokiej obszary poziomu funkcjonalności.</span><span class="sxs-lookup"><span data-stu-id="5a4ec-110">For a large app, it may be advantageous to partition the  app into separate high level areas of functionality.</span></span> <span data-ttu-id="5a4ec-111">Na przykład aplikacji handlu elektronicznego z wiele jednostek biznesowych, takich jak wyewidencjonowania, rozliczeń i wyszukiwania itd. Każdy z tych jednostek ma własne logiczny składnik widoki, kontrolery i modeli.</span><span class="sxs-lookup"><span data-stu-id="5a4ec-111">For instance, an e-commerce app with multiple business units, such as checkout, billing, and search etc. Each of these units have their own logical component views, controllers, and models.</span></span> <span data-ttu-id="5a4ec-112">W tym scenariuszu można obszarów fizycznie partycji składników biznesowej w tym samym projekcie.</span><span class="sxs-lookup"><span data-stu-id="5a4ec-112">In this scenario, you can use Areas to physically partition the business components in the same project.</span></span>

<span data-ttu-id="5a4ec-113">Obszar może być zdefiniowany jako mniejsze jednostki organizacyjne w projekcie platformy ASP.NET Core MVC z zestawem kontrolerów, widoków i modeli.</span><span class="sxs-lookup"><span data-stu-id="5a4ec-113">An area can be defined as smaller functional units in an ASP.NET Core MVC project with its own set of controllers, views, and models.</span></span>

<span data-ttu-id="5a4ec-114">Należy rozważyć użycie obszarów MVC projektu, gdy:</span><span class="sxs-lookup"><span data-stu-id="5a4ec-114">Consider using Areas in an MVC project when:</span></span>

* <span data-ttu-id="5a4ec-115">Aplikacja składa się z wielu wysokiego poziomu funkcjonalności składników, które powinny być oddzielone logicznie</span><span class="sxs-lookup"><span data-stu-id="5a4ec-115">Your application is made of multiple high-level functional components that should be logically separated</span></span>

* <span data-ttu-id="5a4ec-116">Aby partycji projektu MVC, dzięki czemu obszarów funkcjonalnych mogą wykonywać niezależnie</span><span class="sxs-lookup"><span data-stu-id="5a4ec-116">You want to partition your MVC project so that each functional area can be worked on independently</span></span>

<span data-ttu-id="5a4ec-117">Funkcje obszaru:</span><span class="sxs-lookup"><span data-stu-id="5a4ec-117">Area features:</span></span>

* <span data-ttu-id="5a4ec-118">Aplikacja ASP.NET Core MVC może mieć dowolną liczbę obszarów</span><span class="sxs-lookup"><span data-stu-id="5a4ec-118">An ASP.NET Core MVC app can have any number of areas</span></span>

* <span data-ttu-id="5a4ec-119">Każdy obszar ma swoją własną kontrolerów, modeli i widoków</span><span class="sxs-lookup"><span data-stu-id="5a4ec-119">Each area has its own controllers, models, and views</span></span>

* <span data-ttu-id="5a4ec-120">Umożliwia organizowanie dużych projektów MVC w wielu składników wysokiego poziomu, które mogą wykonywać niezależnie</span><span class="sxs-lookup"><span data-stu-id="5a4ec-120">Allows you to organize large MVC projects into multiple high-level components that can be worked on independently</span></span>

* <span data-ttu-id="5a4ec-121">Obsługuje wiele kontrolerów o takiej samej nazwie — tak długo, jak długo mają różne *obszarów*</span><span class="sxs-lookup"><span data-stu-id="5a4ec-121">Supports multiple controllers with the same name - as long as they have different *areas*</span></span>

<span data-ttu-id="5a4ec-122">Spójrzmy na przykład ilustrujący sposób obszarów są tworzone i używane.</span><span class="sxs-lookup"><span data-stu-id="5a4ec-122">Let's take a look at an example to illustrate how Areas are created and used.</span></span> <span data-ttu-id="5a4ec-123">Załóżmy, że masz aplikację sklepu, która ma dwa oddzielne grupy kontrolery i widoki: produktów i usług.</span><span class="sxs-lookup"><span data-stu-id="5a4ec-123">Let's say you have a store app that has two distinct groupings of controllers and views: Products and Services.</span></span> <span data-ttu-id="5a4ec-124">Typowy folder struktury czy za pomocą obszarów MVC wygląda jak poniżej:</span><span class="sxs-lookup"><span data-stu-id="5a4ec-124">A typical folder structure for that using MVC areas looks like below:</span></span>

* <span data-ttu-id="5a4ec-125">Nazwa projektu</span><span class="sxs-lookup"><span data-stu-id="5a4ec-125">Project name</span></span>

  * <span data-ttu-id="5a4ec-126">Obszary</span><span class="sxs-lookup"><span data-stu-id="5a4ec-126">Areas</span></span>

    * <span data-ttu-id="5a4ec-127">Produkty</span><span class="sxs-lookup"><span data-stu-id="5a4ec-127">Products</span></span>

      * <span data-ttu-id="5a4ec-128">Kontrolery</span><span class="sxs-lookup"><span data-stu-id="5a4ec-128">Controllers</span></span>

        * <span data-ttu-id="5a4ec-129">HomeController.cs</span><span class="sxs-lookup"><span data-stu-id="5a4ec-129">HomeController.cs</span></span>

        * <span data-ttu-id="5a4ec-130">ManageController.cs</span><span class="sxs-lookup"><span data-stu-id="5a4ec-130">ManageController.cs</span></span>

      * <span data-ttu-id="5a4ec-131">Widoki</span><span class="sxs-lookup"><span data-stu-id="5a4ec-131">Views</span></span>

        * <span data-ttu-id="5a4ec-132">Home</span><span class="sxs-lookup"><span data-stu-id="5a4ec-132">Home</span></span>

          * <span data-ttu-id="5a4ec-133">Index.cshtml</span><span class="sxs-lookup"><span data-stu-id="5a4ec-133">Index.cshtml</span></span>

        * <span data-ttu-id="5a4ec-134">Zarządzanie</span><span class="sxs-lookup"><span data-stu-id="5a4ec-134">Manage</span></span>

          * <span data-ttu-id="5a4ec-135">Index.cshtml</span><span class="sxs-lookup"><span data-stu-id="5a4ec-135">Index.cshtml</span></span>

    * <span data-ttu-id="5a4ec-136">Usługi</span><span class="sxs-lookup"><span data-stu-id="5a4ec-136">Services</span></span>

      * <span data-ttu-id="5a4ec-137">Kontrolery</span><span class="sxs-lookup"><span data-stu-id="5a4ec-137">Controllers</span></span>

        * <span data-ttu-id="5a4ec-138">HomeController.cs</span><span class="sxs-lookup"><span data-stu-id="5a4ec-138">HomeController.cs</span></span>

      * <span data-ttu-id="5a4ec-139">Widoki</span><span class="sxs-lookup"><span data-stu-id="5a4ec-139">Views</span></span>

        * <span data-ttu-id="5a4ec-140">Home</span><span class="sxs-lookup"><span data-stu-id="5a4ec-140">Home</span></span>

          * <span data-ttu-id="5a4ec-141">Index.cshtml</span><span class="sxs-lookup"><span data-stu-id="5a4ec-141">Index.cshtml</span></span>

<span data-ttu-id="5a4ec-142">Gdy do renderowania widoku w obszarze domyślnie podejmie próbę MVC, próbuje znaleźć w następujących lokalizacjach:</span><span class="sxs-lookup"><span data-stu-id="5a4ec-142">When MVC tries to render a view in an Area, by default, it tries to look in the following locations:</span></span>

```text
/Areas/<Area-Name>/Views/<Controller-Name>/<Action-Name>.cshtml
   /Areas/<Area-Name>/Views/Shared/<Action-Name>.cshtml
   /Views/Shared/<Action-Name>.cshtml
   ```

<span data-ttu-id="5a4ec-143">Są to domyślne lokalizacje, które można zmienić za pośrednictwem `AreaViewLocationFormats` na `Microsoft.AspNetCore.Mvc.Razor.RazorViewEngineOptions`.</span><span class="sxs-lookup"><span data-stu-id="5a4ec-143">These are the default locations which can be changed via the `AreaViewLocationFormats` on the `Microsoft.AspNetCore.Mvc.Razor.RazorViewEngineOptions`.</span></span>

<span data-ttu-id="5a4ec-144">Na przykład w poniższego kodu, zamiast nazwy folderu jako "Obszary", został zmieniony na "Kategorie".</span><span class="sxs-lookup"><span data-stu-id="5a4ec-144">For example, in the below code instead of having the folder name as 'Areas', it has been changed to 'Categories'.</span></span>

```csharp
services.Configure<RazorViewEngineOptions>(options =>
   {
       options.AreaViewLocationFormats.Clear();
       options.AreaViewLocationFormats.Add("/Categories/{2}/Views/{1}/{0}.cshtml");
       options.AreaViewLocationFormats.Add("/Categories/{2}/Views/Shared/{0}.cshtml");
       options.AreaViewLocationFormats.Add("/Views/Shared/{0}.cshtml");
   });
   ```

<span data-ttu-id="5a4ec-145">Jedyną operacją, należy pamiętać, jest strukturą *widoków* folder jest tylko jeden, który jest uznawany za ważny w tym miejscu i zawartości pozostałej części foldery, takich jak *kontrolerów* i *modele* jest **nie** znaczenia.</span><span class="sxs-lookup"><span data-stu-id="5a4ec-145">One thing to note is that the structure of the *Views* folder is the only one which is considered important here and the content of the rest of the folders like *Controllers* and *Models* does **not** matter.</span></span> <span data-ttu-id="5a4ec-146">Na przykład można nie wymagają *kontrolerów* i *modele* folderu wcale.</span><span class="sxs-lookup"><span data-stu-id="5a4ec-146">For example, you need not have a *Controllers* and *Models* folder at all.</span></span> <span data-ttu-id="5a4ec-147">To działa, ponieważ zawartość *kontrolerów* i *modeli* jest tylko kod, który pobiera skompilowany w Biblioteka DLL, gdzie jako zawartość *widoków* nie jest do żądania z kodem Widok zostały wprowadzone.</span><span class="sxs-lookup"><span data-stu-id="5a4ec-147">This works because the content of *Controllers* and *Models* is just code which gets compiled into a .dll where as the content of the *Views* isn't until a request to that view has been made.</span></span>

<span data-ttu-id="5a4ec-148">Po zdefiniowaniu hierarchię folderów, należy sprawdzić MVC, że każdy kontroler jest skojarzony z obszarem.</span><span class="sxs-lookup"><span data-stu-id="5a4ec-148">Once you've defined the folder hierarchy, you need to tell MVC that each controller is associated with an area.</span></span> <span data-ttu-id="5a4ec-149">Można to zrobić, nazwy kontrolera z dekoracji `[Area]` atrybutu.</span><span class="sxs-lookup"><span data-stu-id="5a4ec-149">You do that by decorating the controller name with the `[Area]` attribute.</span></span>

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

<span data-ttu-id="5a4ec-150">Skonfiguruj definicję trasy, która współdziała z nowo utworzonego obszarach.</span><span class="sxs-lookup"><span data-stu-id="5a4ec-150">Set up a route definition that works with your newly created areas.</span></span> <span data-ttu-id="5a4ec-151">[Routingu do akcji kontrolera](routing.md) artykułu przechodzi do szczegółów dotyczących sposobu tworzenia definicji trasy, w tym o korzystaniu z konwencjonalnej tras i tras atrybutów.</span><span class="sxs-lookup"><span data-stu-id="5a4ec-151">The [Routing to Controller Actions](routing.md) article goes into detail about how to create route definitions, including using conventional routes versus attribute routes.</span></span> <span data-ttu-id="5a4ec-152">W tym przykładzie użyjemy konwencjonalnej trasy.</span><span class="sxs-lookup"><span data-stu-id="5a4ec-152">In this example, we'll use a conventional route.</span></span> <span data-ttu-id="5a4ec-153">Aby to zrobić, otwórz *Startup.cs* pliku, a następnie zmodyfikować go przez dodanie `areaRoute` o nazwie poniżej definicji trasy.</span><span class="sxs-lookup"><span data-stu-id="5a4ec-153">To do so, open the *Startup.cs* file and modify it by adding the `areaRoute` named route definition below.</span></span>

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

<span data-ttu-id="5a4ec-154">Przeglądanie do `http://<yourApp>/products`, `Index` metody akcji `HomeController` w `Products` obszaru zostanie wywołany.</span><span class="sxs-lookup"><span data-stu-id="5a4ec-154">Browsing to `http://<yourApp>/products`, the `Index` action method of the `HomeController` in the `Products` area will be invoked.</span></span>

## <a name="link-generation"></a><span data-ttu-id="5a4ec-155">Generowanie konsolidacji</span><span class="sxs-lookup"><span data-stu-id="5a4ec-155">Link Generation</span></span>

* <span data-ttu-id="5a4ec-156">Generowania łączy z akcji wewnątrz obszaru na podstawie kontrolera innego działania w ramach tego samego kontrolera.</span><span class="sxs-lookup"><span data-stu-id="5a4ec-156">Generating links from an action within an area based controller to another action within the same controller.</span></span>

  <span data-ttu-id="5a4ec-157">Załóżmy, że ścieżka bieżącego żądania przypomina`/Products/Home/Create`</span><span class="sxs-lookup"><span data-stu-id="5a4ec-157">Let's say the current request's path is like `/Products/Home/Create`</span></span>

  <span data-ttu-id="5a4ec-158">Składnia HtmlHelper:`@Html.ActionLink("Go to Product's Home Page", "Index")`</span><span class="sxs-lookup"><span data-stu-id="5a4ec-158">HtmlHelper syntax: `@Html.ActionLink("Go to Product's Home Page", "Index")`</span></span>

  <span data-ttu-id="5a4ec-159">Składnia pomocnika tagów:`<a asp-action="Index">Go to Product's Home Page</a>`</span><span class="sxs-lookup"><span data-stu-id="5a4ec-159">TagHelper syntax: `<a asp-action="Index">Go to Product's Home Page</a>`</span></span>

  <span data-ttu-id="5a4ec-160">Należy pamiętać, że, firma Microsoft nie musi dostarczać wartości "obszar" i "controller" poniżej, ponieważ są one już dostępne w kontekście bieżącego żądania.</span><span class="sxs-lookup"><span data-stu-id="5a4ec-160">Note that we need not supply the 'area' and 'controller' values here as they're already available in the context of the current request.</span></span> <span data-ttu-id="5a4ec-161">Tymi rodzaj wartości są nazywane `ambient` wartości.</span><span class="sxs-lookup"><span data-stu-id="5a4ec-161">These kind of values are called `ambient` values.</span></span>

* <span data-ttu-id="5a4ec-162">Generowania łączy z akcji wewnątrz obszaru na podstawie kontrolera do innej akcji na innym kontrolerze</span><span class="sxs-lookup"><span data-stu-id="5a4ec-162">Generating links from an action within an area based controller to another action on a different controller</span></span>

  <span data-ttu-id="5a4ec-163">Załóżmy, że ścieżka bieżącego żądania przypomina`/Products/Home/Create`</span><span class="sxs-lookup"><span data-stu-id="5a4ec-163">Let's say the current request's path is like `/Products/Home/Create`</span></span>

  <span data-ttu-id="5a4ec-164">Składnia HtmlHelper:`@Html.ActionLink("Go to Manage Products Home Page", "Index", "Manage")`</span><span class="sxs-lookup"><span data-stu-id="5a4ec-164">HtmlHelper syntax: `@Html.ActionLink("Go to Manage Products Home Page", "Index", "Manage")`</span></span>

  <span data-ttu-id="5a4ec-165">Składnia pomocnika tagów:`<a asp-controller="Manage" asp-action="Index">Go to Manage Products Home Page</a>`</span><span class="sxs-lookup"><span data-stu-id="5a4ec-165">TagHelper syntax: `<a asp-controller="Manage" asp-action="Index">Go to Manage Products Home Page</a>`</span></span>

  <span data-ttu-id="5a4ec-166">Pamiętaj, że w tym miejscu zostanie użyta wartość otoczenia obszaru, ale jawnie określona wartość "Administrator" powyżej.</span><span class="sxs-lookup"><span data-stu-id="5a4ec-166">Note that here the ambient value of an 'area' is used but the 'controller' value is specified explicitly above.</span></span>

* <span data-ttu-id="5a4ec-167">Generowania łączy z akcji w obrębie kontrolera do innej akcji na podstawie innego kontrolera i inny obszar.</span><span class="sxs-lookup"><span data-stu-id="5a4ec-167">Generating links from an action within an area based controller to another action on a different controller and a different area.</span></span>

  <span data-ttu-id="5a4ec-168">Załóżmy, że ścieżka bieżącego żądania przypomina`/Products/Home/Create`</span><span class="sxs-lookup"><span data-stu-id="5a4ec-168">Let's say the current request's path is like `/Products/Home/Create`</span></span>

  <span data-ttu-id="5a4ec-169">Składnia HtmlHelper:`@Html.ActionLink("Go to Services Home Page", "Index", "Home", new { area = "Services" })`</span><span class="sxs-lookup"><span data-stu-id="5a4ec-169">HtmlHelper syntax: `@Html.ActionLink("Go to Services Home Page", "Index", "Home", new { area = "Services" })`</span></span>

  <span data-ttu-id="5a4ec-170">Składnia pomocnika tagów:`<a asp-area="Services" asp-controller="Home" asp-action="Index">Go to Services Home Page</a>`</span><span class="sxs-lookup"><span data-stu-id="5a4ec-170">TagHelper syntax: `<a asp-area="Services" asp-controller="Home" asp-action="Index">Go to Services Home Page</a>`</span></span>

  <span data-ttu-id="5a4ec-171">Należy pamiętać, że w tym miejscu są używane nie wartości otoczenia.</span><span class="sxs-lookup"><span data-stu-id="5a4ec-171">Note that here no ambient values are used.</span></span>

* <span data-ttu-id="5a4ec-172">Generowanie łącza akcji w obrębie kontrolera obszaru na podstawie innej akcji na innym kontrolerze i **nie** w obszarze.</span><span class="sxs-lookup"><span data-stu-id="5a4ec-172">Generating links from an action within an area based controller to another action on a different controller and **not** in an area.</span></span>

  <span data-ttu-id="5a4ec-173">Składnia HtmlHelper:`@Html.ActionLink("Go to Manage Products  Home Page", "Index", "Home", new { area = "" })`</span><span class="sxs-lookup"><span data-stu-id="5a4ec-173">HtmlHelper syntax: `@Html.ActionLink("Go to Manage Products  Home Page", "Index", "Home", new { area = "" })`</span></span>

  <span data-ttu-id="5a4ec-174">Składnia pomocnika tagów:`<a asp-area="" asp-controller="Manage" asp-action="Index">Go to Manage Products Home Page</a>`</span><span class="sxs-lookup"><span data-stu-id="5a4ec-174">TagHelper syntax: `<a asp-area="" asp-controller="Manage" asp-action="Index">Go to Manage Products Home Page</a>`</span></span>

  <span data-ttu-id="5a4ec-175">Ponieważ chcemy, aby wygenerować łączy do innych niż obszaru na podstawie akcji kontrolera, możemy pusty otoczenia wartość "obszar" w tym miejscu.</span><span class="sxs-lookup"><span data-stu-id="5a4ec-175">Since we want to generate links to a non-area based controller action, we empty the ambient value for 'area' here.</span></span>

## <a name="publishing-areas"></a><span data-ttu-id="5a4ec-176">Obszary publikowania</span><span class="sxs-lookup"><span data-stu-id="5a4ec-176">Publishing Areas</span></span>

<span data-ttu-id="5a4ec-177">Wszystkie `*.cshtml` i `wwwroot/**` pliki są publikowane dane wyjściowe kiedy `<Project Sdk="Microsoft.NET.Sdk.Web">` znajduje się w *.csproj* pliku.</span><span class="sxs-lookup"><span data-stu-id="5a4ec-177">All `*.cshtml` and `wwwroot/**` files are published to output when `<Project Sdk="Microsoft.NET.Sdk.Web">` is included in the *.csproj* file.</span></span>
