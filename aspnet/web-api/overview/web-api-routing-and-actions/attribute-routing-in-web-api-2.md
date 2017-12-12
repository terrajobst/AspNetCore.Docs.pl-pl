---
uid: web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2
title: "Atrybut routingu w składniku ASP.NET Web API 2 | Dokumentacja firmy Microsoft"
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/20/2014
ms.topic: article
ms.assetid: 979d6c9f-0129-4e5b-ae56-4507b281b86d
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2
msc.type: authoredcontent
ms.openlocfilehash: ad44ee525601f308498967159e964aa41a2ce00c
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
<a name="attribute-routing-in-aspnet-web-api-2"></a><span data-ttu-id="73992-102">Atrybut routingu w składniku ASP.NET Web API 2</span><span class="sxs-lookup"><span data-stu-id="73992-102">Attribute Routing in ASP.NET Web API 2</span></span>
====================
<span data-ttu-id="73992-103">przez [Wasson Jan](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="73992-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="73992-104">*Routing* jest sposób interfejsu API sieci Web odpowiada identyfikatora URI do akcji.</span><span class="sxs-lookup"><span data-stu-id="73992-104">*Routing* is how Web API matches a URI to an action.</span></span> <span data-ttu-id="73992-105">Składnik Web API 2 obsługuje nowy typ routingu, nazywany *trasami atrybutów*.</span><span class="sxs-lookup"><span data-stu-id="73992-105">Web API 2 supports a new type of routing, called *attribute routing*.</span></span> <span data-ttu-id="73992-106">Jak wskazuje nazwę, trasami atrybutów używa atrybutów do definiowania trasy.</span><span class="sxs-lookup"><span data-stu-id="73992-106">As the name implies, attribute routing uses attributes to define routes.</span></span> <span data-ttu-id="73992-107">Atrybut routingu zapewnia większą kontrolę nad identyfikatory URI w interfejsie API sieci web.</span><span class="sxs-lookup"><span data-stu-id="73992-107">Attribute routing gives you more control over the URIs in your web API.</span></span> <span data-ttu-id="73992-108">Na przykład można łatwo utworzyć identyfikatory URI, które opisują hierarchie zasobów.</span><span class="sxs-lookup"><span data-stu-id="73992-108">For example, you can easily create URIs that describe hierarchies of resources.</span></span>

<span data-ttu-id="73992-109">Wcześniejszych styl routingu, o nazwie opartych na konwencjach routingu, jest nadal w pełni obsługiwane.</span><span class="sxs-lookup"><span data-stu-id="73992-109">The earlier style of routing, called convention-based routing, is still fully supported.</span></span> <span data-ttu-id="73992-110">W rzeczywistości można łączyć obie techniki, w tym samym projekcie.</span><span class="sxs-lookup"><span data-stu-id="73992-110">In fact, you can combine both techniques in the same project.</span></span>

<span data-ttu-id="73992-111">W tym temacie pokazano, jak włączyć routing atrybutu i zawiera opis różnych opcji trasami atrybutów.</span><span class="sxs-lookup"><span data-stu-id="73992-111">This topic shows how to enable attribute routing and describes the various options for attribute routing.</span></span> <span data-ttu-id="73992-112">Samouczek end-to-end, który używa atrybutu routingu, zobacz [utworzyć interfejs API REST z atrybutu routingu w sieci Web API 2](create-a-rest-api-with-attribute-routing.md).</span><span class="sxs-lookup"><span data-stu-id="73992-112">For an end-to-end tutorial that uses attribute routing, see [Create a REST API with Attribute Routing in Web API 2](create-a-rest-api-with-attribute-routing.md).</span></span>


## <a name="prerequisites"></a><span data-ttu-id="73992-113">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="73992-113">Prerequisites</span></span>

<span data-ttu-id="73992-114">[Visual Studio 2017](https://www.visualstudio.com/vs/) Community, Professional lub Enterprise Edition</span><span class="sxs-lookup"><span data-stu-id="73992-114">[Visual Studio 2017](https://www.visualstudio.com/vs/) Community, Professional, or Enterprise Edition</span></span>

<span data-ttu-id="73992-115">Można również użyć Menedżera pakietów NuGet w celu zainstalowania wymaganych pakietów.</span><span class="sxs-lookup"><span data-stu-id="73992-115">Alternatively, use NuGet Package Manager to install the necessary packages.</span></span> <span data-ttu-id="73992-116">Z **narzędzia** menu w programie Visual Studio, wybierz **Menedżer pakietów biblioteki**, a następnie wybierz pozycję **Konsola Menedżera pakietów**.</span><span class="sxs-lookup"><span data-stu-id="73992-116">From the **Tools** menu in Visual Studio, select **Library Package Manager**, then select **Package Manager Console**.</span></span> <span data-ttu-id="73992-117">Wprowadź następujące polecenie w oknie konsoli Menedżera pakietów:</span><span class="sxs-lookup"><span data-stu-id="73992-117">Enter the following command in the Package Manager Console window:</span></span>

`Install-Package Microsoft.AspNet.WebApi.WebHost`

<a id="why"></a>
## <a name="why-attribute-routing"></a><span data-ttu-id="73992-118">Dlaczego atrybutu routingu?</span><span class="sxs-lookup"><span data-stu-id="73992-118">Why Attribute Routing?</span></span>

<span data-ttu-id="73992-119">Pierwszą wersję interfejsu API sieci Web używany *opartych na konwencjach* routingu.</span><span class="sxs-lookup"><span data-stu-id="73992-119">The first release of Web API used *convention-based* routing.</span></span> <span data-ttu-id="73992-120">W tym typie routingu należy zdefiniować lub więcej szablonów tras, które są po prostu sparametryzowana ciągów.</span><span class="sxs-lookup"><span data-stu-id="73992-120">In that type of routing, you define one or more route templates, which are basically parameterized strings.</span></span> <span data-ttu-id="73992-121">Gdy w ramach odbiera żądanie, odpowiada identyfikator URI dla szablonu trasy.</span><span class="sxs-lookup"><span data-stu-id="73992-121">When the framework receives a request, it matches the URI against the route template.</span></span> <span data-ttu-id="73992-122">(Aby uzyskać więcej informacji na temat opartych na konwencjach routingu, zobacz [routingu na platformie ASP.NET Web API](routing-in-aspnet-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="73992-122">(For more information about convention-based routing, see [Routing in ASP.NET Web API](routing-in-aspnet-web-api.md).</span></span>

<span data-ttu-id="73992-123">Jedną z zalet opartych na konwencjach routingu jest szablonów są definiowane w jednym miejscu, czy spójnego stosowania reguły routingu przez wszystkie kontrolery.</span><span class="sxs-lookup"><span data-stu-id="73992-123">One advantage of convention-based routing is that templates are defined in a single place, and the routing rules are applied consistently across all controllers.</span></span> <span data-ttu-id="73992-124">Niestety opartych na konwencjach routingu utrudnia obsługę niektórych wzorców identyfikatora URI, które są często używane w interfejsy API RESTful.</span><span class="sxs-lookup"><span data-stu-id="73992-124">Unfortunately, convention-based routing makes it hard to support certain URI patterns that are common in RESTful APIs.</span></span> <span data-ttu-id="73992-125">Na przykład zasoby często zawierają zasoby podrzędne: klienci mają zleceń, filmy ma uczestników, książek ma autorzy i tak dalej.</span><span class="sxs-lookup"><span data-stu-id="73992-125">For example, resources often contain child resources: Customers have orders, movies have actors, books have authors, and so forth.</span></span> <span data-ttu-id="73992-126">Jest fizyczne do utworzenia identyfikatorów URI odzwierciedlenia tych relacji:</span><span class="sxs-lookup"><span data-stu-id="73992-126">It's natural to create URIs that reflect these relations:</span></span>

`/customers/1/orders`

<span data-ttu-id="73992-127">Ten typ identyfikatora URI jest trudne do utworzenia przy użyciu opartych na konwencjach routingu.</span><span class="sxs-lookup"><span data-stu-id="73992-127">This type of URI is difficult to create using convention-based routing.</span></span> <span data-ttu-id="73992-128">Mimo że mogą to robić, wyniki nie skalować dobrze, jeśli masz wiele kontrolerów lub typów zasobów.</span><span class="sxs-lookup"><span data-stu-id="73992-128">Although it can be done, the results don't scale well if you have many controllers or resource types.</span></span>

<span data-ttu-id="73992-129">W przypadku routingu atrybutu jest prosta do definiowania trasy dla tego identyfikatora URI.</span><span class="sxs-lookup"><span data-stu-id="73992-129">With attribute routing, it's trivial to define a route for this URI.</span></span> <span data-ttu-id="73992-130">Atrybut jest po prostu Dodaj do akcji kontrolera:</span><span class="sxs-lookup"><span data-stu-id="73992-130">You simply add an attribute to the controller action:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample1.cs)]

<span data-ttu-id="73992-131">Poniżej przedstawiono niektóre wzorców, które atrybutu routingu umożliwia łatwe.</span><span class="sxs-lookup"><span data-stu-id="73992-131">Here are some other patterns that attribute routing makes easy.</span></span>

<span data-ttu-id="73992-132">**Przechowywanie wersji interfejsu API**</span><span class="sxs-lookup"><span data-stu-id="73992-132">**API versioning**</span></span>

<span data-ttu-id="73992-133">W tym przykładzie "/ api/v1/produktów" może być kierowany do kontrolera innego niż "/ api/v2/produktów".</span><span class="sxs-lookup"><span data-stu-id="73992-133">In this example, "/api/v1/products" would be routed to a different controller than "/api/v2/products".</span></span>

`/api/v1/products`  
`/api/v2/products`

<span data-ttu-id="73992-134">**Przeciążone segmentów identyfikatora URI**</span><span class="sxs-lookup"><span data-stu-id="73992-134">**Overloaded URI segments**</span></span>

<span data-ttu-id="73992-135">W tym przykładzie "1" jest numer zamówienia, ale mapuje "oczekujące" do kolekcji.</span><span class="sxs-lookup"><span data-stu-id="73992-135">In this example, "1" is an order number, but "pending" maps to a collection.</span></span>

`/orders/1`  
`/orders/pending`

<span data-ttu-id="73992-136">**Typy parametrów wielu**</span><span class="sxs-lookup"><span data-stu-id="73992-136">**Mulitple parameter types**</span></span>

<span data-ttu-id="73992-137">W tym przykładzie "1" jest numer zamówienia, ale "2013 06/16" Określa datę.</span><span class="sxs-lookup"><span data-stu-id="73992-137">In this example, "1" is an order number, but "2013/06/16" specifies a date.</span></span>

`/orders/1`  
`/orders/2013/06/16`

<a id="enable"></a>
## <a name="enabling-attribute-routing"></a><span data-ttu-id="73992-138">Włączanie trasami atrybutów</span><span class="sxs-lookup"><span data-stu-id="73992-138">Enabling Attribute Routing</span></span>

<span data-ttu-id="73992-139">Aby włączyć routing atrybutu, należy wywołać **MapHttpAttributeRoutes** podczas konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="73992-139">To enable attribute routing, call **MapHttpAttributeRoutes** during configuration.</span></span> <span data-ttu-id="73992-140">Ta metoda rozszerzenia jest zdefiniowany w **System.Web.Http.HttpConfigurationExtensions** klasy.</span><span class="sxs-lookup"><span data-stu-id="73992-140">This extension method is defined in the **System.Web.Http.HttpConfigurationExtensions** class.</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample2.cs)]

<span data-ttu-id="73992-141">Atrybut routingu można łączyć z [opartych na konwencjach](routing-in-aspnet-web-api.md) routingu.</span><span class="sxs-lookup"><span data-stu-id="73992-141">Attribute routing can be combined with [convention-based](routing-in-aspnet-web-api.md) routing.</span></span> <span data-ttu-id="73992-142">Aby zdefiniować na podstawie Konwencji tras, należy wywołać **MapHttpRoute** metody.</span><span class="sxs-lookup"><span data-stu-id="73992-142">To define convention-based routes, call the **MapHttpRoute** method.</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample3.cs)]

<span data-ttu-id="73992-143">Aby uzyskać więcej informacji o konfigurowaniu interfejsu API sieci Web, zobacz [Konfigurowanie ASP.NET Web API 2](../advanced/configuring-aspnet-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="73992-143">For more information about configuring Web API, see [Configuring ASP.NET Web API 2](../advanced/configuring-aspnet-web-api.md).</span></span>

<a id="config"></a>
### <a name="note-migrating-from-web-api-1"></a><span data-ttu-id="73992-144">Uwaga: Migracja z 1 interfejs API sieci Web</span><span class="sxs-lookup"><span data-stu-id="73992-144">Note: Migrating From Web API 1</span></span>

<span data-ttu-id="73992-145">Przed 2 interfejsu API sieci Web Szablony projektu interfejsu API sieci Web wygenerowanego kodu następująco:</span><span class="sxs-lookup"><span data-stu-id="73992-145">Prior to Web API 2, the Web API project templates generated code like this:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample4.cs)]

