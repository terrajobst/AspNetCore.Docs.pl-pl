---
title: Dodaj widok do aplikacji ASP.NET Core MVC
author: rick-anderson
description: Dodawanie widoku na prostej aplikacji ASP.NET Core MVC
ms.author: riande
ms.date: 03/04/2017
uid: tutorials/first-mvc-app/adding-view
ms.openlocfilehash: 321ffd6b0168d4befc950a58035d19561e879491
ms.sourcegitcommit: ec71fd5a988f927ae301813aae5ff764feb3bb6a
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/12/2019
ms.locfileid: "54249454"
---
# <a name="add-a-view-to-an-aspnet-core-mvc-app"></a><span data-ttu-id="a9309-103">Dodaj widok do aplikacji ASP.NET Core MVC</span><span class="sxs-lookup"><span data-stu-id="a9309-103">Add a view to an ASP.NET Core MVC app</span></span>

<span data-ttu-id="a9309-104">Przez [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="a9309-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="a9309-105">W tej sekcji zmodyfikujesz `HelloWorldController` klasy [Razor](xref:mvc/views/razor) widok plików klarownie hermetyzacji proces generowania odpowiedzi HTML do klienta.</span><span class="sxs-lookup"><span data-stu-id="a9309-105">In this section you modify the `HelloWorldController` class to use [Razor](xref:mvc/views/razor) view files to cleanly encapsulate the process of generating HTML responses to a client.</span></span>

<span data-ttu-id="a9309-106">Utworzysz widok pliku szablonu przy użyciu aparatu Razor.</span><span class="sxs-lookup"><span data-stu-id="a9309-106">You create a view template file using Razor.</span></span> <span data-ttu-id="a9309-107">Szablony widoku razor mają *.cshtml* rozszerzenie pliku.</span><span class="sxs-lookup"><span data-stu-id="a9309-107">Razor-based view templates have a *.cshtml* file extension.</span></span> <span data-ttu-id="a9309-108">Zapewniają prosty sposób tworzenia danych wyjściowych HTML przy użyciu C#.</span><span class="sxs-lookup"><span data-stu-id="a9309-108">They provide an elegant way to create HTML output with C#.</span></span>

<span data-ttu-id="a9309-109">Obecnie `Index` metoda zwraca ciąg zawierający komunikat, który jest ustalony w klasie kontrolera.</span><span class="sxs-lookup"><span data-stu-id="a9309-109">Currently the `Index` method returns a string with a message that's hard-coded in the controller class.</span></span> <span data-ttu-id="a9309-110">W `HelloWorldController` klasy, Zastąp `Index` metoda następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="a9309-110">In the `HelloWorldController` class, replace the `Index` method with the following code:</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/HelloWorldController.cs?name=snippet_4)]

<span data-ttu-id="a9309-111">Powyższy kod wywołuje kontrolera <xref:Microsoft.AspNetCore.Mvc.Controller.View*> metody.</span><span class="sxs-lookup"><span data-stu-id="a9309-111">The preceding code calls the controller's <xref:Microsoft.AspNetCore.Mvc.Controller.View*> method.</span></span> <span data-ttu-id="a9309-112">Szablonu widoku jest używane do generowania odpowiedzi HTML.</span><span class="sxs-lookup"><span data-stu-id="a9309-112">It uses a view template to generate an HTML response.</span></span> <span data-ttu-id="a9309-113">Metody kontrolera (znany także jako *metod akcji*), takie jak `Index` ogólnie zwracany przez metodę powyżej <xref:Microsoft.AspNetCore.Mvc.IActionResult> (lub klasą pochodną <xref:Microsoft.AspNetCore.Mvc.ActionResult>), nie jest typem, takich jak `string`.</span><span class="sxs-lookup"><span data-stu-id="a9309-113">Controller methods (also known as *action methods*), such as the `Index` method above, generally return an <xref:Microsoft.AspNetCore.Mvc.IActionResult> (or a class derived from <xref:Microsoft.AspNetCore.Mvc.ActionResult>), not a type like `string`.</span></span>

## <a name="add-a-view"></a><span data-ttu-id="a9309-114">Dodawanie widoku</span><span class="sxs-lookup"><span data-stu-id="a9309-114">Add a view</span></span>

<!-- VS -------------------------->
# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="a9309-115">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a9309-115">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="a9309-116">Kliknij prawym przyciskiem myszy *widoków* folder, a następnie **Dodaj > Nowy Folder** i nadaj folderowi *HelloWorld*.</span><span class="sxs-lookup"><span data-stu-id="a9309-116">Right click on the *Views* folder, and then **Add > New Folder** and name the folder *HelloWorld*.</span></span>

* <span data-ttu-id="a9309-117">Kliknij prawym przyciskiem myszy *widoków/HelloWorld* folder, a następnie **Dodaj > Nowy element**.</span><span class="sxs-lookup"><span data-stu-id="a9309-117">Right click on the *Views/HelloWorld* folder, and then **Add > New Item**.</span></span>

* <span data-ttu-id="a9309-118">W **Dodaj nowy element - MvcMovie** okna dialogowego</span><span class="sxs-lookup"><span data-stu-id="a9309-118">In the **Add New Item - MvcMovie** dialog</span></span>

  * <span data-ttu-id="a9309-119">W polu wyszukiwania w prawym górnym rogu, wprowadź *widoku*</span><span class="sxs-lookup"><span data-stu-id="a9309-119">In the search box in the upper-right, enter *view*</span></span>

  * <span data-ttu-id="a9309-120">Wybierz **widoku Razor**</span><span class="sxs-lookup"><span data-stu-id="a9309-120">Select **Razor View**</span></span>

  * <span data-ttu-id="a9309-121">Zachowaj **nazwa** polu wartość *Index.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="a9309-121">Keep the **Name** box value, *Index.cshtml*.</span></span>

  * <span data-ttu-id="a9309-122">Wybierz **Dodaj**</span><span class="sxs-lookup"><span data-stu-id="a9309-122">Select **Add**</span></span>

![Dodaj okno dialogowe Nowy element](adding-view/_static/add_view.png)

<!-- Code -------------------------->
# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="a9309-124">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="a9309-124">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="a9309-125">Dodaj `Index` wyświetlić `HelloWorldController`.</span><span class="sxs-lookup"><span data-stu-id="a9309-125">Add an `Index` view for the `HelloWorldController`.</span></span>

* <span data-ttu-id="a9309-126">Dodaj nowy folder o nazwie *widoków/HelloWorld*.</span><span class="sxs-lookup"><span data-stu-id="a9309-126">Add a new folder named *Views/HelloWorld*.</span></span>
* <span data-ttu-id="a9309-127">Dodaj nowy plik do *widoków/HelloWorld* nazwa folderu *Index.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="a9309-127">Add a new file to the *Views/HelloWorld* folder name *Index.cshtml*.</span></span>

<!-- Mac -------------------------->
# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="a9309-128">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="a9309-128">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="a9309-129">Kliknij prawym przyciskiem myszy *widoków* folder, a następnie **Dodaj > Nowy Folder** i nadaj folderowi *HelloWorld*.</span><span class="sxs-lookup"><span data-stu-id="a9309-129">Right click on the *Views* folder, and then **Add > New Folder** and name the folder *HelloWorld*.</span></span>
* <span data-ttu-id="a9309-130">Kliknij prawym przyciskiem myszy *widoków/HelloWorld* folder, a następnie **Dodaj > Nowy plik**.</span><span class="sxs-lookup"><span data-stu-id="a9309-130">Right click on the *Views/HelloWorld* folder, and then **Add > New File**.</span></span>
* <span data-ttu-id="a9309-131">W **nowy plik** okno dialogowe:</span><span class="sxs-lookup"><span data-stu-id="a9309-131">In the **New File** dialog:</span></span>

  * <span data-ttu-id="a9309-132">Wybierz **Web** w okienku po lewej stronie.</span><span class="sxs-lookup"><span data-stu-id="a9309-132">Select **Web** in the left pane.</span></span>
  * <span data-ttu-id="a9309-133">Wybierz **plik HTML pusty** w środkowym okienku.</span><span class="sxs-lookup"><span data-stu-id="a9309-133">Select **Empty HTML file** in the center pane.</span></span>
  * <span data-ttu-id="a9309-134">Typ *Index.cshtml* w **nazwa** pole.</span><span class="sxs-lookup"><span data-stu-id="a9309-134">Type *Index.cshtml* in the **Name** box.</span></span>
  * <span data-ttu-id="a9309-135">Wybierz **nowe**.</span><span class="sxs-lookup"><span data-stu-id="a9309-135">Select **New**.</span></span>

![Dodaj okno dialogowe Nowy element](adding-view/_static/add_view.png)

---  
<!-- End of VS tabs -->

<span data-ttu-id="a9309-137">Zastąp zawartość *Views/HelloWorld/Index.cshtml* plik widoku Razor następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="a9309-137">Replace the contents of the *Views/HelloWorld/Index.cshtml* Razor view file with the following:</span></span>

[!code-HTML[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Views/HelloWorld/Index1.cshtml?highlight=7)]

<span data-ttu-id="a9309-138">Przejdź do adresu `https://localhost:xxxx/HelloWorld`.</span><span class="sxs-lookup"><span data-stu-id="a9309-138">Navigate to `https://localhost:xxxx/HelloWorld`.</span></span> <span data-ttu-id="a9309-139">`Index` Method in Class metoda `HelloWorldController` nie znacznie; został uruchomiony instrukcji `return View();`, które określone metody należy używać pliku szablonu widoku do renderowania odpowiedzi do przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="a9309-139">The `Index` method in the `HelloWorldController` didn't do much; it ran the statement `return View();`, which specified that the method should use a view template file to render a response to the browser.</span></span> <span data-ttu-id="a9309-140">Ponieważ nie zostały jawnie określić nazwę pliku szablonu widoku, MVC używa domyślnie *Index.cshtml* plik widoku w */widoków/HelloWorld* folderu.</span><span class="sxs-lookup"><span data-stu-id="a9309-140">Because you didn't explicitly specify the name of the view template file, MVC defaulted to using the *Index.cshtml* view file in the */Views/HelloWorld* folder.</span></span> <span data-ttu-id="a9309-141">Na poniższym obrazie przedstawiono ciąg "Hello z naszych Wyświetl szablon"!</span><span class="sxs-lookup"><span data-stu-id="a9309-141">The image below shows the string "Hello from our View Template!"</span></span> <span data-ttu-id="a9309-142">zakodowane w widoku.</span><span class="sxs-lookup"><span data-stu-id="a9309-142">hard-coded in the view.</span></span>

![Okno przeglądarki](~/tutorials/first-mvc-app/adding-view/_static/hell_template.png)

## <a name="change-views-and-layout-pages"></a><span data-ttu-id="a9309-144">Zmień widoki i układ strony</span><span class="sxs-lookup"><span data-stu-id="a9309-144">Change views and layout pages</span></span>

<span data-ttu-id="a9309-145">Wybierz linki menu (**MvcMovie**, **Home**, i **zachowania**).</span><span class="sxs-lookup"><span data-stu-id="a9309-145">Select the menu links (**MvcMovie**, **Home**, and **Privacy**).</span></span> <span data-ttu-id="a9309-146">Każda strona zawiera ten sam układ menu.</span><span class="sxs-lookup"><span data-stu-id="a9309-146">Each page shows the same menu layout.</span></span> <span data-ttu-id="a9309-147">Układ menu jest zaimplementowana w *Views/Shared/_Layout.cshtml* pliku.</span><span class="sxs-lookup"><span data-stu-id="a9309-147">The menu layout is implemented in the *Views/Shared/_Layout.cshtml* file.</span></span> <span data-ttu-id="a9309-148">Otwórz *Views/Shared/_Layout.cshtml* pliku.</span><span class="sxs-lookup"><span data-stu-id="a9309-148">Open the *Views/Shared/_Layout.cshtml* file.</span></span>

<span data-ttu-id="a9309-149">[Układ](xref:mvc/views/layout) Szablony umożliwiają określ układ kontenera HTML Twojej witryny w jednym miejscu, a następnie zastosować je na wielu stronach w witrynie.</span><span class="sxs-lookup"><span data-stu-id="a9309-149">[Layout](xref:mvc/views/layout) templates allow you to specify the HTML container layout of your site in one place and then apply it across multiple pages in your site.</span></span> <span data-ttu-id="a9309-150">Znajdź `@RenderBody()` wiersza.</span><span class="sxs-lookup"><span data-stu-id="a9309-150">Find the `@RenderBody()` line.</span></span> <span data-ttu-id="a9309-151">`RenderBody` jest symbolem zastępczym, gdzie wszystkie widoku specyficzne dla strony, możesz utworzyć pokaz, *opakowane* w stronę układu.</span><span class="sxs-lookup"><span data-stu-id="a9309-151">`RenderBody` is a placeholder where all the view-specific pages you create show up, *wrapped* in the layout page.</span></span> <span data-ttu-id="a9309-152">Na przykład w przypadku wybrania **zachowania** linku **Views/Home/Privacy.cshtml** renderowania widoku w `RenderBody` metody.</span><span class="sxs-lookup"><span data-stu-id="a9309-152">For example, if you select the **Privacy** link, the **Views/Home/Privacy.cshtml** view is rendered inside the `RenderBody` method.</span></span>

## <a name="change-the-title-footer-and-menu-link-in-the-layout-file"></a><span data-ttu-id="a9309-153">Zmień łącze tytuł, stopki i menu, w pliku układu</span><span class="sxs-lookup"><span data-stu-id="a9309-153">Change the title, footer, and menu link in the layout file</span></span>

* <span data-ttu-id="a9309-154">W elementach tytułu i stopki zmienić `MvcMovie` do `Movie App`.</span><span class="sxs-lookup"><span data-stu-id="a9309-154">In the title and footer elements, change `MvcMovie` to `Movie App`.</span></span>
* <span data-ttu-id="a9309-155">Zmień element kotwicy `<a class="navbar-brand" asp-area="" asp-controller="Home" asp-action="Index">MvcMovie</a>` do `<a class="navbar-brand" asp-controller="Movies" asp-action="Index">Movie App</a>`.</span><span class="sxs-lookup"><span data-stu-id="a9309-155">Change the anchor element `<a class="navbar-brand" asp-area="" asp-controller="Home" asp-action="Index">MvcMovie</a>` to `<a class="navbar-brand" asp-controller="Movies" asp-action="Index">Movie App</a>`.</span></span>

<span data-ttu-id="a9309-156">Następujący kod przedstawia podświetlony zmiany:</span><span class="sxs-lookup"><span data-stu-id="a9309-156">The following markup shows the highlighted changes:</span></span>

[!code-html[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Views/Shared/_Layout.cshtml?highlight=6,24)]

<span data-ttu-id="a9309-157">W poprzednim znaczników `asp-area` [atrybut Pomocnik tagu kotwicy](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper) został pominięty, ponieważ ta aplikacja nie używa [obszarów](xref:mvc/controllers/areas).</span><span class="sxs-lookup"><span data-stu-id="a9309-157">In the preceding markup, the `asp-area` [anchor Tag Helper attribute](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper) was omitted because this app is not using [Areas](xref:mvc/controllers/areas).</span></span>

<!-- Routing has changed in 2.2, it's going to the last route.
>[!WARNING]
> We haven't implemented the `Movies` controller yet, so if you click the `Movie App` link, you get a 404 (Not found) error.
-->

<span data-ttu-id="a9309-158">**Uwaga**: `Movies` Kontrolera nie została zaimplementowana.</span><span class="sxs-lookup"><span data-stu-id="a9309-158">**Note**: The `Movies` controller has not been implemented.</span></span> <span data-ttu-id="a9309-159">W tym momencie `Movie App` link nie działa.</span><span class="sxs-lookup"><span data-stu-id="a9309-159">At this point, the `Movie App` link is not functional.</span></span>

<span data-ttu-id="a9309-160">Zapisać zmiany i wybierz **zachowania** łącza.</span><span class="sxs-lookup"><span data-stu-id="a9309-160">Save your changes and select the **Privacy** link.</span></span> <span data-ttu-id="a9309-161">Zwróć uwagę, jak Wyświetla tytuł, na karcie przeglądarki **ochrona prywatności — aplikacja filmu** zamiast **ochrona prywatności — filmu Mvc**:</span><span class="sxs-lookup"><span data-stu-id="a9309-161">Notice how the title on the browser tab displays **Privacy - Movie App** instead of **Privacy - Mvc Movie**:</span></span>

![Karta Prywatność](~/tutorials/first-mvc-app/adding-view/_static/about2.png)

<span data-ttu-id="a9309-163">Wybierz **Home** link i zwróć uwagę, tekst tytułu i zakotwiczenia także wyświetlać **aplikacja filmu**.</span><span class="sxs-lookup"><span data-stu-id="a9309-163">Select the **Home** link and notice that the title and anchor text also display **Movie App**.</span></span> <span data-ttu-id="a9309-164">Byliśmy w stanie wprowadzić zmianę, jeden raz w szablonie układ, i ma wszystkich stron w witrynie odzwierciedlają nowego tekstu linku i nowy tytuł.</span><span class="sxs-lookup"><span data-stu-id="a9309-164">We were able to make the change once in the layout template and have all pages on the site reflect the new link text and new title.</span></span>

<span data-ttu-id="a9309-165">Sprawdź *Views/_ViewStart.cshtml* pliku:</span><span class="sxs-lookup"><span data-stu-id="a9309-165">Examine the *Views/_ViewStart.cshtml* file:</span></span>

```HTML
@{
    Layout = "_Layout";
}
```

<span data-ttu-id="a9309-166">*Views/_ViewStart.cshtml* wiąże plik *Views/Shared/_Layout.cshtml* pliku do każdego widoku.</span><span class="sxs-lookup"><span data-stu-id="a9309-166">The *Views/_ViewStart.cshtml* file brings in the *Views/Shared/_Layout.cshtml* file to each view.</span></span> <span data-ttu-id="a9309-167">`Layout` Właściwość można ustawić inny układ widoku lub ustaw go na `null` więc plik układu nie będą używane.</span><span class="sxs-lookup"><span data-stu-id="a9309-167">The `Layout` property can be used to set a different layout view, or set it to `null` so no layout file will be used.</span></span>

<span data-ttu-id="a9309-168">Zmienianie tytułu i `<h2>` elementu *Views/HelloWorld/Index.cshtml* Wyświetl plik:</span><span class="sxs-lookup"><span data-stu-id="a9309-168">Change the title and `<h2>` element of the *Views/HelloWorld/Index.cshtml* view file:</span></span>

[!code-HTML[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/HelloWorld/Index2.cshtml?highlight=2,5)]

<span data-ttu-id="a9309-169">Tytuł i `<h2>` element są nieco inne w przypadku, aby było widać, które fragmentem kodu zmienia wyświetlania.</span><span class="sxs-lookup"><span data-stu-id="a9309-169">The title and `<h2>` element are slightly different so you can see which bit of code changes the display.</span></span>

<span data-ttu-id="a9309-170">`ViewData["Title"] = "Movie List";` w kodzie powyżej zestawy `Title` właściwość `ViewData` słownik "Listę programów filmu".</span><span class="sxs-lookup"><span data-stu-id="a9309-170">`ViewData["Title"] = "Movie List";` in the code above sets the `Title` property of the `ViewData` dictionary to "Movie List".</span></span> <span data-ttu-id="a9309-171">`Title` Właściwość jest używana w `<title>` elementu HTML na stronę układu:</span><span class="sxs-lookup"><span data-stu-id="a9309-171">The `Title` property is used in the `<title>` HTML element in the layout page:</span></span>

```HTML
<title>@ViewData["Title"] - Movie App</title>
   ```

<span data-ttu-id="a9309-172">Zapisać zmiany i przejdź do `https://localhost:xxxx/HelloWorld`.</span><span class="sxs-lookup"><span data-stu-id="a9309-172">Save the change and navigate to `https://localhost:xxxx/HelloWorld`.</span></span> <span data-ttu-id="a9309-173">Zauważ, że tytuł przeglądarki, podstawowego nagłówka i dodatkowych nagłówków zostały zmienione.</span><span class="sxs-lookup"><span data-stu-id="a9309-173">Notice that the browser title, the primary heading, and the secondary headings have changed.</span></span> <span data-ttu-id="a9309-174">(Jeśli nie widzisz zmian w przeglądarce, prawdopodobnie przeglądasz zawartość z pamięci podręcznej.</span><span class="sxs-lookup"><span data-stu-id="a9309-174">(If you don't see changes in the browser, you might be viewing cached content.</span></span> <span data-ttu-id="a9309-175">Naciśnij klawisze Ctrl + F5 w przeglądarce, aby wymusić odpowiedzi z serwera do załadowania.) Tytuł przeglądarki jest tworzony z `ViewData["Title"]` jest ustawiany *Index.cshtml* wyświetlić szablonu i dodatkowych "-Movie App" dodane w pliku układu.</span><span class="sxs-lookup"><span data-stu-id="a9309-175">Press Ctrl+F5 in your browser to force the response from the server to be loaded.) The browser title is created with `ViewData["Title"]` we set in the *Index.cshtml* view template and the additional "- Movie App" added in the layout file.</span></span>

<span data-ttu-id="a9309-176">Należy również zauważyć, jak zawartości *Index.cshtml* szablon widoku została scalona z *Views/Shared/_Layout.cshtml* Wyświetl szablon i pojedynczą odpowiedź HTML był wysyłany do przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="a9309-176">Also notice how the content in the *Index.cshtml* view template was merged with the *Views/Shared/_Layout.cshtml* view template and a single HTML response was sent to the browser.</span></span> <span data-ttu-id="a9309-177">Układ Szablony ułatwiają naprawdę wprowadzić zmiany, które są stosowane dla wszystkich stron w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="a9309-177">Layout templates make it really easy to make changes that apply across all of the pages in your application.</span></span> <span data-ttu-id="a9309-178">Aby dowiedzieć się więcej, zobacz [układ](xref:mvc/views/layout).</span><span class="sxs-lookup"><span data-stu-id="a9309-178">To learn more see [Layout](xref:mvc/views/layout).</span></span>

![Widok listy filmu](~/tutorials/first-mvc-app/adding-view/_static/hell3.png)

<span data-ttu-id="a9309-180">Nasze trochę "dane" (w tym przypadku "Hello z naszych Wyświetl szablon!"</span><span class="sxs-lookup"><span data-stu-id="a9309-180">Our little bit of "data" (in this case the "Hello from our View Template!"</span></span> <span data-ttu-id="a9309-181">komunikat) jest ustalony, mimo że.</span><span class="sxs-lookup"><span data-stu-id="a9309-181">message) is hard-coded, though.</span></span> <span data-ttu-id="a9309-182">Aplikacja MVC ma "V" (view) i masz "C" (kontroler), ale nie "M" (model) jeszcze.</span><span class="sxs-lookup"><span data-stu-id="a9309-182">The MVC application has a "V" (view) and you've got a "C" (controller), but no "M" (model) yet.</span></span>

## <a name="passing-data-from-the-controller-to-the-view"></a><span data-ttu-id="a9309-183">Przekazywanie danych z kontrolera do widoku</span><span class="sxs-lookup"><span data-stu-id="a9309-183">Passing Data from the Controller to the View</span></span>

<span data-ttu-id="a9309-184">Akcji kontrolera są wywoływane w odpowiedzi na przychodzące żądanie adresu URL.</span><span class="sxs-lookup"><span data-stu-id="a9309-184">Controller actions are invoked in response to an incoming URL request.</span></span> <span data-ttu-id="a9309-185">Klasa kontrolera jest, gdzie są zapisywane kod obsługujący żądania przychodzące przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="a9309-185">A controller class is where the code is written that handles the incoming browser requests.</span></span> <span data-ttu-id="a9309-186">Kontroler pobiera dane ze źródła danych i decyduje o rodzaju odpowiedzi do odesłania do przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="a9309-186">The controller retrieves data from a data source and decides what type of response to send back to the browser.</span></span> <span data-ttu-id="a9309-187">Wyświetlanie szablonów może służyć za pomocą kontrolera do generowania i formatowanie odpowiedzi HTML do przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="a9309-187">View templates can be used from a controller to generate and format an HTML response to the browser.</span></span>

<span data-ttu-id="a9309-188">Kontrolery są odpowiedzialne za świadczenie danych wymagane dla szablonu widoku do renderowania odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="a9309-188">Controllers are responsible for providing the data required in order for a view template to render a response.</span></span> <span data-ttu-id="a9309-189">Najlepszym rozwiązaniem jest: Wyświetl szablony powinny **nie** wykonania logiki biznesowej lub bezpośredniej interakcji z bazą danych.</span><span class="sxs-lookup"><span data-stu-id="a9309-189">A best practice: View templates should **not** perform business logic or interact with a database directly.</span></span> <span data-ttu-id="a9309-190">Przeciwnie Wyświetl szablon powinny działać tylko w przypadku danych, który znajduje się do niego przez kontroler.</span><span class="sxs-lookup"><span data-stu-id="a9309-190">Rather, a view template should work only with the data that's provided to it by the controller.</span></span> <span data-ttu-id="a9309-191">Obsługa tego "separacji" pomaga kod czystego, zakresie testować i obsługiwać.</span><span class="sxs-lookup"><span data-stu-id="a9309-191">Maintaining this "separation of concerns" helps keep the code clean, testable, and maintainable.</span></span>

<span data-ttu-id="a9309-192">Obecnie `Welcome` method in Class metoda `HelloWorldController` klasy przyjmuje `name` i `ID` parametru, a następnie dane wyjściowe wartości bezpośrednio do przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="a9309-192">Currently, the `Welcome` method in the `HelloWorldController` class takes a `name` and a `ID` parameter and then outputs the values directly to the browser.</span></span> <span data-ttu-id="a9309-193">Zamiast kontrolera renderowania tej odpowiedzi jako ciąg, Zmień kontroler zamiast tego użyć szablonu widoku.</span><span class="sxs-lookup"><span data-stu-id="a9309-193">Rather than have the controller render this response as a string, change the controller to use a view template instead.</span></span> <span data-ttu-id="a9309-194">Wyświetl szablon generuje odpowiedzi dynamicznej, co oznacza, że odpowiednich bitów danych muszą być przekazywane z kontrolera do widoku w celu wygenerowania odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="a9309-194">The view template generates a dynamic response, which means that appropriate bits of data must be passed from the controller to the view in order to generate the response.</span></span> <span data-ttu-id="a9309-195">W tym celu o kontrolera, umieść dane dynamiczne (parametry) wymagającym szablon widoku w `ViewData` słownik, który szablon widoku może uzyskać dostęp.</span><span class="sxs-lookup"><span data-stu-id="a9309-195">Do this by having the controller put the dynamic data (parameters) that the view template needs in a `ViewData` dictionary that the view template can then access.</span></span>

<span data-ttu-id="a9309-196">W *HelloWorldController.cs*, zmień `Welcome` metody w celu dodania `Message` i `NumTimes` wartość `ViewData` słownika.</span><span class="sxs-lookup"><span data-stu-id="a9309-196">In *HelloWorldController.cs*, change the `Welcome` method to add a `Message` and `NumTimes` value to the `ViewData` dictionary.</span></span> <span data-ttu-id="a9309-197">`ViewData` Słownik jest obiekt dynamiczny, co oznacza, można użyć dowolnego typu; `ViewData` obiekt nie ma zdefiniowanej właściwości do momentu coś znajdującym się w nim umieścić.</span><span class="sxs-lookup"><span data-stu-id="a9309-197">The `ViewData` dictionary is a dynamic object, which means any type can be used; the `ViewData` object has no defined properties until you put something inside it.</span></span> <span data-ttu-id="a9309-198">[System powiązań modelu MVC](xref:mvc/models/model-binding) automatycznie mapuje nazwanych parametrów (`name` i `numTimes`) z ciągu zapytania w pasku adresu do parametrów w metodzie.</span><span class="sxs-lookup"><span data-stu-id="a9309-198">The [MVC model binding system](xref:mvc/models/model-binding) automatically maps the named parameters (`name` and `numTimes`) from the query string in the address bar to parameters in your method.</span></span> <span data-ttu-id="a9309-199">Pełne *HelloWorldController.cs* pliku wygląda następująco:</span><span class="sxs-lookup"><span data-stu-id="a9309-199">The complete *HelloWorldController.cs* file looks like this:</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/HelloWorldController.cs?name=snippet_5)]

<span data-ttu-id="a9309-200">`ViewData` Obiekt słownika zawiera dane, które zostaną przekazane do widoku.</span><span class="sxs-lookup"><span data-stu-id="a9309-200">The `ViewData` dictionary object contains data that will be passed to the view.</span></span> 

<span data-ttu-id="a9309-201">Tworzenie szablonu-Zapraszamy wyświetlanie o nazwie *Views/HelloWorld/Welcome.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="a9309-201">Create a Welcome view template named *Views/HelloWorld/Welcome.cshtml*.</span></span>

<span data-ttu-id="a9309-202">Utworzysz pętlę w *Welcome.cshtml* Wyświetl szablon, który wyświetla "Hello" `NumTimes`.</span><span class="sxs-lookup"><span data-stu-id="a9309-202">You'll create a loop in the *Welcome.cshtml* view template that displays "Hello" `NumTimes`.</span></span> <span data-ttu-id="a9309-203">Zastąp zawartość *Views/HelloWorld/Welcome.cshtml* następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="a9309-203">Replace the contents of *Views/HelloWorld/Welcome.cshtml* with the following:</span></span>

[!code-html[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/HelloWorld/Welcome.cshtml)]

<span data-ttu-id="a9309-204">Zapisz zmiany i przejdź do następującego adresu URL:</span><span class="sxs-lookup"><span data-stu-id="a9309-204">Save your changes and browse to the following URL:</span></span>

`https://localhost:xxxx/HelloWorld/Welcome?name=Rick&numtimes=4`

<span data-ttu-id="a9309-205">Dane są pobierane z adresu URL i przekazywane do kontrolera, za pomocą [integratora modelu MVC](xref:mvc/models/model-binding) .</span><span class="sxs-lookup"><span data-stu-id="a9309-205">Data is taken from the URL and passed to the controller using the [MVC model binder](xref:mvc/models/model-binding) .</span></span> <span data-ttu-id="a9309-206">Kontroler pakiety danych do `ViewData` słownik i przebiegi, które obiekt widoku.</span><span class="sxs-lookup"><span data-stu-id="a9309-206">The controller packages the data into a `ViewData` dictionary and passes that object to the view.</span></span> <span data-ttu-id="a9309-207">Widok następnie renderuje dane jako kod HTML do przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="a9309-207">The view then renders the data as HTML to the browser.</span></span>

![Widok zachowania powitalnej etykiety i frazy Hello Rick przedstawiono cztery razy](~/tutorials/first-mvc-app/adding-view/_static/rick2.png)

<span data-ttu-id="a9309-209">W przykładzie powyżej `ViewData` słownika zostało użyte do przekazywania danych z kontrolera do widoku.</span><span class="sxs-lookup"><span data-stu-id="a9309-209">In the sample above, the `ViewData` dictionary was used to pass data from the controller to a view.</span></span> <span data-ttu-id="a9309-210">W dalszej części samouczka model widoku jest używany do przekazywania danych za pomocą kontrolera do widoku.</span><span class="sxs-lookup"><span data-stu-id="a9309-210">Later in the tutorial, a view model is used to pass data from a controller to a view.</span></span> <span data-ttu-id="a9309-211">Widok modelu sposobem przekazywania danych jest zwykle znacznie preferowany nad `ViewData` podejście słownika.</span><span class="sxs-lookup"><span data-stu-id="a9309-211">The view model approach to passing data is generally much preferred over the `ViewData` dictionary approach.</span></span> <span data-ttu-id="a9309-212">Zobacz [kiedy używać elementów ViewBag, ViewData i TempData ](http://www.rachelappel.com/when-to-use-viewbag-viewdata-or-tempdata-in-asp-net-mvc-3-applications/) Aby uzyskać więcej informacji.</span><span class="sxs-lookup"><span data-stu-id="a9309-212">See [When to use ViewBag, ViewData, or TempData ](http://www.rachelappel.com/when-to-use-viewbag-viewdata-or-tempdata-in-asp-net-mvc-3-applications/) for more information.</span></span>

<span data-ttu-id="a9309-213">W następnym samouczku utworzeniu bazy danych filmów.</span><span class="sxs-lookup"><span data-stu-id="a9309-213">In the next tutorial, a database of movies is created.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="a9309-214">[Poprzednie](adding-controller.md)
> [dalej](adding-model.md)</span><span class="sxs-lookup"><span data-stu-id="a9309-214">[Previous](adding-controller.md)
[Next](adding-model.md)</span></span>
