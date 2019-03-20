---
title: Składniki widoków w programie ASP.NET Core
author: rick-anderson
description: Dowiedz się, jak składniki widoków są używane w programie ASP.NET Core oraz dodać je do aplikacji.
ms.author: riande
ms.date: 1/30/2019
uid: mvc/views/view-components
ms.openlocfilehash: b18473c6a76c4dc9030f0a032db1aff733f5acb7
ms.sourcegitcommit: 57792e5f594db1574742588017c708350958bdf0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/20/2019
ms.locfileid: "58264786"
---
# <a name="view-components-in-aspnet-core"></a><span data-ttu-id="a3952-103">Składniki widoków w programie ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="a3952-103">View components in ASP.NET Core</span></span>

<span data-ttu-id="a3952-104">Przez [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="a3952-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="a3952-105">[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/view-components/sample) ([sposobu pobierania](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="a3952-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/view-components/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="view-components"></a><span data-ttu-id="a3952-106">Składniki widoku</span><span class="sxs-lookup"><span data-stu-id="a3952-106">View components</span></span>

<span data-ttu-id="a3952-107">Składniki widoków są podobne do widoków częściowych, ale są one znacznie bardziej wydajne.</span><span class="sxs-lookup"><span data-stu-id="a3952-107">View components are similar to partial views, but they're much more powerful.</span></span> <span data-ttu-id="a3952-108">Składniki widoków nie używaj wiązania modelu i tylko zależą od podanych podczas wywoływania do niego danych.</span><span class="sxs-lookup"><span data-stu-id="a3952-108">View components don't use model binding, and only depend on the data provided when calling into it.</span></span> <span data-ttu-id="a3952-109">W tym artykule został napisany, przy użyciu widoków i kontrolerów, ale wyświetlania składników również Praca ze stronami Razor.</span><span class="sxs-lookup"><span data-stu-id="a3952-109">This article was written using controllers and views, but view components also work with Razor Pages.</span></span>

<span data-ttu-id="a3952-110">Składnik widoku:</span><span class="sxs-lookup"><span data-stu-id="a3952-110">A view component:</span></span>

* <span data-ttu-id="a3952-111">Renderuje fragment, a nie całej odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="a3952-111">Renders a chunk rather than a whole response.</span></span>
* <span data-ttu-id="a3952-112">Obejmuje takie same separacji z uwagi i korzyści z testowania znaleziono między kontrolerem a widokiem.</span><span class="sxs-lookup"><span data-stu-id="a3952-112">Includes the same separation-of-concerns and testability benefits found between a controller and view.</span></span>
* <span data-ttu-id="a3952-113">Może mieć parametrów i logiki biznesowej.</span><span class="sxs-lookup"><span data-stu-id="a3952-113">Can have parameters and business logic.</span></span>
* <span data-ttu-id="a3952-114">Zazwyczaj jest wywoływane ze strony układu.</span><span class="sxs-lookup"><span data-stu-id="a3952-114">Is typically invoked from a layout page.</span></span>

<span data-ttu-id="a3952-115">Składniki widoków mają na celu dowolnym miejscu mieć logikę renderowania wielokrotnego użytku, która jest zbyt złożone dla widoku częściowego, takich jak:</span><span class="sxs-lookup"><span data-stu-id="a3952-115">View components are intended anywhere you have reusable rendering logic that's too complex for a partial view, such as:</span></span>

* <span data-ttu-id="a3952-116">Menu dynamiczne nawigacji</span><span class="sxs-lookup"><span data-stu-id="a3952-116">Dynamic navigation menus</span></span>
* <span data-ttu-id="a3952-117">Obłoku (gdzie zapytań bazy danych)</span><span class="sxs-lookup"><span data-stu-id="a3952-117">Tag cloud (where it queries the database)</span></span>
* <span data-ttu-id="a3952-118">Panel logowania</span><span class="sxs-lookup"><span data-stu-id="a3952-118">Login panel</span></span>
* <span data-ttu-id="a3952-119">Koszyk</span><span class="sxs-lookup"><span data-stu-id="a3952-119">Shopping cart</span></span>
* <span data-ttu-id="a3952-120">Ostatnio opublikowane artykuły</span><span class="sxs-lookup"><span data-stu-id="a3952-120">Recently published articles</span></span>
* <span data-ttu-id="a3952-121">Zawartość paska bocznego na blogu typowe</span><span class="sxs-lookup"><span data-stu-id="a3952-121">Sidebar content on a typical blog</span></span>
* <span data-ttu-id="a3952-122">Panel logowania, który będzie renderowany na każdej stronie i Pokaż łącza Wyloguj się lub zaloguj się w zależności od tego, w dzienniku w stan użytkownika</span><span class="sxs-lookup"><span data-stu-id="a3952-122">A login panel that would be rendered on every page and show either the links to log out or log in, depending on the log in state of the user</span></span>

<span data-ttu-id="a3952-123">Składnik Widok składa się z dwóch części: klasy (zazwyczaj uzyskiwane ze [ViewComponent](/dotnet/api/microsoft.aspnetcore.mvc.viewcomponent)), a wynik zwraca (zazwyczaj widok).</span><span class="sxs-lookup"><span data-stu-id="a3952-123">A view component consists of two parts: the class (typically derived from [ViewComponent](/dotnet/api/microsoft.aspnetcore.mvc.viewcomponent)) and the result it returns (typically a view).</span></span> <span data-ttu-id="a3952-124">Np. kontrolery, składnik widok może być POCO, ale większość programistów będą chcieli korzystać z zalet metody i właściwości dostępne przez pochodząca od `ViewComponent`.</span><span class="sxs-lookup"><span data-stu-id="a3952-124">Like controllers, a view component can be a POCO, but most developers will want to take advantage of the methods and properties available by deriving from `ViewComponent`.</span></span>

## <a name="creating-a-view-component"></a><span data-ttu-id="a3952-125">Tworzenie widoku składnika</span><span class="sxs-lookup"><span data-stu-id="a3952-125">Creating a view component</span></span>

<span data-ttu-id="a3952-126">Ta sekcja zawiera ogólne wymagania dotyczące tworzenia widoku składnika.</span><span class="sxs-lookup"><span data-stu-id="a3952-126">This section contains the high-level requirements to create a view component.</span></span> <span data-ttu-id="a3952-127">W dalszej części tego artykułu utworzymy Sprawdź każdy krok szczegółowo i tworzenie widoku składnika.</span><span class="sxs-lookup"><span data-stu-id="a3952-127">Later in the article, we'll examine each step in detail and create a view component.</span></span>

### <a name="the-view-component-class"></a><span data-ttu-id="a3952-128">Widok klasy składników</span><span class="sxs-lookup"><span data-stu-id="a3952-128">The view component class</span></span>

<span data-ttu-id="a3952-129">Widok klasy składnika mogą być tworzone według dowolnej z następujących czynności:</span><span class="sxs-lookup"><span data-stu-id="a3952-129">A view component class can be created by any of the following:</span></span>

* <span data-ttu-id="a3952-130">Wyprowadzanie z *ViewComponent*</span><span class="sxs-lookup"><span data-stu-id="a3952-130">Deriving from *ViewComponent*</span></span>
* <span data-ttu-id="a3952-131">Urządzanie klasy z `[ViewComponent]` atrybutu lub pochodząca od klasy z `[ViewComponent]` atrybutu</span><span class="sxs-lookup"><span data-stu-id="a3952-131">Decorating a class with the `[ViewComponent]` attribute, or deriving from a class with the `[ViewComponent]` attribute</span></span>
* <span data-ttu-id="a3952-132">Tworzenie klasy, której nazwa kończy się sufiksem *ViewComponent*</span><span class="sxs-lookup"><span data-stu-id="a3952-132">Creating a class where the name ends with the suffix *ViewComponent*</span></span>

<span data-ttu-id="a3952-133">Jak kontrolerów widok składniki muszą być publiczne, -nested i nieabstrakcyjnej klasy.</span><span class="sxs-lookup"><span data-stu-id="a3952-133">Like controllers, view components must be public, non-nested, and non-abstract classes.</span></span> <span data-ttu-id="a3952-134">Nazwa składnika widok jest nazwą klasy, wraz z sufiksem "ViewComponent" usunięte.</span><span class="sxs-lookup"><span data-stu-id="a3952-134">The view component name is the class name with the "ViewComponent" suffix removed.</span></span> <span data-ttu-id="a3952-135">Jego można również jawnie określać z użyciem `ViewComponentAttribute.Name` właściwości.</span><span class="sxs-lookup"><span data-stu-id="a3952-135">It can also be explicitly specified using the `ViewComponentAttribute.Name` property.</span></span>

<span data-ttu-id="a3952-136">Widok klasy składnika:</span><span class="sxs-lookup"><span data-stu-id="a3952-136">A view component class:</span></span>

* <span data-ttu-id="a3952-137">W pełni obsługuje konstruktora [wstrzykiwanie zależności](../../fundamentals/dependency-injection.md)</span><span class="sxs-lookup"><span data-stu-id="a3952-137">Fully supports constructor [dependency injection](../../fundamentals/dependency-injection.md)</span></span>

* <span data-ttu-id="a3952-138">Nie brać udział w cyklu życia kontrolera, co oznacza, nie można użyć [filtry](../controllers/filters.md) w składniku widoku</span><span class="sxs-lookup"><span data-stu-id="a3952-138">Doesn't take part in the controller lifecycle, which means you can't use [filters](../controllers/filters.md) in a view component</span></span>

### <a name="view-component-methods"></a><span data-ttu-id="a3952-139">Wyświetlanie składnika metod</span><span class="sxs-lookup"><span data-stu-id="a3952-139">View component methods</span></span>

<span data-ttu-id="a3952-140">Składnik widok definiuje swojej logiki w `InvokeAsync` metodę, która zwraca `Task<IViewComponentResult>` lub synchronicznego `Invoke` metodę, która zwraca `IViewComponentResult`.</span><span class="sxs-lookup"><span data-stu-id="a3952-140">A view component defines its logic in an `InvokeAsync` method that returns a `Task<IViewComponentResult>` or in a synchronous `Invoke` method that returns an `IViewComponentResult`.</span></span> <span data-ttu-id="a3952-141">Parametry pochodzą bezpośrednio z wywołania części widoku, nie z wiązania modelu.</span><span class="sxs-lookup"><span data-stu-id="a3952-141">Parameters come directly from invocation of the view component, not from model binding.</span></span> <span data-ttu-id="a3952-142">Składnik widok nigdy nie obsługuje bezpośrednio żądania.</span><span class="sxs-lookup"><span data-stu-id="a3952-142">A view component never directly handles a request.</span></span> <span data-ttu-id="a3952-143">Zazwyczaj składnikiem widoku inicjuje modelu i przekazuje je do widoku przez wywołanie metody `View` metody.</span><span class="sxs-lookup"><span data-stu-id="a3952-143">Typically, a view component initializes a model and passes it to a view by calling the `View` method.</span></span> <span data-ttu-id="a3952-144">Podsumowując wyświetlić metody składników:</span><span class="sxs-lookup"><span data-stu-id="a3952-144">In summary, view component methods:</span></span>

* <span data-ttu-id="a3952-145">Zdefiniuj `InvokeAsync` metodę, która zwraca `Task<IViewComponentResult>` lub synchronicznego `Invoke` metodę, która zwraca `IViewComponentResult`.</span><span class="sxs-lookup"><span data-stu-id="a3952-145">Define an `InvokeAsync` method that returns a `Task<IViewComponentResult>` or a synchronous `Invoke` method that returns an `IViewComponentResult`.</span></span>
* <span data-ttu-id="a3952-146">Zazwyczaj inicjuje modelu i przekazuje je do widoku, wywołując `ViewComponent` `View` metody.</span><span class="sxs-lookup"><span data-stu-id="a3952-146">Typically initializes a model and passes it to a view by calling the `ViewComponent` `View` method.</span></span>
* <span data-ttu-id="a3952-147">Parametry pochodzą z wywołania metody, a nie HTTP.</span><span class="sxs-lookup"><span data-stu-id="a3952-147">Parameters come from the calling method, not HTTP.</span></span> <span data-ttu-id="a3952-148">Nie istnieje żadne wiązanie modelu.</span><span class="sxs-lookup"><span data-stu-id="a3952-148">There's no model binding.</span></span>
* <span data-ttu-id="a3952-149">Nie są dostępne bezpośrednio jako punkt końcowy HTTP.</span><span class="sxs-lookup"><span data-stu-id="a3952-149">Are not reachable directly as an HTTP endpoint.</span></span> <span data-ttu-id="a3952-150">Są one wywoływane w kodzie (zwykle w widoku).</span><span class="sxs-lookup"><span data-stu-id="a3952-150">They're invoked from your code (usually in a view).</span></span> <span data-ttu-id="a3952-151">Składnik widok nigdy nie obsługuje żądania.</span><span class="sxs-lookup"><span data-stu-id="a3952-151">A view component never handles a request.</span></span>
* <span data-ttu-id="a3952-152">Są przeciążone dla podpisu, a nie wszystkie szczegóły z bieżącego żądania HTTP.</span><span class="sxs-lookup"><span data-stu-id="a3952-152">Are overloaded on the signature rather than any details from the current HTTP request.</span></span>

### <a name="view-search-path"></a><span data-ttu-id="a3952-153">Ścieżka wyszukiwania widoku</span><span class="sxs-lookup"><span data-stu-id="a3952-153">View search path</span></span>

<span data-ttu-id="a3952-154">Środowisko uruchomieniowe wyszukuje widoku w następujących ścieżkach:</span><span class="sxs-lookup"><span data-stu-id="a3952-154">The runtime searches for the view in the following paths:</span></span>

* <span data-ttu-id="a3952-155">/Components/ /views/ {nazwa kontrolera} {Nazwa widoku składnika} / {Nazwa widoku}</span><span class="sxs-lookup"><span data-stu-id="a3952-155">/Views/{Controller Name}/Components/{View Component Name}/{View Name}</span></span>
* <span data-ttu-id="a3952-156">/ Widoków/Shared/Components / {View nazwa składnika} / {Nazwa widoku}</span><span class="sxs-lookup"><span data-stu-id="a3952-156">/Views/Shared/Components/{View Component Name}/{View Name}</span></span>
* <span data-ttu-id="a3952-157">/ / Udostępnione/składników stron / {View nazwa składnika} / {Nazwa widoku}</span><span class="sxs-lookup"><span data-stu-id="a3952-157">/Pages/Shared/Components/{View Component Name}/{View Name}</span></span>

<span data-ttu-id="a3952-158">Ścieżka wyszukiwania ma zastosowanie do projektów za pomocą kontrolerów i widoków i stron Razor.</span><span class="sxs-lookup"><span data-stu-id="a3952-158">The search path applies to projects using controllers + views and Razor Pages.</span></span>

<span data-ttu-id="a3952-159">Domyślna nazwa widoku składnika widoku to *domyślne*, co oznacza, że plik widoku zazwyczaj będzie miała nazwę *Default.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="a3952-159">The default view name for a view component is *Default*, which means your view file will typically be named *Default.cshtml*.</span></span> <span data-ttu-id="a3952-160">Można określić nazwę innego widoku, tworząc wynik widoku składnika lub podczas wywoływania `View` metody.</span><span class="sxs-lookup"><span data-stu-id="a3952-160">You can specify a different view name when creating the view component result or when calling the `View` method.</span></span>

<span data-ttu-id="a3952-161">Firma Microsoft zaleca, nazwij plik widoku *Default.cshtml* i użyj *widoków/Shared/Components / {Nazwa widoku składnika} / {Nazwa widoku}* ścieżki.</span><span class="sxs-lookup"><span data-stu-id="a3952-161">We recommend you name the view file *Default.cshtml* and use the *Views/Shared/Components/{View Component Name}/{View Name}* path.</span></span> <span data-ttu-id="a3952-162">`PriorityList` Składnik widoku używane w tym przykładzie używa *Views/Shared/Components/PriorityList/Default.cshtml* widoku składnika widoku.</span><span class="sxs-lookup"><span data-stu-id="a3952-162">The `PriorityList` view component used in this sample uses *Views/Shared/Components/PriorityList/Default.cshtml* for the view component view.</span></span>

## <a name="invoking-a-view-component"></a><span data-ttu-id="a3952-163">Wywoływanie składnika widoku</span><span class="sxs-lookup"><span data-stu-id="a3952-163">Invoking a view component</span></span>

<span data-ttu-id="a3952-164">Aby użyć widoku składnika, wywołaj następujące wewnątrz widoku:</span><span class="sxs-lookup"><span data-stu-id="a3952-164">To use the view component, call the following inside a view:</span></span>

```cshtml
@await Component.InvokeAsync("Name of view component", {Anonymous Type Containing Parameters})
```

<span data-ttu-id="a3952-165">Parametry, które zostaną przekazane do `InvokeAsync` metody.</span><span class="sxs-lookup"><span data-stu-id="a3952-165">The parameters will be passed to the `InvokeAsync` method.</span></span> <span data-ttu-id="a3952-166">`PriorityList` Widoku składnika opracowanych w artykule jest wywoływany z *Views/ToDo/Index.cshtml* plik widoku.</span><span class="sxs-lookup"><span data-stu-id="a3952-166">The `PriorityList` view component developed in the article is invoked from the *Views/ToDo/Index.cshtml* view file.</span></span> <span data-ttu-id="a3952-167">Poniższa `InvokeAsync` metoda jest wywoływana z dwoma parametrami:</span><span class="sxs-lookup"><span data-stu-id="a3952-167">In the following, the `InvokeAsync` method is called with two parameters:</span></span>

[!code-cshtml[](view-components/sample/ViewCompFinal/Views/ToDo/IndexFinal.cshtml?range=35)]

::: moniker range=">= aspnetcore-1.1"

## <a name="invoking-a-view-component-as-a-tag-helper"></a><span data-ttu-id="a3952-168">Wywoływanie składnika widok jako pomocnika tagów</span><span class="sxs-lookup"><span data-stu-id="a3952-168">Invoking a view component as a Tag Helper</span></span>

<span data-ttu-id="a3952-169">Dla platformy ASP.NET Core 1.1 lub nowszym, można wywołać składnika widok jako [Pomocnik tagu](xref:mvc/views/tag-helpers/intro):</span><span class="sxs-lookup"><span data-stu-id="a3952-169">For ASP.NET Core 1.1 and higher, you can invoke a view component as a [Tag Helper](xref:mvc/views/tag-helpers/intro):</span></span>

[!code-cshtml[](view-components/sample/ViewCompFinal/Views/ToDo/IndexTagHelper.cshtml?range=37-38)]

<span data-ttu-id="a3952-170">Pascal — z uwzględnieniem wielkości liter parametry klasy i metody pomocników tagów są tłumaczone na ich [przypadek kebab](https://stackoverflow.com/questions/11273282/whats-the-name-for-dash-separated-case/12273101).</span><span class="sxs-lookup"><span data-stu-id="a3952-170">Pascal-cased class and method parameters for Tag Helpers are translated into their [kebab case](https://stackoverflow.com/questions/11273282/whats-the-name-for-dash-separated-case/12273101).</span></span> <span data-ttu-id="a3952-171">Pomocnik tagu do wywoływania składnika widoku używa `<vc></vc>` elementu.</span><span class="sxs-lookup"><span data-stu-id="a3952-171">The Tag Helper to invoke a view component uses the `<vc></vc>` element.</span></span> <span data-ttu-id="a3952-172">Składnik widoku określono w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="a3952-172">The view component is specified as follows:</span></span>

```cshtml
<vc:[view-component-name]
  parameter1="parameter1 value"
  parameter2="parameter2 value">
</vc:[view-component-name]>
```

<span data-ttu-id="a3952-173">Aby użyć widoku składnika jako pomocnika tagów, zarejestruj zestawu zawierającego za pomocą składnika widoku `@addTagHelper` dyrektywy.</span><span class="sxs-lookup"><span data-stu-id="a3952-173">To use a view component as a Tag Helper, register the assembly containing the view component using the `@addTagHelper` directive.</span></span> <span data-ttu-id="a3952-174">Jeśli składnik widoku znajduje się w zestawie o nazwie `MyWebApp`, Dodaj następujące dyrektywy *_ViewImports.cshtml* pliku:</span><span class="sxs-lookup"><span data-stu-id="a3952-174">If your view component is in an assembly called `MyWebApp`, add the following directive to the *_ViewImports.cshtml* file:</span></span>

```cshtml
@addTagHelper *, MyWebApp
```

<span data-ttu-id="a3952-175">Można zarejestrować składnika widok jako pomocnika tagów do każdego pliku, który odwołuje się do składnika widoku.</span><span class="sxs-lookup"><span data-stu-id="a3952-175">You can register a view component as a Tag Helper to any file that references the view component.</span></span> <span data-ttu-id="a3952-176">Zobacz [Zarządzanie zakresem pomocnika tagów](xref:mvc/views/tag-helpers/intro#managing-tag-helper-scope) Aby uzyskać więcej informacji o sposobie rejestrowania pomocników tagów.</span><span class="sxs-lookup"><span data-stu-id="a3952-176">See [Managing Tag Helper Scope](xref:mvc/views/tag-helpers/intro#managing-tag-helper-scope) for more information on how to register Tag Helpers.</span></span>

<span data-ttu-id="a3952-177">`InvokeAsync` Metodę używaną w ramach tego samouczka:</span><span class="sxs-lookup"><span data-stu-id="a3952-177">The `InvokeAsync` method used in this tutorial:</span></span>

[!code-cshtml[](view-components/sample/ViewCompFinal/Views/ToDo/IndexFinal.cshtml?range=35)]

<span data-ttu-id="a3952-178">W znacznikach Pomocnik tagu:</span><span class="sxs-lookup"><span data-stu-id="a3952-178">In Tag Helper markup:</span></span>

[!code-cshtml[](view-components/sample/ViewCompFinal/Views/ToDo/IndexTagHelper.cshtml?range=37-38)]

<span data-ttu-id="a3952-179">W przykładzie powyżej `PriorityList` widoku składnika staje się `priority-list`.</span><span class="sxs-lookup"><span data-stu-id="a3952-179">In the sample above, the `PriorityList` view component becomes `priority-list`.</span></span> <span data-ttu-id="a3952-180">Parametry do składnika widoku są przekazywane jako atrybuty w przypadku kebab.</span><span class="sxs-lookup"><span data-stu-id="a3952-180">The parameters to the view component are passed as attributes in kebab case.</span></span>

::: moniker-end

### <a name="invoking-a-view-component-directly-from-a-controller"></a><span data-ttu-id="a3952-181">Wywoływanie składnika widoku bezpośrednio za pomocą kontrolera</span><span class="sxs-lookup"><span data-stu-id="a3952-181">Invoking a view component directly from a controller</span></span>

<span data-ttu-id="a3952-182">Składniki widoków są zwykle wywoływani z widoku, ale można go wywołać bezpośrednio z metody kontrolera.</span><span class="sxs-lookup"><span data-stu-id="a3952-182">View components are typically invoked from a view, but you can invoke them directly from a controller method.</span></span> <span data-ttu-id="a3952-183">Podczas wyświetlania składników nie Definiuj punktów końcowych, takich jak kontrolerów, akcji kontrolera, która zwraca treść można łatwo zaimplementować `ViewComponentResult`.</span><span class="sxs-lookup"><span data-stu-id="a3952-183">While view components don't define endpoints like controllers, you can easily implement a controller action that returns the content of a `ViewComponentResult`.</span></span>

<span data-ttu-id="a3952-184">W tym przykładzie składnik ten widok jest wywoływany bezpośrednio z kontrolera:</span><span class="sxs-lookup"><span data-stu-id="a3952-184">In this example, the view component is called directly from the controller:</span></span>

[!code-csharp[](view-components/sample/ViewCompFinal/Controllers/ToDoController.cs?name=snippet_IndexVC)]

## <a name="walkthrough-creating-a-simple-view-component"></a><span data-ttu-id="a3952-185">Przewodnik: Tworzenie składnika Widok prosty</span><span class="sxs-lookup"><span data-stu-id="a3952-185">Walkthrough: Creating a simple view component</span></span>

<span data-ttu-id="a3952-186">[Pobierz](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/view-components/sample), tworzyć i testować kod startowy.</span><span class="sxs-lookup"><span data-stu-id="a3952-186">[Download](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/view-components/sample), build and test the starter code.</span></span> <span data-ttu-id="a3952-187">Jest to prosty projekt za pomocą `ToDo` kontrolera, który wyświetla listę *ToDo* elementów.</span><span class="sxs-lookup"><span data-stu-id="a3952-187">It's a simple project with a `ToDo` controller that displays a list of *ToDo* items.</span></span>

![Lista zadań do wykonania](view-components/_static/2dos.png)

### <a name="add-a-viewcomponent-class"></a><span data-ttu-id="a3952-189">Dodaj klasę ViewComponent</span><span class="sxs-lookup"><span data-stu-id="a3952-189">Add a ViewComponent class</span></span>

<span data-ttu-id="a3952-190">Tworzenie *ViewComponents* folderze i dodaj następującą `PriorityListViewComponent` klasy:</span><span class="sxs-lookup"><span data-stu-id="a3952-190">Create a *ViewComponents* folder and add the following `PriorityListViewComponent` class:</span></span>

[!code-csharp[](view-components/sample/ViewCompFinal/ViewComponents/PriorityListViewComponent1.cs?name=snippet1)]

<span data-ttu-id="a3952-191">Uwagi dotyczące kodu:</span><span class="sxs-lookup"><span data-stu-id="a3952-191">Notes on the code:</span></span>

* <span data-ttu-id="a3952-192">Widok klas składników mogą być zawarte w **wszelkie** folderu w projekcie.</span><span class="sxs-lookup"><span data-stu-id="a3952-192">View component classes can be contained in **any** folder in the project.</span></span>
* <span data-ttu-id="a3952-193">Klasa name PriorityList**ViewComponent** kończy się sufiksem **ViewComponent**, środowisko uruchomieniowe będzie używać ciągu "PriorityList" podczas odwoływania się do składnika klasy z widoku.</span><span class="sxs-lookup"><span data-stu-id="a3952-193">Because the class name PriorityList**ViewComponent** ends with the suffix **ViewComponent**, the runtime will use the string "PriorityList" when referencing the class component from a view.</span></span> <span data-ttu-id="a3952-194">Czy mogę wyjaśnię, które bardziej szczegółowo później.</span><span class="sxs-lookup"><span data-stu-id="a3952-194">I'll explain that in more detail later.</span></span>
* <span data-ttu-id="a3952-195">`[ViewComponent]` Atrybutu można zmienić nazwę używaną do się odwoływać do składnika widoku.</span><span class="sxs-lookup"><span data-stu-id="a3952-195">The `[ViewComponent]` attribute can change the name used to reference a view component.</span></span> <span data-ttu-id="a3952-196">Na przykład firma Microsoft może już o nazwie klasy `XYZ` i stosowane `ViewComponent` atrybutu:</span><span class="sxs-lookup"><span data-stu-id="a3952-196">For example, we could've named the class `XYZ` and applied the `ViewComponent` attribute:</span></span>

  ```csharp
  [ViewComponent(Name = "PriorityList")]
     public class XYZ : ViewComponent
     ```

* <span data-ttu-id="a3952-197">`[ViewComponent]` Atrybut powyżej informuje wybór składników widok, aby użyć nazwy `PriorityList` podczas wyszukiwania dla widoków skojarzonych ze składnikiem oraz użyć ciągu "PriorityList" podczas odwoływania się do składnika klasy z widoku.</span><span class="sxs-lookup"><span data-stu-id="a3952-197">The `[ViewComponent]` attribute above tells the view component selector to use the name `PriorityList` when looking for the views associated with the component, and to use the string "PriorityList" when referencing the class component from a view.</span></span> <span data-ttu-id="a3952-198">Czy mogę wyjaśnię, które bardziej szczegółowo później.</span><span class="sxs-lookup"><span data-stu-id="a3952-198">I'll explain that in more detail later.</span></span>
* <span data-ttu-id="a3952-199">Używany przez składnik [wstrzykiwanie zależności](../../fundamentals/dependency-injection.md) Aby udostępnić kontekst danych.</span><span class="sxs-lookup"><span data-stu-id="a3952-199">The component uses [dependency injection](../../fundamentals/dependency-injection.md) to make the data context available.</span></span>
* <span data-ttu-id="a3952-200">`InvokeAsync` udostępnia metody, która może zostać wywołana z widoku, a może zająć dowolnej liczby argumentów.</span><span class="sxs-lookup"><span data-stu-id="a3952-200">`InvokeAsync` exposes a method which can be called from a view, and it can take an arbitrary number of arguments.</span></span>
* <span data-ttu-id="a3952-201">`InvokeAsync` Metoda zwraca zestaw elementów `ToDo` elementów, które spełniają `isDone` i `maxPriority` parametrów.</span><span class="sxs-lookup"><span data-stu-id="a3952-201">The `InvokeAsync` method returns the set of `ToDo` items that satisfy the `isDone` and `maxPriority` parameters.</span></span>

### <a name="create-the-view-component-razor-view"></a><span data-ttu-id="a3952-202">Utwórz widok widoku Razor dla składnika</span><span class="sxs-lookup"><span data-stu-id="a3952-202">Create the view component Razor view</span></span>

* <span data-ttu-id="a3952-203">Tworzenie *widoków/Shared/Components* folderu.</span><span class="sxs-lookup"><span data-stu-id="a3952-203">Create the *Views/Shared/Components* folder.</span></span> <span data-ttu-id="a3952-204">Ten folder **musi** nosić *składniki*.</span><span class="sxs-lookup"><span data-stu-id="a3952-204">This folder **must** be named *Components*.</span></span>

* <span data-ttu-id="a3952-205">Tworzenie *widoków/Shared/składniki/PriorityList* folderu.</span><span class="sxs-lookup"><span data-stu-id="a3952-205">Create the *Views/Shared/Components/PriorityList* folder.</span></span> <span data-ttu-id="a3952-206">Ta nazwa folderu musi odpowiadać Nazwa klasy składnika widoku lub nazwa klasy minus sufiks (jeśli możemy stosowana Konwencja *ViewComponent* sufiksu w nazwie klasy).</span><span class="sxs-lookup"><span data-stu-id="a3952-206">This folder name must match the name of the view component class, or the name of the class minus the suffix (if we followed convention and used the *ViewComponent* suffix in the class name).</span></span> <span data-ttu-id="a3952-207">Jeśli użyto `ViewComponent` atrybutu, nazwa klasy będzie muszą być zgodne z nazwy atrybutu.</span><span class="sxs-lookup"><span data-stu-id="a3952-207">If you used the `ViewComponent` attribute, the class name would need to match the attribute designation.</span></span>

* <span data-ttu-id="a3952-208">Tworzenie *Views/Shared/Components/PriorityList/Default.cshtml* widoku Razor: [!code-cshtml[](view-components/sample/ViewCompFinal/Views/Shared/Components/PriorityList/Default1.cshtml)]</span><span class="sxs-lookup"><span data-stu-id="a3952-208">Create a *Views/Shared/Components/PriorityList/Default.cshtml* Razor view: [!code-cshtml[](view-components/sample/ViewCompFinal/Views/Shared/Components/PriorityList/Default1.cshtml)]</span></span>

   <span data-ttu-id="a3952-209">Widok Razor przyjmuje listę `TodoItem` i wyświetla je.</span><span class="sxs-lookup"><span data-stu-id="a3952-209">The Razor view takes a list of `TodoItem` and displays them.</span></span> <span data-ttu-id="a3952-210">Jeśli składnik widoku `InvokeAsync` metoda nie zakończy się pomyślnie Nazwa widoku (jak w naszym przykładzie), *domyślne* jest używana jako nazwa widoku, zgodnie z Konwencją.</span><span class="sxs-lookup"><span data-stu-id="a3952-210">If the view component `InvokeAsync` method doesn't pass the name of the view (as in our sample), *Default* is used for the view name by convention.</span></span> <span data-ttu-id="a3952-211">W dalszej części tego samouczka I opisano sposób przekazywania nazwy widoku.</span><span class="sxs-lookup"><span data-stu-id="a3952-211">Later in the tutorial, I'll show you how to pass the name of the view.</span></span> <span data-ttu-id="a3952-212">Aby zastąpić stylem domyślnym dla określonego kontrolera, Dodaj widok do folderu określonego kontrolera widoku (na przykład *Views/ToDo/Components/PriorityList/Default.cshtml)*.</span><span class="sxs-lookup"><span data-stu-id="a3952-212">To override the default styling for a specific controller, add a view to the controller-specific view folder (for example *Views/ToDo/Components/PriorityList/Default.cshtml)*.</span></span>

    <span data-ttu-id="a3952-213">Jeśli składnik widok jest specyficzne dla kontrolera, można dodać go do folderu określonego kontrolera (*Views/ToDo/Components/PriorityList/Default.cshtml*).</span><span class="sxs-lookup"><span data-stu-id="a3952-213">If the view component is controller-specific, you can add it to the controller-specific folder (*Views/ToDo/Components/PriorityList/Default.cshtml*).</span></span>

* <span data-ttu-id="a3952-214">Dodaj `div` zawierającym wywołanie składnika Lista priorytetu do dołu *Views/ToDo/index.cshtml* pliku:</span><span class="sxs-lookup"><span data-stu-id="a3952-214">Add a `div` containing a call to the priority list component to the bottom of the *Views/ToDo/index.cshtml* file:</span></span>

    [!code-cshtml[](view-components/sample/ViewCompFinal/Views/ToDo/IndexFirst.cshtml?range=34-38)]

<span data-ttu-id="a3952-215">Znaczniki `@await Component.InvokeAsync` pokazuje składnię do wywoływania składniki widoków.</span><span class="sxs-lookup"><span data-stu-id="a3952-215">The markup `@await Component.InvokeAsync` shows the syntax for calling view components.</span></span> <span data-ttu-id="a3952-216">Pierwszy argument jest nazwa składnika, który chcemy, aby wywołać lub wywołania.</span><span class="sxs-lookup"><span data-stu-id="a3952-216">The first argument is the name of the component we want to invoke or call.</span></span> <span data-ttu-id="a3952-217">Kolejne parametry są przekazywane do składnika.</span><span class="sxs-lookup"><span data-stu-id="a3952-217">Subsequent parameters are passed to the component.</span></span> <span data-ttu-id="a3952-218">`InvokeAsync` może być dowolną liczbę argumentów.</span><span class="sxs-lookup"><span data-stu-id="a3952-218">`InvokeAsync` can take an arbitrary number of arguments.</span></span>

<span data-ttu-id="a3952-219">Testowanie aplikacji.</span><span class="sxs-lookup"><span data-stu-id="a3952-219">Test the app.</span></span> <span data-ttu-id="a3952-220">Na poniższej ilustracji przedstawiono lista czynności do wykonania i elementów o priorytecie:</span><span class="sxs-lookup"><span data-stu-id="a3952-220">The following image shows the ToDo list and the priority items:</span></span>

![elementy listy i priorytet zadań do wykonania](view-components/_static/pi.png)

<span data-ttu-id="a3952-222">Składnik widoku można również wywołać bezpośrednio z kontrolera:</span><span class="sxs-lookup"><span data-stu-id="a3952-222">You can also call the view component directly from the controller:</span></span>

[!code-csharp[](view-components/sample/ViewCompFinal/Controllers/ToDoController.cs?name=snippet_IndexVC)]

![priorytet elementów z IndexVC akcji](view-components/_static/indexvc.png)

### <a name="specifying-a-view-name"></a><span data-ttu-id="a3952-224">Określanie nazwy widoku</span><span class="sxs-lookup"><span data-stu-id="a3952-224">Specifying a view name</span></span>

<span data-ttu-id="a3952-225">Składnik złożonego widoku może być konieczne określić widok innych niż domyślne, w niektórych warunkach.</span><span class="sxs-lookup"><span data-stu-id="a3952-225">A complex view component might need to specify a non-default view under some conditions.</span></span> <span data-ttu-id="a3952-226">Poniższy kod przedstawia sposób określania widok "PVC" z `InvokeAsync` metody.</span><span class="sxs-lookup"><span data-stu-id="a3952-226">The following code shows how to specify the "PVC" view  from the `InvokeAsync` method.</span></span> <span data-ttu-id="a3952-227">Aktualizacja `InvokeAsync` method in Class metoda `PriorityListViewComponent` klasy.</span><span class="sxs-lookup"><span data-stu-id="a3952-227">Update the `InvokeAsync` method in the `PriorityListViewComponent` class.</span></span>

[!code-csharp[](../../mvc/views/view-components/sample/ViewCompFinal/ViewComponents/PriorityListViewComponentFinal.cs?highlight=4,5,6,7,8,9&range=28-39)]

<span data-ttu-id="a3952-228">Kopiuj *Views/Shared/Components/PriorityList/Default.cshtml* pliku do widoku o nazwie *Views/Shared/Components/PriorityList/PVC.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="a3952-228">Copy the *Views/Shared/Components/PriorityList/Default.cshtml* file to a view named *Views/Shared/Components/PriorityList/PVC.cshtml*.</span></span> <span data-ttu-id="a3952-229">Dodaj nagłówek, aby wskazać, że jest używany widok PVC.</span><span class="sxs-lookup"><span data-stu-id="a3952-229">Add a heading to indicate the PVC view is being used.</span></span>

[!code-cshtml[](../../mvc/views/view-components/sample/ViewCompFinal/Views/Shared/Components/PriorityList/PVC.cshtml?highlight=3)]

<span data-ttu-id="a3952-230">Aktualizacja *Views/ToDo/Index.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="a3952-230">Update *Views/ToDo/Index.cshtml*:</span></span>

<!-- Views/ToDo/Index.cshtml is never imported, so change to test tutorial -->

[!code-cshtml[](view-components/sample/ViewCompFinal/Views/ToDo/IndexFinal.cshtml?range=35)]

<span data-ttu-id="a3952-231">Uruchom aplikację i sprawdź widok PVC.</span><span class="sxs-lookup"><span data-stu-id="a3952-231">Run the app and verify PVC view.</span></span>

![Priorytet widoku składnika](view-components/_static/pvc.png)

<span data-ttu-id="a3952-233">Jeśli nie jest renderowany widok PVC, sprawdź, czy są wywoływania składnika widoku z priorytetem 4 lub nowszy.</span><span class="sxs-lookup"><span data-stu-id="a3952-233">If the PVC view isn't rendered, verify you are calling the view component with a priority of 4 or higher.</span></span>

### <a name="examine-the-view-path"></a><span data-ttu-id="a3952-234">Sprawdź ścieżkę widoku</span><span class="sxs-lookup"><span data-stu-id="a3952-234">Examine the view path</span></span>

* <span data-ttu-id="a3952-235">Zmień parametr priorytet do trzech lub mniej, więc Wyświetl priorytet nie jest zwracany.</span><span class="sxs-lookup"><span data-stu-id="a3952-235">Change the priority parameter to three or less so the priority view isn't returned.</span></span>
* <span data-ttu-id="a3952-236">Tymczasowo zmień nazwę *Views/ToDo/Components/PriorityList/Default.cshtml* do *1Default.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="a3952-236">Temporarily rename the *Views/ToDo/Components/PriorityList/Default.cshtml* to *1Default.cshtml*.</span></span>
* <span data-ttu-id="a3952-237">Testowanie aplikacji, zostanie wyświetlony następujący błąd:</span><span class="sxs-lookup"><span data-stu-id="a3952-237">Test the app, you'll get the following error:</span></span>

   ```
   An unhandled exception occurred while processing the request.
   InvalidOperationException: The view 'Components/PriorityList/Default' wasn't found. The following locations were searched:
   /Views/ToDo/Components/PriorityList/Default.cshtml
   /Views/Shared/Components/PriorityList/Default.cshtml
   EnsureSuccessful
   ```

* <span data-ttu-id="a3952-238">Kopiuj *Views/ToDo/Components/PriorityList/1Default.cshtml* do *Views/Shared/Components/PriorityList/Default.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="a3952-238">Copy *Views/ToDo/Components/PriorityList/1Default.cshtml* to *Views/Shared/Components/PriorityList/Default.cshtml*.</span></span>
* <span data-ttu-id="a3952-239">Dodaj kilka znaczników w celu *Shared* ToDo widoku składnika Widok, aby wskazać, w widoku pochodzi z *Shared* folderu.</span><span class="sxs-lookup"><span data-stu-id="a3952-239">Add some markup to the *Shared* ToDo view component view to indicate the view is from the *Shared* folder.</span></span>
* <span data-ttu-id="a3952-240">Test **Shared** widok składnika.</span><span class="sxs-lookup"><span data-stu-id="a3952-240">Test the **Shared** component view.</span></span>

![Dane wyjściowe zadań do wykonania, przy użyciu widoku składnika współużytkowanego](view-components/_static/shared.png)

### <a name="avoiding-hard-coded-strings"></a><span data-ttu-id="a3952-242">Unikanie zakodowane sprzętowo ciągi</span><span class="sxs-lookup"><span data-stu-id="a3952-242">Avoiding hard-coded strings</span></span>

<span data-ttu-id="a3952-243">Jeśli chcesz skompilować bezpieczeństwa czasu, można zastąpić nazwy składnika ustaloną widoku nazwą klasy.</span><span class="sxs-lookup"><span data-stu-id="a3952-243">If you want compile time safety, you can replace the hard-coded view component name with the class name.</span></span> <span data-ttu-id="a3952-244">Utwórz składnik widoku bez sufiksu "ViewComponent":</span><span class="sxs-lookup"><span data-stu-id="a3952-244">Create the view component without the "ViewComponent" suffix:</span></span>

[!code-csharp[](../../mvc/views/view-components/sample/ViewCompFinal/ViewComponents/PriorityList.cs?highlight=10&range=5-35)]

<span data-ttu-id="a3952-245">Dodaj `using` instrukcję, aby Twoje Razor wyświetlanie plików i używanie `nameof` operator:</span><span class="sxs-lookup"><span data-stu-id="a3952-245">Add a `using` statement to your Razor view file, and use the `nameof` operator:</span></span>

[!code-cshtml[](view-components/sample/ViewCompFinal/Views/ToDo/IndexNameof.cshtml?range=1-6,35-)]

## <a name="perform-synchronous-work"></a><span data-ttu-id="a3952-246">Wykonaj Praca synchroniczna</span><span class="sxs-lookup"><span data-stu-id="a3952-246">Perform synchronous work</span></span>

<span data-ttu-id="a3952-247">Struktura obsługuje wywoływanie synchronicznej `Invoke` metody, jeśli nie trzeba wykonywać pracę asynchroniczną.</span><span class="sxs-lookup"><span data-stu-id="a3952-247">The framework handles invoking a synchronous `Invoke` method if you don't need to perform asynchronous work.</span></span> <span data-ttu-id="a3952-248">Poniższa metoda tworzy synchronicznego `Invoke` widoku składnika:</span><span class="sxs-lookup"><span data-stu-id="a3952-248">The following method creates a synchronous `Invoke` view component:</span></span>

```csharp
public class PriorityList : ViewComponent
{
    public IViewComponentResult Invoke(int maxPriority, bool isDone)
    {
        var items = new List<string> { $"maxPriority: {maxPriority}", $"isDone: {isDone}" };
        return View(items);
    }
}
```

<span data-ttu-id="a3952-249">Składnik widoku Razor plik listy ciągi przekazywane do `Invoke` — metoda (*Views/Home/Components/PriorityList/Default.cshtml*):</span><span class="sxs-lookup"><span data-stu-id="a3952-249">The view component's Razor file lists the strings passed to the `Invoke` method (*Views/Home/Components/PriorityList/Default.cshtml*):</span></span>

```cshtml
@model List<string>

<h3>Priority Items</h3>
<ul>
    @foreach (var item in Model)
    {
        <li>@item</li>
    }
</ul>
```

::: moniker range=">= aspnetcore-1.1"

<span data-ttu-id="a3952-250">Składnik widok został wywołany w pliku Razor (na przykład *Views/Home/Index.cshtml*) przy użyciu jednej z następujących metod:</span><span class="sxs-lookup"><span data-stu-id="a3952-250">The view component is invoked in a Razor file (for example, *Views/Home/Index.cshtml*) using one of the following approaches:</span></span>

* <xref:Microsoft.AspNetCore.Mvc.IViewComponentHelper>
* [<span data-ttu-id="a3952-251">Pomocnik tagu</span><span class="sxs-lookup"><span data-stu-id="a3952-251">Tag Helper</span></span>](xref:mvc/views/tag-helpers/intro)

<span data-ttu-id="a3952-252">Aby użyć <xref:Microsoft.AspNetCore.Mvc.IViewComponentHelper> podejście, wywołaj `Component.InvokeAsync`:</span><span class="sxs-lookup"><span data-stu-id="a3952-252">To use the <xref:Microsoft.AspNetCore.Mvc.IViewComponentHelper> approach, call `Component.InvokeAsync`:</span></span>

::: moniker-end

::: moniker range="< aspnetcore-1.1"

<span data-ttu-id="a3952-253">Składnik widok został wywołany w pliku Razor (na przykład *Views/Home/Index.cshtml*) przy użyciu <xref:Microsoft.AspNetCore.Mvc.IViewComponentHelper>.</span><span class="sxs-lookup"><span data-stu-id="a3952-253">The view component is invoked in a Razor file (for example, *Views/Home/Index.cshtml*) with <xref:Microsoft.AspNetCore.Mvc.IViewComponentHelper>.</span></span>

<span data-ttu-id="a3952-254">Wywołaj `Component.InvokeAsync`:</span><span class="sxs-lookup"><span data-stu-id="a3952-254">Call `Component.InvokeAsync`:</span></span>

::: moniker-end

```cshtml
@await Component.InvokeAsync(nameof(PriorityList), new { maxPriority = 4, isDone = true })
```

::: moniker range=">= aspnetcore-1.1"

<span data-ttu-id="a3952-255">Aby użyć pomocnika tagów, należy zarejestrować zestaw zawierający przy użyciu widoku składnika `@addTagHelper` — dyrektywa (składnik widoku znajduje się w zestawie o nazwie `MyWebApp`):</span><span class="sxs-lookup"><span data-stu-id="a3952-255">To use the Tag Helper, register the assembly containing the View Component using the `@addTagHelper` directive (the view component is in an assembly called `MyWebApp`):</span></span>

```cshtml
@addTagHelper *, MyWebApp
```

<span data-ttu-id="a3952-256">Użyj widoku składnika Pomocnik tagu w pliku znaczników Razor:</span><span class="sxs-lookup"><span data-stu-id="a3952-256">Use the view component Tag Helper in the Razor markup file:</span></span>

```cshtml
<vc:priority-list max-priority="999" is-done="false">
</vc:priority-list>
```

::: moniker-end

<span data-ttu-id="a3952-257">Podpis metody `PriorityList.Invoke` jest synchroniczna, ale Razor znajduje i wywołuje metodę z `Component.InvokeAsync` w pliku znaczników.</span><span class="sxs-lookup"><span data-stu-id="a3952-257">The method signature of `PriorityList.Invoke` is synchronous, but Razor finds and calls the method with `Component.InvokeAsync` in the markup file.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="a3952-258">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="a3952-258">Additional resources</span></span>

* [<span data-ttu-id="a3952-259">Wstrzykiwanie zależności do widoków</span><span class="sxs-lookup"><span data-stu-id="a3952-259">Dependency injection into views</span></span>](xref:mvc/views/dependency-injection)
