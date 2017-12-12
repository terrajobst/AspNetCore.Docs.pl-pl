---
uid: web-api/overview/web-api-routing-and-actions/routing-in-aspnet-web-api
title: "Routing w składniku ASP.NET Web API | Dokumentacja firmy Microsoft"
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/11/2012
ms.topic: article
ms.assetid: 0675bdc7-282f-4f47-b7f3-7e02133940ca
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/web-api-routing-and-actions/routing-in-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: aa0ecc96029051fef6a81ac08f7fcf52a24de59c
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
<a name="routing-in-aspnet-web-api"></a><span data-ttu-id="a0869-102">Routing w składniku ASP.NET Web API</span><span class="sxs-lookup"><span data-stu-id="a0869-102">Routing in ASP.NET Web API</span></span>
====================
<span data-ttu-id="a0869-103">przez [Wasson Jan](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="a0869-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="a0869-104">W tym artykule opisano, jak składnika ASP.NET Web API kieruje żądania HTTP do kontrolerów.</span><span class="sxs-lookup"><span data-stu-id="a0869-104">This article describes how ASP.NET Web API routes HTTP requests to controllers.</span></span>

> [!NOTE]
> <span data-ttu-id="a0869-105">Jeśli znasz platformy ASP.NET MVC, Web API routingu jest bardzo podobny do routingu MVC.</span><span class="sxs-lookup"><span data-stu-id="a0869-105">If you are familiar with ASP.NET MVC, Web API routing is very similar to MVC routing.</span></span> <span data-ttu-id="a0869-106">Główna różnica polega na tym, że interfejsu API sieci Web używa metody HTTP, nie ścieżka identyfikatora URI, aby wybrać akcję.</span><span class="sxs-lookup"><span data-stu-id="a0869-106">The main difference is that Web API uses the HTTP method, not the URI path, to select the action.</span></span> <span data-ttu-id="a0869-107">Umożliwia również MVC stylu routingu w składniku Web API.</span><span class="sxs-lookup"><span data-stu-id="a0869-107">You can also use MVC-style routing in Web API.</span></span> <span data-ttu-id="a0869-108">W tym artykule nie przyjmuje żadnych wiedzy na temat platformy ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="a0869-108">This article does not assume any knowledge of ASP.NET MVC.</span></span>


## <a name="routing-tables"></a><span data-ttu-id="a0869-109">Tabele routingu</span><span class="sxs-lookup"><span data-stu-id="a0869-109">Routing Tables</span></span>

<span data-ttu-id="a0869-110">W interfejsie API sieci Web ASP.NET *kontrolera* jest klasa, która obsługuje żądania HTTP.</span><span class="sxs-lookup"><span data-stu-id="a0869-110">In ASP.NET Web API, a *controller* is a class that handles HTTP requests.</span></span> <span data-ttu-id="a0869-111">Metody publiczne kontrolera są nazywane *metod akcji* lub po prostu *akcje*.</span><span class="sxs-lookup"><span data-stu-id="a0869-111">The public methods of the controller are called *action methods* or simply *actions*.</span></span> <span data-ttu-id="a0869-112">Gdy strukturę interfejsu API sieci Web odbiera żądanie, kieruje żądanie do akcji.</span><span class="sxs-lookup"><span data-stu-id="a0869-112">When the Web API framework receives a request, it routes the request to an action.</span></span>

<span data-ttu-id="a0869-113">Aby ustalić, jakie działania należy wywołać, używa platformę *tabeli routingu*.</span><span class="sxs-lookup"><span data-stu-id="a0869-113">To determine which action to invoke, the framework uses a *routing table*.</span></span> <span data-ttu-id="a0869-114">Szablon projektu Visual Studio dla interfejsu API sieci Web tworzy trasę domyślną:</span><span class="sxs-lookup"><span data-stu-id="a0869-114">The Visual Studio project template for Web API creates a default route:</span></span>

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample1.cs)]

