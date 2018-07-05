---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v3/calling-an-odata-service-from-a-net-client
title: Wywoływanie usługi protokołu OData z klienta programu .NET (C#) | Dokumentacja firmy Microsoft
author: MikeWasson
description: W tym samouczku pokazano, jak wywołać usługę OData z poziomu aplikacji klienckiej języka C#. Wersje oprogramowania używany w samouczku programu Visual Studio 2013 (działa Visual S...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/26/2014
ms.topic: article
ms.assetid: 6f448917-ad23-4dcc-9789-897fad74051b
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v3/calling-an-odata-service-from-a-net-client
msc.type: authoredcontent
ms.openlocfilehash: d987e7fbe737055b3e2b690ef3e8de5ca7e2b937
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37369786"
---
<a name="calling-an-odata-service-from-a-net-client-c"></a><span data-ttu-id="20ef6-104">Wywoływanie usługi protokołu OData z klienta programu .NET (C#)</span><span class="sxs-lookup"><span data-stu-id="20ef6-104">Calling an OData Service From a .NET Client (C#)</span></span>
====================
<span data-ttu-id="20ef6-105">przez [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="20ef6-105">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="20ef6-106">Pobieranie ukończone projektu</span><span class="sxs-lookup"><span data-stu-id="20ef6-106">Download Completed Project</span></span>](http://code.msdn.microsoft.com/ASPNET-Web-API-OData-cecdb524)

> <span data-ttu-id="20ef6-107">W tym samouczku pokazano, jak wywołać usługę OData z poziomu aplikacji klienckiej języka C#.</span><span class="sxs-lookup"><span data-stu-id="20ef6-107">This tutorial shows how to call an OData service from a C# client application.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="20ef6-108">Wersje oprogramowania używanego w tym samouczku</span><span class="sxs-lookup"><span data-stu-id="20ef6-108">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="20ef6-109">[Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads) (w programach Visual Studio 2012)</span><span class="sxs-lookup"><span data-stu-id="20ef6-109">[Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads) (works with Visual Studio 2012)</span></span>
> - [<span data-ttu-id="20ef6-110">Biblioteka klienta usług danych WCF</span><span class="sxs-lookup"><span data-stu-id="20ef6-110">WCF Data Services Client Library</span></span>](https://msdn.microsoft.com/library/cc668772.aspx)
> - <span data-ttu-id="20ef6-111">Interfejsu Web API 2.</span><span class="sxs-lookup"><span data-stu-id="20ef6-111">Web API 2.</span></span> <span data-ttu-id="20ef6-112">(Przykład usługi OData został skompilowany przy użyciu Web API 2, ale aplikacja kliencka nie zależy od interfejsu API sieci Web).</span><span class="sxs-lookup"><span data-stu-id="20ef6-112">(The example OData service is built using Web API 2, but the client application does not depend on Web API.)</span></span>


<span data-ttu-id="20ef6-113">W ramach tego samouczka I zawiera opis kroków tworzenia aplikacji klienckiej, która wywołuje usługę OData.</span><span class="sxs-lookup"><span data-stu-id="20ef6-113">In this tutorial, I'll walk through creating a client application that calls an OData service.</span></span> <span data-ttu-id="20ef6-114">Usługa OData przedstawia następujących elementach:</span><span class="sxs-lookup"><span data-stu-id="20ef6-114">The OData service exposes the following entities:</span></span>

- `Product`
- `Supplier`
- `ProductRating`

![](calling-an-odata-service-from-a-net-client/_static/image1.png)

<span data-ttu-id="20ef6-115">Następujące artykuły zawierają instrukcje dotyczące wdrażania usługi OData w interfejsie API sieci Web.</span><span class="sxs-lookup"><span data-stu-id="20ef6-115">The following articles describe how to implement the OData service in Web API.</span></span> <span data-ttu-id="20ef6-116">(Nie trzeba ich zrozumienie tego samouczka, jednak odczytać).</span><span class="sxs-lookup"><span data-stu-id="20ef6-116">(You don't need to read them to understand this tutorial, however.)</span></span>

- [<span data-ttu-id="20ef6-117">Tworzenie punktu końcowego OData w interfejsie Web API 2</span><span class="sxs-lookup"><span data-stu-id="20ef6-117">Creating an OData Endpoint in Web API 2</span></span>](creating-an-odata-endpoint.md)
- [<span data-ttu-id="20ef6-118">Relacje jednostek OData w interfejsie Web API 2</span><span class="sxs-lookup"><span data-stu-id="20ef6-118">OData Entity Relations in Web API 2</span></span>](working-with-entity-relations.md)
- [<span data-ttu-id="20ef6-119">Akcje protokołu OData w interfejsie Web API 2</span><span class="sxs-lookup"><span data-stu-id="20ef6-119">OData Actions in Web API 2</span></span>](odata-actions.md)

## <a name="generate-the-service-proxy"></a><span data-ttu-id="20ef6-120">Generowanie usługi serwera Proxy</span><span class="sxs-lookup"><span data-stu-id="20ef6-120">Generate the Service Proxy</span></span>

<span data-ttu-id="20ef6-121">Pierwszym krokiem jest generowanie usługi serwera proxy.</span><span class="sxs-lookup"><span data-stu-id="20ef6-121">The first step is to generate a service proxy.</span></span> <span data-ttu-id="20ef6-122">Serwer proxy usługi jest klasą .NET, która definiuje metody uzyskiwania dostępu do usługi OData.</span><span class="sxs-lookup"><span data-stu-id="20ef6-122">The service proxy is a .NET class that defines methods for accessing the OData service.</span></span> <span data-ttu-id="20ef6-123">Serwer proxy tłumaczy wywołania metody na żądania HTTP.</span><span class="sxs-lookup"><span data-stu-id="20ef6-123">The proxy translates method calls into HTTP requests.</span></span>

![](calling-an-odata-service-from-a-net-client/_static/image2.png)

<span data-ttu-id="20ef6-124">Rozpocznij, otwierając projekt usługi OData w programie Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="20ef6-124">Start by opening the OData service project in Visual Studio.</span></span> <span data-ttu-id="20ef6-125">Naciśnij klawisze CTRL + F5, aby uruchomić usługę lokalnie w usługach IIS Express.</span><span class="sxs-lookup"><span data-stu-id="20ef6-125">Press CTRL+F5 to run the service locally in IIS Express.</span></span> <span data-ttu-id="20ef6-126">Należy pamiętać adresów lokalnych, w tym numer portu, który przypisuje programu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="20ef6-126">Note the local address, including the port number that Visual Studio assigns.</span></span> <span data-ttu-id="20ef6-127">Podczas tworzenia serwera proxy, należy ten adres.</span><span class="sxs-lookup"><span data-stu-id="20ef6-127">You will need this address when you create the proxy.</span></span>

<span data-ttu-id="20ef6-128">Następnie otwórz inne wystąpienie programu Visual Studio i Utwórz projekt aplikacji konsoli.</span><span class="sxs-lookup"><span data-stu-id="20ef6-128">Next, open another instance of Visual Studio and create a console application project.</span></span> <span data-ttu-id="20ef6-129">Aplikacja konsoli będzie naszej aplikacji klienckiej OData.</span><span class="sxs-lookup"><span data-stu-id="20ef6-129">The console application will be our OData client application.</span></span> <span data-ttu-id="20ef6-130">(Możesz również dodać projekt do tego samego rozwiązania co usługa.)</span><span class="sxs-lookup"><span data-stu-id="20ef6-130">(You can also add the project to the same solution as the service.)</span></span>

> [!NOTE]
> <span data-ttu-id="20ef6-131">Pozostałe kroki można znaleźć projektu konsoli.</span><span class="sxs-lookup"><span data-stu-id="20ef6-131">The remaining steps refer the console project.</span></span>


<span data-ttu-id="20ef6-132">W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy **odwołania** i wybierz **Dodaj odwołanie do usługi**.</span><span class="sxs-lookup"><span data-stu-id="20ef6-132">In Solution Explorer, right-click **References** and select **Add Service Reference**.</span></span>

![](calling-an-odata-service-from-a-net-client/_static/image3.png)

<span data-ttu-id="20ef6-133">W **Dodaj odwołanie do usługi** okno dialogowe, wpisz adres usługi OData:</span><span class="sxs-lookup"><span data-stu-id="20ef6-133">In the **Add Service Reference** dialog, type the address of the OData service:</span></span>

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample1.cmd)]

<span data-ttu-id="20ef6-134">gdzie *portu* jest numerem portu.</span><span class="sxs-lookup"><span data-stu-id="20ef6-134">where *port* is the port number.</span></span>

[![](calling-an-odata-service-from-a-net-client/_static/image5.png)](calling-an-odata-service-from-a-net-client/_static/image4.png)

<span data-ttu-id="20ef6-135">Aby uzyskać **Namespace**, wpisz "ProductService".</span><span class="sxs-lookup"><span data-stu-id="20ef6-135">For **Namespace**, type "ProductService".</span></span> <span data-ttu-id="20ef6-136">Ta opcja określa przestrzeń nazw, klasy proxy.</span><span class="sxs-lookup"><span data-stu-id="20ef6-136">This option defines the namespace of the proxy class.</span></span>

<span data-ttu-id="20ef6-137">Kliknij przycisk **Przejdź**.</span><span class="sxs-lookup"><span data-stu-id="20ef6-137">Click **Go**.</span></span> <span data-ttu-id="20ef6-138">Program Visual Studio odczytuje dokument metadanych OData, aby odnaleźć obiekty usługi.</span><span class="sxs-lookup"><span data-stu-id="20ef6-138">Visual Studio reads the OData metadata document to discover the entities in the service.</span></span>

[![](calling-an-odata-service-from-a-net-client/_static/image7.png)](calling-an-odata-service-from-a-net-client/_static/image6.png)

<span data-ttu-id="20ef6-139">Kliknij przycisk **OK** można dodać klasy serwera proxy do projektu.</span><span class="sxs-lookup"><span data-stu-id="20ef6-139">Click **OK** to add the proxy class to your project.</span></span>

![](calling-an-odata-service-from-a-net-client/_static/image8.png)

## <a name="create-an-instance-of-the-service-proxy-class"></a><span data-ttu-id="20ef6-140">Utwórz wystąpienie klasy serwera Proxy usługi</span><span class="sxs-lookup"><span data-stu-id="20ef6-140">Create an Instance of the Service Proxy Class</span></span>

<span data-ttu-id="20ef6-141">Wewnątrz swojej `Main` metody, Utwórz nowe wystąpienie klasy serwera proxy w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="20ef6-141">Inside your `Main` method, create a new instance of the proxy class, as follows:</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample2.cs)]

<span data-ttu-id="20ef6-142">Ponownie użyj numeru portu rzeczywiste gdzie usługa jest uruchomiona.</span><span class="sxs-lookup"><span data-stu-id="20ef6-142">Again, use the actual port number where your service is running.</span></span> <span data-ttu-id="20ef6-143">Podczas wdrażania usługi, zostanie użyty identyfikator URI usługi na żywo.</span><span class="sxs-lookup"><span data-stu-id="20ef6-143">When you deploy your service, you will use the URI of the live service.</span></span> <span data-ttu-id="20ef6-144">Nie potrzebujesz zaktualizować serwer proxy.</span><span class="sxs-lookup"><span data-stu-id="20ef6-144">You don't need to update the proxy.</span></span>

<span data-ttu-id="20ef6-145">Poniższy kod dodaje program obsługi zdarzeń, który wyświetla żądanie identyfikatorów URI w oknie konsoli.</span><span class="sxs-lookup"><span data-stu-id="20ef6-145">The following code adds an event handler that prints the request URIs to the console window.</span></span> <span data-ttu-id="20ef6-146">Ten krok nie jest wymagana, ale co ciekawe wyświetlić identyfikatory URI, dla każdego zapytania.</span><span class="sxs-lookup"><span data-stu-id="20ef6-146">This step isn't required, but it's interesting to see the URIs for each query.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample3.cs)]

