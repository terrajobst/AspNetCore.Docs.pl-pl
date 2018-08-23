---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/odata-actions-and-functions
title: Akcje i funkcje w protokole OData v4 przy użyciu wzorca ASP.NET Web API 2.2 | Dokumentacja firmy Microsoft
author: MikeWasson
description: W protokole OData akcje i funkcje są w sposób, aby dodać zachowania po stronie serwera, które łatwo nie są zdefiniowane jako operacje CRUD na jednostkach. Ten samouczek pokazuje, jak...
ms.author: riande
ms.date: 06/27/2014
ms.assetid: 0e6fb03c-b16d-4bb0-ab0b-552bd2b6ece1
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/odata-actions-and-functions
msc.type: authoredcontent
ms.openlocfilehash: 4e61c6b4bf59792b6570e32e6d24635d4f5e5ac3
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/16/2018
ms.locfileid: "41751986"
---
<a name="actions-and-functions-in-odata-v4-using-aspnet-web-api-22"></a><span data-ttu-id="67775-104">Akcje i funkcje w protokole OData v4 przy użyciu wzorca ASP.NET Web API 2.2</span><span class="sxs-lookup"><span data-stu-id="67775-104">Actions and Functions in OData v4 Using ASP.NET Web API 2.2</span></span>
====================
<span data-ttu-id="67775-105">przez [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="67775-105">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

> <span data-ttu-id="67775-106">W protokole OData akcje i funkcje są w sposób, aby dodać zachowania po stronie serwera, które łatwo nie są zdefiniowane jako operacje CRUD na jednostkach.</span><span class="sxs-lookup"><span data-stu-id="67775-106">In OData, actions and functions are a way to add server-side behaviors that are not easily defined as CRUD operations on entities.</span></span> <span data-ttu-id="67775-107">W tym samouczku przedstawiono sposób dodawania akcji i funkcji do punktu końcowego OData v4, przy użyciu Web API 2.2.</span><span class="sxs-lookup"><span data-stu-id="67775-107">This tutorial shows how to add actions and functions to an OData v4 endpoint, using Web API 2.2.</span></span> <span data-ttu-id="67775-108">Samouczek opiera się na samouczka [tworzenia protokołu OData v4 punktu końcowego Używanie wzorca ASP.NET Web API 2](create-an-odata-v4-endpoint.md)</span><span class="sxs-lookup"><span data-stu-id="67775-108">The tutorial builds on the tutorial [Create an OData v4 Endpoint Using ASP.NET Web API 2](create-an-odata-v4-endpoint.md)</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="67775-109">Wersje oprogramowania używanego w tym samouczku</span><span class="sxs-lookup"><span data-stu-id="67775-109">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="67775-110">Składnik Web API 2.2</span><span class="sxs-lookup"><span data-stu-id="67775-110">Web API 2.2</span></span>
> - <span data-ttu-id="67775-111">Protokołu OData v4</span><span class="sxs-lookup"><span data-stu-id="67775-111">OData v4</span></span>
> - [<span data-ttu-id="67775-112">Visual Studio 2013 Update 2</span><span class="sxs-lookup"><span data-stu-id="67775-112">Visual Studio 2013 Update 2</span></span>](https://www.visualstudio.com/downloads/download-visual-studio-vs)
> - <span data-ttu-id="67775-113">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="67775-113">.NET 4.5</span></span>
> 
> 
> ## <a name="tutorial-versions"></a><span data-ttu-id="67775-114">Samouczek wersji</span><span class="sxs-lookup"><span data-stu-id="67775-114">Tutorial versions</span></span>
> 
> <span data-ttu-id="67775-115">Dla protokołu OData w wersji 3, zobacz [akcje protokołu OData w programie ASP.NET Web API 2](../odata-v3/odata-actions.md).</span><span class="sxs-lookup"><span data-stu-id="67775-115">For OData Version 3, see [OData Actions in ASP.NET Web API 2](../odata-v3/odata-actions.md).</span></span>


<span data-ttu-id="67775-116">Różnica między *akcje* i *funkcje* akcje mogą mieć skutki uboczne, a nie obsługują funkcji.</span><span class="sxs-lookup"><span data-stu-id="67775-116">The difference between *actions* and *functions* is that actions can have side effects, and functions do not.</span></span> <span data-ttu-id="67775-117">Akcje i funkcje mogą zwracać dane.</span><span class="sxs-lookup"><span data-stu-id="67775-117">Both actions and functions can return data.</span></span> <span data-ttu-id="67775-118">Do niektórych zastosowań akcje obejmują:</span><span class="sxs-lookup"><span data-stu-id="67775-118">Some uses for actions include:</span></span>

- <span data-ttu-id="67775-119">Złożone transakcji.</span><span class="sxs-lookup"><span data-stu-id="67775-119">Complex transactions.</span></span>
- <span data-ttu-id="67775-120">Manipulowanie kilka jednostek naraz.</span><span class="sxs-lookup"><span data-stu-id="67775-120">Manipulating several entities at once.</span></span>
- <span data-ttu-id="67775-121">Zezwalanie na aktualizacje tylko dla niektórych właściwości jednostki.</span><span class="sxs-lookup"><span data-stu-id="67775-121">Allowing updates only to certain properties of an entity.</span></span>
- <span data-ttu-id="67775-122">Wysyłanie danych, który nie jest jednostką.</span><span class="sxs-lookup"><span data-stu-id="67775-122">Sending data that is not an entity.</span></span>

<span data-ttu-id="67775-123">Funkcje są przydatne do zwracania informacji, które nie odpowiadają bezpośrednio do jednostki lub kolekcji.</span><span class="sxs-lookup"><span data-stu-id="67775-123">Functions are useful for returning information that does not correspond directly to an entity or collection.</span></span>

<span data-ttu-id="67775-124">Akcja (lub funkcji) można wskazać pojedynczą jednostkę lub kolekcję.</span><span class="sxs-lookup"><span data-stu-id="67775-124">An action (or function) can target a single entity or a collection.</span></span> <span data-ttu-id="67775-125">W terminologii OData jest *powiązania*.</span><span class="sxs-lookup"><span data-stu-id="67775-125">In OData terminology, this is the *binding*.</span></span> <span data-ttu-id="67775-126">Mogą też istnieć &quot;niepowiązanych&quot; akcji/funkcje, które są wywoływane jako statyczne operacje w usłudze.</span><span class="sxs-lookup"><span data-stu-id="67775-126">You can also have &quot;unbound&quot; actions/functions, which are called as static operations on the service.</span></span>

## <a name="example-adding-an-action"></a><span data-ttu-id="67775-127">Przykład: Dodawanie operacji</span><span class="sxs-lookup"><span data-stu-id="67775-127">Example: Adding an Action</span></span>

<span data-ttu-id="67775-128">Czynnością jest zdefiniowanie akcję, aby ocenić produktu.</span><span class="sxs-lookup"><span data-stu-id="67775-128">Let's define an action to rate a product.</span></span>

> [!NOTE]
> <span data-ttu-id="67775-129">Ten samouczek opiera się na samouczka [tworzenia protokołu OData v4 punktu końcowego Używanie wzorca ASP.NET Web API 2](create-an-odata-v4-endpoint.md)</span><span class="sxs-lookup"><span data-stu-id="67775-129">This tutorial builds on the tutorial [Create an OData v4 Endpoint Using ASP.NET Web API 2](create-an-odata-v4-endpoint.md)</span></span>


<span data-ttu-id="67775-130">Najpierw dodaj `ProductRating` modelu do reprezentowania klasyfikacji.</span><span class="sxs-lookup"><span data-stu-id="67775-130">First, add a `ProductRating` model to represent the ratings.</span></span>

[!code-csharp[Main](odata-actions-and-functions/samples/sample1.cs)]

<span data-ttu-id="67775-131">Również dodać **DbSet** do `ProductsContext` klasy, tak aby EF opisano tworzenie tabeli oceny w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="67775-131">Also add a **DbSet** to the `ProductsContext` class, so that EF will create a Ratings table in the database.</span></span>

[!code-csharp[Main](odata-actions-and-functions/samples/sample2.cs)]

### <a name="add-the-action-to-the-edm"></a><span data-ttu-id="67775-132">Dodawanie akcji do EDM</span><span class="sxs-lookup"><span data-stu-id="67775-132">Add the Action to the EDM</span></span>

<span data-ttu-id="67775-133">W WebApiConfig.cs Dodaj następujący kod:</span><span class="sxs-lookup"><span data-stu-id="67775-133">In WebApiConfig.cs, add the following code:</span></span>

[!code-csharp[Main](odata-actions-and-functions/samples/sample3.cs)]

<span data-ttu-id="67775-134">**EntityTypeConfiguration.Action** metoda dodaje akcję do modelu entity data model (EDM struktury).</span><span class="sxs-lookup"><span data-stu-id="67775-134">The **EntityTypeConfiguration.Action** method adds an action to the entity data model (EDM).</span></span> <span data-ttu-id="67775-135">**Parametr** metody określa typizowany parametr dla akcji.</span><span class="sxs-lookup"><span data-stu-id="67775-135">The **Parameter** method specifies a typed parameter for the action.</span></span>

<span data-ttu-id="67775-136">Ten kod ustawia też przestrzeni nazw dla EDM.</span><span class="sxs-lookup"><span data-stu-id="67775-136">This code also sets the namespace for the EDM.</span></span> <span data-ttu-id="67775-137">Przestrzeń nazw ma znaczenie, ponieważ identyfikator URI akcji zawiera nazwę akcji w pełni kwalifikowana:</span><span class="sxs-lookup"><span data-stu-id="67775-137">The namespace matters because the URI for the action includes the fully-qualified action name:</span></span>

[!code-console[Main](odata-actions-and-functions/samples/sample4.cmd)]

> [!NOTE]
> <span data-ttu-id="67775-138">W typowej konfiguracji usług IIS kropki (.) w tym adresie URL spowoduje, że usługi IIS, aby zwrócić błąd 404.</span><span class="sxs-lookup"><span data-stu-id="67775-138">In a typical IIS configuration, the dot in this URL will cause IIS to return error 404.</span></span> <span data-ttu-id="67775-139">Możesz rozwiązać ten problem, dodając następującą sekcję do pliku Web.Config:</span><span class="sxs-lookup"><span data-stu-id="67775-139">You can resolve this by adding the following section to your Web.Config file:</span></span>

[!code-xml[Main](odata-actions-and-functions/samples/sample5.xml)]

### <a name="add-a-controller-method-for-the-action"></a><span data-ttu-id="67775-140">Dodaj metodę kontrolera dla akcji</span><span class="sxs-lookup"><span data-stu-id="67775-140">Add a Controller Method for the Action</span></span>

<span data-ttu-id="67775-141">Aby włączyć &quot;współczynnik&quot; działania, dodaj następującą metodę do `ProductsController`:</span><span class="sxs-lookup"><span data-stu-id="67775-141">To enable the &quot;Rate&quot; action, add the following method to `ProductsController`:</span></span>

[!code-csharp[Main](odata-actions-and-functions/samples/sample6.cs)]

<span data-ttu-id="67775-142">Należy zauważyć, że nazwa metody odpowiada nazwie akcji.</span><span class="sxs-lookup"><span data-stu-id="67775-142">Notice that the method name matches the action name.</span></span> <span data-ttu-id="67775-143">**[HttpPost]** atrybut określa, metoda jest metodą HTTP POST.</span><span class="sxs-lookup"><span data-stu-id="67775-143">The **[HttpPost]** attribute specifies the method is an HTTP POST method.</span></span>

<span data-ttu-id="67775-144">Aby wywołać akcję, klient wysyła żądanie HTTP POST, jak pokazano poniżej:</span><span class="sxs-lookup"><span data-stu-id="67775-144">To invoke the action, the client sends an HTTP POST request like the following:</span></span>

[!code-console[Main](odata-actions-and-functions/samples/sample7.cmd)]

<span data-ttu-id="67775-145">&quot;Współczynnik&quot; akcji jest powiązany z wystąpień produktu, dlatego identyfikator URI akcji jest nazwa akcji w pełni kwalifikowaną, dołączany do jednostki identyfikatora URI.</span><span class="sxs-lookup"><span data-stu-id="67775-145">The &quot;Rate&quot; action is bound to Product instances, so the URI for the action is the fully-qualified action name appended to the entity URI.</span></span> <span data-ttu-id="67775-146">(Odwołać, możemy ustawić przestrzeni nazw EDM na &quot;ProductService&quot;, więc jest nazwa akcji z w pełni kwalifikowana &quot;ProductService.Rate&quot;.)</span><span class="sxs-lookup"><span data-stu-id="67775-146">(Recall that we set the EDM namespace to &quot;ProductService&quot;, so the fully-qualified action name is &quot;ProductService.Rate&quot;.)</span></span>

<span data-ttu-id="67775-147">Treść żądania zawiera parametry akcji jako ładunek JSON.</span><span class="sxs-lookup"><span data-stu-id="67775-147">The body of the request contains the action parameters as a JSON payload.</span></span> <span data-ttu-id="67775-148">Interfejs API sieci Web automatycznie konwertuje ładunek JSON do **ODataActionParameters** obiektu, który jest po prostu słownika wartości parametrów.</span><span class="sxs-lookup"><span data-stu-id="67775-148">Web API automatically converts the JSON payload to an **ODataActionParameters** object, which is just a dictionary of parameter values.</span></span> <span data-ttu-id="67775-149">Umożliwia dostęp do parametrów w metodzie kontrolera tego słownika.</span><span class="sxs-lookup"><span data-stu-id="67775-149">Use this dictionary to access the parameters in your controller method.</span></span>

<span data-ttu-id="67775-150">Jeśli klient wysyła parametry akcji w nieprawidłowa formatowania, wartości **ModelState.IsValid** ma wartość false.</span><span class="sxs-lookup"><span data-stu-id="67775-150">If the client sends the action parameters in the wrong format, the value of **ModelState.IsValid** is false.</span></span> <span data-ttu-id="67775-151">Sprawdź tę flagę w metodzie kontrolera i zwraca błędu, jeśli **IsValid** ma wartość false.</span><span class="sxs-lookup"><span data-stu-id="67775-151">Check this flag in your controller method and return an error if **IsValid** is false.</span></span>

[!code-csharp[Main](odata-actions-and-functions/samples/sample8.cs)]

## <a name="example-adding-a-function"></a><span data-ttu-id="67775-152">Przykład: Dodawanie funkcji</span><span class="sxs-lookup"><span data-stu-id="67775-152">Example: Adding a Function</span></span>

<span data-ttu-id="67775-153">Teraz możemy dodać funkcję OData, która zwraca iloczyn najbardziej kosztowne.</span><span class="sxs-lookup"><span data-stu-id="67775-153">Now let's add an OData function that returns the most expensive product.</span></span> <span data-ttu-id="67775-154">Jak wcześniej, pierwszym krokiem jest dodanie funkcji do EDM.</span><span class="sxs-lookup"><span data-stu-id="67775-154">As before, the first step is adding the function to the EDM.</span></span> <span data-ttu-id="67775-155">W WebApiConfig.cs Dodaj następujący kod.</span><span class="sxs-lookup"><span data-stu-id="67775-155">In WebApiConfig.cs, add the following code.</span></span>

[!code-csharp[Main](odata-actions-and-functions/samples/sample9.cs)]

<span data-ttu-id="67775-156">W takim przypadku funkcja jest powiązana z kolekcji produktów, a nie poszczególnych wystąpień produktu.</span><span class="sxs-lookup"><span data-stu-id="67775-156">In this case, the function is bound to the Products collection, rather than individual Product instances.</span></span> <span data-ttu-id="67775-157">Klienci wywołać funkcję, wysyłając żądanie GET:</span><span class="sxs-lookup"><span data-stu-id="67775-157">Clients invoke the function by sending a GET request:</span></span>

[!code-console[Main](odata-actions-and-functions/samples/sample10.cmd)]

<span data-ttu-id="67775-158">Poniżej przedstawiono metody kontrolera dla tej funkcji:</span><span class="sxs-lookup"><span data-stu-id="67775-158">Here is the controller method for this function:</span></span>

[!code-csharp[Main](odata-actions-and-functions/samples/sample11.cs)]

<span data-ttu-id="67775-159">Należy zauważyć, że nazwa metody odpowiada nazwie funkcji.</span><span class="sxs-lookup"><span data-stu-id="67775-159">Notice that the method name matches the function name.</span></span> <span data-ttu-id="67775-160">**[HttpGet]** atrybut określa, metoda jest metodą HTTP GET.</span><span class="sxs-lookup"><span data-stu-id="67775-160">The **[HttpGet]** attribute specifies the method is an HTTP GET method.</span></span>

<span data-ttu-id="67775-161">Poniżej przedstawiono odpowiedzi HTTP:</span><span class="sxs-lookup"><span data-stu-id="67775-161">Here is the HTTP response:</span></span>

[!code-console[Main](odata-actions-and-functions/samples/sample12.cmd)]

## <a name="example-adding-an-unbound-function"></a><span data-ttu-id="67775-162">Przykład: Dodawanie niepowiązanych — funkcja</span><span class="sxs-lookup"><span data-stu-id="67775-162">Example: Adding an Unbound Function</span></span>

<span data-ttu-id="67775-163">Poprzedni przykład jest funkcja powiązana z kolekcją.</span><span class="sxs-lookup"><span data-stu-id="67775-163">The previous example was a function bound to a collection.</span></span> <span data-ttu-id="67775-164">W następnym przykładzie utworzymy *niepowiązanych* funkcji.</span><span class="sxs-lookup"><span data-stu-id="67775-164">In this next example, we'll create an *unbound* function.</span></span> <span data-ttu-id="67775-165">Niezwiązane funkcje są wywoływane jako statyczne operacje w usłudze.</span><span class="sxs-lookup"><span data-stu-id="67775-165">Unbound functions are called as static operations on the service.</span></span> <span data-ttu-id="67775-166">Funkcja w tym przykładzie zwraca podatku od sprzedaży dla podanego kodu pocztowego.</span><span class="sxs-lookup"><span data-stu-id="67775-166">The function in this example will return the sales tax for a given postal code.</span></span>

<span data-ttu-id="67775-167">W pliku WebApiConfig Dodaj funkcję do EDM:</span><span class="sxs-lookup"><span data-stu-id="67775-167">In the WebApiConfig file, add the function to the EDM:</span></span>

[!code-csharp[Main](odata-actions-and-functions/samples/sample13.cs)]

<span data-ttu-id="67775-168">Należy zauważyć, że wywołania **funkcja** bezpośrednio na **element ODataModelBuilder**, a nie typu jednostki lub kolekcji.</span><span class="sxs-lookup"><span data-stu-id="67775-168">Notice that we are calling **Function** directly on the **ODataModelBuilder**, instead of the entity type or collection.</span></span> <span data-ttu-id="67775-169">Informuje konstruktora modeli, że funkcja ta jest niepowiązanych.</span><span class="sxs-lookup"><span data-stu-id="67775-169">This tells the model builder that the function is unbound.</span></span>

<span data-ttu-id="67775-170">Poniżej przedstawiono metody kontrolera, który implementuje funkcję:</span><span class="sxs-lookup"><span data-stu-id="67775-170">Here is the controller method that implements the function:</span></span>

[!code-csharp[Main](odata-actions-and-functions/samples/sample14.cs)]

<span data-ttu-id="67775-171">Nie ma znaczenia, który kontroler interfejsu API sieci Web umieścić tę metodę w.</span><span class="sxs-lookup"><span data-stu-id="67775-171">It does not matter which Web API controller you place this method in.</span></span> <span data-ttu-id="67775-172">Możesz umieścić go w `ProductsController`, lub zdefiniować osobnego kontrolera.</span><span class="sxs-lookup"><span data-stu-id="67775-172">You could put it in `ProductsController`, or define a separate controller.</span></span> <span data-ttu-id="67775-173">**[ODataRoute]** atrybut definiuje szablon identyfikatora URI dla tej funkcji.</span><span class="sxs-lookup"><span data-stu-id="67775-173">The **[ODataRoute]** attribute defines the URI template for the function.</span></span>

<span data-ttu-id="67775-174">Oto przykładowe żądanie klienta:</span><span class="sxs-lookup"><span data-stu-id="67775-174">Here is an example client request:</span></span>

[!code-console[Main](odata-actions-and-functions/samples/sample15.cmd)]

<span data-ttu-id="67775-175">Odpowiedź HTTP:</span><span class="sxs-lookup"><span data-stu-id="67775-175">The HTTP response:</span></span>

[!code-console[Main](odata-actions-and-functions/samples/sample16.cmd)]