<span data-ttu-id="a0869-115">Ta trasa jest zdefiniowana w pliku WebApiConfig.cs, który znajduje się w aplikacji\_Start katalogu:</span><span class="sxs-lookup"><span data-stu-id="a0869-115">This route is defined in the WebApiConfig.cs file, which is placed in the App\_Start directory:</span></span>

![](routing-in-aspnet-web-api/_static/image1.png)

<span data-ttu-id="a0869-116">Aby uzyskać więcej informacji na temat **WebApiConfig** , zobacz [konfigurowania składnika ASP.NET Web API](../advanced/configuring-aspnet-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="a0869-116">For more information about the **WebApiConfig** class, see [Configuring ASP.NET Web API](../advanced/configuring-aspnet-web-api.md).</span></span>

<span data-ttu-id="a0869-117">Jeśli samodzielną interfejsu API sieci Web, musisz ustawić tabeli routingu bezpośrednio na **HttpSelfHostConfiguration** obiektu.</span><span class="sxs-lookup"><span data-stu-id="a0869-117">If you self-host Web API, you must set the routing table directly on the **HttpSelfHostConfiguration** object.</span></span> <span data-ttu-id="a0869-118">Aby uzyskać więcej informacji, zobacz [interfejs API sieci Web hosta samodzielnego](../older-versions/self-host-a-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="a0869-118">For more information, see [Self-Host a Web API](../older-versions/self-host-a-web-api.md).</span></span>

<span data-ttu-id="a0869-119">Każdy wpis w tabeli routingu zawiera *szablon trasy*.</span><span class="sxs-lookup"><span data-stu-id="a0869-119">Each entry in the routing table contains a *route template*.</span></span> <span data-ttu-id="a0869-120">Domyślny szablon trasy dla interfejsu API sieci Web jest &quot;interfejsu api / {controller} / {id}&quot;.</span><span class="sxs-lookup"><span data-stu-id="a0869-120">The default route template for Web API is &quot;api/{controller}/{id}&quot;.</span></span> <span data-ttu-id="a0869-121">W tym szablonie &quot;interfejsu api&quot; jest segment ścieżki literału i {controller} i {id} są zmiennymi symbolu zastępczego.</span><span class="sxs-lookup"><span data-stu-id="a0869-121">In this template, &quot;api&quot; is a literal path segment, and {controller} and {id} are placeholder variables.</span></span>

<span data-ttu-id="a0869-122">Gdy strukturę interfejsu API sieci Web otrzymuje żądanie HTTP, próbuje pasuje do identyfikatora URI na jednym z szablonów tras w tabeli routingu.</span><span class="sxs-lookup"><span data-stu-id="a0869-122">When the Web API framework receives an HTTP request, it tries to match the URI against one of the route templates in the routing table.</span></span> <span data-ttu-id="a0869-123">Jeśli żadna trasa nie pasuje, klient odbierze błąd 404.</span><span class="sxs-lookup"><span data-stu-id="a0869-123">If no route matches, the client receives a 404 error.</span></span> <span data-ttu-id="a0869-124">Na przykład następujące identyfikatory URI dopasowania trasy domyślnej:</span><span class="sxs-lookup"><span data-stu-id="a0869-124">For example, the following URIs match the default route:</span></span>

- <span data-ttu-id="a0869-125">/ api/contacts</span><span class="sxs-lookup"><span data-stu-id="a0869-125">/api/contacts</span></span>
- <span data-ttu-id="a0869-126">/API/Contacts/1</span><span class="sxs-lookup"><span data-stu-id="a0869-126">/api/contacts/1</span></span>
- <span data-ttu-id="a0869-127">/API/Products/gizmo1</span><span class="sxs-lookup"><span data-stu-id="a0869-127">/api/products/gizmo1</span></span>

<span data-ttu-id="a0869-128">Jednak następujący identyfikator URI nie jest zgodny, ponieważ brakuje w nim &quot;interfejsu api&quot; segmentu:</span><span class="sxs-lookup"><span data-stu-id="a0869-128">However, the following URI does not match, because it lacks the &quot;api&quot; segment:</span></span>

- <span data-ttu-id="a0869-129">/ Kontakty/1</span><span class="sxs-lookup"><span data-stu-id="a0869-129">/contacts/1</span></span>

> [!NOTE]
> <span data-ttu-id="a0869-130">Powodem przy użyciu "interfejsu api" w trasie jest uniknąć kolizji z routing na platformie ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="a0869-130">The reason for using "api" in the route is to avoid collisions with ASP.NET MVC routing.</span></span> <span data-ttu-id="a0869-131">Dzięki temu można mieć &quot;/kontaktuje się&quot; przejdź do kontrolera MVC i &quot;/api/contacts&quot; przejdź do kontrolera interfejsu API sieci Web.</span><span class="sxs-lookup"><span data-stu-id="a0869-131">That way, you can have &quot;/contacts&quot; go to an MVC controller, and &quot;/api/contacts&quot; go to a Web API controller.</span></span> <span data-ttu-id="a0869-132">Oczywiście jeśli ta Konwencja nie podoba, można zmienić tabeli trasy domyślnej.</span><span class="sxs-lookup"><span data-stu-id="a0869-132">Of course, if you don't like this convention, you can change the default route table.</span></span>

<span data-ttu-id="a0869-133">Po znalezieniu pasującej trasy interfejsu API sieci Web wybiera kontroler i akcję:</span><span class="sxs-lookup"><span data-stu-id="a0869-133">Once a matching route is found, Web API selects the controller and the action:</span></span>

- <span data-ttu-id="a0869-134">Aby odnaleźć kontrolera, interfejsu API sieci Web dodaje &quot;kontrolera&quot; wartość *{controller}* zmiennej.</span><span class="sxs-lookup"><span data-stu-id="a0869-134">To find the controller, Web API adds &quot;Controller&quot; to the value of the *{controller}* variable.</span></span>
- <span data-ttu-id="a0869-135">Można znaleźć akcji, interfejsu API sieci Web analizuje metodę HTTP, a następnie szuka akcji, których nazwa rozpoczyna się z tą nazwą metody HTTP.</span><span class="sxs-lookup"><span data-stu-id="a0869-135">To find the action, Web API looks at the HTTP method, and then looks for an action whose name begins with that HTTP method name.</span></span> <span data-ttu-id="a0869-136">Na przykład z żądania GET, interfejsu API sieci Web szuka akcję, która rozpoczyna się od &quot;uzyskać... &quot;, takich jak &quot;GetContact&quot; lub &quot;GetAllContacts&quot;.</span><span class="sxs-lookup"><span data-stu-id="a0869-136">For example, with a GET request, Web API looks for an action that starts with &quot;Get...&quot;, such as &quot;GetContact&quot; or &quot;GetAllContacts&quot;.</span></span> <span data-ttu-id="a0869-137">Konwencja dotyczy tylko GET, POST, PUT i DELETE metody.</span><span class="sxs-lookup"><span data-stu-id="a0869-137">This convention applies only to GET, POST, PUT, and DELETE methods.</span></span> <span data-ttu-id="a0869-138">Inne metody HTTP można włączyć za pomocą atrybutów kontrolera.</span><span class="sxs-lookup"><span data-stu-id="a0869-138">You can enable other HTTP methods by using attributes on your controller.</span></span> <span data-ttu-id="a0869-139">Firma Microsoft będzie Zobacz przykład tego później.</span><span class="sxs-lookup"><span data-stu-id="a0869-139">We'll see an example of that later.</span></span>
- <span data-ttu-id="a0869-140">Inne zmienne symbolu zastępczego w szablonie trasy, takich jak *{id}* są mapowane na parametry akcji.</span><span class="sxs-lookup"><span data-stu-id="a0869-140">Other placeholder variables in the route template, such as *{id},* are mapped to action parameters.</span></span>

<span data-ttu-id="a0869-141">Oto przykład.</span><span class="sxs-lookup"><span data-stu-id="a0869-141">Let's look at an example.</span></span> <span data-ttu-id="a0869-142">Załóżmy, że zdefiniujesz następujący kontroler:</span><span class="sxs-lookup"><span data-stu-id="a0869-142">Suppose that you define the following controller:</span></span>

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample2.cs)]

