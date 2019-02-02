---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/create-an-odata-v4-endpoint
title: Tworzenie punktu końcowego OData v4 przy użyciu wzorca ASP.NET Web API 2.2 | Dokumentacja firmy Microsoft
author: MikeWasson
description: Open Data Protocol (OData) to protokół dostępu do danych w sieci Web. Protokół OData zapewnia jednolity sposób, w celu wykonywania zapytań i manipulowania zestawami danych przy użyciu operacji CRUD...
ms.author: riande
ms.date: 01/23/2019
ms.assetid: 1e1927c0-ded1-4752-80fd-a146628d2f09
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/create-an-odata-v4-endpoint
msc.type: authoredcontent
ms.openlocfilehash: c6a4aa4eb563fd77d5afd9248175d5f5b7984d19
ms.sourcegitcommit: ed76cc752966c604a795fbc56d5a71d16ded0b58
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/02/2019
ms.locfileid: "55667573"
---
# <a name="create-an-odata-v4-endpoint-using-aspnet-web-api"></a><span data-ttu-id="b1f04-104">Tworzenie punktu końcowego OData v4 przy użyciu wzorca ASP.NET Web API</span><span class="sxs-lookup"><span data-stu-id="b1f04-104">Create an OData v4 Endpoint Using ASP.NET Web API</span></span> 

