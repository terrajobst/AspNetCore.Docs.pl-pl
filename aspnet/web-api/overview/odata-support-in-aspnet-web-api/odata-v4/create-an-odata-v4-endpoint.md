---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/create-an-odata-v4-endpoint
title: Tworzenie punktu końcowego OData v4 przy użyciu wzorca ASP.NET Web API 2.2 | Dokumentacja firmy Microsoft
author: MikeWasson
description: Open Data Protocol (OData) to protokół dostępu do danych w sieci Web. Protokół OData zapewnia jednolity sposób, w celu wykonywania zapytań i manipulowania zestawami danych przy użyciu operacji CRUD...
ms.author: aspnetcontent
ms.date: 06/24/2014
ms.assetid: 1e1927c0-ded1-4752-80fd-a146628d2f09
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/create-an-odata-v4-endpoint
msc.type: authoredcontent
ms.openlocfilehash: 04fad9b569972f11256c6b7288db34d4996ca8bf
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/05/2018
ms.locfileid: "37804215"
---
<a name="create-an-odata-v4-endpoint-using-aspnet-web-api-22"></a><span data-ttu-id="3d90e-104">Tworzenie punktu końcowego OData v4 przy użyciu wzorca ASP.NET Web API 2.2</span><span class="sxs-lookup"><span data-stu-id="3d90e-104">Create an OData v4 Endpoint Using ASP.NET Web API 2.2</span></span>
====================
<span data-ttu-id="3d90e-105">przez [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="3d90e-105">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

> <span data-ttu-id="3d90e-106">Open Data Protocol (OData) to protokół dostępu do danych w sieci Web.</span><span class="sxs-lookup"><span data-stu-id="3d90e-106">The Open Data Protocol (OData) is a data access protocol for the web.</span></span> <span data-ttu-id="3d90e-107">Protokół OData zapewnia jednolity sposób, w celu wykonywania zapytań i manipulowania zestawami danych przy użyciu operacji CRUD (tworzenia, odczytu, aktualizacji i usuwania).</span><span class="sxs-lookup"><span data-stu-id="3d90e-107">OData provides a uniform way to query and manipulate data sets through CRUD operations (create, read, update, and delete).</span></span>
> 
> <span data-ttu-id="3d90e-108">Web API platformy ASP.NET obsługuje zarówno w wersji 3 i 4 protokołu.</span><span class="sxs-lookup"><span data-stu-id="3d90e-108">ASP.NET Web API supports both v3 and v4 of the protocol.</span></span> <span data-ttu-id="3d90e-109">Można nawet mieć punktu końcowego w wersji 4, który uruchamia side-by-side z punktem końcowym v3.</span><span class="sxs-lookup"><span data-stu-id="3d90e-109">You can even have a v4 endpoint that runs side-by-side with a v3 endpoint.</span></span>
> 
> <span data-ttu-id="3d90e-110">W tym samouczku przedstawiono sposób tworzenia punktu końcowego OData v4 obsługującego operacje CRUD.</span><span class="sxs-lookup"><span data-stu-id="3d90e-110">This tutorial shows how to create an OData v4 endpoint that supports CRUD operations.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="3d90e-111">Wersje oprogramowania używanego w tym samouczku</span><span class="sxs-lookup"><span data-stu-id="3d90e-111">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="3d90e-112">Składnik Web API 2.2</span><span class="sxs-lookup"><span data-stu-id="3d90e-112">Web API 2.2</span></span>
> - <span data-ttu-id="3d90e-113">Protokołu OData v4</span><span class="sxs-lookup"><span data-stu-id="3d90e-113">OData v4</span></span>
> - [<span data-ttu-id="3d90e-114">Visual Studio 2013 Update 2</span><span class="sxs-lookup"><span data-stu-id="3d90e-114">Visual Studio 2013 Update 2</span></span>](https://www.visualstudio.com/downloads/download-visual-studio-vs)
> - <span data-ttu-id="3d90e-115">Entity Framework 6</span><span class="sxs-lookup"><span data-stu-id="3d90e-115">Entity Framework 6</span></span>
> - <span data-ttu-id="3d90e-116">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="3d90e-116">.NET 4.5</span></span>
> 
> 
> ## <a name="tutorial-versions"></a><span data-ttu-id="3d90e-117">Samouczek wersji</span><span class="sxs-lookup"><span data-stu-id="3d90e-117">Tutorial versions</span></span>
> 
> <span data-ttu-id="3d90e-118">Dla protokołu OData w wersji 3, zobacz [Tworzenie punktu końcowego OData v3](../odata-v3/creating-an-odata-endpoint.md).</span><span class="sxs-lookup"><span data-stu-id="3d90e-118">For the OData Version 3, see [Creating an OData v3 Endpoint](../odata-v3/creating-an-odata-endpoint.md).</span></span>


## <a name="create-the-visual-studio-project"></a><span data-ttu-id="3d90e-119">Tworzenie projektu programu Visual Studio</span><span class="sxs-lookup"><span data-stu-id="3d90e-119">Create the Visual Studio Project</span></span>

<span data-ttu-id="3d90e-120">W programie Visual Studio z **pliku** menu, wybierz opcję **New** &gt; **projektu**.</span><span class="sxs-lookup"><span data-stu-id="3d90e-120">In Visual Studio, from the **File** menu, select **New** &gt; **Project**.</span></span>

<span data-ttu-id="3d90e-121">Rozwiń **zainstalowane** &gt; **szablony** &gt; **Visual C#** &gt; **Web**i wybierz  **Aplikacja sieci Web ASP.NET** szablonu.</span><span class="sxs-lookup"><span data-stu-id="3d90e-121">Expand **Installed** &gt; **Templates** &gt; **Visual C#** &gt; **Web**, and select the **ASP.NET Web Application** template.</span></span> <span data-ttu-id="3d90e-122">Nadaj projektowi nazwę &quot;ProductService&quot;.</span><span class="sxs-lookup"><span data-stu-id="3d90e-122">Name the project &quot;ProductService&quot;.</span></span>

[![](create-an-odata-v4-endpoint/_static/image2.png)](create-an-odata-v4-endpoint/_static/image1.png)

<span data-ttu-id="3d90e-123">W **nowy projekt** okno dialogowe, wybierz opcję **pusty** szablonu.</span><span class="sxs-lookup"><span data-stu-id="3d90e-123">In the **New Project** dialog, select the **Empty** template.</span></span> <span data-ttu-id="3d90e-124">W obszarze &quot;. Dodaj foldery i podstawowe odwołania... &quot;, kliknij przycisk **interfejsu API sieci Web**.</span><span class="sxs-lookup"><span data-stu-id="3d90e-124">Under &quot;Add folders and core references...&quot;, click **Web API**.</span></span> <span data-ttu-id="3d90e-125">Kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="3d90e-125">Click **OK**.</span></span>

[![](create-an-odata-v4-endpoint/_static/image4.png)](create-an-odata-v4-endpoint/_static/image3.png)

## <a name="install-the-odata-packages"></a><span data-ttu-id="3d90e-126">Instalowanie pakietów OData</span><span class="sxs-lookup"><span data-stu-id="3d90e-126">Install the OData Packages</span></span>

<span data-ttu-id="3d90e-127">Z **narzędzia** menu, wybierz opcję **Menedżera pakietów NuGet** &gt; **Konsola Menedżera pakietów**.</span><span class="sxs-lookup"><span data-stu-id="3d90e-127">From the **Tools** menu, select **NuGet Package Manager** &gt; **Package Manager Console**.</span></span> <span data-ttu-id="3d90e-128">W oknie Konsola Menedżera pakietów wpisz polecenie:</span><span class="sxs-lookup"><span data-stu-id="3d90e-128">In the Package Manager Console window, type:</span></span>

[!code-console[Main](create-an-odata-v4-endpoint/samples/sample1.cmd)]

<span data-ttu-id="3d90e-129">To polecenie powoduje zainstalowanie najnowszych pakietów OData NuGet.</span><span class="sxs-lookup"><span data-stu-id="3d90e-129">This command installs the latest OData NuGet packages.</span></span>

## <a name="add-a-model-class"></a><span data-ttu-id="3d90e-130">Dodawanie klasy modelu</span><span class="sxs-lookup"><span data-stu-id="3d90e-130">Add a Model Class</span></span>

<span data-ttu-id="3d90e-131">A *modelu* jest obiektem, który reprezentuje jednostki danych w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="3d90e-131">A *model* is an object that represents a data entity in your application.</span></span>

<span data-ttu-id="3d90e-132">W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy folderu modeli.</span><span class="sxs-lookup"><span data-stu-id="3d90e-132">In Solution Explorer, right-click the Models folder.</span></span> <span data-ttu-id="3d90e-133">Z menu kontekstowego wybierz **Dodaj** &gt; **klasy**.</span><span class="sxs-lookup"><span data-stu-id="3d90e-133">From the context menu, select **Add** &gt; **Class**.</span></span>

[![](create-an-odata-v4-endpoint/_static/image6.png)](create-an-odata-v4-endpoint/_static/image5.png)

> [!NOTE]
> <span data-ttu-id="3d90e-134">Zgodnie z Konwencją klasy modelu są umieszczane w folderze modeli, ale nie musisz przestrzegać tej Konwencji we własnych projektach.</span><span class="sxs-lookup"><span data-stu-id="3d90e-134">By convention, model classes are placed in the Models folder, but you don't have to follow this convention in your own projects.</span></span>


<span data-ttu-id="3d90e-135">Nazwa klasy `Product`.</span><span class="sxs-lookup"><span data-stu-id="3d90e-135">Name the class `Product`.</span></span> <span data-ttu-id="3d90e-136">W pliku Product.cs Zastąp standardowy kod następujących czynności:</span><span class="sxs-lookup"><span data-stu-id="3d90e-136">In the Product.cs file, replace the boilerplate code with the following:</span></span>

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample2.cs)]

