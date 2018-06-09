---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/create-an-odata-v4-endpoint
title: Tworzenie punktu końcowego OData v4 używanie składnika ASP.NET Web API 2.2 | Dokumentacja firmy Microsoft
author: MikeWasson
description: Open Data Protocol (OData) to protokół dostępu do danych w sieci Web. OData zapewnia jednolity sposób, w celu wykonywania zapytań i manipulowania zestawów danych za pośrednictwem operacji CRUD...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/24/2014
ms.topic: article
ms.assetid: 1e1927c0-ded1-4752-80fd-a146628d2f09
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/create-an-odata-v4-endpoint
msc.type: authoredcontent
ms.openlocfilehash: a3f94818f9674b0e1e9a45b2a6cc9455edc79726
ms.sourcegitcommit: 6784510cfb589308c3875ccb5113eb31031766b4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/08/2018
ms.locfileid: "26566720"
---
<a name="create-an-odata-v4-endpoint-using-aspnet-web-api-22"></a><span data-ttu-id="6fb44-104">Tworzenie punktu końcowego OData v4 używanie składnika ASP.NET Web API 2.2</span><span class="sxs-lookup"><span data-stu-id="6fb44-104">Create an OData v4 Endpoint Using ASP.NET Web API 2.2</span></span>
====================
<span data-ttu-id="6fb44-105">przez [Wasson Jan](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="6fb44-105">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

> <span data-ttu-id="6fb44-106">Open Data Protocol (OData) to protokół dostępu do danych w sieci Web.</span><span class="sxs-lookup"><span data-stu-id="6fb44-106">The Open Data Protocol (OData) is a data access protocol for the web.</span></span> <span data-ttu-id="6fb44-107">OData zapewnia jednolity sposób, w celu wykonywania zapytań i manipulowania zestawów danych za pośrednictwem operacji CRUD (tworzenia, odczytu, aktualizacji i usuwania).</span><span class="sxs-lookup"><span data-stu-id="6fb44-107">OData provides a uniform way to query and manipulate data sets through CRUD operations (create, read, update, and delete).</span></span>
> 
> <span data-ttu-id="6fb44-108">Interfejs API sieci Web platformy ASP.NET obsługuje zarówno w wersji 3, jak i w wersji 4 protokołu.</span><span class="sxs-lookup"><span data-stu-id="6fb44-108">ASP.NET Web API supports both v3 and v4 of the protocol.</span></span> <span data-ttu-id="6fb44-109">Można także ustawić punkt końcowy v4 uruchomioną side-by-side z punktem końcowym v3.</span><span class="sxs-lookup"><span data-stu-id="6fb44-109">You can even have a v4 endpoint that runs side-by-side with a v3 endpoint.</span></span>
> 
> <span data-ttu-id="6fb44-110">W tym samouczku przedstawiono sposób tworzenia punktu końcowego v4 OData, która obsługuje operacje CRUD.</span><span class="sxs-lookup"><span data-stu-id="6fb44-110">This tutorial shows how to create an OData v4 endpoint that supports CRUD operations.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="6fb44-111">Używane w samouczku wersje oprogramowania</span><span class="sxs-lookup"><span data-stu-id="6fb44-111">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="6fb44-112">2.2 interfejsu API sieci Web</span><span class="sxs-lookup"><span data-stu-id="6fb44-112">Web API 2.2</span></span>
> - <span data-ttu-id="6fb44-113">Protokołu OData v4</span><span class="sxs-lookup"><span data-stu-id="6fb44-113">OData v4</span></span>
> - [<span data-ttu-id="6fb44-114">Visual Studio 2013 Update 2</span><span class="sxs-lookup"><span data-stu-id="6fb44-114">Visual Studio 2013 Update 2</span></span>](https://www.visualstudio.com/downloads/download-visual-studio-vs)
> - <span data-ttu-id="6fb44-115">Entity Framework 6</span><span class="sxs-lookup"><span data-stu-id="6fb44-115">Entity Framework 6</span></span>
> - <span data-ttu-id="6fb44-116">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="6fb44-116">.NET 4.5</span></span>
> 
> 
> ## <a name="tutorial-versions"></a><span data-ttu-id="6fb44-117">Samouczek wersji</span><span class="sxs-lookup"><span data-stu-id="6fb44-117">Tutorial versions</span></span>
> 
> <span data-ttu-id="6fb44-118">Dla OData w wersji 3, zobacz [tworzenia punktu końcowego OData v3](../odata-v3/creating-an-odata-endpoint.md).</span><span class="sxs-lookup"><span data-stu-id="6fb44-118">For the OData Version 3, see [Creating an OData v3 Endpoint](../odata-v3/creating-an-odata-endpoint.md).</span></span>


## <a name="create-the-visual-studio-project"></a><span data-ttu-id="6fb44-119">Tworzenie projektu programu Visual Studio</span><span class="sxs-lookup"><span data-stu-id="6fb44-119">Create the Visual Studio Project</span></span>

<span data-ttu-id="6fb44-120">W programie Visual Studio z **pliku** menu, wybierz opcję **nowy** &gt; **projektu**.</span><span class="sxs-lookup"><span data-stu-id="6fb44-120">In Visual Studio, from the **File** menu, select **New** &gt; **Project**.</span></span>

<span data-ttu-id="6fb44-121">Rozwiń węzeł **zainstalowana** &gt; **szablony** &gt; **Visual C#** &gt; **Web**i wybierz  **Aplikacja sieci Web ASP.NET** szablonu.</span><span class="sxs-lookup"><span data-stu-id="6fb44-121">Expand **Installed** &gt; **Templates** &gt; **Visual C#** &gt; **Web**, and select the **ASP.NET Web Application** template.</span></span> <span data-ttu-id="6fb44-122">Nazwij projekt &quot;ProductService&quot;.</span><span class="sxs-lookup"><span data-stu-id="6fb44-122">Name the project &quot;ProductService&quot;.</span></span>

[![](create-an-odata-v4-endpoint/_static/image2.png)](create-an-odata-v4-endpoint/_static/image1.png)

<span data-ttu-id="6fb44-123">W **nowy projekt** okno dialogowe, wybierz opcję **pusty** szablonu.</span><span class="sxs-lookup"><span data-stu-id="6fb44-123">In the **New Project** dialog, select the **Empty** template.</span></span> <span data-ttu-id="6fb44-124">W obszarze &quot;. Dodaj foldery i podstawowe odwołania... &quot;, kliknij przycisk **interfejsu API sieci Web**.</span><span class="sxs-lookup"><span data-stu-id="6fb44-124">Under &quot;Add folders and core references...&quot;, click **Web API**.</span></span> <span data-ttu-id="6fb44-125">Kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="6fb44-125">Click **OK**.</span></span>

[![](create-an-odata-v4-endpoint/_static/image4.png)](create-an-odata-v4-endpoint/_static/image3.png)

## <a name="install-the-odata-packages"></a><span data-ttu-id="6fb44-126">Zainstaluj pakiety OData</span><span class="sxs-lookup"><span data-stu-id="6fb44-126">Install the OData Packages</span></span>

<span data-ttu-id="6fb44-127">Z **narzędzia** menu, wybierz opcję **Menedżera pakietów NuGet** &gt; **Konsola Menedżera pakietów**.</span><span class="sxs-lookup"><span data-stu-id="6fb44-127">From the **Tools** menu, select **NuGet Package Manager** &gt; **Package Manager Console**.</span></span> <span data-ttu-id="6fb44-128">W oknie Konsola Menedżera pakietów wpisz:</span><span class="sxs-lookup"><span data-stu-id="6fb44-128">In the Package Manager Console window, type:</span></span>

[!code-console[Main](create-an-odata-v4-endpoint/samples/sample1.cmd)]

<span data-ttu-id="6fb44-129">To polecenie powoduje zainstalowanie najnowszych pakietów OData NuGet.</span><span class="sxs-lookup"><span data-stu-id="6fb44-129">This command installs the latest OData NuGet packages.</span></span>

## <a name="add-a-model-class"></a><span data-ttu-id="6fb44-130">Dodaj klasę modelu</span><span class="sxs-lookup"><span data-stu-id="6fb44-130">Add a Model Class</span></span>

<span data-ttu-id="6fb44-131">A *modelu* jest obiekt, który reprezentuje jednostkę danych w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="6fb44-131">A *model* is an object that represents a data entity in your application.</span></span>

<span data-ttu-id="6fb44-132">W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy folder modeli.</span><span class="sxs-lookup"><span data-stu-id="6fb44-132">In Solution Explorer, right-click the Models folder.</span></span> <span data-ttu-id="6fb44-133">Wybierz z menu kontekstowego **Dodaj** &gt; **klasy**.</span><span class="sxs-lookup"><span data-stu-id="6fb44-133">From the context menu, select **Add** &gt; **Class**.</span></span>

[![](create-an-odata-v4-endpoint/_static/image6.png)](create-an-odata-v4-endpoint/_static/image5.png)

> [!NOTE]
> <span data-ttu-id="6fb44-134">Według Konwencji klasy modeli są umieszczane w folderze modele, ale nie trzeba wykonywać tę Konwencję do własnych projektów.</span><span class="sxs-lookup"><span data-stu-id="6fb44-134">By convention, model classes are placed in the Models folder, but you don't have to follow this convention in your own projects.</span></span>


<span data-ttu-id="6fb44-135">Nazwa klasy `Product`.</span><span class="sxs-lookup"><span data-stu-id="6fb44-135">Name the class `Product`.</span></span> <span data-ttu-id="6fb44-136">W pliku Product.cs Zastąp schematyczny kod poniżej:</span><span class="sxs-lookup"><span data-stu-id="6fb44-136">In the Product.cs file, replace the boilerplate code with the following:</span></span>

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample2.cs)]