<span data-ttu-id="a0869-143">Poniżej przedstawiono niektóre możliwe żądania HTTP wraz z akcji, które pobiera wywoływane dla każdego:</span><span class="sxs-lookup"><span data-stu-id="a0869-143">Here are some possible HTTP requests, along with the action that gets invoked for each:</span></span>

| <span data-ttu-id="a0869-144">Metoda HTTP</span><span class="sxs-lookup"><span data-stu-id="a0869-144">HTTP Method</span></span> | <span data-ttu-id="a0869-145">Ścieżka identyfikatora URI</span><span class="sxs-lookup"><span data-stu-id="a0869-145">URI Path</span></span> | <span data-ttu-id="a0869-146">Akcja</span><span class="sxs-lookup"><span data-stu-id="a0869-146">Action</span></span> | <span data-ttu-id="a0869-147">Parametr</span><span class="sxs-lookup"><span data-stu-id="a0869-147">Parameter</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="a0869-148">POBIERZ</span><span class="sxs-lookup"><span data-stu-id="a0869-148">GET</span></span> | <span data-ttu-id="a0869-149">Interfejs API/produktów</span><span class="sxs-lookup"><span data-stu-id="a0869-149">api/products</span></span> | <span data-ttu-id="a0869-150">GetAllProducts</span><span class="sxs-lookup"><span data-stu-id="a0869-150">GetAllProducts</span></span> | <span data-ttu-id="a0869-151">*(Brak)*</span><span class="sxs-lookup"><span data-stu-id="a0869-151">*(none)*</span></span> |
| <span data-ttu-id="a0869-152">POBIERZ</span><span class="sxs-lookup"><span data-stu-id="a0869-152">GET</span></span> | <span data-ttu-id="a0869-153">4-API/produktów</span><span class="sxs-lookup"><span data-stu-id="a0869-153">api/products/4</span></span> | <span data-ttu-id="a0869-154">GetProductById</span><span class="sxs-lookup"><span data-stu-id="a0869-154">GetProductById</span></span> | <span data-ttu-id="a0869-155">4</span><span class="sxs-lookup"><span data-stu-id="a0869-155">4</span></span> |
| <span data-ttu-id="a0869-156">DELETE</span><span class="sxs-lookup"><span data-stu-id="a0869-156">DELETE</span></span> | <span data-ttu-id="a0869-157">4-API/produktów</span><span class="sxs-lookup"><span data-stu-id="a0869-157">api/products/4</span></span> | <span data-ttu-id="a0869-158">DeleteProduct</span><span class="sxs-lookup"><span data-stu-id="a0869-158">DeleteProduct</span></span> | <span data-ttu-id="a0869-159">4</span><span class="sxs-lookup"><span data-stu-id="a0869-159">4</span></span> |
| <span data-ttu-id="a0869-160">POST</span><span class="sxs-lookup"><span data-stu-id="a0869-160">POST</span></span> | <span data-ttu-id="a0869-161">Interfejs API/produktów</span><span class="sxs-lookup"><span data-stu-id="a0869-161">api/products</span></span> | <span data-ttu-id="a0869-162">*(Brak dopasowań)*</span><span class="sxs-lookup"><span data-stu-id="a0869-162">*(no match)*</span></span> |  |

