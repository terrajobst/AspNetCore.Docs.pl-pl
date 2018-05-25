---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v3/working-with-entity-relations
title: Obsługa relacjami jednostek w OData v3 z składnika Web API 2 | Dokumentacja firmy Microsoft
author: MikeWasson
description: 'Większość zestawów danych definiować relacje między obiektami: klienci mają zleceń; książki ma autorów; produkty mają dostawców. Używanie protokołu OData, klientów można przechodzić przez...'
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/26/2014
ms.topic: article
ms.assetid: 1e4c2eb4-b6cf-42ff-8a65-4d71ddca0394
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v3/working-with-entity-relations
msc.type: authoredcontent
ms.openlocfilehash: dec7e10e59cc2441c967afe062df227b105106a1
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
<a name="supporting-entity-relations-in-odata-v3-with-web-api-2"></a><span data-ttu-id="8df40-104">Obsługa relacjami jednostek w OData v3 z składnika Web API 2</span><span class="sxs-lookup"><span data-stu-id="8df40-104">Supporting Entity Relations in OData v3 with Web API 2</span></span>
====================
<span data-ttu-id="8df40-105">przez [Wasson Jan](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="8df40-105">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="8df40-106">Pobieranie ukończone projektu</span><span class="sxs-lookup"><span data-stu-id="8df40-106">Download Completed Project</span></span>](http://code.msdn.microsoft.com/ASPNET-Web-API-OData-cecdb524)

> <span data-ttu-id="8df40-107">Większość zestawów danych definiować relacje między obiektami: klienci mają zleceń; książki ma autorów; produkty mają dostawców.</span><span class="sxs-lookup"><span data-stu-id="8df40-107">Most data sets define relations between entities: Customers have orders; books have authors; products have suppliers.</span></span> <span data-ttu-id="8df40-108">Używanie protokołu OData, klienci mogą przechodzić za pośrednictwem relacjami jednostek.</span><span class="sxs-lookup"><span data-stu-id="8df40-108">Using OData, clients can navigate over entity relations.</span></span> <span data-ttu-id="8df40-109">Biorąc pod uwagę produktu, można znaleźć dostawcy.</span><span class="sxs-lookup"><span data-stu-id="8df40-109">Given a product, you can find the supplier.</span></span> <span data-ttu-id="8df40-110">Można również utworzyć lub Usuń relacje.</span><span class="sxs-lookup"><span data-stu-id="8df40-110">You can also create or remove relationships.</span></span> <span data-ttu-id="8df40-111">Na przykład można ustawić dostawcy produktu.</span><span class="sxs-lookup"><span data-stu-id="8df40-111">For example, you can set the supplier for a product.</span></span>
> 
> <span data-ttu-id="8df40-112">W tym samouczku przedstawiono sposób obsługi tych operacji w interfejsie API sieci Web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="8df40-112">This tutorial shows how to support these operations in ASP.NET Web API.</span></span> <span data-ttu-id="8df40-113">Samouczek opiera się na samouczka [tworzenia punktu końcowego OData v3 z sieci Web API 2](creating-an-odata-endpoint.md).</span><span class="sxs-lookup"><span data-stu-id="8df40-113">The tutorial builds on the tutorial [Creating an OData v3 Endpoint with Web API 2](creating-an-odata-endpoint.md).</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="8df40-114">Używane w samouczku wersje oprogramowania</span><span class="sxs-lookup"><span data-stu-id="8df40-114">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="8df40-115">Składnik Web API 2</span><span class="sxs-lookup"><span data-stu-id="8df40-115">Web API 2</span></span>
> - <span data-ttu-id="8df40-116">OData w wersji 3</span><span class="sxs-lookup"><span data-stu-id="8df40-116">OData Version 3</span></span>
> - <span data-ttu-id="8df40-117">Entity Framework 6</span><span class="sxs-lookup"><span data-stu-id="8df40-117">Entity Framework 6</span></span>


## <a name="add-a-supplier-entity"></a><span data-ttu-id="8df40-118">Dodawanie jednostki dostawcy</span><span class="sxs-lookup"><span data-stu-id="8df40-118">Add a Supplier Entity</span></span>

<span data-ttu-id="8df40-119">Najpierw należy dodać nowy typ jednostek do naszej źródła strumieniowego OData.</span><span class="sxs-lookup"><span data-stu-id="8df40-119">First we need to add a new entity type to our OData feed.</span></span> <span data-ttu-id="8df40-120">Dodamy `Supplier` klasy.</span><span class="sxs-lookup"><span data-stu-id="8df40-120">We'll add a `Supplier` class.</span></span>

[!code-csharp[Main](working-with-entity-relations/samples/sample1.cs)]

<span data-ttu-id="8df40-121">Ta klasa korzysta z ciągiem klucza jednostki.</span><span class="sxs-lookup"><span data-stu-id="8df40-121">This class uses a string for the entity key.</span></span> <span data-ttu-id="8df40-122">W praktyce, która może być mniej typowych niż przy użyciu klucza.</span><span class="sxs-lookup"><span data-stu-id="8df40-122">In practice, that might be less common than using an integer key.</span></span> <span data-ttu-id="8df40-123">Jednak warto zobaczenia jak OData obsługuje innych typów kluczy oprócz liczb całkowitych.</span><span class="sxs-lookup"><span data-stu-id="8df40-123">But it's worth seeing how OData handles other key types besides integers.</span></span>

<span data-ttu-id="8df40-124">Następnie utworzymy relacji przez dodanie `Supplier` właściwości `Product` klasy:</span><span class="sxs-lookup"><span data-stu-id="8df40-124">Next, we'll create a relation by adding a `Supplier` property to the `Product` class:</span></span>

[!code-csharp[Main](working-with-entity-relations/samples/sample2.cs)]

<span data-ttu-id="8df40-125">Dodaj nową **DbSet** do `ProductServiceContext` klasy, tak aby uwzględni Entity Framework `Supplier` tabeli w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="8df40-125">Add a new **DbSet** to the `ProductServiceContext` class, so that Entity Framework will include the `Supplier` table in the database.</span></span>

[!code-csharp[Main](working-with-entity-relations/samples/sample3.cs?highlight=9)]

<span data-ttu-id="8df40-126">W WebApiConfig.cs należy dodać do jednostki "Dostawcy" do modelu EDM:</span><span class="sxs-lookup"><span data-stu-id="8df40-126">In WebApiConfig.cs, add a "Suppliers" entity to the EDM model:</span></span>

[!code-csharp[Main](working-with-entity-relations/samples/sample4.cs?highlight=4)]

## <a name="navigation-properties"></a><span data-ttu-id="8df40-127">Właściwości nawigacji</span><span class="sxs-lookup"><span data-stu-id="8df40-127">Navigation Properties</span></span>

<span data-ttu-id="8df40-128">Aby uzyskać dostawcę produktu, klient wysyła żądanie pobrania:</span><span class="sxs-lookup"><span data-stu-id="8df40-128">To get the supplier for a product, the client sends a GET request:</span></span>

[!code-console[Main](working-with-entity-relations/samples/sample5.cmd)]

<span data-ttu-id="8df40-129">W tym miejscu "Dostawca" jest właściwością nawigacji na `Product` typu.</span><span class="sxs-lookup"><span data-stu-id="8df40-129">Here "Supplier" is a navigation property on the `Product` type.</span></span> <span data-ttu-id="8df40-130">W takim przypadku `Supplier` odwołuje się do pojedynczego elementu, ale Nawigacja właściwości można także wrócić kolekcji (relacji jeden do wielu lub wiele do wielu).</span><span class="sxs-lookup"><span data-stu-id="8df40-130">In this case, `Supplier` refers to a single item, but a navigation property can also return a collection (one-to-many or many-to-many relation).</span></span>

<span data-ttu-id="8df40-131">Aby zapewnić obsługę tego żądania, dodaj następującą metodę do `ProductsController` klasy:</span><span class="sxs-lookup"><span data-stu-id="8df40-131">To support this request, add the following method to the `ProductsController` class:</span></span>

[!code-csharp[Main](working-with-entity-relations/samples/sample6.cs)]

<span data-ttu-id="8df40-132">*Klucza* parametru jest klucz produktu.</span><span class="sxs-lookup"><span data-stu-id="8df40-132">The *key* parameter is the key of the product.</span></span> <span data-ttu-id="8df40-133">Metoda zwraca obiekt pokrewny & #8212 w takim przypadku `Supplier` wystąpienia.</span><span class="sxs-lookup"><span data-stu-id="8df40-133">The method returns the related entity&#8212in this case, a `Supplier` instance.</span></span> <span data-ttu-id="8df40-134">Nazwa metody i nazwa parametru są ważne.</span><span class="sxs-lookup"><span data-stu-id="8df40-134">The method name and parameter name are both important.</span></span> <span data-ttu-id="8df40-135">Ogólnie rzecz biorąc Jeśli właściwość nawigacji ma nazwę "X", należy dodać metodę o nazwie "GetX".</span><span class="sxs-lookup"><span data-stu-id="8df40-135">In general, if the navigation property is named "X", you need to add a method named "GetX".</span></span> <span data-ttu-id="8df40-136">Metoda przyjmuje parametr o nazwie "*klucza*" odpowiedniego dla typu danych klucza nadrzędnego.</span><span class="sxs-lookup"><span data-stu-id="8df40-136">The method must take a parameter named "*key*" that matches the data type of the parent's key.</span></span>

<span data-ttu-id="8df40-137">Należy również uwzględnić **[FromOdataUri]** atrybutu w *klucza* parametru.</span><span class="sxs-lookup"><span data-stu-id="8df40-137">It is also important to include the **[FromOdataUri]** attribute in the *key* parameter.</span></span> <span data-ttu-id="8df40-138">Ten atrybut informuje interfejsu API sieci Web do używania reguły składni OData po przeanalizowaniu klucza z identyfikatora URI żądania.</span><span class="sxs-lookup"><span data-stu-id="8df40-138">This attribute tells Web API to use OData syntax rules when it parses the key from the request URI.</span></span>

## <a name="creating-and-deleting-links"></a><span data-ttu-id="8df40-139">Tworzenie i usuwanie łącza</span><span class="sxs-lookup"><span data-stu-id="8df40-139">Creating and Deleting Links</span></span>

<span data-ttu-id="8df40-140">OData obsługuje tworzenie lub usuwanie relacji między dwiema jednostkami.</span><span class="sxs-lookup"><span data-stu-id="8df40-140">OData supports creating or removing relationships between two entities.</span></span> <span data-ttu-id="8df40-141">W terminologii OData relacji jest "link".</span><span class="sxs-lookup"><span data-stu-id="8df40-141">In OData terminology, the relationship is a "link."</span></span> <span data-ttu-id="8df40-142">Każde łącze ma identyfikator URI w formularzu *jednostki*/$links /*jednostki*.</span><span class="sxs-lookup"><span data-stu-id="8df40-142">Each link has a URI with the form *entity*/$links/*entity*.</span></span> <span data-ttu-id="8df40-143">Na przykład link z produktu dostawcy wygląda następująco:</span><span class="sxs-lookup"><span data-stu-id="8df40-143">For example, the link from product to supplier looks like this:</span></span>

[!code-console[Main](working-with-entity-relations/samples/sample7.cmd)]

<span data-ttu-id="8df40-144">Aby utworzyć nowy link, klient wysyła żądanie POST do łącza identyfikatora URI.</span><span class="sxs-lookup"><span data-stu-id="8df40-144">To create a new link, the client sends a POST request to the link URI.</span></span> <span data-ttu-id="8df40-145">Treść żądania jest identyfikatorem URI obiektu docelowego.</span><span class="sxs-lookup"><span data-stu-id="8df40-145">The body of the request is the URI of the target entity.</span></span> <span data-ttu-id="8df40-146">Na przykład załóżmy, że istnieje dostawcy z kluczem "CTSO".</span><span class="sxs-lookup"><span data-stu-id="8df40-146">For example, suppose there is a supplier with the key "CTSO".</span></span> <span data-ttu-id="8df40-147">Aby utworzyć łącze z "Product(1)" do "Supplier('CTSO')", klient wysyła żądanie podobne do poniższych:</span><span class="sxs-lookup"><span data-stu-id="8df40-147">To create a link from "Product(1)" to "Supplier('CTSO')", the client sends a request like the following:</span></span>

[!code-console[Main](working-with-entity-relations/samples/sample8.cmd)]

<span data-ttu-id="8df40-148">Aby usunąć łącza, klient wysyła żądanie usunięcia łącza identyfikatora URI.</span><span class="sxs-lookup"><span data-stu-id="8df40-148">To delete a link, the client sends a DELETE request to the link URI.</span></span>

<span data-ttu-id="8df40-149">**Tworzenie łączy**</span><span class="sxs-lookup"><span data-stu-id="8df40-149">**Creating Links**</span></span>

<span data-ttu-id="8df40-150">Aby włączyć klienta do utworzenia łącza dostawcy produktu, Dodaj następujący kod do `ProductsController` klasy:</span><span class="sxs-lookup"><span data-stu-id="8df40-150">To enable a client to create product-supplier links, add the following code to the `ProductsController` class:</span></span>

[!code-csharp[Main](working-with-entity-relations/samples/sample9.cs)]

<span data-ttu-id="8df40-151">Ta metoda przyjmuje trzy parametry:</span><span class="sxs-lookup"><span data-stu-id="8df40-151">This method takes three parameters:</span></span>

- <span data-ttu-id="8df40-152">*klucz*: klucz do obiektu nadrzędnego (product)</span><span class="sxs-lookup"><span data-stu-id="8df40-152">*key*: The key to the parent entity (the product)</span></span>
- <span data-ttu-id="8df40-153">*Element navigationProperty*: Nazwa właściwości nawigacji.</span><span class="sxs-lookup"><span data-stu-id="8df40-153">*navigationProperty*: The name of the navigation property.</span></span> <span data-ttu-id="8df40-154">W tym przykładzie tylko prawidłową właściwością nawigacji jest "Dostawca".</span><span class="sxs-lookup"><span data-stu-id="8df40-154">In this example, the only valid navigation property is "Supplier".</span></span>
- <span data-ttu-id="8df40-155">*łącze*: identyfikator URI OData powiązanej jednostki.</span><span class="sxs-lookup"><span data-stu-id="8df40-155">*link*: The OData URI of the related entity.</span></span> <span data-ttu-id="8df40-156">Ta wartość jest pobierana z treści żądania.</span><span class="sxs-lookup"><span data-stu-id="8df40-156">This value is taken from the request body.</span></span> <span data-ttu-id="8df40-157">Na przykład łącze identyfikatora URI może być "`http://localhost/odata/Suppliers('CTSO')`, co oznacza dostawcy o identyfikatorze ="CTSO".</span><span class="sxs-lookup"><span data-stu-id="8df40-157">For example, the link URI might be "`http://localhost/odata/Suppliers('CTSO')`, meaning the supplier with ID = ‘CTSO'.</span></span>

<span data-ttu-id="8df40-158">Metoda używa łącze do wyszukania dostawcy.</span><span class="sxs-lookup"><span data-stu-id="8df40-158">The method uses the link to look up the supplier.</span></span> <span data-ttu-id="8df40-159">Jeśli zostanie znaleziony zgodnego dostawcę, Ustawia metodę `Product.Supplier` właściwości i zapisuje go w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="8df40-159">If the matching supplier is found, the method sets the `Product.Supplier` property and saves the result to the database.</span></span>

<span data-ttu-id="8df40-160">Część najtrudniejsze jest podczas analizowania łącza identyfikatora URI.</span><span class="sxs-lookup"><span data-stu-id="8df40-160">The hardest part is parsing the link URI.</span></span> <span data-ttu-id="8df40-161">Zasadniczo należy symulować działanie wysyłania żądania GET do tego identyfikatora URI.</span><span class="sxs-lookup"><span data-stu-id="8df40-161">Basically, you need to simulate the result of sending a GET request to that URI.</span></span> <span data-ttu-id="8df40-162">Następująca metoda pomocnika pokazano, jak to zrobić.</span><span class="sxs-lookup"><span data-stu-id="8df40-162">The following helper method shows how to do this.</span></span> <span data-ttu-id="8df40-163">Metoda wywołuje proces routingu interfejsu API sieci Web i otrzymuje w odpowiedzi **element ODataPath** wystąpienie reprezentującego przeanalizowany ścieżki OData.</span><span class="sxs-lookup"><span data-stu-id="8df40-163">The method invokes the Web API routing process and gets back an **ODataPath** instance that represents the parsed OData path.</span></span> <span data-ttu-id="8df40-164">Łącza identyfikatora URI jednego z segmentów powinna być klucza jednostki.</span><span class="sxs-lookup"><span data-stu-id="8df40-164">For a link URI, one of the segments should be the entity key.</span></span> <span data-ttu-id="8df40-165">(Jeśli nie, klient wysłał nieprawidłowy identyfikator URI.)</span><span class="sxs-lookup"><span data-stu-id="8df40-165">(If not, the client sent a bad URI.)</span></span>

[!code-csharp[Main](working-with-entity-relations/samples/sample10.cs)]

<span data-ttu-id="8df40-166">**Usunięcie łącza**</span><span class="sxs-lookup"><span data-stu-id="8df40-166">**Deleting Links**</span></span>

<span data-ttu-id="8df40-167">Aby usunąć łącza, Dodaj następujący kod do `ProductsController` klasy:</span><span class="sxs-lookup"><span data-stu-id="8df40-167">To delete a link, add the following code to the `ProductsController` class:</span></span>

[!code-csharp[Main](working-with-entity-relations/samples/sample11.cs)]

<span data-ttu-id="8df40-168">W tym przykładzie właściwość nawigacji jest jeden `Supplier` jednostki.</span><span class="sxs-lookup"><span data-stu-id="8df40-168">In this example, the navigation property is a single `Supplier` entity.</span></span> <span data-ttu-id="8df40-169">Jeśli właściwość nawigacji jest kolekcją, identyfikator URI do usunięcia łącza musi zawierać klucz powiązanej jednostki.</span><span class="sxs-lookup"><span data-stu-id="8df40-169">If the navigation property is a collection, the URI to delete a link must include a key for the related entity.</span></span> <span data-ttu-id="8df40-170">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="8df40-170">For example:</span></span>

[!code-console[Main](working-with-entity-relations/samples/sample12.cmd)]

<span data-ttu-id="8df40-171">To żądanie usuwa kolejności 1 klientów 1.</span><span class="sxs-lookup"><span data-stu-id="8df40-171">This request removes order 1 from customer 1.</span></span> <span data-ttu-id="8df40-172">W tym przypadku metoda DeleteLink będzie mieć następującą sygnaturą:</span><span class="sxs-lookup"><span data-stu-id="8df40-172">In this case, the DeleteLink method will have the following signature:</span></span>

[!code-csharp[Main](working-with-entity-relations/samples/sample13.cs)]

<span data-ttu-id="8df40-173">*RelatedKey* parametru zapewnia klucz powiązanej jednostki.</span><span class="sxs-lookup"><span data-stu-id="8df40-173">The *relatedKey* parameter gives the key for the related entity.</span></span> <span data-ttu-id="8df40-174">Tak w Twojej `DeleteLink` metody wyszukiwania podstawowego jednostki według *klucza* parametru znaleźć obiektu pokrewnego przez *relatedKey* parametr, a następnie Usuń skojarzenie.</span><span class="sxs-lookup"><span data-stu-id="8df40-174">So in your `DeleteLink` method, look up the primary entity by the *key* parameter, find the related entity by the *relatedKey* parameter, and then remove the association.</span></span> <span data-ttu-id="8df40-175">W zależności od modelu danych, może być konieczne wdrożenie obie wersje `DeleteLink`.</span><span class="sxs-lookup"><span data-stu-id="8df40-175">Depending on your data model, you might need to implement both versions of `DeleteLink`.</span></span> <span data-ttu-id="8df40-176">Interfejs API sieci Web będzie wywoływać poprawnej wersji oparte na identyfikator URI żądania.</span><span class="sxs-lookup"><span data-stu-id="8df40-176">Web API will call the correct version based on the request URI.</span></span>
