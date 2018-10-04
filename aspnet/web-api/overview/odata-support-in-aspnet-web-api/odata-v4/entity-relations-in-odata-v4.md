---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/entity-relations-in-odata-v4
title: Relacje jednostek w protokole OData v4 przy użyciu wzorca ASP.NET Web API 2.2 | Dokumentacja firmy Microsoft
author: MikeWasson
description: 'Większość zestawów danych zdefiniować relacje między jednostkami: klienci mają zamówienia; książki mają autorzy; Jeśli te produkty mają dostawców. Przy użyciu protokołu OData, klienci mogą przejść za pośrednictwem...'
ms.author: riande
ms.date: 06/26/2014
ms.assetid: 72657550-ec09-4779-9bfc-2fb15ecd51c7
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/entity-relations-in-odata-v4
msc.type: authoredcontent
ms.openlocfilehash: d07ddab83462ee1bc84ba8ab15fe906937f506e6
ms.sourcegitcommit: 7890dfb5a8f8c07d813f166d3ab0c263f893d0c6
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/04/2018
ms.locfileid: "48795395"
---
<a name="entity-relations-in-odata-v4-using-aspnet-web-api-22"></a><span data-ttu-id="51dd7-104">Relacje jednostek w protokole OData v4 przy użyciu wzorca ASP.NET Web API 2.2</span><span class="sxs-lookup"><span data-stu-id="51dd7-104">Entity Relations in OData v4 Using ASP.NET Web API 2.2</span></span>
====================
<span data-ttu-id="51dd7-105">przez [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="51dd7-105">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

> <span data-ttu-id="51dd7-106">Większość zestawów danych zdefiniować relacje między jednostkami: klienci mają zamówienia; książki mają autorzy; Jeśli te produkty mają dostawców.</span><span class="sxs-lookup"><span data-stu-id="51dd7-106">Most data sets define relations between entities: Customers have orders; books have authors; products have suppliers.</span></span> <span data-ttu-id="51dd7-107">Używanie protokołu OData, klienci mogą przejść za pośrednictwem relacji jednostek.</span><span class="sxs-lookup"><span data-stu-id="51dd7-107">Using OData, clients can navigate over entity relations.</span></span> <span data-ttu-id="51dd7-108">Biorąc pod uwagę produktu, można znaleźć dostawcy.</span><span class="sxs-lookup"><span data-stu-id="51dd7-108">Given a product, you can find the supplier.</span></span> <span data-ttu-id="51dd7-109">Można również tworzyć i usuwać relacje.</span><span class="sxs-lookup"><span data-stu-id="51dd7-109">You can also create or remove relationships.</span></span> <span data-ttu-id="51dd7-110">Na przykład można ustawić dostawcę dla produktu.</span><span class="sxs-lookup"><span data-stu-id="51dd7-110">For example, you can set the supplier for a product.</span></span>
>
> <span data-ttu-id="51dd7-111">W tym samouczku pokazano, jak obsługują te operacje w protokole OData v4 przy użyciu interfejsu API sieci Web platformy ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="51dd7-111">This tutorial shows how to support these operations in OData v4 using ASP.NET Web API.</span></span> <span data-ttu-id="51dd7-112">Samouczek opiera się na samouczka [tworzenia protokołu OData v4 punktu końcowego Używanie wzorca ASP.NET Web API 2](create-an-odata-v4-endpoint.md).</span><span class="sxs-lookup"><span data-stu-id="51dd7-112">The tutorial builds on the tutorial [Create an OData v4 Endpoint Using ASP.NET Web API 2](create-an-odata-v4-endpoint.md).</span></span>
>
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="51dd7-113">Wersje oprogramowania używanego w tym samouczku</span><span class="sxs-lookup"><span data-stu-id="51dd7-113">Software versions used in the tutorial</span></span>
>
> - <span data-ttu-id="51dd7-114">Składnik Web API 2.1</span><span class="sxs-lookup"><span data-stu-id="51dd7-114">Web API 2.1</span></span>
> - <span data-ttu-id="51dd7-115">Protokołu OData v4</span><span class="sxs-lookup"><span data-stu-id="51dd7-115">OData v4</span></span>
> - <span data-ttu-id="51dd7-116">Visual Studio 2013 (Pobierz program Visual Studio 2017 [tutaj](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017))</span><span class="sxs-lookup"><span data-stu-id="51dd7-116">Visual Studio 2013 (download Visual Studio 2017 [here](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017))</span></span>
> - <span data-ttu-id="51dd7-117">Entity Framework 6</span><span class="sxs-lookup"><span data-stu-id="51dd7-117">Entity Framework 6</span></span>
> - <span data-ttu-id="51dd7-118">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="51dd7-118">.NET 4.5</span></span>
>
> ## <a name="tutorial-versions"></a><span data-ttu-id="51dd7-119">Samouczek wersji</span><span class="sxs-lookup"><span data-stu-id="51dd7-119">Tutorial versions</span></span>
>
> <span data-ttu-id="51dd7-120">Dla protokołu OData w wersji 3, zobacz [Obsługa relacji jednostek w protokole OData v3](https://asp.net/web-api/overview/odata-support-in-aspnet-web-api/odata-v3/working-with-entity-relations).</span><span class="sxs-lookup"><span data-stu-id="51dd7-120">For the OData Version 3, see [Supporting Entity Relations in OData v3](https://asp.net/web-api/overview/odata-support-in-aspnet-web-api/odata-v3/working-with-entity-relations).</span></span>

## <a name="add-a-supplier-entity"></a><span data-ttu-id="51dd7-121">Dodaj jednostkę dostawcy</span><span class="sxs-lookup"><span data-stu-id="51dd7-121">Add a Supplier Entity</span></span>

> [!NOTE]
> <span data-ttu-id="51dd7-122">Samouczek opiera się na samouczka [tworzenia protokołu OData v4 punktu końcowego Używanie wzorca ASP.NET Web API 2](create-an-odata-v4-endpoint.md).</span><span class="sxs-lookup"><span data-stu-id="51dd7-122">The tutorial builds on the tutorial [Create an OData v4 Endpoint Using ASP.NET Web API 2](create-an-odata-v4-endpoint.md).</span></span>

<span data-ttu-id="51dd7-123">Po pierwsze potrzebujemy powiązanej jednostki.</span><span class="sxs-lookup"><span data-stu-id="51dd7-123">First, we need a related entity.</span></span> <span data-ttu-id="51dd7-124">Dodaj klasę o nazwie `Supplier` w folderze modeli.</span><span class="sxs-lookup"><span data-stu-id="51dd7-124">Add a class named `Supplier` in the Models folder.</span></span>

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample1.cs)]