<span data-ttu-id="a0869-163">Zwróć uwagę, że *{id}* segmencie identyfikatora URI, jeśli istnieje, jest mapowany na *identyfikator* parametru akcji.</span><span class="sxs-lookup"><span data-stu-id="a0869-163">Notice that the *{id}* segment of the URI, if present, is mapped to the *id* parameter of the action.</span></span> <span data-ttu-id="a0869-164">W tym przykładzie kontrolera definiuje dwie metody GET, jeden z *identyfikator* parametr i bez parametrów.</span><span class="sxs-lookup"><span data-stu-id="a0869-164">In this example, the controller defines two GET methods, one with an *id* parameter and one with no parameters.</span></span>

<span data-ttu-id="a0869-165">Ponadto Pamiętaj, że żądanie POST zakończy się niepowodzeniem, ponieważ kontroler nie definiuje &quot;Post... &quot; metody.</span><span class="sxs-lookup"><span data-stu-id="a0869-165">Also, note that the POST request will fail, because the controller does not define a &quot;Post...&quot; method.</span></span>

## <a name="routing-variations"></a><span data-ttu-id="a0869-166">Zmiany routingu</span><span class="sxs-lookup"><span data-stu-id="a0869-166">Routing Variations</span></span>

<span data-ttu-id="a0869-167">Poprzedniej sekcji opisano podstawowego mechanizmu routingu dla interfejsu API sieci Web platformy ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="a0869-167">The previous section described the basic routing mechanism for ASP.NET Web API.</span></span> <span data-ttu-id="a0869-168">W tej sekcji opisano różne wersje.</span><span class="sxs-lookup"><span data-stu-id="a0869-168">This section describes some variations.</span></span>