## <a name="query-the-service"></a><span data-ttu-id="20ef6-147">Zapytanie usługi</span><span class="sxs-lookup"><span data-stu-id="20ef6-147">Query the Service</span></span>

<span data-ttu-id="20ef6-148">Poniższy kod umożliwia pobranie listy produktów z usługi OData.</span><span class="sxs-lookup"><span data-stu-id="20ef6-148">The following code gets the list of products from the OData service.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample4.cs)]

<span data-ttu-id="20ef6-149">Należy zauważyć, że nie trzeba pisać kodu do wysyłania żądań HTTP lub przeanalizować odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="20ef6-149">Notice that you don't need to write any code to send the HTTP request or parse the response.</span></span> <span data-ttu-id="20ef6-150">Klasy serwera proxy jest to automatycznie podczas wyliczania `Container.Products` kolekcji w **foreach** pętli.</span><span class="sxs-lookup"><span data-stu-id="20ef6-150">The proxy class does this automatically when you enumerate the `Container.Products` collection in the **foreach** loop.</span></span>

<span data-ttu-id="20ef6-151">Po uruchomieniu aplikacji, dane wyjściowe powinny wyglądać następująco:</span><span class="sxs-lookup"><span data-stu-id="20ef6-151">When you run the application, the output should look like the following:</span></span>

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample5.cmd)]

