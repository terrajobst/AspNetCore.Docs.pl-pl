---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/entity-relations-in-odata-v4
title: "Relacjami jednostek w protokołu OData v4 używanie składnika ASP.NET Web API 2.2 | Dokumentacja firmy Microsoft"
author: MikeWasson
description: "Większość zestawów danych definiować relacje między obiektami: klienci mają zleceń; książki ma autorów; produkty mają dostawców. Używanie protokołu OData, klientów można przechodzić przez..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/26/2014
ms.topic: article
ms.assetid: 72657550-ec09-4779-9bfc-2fb15ecd51c7
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/entity-relations-in-odata-v4
msc.type: authoredcontent
ms.openlocfilehash: 4127fb2fa83f513a4faeb222068ca99f234ece22
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
<a name="entity-relations-in-odata-v4-using-aspnet-web-api-22"></a><span data-ttu-id="36651-104">Relacjami jednostek w protokołu OData v4 używanie składnika ASP.NET Web API 2.2</span><span class="sxs-lookup"><span data-stu-id="36651-104">Entity Relations in OData v4 Using ASP.NET Web API 2.2</span></span>
====================
<span data-ttu-id="36651-105">przez [Wasson Jan](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="36651-105">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

> <span data-ttu-id="36651-106">Większość zestawów danych definiować relacje między obiektami: klienci mają zleceń; książki ma autorów; produkty mają dostawców.</span><span class="sxs-lookup"><span data-stu-id="36651-106">Most data sets define relations between entities: Customers have orders; books have authors; products have suppliers.</span></span> <span data-ttu-id="36651-107">Używanie protokołu OData, klienci mogą przechodzić za pośrednictwem relacjami jednostek.</span><span class="sxs-lookup"><span data-stu-id="36651-107">Using OData, clients can navigate over entity relations.</span></span> <span data-ttu-id="36651-108">Biorąc pod uwagę produktu, można znaleźć dostawcy.</span><span class="sxs-lookup"><span data-stu-id="36651-108">Given a product, you can find the supplier.</span></span> <span data-ttu-id="36651-109">Można również utworzyć lub Usuń relacje.</span><span class="sxs-lookup"><span data-stu-id="36651-109">You can also create or remove relationships.</span></span> <span data-ttu-id="36651-110">Na przykład można ustawić dostawcy produktu.</span><span class="sxs-lookup"><span data-stu-id="36651-110">For example, you can set the supplier for a product.</span></span>
> 
> <span data-ttu-id="36651-111">W tym samouczku przedstawiono sposób obsługi tych operacji w protokołu OData v4 przy użyciu interfejsu API sieci Web platformy ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="36651-111">This tutorial shows how to support these operations in OData v4 using ASP.NET Web API.</span></span> <span data-ttu-id="36651-112">Samouczek opiera się na samouczka [utworzyć OData v4 punktu końcowego Using ASP.NET Web API 2](create-an-odata-v4-endpoint.md).</span><span class="sxs-lookup"><span data-stu-id="36651-112">The tutorial builds on the tutorial [Create an OData v4 Endpoint Using ASP.NET Web API 2](create-an-odata-v4-endpoint.md).</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="36651-113">Używane w samouczku wersje oprogramowania</span><span class="sxs-lookup"><span data-stu-id="36651-113">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="36651-114">2.1 interfejsu API sieci Web</span><span class="sxs-lookup"><span data-stu-id="36651-114">Web API 2.1</span></span>
> - <span data-ttu-id="36651-115">Protokołu OData v4</span><span class="sxs-lookup"><span data-stu-id="36651-115">OData v4</span></span>
> - [<span data-ttu-id="36651-116">Visual Studio 2013 Update 2</span><span class="sxs-lookup"><span data-stu-id="36651-116">Visual Studio 2013 Update 2</span></span>](https://www.visualstudio.com/downloads/download-visual-studio-vs)
> - <span data-ttu-id="36651-117">Entity Framework 6</span><span class="sxs-lookup"><span data-stu-id="36651-117">Entity Framework 6</span></span>
> - <span data-ttu-id="36651-118">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="36651-118">.NET 4.5</span></span>
> 
> 
> ## <a name="tutorial-versions"></a><span data-ttu-id="36651-119">Samouczek wersji</span><span class="sxs-lookup"><span data-stu-id="36651-119">Tutorial versions</span></span>
> 
> <span data-ttu-id="36651-120">Dla OData w wersji 3, zobacz [obsługi relacjami jednostek w OData v3](https://asp.net/web-api/overview/odata-support-in-aspnet-web-api/odata-v3/working-with-entity-relations).</span><span class="sxs-lookup"><span data-stu-id="36651-120">For the OData Version 3, see [Supporting Entity Relations in OData v3](https://asp.net/web-api/overview/odata-support-in-aspnet-web-api/odata-v3/working-with-entity-relations).</span></span>


## <a name="add-a-supplier-entity"></a><span data-ttu-id="36651-121">Dodawanie jednostki dostawcy</span><span class="sxs-lookup"><span data-stu-id="36651-121">Add a Supplier Entity</span></span>

> [!NOTE]
> <span data-ttu-id="36651-122">Samouczek opiera się na samouczka [utworzyć OData v4 punktu końcowego Using ASP.NET Web API 2](create-an-odata-v4-endpoint.md).</span><span class="sxs-lookup"><span data-stu-id="36651-122">The tutorial builds on the tutorial [Create an OData v4 Endpoint Using ASP.NET Web API 2](create-an-odata-v4-endpoint.md).</span></span>


<span data-ttu-id="36651-123">Najpierw należy powiązanej jednostki.</span><span class="sxs-lookup"><span data-stu-id="36651-123">First, we need a related entity.</span></span> <span data-ttu-id="36651-124">Dodaj klasę o nazwie `Supplier` w folderze modeli.</span><span class="sxs-lookup"><span data-stu-id="36651-124">Add a class named `Supplier` in the Models folder.</span></span>

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample1.cs)]

<span data-ttu-id="36651-125">Właściwość nawigacji, aby dodać `Product` klasy:</span><span class="sxs-lookup"><span data-stu-id="36651-125">Add a navigation property to the `Product` class:</span></span>

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample2.cs?highlight=13-15)]