> <span data-ttu-id="b1f04-105">Open Data Protocol (OData) to protokół dostępu do danych w sieci Web.</span><span class="sxs-lookup"><span data-stu-id="b1f04-105">The Open Data Protocol (OData) is a data access protocol for the web.</span></span> <span data-ttu-id="b1f04-106">Protokół OData zapewnia jednolity sposób, w celu wykonywania zapytań i manipulowania zestawami danych przy użyciu operacji CRUD (tworzenia, odczytu, aktualizacji i usuwania).</span><span class="sxs-lookup"><span data-stu-id="b1f04-106">OData provides a uniform way to query and manipulate data sets through CRUD operations (create, read, update, and delete).</span></span>
>
> <span data-ttu-id="b1f04-107">Web API platformy ASP.NET obsługuje zarówno w wersji 3 i 4 protokołu.</span><span class="sxs-lookup"><span data-stu-id="b1f04-107">ASP.NET Web API supports both v3 and v4 of the protocol.</span></span> <span data-ttu-id="b1f04-108">Można nawet mieć punktu końcowego w wersji 4, który uruchamia side-by-side z punktem końcowym v3.</span><span class="sxs-lookup"><span data-stu-id="b1f04-108">You can even have a v4 endpoint that runs side-by-side with a v3 endpoint.</span></span>
>
> <span data-ttu-id="b1f04-109">W tym samouczku przedstawiono sposób tworzenia punktu końcowego OData v4 obsługującego operacje CRUD.</span><span class="sxs-lookup"><span data-stu-id="b1f04-109">This tutorial shows how to create an OData v4 endpoint that supports CRUD operations.</span></span>
>
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="b1f04-110">Wersje oprogramowania używanego w tym samouczku</span><span class="sxs-lookup"><span data-stu-id="b1f04-110">Software versions used in the tutorial</span></span>
>
> - <span data-ttu-id="b1f04-111">Internetowy interfejs API 5.2</span><span class="sxs-lookup"><span data-stu-id="b1f04-111">Web API 5.2</span></span>
> - <span data-ttu-id="b1f04-112">OData 4</span><span class="sxs-lookup"><span data-stu-id="b1f04-112">OData v4</span></span>
> - <span data-ttu-id="b1f04-113">Program Visual Studio 2017 (Pobierz program Visual Studio 2017 [tutaj](https://visualstudio.microsoft.com/downloads/))</span><span class="sxs-lookup"><span data-stu-id="b1f04-113">Visual Studio 2017 (download Visual Studio 2017 [here](https://visualstudio.microsoft.com/downloads/))</span></span>
> - <span data-ttu-id="b1f04-114">Entity Framework 6</span><span class="sxs-lookup"><span data-stu-id="b1f04-114">Entity Framework 6</span></span>
> - <span data-ttu-id="b1f04-115">.NET 4.7.2</span><span class="sxs-lookup"><span data-stu-id="b1f04-115">.NET 4.7.2</span></span>
>
> ## <a name="tutorial-versions"></a><span data-ttu-id="b1f04-116">Samouczek wersji</span><span class="sxs-lookup"><span data-stu-id="b1f04-116">Tutorial versions</span></span>
>
> <span data-ttu-id="b1f04-117">Dla protokołu OData w wersji 3, zobacz [Tworzenie punktu końcowego OData v3](../odata-v3/creating-an-odata-endpoint.md).</span><span class="sxs-lookup"><span data-stu-id="b1f04-117">For the OData Version 3, see [Creating an OData v3 Endpoint](../odata-v3/creating-an-odata-endpoint.md).</span></span>

## <a name="create-the-visual-studio-project"></a><span data-ttu-id="b1f04-118">Tworzenie projektu programu Visual Studio</span><span class="sxs-lookup"><span data-stu-id="b1f04-118">Create the Visual Studio Project</span></span>

<span data-ttu-id="b1f04-119">W programie Visual Studio z **pliku** menu, wybierz opcję **New** &gt; **projektu**.</span><span class="sxs-lookup"><span data-stu-id="b1f04-119">In Visual Studio, from the **File** menu, select **New** &gt; **Project**.</span></span>

<span data-ttu-id="b1f04-120">Rozwiń **zainstalowane** &gt; **Visual C#**  &gt; **Web**i wybierz **aplikacji sieci Web platformy ASP.NET (.NET Framework)**  szablonu.</span><span class="sxs-lookup"><span data-stu-id="b1f04-120">Expand **Installed** &gt; **Visual C#** &gt; **Web**, and select the **ASP.NET Web Application (.NET Framework)** template.</span></span> <span data-ttu-id="b1f04-121">Nadaj projektowi nazwę &quot;ProductService&quot;.</span><span class="sxs-lookup"><span data-stu-id="b1f04-121">Name the project &quot;ProductService&quot;.</span></span>

[![](create-an-odata-v4-endpoint/_static/image7.png)](create-an-odata-v4-endpoint/_static/image7.png)

<span data-ttu-id="b1f04-122">Kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="b1f04-122">Select **OK**.</span></span>



[![](create-an-odata-v4-endpoint/_static/image8.png)](create-an-odata-v4-endpoint/_static/image8.png)

<span data-ttu-id="b1f04-123">Wybierz **pusty** szablonu.</span><span class="sxs-lookup"><span data-stu-id="b1f04-123">Select the **Empty** template.</span></span> <span data-ttu-id="b1f04-124">W obszarze **. Dodaj foldery i podstawowe odwołania dla:**, wybierz opcję **interfejsu API sieci Web**.</span><span class="sxs-lookup"><span data-stu-id="b1f04-124">Under **Add folders and core references for:**, select **Web API**.</span></span> <span data-ttu-id="b1f04-125">Kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="b1f04-125">Select **OK**.</span></span>

## <a name="install-the-odata-packages"></a><span data-ttu-id="b1f04-126">Instalowanie pakietów OData</span><span class="sxs-lookup"><span data-stu-id="b1f04-126">Install the OData packages</span></span>

<span data-ttu-id="b1f04-127">Z **narzędzia** menu, wybierz opcję **Menedżera pakietów NuGet** &gt; **Konsola Menedżera pakietów**.</span><span class="sxs-lookup"><span data-stu-id="b1f04-127">From the **Tools** menu, select **NuGet Package Manager** &gt; **Package Manager Console**.</span></span> <span data-ttu-id="b1f04-128">W oknie Konsola Menedżera pakietów wpisz polecenie:</span><span class="sxs-lookup"><span data-stu-id="b1f04-128">In the Package Manager Console window, type:</span></span>

[!code-console[Main](create-an-odata-v4-endpoint/samples/sample1.cmd)]

<span data-ttu-id="b1f04-129">To polecenie powoduje zainstalowanie najnowszych pakietów OData NuGet.</span><span class="sxs-lookup"><span data-stu-id="b1f04-129">This command installs the latest OData NuGet packages.</span></span>

## <a name="add-a-model-class"></a><span data-ttu-id="b1f04-130">Dodawanie klasy modelu</span><span class="sxs-lookup"><span data-stu-id="b1f04-130">Add a model class</span></span>

<span data-ttu-id="b1f04-131">A *modelu* jest obiektem, który reprezentuje jednostki danych w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="b1f04-131">A *model* is an object that represents a data entity in your application.</span></span>

<span data-ttu-id="b1f04-132">W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy folder Modele.</span><span class="sxs-lookup"><span data-stu-id="b1f04-132">In Solution Explorer, right-click the Models folder.</span></span> <span data-ttu-id="b1f04-133">Z menu kontekstowego wybierz **Dodaj** &gt; **klasy**.</span><span class="sxs-lookup"><span data-stu-id="b1f04-133">From the context menu, select **Add** &gt; **Class**.</span></span>

[![](create-an-odata-v4-endpoint/_static/image6.png)](create-an-odata-v4-endpoint/_static/image5.png)

> [!NOTE]
> <span data-ttu-id="b1f04-134">Zgodnie z Konwencją klasy modelu są umieszczane w folderze modeli, ale nie musisz przestrzegać tej Konwencji we własnych projektach.</span><span class="sxs-lookup"><span data-stu-id="b1f04-134">By convention, model classes are placed in the Models folder, but you don't have to follow this convention in your own projects.</span></span>


<span data-ttu-id="b1f04-135">Nazwa klasy `Product`.</span><span class="sxs-lookup"><span data-stu-id="b1f04-135">Name the class `Product`.</span></span> <span data-ttu-id="b1f04-136">W pliku Product.cs Zastąp standardowy kod następujących czynności:</span><span class="sxs-lookup"><span data-stu-id="b1f04-136">In the Product.cs file, replace the boilerplate code with the following:</span></span>

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample2.cs)]

