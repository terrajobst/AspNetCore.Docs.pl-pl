---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/odata-actions-and-functions
title: Akcje i funkcje protokołu OData v4 używanie składnika ASP.NET Web API 2.2 | Dokumentacja firmy Microsoft
author: MikeWasson
description: W protokole OData akcje i funkcje są sposobem dodania zachowania po stronie serwera, które łatwo nie są zdefiniowane jako operacje CRUD na jednostkach. Ten samouczek pokazuje, jak...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/27/2014
ms.topic: article
ms.assetid: 0e6fb03c-b16d-4bb0-ab0b-552bd2b6ece1
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/odata-actions-and-functions
msc.type: authoredcontent
ms.openlocfilehash: 532362f0c0faaaf0cb0c04726856f0497e5261b5
ms.sourcegitcommit: 6784510cfb589308c3875ccb5113eb31031766b4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/08/2018
ms.locfileid: "26566774"
---
<a name="actions-and-functions-in-odata-v4-using-aspnet-web-api-22"></a><span data-ttu-id="8db1e-104">Akcje i funkcje w wersji 4 OData przy użyciu interfejsu API 2.2 sieci Web ASP.NET</span><span class="sxs-lookup"><span data-stu-id="8db1e-104">Actions and Functions in OData v4 Using ASP.NET Web API 2.2</span></span>
====================
<span data-ttu-id="8db1e-105">przez [Wasson Jan](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="8db1e-105">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

> <span data-ttu-id="8db1e-106">W protokole OData akcje i funkcje są sposobem dodania zachowania po stronie serwera, które łatwo nie są zdefiniowane jako operacje CRUD na jednostkach.</span><span class="sxs-lookup"><span data-stu-id="8db1e-106">In OData, actions and functions are a way to add server-side behaviors that are not easily defined as CRUD operations on entities.</span></span> <span data-ttu-id="8db1e-107">W tym samouczku przedstawiono sposób dodawania akcje i funkcje na OData v4 punkt końcowy, za pomocą 2.2 interfejsu API sieci Web.</span><span class="sxs-lookup"><span data-stu-id="8db1e-107">This tutorial shows how to add actions and functions to an OData v4 endpoint, using Web API 2.2.</span></span> <span data-ttu-id="8db1e-108">Samouczek opiera się na samouczka [tworzenia dodatku Using ASP.NET Web API 2 OData v4 punktu końcowego](create-an-odata-v4-endpoint.md)</span><span class="sxs-lookup"><span data-stu-id="8db1e-108">The tutorial builds on the tutorial [Create an OData v4 Endpoint Using ASP.NET Web API 2](create-an-odata-v4-endpoint.md)</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="8db1e-109">Używane w samouczku wersje oprogramowania</span><span class="sxs-lookup"><span data-stu-id="8db1e-109">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="8db1e-110">2.2 interfejsu API sieci Web</span><span class="sxs-lookup"><span data-stu-id="8db1e-110">Web API 2.2</span></span>
> - <span data-ttu-id="8db1e-111">Protokołu OData v4</span><span class="sxs-lookup"><span data-stu-id="8db1e-111">OData v4</span></span>
> - [<span data-ttu-id="8db1e-112">Visual Studio 2013 Update 2</span><span class="sxs-lookup"><span data-stu-id="8db1e-112">Visual Studio 2013 Update 2</span></span>](https://www.visualstudio.com/downloads/download-visual-studio-vs)
> - <span data-ttu-id="8db1e-113">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="8db1e-113">.NET 4.5</span></span>
> 
> 
> ## <a name="tutorial-versions"></a><span data-ttu-id="8db1e-114">Samouczek wersji</span><span class="sxs-lookup"><span data-stu-id="8db1e-114">Tutorial versions</span></span>
> 
> <span data-ttu-id="8db1e-115">Dla OData w wersji 3, zobacz [akcji OData w ASP.NET Web API 2](../odata-v3/odata-actions.md).</span><span class="sxs-lookup"><span data-stu-id="8db1e-115">For OData Version 3, see [OData Actions in ASP.NET Web API 2](../odata-v3/odata-actions.md).</span></span>


<span data-ttu-id="8db1e-116">Różnica między *akcje* i *funkcje* akcje może mieć efekty uboczne, a nie w funkcji.</span><span class="sxs-lookup"><span data-stu-id="8db1e-116">The difference between *actions* and *functions* is that actions can have side effects, and functions do not.</span></span> <span data-ttu-id="8db1e-117">Zarówno akcje i funkcje mogą zwracać dane.</span><span class="sxs-lookup"><span data-stu-id="8db1e-117">Both actions and functions can return data.</span></span> <span data-ttu-id="8db1e-118">Niektóre zastosowania dla działania obejmują:</span><span class="sxs-lookup"><span data-stu-id="8db1e-118">Some uses for actions include:</span></span>

- <span data-ttu-id="8db1e-119">Złożone transakcje.</span><span class="sxs-lookup"><span data-stu-id="8db1e-119">Complex transactions.</span></span>
- <span data-ttu-id="8db1e-120">Manipulowanie jednocześnie kilka jednostek.</span><span class="sxs-lookup"><span data-stu-id="8db1e-120">Manipulating several entities at once.</span></span>
- <span data-ttu-id="8db1e-121">Stosowanie aktualizacji tylko do niektórych właściwości jednostki.</span><span class="sxs-lookup"><span data-stu-id="8db1e-121">Allowing updates only to certain properties of an entity.</span></span>
- <span data-ttu-id="8db1e-122">Wysyłanie danych, która nie jest jednostką.</span><span class="sxs-lookup"><span data-stu-id="8db1e-122">Sending data that is not an entity.</span></span>

<span data-ttu-id="8db1e-123">Funkcje są przydatne do zwracania informacji, który nie odpowiada bezpośrednio do jednostki lub kolekcji.</span><span class="sxs-lookup"><span data-stu-id="8db1e-123">Functions are useful for returning information that does not correspond directly to an entity or collection.</span></span>

<span data-ttu-id="8db1e-124">Akcja (lub funkcji) celem może być pojedynczą jednostką lub kolekcji.</span><span class="sxs-lookup"><span data-stu-id="8db1e-124">An action (or function) can target a single entity or a collection.</span></span> <span data-ttu-id="8db1e-125">W terminologii OData jest *powiązania*.</span><span class="sxs-lookup"><span data-stu-id="8db1e-125">In OData terminology, this is the *binding*.</span></span> <span data-ttu-id="8db1e-126">Może także zawierać &quot;niezwiązanego&quot; akcje/funkcje, które są nazywane jako statyczne operacji dla usługi.</span><span class="sxs-lookup"><span data-stu-id="8db1e-126">You can also have &quot;unbound&quot; actions/functions, which are called as static operations on the service.</span></span>

## <a name="example-adding-an-action"></a><span data-ttu-id="8db1e-127">Przykład: Dodawanie operacji</span><span class="sxs-lookup"><span data-stu-id="8db1e-127">Example: Adding an Action</span></span>

<span data-ttu-id="8db1e-128">Umożliwia zdefiniowanie akcji, aby ocenić produktu.</span><span class="sxs-lookup"><span data-stu-id="8db1e-128">Let's define an action to rate a product.</span></span>

> [!NOTE]
> <span data-ttu-id="8db1e-129">W tym samouczku opiera się na samouczka [tworzenia dodatku Using ASP.NET Web API 2 OData v4 punktu końcowego](create-an-odata-v4-endpoint.md)</span><span class="sxs-lookup"><span data-stu-id="8db1e-129">This tutorial builds on the tutorial [Create an OData v4 Endpoint Using ASP.NET Web API 2](create-an-odata-v4-endpoint.md)</span></span>


<span data-ttu-id="8db1e-130">Najpierw dodaj `ProductRating` modelu do reprezentowania klasyfikacji.</span><span class="sxs-lookup"><span data-stu-id="8db1e-130">First, add a `ProductRating` model to represent the ratings.</span></span>

[!code-csharp[Main](odata-actions-and-functions/samples/sample1.cs)]

<span data-ttu-id="8db1e-131">Dodaj również **DbSet** do `ProductsContext` klasy, tak aby EF spowoduje utworzenie tabeli klasyfikacje w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="8db1e-131">Also add a **DbSet** to the `ProductsContext` class, so that EF will create a Ratings table in the database.</span></span>

[!code-csharp[Main](odata-actions-and-functions/samples/sample2.cs)]

### <a name="add-the-action-to-the-edm"></a><span data-ttu-id="8db1e-132">Dodaj akcję do EDM</span><span class="sxs-lookup"><span data-stu-id="8db1e-132">Add the Action to the EDM</span></span>

<span data-ttu-id="8db1e-133">W WebApiConfig.cs Dodaj następujący kod:</span><span class="sxs-lookup"><span data-stu-id="8db1e-133">In WebApiConfig.cs, add the following code:</span></span>

[!code-csharp[Main](odata-actions-and-functions/samples/sample3.cs)]

<span data-ttu-id="8db1e-134">**EntityTypeConfiguration.Action** metody Dodawanie akcji do modelu entity data model (EDM).</span><span class="sxs-lookup"><span data-stu-id="8db1e-134">The **EntityTypeConfiguration.Action** method adds an action to the entity data model (EDM).</span></span> <span data-ttu-id="8db1e-135">**Parametr** metody określa parametr określonego dla akcji.</span><span class="sxs-lookup"><span data-stu-id="8db1e-135">The **Parameter** method specifies a typed parameter for the action.</span></span>

<span data-ttu-id="8db1e-136">Ten kod także ustawia obszar nazw dla EDM.</span><span class="sxs-lookup"><span data-stu-id="8db1e-136">This code also sets the namespace for the EDM.</span></span> <span data-ttu-id="8db1e-137">Przestrzeń nazw jest ważna, ponieważ akcji w pełni kwalifikowana nazwa zawiera identyfikator URI dla akcji:</span><span class="sxs-lookup"><span data-stu-id="8db1e-137">The namespace matters because the URI for the action includes the fully-qualified action name:</span></span>

[!code-console[Main](odata-actions-and-functions/samples/sample4.cmd)]

> [!NOTE]
> <span data-ttu-id="8db1e-138">W typowej konfiguracji usług IIS kropki (.) w tym adresem URL spowoduje, że usługi IIS zwracają błąd 404.</span><span class="sxs-lookup"><span data-stu-id="8db1e-138">In a typical IIS configuration, the dot in this URL will cause IIS to return error 404.</span></span> <span data-ttu-id="8db1e-139">Można go rozwiązać, dodając następującą sekcję do pliku Web.Config:</span><span class="sxs-lookup"><span data-stu-id="8db1e-139">You can resolve this by adding the following section to your Web.Config file:</span></span>

[!code-xml[Main](odata-actions-and-functions/samples/sample5.xml)]

### <a name="add-a-controller-method-for-the-action"></a><span data-ttu-id="8db1e-140">Dodaj metodę kontrolera dla akcji</span><span class="sxs-lookup"><span data-stu-id="8db1e-140">Add a Controller Method for the Action</span></span>

<span data-ttu-id="8db1e-141">Aby włączyć &quot;szybkość&quot; akcji, dodaj następującą metodę do `ProductsController`:</span><span class="sxs-lookup"><span data-stu-id="8db1e-141">To enable the &quot;Rate&quot; action, add the following method to `ProductsController`:</span></span>

[!code-csharp[Main](odata-actions-and-functions/samples/sample6.cs)]

<span data-ttu-id="8db1e-142">Zwróć uwagę, czy nazwa metody jest zgodna z nazwą akcji.</span><span class="sxs-lookup"><span data-stu-id="8db1e-142">Notice that the method name matches the action name.</span></span> <span data-ttu-id="8db1e-143">**[HttpPost]** atrybut określa metoda jest metodą HTTP POST.</span><span class="sxs-lookup"><span data-stu-id="8db1e-143">The **[HttpPost]** attribute specifies the method is an HTTP POST method.</span></span>

<span data-ttu-id="8db1e-144">Aby wywołać akcję, klient wysyła żądanie HTTP POST, takie jak następujące:</span><span class="sxs-lookup"><span data-stu-id="8db1e-144">To invoke the action, the client sends an HTTP POST request like the following:</span></span>

[!code-console[Main](odata-actions-and-functions/samples/sample7.cmd)]

<span data-ttu-id="8db1e-145">&quot;Szybkość&quot; akcji jest powiązana z wystąpieniami produktu, więc identyfikator URI dla akcji jest nazwa akcji pełną dołączany do jednostki identyfikatora URI.</span><span class="sxs-lookup"><span data-stu-id="8db1e-145">The &quot;Rate&quot; action is bound to Product instances, so the URI for the action is the fully-qualified action name appended to the entity URI.</span></span> <span data-ttu-id="8db1e-146">(Odwołać możemy ustawić przestrzeni nazw EDM &quot;ProductService&quot;, więc akcji w pełni kwalifikowana nazwa jest &quot;ProductService.Rate&quot;.)</span><span class="sxs-lookup"><span data-stu-id="8db1e-146">(Recall that we set the EDM namespace to &quot;ProductService&quot;, so the fully-qualified action name is &quot;ProductService.Rate&quot;.)</span></span>

<span data-ttu-id="8db1e-147">Treść żądania zawiera parametry akcji jako ładunek JSON.</span><span class="sxs-lookup"><span data-stu-id="8db1e-147">The body of the request contains the action parameters as a JSON payload.</span></span> <span data-ttu-id="8db1e-148">Interfejs API sieci Web automatycznie konwertuje ładunek JSON do **ODataActionParameters** obiektu, który jest ze słownika wartości parametrów.</span><span class="sxs-lookup"><span data-stu-id="8db1e-148">Web API automatically converts the JSON payload to an **ODataActionParameters** object, which is just a dictionary of parameter values.</span></span> <span data-ttu-id="8db1e-149">Umożliwia dostęp do parametrów w metodę kontrolera tego słownika.</span><span class="sxs-lookup"><span data-stu-id="8db1e-149">Use this dictionary to access the parameters in your controller method.</span></span>

<span data-ttu-id="8db1e-150">Jeśli klient wysyła parametry akcji w niewłaściwy format, wartość **ModelState.IsValid** ma wartość false.</span><span class="sxs-lookup"><span data-stu-id="8db1e-150">If the client sends the action parameters in the wrong format, the value of **ModelState.IsValid** is false.</span></span> <span data-ttu-id="8db1e-151">Sprawdź tę flagę w metodę kontrolera i zwraca błąd, jeśli **IsValid** ma wartość false.</span><span class="sxs-lookup"><span data-stu-id="8db1e-151">Check this flag in your controller method and return an error if **IsValid** is false.</span></span>

[!code-csharp[Main](odata-actions-and-functions/samples/sample8.cs)]

## <a name="example-adding-a-function"></a><span data-ttu-id="8db1e-152">Przykład: Dodawanie funkcji</span><span class="sxs-lookup"><span data-stu-id="8db1e-152">Example: Adding a Function</span></span>

<span data-ttu-id="8db1e-153">Teraz możemy dodać funkcję OData, która zwraca najbardziej kosztownych produktu.</span><span class="sxs-lookup"><span data-stu-id="8db1e-153">Now let's add an OData function that returns the most expensive product.</span></span> <span data-ttu-id="8db1e-154">Ponieważ przed, pierwszym krokiem jest dodanie funkcji do EDM.</span><span class="sxs-lookup"><span data-stu-id="8db1e-154">As before, the first step is adding the function to the EDM.</span></span> <span data-ttu-id="8db1e-155">W WebApiConfig.cs Dodaj następujący kod.</span><span class="sxs-lookup"><span data-stu-id="8db1e-155">In WebApiConfig.cs, add the following code.</span></span>

[!code-csharp[Main](odata-actions-and-functions/samples/sample9.cs)]

<span data-ttu-id="8db1e-156">W takim przypadku funkcja jest powiązana z kolekcji produktów, a nie poszczególnych wystąpień produktu.</span><span class="sxs-lookup"><span data-stu-id="8db1e-156">In this case, the function is bound to the Products collection, rather than individual Product instances.</span></span> <span data-ttu-id="8db1e-157">Klienci wywołanie funkcji, wysyłając żądanie GET:</span><span class="sxs-lookup"><span data-stu-id="8db1e-157">Clients invoke the function by sending a GET request:</span></span>

[!code-console[Main](odata-actions-and-functions/samples/sample10.cmd)]

<span data-ttu-id="8db1e-158">Oto metoda kontrolera dla tej funkcji:</span><span class="sxs-lookup"><span data-stu-id="8db1e-158">Here is the controller method for this function:</span></span>

[!code-csharp[Main](odata-actions-and-functions/samples/sample11.cs)]

<span data-ttu-id="8db1e-159">Zwróć uwagę, czy nazwa metody jest zgodna z nazwą funkcji.</span><span class="sxs-lookup"><span data-stu-id="8db1e-159">Notice that the method name matches the function name.</span></span> <span data-ttu-id="8db1e-160">**[HttpGet]** atrybut określa metoda jest metodą HTTP GET.</span><span class="sxs-lookup"><span data-stu-id="8db1e-160">The **[HttpGet]** attribute specifies the method is an HTTP GET method.</span></span>

<span data-ttu-id="8db1e-161">Poniżej przedstawiono odpowiedzi HTTP:</span><span class="sxs-lookup"><span data-stu-id="8db1e-161">Here is the HTTP response:</span></span>

[!code-console[Main](odata-actions-and-functions/samples/sample12.cmd)]

## <a name="example-adding-an-unbound-function"></a><span data-ttu-id="8db1e-162">Przykład: Dodawanie niepowiązanych — funkcja</span><span class="sxs-lookup"><span data-stu-id="8db1e-162">Example: Adding an Unbound Function</span></span>

<span data-ttu-id="8db1e-163">Poprzedni przykład była funkcja powiązane z kolekcją.</span><span class="sxs-lookup"><span data-stu-id="8db1e-163">The previous example was a function bound to a collection.</span></span> <span data-ttu-id="8db1e-164">W tym przykładzie dalej utworzymy *niezwiązanego* funkcji.</span><span class="sxs-lookup"><span data-stu-id="8db1e-164">In this next example, we'll create an *unbound* function.</span></span> <span data-ttu-id="8db1e-165">Niezwiązane funkcje są nazywane jako statyczne operacji dla usługi.</span><span class="sxs-lookup"><span data-stu-id="8db1e-165">Unbound functions are called as static operations on the service.</span></span> <span data-ttu-id="8db1e-166">Funkcja w tym przykładzie zwraca podatek dla danego kod pocztowy.</span><span class="sxs-lookup"><span data-stu-id="8db1e-166">The function in this example will return the sales tax for a given postal code.</span></span>

<span data-ttu-id="8db1e-167">W pliku WebApiConfig należy dodać funkcję do EDM:</span><span class="sxs-lookup"><span data-stu-id="8db1e-167">In the WebApiConfig file, add the function to the EDM:</span></span>

[!code-csharp[Main](odata-actions-and-functions/samples/sample13.cs)]

<span data-ttu-id="8db1e-168">Należy zauważyć, że wywołania **funkcja** bezpośrednio na **element ODataModelBuilder**, zamiast typu jednostki lub kolekcji.</span><span class="sxs-lookup"><span data-stu-id="8db1e-168">Notice that we are calling **Function** directly on the **ODataModelBuilder**, instead of the entity type or collection.</span></span> <span data-ttu-id="8db1e-169">Informuje konstruktora modeli, czy funkcja jest niepowiązanych.</span><span class="sxs-lookup"><span data-stu-id="8db1e-169">This tells the model builder that the function is unbound.</span></span>

<span data-ttu-id="8db1e-170">Oto metoda kontrolera, który implementuje funkcję:</span><span class="sxs-lookup"><span data-stu-id="8db1e-170">Here is the controller method that implements the function:</span></span>

[!code-csharp[Main](odata-actions-and-functions/samples/sample14.cs)]

<span data-ttu-id="8db1e-171">Nie ma znaczenia, który można umieścić tę metodę w kontroler interfejsu API sieci Web.</span><span class="sxs-lookup"><span data-stu-id="8db1e-171">It does not matter which Web API controller you place this method in.</span></span> <span data-ttu-id="8db1e-172">Można go umieścić `ProductsController`, lub zdefiniuj osobnego kontrolera.</span><span class="sxs-lookup"><span data-stu-id="8db1e-172">You could put it in `ProductsController`, or define a separate controller.</span></span> <span data-ttu-id="8db1e-173">**[ODataRoute]** atrybut określa szablon identyfikatora URI dla funkcji.</span><span class="sxs-lookup"><span data-stu-id="8db1e-173">The **[ODataRoute]** attribute defines the URI template for the function.</span></span>

<span data-ttu-id="8db1e-174">Poniżej przedstawiono przykładowe żądanie klienta:</span><span class="sxs-lookup"><span data-stu-id="8db1e-174">Here is an example client request:</span></span>

[!code-console[Main](odata-actions-and-functions/samples/sample15.cmd)]

<span data-ttu-id="8db1e-175">Odpowiedź HTTP:</span><span class="sxs-lookup"><span data-stu-id="8db1e-175">The HTTP response:</span></span>

[!code-console[Main](odata-actions-and-functions/samples/sample16.cmd)]
