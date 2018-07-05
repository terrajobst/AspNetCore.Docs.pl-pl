---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v3/odata-actions
title: Obsługa akcji OData we wzorcu ASP.NET Web API 2 | Dokumentacja firmy Microsoft
author: MikeWasson
description: 'W protokole OData akcje to sposób, aby dodać zachowania po stronie serwera, które łatwo nie są zdefiniowane jako operacje CRUD na jednostkach. Do niektórych zastosowań akcje obejmują: Implementowanie...'
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/25/2014
ms.topic: article
ms.assetid: 2d7b3aa2-aa47-4e6e-b0ce-3d65a1c6fe02
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v3/odata-actions
msc.type: authoredcontent
ms.openlocfilehash: b7a968082587120c2a19be86524f9b2eba80856e
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37370458"
---
<a name="supporting-odata-actions-in-aspnet-web-api-2"></a><span data-ttu-id="f86fb-104">Obsługa akcji OData we wzorcu ASP.NET Web API 2</span><span class="sxs-lookup"><span data-stu-id="f86fb-104">Supporting OData Actions in ASP.NET Web API 2</span></span>
====================
<span data-ttu-id="f86fb-105">przez [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="f86fb-105">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="f86fb-106">Pobieranie ukończone projektu</span><span class="sxs-lookup"><span data-stu-id="f86fb-106">Download Completed Project</span></span>](http://code.msdn.microsoft.com/ASPNET-Web-API-OData-cecdb524)

> <span data-ttu-id="f86fb-107">W protokole OData *akcje* to sposób, aby dodać zachowania po stronie serwera, które łatwo nie są zdefiniowane jako operacje CRUD na jednostkach.</span><span class="sxs-lookup"><span data-stu-id="f86fb-107">In OData, *actions* are a way to add server-side behaviors that are not easily defined as CRUD operations on entities.</span></span> <span data-ttu-id="f86fb-108">Do niektórych zastosowań akcje obejmują:</span><span class="sxs-lookup"><span data-stu-id="f86fb-108">Some uses for actions include:</span></span>
> 
> - <span data-ttu-id="f86fb-109">Implementowanie złożonych transakcji.</span><span class="sxs-lookup"><span data-stu-id="f86fb-109">Implementing complex transactions.</span></span>
> - <span data-ttu-id="f86fb-110">Manipulowanie kilka jednostek naraz.</span><span class="sxs-lookup"><span data-stu-id="f86fb-110">Manipulating several entities at once.</span></span>
> - <span data-ttu-id="f86fb-111">Zezwalanie na aktualizacje tylko dla niektórych właściwości jednostki.</span><span class="sxs-lookup"><span data-stu-id="f86fb-111">Allowing updates only to certain properties of an entity.</span></span>
> - <span data-ttu-id="f86fb-112">Wysyłanie informacji do serwera, który nie jest zdefiniowany w jednostce.</span><span class="sxs-lookup"><span data-stu-id="f86fb-112">Sending information to the server that is not defined in an entity.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="f86fb-113">Wersje oprogramowania używanego w tym samouczku</span><span class="sxs-lookup"><span data-stu-id="f86fb-113">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="f86fb-114">Składnik Web API 2</span><span class="sxs-lookup"><span data-stu-id="f86fb-114">Web API 2</span></span>
> - <span data-ttu-id="f86fb-115">OData w wersji 3</span><span class="sxs-lookup"><span data-stu-id="f86fb-115">OData Version 3</span></span>
> - <span data-ttu-id="f86fb-116">Entity Framework 6</span><span class="sxs-lookup"><span data-stu-id="f86fb-116">Entity Framework 6</span></span>


## <a name="example-rating-a-product"></a><span data-ttu-id="f86fb-117">Przykład: Ocena produktu</span><span class="sxs-lookup"><span data-stu-id="f86fb-117">Example: Rating a Product</span></span>

<span data-ttu-id="f86fb-118">W tym przykładzie chcemy umożliwić użytkownikom oceniać produkty, a następnie udostępnić średnią ocen dla każdego produktu.</span><span class="sxs-lookup"><span data-stu-id="f86fb-118">In this example, we want to let users rate products, and then expose the average ratings for each product.</span></span> <span data-ttu-id="f86fb-119">W bazie danych będą przechowywane listy klasyfikacji, opartych na kluczach produktów.</span><span class="sxs-lookup"><span data-stu-id="f86fb-119">On the database, we will store a list of ratings, keyed to products.</span></span>

<span data-ttu-id="f86fb-120">Poniżej przedstawiono model, może być używana do reprezentowania klasyfikacje platformy Entity Framework:</span><span class="sxs-lookup"><span data-stu-id="f86fb-120">Here is the model we might use to represent the ratings in Entity Framework:</span></span>

[!code-csharp[Main](odata-actions/samples/sample1.cs)]

<span data-ttu-id="f86fb-121">Ale nie chcemy, aby klienci POST `ProductRating` obiektu do kolekcji "Klasyfikacje".</span><span class="sxs-lookup"><span data-stu-id="f86fb-121">But we don't want clients to POST a `ProductRating` object to a "Ratings" collection.</span></span> <span data-ttu-id="f86fb-122">Intuicyjne ocena jest skojarzona z tą kolekcją produktów, a klient powinien wystarczy wpis wartość oceny.</span><span class="sxs-lookup"><span data-stu-id="f86fb-122">Intuitively, the rating is associated with the Products collection, and the client should only need to post the rating value.</span></span>

<span data-ttu-id="f86fb-123">W związku z tym zamiast korzystać z normalnych operacji CRUD, definiujemy akcję, które klient może wywołać nad produktem.</span><span class="sxs-lookup"><span data-stu-id="f86fb-123">Therefore, instead of using the normal CRUD operations, we define an action that a client can invoke on a Product.</span></span> <span data-ttu-id="f86fb-124">W terminologii OData, akcja jest *powiązany* jednostek produktu.</span><span class="sxs-lookup"><span data-stu-id="f86fb-124">In OData terminology, the action is *bound* to Product entities.</span></span>

><span data-ttu-id="f86fb-125">Akcji ma efekty uboczne, na serwerze.</span><span class="sxs-lookup"><span data-stu-id="f86fb-125">Actions have side-effects on the server.</span></span> <span data-ttu-id="f86fb-126">Z tego powodu są wywoływane, za pomocą żądania HTTP POST.</span><span class="sxs-lookup"><span data-stu-id="f86fb-126">For this reason, they are invoked using HTTP POST requests.</span></span> <span data-ttu-id="f86fb-127">Akcje mogą mieć parametry i zwracane typy, które są opisane w metadanych usługi.</span><span class="sxs-lookup"><span data-stu-id="f86fb-127">Actions can have parameters and return types, which are described in the service metadata.</span></span> <span data-ttu-id="f86fb-128">Klient wysyła parametrów w treści żądania, a serwer wysyła wartości zwracanej w treści odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="f86fb-128">The client sends the parameters in the request body, and the server sends the return value in the response body.</span></span> <span data-ttu-id="f86fb-129">Aby wywołać akcję "Współczynnik produkt", klient wysyła WPIS do identyfikatora URI, jak pokazano poniżej:</span><span class="sxs-lookup"><span data-stu-id="f86fb-129">To invoke the "Rate Product" action, the client sends a POST to a URI like the following:</span></span>

[!code-console[Main](odata-actions/samples/sample2.cmd)]

<span data-ttu-id="f86fb-130">Dane w żądaniu POST jest po prostu ocena produktu:</span><span class="sxs-lookup"><span data-stu-id="f86fb-130">The data in the POST request is simply the product rating:</span></span>

[!code-console[Main](odata-actions/samples/sample3.cmd)]

## <a name="declare-the-action-in-the-entity-data-model"></a><span data-ttu-id="f86fb-131">Zadeklaruj akcji w modelu Entity Data Model</span><span class="sxs-lookup"><span data-stu-id="f86fb-131">Declare the Action in the Entity Data Model</span></span>

<span data-ttu-id="f86fb-132">W konfiguracji interfejsu API sieci Web Dodaj do modelu entity data model (EDM struktury) akcję:</span><span class="sxs-lookup"><span data-stu-id="f86fb-132">In your Web API configuration, add the action to the entity data model (EDM):</span></span>

[!code-csharp[Main](odata-actions/samples/sample4.cs)]

<span data-ttu-id="f86fb-133">Ten kod definiuje "RateProduct" jako akcję, które mogą być wykonywane na jednostkach produktu.</span><span class="sxs-lookup"><span data-stu-id="f86fb-133">This code defines "RateProduct" as an action that can be performed on Product entities.</span></span> <span data-ttu-id="f86fb-134">Również deklaruje, że akcja jest wykonywana **int** parametr o nazwie "Ocena" i zwraca **int** wartość.</span><span class="sxs-lookup"><span data-stu-id="f86fb-134">It also declares that the action takes an **int** parameter named "Rating", and returns an **int** value.</span></span>

## <a name="add-the-action-to-the-controller"></a><span data-ttu-id="f86fb-135">Dodawanie akcji do kontrolera</span><span class="sxs-lookup"><span data-stu-id="f86fb-135">Add the Action to the Controller</span></span>

<span data-ttu-id="f86fb-136">Akcja "RateProduct" jest powiązany z jednostki produktu.</span><span class="sxs-lookup"><span data-stu-id="f86fb-136">The "RateProduct" action is bound to Product entities.</span></span> <span data-ttu-id="f86fb-137">Aby zastosować akcję, Dodaj metodę o nazwie `RateProduct` kontrolerowi produktów:</span><span class="sxs-lookup"><span data-stu-id="f86fb-137">To implement the action, add a method named `RateProduct` to the Products controller:</span></span>

[!code-csharp[Main](odata-actions/samples/sample5.cs)]

<span data-ttu-id="f86fb-138">Należy zauważyć, że nazwa metody odpowiada nazwie w akcji EDM.</span><span class="sxs-lookup"><span data-stu-id="f86fb-138">Notice that the method name matches the name of the action in the EDM.</span></span> <span data-ttu-id="f86fb-139">Metoda ma dwa parametry:</span><span class="sxs-lookup"><span data-stu-id="f86fb-139">The method has two parameters:</span></span>

- <span data-ttu-id="f86fb-140">*klucz*: klucz produktu do szybkości.</span><span class="sxs-lookup"><span data-stu-id="f86fb-140">*key*: The key for the product to rate.</span></span>
- <span data-ttu-id="f86fb-141">*Parametry*: słownika wartości parametrów akcji.</span><span class="sxs-lookup"><span data-stu-id="f86fb-141">*parameters*: A dictionary of action parameter values.</span></span>

<span data-ttu-id="f86fb-142">Jeśli używasz domyślnych Konwencji tras parametr klucza musi mieć nazwę "klucza".</span><span class="sxs-lookup"><span data-stu-id="f86fb-142">If you are using the default routing conventions, the key parameter must be named "key".</span></span> <span data-ttu-id="f86fb-143">Warto również uwzględnić **[FromOdataUri]** atrybutu, jak pokazano.</span><span class="sxs-lookup"><span data-stu-id="f86fb-143">It is also important to include the **[FromOdataUri]** attribute, as shown.</span></span> <span data-ttu-id="f86fb-144">Ten atrybut informuje reguły składni OData używane, gdy jej analizuje klucza z identyfikatora URI żądania interfejsu API sieci Web.</span><span class="sxs-lookup"><span data-stu-id="f86fb-144">This attribute tells Web API to use OData syntax rules when it parses the key from the request URI.</span></span>

<span data-ttu-id="f86fb-145">Użyj *parametry* słownika można pobrać parametrów akcji:</span><span class="sxs-lookup"><span data-stu-id="f86fb-145">Use the *parameters* dictionary to get the action parameters:</span></span>

[!code-csharp[Main](odata-actions/samples/sample6.cs)]

<span data-ttu-id="f86fb-146">Jeśli klient wysyła parametry akcji w poprawny format, wartość **ModelState.IsValid** ma wartość true.</span><span class="sxs-lookup"><span data-stu-id="f86fb-146">If the client sends the action parameters in the correct format, the value of **ModelState.IsValid** is true.</span></span> <span data-ttu-id="f86fb-147">W takim przypadku można użyć **ODataActionParameters** słownika do uzyskania wartości parametrów.</span><span class="sxs-lookup"><span data-stu-id="f86fb-147">In that case, you can use the **ODataActionParameters** dictionary to get the parameter values.</span></span> <span data-ttu-id="f86fb-148">W tym przykładzie `RateProduct` akcji przyjmuje jeden parametr o nazwie "Ocena".</span><span class="sxs-lookup"><span data-stu-id="f86fb-148">In this example, the `RateProduct` action takes a single parameter named "Rating".</span></span>

## <a name="action-metadata"></a><span data-ttu-id="f86fb-149">Metadane akcji</span><span class="sxs-lookup"><span data-stu-id="f86fb-149">Action Metadata</span></span>

<span data-ttu-id="f86fb-150">Aby wyświetlić metadane usługi, Wyślij żądanie Pobierz do /odata/$ metadanych.</span><span class="sxs-lookup"><span data-stu-id="f86fb-150">To view the service metadata, send a GET request to /odata/$metadata.</span></span> <span data-ttu-id="f86fb-151">Poniżej przedstawiono część metadanych, który deklaruje `RateProduct` akcji:</span><span class="sxs-lookup"><span data-stu-id="f86fb-151">Here is the portion of the metadata that declares the `RateProduct` action:</span></span>

[!code-xml[Main](odata-actions/samples/sample7.xml)]

<span data-ttu-id="f86fb-152">**FunctionImport** elementu deklaruje akcji.</span><span class="sxs-lookup"><span data-stu-id="f86fb-152">The **FunctionImport** element declares the action.</span></span> <span data-ttu-id="f86fb-153">Większość pól jest oczywista, ale dwa są warte odnotowania:</span><span class="sxs-lookup"><span data-stu-id="f86fb-153">Most of the fields are self-explanatory, but two are worth noting:</span></span>

- <span data-ttu-id="f86fb-154">**Może być powiązana** oznacza, że akcja może być wywołana na jednostkę docelową co najmniej pewien czas.</span><span class="sxs-lookup"><span data-stu-id="f86fb-154">**IsBindable** means the action can be invoked on the target entity, at least some of the time.</span></span>
- <span data-ttu-id="f86fb-155">**Funkcja IsAlwaysBindable** oznacza, że akcja zawsze może być wywołana na jednostkę docelową.</span><span class="sxs-lookup"><span data-stu-id="f86fb-155">**IsAlwaysBindable** means the action can always be invoked on the target entity.</span></span>

<span data-ttu-id="f86fb-156">Różnica polega na niektóre akcje są zawsze dostępne dla klientów, że inne akcje mogą zależeć od stan jednostki.</span><span class="sxs-lookup"><span data-stu-id="f86fb-156">The difference is that some actions are always available to clients, but other actions might depend on the state of the entity.</span></span> <span data-ttu-id="f86fb-157">Na przykład załóżmy, że definiowania akcji "Kup".</span><span class="sxs-lookup"><span data-stu-id="f86fb-157">For example, suppose you define a "Purchase" action.</span></span> <span data-ttu-id="f86fb-158">Można kupić tylko elementu, który znajduje się w magazynie.</span><span class="sxs-lookup"><span data-stu-id="f86fb-158">You can only purchase an item that is in stock.</span></span> <span data-ttu-id="f86fb-159">Jeśli element znajduje się na stanie, klient nie może wywołać tę akcję.</span><span class="sxs-lookup"><span data-stu-id="f86fb-159">If the item is out of stock, a client cannot invoke that action.</span></span>

<span data-ttu-id="f86fb-160">Podczas definiowania EDM, **akcji** metoda tworzy zawsze powiązać akcji:</span><span class="sxs-lookup"><span data-stu-id="f86fb-160">When you define the EDM, the **Action** method creates an always-bindable action:</span></span>

[!code-csharp[Main](odata-actions/samples/sample8.cs?highlight=1)]

<span data-ttu-id="f86fb-161">Omówię nie zawsze — możliwości powiązania akcji (nazywane również *przejściowy* akcji) w dalszej części tego tematu.</span><span class="sxs-lookup"><span data-stu-id="f86fb-161">I'll talk about not-always-bindable actions (also called *transient* actions) later in this topic.</span></span>

## <a name="invoking-the-action"></a><span data-ttu-id="f86fb-162">Wywoływania akcji</span><span class="sxs-lookup"><span data-stu-id="f86fb-162">Invoking the Action</span></span>

<span data-ttu-id="f86fb-163">Teraz zobaczmy, jak klient powodowałoby wywołanie pliku wykonywalnego tej akcji.</span><span class="sxs-lookup"><span data-stu-id="f86fb-163">Now let's see how a client would invoke this action.</span></span> <span data-ttu-id="f86fb-164">Załóżmy, że klient chce udzielić ma klasyfikację od 2 do produktu o identyfikatorze = 4.</span><span class="sxs-lookup"><span data-stu-id="f86fb-164">Suppose the client wants to give a rating of 2 to the product with ID = 4.</span></span> <span data-ttu-id="f86fb-165">Oto przykładowy komunikat żądania, przy użyciu formatu JSON w treści żądania:</span><span class="sxs-lookup"><span data-stu-id="f86fb-165">Here is an example request message, using JSON format for the request body:</span></span>

[!code-console[Main](odata-actions/samples/sample9.cmd)]

<span data-ttu-id="f86fb-166">Oto komunikat odpowiedzi:</span><span class="sxs-lookup"><span data-stu-id="f86fb-166">Here is the response message:</span></span>

[!code-console[Main](odata-actions/samples/sample10.cmd)]

## <a name="binding-an-action-to-an-entity-set"></a><span data-ttu-id="f86fb-167">Powiązania akcji do zbioru jednostek</span><span class="sxs-lookup"><span data-stu-id="f86fb-167">Binding an Action to an Entity Set</span></span>

<span data-ttu-id="f86fb-168">W poprzednim przykładzie, akcja jest powiązany z pojedynczej jednostki: klient ocenia jednego produktu.</span><span class="sxs-lookup"><span data-stu-id="f86fb-168">In the previous example, the action is bound to a single entity: The client rates a single product.</span></span> <span data-ttu-id="f86fb-169">Możesz również powiązać akcję do kolekcji jednostek.</span><span class="sxs-lookup"><span data-stu-id="f86fb-169">You can also bind an action to a collection of entities.</span></span> <span data-ttu-id="f86fb-170">Po prostu wprowadź następujące zmiany:</span><span class="sxs-lookup"><span data-stu-id="f86fb-170">Just make the following changes:</span></span>

<span data-ttu-id="f86fb-171">W EDM, Dodaj akcję w jednostce **kolekcji** właściwości.</span><span class="sxs-lookup"><span data-stu-id="f86fb-171">In the EDM, add the action to the entity's **Collection** property.</span></span>

[!code-csharp[Main](odata-actions/samples/sample11.cs?highlight=1)]

<span data-ttu-id="f86fb-172">W przypadku metody kontrolera pominąć *klucz* parametru.</span><span class="sxs-lookup"><span data-stu-id="f86fb-172">In the controller method, omit the *key* parameter.</span></span>

[!code-csharp[Main](odata-actions/samples/sample12.cs)]

<span data-ttu-id="f86fb-173">Teraz klient wywołuje akcję w zestawie jednostek produktów:</span><span class="sxs-lookup"><span data-stu-id="f86fb-173">Now the client invokes the action on the Products entity set:</span></span>

[!code-console[Main](odata-actions/samples/sample13.cmd)]

## <a name="actions-with-collection-parameters"></a><span data-ttu-id="f86fb-174">Akcje z parametrami kolekcji</span><span class="sxs-lookup"><span data-stu-id="f86fb-174">Actions with Collection Parameters</span></span>

<span data-ttu-id="f86fb-175">Akcje może mieć parametrów, które przyjmują kolekcję wartości.</span><span class="sxs-lookup"><span data-stu-id="f86fb-175">Actions can have parameters that take a collection of values.</span></span> <span data-ttu-id="f86fb-176">W EDM, użyj **CollectionParameter&lt;T&gt;**  Aby zadeklarować parametr.</span><span class="sxs-lookup"><span data-stu-id="f86fb-176">In the EDM, use **CollectionParameter&lt;T&gt;** to declare the parameter.</span></span>

[!code-csharp[Main](odata-actions/samples/sample14.cs)]

<span data-ttu-id="f86fb-177">To deklaruje parametr o nazwie "Klasyfikacje", które z kolekcji **int** wartości.</span><span class="sxs-lookup"><span data-stu-id="f86fb-177">This declares a parameter named "Ratings" that takes a collection of **int** values.</span></span> <span data-ttu-id="f86fb-178">W przypadku metody kontrolera będzie nadal się pojawiać wartość parametru **ODataActionParameters** obiektu, ale teraz wartość **ICollection&lt;int&gt;**  wartość:</span><span class="sxs-lookup"><span data-stu-id="f86fb-178">In the controller method, you still get the parameter value from the **ODataActionParameters** object, but now the value is an **ICollection&lt;int&gt;** value:</span></span>

[!code-csharp[Main](odata-actions/samples/sample15.cs)]

## <a name="transient-actions"></a><span data-ttu-id="f86fb-179">Akcje przejściowe</span><span class="sxs-lookup"><span data-stu-id="f86fb-179">Transient Actions</span></span>

<span data-ttu-id="f86fb-180">W tym przykładzie "RateProduct" użytkownicy zawsze ocenić produktu, tak akcja jest zawsze dostępna.</span><span class="sxs-lookup"><span data-stu-id="f86fb-180">In the "RateProduct" example, users can always rate a product, so the action is always available.</span></span> <span data-ttu-id="f86fb-181">Jednak niektóre akcje są zależne od stanu jednostki.</span><span class="sxs-lookup"><span data-stu-id="f86fb-181">But some actions depend on the state of the entity.</span></span> <span data-ttu-id="f86fb-182">Na przykład w usłudze wideo wypożyczeń akcji "Wyewidencjonowania" nie zawsze jest dostępne.</span><span class="sxs-lookup"><span data-stu-id="f86fb-182">For example, in a video rental service, the "CheckOut" action is not always available.</span></span> <span data-ttu-id="f86fb-183">(Jest to uzależnione czy kopię tego wideo jest dostępna.) Ten typ akcji jest wywoływana *przejściowy* akcji.</span><span class="sxs-lookup"><span data-stu-id="f86fb-183">(It depends whether a copy of that video is available.) This type of action is called a *transient* action.</span></span>

<span data-ttu-id="f86fb-184">W metadanych usługi ma przejściowy akcji **IsAlwaysBindable** równa false.</span><span class="sxs-lookup"><span data-stu-id="f86fb-184">In the service metadata, a transient action has **IsAlwaysBindable** equal to false.</span></span> <span data-ttu-id="f86fb-185">Który jest faktycznie wartość domyślna, więc metadane będzie wyglądać następująco:</span><span class="sxs-lookup"><span data-stu-id="f86fb-185">That's actually the default value, so the metadata will look like this:</span></span>

[!code-xml[Main](odata-actions/samples/sample16.xml)]

<span data-ttu-id="f86fb-186">Oto dlaczego jest to istotne: Jeśli akcja jest przejściowy, serwer musi sprawdzić klientowi, gdy akcja jest dostępna.</span><span class="sxs-lookup"><span data-stu-id="f86fb-186">Here's why this matters: If an action is transient, the server needs to tell the client when the action is available.</span></span> <span data-ttu-id="f86fb-187">Jest to możliwe dzięki tym link do akcji w jednostce.</span><span class="sxs-lookup"><span data-stu-id="f86fb-187">It does this by including a link to the action in the entity.</span></span> <span data-ttu-id="f86fb-188">Oto przykład dla jednostki film:</span><span class="sxs-lookup"><span data-stu-id="f86fb-188">Here is an example for a Movie entity:</span></span>

[!code-console[Main](odata-actions/samples/sample17.cmd)]

<span data-ttu-id="f86fb-189">Właściwość "#CheckOut" zawiera łącze do Akcja CheckOut.</span><span class="sxs-lookup"><span data-stu-id="f86fb-189">The "#CheckOut" property contains a link to the CheckOut action.</span></span> <span data-ttu-id="f86fb-190">Jeśli akcja nie jest dostępna, serwer pomija łącze.</span><span class="sxs-lookup"><span data-stu-id="f86fb-190">If the action is not available, the server omits the link.</span></span>

<span data-ttu-id="f86fb-191">Aby zadeklarować przejściowy akcji w EDM, należy wywołać **TransientAction** metody:</span><span class="sxs-lookup"><span data-stu-id="f86fb-191">To declare a transient action in the EDM, call the **TransientAction** method:</span></span>

[!code-csharp[Main](odata-actions/samples/sample18.cs)]

<span data-ttu-id="f86fb-192">Ponadto należy podać funkcji, która zwraca łącze akcji dla danej jednostki.</span><span class="sxs-lookup"><span data-stu-id="f86fb-192">Also, you must provide a function that returns an action link for a given entity.</span></span> <span data-ttu-id="f86fb-193">Ustaw tę funkcję, wywołując **HasActionLink**.</span><span class="sxs-lookup"><span data-stu-id="f86fb-193">Set this function by calling **HasActionLink**.</span></span> <span data-ttu-id="f86fb-194">Funkcję można napisać jako wyrażenie lambda:</span><span class="sxs-lookup"><span data-stu-id="f86fb-194">You can write the function as a lambda expression:</span></span>

[!code-csharp[Main](odata-actions/samples/sample19.cs)]

<span data-ttu-id="f86fb-195">Jeśli akcja jest dostępna, wyrażenie lambda zwraca link do akcji.</span><span class="sxs-lookup"><span data-stu-id="f86fb-195">If the action is available, the lambda expression returns a link to the action.</span></span> <span data-ttu-id="f86fb-196">Reprezentuje serializator OData zawiera ten link, gdy to szereguje jednostki.</span><span class="sxs-lookup"><span data-stu-id="f86fb-196">The OData serializer includes this link when it serializes the entity.</span></span> <span data-ttu-id="f86fb-197">Jeśli akcja nie jest dostępna, funkcja zwraca `null`.</span><span class="sxs-lookup"><span data-stu-id="f86fb-197">When the action is not available, the function returns `null`.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="f86fb-198">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="f86fb-198">Additional Resources</span></span>

[<span data-ttu-id="f86fb-199">Przykład akcji OData</span><span class="sxs-lookup"><span data-stu-id="f86fb-199">OData Actions Sample</span></span>](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/OData/v3/ODataActionsSample/)