<span data-ttu-id="3d90e-137">`Id` Właściwość jest kluczem jednostki.</span><span class="sxs-lookup"><span data-stu-id="3d90e-137">The `Id` property is the entity key.</span></span> <span data-ttu-id="3d90e-138">Klienci mogą wysyłać zapytania jednostek według klucza.</span><span class="sxs-lookup"><span data-stu-id="3d90e-138">Clients can query entities by key.</span></span> <span data-ttu-id="3d90e-139">Na przykład, aby uzyskać produkt o identyfikatorze 5, identyfikator URI jest `/Products(5)`.</span><span class="sxs-lookup"><span data-stu-id="3d90e-139">For example, to get the product with ID of 5, the URI is `/Products(5)`.</span></span> <span data-ttu-id="3d90e-140">`Id` Właściwość będzie również klucz podstawowy w wewnętrznej bazy danych.</span><span class="sxs-lookup"><span data-stu-id="3d90e-140">The `Id` property will also be the primary key in the back-end database.</span></span>

## <a name="enable-entity-framework"></a><span data-ttu-id="3d90e-141">Włączanie programu Entity Framework</span><span class="sxs-lookup"><span data-stu-id="3d90e-141">Enable Entity Framework</span></span>

<span data-ttu-id="3d90e-142">W tym samouczku użyjemy Entity Framework (EF) Code First platformy do utworzenia wewnętrznej bazy danych.</span><span class="sxs-lookup"><span data-stu-id="3d90e-142">For this tutorial, we'll use Entity Framework (EF) Code First to create the back-end database.</span></span>