<span data-ttu-id="6fb44-137">`Id` Właściwość jest kluczem jednostki.</span><span class="sxs-lookup"><span data-stu-id="6fb44-137">The `Id` property is the entity key.</span></span> <span data-ttu-id="6fb44-138">Klienci mogą wykonywać kwerendę jednostek według klucza.</span><span class="sxs-lookup"><span data-stu-id="6fb44-138">Clients can query entities by key.</span></span> <span data-ttu-id="6fb44-139">Na przykład, aby uzyskać produktu o identyfikatorze 5, identyfikator URI jest `/Products(5)`.</span><span class="sxs-lookup"><span data-stu-id="6fb44-139">For example, to get the product with ID of 5, the URI is `/Products(5)`.</span></span> <span data-ttu-id="6fb44-140">`Id` Właściwości będzie również klucza podstawowego w bazie danych zaplecza.</span><span class="sxs-lookup"><span data-stu-id="6fb44-140">The `Id` property will also be the primary key in the back-end database.</span></span>

## <a name="enable-entity-framework"></a><span data-ttu-id="6fb44-141">Włączanie programu Entity Framework</span><span class="sxs-lookup"><span data-stu-id="6fb44-141">Enable Entity Framework</span></span>

<span data-ttu-id="6fb44-142">W tym samouczku użyjemy Entity Framework (EF) Code First można utworzyć bazy danych zaplecza.</span><span class="sxs-lookup"><span data-stu-id="6fb44-142">For this tutorial, we'll use Entity Framework (EF) Code First to create the back-end database.</span></span>