<span data-ttu-id="b1f04-137">`Id` Właściwość jest kluczem jednostki.</span><span class="sxs-lookup"><span data-stu-id="b1f04-137">The `Id` property is the entity key.</span></span> <span data-ttu-id="b1f04-138">Klienci mogą wysyłać zapytania jednostek według klucza.</span><span class="sxs-lookup"><span data-stu-id="b1f04-138">Clients can query entities by key.</span></span> <span data-ttu-id="b1f04-139">Na przykład, aby uzyskać produkt o identyfikatorze 5, użyj identyfikatora URI:`/Products(5)`.</span><span class="sxs-lookup"><span data-stu-id="b1f04-139">For example, to get the product with ID of 5, the URI is `/Products(5)`.</span></span> <span data-ttu-id="b1f04-140">`Id` Właściwość będzie również klucz podstawowy w wewnętrznej bazy danych.</span><span class="sxs-lookup"><span data-stu-id="b1f04-140">The `Id` property will also be the primary key in the back-end database.</span></span>

## <a name="enable-entity-framework"></a><span data-ttu-id="b1f04-141">Enable Entity Framework</span><span class="sxs-lookup"><span data-stu-id="b1f04-141">Enable Entity Framework</span></span>

<span data-ttu-id="b1f04-142">W tym samouczku użyjemy Entity Framework (EF) Code First platformy do utworzenia wewnętrznej bazy danych.</span><span class="sxs-lookup"><span data-stu-id="b1f04-142">For this tutorial, we'll use Entity Framework (EF) Code First to create the back-end database.</span></span>

> [!NOTE]
> <span data-ttu-id="b1f04-143">Web API OData nie wymaga EF.</span><span class="sxs-lookup"><span data-stu-id="b1f04-143">Web API OData does not require EF.</span></span> <span data-ttu-id="b1f04-144">Użyj dowolnej warstwy dostępu do danych, która może dokonywać translacji jednostek bazy danych modeli.</span><span class="sxs-lookup"><span data-stu-id="b1f04-144">Use any data-access layer that can translate database entities into models.</span></span>