<span data-ttu-id="73992-146">Jeśli atrybut routing jest włączony, ten kod spowoduje zgłoszenie wyjątku.</span><span class="sxs-lookup"><span data-stu-id="73992-146">If attribute routing is enabled, this code will throw an exception.</span></span> <span data-ttu-id="73992-147">Jeżeli uaktualnisz istniejący projekt interfejsu API sieci Web, aby korzystać z routingu atrybut, upewnij się, że zaktualizuj kod tej konfiguracji do następującego:</span><span class="sxs-lookup"><span data-stu-id="73992-147">If you upgrade an existing Web API project to use attribute routing, make sure to update this configuration code to the following:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample5.cs?highlight=4)]

> [!NOTE]
> <span data-ttu-id="73992-148">Aby uzyskać więcej informacji, zobacz [Konfigurowanie interfejsu API sieci Web z hostingu ASP.NET](../advanced/configuring-aspnet-web-api.md#webhost).</span><span class="sxs-lookup"><span data-stu-id="73992-148">For more information, see [Configuring Web API with ASP.NET Hosting](../advanced/configuring-aspnet-web-api.md#webhost).</span></span>


<a id="add-routes"></a>
## <a name="adding-route-attributes"></a><span data-ttu-id="73992-149">Dodawanie tras atrybutów</span><span class="sxs-lookup"><span data-stu-id="73992-149">Adding Route Attributes</span></span>

<span data-ttu-id="73992-150">Oto przykład trasy zdefiniowane przy użyciu atrybutu:</span><span class="sxs-lookup"><span data-stu-id="73992-150">Here is an example of a route defined using an attribute:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample6.cs)]

