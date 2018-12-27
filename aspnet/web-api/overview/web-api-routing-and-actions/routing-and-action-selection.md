---
uid: web-api/overview/web-api-routing-and-actions/routing-and-action-selection
title: Routing i wybieranie akcji w interfejsie Web API platformy ASP.NET | Dokumentacja firmy Microsoft
author: MikeWasson
description: ''
ms.author: riande
ms.date: 12/14/2018
ms.assetid: bcf2d223-cb7f-411e-be05-f43e96a14015
msc.legacyurl: /web-api/overview/web-api-routing-and-actions/routing-and-action-selection
msc.type: authoredcontent
ms.openlocfilehash: ce54181996376cb5dde3b91c10c16f33b3c6a570
ms.sourcegitcommit: 6548c19f345850ee22b50f7ef9fca732895d9e08
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/14/2018
ms.locfileid: "53425175"
---
<a name="routing-and-action-selection-in-aspnet-web-api"></a><span data-ttu-id="b87ee-102">Routing i wybieranie akcji we wzorcu ASP.NET Web API</span><span class="sxs-lookup"><span data-stu-id="b87ee-102">Routing and Action Selection in ASP.NET Web API</span></span>
====================
<span data-ttu-id="b87ee-103">przez [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="b87ee-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="b87ee-104">W tym artykule opisano, jak ASP.NET Web API kieruje żądania HTTP do określonej akcji w kontrolerze.</span><span class="sxs-lookup"><span data-stu-id="b87ee-104">This article describes how ASP.NET Web API routes an HTTP request to a particular action on a controller.</span></span>

> [!NOTE]
> <span data-ttu-id="b87ee-105">Aby uzyskać ogólne omówienie routingu, zobacz [routingu ASP.NET Web API](routing-in-aspnet-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="b87ee-105">For a high-level overview of routing, see [Routing in ASP.NET Web API](routing-in-aspnet-web-api.md).</span></span>


<span data-ttu-id="b87ee-106">W tym artykule przedstawiono szczegółowych informacji dotyczących procesu routingu.</span><span class="sxs-lookup"><span data-stu-id="b87ee-106">This article looks at the details of the routing process.</span></span> <span data-ttu-id="b87ee-107">Jeśli podczas tworzenia projektu składnika Web API okaże się, że niektóre żądania nie uzyskasz kierowane sposób, w których oczekujesz, miejmy nadzieję ten artykuł pomoże.</span><span class="sxs-lookup"><span data-stu-id="b87ee-107">If you create a Web API project and find that some requests don't get routed the way you expect, hopefully this article will help.</span></span>

<span data-ttu-id="b87ee-108">Routing obejmuje trzy główne etapy:</span><span class="sxs-lookup"><span data-stu-id="b87ee-108">Routing has three main phases:</span></span>

1. <span data-ttu-id="b87ee-109">Dopasowywania identyfikatora URI do szablonu trasy.</span><span class="sxs-lookup"><span data-stu-id="b87ee-109">Matching the URI to a route template.</span></span>
2. <span data-ttu-id="b87ee-110">Wybiera kontroler.</span><span class="sxs-lookup"><span data-stu-id="b87ee-110">Selecting a controller.</span></span>
3. <span data-ttu-id="b87ee-111">Wybieranie akcji.</span><span class="sxs-lookup"><span data-stu-id="b87ee-111">Selecting an action.</span></span>

<span data-ttu-id="b87ee-112">Możesz zastąpić niektóre części procesu własne niestandardowe zachowania.</span><span class="sxs-lookup"><span data-stu-id="b87ee-112">You can replace some parts of the process with your own custom behaviors.</span></span> <span data-ttu-id="b87ee-113">W tym artykule I opisano domyślne zachowanie.</span><span class="sxs-lookup"><span data-stu-id="b87ee-113">In this article, I describe the default behavior.</span></span> <span data-ttu-id="b87ee-114">Na koniec czy należy pamiętać, miejsca, w którym można dostosować zachowanie.</span><span class="sxs-lookup"><span data-stu-id="b87ee-114">At the end, I note the places where you can customize the behavior.</span></span>

## <a name="route-templates"></a><span data-ttu-id="b87ee-115">Szablonów tras</span><span class="sxs-lookup"><span data-stu-id="b87ee-115">Route Templates</span></span>

<span data-ttu-id="b87ee-116">Szablon trasy wygląda podobnie do ścieżki identyfikatora URI, ale może mieć wartości symboli zastępczych, wskazane w nawiasach klamrowych:</span><span class="sxs-lookup"><span data-stu-id="b87ee-116">A route template looks similar to a URI path, but it can have placeholder values, indicated with curly braces:</span></span>

[!code-csharp[Main](routing-and-action-selection/samples/sample1.ps1)]

<span data-ttu-id="b87ee-117">Podczas tworzenia trasy, możesz podać wartości domyślne, niektóre lub wszystkie symbole zastępcze:</span><span class="sxs-lookup"><span data-stu-id="b87ee-117">When you create a route, you can provide default values for some or all of the placeholders:</span></span>

[!code-csharp[Main](routing-and-action-selection/samples/sample2.cs)]

<span data-ttu-id="b87ee-118">Możesz też podać ograniczenia, które ograniczają sposób segmentem identyfikatora URI może odnosić się do symbolu zastępczego:</span><span class="sxs-lookup"><span data-stu-id="b87ee-118">You can also provide constraints, which restrict how a URI segment can match a placeholder:</span></span>

[!code-csharp[Main](routing-and-action-selection/samples/sample3.js)]

<span data-ttu-id="b87ee-119">Struktura próbuje dopasować segmenty ścieżki identyfikatora URI do szablonu.</span><span class="sxs-lookup"><span data-stu-id="b87ee-119">The framework tries to match the segments in the URI path to the template.</span></span> <span data-ttu-id="b87ee-120">Literały w szablonie musi dokładnie pasować.</span><span class="sxs-lookup"><span data-stu-id="b87ee-120">Literals in the template must match exactly.</span></span> <span data-ttu-id="b87ee-121">Symbol zastępczy pasuje do dowolnej wartości, chyba że określono ograniczenia.</span><span class="sxs-lookup"><span data-stu-id="b87ee-121">A placeholder matches any value, unless you specify constraints.</span></span> <span data-ttu-id="b87ee-122">Struktura jest niezgodna z innych części identyfikatora URI, takich jak nazwa hosta lub parametrów zapytań.</span><span class="sxs-lookup"><span data-stu-id="b87ee-122">The framework does not match other parts of the URI, such as the host name or the query parameters.</span></span> <span data-ttu-id="b87ee-123">Struktura wybiera pierwszy trasy w tabeli tras, która pasuje do identyfikatora URI.</span><span class="sxs-lookup"><span data-stu-id="b87ee-123">The framework selects the first route in the route table that matches the URI.</span></span>

<span data-ttu-id="b87ee-124">Istnieją dwa specjalne symbole zastępcze: "{controller}" i "{action}".</span><span class="sxs-lookup"><span data-stu-id="b87ee-124">There are two special placeholders: "{controller}" and "{action}".</span></span>

- <span data-ttu-id="b87ee-125">"{controller}" zawiera nazwę kontrolera.</span><span class="sxs-lookup"><span data-stu-id="b87ee-125">"{controller}" provides the name of the controller.</span></span>
- <span data-ttu-id="b87ee-126">"{action}" zawiera nazwę akcji.</span><span class="sxs-lookup"><span data-stu-id="b87ee-126">"{action}" provides the name of the action.</span></span> <span data-ttu-id="b87ee-127">W interfejsie API sieci Web standardowej konwencji jest pominięcie "{action}".</span><span class="sxs-lookup"><span data-stu-id="b87ee-127">In Web API, the usual convention is to omit "{action}".</span></span>

### <a name="defaults"></a><span data-ttu-id="b87ee-128">Wartość domyślna</span><span class="sxs-lookup"><span data-stu-id="b87ee-128">Defaults</span></span>

<span data-ttu-id="b87ee-129">Jeśli podano wartości domyślne trasy będą zgodne identyfikator URI, który nie ma te segmenty.</span><span class="sxs-lookup"><span data-stu-id="b87ee-129">If you provide defaults, the route will match a URI that is missing those segments.</span></span> <span data-ttu-id="b87ee-130">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="b87ee-130">For example:</span></span>

[!code-csharp[Main](routing-and-action-selection/samples/sample4.cs)]

<span data-ttu-id="b87ee-131">Identyfikatory URI `http://localhost/api/products/all` i `http://localhost/api/products` dopasować poprzedni trasy.</span><span class="sxs-lookup"><span data-stu-id="b87ee-131">The URIs `http://localhost/api/products/all` and `http://localhost/api/products` match the preceding route.</span></span> <span data-ttu-id="b87ee-132">W ostatnim identyfikatorze URI brakujący `{category}` segmentu jest przypisywana wartość domyślną `all`.</span><span class="sxs-lookup"><span data-stu-id="b87ee-132">In the latter URI, the missing `{category}` segment is assigned the default value `all`.</span></span>

### <a name="route-dictionary"></a><span data-ttu-id="b87ee-133">Słownika trasy</span><span class="sxs-lookup"><span data-stu-id="b87ee-133">Route Dictionary</span></span>

<span data-ttu-id="b87ee-134">Jeśli struktura znajdzie dopasowanie dla identyfikatora URI, tworzy słownik, który zawiera wartość dla każdego symbolu zastępczego.</span><span class="sxs-lookup"><span data-stu-id="b87ee-134">If the framework finds a match for a URI, it creates a dictionary that contains the value for each placeholder.</span></span> <span data-ttu-id="b87ee-135">Klucze są nazw zastępczych, nie wliczając nawiasów klamrowych.</span><span class="sxs-lookup"><span data-stu-id="b87ee-135">The keys are the placeholder names, not including the curly braces.</span></span> <span data-ttu-id="b87ee-136">Wartości te są pobierane z składnik path identyfikatora URI lub wartości domyślne.</span><span class="sxs-lookup"><span data-stu-id="b87ee-136">The values are taken from the URI path or from the defaults.</span></span> <span data-ttu-id="b87ee-137">Słownik jest przechowywany w **IHttpRouteData** obiektu.</span><span class="sxs-lookup"><span data-stu-id="b87ee-137">The dictionary is stored in the **IHttpRouteData** object.</span></span>

<span data-ttu-id="b87ee-138">W tej fazie dopasowania trasy specjalnego "{controller}" i symbole zastępcze "{action}" są traktowane tak samo jak inne symbole zastępcze.</span><span class="sxs-lookup"><span data-stu-id="b87ee-138">During this route-matching phase, the special "{controller}" and "{action}" placeholders are treated just like the other placeholders.</span></span> <span data-ttu-id="b87ee-139">Po prostu są one przechowywane w słowniku inne wartości.</span><span class="sxs-lookup"><span data-stu-id="b87ee-139">They are simply stored in the dictionary with the other values.</span></span>

<span data-ttu-id="b87ee-140">Domyślny może mieć specjalna wartość **RouteParameter.Optional**.</span><span class="sxs-lookup"><span data-stu-id="b87ee-140">A default can have the special value **RouteParameter.Optional**.</span></span> <span data-ttu-id="b87ee-141">Jeśli symbol zastępczy, pobiera przypisaną tę wartość, wartość nie zostanie dodany do słownika trasy.</span><span class="sxs-lookup"><span data-stu-id="b87ee-141">If a placeholder gets assigned this value, the value is not added to the route dictionary.</span></span> <span data-ttu-id="b87ee-142">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="b87ee-142">For example:</span></span>

[!code-csharp[Main](routing-and-action-selection/samples/sample5.cs)]

<span data-ttu-id="b87ee-143">Dla ścieżki identyfikatora URI "interfejsu api/produkty" będzie zawierać słownika trasy:</span><span class="sxs-lookup"><span data-stu-id="b87ee-143">For the URI path "api/products", the route dictionary will contain:</span></span>

- <span data-ttu-id="b87ee-144">Kontroler: "produkty"</span><span class="sxs-lookup"><span data-stu-id="b87ee-144">controller: "products"</span></span>
- <span data-ttu-id="b87ee-145">Kategoria: "all"</span><span class="sxs-lookup"><span data-stu-id="b87ee-145">category: "all"</span></span>

<span data-ttu-id="b87ee-146">Dla "interfejsu api/produkty/zabawki/123" jednak słownika trasy będzie zawierać:</span><span class="sxs-lookup"><span data-stu-id="b87ee-146">For "api/products/toys/123", however, the route dictionary will contain:</span></span>

- <span data-ttu-id="b87ee-147">Kontroler: "produkty"</span><span class="sxs-lookup"><span data-stu-id="b87ee-147">controller: "products"</span></span>
- <span data-ttu-id="b87ee-148">Kategoria: "zabawki"</span><span class="sxs-lookup"><span data-stu-id="b87ee-148">category: "toys"</span></span>
- <span data-ttu-id="b87ee-149">Identyfikator: "123"</span><span class="sxs-lookup"><span data-stu-id="b87ee-149">id: "123"</span></span>

<span data-ttu-id="b87ee-150">Wartości domyślne mogą również zawierać wartość, która nie występować w dowolnym miejscu w szablonie trasy.</span><span class="sxs-lookup"><span data-stu-id="b87ee-150">The defaults can also include a value that does not appear anywhere in the route template.</span></span> <span data-ttu-id="b87ee-151">Jeśli trasa odpowiada, ta wartość jest przechowywany w słowniku.</span><span class="sxs-lookup"><span data-stu-id="b87ee-151">If the route matches, that value is stored in the dictionary.</span></span> <span data-ttu-id="b87ee-152">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="b87ee-152">For example:</span></span>

[!code-csharp[Main](routing-and-action-selection/samples/sample6.cs)]

<span data-ttu-id="b87ee-153">Jeśli ścieżka identyfikatora URI to "interfejsu api/główny/8", słownik będzie zawierać dwie wartości:</span><span class="sxs-lookup"><span data-stu-id="b87ee-153">If the URI path is "api/root/8", the dictionary will contain two values:</span></span>

- <span data-ttu-id="b87ee-154">Kontroler: "klientów"</span><span class="sxs-lookup"><span data-stu-id="b87ee-154">controller: "customers"</span></span>
- <span data-ttu-id="b87ee-155">Identyfikator: "8"</span><span class="sxs-lookup"><span data-stu-id="b87ee-155">id: "8"</span></span>

## <a name="selecting-a-controller"></a><span data-ttu-id="b87ee-156">Wybiera kontroler</span><span class="sxs-lookup"><span data-stu-id="b87ee-156">Selecting a Controller</span></span>

<span data-ttu-id="b87ee-157">Wybieranie kontrolera jest obsługiwany przez **IHttpControllerSelector.SelectController** metody.</span><span class="sxs-lookup"><span data-stu-id="b87ee-157">Controller selection is handled by the **IHttpControllerSelector.SelectController** method.</span></span> <span data-ttu-id="b87ee-158">Ta metoda przyjmuje **HttpRequestMessage** wystąpienie i zwraca **HttpControllerDescriptor**.</span><span class="sxs-lookup"><span data-stu-id="b87ee-158">This method takes an **HttpRequestMessage** instance and returns an **HttpControllerDescriptor**.</span></span> <span data-ttu-id="b87ee-159">Domyślna implementacja jest dostarczany przez **DefaultHttpControllerSelector** klasy.</span><span class="sxs-lookup"><span data-stu-id="b87ee-159">The default implementation is provided by the **DefaultHttpControllerSelector** class.</span></span> <span data-ttu-id="b87ee-160">Ta klasa używa algorytmu proste:</span><span class="sxs-lookup"><span data-stu-id="b87ee-160">This class uses a straightforward algorithm:</span></span>

1. <span data-ttu-id="b87ee-161">Znajdź słownika trasy dla klucza "controller".</span><span class="sxs-lookup"><span data-stu-id="b87ee-161">Look in the route dictionary for the key "controller".</span></span>
2. <span data-ttu-id="b87ee-162">Pobrać wartość dla tego klucza i Dołącz ciąg "Controller" w celu otrzymania nazwy typu kontrolera.</span><span class="sxs-lookup"><span data-stu-id="b87ee-162">Take the value for this key and append the string "Controller" to get the controller type name.</span></span>
3. <span data-ttu-id="b87ee-163">Zwróć uwagę na kontrolerze interfejsu API sieci Web o tej nazwie typu.</span><span class="sxs-lookup"><span data-stu-id="b87ee-163">Look for a Web API controller with this type name.</span></span>

<span data-ttu-id="b87ee-164">Na przykład jeśli słownika trasy zawiera pary klucz wartość "controller" = "produkty", a następnie typ kontrolera jest "ProductsController".</span><span class="sxs-lookup"><span data-stu-id="b87ee-164">For example, if the route dictionary contains the key-value pair "controller" = "products", then the controller type is "ProductsController".</span></span> <span data-ttu-id="b87ee-165">Jeśli określono żadnego typu zgodnego lub wiele dopasowań, struktura zwraca błąd do klienta.</span><span class="sxs-lookup"><span data-stu-id="b87ee-165">If there is no matching type, or multiple matches, the framework returns an error to the client.</span></span>

<span data-ttu-id="b87ee-166">W kroku 3 **DefaultHttpControllerSelector** używa **IHttpControllerTypeResolver** interfejsu, aby uzyskać listę typów kontrolera interfejsu API sieci Web.</span><span class="sxs-lookup"><span data-stu-id="b87ee-166">For step 3, **DefaultHttpControllerSelector** uses the **IHttpControllerTypeResolver** interface to get the list of Web API controller types.</span></span> <span data-ttu-id="b87ee-167">Domyślna implementacja klasy **IHttpControllerTypeResolver** zwraca wszystkie klasy publiczne, które implementują () **IHttpController**, (b) jest abstrakcyjna i (c) mają nazwę, która kończy się na "Controller".</span><span class="sxs-lookup"><span data-stu-id="b87ee-167">The default implementation of **IHttpControllerTypeResolver** returns all public classes that (a) implement **IHttpController**, (b) are not abstract, and (c) have a name that ends in "Controller".</span></span>

## <a name="action-selection"></a><span data-ttu-id="b87ee-168">Wybór akcji</span><span class="sxs-lookup"><span data-stu-id="b87ee-168">Action Selection</span></span>

<span data-ttu-id="b87ee-169">Po wybraniu kontrolera, struktura wybiera akcję, wywołując **IHttpActionSelector.SelectAction** metody.</span><span class="sxs-lookup"><span data-stu-id="b87ee-169">After selecting the controller, the framework selects the action by calling the **IHttpActionSelector.SelectAction** method.</span></span> <span data-ttu-id="b87ee-170">Ta metoda przyjmuje **HttpControllerContext** i zwraca **HttpActionDescriptor**.</span><span class="sxs-lookup"><span data-stu-id="b87ee-170">This method takes an **HttpControllerContext** and returns an **HttpActionDescriptor**.</span></span>

<span data-ttu-id="b87ee-171">Domyślna implementacja jest dostarczany przez **ApiControllerActionSelector** klasy.</span><span class="sxs-lookup"><span data-stu-id="b87ee-171">The default implementation is provided by the **ApiControllerActionSelector** class.</span></span> <span data-ttu-id="b87ee-172">Aby wybrać akcję, odbywa się na następujących czynności:</span><span class="sxs-lookup"><span data-stu-id="b87ee-172">To select an action, it looks at the following:</span></span>

- <span data-ttu-id="b87ee-173">Metoda HTTP żądania.</span><span class="sxs-lookup"><span data-stu-id="b87ee-173">The HTTP method of the request.</span></span>
- <span data-ttu-id="b87ee-174">Symbol zastępczy "{action}" w szablonie trasy, jeśli jest obecny.</span><span class="sxs-lookup"><span data-stu-id="b87ee-174">The "{action}" placeholder in the route template, if present.</span></span>
- <span data-ttu-id="b87ee-175">Parametry akcji w kontrolerze.</span><span class="sxs-lookup"><span data-stu-id="b87ee-175">The parameters of the actions on the controller.</span></span>

<span data-ttu-id="b87ee-176">Przed obejrzeniem algorytm wybór, należy zrozumieć kilka rzeczy, o akcji kontrolera.</span><span class="sxs-lookup"><span data-stu-id="b87ee-176">Before looking at the selection algorithm, we need to understand some things about controller actions.</span></span>

<span data-ttu-id="b87ee-177">**Które metody na kontrolerze są traktowane jako "Akcje"?**</span><span class="sxs-lookup"><span data-stu-id="b87ee-177">**Which methods on the controller are considered "actions"?**</span></span> <span data-ttu-id="b87ee-178">Wybierając akcję, struktura przegląda tylko metody publiczne wystąpienia kontrolera.</span><span class="sxs-lookup"><span data-stu-id="b87ee-178">When selecting an action, the framework only looks at public instance methods on the controller.</span></span> <span data-ttu-id="b87ee-179">Ponadto nie obejmuje ["specjalne name"](https://msdn.microsoft.com/library/system.reflection.methodbase.isspecialname) metody (konstruktory, zdarzenia, przeciążenia operatorów i tak dalej) i metod odziedziczone **klasy ApiController** klasy.</span><span class="sxs-lookup"><span data-stu-id="b87ee-179">Also, it excludes ["special name"](https://msdn.microsoft.com/library/system.reflection.methodbase.isspecialname) methods (constructors, events, operator overloads, and so forth), and methods inherited from the **ApiController** class.</span></span>

<span data-ttu-id="b87ee-180">**Metody HTTP.**</span><span class="sxs-lookup"><span data-stu-id="b87ee-180">**HTTP Methods.**</span></span> <span data-ttu-id="b87ee-181">Struktura wybiera tylko akcje, które odpowiada metoda HTTP żądania, określany w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="b87ee-181">The framework only chooses actions that match the HTTP method of the request, determined as follows:</span></span>

1. <span data-ttu-id="b87ee-182">Metoda HTTP można określić za pomocą atrybutu: **AcceptVerbs**, **HttpDelete**, **HttpGet**, **HttpHead**, **httpoptions miał**, **HttpPatch**, **HttpPost**, lub **HttpPut**.</span><span class="sxs-lookup"><span data-stu-id="b87ee-182">You can specify the HTTP method with an attribute: **AcceptVerbs**, **HttpDelete**, **HttpGet**, **HttpHead**, **HttpOptions**, **HttpPatch**, **HttpPost**, or **HttpPut**.</span></span>
2. <span data-ttu-id="b87ee-183">W przeciwnym razie jeśli nazwa metody kontrolera rozpoczyna się od "Get", "Post", "Put", "Delete", "Head", "Opcje" lub "Poprawka", następnie zgodnie z Konwencją obsługiwane przez akcję tę metodę HTTP.</span><span class="sxs-lookup"><span data-stu-id="b87ee-183">Otherwise, if the name of the controller method starts with "Get", "Post", "Put", "Delete", "Head", "Options", or "Patch", then by convention the action supports that HTTP method.</span></span>
3. <span data-ttu-id="b87ee-184">Jeśli żadne z powyższych metoda obsługuje POST.</span><span class="sxs-lookup"><span data-stu-id="b87ee-184">If none of the above, the method supports POST.</span></span>

<span data-ttu-id="b87ee-185">**Powiązania parametrów.**</span><span class="sxs-lookup"><span data-stu-id="b87ee-185">**Parameter Bindings.**</span></span> <span data-ttu-id="b87ee-186">Wiązanie parametru jest o tym, jak interfejs API sieci Web tworzy wartość dla parametru.</span><span class="sxs-lookup"><span data-stu-id="b87ee-186">A parameter binding is how Web API creates a value for a parameter.</span></span> <span data-ttu-id="b87ee-187">Poniżej przedstawiono domyślną regułę dla wiązania parametru:</span><span class="sxs-lookup"><span data-stu-id="b87ee-187">Here is the default rule for parameter binding:</span></span>

- <span data-ttu-id="b87ee-188">Proste typy są pobierane z identyfikatora URI.</span><span class="sxs-lookup"><span data-stu-id="b87ee-188">Simple types are taken from the URI.</span></span>
- <span data-ttu-id="b87ee-189">Typy złożone są pobierane z treści żądania.</span><span class="sxs-lookup"><span data-stu-id="b87ee-189">Complex types are taken from the request body.</span></span>

<span data-ttu-id="b87ee-190">Proste typy obejmują wszystkie [pierwotnych typów programu .NET Framework](https://msdn.microsoft.com/library/system.type.isprimitive), oraz **daty/godziny**, **dziesiętna**, **Guid**, **ciągu** , i **TimeSpan**.</span><span class="sxs-lookup"><span data-stu-id="b87ee-190">Simple types include all of the [.NET Framework primitive types](https://msdn.microsoft.com/library/system.type.isprimitive), plus **DateTime**, **Decimal**, **Guid**, **String**, and **TimeSpan**.</span></span> <span data-ttu-id="b87ee-191">Dla każdej akcji co najwyżej jeden parametr można odczytać treści żądania.</span><span class="sxs-lookup"><span data-stu-id="b87ee-191">For each action, at most one parameter can read the request body.</span></span>

> [!NOTE]
> <span data-ttu-id="b87ee-192">Istnieje możliwość zastąpić domyślne reguły powiązania.</span><span class="sxs-lookup"><span data-stu-id="b87ee-192">It is possible to override the default binding rules.</span></span> <span data-ttu-id="b87ee-193">Zobacz [wiązanie parametru WebAPI kulisy](https://blogs.msdn.com/b/jmstall/archive/2012/05/11/webapi-parameter-binding-under-the-hood.aspx).</span><span class="sxs-lookup"><span data-stu-id="b87ee-193">See [WebAPI Parameter binding under the hood](https://blogs.msdn.com/b/jmstall/archive/2012/05/11/webapi-parameter-binding-under-the-hood.aspx).</span></span>


<span data-ttu-id="b87ee-194">W tle Oto algorytm wybór akcji.</span><span class="sxs-lookup"><span data-stu-id="b87ee-194">With that background, here is the action selection algorithm.</span></span>

1. <span data-ttu-id="b87ee-195">Utwórz listę wszystkich działań na kontrolerze, który odpowiada metoda żądania HTTP.</span><span class="sxs-lookup"><span data-stu-id="b87ee-195">Create a list of all actions on the controller that match the HTTP request method.</span></span>
2. <span data-ttu-id="b87ee-196">Jeśli słownika trasy ma wpis "action", Usuń akcje, których nazwa jest niezgodna z tej wartości.</span><span class="sxs-lookup"><span data-stu-id="b87ee-196">If the route dictionary has an "action" entry, remove actions whose name does not match this value.</span></span>
3. <span data-ttu-id="b87ee-197">Podjąć próbę dopasowania parametry akcji do identyfikatora URI, w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="b87ee-197">Try to match action parameters to the URI, as follows:</span></span> 

    1. <span data-ttu-id="b87ee-198">Dla każdej akcji zostanie wyświetlona lista parametrów, które są typu prostego, w którym pobiera parametr wiązania z identyfikatora URI.</span><span class="sxs-lookup"><span data-stu-id="b87ee-198">For each action, get a list of the parameters that are a simple type, where the binding gets the parameter from the URI.</span></span> <span data-ttu-id="b87ee-199">Wyklucz następujące parametry opcjonalne.</span><span class="sxs-lookup"><span data-stu-id="b87ee-199">Exclude optional parameters.</span></span>
    2. <span data-ttu-id="b87ee-200">Z tej listy spróbuj znaleźć dopasowania dla każdej nazwy parametru słownika trasy albo w ciągu zapytania identyfikatora URI.</span><span class="sxs-lookup"><span data-stu-id="b87ee-200">From this list, try to find a match for each parameter name, either in the route dictionary or in the URI query string.</span></span> <span data-ttu-id="b87ee-201">Dopasowania jest rozróżniana wielkość liter i nie są zależne od kolejność parametrów.</span><span class="sxs-lookup"><span data-stu-id="b87ee-201">Matches are case insensitive and do not depend on the parameter order.</span></span>
    3. <span data-ttu-id="b87ee-202">Wybierz akcję, której każdy parametr na liście jest zgodny w identyfikatorze URI.</span><span class="sxs-lookup"><span data-stu-id="b87ee-202">Select an action where every parameter in the list has a match in the URI.</span></span>
    4. <span data-ttu-id="b87ee-203">Jeśli bardziej tego jedną akcję spełnia te kryteria, wybierz jedną z większości dopasowaniami parametru.</span><span class="sxs-lookup"><span data-stu-id="b87ee-203">If more that one action meets these criteria, pick the one with the most parameter matches.</span></span>
4. <span data-ttu-id="b87ee-204">Ignoruj akcji przy użyciu **[NonAction]** atrybutu.</span><span class="sxs-lookup"><span data-stu-id="b87ee-204">Ignore actions with the **[NonAction]** attribute.</span></span>

<span data-ttu-id="b87ee-205">Krok #3 jest prawdopodobnie najbardziej mylące.</span><span class="sxs-lookup"><span data-stu-id="b87ee-205">Step #3 is probably the most confusing.</span></span> <span data-ttu-id="b87ee-206">Podstawowa koncepcja jest parametrem można jego wartość z identyfikatora URI, z treści żądania albo z niestandardowego powiązania.</span><span class="sxs-lookup"><span data-stu-id="b87ee-206">The basic idea is that a parameter can get its value either from the URI, from the request body, or from a custom binding.</span></span> <span data-ttu-id="b87ee-207">Dla parametrów, które pochodzą z identyfikatora URI chcemy się upewnić, że identyfikator URI faktycznie zawiera wartości tego parametru, w polu Ścieżka (za pomocą słownika trasy) lub w ciągu zapytania.</span><span class="sxs-lookup"><span data-stu-id="b87ee-207">For parameters that come from the URI, we want to ensure that the URI actually contains a value for that parameter, either in the path (via the route dictionary) or in the query string.</span></span>

<span data-ttu-id="b87ee-208">Na przykład rozważmy następującą akcję:</span><span class="sxs-lookup"><span data-stu-id="b87ee-208">For example, consider the following action:</span></span>

[!code-csharp[Main](routing-and-action-selection/samples/sample7.cs)]

<span data-ttu-id="b87ee-209">*Identyfikator* powiązanie parametru identyfikatora URI.</span><span class="sxs-lookup"><span data-stu-id="b87ee-209">The *id* parameter binds to the URI.</span></span> <span data-ttu-id="b87ee-210">W związku z tym ta akcja może wyłącznie odpowiadać identyfikator URI, który zawiera wartość "id" słownika trasy albo w ciągu zapytania.</span><span class="sxs-lookup"><span data-stu-id="b87ee-210">Therefore, this action can only match a URI that contains a value for "id", either in the route dictionary or in the query string.</span></span>

<span data-ttu-id="b87ee-211">Następujące parametry opcjonalne są wyjątku, ponieważ są opcjonalne.</span><span class="sxs-lookup"><span data-stu-id="b87ee-211">Optional parameters are an exception, because they are optional.</span></span> <span data-ttu-id="b87ee-212">Parametr opcjonalny jest OK Jeśli wiązanie nie może pobrać wartość z identyfikatora URI.</span><span class="sxs-lookup"><span data-stu-id="b87ee-212">For an optional parameter, it's OK if the binding can't get the value from the URI.</span></span>

<span data-ttu-id="b87ee-213">Typy złożone są wyjątkiem od innego powodu.</span><span class="sxs-lookup"><span data-stu-id="b87ee-213">Complex types are an exception for a different reason.</span></span> <span data-ttu-id="b87ee-214">Typ złożony można powiązać tylko z identyfikatora URI za pomocą niestandardowego powiązania.</span><span class="sxs-lookup"><span data-stu-id="b87ee-214">A complex type can only bind to the URI through a custom binding.</span></span> <span data-ttu-id="b87ee-215">Jednak w takim przypadku ramach nie wiedzieć z wyprzedzeniem czy parametr będzie powiązać z określonego identyfikatora URI.</span><span class="sxs-lookup"><span data-stu-id="b87ee-215">But in that case, the framework cannot know in advance whether the parameter would bind to a particular URI.</span></span> <span data-ttu-id="b87ee-216">Aby dowiedzieć się, należałoby do wywołania wiązania.</span><span class="sxs-lookup"><span data-stu-id="b87ee-216">To find out, it would need to invoke the binding.</span></span> <span data-ttu-id="b87ee-217">Celem algorytm wybór jest wybierz akcję z opisu statyczne, przed wywołaniem wszystkie powiązania.</span><span class="sxs-lookup"><span data-stu-id="b87ee-217">The goal of the selection algorithm is to select an action from the static description, before invoking any bindings.</span></span> <span data-ttu-id="b87ee-218">W związku z tym typy złożone są wykluczone z algorytm dopasowania.</span><span class="sxs-lookup"><span data-stu-id="b87ee-218">Therefore, complex types are excluded from the matching algorithm.</span></span>

<span data-ttu-id="b87ee-219">Po wybraniu akcji są wywoływane wszystkie powiązania parametrów.</span><span class="sxs-lookup"><span data-stu-id="b87ee-219">After the action is selected, all parameter bindings are invoked.</span></span>

<span data-ttu-id="b87ee-220">Podsumowanie:</span><span class="sxs-lookup"><span data-stu-id="b87ee-220">Summary:</span></span>

- <span data-ttu-id="b87ee-221">Akcja musi być zgodna metoda HTTP żądania.</span><span class="sxs-lookup"><span data-stu-id="b87ee-221">The action must match the HTTP method of the request.</span></span>
- <span data-ttu-id="b87ee-222">Nazwa akcji musi odpowiadać wpis "action" w słownika trasy, jeśli jest obecny.</span><span class="sxs-lookup"><span data-stu-id="b87ee-222">The action name must match the "action" entry in the route dictionary, if present.</span></span>
- <span data-ttu-id="b87ee-223">Dla każdego parametru akcji Jeśli parametr pochodzi z identyfikatora URI, następnie nazwa parametru musi można znaleźć słownika trasy albo w ciągu zapytania identyfikatora URI.</span><span class="sxs-lookup"><span data-stu-id="b87ee-223">For every parameter of the action, if the parameter is taken from the URI, then the parameter name must be found either in the route dictionary or in the URI query string.</span></span> <span data-ttu-id="b87ee-224">(Opcjonalne parametry i parametry za pomocą typy złożone są wyłączone.)</span><span class="sxs-lookup"><span data-stu-id="b87ee-224">(Optional parameters and parameters with complex types are excluded.)</span></span>
- <span data-ttu-id="b87ee-225">Podjąć próbę dopasowania z największą liczbą parametrów.</span><span class="sxs-lookup"><span data-stu-id="b87ee-225">Try to match the most number of parameters.</span></span> <span data-ttu-id="b87ee-226">Najlepsze dopasowanie może być metoda bez parametrów.</span><span class="sxs-lookup"><span data-stu-id="b87ee-226">The best match might be a method with no parameters.</span></span>

## <a name="extended-example"></a><span data-ttu-id="b87ee-227">Przykład rozszerzone</span><span class="sxs-lookup"><span data-stu-id="b87ee-227">Extended Example</span></span>

<span data-ttu-id="b87ee-228">Trasy:</span><span class="sxs-lookup"><span data-stu-id="b87ee-228">Routes:</span></span>

[!code-csharp[Main](routing-and-action-selection/samples/sample8.cs)]

<span data-ttu-id="b87ee-229">Kontroler:</span><span class="sxs-lookup"><span data-stu-id="b87ee-229">Controller:</span></span>

[!code-csharp[Main](routing-and-action-selection/samples/sample9.cs)]

<span data-ttu-id="b87ee-230">Żądania HTTP:</span><span class="sxs-lookup"><span data-stu-id="b87ee-230">HTTP request:</span></span>

[!code-console[Main](routing-and-action-selection/samples/sample10.cmd)]

### <a name="route-matching"></a><span data-ttu-id="b87ee-231">Kierowanie dopasowania</span><span class="sxs-lookup"><span data-stu-id="b87ee-231">Route Matching</span></span>

<span data-ttu-id="b87ee-232">Trasy o nazwie "DefaultApi" pasuje do identyfikatora URI.</span><span class="sxs-lookup"><span data-stu-id="b87ee-232">The URI matches the route named "DefaultApi".</span></span> <span data-ttu-id="b87ee-233">Słownik trasa zawiera następujące wpisy:</span><span class="sxs-lookup"><span data-stu-id="b87ee-233">The route dictionary contains the following entries:</span></span>

- <span data-ttu-id="b87ee-234">Kontroler: "produkty"</span><span class="sxs-lookup"><span data-stu-id="b87ee-234">controller: "products"</span></span>
- <span data-ttu-id="b87ee-235">Identyfikator: "1"</span><span class="sxs-lookup"><span data-stu-id="b87ee-235">id: "1"</span></span>

<span data-ttu-id="b87ee-236">Słownika trasy nie zawiera parametrów ciągu zapytania, "wersja" i "szczegóły", ale nadal będą one traktowane podczas wybór akcji.</span><span class="sxs-lookup"><span data-stu-id="b87ee-236">The route dictionary does not contain the query string parameters, "version" and "details", but these will still be considered during action selection.</span></span>

### <a name="controller-selection"></a><span data-ttu-id="b87ee-237">Wybieranie kontrolera</span><span class="sxs-lookup"><span data-stu-id="b87ee-237">Controller Selection</span></span>

<span data-ttu-id="b87ee-238">Z pozycji "controller" w słowniku trasy, jest typ kontrolera `ProductsController`.</span><span class="sxs-lookup"><span data-stu-id="b87ee-238">From the "controller" entry in the route dictionary, the controller type is `ProductsController`.</span></span>

### <a name="action-selection"></a><span data-ttu-id="b87ee-239">Wybór akcji</span><span class="sxs-lookup"><span data-stu-id="b87ee-239">Action Selection</span></span>

<span data-ttu-id="b87ee-240">Żądanie HTTP jest żądaniem GET.</span><span class="sxs-lookup"><span data-stu-id="b87ee-240">The HTTP request is a GET request.</span></span> <span data-ttu-id="b87ee-241">Akcji kontrolera, które obsługują GET są `GetAll`, `GetById`, i `FindProductsByName`.</span><span class="sxs-lookup"><span data-stu-id="b87ee-241">The controller actions that support GET are `GetAll`, `GetById`, and `FindProductsByName`.</span></span> <span data-ttu-id="b87ee-242">Słownika trasy nie zawiera wpis dla "action", więc nie trzeba być zgodna z nazwą akcji.</span><span class="sxs-lookup"><span data-stu-id="b87ee-242">The route dictionary does not contain an entry for "action", so we don't need to match the action name.</span></span>

<span data-ttu-id="b87ee-243">Następnie próbujemy dopasować nazw parametrów dla akcji, patrząc tylko operacje GET.</span><span class="sxs-lookup"><span data-stu-id="b87ee-243">Next, we try to match parameter names for the actions, looking only at the GET actions.</span></span>

| <span data-ttu-id="b87ee-244">Akcja</span><span class="sxs-lookup"><span data-stu-id="b87ee-244">Action</span></span> | <span data-ttu-id="b87ee-245">Parametry dopasowania</span><span class="sxs-lookup"><span data-stu-id="b87ee-245">Parameters to Match</span></span> |
| --- | --- |
| `GetAll` | <span data-ttu-id="b87ee-246">brak</span><span class="sxs-lookup"><span data-stu-id="b87ee-246">none</span></span> |
| `GetById` | <span data-ttu-id="b87ee-247">"id"</span><span class="sxs-lookup"><span data-stu-id="b87ee-247">"id"</span></span> |
| `FindProductsByName` | <span data-ttu-id="b87ee-248">"name"</span><span class="sxs-lookup"><span data-stu-id="b87ee-248">"name"</span></span> |

<span data-ttu-id="b87ee-249">Należy zauważyć, że *wersji* parametru `GetById` nie uznaje się, ponieważ jest on opcjonalny parametr.</span><span class="sxs-lookup"><span data-stu-id="b87ee-249">Notice that the *version* parameter of `GetById` is not considered, because it is an optional parameter.</span></span>

<span data-ttu-id="b87ee-250">`GetAll` Metoda odpowiada przypadku.</span><span class="sxs-lookup"><span data-stu-id="b87ee-250">The `GetAll` method matches trivially.</span></span> <span data-ttu-id="b87ee-251">`GetById` Metoda również jest zgodny, ponieważ słownika trasy zawiera "id".</span><span class="sxs-lookup"><span data-stu-id="b87ee-251">The `GetById` method also matches, because the route dictionary contains "id".</span></span> <span data-ttu-id="b87ee-252">`FindProductsByName` Metoda nie jest zgodny.</span><span class="sxs-lookup"><span data-stu-id="b87ee-252">The `FindProductsByName` method does not match.</span></span>

<span data-ttu-id="b87ee-253">`GetById` Metoda serwery wins, ponieważ jest on zgodny jeden parametr, a żadnych parametrów `GetAll`.</span><span class="sxs-lookup"><span data-stu-id="b87ee-253">The `GetById` method wins, because it matches one parameter, versus no parameters for `GetAll`.</span></span> <span data-ttu-id="b87ee-254">Metoda jest wywoływana z następującymi wartościami parametrów:</span><span class="sxs-lookup"><span data-stu-id="b87ee-254">The method is invoked with the following parameter values:</span></span>

- <span data-ttu-id="b87ee-255">*id* = 1</span><span class="sxs-lookup"><span data-stu-id="b87ee-255">*id* = 1</span></span>
- <span data-ttu-id="b87ee-256">*Wersja* = 1.5</span><span class="sxs-lookup"><span data-stu-id="b87ee-256">*version* = 1.5</span></span>

<span data-ttu-id="b87ee-257">Należy zauważyć, że nawet jeśli *wersji* nie był używany w algorytm wybór wartości parametru pochodzi z ciągu zapytania identyfikatora URI.</span><span class="sxs-lookup"><span data-stu-id="b87ee-257">Notice that even though *version* was not used in the selection algorithm, the value of the parameter comes from the URI query string.</span></span>

## <a name="extension-points"></a><span data-ttu-id="b87ee-258">Punkty rozszerzenia</span><span class="sxs-lookup"><span data-stu-id="b87ee-258">Extension Points</span></span>

<span data-ttu-id="b87ee-259">Internetowy interfejs API udostępnia punkty rozszerzenia dla niektórych części procesu routingu.</span><span class="sxs-lookup"><span data-stu-id="b87ee-259">Web API provides extension points for some parts of the routing process.</span></span>

| <span data-ttu-id="b87ee-260">Interface</span><span class="sxs-lookup"><span data-stu-id="b87ee-260">Interface</span></span> | <span data-ttu-id="b87ee-261">Opis</span><span class="sxs-lookup"><span data-stu-id="b87ee-261">Description</span></span> |
| --- | --- |
| <span data-ttu-id="b87ee-262">**IHttpControllerSelector**</span><span class="sxs-lookup"><span data-stu-id="b87ee-262">**IHttpControllerSelector**</span></span> | <span data-ttu-id="b87ee-263">Wybiera kontroler.</span><span class="sxs-lookup"><span data-stu-id="b87ee-263">Selects the controller.</span></span> |
| <span data-ttu-id="b87ee-264">**IHttpControllerTypeResolver**</span><span class="sxs-lookup"><span data-stu-id="b87ee-264">**IHttpControllerTypeResolver**</span></span> | <span data-ttu-id="b87ee-265">Pobiera listę typów kontrolerów.</span><span class="sxs-lookup"><span data-stu-id="b87ee-265">Gets the list of controller types.</span></span> <span data-ttu-id="b87ee-266">**DefaultHttpControllerSelector** wybiera typ kontrolera z tej listy.</span><span class="sxs-lookup"><span data-stu-id="b87ee-266">The **DefaultHttpControllerSelector** chooses the controller type from this list.</span></span> |
| <span data-ttu-id="b87ee-267">**IAssembliesResolver**</span><span class="sxs-lookup"><span data-stu-id="b87ee-267">**IAssembliesResolver**</span></span> | <span data-ttu-id="b87ee-268">Pobiera listę zestawów do projektu.</span><span class="sxs-lookup"><span data-stu-id="b87ee-268">Gets the list of project assemblies.</span></span> <span data-ttu-id="b87ee-269">**IHttpControllerTypeResolver** interfejsu używa tej listy, aby znaleźć typy kontrolerów.</span><span class="sxs-lookup"><span data-stu-id="b87ee-269">The **IHttpControllerTypeResolver** interface uses this list to find the controller types.</span></span> |
| <span data-ttu-id="b87ee-270">**IHttpControllerActivator**</span><span class="sxs-lookup"><span data-stu-id="b87ee-270">**IHttpControllerActivator**</span></span> | <span data-ttu-id="b87ee-271">Tworzy nowe wystąpienia kontrolera.</span><span class="sxs-lookup"><span data-stu-id="b87ee-271">Creates new controller instances.</span></span> |
| <span data-ttu-id="b87ee-272">**IHttpActionSelector**</span><span class="sxs-lookup"><span data-stu-id="b87ee-272">**IHttpActionSelector**</span></span> | <span data-ttu-id="b87ee-273">Wybiera akcję.</span><span class="sxs-lookup"><span data-stu-id="b87ee-273">Selects the action.</span></span> |
| <span data-ttu-id="b87ee-274">**IHttpActionInvoker**</span><span class="sxs-lookup"><span data-stu-id="b87ee-274">**IHttpActionInvoker**</span></span> | <span data-ttu-id="b87ee-275">Wywołuje akcję.</span><span class="sxs-lookup"><span data-stu-id="b87ee-275">Invokes the action.</span></span> |

<span data-ttu-id="b87ee-276">Aby dostarczyć własnej implementacji dla każdego z tych interfejsów, użyj **usług** kolekcji na **HttpConfiguration** obiektu:</span><span class="sxs-lookup"><span data-stu-id="b87ee-276">To provide your own implementation for any of these interfaces, use the **Services** collection on the **HttpConfiguration** object:</span></span>

[!code-csharp[Main](routing-and-action-selection/samples/sample11.cs)]