<span data-ttu-id="51dd7-125">Właściwość nawigacji, aby dodać `Product` klasy:</span><span class="sxs-lookup"><span data-stu-id="51dd7-125">Add a navigation property to the `Product` class:</span></span>

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample2.cs?highlight=13-15)]

<span data-ttu-id="51dd7-126">Dodaj nową **DbSet** do `ProductsContext` klasy, tak aby Entity Framework będzie zawierać tabeli dostawcy w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="51dd7-126">Add a new **DbSet** to the `ProductsContext` class, so that Entity Framework will include the Supplier table in the database.</span></span>

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample3.cs?highlight=10)]

<span data-ttu-id="51dd7-127">W WebApiConfig.cs, Dodaj &quot;dostawców&quot; zestawu jednostek do modelu danych jednostki:</span><span class="sxs-lookup"><span data-stu-id="51dd7-127">In WebApiConfig.cs, add a &quot;Suppliers&quot; entity set to the entity data model:</span></span>

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample4.cs?highlight=6)]

## <a name="add-a-suppliers-controller"></a><span data-ttu-id="51dd7-128">Dodawanie kontrolera dostawców</span><span class="sxs-lookup"><span data-stu-id="51dd7-128">Add a Suppliers Controller</span></span>

<span data-ttu-id="51dd7-129">Dodaj `SuppliersController` klasy do folderu kontrolerów.</span><span class="sxs-lookup"><span data-stu-id="51dd7-129">Add a `SuppliersController` class to the Controllers folder.</span></span>

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample5.cs)]

