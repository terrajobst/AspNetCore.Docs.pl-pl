---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-6
title: 'Część 6: Tworzenie produktu i kolejność kontrolerów | Dokumentacja firmy Microsoft'
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/04/2012
ms.topic: article
ms.assetid: 91ee29ee-0689-40ee-914a-e7dd733b6622
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-6
msc.type: authoredcontent
ms.openlocfilehash: 6bd485d29821af12b9ebe31b2d04a2d9ab826731
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
---
<a name="part-6-creating-product-and-order-controllers"></a><span data-ttu-id="ac261-102">Część 6: Tworzenie produktu i kolejność kontrolerów</span><span class="sxs-lookup"><span data-stu-id="ac261-102">Part 6: Creating Product and Order Controllers</span></span>
====================
<span data-ttu-id="ac261-103">przez [Wasson Jan](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="ac261-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="ac261-104">Pobieranie ukończone projektu</span><span class="sxs-lookup"><span data-stu-id="ac261-104">Download Completed Project</span></span>](http://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

## <a name="add-a-products-controller"></a><span data-ttu-id="ac261-105">Dodawanie kontrolera produktów</span><span class="sxs-lookup"><span data-stu-id="ac261-105">Add a Products Controller</span></span>

<span data-ttu-id="ac261-106">Kontroler administratora jest dla użytkowników, którzy mają uprawnienia administratora.</span><span class="sxs-lookup"><span data-stu-id="ac261-106">The Admin controller is for users who have administrator privileges.</span></span> <span data-ttu-id="ac261-107">Klientów, z drugiej strony, można wyświetlić produktów, ale nie można utworzyć, zaktualizować lub usuń je.</span><span class="sxs-lookup"><span data-stu-id="ac261-107">Customers, on the other hand, can view products but cannot create, update, or delete them.</span></span>

<span data-ttu-id="ac261-108">Firma Microsoft łatwo ograniczyć dostęp do metody Post, Put i Delete, pozostawiając Otwórz metody Get.</span><span class="sxs-lookup"><span data-stu-id="ac261-108">We can easily restrict access to the Post, Put, and Delete methods, while leaving the Get methods open.</span></span> <span data-ttu-id="ac261-109">Ale przyjrzeć się dane, które są zwracane do produktu:</span><span class="sxs-lookup"><span data-stu-id="ac261-109">But look at the data that is returned for a product:</span></span>

[!code-json[Main](using-web-api-with-entity-framework-part-6/samples/sample1.json?highlight=1)]

<span data-ttu-id="ac261-110">`ActualCost` Właściwości nie powinny być widoczne dla klientów!</span><span class="sxs-lookup"><span data-stu-id="ac261-110">The `ActualCost` property should not be visible to customers!</span></span> <span data-ttu-id="ac261-111">Rozwiązanie jest określenie *obiektu transferu danych* (DTO), który zawiera podzbiór właściwości, które powinny być widoczne dla klientów.</span><span class="sxs-lookup"><span data-stu-id="ac261-111">The solution is to define a *data transfer object* (DTO) that includes a subset of properties that should be visible to customers.</span></span> <span data-ttu-id="ac261-112">Używamy LINQ do projektu `Product` wystąpień do `ProductDTO` wystąpień.</span><span class="sxs-lookup"><span data-stu-id="ac261-112">We will use LINQ to project `Product` instances to `ProductDTO` instances.</span></span>

<span data-ttu-id="ac261-113">Dodaj klasę o nazwie `ProductDTO` do folderu modeli.</span><span class="sxs-lookup"><span data-stu-id="ac261-113">Add a class named `ProductDTO` to the Models folder.</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample2.cs)]

<span data-ttu-id="ac261-114">Teraz Dodaj kontroler.</span><span class="sxs-lookup"><span data-stu-id="ac261-114">Now add the controller.</span></span> <span data-ttu-id="ac261-115">W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy folder kontrolery.</span><span class="sxs-lookup"><span data-stu-id="ac261-115">In Solution Explorer, right-click the Controllers folder.</span></span> <span data-ttu-id="ac261-116">Wybierz **Dodaj**, a następnie wybierz pozycję **kontrolera**.</span><span class="sxs-lookup"><span data-stu-id="ac261-116">Select **Add**, then select **Controller**.</span></span> <span data-ttu-id="ac261-117">W **Dodaj kontroler** okna dialogowego, nazwy kontrolera &quot;ProductsController&quot;.</span><span class="sxs-lookup"><span data-stu-id="ac261-117">In the **Add Controller** dialog, name the controller &quot;ProductsController&quot;.</span></span> <span data-ttu-id="ac261-118">W obszarze **szablonu**, wybierz pozycję **Kontroler interfejsu API pusty**.</span><span class="sxs-lookup"><span data-stu-id="ac261-118">Under **Template**, select **Empty API controller**.</span></span>

