---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v3/calling-an-odata-service-from-a-net-client
title: Wywoływanie usługi OData z klienta programu .NET (C#) | Dokumentacja firmy Microsoft
author: MikeWasson
description: Ten samouczek pokazuje sposób wywoływania usługi OData od aplikacji klienckiej C#. Wersje oprogramowania używany w samouczek Visual Studio 2013 (działa Visual S...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/26/2014
ms.topic: article
ms.assetid: 6f448917-ad23-4dcc-9789-897fad74051b
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v3/calling-an-odata-service-from-a-net-client
msc.type: authoredcontent
ms.openlocfilehash: 497102cfa98680f2156a56ff9e36d84b7c820020
ms.sourcegitcommit: 6784510cfb589308c3875ccb5113eb31031766b4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/08/2018
ms.locfileid: "28042397"
---
<a name="calling-an-odata-service-from-a-net-client-c"></a><span data-ttu-id="b95bb-104">Wywoływanie usługi OData z klienta programu .NET (C#)</span><span class="sxs-lookup"><span data-stu-id="b95bb-104">Calling an OData Service From a .NET Client (C#)</span></span>
====================
<span data-ttu-id="b95bb-105">przez [Wasson Jan](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="b95bb-105">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="b95bb-106">Pobieranie ukończone projektu</span><span class="sxs-lookup"><span data-stu-id="b95bb-106">Download Completed Project</span></span>](http://code.msdn.microsoft.com/ASPNET-Web-API-OData-cecdb524)

> <span data-ttu-id="b95bb-107">Ten samouczek pokazuje sposób wywoływania usługi OData od aplikacji klienckiej C#.</span><span class="sxs-lookup"><span data-stu-id="b95bb-107">This tutorial shows how to call an OData service from a C# client application.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="b95bb-108">Używane w samouczku wersje oprogramowania</span><span class="sxs-lookup"><span data-stu-id="b95bb-108">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="b95bb-109">[Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads) (współpracuje z programu Visual Studio 2012)</span><span class="sxs-lookup"><span data-stu-id="b95bb-109">[Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads) (works with Visual Studio 2012)</span></span>
> - [<span data-ttu-id="b95bb-110">Biblioteka klienta usług danych WCF</span><span class="sxs-lookup"><span data-stu-id="b95bb-110">WCF Data Services Client Library</span></span>](https://msdn.microsoft.com/library/cc668772.aspx)
> - <span data-ttu-id="b95bb-111">Składnik Web API 2.</span><span class="sxs-lookup"><span data-stu-id="b95bb-111">Web API 2.</span></span> <span data-ttu-id="b95bb-112">(Przykład usługi OData jest utworzony przy użyciu 2 interfejsu API sieci Web, ale aplikacja kliencka nie zależy od interfejsu API sieci Web).</span><span class="sxs-lookup"><span data-stu-id="b95bb-112">(The example OData service is built using Web API 2, but the client application does not depend on Web API.)</span></span>


<span data-ttu-id="b95bb-113">W tym samouczku I będzie przeprowadzenie tworzenia aplikacji klienckiej, która wywołuje usługi OData.</span><span class="sxs-lookup"><span data-stu-id="b95bb-113">In this tutorial, I'll walk through creating a client application that calls an OData service.</span></span> <span data-ttu-id="b95bb-114">Usługa OData udostępnia następujących obiektów:</span><span class="sxs-lookup"><span data-stu-id="b95bb-114">The OData service exposes the following entities:</span></span>

- `Product`
- `Supplier`
- `ProductRating`

![](calling-an-odata-service-from-a-net-client/_static/image1.png)

<span data-ttu-id="b95bb-115">Następujące artykuły opisano sposób wdrażania usługi OData w składniku Web API.</span><span class="sxs-lookup"><span data-stu-id="b95bb-115">The following articles describe how to implement the OData service in Web API.</span></span> <span data-ttu-id="b95bb-116">(Nie trzeba ich zrozumienie tego samouczka, jednak odczytać).</span><span class="sxs-lookup"><span data-stu-id="b95bb-116">(You don't need to read them to understand this tutorial, however.)</span></span>

- [<span data-ttu-id="b95bb-117">Tworzenie punktu końcowego OData w składniku Web API 2</span><span class="sxs-lookup"><span data-stu-id="b95bb-117">Creating an OData Endpoint in Web API 2</span></span>](creating-an-odata-endpoint.md)
- [<span data-ttu-id="b95bb-118">Relacji z jednostką OData w składniku Web API 2</span><span class="sxs-lookup"><span data-stu-id="b95bb-118">OData Entity Relations in Web API 2</span></span>](working-with-entity-relations.md)
- [<span data-ttu-id="b95bb-119">Akcje protokołu OData w interfejsie Web API 2</span><span class="sxs-lookup"><span data-stu-id="b95bb-119">OData Actions in Web API 2</span></span>](odata-actions.md)

## <a name="generate-the-service-proxy"></a><span data-ttu-id="b95bb-120">Generowanie serwera Proxy usługi</span><span class="sxs-lookup"><span data-stu-id="b95bb-120">Generate the Service Proxy</span></span>

<span data-ttu-id="b95bb-121">Pierwszym krokiem jest generowanie usługi serwera proxy.</span><span class="sxs-lookup"><span data-stu-id="b95bb-121">The first step is to generate a service proxy.</span></span> <span data-ttu-id="b95bb-122">Serwer proxy usługi jest klasą .NET, który definiuje metody do uzyskiwania dostępu do usługi OData.</span><span class="sxs-lookup"><span data-stu-id="b95bb-122">The service proxy is a .NET class that defines methods for accessing the OData service.</span></span> <span data-ttu-id="b95bb-123">Serwer proxy tłumaczy wywołania metody do żądania HTTP.</span><span class="sxs-lookup"><span data-stu-id="b95bb-123">The proxy translates method calls into HTTP requests.</span></span>

![](calling-an-odata-service-from-a-net-client/_static/image2.png)

<span data-ttu-id="b95bb-124">Rozpocznij od otwierania projektu usługi OData w programie Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="b95bb-124">Start by opening the OData service project in Visual Studio.</span></span> <span data-ttu-id="b95bb-125">Naciśnij klawisze CTRL + F5, aby uruchomić usługę lokalnie w usługach IIS Express.</span><span class="sxs-lookup"><span data-stu-id="b95bb-125">Press CTRL+F5 to run the service locally in IIS Express.</span></span> <span data-ttu-id="b95bb-126">Należy zwrócić uwagę adresu lokalnego, w tym numer portu, który przypisuje Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="b95bb-126">Note the local address, including the port number that Visual Studio assigns.</span></span> <span data-ttu-id="b95bb-127">Podczas tworzenia serwera proxy, należy ten adres.</span><span class="sxs-lookup"><span data-stu-id="b95bb-127">You will need this address when you create the proxy.</span></span>

<span data-ttu-id="b95bb-128">Następnie otwórz inne wystąpienie programu Visual Studio i utworzyć projekt aplikacji konsoli.</span><span class="sxs-lookup"><span data-stu-id="b95bb-128">Next, open another instance of Visual Studio and create a console application project.</span></span> <span data-ttu-id="b95bb-129">Aplikacja konsoli będzie naszej aplikacji klienckiej OData.</span><span class="sxs-lookup"><span data-stu-id="b95bb-129">The console application will be our OData client application.</span></span> <span data-ttu-id="b95bb-130">(Można również dodać projekt do tego samego rozwiązania co usługa.)</span><span class="sxs-lookup"><span data-stu-id="b95bb-130">(You can also add the project to the same solution as the service.)</span></span>

> [!NOTE]
> <span data-ttu-id="b95bb-131">Pozostałe kroki można znaleźć projektu konsoli.</span><span class="sxs-lookup"><span data-stu-id="b95bb-131">The remaining steps refer the console project.</span></span>


<span data-ttu-id="b95bb-132">W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy **odwołania** i wybierz **Dodaj odwołanie do usługi**.</span><span class="sxs-lookup"><span data-stu-id="b95bb-132">In Solution Explorer, right-click **References** and select **Add Service Reference**.</span></span>

![](calling-an-odata-service-from-a-net-client/_static/image3.png)

<span data-ttu-id="b95bb-133">W **Dodaj odwołanie do usługi** okna dialogowego, wpisz adres usługi OData:</span><span class="sxs-lookup"><span data-stu-id="b95bb-133">In the **Add Service Reference** dialog, type the address of the OData service:</span></span>

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample1.cmd)]

<span data-ttu-id="b95bb-134">gdzie *portu* numer portu.</span><span class="sxs-lookup"><span data-stu-id="b95bb-134">where *port* is the port number.</span></span>

[![](calling-an-odata-service-from-a-net-client/_static/image5.png)](calling-an-odata-service-from-a-net-client/_static/image4.png)

<span data-ttu-id="b95bb-135">Aby uzyskać **Namespace**, wpisz "ProductService".</span><span class="sxs-lookup"><span data-stu-id="b95bb-135">For **Namespace**, type "ProductService".</span></span> <span data-ttu-id="b95bb-136">Ta opcja definiuje przestrzeń nazw, klasy proxy.</span><span class="sxs-lookup"><span data-stu-id="b95bb-136">This option defines the namespace of the proxy class.</span></span>

<span data-ttu-id="b95bb-137">Kliknij przycisk **Przejdź**.</span><span class="sxs-lookup"><span data-stu-id="b95bb-137">Click **Go**.</span></span> <span data-ttu-id="b95bb-138">Visual Studio odczytuje dokument metadanych OData do odnajdywania obiektów w usłudze.</span><span class="sxs-lookup"><span data-stu-id="b95bb-138">Visual Studio reads the OData metadata document to discover the entities in the service.</span></span>

[![](calling-an-odata-service-from-a-net-client/_static/image7.png)](calling-an-odata-service-from-a-net-client/_static/image6.png)

<span data-ttu-id="b95bb-139">Kliknij przycisk **OK** do dodania do projektu klasy serwera proxy.</span><span class="sxs-lookup"><span data-stu-id="b95bb-139">Click **OK** to add the proxy class to your project.</span></span>

![](calling-an-odata-service-from-a-net-client/_static/image8.png)

## <a name="create-an-instance-of-the-service-proxy-class"></a><span data-ttu-id="b95bb-140">Utwórz wystąpienie klasy serwera Proxy usługi</span><span class="sxs-lookup"><span data-stu-id="b95bb-140">Create an Instance of the Service Proxy Class</span></span>

<span data-ttu-id="b95bb-141">Wewnątrz sieci `Main` metody, Utwórz nowe wystąpienie klasy serwera proxy w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="b95bb-141">Inside your `Main` method, create a new instance of the proxy class, as follows:</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample2.cs)]

<span data-ttu-id="b95bb-142">Użyj numeru portu rzeczywiste którym usługa jest uruchomiona.</span><span class="sxs-lookup"><span data-stu-id="b95bb-142">Again, use the actual port number where your service is running.</span></span> <span data-ttu-id="b95bb-143">Podczas wdrażania usługi, zostanie użyty identyfikator URI usługi na żywo.</span><span class="sxs-lookup"><span data-stu-id="b95bb-143">When you deploy your service, you will use the URI of the live service.</span></span> <span data-ttu-id="b95bb-144">Nie trzeba zaktualizować serwera proxy.</span><span class="sxs-lookup"><span data-stu-id="b95bb-144">You don't need to update the proxy.</span></span>

<span data-ttu-id="b95bb-145">Poniższy kod dodaje obsługi zdarzeń, który Wyświetla identyfikatory URI żądania w oknie konsoli.</span><span class="sxs-lookup"><span data-stu-id="b95bb-145">The following code adds an event handler that prints the request URIs to the console window.</span></span> <span data-ttu-id="b95bb-146">Ten krok nie jest wymagane, ale jest interesujące wyświetlić identyfikator URI dla każdego zapytania.</span><span class="sxs-lookup"><span data-stu-id="b95bb-146">This step isn't required, but it's interesting to see the URIs for each query.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample3.cs)]