<span data-ttu-id="36651-126">Dodaj nową **DbSet** do `ProductsContext` klasy, tak aby zawiera tabeli dostawcy programu Entity Framework w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="36651-126">Add a new **DbSet** to the `ProductsContext` class, so that Entity Framework will include the Supplier table in the database.</span></span>

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample3.cs?highlight=10)]

<span data-ttu-id="36651-127">W WebApiConfig.cs, Dodaj &quot;dostawców&quot; zestawu jednostek do modelu danych jednostki:</span><span class="sxs-lookup"><span data-stu-id="36651-127">In WebApiConfig.cs, add a &quot;Suppliers&quot; entity set to the entity data model:</span></span>

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample4.cs?highlight=6)]

## <a name="add-a-suppliers-controller"></a><span data-ttu-id="36651-128">Dodawanie kontrolera dostawców</span><span class="sxs-lookup"><span data-stu-id="36651-128">Add a Suppliers Controller</span></span>

<span data-ttu-id="36651-129">Dodaj `SuppliersController` klasy do folderu kontrolerów.</span><span class="sxs-lookup"><span data-stu-id="36651-129">Add a `SuppliersController` class to the Controllers folder.</span></span>

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample5.cs)]

<span data-ttu-id="36651-130">I nie będzie pokazują, jak dodać operacje CRUD dla tego kontrolera.</span><span class="sxs-lookup"><span data-stu-id="36651-130">I won't show how to add CRUD operations for this controller.</span></span> <span data-ttu-id="36651-131">Kroki są takie same jak dla kontrolera produktów (zobacz [utworzyć punktu końcowego OData v4](create-an-odata-v4-endpoint.md)).</span><span class="sxs-lookup"><span data-stu-id="36651-131">The steps are the same as for the Products controller (see [Create an OData v4 Endpoint](create-an-odata-v4-endpoint.md)).</span></span>

## <a name="getting-related-entities"></a><span data-ttu-id="36651-132">Pobieranie powiązanych jednostek</span><span class="sxs-lookup"><span data-stu-id="36651-132">Getting Related Entities</span></span>

<span data-ttu-id="36651-133">Aby uzyskać dostawcę produktu, klient wysyła żądanie pobrania:</span><span class="sxs-lookup"><span data-stu-id="36651-133">To get the supplier for a product, the client sends a GET request:</span></span>

[!code-console[Main](entity-relations-in-odata-v4/samples/sample6.cmd)]

<span data-ttu-id="36651-134">Aby zapewnić obsługę tego żądania, dodaj następującą metodę do `ProductsController` klasy:</span><span class="sxs-lookup"><span data-stu-id="36651-134">To support this request, add the following method to the `ProductsController` class:</span></span>

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample7.cs)]