![](using-web-api-with-entity-framework-part-6/_static/image1.png)

<span data-ttu-id="ac261-119">Zastąp wszystkie elementy w pliku źródłowym następujący kod:</span><span class="sxs-lookup"><span data-stu-id="ac261-119">Replace everything in the source file with the following code:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample3.cs)]

<span data-ttu-id="ac261-120">Nadal używa kontrolera `OrdersContext` aby w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="ac261-120">The controller still uses the `OrdersContext` to query the database.</span></span> <span data-ttu-id="ac261-121">Zamiast zwracać, ale `Product` wystąpień bezpośrednio, nazywamy `MapProducts` do projektu, na `ProductDTO` wystąpień:</span><span class="sxs-lookup"><span data-stu-id="ac261-121">But instead of returning `Product` instances directly, we call `MapProducts` to project them onto `ProductDTO` instances:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample4.cs?highlight=1)]

<span data-ttu-id="ac261-122">`MapProducts` Metoda zwraca **IQueryable**, dlatego firma Microsoft tworzą wynik z innych parametrów zapytania.</span><span class="sxs-lookup"><span data-stu-id="ac261-122">The `MapProducts` method returns an **IQueryable**, so we can compose the result with other query parameters.</span></span> <span data-ttu-id="ac261-123">Widać to w `GetProduct` metodę, która dodaje **gdzie** klauzuli zapytania:</span><span class="sxs-lookup"><span data-stu-id="ac261-123">You can see this in the `GetProduct` method, which adds a **where** clause to the query:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample5.cs?highlight=2)]

## <a name="add-an-orders-controller"></a><span data-ttu-id="ac261-124">Dodawanie kontrolera zlecenia</span><span class="sxs-lookup"><span data-stu-id="ac261-124">Add an Orders Controller</span></span>

<span data-ttu-id="ac261-125">Następnie dodaj kontroler, który umożliwia użytkownikom tworzenie i wyświetlanie zleceń.</span><span class="sxs-lookup"><span data-stu-id="ac261-125">Next, add a controller that lets users create and view orders.</span></span>

<span data-ttu-id="ac261-126">Zaczniemy DTO innego.</span><span class="sxs-lookup"><span data-stu-id="ac261-126">We'll start with another DTO.</span></span> <span data-ttu-id="ac261-127">W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy folder modeli, a następnie Dodaj klasę o nazwie `OrderDTO` Użyj następujących implementacji:</span><span class="sxs-lookup"><span data-stu-id="ac261-127">In Solution Explorer, right-click the Models folder and add a class named `OrderDTO` Use the following implementation:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample6.cs)]

<span data-ttu-id="ac261-128">Teraz Dodaj kontroler.</span><span class="sxs-lookup"><span data-stu-id="ac261-128">Now add the controller.</span></span> <span data-ttu-id="ac261-129">W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy folder kontrolery.</span><span class="sxs-lookup"><span data-stu-id="ac261-129">In Solution Explorer, right-click the Controllers folder.</span></span> <span data-ttu-id="ac261-130">Wybierz **Dodaj**, a następnie wybierz pozycję **kontrolera**.</span><span class="sxs-lookup"><span data-stu-id="ac261-130">Select **Add**, then select **Controller**.</span></span> <span data-ttu-id="ac261-131">W **Dodaj kontroler** okna dialogowego, ustaw następujące opcje:</span><span class="sxs-lookup"><span data-stu-id="ac261-131">In the **Add Controller** dialog, set the following options:</span></span>

- <span data-ttu-id="ac261-132">W obszarze **nazwy kontrolera**, wprowadź "OrdersController".</span><span class="sxs-lookup"><span data-stu-id="ac261-132">Under **Controller Name**, enter "OrdersController".</span></span>
- <span data-ttu-id="ac261-133">W obszarze **szablonu**, wybierz pozycję "Kontroler interfejsu API z akcjami odczytu/zapisu, przy użyciu programu Entity Framework".</span><span class="sxs-lookup"><span data-stu-id="ac261-133">Under **Template**, select "API controller with read/write actions, using Entity Framework".</span></span>
- <span data-ttu-id="ac261-134">W obszarze **klasa modelu**, wybierz pozycję &quot;zamówienia (ProductStore.Models)&quot;.</span><span class="sxs-lookup"><span data-stu-id="ac261-134">Under **Model class**, select &quot;Order (ProductStore.Models)&quot;.</span></span>
- <span data-ttu-id="ac261-135">W obszarze **klasa kontekstu danych**, wybierz pozycję &quot;OrdersContext (ProductStore.Models)&quot;.</span><span class="sxs-lookup"><span data-stu-id="ac261-135">Under **Data context class**, select &quot;OrdersContext (ProductStore.Models)&quot;.</span></span>