<span data-ttu-id="51dd7-130">Czy mogę nie pokazują, jak dodać operacje CRUD dla tego kontrolera.</span><span class="sxs-lookup"><span data-stu-id="51dd7-130">I won't show how to add CRUD operations for this controller.</span></span> <span data-ttu-id="51dd7-131">Kroki są takie same jak dla kontrolera produktów (zobacz [utworzenie punktu końcowego OData v4](create-an-odata-v4-endpoint.md)).</span><span class="sxs-lookup"><span data-stu-id="51dd7-131">The steps are the same as for the Products controller (see [Create an OData v4 Endpoint](create-an-odata-v4-endpoint.md)).</span></span>

## <a name="getting-related-entities"></a><span data-ttu-id="51dd7-132">Pobieranie powiązanych jednostek</span><span class="sxs-lookup"><span data-stu-id="51dd7-132">Getting Related Entities</span></span>

<span data-ttu-id="51dd7-133">Aby uzyskać dostawcy, produktu, klient wysyła żądanie pobrania:</span><span class="sxs-lookup"><span data-stu-id="51dd7-133">To get the supplier for a product, the client sends a GET request:</span></span>

[!code-console[Main](entity-relations-in-odata-v4/samples/sample6.cmd)]

<span data-ttu-id="51dd7-134">Aby zapewnić obsługę tego żądania, dodaj następującą metodę do `ProductsController` klasy:</span><span class="sxs-lookup"><span data-stu-id="51dd7-134">To support this request, add the following method to the `ProductsController` class:</span></span>

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample7.cs)]

<span data-ttu-id="51dd7-135">Ta metoda używa domyślnej konwencji nazewnictwa</span><span class="sxs-lookup"><span data-stu-id="51dd7-135">This method uses a default naming convention</span></span>

- <span data-ttu-id="51dd7-136">Nazwa metody: metody GetX, gdzie X jest właściwość nawigacji.</span><span class="sxs-lookup"><span data-stu-id="51dd7-136">Method name: GetX, where X is the navigation property.</span></span>
- <span data-ttu-id="51dd7-137">Nazwa parametru: *klucza*</span><span class="sxs-lookup"><span data-stu-id="51dd7-137">Parameter name: *key*</span></span>

<span data-ttu-id="51dd7-138">Jeśli wykonujesz tę konwencję nazewnictwa, interfejs API sieci Web automatycznie mapuje żądania HTTP do metody kontrolera.</span><span class="sxs-lookup"><span data-stu-id="51dd7-138">If you follow this naming convention, Web API automatically maps the HTTP request to the controller method.</span></span>

<span data-ttu-id="51dd7-139">Przykład HTTP żądania:</span><span class="sxs-lookup"><span data-stu-id="51dd7-139">Example HTTP request:</span></span>

[!code-console[Main](entity-relations-in-odata-v4/samples/sample8.cmd)]

<span data-ttu-id="51dd7-140">Odpowiedź na przykład HTTP:</span><span class="sxs-lookup"><span data-stu-id="51dd7-140">Example HTTP response:</span></span>

[!code-console[Main](entity-relations-in-odata-v4/samples/sample9.cmd)]

### <a name="getting-a-related-collection"></a><span data-ttu-id="51dd7-141">Pobieranie kolekcji powiązanych</span><span class="sxs-lookup"><span data-stu-id="51dd7-141">Getting a related collection</span></span>