> [!NOTE]
> <span data-ttu-id="3d90e-143">Web API OData nie wymaga EF.</span><span class="sxs-lookup"><span data-stu-id="3d90e-143">Web API OData does not require EF.</span></span> <span data-ttu-id="3d90e-144">Użyj dowolnej warstwy dostępu do danych, która może dokonywać translacji jednostek bazy danych modeli.</span><span class="sxs-lookup"><span data-stu-id="3d90e-144">Use any data-access layer that can translate database entities into models.</span></span>


<span data-ttu-id="3d90e-145">Najpierw zainstaluj pakiet NuGet dla platformy EF.</span><span class="sxs-lookup"><span data-stu-id="3d90e-145">First, install the NuGet package for EF.</span></span> <span data-ttu-id="3d90e-146">Z **narzędzia** menu, wybierz opcję **Menedżera pakietów NuGet** &gt; **Konsola Menedżera pakietów**.</span><span class="sxs-lookup"><span data-stu-id="3d90e-146">From the **Tools** menu, select **NuGet Package Manager** &gt; **Package Manager Console**.</span></span> <span data-ttu-id="3d90e-147">W oknie Konsola Menedżera pakietów wpisz polecenie:</span><span class="sxs-lookup"><span data-stu-id="3d90e-147">In the Package Manager Console window, type:</span></span>

