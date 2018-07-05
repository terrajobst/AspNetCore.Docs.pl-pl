---
uid: web-api/overview/odata-support-in-aspnet-web-api/using-select-expand-and-value
title: Za pomocą $select, $expand and $value w programie ASP.NET Web API 2 OData | Dokumentacja firmy Microsoft
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/11/2013
ms.topic: article
ms.assetid: 43279a80-a96c-4564-b6ea-ad992a2d6828
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/using-select-expand-and-value
msc.type: authoredcontent
ms.openlocfilehash: f7e70bd76668f2af9215d57ee1cc9e7d41948c67
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37384767"
---
<a name="using-select-expand-and-value-in-aspnet-web-api-2-odata"></a><span data-ttu-id="e80d0-102">Za pomocą $select, $expand and $value w programie ASP.NET Web API 2 OData</span><span class="sxs-lookup"><span data-stu-id="e80d0-102">Using $select, $expand, and $value in ASP.NET Web API 2 OData</span></span>
====================
<span data-ttu-id="e80d0-103">przez [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="e80d0-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="e80d0-104">Składnik Web API 2 dodaje obsługę dla $expand, $select i opcje $value w protokole OData.</span><span class="sxs-lookup"><span data-stu-id="e80d0-104">Web API 2 adds support for the $expand, $select, and $value options in OData.</span></span> <span data-ttu-id="e80d0-105">Te opcje umożliwiają klientowi do kontrolowania reprezentacji, która go ponownie z serwera.</span><span class="sxs-lookup"><span data-stu-id="e80d0-105">These options allow a client to control the representation that it gets back from the server.</span></span>

- <span data-ttu-id="e80d0-106">**$expand** powoduje, że z powiązanymi obiektami być wbudowane w odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="e80d0-106">**$expand** causes related entities to be included inline in the response.</span></span>
- <span data-ttu-id="e80d0-107">**$select** wybierze podzbiór właściwości do uwzględnienia w odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="e80d0-107">**$select** selects a subset of properties to include in the response.</span></span>
- <span data-ttu-id="e80d0-108">**$value** pobiera nieprzetworzonej wartości właściwości.</span><span class="sxs-lookup"><span data-stu-id="e80d0-108">**$value** gets the raw value of a property.</span></span>

## <a name="example-schema"></a><span data-ttu-id="e80d0-109">Przykład schematu</span><span class="sxs-lookup"><span data-stu-id="e80d0-109">Example Schema</span></span>

<span data-ttu-id="e80d0-110">W tym artykule, będziesz używać usługi OData, który definiuje trzy jednostki: produktów, dostawcy i kategorii.</span><span class="sxs-lookup"><span data-stu-id="e80d0-110">For this article, I'll use an OData service that defines three entities: Product, Supplier, and Category.</span></span> <span data-ttu-id="e80d0-111">Każdy produkt ma jedną kategorię i jednego dostawcy.</span><span class="sxs-lookup"><span data-stu-id="e80d0-111">Each product has one category and one supplier.</span></span>

![](using-select-expand-and-value/_static/image1.png)

<span data-ttu-id="e80d0-112">Poniżej przedstawiono klasy C#, które definiują modeli jednostki:</span><span class="sxs-lookup"><span data-stu-id="e80d0-112">Here are the C# classes that define the entity models:</span></span>

[!code-csharp[Main](using-select-expand-and-value/samples/sample1.cs)]

<span data-ttu-id="e80d0-113">Należy zauważyć, że `Product` klasy definiuje właściwości nawigacji dla `Supplier` i `Category`.</span><span class="sxs-lookup"><span data-stu-id="e80d0-113">Notice that the `Product` class defines navigation properties for the `Supplier` and `Category`.</span></span> <span data-ttu-id="e80d0-114">`Category` Klasa definiuje właściwość nawigacji dla produktów w każdej kategorii.</span><span class="sxs-lookup"><span data-stu-id="e80d0-114">The `Category` class defines a navigation property for the products in each category.</span></span>

<span data-ttu-id="e80d0-115">Aby utworzyć punkt końcowy OData dla tego schematu, użyj szkieletu programu Visual Studio 2013, zgodnie z opisem w [Tworzenie punktu końcowego OData w interfejsie API sieci Web platformy ASP.NET](odata-v3/creating-an-odata-endpoint.md).</span><span class="sxs-lookup"><span data-stu-id="e80d0-115">To create an OData endpoint for this schema, use the Visual Studio 2013 scaffolding, as described in [Creating an OData Endpoint in ASP.NET Web API](odata-v3/creating-an-odata-endpoint.md).</span></span> <span data-ttu-id="e80d0-116">Dodaj kontrolery oddzielny produkt, kategoria i dostawcy.</span><span class="sxs-lookup"><span data-stu-id="e80d0-116">Add separate controllers for Product, Category, and Supplier.</span></span>

## <a name="enabling-expand-and-select"></a><span data-ttu-id="e80d0-117">Włączanie $Rozwiń i $select</span><span class="sxs-lookup"><span data-stu-id="e80d0-117">Enabling $expand and $select</span></span>

<span data-ttu-id="e80d0-118">W programie Visual Studio 2013 rusztowania Web API OData tworzy kontroler, automatycznie obsługuje $expand i $select.</span><span class="sxs-lookup"><span data-stu-id="e80d0-118">In Visual Studio 2013, the Web API OData scaffolding creates a controller that automatically supports $expand and $select.</span></span> <span data-ttu-id="e80d0-119">Odwołanie, poniżej przedstawiono wymagania dotyczące obsługi $Rozwiń i $select w kontrolerze.</span><span class="sxs-lookup"><span data-stu-id="e80d0-119">For reference, here are the requirements to support $expand and $select in a controller.</span></span>

<span data-ttu-id="e80d0-120">Dla kolekcji, kontroler firmy `Get` metoda musi zwracać **IQueryable**.</span><span class="sxs-lookup"><span data-stu-id="e80d0-120">For collections, the controller's `Get` method must return an **IQueryable**.</span></span>

[!code-csharp[Main](using-select-expand-and-value/samples/sample2.cs)]

<span data-ttu-id="e80d0-121">W przypadku pojedynczych jednostek zwracają **SingleResult&lt;T&gt;**, gdzie T jest **IQueryable** zawierający zero lub jedną jednostkę.</span><span class="sxs-lookup"><span data-stu-id="e80d0-121">For single entities, return a **SingleResult&lt;T&gt;**, where T is an **IQueryable** that contains zero or one entities.</span></span>

[!code-csharp[Main](using-select-expand-and-value/samples/sample3.cs)]

<span data-ttu-id="e80d0-122">Ponadto dekoracji swoje `Get` metod z **[Queryable]** atrybutu, jak pokazano w poprzednich fragmentach kodu.</span><span class="sxs-lookup"><span data-stu-id="e80d0-122">Also, decorate your `Get` methods with the **[Queryable]** attribute, as shown in the previous code snippets.</span></span> <span data-ttu-id="e80d0-123">Alternatywnie wywołać **EnableQuerySupport** na **HttpConfiguration** obiektu podczas uruchamiania.</span><span class="sxs-lookup"><span data-stu-id="e80d0-123">Alternatively, call **EnableQuerySupport** on the **HttpConfiguration** object at startup.</span></span> <span data-ttu-id="e80d0-124">(Aby uzyskać więcej informacji, zobacz [włączenie opcji zapytania OData](supporting-odata-query-options.md#enable).)</span><span class="sxs-lookup"><span data-stu-id="e80d0-124">(For more information, see [Enabling OData Query Options](supporting-odata-query-options.md#enable).)</span></span>

## <a name="using-expand"></a><span data-ttu-id="e80d0-125">Rozwiń przy użyciu ciągu $</span><span class="sxs-lookup"><span data-stu-id="e80d0-125">Using $expand</span></span>

<span data-ttu-id="e80d0-126">Kiedy wykonujesz zapytanie OData jednostki lub kolekcji, domyślną odpowiedź nie zawiera powiązanych jednostek.</span><span class="sxs-lookup"><span data-stu-id="e80d0-126">When you query an OData entity or collection, the default response does not include related entities.</span></span> <span data-ttu-id="e80d0-127">Na przykład poniżej przedstawiono odpowiedź domyślną dla zestawu jednostek kategorie:</span><span class="sxs-lookup"><span data-stu-id="e80d0-127">For example, here is the default response for the Categories entity set:</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample4.cmd)]

<span data-ttu-id="e80d0-128">Jak widać, odpowiedź nie obejmuje wszystkie produkty, nawet jeśli jednostka kategoria zawiera łącze nawigacyjne produktów.</span><span class="sxs-lookup"><span data-stu-id="e80d0-128">As you can see, the response does not include any products, even though the Category entity has a Products navigation link.</span></span> <span data-ttu-id="e80d0-129">Jednak klient może używać $Rozwiń, aby uzyskać listę produktów dla każdej kategorii.</span><span class="sxs-lookup"><span data-stu-id="e80d0-129">However, the client can use $expand to get the list of products for each category.</span></span> <span data-ttu-id="e80d0-130">$Expand opcja znajduje się w ciągu zapytania żądania:</span><span class="sxs-lookup"><span data-stu-id="e80d0-130">The $expand option goes in the query string of the request:</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample5.cmd)]