## <a name="query-the-service"></a><span data-ttu-id="b95bb-147">Zapytanie usługi</span><span class="sxs-lookup"><span data-stu-id="b95bb-147">Query the Service</span></span>

<span data-ttu-id="b95bb-148">Poniższy kod umożliwia pobranie listy produktów z usługi OData.</span><span class="sxs-lookup"><span data-stu-id="b95bb-148">The following code gets the list of products from the OData service.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample4.cs)]

<span data-ttu-id="b95bb-149">Zwróć uwagę, że nie należy do pisania kodu do wysyłania żądań HTTP i przeanalizować odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="b95bb-149">Notice that you don't need to write any code to send the HTTP request or parse the response.</span></span> <span data-ttu-id="b95bb-150">Klasy serwera proxy jest to automatycznie podczas wyliczania `Container.Products` kolekcji w **foreach** pętli.</span><span class="sxs-lookup"><span data-stu-id="b95bb-150">The proxy class does this automatically when you enumerate the `Container.Products` collection in the **foreach** loop.</span></span>

<span data-ttu-id="b95bb-151">Po uruchomieniu aplikacji, dane wyjściowe powinny wyglądać następująco:</span><span class="sxs-lookup"><span data-stu-id="b95bb-151">When you run the application, the output should look like the following:</span></span>

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample5.cmd)]

