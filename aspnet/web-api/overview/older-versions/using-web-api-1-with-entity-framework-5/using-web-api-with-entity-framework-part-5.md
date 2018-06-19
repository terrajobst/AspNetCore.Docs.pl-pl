---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-5
title: 'Część 5: Tworzenie dynamicznego interfejsu użytkownika z Knockout.js | Dokumentacja firmy Microsoft'
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/04/2012
ms.topic: article
ms.assetid: 9d9cb3b0-f4a7-434e-a508-9fc0ad0eb813
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-5
msc.type: authoredcontent
ms.openlocfilehash: b63446d076fbb1143641dead788042967b996bf8
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
ms.locfileid: "30873813"
---
<a name="part-5-creating-a-dynamic-ui-with-knockoutjs"></a><span data-ttu-id="3e1e9-102">Część 5: Tworzenie dynamicznego interfejsu użytkownika z Knockout.js</span><span class="sxs-lookup"><span data-stu-id="3e1e9-102">Part 5: Creating a Dynamic UI with Knockout.js</span></span>
====================
<span data-ttu-id="3e1e9-103">przez [Wasson Jan](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="3e1e9-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="3e1e9-104">Pobieranie ukończone projektu</span><span class="sxs-lookup"><span data-stu-id="3e1e9-104">Download Completed Project</span></span>](http://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

## <a name="creating-a-dynamic-ui-with-knockoutjs"></a><span data-ttu-id="3e1e9-105">Tworzenie dynamicznego interfejsu użytkownika z Knockout.js</span><span class="sxs-lookup"><span data-stu-id="3e1e9-105">Creating a Dynamic UI with Knockout.js</span></span>

<span data-ttu-id="3e1e9-106">W tej sekcji użyjemy Knockout.js Dodawanie funkcji do widoku administracyjnej.</span><span class="sxs-lookup"><span data-stu-id="3e1e9-106">In this section, we'll use Knockout.js to add functionality to the Admin view.</span></span>

<span data-ttu-id="3e1e9-107">[Knockout.js](http://knockoutjs.com/) biblioteka języka Javascript, która ułatwia powiązanie kontrolek HTML z danymi.</span><span class="sxs-lookup"><span data-stu-id="3e1e9-107">[Knockout.js](http://knockoutjs.com/) is a Javascript library that makes it easy to bind HTML controls to data.</span></span> <span data-ttu-id="3e1e9-108">Knockout.js korzysta ze wzorca Model-View-ViewModel (MVVM).</span><span class="sxs-lookup"><span data-stu-id="3e1e9-108">Knockout.js uses the Model-View-ViewModel (MVVM) pattern.</span></span>

- <span data-ttu-id="3e1e9-109">*Modelu* jest po stronie serwera reprezentację danych w domenie business (w przypadku, produktów i zamówienia).</span><span class="sxs-lookup"><span data-stu-id="3e1e9-109">The *model* is the server-side representation of the data in the business domain (in our case, products and orders).</span></span>
- <span data-ttu-id="3e1e9-110">*Widoku* to warstwa prezentacji (HTML).</span><span class="sxs-lookup"><span data-stu-id="3e1e9-110">The *view* is the presentation layer (HTML).</span></span>
- <span data-ttu-id="3e1e9-111">*Model widoku* jest obiektem Javascript, która przechowuje dane modelu.</span><span class="sxs-lookup"><span data-stu-id="3e1e9-111">The *view-model* is a Javascript object that holds the model data.</span></span> <span data-ttu-id="3e1e9-112">Model widoku jest abstrakcji kodu interfejsu użytkownika.</span><span class="sxs-lookup"><span data-stu-id="3e1e9-112">The view-model is a code abstraction of the UI.</span></span> <span data-ttu-id="3e1e9-113">Go nie ma informacji o reprezentacji w formacie HTML.</span><span class="sxs-lookup"><span data-stu-id="3e1e9-113">It has no knowledge of the HTML representation.</span></span> <span data-ttu-id="3e1e9-114">Zamiast tego reprezentuje funkcje abstrakcyjne widoku, takie jak "listę elementów".</span><span class="sxs-lookup"><span data-stu-id="3e1e9-114">Instead, it represents abstract features of the view, such as "a list of items".</span></span>

<span data-ttu-id="3e1e9-115">Widok jest powiązany z danymi model widoku.</span><span class="sxs-lookup"><span data-stu-id="3e1e9-115">The view is data-bound to the view-model.</span></span> <span data-ttu-id="3e1e9-116">Aktualizacje na model widoku są automatycznie odzwierciedlane w widoku.</span><span class="sxs-lookup"><span data-stu-id="3e1e9-116">Updates to the view-model are automatically reflected in the view.</span></span> <span data-ttu-id="3e1e9-117">Model widoku również pobiera zdarzenia w widoku, takie jak kliknięcia przycisków i wykonuje operacje na model, takich jak tworzenie zamówienia.</span><span class="sxs-lookup"><span data-stu-id="3e1e9-117">The view-model also gets events from the view, such as button clicks, and performs operations on the model, such as creating an order.</span></span>

![](using-web-api-with-entity-framework-part-5/_static/image1.png)

<span data-ttu-id="3e1e9-118">Najpierw zdefiniujemy model widoku.</span><span class="sxs-lookup"><span data-stu-id="3e1e9-118">First we'll define the view-model.</span></span> <span data-ttu-id="3e1e9-119">Po tym firma Microsoft będzie powiązać kod znaczników HTML na model widoku.</span><span class="sxs-lookup"><span data-stu-id="3e1e9-119">After that, we will bind the HTML markup to the view-model.</span></span>

<span data-ttu-id="3e1e9-120">Dodaj poniższą sekcję Razor do Admin.cshtml:</span><span class="sxs-lookup"><span data-stu-id="3e1e9-120">Add the following Razor section to Admin.cshtml:</span></span>

[!code-cshtml[Main](using-web-api-with-entity-framework-part-5/samples/sample1.cshtml)]

<span data-ttu-id="3e1e9-121">W tej sekcji można dodać dowolne miejsce w pliku.</span><span class="sxs-lookup"><span data-stu-id="3e1e9-121">You can add this section anywhere in the file.</span></span> <span data-ttu-id="3e1e9-122">Podczas renderowania widoku sekcji pojawi się w dolnej części strony HTML, kliknij prawym przyciskiem myszy przed tagiem zamykającym &lt;/body&gt; tagu.</span><span class="sxs-lookup"><span data-stu-id="3e1e9-122">When the view is rendered, the section appears at the bottom of the HTML page, right before the closing &lt;/body&gt; tag.</span></span>

<span data-ttu-id="3e1e9-123">Wszystkie skryptu ta strona zostanie umieszczona wewnątrz tagu skryptu wskazywanym przez komentarz:</span><span class="sxs-lookup"><span data-stu-id="3e1e9-123">All of the script for this page will go inside the script tag indicated by the comment:</span></span>

[!code-html[Main](using-web-api-with-entity-framework-part-5/samples/sample2.html)]

<span data-ttu-id="3e1e9-124">Najpierw należy zdefiniować klasę modelu widoku:</span><span class="sxs-lookup"><span data-stu-id="3e1e9-124">First, define a view-model class:</span></span>

[!code-javascript[Main](using-web-api-with-entity-framework-part-5/samples/sample3.js)]

<span data-ttu-id="3e1e9-125">**ko.observableArray** jest specjalnym rodzajem obiektu w odcinania o nazwie *według*.</span><span class="sxs-lookup"><span data-stu-id="3e1e9-125">**ko.observableArray** is a special kind of object in Knockout, called an *observable*.</span></span> <span data-ttu-id="3e1e9-126">Z [dokumentacji Knockout.js](http://knockoutjs.com/documentation/observables.html): zauważalny jest "JavaScript obiekt, który może powiadomić subskrybentów o zmianach."</span><span class="sxs-lookup"><span data-stu-id="3e1e9-126">From the [Knockout.js documentation](http://knockoutjs.com/documentation/observables.html): An observable is a "JavaScript object that can notify subscribers about changes."</span></span> <span data-ttu-id="3e1e9-127">Po zmianie zawartości zauważalny, widok jest automatycznie aktualizowany do dopasowania.</span><span class="sxs-lookup"><span data-stu-id="3e1e9-127">When the contents of an observable change, the view is automatically updated to match.</span></span>

<span data-ttu-id="3e1e9-128">Aby wypełnić `products` tablicy, przesyłania żądania AJAX do interfejsu API sieci web.</span><span class="sxs-lookup"><span data-stu-id="3e1e9-128">To populate the `products` array, make an AJAX request to the web API.</span></span> <span data-ttu-id="3e1e9-129">Odwołaj możemy przechowywane podstawowy identyfikator URI dla interfejsu API w zbiorze widoku (zobacz [część 4](using-web-api-with-entity-framework-part-4.md) samouczka).</span><span class="sxs-lookup"><span data-stu-id="3e1e9-129">Recall that we stored the base URI for the API in the view bag (see [Part 4](using-web-api-with-entity-framework-part-4.md) of the tutorial).</span></span>

[!code-javascript[Main](using-web-api-with-entity-framework-part-5/samples/sample4.js?highlight=5)]

<span data-ttu-id="3e1e9-130">Następnie dodaj funkcje do modelu widoku do tworzenia, aktualizacji i usuwania produktów.</span><span class="sxs-lookup"><span data-stu-id="3e1e9-130">Next, add functions to the view-model to create, update, and delete products.</span></span> <span data-ttu-id="3e1e9-131">Te funkcje przesyłania wywołania AJAX do interfejsu API sieci web i użyć wyników, aby zaktualizować model widoku.</span><span class="sxs-lookup"><span data-stu-id="3e1e9-131">These functions submit AJAX calls to the web API and use the results to update the view-model.</span></span>

[!code-javascript[Main](using-web-api-with-entity-framework-part-5/samples/sample5.js?highlight=7)]

<span data-ttu-id="3e1e9-132">Teraz najważniejszy element: gdy DOM jest fulled załadowany, wywołanie **ko.applyBindings** funkcji i podaj nowe wystąpienie klasy `ProductsViewModel`:</span><span class="sxs-lookup"><span data-stu-id="3e1e9-132">Now the most important part: When the DOM is fulled loaded, call the **ko.applyBindings** function and pass in a new instance of the `ProductsViewModel`:</span></span>

[!code-javascript[Main](using-web-api-with-entity-framework-part-5/samples/sample6.js)]

<span data-ttu-id="3e1e9-133">**Ko.applyBindings** metody aktywuje Knockout i tworzącej powiązania modelu widoku do widoku.</span><span class="sxs-lookup"><span data-stu-id="3e1e9-133">The **ko.applyBindings** method activates Knockout and wires up the view-model to the view.</span></span>

<span data-ttu-id="3e1e9-134">Teraz, gdy mamy model widoku, można utworzyć powiązania.</span><span class="sxs-lookup"><span data-stu-id="3e1e9-134">Now that we have a view-model, we can create the bindings.</span></span> <span data-ttu-id="3e1e9-135">W Knockout.js, możesz to zrobić przez dodanie `data-bind` atrybutów elementów HTML.</span><span class="sxs-lookup"><span data-stu-id="3e1e9-135">In Knockout.js, you do this by adding `data-bind` attributes to HTML elements.</span></span> <span data-ttu-id="3e1e9-136">Na przykład, aby powiązać listy HTML do tablicy, należy użyć `foreach` powiązania:</span><span class="sxs-lookup"><span data-stu-id="3e1e9-136">For example, to bind an HTML list to an array, use the `foreach` binding:</span></span>

[!code-html[Main](using-web-api-with-entity-framework-part-5/samples/sample7.html?highlight=1)]

<span data-ttu-id="3e1e9-137">`foreach` Powiązanie iteruje tablicy i tworzy podrzędne elementy dla każdego obiektu w tablicy.</span><span class="sxs-lookup"><span data-stu-id="3e1e9-137">The `foreach` binding iterates through the array and creates child elements for each object in the array.</span></span> <span data-ttu-id="3e1e9-138">Wiązania na elementy podrzędne mogą odwoływać się do właściwości tablicy obiektów.</span><span class="sxs-lookup"><span data-stu-id="3e1e9-138">Bindings on the child elements can refer to properties on the array objects.</span></span>

<span data-ttu-id="3e1e9-139">Dodaj następujące powiązania do listy "aktualizacji produktów":</span><span class="sxs-lookup"><span data-stu-id="3e1e9-139">Add the following bindings to the "update-products" list:</span></span>

[!code-html[Main](using-web-api-with-entity-framework-part-5/samples/sample8.html)]

<span data-ttu-id="3e1e9-140">`<li>` Element występuje w zakresie **foreach** powiązania.</span><span class="sxs-lookup"><span data-stu-id="3e1e9-140">The `<li>` element occurs within the scope of the **foreach** binding.</span></span> <span data-ttu-id="3e1e9-141">Czy oznacza odcinania będzie renderować element raz dla każdego produktu w `products` tablicy.</span><span class="sxs-lookup"><span data-stu-id="3e1e9-141">That means Knockout will render the element once for each product in the `products` array.</span></span> <span data-ttu-id="3e1e9-142">Wszystkie powiązania w ramach `<li>` element się odwoływać do tego wystąpienia produktu.</span><span class="sxs-lookup"><span data-stu-id="3e1e9-142">All of the bindings within the `<li>` element refer to that product instance.</span></span> <span data-ttu-id="3e1e9-143">Na przykład `$data.Name` odwołuje się do `Name` właściwości dla produktu.</span><span class="sxs-lookup"><span data-stu-id="3e1e9-143">For example, `$data.Name` refers to the `Name` property on the product.</span></span>

<span data-ttu-id="3e1e9-144">Aby ustawić wartości danych wejściowych tekstu, należy użyć `value` powiązania.</span><span class="sxs-lookup"><span data-stu-id="3e1e9-144">To set the values of the text inputs, use the `value` binding.</span></span> <span data-ttu-id="3e1e9-145">Przyciski są powiązane z funkcji w widoku modelu przy użyciu `click` powiązania.</span><span class="sxs-lookup"><span data-stu-id="3e1e9-145">The buttons are bound to functions on the model-view, using the `click` binding.</span></span> <span data-ttu-id="3e1e9-146">Wystąpienie produktu są przekazywane jako parametr do każdej funkcji.</span><span class="sxs-lookup"><span data-stu-id="3e1e9-146">The product instance is passed as a parameter to each function.</span></span> <span data-ttu-id="3e1e9-147">Aby uzyskać więcej informacji [dokumentacji Knockout.js](http://knockoutjs.com/documentation/observables.html) ma dobrej opisy różnych powiązań.</span><span class="sxs-lookup"><span data-stu-id="3e1e9-147">For more information, the [Knockout.js documentation](http://knockoutjs.com/documentation/observables.html) has good descriptions of the various bindings.</span></span>

<span data-ttu-id="3e1e9-148">Następnie Dodaj powiązanie dla **przesłać** zdarzenia w formularzu Dodawanie produktu:</span><span class="sxs-lookup"><span data-stu-id="3e1e9-148">Next, add a binding for the **submit** event on the Add Product form:</span></span>

[!code-html[Main](using-web-api-with-entity-framework-part-5/samples/sample9.html)]

<span data-ttu-id="3e1e9-149">Wymaga to powiązanie `create` funkcja na model widoku do tworzenia nowych produktów.</span><span class="sxs-lookup"><span data-stu-id="3e1e9-149">This binding calls the `create` function on the view-model to create a new product.</span></span>

<span data-ttu-id="3e1e9-150">W tym miejscu jest kompletny kod dla widoku administracyjnej:</span><span class="sxs-lookup"><span data-stu-id="3e1e9-150">Here is the complete code for the Admin view:</span></span>

[!code-cshtml[Main](using-web-api-with-entity-framework-part-5/samples/sample10.cshtml)]

<span data-ttu-id="3e1e9-151">Uruchom aplikację, zaloguj się przy użyciu konta administratora, a następnie kliknij łącze "Admin".</span><span class="sxs-lookup"><span data-stu-id="3e1e9-151">Run the application, log in with the Administrator account, and click the "Admin" link.</span></span> <span data-ttu-id="3e1e9-152">Należy zapoznać się z listą produktów i można tworzyć, aktualizować lub usuwać produktów.</span><span class="sxs-lookup"><span data-stu-id="3e1e9-152">You should see the list of products, and be able to create, update, or delete products.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="3e1e9-153">[Poprzednie](using-web-api-with-entity-framework-part-4.md)
> [dalej](using-web-api-with-entity-framework-part-6.md)</span><span class="sxs-lookup"><span data-stu-id="3e1e9-153">[Previous](using-web-api-with-entity-framework-part-4.md)
[Next](using-web-api-with-entity-framework-part-6.md)</span></span>