<span data-ttu-id="73992-151">Ciąg &quot;klienci / {customerId} / porządkuje&quot; jest szablon identyfikatora URI dla trasy.</span><span class="sxs-lookup"><span data-stu-id="73992-151">The string &quot;customers/{customerId}/orders&quot; is the URI template for the route.</span></span> <span data-ttu-id="73992-152">Interfejs API sieci Web próbuje pasuje do identyfikatora URI żądania do tego szablonu.</span><span class="sxs-lookup"><span data-stu-id="73992-152">Web API tries to match the request URI to the template.</span></span> <span data-ttu-id="73992-153">W tym przykładzie "klienci" i "zamówienia" są segmenty literału i "{customerId}" jest parametrem zmiennej.</span><span class="sxs-lookup"><span data-stu-id="73992-153">In this example, "customers" and "orders" are literal segments, and "{customerId}" is a variable parameter.</span></span> <span data-ttu-id="73992-154">Następujące identyfikatory URI spowoduje dopasowanie tego szablonu:</span><span class="sxs-lookup"><span data-stu-id="73992-154">The following URIs would match this template:</span></span>

- `http://localhost/customers/1/orders`
- `http://localhost/customers/bob/orders`
- `http://localhost/customers/1234-5678/orders`

<span data-ttu-id="73992-155">Można ograniczyć przy użyciu odpowiedniego [ograniczenia](#constraints), które zostały opisane w dalszej części tego tematu.</span><span class="sxs-lookup"><span data-stu-id="73992-155">You can restrict the matching by using [constraints](#constraints), described later in this topic.</span></span>

<span data-ttu-id="73992-156">Zwróć uwagę, że &quot;{customerId}&quot; parametru w szablonie trasy odpowiada nazwie *customerId* parametru w metodzie.</span><span class="sxs-lookup"><span data-stu-id="73992-156">Notice that the &quot;{customerId}&quot; parameter in the route template matches the name of the *customerId* parameter in the method.</span></span> <span data-ttu-id="73992-157">Gdy interfejs API sieci Web wywołuje akcji kontrolera, próbuje wiązania parametrów trasy.</span><span class="sxs-lookup"><span data-stu-id="73992-157">When Web API invokes the controller action, it tries to bind the route parameters.</span></span> <span data-ttu-id="73992-158">Na przykład, jeśli identyfikator URI jest `http://example.com/customers/1/orders`, interfejsu API sieci Web próbuje powiązać wartość "1" *customerId* parametru tej akcji.</span><span class="sxs-lookup"><span data-stu-id="73992-158">For example, if the URI is `http://example.com/customers/1/orders`, Web API tries to bind the value "1" to the *customerId* parameter in the action.</span></span>

<span data-ttu-id="73992-159">Szablon identyfikatora URI może mieć kilka parametrów:</span><span class="sxs-lookup"><span data-stu-id="73992-159">A URI template can have several parameters:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample7.cs)]

