---
title: Widoki w programie ASP.NET Core MVC
author: ardalis
description: Dowiedz się, jak widoki obsługiwać prezentacji danych aplikacji i interakcji z użytkownikiem w aplikacji ASP.NET Core MVC.
ms.author: riande
ms.date: 04/03/2019
uid: mvc/views/overview
ms.openlocfilehash: 766996645bc6ef2b6be42d729baf5d57f55b6ddd
ms.sourcegitcommit: 1a7000630e55da90da19b284e1b2f2f13a393d74
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/04/2019
ms.locfileid: "59012802"
---
# <a name="views-in-aspnet-core-mvc"></a><span data-ttu-id="17ea3-103">Widoki w programie ASP.NET Core MVC</span><span class="sxs-lookup"><span data-stu-id="17ea3-103">Views in ASP.NET Core MVC</span></span>

<span data-ttu-id="17ea3-104">Przez [Steve Smith](https://ardalis.com/) i [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="17ea3-104">By [Steve Smith](https://ardalis.com/) and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="17ea3-105">W tym dokumencie wyjaśniono w widokach używanych w aplikacji ASP.NET Core MVC.</span><span class="sxs-lookup"><span data-stu-id="17ea3-105">This document explains views used in ASP.NET Core MVC applications.</span></span> <span data-ttu-id="17ea3-106">Aby uzyskać informacji na temat stron Razor, zobacz [wprowadzenie do stron Razor](xref:razor-pages/index).</span><span class="sxs-lookup"><span data-stu-id="17ea3-106">For information on Razor Pages, see [Introduction to Razor Pages](xref:razor-pages/index).</span></span>

<span data-ttu-id="17ea3-107">We wzorcu Model-View-Controller (MVC) *widoku* obsługuje interakcję prezentacji i użytkownika danych aplikacji.</span><span class="sxs-lookup"><span data-stu-id="17ea3-107">In the Model-View-Controller (MVC) pattern, the *view* handles the app's data presentation and user interaction.</span></span> <span data-ttu-id="17ea3-108">Widok jest szablon HTML z osadzonymi [znaczników Razor](xref:mvc/views/razor).</span><span class="sxs-lookup"><span data-stu-id="17ea3-108">A view is an HTML template with embedded [Razor markup](xref:mvc/views/razor).</span></span> <span data-ttu-id="17ea3-109">Kod znaczników razor jest kod, który wchodzi w interakcję z kod znaczników HTML, aby utworzyć stronę sieci Web, które są wysyłane do klienta.</span><span class="sxs-lookup"><span data-stu-id="17ea3-109">Razor markup is code that interacts with HTML markup to produce a webpage that's sent to the client.</span></span>

<span data-ttu-id="17ea3-110">W aplikacji ASP.NET Core MVC, widoki są *.cshtml* pliki, które używają [języka programowania C#](/dotnet/csharp/) w znacznikach Razor.</span><span class="sxs-lookup"><span data-stu-id="17ea3-110">In ASP.NET Core MVC, views are *.cshtml* files that use the [C# programming language](/dotnet/csharp/) in Razor markup.</span></span> <span data-ttu-id="17ea3-111">Zwykle, Wyświetl pliki są pogrupowane w foldery o nazwie dla każdej aplikacji [kontrolerów](xref:mvc/controllers/actions).</span><span class="sxs-lookup"><span data-stu-id="17ea3-111">Usually, view files are grouped into folders named for each of the app's [controllers](xref:mvc/controllers/actions).</span></span> <span data-ttu-id="17ea3-112">Foldery są przechowywane w *widoków* folder w katalogu głównym aplikacji:</span><span class="sxs-lookup"><span data-stu-id="17ea3-112">The folders are stored in a *Views* folder at the root of the app:</span></span>

![Widoki folder w rozwiązaniu Explorer programu Visual Studio jest otwarty z folderu macierzystego otwarty, aby wyświetlić pliki About.cshtml, Contact.cshtml i Index.cshtml](overview/_static/views_solution_explorer.png)

<span data-ttu-id="17ea3-114">*Home* kontrolera jest reprezentowany przez *Home* folder wewnątrz *widoków* folderu.</span><span class="sxs-lookup"><span data-stu-id="17ea3-114">The *Home* controller is represented by a *Home* folder inside the *Views* folder.</span></span> <span data-ttu-id="17ea3-115">*Home* folder zawiera widoki dla *o*, *skontaktuj się z pomocą*, i *indeksu* stron sieci Web (głównej).</span><span class="sxs-lookup"><span data-stu-id="17ea3-115">The *Home* folder contains the views for the *About*, *Contact*, and *Index* (homepage) webpages.</span></span> <span data-ttu-id="17ea3-116">Gdy użytkownik zażąda, jeden z tych trzech stronach akcji kontrolera w *Home* kontrolera ustalić, które trzech widoków jest używane do tworzenia i zwrócić strony sieci Web do użytkownika.</span><span class="sxs-lookup"><span data-stu-id="17ea3-116">When a user requests one of these three webpages, controller actions in the *Home* controller determine which of the three views is used to build and return a webpage to the user.</span></span>

<span data-ttu-id="17ea3-117">Użyj [układy](xref:mvc/views/layout) sekcje spójne strony sieci Web i obniżyć powtarzających się kodów.</span><span class="sxs-lookup"><span data-stu-id="17ea3-117">Use [layouts](xref:mvc/views/layout) to provide consistent webpage sections and reduce code repetition.</span></span> <span data-ttu-id="17ea3-118">Układy często zawierają nagłówka, elementy menu i nawigacji i stopka.</span><span class="sxs-lookup"><span data-stu-id="17ea3-118">Layouts often contain the header, navigation and menu elements, and the footer.</span></span> <span data-ttu-id="17ea3-119">Nagłówek i stopka zawierają zwykle standardowy kod znaczników dla wielu elementów metadanych i linki do zasobów skryptu i stylu.</span><span class="sxs-lookup"><span data-stu-id="17ea3-119">The header and footer usually contain boilerplate markup for many metadata elements and links to script and style assets.</span></span> <span data-ttu-id="17ea3-120">Układy pomóc w uniknięciu ten kod znaczników standardowy, w sekcji Widoki.</span><span class="sxs-lookup"><span data-stu-id="17ea3-120">Layouts help you avoid this boilerplate markup in your views.</span></span>

<span data-ttu-id="17ea3-121">[Widoki częściowe](xref:mvc/views/partial) zmniejszyć zduplikowania kodu, umożliwiając zarządzanie wielokrotnego użytku części widoków.</span><span class="sxs-lookup"><span data-stu-id="17ea3-121">[Partial views](xref:mvc/views/partial) reduce code duplication by managing reusable parts of views.</span></span> <span data-ttu-id="17ea3-122">Na przykład widoku częściowego jest przydatne w przypadku biografia Autor na blogu witryny sieci Web w kilka widoków.</span><span class="sxs-lookup"><span data-stu-id="17ea3-122">For example, a partial view is useful for an author biography on a blog website that appears in several views.</span></span> <span data-ttu-id="17ea3-123">Biografia autor jest wyświetlanie zwykłej zawartości i nie wymagają kodu do wykonania w celu utworzenia zawartości dla strony sieci Web.</span><span class="sxs-lookup"><span data-stu-id="17ea3-123">An author biography is ordinary view content and doesn't require code to execute in order to produce the content for the webpage.</span></span> <span data-ttu-id="17ea3-124">Autor biografia zawartość jest dostępna dla widoku przez powiązanie modelu samodzielnie, dzięki czemu przy użyciu widoku częściowego dla tego typu zawartości jest idealnym rozwiązaniem.</span><span class="sxs-lookup"><span data-stu-id="17ea3-124">Author biography content is available to the view by model binding alone, so using a partial view for this type of content is ideal.</span></span>

<span data-ttu-id="17ea3-125">[Wyświetlanie składników](xref:mvc/views/view-components) są podobne do częściowe widoki, pozwalają one ograniczenia powtarzania kodu, ale są one odpowiednie dla zawartości widoku, który wymaga kod wymagany do uruchomienia na serwerze, aby było możliwe renderowanie strony sieci Web.</span><span class="sxs-lookup"><span data-stu-id="17ea3-125">[View components](xref:mvc/views/view-components) are similar to partial views in that they allow you to reduce repetitive code, but they're appropriate for view content that requires code to run on the server in order to render the webpage.</span></span> <span data-ttu-id="17ea3-126">Składniki widoków są przydatne, gdy renderowanej zawartości wymaga interakcji bazy danych, takie jak koszyk witryny sieci Web.</span><span class="sxs-lookup"><span data-stu-id="17ea3-126">View components are useful when the rendered content requires database interaction, such as for a website shopping cart.</span></span> <span data-ttu-id="17ea3-127">Składniki widoków nie są ograniczone do wiązanie modelu w celu generowania danych wyjściowych strony sieci Web.</span><span class="sxs-lookup"><span data-stu-id="17ea3-127">View components aren't limited to model binding in order to produce webpage output.</span></span>

## <a name="benefits-of-using-views"></a><span data-ttu-id="17ea3-128">Korzyści z używania widoków</span><span class="sxs-lookup"><span data-stu-id="17ea3-128">Benefits of using views</span></span>

<span data-ttu-id="17ea3-129">Widoki pomóc w ustaleniu [separacji](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#separation-of-concerns) w aplikacji MVC, oddzielając znaczników interfejsu użytkownika z innych części aplikacji.</span><span class="sxs-lookup"><span data-stu-id="17ea3-129">Views help to establish [separation of concerns](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#separation-of-concerns) within an MVC app by separating the user interface markup from other parts of the app.</span></span> <span data-ttu-id="17ea3-130">Następujące projektowania SoC sprawia, że aplikacja modułowe oprogramowanie, który zapewnia kilka korzyści:</span><span class="sxs-lookup"><span data-stu-id="17ea3-130">Following SoC design makes your app modular, which provides several benefits:</span></span>

* <span data-ttu-id="17ea3-131">Aplikacja jest łatwiejsze w obsłudze, ponieważ jest lepiej zorganizowany.</span><span class="sxs-lookup"><span data-stu-id="17ea3-131">The app is easier to maintain because it's better organized.</span></span> <span data-ttu-id="17ea3-132">Widoki są ogólnie pogrupowane według funkcji aplikacji.</span><span class="sxs-lookup"><span data-stu-id="17ea3-132">Views are generally grouped by app feature.</span></span> <span data-ttu-id="17ea3-133">Dzięki temu można łatwiej znaleźć widoki pokrewne podczas pracy z funkcją.</span><span class="sxs-lookup"><span data-stu-id="17ea3-133">This makes it easier to find related views when working on a feature.</span></span>
* <span data-ttu-id="17ea3-134">Części aplikacji są luźno powiązane.</span><span class="sxs-lookup"><span data-stu-id="17ea3-134">The parts of the app are loosely coupled.</span></span> <span data-ttu-id="17ea3-135">Można tworzyć i aktualizować widoków aplikacji niezależnie od składniki dostępu logikę i dane biznesowe.</span><span class="sxs-lookup"><span data-stu-id="17ea3-135">You can build and update the app's views separately from the business logic and data access components.</span></span> <span data-ttu-id="17ea3-136">Widoki aplikacji można modyfikować, bez konieczności aktualizacji innych części aplikacji.</span><span class="sxs-lookup"><span data-stu-id="17ea3-136">You can modify the views of the app without necessarily having to update other parts of the app.</span></span>
* <span data-ttu-id="17ea3-137">Jest to łatwiejsze testowanie części interfejsu użytkownika aplikacji, ponieważ opinie są oddzielne zespoły.</span><span class="sxs-lookup"><span data-stu-id="17ea3-137">It's easier to test the user interface parts of the app because the views are separate units.</span></span>
* <span data-ttu-id="17ea3-138">Ze względu na lepsze organizacji jest mniej prawdopodobne, że należy powtórzyć przypadkowo części interfejsu użytkownika.</span><span class="sxs-lookup"><span data-stu-id="17ea3-138">Due to better organization, it's less likely that you'll accidentally repeat sections of the user interface.</span></span>

## <a name="creating-a-view"></a><span data-ttu-id="17ea3-139">Tworzenie widoku</span><span class="sxs-lookup"><span data-stu-id="17ea3-139">Creating a view</span></span>

<span data-ttu-id="17ea3-140">Widoki, które są specyficzne dla kontrolera są tworzone w *widoków / [ControllerName]* folderu.</span><span class="sxs-lookup"><span data-stu-id="17ea3-140">Views that are specific to a controller are created in the *Views/[ControllerName]* folder.</span></span> <span data-ttu-id="17ea3-141">Widoki, które są współużytkowane przez kontrolery są umieszczane w *widoków/Shared* folderu.</span><span class="sxs-lookup"><span data-stu-id="17ea3-141">Views that are shared among controllers are placed in the *Views/Shared* folder.</span></span> <span data-ttu-id="17ea3-142">Aby utworzyć widok, Dodaj nowy plik i nadaj mu taką samą nazwę jak jej działaniem kontrolera skojarzone z *.cshtml* rozszerzenie pliku.</span><span class="sxs-lookup"><span data-stu-id="17ea3-142">To create a view, add a new file and give it the same name as its associated controller action with the *.cshtml* file extension.</span></span> <span data-ttu-id="17ea3-143">Aby utworzyć widok, który odpowiada za pomocą *o* akcji w *Home* kontroler, tworzyć *About.cshtml* w pliku *widoków domowych*folderu:</span><span class="sxs-lookup"><span data-stu-id="17ea3-143">To create a view that corresponds with the *About* action in the *Home* controller, create an *About.cshtml* file in the *Views/Home* folder:</span></span>

[!code-cshtml[](../../common/samples/WebApplication1/Views/Home/About.cshtml)]

<span data-ttu-id="17ea3-144">*Razor* znaczników, który rozpoczyna się od `@` symboli.</span><span class="sxs-lookup"><span data-stu-id="17ea3-144">*Razor* markup starts with the `@` symbol.</span></span> <span data-ttu-id="17ea3-145">C# instrukcje uruchamiania, umieszczając C# kodu w ramach [bloków kodu Razor](xref:mvc/views/razor#razor-code-blocks) przez nawiasy klamrowe (`{ ... }`).</span><span class="sxs-lookup"><span data-stu-id="17ea3-145">Run C# statements by placing C# code within [Razor code blocks](xref:mvc/views/razor#razor-code-blocks) set off by curly braces (`{ ... }`).</span></span> <span data-ttu-id="17ea3-146">Na przykład, zobacz przypisanie "About", aby `ViewData["Title"]` przedstawionych powyżej.</span><span class="sxs-lookup"><span data-stu-id="17ea3-146">For example, see the assignment of "About" to `ViewData["Title"]` shown above.</span></span> <span data-ttu-id="17ea3-147">Można wyświetlić wartości w pliku HTML, po prostu odwołując się do wartości z `@` symboli.</span><span class="sxs-lookup"><span data-stu-id="17ea3-147">You can display values within HTML by simply referencing the value with the `@` symbol.</span></span> <span data-ttu-id="17ea3-148">Zobacz zawartość `<h2>` i `<h3>` elementów wymienionych powyżej.</span><span class="sxs-lookup"><span data-stu-id="17ea3-148">See the contents of the `<h2>` and `<h3>` elements above.</span></span>

<span data-ttu-id="17ea3-149">Wyświetl zawartość powyżej jest tylko część całą stronę sieci Web, który jest renderowany do użytkownika.</span><span class="sxs-lookup"><span data-stu-id="17ea3-149">The view content shown above is only part of the entire webpage that's rendered to the user.</span></span> <span data-ttu-id="17ea3-150">Resztę układu strony i innych typowych aspektów widoku są określone w innych plikach widoku.</span><span class="sxs-lookup"><span data-stu-id="17ea3-150">The rest of the page's layout and other common aspects of the view are specified in other view files.</span></span> <span data-ttu-id="17ea3-151">Aby dowiedzieć się więcej, zobacz [temat układu](xref:mvc/views/layout).</span><span class="sxs-lookup"><span data-stu-id="17ea3-151">To learn more, see the [Layout topic](xref:mvc/views/layout).</span></span>

## <a name="how-controllers-specify-views"></a><span data-ttu-id="17ea3-152">Jak określić widoki, kontrolery</span><span class="sxs-lookup"><span data-stu-id="17ea3-152">How controllers specify views</span></span>

<span data-ttu-id="17ea3-153">Widoki są zwykle zwracane z działań, jak [ViewResult](/dotnet/api/microsoft.aspnetcore.mvc.viewresult), który jest typem [ActionResult](/dotnet/api/microsoft.aspnetcore.mvc.actionresult).</span><span class="sxs-lookup"><span data-stu-id="17ea3-153">Views are typically returned from actions as a [ViewResult](/dotnet/api/microsoft.aspnetcore.mvc.viewresult), which is a type of [ActionResult](/dotnet/api/microsoft.aspnetcore.mvc.actionresult).</span></span> <span data-ttu-id="17ea3-154">Metodzie akcji mogą tworzyć i zwracać `ViewResult` bezpośrednio, ale który nie jest często wykonywane.</span><span class="sxs-lookup"><span data-stu-id="17ea3-154">Your action method can create and return a `ViewResult` directly, but that isn't commonly done.</span></span> <span data-ttu-id="17ea3-155">Ponieważ większość kontrolerów dziedziczy z [kontrolera](/dotnet/api/microsoft.aspnetcore.mvc.controller), po prostu użyć `View` metody pomocnika do zwrócenia `ViewResult`:</span><span class="sxs-lookup"><span data-stu-id="17ea3-155">Since most controllers inherit from [Controller](/dotnet/api/microsoft.aspnetcore.mvc.controller), you simply use the `View` helper method to return the `ViewResult`:</span></span>

*<span data-ttu-id="17ea3-156">HomeController.cs</span><span class="sxs-lookup"><span data-stu-id="17ea3-156">HomeController.cs</span></span>*

[!code-csharp[](../../common/samples/WebApplication1/Controllers/HomeController.cs?highlight=5&range=16-21)]

<span data-ttu-id="17ea3-157">Po powrocie z tej akcji *About.cshtml* widoku wyświetlane w ostatniej sekcji jest renderowane jako następującej stronie sieci Web:</span><span class="sxs-lookup"><span data-stu-id="17ea3-157">When this action returns, the *About.cshtml* view shown in the last section is rendered as the following webpage:</span></span>

![Strona renderowane w przeglądarce Microsoft Edge — informacje](overview/_static/about-page.png)

<span data-ttu-id="17ea3-159">`View` Metody pomocnika ma kilka przeciążeń.</span><span class="sxs-lookup"><span data-stu-id="17ea3-159">The `View` helper method has several overloads.</span></span> <span data-ttu-id="17ea3-160">Opcjonalnie można określić:</span><span class="sxs-lookup"><span data-stu-id="17ea3-160">You can optionally specify:</span></span>

* <span data-ttu-id="17ea3-161">Jawne widoku do zwrócenia:</span><span class="sxs-lookup"><span data-stu-id="17ea3-161">An explicit view to return:</span></span>

  ```csharp
  return View("Orders");
  ```

* <span data-ttu-id="17ea3-162">A [modelu](xref:mvc/models/model-binding) do przekazania do widoku:</span><span class="sxs-lookup"><span data-stu-id="17ea3-162">A [model](xref:mvc/models/model-binding) to pass to the view:</span></span>

  ```csharp
  return View(Orders);
  ```

* <span data-ttu-id="17ea3-163">Widoku i modelu:</span><span class="sxs-lookup"><span data-stu-id="17ea3-163">Both a view and a model:</span></span>

  ```csharp
  return View("Orders", Orders);
  ```

### <a name="view-discovery"></a><span data-ttu-id="17ea3-164">Widok odnajdywania</span><span class="sxs-lookup"><span data-stu-id="17ea3-164">View discovery</span></span>

<span data-ttu-id="17ea3-165">Po powrocie z akcji widoku proces jest nazywany *widok odnajdywania* ma miejsce.</span><span class="sxs-lookup"><span data-stu-id="17ea3-165">When an action returns a view, a process called *view discovery* takes place.</span></span> <span data-ttu-id="17ea3-166">Ten proces Określa plik, który widok jest używany, na podstawie nazwy widoku.</span><span class="sxs-lookup"><span data-stu-id="17ea3-166">This process determines which view file is used based on the view name.</span></span> 

<span data-ttu-id="17ea3-167">Domyślne zachowanie `View` — metoda (`return View();`) jest do zwrócenia widoku z taką samą nazwę jak metody akcji, w którym jest wywoływana.</span><span class="sxs-lookup"><span data-stu-id="17ea3-167">The default behavior of the `View` method (`return View();`) is to return a view with the same name as the action method from which it's called.</span></span> <span data-ttu-id="17ea3-168">Na przykład *o* `ActionResult` Nazwa metody kontrolera jest używana do wyszukiwania Wyświetl plik o nazwie *About.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="17ea3-168">For example, the *About* `ActionResult` method name of the controller is used to search for a view file named *About.cshtml*.</span></span> <span data-ttu-id="17ea3-169">Po pierwsze, środowisko uruchomieniowe poszukuje *widoków / [ControllerName]* folder widoku.</span><span class="sxs-lookup"><span data-stu-id="17ea3-169">First, the runtime looks in the *Views/[ControllerName]* folder for the view.</span></span> <span data-ttu-id="17ea3-170">Jeśli nie znajdzie zgodnego widoku wyszukiwania *Shared* folder widoku.</span><span class="sxs-lookup"><span data-stu-id="17ea3-170">If it doesn't find a matching view there, it searches the *Shared* folder for the view.</span></span>

<span data-ttu-id="17ea3-171">Nie ma znaczenia, jeśli zwróci niejawnie `ViewResult` z `return View();` lub jawnie przekazać nazwę widoku, aby `View` metody z `return View("<ViewName>");`.</span><span class="sxs-lookup"><span data-stu-id="17ea3-171">It doesn't matter if you implicitly return the `ViewResult` with `return View();` or explicitly pass the view name to the `View` method with `return View("<ViewName>");`.</span></span> <span data-ttu-id="17ea3-172">W obu przypadkach widok odnajdywania wyszukuje pasującego pliku widoku w następującej kolejności:</span><span class="sxs-lookup"><span data-stu-id="17ea3-172">In both cases, view discovery searches for a matching view file in this order:</span></span>

   1. *<span data-ttu-id="17ea3-173">Widoki /\[ControllerName] /\[Nazwa widoku] .cshtml</span><span class="sxs-lookup"><span data-stu-id="17ea3-173">Views/\[ControllerName]/\[ViewName].cshtml</span></span>*
   1. *<span data-ttu-id="17ea3-174">Widoki/udostępnione/\[Nazwa widoku] .cshtml</span><span class="sxs-lookup"><span data-stu-id="17ea3-174">Views/Shared/\[ViewName].cshtml</span></span>*

<span data-ttu-id="17ea3-175">Ścieżka pliku widoku można podać zamiast nazwy widoku.</span><span class="sxs-lookup"><span data-stu-id="17ea3-175">A view file path can be provided instead of a view name.</span></span> <span data-ttu-id="17ea3-176">Jeśli przy użyciu ścieżką bezwzględną, zaczynając od katalogu głównego aplikacji (opcjonalnie, począwszy od "/" lub "~ /"), *.cshtml* rozszerzenia należy określić:</span><span class="sxs-lookup"><span data-stu-id="17ea3-176">If using an absolute path starting at the app root (optionally starting with "/" or "~/"), the *.cshtml* extension must be specified:</span></span>

```csharp
return View("Views/Home/About.cshtml");
```

<span data-ttu-id="17ea3-177">Ścieżki względnej można również użyć, możesz określić widoki w różnych katalogach bez *.cshtml* rozszerzenia.</span><span class="sxs-lookup"><span data-stu-id="17ea3-177">You can also use a relative path to specify views in different directories without the *.cshtml* extension.</span></span> <span data-ttu-id="17ea3-178">Wewnątrz `HomeController`, może zwrócić *indeksu* widoku Twojej *Zarządzaj* widoków ze ścieżką względną:</span><span class="sxs-lookup"><span data-stu-id="17ea3-178">Inside the `HomeController`, you can return the *Index* view of your *Manage* views with a relative path:</span></span>

```csharp
return View("../Manage/Index");
```

<span data-ttu-id="17ea3-179">Podobnie, można wskazać bieżący katalog specyficzne dla kontrolera, za pomocą ". /" prefiks:</span><span class="sxs-lookup"><span data-stu-id="17ea3-179">Similarly, you can indicate the current controller-specific directory with the "./" prefix:</span></span>

```csharp
return View("./About");
```

<span data-ttu-id="17ea3-180">[Widoki częściowe](xref:mvc/views/partial) i [wyświetlanie składników](xref:mvc/views/view-components) używać mechanizmu odnajdywania podobne (ale nie są identyczne).</span><span class="sxs-lookup"><span data-stu-id="17ea3-180">[Partial views](xref:mvc/views/partial) and [view components](xref:mvc/views/view-components) use similar (but not identical) discovery mechanisms.</span></span>

<span data-ttu-id="17ea3-181">Można dostosować domyślnej Konwencji dla jak widoki znajdują się w aplikacji przy użyciu niestandardowego [IViewLocationExpander](/dotnet/api/microsoft.aspnetcore.mvc.razor.iviewlocationexpander).</span><span class="sxs-lookup"><span data-stu-id="17ea3-181">You can customize the default convention for how views are located within the app by using a custom [IViewLocationExpander](/dotnet/api/microsoft.aspnetcore.mvc.razor.iviewlocationexpander).</span></span>

<span data-ttu-id="17ea3-182">Widok odnajdywania zależy od tego, znajdowanie Wyświetl pliki według nazwy pliku.</span><span class="sxs-lookup"><span data-stu-id="17ea3-182">View discovery relies on finding view files by file name.</span></span> <span data-ttu-id="17ea3-183">Jeśli źródłowy system plików jest uwzględniana wielkość liter, nazwy widoku są prawdopodobnie z uwzględnieniem wielkości liter.</span><span class="sxs-lookup"><span data-stu-id="17ea3-183">If the underlying file system is case sensitive, view names are probably case sensitive.</span></span> <span data-ttu-id="17ea3-184">Zgodność systemów operacyjnych Uwzględnij wielkość liter, między kontrolera i nazwy akcji i skojarzony widok folderów i plików.</span><span class="sxs-lookup"><span data-stu-id="17ea3-184">For compatibility across operating systems, match case between controller and action names and associated view folders and file names.</span></span> <span data-ttu-id="17ea3-185">Jeśli wystąpi błąd, którego nie można odnaleźć pliku widoku podczas pracy w systemie plików rozróżniana wielkość liter, upewnij się, że wielkość liter w wyrazie odpowiada między plikiem żądany widok i nazwa pliku rzeczywiste widoku.</span><span class="sxs-lookup"><span data-stu-id="17ea3-185">If you encounter an error that a view file can't be found while working with a case-sensitive file system, confirm that the casing matches between the requested view file and the actual view file name.</span></span>

<span data-ttu-id="17ea3-186">Postępuj zgodnie z najlepszym rozwiązaniem jest organizowania struktury plików dla widoków w taki sposób uwzględnić relacje między kontrolerów, akcji i widoki, łatwość konserwacji i przejrzystości.</span><span class="sxs-lookup"><span data-stu-id="17ea3-186">Follow the best practice of organizing the file structure for your views to reflect the relationships among controllers, actions, and views for maintainability and clarity.</span></span>

## <a name="passing-data-to-views"></a><span data-ttu-id="17ea3-187">Przekazywanie danych do widoków</span><span class="sxs-lookup"><span data-stu-id="17ea3-187">Passing data to views</span></span>

<span data-ttu-id="17ea3-188">Przekazywanie danych do widoków przy użyciu kilku metod:</span><span class="sxs-lookup"><span data-stu-id="17ea3-188">Pass data to views using several approaches:</span></span>

* <span data-ttu-id="17ea3-189">Silnie typizowane dane: viewmodel</span><span class="sxs-lookup"><span data-stu-id="17ea3-189">Strongly typed data: viewmodel</span></span>
* <span data-ttu-id="17ea3-190">Ze słabą kontrolą typów danych</span><span class="sxs-lookup"><span data-stu-id="17ea3-190">Weakly typed data</span></span>
  * `ViewData` <span data-ttu-id="17ea3-191">(`ViewDataAttribute`)</span><span class="sxs-lookup"><span data-stu-id="17ea3-191">(`ViewDataAttribute`)</span></span>
  * `ViewBag`

### <a name="strongly-typed-data-viewmodel"></a><span data-ttu-id="17ea3-192">Silnie typizowane dane (viewmodel)</span><span class="sxs-lookup"><span data-stu-id="17ea3-192">Strongly typed data (viewmodel)</span></span>

<span data-ttu-id="17ea3-193">Najbardziej niezawodną metodą jest określenie [modelu](xref:mvc/models/model-binding) typu w widoku.</span><span class="sxs-lookup"><span data-stu-id="17ea3-193">The most robust approach is to specify a [model](xref:mvc/models/model-binding) type in the view.</span></span> <span data-ttu-id="17ea3-194">Ten model jest często określany jako *viewmodel*.</span><span class="sxs-lookup"><span data-stu-id="17ea3-194">This model is commonly referred to as a *viewmodel*.</span></span> <span data-ttu-id="17ea3-195">Wystąpienie typu viewmodel są przekazywane do widoku akcji.</span><span class="sxs-lookup"><span data-stu-id="17ea3-195">You pass an instance of the viewmodel type to the view from the action.</span></span>

<span data-ttu-id="17ea3-196">Aby przekazać dane do widoku przy użyciu viewmodel umożliwia widok móc korzystać z *silne* sprawdzania typu.</span><span class="sxs-lookup"><span data-stu-id="17ea3-196">Using a viewmodel to pass data to a view allows the view to take advantage of *strong* type checking.</span></span> <span data-ttu-id="17ea3-197">*Silne wpisywanie* (lub *silnie typizowane*) oznacza, że każda zmienna i stała ma jawnie zdefiniowanych typów (na przykład `string`, `int`, lub `DateTime`).</span><span class="sxs-lookup"><span data-stu-id="17ea3-197">*Strong typing* (or *strongly typed*) means that every variable and constant has an explicitly defined type (for example, `string`, `int`, or `DateTime`).</span></span> <span data-ttu-id="17ea3-198">Poprawność typów używanych w widoku jest sprawdzany w czasie kompilacji.</span><span class="sxs-lookup"><span data-stu-id="17ea3-198">The validity of types used in a view is checked at compile time.</span></span>

<span data-ttu-id="17ea3-199">[Program Visual Studio](https://www.visualstudio.com/vs/) i [programu Visual Studio Code](https://code.visualstudio.com/) listy elementów członkowskich silnie typizowanej klasy przy użyciu funkcji o nazwie [IntelliSense](/visualstudio/ide/using-intellisense).</span><span class="sxs-lookup"><span data-stu-id="17ea3-199">[Visual Studio](https://www.visualstudio.com/vs/) and [Visual Studio Code](https://code.visualstudio.com/) list strongly typed class members using a feature called [IntelliSense](/visualstudio/ide/using-intellisense).</span></span> <span data-ttu-id="17ea3-200">Jeśli chcesz wyświetlić właściwości viewmodel, wpisz nazwę zmiennej viewmodel następuje kropka (`.`).</span><span class="sxs-lookup"><span data-stu-id="17ea3-200">When you want to see the properties of a viewmodel, type the variable name for the viewmodel followed by a period (`.`).</span></span> <span data-ttu-id="17ea3-201">Dzięki temu można napisać kod szybciej z mniejszą liczbą błędów.</span><span class="sxs-lookup"><span data-stu-id="17ea3-201">This helps you write code faster with fewer errors.</span></span>

<span data-ttu-id="17ea3-202">Określ model za pomocą `@model` dyrektywy.</span><span class="sxs-lookup"><span data-stu-id="17ea3-202">Specify a model using the `@model` directive.</span></span> <span data-ttu-id="17ea3-203">Użyj modelu z `@Model`:</span><span class="sxs-lookup"><span data-stu-id="17ea3-203">Use the model with `@Model`:</span></span>

```cshtml
@model WebApplication1.ViewModels.Address

<h2>Contact</h2>
<address>
    @Model.Street<br>
    @Model.City, @Model.State @Model.PostalCode<br>
    <abbr title="Phone">P:</abbr> 425.555.0100
</address>
```

<span data-ttu-id="17ea3-204">Aby zapewnić model widoku, kontroler przekazuje go jako parametr:</span><span class="sxs-lookup"><span data-stu-id="17ea3-204">To provide the model to the view, the controller passes it as a parameter:</span></span>

```csharp
public IActionResult Contact()
{
    ViewData["Message"] = "Your contact page.";

    var viewModel = new Address()
    {
        Name = "Microsoft",
        Street = "One Microsoft Way",
        City = "Redmond",
        State = "WA",
        PostalCode = "98052-6399"
    };

    return View(viewModel);
}
```

<span data-ttu-id="17ea3-205">Nie istnieją żadne ograniczenia dotyczące typów modeli, które można udostępnić widok.</span><span class="sxs-lookup"><span data-stu-id="17ea3-205">There are no restrictions on the model types that you can provide to a view.</span></span> <span data-ttu-id="17ea3-206">Zalecamy używanie modele widoków zwykłe stare CLR obiektu (— POCO) z działaniem niewielkiego lub żadnego (metod), zdefiniowane.</span><span class="sxs-lookup"><span data-stu-id="17ea3-206">We recommend using Plain Old CLR Object (POCO) viewmodels with little or no behavior (methods) defined.</span></span> <span data-ttu-id="17ea3-207">Zazwyczaj klas viewmodel albo są przechowywane w *modeli* folderu lub osobnej *modele widoków* folder w katalogu głównym aplikacji.</span><span class="sxs-lookup"><span data-stu-id="17ea3-207">Usually, viewmodel classes are either stored in the *Models* folder or a separate *ViewModels* folder at the root of the app.</span></span> <span data-ttu-id="17ea3-208">*Adres* viewmodel używane w powyższym przykładzie jest viewmodel POCO, przechowywane w pliku o nazwie *Address.cs*:</span><span class="sxs-lookup"><span data-stu-id="17ea3-208">The *Address* viewmodel used in the example above is a POCO viewmodel stored in a file named *Address.cs*:</span></span>

```csharp
namespace WebApplication1.ViewModels
{
    public class Address
    {
        public string Name { get; set; }
        public string Street { get; set; }
        public string City { get; set; }
        public string State { get; set; }
        public string PostalCode { get; set; }
    }
}
```

<span data-ttu-id="17ea3-209">Nic nie uniemożliwia przy użyciu tych samych klas dla typów viewmodel i swoje typy modeli biznesowych.</span><span class="sxs-lookup"><span data-stu-id="17ea3-209">Nothing prevents you from using the same classes for both your viewmodel types and your business model types.</span></span> <span data-ttu-id="17ea3-210">Jednak przy użyciu osobnych modeli umożliwia widoki, aby się nieznacznie różnić niezależnie od logiki biznesowej i danych dostęp do części aplikacji.</span><span class="sxs-lookup"><span data-stu-id="17ea3-210">However, using separate models allows your views to vary independently from the business logic and data access parts of your app.</span></span> <span data-ttu-id="17ea3-211">Rozdzielenie modeli i modele widoków oferuje również korzyści w zakresie zabezpieczeń użycia modeli [wiązanie modelu](xref:mvc/models/model-binding) i [weryfikacji](xref:mvc/models/validation) dla danych przesyłanych do aplikacji przez użytkownika.</span><span class="sxs-lookup"><span data-stu-id="17ea3-211">Separation of models and viewmodels also offers security benefits when models use [model binding](xref:mvc/models/model-binding) and [validation](xref:mvc/models/validation) for data sent to the app by the user.</span></span>

<a name="VD_VB"></a>

### <a name="weakly-typed-data-viewdata-viewdata-attribute-and-viewbag"></a><span data-ttu-id="17ea3-212">Słabo wpisanych danych (ViewData, atrybut ViewData i obiekt ViewBag)</span><span class="sxs-lookup"><span data-stu-id="17ea3-212">Weakly typed data (ViewData, ViewData attribute, and ViewBag)</span></span>

`ViewBag` *<span data-ttu-id="17ea3-213">nie jest dostępna w stron Razor.</span><span class="sxs-lookup"><span data-stu-id="17ea3-213">isn't available in Razor Pages.</span></span>*

<span data-ttu-id="17ea3-214">Oprócz silnie typizowane widoki, widoki mają dostęp do *słabo typizowaną* (nazywane również *luźno typizowanym*) zbierania danych.</span><span class="sxs-lookup"><span data-stu-id="17ea3-214">In addition to strongly typed views, views have access to a *weakly typed* (also called *loosely typed*) collection of data.</span></span> <span data-ttu-id="17ea3-215">W przeciwieństwie do silnej typów *słabe typy* (lub *luźno typy*) oznacza, że nie jawnie deklarować typ danych jest używany.</span><span class="sxs-lookup"><span data-stu-id="17ea3-215">Unlike strong types, *weak types* (or *loose types*) means that you don't explicitly declare the type of data you're using.</span></span> <span data-ttu-id="17ea3-216">Kolekcja ze słabą kontrolą typów danych służy do przekazywania małe ilości danych do i z widoków i kontrolerów.</span><span class="sxs-lookup"><span data-stu-id="17ea3-216">You can use the collection of weakly typed data for passing small amounts of data in and out of controllers and views.</span></span>

| <span data-ttu-id="17ea3-217">Przekazywanie danych pomiędzy...</span><span class="sxs-lookup"><span data-stu-id="17ea3-217">Passing data between a ...</span></span>                        | <span data-ttu-id="17ea3-218">Przykład</span><span class="sxs-lookup"><span data-stu-id="17ea3-218">Example</span></span>                                                                        |
| ------------------------------------------------- | ------------------------------------------------------------------------------ |
| <span data-ttu-id="17ea3-219">Kontrolerem i widokiem</span><span class="sxs-lookup"><span data-stu-id="17ea3-219">Controller and a view</span></span>                             | <span data-ttu-id="17ea3-220">Wypełnianie listę rozwijaną z danymi.</span><span class="sxs-lookup"><span data-stu-id="17ea3-220">Populating a dropdown list with data.</span></span>                                          |
| <span data-ttu-id="17ea3-221">Wyświetl i [widoku układu](xref:mvc/views/layout)</span><span class="sxs-lookup"><span data-stu-id="17ea3-221">View and a [layout view](xref:mvc/views/layout)</span></span>   | <span data-ttu-id="17ea3-222">Ustawienie  **\<title >** zawartości elementu w widoku układu z plikiem widoku.</span><span class="sxs-lookup"><span data-stu-id="17ea3-222">Setting the **\<title>** element content in the layout view from a view file.</span></span>  |
| <span data-ttu-id="17ea3-223">[Widok częściowy](xref:mvc/views/partial) i widok</span><span class="sxs-lookup"><span data-stu-id="17ea3-223">[Partial view](xref:mvc/views/partial) and a view</span></span> | <span data-ttu-id="17ea3-224">Element widget wyświetlający dane oparte na sieci Web, który użytkownik zażądał.</span><span class="sxs-lookup"><span data-stu-id="17ea3-224">A widget that displays data based on the webpage that the user requested.</span></span>      |

<span data-ttu-id="17ea3-225">Ta kolekcja można się odwoływać przy użyciu jednej `ViewData` lub `ViewBag` właściwości dla widoków i kontrolerów.</span><span class="sxs-lookup"><span data-stu-id="17ea3-225">This collection can be referenced through either the `ViewData` or `ViewBag` properties on controllers and views.</span></span> <span data-ttu-id="17ea3-226">`ViewData` Właściwość jest słabo typizowanych obiektów ze słownika.</span><span class="sxs-lookup"><span data-stu-id="17ea3-226">The `ViewData` property is a dictionary of weakly typed objects.</span></span> <span data-ttu-id="17ea3-227">`ViewBag` Właściwość jest otokę `ViewData` zapewniający właściwości dynamicznych bazowego `ViewData` kolekcji.</span><span class="sxs-lookup"><span data-stu-id="17ea3-227">The `ViewBag` property is a wrapper around `ViewData` that provides dynamic properties for the underlying `ViewData` collection.</span></span> <span data-ttu-id="17ea3-228">Uwaga: Klucza wyszukiwania są bez uwzględniania wielkości liter dla obu `ViewData` i `ViewBag`.</span><span class="sxs-lookup"><span data-stu-id="17ea3-228">Note: Key lookups are case-insensitive for both `ViewData` and `ViewBag`.</span></span>

`ViewData` <span data-ttu-id="17ea3-229">i `ViewBag` są dynamicznie rozwiązane w czasie wykonywania.</span><span class="sxs-lookup"><span data-stu-id="17ea3-229">and `ViewBag` are dynamically resolved at runtime.</span></span> <span data-ttu-id="17ea3-230">Ponieważ nie oferują typu w czasie kompilacji sprawdzania, oba są zwykle bardziej podatne na błędy niż przy użyciu viewmodel.</span><span class="sxs-lookup"><span data-stu-id="17ea3-230">Since they don't offer compile-time type checking, both are generally more error-prone than using a viewmodel.</span></span> <span data-ttu-id="17ea3-231">Z tego powodu niektórzy deweloperzy wolą używać co najmniej lub nigdy nie `ViewData` i `ViewBag`.</span><span class="sxs-lookup"><span data-stu-id="17ea3-231">For that reason, some developers prefer to minimally or never use `ViewData` and `ViewBag`.</span></span>

<a name="VD"></a>

**<span data-ttu-id="17ea3-232">ViewData</span><span class="sxs-lookup"><span data-stu-id="17ea3-232">ViewData</span></span>**

`ViewData` <span data-ttu-id="17ea3-233">jest [ViewDataDictionary](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.viewdatadictionary) dostępne za pośrednictwem obiektu `string` kluczy.</span><span class="sxs-lookup"><span data-stu-id="17ea3-233">is a [ViewDataDictionary](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.viewdatadictionary) object accessed through `string` keys.</span></span> <span data-ttu-id="17ea3-234">Dane ciągu może być przechowywane i używane bezpośrednio, bez konieczności rzutowania, ale należy rzutować innych `ViewData` obiektu wartości do określonych typów, kiedy ich wyodrębnić.</span><span class="sxs-lookup"><span data-stu-id="17ea3-234">String data can be stored and used directly without the need for a cast, but you must cast other `ViewData` object values to specific types when you extract them.</span></span> <span data-ttu-id="17ea3-235">Możesz użyć `ViewData` do przekazywania danych z kontrolerów, widoków i w obrębie widoków, w tym [widoki częściowe](xref:mvc/views/partial) i [układy](xref:mvc/views/layout).</span><span class="sxs-lookup"><span data-stu-id="17ea3-235">You can use `ViewData` to pass data from controllers to views and within views, including [partial views](xref:mvc/views/partial) and [layouts](xref:mvc/views/layout).</span></span>

<span data-ttu-id="17ea3-236">Oto przykład, który ustawia wartości pozdrowienia i usługi za pomocą adresu `ViewData` w akcji:</span><span class="sxs-lookup"><span data-stu-id="17ea3-236">The following is an example that sets values for a greeting and an address using `ViewData` in an action:</span></span>

```csharp
public IActionResult SomeAction()
{
    ViewData["Greeting"] = "Hello";
    ViewData["Address"]  = new Address()
    {
        Name = "Steve",
        Street = "123 Main St",
        City = "Hudson",
        State = "OH",
        PostalCode = "44236"
    };

    return View();
}
```

<span data-ttu-id="17ea3-237">Praca z danymi w widoku:</span><span class="sxs-lookup"><span data-stu-id="17ea3-237">Work with the data in a view:</span></span>

```cshtml
@{
    // Since Address isn't a string, it requires a cast.
    var address = ViewData["Address"] as Address;
}

@ViewData["Greeting"] World!

<address>
    @address.Name<br>
    @address.Street<br>
    @address.City, @address.State @address.PostalCode
</address>
```

::: moniker range=">= aspnetcore-2.1"

**<span data-ttu-id="17ea3-238">Atrybut viewData</span><span class="sxs-lookup"><span data-stu-id="17ea3-238">ViewData attribute</span></span>**

<span data-ttu-id="17ea3-239">Innym rozwiązaniem, który używa [ViewDataDictionary](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.viewdatadictionary) jest [ViewDataAttribute](/dotnet/api/microsoft.aspnetcore.mvc.viewdataattribute).</span><span class="sxs-lookup"><span data-stu-id="17ea3-239">Another approach that uses the [ViewDataDictionary](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.viewdatadictionary) is [ViewDataAttribute](/dotnet/api/microsoft.aspnetcore.mvc.viewdataattribute).</span></span> <span data-ttu-id="17ea3-240">Właściwości na kontrolerach ani w modelach strony Razor ozdobione `[ViewData]` ich wartości przechowywane i załadowany ze słownika.</span><span class="sxs-lookup"><span data-stu-id="17ea3-240">Properties on controllers or Razor Page models decorated with `[ViewData]` have their values stored and loaded from the dictionary.</span></span>

<span data-ttu-id="17ea3-241">W poniższym przykładzie zawiera kontrolera głównego `Title` ozdobione właściwość `[ViewData]`.</span><span class="sxs-lookup"><span data-stu-id="17ea3-241">In the following example, the Home controller contains a `Title` property decorated with `[ViewData]`.</span></span> <span data-ttu-id="17ea3-242">`About` Metoda Ustawia tytuł dla tego widoku informacje:</span><span class="sxs-lookup"><span data-stu-id="17ea3-242">The `About` method sets the title for the About view:</span></span>

```csharp
public class HomeController : Controller
{
    [ViewData]
    public string Title { get; set; }

    public IActionResult About()
    {
        Title = "About Us";
        ViewData["Message"] = "Your application description page.";

        return View();
    }
}
```

<span data-ttu-id="17ea3-243">W widoku informacje dostępu `Title` właściwość jako właściwość modelu:</span><span class="sxs-lookup"><span data-stu-id="17ea3-243">In the About view, access the `Title` property as a model property:</span></span>

```cshtml
<h1>@Model.Title</h1>
```

<span data-ttu-id="17ea3-244">W układzie tytuł jest do odczytu ze słownika ViewData:</span><span class="sxs-lookup"><span data-stu-id="17ea3-244">In the layout, the title is read from the ViewData dictionary:</span></span>

```cshtml
<!DOCTYPE html>
<html lang="en">
<head>
    <title>@ViewData["Title"] - WebApplication</title>
    ...
```

::: moniker-end

**<span data-ttu-id="17ea3-245">ViewBag</span><span class="sxs-lookup"><span data-stu-id="17ea3-245">ViewBag</span></span>**

`ViewBag` *<span data-ttu-id="17ea3-246">nie jest dostępna w stron Razor.</span><span class="sxs-lookup"><span data-stu-id="17ea3-246">isn't available in Razor Pages.</span></span>*

`ViewBag` <span data-ttu-id="17ea3-247">jest [DynamicViewData](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.internal.dynamicviewdata) obiektu, który dostarcza dynamiczny dostęp do obiektów przechowywanych w `ViewData`.</span><span class="sxs-lookup"><span data-stu-id="17ea3-247">is a [DynamicViewData](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.internal.dynamicviewdata) object that provides dynamic access to the objects stored in `ViewData`.</span></span> `ViewBag` <span data-ttu-id="17ea3-248">może być bardziej wygodne chcesz pracować, ponieważ nie wymaga rzutowania.</span><span class="sxs-lookup"><span data-stu-id="17ea3-248">can be more convenient to work with, since it doesn't require casting.</span></span> <span data-ttu-id="17ea3-249">Poniższy przykład pokazuje, jak używać `ViewBag` o ten sam wynik jako przy użyciu `ViewData` powyżej:</span><span class="sxs-lookup"><span data-stu-id="17ea3-249">The following example shows how to use `ViewBag` with the same result as using `ViewData` above:</span></span>

```csharp
public IActionResult SomeAction()
{
    ViewBag.Greeting = "Hello";
    ViewBag.Address  = new Address()
    {
        Name = "Steve",
        Street = "123 Main St",
        City = "Hudson",
        State = "OH",
        PostalCode = "44236"
    };

    return View();
}
```

```cshtml
@ViewBag.Greeting World!

<address>
    @ViewBag.Address.Name<br>
    @ViewBag.Address.Street<br>
    @ViewBag.Address.City, @ViewBag.Address.State @ViewBag.Address.PostalCode
</address>
```

**<span data-ttu-id="17ea3-250">Korzystanie z podejścia ViewData i obiekt ViewBag jednocześnie</span><span class="sxs-lookup"><span data-stu-id="17ea3-250">Using ViewData and ViewBag simultaneously</span></span>**

`ViewBag` *<span data-ttu-id="17ea3-251">nie jest dostępna w stron Razor.</span><span class="sxs-lookup"><span data-stu-id="17ea3-251">isn't available in Razor Pages.</span></span>*

<span data-ttu-id="17ea3-252">Ponieważ `ViewData` i `ViewBag` odnoszą się do tych samych podstawowych `ViewData` kolekcji, możesz używać ich obu `ViewData` i `ViewBag` mieszać i zgodne z nich podczas odczytywania i zapisywania wartości.</span><span class="sxs-lookup"><span data-stu-id="17ea3-252">Since `ViewData` and `ViewBag` refer to the same underlying `ViewData` collection, you can use both `ViewData` and `ViewBag` and mix and match between them when reading and writing values.</span></span>

<span data-ttu-id="17ea3-253">Można ustawić przy użyciu tytułu `ViewBag` i używanie opis `ViewData` w górnej części *About.cshtml* widoku:</span><span class="sxs-lookup"><span data-stu-id="17ea3-253">Set the title using `ViewBag` and the description using `ViewData` at the top of an *About.cshtml* view:</span></span>

```cshtml
@{
    Layout = "/Views/Shared/_Layout.cshtml";
    ViewBag.Title = "About Contoso";
    ViewData["Description"] = "Let us tell you about Contoso's philosophy and mission.";
}
```

<span data-ttu-id="17ea3-254">Odczytane właściwości, ale odwrotnego użytkowania `ViewData` i `ViewBag`.</span><span class="sxs-lookup"><span data-stu-id="17ea3-254">Read the properties but reverse the use of `ViewData` and `ViewBag`.</span></span> <span data-ttu-id="17ea3-255">W *_Layout.cshtml* plików, Uzyskaj za pomocą tytuł `ViewData` i Uzyskaj za pomocą opis `ViewBag`:</span><span class="sxs-lookup"><span data-stu-id="17ea3-255">In the *_Layout.cshtml* file, obtain the title using `ViewData` and obtain the description using `ViewBag`:</span></span>

```cshtml
<!DOCTYPE html>
<html lang="en">
<head>
    <title>@ViewData["Title"]</title>
    <meta name="description" content="@ViewBag.Description">
    ...
```

<span data-ttu-id="17ea3-256">Należy pamiętać, że ciągi nie wymagają oddanych na `ViewData`.</span><span class="sxs-lookup"><span data-stu-id="17ea3-256">Remember that strings don't require a cast for `ViewData`.</span></span> <span data-ttu-id="17ea3-257">Możesz użyć `@ViewData["Title"]` bez rzutowania.</span><span class="sxs-lookup"><span data-stu-id="17ea3-257">You can use `@ViewData["Title"]` without casting.</span></span>

<span data-ttu-id="17ea3-258">Przy użyciu zarówno `ViewData` i `ViewBag` na tym samym działa czasu, co robi mieszanie i dopasowywanie odczytywanie i zapisywanie właściwości.</span><span class="sxs-lookup"><span data-stu-id="17ea3-258">Using both `ViewData` and `ViewBag` at the same time works, as does mixing and matching reading and writing the properties.</span></span> <span data-ttu-id="17ea3-259">Jest renderowany w niej następujące znaczniki:</span><span class="sxs-lookup"><span data-stu-id="17ea3-259">The following markup is rendered:</span></span>

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <title>About Contoso</title>
    <meta name="description" content="Let us tell you about Contoso's philosophy and mission.">
    ...
```

**<span data-ttu-id="17ea3-260">Podsumowanie różnic między ViewData i obiekt ViewBag</span><span class="sxs-lookup"><span data-stu-id="17ea3-260">Summary of the differences between ViewData and ViewBag</span></span>**

 `ViewBag` <span data-ttu-id="17ea3-261">nie jest dostępna w stron Razor.</span><span class="sxs-lookup"><span data-stu-id="17ea3-261">isn't available in the Razor Pages.</span></span>

* `ViewData`
  * <span data-ttu-id="17ea3-262">Pochodzi od klasy [ViewDataDictionary](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.viewdatadictionary), więc posiada słownika właściwości, które mogą być przydatne, takie jak `ContainsKey`, `Add`, `Remove`, i `Clear`.</span><span class="sxs-lookup"><span data-stu-id="17ea3-262">Derives from [ViewDataDictionary](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.viewdatadictionary), so it has dictionary properties that can be useful, such as `ContainsKey`, `Add`, `Remove`, and `Clear`.</span></span>
  * <span data-ttu-id="17ea3-263">Klucze w słowniku są ciągami, umożliwić białe znaki.</span><span class="sxs-lookup"><span data-stu-id="17ea3-263">Keys in the dictionary are strings, so whitespace is allowed.</span></span> <span data-ttu-id="17ea3-264">Przykład:</span><span class="sxs-lookup"><span data-stu-id="17ea3-264">Example:</span></span> `ViewData["Some Key With Whitespace"]`
  * <span data-ttu-id="17ea3-265">Dowolny typ inny niż `string` musi być rzutowany w widoku, aby użyć `ViewData`.</span><span class="sxs-lookup"><span data-stu-id="17ea3-265">Any type other than a `string` must be cast in the view to use `ViewData`.</span></span>
* `ViewBag`
  * <span data-ttu-id="17ea3-266">Pochodzi od klasy [DynamicViewData](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.internal.dynamicviewdata), dzięki czemu umożliwia tworzenie właściwości dynamicznych, używając zapisu kropkowego (`@ViewBag.SomeKey = <value or object>`), a Rzutowanie nie jest wymagany.</span><span class="sxs-lookup"><span data-stu-id="17ea3-266">Derives from [DynamicViewData](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.internal.dynamicviewdata), so it allows the creation of dynamic properties using dot notation (`@ViewBag.SomeKey = <value or object>`), and no casting is required.</span></span> <span data-ttu-id="17ea3-267">Składnia `ViewBag` ułatwia szybsze do dodania do widoków i kontrolerów.</span><span class="sxs-lookup"><span data-stu-id="17ea3-267">The syntax of `ViewBag` makes it quicker to add to controllers and views.</span></span>
  * <span data-ttu-id="17ea3-268">Prostsze pod kątem wartości null.</span><span class="sxs-lookup"><span data-stu-id="17ea3-268">Simpler to check for null values.</span></span> <span data-ttu-id="17ea3-269">Przykład:</span><span class="sxs-lookup"><span data-stu-id="17ea3-269">Example:</span></span> `@ViewBag.Person?.Name`

**<span data-ttu-id="17ea3-270">Kiedy używać ViewData i obiekt ViewBag</span><span class="sxs-lookup"><span data-stu-id="17ea3-270">When to use ViewData or ViewBag</span></span>**

<span data-ttu-id="17ea3-271">Zarówno `ViewData` i `ViewBag` są równie prawidłowe podejścia do przekazywania małe ilości danych między widoków i kontrolerów.</span><span class="sxs-lookup"><span data-stu-id="17ea3-271">Both `ViewData` and `ViewBag` are equally valid approaches for passing small amounts of data among controllers and views.</span></span> <span data-ttu-id="17ea3-272">Wybór co do użycia zależy od preferencji.</span><span class="sxs-lookup"><span data-stu-id="17ea3-272">The choice of which one to use is based on preference.</span></span> <span data-ttu-id="17ea3-273">Można mieszać i dopasowywać `ViewData` i `ViewBag` obiektów, jednak ten kod jest łatwiej odczytywać i utrzymywać z jednym z podejść stosowane konsekwentnie.</span><span class="sxs-lookup"><span data-stu-id="17ea3-273">You can mix and match `ViewData` and `ViewBag` objects, however, the code is easier to read and maintain with one approach used consistently.</span></span> <span data-ttu-id="17ea3-274">Oba podejścia są dynamicznie rozwiązane w czasie wykonywania i dlatego podatne na powodując błędy w czasie wykonywania.</span><span class="sxs-lookup"><span data-stu-id="17ea3-274">Both approaches are dynamically resolved at runtime and thus prone to causing runtime errors.</span></span> <span data-ttu-id="17ea3-275">Niektóre zespoły deweloperów, unikaj ich.</span><span class="sxs-lookup"><span data-stu-id="17ea3-275">Some development teams avoid them.</span></span>

### <a name="dynamic-views"></a><span data-ttu-id="17ea3-276">Widoki dynamiczne</span><span class="sxs-lookup"><span data-stu-id="17ea3-276">Dynamic views</span></span>

<span data-ttu-id="17ea3-277">Widoki, które nie deklarują modelu typu przy użyciu `@model` , ale mają wystąpienie modelu, przekazana do nich (na przykład `return View(Address);`) można odwoływać się do właściwości wystąpienia dynamicznie:</span><span class="sxs-lookup"><span data-stu-id="17ea3-277">Views that don't declare a model type using `@model` but that have a model instance passed to them (for example, `return View(Address);`) can reference the instance's properties dynamically:</span></span>

```cshtml
<address>
    @Model.Street<br>
    @Model.City, @Model.State @Model.PostalCode<br>
    <abbr title="Phone">P:</abbr> 425.555.0100
</address>
```

<span data-ttu-id="17ea3-278">Ta funkcja zapewnia elastyczność, ale nie oferuje ochronę kompilacji i technologii IntelliSense.</span><span class="sxs-lookup"><span data-stu-id="17ea3-278">This feature offers flexibility but doesn't offer compilation protection or IntelliSense.</span></span> <span data-ttu-id="17ea3-279">Jeśli właściwość nie istnieje, generowania strony sieci Web kończy się niepowodzeniem w czasie wykonywania.</span><span class="sxs-lookup"><span data-stu-id="17ea3-279">If the property doesn't exist, webpage generation fails at runtime.</span></span>

## <a name="more-view-features"></a><span data-ttu-id="17ea3-280">Więcej funkcji widoku</span><span class="sxs-lookup"><span data-stu-id="17ea3-280">More view features</span></span>

<span data-ttu-id="17ea3-281">[Pomocników tagów](xref:mvc/views/tag-helpers/intro) ułatwiają dodawanie zachowania po stronie serwera do istniejących tagów HTML.</span><span class="sxs-lookup"><span data-stu-id="17ea3-281">[Tag Helpers](xref:mvc/views/tag-helpers/intro) make it easy to add server-side behavior to existing HTML tags.</span></span> <span data-ttu-id="17ea3-282">Używanie pomocników tagów eliminuje konieczność pisania niestandardowego kodu lub pomocników w obrębie widoków.</span><span class="sxs-lookup"><span data-stu-id="17ea3-282">Using Tag Helpers avoids the need to write custom code or helpers within your views.</span></span> <span data-ttu-id="17ea3-283">Pomocnicy tagów są stosowane jako atrybuty do elementów kodu HTML i są ignorowane przez edytory, które nie może ich przetworzyć.</span><span class="sxs-lookup"><span data-stu-id="17ea3-283">Tag helpers are applied as attributes to HTML elements and are ignored by editors that can't process them.</span></span> <span data-ttu-id="17ea3-284">Dzięki temu można edytować i renderowania kodu znaczników widoku w różnych narzędzi.</span><span class="sxs-lookup"><span data-stu-id="17ea3-284">This allows you to edit and render view markup in a variety of tools.</span></span>

<span data-ttu-id="17ea3-285">Generowanie niestandardowego kodu znaczników HTML może odbyć się z wielu wbudowanych pomocników HTML.</span><span class="sxs-lookup"><span data-stu-id="17ea3-285">Generating custom HTML markup can be achieved with many built-in HTML Helpers.</span></span> <span data-ttu-id="17ea3-286">Bardziej złożona logika interfejsu użytkownika może zostać obsłużony przez [składniki widoków](xref:mvc/views/view-components).</span><span class="sxs-lookup"><span data-stu-id="17ea3-286">More complex user interface logic can be handled by [View Components](xref:mvc/views/view-components).</span></span> <span data-ttu-id="17ea3-287">Składniki widoków Podaj ten sam SoC tego kontrolerów, a widoki oferują.</span><span class="sxs-lookup"><span data-stu-id="17ea3-287">View components provide the same SoC that controllers and views offer.</span></span> <span data-ttu-id="17ea3-288">Mogą one wyeliminować potrzebę akcjami i widokami, które zajmują się dane używane przez wspólne elementy interfejsu użytkownika.</span><span class="sxs-lookup"><span data-stu-id="17ea3-288">They can eliminate the need for actions and views that deal with data used by common user interface elements.</span></span>

<span data-ttu-id="17ea3-289">Podobnie jak wiele innych aspektów platformy ASP.NET Core, widoki pomocy technicznej [wstrzykiwanie zależności](xref:fundamentals/dependency-injection), dzięki czemu usługi [do widoków](xref:mvc/views/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="17ea3-289">Like many other aspects of ASP.NET Core, views support [dependency injection](xref:fundamentals/dependency-injection), allowing services to be [injected into views](xref:mvc/views/dependency-injection).</span></span>