<span data-ttu-id="20ef6-152">Aby uzyskać jednostki według Identyfikatora, należy użyć `where` klauzuli.</span><span class="sxs-lookup"><span data-stu-id="20ef6-152">To get an entity by ID, use a `where` clause.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample6.cs)]

<span data-ttu-id="20ef6-153">Dla pozostałej części tego tematu, I nie będzie zawierać całą `Main` działać tylko kod potrzebny do wywołania tej usługi.</span><span class="sxs-lookup"><span data-stu-id="20ef6-153">For the rest of this topic, I won't show the entire `Main` function, just the code needed to call the service.</span></span>

## <a name="apply-query-options"></a><span data-ttu-id="20ef6-154">Stosowanie opcji zapytania</span><span class="sxs-lookup"><span data-stu-id="20ef6-154">Apply Query Options</span></span>

<span data-ttu-id="20ef6-155">Usługa OData definiuje [opcje kwerendy](../supporting-odata-query-options.md) który może służyć do filtrowania, sortowania, dane strony i tak dalej.</span><span class="sxs-lookup"><span data-stu-id="20ef6-155">OData defines [query options](../supporting-odata-query-options.md) that can be used to filter, sort, page data, and so forth.</span></span> <span data-ttu-id="20ef6-156">Na serwerze proxy usługi można stosować te opcje przy użyciu różnych wyrażenia LINQ.</span><span class="sxs-lookup"><span data-stu-id="20ef6-156">In the service proxy, you can apply these options by using various LINQ expressions.</span></span>