<span data-ttu-id="73992-160">Wszystkie metody kontrolera, które nie mają atrybutu trasy korzystać z opartych na konwencjach routingu.</span><span class="sxs-lookup"><span data-stu-id="73992-160">Any controller methods that do not have a route attribute use convention-based routing.</span></span> <span data-ttu-id="73992-161">W ten sposób można połączyć oba rodzaje routingu w tym samym projekcie.</span><span class="sxs-lookup"><span data-stu-id="73992-161">That way, you can combine both types of routing in the same project.</span></span>

## <a name="http-methods"></a><span data-ttu-id="73992-162">Metody HTTP</span><span class="sxs-lookup"><span data-stu-id="73992-162">HTTP Methods</span></span>

<span data-ttu-id="73992-163">Interfejs API sieci Web wybiera także działania na podstawie metody HTTP żądania (GET, POST itp.).</span><span class="sxs-lookup"><span data-stu-id="73992-163">Web API also selects actions based on the HTTP method of the request (GET, POST, etc).</span></span> <span data-ttu-id="73992-164">Domyślnie interfejsu API sieci Web wygląda bez uwzględniania wielkości liter dopasowanie z początku nazwy metody kontrolera.</span><span class="sxs-lookup"><span data-stu-id="73992-164">By default, Web API looks for a case-insensitive match with the start of the controller method name.</span></span> <span data-ttu-id="73992-165">Na przykład metoda kontrolera o nazwie `PutCustomers` dopasowuje żądanie HTTP PUT.</span><span class="sxs-lookup"><span data-stu-id="73992-165">For example, a controller method named `PutCustomers` matches an HTTP PUT request.</span></span>

<span data-ttu-id="73992-166">Można zastąpić tę Konwencję dekoracji — metoda ze wszystkimi następującymi atrybutami:</span><span class="sxs-lookup"><span data-stu-id="73992-166">You can override this convention by decorating the mathod with any the following attributes:</span></span>

- <span data-ttu-id="73992-167">**[HttpDelete]**</span><span class="sxs-lookup"><span data-stu-id="73992-167">**[HttpDelete]**</span></span>
- <span data-ttu-id="73992-168">**[HttpGet]**</span><span class="sxs-lookup"><span data-stu-id="73992-168">**[HttpGet]**</span></span>
- <span data-ttu-id="73992-169">**[HttpHead]**</span><span class="sxs-lookup"><span data-stu-id="73992-169">**[HttpHead]**</span></span>
- <span data-ttu-id="73992-170">**[Httpoptions miał]**</span><span class="sxs-lookup"><span data-stu-id="73992-170">**[HttpOptions]**</span></span>
- <span data-ttu-id="73992-171">**[HttpPatch]**</span><span class="sxs-lookup"><span data-stu-id="73992-171">**[HttpPatch]**</span></span>
- <span data-ttu-id="73992-172">**[HttpPost]**</span><span class="sxs-lookup"><span data-stu-id="73992-172">**[HttpPost]**</span></span>
- <span data-ttu-id="73992-173">**[HttpPut]**</span><span class="sxs-lookup"><span data-stu-id="73992-173">**[HttpPut]**</span></span>

<span data-ttu-id="73992-174">Poniższy przykład mapuje CreateBook metoda żądania HTTP POST.</span><span class="sxs-lookup"><span data-stu-id="73992-174">The following example maps the CreateBook method to HTTP POST requests.</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample8.cs)]

<span data-ttu-id="73992-175">W przypadku wszystkich innych metod HTTP, łącznie z metod niestandardowych, użycie **AcceptVerbs** atrybut, który przyjmuje listę metod HTTP.</span><span class="sxs-lookup"><span data-stu-id="73992-175">For all other HTTP methods, including non-standard methods, use the **AcceptVerbs** attribute, which takes a list of HTTP methods.</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample9.cs)]

<a id="prefixes"></a>
## <a name="route-prefixes"></a><span data-ttu-id="73992-176">Prefiksy trasy</span><span class="sxs-lookup"><span data-stu-id="73992-176">Route Prefixes</span></span>

<span data-ttu-id="73992-177">Często trasy w kontrolerze wszystkie rozpoczyna się od tego samego prefiksu.</span><span class="sxs-lookup"><span data-stu-id="73992-177">Often, the routes in a controller all start with the same prefix.</span></span> <span data-ttu-id="73992-178">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="73992-178">For example:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample10.cs)]

<span data-ttu-id="73992-179">Należy określić Wspólny prefiks dla całego kontrolera przy użyciu **[RoutePrefix]** atrybutu:</span><span class="sxs-lookup"><span data-stu-id="73992-179">You can set a common prefix for an entire controller by using the **[RoutePrefix]** attribute:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample11.cs)]

<span data-ttu-id="73992-180">Użyj tyldy (~) na atrybut method, aby przesłonić prefiks trasy:</span><span class="sxs-lookup"><span data-stu-id="73992-180">Use a tilde (~) on the method attribute to override the route prefix:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample12.cs)]

<span data-ttu-id="73992-181">Prefiks trasy może zawierać parametry:</span><span class="sxs-lookup"><span data-stu-id="73992-181">The route prefix can include parameters:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample13.cs)]

<a id="constraints"></a>
## <a name="route-constraints"></a><span data-ttu-id="73992-182">Ograniczenia trasy</span><span class="sxs-lookup"><span data-stu-id="73992-182">Route Constraints</span></span>

<span data-ttu-id="73992-183">Ograniczenia trasy pozwalają ograniczyć, jak są dopasowywane parametry w szablonie trasy.</span><span class="sxs-lookup"><span data-stu-id="73992-183">Route constraints let you restrict how the parameters in the route template are matched.</span></span> <span data-ttu-id="73992-184">Ogólna składnia jest &quot;{ograniczenia parametru}:&quot;.</span><span class="sxs-lookup"><span data-stu-id="73992-184">The general syntax is &quot;{parameter:constraint}&quot;.</span></span> <span data-ttu-id="73992-185">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="73992-185">For example:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample14.cs)]