<span data-ttu-id="b95bb-152">Aby uzyskać jednostki według Identyfikatora, należy użyć `where` klauzuli.</span><span class="sxs-lookup"><span data-stu-id="b95bb-152">To get an entity by ID, use a `where` clause.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample6.cs)]

<span data-ttu-id="b95bb-153">W pozostałej części tego tematu, I nie będzie zawierać całą `Main` działać tylko kod potrzebne do wywołania tej usługi.</span><span class="sxs-lookup"><span data-stu-id="b95bb-153">For the rest of this topic, I won't show the entire `Main` function, just the code needed to call the service.</span></span>

## <a name="apply-query-options"></a><span data-ttu-id="b95bb-154">Stosowanie opcji zapytania</span><span class="sxs-lookup"><span data-stu-id="b95bb-154">Apply Query Options</span></span>

<span data-ttu-id="b95bb-155">Definiuje OData [opcje kwerendy](../supporting-odata-query-options.md) który może służyć do filtrowania, sortowania, dane strony i tak dalej.</span><span class="sxs-lookup"><span data-stu-id="b95bb-155">OData defines [query options](../supporting-odata-query-options.md) that can be used to filter, sort, page data, and so forth.</span></span> <span data-ttu-id="b95bb-156">Na serwerze proxy usług można stosować te opcje przy użyciu różnych wyrażenia LINQ.</span><span class="sxs-lookup"><span data-stu-id="b95bb-156">In the service proxy, you can apply these options by using various LINQ expressions.</span></span>