> [!NOTE]
> <span data-ttu-id="6fb44-143">Web API OData nie wymaga EF.</span><span class="sxs-lookup"><span data-stu-id="6fb44-143">Web API OData does not require EF.</span></span> <span data-ttu-id="6fb44-144">Użyj dowolnej warstwy dostępu do danych, która może dokonywać translacji jednostki bazy danych do modeli.</span><span class="sxs-lookup"><span data-stu-id="6fb44-144">Use any data-access layer that can translate database entities into models.</span></span>


<span data-ttu-id="6fb44-145">Najpierw zainstaluj pakiet NuGet dla EF.</span><span class="sxs-lookup"><span data-stu-id="6fb44-145">First, install the NuGet package for EF.</span></span> <span data-ttu-id="6fb44-146">Z **narzędzia** menu, wybierz opcję **Menedżera pakietów NuGet** &gt; **Konsola Menedżera pakietów**.</span><span class="sxs-lookup"><span data-stu-id="6fb44-146">From the **Tools** menu, select **NuGet Package Manager** &gt; **Package Manager Console**.</span></span> <span data-ttu-id="6fb44-147">W oknie Konsola Menedżera pakietów wpisz:</span><span class="sxs-lookup"><span data-stu-id="6fb44-147">In the Package Manager Console window, type:</span></span>