<span data-ttu-id="51dd7-142">W poprzednim przykładzie produkt ma jeden dostawca.</span><span class="sxs-lookup"><span data-stu-id="51dd7-142">In the previous example, a product has one supplier.</span></span> <span data-ttu-id="51dd7-143">Właściwość nawigacji może również zwracać kolekcję.</span><span class="sxs-lookup"><span data-stu-id="51dd7-143">A navigation property can also return a collection.</span></span> <span data-ttu-id="51dd7-144">Poniższy kod umożliwia pobranie produktów dla dostawcy:</span><span class="sxs-lookup"><span data-stu-id="51dd7-144">The following code gets the products for a supplier:</span></span>

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample10.cs)]

<span data-ttu-id="51dd7-145">W tym przypadku metoda ta zwraca **IQueryable** zamiast **SingleResult&lt;T&gt;**</span><span class="sxs-lookup"><span data-stu-id="51dd7-145">In this case, the method returns an **IQueryable** instead of a **SingleResult&lt;T&gt;**</span></span>

<span data-ttu-id="51dd7-146">Przykład HTTP żądania:</span><span class="sxs-lookup"><span data-stu-id="51dd7-146">Example HTTP request:</span></span>

[!code-console[Main](entity-relations-in-odata-v4/samples/sample11.cmd)]

<span data-ttu-id="51dd7-147">Odpowiedź na przykład HTTP:</span><span class="sxs-lookup"><span data-stu-id="51dd7-147">Example HTTP response:</span></span>

[!code-console[Main](entity-relations-in-odata-v4/samples/sample12.cmd)]

## <a name="creating-a-relationship-between-entities"></a><span data-ttu-id="51dd7-148">Tworzenie relacji między jednostkami</span><span class="sxs-lookup"><span data-stu-id="51dd7-148">Creating a Relationship Between Entities</span></span>

<span data-ttu-id="51dd7-149">OData obsługuje tworzenie lub usuwanie relacji między dwiema jednostkami istniejących.</span><span class="sxs-lookup"><span data-stu-id="51dd7-149">OData supports creating or removing relationships between two existing entities.</span></span> <span data-ttu-id="51dd7-150">W terminologii OData v4 relacja jest &quot;odwołania&quot;.</span><span class="sxs-lookup"><span data-stu-id="51dd7-150">In OData v4 terminology, the relationship is a &quot;reference&quot;.</span></span> <span data-ttu-id="51dd7-151">(W protokole OData v3 relacja została wywołana *łącze*.</span><span class="sxs-lookup"><span data-stu-id="51dd7-151">(In OData v3, the relationship was called a *link*.</span></span> <span data-ttu-id="51dd7-152">Różnice protokołu nie ma znaczenia, w tym samouczku.)</span><span class="sxs-lookup"><span data-stu-id="51dd7-152">The protocol differences don't matter for this tutorial.)</span></span>

<span data-ttu-id="51dd7-153">Odwołanie ma swój własny identyfikator URI, za pomocą formularza `/Entity/NavigationProperty/$ref`.</span><span class="sxs-lookup"><span data-stu-id="51dd7-153">A reference has its own URI, with the form `/Entity/NavigationProperty/$ref`.</span></span> <span data-ttu-id="51dd7-154">Na przykład Oto identyfikatora URI, aby rozwiązać odwołania między produktem i jego dostawcy:</span><span class="sxs-lookup"><span data-stu-id="51dd7-154">For example, here is the URI to address the reference between a product and its supplier:</span></span>

[!code-console[Main](entity-relations-in-odata-v4/samples/sample13.cmd)]

<span data-ttu-id="51dd7-155">Aby dodać relację, klient wysyła żądanie POST lub PUT z tym adresem.</span><span class="sxs-lookup"><span data-stu-id="51dd7-155">To add a relationship, the client sends a POST or PUT request to this address.</span></span>

- <span data-ttu-id="51dd7-156">Umieść, jeśli właściwość nawigacji jest pojedynczą jednostką, takich jak `Product.Supplier`.</span><span class="sxs-lookup"><span data-stu-id="51dd7-156">PUT if the navigation property is a single entity, such as `Product.Supplier`.</span></span>
- <span data-ttu-id="51dd7-157">Jeśli właściwość nawigacji jest kolekcją, takich jak `Supplier.Products`.</span><span class="sxs-lookup"><span data-stu-id="51dd7-157">POST if the navigation property is a collection, such as `Supplier.Products`.</span></span>

<span data-ttu-id="51dd7-158">Treść żądania zawiera identyfikator URI inną jednostkę w relacji.</span><span class="sxs-lookup"><span data-stu-id="51dd7-158">The body of the request contains the URI of the other entity in the relation.</span></span> <span data-ttu-id="51dd7-159">Oto przykładowe żądanie:</span><span class="sxs-lookup"><span data-stu-id="51dd7-159">Here is an example request:</span></span>

[!code-console[Main](entity-relations-in-odata-v4/samples/sample14.cmd)]

<span data-ttu-id="51dd7-160">W tym przykładzie klient wysyła żądanie PUT `/Products(6)/Supplier/$ref`, czyli identyfikator URI $ref `Supplier` produktu o identyfikatorze = 6.</span><span class="sxs-lookup"><span data-stu-id="51dd7-160">In this example, the client sends a PUT request to `/Products(6)/Supplier/$ref`, which is the $ref URI for the `Supplier` of the product with ID = 6.</span></span> <span data-ttu-id="51dd7-161">Jeśli żądanie zakończy się powodzeniem, serwer wysyła odpowiedź 204 (Brak zawartości):</span><span class="sxs-lookup"><span data-stu-id="51dd7-161">If the request succeeds, the server sends a 204 (No Content) response:</span></span>

[!code-console[Main](entity-relations-in-odata-v4/samples/sample15.cmd)]

<span data-ttu-id="51dd7-162">Poniżej przedstawiono metody kontrolera, aby dodać relację do `Product`:</span><span class="sxs-lookup"><span data-stu-id="51dd7-162">Here is the controller method to add a relationship to a `Product`:</span></span>

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample16.cs)]