<span data-ttu-id="b95bb-157">W tej sekcji opisano I krótki przykłady.</span><span class="sxs-lookup"><span data-stu-id="b95bb-157">In this section, I'll show brief examples.</span></span> <span data-ttu-id="b95bb-158">Aby uzyskać więcej informacji, zobacz temat [zagadnienia dotyczące LINQ (usługi danych WCF)](https://msdn.microsoft.com/library/ee622463.aspx) w witrynie MSDN.</span><span class="sxs-lookup"><span data-stu-id="b95bb-158">For more details, see the topic [LINQ Considerations (WCF Data Services)](https://msdn.microsoft.com/library/ee622463.aspx) on MSDN.</span></span>

### <a name="filtering-filter"></a><span data-ttu-id="b95bb-159">Filtrowania ($filter)</span><span class="sxs-lookup"><span data-stu-id="b95bb-159">Filtering ($filter)</span></span>

<span data-ttu-id="b95bb-160">Aby filtrować, użyj `where` klauzuli.</span><span class="sxs-lookup"><span data-stu-id="b95bb-160">To filter, use a `where` clause.</span></span> <span data-ttu-id="b95bb-161">Następujące przykładowe filtry według kategorii produktów.</span><span class="sxs-lookup"><span data-stu-id="b95bb-161">The following example filters by product category.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample7.cs)]

<span data-ttu-id="b95bb-162">Ten kod odnosi się do następującego zapytania OData.</span><span class="sxs-lookup"><span data-stu-id="b95bb-162">This code corresponds to the following OData query.</span></span>

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample8.cmd)]

<span data-ttu-id="b95bb-163">Należy zauważyć, że serwer proxy konwertuje `where` klauzuli do OData `$filter` wyrażenia.</span><span class="sxs-lookup"><span data-stu-id="b95bb-163">Notice that the proxy converts the `where` clause into an OData `$filter` expression.</span></span>

### <a name="sorting-orderby"></a><span data-ttu-id="b95bb-164">Sortowanie ($orderby)</span><span class="sxs-lookup"><span data-stu-id="b95bb-164">Sorting ($orderby)</span></span>