<span data-ttu-id="20ef6-157">W tej sekcji I pokazano przykładowe krótki.</span><span class="sxs-lookup"><span data-stu-id="20ef6-157">In this section, I'll show brief examples.</span></span> <span data-ttu-id="20ef6-158">Aby uzyskać więcej informacji, zobacz temat [zagadnienia dotyczące LINQ (WCF Data Services)](https://msdn.microsoft.com/library/ee622463.aspx) w witrynie MSDN.</span><span class="sxs-lookup"><span data-stu-id="20ef6-158">For more details, see the topic [LINQ Considerations (WCF Data Services)](https://msdn.microsoft.com/library/ee622463.aspx) on MSDN.</span></span>

### <a name="filtering-filter"></a><span data-ttu-id="20ef6-159">Filtrowanie ($filter)</span><span class="sxs-lookup"><span data-stu-id="20ef6-159">Filtering ($filter)</span></span>

<span data-ttu-id="20ef6-160">Aby filtrować, użyj `where` klauzuli.</span><span class="sxs-lookup"><span data-stu-id="20ef6-160">To filter, use a `where` clause.</span></span> <span data-ttu-id="20ef6-161">Następujące przykładowe filtry według kategorii produktów.</span><span class="sxs-lookup"><span data-stu-id="20ef6-161">The following example filters by product category.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample7.cs)]

<span data-ttu-id="20ef6-162">Ten kod odnosi się do następującego zapytania OData.</span><span class="sxs-lookup"><span data-stu-id="20ef6-162">This code corresponds to the following OData query.</span></span>

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample8.cmd)]

<span data-ttu-id="20ef6-163">Należy zauważyć, że serwer proxy konwertuje `where` klauzuli do OData `$filter` wyrażenia.</span><span class="sxs-lookup"><span data-stu-id="20ef6-163">Notice that the proxy converts the `where` clause into an OData `$filter` expression.</span></span>

### <a name="sorting-orderby"></a><span data-ttu-id="20ef6-164">Sortowanie ($orderby)</span><span class="sxs-lookup"><span data-stu-id="20ef6-164">Sorting ($orderby)</span></span>