![](using-web-api-with-entity-framework-part-6/_static/image2.png)

<span data-ttu-id="ac261-136">Kliknij przycisk **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="ac261-136">Click **Add**.</span></span> <span data-ttu-id="ac261-137">Spowoduje to dodanie pliku o nazwie OrdersController.cs.</span><span class="sxs-lookup"><span data-stu-id="ac261-137">This adds a file named OrdersController.cs.</span></span> <span data-ttu-id="ac261-138">Następnie należy zmodyfikować domyślną implementację kontrolera.</span><span class="sxs-lookup"><span data-stu-id="ac261-138">Next, we need to modify the default implementation of the controller.</span></span>

<span data-ttu-id="ac261-139">Należy najpierw usunąć `PutOrder` i `DeleteOrder` metody.</span><span class="sxs-lookup"><span data-stu-id="ac261-139">First, delete the `PutOrder` and `DeleteOrder` methods.</span></span> <span data-ttu-id="ac261-140">Dla tego przykładu klienci nie można zmodyfikować lub usunąć istniejące zlecenia.</span><span class="sxs-lookup"><span data-stu-id="ac261-140">For this sample, customers cannot modify or delete existing orders.</span></span> <span data-ttu-id="ac261-141">W rzeczywistej aplikacji będzie potrzebny partii logiki zaplecza do obsługi tych przypadkach.</span><span class="sxs-lookup"><span data-stu-id="ac261-141">In a real application, you would need lots of back-end logic to handle these cases.</span></span> <span data-ttu-id="ac261-142">(Na przykład kolejność już dostarczono?)</span><span class="sxs-lookup"><span data-stu-id="ac261-142">(For example, was the order already shipped?)</span></span>

<span data-ttu-id="ac261-143">Zmień `GetOrders` metodę, aby zwrócić tylko zleceń, które należą do użytkownika:</span><span class="sxs-lookup"><span data-stu-id="ac261-143">Change the `GetOrders` method to return just the orders that belong to the user:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample7.cs)]

<span data-ttu-id="ac261-144">Zmień `GetOrder` metody w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="ac261-144">Change the `GetOrder` method as follows:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample8.cs)]

<span data-ttu-id="ac261-145">Poniżej przedstawiono zmiany wprowadzone do metody:</span><span class="sxs-lookup"><span data-stu-id="ac261-145">Here are the changes that we made to the method:</span></span>