<span data-ttu-id="73992-186">W tym miejscu trasy pierwszy będzie można wybrać tylko jeśli &quot;identyfikator&quot; segmencie identyfikatora URI jest liczbą całkowitą.</span><span class="sxs-lookup"><span data-stu-id="73992-186">Here, the first route will only be selected if the &quot;id&quot; segment of the URI is an integer.</span></span> <span data-ttu-id="73992-187">W przeciwnym razie zostanie wybrany drugi trasy.</span><span class="sxs-lookup"><span data-stu-id="73992-187">Otherwise, the second route will be chosen.</span></span>

<span data-ttu-id="73992-188">W poniższej tabeli wymieniono ograniczenia, które są obsługiwane.</span><span class="sxs-lookup"><span data-stu-id="73992-188">The following table lists the constraints that are supported.</span></span>

| <span data-ttu-id="73992-189">Ograniczenia</span><span class="sxs-lookup"><span data-stu-id="73992-189">Constraint</span></span> | <span data-ttu-id="73992-190">Opis</span><span class="sxs-lookup"><span data-stu-id="73992-190">Description</span></span> | <span data-ttu-id="73992-191">Przykład</span><span class="sxs-lookup"><span data-stu-id="73992-191">Example</span></span> |
| --- | --- | --- |
| <span data-ttu-id="73992-192">Alpha</span><span class="sxs-lookup"><span data-stu-id="73992-192">alpha</span></span> | <span data-ttu-id="73992-193">Dopasowań wielkie lub małe litery alfabetu łacińskiego (a – z, A-Z)</span><span class="sxs-lookup"><span data-stu-id="73992-193">Matches uppercase or lowercase Latin alphabet characters (a-z, A-Z)</span></span> | <span data-ttu-id="73992-194">{x: alfa}</span><span class="sxs-lookup"><span data-stu-id="73992-194">{x:alpha}</span></span> |
| <span data-ttu-id="73992-195">bool</span><span class="sxs-lookup"><span data-stu-id="73992-195">bool</span></span> | <span data-ttu-id="73992-196">Dopasowuje wartość logiczną.</span><span class="sxs-lookup"><span data-stu-id="73992-196">Matches a Boolean value.</span></span> | <span data-ttu-id="73992-197">{x: bool}</span><span class="sxs-lookup"><span data-stu-id="73992-197">{x:bool}</span></span> |
| <span data-ttu-id="73992-198">datetime</span><span class="sxs-lookup"><span data-stu-id="73992-198">datetime</span></span> | <span data-ttu-id="73992-199">Dopasowań **DateTime** wartość.</span><span class="sxs-lookup"><span data-stu-id="73992-199">Matches a **DateTime** value.</span></span> | <span data-ttu-id="73992-200">{x: datetime}</span><span class="sxs-lookup"><span data-stu-id="73992-200">{x:datetime}</span></span> |
| <span data-ttu-id="73992-201">decimal</span><span class="sxs-lookup"><span data-stu-id="73992-201">decimal</span></span> | <span data-ttu-id="73992-202">Odpowiada wartości dziesiętnej.</span><span class="sxs-lookup"><span data-stu-id="73992-202">Matches a decimal value.</span></span> | <span data-ttu-id="73992-203">{x: decimal}</span><span class="sxs-lookup"><span data-stu-id="73992-203">{x:decimal}</span></span> |
| <span data-ttu-id="73992-204">double</span><span class="sxs-lookup"><span data-stu-id="73992-204">double</span></span> | <span data-ttu-id="73992-205">Odpowiada wartość 64-bitowych liczb zmiennoprzecinkowych.</span><span class="sxs-lookup"><span data-stu-id="73992-205">Matches a 64-bit floating-point value.</span></span> | <span data-ttu-id="73992-206">{x: double}</span><span class="sxs-lookup"><span data-stu-id="73992-206">{x:double}</span></span> |
| <span data-ttu-id="73992-207">float</span><span class="sxs-lookup"><span data-stu-id="73992-207">float</span></span> | <span data-ttu-id="73992-208">Dopasowuje wartości zmiennoprzecinkowej 32-bitowych.</span><span class="sxs-lookup"><span data-stu-id="73992-208">Matches a 32-bit floating-point value.</span></span> | <span data-ttu-id="73992-209">{x: float}</span><span class="sxs-lookup"><span data-stu-id="73992-209">{x:float}</span></span> |
| <span data-ttu-id="73992-210">Identyfikator GUID</span><span class="sxs-lookup"><span data-stu-id="73992-210">guid</span></span> | <span data-ttu-id="73992-211">Dopasowuje wartości identyfikatora GUID.</span><span class="sxs-lookup"><span data-stu-id="73992-211">Matches a GUID value.</span></span> | <span data-ttu-id="73992-212">{x: guid}</span><span class="sxs-lookup"><span data-stu-id="73992-212">{x:guid}</span></span> |
| <span data-ttu-id="73992-213">int</span><span class="sxs-lookup"><span data-stu-id="73992-213">int</span></span> | <span data-ttu-id="73992-214">Zgodna z wartością 32-bitową liczbę całkowitą.</span><span class="sxs-lookup"><span data-stu-id="73992-214">Matches a 32-bit integer value.</span></span> | <span data-ttu-id="73992-215">{x: int}</span><span class="sxs-lookup"><span data-stu-id="73992-215">{x:int}</span></span> |
| <span data-ttu-id="73992-216">length</span><span class="sxs-lookup"><span data-stu-id="73992-216">length</span></span> | <span data-ttu-id="73992-217">Dopasowuje ciąg znaków o określonej długości lub zakresu określonej długości.</span><span class="sxs-lookup"><span data-stu-id="73992-217">Matches a string with the specified length or within a specified range of lengths.</span></span> | <span data-ttu-id="73992-218">{x: length(6)} {x: length(1,20)}</span><span class="sxs-lookup"><span data-stu-id="73992-218">{x:length(6)} {x:length(1,20)}</span></span> |
| <span data-ttu-id="73992-219">long</span><span class="sxs-lookup"><span data-stu-id="73992-219">long</span></span> | <span data-ttu-id="73992-220">Odpowiada wartość 64-bitową liczbę całkowitą.</span><span class="sxs-lookup"><span data-stu-id="73992-220">Matches a 64-bit integer value.</span></span> | <span data-ttu-id="73992-221">{x: long}</span><span class="sxs-lookup"><span data-stu-id="73992-221">{x:long}</span></span> |
| <span data-ttu-id="73992-222">max</span><span class="sxs-lookup"><span data-stu-id="73992-222">max</span></span> | <span data-ttu-id="73992-223">Dopasowuje typu integer o wartości maksymalnej.</span><span class="sxs-lookup"><span data-stu-id="73992-223">Matches an integer with a maximum value.</span></span> | <span data-ttu-id="73992-224">{x: max(10)}</span><span class="sxs-lookup"><span data-stu-id="73992-224">{x:max(10)}</span></span> |
| <span data-ttu-id="73992-225">Element MaxLength</span><span class="sxs-lookup"><span data-stu-id="73992-225">maxlength</span></span> | <span data-ttu-id="73992-226">Dopasowuje ciąg o maksymalnej długości.</span><span class="sxs-lookup"><span data-stu-id="73992-226">Matches a string with a maximum length.</span></span> | <span data-ttu-id="73992-227">{x: maxlength(10)}</span><span class="sxs-lookup"><span data-stu-id="73992-227">{x:maxlength(10)}</span></span> |
| <span data-ttu-id="73992-228">min</span><span class="sxs-lookup"><span data-stu-id="73992-228">min</span></span> | <span data-ttu-id="73992-229">Dopasowuje jako liczba całkowita, wartość minimalna.</span><span class="sxs-lookup"><span data-stu-id="73992-229">Matches an integer with a minimum value.</span></span> | <span data-ttu-id="73992-230">{x: min(10)}</span><span class="sxs-lookup"><span data-stu-id="73992-230">{x:min(10)}</span></span> |
| <span data-ttu-id="73992-231">Element MinLength</span><span class="sxs-lookup"><span data-stu-id="73992-231">minlength</span></span> | <span data-ttu-id="73992-232">Dopasowuje ciąg o minimalnej długości.</span><span class="sxs-lookup"><span data-stu-id="73992-232">Matches a string with a minimum length.</span></span> | <span data-ttu-id="73992-233">{x: minlength(10)}</span><span class="sxs-lookup"><span data-stu-id="73992-233">{x:minlength(10)}</span></span> |
| <span data-ttu-id="73992-234">range</span><span class="sxs-lookup"><span data-stu-id="73992-234">range</span></span> | <span data-ttu-id="73992-235">Dopasowuje całkowitą w zakresie wartości.</span><span class="sxs-lookup"><span data-stu-id="73992-235">Matches an integer within a range of values.</span></span> | <span data-ttu-id="73992-236">{x: range(10,50)}</span><span class="sxs-lookup"><span data-stu-id="73992-236">{x:range(10,50)}</span></span> |
| <span data-ttu-id="73992-237">wyrażenia regularnego</span><span class="sxs-lookup"><span data-stu-id="73992-237">regex</span></span> | <span data-ttu-id="73992-238">Pasuje do wyrażenia regularnego.</span><span class="sxs-lookup"><span data-stu-id="73992-238">Matches a regular expression.</span></span> | <span data-ttu-id="73992-239">{x: regex(^\d{3}-\d{3}-\d{4}$)}</span><span class="sxs-lookup"><span data-stu-id="73992-239">{x:regex(^\d{3}-\d{3}-\d{4}$)}</span></span> |