<span data-ttu-id="51dd7-163">*Element navigationProperty* parametr określa relacji, które można ustawić.</span><span class="sxs-lookup"><span data-stu-id="51dd7-163">The *navigationProperty* parameter specifies which relationship to set.</span></span> <span data-ttu-id="51dd7-164">(Jeśli istnieje więcej niż jednej właściwości nawigacji jednostki, możesz dodać więcej `case` instrukcji.)</span><span class="sxs-lookup"><span data-stu-id="51dd7-164">(If there is more than one navigation property on the entity, you can add more `case` statements.)</span></span>

<span data-ttu-id="51dd7-165">*Łącze* parametr zawiera identyfikator URI dostawcy.</span><span class="sxs-lookup"><span data-stu-id="51dd7-165">The *link* parameter contains the URI of the supplier.</span></span> <span data-ttu-id="51dd7-166">Interfejs API sieci Web automatycznie analizuje treść żądania, aby uzyskać wartość tego parametru.</span><span class="sxs-lookup"><span data-stu-id="51dd7-166">Web API automatically parses the request body to get the value for this parameter.</span></span>

<span data-ttu-id="51dd7-167">Aby wyszukać dostawcy, potrzebujemy Identyfikatora (lub klucza), który jest częścią *łącze* parametru.</span><span class="sxs-lookup"><span data-stu-id="51dd7-167">To look up the supplier, we need the ID (or key), which is part of the *link* parameter.</span></span> <span data-ttu-id="51dd7-168">Aby to zrobić, użyj następującej metody pomocnika:</span><span class="sxs-lookup"><span data-stu-id="51dd7-168">To do this, use the following helper method:</span></span>

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample17.cs)]

<span data-ttu-id="51dd7-169">Zasadniczo ta metoda używa biblioteki OData Podziel składnik path identyfikatora URI na segmenty, Znajdź segment, który zawiera klucz i przekonwertować klucz do poprawnego typu.</span><span class="sxs-lookup"><span data-stu-id="51dd7-169">Basically, this method uses the OData library to split the URI path into segments, find the segment that contains the key, and convert the key into the correct type.</span></span>