[!code-console[Main](create-an-odata-v4-endpoint/samples/sample3.cmd)]

<span data-ttu-id="3d90e-148">Otwórz plik Web.config i dodaj następującą sekcję wewnątrz **konfiguracji** elementu po **configSections** elementu.</span><span class="sxs-lookup"><span data-stu-id="3d90e-148">Open the Web.config file, and add the following section inside the **configuration** element, after the **configSections** element.</span></span>

[!code-xml[Main](create-an-odata-v4-endpoint/samples/sample4.xml?highlight=6)]

<span data-ttu-id="3d90e-149">To ustawienie dodaje parametry połączenia dla bazy danych LocalDB.</span><span class="sxs-lookup"><span data-stu-id="3d90e-149">This setting adds a connection string for a LocalDB database.</span></span> <span data-ttu-id="3d90e-150">Ta baza danych będzie służyć lokalnie uruchomić aplikację.</span><span class="sxs-lookup"><span data-stu-id="3d90e-150">This database will be used when you run the app locally.</span></span>

<span data-ttu-id="3d90e-151">Następnie Dodaj klasę o nazwie `ProductsContext` do folderu modeli:</span><span class="sxs-lookup"><span data-stu-id="3d90e-151">Next, add a class named `ProductsContext` to the Models folder:</span></span>

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample5.cs)]

<span data-ttu-id="3d90e-152">W Konstruktorze `"name=ProductsContext"` nadaje nazwę parametrów połączenia.</span><span class="sxs-lookup"><span data-stu-id="3d90e-152">In the constructor, `"name=ProductsContext"` gives the name of the connection string.</span></span>

## <a name="configure-the-odata-endpoint"></a><span data-ttu-id="3d90e-153">Skonfiguruj punkt końcowy OData</span><span class="sxs-lookup"><span data-stu-id="3d90e-153">Configure the OData Endpoint</span></span>

<span data-ttu-id="3d90e-154">Otwórz plik App\_Start/WebApiConfig.cs.</span><span class="sxs-lookup"><span data-stu-id="3d90e-154">Open the file App\_Start/WebApiConfig.cs.</span></span> <span data-ttu-id="3d90e-155">Dodaj następujący kod **przy użyciu** instrukcji:</span><span class="sxs-lookup"><span data-stu-id="3d90e-155">Add the following **using** statements:</span></span>

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample6.cs)]

<span data-ttu-id="3d90e-156">Następnie dodaj następujący kod do **zarejestrować** metody:</span><span class="sxs-lookup"><span data-stu-id="3d90e-156">Then add the following code to the **Register** method:</span></span>

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample7.cs)]

<span data-ttu-id="3d90e-157">Ten kod wykonuje dwie czynności:</span><span class="sxs-lookup"><span data-stu-id="3d90e-157">This code does two things:</span></span>

- <span data-ttu-id="3d90e-158">Tworzy model Entity Data Model (EDM struktury).</span><span class="sxs-lookup"><span data-stu-id="3d90e-158">Creates an Entity Data Model (EDM).</span></span>
- <span data-ttu-id="3d90e-159">Dodaje trasę.</span><span class="sxs-lookup"><span data-stu-id="3d90e-159">Adds a route.</span></span>

