---
uid: mvc/overview/older-versions-1/views/creating-custom-html-helpers-vb
title: Tworzenie niestandardowych pomocników HTML (VB) | Dokumentacja firmy Microsoft
author: microsoft
description: Celem tego samouczka jest pokazują sposób tworzenia niestandardowych pomocników HTML używanego w ramach widoków MVC. Dzięki wykorzystaniu pomocnika kodu HTML...
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/07/2008
ms.topic: article
ms.assetid: f96f4800-19ef-44c0-b457-55e777eb5de8
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions-1/views/creating-custom-html-helpers-vb
msc.type: authoredcontent
ms.openlocfilehash: 0b1f4a6afc62eb23d4591d515e973298da4630f9
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37396601"
---
<a name="creating-custom-html-helpers-vb"></a><span data-ttu-id="f8df3-104">Tworzenie niestandardowych pomocników HTML (VB)</span><span class="sxs-lookup"><span data-stu-id="f8df3-104">Creating Custom HTML Helpers (VB)</span></span>
====================
<span data-ttu-id="f8df3-105">przez [firmy Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="f8df3-105">by [Microsoft](https://github.com/microsoft)</span></span>

[<span data-ttu-id="f8df3-106">Pobierz plik PDF</span><span class="sxs-lookup"><span data-stu-id="f8df3-106">Download PDF</span></span>](http://download.microsoft.com/download/1/1/f/11f721aa-d749-4ed7-bb89-a681b68894e6/ASPNET_MVC_Tutorial_9_VB.pdf)

> <span data-ttu-id="f8df3-107">Celem tego samouczka jest pokazują sposób tworzenia niestandardowych pomocników HTML używanego w ramach widoków MVC.</span><span class="sxs-lookup"><span data-stu-id="f8df3-107">The goal of this tutorial is to demonstrate how you can create custom HTML Helpers that you can use within your MVC views.</span></span> <span data-ttu-id="f8df3-108">Dzięki wykorzystaniu pomocników HTML, można zmniejszyć ilość tedious wpisywania tagów HTML, czy należy wykonać, aby utworzyć standardowej strony HTML.</span><span class="sxs-lookup"><span data-stu-id="f8df3-108">By taking advantage of HTML Helpers, you can reduce the amount of tedious typing of HTML tags that you must perform to create a standard HTML page.</span></span>


<span data-ttu-id="f8df3-109">Celem tego samouczka jest pokazują sposób tworzenia niestandardowych pomocników HTML używanego w ramach widoków MVC.</span><span class="sxs-lookup"><span data-stu-id="f8df3-109">The goal of this tutorial is to demonstrate how you can create custom HTML Helpers that you can use within your MVC views.</span></span> <span data-ttu-id="f8df3-110">Dzięki wykorzystaniu pomocników HTML, można zmniejszyć ilość tedious wpisywania tagów HTML, czy należy wykonać, aby utworzyć standardowej strony HTML.</span><span class="sxs-lookup"><span data-stu-id="f8df3-110">By taking advantage of HTML Helpers, you can reduce the amount of tedious typing of HTML tags that you must perform to create a standard HTML page.</span></span>

<span data-ttu-id="f8df3-111">W pierwszej części tego samouczka I opisano niektóre istniejące pomocników HTML dołączone do struktury ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="f8df3-111">In the first part of this tutorial, I describe some of the existing HTML Helpers included with the ASP.NET MVC framework.</span></span> <span data-ttu-id="f8df3-112">Następnie na opisują jest tworzenie niestandardowych pomocników HTML na dwa sposoby: opisano sposób tworzenia niestandardowych pomocników HTML, tworząc udostępnionej metody i tworząc metodą rozszerzenia.</span><span class="sxs-lookup"><span data-stu-id="f8df3-112">Next, I describe two methods of creating custom HTML Helpers: I explain how to create custom HTML Helpers by creating a shared method and by creating an extension method.</span></span>

## <a name="understanding-html-helpers"></a><span data-ttu-id="f8df3-113">Opis pomocników HTML</span><span class="sxs-lookup"><span data-stu-id="f8df3-113">Understanding HTML Helpers</span></span>

<span data-ttu-id="f8df3-114">Pomocnik kodu HTML jest po prostu metody, która zwraca wartość typu ciąg.</span><span class="sxs-lookup"><span data-stu-id="f8df3-114">An HTML Helper is just a method that returns a string.</span></span> <span data-ttu-id="f8df3-115">Ciąg może reprezentować dowolnego typu zawartości.</span><span class="sxs-lookup"><span data-stu-id="f8df3-115">The string can represent any type of content that you want.</span></span> <span data-ttu-id="f8df3-116">Na przykład, można użyć pomocników HTML do renderowania standardowych znaczników HTML, takich jak HTML `<input>` i `<img>` tagów.</span><span class="sxs-lookup"><span data-stu-id="f8df3-116">For example, you can use HTML Helpers to render standard HTML tags like HTML `<input>` and `<img>` tags.</span></span> <span data-ttu-id="f8df3-117">Możesz również użyć pomocników HTML do renderowania zawartości bardziej złożonych, takie jak pasek kart lub tabeli HTML danych bazy danych.</span><span class="sxs-lookup"><span data-stu-id="f8df3-117">You also can use HTML Helpers to render more complex content such as a tab strip or an HTML table of database data.</span></span>

<span data-ttu-id="f8df3-118">Platforma ASP.NET MVC zawiera następujący zestaw standardowych pomocników HTML (nie jest to pełna lista):</span><span class="sxs-lookup"><span data-stu-id="f8df3-118">The ASP.NET MVC framework includes the following set of standard HTML Helpers (this is not a complete list):</span></span>

- <span data-ttu-id="f8df3-119">Html.ActionLink()</span><span class="sxs-lookup"><span data-stu-id="f8df3-119">Html.ActionLink()</span></span>
- <span data-ttu-id="f8df3-120">Html.BeginForm()</span><span class="sxs-lookup"><span data-stu-id="f8df3-120">Html.BeginForm()</span></span>
- <span data-ttu-id="f8df3-121">Html.CheckBox()</span><span class="sxs-lookup"><span data-stu-id="f8df3-121">Html.CheckBox()</span></span>
- <span data-ttu-id="f8df3-122">Html.DropDownList()</span><span class="sxs-lookup"><span data-stu-id="f8df3-122">Html.DropDownList()</span></span>
- <span data-ttu-id="f8df3-123">Html.EndForm()</span><span class="sxs-lookup"><span data-stu-id="f8df3-123">Html.EndForm()</span></span>
- <span data-ttu-id="f8df3-124">Html.Hidden()</span><span class="sxs-lookup"><span data-stu-id="f8df3-124">Html.Hidden()</span></span>
- <span data-ttu-id="f8df3-125">Html.ListBox()</span><span class="sxs-lookup"><span data-stu-id="f8df3-125">Html.ListBox()</span></span>
- <span data-ttu-id="f8df3-126">Html.Password()</span><span class="sxs-lookup"><span data-stu-id="f8df3-126">Html.Password()</span></span>
- <span data-ttu-id="f8df3-127">Html.RadioButton()</span><span class="sxs-lookup"><span data-stu-id="f8df3-127">Html.RadioButton()</span></span>
- <span data-ttu-id="f8df3-128">Html.TextArea()</span><span class="sxs-lookup"><span data-stu-id="f8df3-128">Html.TextArea()</span></span>
- <span data-ttu-id="f8df3-129">Html.TextBox()</span><span class="sxs-lookup"><span data-stu-id="f8df3-129">Html.TextBox()</span></span>

<span data-ttu-id="f8df3-130">Rozważmy na przykład formularz w ofercie 1.</span><span class="sxs-lookup"><span data-stu-id="f8df3-130">For example, consider the form in Listing 1.</span></span> <span data-ttu-id="f8df3-131">Ta forma jest renderowany przy pomocy dwóch standardowa pomocników HTML (patrz rysunek 1).</span><span class="sxs-lookup"><span data-stu-id="f8df3-131">This form is rendered with the help of two of the standard HTML Helpers (see Figure 1).</span></span> <span data-ttu-id="f8df3-132">Ten formularz używa `Html.BeginForm()` i `Html.TextBox()` metody pomocnika.</span><span class="sxs-lookup"><span data-stu-id="f8df3-132">This form uses the `Html.BeginForm()` and `Html.TextBox()` Helper methods.</span></span>


<span data-ttu-id="f8df3-133">[![Strony renderowane przy użyciu pomocników HTML](creating-custom-html-helpers-vb/_static/image2.png)](creating-custom-html-helpers-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="f8df3-133">[![Page rendered with HTML Helpers](creating-custom-html-helpers-vb/_static/image2.png)](creating-custom-html-helpers-vb/_static/image1.png)</span></span>

<span data-ttu-id="f8df3-134">**Rysunek 01**: strony renderowane przy użyciu pomocników HTML ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-custom-html-helpers-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="f8df3-134">**Figure 01**: Page rendered with HTML Helpers ([Click to view full-size image](creating-custom-html-helpers-vb/_static/image3.png))</span></span>


<span data-ttu-id="f8df3-135">**1 — Lista `Views\Home\Index.aspx`**</span><span class="sxs-lookup"><span data-stu-id="f8df3-135">**Listing 1 – `Views\Home\Index.aspx`**</span></span>

[!code-aspx[Main](creating-custom-html-helpers-vb/samples/sample1.aspx)]

<span data-ttu-id="f8df3-136">`Html.BeginForm()` Metody pomocnika służy do tworzenia kodu HTML otwierający i zamykający `<form>` tagów.</span><span class="sxs-lookup"><span data-stu-id="f8df3-136">The `Html.BeginForm()` Helper method is used to create the opening and closing HTML `<form>` tags.</span></span> <span data-ttu-id="f8df3-137">Należy zauważyć, że `Html.BeginForm()` metoda jest wywoływana w obrębie za pomocą instrukcji.</span><span class="sxs-lookup"><span data-stu-id="f8df3-137">Notice that the `Html.BeginForm()` method is called within a using statement.</span></span> <span data-ttu-id="f8df3-138">Za pomocą instrukcji zapewnia, że `<form>` tag zostanie zamknięty na końcu używając bloku.</span><span class="sxs-lookup"><span data-stu-id="f8df3-138">The using statement ensures that the `<form>` tag gets closed at the end of the using block.</span></span>

<span data-ttu-id="f8df3-139">Jeśli wolisz, zamiast tworzyć za pomocą bloku, można wywołać metodę pomocnika Html.EndForm(), aby zamknąć `<form>` tagu.</span><span class="sxs-lookup"><span data-stu-id="f8df3-139">If you prefer, instead of creating a using block, you can call the Html.EndForm() Helper method to close the `<form>` tag.</span></span> <span data-ttu-id="f8df3-140">Użyj jednego z tych podejście do tworzenia otwierający i zamykający `<form>` tag, który wydaje się być najbardziej intuicyjnym do Ciebie.</span><span class="sxs-lookup"><span data-stu-id="f8df3-140">Use whichever approach to creating an opening and closing `<form>` tag that seems most intuitive to you.</span></span>

<span data-ttu-id="f8df3-141">`Html.TextBox()` Metody pomocnika służą do renderowania elementów HTML w ofercie 1 `<input>` tagów.</span><span class="sxs-lookup"><span data-stu-id="f8df3-141">The `Html.TextBox()` Helper methods are used in Listing 1 to render HTML `<input>` tags.</span></span> <span data-ttu-id="f8df3-142">Jeśli wybierzesz Wyświetl źródło w przeglądarce zobaczysz do źródła HTML w ofercie 2.</span><span class="sxs-lookup"><span data-stu-id="f8df3-142">If you select view source in your browser then you see the HTML source in Listing 2.</span></span> <span data-ttu-id="f8df3-143">Należy zauważyć, że źródłowa zawiera standardowych znaczników HTML.</span><span class="sxs-lookup"><span data-stu-id="f8df3-143">Notice that the source contains standard HTML tags.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="f8df3-144">Należy zauważyć, że `Html.TextBox()`— HTML pomocnika jest renderowany przy użyciu `<%= %>` tagów zamiast `<% %>` tagów.</span><span class="sxs-lookup"><span data-stu-id="f8df3-144">notice that the `Html.TextBox()`-HTML Helper is rendered with `<%= %>` tags instead of `<% %>` tags.</span></span> <span data-ttu-id="f8df3-145">Jeśli nie dołączysz znaku równości, następnie nic nie pobiera renderowane w przeglądarce.</span><span class="sxs-lookup"><span data-stu-id="f8df3-145">If you don't include the equal sign, then nothing gets rendered to the browser.</span></span>

<span data-ttu-id="f8df3-146">Platforma ASP.NET MVC zawiera niewielki zestaw pomocników.</span><span class="sxs-lookup"><span data-stu-id="f8df3-146">The ASP.NET MVC framework contains a small set of helpers.</span></span> <span data-ttu-id="f8df3-147">Prawdopodobnie należy rozszerzyć platformę MVC za pomocą niestandardowych pomocników HTML.</span><span class="sxs-lookup"><span data-stu-id="f8df3-147">Most likely, you will need to extend the MVC framework with custom HTML Helpers.</span></span> <span data-ttu-id="f8df3-148">W pozostałej części tego samouczka dowiesz się, tworzenie niestandardowych pomocników HTML na dwa sposoby.</span><span class="sxs-lookup"><span data-stu-id="f8df3-148">In the remainder of this tutorial, you learn two methods of creating custom HTML Helpers.</span></span>

<span data-ttu-id="f8df3-149">**2 — Lista `Index.aspx Source`**</span><span class="sxs-lookup"><span data-stu-id="f8df3-149">**Listing 2 – `Index.aspx Source`**</span></span>

[!code-aspx[Main](creating-custom-html-helpers-vb/samples/sample2.aspx)]

### <a name="creating-html-helpers-with-shared-methods"></a><span data-ttu-id="f8df3-150">Tworzenie pomocników HTML za pomocą udostępnionych metod</span><span class="sxs-lookup"><span data-stu-id="f8df3-150">Creating HTML Helpers with Shared Methods</span></span>

<span data-ttu-id="f8df3-151">Najprostszym sposobem utworzenia nowego pomocnika kodu HTML jest tworzenie udostępnionego metody, która zwraca wartość typu ciąg.</span><span class="sxs-lookup"><span data-stu-id="f8df3-151">The easiest way to create a new HTML Helper is to create a shared method that returns a string.</span></span> <span data-ttu-id="f8df3-152">Wyobraź sobie, na przykład zdecydujesz utworzyć nowego pomocnika HTML, który powoduje wyświetlenie kodu HTML `<label>` tagu.</span><span class="sxs-lookup"><span data-stu-id="f8df3-152">Imagine, for example, that you decide to create a new HTML Helper that renders an HTML `<label>` tag.</span></span> <span data-ttu-id="f8df3-153">Można użyć klasy w ofercie 2 do renderowania `<label>`.</span><span class="sxs-lookup"><span data-stu-id="f8df3-153">You can use the class in Listing 2 to render a `<label>`.</span></span>

<span data-ttu-id="f8df3-154">**2 — Lista `Helpers\LabelHelper.vb`**</span><span class="sxs-lookup"><span data-stu-id="f8df3-154">**Listing 2 – `Helpers\LabelHelper.vb`**</span></span>

[!code-vb[Main](creating-custom-html-helpers-vb/samples/sample3.vb)]

<span data-ttu-id="f8df3-155">Nie ma nic specjalnego informacje o klasie w ofercie 2.</span><span class="sxs-lookup"><span data-stu-id="f8df3-155">There is nothing special about the class in Listing 2.</span></span> <span data-ttu-id="f8df3-156">`Label()` Metoda po prostu zwraca ciąg.</span><span class="sxs-lookup"><span data-stu-id="f8df3-156">The `Label()` method simply returns a string.</span></span>

<span data-ttu-id="f8df3-157">Używa zmodyfikowanego widoku indeksu w ofercie 3 `LabelHelper` do renderowania elementów HTML `<label>` tagów.</span><span class="sxs-lookup"><span data-stu-id="f8df3-157">The modified Index view in Listing 3 uses the `LabelHelper` to render HTML `<label>` tags.</span></span> <span data-ttu-id="f8df3-158">Należy zauważyć, że widok zawiera `<%@ imports %>` dyrektywę, który importuje Application1.Helpers przestrzeni nazw.</span><span class="sxs-lookup"><span data-stu-id="f8df3-158">Notice that the view includes an `<%@ imports %>` directive that imports the Application1.Helpers namespace.</span></span>

<span data-ttu-id="f8df3-159">**2 — Lista `Views\Home\Index2.aspx`**</span><span class="sxs-lookup"><span data-stu-id="f8df3-159">**Listing 2 – `Views\Home\Index2.aspx`**</span></span>

[!code-aspx[Main](creating-custom-html-helpers-vb/samples/sample4.aspx)]

### <a name="creating-html-helpers-with-extension-methods"></a><span data-ttu-id="f8df3-160">Tworzenie pomocników HTML przy użyciu metody rozszerzenia</span><span class="sxs-lookup"><span data-stu-id="f8df3-160">Creating HTML Helpers with Extension Methods</span></span>

<span data-ttu-id="f8df3-161">Jeśli chcesz tworzyć pomocników HTML, które działają podobnie, takich jak standardowy pomocników HTML zawarte w platformę ASP.NET MVC, a następnie należy utworzyć metody rozszerzenia.</span><span class="sxs-lookup"><span data-stu-id="f8df3-161">If you want to create HTML Helpers that work just like the standard HTML Helpers included in the ASP.NET MVC framework then you need to create extension methods.</span></span> <span data-ttu-id="f8df3-162">Metody rozszerzające umożliwiają dodawanie nowych metod do istniejącej klasy.</span><span class="sxs-lookup"><span data-stu-id="f8df3-162">Extension methods enable you to add new methods to an existing class.</span></span> <span data-ttu-id="f8df3-163">Podczas tworzenia metodę pomocnika kodu HTML, możesz dodać nowe metody `HtmlHelper` klasy reprezentowane przez właściwości Html widoku.</span><span class="sxs-lookup"><span data-stu-id="f8df3-163">When creating an HTML Helper method, you add new methods to the `HtmlHelper` class represented by a view's Html property.</span></span>

<span data-ttu-id="f8df3-164">Moduł Visual Basic w ofercie 3 dodaje metodę rozszerzenia o nazwie `Label()` do `HtmlHelper` klasy.</span><span class="sxs-lookup"><span data-stu-id="f8df3-164">The Visual Basic module in Listing 3 adds an extension method named `Label()` to the `HtmlHelper` class.</span></span> <span data-ttu-id="f8df3-165">Istnieje kilka rzeczy, które powinny być informacja dotycząca ten moduł.</span><span class="sxs-lookup"><span data-stu-id="f8df3-165">There are a couple of things that you should notice about this module.</span></span> <span data-ttu-id="f8df3-166">Najpierw zwróć uwagę, że moduł zostanie nadany `<Extension()>` atrybutu.</span><span class="sxs-lookup"><span data-stu-id="f8df3-166">First, notice that the module is decorated with the `<Extension()>` attribute.</span></span> <span data-ttu-id="f8df3-167">Aby można było używać tego atrybutu, należy zaimportować `System.Runtime.CompilerServices` przestrzeni nazw</span><span class="sxs-lookup"><span data-stu-id="f8df3-167">In order to use this attribute, you must import the `System.Runtime.CompilerServices` namespace</span></span>

<span data-ttu-id="f8df3-168">Po drugie, zwróć uwagę, że pierwszy parametr `Label()` metody reprezentująca `HtmlHelper` klasy.</span><span class="sxs-lookup"><span data-stu-id="f8df3-168">Second, notice that the first parameter of the `Label()` method represents the `HtmlHelper` class.</span></span> <span data-ttu-id="f8df3-169">Pierwszy parametr metody rozszerzenia wskazuje klasę, która rozszerza metoda rozszerzenia.</span><span class="sxs-lookup"><span data-stu-id="f8df3-169">The first parameter of an extension method indicates the class that the extension method extends.</span></span>

<span data-ttu-id="f8df3-170">**3 — lista `Helpers\LabelExtensions.vb`**</span><span class="sxs-lookup"><span data-stu-id="f8df3-170">**Listing 3 – `Helpers\LabelExtensions.vb`**</span></span>

[!code-vb[Main](creating-custom-html-helpers-vb/samples/sample5.vb)]

<span data-ttu-id="f8df3-171">Po utworzeniu metodę rozszerzenia, a następnie skompilować aplikację pomyślnym, metoda rozszerzenia pojawia się w Visual Studio technologii Intellisense, podobnie jak wszystkie inne metody klasy (patrz rysunek 2).</span><span class="sxs-lookup"><span data-stu-id="f8df3-171">After you create an extension method, and build your application successfully, the extension method appears in Visual Studio Intellisense like all of the other methods of a class (see Figure 2).</span></span> <span data-ttu-id="f8df3-172">Jedyną różnicą jest to rozszerzenie, które metody są wyświetlane z symbolem specjalne obok nich (ikona strzałki w dół).</span><span class="sxs-lookup"><span data-stu-id="f8df3-172">The only difference is that extension methods appear with a special symbol next to them (an icon of a downward arrow).</span></span>


<span data-ttu-id="f8df3-173">[![Przy użyciu metody rozszerzenia Html.Label()](creating-custom-html-helpers-vb/_static/image5.png)](creating-custom-html-helpers-vb/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="f8df3-173">[![Using the Html.Label() extension method](creating-custom-html-helpers-vb/_static/image5.png)](creating-custom-html-helpers-vb/_static/image4.png)</span></span>

<span data-ttu-id="f8df3-174">**Rysunek 02**: przy użyciu metody rozszerzenia Html.Label() ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-custom-html-helpers-vb/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="f8df3-174">**Figure 02**: Using the Html.Label() extension method  ([Click to view full-size image](creating-custom-html-helpers-vb/_static/image6.png))</span></span>


<span data-ttu-id="f8df3-175">Zmodyfikowany widok indeksu w ofercie 4 używa Html.Label() metody rozszerzenia do renderowania, wszystkie jego &lt;etykiety&gt; tagów.</span><span class="sxs-lookup"><span data-stu-id="f8df3-175">The modified Index view in Listing 4 uses the Html.Label() extension method to render all of its &lt;label&gt; tags.</span></span>

<span data-ttu-id="f8df3-176">**4 — lista `Views\Home\Index3.aspx`**</span><span class="sxs-lookup"><span data-stu-id="f8df3-176">**Listing 4 – `Views\Home\Index3.aspx`**</span></span>

[!code-aspx[Main](creating-custom-html-helpers-vb/samples/sample6.aspx)]

## <a name="summary"></a><span data-ttu-id="f8df3-177">Podsumowanie</span><span class="sxs-lookup"><span data-stu-id="f8df3-177">Summary</span></span>

<span data-ttu-id="f8df3-178">W tym samouczku przedstawiono tworzenie niestandardowych pomocników HTML na dwa sposoby.</span><span class="sxs-lookup"><span data-stu-id="f8df3-178">In this tutorial, you learned two methods of creating custom HTML Helpers.</span></span> <span data-ttu-id="f8df3-179">Po pierwsze, wiesz, jak utworzyć niestandardową `Label()` pomocnika kodu HTML, tworząc udostępnionej metody, która zwraca wartość typu ciąg.</span><span class="sxs-lookup"><span data-stu-id="f8df3-179">First, you learned how to create a custom `Label()` HTML Helper by creating a shared method that returns a string.</span></span> <span data-ttu-id="f8df3-180">Następnie pokazano, jak utworzyć niestandardową `Label()` metody pomocnika kodu HTML, tworząc metodę rozszerzającą o `HtmlHelper` klasy.</span><span class="sxs-lookup"><span data-stu-id="f8df3-180">Next, you learned how to create a custom `Label()` HTML Helper method by creating an extension method on the `HtmlHelper` class.</span></span>

<span data-ttu-id="f8df3-181">W tym samouczku I koncentruje się na tworzeniu bardzo prostą metodę pomocnika kodu HTML.</span><span class="sxs-lookup"><span data-stu-id="f8df3-181">In this tutorial, I focused on building an extremely simple HTML Helper method.</span></span> <span data-ttu-id="f8df3-182">Należy pamiętać, że pomocnika kodu HTML, może być tak skomplikowane, jak chcesz.</span><span class="sxs-lookup"><span data-stu-id="f8df3-182">Realize that an HTML Helper can be as complicated as you want.</span></span> <span data-ttu-id="f8df3-183">Można tworzyć pomocników HTML, renderowanie sformatowanej zawartości, takich jak widoki drzewa i menu tabel bazy danych.</span><span class="sxs-lookup"><span data-stu-id="f8df3-183">You can build HTML Helpers that render rich content such as tree views, menus, or tables of database data.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="f8df3-184">[Poprzednie](asp-net-mvc-views-overview-vb.md)
> [dalej](using-the-tagbuilder-class-to-build-html-helpers-vb.md)</span><span class="sxs-lookup"><span data-stu-id="f8df3-184">[Previous](asp-net-mvc-views-overview-vb.md)
[Next](using-the-tagbuilder-class-to-build-html-helpers-vb.md)</span></span>