<span data-ttu-id="e80d0-131">Teraz serwer będzie zawierać produktów dla każdej kategorii, wbudowany z kategorii.</span><span class="sxs-lookup"><span data-stu-id="e80d0-131">Now the server will include the products for each category, inline with the categories.</span></span> <span data-ttu-id="e80d0-132">Oto ładunek odpowiedzi:</span><span class="sxs-lookup"><span data-stu-id="e80d0-132">Here is the response payload:</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample6.cmd)]

<span data-ttu-id="e80d0-133">Należy zauważyć, że każdy wpis w tablicy "wartość" zawiera listę produktów.</span><span class="sxs-lookup"><span data-stu-id="e80d0-133">Notice that each entry in the "value" array contains a Products list.</span></span>

<span data-ttu-id="e80d0-134">$Rozwiń opcję przyjmuje rozdzielonych przecinkami lista właściwości nawigacyjne do rozszerzenia.</span><span class="sxs-lookup"><span data-stu-id="e80d0-134">The $expand option takes a comma-separated list of navigation properties to expand.</span></span> <span data-ttu-id="e80d0-135">Następujące żądanie rozwija kategorii i dostawcy dla produktu.</span><span class="sxs-lookup"><span data-stu-id="e80d0-135">The following request expands both the category and the supplier for a product.</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample7.cmd)]