<span data-ttu-id="36651-135">Ta metoda używa domyślnej konwencji nazewnictwa</span><span class="sxs-lookup"><span data-stu-id="36651-135">This method uses a default naming convention</span></span>

- <span data-ttu-id="36651-136">Nazwa metody: GetX, gdzie X jest właściwością nawigacji.</span><span class="sxs-lookup"><span data-stu-id="36651-136">Method name: GetX, where X is the navigation property.</span></span>
- <span data-ttu-id="36651-137">Nazwa parametru: *klucza*</span><span class="sxs-lookup"><span data-stu-id="36651-137">Parameter name: *key*</span></span>

<span data-ttu-id="36651-138">Jeśli wykonujesz tę konwencję nazewnictwa interfejsu API sieci Web automatycznie mapuje żądania HTTP do metody kontrolera.</span><span class="sxs-lookup"><span data-stu-id="36651-138">If you follow this naming convention, Web API automatically maps the HTTP request to the controller method.</span></span>

<span data-ttu-id="36651-139">Przykład HTTP żądania:</span><span class="sxs-lookup"><span data-stu-id="36651-139">Example HTTP request:</span></span>

[!code-console[Main](entity-relations-in-odata-v4/samples/sample8.cmd)]

<span data-ttu-id="36651-140">Przykład HTTP odpowiedzi:</span><span class="sxs-lookup"><span data-stu-id="36651-140">Example HTTP response:</span></span>

[!code-console[Main](entity-relations-in-odata-v4/samples/sample9.cmd)]

### <a name="getting-a-related-collection"></a><span data-ttu-id="36651-141">Pobierania kolekcję pokrewne</span><span class="sxs-lookup"><span data-stu-id="36651-141">Getting a related collection</span></span>

<span data-ttu-id="36651-142">W poprzednim przykładzie produkt ma jeden dostawca.</span><span class="sxs-lookup"><span data-stu-id="36651-142">In the previous example, a product has one supplier.</span></span> <span data-ttu-id="36651-143">Właściwość nawigacji można także wrócić kolekcji.</span><span class="sxs-lookup"><span data-stu-id="36651-143">A navigation property can also return a collection.</span></span> <span data-ttu-id="36651-144">Poniższy kod pobiera produktów dla dostawcy:</span><span class="sxs-lookup"><span data-stu-id="36651-144">The following code gets the products for a supplier:</span></span>

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample10.cs)]

<span data-ttu-id="36651-145">W tym przypadku metoda zwraca **IQueryable** zamiast **SingleResult&lt;T&gt;**</span><span class="sxs-lookup"><span data-stu-id="36651-145">In this case, the method returns an **IQueryable** instead of a **SingleResult&lt;T&gt;**</span></span>

<span data-ttu-id="36651-146">Przykład HTTP żądania:</span><span class="sxs-lookup"><span data-stu-id="36651-146">Example HTTP request:</span></span>

[!code-console[Main](entity-relations-in-odata-v4/samples/sample11.cmd)]

<span data-ttu-id="36651-147">Przykład HTTP odpowiedzi:</span><span class="sxs-lookup"><span data-stu-id="36651-147">Example HTTP response:</span></span>

[!code-console[Main](entity-relations-in-odata-v4/samples/sample12.cmd)]

## <a name="creating-a-relationship-between-entities"></a><span data-ttu-id="36651-148">Tworzenie relacji między jednostkami</span><span class="sxs-lookup"><span data-stu-id="36651-148">Creating a Relationship Between Entities</span></span>