<span data-ttu-id="3d90e-160">EDM jest abstrakcyjny model danych.</span><span class="sxs-lookup"><span data-stu-id="3d90e-160">An EDM is an abstract model of the data.</span></span> <span data-ttu-id="3d90e-161">EDM jest używany do utworzenia dokumentu metadanych usługi.</span><span class="sxs-lookup"><span data-stu-id="3d90e-161">The EDM is used to create the service metadata document.</span></span> <span data-ttu-id="3d90e-162">**ODataConventionModelBuilder** klasy tworzy EDM przy użyciu domyślnych konwencji nazewnictwa.</span><span class="sxs-lookup"><span data-stu-id="3d90e-162">The **ODataConventionModelBuilder** class creates an EDM by using default naming conventions.</span></span> <span data-ttu-id="3d90e-163">Takie podejście wymaga co najmniej kodu.</span><span class="sxs-lookup"><span data-stu-id="3d90e-163">This approach requires the least code.</span></span> <span data-ttu-id="3d90e-164">Jeśli chcesz, aby mieć większą kontrolę nad EDM, możesz użyć **element ODataModelBuilder** klasy w celu utworzenia EDM, jawnie dodając właściwości, kluczy i właściwości nawigacji.</span><span class="sxs-lookup"><span data-stu-id="3d90e-164">If you want more control over the EDM, you can use the **ODataModelBuilder** class to create the EDM by adding properties, keys, and navigation properties explicitly.</span></span>

<span data-ttu-id="3d90e-165">A *trasy* zawiera informacje dotyczące określenia trasy żądań HTTP do punktu końcowego interfejsu API sieci Web.</span><span class="sxs-lookup"><span data-stu-id="3d90e-165">A *route* tells Web API how to route HTTP requests to the endpoint.</span></span> <span data-ttu-id="3d90e-166">Aby utworzyć trasę OData v4, wywołaj **MapODataServiceRoute** — metoda rozszerzenia.</span><span class="sxs-lookup"><span data-stu-id="3d90e-166">To create an OData v4 route, call the **MapODataServiceRoute** extension method.</span></span>

<span data-ttu-id="3d90e-167">Jeśli aplikacja ma wiele punktów końcowych OData, Utwórz oddzielne trasy dla każdego.</span><span class="sxs-lookup"><span data-stu-id="3d90e-167">If your application has multiple OData endpoints, create a separate route for each.</span></span> <span data-ttu-id="3d90e-168">Nadaj każdej trasy trasy unikatową nazwę i prefiksu.</span><span class="sxs-lookup"><span data-stu-id="3d90e-168">Give each route a unique route name and prefix.</span></span>

## <a name="add-the-odata-controller"></a><span data-ttu-id="3d90e-169">Dodawanie kontrolera OData</span><span class="sxs-lookup"><span data-stu-id="3d90e-169">Add the OData Controller</span></span>

<span data-ttu-id="3d90e-170">A *kontrolera* to klasa, która obsługuje żądania HTTP.</span><span class="sxs-lookup"><span data-stu-id="3d90e-170">A *controller* is a class that handles HTTP requests.</span></span> <span data-ttu-id="3d90e-171">Możesz utworzyć oddzielne kontrolera dla każdej jednostki w usługi OData.</span><span class="sxs-lookup"><span data-stu-id="3d90e-171">You create a separate controller for each entity set in your OData service.</span></span> <span data-ttu-id="3d90e-172">W tym samouczku utworzysz jednego kontrolera dla `Product` jednostki.</span><span class="sxs-lookup"><span data-stu-id="3d90e-172">In this tutorial, you will create one controller, for the `Product` entity.</span></span>

