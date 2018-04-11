---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-7
title: 'Część 7: Tworzenie głównym strony | Dokumentacja firmy Microsoft'
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/04/2012
ms.topic: article
ms.assetid: eb32a17b-626c-4373-9a7d-3387992f3c04
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-7
msc.type: authoredcontent
ms.openlocfilehash: 2c378e68a1e6600daf655c19afbfe355e89496d4
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/10/2018
---
<a name="part-7-creating-the-main-page"></a><span data-ttu-id="71cce-102">Część 7: Tworzenie głównym strony</span><span class="sxs-lookup"><span data-stu-id="71cce-102">Part 7: Creating the Main Page</span></span>
====================
<span data-ttu-id="71cce-103">przez [Wasson Jan](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="71cce-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="71cce-104">Pobieranie ukończone projektu</span><span class="sxs-lookup"><span data-stu-id="71cce-104">Download Completed Project</span></span>](http://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

## <a name="creating-the-main-page"></a><span data-ttu-id="71cce-105">Tworzenie głównego strony</span><span class="sxs-lookup"><span data-stu-id="71cce-105">Creating the Main Page</span></span>

<span data-ttu-id="71cce-106">W tej sekcji utworzysz strony głównej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="71cce-106">In this section, you will create the main application page.</span></span> <span data-ttu-id="71cce-107">Ta strona będzie bardziej skomplikowane niż strony administratora, więc firma Microsoft będzie podejścia w kilku etapach.</span><span class="sxs-lookup"><span data-stu-id="71cce-107">This page will be more complex than the Admin page, so we'll approach it in several steps.</span></span> <span data-ttu-id="71cce-108">Na bieżąco zobaczysz niektórych bardziej zaawansowanych technik Knockout.js.</span><span class="sxs-lookup"><span data-stu-id="71cce-108">Along the way, you'll see some more advanced Knockout.js techniques.</span></span> <span data-ttu-id="71cce-109">Poniżej przedstawiono podstawowe układ strony:</span><span class="sxs-lookup"><span data-stu-id="71cce-109">Here is the basic layout of the page:</span></span>

![](using-web-api-with-entity-framework-part-7/_static/image1.png)

- <span data-ttu-id="71cce-110">"Produkty" zawiera tablicę produktów.</span><span class="sxs-lookup"><span data-stu-id="71cce-110">"Products" holds an array of products.</span></span>
- <span data-ttu-id="71cce-111">"Koszyk" zawiera tablicę produkty z ilości.</span><span class="sxs-lookup"><span data-stu-id="71cce-111">"Cart" holds an array of products with quantities.</span></span> <span data-ttu-id="71cce-112">Klikając przycisk "Dodaj do koszyka" aktualizuje koszyka.</span><span class="sxs-lookup"><span data-stu-id="71cce-112">Clicking "Add to Cart" updates the cart.</span></span>
- <span data-ttu-id="71cce-113">"Zamówienia" zawiera tablicę kolejności identyfikatorów.</span><span class="sxs-lookup"><span data-stu-id="71cce-113">"Orders" holds an array of order IDs.</span></span>
- <span data-ttu-id="71cce-114">"Szczegóły" posiada szczegółami zamówienia, czyli tablicę elementów (produkty z ilości)</span><span class="sxs-lookup"><span data-stu-id="71cce-114">"Details" holds an order detail, which is an array of items (products with quantities)</span></span>

<span data-ttu-id="71cce-115">Zaczniemy, definiując niektóre podstawowe układu w języku HTML, bez powiązania danych lub skryptu.</span><span class="sxs-lookup"><span data-stu-id="71cce-115">We'll start by defining some basic layout in HTML, with no data binding or script.</span></span> <span data-ttu-id="71cce-116">Otwórz plik Views/Home/Index.cshtml i zastąpienie całej zawartości z następujących czynności:</span><span class="sxs-lookup"><span data-stu-id="71cce-116">Open the file Views/Home/Index.cshtml and replace all of the contents with the following:</span></span>

[!code-html[Main](using-web-api-with-entity-framework-part-7/samples/sample1.html)]

<span data-ttu-id="71cce-117">Następnie dodaj sekcję skryptów i utworzyć pusty model widoku:</span><span class="sxs-lookup"><span data-stu-id="71cce-117">Next, add a Scripts section and create an empty view-model:</span></span>

[!code-cshtml[Main](using-web-api-with-entity-framework-part-7/samples/sample2.cshtml)]

<span data-ttu-id="71cce-118">Na podstawie projektu rysowane wcześniej, nasz model widoku musi dostrzegalne elementy dla produktów, koszyka zleceń i szczegóły.</span><span class="sxs-lookup"><span data-stu-id="71cce-118">Based on the design sketched earlier, our view model needs observables for products, cart, orders, and details.</span></span> <span data-ttu-id="71cce-119">Dodaj następujące zmienne, które mają `AppViewModel` obiektu:</span><span class="sxs-lookup"><span data-stu-id="71cce-119">Add the following variables to the `AppViewModel` object:</span></span>

[!code-javascript[Main](using-web-api-with-entity-framework-part-7/samples/sample3.js)]

<span data-ttu-id="71cce-120">Użytkownicy mogą dodać elementy z listy produktów do koszyka i usuwanie elementów z koszyka.</span><span class="sxs-lookup"><span data-stu-id="71cce-120">Users can add items from the products list into the cart, and remove items from the cart.</span></span> <span data-ttu-id="71cce-121">Aby hermetyzować tych funkcji, utworzymy innej klasy modelu widoku, która reprezentuje produkt.</span><span class="sxs-lookup"><span data-stu-id="71cce-121">To encapsulate these functions, we'll create another view-model class that represents a product.</span></span> <span data-ttu-id="71cce-122">Dodaj następujący kod do `AppViewModel`:</span><span class="sxs-lookup"><span data-stu-id="71cce-122">Add the following code to `AppViewModel`:</span></span>

[!code-javascript[Main](using-web-api-with-entity-framework-part-7/samples/sample4.js?highlight=4)]

<span data-ttu-id="71cce-123">`ProductViewModel` Klasa zawiera dwie funkcje, które są używane do przenoszenia produktu do i z koszyka: `addItemToCart` dodaje jedną jednostkę produktu do koszyka, i `removeAllFromCart` spowoduje usunięcie wszystkich ilości produktu.</span><span class="sxs-lookup"><span data-stu-id="71cce-123">The `ProductViewModel` class contains two functions that are used to move the product to and from the cart: `addItemToCart` adds one unit of the product to the cart, and `removeAllFromCart` removes all quantities of the product.</span></span>

<span data-ttu-id="71cce-124">Użytkownicy mogą wybrać istniejące i pobrać szczegółów zamówienia.</span><span class="sxs-lookup"><span data-stu-id="71cce-124">Users can select an existing order and get the order details.</span></span> <span data-ttu-id="71cce-125">Umieściliśmy tej funkcji do innego modelu widoku:</span><span class="sxs-lookup"><span data-stu-id="71cce-125">We'll encapsulate this functionality into another view-model:</span></span>

[!code-javascript[Main](using-web-api-with-entity-framework-part-7/samples/sample5.js?highlight=4)]

<span data-ttu-id="71cce-126">`OrderDetailsViewModel` Jest inicjowany z zlecenia i jego pobiera szczegółów zamówienia, wysyłając żądanie AJAX do serwera.</span><span class="sxs-lookup"><span data-stu-id="71cce-126">The `OrderDetailsViewModel` is initialized with an order, and it fetches the order details by sending an AJAX request to the server.</span></span>

<span data-ttu-id="71cce-127">Zauważ również, `total` właściwość `OrderDetailsViewModel`.</span><span class="sxs-lookup"><span data-stu-id="71cce-127">Also, notice the `total` property on the `OrderDetailsViewModel`.</span></span> <span data-ttu-id="71cce-128">Ta właściwość jest specjalnym rodzajem według o nazwie [obliczana według](http://knockoutjs.com/documentation/computedObservables.html).</span><span class="sxs-lookup"><span data-stu-id="71cce-128">This property is a special kind of observable called a [computed observable](http://knockoutjs.com/documentation/computedObservables.html).</span></span> <span data-ttu-id="71cce-129">Jak wskazuje nazwę, obliczoną według umożliwia tworzenia powiązań danych obliczoną wartość&#8212;w tym przypadku łączny koszt zlecenia.</span><span class="sxs-lookup"><span data-stu-id="71cce-129">As the name implies, a computed observable lets you data bind to a computed value&#8212;in this case, the total cost of the order.</span></span>

<span data-ttu-id="71cce-130">Następnie dodaj tych funkcji `AppViewModel`:</span><span class="sxs-lookup"><span data-stu-id="71cce-130">Next, add these functions to `AppViewModel`:</span></span>

- <span data-ttu-id="71cce-131">`resetCart` Usuwa wszystkie elementy z koszyka.</span><span class="sxs-lookup"><span data-stu-id="71cce-131">`resetCart` removes all items from the cart.</span></span>
- <span data-ttu-id="71cce-132">`getDetails` pobiera szczegóły dla zamówienia (przez pusing nowy `OrderDetailsViewModel` na `details` listy).</span><span class="sxs-lookup"><span data-stu-id="71cce-132">`getDetails` gets the details for an order (by pusing a new `OrderDetailsViewModel` onto the `details` list).</span></span>
- <span data-ttu-id="71cce-133">`createOrder` Tworzy nową kolejność i opróżnia koszyka.</span><span class="sxs-lookup"><span data-stu-id="71cce-133">`createOrder` creates a new order and empties the cart.</span></span>


[!code-javascript[Main](using-web-api-with-entity-framework-part-7/samples/sample6.js?highlight=4)]

<span data-ttu-id="71cce-134">Na koniec zainicjować modelu widoku dokonując żądania AJAX dla produktów i zleceń:</span><span class="sxs-lookup"><span data-stu-id="71cce-134">Finally, initialize the view model by making AJAX requests for the products and orders:</span></span>

[!code-javascript[Main](using-web-api-with-entity-framework-part-7/samples/sample7.js)]

<span data-ttu-id="71cce-135">OK dużo kodu, ale jego budujemy się krok po kroku, więc miejmy nadzieję, że projekt jest wyczyszczone.</span><span class="sxs-lookup"><span data-stu-id="71cce-135">OK, that's a lot of code, but we built it up step-by-step, so hopefully the design is clear.</span></span> <span data-ttu-id="71cce-136">Teraz możemy dodać niektóre wiązania Knockout.js do kodu HTML.</span><span class="sxs-lookup"><span data-stu-id="71cce-136">Now we can add some Knockout.js bindings to the HTML.</span></span>

<span data-ttu-id="71cce-137">**Produkty**</span><span class="sxs-lookup"><span data-stu-id="71cce-137">**Products**</span></span>

<span data-ttu-id="71cce-138">Poniżej przedstawiono powiązania dla listy produktów:</span><span class="sxs-lookup"><span data-stu-id="71cce-138">Here are the bindings for the product list:</span></span>

[!code-html[Main](using-web-api-with-entity-framework-part-7/samples/sample8.html)]

<span data-ttu-id="71cce-139">Wykonuje iterację na tablicy produktów i wyświetla nazwę i cenę.</span><span class="sxs-lookup"><span data-stu-id="71cce-139">This iterates over the products array and displays the name and price.</span></span> <span data-ttu-id="71cce-140">Przycisk "Dodaj do kolejność" jest widoczny tylko wtedy, gdy użytkownik jest zalogowany.</span><span class="sxs-lookup"><span data-stu-id="71cce-140">The "Add to Order" button is visible only when the user is logged in.</span></span>

<span data-ttu-id="71cce-141">Wywołania przycisk "Dodaj do kolejność" `addItemToCart` na `ProductViewModel` wystąpienia dla produktu.</span><span class="sxs-lookup"><span data-stu-id="71cce-141">The "Add to Order" button calls `addItemToCart` on the `ProductViewModel` instance for the product.</span></span> <span data-ttu-id="71cce-142">Oznacza to dobry funkcja Knockout.js: gdy model widoku zawiera inne modele widoku, można zastosować powiązania wewnętrzny modelu.</span><span class="sxs-lookup"><span data-stu-id="71cce-142">This demonstrates a nice feature of Knockout.js: When a view-model contains other view-models, you can apply the bindings to the inner model.</span></span> <span data-ttu-id="71cce-143">W tym przykładzie powiązania w ramach `foreach` są stosowane do każdego z `ProductViewModel` wystąpień.</span><span class="sxs-lookup"><span data-stu-id="71cce-143">In this example, the bindings within the `foreach` are applied to each of the `ProductViewModel` instances.</span></span> <span data-ttu-id="71cce-144">Ta metoda jest znacznie czyszczący niż wprowadzanie wszystkich funkcji do pojedynczego model widoku.</span><span class="sxs-lookup"><span data-stu-id="71cce-144">This approach is much cleaner than putting all of the functionality into a single view-model.</span></span>

<span data-ttu-id="71cce-145">**Koszyka**</span><span class="sxs-lookup"><span data-stu-id="71cce-145">**Cart**</span></span>

<span data-ttu-id="71cce-146">Poniżej przedstawiono powiązania z koszyka:</span><span class="sxs-lookup"><span data-stu-id="71cce-146">Here are the bindings for the cart:</span></span>

[!code-html[Main](using-web-api-with-entity-framework-part-7/samples/sample9.html)]

<span data-ttu-id="71cce-147">Wykonuje iterację na tablicy koszyka i wyświetla nazwę, ceny i ilości.</span><span class="sxs-lookup"><span data-stu-id="71cce-147">This iterates over the cart array and displays the name, price, and quantity.</span></span> <span data-ttu-id="71cce-148">Należy zwrócić uwagę, czy przycisk "Tworzenie zamówienia" i "Usuń" łącze jest powiązana z funkcji model widoku.</span><span class="sxs-lookup"><span data-stu-id="71cce-148">Note that the "Remove" link and the "Create Order" button are bound to view-model functions.</span></span>

<span data-ttu-id="71cce-149">**Zlecenia**</span><span class="sxs-lookup"><span data-stu-id="71cce-149">**Orders**</span></span>

<span data-ttu-id="71cce-150">Poniżej przedstawiono powiązania listę zleceń:</span><span class="sxs-lookup"><span data-stu-id="71cce-150">Here are the bindings for the orders list:</span></span>

[!code-html[Main](using-web-api-with-entity-framework-part-7/samples/sample10.html)]

<span data-ttu-id="71cce-151">To przechodzi przez zamówienia i zawiera identyfikator zamówienia.</span><span class="sxs-lookup"><span data-stu-id="71cce-151">This iterates over the orders and shows the order ID.</span></span> <span data-ttu-id="71cce-152">Zdarzenie click łącze jest powiązany z `getDetails` funkcji.</span><span class="sxs-lookup"><span data-stu-id="71cce-152">The click event on the link is bound to the `getDetails` function.</span></span>

<span data-ttu-id="71cce-153">**Szczegóły zlecenia**</span><span class="sxs-lookup"><span data-stu-id="71cce-153">**Order Details**</span></span>

<span data-ttu-id="71cce-154">Poniżej przedstawiono powiązania szczegółów zamówienia:</span><span class="sxs-lookup"><span data-stu-id="71cce-154">Here are the bindings for the order details:</span></span>

[!code-html[Main](using-web-api-with-entity-framework-part-7/samples/sample11.html)]

<span data-ttu-id="71cce-155">Wykonuje iterację na elementy w kolejności i wyświetla produktu, ceny i ilość.</span><span class="sxs-lookup"><span data-stu-id="71cce-155">This iterates over the items in the order and displays the product, price, and quanity.</span></span> <span data-ttu-id="71cce-156">Otaczające div jest widoczny tylko wtedy, gdy tablica szczegóły zawiera jeden lub więcej elementów.</span><span class="sxs-lookup"><span data-stu-id="71cce-156">The surrounding div is visible only if the details array contains one or more items.</span></span>

## <a name="conclusion"></a><span data-ttu-id="71cce-157">Wniosek</span><span class="sxs-lookup"><span data-stu-id="71cce-157">Conclusion</span></span>

<span data-ttu-id="71cce-158">W tym samouczku utworzono aplikację, która korzysta z programu Entity Framework do komunikacji z bazy danych i interfejsu API sieci Web programu ASP.NET zapewnia interfejs publicznych na podstawie warstwy danych.</span><span class="sxs-lookup"><span data-stu-id="71cce-158">In this tutorial, you created an application that uses Entity Framework to communicate with the database, and ASP.NET Web API to provide a public-facing interface on top of the data layer.</span></span> <span data-ttu-id="71cce-159">Używamy ASP.NET MVC 4 do renderowania stron HTML i Knockout.js i jQuery aby zapewnić dynamiczne interakcje bez przeładowania strony.</span><span class="sxs-lookup"><span data-stu-id="71cce-159">We use ASP.NET MVC 4 to render the HTML pages, and Knockout.js plus jQuery to provide dynamic interactions without page reloads.</span></span>

<span data-ttu-id="71cce-160">Dodatkowe zasoby:</span><span class="sxs-lookup"><span data-stu-id="71cce-160">Additional resources:</span></span>

- [<span data-ttu-id="71cce-161">Mapa zawartości dostępu do danych w programie ASP.NET</span><span class="sxs-lookup"><span data-stu-id="71cce-161">ASP.NET Data Access Content Map</span></span>](https://msdn.microsoft.com/library/6759sth4.aspx)
- [<span data-ttu-id="71cce-162">Centrum deweloperów Entity Framework</span><span class="sxs-lookup"><span data-stu-id="71cce-162">Entity Framework Developer Center</span></span>](https://msdn.microsoft.com/data/ef)

> [!div class="step-by-step"]
> [<span data-ttu-id="71cce-163">Poprzednie</span><span class="sxs-lookup"><span data-stu-id="71cce-163">Previous</span></span>](using-web-api-with-entity-framework-part-6.md)