[!code-console[Main](create-an-odata-v4-endpoint/samples/sample3.cmd)]

<span data-ttu-id="6fb44-148">Otwórz plik Web.config i dodaj następującą sekcję wewnątrz **konfiguracji** elementu, po **configSections** elementu.</span><span class="sxs-lookup"><span data-stu-id="6fb44-148">Open the Web.config file, and add the following section inside the **configuration** element, after the **configSections** element.</span></span>

[!code-xml[Main](create-an-odata-v4-endpoint/samples/sample4.xml?highlight=6)]

<span data-ttu-id="6fb44-149">To ustawienie dodaje ciąg połączenia bazy danych LocalDB.</span><span class="sxs-lookup"><span data-stu-id="6fb44-149">This setting adds a connection string for a LocalDB database.</span></span> <span data-ttu-id="6fb44-150">Ta baza danych będzie można użyć, gdy lokalne uruchamianie aplikacji.</span><span class="sxs-lookup"><span data-stu-id="6fb44-150">This database will be used when you run the app locally.</span></span>

<span data-ttu-id="6fb44-151">Następnie Dodaj klasę o nazwie `ProductsContext` do folderu modeli:</span><span class="sxs-lookup"><span data-stu-id="6fb44-151">Next, add a class named `ProductsContext` to the Models folder:</span></span>

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample5.cs)]

<span data-ttu-id="6fb44-152">W Konstruktorze `"name=ProductsContext"` nadaje nazwę parametrów połączenia.</span><span class="sxs-lookup"><span data-stu-id="6fb44-152">In the constructor, `"name=ProductsContext"` gives the name of the connection string.</span></span>

## <a name="configure-the-odata-endpoint"></a><span data-ttu-id="6fb44-153">Konfigurowanie punktu końcowego OData</span><span class="sxs-lookup"><span data-stu-id="6fb44-153">Configure the OData Endpoint</span></span>

<span data-ttu-id="6fb44-154">Otwórz plik aplikacji\_Start/WebApiConfig.cs.</span><span class="sxs-lookup"><span data-stu-id="6fb44-154">Open the file App\_Start/WebApiConfig.cs.</span></span> <span data-ttu-id="6fb44-155">Dodaj następujące **przy użyciu** instrukcji:</span><span class="sxs-lookup"><span data-stu-id="6fb44-155">Add the following **using** statements:</span></span>

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample6.cs)]

<span data-ttu-id="6fb44-156">Następnie dodaj następujący kod, aby **zarejestrować** metody:</span><span class="sxs-lookup"><span data-stu-id="6fb44-156">Then add the following code to the **Register** method:</span></span>

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample7.cs)]

<span data-ttu-id="6fb44-157">Ten kod wykonuje dwie czynności:</span><span class="sxs-lookup"><span data-stu-id="6fb44-157">This code does two things:</span></span>

- <span data-ttu-id="6fb44-158">Tworzy modelu Entity Data Model (EDM).</span><span class="sxs-lookup"><span data-stu-id="6fb44-158">Creates an Entity Data Model (EDM).</span></span>
- <span data-ttu-id="6fb44-159">Dodaje trasę.</span><span class="sxs-lookup"><span data-stu-id="6fb44-159">Adds a route.</span></span>