### <a name="http-methods"></a><span data-ttu-id="a0869-169">Metody HTTP</span><span class="sxs-lookup"><span data-stu-id="a0869-169">HTTP Methods</span></span>

<span data-ttu-id="a0869-170">Zamiast używać konwencji nazewnictwa dla metod HTTP, zostaną jawnie określone metody HTTP dla akcji przez metodę akcji za pomocą dekoracji **HttpGet**, **HttpPut**, **HttpPost** , lub **HttpDelete** atrybutu.</span><span class="sxs-lookup"><span data-stu-id="a0869-170">Instead of using the naming convention for HTTP methods, you can explicitly specify the HTTP method for an action by decorating the action method with the **HttpGet**, **HttpPut**, **HttpPost**, or **HttpDelete** attribute.</span></span>

<span data-ttu-id="a0869-171">W poniższym przykładzie metoda FindProduct jest mapowany na żądania GET:</span><span class="sxs-lookup"><span data-stu-id="a0869-171">In the following example, the FindProduct method is mapped to GET requests:</span></span>

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample3.cs)]

<span data-ttu-id="a0869-172">Zezwalaj na wiele metod HTTP dla akcji lub Zezwalaj metod HTTP innych niż GET, PUT, POST i DELETE, należy użyć **AcceptVerbs** atrybut, który przyjmuje listę metod HTTP.</span><span class="sxs-lookup"><span data-stu-id="a0869-172">To allow multiple HTTP methods for an action, or to allow HTTP methods other than GET, PUT, POST, and DELETE, use the **AcceptVerbs** attribute, which takes a list of HTTP methods.</span></span>

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample4.cs)]

<a id="routing_by_action_name"></a>
### <a name="routing-by-action-name"></a><span data-ttu-id="a0869-173">Routing według nazwy akcji</span><span class="sxs-lookup"><span data-stu-id="a0869-173">Routing by Action Name</span></span>

<span data-ttu-id="a0869-174">Z szablon routingu domyślnego interfejsu API sieci Web używa metody HTTP, aby wybrać akcję.</span><span class="sxs-lookup"><span data-stu-id="a0869-174">With the default routing template, Web API uses the HTTP method to select the action.</span></span> <span data-ttu-id="a0869-175">Można jednak również utworzyć trasy, gdzie nazwa akcji jest dołączona do identyfikatora URI:</span><span class="sxs-lookup"><span data-stu-id="a0869-175">However, you can also create a route where the action name is included in the URI:</span></span>

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample5.cs)]