<span data-ttu-id="b1f04-145">Najpierw zainstaluj pakiet NuGet dla platformy EF.</span><span class="sxs-lookup"><span data-stu-id="b1f04-145">First, install the NuGet package for EF.</span></span> <span data-ttu-id="b1f04-146">Z **narzędzia** menu, wybierz opcję **Menedżera pakietów NuGet** &gt; **Konsola Menedżera pakietów**.</span><span class="sxs-lookup"><span data-stu-id="b1f04-146">From the **Tools** menu, select **NuGet Package Manager** &gt; **Package Manager Console**.</span></span> <span data-ttu-id="b1f04-147">W oknie Konsola Menedżera pakietów wpisz polecenie:</span><span class="sxs-lookup"><span data-stu-id="b1f04-147">In the Package Manager Console window, type:</span></span>

[!code-console[Main](create-an-odata-v4-endpoint/samples/sample3.cmd)]

<span data-ttu-id="b1f04-148">Otwórz plik Web.config i dodaj następującą sekcję wewnątrz **konfiguracji** elementu po **configSections** elementu.</span><span class="sxs-lookup"><span data-stu-id="b1f04-148">Open the Web.config file, and add the following section inside the **configuration** element, after the **configSections** element.</span></span>

[!code-xml[Main](create-an-odata-v4-endpoint/samples/sample4.xml?highlight=6)]

<span data-ttu-id="b1f04-149">To ustawienie dodaje parametry połączenia dla bazy danych LocalDB.</span><span class="sxs-lookup"><span data-stu-id="b1f04-149">This setting adds a connection string for a LocalDB database.</span></span> <span data-ttu-id="b1f04-150">Ta baza danych będzie służyć lokalnie uruchomić aplikację.</span><span class="sxs-lookup"><span data-stu-id="b1f04-150">This database will be used when you run the app locally.</span></span>

<span data-ttu-id="b1f04-151">Następnie Dodaj klasę o nazwie `ProductsContext` do folderu modeli:</span><span class="sxs-lookup"><span data-stu-id="b1f04-151">Next, add a class named `ProductsContext` to the Models folder:</span></span>

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample5.cs)]

<span data-ttu-id="b1f04-152">W Konstruktorze `"name=ProductsContext"` nadaje nazwę parametrów połączenia.</span><span class="sxs-lookup"><span data-stu-id="b1f04-152">In the constructor, `"name=ProductsContext"` gives the name of the connection string.</span></span>

## <a name="configure-the-odata-endpoint"></a><span data-ttu-id="b1f04-153">Skonfiguruj punkt końcowy OData</span><span class="sxs-lookup"><span data-stu-id="b1f04-153">Configure the OData endpoint</span></span>

<span data-ttu-id="b1f04-154">Otwórz plik App\_Start/WebApiConfig.cs.</span><span class="sxs-lookup"><span data-stu-id="b1f04-154">Open the file App\_Start/WebApiConfig.cs.</span></span> <span data-ttu-id="b1f04-155">Dodaj następujący kod **przy użyciu** instrukcji:</span><span class="sxs-lookup"><span data-stu-id="b1f04-155">Add the following **using** statements:</span></span>

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample6.cs)]

<span data-ttu-id="b1f04-156">Następnie dodaj następujący kod do **zarejestrować** metody:</span><span class="sxs-lookup"><span data-stu-id="b1f04-156">Then add the following code to the **Register** method:</span></span>

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample7.cs)]

<span data-ttu-id="b1f04-157">Ten kod wykonuje dwie czynności:</span><span class="sxs-lookup"><span data-stu-id="b1f04-157">This code does two things:</span></span>