<span data-ttu-id="20ef6-165">Aby posortować, należy użyć `orderby` klauzuli.</span><span class="sxs-lookup"><span data-stu-id="20ef6-165">To sort, use an `orderby` clause.</span></span> <span data-ttu-id="20ef6-166">Poniższy przykład sortowania ceny, od najwyższego do najniższego.</span><span class="sxs-lookup"><span data-stu-id="20ef6-166">The following example sorts by price, from highest to lowest.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample9.cs)]

<span data-ttu-id="20ef6-167">Oto odpowiednie żądania OData.</span><span class="sxs-lookup"><span data-stu-id="20ef6-167">Here is the corresponding OData request.</span></span>

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample10.cmd)]

### <a name="client-side-paging-skip-and-top"></a><span data-ttu-id="20ef6-168">Po stronie klienta stronicowania ($skip i $top)</span><span class="sxs-lookup"><span data-stu-id="20ef6-168">Client-Side Paging ($skip and $top)</span></span>

<span data-ttu-id="20ef6-169">Dla zestawów dużych jednostek klient chcieć ograniczyć liczbę wyników.</span><span class="sxs-lookup"><span data-stu-id="20ef6-169">For large entity sets, the client might want to limit the number of results.</span></span> <span data-ttu-id="20ef6-170">Na przykład klient może wyświetlać 10 wpisów w danym momencie.</span><span class="sxs-lookup"><span data-stu-id="20ef6-170">For example, a client might show 10 entries at a time.</span></span> <span data-ttu-id="20ef6-171">Jest to nazywane *stronicowania po stronie klienta*.</span><span class="sxs-lookup"><span data-stu-id="20ef6-171">This is called *client-side paging*.</span></span> <span data-ttu-id="20ef6-172">(Dostępna jest również [stronicowania po stronie serwera](../supporting-odata-query-options.md#server-paging), gdy serwer ogranicza liczbę wyników.) Aby wykonać stronicowania po stronie klienta, należy użyć LINQ **Pomiń** i **zająć** metody.</span><span class="sxs-lookup"><span data-stu-id="20ef6-172">(There is also [server-side paging](../supporting-odata-query-options.md#server-paging), where the server limits the number of results.) To perform client-side paging, use the LINQ **Skip** and **Take** methods.</span></span> <span data-ttu-id="20ef6-173">Poniższy przykład pomija najpierw 40 wyniki i pobiera następny 10.</span><span class="sxs-lookup"><span data-stu-id="20ef6-173">The following example skips the first 40 results and takes the next 10.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample11.cs)]

<span data-ttu-id="20ef6-174">Oto odpowiednie żądania OData:</span><span class="sxs-lookup"><span data-stu-id="20ef6-174">Here is the corresponding OData request:</span></span>

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample12.cmd)]

### <a name="select-select-and-expand-expand"></a><span data-ttu-id="20ef6-175">Wybierz ($select) i rozwiń ($expand)</span><span class="sxs-lookup"><span data-stu-id="20ef6-175">Select ($select) and Expand ($expand)</span></span>

<span data-ttu-id="20ef6-176">Aby dołączyć powiązanych jednostek, należy użyć `DataServiceQuery<t>.Expand` metody.</span><span class="sxs-lookup"><span data-stu-id="20ef6-176">To include related entities, use the `DataServiceQuery<t>.Expand` method.</span></span> <span data-ttu-id="20ef6-177">Na przykład w celu uwzględnienia `Supplier` dla każdego `Product`:</span><span class="sxs-lookup"><span data-stu-id="20ef6-177">For example, to include the `Supplier` for each `Product`:</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample13.cs)]

<span data-ttu-id="20ef6-178">Oto odpowiednie żądania OData:</span><span class="sxs-lookup"><span data-stu-id="20ef6-178">Here is the corresponding OData request:</span></span>

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample14.cmd)]

<span data-ttu-id="20ef6-179">Aby zmienić kształt odpowiedzi, należy użyć LINQ **wybierz** klauzuli.</span><span class="sxs-lookup"><span data-stu-id="20ef6-179">To change the shape of the response, use the LINQ **select** clause.</span></span> <span data-ttu-id="20ef6-180">Poniższy przykład pobiera tylko nazwę każdego produktu, bez innych właściwości.</span><span class="sxs-lookup"><span data-stu-id="20ef6-180">The following example gets just the name of each product, with no other properties.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample15.cs)]