<span data-ttu-id="73992-240">Powiadomienie niektórych ograniczeń, takich jak &quot;min&quot;, przyjmuje argumentów w nawiasach.</span><span class="sxs-lookup"><span data-stu-id="73992-240">Notice that some of the constraints, such as &quot;min&quot;, take arguments in parentheses.</span></span> <span data-ttu-id="73992-241">Można stosować wiele ograniczeń do parametru, oddzielone dwukropkiem.</span><span class="sxs-lookup"><span data-stu-id="73992-241">You can apply multiple constraints to a parameter, separated by a colon.</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample15.cs)]

### <a name="custom-route-constraints"></a><span data-ttu-id="73992-242">Ograniczenia trasy niestandardowe</span><span class="sxs-lookup"><span data-stu-id="73992-242">Custom Route Constraints</span></span>

<span data-ttu-id="73992-243">Można utworzyć ograniczenia trasy niestandardowe zaimplementowanie **IHttpRouteConstraint** interfejsu.</span><span class="sxs-lookup"><span data-stu-id="73992-243">You can create custom route constraints by implementing the **IHttpRouteConstraint** interface.</span></span> <span data-ttu-id="73992-244">Na przykład następujące ograniczenia ogranicza parametr na wartość niezerową liczbą całkowitą.</span><span class="sxs-lookup"><span data-stu-id="73992-244">For example, the following constraint restricts a parameter to a non-zero integer value.</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample16.cs)]

<span data-ttu-id="73992-245">Poniższy kod przedstawia sposób rejestrowania ograniczenia:</span><span class="sxs-lookup"><span data-stu-id="73992-245">The following code shows how to register the constraint:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample17.cs)]

<span data-ttu-id="73992-246">Teraz można zastosować ograniczenia trasy:</span><span class="sxs-lookup"><span data-stu-id="73992-246">Now you can apply the constraint in your routes:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample18.cs)]