- <span data-ttu-id="b1f04-158">Tworzy model Entity Data Model (EDM struktury).</span><span class="sxs-lookup"><span data-stu-id="b1f04-158">Creates an Entity Data Model (EDM).</span></span>
- <span data-ttu-id="b1f04-159">Dodaje trasę.</span><span class="sxs-lookup"><span data-stu-id="b1f04-159">Adds a route.</span></span>

<span data-ttu-id="b1f04-160">EDM jest abstrakcyjny model danych.</span><span class="sxs-lookup"><span data-stu-id="b1f04-160">An EDM is an abstract model of the data.</span></span> <span data-ttu-id="b1f04-161">EDM jest używany do utworzenia dokumentu metadanych usługi.</span><span class="sxs-lookup"><span data-stu-id="b1f04-161">The EDM is used to create the service metadata document.</span></span> <span data-ttu-id="b1f04-162">**ODataConventionModelBuilder** klasy tworzy EDM przy użyciu domyślnych konwencji nazewnictwa.</span><span class="sxs-lookup"><span data-stu-id="b1f04-162">The **ODataConventionModelBuilder** class creates an EDM by using default naming conventions.</span></span> <span data-ttu-id="b1f04-163">Takie podejście wymaga co najmniej kodu.</span><span class="sxs-lookup"><span data-stu-id="b1f04-163">This approach requires the least code.</span></span> <span data-ttu-id="b1f04-164">Jeśli chcesz, aby mieć większą kontrolę nad EDM, możesz użyć **element ODataModelBuilder** klasy w celu utworzenia EDM, jawnie dodając właściwości, kluczy i właściwości nawigacji.</span><span class="sxs-lookup"><span data-stu-id="b1f04-164">If you want more control over the EDM, you can use the **ODataModelBuilder** class to create the EDM by adding properties, keys, and navigation properties explicitly.</span></span>

<span data-ttu-id="b1f04-165">A *trasy* zawiera informacje dotyczące określenia trasy żądań HTTP do punktu końcowego interfejsu API sieci Web.</span><span class="sxs-lookup"><span data-stu-id="b1f04-165">A *route* tells Web API how to route HTTP requests to the endpoint.</span></span> <span data-ttu-id="b1f04-166">Aby utworzyć trasę OData v4, wywołaj **MapODataServiceRoute** — metoda rozszerzenia.</span><span class="sxs-lookup"><span data-stu-id="b1f04-166">To create an OData v4 route, call the **MapODataServiceRoute** extension method.</span></span>

<span data-ttu-id="b1f04-167">Jeśli aplikacja ma wiele punktów końcowych OData, Utwórz oddzielne trasy dla każdego.</span><span class="sxs-lookup"><span data-stu-id="b1f04-167">If your application has multiple OData endpoints, create a separate route for each.</span></span> <span data-ttu-id="b1f04-168">Nadaj każdej trasy trasy unikatową nazwę i prefiksu.</span><span class="sxs-lookup"><span data-stu-id="b1f04-168">Give each route a unique route name and prefix.</span></span>

## <a name="add-the-odata-controller"></a><span data-ttu-id="b1f04-169">Dodawanie kontrolera OData</span><span class="sxs-lookup"><span data-stu-id="b1f04-169">Add the OData controller</span></span>

<span data-ttu-id="b1f04-170">A *kontrolera* to klasa, która obsługuje żądania HTTP.</span><span class="sxs-lookup"><span data-stu-id="b1f04-170">A *controller* is a class that handles HTTP requests.</span></span> <span data-ttu-id="b1f04-171">Możesz utworzyć oddzielne kontrolera dla każdej jednostki w usługi OData.</span><span class="sxs-lookup"><span data-stu-id="b1f04-171">You create a separate controller for each entity set in your OData service.</span></span> <span data-ttu-id="b1f04-172">W tym samouczku utworzysz jednego kontrolera dla `Product` jednostki.</span><span class="sxs-lookup"><span data-stu-id="b1f04-172">In this tutorial, you will create one controller, for the `Product` entity.</span></span>