<span data-ttu-id="6fb44-160">EDM jest abstrakcyjny modelu danych.</span><span class="sxs-lookup"><span data-stu-id="6fb44-160">An EDM is an abstract model of the data.</span></span> <span data-ttu-id="6fb44-161">EDM służy do tworzenia dokumentu metadanych usługi.</span><span class="sxs-lookup"><span data-stu-id="6fb44-161">The EDM is used to create the service metadata document.</span></span> <span data-ttu-id="6fb44-162">**ODataConventionModelBuilder** klasy tworzy EDM przy użyciu domyślnych konwencji nazewnictwa.</span><span class="sxs-lookup"><span data-stu-id="6fb44-162">The **ODataConventionModelBuilder** class creates an EDM by using default naming conventions.</span></span> <span data-ttu-id="6fb44-163">Ta metoda wymaga co najmniej kodu.</span><span class="sxs-lookup"><span data-stu-id="6fb44-163">This approach requires the least code.</span></span> <span data-ttu-id="6fb44-164">Jeśli chcesz mieć większą kontrolę nad EDM, możesz użyć **element ODataModelBuilder** klasy można utworzyć przez dodanie jawnie właściwości, kluczy i właściwości nawigacji EDM.</span><span class="sxs-lookup"><span data-stu-id="6fb44-164">If you want more control over the EDM, you can use the **ODataModelBuilder** class to create the EDM by adding properties, keys, and navigation properties explicitly.</span></span>

<span data-ttu-id="6fb44-165">A *trasy* informuje interfejsu API sieci Web, jak można przekierować żądania HTTP do punktu końcowego.</span><span class="sxs-lookup"><span data-stu-id="6fb44-165">A *route* tells Web API how to route HTTP requests to the endpoint.</span></span> <span data-ttu-id="6fb44-166">Aby utworzyć trasę OData v4, należy wywołać **MapODataServiceRoute** — metoda rozszerzenia.</span><span class="sxs-lookup"><span data-stu-id="6fb44-166">To create an OData v4 route, call the **MapODataServiceRoute** extension method.</span></span>

<span data-ttu-id="6fb44-167">Jeśli aplikacja ma wiele punktów końcowych OData, Utwórz oddzielne trasy dla każdego.</span><span class="sxs-lookup"><span data-stu-id="6fb44-167">If your application has multiple OData endpoints, create a separate route for each.</span></span> <span data-ttu-id="6fb44-168">Nadaj każdej trasy trasy unikatową nazwę i prefiksu.</span><span class="sxs-lookup"><span data-stu-id="6fb44-168">Give each route a unique route name and prefix.</span></span>

## <a name="add-the-odata-controller"></a><span data-ttu-id="6fb44-169">Dodawanie kontrolera OData</span><span class="sxs-lookup"><span data-stu-id="6fb44-169">Add the OData Controller</span></span>

<span data-ttu-id="6fb44-170">A *kontrolera* jest klasa, która obsługuje żądania HTTP.</span><span class="sxs-lookup"><span data-stu-id="6fb44-170">A *controller* is a class that handles HTTP requests.</span></span> <span data-ttu-id="6fb44-171">Możesz utworzyć oddzielne kontrolera dla każdego obiektu w usłudze OData.</span><span class="sxs-lookup"><span data-stu-id="6fb44-171">You create a separate controller for each entity set in your OData service.</span></span> <span data-ttu-id="6fb44-172">W tym samouczku utworzysz jeden kontroler dla `Product` jednostki.</span><span class="sxs-lookup"><span data-stu-id="6fb44-172">In this tutorial, you will create one controller, for the `Product` entity.</span></span>