<span data-ttu-id="36651-149">OData obsługuje tworzenie lub usuwanie relacji między dwiema jednostkami istniejących.</span><span class="sxs-lookup"><span data-stu-id="36651-149">OData supports creating or removing relationships between two existing entities.</span></span> <span data-ttu-id="36651-150">W terminologii OData v4 relacji jest &quot;odwołania&quot;.</span><span class="sxs-lookup"><span data-stu-id="36651-150">In OData v4 terminology, the relationship is a &quot;reference&quot;.</span></span> <span data-ttu-id="36651-151">(W wersji 3 OData, wywołano relacji *łącze*.</span><span class="sxs-lookup"><span data-stu-id="36651-151">(In OData v3, the relationship was called a *link*.</span></span> <span data-ttu-id="36651-152">Różnice protokołu nie ma znaczenia w tym samouczku.)</span><span class="sxs-lookup"><span data-stu-id="36651-152">The protocol differences don't matter for this tutorial.)</span></span>

<span data-ttu-id="36651-153">Odwołanie ma własny identyfikator URI, za pomocą formularza `/Entity/NavigationProperty/$ref`.</span><span class="sxs-lookup"><span data-stu-id="36651-153">A reference has its own URI, with the form `/Entity/NavigationProperty/$ref`.</span></span> <span data-ttu-id="36651-154">Na przykład w tym miejscu jest identyfikator URI do adresów odwołanie między produktu i jego dostawcy:</span><span class="sxs-lookup"><span data-stu-id="36651-154">For example, here is the URI to address the reference between a product and its supplier:</span></span>

[!code-console[Main](entity-relations-in-odata-v4/samples/sample13.cmd)]

<span data-ttu-id="36651-155">Aby dodać relację, klient wysyła żądanie POST i PUT na ten adres.</span><span class="sxs-lookup"><span data-stu-id="36651-155">To add a relationship, the client sends a POST or PUT request to this address.</span></span>

- <span data-ttu-id="36651-156">USTAWIĆ właściwość nawigacji jest pojedyncza jednostka, takich jak `Product.Supplier`.</span><span class="sxs-lookup"><span data-stu-id="36651-156">PUT if the navigation property is a single entity, such as `Product.Supplier`.</span></span>
- <span data-ttu-id="36651-157">Jeśli właściwość nawigacji jest kolekcją, takich jak `Supplier.Products`.</span><span class="sxs-lookup"><span data-stu-id="36651-157">POST if the navigation property is a collection, such as `Supplier.Products`.</span></span>

<span data-ttu-id="36651-158">Treść żądania zawiera identyfikator URI inne jednostki w relacji.</span><span class="sxs-lookup"><span data-stu-id="36651-158">The body of the request contains the URI of the other entity in the relation.</span></span> <span data-ttu-id="36651-159">Poniżej przedstawiono przykładowe żądanie:</span><span class="sxs-lookup"><span data-stu-id="36651-159">Here is an example request:</span></span>

[!code-console[Main](entity-relations-in-odata-v4/samples/sample14.cmd)]

<span data-ttu-id="36651-160">W tym przykładzie klient wysyła żądanie PUT `/Products(6)/Supplier/$ref`, który jest identyfikatorem URI $ref dla `Supplier` produktu o identyfikatorze = 6.</span><span class="sxs-lookup"><span data-stu-id="36651-160">In this example, the client sends a PUT request to `/Products(6)/Supplier/$ref`, which is the $ref URI for the `Supplier` of the product with ID = 6.</span></span> <span data-ttu-id="36651-161">Jeśli żądanie zakończy się powodzeniem, serwer wysyła 204 odpowiedzi (bez zawartości):</span><span class="sxs-lookup"><span data-stu-id="36651-161">If the request succeeds, the server sends a 204 (No Content) response:</span></span>

[!code-console[Main](entity-relations-in-odata-v4/samples/sample15.cmd)]

<span data-ttu-id="36651-162">Oto metoda kontrolera, można dodać relacji do `Product`:</span><span class="sxs-lookup"><span data-stu-id="36651-162">Here is the controller method to add a relationship to a `Product`:</span></span>

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample16.cs)]

<span data-ttu-id="36651-163">*NavigationProperty* parametr określa relacji, które można ustawić.</span><span class="sxs-lookup"><span data-stu-id="36651-163">The *navigationProperty* parameter specifies which relationship to set.</span></span> <span data-ttu-id="36651-164">(Jeśli istnieje więcej niż jedna właściwość nawigacji na jednostce, możesz dodać więcej `case` instrukcji.)</span><span class="sxs-lookup"><span data-stu-id="36651-164">(If there is more than one navigation property on the entity, you can add more `case` statements.)</span></span>

<span data-ttu-id="36651-165">*Łącze* parametr zawiera identyfikator URI dostawcy.</span><span class="sxs-lookup"><span data-stu-id="36651-165">The *link* parameter contains the URI of the supplier.</span></span> <span data-ttu-id="36651-166">Interfejs API sieci Web automatycznie analizuje treść żądania, aby uzyskać wartość tego parametru.</span><span class="sxs-lookup"><span data-stu-id="36651-166">Web API automatically parses the request body to get the value for this parameter.</span></span>

