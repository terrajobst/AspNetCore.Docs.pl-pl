---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v3/working-with-entity-relations
title: Obsługa relacji jednostek w protokole OData v3 z interfejsu Web API 2 | Dokumentacja firmy Microsoft
author: MikeWasson
description: 'Większość zestawów danych zdefiniować relacje między jednostkami: klienci mają zamówienia; książki mają autorzy; Jeśli te produkty mają dostawców. Przy użyciu protokołu OData, klienci mogą przejść za pośrednictwem...'
ms.author: aspnetcontent
ms.date: 02/26/2014
ms.assetid: 1e4c2eb4-b6cf-42ff-8a65-4d71ddca0394
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v3/working-with-entity-relations
msc.type: authoredcontent
ms.openlocfilehash: b24e3ca4e3d39b424bec6bb408bb0f85825c6761
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/05/2018
ms.locfileid: "37810747"
---
<a name="supporting-entity-relations-in-odata-v3-with-web-api-2"></a><span data-ttu-id="3a2b1-104">Obsługa relacji jednostek w protokole OData v3 z interfejsu Web API 2</span><span class="sxs-lookup"><span data-stu-id="3a2b1-104">Supporting Entity Relations in OData v3 with Web API 2</span></span>
====================
<span data-ttu-id="3a2b1-105">przez [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="3a2b1-105">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="3a2b1-106">Pobieranie ukończone projektu</span><span class="sxs-lookup"><span data-stu-id="3a2b1-106">Download Completed Project</span></span>](http://code.msdn.microsoft.com/ASPNET-Web-API-OData-cecdb524)

> <span data-ttu-id="3a2b1-107">Większość zestawów danych zdefiniować relacje między jednostkami: klienci mają zamówienia; książki mają autorzy; Jeśli te produkty mają dostawców.</span><span class="sxs-lookup"><span data-stu-id="3a2b1-107">Most data sets define relations between entities: Customers have orders; books have authors; products have suppliers.</span></span> <span data-ttu-id="3a2b1-108">Używanie protokołu OData, klienci mogą przejść za pośrednictwem relacji jednostek.</span><span class="sxs-lookup"><span data-stu-id="3a2b1-108">Using OData, clients can navigate over entity relations.</span></span> <span data-ttu-id="3a2b1-109">Biorąc pod uwagę produktu, można znaleźć dostawcy.</span><span class="sxs-lookup"><span data-stu-id="3a2b1-109">Given a product, you can find the supplier.</span></span> <span data-ttu-id="3a2b1-110">Można również tworzyć i usuwać relacje.</span><span class="sxs-lookup"><span data-stu-id="3a2b1-110">You can also create or remove relationships.</span></span> <span data-ttu-id="3a2b1-111">Na przykład można ustawić dostawcę dla produktu.</span><span class="sxs-lookup"><span data-stu-id="3a2b1-111">For example, you can set the supplier for a product.</span></span>
> 
> <span data-ttu-id="3a2b1-112">W tym samouczku pokazano, jak obsługują te operacje w interfejsie API sieci Web platformy ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="3a2b1-112">This tutorial shows how to support these operations in ASP.NET Web API.</span></span> <span data-ttu-id="3a2b1-113">Samouczek opiera się na samouczka [Tworzenie punktu końcowego OData v3 z sieci Web API 2](creating-an-odata-endpoint.md).</span><span class="sxs-lookup"><span data-stu-id="3a2b1-113">The tutorial builds on the tutorial [Creating an OData v3 Endpoint with Web API 2](creating-an-odata-endpoint.md).</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="3a2b1-114">Wersje oprogramowania używanego w tym samouczku</span><span class="sxs-lookup"><span data-stu-id="3a2b1-114">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="3a2b1-115">Składnik Web API 2</span><span class="sxs-lookup"><span data-stu-id="3a2b1-115">Web API 2</span></span>
> - <span data-ttu-id="3a2b1-116">OData w wersji 3</span><span class="sxs-lookup"><span data-stu-id="3a2b1-116">OData Version 3</span></span>
> - <span data-ttu-id="3a2b1-117">Entity Framework 6</span><span class="sxs-lookup"><span data-stu-id="3a2b1-117">Entity Framework 6</span></span>


## <a name="add-a-supplier-entity"></a><span data-ttu-id="3a2b1-118">Dodaj jednostkę dostawcy</span><span class="sxs-lookup"><span data-stu-id="3a2b1-118">Add a Supplier Entity</span></span>

<span data-ttu-id="3a2b1-119">Najpierw musimy dodać nowy typ jednostek do naszych źródła danych OData.</span><span class="sxs-lookup"><span data-stu-id="3a2b1-119">First we need to add a new entity type to our OData feed.</span></span> <span data-ttu-id="3a2b1-120">Dodamy `Supplier` klasy.</span><span class="sxs-lookup"><span data-stu-id="3a2b1-120">We'll add a `Supplier` class.</span></span>

[!code-csharp[Main](working-with-entity-relations/samples/sample1.cs)]

<span data-ttu-id="3a2b1-121">Ta klasa używa ciągu klucza jednostki.</span><span class="sxs-lookup"><span data-stu-id="3a2b1-121">This class uses a string for the entity key.</span></span> <span data-ttu-id="3a2b1-122">W praktyce, które mogą okazać się rzadziej niż przy użyciu klucza liczby całkowitej.</span><span class="sxs-lookup"><span data-stu-id="3a2b1-122">In practice, that might be less common than using an integer key.</span></span> <span data-ttu-id="3a2b1-123">Jednak warto widoczne, jak OData obsługuje inne typy kluczy, oprócz liczby całkowite.</span><span class="sxs-lookup"><span data-stu-id="3a2b1-123">But it's worth seeing how OData handles other key types besides integers.</span></span>

<span data-ttu-id="3a2b1-124">Następnie utworzymy relację, dodając `Supplier` właściwość `Product` klasy:</span><span class="sxs-lookup"><span data-stu-id="3a2b1-124">Next, we'll create a relation by adding a `Supplier` property to the `Product` class:</span></span>

[!code-csharp[Main](working-with-entity-relations/samples/sample2.cs)]

<span data-ttu-id="3a2b1-125">Dodaj nową **DbSet** do `ProductServiceContext` klasy tak, aby Entity Framework będzie zawierać `Supplier` tabeli w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="3a2b1-125">Add a new **DbSet** to the `ProductServiceContext` class, so that Entity Framework will include the `Supplier` table in the database.</span></span>

[!code-csharp[Main](working-with-entity-relations/samples/sample3.cs?highlight=9)]

<span data-ttu-id="3a2b1-126">W WebApiConfig.cs należy dodać jednostki "Dostawcy" model EDM:</span><span class="sxs-lookup"><span data-stu-id="3a2b1-126">In WebApiConfig.cs, add a "Suppliers" entity to the EDM model:</span></span>

[!code-csharp[Main](working-with-entity-relations/samples/sample4.cs?highlight=4)]

## <a name="navigation-properties"></a><span data-ttu-id="3a2b1-127">Właściwości nawigacji</span><span class="sxs-lookup"><span data-stu-id="3a2b1-127">Navigation Properties</span></span>

<span data-ttu-id="3a2b1-128">Aby uzyskać dostawcy, produktu, klient wysyła żądanie pobrania:</span><span class="sxs-lookup"><span data-stu-id="3a2b1-128">To get the supplier for a product, the client sends a GET request:</span></span>

[!code-console[Main](working-with-entity-relations/samples/sample5.cmd)]

<span data-ttu-id="3a2b1-129">Poniżej przedstawiono właściwości nawigacji "Dostawca" na `Product` typu.</span><span class="sxs-lookup"><span data-stu-id="3a2b1-129">Here "Supplier" is a navigation property on the `Product` type.</span></span> <span data-ttu-id="3a2b1-130">W tym przypadku `Supplier` odnosi się do jednego elementu, ale nawigacji właściwość może również zwracać kolekcję (relacji jeden do wielu lub wiele do wielu).</span><span class="sxs-lookup"><span data-stu-id="3a2b1-130">In this case, `Supplier` refers to a single item, but a navigation property can also return a collection (one-to-many or many-to-many relation).</span></span>

<span data-ttu-id="3a2b1-131">Aby zapewnić obsługę tego żądania, dodaj następującą metodę do `ProductsController` klasy:</span><span class="sxs-lookup"><span data-stu-id="3a2b1-131">To support this request, add the following method to the `ProductsController` class:</span></span>

[!code-csharp[Main](working-with-entity-relations/samples/sample6.cs)]

<span data-ttu-id="3a2b1-132">*Klucz* parametr jest klucz produktu.</span><span class="sxs-lookup"><span data-stu-id="3a2b1-132">The *key* parameter is the key of the product.</span></span> <span data-ttu-id="3a2b1-133">Metoda ta zwraca powiązanej jednostki & #8212 w tym przypadku `Supplier` wystąpienia.</span><span class="sxs-lookup"><span data-stu-id="3a2b1-133">The method returns the related entity&#8212in this case, a `Supplier` instance.</span></span> <span data-ttu-id="3a2b1-134">Nazwa metody i nazwa parametru są ważne.</span><span class="sxs-lookup"><span data-stu-id="3a2b1-134">The method name and parameter name are both important.</span></span> <span data-ttu-id="3a2b1-135">Ogólnie rzecz biorąc Jeśli właściwość nawigacji nosi nazwę "X", należy dodać metodę o nazwie "Metody GetX".</span><span class="sxs-lookup"><span data-stu-id="3a2b1-135">In general, if the navigation property is named "X", you need to add a method named "GetX".</span></span> <span data-ttu-id="3a2b1-136">Metoda przyjmuje parametr o nazwie "*klucz*" który jest zgodny z typem danych klucza nadrzędnego.</span><span class="sxs-lookup"><span data-stu-id="3a2b1-136">The method must take a parameter named "*key*" that matches the data type of the parent's key.</span></span>

<span data-ttu-id="3a2b1-137">Warto również uwzględnić **[FromOdataUri]** atrybutu w *klucz* parametru.</span><span class="sxs-lookup"><span data-stu-id="3a2b1-137">It is also important to include the **[FromOdataUri]** attribute in the *key* parameter.</span></span> <span data-ttu-id="3a2b1-138">Ten atrybut informuje reguły składni OData używane, gdy jej analizuje klucza z identyfikatora URI żądania interfejsu API sieci Web.</span><span class="sxs-lookup"><span data-stu-id="3a2b1-138">This attribute tells Web API to use OData syntax rules when it parses the key from the request URI.</span></span>

## <a name="creating-and-deleting-links"></a><span data-ttu-id="3a2b1-139">Tworzenie i usuwanie łącza</span><span class="sxs-lookup"><span data-stu-id="3a2b1-139">Creating and Deleting Links</span></span>

<span data-ttu-id="3a2b1-140">OData obsługuje tworzenie lub usuwanie relacji między dwiema jednostkami.</span><span class="sxs-lookup"><span data-stu-id="3a2b1-140">OData supports creating or removing relationships between two entities.</span></span> <span data-ttu-id="3a2b1-141">W terminologii OData relacja jest "Połącz".</span><span class="sxs-lookup"><span data-stu-id="3a2b1-141">In OData terminology, the relationship is a "link."</span></span> <span data-ttu-id="3a2b1-142">Każde połączenie ma identyfikator URI z formularzem *jednostki*/$links /*jednostki*.</span><span class="sxs-lookup"><span data-stu-id="3a2b1-142">Each link has a URI with the form *entity*/$links/*entity*.</span></span> <span data-ttu-id="3a2b1-143">Na przykład łącze z produktu dostawcy wygląda następująco:</span><span class="sxs-lookup"><span data-stu-id="3a2b1-143">For example, the link from product to supplier looks like this:</span></span>

[!code-console[Main](working-with-entity-relations/samples/sample7.cmd)]

<span data-ttu-id="3a2b1-144">Aby utworzyć nowy link, klient wysyła żądanie POST do identyfikator URI linku.</span><span class="sxs-lookup"><span data-stu-id="3a2b1-144">To create a new link, the client sends a POST request to the link URI.</span></span> <span data-ttu-id="3a2b1-145">Treść żądania jest identyfikator URI obiektu docelowego.</span><span class="sxs-lookup"><span data-stu-id="3a2b1-145">The body of the request is the URI of the target entity.</span></span> <span data-ttu-id="3a2b1-146">Na przykład załóżmy, że istnieje dostawcy z kluczem "CTSO".</span><span class="sxs-lookup"><span data-stu-id="3a2b1-146">For example, suppose there is a supplier with the key "CTSO".</span></span> <span data-ttu-id="3a2b1-147">Aby utworzyć łącze z "Product(1)" do "Supplier('CTSO')", klient wysyła żądanie, jak pokazano poniżej:</span><span class="sxs-lookup"><span data-stu-id="3a2b1-147">To create a link from "Product(1)" to "Supplier('CTSO')", the client sends a request like the following:</span></span>

[!code-console[Main](working-with-entity-relations/samples/sample8.cmd)]

<span data-ttu-id="3a2b1-148">Aby usunąć łącze, klient wysyła żądanie usunięcia łącza identyfikatora URI.</span><span class="sxs-lookup"><span data-stu-id="3a2b1-148">To delete a link, the client sends a DELETE request to the link URI.</span></span>

<span data-ttu-id="3a2b1-149">**Tworzenie łączy główny**</span><span class="sxs-lookup"><span data-stu-id="3a2b1-149">**Creating Links**</span></span>

<span data-ttu-id="3a2b1-150">Aby włączyć klienta utworzyć łącza dostawcy produktu, Dodaj następujący kod, aby `ProductsController` klasy:</span><span class="sxs-lookup"><span data-stu-id="3a2b1-150">To enable a client to create product-supplier links, add the following code to the `ProductsController` class:</span></span>

[!code-csharp[Main](working-with-entity-relations/samples/sample9.cs)]

<span data-ttu-id="3a2b1-151">Ta metoda przyjmuje trzy parametry:</span><span class="sxs-lookup"><span data-stu-id="3a2b1-151">This method takes three parameters:</span></span>

- <span data-ttu-id="3a2b1-152">*klucz*: klucz do obiektu nadrzędnego (produkt)</span><span class="sxs-lookup"><span data-stu-id="3a2b1-152">*key*: The key to the parent entity (the product)</span></span>
- <span data-ttu-id="3a2b1-153">*Element navigationProperty*: Nazwa właściwości nawigacji.</span><span class="sxs-lookup"><span data-stu-id="3a2b1-153">*navigationProperty*: The name of the navigation property.</span></span> <span data-ttu-id="3a2b1-154">W tym przykładzie właściwość nawigacji z jedynymi prawidłowymi jest "Dostawca".</span><span class="sxs-lookup"><span data-stu-id="3a2b1-154">In this example, the only valid navigation property is "Supplier".</span></span>
- <span data-ttu-id="3a2b1-155">*Link*: identyfikatora URI OData powiązanej jednostki.</span><span class="sxs-lookup"><span data-stu-id="3a2b1-155">*link*: The OData URI of the related entity.</span></span> <span data-ttu-id="3a2b1-156">Ta wartość jest pobierana z treści żądania.</span><span class="sxs-lookup"><span data-stu-id="3a2b1-156">This value is taken from the request body.</span></span> <span data-ttu-id="3a2b1-157">Na przykład łącze identyfikatora URI może być "`http://localhost/odata/Suppliers('CTSO')`, co oznacza dostawcy o identyfikatorze ="CTSO".</span><span class="sxs-lookup"><span data-stu-id="3a2b1-157">For example, the link URI might be "`http://localhost/odata/Suppliers('CTSO')`, meaning the supplier with ID = ‘CTSO'.</span></span>

<span data-ttu-id="3a2b1-158">Metoda używa łącze do wyszukiwania dostawcy.</span><span class="sxs-lookup"><span data-stu-id="3a2b1-158">The method uses the link to look up the supplier.</span></span> <span data-ttu-id="3a2b1-159">Jeśli zostanie znalezione pasujące dostawcy, metoda ustawia `Product.Supplier` właściwości i zapisuje wynik w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="3a2b1-159">If the matching supplier is found, the method sets the `Product.Supplier` property and saves the result to the database.</span></span>

<span data-ttu-id="3a2b1-160">Część najtrudniejsze jest analizowanie identyfikator URI linku.</span><span class="sxs-lookup"><span data-stu-id="3a2b1-160">The hardest part is parsing the link URI.</span></span> <span data-ttu-id="3a2b1-161">Po prostu musisz symulować wynik wysyłającego żądanie GET do tego identyfikatora URI.</span><span class="sxs-lookup"><span data-stu-id="3a2b1-161">Basically, you need to simulate the result of sending a GET request to that URI.</span></span> <span data-ttu-id="3a2b1-162">Następującą metodę pomocnika pokazuje, jak to zrobić.</span><span class="sxs-lookup"><span data-stu-id="3a2b1-162">The following helper method shows how to do this.</span></span> <span data-ttu-id="3a2b1-163">Metoda wywołuje proces routingu internetowego interfejsu API i otrzymuje **element ODataPath** wystąpienia, która reprezentuje przeanalizowany ścieżki OData.</span><span class="sxs-lookup"><span data-stu-id="3a2b1-163">The method invokes the Web API routing process and gets back an **ODataPath** instance that represents the parsed OData path.</span></span> <span data-ttu-id="3a2b1-164">Identyfikator URI linku jednego z segmentów powinna być kluczem jednostki.</span><span class="sxs-lookup"><span data-stu-id="3a2b1-164">For a link URI, one of the segments should be the entity key.</span></span> <span data-ttu-id="3a2b1-165">(Jeśli nie, klient wysłał nieprawidłowy identyfikator URI.)</span><span class="sxs-lookup"><span data-stu-id="3a2b1-165">(If not, the client sent a bad URI.)</span></span>

[!code-csharp[Main](working-with-entity-relations/samples/sample10.cs)]

<span data-ttu-id="3a2b1-166">**Usuwanie łącza**</span><span class="sxs-lookup"><span data-stu-id="3a2b1-166">**Deleting Links**</span></span>

<span data-ttu-id="3a2b1-167">Aby usunąć łącze, Dodaj następujący kod, aby `ProductsController` klasy:</span><span class="sxs-lookup"><span data-stu-id="3a2b1-167">To delete a link, add the following code to the `ProductsController` class:</span></span>

[!code-csharp[Main](working-with-entity-relations/samples/sample11.cs)]

<span data-ttu-id="3a2b1-168">W tym przykładzie właściwość nawigacji jest pojedynczym `Supplier` jednostki.</span><span class="sxs-lookup"><span data-stu-id="3a2b1-168">In this example, the navigation property is a single `Supplier` entity.</span></span> <span data-ttu-id="3a2b1-169">Jeśli właściwość nawigacji jest kolekcją, identyfikator URI do usunięcia łącza musi zawierać klucz dla obiektu pokrewnego.</span><span class="sxs-lookup"><span data-stu-id="3a2b1-169">If the navigation property is a collection, the URI to delete a link must include a key for the related entity.</span></span> <span data-ttu-id="3a2b1-170">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="3a2b1-170">For example:</span></span>

[!code-console[Main](working-with-entity-relations/samples/sample12.cmd)]

<span data-ttu-id="3a2b1-171">To żądanie usuwa zamówienie 1 z klienta customer 1.</span><span class="sxs-lookup"><span data-stu-id="3a2b1-171">This request removes order 1 from customer 1.</span></span> <span data-ttu-id="3a2b1-172">W tym przypadku metoda DeleteLink mają następujący podpis:</span><span class="sxs-lookup"><span data-stu-id="3a2b1-172">In this case, the DeleteLink method will have the following signature:</span></span>

[!code-csharp[Main](working-with-entity-relations/samples/sample13.cs)]

<span data-ttu-id="3a2b1-173">*RelatedKey* parametru udostępnia klucz dla obiektu pokrewnego.</span><span class="sxs-lookup"><span data-stu-id="3a2b1-173">The *relatedKey* parameter gives the key for the related entity.</span></span> <span data-ttu-id="3a2b1-174">Tak w Twojej `DeleteLink` metody odnośnika podstawowej jednostki przez *klucz* parametru, Znajdź powiązanej jednostki przez *relatedKey* parametru, a następnie Usuń skojarzenie.</span><span class="sxs-lookup"><span data-stu-id="3a2b1-174">So in your `DeleteLink` method, look up the primary entity by the *key* parameter, find the related entity by the *relatedKey* parameter, and then remove the association.</span></span> <span data-ttu-id="3a2b1-175">W zależności od modelu danych, czasami trzeba zaimplementować obie wersje `DeleteLink`.</span><span class="sxs-lookup"><span data-stu-id="3a2b1-175">Depending on your data model, you might need to implement both versions of `DeleteLink`.</span></span> <span data-ttu-id="3a2b1-176">Interfejs API sieci Web będzie wywoływać poprawnej wersji oparte na żądaniu identyfikatora URI.</span><span class="sxs-lookup"><span data-stu-id="3a2b1-176">Web API will call the correct version based on the request URI.</span></span>