<span data-ttu-id="b95bb-165">Aby posortować, użyj `orderby` klauzuli.</span><span class="sxs-lookup"><span data-stu-id="b95bb-165">To sort, use an `orderby` clause.</span></span> <span data-ttu-id="b95bb-166">Poniższy przykład sortowania cen w kolejności malejącej.</span><span class="sxs-lookup"><span data-stu-id="b95bb-166">The following example sorts by price, from highest to lowest.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample9.cs)]

<span data-ttu-id="b95bb-167">Oto odpowiedniego żądania OData.</span><span class="sxs-lookup"><span data-stu-id="b95bb-167">Here is the corresponding OData request.</span></span>

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample10.cmd)]

### <a name="client-side-paging-skip-and-top"></a><span data-ttu-id="b95bb-168">Stronicowania po stronie klienta ($skip ani $top)</span><span class="sxs-lookup"><span data-stu-id="b95bb-168">Client-Side Paging ($skip and $top)</span></span>

<span data-ttu-id="b95bb-169">Dla zestawów duża jednostka klienta mogą chcieć ograniczyć liczbę wyników.</span><span class="sxs-lookup"><span data-stu-id="b95bb-169">For large entity sets, the client might want to limit the number of results.</span></span> <span data-ttu-id="b95bb-170">Na przykład klienta mogą być wyświetlane wpisy 10 naraz.</span><span class="sxs-lookup"><span data-stu-id="b95bb-170">For example, a client might show 10 entries at a time.</span></span> <span data-ttu-id="b95bb-171">Ta metoda jest wywoływana *stronicowania po stronie klienta*.</span><span class="sxs-lookup"><span data-stu-id="b95bb-171">This is called *client-side paging*.</span></span> <span data-ttu-id="b95bb-172">(Dostępne są także [stronicowania po stronie serwera](../supporting-odata-query-options.md#server-paging), gdy serwer ogranicza liczbę wyników.) Aby wykonać stronicowania po stronie klienta, należy użyć LINQ **Pomiń** i **zająć** metody.</span><span class="sxs-lookup"><span data-stu-id="b95bb-172">(There is also [server-side paging](../supporting-odata-query-options.md#server-paging), where the server limits the number of results.) To perform client-side paging, use the LINQ **Skip** and **Take** methods.</span></span> <span data-ttu-id="b95bb-173">Poniższy przykład pomija najpierw 40 wyników i przejście dalej 10.</span><span class="sxs-lookup"><span data-stu-id="b95bb-173">The following example skips the first 40 results and takes the next 10.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample11.cs)]

<span data-ttu-id="b95bb-174">Oto odpowiedniego żądania OData:</span><span class="sxs-lookup"><span data-stu-id="b95bb-174">Here is the corresponding OData request:</span></span>

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample12.cmd)]

### <a name="select-select-and-expand-expand"></a><span data-ttu-id="b95bb-175">Wybierz ($select) i rozwiń ($expand)</span><span class="sxs-lookup"><span data-stu-id="b95bb-175">Select ($select) and Expand ($expand)</span></span>

<span data-ttu-id="b95bb-176">Aby dołączyć powiązanych jednostek, należy użyć `DataServiceQuery<t>.Expand` metody.</span><span class="sxs-lookup"><span data-stu-id="b95bb-176">To include related entities, use the `DataServiceQuery<t>.Expand` method.</span></span> <span data-ttu-id="b95bb-177">Na przykład, aby uwzględnić `Supplier` dla każdego `Product`:</span><span class="sxs-lookup"><span data-stu-id="b95bb-177">For example, to include the `Supplier` for each `Product`:</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample13.cs)]

<span data-ttu-id="b95bb-178">Oto odpowiedniego żądania OData:</span><span class="sxs-lookup"><span data-stu-id="b95bb-178">Here is the corresponding OData request:</span></span>

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample14.cmd)]

<span data-ttu-id="b95bb-179">Aby zmienić kształt odpowiedzi, należy użyć LINQ **wybierz** klauzuli.</span><span class="sxs-lookup"><span data-stu-id="b95bb-179">To change the shape of the response, use the LINQ **select** clause.</span></span> <span data-ttu-id="b95bb-180">Poniższy przykład pobiera tylko nazwę każdego produktu bez innych właściwości.</span><span class="sxs-lookup"><span data-stu-id="b95bb-180">The following example gets just the name of each product, with no other properties.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample15.cs)]