## <a name="deleting-a-relationship-between-entities"></a><span data-ttu-id="51dd7-170">Usuwanie relacji między jednostkami</span><span class="sxs-lookup"><span data-stu-id="51dd7-170">Deleting a Relationship Between Entities</span></span>

<span data-ttu-id="51dd7-171">Aby usunąć relację, klient wysyła żądanie HTTP DELETE do identyfikatora URI $ref:</span><span class="sxs-lookup"><span data-stu-id="51dd7-171">To delete a relationship, the client sends an HTTP DELETE request to the $ref URI:</span></span>

[!code-console[Main](entity-relations-in-odata-v4/samples/sample18.cmd)]

<span data-ttu-id="51dd7-172">Poniżej przedstawiono metody kontrolera, aby usunąć relację między produktem i dostawcy:</span><span class="sxs-lookup"><span data-stu-id="51dd7-172">Here is the controller method to delete the relationship between a Product and a Supplier:</span></span>

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample19.cs)]

<span data-ttu-id="51dd7-173">W tym przypadku `Product.Supplier` jest &quot;1&quot; end relacji 1-do wielu, dzięki czemu można usunąć relacji, po prostu przez ustawienie `Product.Supplier` do `null`.</span><span class="sxs-lookup"><span data-stu-id="51dd7-173">In this case, `Product.Supplier` is the &quot;1&quot; end of a 1-to-many relation, so you can remove the relationship just by setting `Product.Supplier` to `null`.</span></span>

<span data-ttu-id="51dd7-174">W &quot;wiele&quot; końcu relacji klienta należy określić, które powiązanego obiektu do usunięcia.</span><span class="sxs-lookup"><span data-stu-id="51dd7-174">In the &quot;many&quot; end of a relationship, the client must specify which related entity to remove.</span></span> <span data-ttu-id="51dd7-175">Aby to zrobić, klient wysyła identyfikator URI powiązanej jednostki w ciągu zapytania żądania.</span><span class="sxs-lookup"><span data-stu-id="51dd7-175">To do so, the client sends the URI of the related entity in the query string of the request.</span></span> <span data-ttu-id="51dd7-176">Na przykład, aby usunąć "Product 1" z "Dostawca 1":</span><span class="sxs-lookup"><span data-stu-id="51dd7-176">For example, to remove "Product 1" from "Supplier 1":</span></span>

[!code-console[Main](entity-relations-in-odata-v4/samples/sample20.cmd?highlight=1)]

<span data-ttu-id="51dd7-177">Aby to umożliwić w interfejsie API sieci Web, musimy uwzględnić dodatkowego parametru w `DeleteRef` metody.</span><span class="sxs-lookup"><span data-stu-id="51dd7-177">To support this in Web API, we need to include an extra parameter in the `DeleteRef` method.</span></span> <span data-ttu-id="51dd7-178">Poniżej przedstawiono metody kontrolera, aby usunąć produkt z `Supplier.Products` relacji.</span><span class="sxs-lookup"><span data-stu-id="51dd7-178">Here is the controller method to remove a product from the `Supplier.Products` relation.</span></span>

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample21.cs)]

<span data-ttu-id="51dd7-179">*Klucz* parametr jest klucz dla dostawcy i *relatedKey* parametr jest klucz produktu usunąć z `Products` relacji.</span><span class="sxs-lookup"><span data-stu-id="51dd7-179">The *key* parameter is the key for the supplier, and the *relatedKey* parameter is the key for the product to remove from the `Products` relationship.</span></span> <span data-ttu-id="51dd7-180">Należy pamiętać, że interfejs API sieci Web automatycznie pobiera klucz z ciągu zapytania.</span><span class="sxs-lookup"><span data-stu-id="51dd7-180">Note that Web API automatically gets the key from the query string.</span></span>
