---
uid: web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api
title: Wprowadzenie do wzorca ASP.NET Web API 2 (C#)
author: MikeWasson
description: Protokół HTTP nie jest dostępne tylko dla wysyłaniu stron sieci web. Ponadto jest to zaawansowana platforma do tworzenia interfejsów API, które udostępniają dane i usługi. Protokół HTTP jest proste, elastyczne i ubiq...
ms.author: aspnetcontent
ms.date: 11/28/2017
msc.legacyurl: /web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api
msc.type: authoredcontent
ms.openlocfilehash: 170e361c46631e7ecdbe026061c181158dcf803f
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/05/2018
ms.locfileid: "37823423"
---
<a name="get-started-with-aspnet-web-api-2-c"></a><span data-ttu-id="90f52-105">Wprowadzenie do wzorca ASP.NET Web API 2 (C#)</span><span class="sxs-lookup"><span data-stu-id="90f52-105">Get Started with ASP.NET Web API 2 (C#)</span></span>
====================
<span data-ttu-id="90f52-106">przez [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="90f52-106">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="90f52-107">Pobieranie ukończone projektu</span><span class="sxs-lookup"><span data-stu-id="90f52-107">Download Completed Project</span></span>](https://code.msdn.microsoft.com/Sample-code-of-Getting-c56ccb28)

<span data-ttu-id="90f52-108">Protokół HTTP nie jest dostępne tylko dla wysyłaniu stron sieci web.</span><span class="sxs-lookup"><span data-stu-id="90f52-108">HTTP is not just for serving up web pages.</span></span> <span data-ttu-id="90f52-109">Protokół HTTP, również jest to zaawansowana platforma do tworzenia interfejsów API, które udostępniają dane i usługi.</span><span class="sxs-lookup"><span data-stu-id="90f52-109">HTTP is also a powerful platform for building APIs that expose services and data.</span></span> <span data-ttu-id="90f52-110">Protokół HTTP jest proste, elastyczne i uniwersalnych.</span><span class="sxs-lookup"><span data-stu-id="90f52-110">HTTP is simple, flexible, and ubiquitous.</span></span> <span data-ttu-id="90f52-111">Niemal dowolną platformą, która można traktować ma biblioteki HTTP, więc usług HTTP można dotrzeć do szerokiej gamy klientów, w tym przeglądarek i urządzeń przenośnych, tradycyjne aplikacje komputerowe.</span><span class="sxs-lookup"><span data-stu-id="90f52-111">Almost any platform that you can think of has an HTTP library, so HTTP services can reach a broad range of clients, including browsers, mobile devices, and traditional desktop applications.</span></span>
 
<span data-ttu-id="90f52-112">Web API platformy ASP.NET to architektura służąca do tworzenia interfejsów API w programie .NET Framework w sieci web.</span><span class="sxs-lookup"><span data-stu-id="90f52-112">ASP.NET Web API is a framework for building web APIs on top of the .NET Framework.</span></span> <span data-ttu-id="90f52-113">W tym samouczku użyjesz interfejsu API sieci Web platformy ASP.NET do tworzenia internetowego interfejsu API, które zwraca listę produktów.</span><span class="sxs-lookup"><span data-stu-id="90f52-113">In this tutorial, you will use ASP.NET Web API to create a web API that returns a list of products.</span></span>

 
 ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="90f52-114">Wersje oprogramowania używanego w tym samouczku</span><span class="sxs-lookup"><span data-stu-id="90f52-114">Software versions used in the tutorial</span></span>
  
 - [<span data-ttu-id="90f52-115">Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="90f52-115">Visual Studio 2017</span></span>](https://www.visualstudio.com/downloads/)
 - <span data-ttu-id="90f52-116">Składnik Web API 2</span><span class="sxs-lookup"><span data-stu-id="90f52-116">Web API 2</span></span>

<span data-ttu-id="90f52-117">Zobacz [Tworzenie internetowego interfejsu API za pomocą platformy ASP.NET Core i Visual Studio for Windows](https://docs.microsoft.com/aspnet/core/tutorials/first-web-api) jest dostępna nowsza wersja tego samouczka.</span><span class="sxs-lookup"><span data-stu-id="90f52-117">See [Create a web API with ASP.NET Core and Visual Studio for Windows](https://docs.microsoft.com/aspnet/core/tutorials/first-web-api) for a newer version of this tutorial.</span></span>

## <a name="create-a-web-api-project"></a><span data-ttu-id="90f52-118">Utwórz projekt internetowego interfejsu API</span><span class="sxs-lookup"><span data-stu-id="90f52-118">Create a Web API Project</span></span>

<span data-ttu-id="90f52-119">W tym samouczku użyjesz interfejsu API sieci Web platformy ASP.NET do tworzenia internetowego interfejsu API, które zwraca listę produktów.</span><span class="sxs-lookup"><span data-stu-id="90f52-119">In this tutorial, you will use ASP.NET Web API to create a web API that returns a list of products.</span></span> <span data-ttu-id="90f52-120">Strony sieci web frontonu używa jQuery, aby wyświetlić wyniki.</span><span class="sxs-lookup"><span data-stu-id="90f52-120">The front-end web page uses jQuery to display the results.</span></span>

![](tutorial-your-first-web-api/_static/image1.png)

<span data-ttu-id="90f52-121">Uruchom program Visual Studio i wybierz **nowy projekt** z **Start** strony.</span><span class="sxs-lookup"><span data-stu-id="90f52-121">Start Visual Studio and select **New Project** from the **Start** page.</span></span> <span data-ttu-id="90f52-122">Lub z **pliku** menu, wybierz opcję **New** i następnie **projektu**.</span><span class="sxs-lookup"><span data-stu-id="90f52-122">Or, from the **File** menu, select **New** and then **Project**.</span></span>

<span data-ttu-id="90f52-123">W **szablony** okienku wybierz **zainstalowane szablony** i rozwiń **Visual C#** węzła.</span><span class="sxs-lookup"><span data-stu-id="90f52-123">In the **Templates** pane, select **Installed Templates** and expand the **Visual C#** node.</span></span> <span data-ttu-id="90f52-124">W obszarze **Visual C#**, wybierz opcję **Web**.</span><span class="sxs-lookup"><span data-stu-id="90f52-124">Under **Visual C#**, select **Web**.</span></span> <span data-ttu-id="90f52-125">Na liście szablonów projektu wybierz **aplikacji sieci Web ASP.NET**.</span><span class="sxs-lookup"><span data-stu-id="90f52-125">In the list of project templates, select **ASP.NET Web Application**.</span></span> <span data-ttu-id="90f52-126">Nadaj projektowi nazwę "ProductsApp", a następnie kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="90f52-126">Name the project "ProductsApp" and click **OK**.</span></span>

![](tutorial-your-first-web-api/_static/image2.png)

<span data-ttu-id="90f52-127">W **nowy projekt ASP.NET** okno dialogowe, wybierz opcję **pusty** szablonu.</span><span class="sxs-lookup"><span data-stu-id="90f52-127">In the **New ASP.NET Project** dialog, select the **Empty** template.</span></span> <span data-ttu-id="90f52-128">W obszarze &quot;. Dodaj foldery i podstawowe odwołania dla&quot;, sprawdź **interfejsu API sieci Web**.</span><span class="sxs-lookup"><span data-stu-id="90f52-128">Under &quot;Add folders and core references for&quot;, check **Web API**.</span></span> <span data-ttu-id="90f52-129">Kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="90f52-129">Click **OK**.</span></span>

![](tutorial-your-first-web-api/_static/image3.png)

> [!NOTE]
> <span data-ttu-id="90f52-130">Można również utworzyć projekt interfejsu API sieci Web za pomocą &quot;interfejsu API sieci Web&quot; szablonu.</span><span class="sxs-lookup"><span data-stu-id="90f52-130">You can also create a Web API project using the &quot;Web API&quot; template.</span></span> <span data-ttu-id="90f52-131">Szablon interfejsu API sieci Web używa platformy ASP.NET MVC w celu zapewnienia stron pomocy interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="90f52-131">The Web API template uses ASP.NET MVC to provide API help pages.</span></span> <span data-ttu-id="90f52-132">Używam pustego szablonu na potrzeby tego samouczka ponieważ chcę pokazać interfejs API sieci Web bez MVC.</span><span class="sxs-lookup"><span data-stu-id="90f52-132">I'm using the Empty template for this tutorial because I want to show Web API without MVC.</span></span> <span data-ttu-id="90f52-133">Ogólnie rzecz biorąc nie trzeba znać platformy ASP.NET MVC w celu korzystania z interfejsu API sieci Web.</span><span class="sxs-lookup"><span data-stu-id="90f52-133">In general, you don't need to know ASP.NET MVC to use Web API.</span></span>


## <a name="adding-a-model"></a><span data-ttu-id="90f52-134">Dodawanie modelu</span><span class="sxs-lookup"><span data-stu-id="90f52-134">Adding a Model</span></span>

<span data-ttu-id="90f52-135">A *modelu* jest obiektem, który reprezentuje dane w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="90f52-135">A *model* is an object that represents the data in your application.</span></span> <span data-ttu-id="90f52-136">ASP.NET Web API może automatycznie serializuj model do formatu JSON, XML lub innym formacie, a następnie wpisz dane serializowane w treści komunikatu odpowiedzi HTTP.</span><span class="sxs-lookup"><span data-stu-id="90f52-136">ASP.NET Web API can automatically serialize your model to JSON, XML, or some other format, and then write the serialized data into the body of the HTTP response message.</span></span> <span data-ttu-id="90f52-137">Tak długo, jak długo klient może odczytywać format serializacji, może wykonywać deserializację obiektu.</span><span class="sxs-lookup"><span data-stu-id="90f52-137">As long as a client can read the serialization format, it can deserialize the object.</span></span> <span data-ttu-id="90f52-138">Większość klientów może przeanalizować, XML lub JSON.</span><span class="sxs-lookup"><span data-stu-id="90f52-138">Most clients can parse either XML or JSON.</span></span> <span data-ttu-id="90f52-139">Ponadto klient może wskazywać formatu, który chce, ustawiając nagłówek Accept w komunikacie żądania HTTP.</span><span class="sxs-lookup"><span data-stu-id="90f52-139">Moreover, the client can indicate which format it wants by setting the Accept header in the HTTP request message.</span></span>

<span data-ttu-id="90f52-140">Zacznijmy od utworzenia prostego modelu, który reprezentuje produkt.</span><span class="sxs-lookup"><span data-stu-id="90f52-140">Let's start by creating a simple model that represents a product.</span></span>

<span data-ttu-id="90f52-141">Eksplorator rozwiązań nie jest jeszcze widoczny, kliknij przycisk **widoku** menu, a następnie wybierz **Eksploratora rozwiązań**.</span><span class="sxs-lookup"><span data-stu-id="90f52-141">If Solution Explorer is not already visible, click the **View** menu and select **Solution Explorer**.</span></span> <span data-ttu-id="90f52-142">W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy folderu modeli.</span><span class="sxs-lookup"><span data-stu-id="90f52-142">In Solution Explorer, right-click the Models folder.</span></span> <span data-ttu-id="90f52-143">Z menu kontekstowego wybierz **Dodaj** polecenie **klasy**.</span><span class="sxs-lookup"><span data-stu-id="90f52-143">From the context menu, select **Add** then select **Class**.</span></span>

![](tutorial-your-first-web-api/_static/image4.png)

<span data-ttu-id="90f52-144">Nazwa klasy &quot;produktu&quot;.</span><span class="sxs-lookup"><span data-stu-id="90f52-144">Name the class &quot;Product&quot;.</span></span> <span data-ttu-id="90f52-145">Dodaj następujące właściwości do `Product` klasy.</span><span class="sxs-lookup"><span data-stu-id="90f52-145">Add the following properties to the `Product` class.</span></span>

[!code-csharp[Main](tutorial-your-first-web-api/samples/sample1.cs)]

## <a name="adding-a-controller"></a><span data-ttu-id="90f52-146">Dodawanie kontrolera</span><span class="sxs-lookup"><span data-stu-id="90f52-146">Adding a Controller</span></span>

<span data-ttu-id="90f52-147">W interfejsie API sieci Web *kontrolera* jest obiektem, który obsługuje żądania HTTP.</span><span class="sxs-lookup"><span data-stu-id="90f52-147">In Web API, a *controller* is an object that handles HTTP requests.</span></span> <span data-ttu-id="90f52-148">Dodamy kontroler, który może zwrócić listę produktów lub jednego produktu, określonego przez identyfikator.</span><span class="sxs-lookup"><span data-stu-id="90f52-148">We'll add a controller that can return either a list of products or a single product specified by ID.</span></span>

> [!NOTE]
> <span data-ttu-id="90f52-149">Jeśli używasz platformy ASP.NET MVC, znasz już kontrolerów.</span><span class="sxs-lookup"><span data-stu-id="90f52-149">If you have used ASP.NET MVC, you are already familiar with controllers.</span></span> <span data-ttu-id="90f52-150">Interfejs API sieci Web kontrolery są podobne do kontrolerów MVC, ale dziedziczą **klasy ApiController** klasy zamiast **kontrolera** klasy.</span><span class="sxs-lookup"><span data-stu-id="90f52-150">Web API controllers are similar to MVC controllers, but inherit the **ApiController** class instead of the **Controller** class.</span></span>

<span data-ttu-id="90f52-151">W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy folder kontrolerów.</span><span class="sxs-lookup"><span data-stu-id="90f52-151">In **Solution Explorer**, right-click the Controllers folder.</span></span> <span data-ttu-id="90f52-152">Wybierz **Dodaj** , a następnie wybierz **kontrolera**.</span><span class="sxs-lookup"><span data-stu-id="90f52-152">Select **Add** and then select **Controller**.</span></span>

![](tutorial-your-first-web-api/_static/image5.png)

<span data-ttu-id="90f52-153">W **Dodawanie szkieletu** okno dialogowe, wybierz opcję **kontroler internetowego interfejsu API — pusty**.</span><span class="sxs-lookup"><span data-stu-id="90f52-153">In the **Add Scaffold** dialog, select **Web API Controller - Empty**.</span></span> <span data-ttu-id="90f52-154">Kliknij przycisk **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="90f52-154">Click **Add**.</span></span>

![](tutorial-your-first-web-api/_static/image6.png)

<span data-ttu-id="90f52-155">W **Dodaj kontroler** okno dialogowe, nazwy kontrolera &quot;ProductsController&quot;.</span><span class="sxs-lookup"><span data-stu-id="90f52-155">In the **Add Controller** dialog, name the controller &quot;ProductsController&quot;.</span></span> <span data-ttu-id="90f52-156">Kliknij przycisk **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="90f52-156">Click **Add**.</span></span>

![](tutorial-your-first-web-api/_static/image7.png)

<span data-ttu-id="90f52-157">Szkieletu tworzy plik o nazwie ProductsController.cs w folderze kontrolerów.</span><span class="sxs-lookup"><span data-stu-id="90f52-157">The scaffolding creates a file named ProductsController.cs in the Controllers folder.</span></span>

![](tutorial-your-first-web-api/_static/image8.png)

> [!NOTE]
> <span data-ttu-id="90f52-158">Nie ma potrzeby kontrolerach należy umieścić w folderze o nazwie kontrolerów.</span><span class="sxs-lookup"><span data-stu-id="90f52-158">You don't need to put your controllers into a folder named Controllers.</span></span> <span data-ttu-id="90f52-159">Nazwa folderu jest po prostu wygodny sposób organizowania plików źródłowych.</span><span class="sxs-lookup"><span data-stu-id="90f52-159">The folder name is just a convenient way to organize your source files.</span></span>


<span data-ttu-id="90f52-160">Jeśli ten plik nie jest jeszcze otwarty, kliknij dwukrotnie plik, aby go otworzyć.</span><span class="sxs-lookup"><span data-stu-id="90f52-160">If this file is not open already, double-click the file to open it.</span></span> <span data-ttu-id="90f52-161">Zastąp kod w tym pliku następujących czynności:</span><span class="sxs-lookup"><span data-stu-id="90f52-161">Replace the code in this file with the following:</span></span>

[!code-csharp[Main](tutorial-your-first-web-api/samples/sample2.cs)]

<span data-ttu-id="90f52-162">W celu uproszczenia w przykładzie produkty są przechowywane w tablicy o stałym wewnątrz klasy kontrolera.</span><span class="sxs-lookup"><span data-stu-id="90f52-162">To keep the example simple, products are stored in a fixed array inside the controller class.</span></span> <span data-ttu-id="90f52-163">Oczywiście w rzeczywistej aplikacji, czy zapytanie dotyczące bazy danych lub użyj innego źródła danych zewnętrznych.</span><span class="sxs-lookup"><span data-stu-id="90f52-163">Of course, in a real application, you would query a database or use some other external data source.</span></span>

<span data-ttu-id="90f52-164">Kontroler definiuje dwie metody, które zwracają produktów:</span><span class="sxs-lookup"><span data-stu-id="90f52-164">The controller defines two methods that return products:</span></span>

- <span data-ttu-id="90f52-165">`GetAllProducts` Metoda zwraca całą listę produktów jako **IEnumerable&lt;produktu&gt;**  typu.</span><span class="sxs-lookup"><span data-stu-id="90f52-165">The `GetAllProducts` method returns the entire list of products as an **IEnumerable&lt;Product&gt;** type.</span></span>
- <span data-ttu-id="90f52-166">`GetProduct` Metoda odwołuje się do jednego produktu za pomocą jego identyfikatora.</span><span class="sxs-lookup"><span data-stu-id="90f52-166">The `GetProduct` method looks up a single product by its ID.</span></span>

<span data-ttu-id="90f52-167">To wszystko!</span><span class="sxs-lookup"><span data-stu-id="90f52-167">That's it!</span></span> <span data-ttu-id="90f52-168">Masz pracy internetowego interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="90f52-168">You have a working web API.</span></span> <span data-ttu-id="90f52-169">Każda metoda na kontrolerze odnosi się do jednego lub więcej identyfikatorów URI:</span><span class="sxs-lookup"><span data-stu-id="90f52-169">Each method on the controller corresponds to one or more URIs:</span></span>

| <span data-ttu-id="90f52-170">Metoda kontrolera</span><span class="sxs-lookup"><span data-stu-id="90f52-170">Controller Method</span></span> | <span data-ttu-id="90f52-171">Identyfikator URI</span><span class="sxs-lookup"><span data-stu-id="90f52-171">URI</span></span> |
| --- | --- |
| <span data-ttu-id="90f52-172">GetAllProducts</span><span class="sxs-lookup"><span data-stu-id="90f52-172">GetAllProducts</span></span> | <span data-ttu-id="90f52-173">/ api/produktów</span><span class="sxs-lookup"><span data-stu-id="90f52-173">/api/products</span></span> |
| <span data-ttu-id="90f52-174">GetProduct</span><span class="sxs-lookup"><span data-stu-id="90f52-174">GetProduct</span></span> | <span data-ttu-id="90f52-175">/ InterfejsAPI/produkty/*identyfikator*</span><span class="sxs-lookup"><span data-stu-id="90f52-175">/api/products/*id*</span></span> |

<span data-ttu-id="90f52-176">Aby uzyskać `GetProduct` metody *identyfikator* w identyfikatorze URI jest symbolem zastępczym.</span><span class="sxs-lookup"><span data-stu-id="90f52-176">For the `GetProduct` method, the *id* in the URI is a placeholder.</span></span> <span data-ttu-id="90f52-177">Na przykład, aby uzyskać produkt o identyfikatorze 5, identyfikator URI jest `api/products/5`.</span><span class="sxs-lookup"><span data-stu-id="90f52-177">For example, to get the product with ID of 5, the URI is `api/products/5`.</span></span>

<span data-ttu-id="90f52-178">Aby uzyskać więcej informacji na temat sposobu kierowania żądań HTTP interfejsu API sieci Web do metod kontrolera, zobacz [routingu ASP.NET Web API](../web-api-routing-and-actions/routing-in-aspnet-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="90f52-178">For more information about how Web API routes HTTP requests to controller methods, see [Routing in ASP.NET Web API](../web-api-routing-and-actions/routing-in-aspnet-web-api.md).</span></span>

## <a name="calling-the-web-api-with-javascript-and-jquery"></a><span data-ttu-id="90f52-179">Wywoływanie interfejsu API sieci Web za pomocą języka Javascript i jQuery</span><span class="sxs-lookup"><span data-stu-id="90f52-179">Calling the Web API with Javascript and jQuery</span></span>

<span data-ttu-id="90f52-180">W tej sekcji dodamy strona HTML, która używa technologii AJAX w celu wywołania interfejsu API sieci web.</span><span class="sxs-lookup"><span data-stu-id="90f52-180">In this section, we'll add an HTML page that uses AJAX to call the web API.</span></span> <span data-ttu-id="90f52-181">Użyjemy jQuery wykonywanie wywołań AJAX, a także do aktualizacji strony z wynikami.</span><span class="sxs-lookup"><span data-stu-id="90f52-181">We'll use jQuery to make the AJAX calls and also to update the page with the results.</span></span>

<span data-ttu-id="90f52-182">W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy projekt i wybierz **Dodaj**, a następnie wybierz **nowy element**.</span><span class="sxs-lookup"><span data-stu-id="90f52-182">In Solution Explorer, right-click the project and select **Add**, then select **New Item**.</span></span>

![](tutorial-your-first-web-api/_static/image9.png)

<span data-ttu-id="90f52-183">W **Dodaj nowy element** okno dialogowe, wybierz opcję **Web** węzeł w węźle **Visual C#**, a następnie wybierz pozycję **strony HTML** elementu.</span><span class="sxs-lookup"><span data-stu-id="90f52-183">In the **Add New Item** dialog, select the **Web** node under **Visual C#**, and then select the **HTML Page** item.</span></span> <span data-ttu-id="90f52-184">Nazwij stronę &quot;index.html&quot;.</span><span class="sxs-lookup"><span data-stu-id="90f52-184">Name the page &quot;index.html&quot;.</span></span>

![](tutorial-your-first-web-api/_static/image10.png)

<span data-ttu-id="90f52-185">Zamień wszystko, co w tym pliku następujące czynności:</span><span class="sxs-lookup"><span data-stu-id="90f52-185">Replace everything in this file with the following:</span></span>

[!code-html[Main](tutorial-your-first-web-api/samples/sample3.html)]

<span data-ttu-id="90f52-186">Istnieje kilka sposobów uzyskania jQuery.</span><span class="sxs-lookup"><span data-stu-id="90f52-186">There are several ways to get jQuery.</span></span> <span data-ttu-id="90f52-187">W tym przykładzie używany [Microsoft Ajax CDN](../../../ajax/cdn/overview.md).</span><span class="sxs-lookup"><span data-stu-id="90f52-187">In this example, I used the [Microsoft Ajax CDN](../../../ajax/cdn/overview.md).</span></span> <span data-ttu-id="90f52-188">Można również pobrać go z [ http://jquery.com/ ](http://jquery.com/)i programu ASP.NET "Interfejs API sieci Web" szablon projektu obejmuje także jQuery.</span><span class="sxs-lookup"><span data-stu-id="90f52-188">You can also download it from [http://jquery.com/](http://jquery.com/), and the ASP.NET "Web API" project template includes jQuery as well.</span></span>

### <a name="getting-a-list-of-products"></a><span data-ttu-id="90f52-189">Pobieranie listy produktów</span><span class="sxs-lookup"><span data-stu-id="90f52-189">Getting a List of Products</span></span>

<span data-ttu-id="90f52-190">Aby uzyskać listę produktów, Wyślij żądanie HTTP GET do &quot;/api/produktów&quot;.</span><span class="sxs-lookup"><span data-stu-id="90f52-190">To get a list of products, send an HTTP GET request to &quot;/api/products&quot;.</span></span>

<span data-ttu-id="90f52-191">JQuery [getJSON](http://api.jquery.com/jQuery.getJSON/) funkcja wysyła żądanie AJAX.</span><span class="sxs-lookup"><span data-stu-id="90f52-191">The jQuery [getJSON](http://api.jquery.com/jQuery.getJSON/) function sends an AJAX request.</span></span> <span data-ttu-id="90f52-192">Odpowiedź zawiera tablicę obiektów JSON.</span><span class="sxs-lookup"><span data-stu-id="90f52-192">For response contains array of JSON objects.</span></span> <span data-ttu-id="90f52-193">`done` Określa funkcja wywołania zwrotnego, która jest wywołana, jeśli żądanie zakończy się pomyślnie.</span><span class="sxs-lookup"><span data-stu-id="90f52-193">The `done` function specifies a callback that is called if the request succeeds.</span></span> <span data-ttu-id="90f52-194">Wywołanie zwrotne firma Microsoft aktualizuje modelu DOM przy użyciu informacji o produkcie.</span><span class="sxs-lookup"><span data-stu-id="90f52-194">In the callback, we update the DOM with the product information.</span></span>

[!code-html[Main](tutorial-your-first-web-api/samples/sample4.html)]

### <a name="getting-a-product-by-id"></a><span data-ttu-id="90f52-195">Pobieranie produktu według Identyfikatora</span><span class="sxs-lookup"><span data-stu-id="90f52-195">Getting a Product By ID</span></span>

<span data-ttu-id="90f52-196">Aby uzyskać produkt za pomocą Identyfikatora, Wyślij żądanie HTTP GET do &quot;/interfejs API/produkty/*identyfikator*&quot;, gdzie *identyfikator* jest identyfikator produktu.</span><span class="sxs-lookup"><span data-stu-id="90f52-196">To get a product by ID, send an HTTP GET request to &quot;/api/products/*id*&quot;, where *id* is the product ID.</span></span>

[!code-javascript[Main](tutorial-your-first-web-api/samples/sample5.js)]

<span data-ttu-id="90f52-197">Firma Microsoft wciąż wywołać `getJSON` Aby wysłać żądanie AJAX, ale tym razem utworzona Państwu identyfikator w identyfikatorze URI żądania.</span><span class="sxs-lookup"><span data-stu-id="90f52-197">We still call `getJSON` to send the AJAX request, but this time we put the ID in the request URI.</span></span> <span data-ttu-id="90f52-198">Odpowiedź z tym żądaniem jest reprezentacja JSON jednego produktu.</span><span class="sxs-lookup"><span data-stu-id="90f52-198">The response from this request is a JSON representation of a single product.</span></span>

## <a name="running-the-application"></a><span data-ttu-id="90f52-199">Uruchamianie aplikacji</span><span class="sxs-lookup"><span data-stu-id="90f52-199">Running the Application</span></span>

<span data-ttu-id="90f52-200">Naciśnij klawisz F5, aby rozpocząć debugowanie aplikacji.</span><span class="sxs-lookup"><span data-stu-id="90f52-200">Press F5 to start debugging the application.</span></span> <span data-ttu-id="90f52-201">Strony sieci web powinna wyglądać następująco:</span><span class="sxs-lookup"><span data-stu-id="90f52-201">The web page should look like the following:</span></span>

![](tutorial-your-first-web-api/_static/image11.png)

<span data-ttu-id="90f52-202">Aby uzyskać produkt za pomocą Identyfikatora, wprowadź identyfikator, a następnie kliknij przycisk wyszukiwania:</span><span class="sxs-lookup"><span data-stu-id="90f52-202">To get a product by ID, enter the ID and click Search:</span></span>

![](tutorial-your-first-web-api/_static/image12.png)

<span data-ttu-id="90f52-203">Jeśli wprowadzisz nieprawidłowy identyfikator, serwer zwraca błąd HTTP:</span><span class="sxs-lookup"><span data-stu-id="90f52-203">If you enter an invalid ID, the server returns an HTTP error:</span></span>

![](tutorial-your-first-web-api/_static/image13.png)

## <a name="using-f12-to-view-the-http-request-and-response"></a><span data-ttu-id="90f52-204">Za pomocą F12, aby wyświetlić żądania HTTP i odpowiedzi</span><span class="sxs-lookup"><span data-stu-id="90f52-204">Using F12 to View the HTTP Request and Response</span></span>

<span data-ttu-id="90f52-205">Podczas pracy z usługą HTTP może być bardzo przydatne zobaczyć żądania HTTP i żądania wiadomości.</span><span class="sxs-lookup"><span data-stu-id="90f52-205">When you are working with an HTTP service, it can be very useful to see the HTTP request and request messages.</span></span> <span data-ttu-id="90f52-206">Można to zrobić przy użyciu narzędzi deweloperskich F12 w programie Internet Explorer 9.</span><span class="sxs-lookup"><span data-stu-id="90f52-206">You can do this by using the F12 developer tools in Internet Explorer 9.</span></span> <span data-ttu-id="90f52-207">Z programu Internet Explorer 9, naciśnij klawisz **F12** można otworzyć narzędzia.</span><span class="sxs-lookup"><span data-stu-id="90f52-207">From Internet Explorer 9, press **F12** to open the tools.</span></span> <span data-ttu-id="90f52-208">Kliknij przycisk **sieci** kartę, a następnie naciśnij klawisz **Rozpocznij przechwytywanie**.</span><span class="sxs-lookup"><span data-stu-id="90f52-208">Click the **Network** tab and press **Start Capturing**.</span></span> <span data-ttu-id="90f52-209">Teraz wróć do strony sieci web i naciśnij klawisz **F5** ponowne załadowanie strony sieci web.</span><span class="sxs-lookup"><span data-stu-id="90f52-209">Now go back to the web page and press **F5** to reload the web page.</span></span> <span data-ttu-id="90f52-210">Program Internet Explorer będzie przechwytywać ruch HTTP między przeglądarką a serwerem sieci web.</span><span class="sxs-lookup"><span data-stu-id="90f52-210">Internet Explorer will capture the HTTP traffic between the browser and the web server.</span></span> <span data-ttu-id="90f52-211">Widok podsumowania przedstawia cały ruch sieciowy dla strony:</span><span class="sxs-lookup"><span data-stu-id="90f52-211">The summary view shows all the network traffic for a page:</span></span>

![](tutorial-your-first-web-api/_static/image14.png)

<span data-ttu-id="90f52-212">Zlokalizuj wpis dla względny identyfikator URI "interfejsu api/produkty /".</span><span class="sxs-lookup"><span data-stu-id="90f52-212">Locate the entry for the relative URI "api/products/".</span></span> <span data-ttu-id="90f52-213">Wybierz ten wpis, a następnie kliknij przycisk **przejdź do widoku szczegółowym**.</span><span class="sxs-lookup"><span data-stu-id="90f52-213">Select this entry and click **Go to detailed view**.</span></span> <span data-ttu-id="90f52-214">W widoku szczegółów istnieją karty, aby wyświetlić żądanie i odpowiedź nagłówki i treść.</span><span class="sxs-lookup"><span data-stu-id="90f52-214">In the detail view, there are tabs to view the request and response headers and bodies.</span></span> <span data-ttu-id="90f52-215">Na przykład jeśli klikniesz **nagłówki żądań** karcie widać, że klient zażądał &quot;application/json&quot; w nagłówku Accept.</span><span class="sxs-lookup"><span data-stu-id="90f52-215">For example, if you click the **Request headers** tab, you can see that the client requested &quot;application/json&quot; in the Accept header.</span></span>

![](tutorial-your-first-web-api/_static/image15.png)

<span data-ttu-id="90f52-216">Jeśli klikniesz kartę treść odpowiedzi, zostanie wyświetlony, jak lista produktów został wydany do formatu JSON.</span><span class="sxs-lookup"><span data-stu-id="90f52-216">If you click the Response body tab, you can see how the product list was serialized to JSON.</span></span> <span data-ttu-id="90f52-217">Inne przeglądarki mają podobne funkcje.</span><span class="sxs-lookup"><span data-stu-id="90f52-217">Other browsers have similar functionality.</span></span> <span data-ttu-id="90f52-218">Inne przydatne narzędzie jest [Fiddler](http://www.fiddler2.com/fiddler2/), internetowy serwer proxy debugowania.</span><span class="sxs-lookup"><span data-stu-id="90f52-218">Another useful tool is [Fiddler](http://www.fiddler2.com/fiddler2/), a web debugging proxy.</span></span> <span data-ttu-id="90f52-219">Służy narzędzia Fiddler do wyświetlania ruchu HTTP, a także do tworzenia żądań HTTP, co daje pełną kontrolę nad nagłówków HTTP żądania.</span><span class="sxs-lookup"><span data-stu-id="90f52-219">You can use Fiddler to view your HTTP traffic, and also to compose HTTP requests, which gives you full control over the HTTP headers in the request.</span></span>

## <a name="see-this-app-running-on-azure"></a><span data-ttu-id="90f52-220">Zobacz tej aplikacji działających na platformie Azure</span><span class="sxs-lookup"><span data-stu-id="90f52-220">See this App Running on Azure</span></span>

<span data-ttu-id="90f52-221">Czy chcesz w witrynie Zakończono działającego jako aplikacja sieci web?</span><span class="sxs-lookup"><span data-stu-id="90f52-221">Would you like to see the finished site running as a live web app?</span></span> <span data-ttu-id="90f52-222">Pełną wersję aplikacji można wdrożyć do konta platformy Azure, po prostu kliknąć poniższy przycisk.</span><span class="sxs-lookup"><span data-stu-id="90f52-222">You can deploy a complete version of the app to your Azure account by simply clicking the following button.</span></span>

[![](http://azuredeploy.net/deploybutton.png)](https://deploy.azure.com/?WT.mc_id=deploy_azure_aspnet&repository=https://github.com/tfitzmac/WebAPI-ProductsApp#/form/setup)

<span data-ttu-id="90f52-223">Potrzebujesz konta platformy Azure, aby wdrożyć to rozwiązanie na platformie Azure.</span><span class="sxs-lookup"><span data-stu-id="90f52-223">You need an Azure account to deploy this solution to Azure.</span></span> <span data-ttu-id="90f52-224">Jeśli nie masz już konto, masz następujące opcje:</span><span class="sxs-lookup"><span data-stu-id="90f52-224">If you do not already have an account, you have the following options:</span></span>

- <span data-ttu-id="90f52-225">[Otworzyć bezpłatne konto platformy Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A443DD604) — otrzymasz kredyt służy do wypróbowania płatnych usług platformy Azure, a nawet w przypadku, po ich wyczerpaniu nawet możesz zachować konto i korzystać z bezpłatnych usług platformy Azure.</span><span class="sxs-lookup"><span data-stu-id="90f52-225">[Open an Azure account for free](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A443DD604) - You get credits you can use to try out paid Azure services, and even after they're used up you can keep the account and use free Azure services.</span></span>
- <span data-ttu-id="90f52-226">[Aktywować korzyści dla subskrybentów MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604) -ramach subskrypcji MSDN daje środki na korzystanie z każdego miesiąca, używanego do płatne usługi platformy Azure.</span><span class="sxs-lookup"><span data-stu-id="90f52-226">[Activate MSDN subscriber benefits](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604) - Your MSDN subscription gives you credits every month that you can use for paid Azure services.</span></span>

## <a name="next-steps"></a><span data-ttu-id="90f52-227">Następne kroki</span><span class="sxs-lookup"><span data-stu-id="90f52-227">Next Steps</span></span>

- <span data-ttu-id="90f52-228">Aby uzyskać bardziej szczegółowy przykład usługi HTTP, która obsługuje operacje POST, PUT i DELETE i zapisuje je do bazy danych, zobacz [przy użyciu Web API 2 przy użyciu platformy Entity Framework 6](../data/using-web-api-with-entity-framework/part-1.md).</span><span class="sxs-lookup"><span data-stu-id="90f52-228">For a more complete example of an HTTP service that supports POST, PUT, and DELETE actions and writes to a database, see [Using Web API 2 with Entity Framework 6](../data/using-web-api-with-entity-framework/part-1.md).</span></span>
- <span data-ttu-id="90f52-229">Aby uzyskać więcej informacji o tworzeniu aplikacji sieci web płynne i szybkie działanie na podstawie usługi HTTP, zobacz [aplikacji jednostronicowej ASP.NET](../../../single-page-application/index.md).</span><span class="sxs-lookup"><span data-stu-id="90f52-229">For more about creating fluid and responsive web applications on top of an HTTP service, see [ASP.NET Single Page Application](../../../single-page-application/index.md).</span></span>
- <span data-ttu-id="90f52-230">Aby uzyskać informacje o sposobie wdrażania projektu sieci web programu Visual Studio w usłudze Azure App Service, zobacz [tworzenie aplikacji sieci web platformy ASP.NET w usłudze Azure App Service](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-get-started/).</span><span class="sxs-lookup"><span data-stu-id="90f52-230">For information about how to deploy a Visual Studio web project to Azure App Service, see [Create an ASP.NET web app in Azure App Service](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-get-started/).</span></span>