<span data-ttu-id="20ef6-181">Oto odpowiednie żądania OData:</span><span class="sxs-lookup"><span data-stu-id="20ef6-181">Here is the corresponding OData request:</span></span>

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample16.cmd)]

<span data-ttu-id="20ef6-182">Klauzula select może obejmować powiązanych jednostek.</span><span class="sxs-lookup"><span data-stu-id="20ef6-182">A select clause can include related entities.</span></span> <span data-ttu-id="20ef6-183">W takiej sytuacji nie należy wywoływać metody **rozwiń**; w takim przypadku serwer proxy się automatycznie uwzględnia rozszerzenie.</span><span class="sxs-lookup"><span data-stu-id="20ef6-183">In that case, do not call **Expand**; the proxy automatically includes the expansion in this case.</span></span> <span data-ttu-id="20ef6-184">Poniższy przykład pobiera nazwę i dostawcy każdego produktu.</span><span class="sxs-lookup"><span data-stu-id="20ef6-184">The following example gets the name and supplier of each product.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample17.cs)]

<span data-ttu-id="20ef6-185">Oto odpowiednie żądania OData.</span><span class="sxs-lookup"><span data-stu-id="20ef6-185">Here is the corresponding OData request.</span></span> <span data-ttu-id="20ef6-186">Należy zauważyć, że zawiera on **$expand** opcji.</span><span class="sxs-lookup"><span data-stu-id="20ef6-186">Notice that it includes the **$expand** option.</span></span>

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample18.cmd)]

<span data-ttu-id="20ef6-187">Aby uzyskać więcej informacji na temat $select i $rozwiń, zobacz [przy użyciu $select, $expand and $value w sieci Web API 2](../using-select-expand-and-value.md).</span><span class="sxs-lookup"><span data-stu-id="20ef6-187">For more information about $select and $expand, see [Using $select, $expand, and $value in Web API 2](../using-select-expand-and-value.md).</span></span>

## <a name="add-a-new-entity"></a><span data-ttu-id="20ef6-188">Dodaj nową jednostkę</span><span class="sxs-lookup"><span data-stu-id="20ef6-188">Add a New Entity</span></span>

<span data-ttu-id="20ef6-189">Aby dodać nową jednostkę do zbioru jednostek, należy wywołać `AddToEntitySet`, gdzie *EntitySet* to nazwa zestawu jednostek.</span><span class="sxs-lookup"><span data-stu-id="20ef6-189">To add a new entity to an entity set, call `AddToEntitySet`, where *EntitySet* is the name of the entity set.</span></span> <span data-ttu-id="20ef6-190">Na przykład `AddToProducts` dodaje nowy `Product` do `Products` zestawu jednostek.</span><span class="sxs-lookup"><span data-stu-id="20ef6-190">For example, `AddToProducts` adds a new `Product` to the `Products` entity set.</span></span> <span data-ttu-id="20ef6-191">Podczas generowania serwera proxy usług danych WCF automatycznie tworzy następujące silnie typizowane **AddTo** metody.</span><span class="sxs-lookup"><span data-stu-id="20ef6-191">When you generate the proxy, WCF Data Services automatically creates these strongly-typed **AddTo** methods.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample19.cs)]

<span data-ttu-id="20ef6-192">Aby dodać łącze między dwiema jednostkami, użyj **AddLink** i **SetLink** metody.</span><span class="sxs-lookup"><span data-stu-id="20ef6-192">To add a link between two entities, use the **AddLink** and **SetLink** methods.</span></span> <span data-ttu-id="20ef6-193">Poniższy kod dodaje nowego dostawcy i nowego produktu, a następnie tworzy łącza między nimi.</span><span class="sxs-lookup"><span data-stu-id="20ef6-193">The following code adds a new supplier and a new product, and then creates links between them.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample20.cs)]

<span data-ttu-id="20ef6-194">Użyj **AddLink** po właściwości nawigacji kolekcji.</span><span class="sxs-lookup"><span data-stu-id="20ef6-194">Use **AddLink** when the navigation property is a collection.</span></span> <span data-ttu-id="20ef6-195">W tym przykładzie dodamy produktu do `Products` kolekcji na dostawcy.</span><span class="sxs-lookup"><span data-stu-id="20ef6-195">In this example, we are adding a product to the `Products` collection on the supplier.</span></span>