<span data-ttu-id="36651-167">Aby wyszukać dostawcy, potrzebujemy identyfikator (lub kluczy), który jest częścią *łącze* parametru.</span><span class="sxs-lookup"><span data-stu-id="36651-167">To look up the supplier, we need the ID (or key), which is part of the *link* parameter.</span></span> <span data-ttu-id="36651-168">Aby to zrobić, użyj następującej metody pomocnika:</span><span class="sxs-lookup"><span data-stu-id="36651-168">To do this, use the following helper method:</span></span>

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample17.cs)]

<span data-ttu-id="36651-169">Zasadniczo ta metoda korzysta z biblioteki OData ścieżka identyfikatora URI jest podzielona na segmenty, Znajdź segment, który zawiera klucz i przekonwertować klucz do poprawnego typu.</span><span class="sxs-lookup"><span data-stu-id="36651-169">Basically, this method uses the OData library to split the URI path into segments, find the segment that contains the key, and convert the key into the correct type.</span></span>

## <a name="deleting-a-relationship-between-entities"></a><span data-ttu-id="36651-170">Usuwanie relacji między jednostkami</span><span class="sxs-lookup"><span data-stu-id="36651-170">Deleting a Relationship Between Entities</span></span>

<span data-ttu-id="36651-171">Aby usunąć relacji, klient wysyła żądanie HTTP DELETE do $ref identyfikatora URI:</span><span class="sxs-lookup"><span data-stu-id="36651-171">To delete a relationship, the client sends an HTTP DELETE request to the $ref URI:</span></span>

[!code-console[Main](entity-relations-in-odata-v4/samples/sample18.cmd)]

<span data-ttu-id="36651-172">Oto metoda kontrolera, można usunąć relacji między produktu i dostawcy:</span><span class="sxs-lookup"><span data-stu-id="36651-172">Here is the controller method to delete the relationship between a Product and a Supplier:</span></span>

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample19.cs)]

<span data-ttu-id="36651-173">W takim przypadku `Product.Supplier` jest &quot;1&quot; końca relacji 1-do wielu, dlatego w przypadku usunięcia relacji tylko przez ustawienie `Product.Supplier` do `null`.</span><span class="sxs-lookup"><span data-stu-id="36651-173">In this case, `Product.Supplier` is the &quot;1&quot; end of a 1-to-many relation, so you can remove the relationship just by setting `Product.Supplier` to `null`.</span></span>

<span data-ttu-id="36651-174">W &quot;wiele&quot; końca relacji klienta należy określić, który powiązanego obiektu do usunięcia.</span><span class="sxs-lookup"><span data-stu-id="36651-174">In the &quot;many&quot; end of a relationship, the client must specify which related entity to remove.</span></span> <span data-ttu-id="36651-175">Aby to zrobić, klient wysyła identyfikator URI obiektu pokrewnego w ciągu zapytania żądania.</span><span class="sxs-lookup"><span data-stu-id="36651-175">To do so, the client sends the URI of the related entity in the query string of the request.</span></span> <span data-ttu-id="36651-176">Na przykład, aby usunąć "Produktu 1" z "dostawcy 1":</span><span class="sxs-lookup"><span data-stu-id="36651-176">For example, to remove "Product 1" from "Supplier 1":</span></span>

[!code-console[Main](entity-relations-in-odata-v4/samples/sample20.cmd?highlight=1)]

<span data-ttu-id="36651-177">Aby zapewnić tę obsługę w składniku Web API, musimy dołączyć dodatkowy parametr w `DeleteRef` metody.</span><span class="sxs-lookup"><span data-stu-id="36651-177">To support this in Web API, we need to include an extra parameter in the `DeleteRef` method.</span></span> <span data-ttu-id="36651-178">Oto metoda kontrolera, aby usunąć produkt z `Supplier.Products` relacji.</span><span class="sxs-lookup"><span data-stu-id="36651-178">Here is the controller method to remove a product from the `Supplier.Products` relation.</span></span>

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample21.cs)]

<span data-ttu-id="36651-179">*Klucza* parametr jest klucz dla dostawcy, a *relatedKey* parametru jest klucz produktu usunąć z `Products` relacji.</span><span class="sxs-lookup"><span data-stu-id="36651-179">The *key* parameter is the key for the supplier, and the *relatedKey* parameter is the key for the product to remove from the `Products` relationship.</span></span> <span data-ttu-id="36651-180">Należy pamiętać, że interfejs API sieci Web automatycznie pobiera klucz z ciągu zapytania.</span><span class="sxs-lookup"><span data-stu-id="36651-180">Note that Web API automatically gets the key from the query string.</span></span>