<span data-ttu-id="73992-247">Można również zastąpić całą **DefaultInlineConstraintResolver** klasy zaimplementowanie **IInlineConstraintResolver** interfejsu.</span><span class="sxs-lookup"><span data-stu-id="73992-247">You can also replace the entire **DefaultInlineConstraintResolver** class by implementing the **IInlineConstraintResolver** interface.</span></span> <span data-ttu-id="73992-248">W ten sposób spowoduje zastąpienie wszystkich wbudowane ograniczenia, chyba że implementacji **IInlineConstraintResolver** specjalnie dodaje je.</span><span class="sxs-lookup"><span data-stu-id="73992-248">Doing so will replace all of the built-in constraints, unless your implementation of **IInlineConstraintResolver** specifically adds them.</span></span>

<a id="optional"></a>
## <a name="optional-uri-parameters-and-default-values"></a><span data-ttu-id="73992-249">Parametry opcjonalne identyfikatora URI i wartości domyślnych</span><span class="sxs-lookup"><span data-stu-id="73992-249">Optional URI Parameters and Default Values</span></span>

<span data-ttu-id="73992-250">Możesz wprowadzić parametr URI opcjonalne, dodając znak zapytania do parametru trasy.</span><span class="sxs-lookup"><span data-stu-id="73992-250">You can make a URI parameter optional by adding a question mark to the route parameter.</span></span> <span data-ttu-id="73992-251">Jeśli parametr trasy jest opcjonalny, należy zdefiniować wartości domyślnej dla parametru metody.</span><span class="sxs-lookup"><span data-stu-id="73992-251">If a route parameter is optional, you must define a default value for the method parameter.</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample19.cs)]

<span data-ttu-id="73992-252">W tym przykładzie `/api/books/locale/1033` i `/api/books/locale` zwrócić tego samego zasobu.</span><span class="sxs-lookup"><span data-stu-id="73992-252">In this example, `/api/books/locale/1033` and `/api/books/locale` return the same resource.</span></span>

<span data-ttu-id="73992-253">Alternatywnie można określić wartość domyślną w szablonie trasy w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="73992-253">Alternatively, you can specify a default value inside the route template, as follows:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample20.cs)]

<span data-ttu-id="73992-254">To jest prawie takie same, co w poprzednim przykładzie, ale istnieje niewielka różnica zachowania, gdy wartością domyślną jest stosowany.</span><span class="sxs-lookup"><span data-stu-id="73992-254">This is almost the same as the previous example, but there is a slight difference of behavior when the default value is applied.</span></span>

- <span data-ttu-id="73992-255">W pierwszym przykładzie ("{lcid?}") wartość domyślna 1033 jest przypisywane bezpośrednio do parametru metody, tak aby było to dokładną wartość parametru.</span><span class="sxs-lookup"><span data-stu-id="73992-255">In the first example ("{lcid?}"), the default value of 1033 is assigned directly to the method parameter, so the parameter will have this exact value.</span></span>
- <span data-ttu-id="73992-256">W drugim przykładzie ("{lcid = 1033}"), wartość domyślna "1033" przechodzi przez proces tworzenia powiązania modelu.</span><span class="sxs-lookup"><span data-stu-id="73992-256">In the second example ("{lcid=1033}"), the default value of "1033" goes through the model-binding process.</span></span> <span data-ttu-id="73992-257">Wartość liczbowa 1033 przekonwertuje "1033" domyślnego-integratora modelu.</span><span class="sxs-lookup"><span data-stu-id="73992-257">The default model-binder will convert "1033" to the numeric value 1033.</span></span> <span data-ttu-id="73992-258">Można jednak dodatku niestandardowego integratora modelu, co może zrobić coś innego.</span><span class="sxs-lookup"><span data-stu-id="73992-258">However, you could plug in a custom model binder, which might do something different.</span></span>

<span data-ttu-id="73992-259">(W większości przypadków, chyba że masz integratorów modeli niestandardowych w potoku sieci dwa formularze będą równoważne).</span><span class="sxs-lookup"><span data-stu-id="73992-259">(In most cases, unless you have custom model binders in your pipeline, the two forms will be equivalent.)</span></span>

<a id="route-names"></a>
## <a name="route-names"></a><span data-ttu-id="73992-260">Nazwy tras</span><span class="sxs-lookup"><span data-stu-id="73992-260">Route Names</span></span>

<span data-ttu-id="73992-261">W składniku Web API każdy ma nazwę.</span><span class="sxs-lookup"><span data-stu-id="73992-261">In Web API, every route has a name.</span></span> <span data-ttu-id="73992-262">Nazwy trasy są przydatne podczas generowania łączy, tak, aby można uwzględnić łącze w odpowiedzi HTTP.</span><span class="sxs-lookup"><span data-stu-id="73992-262">Route names are useful for generating links, so that you can include a link in an HTTP response.</span></span>

<span data-ttu-id="73992-263">Aby określić nazwę trasy, ustaw **nazwa** właściwości atrybutu.</span><span class="sxs-lookup"><span data-stu-id="73992-263">To specify the route name, set the **Name** property on the attribute.</span></span> <span data-ttu-id="73992-264">Poniższy przykład przedstawia sposób ustawiania nazwy trasy i sposobu użycia nazwy trasy podczas generowania łącza.</span><span class="sxs-lookup"><span data-stu-id="73992-264">The following example shows how to set the route name, and also how to use the route name when generating a link.</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample21.cs)]

<a id="order"></a>
## <a name="route-order"></a><span data-ttu-id="73992-265">Kolejność trasy</span><span class="sxs-lookup"><span data-stu-id="73992-265">Route Order</span></span>