<span data-ttu-id="20ef6-196">Użyj **SetLink** gdy właściwość nawigacji jest pojedynczą jednostką.</span><span class="sxs-lookup"><span data-stu-id="20ef6-196">Use **SetLink** when the navigation property is a single entity.</span></span> <span data-ttu-id="20ef6-197">W tym przykładzie firma Microsoft ustawienie `Supplier` właściwość nad produktem.</span><span class="sxs-lookup"><span data-stu-id="20ef6-197">In this example, we are setting the `Supplier` property on the product.</span></span>

## <a name="update--patch"></a><span data-ttu-id="20ef6-198">Aktualizacji / poprawek</span><span class="sxs-lookup"><span data-stu-id="20ef6-198">Update / Patch</span></span>

<span data-ttu-id="20ef6-199">Aby zaktualizować jednostkę, należy wywołać **UpdateObject** metody.</span><span class="sxs-lookup"><span data-stu-id="20ef6-199">To update an entity, call the **UpdateObject** method.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample21.cs)]

<span data-ttu-id="20ef6-200">Aktualizacja jest wykonywana po wywołaniu **SaveChanges**.</span><span class="sxs-lookup"><span data-stu-id="20ef6-200">The update is performed when you call **SaveChanges**.</span></span> <span data-ttu-id="20ef6-201">Domyślnie program WCF wysyła żądanie HTTP scalania.</span><span class="sxs-lookup"><span data-stu-id="20ef6-201">By default, WCF sends an HTTP MERGE request.</span></span> <span data-ttu-id="20ef6-202">**PatchOnUpdate** przekazuje WCF, zamiast wysyłać HTTP PATCH.</span><span class="sxs-lookup"><span data-stu-id="20ef6-202">The **PatchOnUpdate** option tells WCF to send an HTTP PATCH instead.</span></span>

> [!NOTE]
> <span data-ttu-id="20ef6-203">Dlaczego PATCH i MERGE?</span><span class="sxs-lookup"><span data-stu-id="20ef6-203">Why PATCH versus MERGE?</span></span> <span data-ttu-id="20ef6-204">Pierwotną specyfikację protokołu HTTP 1.1 ([RCF 2616](http://tools.ietf.org/html/rfc2616)) nie definiują dowolną metodę HTTP, za pomocą semantyki "częściową aktualizację".</span><span class="sxs-lookup"><span data-stu-id="20ef6-204">The original HTTP 1.1 specification ([RCF 2616](http://tools.ietf.org/html/rfc2616)) did not define any HTTP method with "partial update" semantics.</span></span> <span data-ttu-id="20ef6-205">Aby obsługiwać aktualizacje częściowe, specyfikacją OData zdefiniowane MERGE — metoda.</span><span class="sxs-lookup"><span data-stu-id="20ef6-205">To support partial updates, the OData specification defined the MERGE method.</span></span> <span data-ttu-id="20ef6-206">W 2010 r. [RFC 5789](http://tools.ietf.org/html/rfc5789) zdefiniowane metody PATCH częściowej aktualizacji.</span><span class="sxs-lookup"><span data-stu-id="20ef6-206">In 2010, [RFC 5789](http://tools.ietf.org/html/rfc5789) defined the PATCH method for partial updates.</span></span> <span data-ttu-id="20ef6-207">Niektóre z historii, w tym przeczytasz [wpis w blogu](https://blogs.msdn.com/b/astoriateam/archive/2008/05/20/merge-vs-replace-semantics-for-update-operations.aspx) na blogu usługi danych WCF.</span><span class="sxs-lookup"><span data-stu-id="20ef6-207">You can read some of the history in this [blog post](https://blogs.msdn.com/b/astoriateam/archive/2008/05/20/merge-vs-replace-semantics-for-update-operations.aspx) on the WCF Data Services Blog.</span></span> <span data-ttu-id="20ef6-208">Już dziś poprawki jest preferowana nad scalania.</span><span class="sxs-lookup"><span data-stu-id="20ef6-208">Today, PATCH is preferred over MERGE.</span></span> <span data-ttu-id="20ef6-209">Kontrolera OData, utworzonego przez tworzenie szkieletu interfejsu API sieci Web obsługuje obie metody.</span><span class="sxs-lookup"><span data-stu-id="20ef6-209">The OData controller created by the Web API scaffolding supports both methods.</span></span>


<span data-ttu-id="20ef6-210">Jeśli chcesz zamienić całej jednostki (PUT semantyki), należy określić **ReplaceOnUpdate** opcji.</span><span class="sxs-lookup"><span data-stu-id="20ef6-210">If you want to replace the entire entity (PUT semantics), specify the **ReplaceOnUpdate** option.</span></span> <span data-ttu-id="20ef6-211">Powoduje to WCF możesz wysłać żądanie HTTP PUT.</span><span class="sxs-lookup"><span data-stu-id="20ef6-211">This causes WCF to send an HTTP PUT request.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample22.cs)]