<span data-ttu-id="6fb44-173">W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy folder kontrolery, a następnie wybierz **Dodaj** &gt; **klasy**.</span><span class="sxs-lookup"><span data-stu-id="6fb44-173">In Solution Explorer, right-click the Controllers folder and select **Add** &gt; **Class**.</span></span> <span data-ttu-id="6fb44-174">Nazwa klasy `ProductsController`.</span><span class="sxs-lookup"><span data-stu-id="6fb44-174">Name the class `ProductsController`.</span></span>

> [!NOTE]
> <span data-ttu-id="6fb44-175">Wersja tego samouczka dla OData v3 używa **Dodaj kontroler** szkieletów.</span><span class="sxs-lookup"><span data-stu-id="6fb44-175">The version of this tutorial for OData v3 uses the **Add Controller** scaffolding.</span></span> <span data-ttu-id="6fb44-176">Obecnie nie są nie funkcją szkieletów dla protokołu OData v4.</span><span class="sxs-lookup"><span data-stu-id="6fb44-176">Currently, there is no scaffolding for OData v4.</span></span>


<span data-ttu-id="6fb44-177">Zamień schematyczny kod w ProductsController.cs poniżej.</span><span class="sxs-lookup"><span data-stu-id="6fb44-177">Replace the boilerplate code in ProductsController.cs with the following.</span></span>

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample8.cs)]

<span data-ttu-id="6fb44-178">Używa kontrolera `ProductsContext` klasy dostęp do bazy danych przy użyciu EF.</span><span class="sxs-lookup"><span data-stu-id="6fb44-178">The controller uses the `ProductsContext` class to access the database using EF.</span></span> <span data-ttu-id="6fb44-179">Należy zauważyć, że kontroler zastępuje **Dispose** metody zlikwidować **ProductsContext**.</span><span class="sxs-lookup"><span data-stu-id="6fb44-179">Notice that the controller overrides the **Dispose** method to dispose of the **ProductsContext**.</span></span>

<span data-ttu-id="6fb44-180">To jest punkt początkowy dla kontrolera.</span><span class="sxs-lookup"><span data-stu-id="6fb44-180">This is the starting point for the controller.</span></span> <span data-ttu-id="6fb44-181">Następnie dodamy metod dla wszystkich operacji CRUD.</span><span class="sxs-lookup"><span data-stu-id="6fb44-181">Next, we'll add methods for all of the CRUD operations.</span></span>

## <a name="querying-the-entity-set"></a><span data-ttu-id="6fb44-182">Zapytanie dotyczące zestawu jednostek</span><span class="sxs-lookup"><span data-stu-id="6fb44-182">Querying the Entity Set</span></span>

<span data-ttu-id="6fb44-183">Dodaj następujące metody umożliwiające `ProductsController`.</span><span class="sxs-lookup"><span data-stu-id="6fb44-183">Add the following methods to `ProductsController`.</span></span>

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample9.cs)]

<span data-ttu-id="6fb44-184">Wersja bez parametrów `Get` metoda zwraca całą kolekcję produktów.</span><span class="sxs-lookup"><span data-stu-id="6fb44-184">The parameterless version of the `Get` method returns the entire Products collection.</span></span> <span data-ttu-id="6fb44-185">`Get` Metody z *klucza* parametru wyszukuje produktu według jego klucza (w tym przypadku `Id` właściwości).</span><span class="sxs-lookup"><span data-stu-id="6fb44-185">The `Get` method with a *key* parameter looks up a product by its key (in this case, the `Id` property).</span></span>

<span data-ttu-id="6fb44-186">**[EnableQuery]** atrybutu umożliwia klientom zmodyfikować zapytanie, za pomocą opcji zapytania $filter, $sort i $page.</span><span class="sxs-lookup"><span data-stu-id="6fb44-186">The **[EnableQuery]** attribute enables clients to modify the query, by using query options such as $filter, $sort, and $page.</span></span> <span data-ttu-id="6fb44-187">Aby uzyskać więcej informacji, zobacz [obsługi opcji zapytania OData](../supporting-odata-query-options.md).</span><span class="sxs-lookup"><span data-stu-id="6fb44-187">For more information, see [Supporting OData Query Options](../supporting-odata-query-options.md).</span></span>

