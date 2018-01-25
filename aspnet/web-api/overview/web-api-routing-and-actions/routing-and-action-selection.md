---
uid: web-api/overview/web-api-routing-and-actions/routing-and-action-selection
title: "Wybieranie routingu i akcji w składniku ASP.NET Web API | Dokumentacja firmy Microsoft"
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/27/2012
ms.topic: article
ms.assetid: bcf2d223-cb7f-411e-be05-f43e96a14015
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/web-api-routing-and-actions/routing-and-action-selection
msc.type: authoredcontent
ms.openlocfilehash: 997582263bd48590b74434ee0ffc6be928fa1e08
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/24/2018
---
<a name="routing-and-action-selection-in-aspnet-web-api"></a><span data-ttu-id="088f6-102">Wybieranie routingu i akcji w składniku ASP.NET Web API</span><span class="sxs-lookup"><span data-stu-id="088f6-102">Routing and Action Selection in ASP.NET Web API</span></span>
====================
<span data-ttu-id="088f6-103">przez [Wasson Jan](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="088f6-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="088f6-104">W tym artykule opisano, jak składnika ASP.NET Web API kieruje żądania HTTP do określonej akcji w kontrolerze.</span><span class="sxs-lookup"><span data-stu-id="088f6-104">This article describes how ASP.NET Web API routes an HTTP request to a particular action on a controller.</span></span>

> [!NOTE]
> <span data-ttu-id="088f6-105">Szczegółowe omówienie routingu, zobacz [routingu na platformie ASP.NET Web API](routing-in-aspnet-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="088f6-105">For a high-level overview of routing, see [Routing in ASP.NET Web API](routing-in-aspnet-web-api.md).</span></span>


<span data-ttu-id="088f6-106">W tym artykule analizuje treść proces routingu.</span><span class="sxs-lookup"><span data-stu-id="088f6-106">This article looks at the details of the routing process.</span></span> <span data-ttu-id="088f6-107">Jeśli okaże się, że niektóre żądania nie pobieraj kierowane w oczekiwany sposób tworzenia projektu interfejsu API sieci Web, miejmy nadzieję, że ten artykuł pomoże.</span><span class="sxs-lookup"><span data-stu-id="088f6-107">If you create a Web API project and find that some requests don't get routed the way you expect, hopefully this article will help.</span></span>

<span data-ttu-id="088f6-108">Routing ma trzy główne fazy:</span><span class="sxs-lookup"><span data-stu-id="088f6-108">Routing has three main phases:</span></span>

1. <span data-ttu-id="088f6-109">Dopasowywania identyfikatora URI w szablonie trasy.</span><span class="sxs-lookup"><span data-stu-id="088f6-109">Matching the URI to a route template.</span></span>
2. <span data-ttu-id="088f6-110">Wybiera kontroler.</span><span class="sxs-lookup"><span data-stu-id="088f6-110">Selecting a controller.</span></span>
3. <span data-ttu-id="088f6-111">Wybranie akcji.</span><span class="sxs-lookup"><span data-stu-id="088f6-111">Selecting an action.</span></span>

<span data-ttu-id="088f6-112">Niektórych części procesu można zastąpić zachowania niestandardowych.</span><span class="sxs-lookup"><span data-stu-id="088f6-112">You can replace some parts of the process with your own custom behaviors.</span></span> <span data-ttu-id="088f6-113">W tym artykule opisano I zachowanie domyślne.</span><span class="sxs-lookup"><span data-stu-id="088f6-113">In this article, I describe the default behavior.</span></span> <span data-ttu-id="088f6-114">Na koniec I uwaga miejsca, w którym można dostosować zachowanie.</span><span class="sxs-lookup"><span data-stu-id="088f6-114">At the end, I note the places where you can customize the behavior.</span></span>

## <a name="route-templates"></a><span data-ttu-id="088f6-115">Szablony trasy</span><span class="sxs-lookup"><span data-stu-id="088f6-115">Route Templates</span></span>

<span data-ttu-id="088f6-116">Szablon trasy wygląda podobnie do ścieżka identyfikatora URI, ale może mieć wartości symbolu zastępczego, wskazane w nawiasach klamrowych:</span><span class="sxs-lookup"><span data-stu-id="088f6-116">A route template looks similar to a URI path, but it can have placeholder values, indicated with curly braces:</span></span>

[!code-csharp[Main](routing-and-action-selection/samples/sample1.ps1)]

<span data-ttu-id="088f6-117">Gdy tworzona jest trasa, można podać wartości domyślnej niektóre lub wszystkie symbole zastępcze:</span><span class="sxs-lookup"><span data-stu-id="088f6-117">When you create a route, you can provide default values for some or all of the placeholders:</span></span>

[!code-csharp[Main](routing-and-action-selection/samples/sample2.cs)]

<span data-ttu-id="088f6-118">Można też podać ograniczeń, które ograniczenia, jak segment identyfikatora URI może być zgodne z symbolem zastępczym:</span><span class="sxs-lookup"><span data-stu-id="088f6-118">You can also provide constraints, which restrict how a URI segment can match a placeholder:</span></span>

[!code-csharp[Main](routing-and-action-selection/samples/sample3.js)]

<span data-ttu-id="088f6-119">Platformę próbuje odpowiada segmentów w ścieżce identyfikator URI do szablonu.</span><span class="sxs-lookup"><span data-stu-id="088f6-119">The framework tries to match the segments in the URI path to the template.</span></span> <span data-ttu-id="088f6-120">Literały w szablonie muszą być zgodne.</span><span class="sxs-lookup"><span data-stu-id="088f6-120">Literals in the template must match exactly.</span></span> <span data-ttu-id="088f6-121">Symbol zastępczy dopasowuje dowolną wartością, chyba że zostanie ograniczenia.</span><span class="sxs-lookup"><span data-stu-id="088f6-121">A placeholder matches any value, unless you specify constraints.</span></span> <span data-ttu-id="088f6-122">Platformę nie pasuje do innych części identyfikatora URI, takich jak nazwa hosta lub parametrów zapytania.</span><span class="sxs-lookup"><span data-stu-id="088f6-122">The framework does not match other parts of the URI, such as the host name or the query parameters.</span></span> <span data-ttu-id="088f6-123">Platformę wybiera pierwszy trasy w tabeli tras, który pasuje do identyfikatora URI.</span><span class="sxs-lookup"><span data-stu-id="088f6-123">The framework selects the first route in the route table that matches the URI.</span></span>

<span data-ttu-id="088f6-124">Istnieją dwa specjalne symbole zastępcze: "{controller}" i "{action}".</span><span class="sxs-lookup"><span data-stu-id="088f6-124">There are two special placeholders: "{controller}" and "{action}".</span></span>

- <span data-ttu-id="088f6-125">"{controller}" zawiera nazwę kontrolera.</span><span class="sxs-lookup"><span data-stu-id="088f6-125">"{controller}" provides the name of the controller.</span></span>
- <span data-ttu-id="088f6-126">"{action}" zawiera nazwę akcji.</span><span class="sxs-lookup"><span data-stu-id="088f6-126">"{action}" provides the name of the action.</span></span> <span data-ttu-id="088f6-127">W składniku Web API standardowej konwencji jest, aby pominąć "{action}".</span><span class="sxs-lookup"><span data-stu-id="088f6-127">In Web API, the usual convention is to omit "{action}".</span></span>

### <a name="defaults"></a><span data-ttu-id="088f6-128">Wartość domyślna</span><span class="sxs-lookup"><span data-stu-id="088f6-128">Defaults</span></span>

<span data-ttu-id="088f6-129">Jeśli podasz wartości domyślnych trasy będzie odpowiadała identyfikator URI, który nie ma tych segmentów.</span><span class="sxs-lookup"><span data-stu-id="088f6-129">If you provide defaults, the route will match a URI that is missing those segments.</span></span> <span data-ttu-id="088f6-130">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="088f6-130">For example:</span></span>

[!code-csharp[Main](routing-and-action-selection/samples/sample4.cs)]

<span data-ttu-id="088f6-131">Identyfikator URI "`http://localhost/api/products`" tej trasie.</span><span class="sxs-lookup"><span data-stu-id="088f6-131">The URI "`http://localhost/api/products`" matches this route.</span></span> <span data-ttu-id="088f6-132">Segment "{kategorii}" jest przypisany wartość domyślna "all".</span><span class="sxs-lookup"><span data-stu-id="088f6-132">The "{category}" segment is assigned the default value "all".</span></span>

### <a name="route-dictionary"></a><span data-ttu-id="088f6-133">Słownika trasy</span><span class="sxs-lookup"><span data-stu-id="088f6-133">Route Dictionary</span></span>

<span data-ttu-id="088f6-134">Jeśli w ramach znalezienia dopasowania dla identyfikatora URI, tworzy słownik, który zawiera wartość dla każdego symbolu zastępczego.</span><span class="sxs-lookup"><span data-stu-id="088f6-134">If the framework finds a match for a URI, it creates a dictionary that contains the value for each placeholder.</span></span> <span data-ttu-id="088f6-135">Klucze są nazwy symbolu zastępczego, nie włączając nawiasów klamrowych.</span><span class="sxs-lookup"><span data-stu-id="088f6-135">The keys are the placeholder names, not including the curly braces.</span></span> <span data-ttu-id="088f6-136">Wartości te są pobierane z ścieżka identyfikatora URI lub wartości domyślne.</span><span class="sxs-lookup"><span data-stu-id="088f6-136">The values are taken from the URI path or from the defaults.</span></span> <span data-ttu-id="088f6-137">Słownik jest przechowywany w **IHttpRouteData** obiektu.</span><span class="sxs-lookup"><span data-stu-id="088f6-137">The dictionary is stored in the **IHttpRouteData** object.</span></span>

<span data-ttu-id="088f6-138">W tej fazie dopasowywanie trasy podobnie jak inne symbole zastępcze są traktowane specjalnych "{controller}" i "{action}" symbole zastępcze.</span><span class="sxs-lookup"><span data-stu-id="088f6-138">During this route-matching phase, the special "{controller}" and "{action}" placeholders are treated just like the other placeholders.</span></span> <span data-ttu-id="088f6-139">Po prostu są przechowywane w słowniku z innych wartości.</span><span class="sxs-lookup"><span data-stu-id="088f6-139">They are simply stored in the dictionary with the other values.</span></span>

<span data-ttu-id="088f6-140">Domyślnie może mieć wartości specjalne **RouteParameter.Optional**.</span><span class="sxs-lookup"><span data-stu-id="088f6-140">A default can have the special value **RouteParameter.Optional**.</span></span> <span data-ttu-id="088f6-141">Symbol zastępczy pobiera przypisany tej wartości, wartość nie została dodana do słownika trasy.</span><span class="sxs-lookup"><span data-stu-id="088f6-141">If a placeholder gets assigned this value, the value is not added to the route dictionary.</span></span> <span data-ttu-id="088f6-142">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="088f6-142">For example:</span></span>

[!code-csharp[Main](routing-and-action-selection/samples/sample5.cs)]

<span data-ttu-id="088f6-143">Dla ścieżki identyfikatora URI "interfejsu api/produktów" będzie zawierać słownika trasy:</span><span class="sxs-lookup"><span data-stu-id="088f6-143">For the URI path "api/products", the route dictionary will contain:</span></span>

- <span data-ttu-id="088f6-144">Kontroler: "produktów"</span><span class="sxs-lookup"><span data-stu-id="088f6-144">controller: "products"</span></span>
- <span data-ttu-id="088f6-145">Kategoria: "wszystkie"</span><span class="sxs-lookup"><span data-stu-id="088f6-145">category: "all"</span></span>

<span data-ttu-id="088f6-146">Dla "interfejsu api/produkty/toys/123" jednak słownika trasy będzie zawierać:</span><span class="sxs-lookup"><span data-stu-id="088f6-146">For "api/products/toys/123", however, the route dictionary will contain:</span></span>

- <span data-ttu-id="088f6-147">Kontroler: "produktów"</span><span class="sxs-lookup"><span data-stu-id="088f6-147">controller: "products"</span></span>
- <span data-ttu-id="088f6-148">Kategoria: "toys"</span><span class="sxs-lookup"><span data-stu-id="088f6-148">category: "toys"</span></span>
- <span data-ttu-id="088f6-149">Identyfikator: "123"</span><span class="sxs-lookup"><span data-stu-id="088f6-149">id: "123"</span></span>

<span data-ttu-id="088f6-150">Ustawienia domyślne mogą również obejmować wartość, która nie występować w dowolnym miejscu w szablonie trasy.</span><span class="sxs-lookup"><span data-stu-id="088f6-150">The defaults can also include a value that does not appear anywhere in the route template.</span></span> <span data-ttu-id="088f6-151">Jeśli trasa odpowiada, ta wartość jest przechowywane w słowniku.</span><span class="sxs-lookup"><span data-stu-id="088f6-151">If the route matches, that value is stored in the dictionary.</span></span> <span data-ttu-id="088f6-152">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="088f6-152">For example:</span></span>

[!code-csharp[Main](routing-and-action-selection/samples/sample6.cs)]

<span data-ttu-id="088f6-153">Jeśli ścieżka identyfikatora URI to "interfejsu api głównego/8", słownik będzie zawierać dwóch wartości:</span><span class="sxs-lookup"><span data-stu-id="088f6-153">If the URI path is "api/root/8", the dictionary will contain two values:</span></span>

- <span data-ttu-id="088f6-154">Kontroler: "klientów"</span><span class="sxs-lookup"><span data-stu-id="088f6-154">controller: "customers"</span></span>
- <span data-ttu-id="088f6-155">Identyfikator: "8"</span><span class="sxs-lookup"><span data-stu-id="088f6-155">id: "8"</span></span>

## <a name="selecting-a-controller"></a><span data-ttu-id="088f6-156">Wybiera kontroler</span><span class="sxs-lookup"><span data-stu-id="088f6-156">Selecting a Controller</span></span>

<span data-ttu-id="088f6-157">Wybór kontrolera jest obsługiwany przez **IHttpControllerSelector.SelectController** metody.</span><span class="sxs-lookup"><span data-stu-id="088f6-157">Controller selection is handled by the **IHttpControllerSelector.SelectController** method.</span></span> <span data-ttu-id="088f6-158">Ta metoda przyjmuje **HttpRequestMessage** wystąpienia i zwraca **HttpControllerDescriptor**.</span><span class="sxs-lookup"><span data-stu-id="088f6-158">This method takes an **HttpRequestMessage** instance and returns an **HttpControllerDescriptor**.</span></span> <span data-ttu-id="088f6-159">Domyślna implementacja jest zapewniana przez **DefaultHttpControllerSelector** klasy.</span><span class="sxs-lookup"><span data-stu-id="088f6-159">The default implementation is provided by the **DefaultHttpControllerSelector** class.</span></span> <span data-ttu-id="088f6-160">Ta klasa korzysta z prostego algorytmu:</span><span class="sxs-lookup"><span data-stu-id="088f6-160">This class uses a straightforward algorithm:</span></span>

1. <span data-ttu-id="088f6-161">Szukaj w słowniku trasy dla klucza "controller".</span><span class="sxs-lookup"><span data-stu-id="088f6-161">Look in the route dictionary for the key "controller".</span></span>
2. <span data-ttu-id="088f6-162">Pobrać wartość tego klucza i Dołącz ciąg "Controller", aby uzyskać nazwę typu kontrolera.</span><span class="sxs-lookup"><span data-stu-id="088f6-162">Take the value for this key and append the string "Controller" to get the controller type name.</span></span>
3. <span data-ttu-id="088f6-163">Poszukaj kontrolera interfejsu API sieci Web o tej nazwie typu.</span><span class="sxs-lookup"><span data-stu-id="088f6-163">Look for a Web API controller with this type name.</span></span>

<span data-ttu-id="088f6-164">Na przykład jeśli słownika trasy zawiera pary klucz wartość "controller" = "produktów", "ProductsController" jest typ kontrolera.</span><span class="sxs-lookup"><span data-stu-id="088f6-164">For example, if the route dictionary contains the key-value pair "controller" = "products", then the controller type is "ProductsController".</span></span> <span data-ttu-id="088f6-165">Jeśli określono żadnego typu zgodnego lub wiele dopasowań, platformę zwraca błąd do klienta.</span><span class="sxs-lookup"><span data-stu-id="088f6-165">If there is no matching type, or multiple matches, the framework returns an error to the client.</span></span>

<span data-ttu-id="088f6-166">W kroku 3 **DefaultHttpControllerSelector** używa **IHttpControllerTypeResolver** interfejsu, aby uzyskać listę typów kontrolera interfejsu API sieci Web.</span><span class="sxs-lookup"><span data-stu-id="088f6-166">For step 3, **DefaultHttpControllerSelector** uses the **IHttpControllerTypeResolver** interface to get the list of Web API controller types.</span></span> <span data-ttu-id="088f6-167">Domyślna implementacja **IHttpControllerTypeResolver** zwraca wszystkich publicznych klas, które implementują () **IHttpController**, (b) jest abstrakcyjny i (c) ma nazwę, która kończy się na "Controller".</span><span class="sxs-lookup"><span data-stu-id="088f6-167">The default implementation of **IHttpControllerTypeResolver** returns all public classes that (a) implement **IHttpController**, (b) are not abstract, and (c) have a name that ends in "Controller".</span></span>

## <a name="action-selection"></a><span data-ttu-id="088f6-168">Wybór działania</span><span class="sxs-lookup"><span data-stu-id="088f6-168">Action Selection</span></span>

<span data-ttu-id="088f6-169">Po wybraniu kontrolera, platformę wybiera akcję wywołując **IHttpActionSelector.SelectAction** metody.</span><span class="sxs-lookup"><span data-stu-id="088f6-169">After selecting the controller, the framework selects the action by calling the **IHttpActionSelector.SelectAction** method.</span></span> <span data-ttu-id="088f6-170">Ta metoda przyjmuje **HttpControllerContext** i zwraca **HttpActionDescriptor**.</span><span class="sxs-lookup"><span data-stu-id="088f6-170">This method takes an **HttpControllerContext** and returns an **HttpActionDescriptor**.</span></span>

<span data-ttu-id="088f6-171">Domyślna implementacja jest zapewniana przez **ApiControllerActionSelector** klasy.</span><span class="sxs-lookup"><span data-stu-id="088f6-171">The default implementation is provided by the **ApiControllerActionSelector** class.</span></span> <span data-ttu-id="088f6-172">Aby wybrać akcję, odbywa się na następujących czynności:</span><span class="sxs-lookup"><span data-stu-id="088f6-172">To select an action, it looks at the following:</span></span>

- <span data-ttu-id="088f6-173">Metoda HTTP żądania.</span><span class="sxs-lookup"><span data-stu-id="088f6-173">The HTTP method of the request.</span></span>
- <span data-ttu-id="088f6-174">Symbol zastępczy "{action}" w szablonie trasy, jeśli jest obecny.</span><span class="sxs-lookup"><span data-stu-id="088f6-174">The "{action}" placeholder in the route template, if present.</span></span>
- <span data-ttu-id="088f6-175">Parametry akcji w kontrolerze.</span><span class="sxs-lookup"><span data-stu-id="088f6-175">The parameters of the actions on the controller.</span></span>

<span data-ttu-id="088f6-176">Przed patrzeć algorytm zaznaczenia, należy poznać kilka rzeczy, o akcji kontrolera.</span><span class="sxs-lookup"><span data-stu-id="088f6-176">Before looking at the selection algorithm, we need to understand some things about controller actions.</span></span>

<span data-ttu-id="088f6-177">**Które metody na kontrolerze są traktowane jako "Akcje"?**</span><span class="sxs-lookup"><span data-stu-id="088f6-177">**Which methods on the controller are considered "actions"?**</span></span> <span data-ttu-id="088f6-178">Po wybraniu akcji, platformę przegląda tylko metody wystąpienia publicznego na kontrolerze.</span><span class="sxs-lookup"><span data-stu-id="088f6-178">When selecting an action, the framework only looks at public instance methods on the controller.</span></span> <span data-ttu-id="088f6-179">Ponadto wyklucza ["specjalną nazwą"](https://msdn.microsoft.com/library/system.reflection.methodbase.isspecialname) metod (konstruktorów, zdarzenia przeciążenia operatora i tak dalej) i dziedziczone z metody **klasy ApiController** klasy.</span><span class="sxs-lookup"><span data-stu-id="088f6-179">Also, it excludes ["special name"](https://msdn.microsoft.com/library/system.reflection.methodbase.isspecialname) methods (constructors, events, operator overloads, and so forth), and methods inherited from the **ApiController** class.</span></span>

<span data-ttu-id="088f6-180">**Metody HTTP.**</span><span class="sxs-lookup"><span data-stu-id="088f6-180">**HTTP Methods.**</span></span> <span data-ttu-id="088f6-181">Platformę wybiera tylko akcje, które odpowiada metoda HTTP żądania określane w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="088f6-181">The framework only chooses actions that match the HTTP method of the request, determined as follows:</span></span>

1. <span data-ttu-id="088f6-182">Metoda HTTP można określić za pomocą atrybutu: **AcceptVerbs**, **HttpDelete**, **HttpGet**, **HttpHead**,  **Httpoptions miał**, **HttpPatch**, **HttpPost**, lub **HttpPut**.</span><span class="sxs-lookup"><span data-stu-id="088f6-182">You can specify the HTTP method with an attribute: **AcceptVerbs**, **HttpDelete**, **HttpGet**, **HttpHead**, **HttpOptions**, **HttpPatch**, **HttpPost**, or **HttpPut**.</span></span>
2. <span data-ttu-id="088f6-183">W przeciwnym razie jeśli nazwa metody kontrolera rozpoczyna się od "Get", "Post", "Put", "Delete", "Head", "Opcje" lub "Poprawka", następnie według Konwencji obsługiwane przez akcję tej metody HTTP.</span><span class="sxs-lookup"><span data-stu-id="088f6-183">Otherwise, if the name of the controller method starts with "Get", "Post", "Put", "Delete", "Head", "Options", or "Patch", then by convention the action supports that HTTP method.</span></span>
3. <span data-ttu-id="088f6-184">Jeśli żaden z powyższych metoda obsługuje POST.</span><span class="sxs-lookup"><span data-stu-id="088f6-184">If none of the above, the method supports POST.</span></span>

<span data-ttu-id="088f6-185">**Powiązania parametrów.**</span><span class="sxs-lookup"><span data-stu-id="088f6-185">**Parameter Bindings.**</span></span> <span data-ttu-id="088f6-186">Wiązanie parametru jest sposób interfejsu API sieci Web tworzy wartość dla parametru.</span><span class="sxs-lookup"><span data-stu-id="088f6-186">A parameter binding is how Web API creates a value for a parameter.</span></span> <span data-ttu-id="088f6-187">Oto reguły domyślnej dla parametru wiązania:</span><span class="sxs-lookup"><span data-stu-id="088f6-187">Here is the default rule for parameter binding:</span></span>

- <span data-ttu-id="088f6-188">Proste typy są pobierane z identyfikatora URI.</span><span class="sxs-lookup"><span data-stu-id="088f6-188">Simple types are taken from the URI.</span></span>
- <span data-ttu-id="088f6-189">Typy złożone są pobierane z treści żądania.</span><span class="sxs-lookup"><span data-stu-id="088f6-189">Complex types are taken from the request body.</span></span>

<span data-ttu-id="088f6-190">Proste typy obejmują wszystkie [typów pierwotnych .NET Framework](https://msdn.microsoft.com/library/system.type.isprimitive), plus **DateTime**, **dziesiętną**, **identyfikatora Guid**, **ciągu** , i **TimeSpan**.</span><span class="sxs-lookup"><span data-stu-id="088f6-190">Simple types include all of the [.NET Framework primitive types](https://msdn.microsoft.com/library/system.type.isprimitive), plus **DateTime**, **Decimal**, **Guid**, **String**, and **TimeSpan**.</span></span> <span data-ttu-id="088f6-191">Dla każdej akcji co najwyżej jeden parametr można odczytać treści żądania.</span><span class="sxs-lookup"><span data-stu-id="088f6-191">For each action, at most one parameter can read the request body.</span></span>

> [!NOTE]
> <span data-ttu-id="088f6-192">Istnieje możliwość zastąpienia domyślnych reguł powiązania.</span><span class="sxs-lookup"><span data-stu-id="088f6-192">It is possible to override the default binding rules.</span></span> <span data-ttu-id="088f6-193">Zobacz [wiązanie parametru WebAPI kulisy](https://blogs.msdn.com/b/jmstall/archive/2012/05/11/webapi-parameter-binding-under-the-hood.aspx).</span><span class="sxs-lookup"><span data-stu-id="088f6-193">See [WebAPI Parameter binding under the hood](https://blogs.msdn.com/b/jmstall/archive/2012/05/11/webapi-parameter-binding-under-the-hood.aspx).</span></span>


<span data-ttu-id="088f6-194">Tło Oto algorytm wybór akcji.</span><span class="sxs-lookup"><span data-stu-id="088f6-194">With that background, here is the action selection algorithm.</span></span>

1. <span data-ttu-id="088f6-195">Utwórz listę wszystkich działań na kontrolerze, który odpowiada metoda żądania HTTP.</span><span class="sxs-lookup"><span data-stu-id="088f6-195">Create a list of all actions on the controller that match the HTTP request method.</span></span>
2. <span data-ttu-id="088f6-196">Jeśli słownika trasy ma wpis "Akcja", Usuń akcje, którego nazwa jest niezgodna z tej wartości.</span><span class="sxs-lookup"><span data-stu-id="088f6-196">If the route dictionary has an "action" entry, remove actions whose name does not match this value.</span></span>
3. <span data-ttu-id="088f6-197">Spróbuj odpowiadające parametry akcji do identyfikatora URI, w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="088f6-197">Try to match action parameters to the URI, as follows:</span></span> 

    1. <span data-ttu-id="088f6-198">Dla każdej akcji pobrać listę parametrów, które są typu prostego, gdzie pobiera parametr wiązania z identyfikatora URI.</span><span class="sxs-lookup"><span data-stu-id="088f6-198">For each action, get a list of the parameters that are a simple type, where the binding gets the parameter from the URI.</span></span> <span data-ttu-id="088f6-199">Wyklucz następujące parametry opcjonalne.</span><span class="sxs-lookup"><span data-stu-id="088f6-199">Exclude optional parameters.</span></span>
    2. <span data-ttu-id="088f6-200">Z tej listy prób znalezienia dopasowania dla każdej nazwy parametru w słowniku trasy lub w ciągu zapytania identyfikatora URI.</span><span class="sxs-lookup"><span data-stu-id="088f6-200">From this list, try to find a match for each parameter name, either in the route dictionary or in the URI query string.</span></span> <span data-ttu-id="088f6-201">Dopasowań uwzględniana jest wielkość liter i nie zależą od kolejność parametrów.</span><span class="sxs-lookup"><span data-stu-id="088f6-201">Matches are case insensitive and do not depend on the parameter order.</span></span>
    3. <span data-ttu-id="088f6-202">Wybierz akcję, gdzie każdy parametr na liście ma dopasowania w identyfikatorze URI.</span><span class="sxs-lookup"><span data-stu-id="088f6-202">Select an action where every parameter in the list has a match in the URI.</span></span>
    4. <span data-ttu-id="088f6-203">Jeśli bardziej tego jedną akcję spełnia te kryteria, wybierz jedną z większości dopasowań parametru.</span><span class="sxs-lookup"><span data-stu-id="088f6-203">If more that one action meets these criteria, pick the one with the most parameter matches.</span></span>
4. <span data-ttu-id="088f6-204">Ignoruj akcji z **[NonAction]** atrybutu.</span><span class="sxs-lookup"><span data-stu-id="088f6-204">Ignore actions with the **[NonAction]** attribute.</span></span>

<span data-ttu-id="088f6-205">Krok #3 jest prawdopodobnie najbardziej mylące.</span><span class="sxs-lookup"><span data-stu-id="088f6-205">Step #3 is probably the most confusing.</span></span> <span data-ttu-id="088f6-206">Podstawową koncepcją jest parametrem można uzyskać jego wartość z identyfikatora URI, z treści żądania albo z niestandardowego powiązania.</span><span class="sxs-lookup"><span data-stu-id="088f6-206">The basic idea is that a parameter can get its value either from the URI, from the request body, or from a custom binding.</span></span> <span data-ttu-id="088f6-207">Dla parametrów, które pochodzą z identyfikatora URI chcemy upewnić się, że identyfikator URI zawiera wartości tego parametru, w polu Ścieżka (za pomocą słownika trasy) lub w ciągu zapytania.</span><span class="sxs-lookup"><span data-stu-id="088f6-207">For parameters that come from the URI, we want to ensure that the URI actually contains a value for that parameter, either in the path (via the route dictionary) or in the query string.</span></span>

<span data-ttu-id="088f6-208">Na przykład należy rozważyć następujące działania:</span><span class="sxs-lookup"><span data-stu-id="088f6-208">For example, consider the following action:</span></span>

[!code-csharp[Main](routing-and-action-selection/samples/sample7.cs)]

<span data-ttu-id="088f6-209">*Identyfikator* powiązanie parametru identyfikatora URI.</span><span class="sxs-lookup"><span data-stu-id="088f6-209">The *id* parameter binds to the URI.</span></span> <span data-ttu-id="088f6-210">W związku z tym ta akcja może wyłącznie odpowiadać identyfikator URI, który zawiera wartość "id" w słowniku trasy lub w ciągu zapytania.</span><span class="sxs-lookup"><span data-stu-id="088f6-210">Therefore, this action can only match a URI that contains a value for "id", either in the route dictionary or in the query string.</span></span>

<span data-ttu-id="088f6-211">Parametry opcjonalne są wyjątek, ponieważ są opcjonalne.</span><span class="sxs-lookup"><span data-stu-id="088f6-211">Optional parameters are an exception, because they are optional.</span></span> <span data-ttu-id="088f6-212">Parametr opcjonalny jest OK Jeśli powiązanie nie może pobrać wartości z identyfikatora URI.</span><span class="sxs-lookup"><span data-stu-id="088f6-212">For an optional parameter, it's OK if the binding can't get the value from the URI.</span></span>

<span data-ttu-id="088f6-213">Typy złożone są wyjątkiem z różnych przyczyn.</span><span class="sxs-lookup"><span data-stu-id="088f6-213">Complex types are an exception for a different reason.</span></span> <span data-ttu-id="088f6-214">Typ złożony można powiązać tylko z identyfikatora URI za pomocą niestandardowego powiązania.</span><span class="sxs-lookup"><span data-stu-id="088f6-214">A complex type can only bind to the URI through a custom binding.</span></span> <span data-ttu-id="088f6-215">Jednak w takim przypadku platformę nie wiedzieć z wyprzedzeniem, czy parametr czy powiązania do określonego identyfikatora URI.</span><span class="sxs-lookup"><span data-stu-id="088f6-215">But in that case, the framework cannot know in advance whether the parameter would bind to a particular URI.</span></span> <span data-ttu-id="088f6-216">Aby dowiedzieć się, czy trzeba wywołać powiązanie.</span><span class="sxs-lookup"><span data-stu-id="088f6-216">To find out, it would need to invoke the binding.</span></span> <span data-ttu-id="088f6-217">Celem algorytm zaznaczenie ma wybierz akcję z opisu statycznych, zanim wywoła powiązań.</span><span class="sxs-lookup"><span data-stu-id="088f6-217">The goal of the selection algorithm is to select an action from the static description, before invoking any bindings.</span></span> <span data-ttu-id="088f6-218">W związku z tym typy złożone są wykluczone z algorytm dopasowania.</span><span class="sxs-lookup"><span data-stu-id="088f6-218">Therefore, complex types are excluded from the matching algorithm.</span></span>

<span data-ttu-id="088f6-219">Po wybraniu akcji są wywoływane wszystkie powiązania parametrów.</span><span class="sxs-lookup"><span data-stu-id="088f6-219">After the action is selected, all parameter bindings are invoked.</span></span>

<span data-ttu-id="088f6-220">Podsumowanie:</span><span class="sxs-lookup"><span data-stu-id="088f6-220">Summary:</span></span>

- <span data-ttu-id="088f6-221">Akcja musi być zgodna metoda HTTP żądania.</span><span class="sxs-lookup"><span data-stu-id="088f6-221">The action must match the HTTP method of the request.</span></span>
- <span data-ttu-id="088f6-222">Nazwa akcji musi być zgodna wpis "Akcja" w słowniku trasy, jeśli jest obecny.</span><span class="sxs-lookup"><span data-stu-id="088f6-222">The action name must match the "action" entry in the route dictionary, if present.</span></span>
- <span data-ttu-id="088f6-223">Dla każdego parametru akcji Jeśli parametr pochodzi z identyfikatora URI, następnie nazwa parametru musi zostać znaleziony w słowniku trasy lub w ciągu zapytania identyfikatora URI.</span><span class="sxs-lookup"><span data-stu-id="088f6-223">For every parameter of the action, if the parameter is taken from the URI, then the parameter name must be found either in the route dictionary or in the URI query string.</span></span> <span data-ttu-id="088f6-224">(Opcjonalne parametry i parametry o typach złożonych, są wyłączone.)</span><span class="sxs-lookup"><span data-stu-id="088f6-224">(Optional parameters and parameters with complex types are excluded.)</span></span>
- <span data-ttu-id="088f6-225">Podjąć próbę dopasowania największą liczbą parametrów.</span><span class="sxs-lookup"><span data-stu-id="088f6-225">Try to match the most number of parameters.</span></span> <span data-ttu-id="088f6-226">Najlepsze dopasowanie może być metodą bez parametrów.</span><span class="sxs-lookup"><span data-stu-id="088f6-226">The best match might be a method with no parameters.</span></span>

## <a name="extended-example"></a><span data-ttu-id="088f6-227">Przykład rozszerzone</span><span class="sxs-lookup"><span data-stu-id="088f6-227">Extended Example</span></span>

<span data-ttu-id="088f6-228">Trasy:</span><span class="sxs-lookup"><span data-stu-id="088f6-228">Routes:</span></span>

[!code-csharp[Main](routing-and-action-selection/samples/sample8.cs)]

<span data-ttu-id="088f6-229">Kontroler:</span><span class="sxs-lookup"><span data-stu-id="088f6-229">Controller:</span></span>

[!code-csharp[Main](routing-and-action-selection/samples/sample9.cs)]

<span data-ttu-id="088f6-230">Żądania HTTP:</span><span class="sxs-lookup"><span data-stu-id="088f6-230">HTTP request:</span></span>

[!code-console[Main](routing-and-action-selection/samples/sample10.cmd)]

### <a name="route-matching"></a><span data-ttu-id="088f6-231">Trasy dopasowania</span><span class="sxs-lookup"><span data-stu-id="088f6-231">Route Matching</span></span>

<span data-ttu-id="088f6-232">Identyfikator URI trasie, o nazwie "DefaultApi".</span><span class="sxs-lookup"><span data-stu-id="088f6-232">The URI matches the route named "DefaultApi".</span></span> <span data-ttu-id="088f6-233">Słownika trasy zawiera następujące wpisy:</span><span class="sxs-lookup"><span data-stu-id="088f6-233">The route dictionary contains the following entries:</span></span>

- <span data-ttu-id="088f6-234">Kontroler: "produktów"</span><span class="sxs-lookup"><span data-stu-id="088f6-234">controller: "products"</span></span>
- <span data-ttu-id="088f6-235">Identyfikator: "1"</span><span class="sxs-lookup"><span data-stu-id="088f6-235">id: "1"</span></span>

<span data-ttu-id="088f6-236">Słownika trasy nie zawiera parametrów ciągu zapytania, "version" i "szczegóły", ale nadal będą one traktowane podczas wybór akcji.</span><span class="sxs-lookup"><span data-stu-id="088f6-236">The route dictionary does not contain the query string parameters, "version" and "details", but these will still be considered during action selection.</span></span>

### <a name="controller-selection"></a><span data-ttu-id="088f6-237">Wybór kontrolera</span><span class="sxs-lookup"><span data-stu-id="088f6-237">Controller Selection</span></span>

<span data-ttu-id="088f6-238">Z wpisu "controller" w słowniku trasy, jest typ kontrolera `ProductsController`.</span><span class="sxs-lookup"><span data-stu-id="088f6-238">From the "controller" entry in the route dictionary, the controller type is `ProductsController`.</span></span>

### <a name="action-selection"></a><span data-ttu-id="088f6-239">Wybór działania</span><span class="sxs-lookup"><span data-stu-id="088f6-239">Action Selection</span></span>

<span data-ttu-id="088f6-240">Żądanie HTTP jest żądaniem GET.</span><span class="sxs-lookup"><span data-stu-id="088f6-240">The HTTP request is a GET request.</span></span> <span data-ttu-id="088f6-241">Akcji kontrolera, które obsługują GET są `GetAll`, `GetById`, i `FindProductsByName`.</span><span class="sxs-lookup"><span data-stu-id="088f6-241">The controller actions that support GET are `GetAll`, `GetById`, and `FindProductsByName`.</span></span> <span data-ttu-id="088f6-242">Słownika trasy nie zawiera pozycji "Akcja", więc musimy nie jest zgodna z nazwą akcji.</span><span class="sxs-lookup"><span data-stu-id="088f6-242">The route dictionary does not contain an entry for "action", so we don't need to match the action name.</span></span>

<span data-ttu-id="088f6-243">Następnie próbujemy dopasować nazwy parametrów działań, patrzeć tylko akcje GET.</span><span class="sxs-lookup"><span data-stu-id="088f6-243">Next, we try to match parameter names for the actions, looking only at the GET actions.</span></span>

| <span data-ttu-id="088f6-244">Akcja</span><span class="sxs-lookup"><span data-stu-id="088f6-244">Action</span></span> | <span data-ttu-id="088f6-245">Parametry dopasowania</span><span class="sxs-lookup"><span data-stu-id="088f6-245">Parameters to Match</span></span> |
| --- | --- |
| `GetAll` | <span data-ttu-id="088f6-246">brak</span><span class="sxs-lookup"><span data-stu-id="088f6-246">none</span></span> |
| `GetById` | <span data-ttu-id="088f6-247">"id"</span><span class="sxs-lookup"><span data-stu-id="088f6-247">"id"</span></span> |
| `FindProductsByName` | <span data-ttu-id="088f6-248">"Nazwa"</span><span class="sxs-lookup"><span data-stu-id="088f6-248">"name"</span></span> |

<span data-ttu-id="088f6-249">Zwróć uwagę, że *wersji* parametr `GetById` jest uważana za, ponieważ jest parametrem opcjonalnym.</span><span class="sxs-lookup"><span data-stu-id="088f6-249">Notice that the *version* parameter of `GetById` is not considered, because it is an optional parameter.</span></span>

<span data-ttu-id="088f6-250">`GetAll` Metoda trivially odpowiada.</span><span class="sxs-lookup"><span data-stu-id="088f6-250">The `GetAll` method matches trivially.</span></span> <span data-ttu-id="088f6-251">`GetById` Metody zgodny, ponieważ słownika trasy zawiera "id".</span><span class="sxs-lookup"><span data-stu-id="088f6-251">The `GetById` method also matches, because the route dictionary contains "id".</span></span> <span data-ttu-id="088f6-252">`FindProductsByName` — Metoda nie jest zgodna.</span><span class="sxs-lookup"><span data-stu-id="088f6-252">The `FindProductsByName` method does not match.</span></span>

<span data-ttu-id="088f6-253">`GetById` Metody wins, ponieważ jest on zgodny jeden parametr, a żadne parametry dla `GetAll`.</span><span class="sxs-lookup"><span data-stu-id="088f6-253">The `GetById` method wins, because it matches one parameter, versus no parameters for `GetAll`.</span></span> <span data-ttu-id="088f6-254">Metoda jest wywoływana z następujących wartości parametrów:</span><span class="sxs-lookup"><span data-stu-id="088f6-254">The method is invoked with the following parameter values:</span></span>

- <span data-ttu-id="088f6-255">*id* = 1</span><span class="sxs-lookup"><span data-stu-id="088f6-255">*id* = 1</span></span>
- <span data-ttu-id="088f6-256">*Wersja* = 1.5</span><span class="sxs-lookup"><span data-stu-id="088f6-256">*version* = 1.5</span></span>

<span data-ttu-id="088f6-257">Należy zauważyć, że nawet jeśli *wersji* nie były używane w algorytmie zaznaczenia, wartość parametru pochodzi z ciągu zapytania identyfikatora URI.</span><span class="sxs-lookup"><span data-stu-id="088f6-257">Notice that even though *version* was not used in the selection algorithm, the value of the parameter comes from the URI query string.</span></span>

## <a name="extension-points"></a><span data-ttu-id="088f6-258">Punkty rozszerzenia</span><span class="sxs-lookup"><span data-stu-id="088f6-258">Extension Points</span></span>

<span data-ttu-id="088f6-259">Interfejsu API sieci Web udostępniają punkty rozszerzeń dla niektórych części procesu routingu.</span><span class="sxs-lookup"><span data-stu-id="088f6-259">Web API provides extension points for some parts of the routing process.</span></span>

| <span data-ttu-id="088f6-260">Interface</span><span class="sxs-lookup"><span data-stu-id="088f6-260">Interface</span></span> | <span data-ttu-id="088f6-261">Opis</span><span class="sxs-lookup"><span data-stu-id="088f6-261">Description</span></span> |
| --- | --- |
| <span data-ttu-id="088f6-262">**IHttpControllerSelector**</span><span class="sxs-lookup"><span data-stu-id="088f6-262">**IHttpControllerSelector**</span></span> | <span data-ttu-id="088f6-263">Wybiera kontrolera.</span><span class="sxs-lookup"><span data-stu-id="088f6-263">Selects the controller.</span></span> |
| <span data-ttu-id="088f6-264">**IHttpControllerTypeResolver**</span><span class="sxs-lookup"><span data-stu-id="088f6-264">**IHttpControllerTypeResolver**</span></span> | <span data-ttu-id="088f6-265">Pobiera listę typów kontrolerów.</span><span class="sxs-lookup"><span data-stu-id="088f6-265">Gets the list of controller types.</span></span> <span data-ttu-id="088f6-266">**DefaultHttpControllerSelector** wybiera typ kontrolera z tej listy.</span><span class="sxs-lookup"><span data-stu-id="088f6-266">The **DefaultHttpControllerSelector** chooses the controller type from this list.</span></span> |
| <span data-ttu-id="088f6-267">**IAssembliesResolver**</span><span class="sxs-lookup"><span data-stu-id="088f6-267">**IAssembliesResolver**</span></span> | <span data-ttu-id="088f6-268">Pobiera listę zestawów projektu.</span><span class="sxs-lookup"><span data-stu-id="088f6-268">Gets the list of project assemblies.</span></span> <span data-ttu-id="088f6-269">**IHttpControllerTypeResolver** interfejs używa tej listy można znaleźć typów kontrolerów.</span><span class="sxs-lookup"><span data-stu-id="088f6-269">The **IHttpControllerTypeResolver** interface uses this list to find the controller types.</span></span> |
| <span data-ttu-id="088f6-270">**IHttpControllerActivator**</span><span class="sxs-lookup"><span data-stu-id="088f6-270">**IHttpControllerActivator**</span></span> | <span data-ttu-id="088f6-271">Tworzy nowe wystąpienia kontrolera.</span><span class="sxs-lookup"><span data-stu-id="088f6-271">Creates new controller instances.</span></span> |
| <span data-ttu-id="088f6-272">**IHttpActionSelector**</span><span class="sxs-lookup"><span data-stu-id="088f6-272">**IHttpActionSelector**</span></span> | <span data-ttu-id="088f6-273">Wybiera akcję.</span><span class="sxs-lookup"><span data-stu-id="088f6-273">Selects the action.</span></span> |
| <span data-ttu-id="088f6-274">**IHttpActionInvoker**</span><span class="sxs-lookup"><span data-stu-id="088f6-274">**IHttpActionInvoker**</span></span> | <span data-ttu-id="088f6-275">Wywołuje akcję.</span><span class="sxs-lookup"><span data-stu-id="088f6-275">Invokes the action.</span></span> |

<span data-ttu-id="088f6-276">Aby podać własne wdrożenia dla każdego z tych interfejsów, należy użyć **usług** kolekcji na **HttpConfiguration** obiektu:</span><span class="sxs-lookup"><span data-stu-id="088f6-276">To provide your own implementation for any of these interfaces, use the **Services** collection on the **HttpConfiguration** object:</span></span>

[!code-csharp[Main](routing-and-action-selection/samples/sample11.cs)]