<span data-ttu-id="b95bb-181">Oto odpowiedniego żądania OData:</span><span class="sxs-lookup"><span data-stu-id="b95bb-181">Here is the corresponding OData request:</span></span>

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample16.cmd)]

<span data-ttu-id="b95bb-182">Klauzula select może zawierać powiązanych jednostek.</span><span class="sxs-lookup"><span data-stu-id="b95bb-182">A select clause can include related entities.</span></span> <span data-ttu-id="b95bb-183">W takim przypadku nie wywołuj **rozwiń**; w takim przypadku serwer proxy rozszerzenie zawiera automatycznie.</span><span class="sxs-lookup"><span data-stu-id="b95bb-183">In that case, do not call **Expand**; the proxy automatically includes the expansion in this case.</span></span> <span data-ttu-id="b95bb-184">Poniższy przykład pobiera nazwę i dostawcy każdego produktu.</span><span class="sxs-lookup"><span data-stu-id="b95bb-184">The following example gets the name and supplier of each product.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample17.cs)]

<span data-ttu-id="b95bb-185">Oto odpowiedniego żądania OData.</span><span class="sxs-lookup"><span data-stu-id="b95bb-185">Here is the corresponding OData request.</span></span> <span data-ttu-id="b95bb-186">Należy zauważyć, że zawiera on **rozwiń $** opcji.</span><span class="sxs-lookup"><span data-stu-id="b95bb-186">Notice that it includes the **$expand** option.</span></span>

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample18.cmd)]

<span data-ttu-id="b95bb-187">Aby uzyskać więcej informacji na temat $select i $expand Rozwiń, zobacz [przy użyciu $select, rozwinąć $ i $value w sieci Web API 2](../using-select-expand-and-value.md).</span><span class="sxs-lookup"><span data-stu-id="b95bb-187">For more information about $select and $expand, see [Using $select, $expand, and $value in Web API 2](../using-select-expand-and-value.md).</span></span>

## <a name="add-a-new-entity"></a><span data-ttu-id="b95bb-188">Dodaj nową jednostkę</span><span class="sxs-lookup"><span data-stu-id="b95bb-188">Add a New Entity</span></span>

<span data-ttu-id="b95bb-189">Aby dodać nową jednostkę do zbioru jednostek, należy wywołać `AddToEntitySet`, gdzie *EntitySet* jest nazwą zestawu jednostek.</span><span class="sxs-lookup"><span data-stu-id="b95bb-189">To add a new entity to an entity set, call `AddToEntitySet`, where *EntitySet* is the name of the entity set.</span></span> <span data-ttu-id="b95bb-190">Na przykład `AddToProducts` dodaje nowy `Product` do `Products` zestawu jednostek.</span><span class="sxs-lookup"><span data-stu-id="b95bb-190">For example, `AddToProducts` adds a new `Product` to the `Products` entity set.</span></span> <span data-ttu-id="b95bb-191">Podczas generowania serwera proxy usługi danych WCF automatycznie tworzy następujące jednoznacznie **AddTo** metody.</span><span class="sxs-lookup"><span data-stu-id="b95bb-191">When you generate the proxy, WCF Data Services automatically creates these strongly-typed **AddTo** methods.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample19.cs)]

<span data-ttu-id="b95bb-192">Aby dodać łącze między dwiema jednostkami, należy użyć **metody AddLink** i **SetLink** metody.</span><span class="sxs-lookup"><span data-stu-id="b95bb-192">To add a link between two entities, use the **AddLink** and **SetLink** methods.</span></span> <span data-ttu-id="b95bb-193">Poniższy kod dodaje nowego dostawcy i nowego produktu, a następnie tworzy linki między nimi.</span><span class="sxs-lookup"><span data-stu-id="b95bb-193">The following code adds a new supplier and a new product, and then creates links between them.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample20.cs)]