## <a name="adding-an-entity-to-the-entity-set"></a><span data-ttu-id="6fb44-188">Dodawanie jednostki do zestawu jednostek</span><span class="sxs-lookup"><span data-stu-id="6fb44-188">Adding an Entity to the Entity Set</span></span>

<span data-ttu-id="6fb44-189">Aby umożliwić klientom dodawanie nowego produktu do bazy danych, dodaj następującą metodę do `ProductsController`.</span><span class="sxs-lookup"><span data-stu-id="6fb44-189">To enable clients to add a new product to the database, add the following method to `ProductsController`.</span></span>

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample10.cs)]

## <a name="updating-an-entity"></a><span data-ttu-id="6fb44-190">Aktualizowanie jednostki</span><span class="sxs-lookup"><span data-stu-id="6fb44-190">Updating an Entity</span></span>

<span data-ttu-id="6fb44-191">OData obsługuje dwa semantykę różną aktualizowania jednostki, poprawki i PUT.</span><span class="sxs-lookup"><span data-stu-id="6fb44-191">OData supports two different semantics for updating an entity, PATCH and PUT.</span></span>

- <span data-ttu-id="6fb44-192">POPRAWKA wykonuje częściowej aktualizacji.</span><span class="sxs-lookup"><span data-stu-id="6fb44-192">PATCH performs a partial update.</span></span> <span data-ttu-id="6fb44-193">Klient określa tylko właściwości do zaktualizowania.</span><span class="sxs-lookup"><span data-stu-id="6fb44-193">The client specifies just the properties to update.</span></span>
- <span data-ttu-id="6fb44-194">Umieść zastępuje całej jednostki.</span><span class="sxs-lookup"><span data-stu-id="6fb44-194">PUT replaces the entire entity.</span></span>

<span data-ttu-id="6fb44-195">Wadą PUT jest klient musi wysłać wartości dla wszystkich właściwości w obiekcie, w tym wartości, które nie są zmieniane.</span><span class="sxs-lookup"><span data-stu-id="6fb44-195">The disadvantage of PUT is that the client must send values for all of the properties in the entity, including values that are not changing.</span></span> <span data-ttu-id="6fb44-196">[Specyfikację OData](http://docs.oasis-open.org/odata/odata/v4.0/os/part1-protocol/odata-v4.0-os-part1-protocol.html#_Toc372793719) stwierdza, że poprawka jest preferowana.</span><span class="sxs-lookup"><span data-stu-id="6fb44-196">The [OData spec](http://docs.oasis-open.org/odata/odata/v4.0/os/part1-protocol/odata-v4.0-os-part1-protocol.html#_Toc372793719) states that PATCH is preferred.</span></span>

<span data-ttu-id="6fb44-197">W każdym przypadku Oto kod dla metody zarówno poprawki i PUT:</span><span class="sxs-lookup"><span data-stu-id="6fb44-197">In any case, here is the code for both PATCH and PUT methods:</span></span>

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample11.cs)]

<span data-ttu-id="6fb44-198">W przypadku poprawek, używa kontrolera **Delta&lt;T&gt;**  typ do śledzenia zmian.</span><span class="sxs-lookup"><span data-stu-id="6fb44-198">In the case of PATCH, the controller uses the **Delta&lt;T&gt;** type to track the changes.</span></span>

## <a name="deleting-an-entity"></a><span data-ttu-id="6fb44-199">Usuwanie jednostki</span><span class="sxs-lookup"><span data-stu-id="6fb44-199">Deleting an Entity</span></span>

<span data-ttu-id="6fb44-200">Aby umożliwić klientom usuwanie produktu z bazy danych, dodaj następującą metodę do `ProductsController`.</span><span class="sxs-lookup"><span data-stu-id="6fb44-200">To enable clients to delete a product from the database, add the following method to `ProductsController`.</span></span>

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample12.cs)]