- <span data-ttu-id="ac261-146">Wartość zwracana jest `OrderDTO` wystąpienia, zamiast `Order`.</span><span class="sxs-lookup"><span data-stu-id="ac261-146">The return value is an `OrderDTO` instance, instead of an `Order`.</span></span>
- <span data-ttu-id="ac261-147">Firma Microsoft kwerendy bazy danych dla zlecenia, używamy [DbQuery.Include](https://msdn.microsoft.com/library/gg696395) metodę, aby pobrać pokrewny `OrderDetail` i `Product` jednostek.</span><span class="sxs-lookup"><span data-stu-id="ac261-147">When we query the database for the order, we use the [DbQuery.Include](https://msdn.microsoft.com/library/gg696395) method to fetch the related `OrderDetail` and `Product` entities.</span></span>
- <span data-ttu-id="ac261-148">Firma Microsoft spłaszczanie wynik przy użyciu projekcji.</span><span class="sxs-lookup"><span data-stu-id="ac261-148">We flatten the result by using a projection.</span></span>

<span data-ttu-id="ac261-149">Odpowiedź HTTP będzie zawierać tablicę produkty z ilości:</span><span class="sxs-lookup"><span data-stu-id="ac261-149">The HTTP response will contain an array of products with quantities:</span></span>

[!code-json[Main](using-web-api-with-entity-framework-part-6/samples/sample9.json)]

<span data-ttu-id="ac261-150">Ten format jest ułatwia klientom korzystać niż oryginalny wykres obiektu zawiera zagnieżdżone jednostki (kolejności, szczegóły i produktów).</span><span class="sxs-lookup"><span data-stu-id="ac261-150">This format is easier for clients to consume than the original object graph, which contains nested entities (order, details, and products).</span></span>

<span data-ttu-id="ac261-151">Metoda ostatniego uważają, że `PostOrder`.</span><span class="sxs-lookup"><span data-stu-id="ac261-151">The last method to consider it `PostOrder`.</span></span> <span data-ttu-id="ac261-152">Teraz, ta metoda przyjmuje `Order` wystąpienia.</span><span class="sxs-lookup"><span data-stu-id="ac261-152">Right now, this method takes an `Order` instance.</span></span> <span data-ttu-id="ac261-153">Jednak rozważyć, co się stanie, jeśli klient wyśle treści żądania następująco:</span><span class="sxs-lookup"><span data-stu-id="ac261-153">But consider what happens if a client sends a request body like this:</span></span>

[!code-json[Main](using-web-api-with-entity-framework-part-6/samples/sample10.json)]

<span data-ttu-id="ac261-154">Jest to dobrze kolejności i Entity Framework będzie happily wstawić je do bazy danych.</span><span class="sxs-lookup"><span data-stu-id="ac261-154">This is a well-structured order, and Entity Framework will happily insert it into the database.</span></span> <span data-ttu-id="ac261-155">Ale zawiera jednostki produktu, która wcześniej nie istnieje.</span><span class="sxs-lookup"><span data-stu-id="ac261-155">But it contains a Product entity that did not exist previously.</span></span> <span data-ttu-id="ac261-156">Klient właśnie utworzony nowy produkt w naszej bazie danych!</span><span class="sxs-lookup"><span data-stu-id="ac261-156">The client just created a new product in our database!</span></span> <span data-ttu-id="ac261-157">Będzie zaskoczeniem do działu fullfilment kolejności, po zgłoszeniu kolejność posiada koala.</span><span class="sxs-lookup"><span data-stu-id="ac261-157">This will be a suprise to the order fullfilment department, when they see an order for koala bears.</span></span> <span data-ttu-id="ac261-158">Wnioski, należy zachować ostrożność naprawdę dane, które akceptują w żądaniu POST i PUT.</span><span class="sxs-lookup"><span data-stu-id="ac261-158">The moral is, be really careful about the data you accept in a POST or PUT request.</span></span>

<span data-ttu-id="ac261-159">Aby uniknąć tego problemu, należy zmienić `PostOrder` metody podjęcie `OrderDTO` wystąpienia.</span><span class="sxs-lookup"><span data-stu-id="ac261-159">To avoid this problem, change the `PostOrder` method to take an `OrderDTO` instance.</span></span> <span data-ttu-id="ac261-160">Użyj `OrderDTO` do utworzenia `Order`.</span><span class="sxs-lookup"><span data-stu-id="ac261-160">Use the `OrderDTO` to create the `Order`.</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample11.cs)]

<span data-ttu-id="ac261-161">Należy zauważyć, że używamy `ProductID` i `Quantity` właściwości i firma Microsoft ignorować wszystkie wartości, które klient wysłany nazwa produktu lub ceny.</span><span class="sxs-lookup"><span data-stu-id="ac261-161">Notice that we use the `ProductID` and `Quantity` properties, and we ignore any values that the client sent for either product name or price.</span></span> <span data-ttu-id="ac261-162">Jeśli identyfikator produktu nie jest prawidłowy, spowodują naruszenie ograniczenia klucza obcego w bazie danych i Wstaw zakończy się niepowodzeniem, jak powinno.</span><span class="sxs-lookup"><span data-stu-id="ac261-162">If the product ID is not valid, it will violate the foreign key constraint in the database, and the insert will fail, as it should.</span></span>

<span data-ttu-id="ac261-163">Poniżej przedstawiono pełną `PostOrder` metody:</span><span class="sxs-lookup"><span data-stu-id="ac261-163">Here is the complete `PostOrder` method:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample12.cs)]

<span data-ttu-id="ac261-164">Na koniec należy dodać **autoryzacji** atrybutu na kontrolerze:</span><span class="sxs-lookup"><span data-stu-id="ac261-164">Finally, add the **Authorize** attribute to the controller:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample13.cs)]

<span data-ttu-id="ac261-165">Teraz można tworzyć tylko zarejestrowani użytkownicy lub wyświetlić zamówienia.</span><span class="sxs-lookup"><span data-stu-id="ac261-165">Now only registered users can create or view orders.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="ac261-166">[Poprzednie](using-web-api-with-entity-framework-part-5.md)
> [dalej](using-web-api-with-entity-framework-part-7.md)</span><span class="sxs-lookup"><span data-stu-id="ac261-166">[Previous](using-web-api-with-entity-framework-part-5.md)
[Next](using-web-api-with-entity-framework-part-7.md)</span></span>