<span data-ttu-id="b95bb-194">Użyj **metody AddLink** przypadku właściwości nawigacji kolekcji.</span><span class="sxs-lookup"><span data-stu-id="b95bb-194">Use **AddLink** when the navigation property is a collection.</span></span> <span data-ttu-id="b95bb-195">W tym przykładzie dodajemy produkt `Products` kolekcji na dostawcy.</span><span class="sxs-lookup"><span data-stu-id="b95bb-195">In this example, we are adding a product to the `Products` collection on the supplier.</span></span>

<span data-ttu-id="b95bb-196">Użyj **SetLink** gdy właściwość nawigacji jest pojedynczą jednostką.</span><span class="sxs-lookup"><span data-stu-id="b95bb-196">Use **SetLink** when the navigation property is a single entity.</span></span> <span data-ttu-id="b95bb-197">W tym przykładzie konfigurujemy ustawienia `Supplier` właściwości dla produktu.</span><span class="sxs-lookup"><span data-stu-id="b95bb-197">In this example, we are setting the `Supplier` property on the product.</span></span>

## <a name="update--patch"></a><span data-ttu-id="b95bb-198">Aktualizacji lub poprawki</span><span class="sxs-lookup"><span data-stu-id="b95bb-198">Update / Patch</span></span>

<span data-ttu-id="b95bb-199">Aby zaktualizować jednostkę, należy wywołać **UpdateObject** metody.</span><span class="sxs-lookup"><span data-stu-id="b95bb-199">To update an entity, call the **UpdateObject** method.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample21.cs)]

<span data-ttu-id="b95bb-200">Aktualizacja jest wykonywana po wywołaniu **SaveChanges**.</span><span class="sxs-lookup"><span data-stu-id="b95bb-200">The update is performed when you call **SaveChanges**.</span></span> <span data-ttu-id="b95bb-201">Domyślnie program WCF wysyła żądanie HTTP scalania.</span><span class="sxs-lookup"><span data-stu-id="b95bb-201">By default, WCF sends an HTTP MERGE request.</span></span> <span data-ttu-id="b95bb-202">**PatchOnUpdate** opcja nakazuje WCF, aby zamiast wysyłania HTTP PATCH.</span><span class="sxs-lookup"><span data-stu-id="b95bb-202">The **PatchOnUpdate** option tells WCF to send an HTTP PATCH instead.</span></span>

> [!NOTE]
> <span data-ttu-id="b95bb-203">Dlaczego PATCH i MERGE?</span><span class="sxs-lookup"><span data-stu-id="b95bb-203">Why PATCH versus MERGE?</span></span> <span data-ttu-id="b95bb-204">Pierwotnej specyfikacji protokołu HTTP 1.1 ([RCF 2616](http://tools.ietf.org/html/rfc2616)) nie zdefiniowano żadnych metody HTTP z semantyki "częściowej aktualizacji".</span><span class="sxs-lookup"><span data-stu-id="b95bb-204">The original HTTP 1.1 specification ([RCF 2616](http://tools.ietf.org/html/rfc2616)) did not define any HTTP method with "partial update" semantics.</span></span> <span data-ttu-id="b95bb-205">Aby obsługiwać aktualizacje częściowe, specyfikację OData zdefiniowane MERGE — metoda.</span><span class="sxs-lookup"><span data-stu-id="b95bb-205">To support partial updates, the OData specification defined the MERGE method.</span></span> <span data-ttu-id="b95bb-206">W 2010 r. [RFC 5789](http://tools.ietf.org/html/rfc5789) zdefiniowane poprawki metody częściowej aktualizacji.</span><span class="sxs-lookup"><span data-stu-id="b95bb-206">In 2010, [RFC 5789](http://tools.ietf.org/html/rfc5789) defined the PATCH method for partial updates.</span></span> <span data-ttu-id="b95bb-207">Można znaleźć niektórych historii w tym [wpis w blogu](https://blogs.msdn.com/b/astoriateam/archive/2008/05/20/merge-vs-replace-semantics-for-update-operations.aspx) w blogu usługi danych WCF.</span><span class="sxs-lookup"><span data-stu-id="b95bb-207">You can read some of the history in this [blog post](https://blogs.msdn.com/b/astoriateam/archive/2008/05/20/merge-vs-replace-semantics-for-update-operations.aspx) on the WCF Data Services Blog.</span></span> <span data-ttu-id="b95bb-208">Obecnie poprawka jest preferowana przez scalania.</span><span class="sxs-lookup"><span data-stu-id="b95bb-208">Today, PATCH is preferred over MERGE.</span></span> <span data-ttu-id="b95bb-209">Kontrolera OData utworzonego przez funkcję szkieletów interfejsu API sieci Web obsługuje obie metody.</span><span class="sxs-lookup"><span data-stu-id="b95bb-209">The OData controller created by the Web API scaffolding supports both methods.</span></span>


<span data-ttu-id="b95bb-210">Jeśli chcesz zamienić całej jednostki (PUT semantyki), określ **ReplaceOnUpdate** opcji.</span><span class="sxs-lookup"><span data-stu-id="b95bb-210">If you want to replace the entire entity (PUT semantics), specify the **ReplaceOnUpdate** option.</span></span> <span data-ttu-id="b95bb-211">Powoduje to WCF można wysłać żądania HTTP PUT.</span><span class="sxs-lookup"><span data-stu-id="b95bb-211">This causes WCF to send an HTTP PUT request.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample22.cs)]