<span data-ttu-id="b1f04-173">W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy folder kontrolerów, a następnie wybierz **Dodaj** &gt; **klasy**.</span><span class="sxs-lookup"><span data-stu-id="b1f04-173">In Solution Explorer, right-click the Controllers folder and select **Add** &gt; **Class**.</span></span> <span data-ttu-id="b1f04-174">Nazwa klasy `ProductsController`.</span><span class="sxs-lookup"><span data-stu-id="b1f04-174">Name the class `ProductsController`.</span></span>

> [!NOTE]
> <span data-ttu-id="b1f04-175">Wersja tego samouczka dla protokołu OData v3 używa **Dodaj kontroler** tworzenia szkieletów.</span><span class="sxs-lookup"><span data-stu-id="b1f04-175">The version of this tutorial for OData v3 uses the **Add Controller** scaffolding.</span></span> <span data-ttu-id="b1f04-176">Obecnie nie ma żadnych funkcją szkieletów dla protokołu OData v4.</span><span class="sxs-lookup"><span data-stu-id="b1f04-176">Currently, there is no scaffolding for OData v4.</span></span>


<span data-ttu-id="b1f04-177">Zamień schematyczny kod w ProductsController.cs poniżej.</span><span class="sxs-lookup"><span data-stu-id="b1f04-177">Replace the boilerplate code in ProductsController.cs with the following.</span></span>

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample8.cs)]

<span data-ttu-id="b1f04-178">Zastosowań kontrolera `ProductsContext` klasy dostępu do bazy danych przy użyciu programu EF.</span><span class="sxs-lookup"><span data-stu-id="b1f04-178">The controller uses the `ProductsContext` class to access the database using EF.</span></span> <span data-ttu-id="b1f04-179">Należy zauważyć, że kontroler zastępuje **Dispose** metodę, aby usunąć **ProductsContext**.</span><span class="sxs-lookup"><span data-stu-id="b1f04-179">Notice that the controller overrides the **Dispose** method to dispose of the **ProductsContext**.</span></span>

<span data-ttu-id="b1f04-180">Jest to punkt początkowy dla kontrolera.</span><span class="sxs-lookup"><span data-stu-id="b1f04-180">This is the starting point for the controller.</span></span> <span data-ttu-id="b1f04-181">Następnie dodamy metody dla wszystkich operacji CRUD.</span><span class="sxs-lookup"><span data-stu-id="b1f04-181">Next, we'll add methods for all of the CRUD operations.</span></span>

## <a name="query-the-entity-set"></a><span data-ttu-id="b1f04-182">Zapytanie zestawu jednostek</span><span class="sxs-lookup"><span data-stu-id="b1f04-182">Query the entity set</span></span>

<span data-ttu-id="b1f04-183">Dodaj następujące metody umożliwiające `ProductsController`.</span><span class="sxs-lookup"><span data-stu-id="b1f04-183">Add the following methods to `ProductsController`.</span></span>

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample9.cs)]

<span data-ttu-id="b1f04-184">Bez parametrów wersję `Get` metoda zwraca całą kolekcję produktów.</span><span class="sxs-lookup"><span data-stu-id="b1f04-184">The parameterless version of the `Get` method returns the entire Products collection.</span></span> <span data-ttu-id="b1f04-185">`Get` Metody z *klucz* parametru wyszukuje produktu według jego klucza (w tym przypadku `Id` właściwości).</span><span class="sxs-lookup"><span data-stu-id="b1f04-185">The `Get` method with a *key* parameter looks up a product by its key (in this case, the `Id` property).</span></span>

<span data-ttu-id="b1f04-186">**[EnableQuery]** atrybut umożliwia klientom zmodyfikować zapytanie, za pomocą opcji zapytania $filter, $sort i $page.</span><span class="sxs-lookup"><span data-stu-id="b1f04-186">The **[EnableQuery]** attribute enables clients to modify the query, by using query options such as $filter, $sort, and $page.</span></span> <span data-ttu-id="b1f04-187">Aby uzyskać więcej informacji, zobacz [obsługi opcji zapytania OData](../supporting-odata-query-options.md).</span><span class="sxs-lookup"><span data-stu-id="b1f04-187">For more information, see [Supporting OData Query Options](../supporting-odata-query-options.md).</span></span>