<span data-ttu-id="3d90e-173">W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy folder kontrolerów, a następnie wybierz **Dodaj** &gt; **klasy**.</span><span class="sxs-lookup"><span data-stu-id="3d90e-173">In Solution Explorer, right-click the Controllers folder and select **Add** &gt; **Class**.</span></span> <span data-ttu-id="3d90e-174">Nazwa klasy `ProductsController`.</span><span class="sxs-lookup"><span data-stu-id="3d90e-174">Name the class `ProductsController`.</span></span>

> [!NOTE]
> <span data-ttu-id="3d90e-175">Wersja tego samouczka dla protokołu OData v3 używa **Dodaj kontroler** tworzenia szkieletów.</span><span class="sxs-lookup"><span data-stu-id="3d90e-175">The version of this tutorial for OData v3 uses the **Add Controller** scaffolding.</span></span> <span data-ttu-id="3d90e-176">Obecnie nie ma żadnych funkcją szkieletów dla protokołu OData v4.</span><span class="sxs-lookup"><span data-stu-id="3d90e-176">Currently, there is no scaffolding for OData v4.</span></span>


<span data-ttu-id="3d90e-177">Zamień schematyczny kod w ProductsController.cs poniżej.</span><span class="sxs-lookup"><span data-stu-id="3d90e-177">Replace the boilerplate code in ProductsController.cs with the following.</span></span>

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample8.cs)]

<span data-ttu-id="3d90e-178">Zastosowań kontrolera `ProductsContext` klasy dostępu do bazy danych przy użyciu programu EF.</span><span class="sxs-lookup"><span data-stu-id="3d90e-178">The controller uses the `ProductsContext` class to access the database using EF.</span></span> <span data-ttu-id="3d90e-179">Należy zauważyć, że kontroler zastępuje **Dispose** metodę, aby usunąć **ProductsContext**.</span><span class="sxs-lookup"><span data-stu-id="3d90e-179">Notice that the controller overrides the **Dispose** method to dispose of the **ProductsContext**.</span></span>

<span data-ttu-id="3d90e-180">Jest to punkt początkowy dla kontrolera.</span><span class="sxs-lookup"><span data-stu-id="3d90e-180">This is the starting point for the controller.</span></span> <span data-ttu-id="3d90e-181">Następnie dodamy metody dla wszystkich operacji CRUD.</span><span class="sxs-lookup"><span data-stu-id="3d90e-181">Next, we'll add methods for all of the CRUD operations.</span></span>

## <a name="querying-the-entity-set"></a><span data-ttu-id="3d90e-182">Zapytanie dotyczące zestawu jednostek</span><span class="sxs-lookup"><span data-stu-id="3d90e-182">Querying the Entity Set</span></span>

<span data-ttu-id="3d90e-183">Dodaj następujące metody umożliwiające `ProductsController`.</span><span class="sxs-lookup"><span data-stu-id="3d90e-183">Add the following methods to `ProductsController`.</span></span>

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample9.cs)]

<span data-ttu-id="3d90e-184">Bez parametrów wersję `Get` metoda zwraca całą kolekcję produktów.</span><span class="sxs-lookup"><span data-stu-id="3d90e-184">The parameterless version of the `Get` method returns the entire Products collection.</span></span> <span data-ttu-id="3d90e-185">`Get` Metody z *klucz* parametru wyszukuje produktu według jego klucza (w tym przypadku `Id` właściwości).</span><span class="sxs-lookup"><span data-stu-id="3d90e-185">The `Get` method with a *key* parameter looks up a product by its key (in this case, the `Id` property).</span></span>

<span data-ttu-id="3d90e-186">**[EnableQuery]** atrybut umożliwia klientom zmodyfikować zapytanie, za pomocą opcji zapytania $filter, $sort i $page.</span><span class="sxs-lookup"><span data-stu-id="3d90e-186">The **[EnableQuery]** attribute enables clients to modify the query, by using query options such as $filter, $sort, and $page.</span></span> <span data-ttu-id="3d90e-187">Aby uzyskać więcej informacji, zobacz [obsługi opcji zapytania OData](../supporting-odata-query-options.md).</span><span class="sxs-lookup"><span data-stu-id="3d90e-187">For more information, see [Supporting OData Query Options](../supporting-odata-query-options.md).</span></span>