## <a name="delete-an-entity"></a><span data-ttu-id="b95bb-212">Usuwanie jednostki</span><span class="sxs-lookup"><span data-stu-id="b95bb-212">Delete an Entity</span></span>

<span data-ttu-id="b95bb-213">Aby usunąć jednostkę, wywołaj **DeleteObject**.</span><span class="sxs-lookup"><span data-stu-id="b95bb-213">To delete an entity, call **DeleteObject**.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample23.cs)]

## <a name="invoke-an-odata-action"></a><span data-ttu-id="b95bb-214">Wywołaj akcję OData</span><span class="sxs-lookup"><span data-stu-id="b95bb-214">Invoke an OData Action</span></span>

<span data-ttu-id="b95bb-215">W protokole OData [akcje](odata-actions.md) służą do dodawania zachowań po stronie serwera, które łatwo nie są zdefiniowane jako operacje CRUD na jednostkach.</span><span class="sxs-lookup"><span data-stu-id="b95bb-215">In OData, [actions](odata-actions.md) are a way to add server-side behaviors that are not easily defined as CRUD operations on entities.</span></span>

<span data-ttu-id="b95bb-216">Mimo że ten dokument metadanych OData opisano akcje, klasy serwera proxy nie tworzy żadnych metod jednoznacznie dla nich.</span><span class="sxs-lookup"><span data-stu-id="b95bb-216">Although the OData metadata document describes the actions, the proxy class does not create any strongly-typed methods for them.</span></span> <span data-ttu-id="b95bb-217">Nadal można wywołać akcję OData przy użyciu ogólnej **Execute** metody.</span><span class="sxs-lookup"><span data-stu-id="b95bb-217">You can still invoke an OData action by using the generic **Execute** method.</span></span> <span data-ttu-id="b95bb-218">Należy znać typów danych parametrów i zwracanych wartości.</span><span class="sxs-lookup"><span data-stu-id="b95bb-218">However, you will need to know the data types of the parameters and the return value.</span></span>

<span data-ttu-id="b95bb-219">Na przykład `RateProduct` akcja ma parametr o nazwie "Klasyfikacji" typu `Int32` i zwraca `double`.</span><span class="sxs-lookup"><span data-stu-id="b95bb-219">For example, the `RateProduct` action takes parameter named "Rating" of type `Int32` and returns a `double`.</span></span> <span data-ttu-id="b95bb-220">Poniższy kod przedstawia sposób wywołania tej akcji.</span><span class="sxs-lookup"><span data-stu-id="b95bb-220">The following code shows how to invoke this action.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample24.cs)]

<span data-ttu-id="b95bb-221">Aby uzyskać więcej informacji, zobacz[wywoływanie operacji usługi i działania](https://msdn.microsoft.com/library/hh230677.aspx).</span><span class="sxs-lookup"><span data-stu-id="b95bb-221">For more information, see[Calling Service Operations and Actions](https://msdn.microsoft.com/library/hh230677.aspx).</span></span>

<span data-ttu-id="b95bb-222">Jedną z opcji jest rozszerzenie **kontenera** klasy zapewnienie silnie typizowane metody, która wywołuje akcję:</span><span class="sxs-lookup"><span data-stu-id="b95bb-222">One option is to extend the **Container** class to provide a strongly typed method that invokes the action:</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample25.cs)]