## <a name="add-an-entity-to-the-entity-set"></a><span data-ttu-id="b1f04-188">Dodawanie jednostki do zestawu jednostek</span><span class="sxs-lookup"><span data-stu-id="b1f04-188">Add an entity to the entity set</span></span>

<span data-ttu-id="b1f04-189">Aby umożliwić klientom dodać nowy produkt do bazy danych, dodaj następującą metodę do `ProductsController`.</span><span class="sxs-lookup"><span data-stu-id="b1f04-189">To enable clients to add a new product to the database, add the following method to `ProductsController`.</span></span>

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample10.cs)]

## <a name="update-an-entity"></a><span data-ttu-id="b1f04-190">Aktualizuj jednostkę</span><span class="sxs-lookup"><span data-stu-id="b1f04-190">Update an entity</span></span>

<span data-ttu-id="b1f04-191">OData obsługuje dwie semantykę różną w przypadku aktualizowania jednostki, poprawki i PUT.</span><span class="sxs-lookup"><span data-stu-id="b1f04-191">OData supports two different semantics for updating an entity, PATCH and PUT.</span></span>

- <span data-ttu-id="b1f04-192">POPRAWKA wykonuje częściową aktualizację.</span><span class="sxs-lookup"><span data-stu-id="b1f04-192">PATCH performs a partial update.</span></span> <span data-ttu-id="b1f04-193">Klient określa tylko właściwości do zaktualizowania.</span><span class="sxs-lookup"><span data-stu-id="b1f04-193">The client specifies just the properties to update.</span></span>
- <span data-ttu-id="b1f04-194">Umieść zastępuje całej jednostki.</span><span class="sxs-lookup"><span data-stu-id="b1f04-194">PUT replaces the entire entity.</span></span>

<span data-ttu-id="b1f04-195">Wadą PUT jest, klient musi wysłać wartości dla wszystkich właściwości w obiekcie, łącznie z wartościami, które nie zmieniają się.</span><span class="sxs-lookup"><span data-stu-id="b1f04-195">The disadvantage of PUT is that the client must send values for all of the properties in the entity, including values that are not changing.</span></span> <span data-ttu-id="b1f04-196">[Specyfikacji protokołu OData](http://docs.oasis-open.org/odata/odata/v4.0/os/part1-protocol/odata-v4.0-os-part1-protocol.html#_Toc372793719) stwierdzający, że poprawka jest preferowana.</span><span class="sxs-lookup"><span data-stu-id="b1f04-196">The [OData spec](http://docs.oasis-open.org/odata/odata/v4.0/os/part1-protocol/odata-v4.0-os-part1-protocol.html#_Toc372793719) states that PATCH is preferred.</span></span>

<span data-ttu-id="b1f04-197">W każdym przypadku poniżej przedstawiono kod dla metody PUT i PATCH:</span><span class="sxs-lookup"><span data-stu-id="b1f04-197">In any case, here is the code for both PATCH and PUT methods:</span></span>

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample11.cs)]

<span data-ttu-id="b1f04-198">W przypadku poprawek, używa kontrolera **Delta&lt;T&gt;**  typ do śledzenia zmian.</span><span class="sxs-lookup"><span data-stu-id="b1f04-198">In the case of PATCH, the controller uses the **Delta&lt;T&gt;** type to track the changes.</span></span>

## <a name="delete-an-entity"></a><span data-ttu-id="b1f04-199">Usunięcie jednostki</span><span class="sxs-lookup"><span data-stu-id="b1f04-199">Delete an entity</span></span>

<span data-ttu-id="b1f04-200">Aby umożliwić klientom usunąć produkt z bazy danych, dodaj następującą metodę do `ProductsController`.</span><span class="sxs-lookup"><span data-stu-id="b1f04-200">To enable clients to delete a product from the database, add the following method to `ProductsController`.</span></span>

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample12.cs)]