## <a name="adding-an-entity-to-the-entity-set"></a><span data-ttu-id="3d90e-188">Dodawanie jednostki do zestawu jednostek</span><span class="sxs-lookup"><span data-stu-id="3d90e-188">Adding an Entity to the Entity Set</span></span>

<span data-ttu-id="3d90e-189">Aby umożliwić klientom dodać nowy produkt do bazy danych, dodaj następującą metodę do `ProductsController`.</span><span class="sxs-lookup"><span data-stu-id="3d90e-189">To enable clients to add a new product to the database, add the following method to `ProductsController`.</span></span>

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample10.cs)]

## <a name="updating-an-entity"></a><span data-ttu-id="3d90e-190">Aktualizowanie jednostki</span><span class="sxs-lookup"><span data-stu-id="3d90e-190">Updating an Entity</span></span>

<span data-ttu-id="3d90e-191">OData obsługuje dwie semantykę różną w przypadku aktualizowania jednostki, poprawki i PUT.</span><span class="sxs-lookup"><span data-stu-id="3d90e-191">OData supports two different semantics for updating an entity, PATCH and PUT.</span></span>

- <span data-ttu-id="3d90e-192">POPRAWKA wykonuje częściową aktualizację.</span><span class="sxs-lookup"><span data-stu-id="3d90e-192">PATCH performs a partial update.</span></span> <span data-ttu-id="3d90e-193">Klient określa tylko właściwości do zaktualizowania.</span><span class="sxs-lookup"><span data-stu-id="3d90e-193">The client specifies just the properties to update.</span></span>
- <span data-ttu-id="3d90e-194">Umieść zastępuje całej jednostki.</span><span class="sxs-lookup"><span data-stu-id="3d90e-194">PUT replaces the entire entity.</span></span>

<span data-ttu-id="3d90e-195">Wadą PUT jest, klient musi wysłać wartości dla wszystkich właściwości w obiekcie, łącznie z wartościami, które nie zmieniają się.</span><span class="sxs-lookup"><span data-stu-id="3d90e-195">The disadvantage of PUT is that the client must send values for all of the properties in the entity, including values that are not changing.</span></span> <span data-ttu-id="3d90e-196">[Specyfikacji protokołu OData](http://docs.oasis-open.org/odata/odata/v4.0/os/part1-protocol/odata-v4.0-os-part1-protocol.html#_Toc372793719) stwierdzający, że poprawka jest preferowana.</span><span class="sxs-lookup"><span data-stu-id="3d90e-196">The [OData spec](http://docs.oasis-open.org/odata/odata/v4.0/os/part1-protocol/odata-v4.0-os-part1-protocol.html#_Toc372793719) states that PATCH is preferred.</span></span>

<span data-ttu-id="3d90e-197">W każdym przypadku poniżej przedstawiono kod dla metody PUT i PATCH:</span><span class="sxs-lookup"><span data-stu-id="3d90e-197">In any case, here is the code for both PATCH and PUT methods:</span></span>

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample11.cs)]

<span data-ttu-id="3d90e-198">W przypadku poprawek, używa kontrolera **Delta&lt;T&gt;**  typ do śledzenia zmian.</span><span class="sxs-lookup"><span data-stu-id="3d90e-198">In the case of PATCH, the controller uses the **Delta&lt;T&gt;** type to track the changes.</span></span>

## <a name="deleting-an-entity"></a><span data-ttu-id="3d90e-199">Usuwanie jednostki</span><span class="sxs-lookup"><span data-stu-id="3d90e-199">Deleting an Entity</span></span>

<span data-ttu-id="3d90e-200">Aby umożliwić klientom usunąć produkt z bazy danych, dodaj następującą metodę do `ProductsController`.</span><span class="sxs-lookup"><span data-stu-id="3d90e-200">To enable clients to delete a product from the database, add the following method to `ProductsController`.</span></span>

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample12.cs)]