<span data-ttu-id="a0869-176">W tym szablonie trasy *{action}* nazwy parametrów metody akcji kontrolera.</span><span class="sxs-lookup"><span data-stu-id="a0869-176">In this route template, the *{action}* parameter names the action method on the controller.</span></span> <span data-ttu-id="a0869-177">Z tym stylem routingu umożliwia określenie dopuszczalnych metod HTTP atrybutów.</span><span class="sxs-lookup"><span data-stu-id="a0869-177">With this style of routing, use attributes to specify the allowed HTTP methods.</span></span> <span data-ttu-id="a0869-178">Na przykład załóżmy, że dany kontroler ma następującą metodę:</span><span class="sxs-lookup"><span data-stu-id="a0869-178">For example, suppose your controller has the following method:</span></span>

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample6.cs)]

<span data-ttu-id="a0869-179">W takim przypadku żądania GET "interfejsu api/produkty/szczegóły/1" spowoduje mapy do metody szczegóły.</span><span class="sxs-lookup"><span data-stu-id="a0869-179">In this case, a GET request for "api/products/details/1" would map to the Details method.</span></span> <span data-ttu-id="a0869-180">Ten styl routingu jest podobny do platformy ASP.NET MVC i może być odpowiednie dla interfejsu API w stylu wywołania RPC.</span><span class="sxs-lookup"><span data-stu-id="a0869-180">This style of routing is similar to ASP.NET MVC, and may be appropriate for an RPC-style API.</span></span>

<span data-ttu-id="a0869-181">Nazwa akcji można zastąpić przy użyciu **Nazwa akcji** atrybutu.</span><span class="sxs-lookup"><span data-stu-id="a0869-181">You can override the action name by using the **ActionName** attribute.</span></span> <span data-ttu-id="a0869-182">W poniższym przykładzie są dwie akcje, które mapują &quot;interfejsu api/produkty/miniatur/*identyfikator*. Obsługuje jeden GET, a druga POST:</span><span class="sxs-lookup"><span data-stu-id="a0869-182">In the following example, there are two actions that map to &quot;api/products/thumbnail/*id*. One supports GET and the other supports POST:</span></span>

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample7.cs)]

### <a name="non-actions"></a><span data-ttu-id="a0869-183">Akcje z systemem innym niż</span><span class="sxs-lookup"><span data-stu-id="a0869-183">Non-Actions</span></span>

<span data-ttu-id="a0869-184">Aby zapobiec metodę pobierania wywołany jako akcję, należy użyć **NonAction** atrybutu.</span><span class="sxs-lookup"><span data-stu-id="a0869-184">To prevent a method from getting invoked as an action, use the **NonAction** attribute.</span></span> <span data-ttu-id="a0869-185">To sygnały do struktury czy metoda nie jest akcją, nawet jeśli w przeciwnym razie dopasuje reguły routingu.</span><span class="sxs-lookup"><span data-stu-id="a0869-185">This signals to the framework that the method is not an action, even if it would otherwise match the routing rules.</span></span>

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample8.cs)]

## <a name="further-reading"></a><span data-ttu-id="a0869-186">Dalsze informacje</span><span class="sxs-lookup"><span data-stu-id="a0869-186">Further Reading</span></span>

<span data-ttu-id="a0869-187">W tym temacie podano Widok ogólny routingu.</span><span class="sxs-lookup"><span data-stu-id="a0869-187">This topic provided a high-level view of routing.</span></span> <span data-ttu-id="a0869-188">Aby uzyskać więcej szczegółów, zobacz [routingu i wybór akcji](routing-and-action-selection.md), która opisuje dokładnie sposób platformę pasuje identyfikator URI do trasy, wybiera kontrolera, a następnie wybiera akcję do wywołania.</span><span class="sxs-lookup"><span data-stu-id="a0869-188">For more detail, see [Routing and Action Selection](routing-and-action-selection.md), which describes exactly how the framework matches a URI to a route, selects a controller, and then selects the action to invoke.</span></span>