<span data-ttu-id="73992-266">Próba URI trasa pasuje przez platformę oblicza trasy w określonej kolejności.</span><span class="sxs-lookup"><span data-stu-id="73992-266">When the framework tries to match a URI with a route, it evaluates the routes in a particular order.</span></span> <span data-ttu-id="73992-267">Aby określić kolejność, ustaw **RouteOrder** właściwość atrybut trasy.</span><span class="sxs-lookup"><span data-stu-id="73992-267">To specify the order, set the **RouteOrder** property on the route attribute.</span></span> <span data-ttu-id="73992-268">Niższe wartości są sprawdzane jako pierwsze.</span><span class="sxs-lookup"><span data-stu-id="73992-268">Lower values are evaluated first.</span></span> <span data-ttu-id="73992-269">Wartość domyślna kolejności wynosi zero.</span><span class="sxs-lookup"><span data-stu-id="73992-269">The default order value is zero.</span></span>

<span data-ttu-id="73992-270">Oto, jak całkowita kolejność jest określana:</span><span class="sxs-lookup"><span data-stu-id="73992-270">Here is how the total ordering is determined:</span></span>

1. <span data-ttu-id="73992-271">Porównaj **RouteOrder** właściwości atrybutu trasy.</span><span class="sxs-lookup"><span data-stu-id="73992-271">Compare the **RouteOrder** property of the route attribute.</span></span>
2. <span data-ttu-id="73992-272">Szukaj w każdym segmencie identyfikatora URI w szablonie trasy.</span><span class="sxs-lookup"><span data-stu-id="73992-272">Look at each URI segment in the route template.</span></span> <span data-ttu-id="73992-273">Dla każdego segmentu kolejność w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="73992-273">For each segment, order as follows:</span></span> 

    1. <span data-ttu-id="73992-274">Literał segmentów.</span><span class="sxs-lookup"><span data-stu-id="73992-274">Literal segments.</span></span>
    2. <span data-ttu-id="73992-275">Parametry trasy z ograniczeniami.</span><span class="sxs-lookup"><span data-stu-id="73992-275">Route parameters with constraints.</span></span>
    3. <span data-ttu-id="73992-276">Parametry trasy bez ograniczeń.</span><span class="sxs-lookup"><span data-stu-id="73992-276">Route parameters without constraints.</span></span>
    4. <span data-ttu-id="73992-277">Symboli wieloznacznych parametrem segmentów z ograniczeniami.</span><span class="sxs-lookup"><span data-stu-id="73992-277">Wildcard parameter segments with constraints.</span></span>
    5. <span data-ttu-id="73992-278">Symbol wieloznaczny parametrów segmenty bez ograniczeń.</span><span class="sxs-lookup"><span data-stu-id="73992-278">Wildcard parameter segments without constraints.</span></span>
3. <span data-ttu-id="73992-279">W przypadku zostanie rozwiązany, trasy są uporządkowane według porównania ciągów porządkowych bez uwzględniania wielkości liter ([OrdinalIgnoreCase](https://msdn.microsoft.com/en-us/library/system.stringcomparer.ordinalignorecase.aspx)) szablonu trasy.</span><span class="sxs-lookup"><span data-stu-id="73992-279">In the case of a tie, routes are ordered by a case-insensitive ordinal string comparison ([OrdinalIgnoreCase](https://msdn.microsoft.com/en-us/library/system.stringcomparer.ordinalignorecase.aspx)) of the route template.</span></span>

<span data-ttu-id="73992-280">Oto przykład.</span><span class="sxs-lookup"><span data-stu-id="73992-280">Here is an example.</span></span> <span data-ttu-id="73992-281">Załóżmy, że zdefiniujesz następujący kontroler:</span><span class="sxs-lookup"><span data-stu-id="73992-281">Suppose you define the following controller:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample22.cs)]

<span data-ttu-id="73992-282">Te trasy są sortowane w następujący sposób.</span><span class="sxs-lookup"><span data-stu-id="73992-282">These routes are ordered as follows.</span></span>

1. <span data-ttu-id="73992-283">Szczegóły/zamówień</span><span class="sxs-lookup"><span data-stu-id="73992-283">orders/details</span></span>
2. <span data-ttu-id="73992-284">zamówienia / {id}</span><span class="sxs-lookup"><span data-stu-id="73992-284">orders/{id}</span></span>
3. <span data-ttu-id="73992-285">zamówienia / {customerName}</span><span class="sxs-lookup"><span data-stu-id="73992-285">orders/{customerName}</span></span>
4. <span data-ttu-id="73992-286">zamówienia / {\*Data}</span><span class="sxs-lookup"><span data-stu-id="73992-286">orders/{\*date}</span></span>
5. <span data-ttu-id="73992-287">zamówienia / oczekujące</span><span class="sxs-lookup"><span data-stu-id="73992-287">orders/pending</span></span>

<span data-ttu-id="73992-288">Powiadomienie, że "szczegóły" jest segmentem literału i pojawia się przed "{id}", ale "oczekujące" pojawia się ostatnio ponieważ **RouteOrder** właściwość jest 1.</span><span class="sxs-lookup"><span data-stu-id="73992-288">Notice that "details" is a literal segment and appears before "{id}", but "pending" appears last because the **RouteOrder** property is 1.</span></span> <span data-ttu-id="73992-289">(W tym przykładzie przyjęto założenie, są Brak odbiorców o nazwie "szczegóły" lub "oczekujące".</span><span class="sxs-lookup"><span data-stu-id="73992-289">(This example assumes there are no customers named "details" or "pending".</span></span> <span data-ttu-id="73992-290">Ogólnie rzecz biorąc należy unikać niejednoznaczne trasy.</span><span class="sxs-lookup"><span data-stu-id="73992-290">In general, try to avoid ambiguous routes.</span></span> <span data-ttu-id="73992-291">W tym przykładzie lepsze szablon trasy dla `GetByCustomer` jest "klienci / {customerName}")</span><span class="sxs-lookup"><span data-stu-id="73992-291">In this example, a better route template for `GetByCustomer` is "customers/{customerName}" )</span></span>