<span data-ttu-id="e80d0-136">Oto treści odpowiedzi:</span><span class="sxs-lookup"><span data-stu-id="e80d0-136">Here is the response body:</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample8.cmd)]

<span data-ttu-id="e80d0-137">Możesz rozwinąć więcej niż jednego poziomu właściwości nawigacji.</span><span class="sxs-lookup"><span data-stu-id="e80d0-137">You can expand more than one level of navigation property.</span></span> <span data-ttu-id="e80d0-138">Poniższy przykład obejmuje wszystkie produkty dla kategorii, a także dostawcy dla każdego produktu.</span><span class="sxs-lookup"><span data-stu-id="e80d0-138">The following example includes all the products for a category and also the supplier for each product.</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample9.cmd)]

<span data-ttu-id="e80d0-139">Oto treści odpowiedzi:</span><span class="sxs-lookup"><span data-stu-id="e80d0-139">Here is the response body:</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample10.cmd)]

<span data-ttu-id="e80d0-140">Domyślnie interfejs API sieci Web ogranicza głębokość rozszerzenia maksymalną na 2.</span><span class="sxs-lookup"><span data-stu-id="e80d0-140">By default, Web API limits the maximum expansion depth to 2.</span></span> <span data-ttu-id="e80d0-141">Która zapobiega wysyłaniu złożonych żądań, takich jak przez klienta `$expand=Orders/OrderDetails/Product/Supplier/Region`, który może być nieefektywne w celu wykonywania zapytań i tworzenia dużych odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="e80d0-141">That prevents the client from sending complex requests like `$expand=Orders/OrderDetails/Product/Supplier/Region`, which might be inefficient to query and create large responses.</span></span> <span data-ttu-id="e80d0-142">Aby zastąpić domyślne, należy ustawić **MaxExpansionDepth** właściwość **[Queryable]** atrybutu.</span><span class="sxs-lookup"><span data-stu-id="e80d0-142">To override the default, set the **MaxExpansionDepth** property on the **[Queryable]** attribute.</span></span>

