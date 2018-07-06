---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-5
title: 'Część 5: Tworzenie dynamicznego interfejsu użytkownika z użyciem Knockout.js | Dokumentacja firmy Microsoft'
author: MikeWasson
description: ''
ms.author: aspnetcontent
ms.date: 07/04/2012
ms.assetid: 9d9cb3b0-f4a7-434e-a508-9fc0ad0eb813
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-5
msc.type: authoredcontent
ms.openlocfilehash: 7d8bd4732ecf73c14251ceebfd667310f6e197b4
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/05/2018
ms.locfileid: "37801696"
---
<a name="part-5-creating-a-dynamic-ui-with-knockoutjs"></a><span data-ttu-id="906d2-102">Część 5: Tworzenie dynamicznego interfejsu użytkownika z użyciem Knockout.js</span><span class="sxs-lookup"><span data-stu-id="906d2-102">Part 5: Creating a Dynamic UI with Knockout.js</span></span>
====================
<span data-ttu-id="906d2-103">przez [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="906d2-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="906d2-104">Pobieranie ukończone projektu</span><span class="sxs-lookup"><span data-stu-id="906d2-104">Download Completed Project</span></span>](http://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

## <a name="creating-a-dynamic-ui-with-knockoutjs"></a><span data-ttu-id="906d2-105">Tworzenie dynamicznego interfejsu użytkownika z użyciem Knockout.js</span><span class="sxs-lookup"><span data-stu-id="906d2-105">Creating a Dynamic UI with Knockout.js</span></span>

<span data-ttu-id="906d2-106">W tej sekcji użyjemy struktura Knockout.js Dodawanie funkcji do widoku administratora.</span><span class="sxs-lookup"><span data-stu-id="906d2-106">In this section, we'll use Knockout.js to add functionality to the Admin view.</span></span>

<span data-ttu-id="906d2-107">[Struktura Knockout.js](http://knockoutjs.com/) to biblioteka języka Javascript, która ułatwia powiązania kontrolek HTML do danych.</span><span class="sxs-lookup"><span data-stu-id="906d2-107">[Knockout.js](http://knockoutjs.com/) is a Javascript library that makes it easy to bind HTML controls to data.</span></span> <span data-ttu-id="906d2-108">Struktura Knockout.js używa wzorca Model-View-ViewModel (MVVM).</span><span class="sxs-lookup"><span data-stu-id="906d2-108">Knockout.js uses the Model-View-ViewModel (MVVM) pattern.</span></span>

- <span data-ttu-id="906d2-109">*Modelu* jest po stronie serwera reprezentacja danych w domenie firmy (w naszym przypadku, produkty i zamówienia).</span><span class="sxs-lookup"><span data-stu-id="906d2-109">The *model* is the server-side representation of the data in the business domain (in our case, products and orders).</span></span>
- <span data-ttu-id="906d2-110">*Widoku* to warstwa prezentacji (HTML).</span><span class="sxs-lookup"><span data-stu-id="906d2-110">The *view* is the presentation layer (HTML).</span></span>
- <span data-ttu-id="906d2-111">*Model widoku* jest obiekt Javascript, która przechowuje dane modelu.</span><span class="sxs-lookup"><span data-stu-id="906d2-111">The *view-model* is a Javascript object that holds the model data.</span></span> <span data-ttu-id="906d2-112">Model widoku jest abstrakcji kodu interfejsu użytkownika.</span><span class="sxs-lookup"><span data-stu-id="906d2-112">The view-model is a code abstraction of the UI.</span></span> <span data-ttu-id="906d2-113">Go nie zna reprezentacji w formacie HTML.</span><span class="sxs-lookup"><span data-stu-id="906d2-113">It has no knowledge of the HTML representation.</span></span> <span data-ttu-id="906d2-114">Zamiast tego reprezentuje abstrakcyjnej funkcji widoku, takie jak "Lista elementów".</span><span class="sxs-lookup"><span data-stu-id="906d2-114">Instead, it represents abstract features of the view, such as "a list of items".</span></span>

<span data-ttu-id="906d2-115">Widok jest powiązany z danymi model widoku.</span><span class="sxs-lookup"><span data-stu-id="906d2-115">The view is data-bound to the view-model.</span></span> <span data-ttu-id="906d2-116">Model widoku aktualizacje są automatycznie odzwierciedlane w widoku.</span><span class="sxs-lookup"><span data-stu-id="906d2-116">Updates to the view-model are automatically reflected in the view.</span></span> <span data-ttu-id="906d2-117">Model widoku również pobiera zdarzenia z widoku, takich jak kliknięcia przycisków i wykonuje operacje na modelu, na przykład utworzyć zamówienie.</span><span class="sxs-lookup"><span data-stu-id="906d2-117">The view-model also gets events from the view, such as button clicks, and performs operations on the model, such as creating an order.</span></span>

![](using-web-api-with-entity-framework-part-5/_static/image1.png)

<span data-ttu-id="906d2-118">Najpierw zdefiniujemy model widoku.</span><span class="sxs-lookup"><span data-stu-id="906d2-118">First we'll define the view-model.</span></span> <span data-ttu-id="906d2-119">Po tym firma Microsoft będzie powiązać kod znaczników HTML model widoku.</span><span class="sxs-lookup"><span data-stu-id="906d2-119">After that, we will bind the HTML markup to the view-model.</span></span>

<span data-ttu-id="906d2-120">Dodaj następującą sekcję Razor do Admin.cshtml:</span><span class="sxs-lookup"><span data-stu-id="906d2-120">Add the following Razor section to Admin.cshtml:</span></span>

[!code-cshtml[Main](using-web-api-with-entity-framework-part-5/samples/sample1.cshtml)]

<span data-ttu-id="906d2-121">W tej sekcji można dodać w dowolnym miejscu w pliku.</span><span class="sxs-lookup"><span data-stu-id="906d2-121">You can add this section anywhere in the file.</span></span> <span data-ttu-id="906d2-122">Podczas renderowania widoku sekcji pojawi się w dolnej części strony HTML bezpośrednio przed zamykającym &lt;/body&gt; tagu.</span><span class="sxs-lookup"><span data-stu-id="906d2-122">When the view is rendered, the section appears at the bottom of the HTML page, right before the closing &lt;/body&gt; tag.</span></span>

<span data-ttu-id="906d2-123">Wszystkie skrypt na tej stronie zaczną się wewnątrz tagu wskazywanym przez komentarz:</span><span class="sxs-lookup"><span data-stu-id="906d2-123">All of the script for this page will go inside the script tag indicated by the comment:</span></span>

[!code-html[Main](using-web-api-with-entity-framework-part-5/samples/sample2.html)]

<span data-ttu-id="906d2-124">Najpierw należy zdefiniować klasę modelu widoku:</span><span class="sxs-lookup"><span data-stu-id="906d2-124">First, define a view-model class:</span></span>

[!code-javascript[Main](using-web-api-with-entity-framework-part-5/samples/sample3.js)]

<span data-ttu-id="906d2-125">**ko.observableArray** to specjalny rodzaj obiektu w Knockout, o nazwie *obserwowalnymi*.</span><span class="sxs-lookup"><span data-stu-id="906d2-125">**ko.observableArray** is a special kind of object in Knockout, called an *observable*.</span></span> <span data-ttu-id="906d2-126">Z [dokumentacji struktura Knockout.js](http://knockoutjs.com/documentation/observables.html): zauważalny jest "JavaScript obiekt, który może generować powiadomienia subskrybentów o zmianach."</span><span class="sxs-lookup"><span data-stu-id="906d2-126">From the [Knockout.js documentation](http://knockoutjs.com/documentation/observables.html): An observable is a "JavaScript object that can notify subscribers about changes."</span></span> <span data-ttu-id="906d2-127">Po zmianie zawartości możliwość obserwowania, widok jest automatycznie aktualizowana do dopasowania.</span><span class="sxs-lookup"><span data-stu-id="906d2-127">When the contents of an observable change, the view is automatically updated to match.</span></span>

<span data-ttu-id="906d2-128">Aby wypełnić `products` tablicy, przesyłania żądania AJAX do internetowego interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="906d2-128">To populate the `products` array, make an AJAX request to the web API.</span></span> <span data-ttu-id="906d2-129">Odwołaj, firma Microsoft przechowywane podstawowy identyfikator URI dla interfejsu API w zbiorze widoku (zobacz [część 4](using-web-api-with-entity-framework-part-4.md) tego samouczka).</span><span class="sxs-lookup"><span data-stu-id="906d2-129">Recall that we stored the base URI for the API in the view bag (see [Part 4](using-web-api-with-entity-framework-part-4.md) of the tutorial).</span></span>

[!code-javascript[Main](using-web-api-with-entity-framework-part-5/samples/sample4.js?highlight=5)]

<span data-ttu-id="906d2-130">Następnie dodaj funkcje do modelu widoku na tworzenie, aktualizowanie i usuwanie produktów.</span><span class="sxs-lookup"><span data-stu-id="906d2-130">Next, add functions to the view-model to create, update, and delete products.</span></span> <span data-ttu-id="906d2-131">Te funkcje przesyłania wywołania AJAX do internetowego interfejsu API i użyj wyników, aby zaktualizować model widoku.</span><span class="sxs-lookup"><span data-stu-id="906d2-131">These functions submit AJAX calls to the web API and use the results to update the view-model.</span></span>

[!code-javascript[Main](using-web-api-with-entity-framework-part-5/samples/sample5.js?highlight=7)]

<span data-ttu-id="906d2-132">Teraz najważniejszy element: gdy DOM jest fulled załadowany, wywołanie **ko.applyBindings** działać, a następnie przekazać nowe wystąpienie klasy `ProductsViewModel`:</span><span class="sxs-lookup"><span data-stu-id="906d2-132">Now the most important part: When the DOM is fulled loaded, call the **ko.applyBindings** function and pass in a new instance of the `ProductsViewModel`:</span></span>

[!code-javascript[Main](using-web-api-with-entity-framework-part-5/samples/sample6.js)]

<span data-ttu-id="906d2-133">**Ko.applyBindings** metoda aktywuje Knockout i tworzącej powiązania modelu widoku do widoku.</span><span class="sxs-lookup"><span data-stu-id="906d2-133">The **ko.applyBindings** method activates Knockout and wires up the view-model to the view.</span></span>

<span data-ttu-id="906d2-134">Teraz, gdy model widoku, możemy utworzyć powiązania.</span><span class="sxs-lookup"><span data-stu-id="906d2-134">Now that we have a view-model, we can create the bindings.</span></span> <span data-ttu-id="906d2-135">W struktura Knockout.js, możesz to zrobić, dodając `data-bind` atrybutów elementów HTML.</span><span class="sxs-lookup"><span data-stu-id="906d2-135">In Knockout.js, you do this by adding `data-bind` attributes to HTML elements.</span></span> <span data-ttu-id="906d2-136">Na przykład, aby powiązać lista HTML do tablicy, należy użyć `foreach` powiązania:</span><span class="sxs-lookup"><span data-stu-id="906d2-136">For example, to bind an HTML list to an array, use the `foreach` binding:</span></span>

[!code-html[Main](using-web-api-with-entity-framework-part-5/samples/sample7.html?highlight=1)]

<span data-ttu-id="906d2-137">`foreach` Powiązanie dokonuje iteracji tablicy i tworzy elementy dla każdego obiektu podrzędnego w tablicy.</span><span class="sxs-lookup"><span data-stu-id="906d2-137">The `foreach` binding iterates through the array and creates child elements for each object in the array.</span></span> <span data-ttu-id="906d2-138">Wiązania na elementy podrzędne mogą odwoływać się do właściwości obiektów w tablicy.</span><span class="sxs-lookup"><span data-stu-id="906d2-138">Bindings on the child elements can refer to properties on the array objects.</span></span>

<span data-ttu-id="906d2-139">Dodaj następujące powiązania do listy "update produkty":</span><span class="sxs-lookup"><span data-stu-id="906d2-139">Add the following bindings to the "update-products" list:</span></span>

[!code-html[Main](using-web-api-with-entity-framework-part-5/samples/sample8.html)]

<span data-ttu-id="906d2-140">`<li>` Element występuje zakresie **foreach** powiązania.</span><span class="sxs-lookup"><span data-stu-id="906d2-140">The `<li>` element occurs within the scope of the **foreach** binding.</span></span> <span data-ttu-id="906d2-141">Czy oznacza, że będą Knockout renderować element raz dla każdego produktu w `products` tablicy.</span><span class="sxs-lookup"><span data-stu-id="906d2-141">That means Knockout will render the element once for each product in the `products` array.</span></span> <span data-ttu-id="906d2-142">Wszystkie powiązania w ramach `<li>` element odwołują się do tego wystąpienia produktu.</span><span class="sxs-lookup"><span data-stu-id="906d2-142">All of the bindings within the `<li>` element refer to that product instance.</span></span> <span data-ttu-id="906d2-143">Na przykład `$data.Name` odwołuje się do `Name` właściwość nad produktem.</span><span class="sxs-lookup"><span data-stu-id="906d2-143">For example, `$data.Name` refers to the `Name` property on the product.</span></span>

<span data-ttu-id="906d2-144">Aby ustawić wartości tekstowe dane wejściowe, użyj `value` powiązania.</span><span class="sxs-lookup"><span data-stu-id="906d2-144">To set the values of the text inputs, use the `value` binding.</span></span> <span data-ttu-id="906d2-145">Przyciski są powiązane z funkcji na widok modelu przy użyciu `click` powiązania.</span><span class="sxs-lookup"><span data-stu-id="906d2-145">The buttons are bound to functions on the model-view, using the `click` binding.</span></span> <span data-ttu-id="906d2-146">Wystąpienie produktu jest przekazywany jako parametr do każdej funkcji.</span><span class="sxs-lookup"><span data-stu-id="906d2-146">The product instance is passed as a parameter to each function.</span></span> <span data-ttu-id="906d2-147">Aby uzyskać więcej informacji [dokumentacji struktura Knockout.js](http://knockoutjs.com/documentation/observables.html) ma dobrą opisy różnych powiązania.</span><span class="sxs-lookup"><span data-stu-id="906d2-147">For more information, the [Knockout.js documentation](http://knockoutjs.com/documentation/observables.html) has good descriptions of the various bindings.</span></span>

<span data-ttu-id="906d2-148">Następnie Dodaj powiązanie dla **przesłać** zdarzeń w formularzu Dodawanie produktu:</span><span class="sxs-lookup"><span data-stu-id="906d2-148">Next, add a binding for the **submit** event on the Add Product form:</span></span>

[!code-html[Main](using-web-api-with-entity-framework-part-5/samples/sample9.html)]

<span data-ttu-id="906d2-149">Wywołuje to powiązanie `create` funkcji na podstawie modelu widoku, aby utworzyć nowy produkt.</span><span class="sxs-lookup"><span data-stu-id="906d2-149">This binding calls the `create` function on the view-model to create a new product.</span></span>

<span data-ttu-id="906d2-150">Oto kompletny kod dla widoku administratora:</span><span class="sxs-lookup"><span data-stu-id="906d2-150">Here is the complete code for the Admin view:</span></span>

[!code-cshtml[Main](using-web-api-with-entity-framework-part-5/samples/sample10.cshtml)]

<span data-ttu-id="906d2-151">Uruchom aplikację, zaloguj się przy użyciu konta administratora, a następnie kliknij link "Admin".</span><span class="sxs-lookup"><span data-stu-id="906d2-151">Run the application, log in with the Administrator account, and click the "Admin" link.</span></span> <span data-ttu-id="906d2-152">Należy zapoznać się z listą produktów i mieć możliwość utworzenia, aktualizacji lub usunięcia produktów.</span><span class="sxs-lookup"><span data-stu-id="906d2-152">You should see the list of products, and be able to create, update, or delete products.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="906d2-153">[Poprzednie](using-web-api-with-entity-framework-part-4.md)
> [dalej](using-web-api-with-entity-framework-part-6.md)</span><span class="sxs-lookup"><span data-stu-id="906d2-153">[Previous](using-web-api-with-entity-framework-part-4.md)
[Next](using-web-api-with-entity-framework-part-6.md)</span></span>
