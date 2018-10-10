---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v3/creating-an-odata-endpoint
title: Tworzenie punktu końcowego OData v3 z interfejsu Web API 2 | Dokumentacja firmy Microsoft
author: MikeWasson
description: Open Data Protocol (OData) to protokół dostępu do danych w sieci Web. Protokół OData zapewnia ujednolicone struktury danych, wykonywania zapytań o dane i manipulowania danymi...
ms.author: riande
ms.date: 02/25/2014
ms.assetid: 262843d6-43a2-4f1c-82d9-0b90ae6df0cf
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v3/creating-an-odata-endpoint
msc.type: authoredcontent
ms.openlocfilehash: 2e0d3b45fd51192d227d852dc2f05b45ca42944c
ms.sourcegitcommit: a4dcca4f1cb81227c5ed3c92dc0e28be6e99447b
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/10/2018
ms.locfileid: "48910918"
---
<a name="creating-an-odata-v3-endpoint-with-web-api-2"></a><span data-ttu-id="7d377-104">Tworzenie punktu końcowego OData v3 z interfejsu Web API 2</span><span class="sxs-lookup"><span data-stu-id="7d377-104">Creating an OData v3 Endpoint with Web API 2</span></span>
====================
<span data-ttu-id="7d377-105">przez [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="7d377-105">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="7d377-106">Pobierz ukończony projekt</span><span class="sxs-lookup"><span data-stu-id="7d377-106">Download Completed Project</span></span>](http://code.msdn.microsoft.com/ASPNET-Web-API-OData-cecdb524)

> <span data-ttu-id="7d377-107">[Open Data Protocol](http://www.odata.org/) (OData) to protokół dostępu do danych w sieci Web.</span><span class="sxs-lookup"><span data-stu-id="7d377-107">The [Open Data Protocol](http://www.odata.org/) (OData) is a data access protocol for the web.</span></span> <span data-ttu-id="7d377-108">Protokół OData zapewnia ujednolicone struktury danych, wykonywania zapytań o dane i manipulowania zestawu danych przy użyciu operacji CRUD (tworzenia, odczytu, aktualizacji i usuwania).</span><span class="sxs-lookup"><span data-stu-id="7d377-108">OData provides a uniform way to structure data, query the data, and manipulate the data set through CRUD operations (create, read, update, and delete).</span></span> <span data-ttu-id="7d377-109">OData obsługuje formaty JSON i AtomPub (XML).</span><span class="sxs-lookup"><span data-stu-id="7d377-109">OData supports both AtomPub (XML) and JSON formats.</span></span> <span data-ttu-id="7d377-110">Usługa OData definiuje również sposób do udostępnienia metadanych dotyczących danych.</span><span class="sxs-lookup"><span data-stu-id="7d377-110">OData also defines a way to expose metadata about the data.</span></span> <span data-ttu-id="7d377-111">Klienci mogą używać metadane, aby dowiedzieć się, informacje o typie i relacje dla zestawu danych.</span><span class="sxs-lookup"><span data-stu-id="7d377-111">Clients can use the metadata to discover the type information and relationships for the data set.</span></span>
>
> <span data-ttu-id="7d377-112">ASP.NET Web API ułatwia utworzenie punktu końcowego OData dla zestawu danych.</span><span class="sxs-lookup"><span data-stu-id="7d377-112">ASP.NET Web API makes it easy to create an OData endpoint for a data set.</span></span> <span data-ttu-id="7d377-113">Można kontrolować, obsługuje punkt końcowy dokładnie operacje OData.</span><span class="sxs-lookup"><span data-stu-id="7d377-113">You can control exactly which OData operations the endpoint supports.</span></span> <span data-ttu-id="7d377-114">Może obsługiwać wiele punktów końcowych OData, wraz z punktów końcowych bez OData.</span><span class="sxs-lookup"><span data-stu-id="7d377-114">You can host multiple OData endpoints, alongside non-OData endpoints.</span></span> <span data-ttu-id="7d377-115">Masz pełną kontrolę nad modelu danych, logika biznesowa zaplecza i warstwy danych.</span><span class="sxs-lookup"><span data-stu-id="7d377-115">You have full control over your data model, back-end business logic, and data layer.</span></span>
>
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="7d377-116">Wersje oprogramowania używanego w tym samouczku</span><span class="sxs-lookup"><span data-stu-id="7d377-116">Software versions used in the tutorial</span></span>
>
>
> - [<span data-ttu-id="7d377-117">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="7d377-117">Visual Studio 2013</span></span>](https://my.visualstudio.com/Downloads?q=visual%20studio%202013)
> - <span data-ttu-id="7d377-118">Internetowy interfejs API 2</span><span class="sxs-lookup"><span data-stu-id="7d377-118">Web API 2</span></span>
> - <span data-ttu-id="7d377-119">OData w wersji 3</span><span class="sxs-lookup"><span data-stu-id="7d377-119">OData Version 3</span></span>
> - <span data-ttu-id="7d377-120">Entity Framework 6</span><span class="sxs-lookup"><span data-stu-id="7d377-120">Entity Framework 6</span></span>
> - [<span data-ttu-id="7d377-121">Debugowanie serwera Proxy (opcjonalnie) w sieci Web programu fiddler</span><span class="sxs-lookup"><span data-stu-id="7d377-121">Fiddler Web Debugging Proxy (Optional)</span></span>](http://www.fiddler2.com)
>
> <span data-ttu-id="7d377-122">Obsługa protokołu OData interfejsu API sieci Web został dodany w [platformy ASP.NET i Web Tools 2012.2 Update](https://go.microsoft.com/fwlink/?LinkId=282650).</span><span class="sxs-lookup"><span data-stu-id="7d377-122">Web API OData support was added in [ASP.NET and Web Tools 2012.2 Update](https://go.microsoft.com/fwlink/?LinkId=282650).</span></span> <span data-ttu-id="7d377-123">Jednak ten samouczek używa tworzenia szkieletu, który został dodany w programie Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="7d377-123">However, this tutorial uses scaffolding that was added in Visual Studio 2013.</span></span>


<span data-ttu-id="7d377-124">W tym samouczku utworzysz prosty punkt końcowy OData, które klienci mogą wysyłać zapytania.</span><span class="sxs-lookup"><span data-stu-id="7d377-124">In this tutorial, you will create a simple OData endpoint that clients can query.</span></span> <span data-ttu-id="7d377-125">Ponadto utworzysz klienta języka C# dla punktu końcowego.</span><span class="sxs-lookup"><span data-stu-id="7d377-125">You will also create a C# client for the endpoint.</span></span> <span data-ttu-id="7d377-126">Po ukończeniu tego samouczka kolejny zbiór samouczki pokazują, jak dodać więcej funkcji, w tym relacji jednostek, akcje i wybierz Rozwiń $/ $.</span><span class="sxs-lookup"><span data-stu-id="7d377-126">After you complete this tutorial, the next set of tutorials show how to add more functionality, including entity relations, actions, and $expand/$select.</span></span>

- [<span data-ttu-id="7d377-127">Tworzenie projektu programu Visual Studio</span><span class="sxs-lookup"><span data-stu-id="7d377-127">Create the Visual Studio Project</span></span>](#create-project)
- [<span data-ttu-id="7d377-128">Dodawanie modelu jednostki</span><span class="sxs-lookup"><span data-stu-id="7d377-128">Add an Entity Model</span></span>](#add-model)
- [<span data-ttu-id="7d377-129">Dodawanie kontrolera OData</span><span class="sxs-lookup"><span data-stu-id="7d377-129">Add an OData Controller</span></span>](#add-controller)
- [<span data-ttu-id="7d377-130">Dodaj EDM i trasy</span><span class="sxs-lookup"><span data-stu-id="7d377-130">Add the EDM and Route</span></span>](#edm)
- [<span data-ttu-id="7d377-131">Inicjowanie bazy danych (opcjonalnie)</span><span class="sxs-lookup"><span data-stu-id="7d377-131">Seed the Database (Optional)</span></span>](#seed-db)
- [<span data-ttu-id="7d377-132">Eksplorowanie punkt końcowy OData</span><span class="sxs-lookup"><span data-stu-id="7d377-132">Exploring the OData Endpoint</span></span>](#explore)
- [<span data-ttu-id="7d377-133">Formaty serializacji OData</span><span class="sxs-lookup"><span data-stu-id="7d377-133">OData Serialization Formats</span></span>](#formats)

<a id="create-project"></a>
## <a name="create-the-visual-studio-project"></a><span data-ttu-id="7d377-134">Tworzenie projektu programu Visual Studio</span><span class="sxs-lookup"><span data-stu-id="7d377-134">Create the Visual Studio Project</span></span>

<span data-ttu-id="7d377-135">W tym samouczku utworzysz punkt końcowy OData, który obsługuje podstawowe operacje CRUD.</span><span class="sxs-lookup"><span data-stu-id="7d377-135">In this tutorial, you will create an OData endpoint that supports basic CRUD operations.</span></span> <span data-ttu-id="7d377-136">Punkt końcowy udostępni pojedynczy zasób listę produktów.</span><span class="sxs-lookup"><span data-stu-id="7d377-136">The endpoint will expose a single resource, a list of products.</span></span> <span data-ttu-id="7d377-137">Spowoduje to dodanie kolejnych samouczkach więcej funkcji.</span><span class="sxs-lookup"><span data-stu-id="7d377-137">Later tutorials will add more features.</span></span>

<span data-ttu-id="7d377-138">Uruchom program Visual Studio i wybierz **nowy projekt** ze strony początkowej.</span><span class="sxs-lookup"><span data-stu-id="7d377-138">Start Visual Studio and select **New Project** from the Start page.</span></span> <span data-ttu-id="7d377-139">Możesz również z menu **Plik** wybrać pozycję **Nowy**, a następnie **Projekt**.</span><span class="sxs-lookup"><span data-stu-id="7d377-139">Or, from the **File** menu, select **New** and then **Project**.</span></span>

<span data-ttu-id="7d377-140">W **szablony** okienku wybierz **zainstalowane szablony** i rozwiń węzeł Visual C#.</span><span class="sxs-lookup"><span data-stu-id="7d377-140">In the **Templates** pane, select **Installed Templates** and expand the Visual C# node.</span></span> <span data-ttu-id="7d377-141">W obszarze **Visual C#**, wybierz pozycję **Sieć Web**.</span><span class="sxs-lookup"><span data-stu-id="7d377-141">Under **Visual C#**, select **Web**.</span></span> <span data-ttu-id="7d377-142">Wybierz **aplikacji sieci Web ASP.NET** szablonu.</span><span class="sxs-lookup"><span data-stu-id="7d377-142">Select **the ASP.NET Web Application** template.</span></span>

![](creating-an-odata-endpoint/_static/image1.png)

<span data-ttu-id="7d377-143">W oknie dialogowym **Nowy projekt ASP.NET** wybierz szablon **Pusty**.</span><span class="sxs-lookup"><span data-stu-id="7d377-143">In the **New ASP.NET Project** dialog, select the **Empty** template.</span></span> <span data-ttu-id="7d377-144">W obszarze &quot;. Dodaj foldery i podstawowe odwołania dla... &quot;, sprawdź **interfejsu API sieci Web**.</span><span class="sxs-lookup"><span data-stu-id="7d377-144">Under &quot;Add folders and core references for...&quot;, check **Web API**.</span></span> <span data-ttu-id="7d377-145">Kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="7d377-145">Click **OK**.</span></span>

![](creating-an-odata-endpoint/_static/image2.png)

<a id="add-model"></a>
## <a name="add-an-entity-model"></a><span data-ttu-id="7d377-146">Dodawanie modelu jednostki</span><span class="sxs-lookup"><span data-stu-id="7d377-146">Add an Entity Model</span></span>

<span data-ttu-id="7d377-147">*Model* jest obiektem, który reprezentuje dane w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="7d377-147">A *model* is an object that represents the data in your application.</span></span> <span data-ttu-id="7d377-148">W tym samouczku potrzebujemy modelu, który reprezentuje produkt.</span><span class="sxs-lookup"><span data-stu-id="7d377-148">For this tutorial, we need a model that represents a product.</span></span> <span data-ttu-id="7d377-149">Model odnosi się do naszych OData typu jednostki.</span><span class="sxs-lookup"><span data-stu-id="7d377-149">The model corresponds to our OData entity type.</span></span>

<span data-ttu-id="7d377-150">W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy folder Modele.</span><span class="sxs-lookup"><span data-stu-id="7d377-150">In Solution Explorer, right-click the Models folder.</span></span> <span data-ttu-id="7d377-151">Z menu kontekstowego wybierz pozycję **Dodaj**, a następnie **Klasa**.</span><span class="sxs-lookup"><span data-stu-id="7d377-151">From the context menu, select **Add** then select **Class**.</span></span>

![](creating-an-odata-endpoint/_static/image3.png)

<span data-ttu-id="7d377-152">W **Dodaj nowe** elementu okno dialogowe, określ nazwę klasy &quot;produktu&quot;.</span><span class="sxs-lookup"><span data-stu-id="7d377-152">In the **Add New** Item dialog, name the class &quot;Product&quot;.</span></span>

![](creating-an-odata-endpoint/_static/image4.png)

> [!NOTE]
> <span data-ttu-id="7d377-153">Zgodnie z Konwencją klasy modelu są umieszczane w folderze modeli.</span><span class="sxs-lookup"><span data-stu-id="7d377-153">By convention, model classes are placed in the Models folder.</span></span> <span data-ttu-id="7d377-154">Nie musisz przestrzegać tej Konwencji we własnych projektach, ale będziemy z niego korzystać na potrzeby tego samouczka.</span><span class="sxs-lookup"><span data-stu-id="7d377-154">You don't have to follow this convention in your own projects, but we'll use it for this tutorial.</span></span>


<span data-ttu-id="7d377-155">W pliku Product.cs Dodaj następującą definicję klasy:</span><span class="sxs-lookup"><span data-stu-id="7d377-155">In the Product.cs file, add the following class definition:</span></span>

[!code-csharp[Main](creating-an-odata-endpoint/samples/sample1.cs)]

<span data-ttu-id="7d377-156">Właściwość ID będzie klucza jednostki.</span><span class="sxs-lookup"><span data-stu-id="7d377-156">The ID property will be the entity key.</span></span> <span data-ttu-id="7d377-157">Klienci mogą wysyłać zapytania produkty według identyfikatora.</span><span class="sxs-lookup"><span data-stu-id="7d377-157">Clients can query products by ID.</span></span> <span data-ttu-id="7d377-158">To pole będzie także klucz podstawowy w wewnętrznej bazy danych.</span><span class="sxs-lookup"><span data-stu-id="7d377-158">This field would also be the primary key in the back-end database.</span></span>

<span data-ttu-id="7d377-159">Skompiluj teraz projekt.</span><span class="sxs-lookup"><span data-stu-id="7d377-159">Build the project now.</span></span> <span data-ttu-id="7d377-160">W następnym kroku użyjemy niektóre szkieletu programu Visual Studio, który używa odbicia w celu odnalezienia typu produktu.</span><span class="sxs-lookup"><span data-stu-id="7d377-160">In the next step, we'll use some Visual Studio scaffolding that uses reflection to find the Product type.</span></span>

<a id="add-controller"></a>
## <a name="add-an-odata-controller"></a><span data-ttu-id="7d377-161">Dodawanie kontrolera OData</span><span class="sxs-lookup"><span data-stu-id="7d377-161">Add an OData Controller</span></span>

<span data-ttu-id="7d377-162">A *kontrolera* to klasa, która obsługuje żądania HTTP.</span><span class="sxs-lookup"><span data-stu-id="7d377-162">A *controller* is a class that handles HTTP requests.</span></span> <span data-ttu-id="7d377-163">Należy zdefiniować kontrolera osobne dla każdego zestawu jednostek w przypadku usługi OData.</span><span class="sxs-lookup"><span data-stu-id="7d377-163">You define a separate controller for each entity set in you OData service.</span></span> <span data-ttu-id="7d377-164">W tym samouczku utworzymy jednego kontrolera.</span><span class="sxs-lookup"><span data-stu-id="7d377-164">In this tutorial, we'll create a single controller.</span></span>

<span data-ttu-id="7d377-165">W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy folder kontrolerów.</span><span class="sxs-lookup"><span data-stu-id="7d377-165">In Solution Explorer, right-click the Controllers folder.</span></span> <span data-ttu-id="7d377-166">Wybierz pozycję **Dodaj**, a następnie **Kontroler**.</span><span class="sxs-lookup"><span data-stu-id="7d377-166">Select **Add** and then select **Controller**.</span></span>

![](creating-an-odata-endpoint/_static/image5.png)

<span data-ttu-id="7d377-167">W **Dodawanie szkieletu** okno dialogowe, wybierz opcję &quot;kontroler internetowego interfejsu API 2 OData z akcjami używający narzędzia Entity Framework&quot;.</span><span class="sxs-lookup"><span data-stu-id="7d377-167">In the **Add Scaffold** dialog, select &quot;Web API 2 OData Controller with actions, using Entity Framework&quot;.</span></span>

![](creating-an-odata-endpoint/_static/image6.png)

<span data-ttu-id="7d377-168">W **Dodaj kontroler** okno dialogowe, nazwy kontrolera "ProductsController".</span><span class="sxs-lookup"><span data-stu-id="7d377-168">In the **Add Controller** dialog, name the controller "ProductsController".</span></span> <span data-ttu-id="7d377-169">Wybierz &quot;używać asynchronicznych akcji kontrolera&quot; pola wyboru.</span><span class="sxs-lookup"><span data-stu-id="7d377-169">Select the &quot;Use async controller actions&quot; checkbox.</span></span> <span data-ttu-id="7d377-170">W **modelu** listy rozwijanej wybierz klasy produktu.</span><span class="sxs-lookup"><span data-stu-id="7d377-170">In the **Model** drop-down list, select the Product class.</span></span>

![](creating-an-odata-endpoint/_static/image7.png)

<span data-ttu-id="7d377-171">Kliknij przycisk **nowy kontekst danych...**  przycisku.</span><span class="sxs-lookup"><span data-stu-id="7d377-171">Click the **New data context...** button.</span></span> <span data-ttu-id="7d377-172">Pozostaw nazwę domyślną dla typu kontekstu danych, a następnie kliknij przycisk **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="7d377-172">Leave the default name for the data context type, and click **Add**.</span></span>

![](creating-an-odata-endpoint/_static/image8.png)

<span data-ttu-id="7d377-173">W oknie dialogowym Dodaj kontroler, aby dodać kontroler, kliknij przycisk Dodaj.</span><span class="sxs-lookup"><span data-stu-id="7d377-173">Click Add in the Add Controller dialog to add the controller.</span></span>

![](creating-an-odata-endpoint/_static/image9.png)

<span data-ttu-id="7d377-174">Uwaga: Jeśli otrzymasz komunikat o błędzie z tekstem &quot;wystąpił błąd podczas pobierania typu... &quot;, upewnij się, utworzony projekt programu Visual Studio, po dodaniu klasy produktu.</span><span class="sxs-lookup"><span data-stu-id="7d377-174">Note: If you get an error message that says &quot;There was an error getting the type...&quot;, make sure that you built the Visual Studio project after you added the Product class.</span></span> <span data-ttu-id="7d377-175">Szkieletu używa odbicia w celu wyszukania klasy.</span><span class="sxs-lookup"><span data-stu-id="7d377-175">The scaffolding uses reflection to find the class.</span></span>

![](creating-an-odata-endpoint/_static/image10.png)

<span data-ttu-id="7d377-176">Szkieletu dodaje dwa pliki kodu do projektu:</span><span class="sxs-lookup"><span data-stu-id="7d377-176">The scaffolding adds two code files to the project:</span></span>

- <span data-ttu-id="7d377-177">Products.cs definiuje kontrolera interfejsu API sieci Web, który implementuje punkt końcowy OData.</span><span class="sxs-lookup"><span data-stu-id="7d377-177">Products.cs defines the Web API controller that implements the OData endpoint.</span></span>
- <span data-ttu-id="7d377-178">ProductServiceContext.cs udostępnia metody do wykonywania zapytań w źródłowej bazie danych przy użyciu platformy Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="7d377-178">ProductServiceContext.cs provides methods to query the underlying database, using Entity Framework.</span></span>

![](creating-an-odata-endpoint/_static/image11.png)

<a id="edm"></a>
## <a name="add-the-edm-and-route"></a><span data-ttu-id="7d377-179">Dodaj EDM i trasy</span><span class="sxs-lookup"><span data-stu-id="7d377-179">Add the EDM and Route</span></span>

<span data-ttu-id="7d377-180">W Eksploratorze rozwiązań rozwiń aplikacji\_Uruchom folder i Otwórz plik o nazwie WebApiConfig.cs.</span><span class="sxs-lookup"><span data-stu-id="7d377-180">In Solution Explorer, expand the App\_Start folder and open the file named WebApiConfig.cs.</span></span> <span data-ttu-id="7d377-181">Ta klasa przechowuje kod konfiguracji interfejsu API sieci Web.</span><span class="sxs-lookup"><span data-stu-id="7d377-181">This class holds configuration code for Web API.</span></span> <span data-ttu-id="7d377-182">Zastąp ten kod następujących czynności:</span><span class="sxs-lookup"><span data-stu-id="7d377-182">Replace this code with the following:</span></span>

[!code-csharp[Main](creating-an-odata-endpoint/samples/sample2.cs)]

<span data-ttu-id="7d377-183">Ten kod wykonuje dwie czynności:</span><span class="sxs-lookup"><span data-stu-id="7d377-183">This code does two things:</span></span>

- <span data-ttu-id="7d377-184">Tworzy Entity Data Model (EDM) dla punktu końcowego OData.</span><span class="sxs-lookup"><span data-stu-id="7d377-184">Creates an Entity Data Model (EDM) for the OData endpoint.</span></span>
- <span data-ttu-id="7d377-185">Dodaje trasę dla punktu końcowego.</span><span class="sxs-lookup"><span data-stu-id="7d377-185">Adds a route for the endpoint.</span></span>

<span data-ttu-id="7d377-186">EDM jest abstrakcyjny model danych.</span><span class="sxs-lookup"><span data-stu-id="7d377-186">An EDM is an abstract model of the data.</span></span> <span data-ttu-id="7d377-187">EDM jest używany do tworzenia dokumentów metadanych i definiowania identyfikatorów URI usługi.</span><span class="sxs-lookup"><span data-stu-id="7d377-187">The EDM is used to create the metadata document and define the URIs for the service.</span></span> <span data-ttu-id="7d377-188">**ODataConventionModelBuilder** tworzy EDM przy użyciu zestaw domyślnych konwencji nazewnictwa EDM.</span><span class="sxs-lookup"><span data-stu-id="7d377-188">The **ODataConventionModelBuilder** creates an EDM by using a set of default naming conventions EDM.</span></span> <span data-ttu-id="7d377-189">Takie podejście wymaga co najmniej kodu.</span><span class="sxs-lookup"><span data-stu-id="7d377-189">This approach requires the least code.</span></span> <span data-ttu-id="7d377-190">Jeśli chcesz, aby mieć większą kontrolę nad EDM, możesz użyć **element ODataModelBuilder** klasy w celu utworzenia EDM, jawnie dodając właściwości, kluczy i właściwości nawigacji.</span><span class="sxs-lookup"><span data-stu-id="7d377-190">If you want more control over the EDM, you can use the **ODataModelBuilder** class to create the EDM by adding properties, keys, and navigation properties explicitly.</span></span>

<span data-ttu-id="7d377-191">**EntitySet** metoda dodaje wartość EDM jednostki:</span><span class="sxs-lookup"><span data-stu-id="7d377-191">The **EntitySet** method adds an entity set to the EDM:</span></span>

[!code-csharp[Main](creating-an-odata-endpoint/samples/sample3.cs)]

<span data-ttu-id="7d377-192">Ciąg "Produkty" definiuje nazwę zestawu jednostek.</span><span class="sxs-lookup"><span data-stu-id="7d377-192">The string "Products" defines the name of the entity set.</span></span> <span data-ttu-id="7d377-193">Nazwa kontrolera musi odpowiadać Nazwa zestawu jednostek.</span><span class="sxs-lookup"><span data-stu-id="7d377-193">The name of the controller must match the name of the entity set.</span></span> <span data-ttu-id="7d377-194">W tym samouczku zestaw jednostek nosi nazwę "Produkty" i nosi nazwę kontrolera `ProductsController`.</span><span class="sxs-lookup"><span data-stu-id="7d377-194">In this tutorial, the entity set is named "Products" and the controller is named `ProductsController`.</span></span> <span data-ttu-id="7d377-195">Jeśli nosi nazwę "ProductSet" Zestaw jednostek, czy nazwa kontrolera `ProductSetController`.</span><span class="sxs-lookup"><span data-stu-id="7d377-195">If you named the entity set "ProductSet", you would name the controller `ProductSetController`.</span></span> <span data-ttu-id="7d377-196">Należy pamiętać, że punkt końcowy może mieć wiele zestawów jednostek.</span><span class="sxs-lookup"><span data-stu-id="7d377-196">Note that an endpoint can have multiple entity sets.</span></span> <span data-ttu-id="7d377-197">Wywołaj **EntitySet&lt;T&gt;**  dla każdego obiektu zestawu, a następnie zdefiniuj odpowiedniego kontrolera.</span><span class="sxs-lookup"><span data-stu-id="7d377-197">Call **EntitySet&lt;T&gt;** for each entity set, and then define a corresponding controller.</span></span>

<span data-ttu-id="7d377-198">**MapODataRoute** metoda dodaje trasę dla punktu końcowego OData.</span><span class="sxs-lookup"><span data-stu-id="7d377-198">The **MapODataRoute** method adds a route for the OData endpoint.</span></span>

[!code-csharp[Main](creating-an-odata-endpoint/samples/sample4.cs)]

<span data-ttu-id="7d377-199">Pierwszy parametr jest przyjazna nazwa dla danej trasy.</span><span class="sxs-lookup"><span data-stu-id="7d377-199">The first parameter is a friendly name for the route.</span></span> <span data-ttu-id="7d377-200">Klienci usługi nie widzą tę nazwę.</span><span class="sxs-lookup"><span data-stu-id="7d377-200">Clients of your service do not see this name.</span></span> <span data-ttu-id="7d377-201">Drugi parametr jest prefiks identyfikatora URI punktu końcowego.</span><span class="sxs-lookup"><span data-stu-id="7d377-201">The second parameter is the URI prefix for the endpoint.</span></span> <span data-ttu-id="7d377-202">Biorąc pod uwagę ten kod, identyfikator URI dla zestawu jednostek produktów jest http://<em>hostname</em>  /odata/produktów.</span><span class="sxs-lookup"><span data-stu-id="7d377-202">Given this code, the URI for the Products entity set is http://<em>hostname</em>/odata/Products.</span></span> <span data-ttu-id="7d377-203">Aplikacja może mieć więcej niż jeden punkt końcowy OData.</span><span class="sxs-lookup"><span data-stu-id="7d377-203">Your application can have more than one OData endpoint.</span></span> <span data-ttu-id="7d377-204">Dla każdego punktu końcowego, należy wywołać <strong>MapODataRoute</strong> i podaj nazwę unikatową trasę i unikatowy prefiks identyfikatora URI.</span><span class="sxs-lookup"><span data-stu-id="7d377-204">For each endpoint, call <strong>MapODataRoute</strong> and provide a unique route name and a unique URI prefix.</span></span>

<a id="seed-db"></a>
## <a name="seed-the-database-optional"></a><span data-ttu-id="7d377-205">Inicjowanie bazy danych (opcjonalnie)</span><span class="sxs-lookup"><span data-stu-id="7d377-205">Seed the Database (Optional)</span></span>

<span data-ttu-id="7d377-206">W tym kroku użyjesz programu Entity Framework do umieszczenia w bazie danych niektórych testów.</span><span class="sxs-lookup"><span data-stu-id="7d377-206">In this step, you will use Entity Framework to seed the database with some test data.</span></span> <span data-ttu-id="7d377-207">Ten krok jest opcjonalny, ale dzięki temu można natychmiast przetestowania punkt końcowy usługi OData.</span><span class="sxs-lookup"><span data-stu-id="7d377-207">This step is optional, but it lets you test out your OData endpoint right away.</span></span>

<span data-ttu-id="7d377-208">Z **narzędzia** menu, wybierz opcję **Menedżera pakietów NuGet**, a następnie wybierz **Konsola Menedżera pakietów**.</span><span class="sxs-lookup"><span data-stu-id="7d377-208">From the **Tools** menu, select **NuGet Package Manager**, then select **Package Manager Console**.</span></span> <span data-ttu-id="7d377-209">W oknie Konsola Menedżera pakietów wprowadź następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="7d377-209">In the Package Manager Console window, enter the following command:</span></span>

[!code-console[Main](creating-an-odata-endpoint/samples/sample5.cmd)]

<span data-ttu-id="7d377-210">Spowoduje to dodanie folder o nazwie migracje i o nazwie Configuration.cs pliku z kodem.</span><span class="sxs-lookup"><span data-stu-id="7d377-210">This adds a folder named Migrations and a code file named Configuration.cs.</span></span>

![](creating-an-odata-endpoint/_static/image12.png)

<span data-ttu-id="7d377-211">Otwórz ten plik i Dodaj następujący kod do `Configuration.Seed` metody.</span><span class="sxs-lookup"><span data-stu-id="7d377-211">Open this file and add the following code to the `Configuration.Seed` method.</span></span>

[!code-csharp[Main](creating-an-odata-endpoint/samples/sample6.cs)]

<span data-ttu-id="7d377-212">W oknie Konsola Menedżera pakietów wprowadź następujące polecenia:</span><span class="sxs-lookup"><span data-stu-id="7d377-212">In the Package Manager Console Window, enter the following commands:</span></span>

[!code-console[Main](creating-an-odata-endpoint/samples/sample7.cmd)]

<span data-ttu-id="7d377-213">Te polecenia Generuj kod, który tworzy bazę danych, a następnie wykonuje kod.</span><span class="sxs-lookup"><span data-stu-id="7d377-213">These commands generate code that creates the database, and then executes that code.</span></span>

<a id="explore"></a>
## <a name="exploring-the-odata-endpoint"></a><span data-ttu-id="7d377-214">Eksplorowanie punkt końcowy OData</span><span class="sxs-lookup"><span data-stu-id="7d377-214">Exploring the OData Endpoint</span></span>

<span data-ttu-id="7d377-215">W tej sekcji użyjemy [debugowanie serwera Proxy sieci Web programu Fiddler](http://www.fiddler2.com) do wysyłania żądań do punktu końcowego i Sprawdź komunikaty odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="7d377-215">In this section, we'll use the [Fiddler Web Debugging Proxy](http://www.fiddler2.com) to send requests to the endpoint and examine the response messages.</span></span> <span data-ttu-id="7d377-216">To pomoże Ci zapoznać się z funkcjami punktu końcowego OData.</span><span class="sxs-lookup"><span data-stu-id="7d377-216">This will help you to understand the capabilities of an OData endpoint.</span></span>

<span data-ttu-id="7d377-217">W programie Visual Studio naciśnij klawisz F5, aby rozpocząć debugowanie.</span><span class="sxs-lookup"><span data-stu-id="7d377-217">In Visual Studio, press F5 to start debugging.</span></span> <span data-ttu-id="7d377-218">Domyślnie program Visual Studio Otwiera przeglądarkę, aby `http://localhost:*port*`, gdzie *portu* jest numerem portu skonfigurowanego w ustawieniach projektu.</span><span class="sxs-lookup"><span data-stu-id="7d377-218">By default, Visual Studio opens your browser to `http://localhost:*port*`, where *port* is the port number configured in the project settings.</span></span>

<span data-ttu-id="7d377-219">Możesz zmienić numer portu w ustawieniach projektu.</span><span class="sxs-lookup"><span data-stu-id="7d377-219">You can change the port number in the project settings.</span></span> <span data-ttu-id="7d377-220">W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy projekt i wybierz **właściwości**.</span><span class="sxs-lookup"><span data-stu-id="7d377-220">In Solution Explorer, right-click the project and select **Properties**.</span></span> <span data-ttu-id="7d377-221">W oknie dialogowym właściwości wybierz **Web**.</span><span class="sxs-lookup"><span data-stu-id="7d377-221">In the properties window, select **Web**.</span></span> <span data-ttu-id="7d377-222">Wprowadź numer portu w ramach **adres Url projektu**.</span><span class="sxs-lookup"><span data-stu-id="7d377-222">Enter the port number under **Project Url**.</span></span>

### <a name="service-document"></a><span data-ttu-id="7d377-223">Dokument usługi</span><span class="sxs-lookup"><span data-stu-id="7d377-223">Service Document</span></span>

<span data-ttu-id="7d377-224">*Dokument usługi* znajduje się lista zestawów encji dla punktu końcowego OData.</span><span class="sxs-lookup"><span data-stu-id="7d377-224">The *service document* contains a list of the entity sets for the OData endpoint.</span></span> <span data-ttu-id="7d377-225">Aby uzyskać dokumentu usługi, Wyślij żądanie Pobierz do katalogu głównego identyfikatora URI usługi.</span><span class="sxs-lookup"><span data-stu-id="7d377-225">To get the service document, send a GET request to the root URI of the service.</span></span>

<span data-ttu-id="7d377-226">Za pomocą programu Fiddler, wprowadź następujący identyfikator URI w **Composer** kartę: `http://localhost:port/odata/`, gdzie *portu* jest numerem portu.</span><span class="sxs-lookup"><span data-stu-id="7d377-226">Using Fiddler, enter the following URI in the **Composer** tab: `http://localhost:port/odata/`, where *port* is the port number.</span></span>

![](creating-an-odata-endpoint/_static/image13.png)

<span data-ttu-id="7d377-227">Kliknij przycisk **Execute** przycisku.</span><span class="sxs-lookup"><span data-stu-id="7d377-227">Click the **Execute** button.</span></span> <span data-ttu-id="7d377-228">Narzędzie fiddler wysyła żądanie HTTP GET do aplikacji.</span><span class="sxs-lookup"><span data-stu-id="7d377-228">Fiddler sends an HTTP GET request to your application.</span></span> <span data-ttu-id="7d377-229">Powinny pojawić się odpowiedź na liście sesji w sieci Web.</span><span class="sxs-lookup"><span data-stu-id="7d377-229">You should see the response in the Web Sessions list.</span></span> <span data-ttu-id="7d377-230">Jeśli wszystko działa, kod stanu będzie 200.</span><span class="sxs-lookup"><span data-stu-id="7d377-230">If everything is working, the status code will be 200.</span></span>

![](creating-an-odata-endpoint/_static/image14.png)

<span data-ttu-id="7d377-231">Kliknij dwukrotnie odpowiedzi na liście sesji w sieci Web, aby wyświetlić szczegóły komunikatu odpowiedzi na karcie inspektorzy.</span><span class="sxs-lookup"><span data-stu-id="7d377-231">Double-click the response in the Web Sessions list to see the details of the response message in the Inspectors tab.</span></span>

![](creating-an-odata-endpoint/_static/image15.png)

<span data-ttu-id="7d377-232">Nieprzetworzone komunikatu odpowiedzi HTTP powinien wyglądać podobnie do następującego:</span><span class="sxs-lookup"><span data-stu-id="7d377-232">The raw HTTP response message should look similar to the following:</span></span>

[!code-console[Main](creating-an-odata-endpoint/samples/sample8.cmd)]

<span data-ttu-id="7d377-233">Domyślnie internetowy interfejs API zwraca dokument usługi w formacie AtomPub.</span><span class="sxs-lookup"><span data-stu-id="7d377-233">By default, Web API returns the service document in AtomPub format.</span></span> <span data-ttu-id="7d377-234">Aby zażądać JSON, należy dodać następujący nagłówek żądania HTTP:</span><span class="sxs-lookup"><span data-stu-id="7d377-234">To request JSON, add the following header to the HTTP request:</span></span>

`Accept: application/json`

![](creating-an-odata-endpoint/_static/image16.png)

<span data-ttu-id="7d377-235">Odpowiedź HTTP zawiera teraz ładunek w formacie JSON:</span><span class="sxs-lookup"><span data-stu-id="7d377-235">Now the HTTP response contains a JSON payload:</span></span>

[!code-console[Main](creating-an-odata-endpoint/samples/sample9.cmd)]

### <a name="service-metadata-document"></a><span data-ttu-id="7d377-236">Dokument metadanych usługi</span><span class="sxs-lookup"><span data-stu-id="7d377-236">Service Metadata Document</span></span>

<span data-ttu-id="7d377-237">*Dokument metadanych usługi* w tym artykule opisano model danych usługi, za pomocą języka XML o nazwie języka definicji schematu koncepcyjnego (CSDL).</span><span class="sxs-lookup"><span data-stu-id="7d377-237">The *service metadata document* describes the data model of the service, using an XML language called the Conceptual Schema Definition Language (CSDL).</span></span> <span data-ttu-id="7d377-238">Dokument metadanych zawiera struktury danych w usłudze i może służyć do generowania kodu klienta.</span><span class="sxs-lookup"><span data-stu-id="7d377-238">The metadata document shows the structure of the data in the service, and can be used to generate client code.</span></span>

<span data-ttu-id="7d377-239">Do pobierania dokumentu metadanych, Wyślij żądanie Pobierz do `http://localhost:port/odata/$metadata`.</span><span class="sxs-lookup"><span data-stu-id="7d377-239">To get the metadata document, send a GET request to `http://localhost:port/odata/$metadata`.</span></span> <span data-ttu-id="7d377-240">Oto metadanych dla punktu końcowego, przedstawione w tym samouczku.</span><span class="sxs-lookup"><span data-stu-id="7d377-240">Here is the metadata for the endpoint shown in this tutorial.</span></span>

[!code-console[Main](creating-an-odata-endpoint/samples/sample10.cmd)]

### <a name="entity-set"></a><span data-ttu-id="7d377-241">Zestaw jednostek</span><span class="sxs-lookup"><span data-stu-id="7d377-241">Entity Set</span></span>

<span data-ttu-id="7d377-242">Aby uzyskać zestaw jednostek produktów, Wyślij żądanie Pobierz do `http://localhost:port/odata/Products`.</span><span class="sxs-lookup"><span data-stu-id="7d377-242">To get the Products entity set, send a GET request to `http://localhost:port/odata/Products`.</span></span>

[!code-console[Main](creating-an-odata-endpoint/samples/sample11.cmd)]

### <a name="entity"></a><span data-ttu-id="7d377-243">Jednostka</span><span class="sxs-lookup"><span data-stu-id="7d377-243">Entity</span></span>

<span data-ttu-id="7d377-244">Aby uzyskać indywidualnych produktów, Wyślij żądanie Pobierz do `http://localhost:port/odata/Products(1)`, gdzie "1" to identyfikator produktu.</span><span class="sxs-lookup"><span data-stu-id="7d377-244">To get an individual product, send a GET request to `http://localhost:port/odata/Products(1)`, where "1" is the product ID.</span></span>

[!code-console[Main](creating-an-odata-endpoint/samples/sample12.cmd)]

<a id="formats"></a>
## <a name="odata-serialization-formats"></a><span data-ttu-id="7d377-245">Formaty serializacji OData</span><span class="sxs-lookup"><span data-stu-id="7d377-245">OData Serialization Formats</span></span>

<span data-ttu-id="7d377-246">OData obsługuje kilka formatów serializacji:</span><span class="sxs-lookup"><span data-stu-id="7d377-246">OData supports several serialization formats:</span></span>

- <span data-ttu-id="7d377-247">Atom Pub (XML)</span><span class="sxs-lookup"><span data-stu-id="7d377-247">Atom Pub (XML)</span></span>
- <span data-ttu-id="7d377-248">JSON "jasny" (zostanie wprowadzony w protokole OData v3)</span><span class="sxs-lookup"><span data-stu-id="7d377-248">JSON "light" (introduced in OData v3)</span></span>
- <span data-ttu-id="7d377-249">JSON "pełne" (wersja 2 OData)</span><span class="sxs-lookup"><span data-stu-id="7d377-249">JSON "verbose" (OData v2)</span></span>

<span data-ttu-id="7d377-250">Domyślnie interfejs API sieci Web w formacie AtomPubJSON "jasny".</span><span class="sxs-lookup"><span data-stu-id="7d377-250">By default, Web API uses AtomPubJSON "light" format.</span></span>

<span data-ttu-id="7d377-251">Aby uzyskać AtomPub format, ustaw nagłówek Accept "application/atom + xml".</span><span class="sxs-lookup"><span data-stu-id="7d377-251">To get AtomPub format, set the Accept header to "application/atom+xml".</span></span> <span data-ttu-id="7d377-252">Oto przykład treści odpowiedzi:</span><span class="sxs-lookup"><span data-stu-id="7d377-252">Here is an example response body:</span></span>

[!code-console[Main](creating-an-odata-endpoint/samples/sample13.cmd)]

<span data-ttu-id="7d377-253">Widoczne oczywiste niedogodność Atom format: jest o wiele bardziej szczegółowy niż lekka serializacji JSON.</span><span class="sxs-lookup"><span data-stu-id="7d377-253">You can see one obvious disadvantage of the Atom format: It's a lot more verbose than the JSON light.</span></span> <span data-ttu-id="7d377-254">Jednak w przypadku klienta, który rozumie AtomPub klienta preferować tego formatu ciągu JSON.</span><span class="sxs-lookup"><span data-stu-id="7d377-254">However, if you have a client that understands AtomPub, the client might prefer that format over JSON.</span></span>

<span data-ttu-id="7d377-255">Oto JSON uproszczonej wersji tej samej jednostki:</span><span class="sxs-lookup"><span data-stu-id="7d377-255">Here is the JSON light version of the same entity:</span></span>

[!code-console[Main](creating-an-odata-endpoint/samples/sample14.cmd)]

<span data-ttu-id="7d377-256">Format JSON światła została wprowadzona w wersji 3 protokołu OData.</span><span class="sxs-lookup"><span data-stu-id="7d377-256">The JSON light format was introduced in version 3 of the OData protocol.</span></span> <span data-ttu-id="7d377-257">W celu zapewnienia zgodności z poprzednimi wersjami starsze "pełne" format JSON może żądać klient.</span><span class="sxs-lookup"><span data-stu-id="7d377-257">For backward compatibility, a client can request the older "verbose" JSON format.</span></span> <span data-ttu-id="7d377-258">Aby zażądać informacji pełnej JSON, ustaw nagłówek Accept `application/json;odata=verbose`.</span><span class="sxs-lookup"><span data-stu-id="7d377-258">To request verbose JSON, set the Accept header to `application/json;odata=verbose`.</span></span> <span data-ttu-id="7d377-259">Poniżej przedstawiono pełne wersji:</span><span class="sxs-lookup"><span data-stu-id="7d377-259">Here is the verbose version:</span></span>

[!code-console[Main](creating-an-odata-endpoint/samples/sample15.cmd)]

<span data-ttu-id="7d377-260">Ten format powoduje większej ilości metadanych w treści odpowiedzi, które można dodać znaczne obciążenie w czasie całej sesji.</span><span class="sxs-lookup"><span data-stu-id="7d377-260">This format conveys more metadata in the response body, which can add considerable overhead over an entire session.</span></span> <span data-ttu-id="7d377-261">Ponadto dodaje poziom pośrednictwa, opakowując obiekt we właściwości o nazwie "d".</span><span class="sxs-lookup"><span data-stu-id="7d377-261">Also, it adds a level of indirection by wrapping the object in a property named "d".</span></span>

## <a name="next-steps"></a><span data-ttu-id="7d377-262">Następne kroki</span><span class="sxs-lookup"><span data-stu-id="7d377-262">Next Steps</span></span>

- [<span data-ttu-id="7d377-263">Dodawanie relacji jednostki</span><span class="sxs-lookup"><span data-stu-id="7d377-263">Add Entity Relations</span></span>](working-with-entity-relations.md)
- [<span data-ttu-id="7d377-264">Dodawanie akcji OData</span><span class="sxs-lookup"><span data-stu-id="7d377-264">Add OData Actions</span></span>](odata-actions.md)
- [<span data-ttu-id="7d377-265">Wywoływanie usługi protokołu OData z klienta programu .NET</span><span class="sxs-lookup"><span data-stu-id="7d377-265">Call the OData Service From a .NET Client</span></span>](calling-an-odata-service-from-a-net-client.md)