[!code-csharp[Main](using-select-expand-and-value/samples/sample11.cs)]

<span data-ttu-id="e80d0-143">Aby uzyskać więcej informacji na temat $ rozwiń węzeł opcji, zobacz [rozwiń System stosowanie opcji zapytania ($expand)](http://www.odata.org/documentation/odata-v2-documentation/uri-conventions/#46_Expand_System_Query_Option_expand) w oficjalnej dokumentacji OData.</span><span class="sxs-lookup"><span data-stu-id="e80d0-143">For more information about the $expand option, see [Expand System Query Option ($expand)](http://www.odata.org/documentation/odata-v2-documentation/uri-conventions/#46_Expand_System_Query_Option_expand) in the official OData documentation.</span></span>

## <a name="using-select"></a><span data-ttu-id="e80d0-144">Za pomocą $select</span><span class="sxs-lookup"><span data-stu-id="e80d0-144">Using $select</span></span>

<span data-ttu-id="e80d0-145">Opcja $select określa podzbiór właściwości do uwzględnienia w treści odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="e80d0-145">The $select option specifies a subset of properties to include in the response body.</span></span> <span data-ttu-id="e80d0-146">Na przykład można pobrać tylko nazwa i cena każdego produktu, należy użyć następującej kwerendy:</span><span class="sxs-lookup"><span data-stu-id="e80d0-146">For example, to get only the name and price of each product, use the following query:</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample12.cmd)]

<span data-ttu-id="e80d0-147">Oto treści odpowiedzi:</span><span class="sxs-lookup"><span data-stu-id="e80d0-147">Here is the response body:</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample13.cmd)]

<span data-ttu-id="e80d0-148">Można połączyć $select i $expand w jednym zapytaniu.</span><span class="sxs-lookup"><span data-stu-id="e80d0-148">You can combine $select and $expand in the same query.</span></span> <span data-ttu-id="e80d0-149">Upewnij się, że będą zawierać właściwości rozszerzonej w opcji $select.</span><span class="sxs-lookup"><span data-stu-id="e80d0-149">Make sure to include the expanded property in the $select option.</span></span> <span data-ttu-id="e80d0-150">Na przykład następujące żądanie pobiera nazwę produktu i dostawcy.</span><span class="sxs-lookup"><span data-stu-id="e80d0-150">For example, the following request gets the product name and supplier.</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample14.cmd)]

<span data-ttu-id="e80d0-151">Oto treści odpowiedzi:</span><span class="sxs-lookup"><span data-stu-id="e80d0-151">Here is the response body:</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample15.cmd)]

<span data-ttu-id="e80d0-152">Możesz również wybrać właściwości w ramach właściwości rozszerzonej.</span><span class="sxs-lookup"><span data-stu-id="e80d0-152">You can also select the properties within an expanded property.</span></span> <span data-ttu-id="e80d0-153">Następujące żądanie rozwija produktów i wybiera nazwę kategorii, a także nazwę produktu.</span><span class="sxs-lookup"><span data-stu-id="e80d0-153">The following request expands Products and selects category name plus product name.</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample16.cmd)]

