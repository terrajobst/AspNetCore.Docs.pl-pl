---
uid: web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api
title: "Wprowadzenie do składnika ASP.NET Web API 2 (C#)"
author: MikeWasson
description: "HTTP nie jest po prostu wysyłaniu stron sieci web. Istnieje również zaawansowane platformę do tworzenia interfejsów API, który ujawnia usług i danych. HTTP jest prosty i elastyczny i ubiq..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/28/2017
ms.topic: article
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api
msc.type: authoredcontent
ms.openlocfilehash: d881563cdb6449aada444ef0528061581113a925
ms.sourcegitcommit: 493a215355576cfa481773365de021bcf04bb9c7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/15/2018
---
<a name="get-started-with-aspnet-web-api-2-c"></a><span data-ttu-id="ed19e-105">Wprowadzenie do składnika ASP.NET Web API 2 (C#)</span><span class="sxs-lookup"><span data-stu-id="ed19e-105">Get Started with ASP.NET Web API 2 (C#)</span></span>
====================
<span data-ttu-id="ed19e-106">przez [Wasson Jan](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="ed19e-106">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="ed19e-107">Pobieranie ukończone projektu</span><span class="sxs-lookup"><span data-stu-id="ed19e-107">Download Completed Project</span></span>](https://code.msdn.microsoft.com/Sample-code-of-Getting-c56ccb28)

<span data-ttu-id="ed19e-108">HTTP nie jest po prostu wysyłaniu stron sieci web.</span><span class="sxs-lookup"><span data-stu-id="ed19e-108">HTTP is not just for serving up web pages.</span></span> <span data-ttu-id="ed19e-109">HTTP jest również zaawansowane platformę do tworzenia interfejsów API, który ujawnia usług i danych.</span><span class="sxs-lookup"><span data-stu-id="ed19e-109">HTTP is also a powerful platform for building APIs that expose services and data.</span></span> <span data-ttu-id="ed19e-110">HTTP jest proste, elastyczne i uniwersalnych.</span><span class="sxs-lookup"><span data-stu-id="ed19e-110">HTTP is simple, flexible, and ubiquitous.</span></span> <span data-ttu-id="ed19e-111">Prawie każdego platformy, które można traktować ma biblioteki HTTP, więc usług HTTP może korzystać szeroka gama klientów, takich jak przeglądarki, urządzeń przenośnych i tradycyjnych aplikacji klasycznych.</span><span class="sxs-lookup"><span data-stu-id="ed19e-111">Almost any platform that you can think of has an HTTP library, so HTTP services can reach a broad range of clients, including browsers, mobile devices, and traditional desktop applications.</span></span>
 
<span data-ttu-id="ed19e-112">Interfejs API sieci Web platformy ASP.NET to platforma do tworzenia interfejsów API na programie .NET Framework w sieci web.</span><span class="sxs-lookup"><span data-stu-id="ed19e-112">ASP.NET Web API is a framework for building web APIs on top of the .NET Framework.</span></span> <span data-ttu-id="ed19e-113">W tym samouczku użyjesz interfejsu API sieci Web platformy ASP.NET można utworzyć składnika web API, które zwraca listę produktów.</span><span class="sxs-lookup"><span data-stu-id="ed19e-113">In this tutorial, you will use ASP.NET Web API to create a web API that returns a list of products.</span></span>

 
 ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="ed19e-114">Używane w samouczku wersje oprogramowania</span><span class="sxs-lookup"><span data-stu-id="ed19e-114">Software versions used in the tutorial</span></span>
  
 - [<span data-ttu-id="ed19e-115">Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="ed19e-115">Visual Studio 2017</span></span>](https://www.visualstudio.com/downloads/)
 - <span data-ttu-id="ed19e-116">Składnik Web API 2</span><span class="sxs-lookup"><span data-stu-id="ed19e-116">Web API 2</span></span>

<span data-ttu-id="ed19e-117">Zobacz [Tworzenie składnika web API platformy ASP.NET Core i Visual Studio dla systemu Windows](https://docs.microsoft.com/aspnet/core/tutorials/first-web-api) jest dostępna nowsza wersja tego samouczka.</span><span class="sxs-lookup"><span data-stu-id="ed19e-117">See [Create a web API with ASP.NET Core and Visual Studio for Windows](https://docs.microsoft.com/aspnet/core/tutorials/first-web-api) for a newer version of this tutorial.</span></span>

## <a name="create-a-web-api-project"></a><span data-ttu-id="ed19e-118">Utwórz projekt interfejsu API sieci Web</span><span class="sxs-lookup"><span data-stu-id="ed19e-118">Create a Web API Project</span></span>

<span data-ttu-id="ed19e-119">W tym samouczku użyjesz interfejsu API sieci Web platformy ASP.NET można utworzyć składnika web API, które zwraca listę produktów.</span><span class="sxs-lookup"><span data-stu-id="ed19e-119">In this tutorial, you will use ASP.NET Web API to create a web API that returns a list of products.</span></span> <span data-ttu-id="ed19e-120">Strony sieci web frontonu używa jQuery, aby wyświetlić wyniki.</span><span class="sxs-lookup"><span data-stu-id="ed19e-120">The front-end web page uses jQuery to display the results.</span></span>

![](tutorial-your-first-web-api/_static/image1.png)

<span data-ttu-id="ed19e-121">Uruchom program Visual Studio i wybierz **nowy projekt** z **Start** strony.</span><span class="sxs-lookup"><span data-stu-id="ed19e-121">Start Visual Studio and select **New Project** from the **Start** page.</span></span> <span data-ttu-id="ed19e-122">Lub z **pliku** menu, wybierz opcję **nowy** , a następnie **projektu**.</span><span class="sxs-lookup"><span data-stu-id="ed19e-122">Or, from the **File** menu, select **New** and then **Project**.</span></span>

<span data-ttu-id="ed19e-123">W **szablony** okienku wybierz **zainstalowane szablony** i rozwiń **Visual C#** węzła.</span><span class="sxs-lookup"><span data-stu-id="ed19e-123">In the **Templates** pane, select **Installed Templates** and expand the **Visual C#** node.</span></span> <span data-ttu-id="ed19e-124">W obszarze **Visual C#**, wybierz pozycję **Web**.</span><span class="sxs-lookup"><span data-stu-id="ed19e-124">Under **Visual C#**, select **Web**.</span></span> <span data-ttu-id="ed19e-125">Na liście szablony projektów, wybierz **aplikacji sieci Web ASP.NET**.</span><span class="sxs-lookup"><span data-stu-id="ed19e-125">In the list of project templates, select **ASP.NET Web Application**.</span></span> <span data-ttu-id="ed19e-126">Nazwij projekt "ProductsApp", a następnie kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="ed19e-126">Name the project "ProductsApp" and click **OK**.</span></span>

![](tutorial-your-first-web-api/_static/image2.png)

<span data-ttu-id="ed19e-127">W **nowy projekt ASP.NET** okno dialogowe, wybierz opcję **pusty** szablonu.</span><span class="sxs-lookup"><span data-stu-id="ed19e-127">In the **New ASP.NET Project** dialog, select the **Empty** template.</span></span> <span data-ttu-id="ed19e-128">W obszarze &quot;. Dodaj foldery i podstawowe odwołania dla&quot;, sprawdź **interfejsu API sieci Web**.</span><span class="sxs-lookup"><span data-stu-id="ed19e-128">Under &quot;Add folders and core references for&quot;, check **Web API**.</span></span> <span data-ttu-id="ed19e-129">Kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="ed19e-129">Click **OK**.</span></span>

![](tutorial-your-first-web-api/_static/image3.png)

> [!NOTE]
> <span data-ttu-id="ed19e-130">Można również utworzyć projekt interfejsu API sieci Web za pomocą &quot;interfejsu API sieci Web&quot; szablonu.</span><span class="sxs-lookup"><span data-stu-id="ed19e-130">You can also create a Web API project using the &quot;Web API&quot; template.</span></span> <span data-ttu-id="ed19e-131">Szablon interfejsu API sieci Web używa platformy ASP.NET MVC, aby zapewnić strony pomocy interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="ed19e-131">The Web API template uses ASP.NET MVC to provide API help pages.</span></span> <span data-ttu-id="ed19e-132">Używam programu pustego szablonu w tym samouczku, ponieważ chcę wyświetlić interfejs API sieci Web bez MVC.</span><span class="sxs-lookup"><span data-stu-id="ed19e-132">I'm using the Empty template for this tutorial because I want to show Web API without MVC.</span></span> <span data-ttu-id="ed19e-133">Ogólnie rzecz biorąc nie trzeba znać ASP.NET MVC można użyć interfejsu API sieci Web.</span><span class="sxs-lookup"><span data-stu-id="ed19e-133">In general, you don't need to know ASP.NET MVC to use Web API.</span></span>


## <a name="adding-a-model"></a><span data-ttu-id="ed19e-134">Dodawanie modelu</span><span class="sxs-lookup"><span data-stu-id="ed19e-134">Adding a Model</span></span>

<span data-ttu-id="ed19e-135">A *modelu* jest obiekt, który reprezentuje dane w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="ed19e-135">A *model* is an object that represents the data in your application.</span></span> <span data-ttu-id="ed19e-136">Interfejs API sieci Web platformy ASP.NET można automatycznie serializuj model do formatu JSON, XML lub niektóre inny format, a następnie wpisz dane serializowane w treści komunikatu odpowiedzi HTTP.</span><span class="sxs-lookup"><span data-stu-id="ed19e-136">ASP.NET Web API can automatically serialize your model to JSON, XML, or some other format, and then write the serialized data into the body of the HTTP response message.</span></span> <span data-ttu-id="ed19e-137">Tak długo, jak długo klient może odczytywać format serializacji, może wykonywać deserializację obiektu.</span><span class="sxs-lookup"><span data-stu-id="ed19e-137">As long as a client can read the serialization format, it can deserialize the object.</span></span> <span data-ttu-id="ed19e-138">Większość klientów może przeanalizować składni XML lub JSON.</span><span class="sxs-lookup"><span data-stu-id="ed19e-138">Most clients can parse either XML or JSON.</span></span> <span data-ttu-id="ed19e-139">Ponadto klienta można wskazać, który format chce przez ustawienie nagłówek Accept w komunikacie żądania HTTP.</span><span class="sxs-lookup"><span data-stu-id="ed19e-139">Moreover, the client can indicate which format it wants by setting the Accept header in the HTTP request message.</span></span>

<span data-ttu-id="ed19e-140">Zacznijmy od utworzenia prostego modelu, który reprezentuje produkt.</span><span class="sxs-lookup"><span data-stu-id="ed19e-140">Let's start by creating a simple model that represents a product.</span></span>

<span data-ttu-id="ed19e-141">Jeśli w Eksploratorze rozwiązań nie jest widoczny, kliknij przycisk **widoku** menu i wybierz **Eksploratora rozwiązań**.</span><span class="sxs-lookup"><span data-stu-id="ed19e-141">If Solution Explorer is not already visible, click the **View** menu and select **Solution Explorer**.</span></span> <span data-ttu-id="ed19e-142">W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy folder modeli.</span><span class="sxs-lookup"><span data-stu-id="ed19e-142">In Solution Explorer, right-click the Models folder.</span></span> <span data-ttu-id="ed19e-143">Wybierz z menu kontekstowego **Dodaj** następnie wybierz **klasy**.</span><span class="sxs-lookup"><span data-stu-id="ed19e-143">From the context menu, select **Add** then select **Class**.</span></span>

![](tutorial-your-first-web-api/_static/image4.png)

<span data-ttu-id="ed19e-144">Nazwa klasy &quot;produktu&quot;.</span><span class="sxs-lookup"><span data-stu-id="ed19e-144">Name the class &quot;Product&quot;.</span></span> <span data-ttu-id="ed19e-145">Dodaj następujące właściwości `Product` klasy.</span><span class="sxs-lookup"><span data-stu-id="ed19e-145">Add the following properties to the `Product` class.</span></span>

[!code-csharp[Main](tutorial-your-first-web-api/samples/sample1.cs)]

## <a name="adding-a-controller"></a><span data-ttu-id="ed19e-146">Dodawanie kontrolera</span><span class="sxs-lookup"><span data-stu-id="ed19e-146">Adding a Controller</span></span>

<span data-ttu-id="ed19e-147">W składniku Web API *kontrolera* jest obiekt, który obsługuje żądania HTTP.</span><span class="sxs-lookup"><span data-stu-id="ed19e-147">In Web API, a *controller* is an object that handles HTTP requests.</span></span> <span data-ttu-id="ed19e-148">Dodamy kontrolera, które może zwracać listę produktów lub jeden produkt określony przez identyfikator.</span><span class="sxs-lookup"><span data-stu-id="ed19e-148">We'll add a controller that can return either a list of products or a single product specified by ID.</span></span>

> [!NOTE]
> <span data-ttu-id="ed19e-149">Użycie programu ASP.NET MVC znasz już kontrolerów.</span><span class="sxs-lookup"><span data-stu-id="ed19e-149">If you have used ASP.NET MVC, you are already familiar with controllers.</span></span> <span data-ttu-id="ed19e-150">Kontrolery interfejsu API sieci Web są podobne do kontrolerów MVC, ale dziedziczą **klasy ApiController** klasy zamiast **kontrolera** klasy.</span><span class="sxs-lookup"><span data-stu-id="ed19e-150">Web API controllers are similar to MVC controllers, but inherit the **ApiController** class instead of the **Controller** class.</span></span>

<span data-ttu-id="ed19e-151">W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy folder kontrolery.</span><span class="sxs-lookup"><span data-stu-id="ed19e-151">In **Solution Explorer**, right-click the Controllers folder.</span></span> <span data-ttu-id="ed19e-152">Wybierz **Dodaj** , a następnie wybierz **kontrolera**.</span><span class="sxs-lookup"><span data-stu-id="ed19e-152">Select **Add** and then select **Controller**.</span></span>

![](tutorial-your-first-web-api/_static/image5.png)

<span data-ttu-id="ed19e-153">W **Dodawanie szkieletu** okno dialogowe, wybierz opcję **kontrolera interfejsu API sieci Web — pusty**.</span><span class="sxs-lookup"><span data-stu-id="ed19e-153">In the **Add Scaffold** dialog, select **Web API Controller - Empty**.</span></span> <span data-ttu-id="ed19e-154">Kliknij przycisk **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="ed19e-154">Click **Add**.</span></span>

![](tutorial-your-first-web-api/_static/image6.png)

<span data-ttu-id="ed19e-155">W **Dodaj kontroler** okna dialogowego, nazwy kontrolera &quot;ProductsController&quot;.</span><span class="sxs-lookup"><span data-stu-id="ed19e-155">In the **Add Controller** dialog, name the controller &quot;ProductsController&quot;.</span></span> <span data-ttu-id="ed19e-156">Kliknij przycisk **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="ed19e-156">Click **Add**.</span></span>

![](tutorial-your-first-web-api/_static/image7.png)

<span data-ttu-id="ed19e-157">Rusztowania tworzy plik o nazwie ProductsController.cs w folderze kontrolerów.</span><span class="sxs-lookup"><span data-stu-id="ed19e-157">The scaffolding creates a file named ProductsController.cs in the Controllers folder.</span></span>

![](tutorial-your-first-web-api/_static/image8.png)

> [!NOTE]
> <span data-ttu-id="ed19e-158">Nie trzeba umieścić kontrolerów w folderze o nazwie kontrolerów.</span><span class="sxs-lookup"><span data-stu-id="ed19e-158">You don't need to put your controllers into a folder named Controllers.</span></span> <span data-ttu-id="ed19e-159">Nazwa folderu jest po prostu wygodny sposób organizowania plików źródłowych.</span><span class="sxs-lookup"><span data-stu-id="ed19e-159">The folder name is just a convenient way to organize your source files.</span></span>


<span data-ttu-id="ed19e-160">Jeśli ten plik nie jest jeszcze otwarty, kliknij dwukrotnie plik, aby go otworzyć.</span><span class="sxs-lookup"><span data-stu-id="ed19e-160">If this file is not open already, double-click the file to open it.</span></span> <span data-ttu-id="ed19e-161">Zastąp kod w tym pliku z następujących czynności:</span><span class="sxs-lookup"><span data-stu-id="ed19e-161">Replace the code in this file with the following:</span></span>

[!code-csharp[Main](tutorial-your-first-web-api/samples/sample2.cs)]

<span data-ttu-id="ed19e-162">Aby zachować prosty przykład, produkty są przechowywane w tablicy o ustalonym wewnątrz klasy kontrolera.</span><span class="sxs-lookup"><span data-stu-id="ed19e-162">To keep the example simple, products are stored in a fixed array inside the controller class.</span></span> <span data-ttu-id="ed19e-163">Oczywiście w rzeczywistej aplikacji, czy zapytanie dotyczące bazy danych lub użyj innego źródła danych zewnętrznych.</span><span class="sxs-lookup"><span data-stu-id="ed19e-163">Of course, in a real application, you would query a database or use some other external data source.</span></span>

<span data-ttu-id="ed19e-164">Kontroler definiuje dwie metody, które zwracają produktów:</span><span class="sxs-lookup"><span data-stu-id="ed19e-164">The controller defines two methods that return products:</span></span>

- <span data-ttu-id="ed19e-165">`GetAllProducts` Metoda zwraca całą listę produktów jako **IEnumerable&lt;produktu&gt;**  typu.</span><span class="sxs-lookup"><span data-stu-id="ed19e-165">The `GetAllProducts` method returns the entire list of products as an **IEnumerable&lt;Product&gt;** type.</span></span>
- <span data-ttu-id="ed19e-166">`GetProduct` Metody wyszukuje pojedynczy produkt przez jego identyfikator.</span><span class="sxs-lookup"><span data-stu-id="ed19e-166">The `GetProduct` method looks up a single product by its ID.</span></span>

<span data-ttu-id="ed19e-167">To już wszystko!</span><span class="sxs-lookup"><span data-stu-id="ed19e-167">That's it!</span></span> <span data-ttu-id="ed19e-168">Masz pracy składnika web API.</span><span class="sxs-lookup"><span data-stu-id="ed19e-168">You have a working web API.</span></span> <span data-ttu-id="ed19e-169">Każda metoda na kontrolerze odnosi się do co najmniej jeden identyfikator URI:</span><span class="sxs-lookup"><span data-stu-id="ed19e-169">Each method on the controller corresponds to one or more URIs:</span></span>

| <span data-ttu-id="ed19e-170">Kontroler — metoda</span><span class="sxs-lookup"><span data-stu-id="ed19e-170">Controller Method</span></span> | <span data-ttu-id="ed19e-171">Identyfikator URI</span><span class="sxs-lookup"><span data-stu-id="ed19e-171">URI</span></span> |
| --- | --- |
| <span data-ttu-id="ed19e-172">GetAllProducts</span><span class="sxs-lookup"><span data-stu-id="ed19e-172">GetAllProducts</span></span> | <span data-ttu-id="ed19e-173">/ api/produktów</span><span class="sxs-lookup"><span data-stu-id="ed19e-173">/api/products</span></span> |
| <span data-ttu-id="ed19e-174">GetProduct</span><span class="sxs-lookup"><span data-stu-id="ed19e-174">GetProduct</span></span> | <span data-ttu-id="ed19e-175">/API/produkty/*id*</span><span class="sxs-lookup"><span data-stu-id="ed19e-175">/api/products/*id*</span></span> |

<span data-ttu-id="ed19e-176">Aby uzyskać `GetProduct` metody *identyfikator* w identyfikatorze URI to symbol zastępczy.</span><span class="sxs-lookup"><span data-stu-id="ed19e-176">For the `GetProduct` method, the *id* in the URI is a placeholder.</span></span> <span data-ttu-id="ed19e-177">Na przykład, aby uzyskać produktu o identyfikatorze 5, identyfikator URI jest `api/products/5`.</span><span class="sxs-lookup"><span data-stu-id="ed19e-177">For example, to get the product with ID of 5, the URI is `api/products/5`.</span></span>

<span data-ttu-id="ed19e-178">Aby uzyskać więcej informacji dotyczących sposobu interfejsu API sieci Web kieruje żądania HTTP do metod kontrolera, zobacz [routingu na platformie ASP.NET Web API](../web-api-routing-and-actions/routing-in-aspnet-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="ed19e-178">For more information about how Web API routes HTTP requests to controller methods, see [Routing in ASP.NET Web API](../web-api-routing-and-actions/routing-in-aspnet-web-api.md).</span></span>

## <a name="calling-the-web-api-with-javascript-and-jquery"></a><span data-ttu-id="ed19e-179">Wywołanie interfejsu API sieci Web z Javascript i jQuery</span><span class="sxs-lookup"><span data-stu-id="ed19e-179">Calling the Web API with Javascript and jQuery</span></span>

<span data-ttu-id="ed19e-180">W tej sekcji dodamy strona HTML, która używa technologii AJAX do wywołania interfejsu API sieci web.</span><span class="sxs-lookup"><span data-stu-id="ed19e-180">In this section, we'll add an HTML page that uses AJAX to call the web API.</span></span> <span data-ttu-id="ed19e-181">Użyjemy jQuery wywołań AJAX i aktualizowanie strony z wynikami.</span><span class="sxs-lookup"><span data-stu-id="ed19e-181">We'll use jQuery to make the AJAX calls and also to update the page with the results.</span></span>

<span data-ttu-id="ed19e-182">W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy projekt i wybierz **Dodaj**, a następnie wybierz pozycję **nowy element**.</span><span class="sxs-lookup"><span data-stu-id="ed19e-182">In Solution Explorer, right-click the project and select **Add**, then select **New Item**.</span></span>

![](tutorial-your-first-web-api/_static/image9.png)

<span data-ttu-id="ed19e-183">W **Dodaj nowy element** okno dialogowe, wybierz opcję **sieci Web** węźle **Visual C#**, a następnie wybierz **strony HTML** elementu.</span><span class="sxs-lookup"><span data-stu-id="ed19e-183">In the **Add New Item** dialog, select the **Web** node under **Visual C#**, and then select the **HTML Page** item.</span></span> <span data-ttu-id="ed19e-184">Nazwa strony &quot;index.html&quot;.</span><span class="sxs-lookup"><span data-stu-id="ed19e-184">Name the page &quot;index.html&quot;.</span></span>

![](tutorial-your-first-web-api/_static/image10.png)

<span data-ttu-id="ed19e-185">Zastąp wszystkie elementy w tym pliku następujące czynności:</span><span class="sxs-lookup"><span data-stu-id="ed19e-185">Replace everything in this file with the following:</span></span>

[!code-html[Main](tutorial-your-first-web-api/samples/sample3.html)]

<span data-ttu-id="ed19e-186">Istnieje kilka sposobów, aby uzyskać jQuery.</span><span class="sxs-lookup"><span data-stu-id="ed19e-186">There are several ways to get jQuery.</span></span> <span data-ttu-id="ed19e-187">W tym przykładzie używany [Microsoft Ajax CDN](../../../ajax/cdn/overview.md).</span><span class="sxs-lookup"><span data-stu-id="ed19e-187">In this example, I used the [Microsoft Ajax CDN](../../../ajax/cdn/overview.md).</span></span> <span data-ttu-id="ed19e-188">Można również pobrać ją z [ http://jquery.com/ ](http://jquery.com/)i ASP.NET "Interfejsu API sieci Web" szablon projektu zawiera również jQuery.</span><span class="sxs-lookup"><span data-stu-id="ed19e-188">You can also download it from [http://jquery.com/](http://jquery.com/), and the ASP.NET "Web API" project template includes jQuery as well.</span></span>

### <a name="getting-a-list-of-products"></a><span data-ttu-id="ed19e-189">Pobieranie listy produktów</span><span class="sxs-lookup"><span data-stu-id="ed19e-189">Getting a List of Products</span></span>

<span data-ttu-id="ed19e-190">Aby uzyskać listę produktów, Wyślij żądanie HTTP GET do &quot;produkty/api/&quot;.</span><span class="sxs-lookup"><span data-stu-id="ed19e-190">To get a list of products, send an HTTP GET request to &quot;/api/products&quot;.</span></span>

<span data-ttu-id="ed19e-191">JQuery [getJSON](http://api.jquery.com/jQuery.getJSON/) funkcja wysyła żądanie AJAX.</span><span class="sxs-lookup"><span data-stu-id="ed19e-191">The jQuery [getJSON](http://api.jquery.com/jQuery.getJSON/) function sends an AJAX request.</span></span> <span data-ttu-id="ed19e-192">Odpowiedź zawiera tablicę obiektów JSON.</span><span class="sxs-lookup"><span data-stu-id="ed19e-192">For response contains array of JSON objects.</span></span> <span data-ttu-id="ed19e-193">`done` Funkcja określa wywołania zwrotnego, która jest wywoływana, gdy żądanie zakończy się pomyślnie.</span><span class="sxs-lookup"><span data-stu-id="ed19e-193">The `done` function specifies a callback that is called if the request succeeds.</span></span> <span data-ttu-id="ed19e-194">Wywołanie zwrotne możemy zaktualizować modelu DOM z informacji o produkcie.</span><span class="sxs-lookup"><span data-stu-id="ed19e-194">In the callback, we update the DOM with the product information.</span></span>

[!code-html[Main](tutorial-your-first-web-api/samples/sample4.html)]

### <a name="getting-a-product-by-id"></a><span data-ttu-id="ed19e-195">Uzyskiwanie produktu według Identyfikatora</span><span class="sxs-lookup"><span data-stu-id="ed19e-195">Getting a Product By ID</span></span>

<span data-ttu-id="ed19e-196">Aby uzyskać produktu przez identyfikator, Wyślij żądanie HTTP GET do &quot;/api/produkty/*identyfikator*&quot;, gdzie *identyfikator* jest identyfikatora produktu.</span><span class="sxs-lookup"><span data-stu-id="ed19e-196">To get a product by ID, send an HTTP GET request to &quot;/api/products/*id*&quot;, where *id* is the product ID.</span></span>

[!code-javascript[Main](tutorial-your-first-web-api/samples/sample5.js)]

<span data-ttu-id="ed19e-197">Nadal nazywamy `getJSON` do wysłania żądania AJAX, ale tym razem testujemy identyfikator w identyfikatorze URI żądania.</span><span class="sxs-lookup"><span data-stu-id="ed19e-197">We still call `getJSON` to send the AJAX request, but this time we put the ID in the request URI.</span></span> <span data-ttu-id="ed19e-198">Odpowiedź z tym żądaniem jest reprezentacja JSON jeden produkt.</span><span class="sxs-lookup"><span data-stu-id="ed19e-198">The response from this request is a JSON representation of a single product.</span></span>

## <a name="running-the-application"></a><span data-ttu-id="ed19e-199">Uruchamianie aplikacji</span><span class="sxs-lookup"><span data-stu-id="ed19e-199">Running the Application</span></span>

<span data-ttu-id="ed19e-200">Naciśnij klawisz F5, aby rozpocząć debugowania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="ed19e-200">Press F5 to start debugging the application.</span></span> <span data-ttu-id="ed19e-201">Strony sieci web powinien wyglądać następująco:</span><span class="sxs-lookup"><span data-stu-id="ed19e-201">The web page should look like the following:</span></span>

![](tutorial-your-first-web-api/_static/image11.png)

<span data-ttu-id="ed19e-202">Uzyskanie produktu przez identyfikator, wprowadź identyfikator, a następnie kliknij przycisk wyszukiwania:</span><span class="sxs-lookup"><span data-stu-id="ed19e-202">To get a product by ID, enter the ID and click Search:</span></span>

![](tutorial-your-first-web-api/_static/image12.png)

<span data-ttu-id="ed19e-203">Jeśli wprowadzisz nieprawidłowy identyfikator, serwer zwraca błąd HTTP:</span><span class="sxs-lookup"><span data-stu-id="ed19e-203">If you enter an invalid ID, the server returns an HTTP error:</span></span>

![](tutorial-your-first-web-api/_static/image13.png)

## <a name="using-f12-to-view-the-http-request-and-response"></a><span data-ttu-id="ed19e-204">Aby wyświetlić żądania HTTP i odpowiedzi przy użyciu F12</span><span class="sxs-lookup"><span data-stu-id="ed19e-204">Using F12 to View the HTTP Request and Response</span></span>

<span data-ttu-id="ed19e-205">Podczas pracy z usługą HTTP, może być bardzo przydatne zobaczyć żądania HTTP i żądania wiadomości.</span><span class="sxs-lookup"><span data-stu-id="ed19e-205">When you are working with an HTTP service, it can be very useful to see the HTTP request and request messages.</span></span> <span data-ttu-id="ed19e-206">Można to zrobić za pomocą narzędzi deweloperskich F12 w programie Internet Explorer 9.</span><span class="sxs-lookup"><span data-stu-id="ed19e-206">You can do this by using the F12 developer tools in Internet Explorer 9.</span></span> <span data-ttu-id="ed19e-207">Internet Explorer 9, naciśnij klawisze **F12** można otworzyć narzędzia.</span><span class="sxs-lookup"><span data-stu-id="ed19e-207">From Internet Explorer 9, press **F12** to open the tools.</span></span> <span data-ttu-id="ed19e-208">Kliknij przycisk **sieci** kartę i naciśnij klawisz **Start Przechwytywanie**.</span><span class="sxs-lookup"><span data-stu-id="ed19e-208">Click the **Network** tab and press **Start Capturing**.</span></span> <span data-ttu-id="ed19e-209">Teraz przejdź do strony sieci web i naciśnij klawisz **F5** ponowne załadowanie tej strony sieci web.</span><span class="sxs-lookup"><span data-stu-id="ed19e-209">Now go back to the web page and press **F5** to reload the web page.</span></span> <span data-ttu-id="ed19e-210">Program Internet Explorer będzie przechwytywać ruchu HTTP między przeglądarką a serwerem sieci web.</span><span class="sxs-lookup"><span data-stu-id="ed19e-210">Internet Explorer will capture the HTTP traffic between the browser and the web server.</span></span> <span data-ttu-id="ed19e-211">Widok podsumowania przedstawia cały ruch sieciowy dla strony:</span><span class="sxs-lookup"><span data-stu-id="ed19e-211">The summary view shows all the network traffic for a page:</span></span>

![](tutorial-your-first-web-api/_static/image14.png)

<span data-ttu-id="ed19e-212">Znajdź wpis dla względnego identyfikatora URI "produkty/api /".</span><span class="sxs-lookup"><span data-stu-id="ed19e-212">Locate the entry for the relative URI "api/products/".</span></span> <span data-ttu-id="ed19e-213">Wybierz tę pozycję, a następnie kliknij przycisk **przejdź do widoku szczegółowe**.</span><span class="sxs-lookup"><span data-stu-id="ed19e-213">Select this entry and click **Go to detailed view**.</span></span> <span data-ttu-id="ed19e-214">W widoku szczegółów istnieją karty, aby wyświetlić nagłówki żądań i odpowiedzi i treść.</span><span class="sxs-lookup"><span data-stu-id="ed19e-214">In the detail view, there are tabs to view the request and response headers and bodies.</span></span> <span data-ttu-id="ed19e-215">Na przykład, jeśli klikniesz przycisk **nagłówki żądań** karcie widać, że klient żądał &quot;application/json&quot; w nagłówku Accept.</span><span class="sxs-lookup"><span data-stu-id="ed19e-215">For example, if you click the **Request headers** tab, you can see that the client requested &quot;application/json&quot; in the Accept header.</span></span>

![](tutorial-your-first-web-api/_static/image15.png)

<span data-ttu-id="ed19e-216">Jeśli klikniesz kartę treść odpowiedzi widać, jak lista produktów została wykonana serializacja do formatu JSON.</span><span class="sxs-lookup"><span data-stu-id="ed19e-216">If you click the Response body tab, you can see how the product list was serialized to JSON.</span></span> <span data-ttu-id="ed19e-217">Inne przeglądarki mają podobną funkcjonalność.</span><span class="sxs-lookup"><span data-stu-id="ed19e-217">Other browsers have similar functionality.</span></span> <span data-ttu-id="ed19e-218">Inne przydatne narzędzie jest [Fiddler](http://www.fiddler2.com/fiddler2/), debugowanie serwera proxy sieci web.</span><span class="sxs-lookup"><span data-stu-id="ed19e-218">Another useful tool is [Fiddler](http://www.fiddler2.com/fiddler2/), a web debugging proxy.</span></span> <span data-ttu-id="ed19e-219">Służy Fiddler do wyświetlania ruchu HTTP i tworzą żądania HTTP, co pozwala na pełną kontrolę nad nagłówków HTTP w żądaniu.</span><span class="sxs-lookup"><span data-stu-id="ed19e-219">You can use Fiddler to view your HTTP traffic, and also to compose HTTP requests, which gives you full control over the HTTP headers in the request.</span></span>

## <a name="see-this-app-running-on-azure"></a><span data-ttu-id="ed19e-220">Zobacz tej aplikacji działających na platformie Azure</span><span class="sxs-lookup"><span data-stu-id="ed19e-220">See this App Running on Azure</span></span>

<span data-ttu-id="ed19e-221">Czy chcesz w witrynie Zakończono uruchomione jako aplikacji sieci web?</span><span class="sxs-lookup"><span data-stu-id="ed19e-221">Would you like to see the finished site running as a live web app?</span></span> <span data-ttu-id="ed19e-222">Pełną wersję aplikacji można wdrożyć do konta platformy Azure w celu wystarczy kliknąć poniższy przycisk.</span><span class="sxs-lookup"><span data-stu-id="ed19e-222">You can deploy a complete version of the app to your Azure account by simply clicking the following button.</span></span>

[![](http://azuredeploy.net/deploybutton.png)](https://deploy.azure.com/?WT.mc_id=deploy_azure_aspnet&repository=https://github.com/tfitzmac/WebAPI-ProductsApp#/form/setup)

<span data-ttu-id="ed19e-223">Potrzebujesz konta platformy Azure, aby wdrożyć to rozwiązanie do platformy Azure.</span><span class="sxs-lookup"><span data-stu-id="ed19e-223">You need an Azure account to deploy this solution to Azure.</span></span> <span data-ttu-id="ed19e-224">Jeśli nie masz już konto, dostępne są następujące opcje:</span><span class="sxs-lookup"><span data-stu-id="ed19e-224">If you do not already have an account, you have the following options:</span></span>

- <span data-ttu-id="ed19e-225">[Otworzyć bezpłatne konto platformy Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A443DD604) — otrzymasz kredyt służy do wypróbowania płatnych usług Azure, a nawet po wyczerpaniu kredytu możesz zachować konto i korzystać z bezpłatnych usług platformy Azure.</span><span class="sxs-lookup"><span data-stu-id="ed19e-225">[Open an Azure account for free](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A443DD604) - You get credits you can use to try out paid Azure services, and even after they're used up you can keep the account and use free Azure services.</span></span>
- <span data-ttu-id="ed19e-226">[Aktywować korzyści dla subskrybentów MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604) -subskrypcji Your MSDN otrzymasz kredyt, co miesiąc, używanego programu płatnych usług Azure.</span><span class="sxs-lookup"><span data-stu-id="ed19e-226">[Activate MSDN subscriber benefits](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604) - Your MSDN subscription gives you credits every month that you can use for paid Azure services.</span></span>

## <a name="next-steps"></a><span data-ttu-id="ed19e-227">Następne kroki</span><span class="sxs-lookup"><span data-stu-id="ed19e-227">Next Steps</span></span>

- <span data-ttu-id="ed19e-228">Aby bardziej szczegółowy przykład usługi HTTP, która obsługuje operacje POST, PUT i DELETE i zapisuje do bazy danych, zobacz [przy użyciu sieci Web API 2 z programu Entity Framework 6](../data/using-web-api-with-entity-framework/part-1.md).</span><span class="sxs-lookup"><span data-stu-id="ed19e-228">For a more complete example of an HTTP service that supports POST, PUT, and DELETE actions and writes to a database, see [Using Web API 2 with Entity Framework 6](../data/using-web-api-with-entity-framework/part-1.md).</span></span>
- <span data-ttu-id="ed19e-229">Aby uzyskać więcej informacji na temat tworzenia aplikacji sieci web płynne i szybkie ponad usługą HTTP, zobacz [aplikacji jednej strony ASP.NET](../../../single-page-application/index.md).</span><span class="sxs-lookup"><span data-stu-id="ed19e-229">For more about creating fluid and responsive web applications on top of an HTTP service, see [ASP.NET Single Page Application](../../../single-page-application/index.md).</span></span>
- <span data-ttu-id="ed19e-230">Aby uzyskać informacje dotyczące wdrażania projektu sieci web programu Visual Studio w usłudze Azure App Service, zobacz [tworzenie aplikacji sieci web platformy ASP.NET w usłudze Azure App Service](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-get-started/).</span><span class="sxs-lookup"><span data-stu-id="ed19e-230">For information about how to deploy a Visual Studio web project to Azure App Service, see [Create an ASP.NET web app in Azure App Service](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-get-started/).</span></span>