## <a name="delete-an-entity"></a><span data-ttu-id="20ef6-212">Usunięcie jednostki</span><span class="sxs-lookup"><span data-stu-id="20ef6-212">Delete an Entity</span></span>

<span data-ttu-id="20ef6-213">Aby usunąć jednostkę, wywołaj **DeleteObject**.</span><span class="sxs-lookup"><span data-stu-id="20ef6-213">To delete an entity, call **DeleteObject**.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample23.cs)]

## <a name="invoke-an-odata-action"></a><span data-ttu-id="20ef6-214">Wywołaj akcję OData</span><span class="sxs-lookup"><span data-stu-id="20ef6-214">Invoke an OData Action</span></span>

<span data-ttu-id="20ef6-215">W protokole OData [akcje](odata-actions.md) to sposób, aby dodać zachowania po stronie serwera, które łatwo nie są zdefiniowane jako operacje CRUD na jednostkach.</span><span class="sxs-lookup"><span data-stu-id="20ef6-215">In OData, [actions](odata-actions.md) are a way to add server-side behaviors that are not easily defined as CRUD operations on entities.</span></span>

<span data-ttu-id="20ef6-216">Mimo że ten dokument metadanych OData w tym artykule opisano czynności, klasę proxy nie powoduje utworzenia wszelkie silnie typizowane metody dla nich.</span><span class="sxs-lookup"><span data-stu-id="20ef6-216">Although the OData metadata document describes the actions, the proxy class does not create any strongly-typed methods for them.</span></span> <span data-ttu-id="20ef6-217">Nadal możesz wywołać akcję OData przy użyciu ogólnej **Execute** metody.</span><span class="sxs-lookup"><span data-stu-id="20ef6-217">You can still invoke an OData action by using the generic **Execute** method.</span></span> <span data-ttu-id="20ef6-218">Jednakże należy znać typów danych parametrów i zwracanej wartości.</span><span class="sxs-lookup"><span data-stu-id="20ef6-218">However, you will need to know the data types of the parameters and the return value.</span></span>

<span data-ttu-id="20ef6-219">Na przykład `RateProduct` akcja ma parametr o nazwie "Ocena" o typie `Int32` i zwraca `double`.</span><span class="sxs-lookup"><span data-stu-id="20ef6-219">For example, the `RateProduct` action takes parameter named "Rating" of type `Int32` and returns a `double`.</span></span> <span data-ttu-id="20ef6-220">Poniższy kod przedstawia sposób wywołania tej akcji.</span><span class="sxs-lookup"><span data-stu-id="20ef6-220">The following code shows how to invoke this action.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample24.cs)]

<span data-ttu-id="20ef6-221">Aby uzyskać więcej informacji, zobacz[wywoływanie operacji usługi i akcje](https://msdn.microsoft.com/library/hh230677.aspx).</span><span class="sxs-lookup"><span data-stu-id="20ef6-221">For more information, see[Calling Service Operations and Actions](https://msdn.microsoft.com/library/hh230677.aspx).</span></span>

<span data-ttu-id="20ef6-222">Jedną z opcji jest rozszerzenie **kontenera** Aby klasa zapewniała silnie typizowane metody, która wywołuje akcję:</span><span class="sxs-lookup"><span data-stu-id="20ef6-222">One option is to extend the **Container** class to provide a strongly typed method that invokes the action:</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample25.cs)]