<span data-ttu-id="e80d0-154">Oto treści odpowiedzi:</span><span class="sxs-lookup"><span data-stu-id="e80d0-154">Here is the response body:</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample17.cmd)]

<span data-ttu-id="e80d0-155">Aby uzyskać więcej informacji na temat opcji $select, zobacz [wybierz opcję zapytania systemu ($select)](http://www.odata.org/documentation/odata-v2-documentation/uri-conventions/#48_Select_System_Query_Option_select) w oficjalnej dokumentacji OData.</span><span class="sxs-lookup"><span data-stu-id="e80d0-155">For more information about the $select option, see [Select System Query Option ($select)](http://www.odata.org/documentation/odata-v2-documentation/uri-conventions/#48_Select_System_Query_Option_select) in the official OData documentation.</span></span>

## <a name="getting-individual-properties-of-an-entity-value"></a><span data-ttu-id="e80d0-156">Pobieranie właściwości poszczególne jednostki ($value)</span><span class="sxs-lookup"><span data-stu-id="e80d0-156">Getting Individual Properties of an Entity ($value)</span></span>

<span data-ttu-id="e80d0-157">Istnieją dwa sposoby dla klienta OData można pobrać wybranej właściwości z jednostką.</span><span class="sxs-lookup"><span data-stu-id="e80d0-157">There are two ways for an OData client to get an individual property from an entity.</span></span> <span data-ttu-id="e80d0-158">Klienta można uzyskać wartość w formacie OData lub uzyskaj nieprzetworzonej wartości właściwości.</span><span class="sxs-lookup"><span data-stu-id="e80d0-158">The client can either get the value in OData format, or get the raw value of the property.</span></span>

<span data-ttu-id="e80d0-159">Następujące żądanie pobiera właściwości w formacie OData.</span><span class="sxs-lookup"><span data-stu-id="e80d0-159">The following request gets a property in OData format.</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample18.cmd)]

<span data-ttu-id="e80d0-160">Poniżej przedstawiono przykładową odpowiedź w formacie JSON:</span><span class="sxs-lookup"><span data-stu-id="e80d0-160">Here is an example response in JSON format:</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample19.cmd)]

<span data-ttu-id="e80d0-161">Aby uzyskać nieprzetworzonej wartości właściwości, należy dołączyć $value do identyfikatora URI:</span><span class="sxs-lookup"><span data-stu-id="e80d0-161">To get the raw value of the property, append $value to the URI:</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample20.cmd)]

<span data-ttu-id="e80d0-162">Poniżej przedstawiono odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="e80d0-162">Here is the response.</span></span> <span data-ttu-id="e80d0-163">Należy zauważyć, że typ zawartości "text/plain" nie jest kodem JSON.</span><span class="sxs-lookup"><span data-stu-id="e80d0-163">Notice that the content type is "text/plain", not JSON.</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample21.cmd)]

<span data-ttu-id="e80d0-164">Aby obsługiwać te zapytania w kontrolerze OData, Dodaj metodę o nazwie `GetProperty`, gdzie `Property` jest nazwą właściwości.</span><span class="sxs-lookup"><span data-stu-id="e80d0-164">To support these queries in your OData controller, add a method named `GetProperty`, where `Property` is the name of the property.</span></span> <span data-ttu-id="e80d0-165">Na przykład, czy nazwę metody pobierania właściwości Name `GetName`.</span><span class="sxs-lookup"><span data-stu-id="e80d0-165">For example, the method to get the Name property would be named `GetName`.</span></span> <span data-ttu-id="e80d0-166">Metoda powinna zwrócić wartość tej właściwości:</span><span class="sxs-lookup"><span data-stu-id="e80d0-166">The method should return the value of that property:</span></span>

[!code-csharp[Main](using-select-expand-and-value/samples/sample22.cs)]
