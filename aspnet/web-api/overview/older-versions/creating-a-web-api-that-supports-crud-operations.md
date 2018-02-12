---
uid: web-api/overview/older-versions/creating-a-web-api-that-supports-crud-operations
title: "Włączanie operacji CRUD w składniku ASP.NET Web API 1 | Dokumentacja firmy Microsoft"
author: MikeWasson
description: "W tym samouczku przedstawiono sposób obsługi operacji CRUD usługi HTTP przy użyciu interfejsu API sieci Web platformy ASP.NET. Używane w samouczek Visual Studio 2012 Web region wersje oprogramowania..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/28/2012
ms.topic: article
ms.assetid: c125ca47-606a-4d6f-a1fc-1fc62928af93
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/older-versions/creating-a-web-api-that-supports-crud-operations
msc.type: authoredcontent
ms.openlocfilehash: 69b7d5453b6ff36d6e28a69428b016cb8cfd06e9
ms.sourcegitcommit: 016f4d58663bcd442930227022de23fb3abee0b3
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/12/2018
---
<a name="enabling-crud-operations-in-aspnet-web-api-1"></a><span data-ttu-id="79d3b-104">Włączanie operacji CRUD w składniku ASP.NET Web API 1</span><span class="sxs-lookup"><span data-stu-id="79d3b-104">Enabling CRUD Operations in ASP.NET Web API 1</span></span>
====================
<span data-ttu-id="79d3b-105">przez [Wasson Jan](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="79d3b-105">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="79d3b-106">Pobieranie ukończone projektu</span><span class="sxs-lookup"><span data-stu-id="79d3b-106">Download Completed Project</span></span>](http://code.msdn.microsoft.com/ASP-NET-Web-API-Tutorial-c4761894)

> <span data-ttu-id="79d3b-107">W tym samouczku przedstawiono sposób obsługi operacji CRUD usługi HTTP przy użyciu interfejsu API sieci Web platformy ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="79d3b-107">This tutorial shows how to support CRUD operations in an HTTP service using ASP.NET Web API.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="79d3b-108">Używane w samouczku wersje oprogramowania</span><span class="sxs-lookup"><span data-stu-id="79d3b-108">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="79d3b-109">Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="79d3b-109">Visual Studio 2012</span></span>
> - <span data-ttu-id="79d3b-110">Składnik Web API 1 (dotyczy również 2 interfejsu API sieci Web)</span><span class="sxs-lookup"><span data-stu-id="79d3b-110">Web API 1 (also works with Web API 2)</span></span>


<span data-ttu-id="79d3b-111">Oznacza CRUD &quot;tworzenia, odczytu, aktualizacji i usuwania,&quot; będące cztery operacje podstawowej bazy danych.</span><span class="sxs-lookup"><span data-stu-id="79d3b-111">CRUD stands for &quot;Create, Read, Update, and Delete,&quot; which are the four basic database operations.</span></span> <span data-ttu-id="79d3b-112">Wiele usług HTTP modelu również operacji CRUD za pośrednictwem interfejsu API REST przypominającej lub REST.</span><span class="sxs-lookup"><span data-stu-id="79d3b-112">Many HTTP services also model CRUD operations through REST or REST-like APIs.</span></span>

<span data-ttu-id="79d3b-113">W tym samouczku utworzysz bardzo proste składnika web API do zarządzania listą produktów.</span><span class="sxs-lookup"><span data-stu-id="79d3b-113">In this tutorial, you will build a very simple web API to manage a list of products.</span></span> <span data-ttu-id="79d3b-114">Każdy produkt będzie zawierać nazwę, ceny i kategorii (takich jak &quot;toys&quot; lub &quot;sprzętu&quot;), oraz identyfikatora produktu.</span><span class="sxs-lookup"><span data-stu-id="79d3b-114">Each product will contain a name, price, and category (such as &quot;toys&quot; or &quot;hardware&quot;), plus a product ID.</span></span>

<span data-ttu-id="79d3b-115">Powoduje to udostępnienie następujących metod produktów interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="79d3b-115">The products API will expose following methods.</span></span>

| <span data-ttu-id="79d3b-116">Akcja</span><span class="sxs-lookup"><span data-stu-id="79d3b-116">Action</span></span> | <span data-ttu-id="79d3b-117">Metoda HTTP</span><span class="sxs-lookup"><span data-stu-id="79d3b-117">HTTP method</span></span> | <span data-ttu-id="79d3b-118">Względny identyfikator URI</span><span class="sxs-lookup"><span data-stu-id="79d3b-118">Relative URI</span></span> |
| --- | --- | --- |
| <span data-ttu-id="79d3b-119">Pobranie listy wszystkich produktów</span><span class="sxs-lookup"><span data-stu-id="79d3b-119">Get a list of all products</span></span> | <span data-ttu-id="79d3b-120">GET</span><span class="sxs-lookup"><span data-stu-id="79d3b-120">GET</span></span> | <span data-ttu-id="79d3b-121">/ api/produktów</span><span class="sxs-lookup"><span data-stu-id="79d3b-121">/api/products</span></span> |
| <span data-ttu-id="79d3b-122">Uzyskiwanie produktu według Identyfikatora</span><span class="sxs-lookup"><span data-stu-id="79d3b-122">Get a product by ID</span></span> | <span data-ttu-id="79d3b-123">GET</span><span class="sxs-lookup"><span data-stu-id="79d3b-123">GET</span></span> | <span data-ttu-id="79d3b-124">/API/produkty/*id*</span><span class="sxs-lookup"><span data-stu-id="79d3b-124">/api/products/*id*</span></span> |
| <span data-ttu-id="79d3b-125">Uzyskiwanie produktu według kategorii</span><span class="sxs-lookup"><span data-stu-id="79d3b-125">Get a product by category</span></span> | <span data-ttu-id="79d3b-126">GET</span><span class="sxs-lookup"><span data-stu-id="79d3b-126">GET</span></span> | <span data-ttu-id="79d3b-127">produkty/api /? kategorii =*kategorii*</span><span class="sxs-lookup"><span data-stu-id="79d3b-127">/api/products?category=*category*</span></span> |
| <span data-ttu-id="79d3b-128">Tworzenie nowego produktu</span><span class="sxs-lookup"><span data-stu-id="79d3b-128">Create a new product</span></span> | <span data-ttu-id="79d3b-129">POST</span><span class="sxs-lookup"><span data-stu-id="79d3b-129">POST</span></span> | <span data-ttu-id="79d3b-130">/ api/produktów</span><span class="sxs-lookup"><span data-stu-id="79d3b-130">/api/products</span></span> |
| <span data-ttu-id="79d3b-131">Aktualizacji produktu</span><span class="sxs-lookup"><span data-stu-id="79d3b-131">Update a product</span></span> | <span data-ttu-id="79d3b-132">UMIEŚĆ</span><span class="sxs-lookup"><span data-stu-id="79d3b-132">PUT</span></span> | <span data-ttu-id="79d3b-133">/API/produkty/*id*</span><span class="sxs-lookup"><span data-stu-id="79d3b-133">/api/products/*id*</span></span> |
| <span data-ttu-id="79d3b-134">Usuwanie produktu</span><span class="sxs-lookup"><span data-stu-id="79d3b-134">Delete a product</span></span> | <span data-ttu-id="79d3b-135">DELETE</span><span class="sxs-lookup"><span data-stu-id="79d3b-135">DELETE</span></span> | <span data-ttu-id="79d3b-136">/API/produkty/*id*</span><span class="sxs-lookup"><span data-stu-id="79d3b-136">/api/products/*id*</span></span> |

<span data-ttu-id="79d3b-137">Zwróć uwagę, niektóre identyfikatory URI są identyfikator produktu w ścieżce.</span><span class="sxs-lookup"><span data-stu-id="79d3b-137">Notice that some of the URIs include the product ID in path.</span></span> <span data-ttu-id="79d3b-138">Na przykład, aby uzyskać produktu o identyfikatorze jest 28, klient wysyła żądanie GET `http://hostname/api/products/28`.</span><span class="sxs-lookup"><span data-stu-id="79d3b-138">For example, to get the product whose ID is 28, the client sends a GET request for `http://hostname/api/products/28`.</span></span>

### <a name="resources"></a><span data-ttu-id="79d3b-139">Resources</span><span class="sxs-lookup"><span data-stu-id="79d3b-139">Resources</span></span>

<span data-ttu-id="79d3b-140">Produkty interfejsu API definiuje identyfikatorów URI dla dwa typy zasobów:</span><span class="sxs-lookup"><span data-stu-id="79d3b-140">The products API defines URIs for two resource types:</span></span>

| <span data-ttu-id="79d3b-141">Zasób</span><span class="sxs-lookup"><span data-stu-id="79d3b-141">Resource</span></span> | <span data-ttu-id="79d3b-142">Identyfikator URI</span><span class="sxs-lookup"><span data-stu-id="79d3b-142">URI</span></span> |
| --- | --- |
| <span data-ttu-id="79d3b-143">Lista wszystkich produktów.</span><span class="sxs-lookup"><span data-stu-id="79d3b-143">The list of all the products.</span></span> | <span data-ttu-id="79d3b-144">/ api/produktów</span><span class="sxs-lookup"><span data-stu-id="79d3b-144">/api/products</span></span> |
| <span data-ttu-id="79d3b-145">Indywidualnych produktów.</span><span class="sxs-lookup"><span data-stu-id="79d3b-145">An individual product.</span></span> | <span data-ttu-id="79d3b-146">/API/produkty/*id*</span><span class="sxs-lookup"><span data-stu-id="79d3b-146">/api/products/*id*</span></span> |

### <a name="methods"></a><span data-ttu-id="79d3b-147">Metody</span><span class="sxs-lookup"><span data-stu-id="79d3b-147">Methods</span></span>

<span data-ttu-id="79d3b-148">Cztery główne metody HTTP (GET, PUT, POST i DELETE) mogą być mapowane na operacje CRUD w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="79d3b-148">The four main HTTP methods (GET, PUT, POST, and DELETE) can be mapped to CRUD operations as follows:</span></span>

- <span data-ttu-id="79d3b-149">GET pobiera reprezentacja zasobu określonego identyfikatora URI.</span><span class="sxs-lookup"><span data-stu-id="79d3b-149">GET retrieves the representation of the resource at a specified URI.</span></span> <span data-ttu-id="79d3b-150">GET, powinien mieć żadnych efektów ubocznych na serwerze.</span><span class="sxs-lookup"><span data-stu-id="79d3b-150">GET should have no side effects on the server.</span></span>
- <span data-ttu-id="79d3b-151">Umieść aktualizuje zasób o określonym identyfikatorze URI.</span><span class="sxs-lookup"><span data-stu-id="79d3b-151">PUT updates a resource at a specified URI.</span></span> <span data-ttu-id="79d3b-152">PUT można również utworzyć nowy zasób o określonym identyfikatorze URI, jeśli serwer umożliwia klientom określ nowy identyfikator URI.</span><span class="sxs-lookup"><span data-stu-id="79d3b-152">PUT can also be used to create a new resource at a specified URI, if the server allows clients to specify new URIs.</span></span> <span data-ttu-id="79d3b-153">W tym samouczku interfejs API nie obsługuje tworzenia za pomocą PUT.</span><span class="sxs-lookup"><span data-stu-id="79d3b-153">For this tutorial, the API will not support creation through PUT.</span></span>
- <span data-ttu-id="79d3b-154">POST tworzy nowy zasób.</span><span class="sxs-lookup"><span data-stu-id="79d3b-154">POST creates a new resource.</span></span> <span data-ttu-id="79d3b-155">Serwer przypisuje identyfikator URI dla nowego obiektu i zwraca ten identyfikator URI jako część komunikatu odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="79d3b-155">The server assigns the URI for the new object and returns this URI as part of the response message.</span></span>
- <span data-ttu-id="79d3b-156">Usuń Usuwa zasób o określonym identyfikatorze URI.</span><span class="sxs-lookup"><span data-stu-id="79d3b-156">DELETE deletes a resource at a specified URI.</span></span>

<span data-ttu-id="79d3b-157">Uwaga: Metody PUT zastępuje jednostki całego produktu.</span><span class="sxs-lookup"><span data-stu-id="79d3b-157">Note: The PUT method replaces the entire product entity.</span></span> <span data-ttu-id="79d3b-158">Oznacza to klient powinien wysłać pełną reprezentację zaktualizowane produktu.</span><span class="sxs-lookup"><span data-stu-id="79d3b-158">That is, the client is expected to send a complete representation of the updated product.</span></span> <span data-ttu-id="79d3b-159">Jeśli chcesz obsługiwać aktualizacje częściowe metoda poprawki jest zalecana.</span><span class="sxs-lookup"><span data-stu-id="79d3b-159">If you want to support partial updates, the PATCH method is preferred.</span></span> <span data-ttu-id="79d3b-160">W tym samouczku nie implementuje poprawki.</span><span class="sxs-lookup"><span data-stu-id="79d3b-160">This tutorial does not implement PATCH.</span></span>

## <a name="create-a-new-web-api-project"></a><span data-ttu-id="79d3b-161">Utwórz nowy projekt interfejsu API sieci Web</span><span class="sxs-lookup"><span data-stu-id="79d3b-161">Create a New Web API Project</span></span>

<span data-ttu-id="79d3b-162">Zacznij od uruchomienia programu Visual Studio i wybierz **nowy projekt** z **Start** strony.</span><span class="sxs-lookup"><span data-stu-id="79d3b-162">Start by running Visual Studio and select **New Project** from the **Start** page.</span></span> <span data-ttu-id="79d3b-163">Lub z **pliku** menu, wybierz opcję **nowy** , a następnie **projektu**.</span><span class="sxs-lookup"><span data-stu-id="79d3b-163">Or, from the **File** menu, select **New** and then **Project**.</span></span>

<span data-ttu-id="79d3b-164">W **szablony** okienku wybierz **zainstalowane szablony** i rozwiń **Visual C#** węzła.</span><span class="sxs-lookup"><span data-stu-id="79d3b-164">In the **Templates** pane, select **Installed Templates** and expand the **Visual C#** node.</span></span> <span data-ttu-id="79d3b-165">W obszarze **Visual C#**, wybierz pozycję **Web**.</span><span class="sxs-lookup"><span data-stu-id="79d3b-165">Under **Visual C#**, select **Web**.</span></span> <span data-ttu-id="79d3b-166">Na liście szablony projektów, wybierz **aplikacji sieci Web programu ASP.NET MVC 4**.</span><span class="sxs-lookup"><span data-stu-id="79d3b-166">In the list of project templates, select **ASP.NET MVC 4 Web Application**.</span></span> <span data-ttu-id="79d3b-167">Nazwij projekt &quot;ProductStore&quot; i kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="79d3b-167">Name the project &quot;ProductStore&quot; and click **OK**.</span></span>

![](creating-a-web-api-that-supports-crud-operations/_static/image1.png)

<span data-ttu-id="79d3b-168">W **nowy projekt programu ASP.NET MVC 4** okno dialogowe, wybierz opcję **interfejsu API sieci Web** i kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="79d3b-168">In the **New ASP.NET MVC 4 Project** dialog, select **Web API** and click **OK**.</span></span>

![](creating-a-web-api-that-supports-crud-operations/_static/image2.png)

## <a name="adding-a-model"></a><span data-ttu-id="79d3b-169">Dodawanie modelu</span><span class="sxs-lookup"><span data-stu-id="79d3b-169">Adding a Model</span></span>

<span data-ttu-id="79d3b-170">A *modelu* jest obiekt, który reprezentuje dane w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="79d3b-170">A *model* is an object that represents the data in your application.</span></span> <span data-ttu-id="79d3b-171">W interfejsie API sieci Web ASP.NET silnie typizowanych obiektów CLR można użyć jako modele i one będą automatycznie wykonywane szeregowo XML lub JSON dla klienta.</span><span class="sxs-lookup"><span data-stu-id="79d3b-171">In ASP.NET Web API, you can use strongly typed CLR objects as models, and they will automatically be serialized to XML or JSON for the client.</span></span>

<span data-ttu-id="79d3b-172">ProductStore API naszych danych składa się z produktów, dlatego utworzymy nową klasę o nazwie `Product`.</span><span class="sxs-lookup"><span data-stu-id="79d3b-172">For the ProductStore API, our data consists of products, so we'll create a new class named `Product`.</span></span>

<span data-ttu-id="79d3b-173">Jeśli w Eksploratorze rozwiązań nie jest widoczny, kliknij przycisk **widoku** menu i wybierz **Eksploratora rozwiązań**.</span><span class="sxs-lookup"><span data-stu-id="79d3b-173">If Solution Explorer is not already visible, click the **View** menu and select **Solution Explorer**.</span></span> <span data-ttu-id="79d3b-174">W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy **modele** folderu.</span><span class="sxs-lookup"><span data-stu-id="79d3b-174">In Solution Explorer, right-click the **Models** folder.</span></span> <span data-ttu-id="79d3b-175">Wybierz z meny kontekstu **Dodaj**, a następnie wybierz pozycję **klasy**.</span><span class="sxs-lookup"><span data-stu-id="79d3b-175">From the context meny, select **Add**, then select **Class**.</span></span> <span data-ttu-id="79d3b-176">Nazwa klasy &quot;produktu&quot;.</span><span class="sxs-lookup"><span data-stu-id="79d3b-176">Name the class &quot;Product&quot;.</span></span>

![](creating-a-web-api-that-supports-crud-operations/_static/image3.png)

<span data-ttu-id="79d3b-177">Dodaj następujące właściwości `Product` klasy.</span><span class="sxs-lookup"><span data-stu-id="79d3b-177">Add the following properties to the `Product` class.</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample1.cs)]

## <a name="adding-a-repository"></a><span data-ttu-id="79d3b-178">Dodawanie repozytorium</span><span class="sxs-lookup"><span data-stu-id="79d3b-178">Adding a Repository</span></span>

<span data-ttu-id="79d3b-179">Musimy przechowywanie kolekcji produktów.</span><span class="sxs-lookup"><span data-stu-id="79d3b-179">We need to store a collection of products.</span></span> <span data-ttu-id="79d3b-180">Jest dobrym pomysłem jest oddzielnych kolekcji z naszych implementacji usługi.</span><span class="sxs-lookup"><span data-stu-id="79d3b-180">It's a good idea to separate the collection from our service implementation.</span></span> <span data-ttu-id="79d3b-181">W ten sposób możemy zmienić magazynu zapasowego bez ponownego tworzenia klasy usługi.</span><span class="sxs-lookup"><span data-stu-id="79d3b-181">That way, we can change the backing store without rewriting the service class.</span></span> <span data-ttu-id="79d3b-182">Ten typ projektu jest nazywany *repozytorium* wzorca.</span><span class="sxs-lookup"><span data-stu-id="79d3b-182">This type of design is called the *repository* pattern.</span></span> <span data-ttu-id="79d3b-183">Rozpocznij od zdefiniowania ogólny interfejs umożliwiający repozytorium.</span><span class="sxs-lookup"><span data-stu-id="79d3b-183">Start by defining a generic interface for the repository.</span></span>

<span data-ttu-id="79d3b-184">W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy **modele** folderu.</span><span class="sxs-lookup"><span data-stu-id="79d3b-184">In Solution Explorer, right-click the **Models** folder.</span></span> <span data-ttu-id="79d3b-185">Wybierz **Dodaj**, a następnie wybierz pozycję **nowy element**.</span><span class="sxs-lookup"><span data-stu-id="79d3b-185">Select **Add**, then select **New Item**.</span></span>

![](creating-a-web-api-that-supports-crud-operations/_static/image4.png)

<span data-ttu-id="79d3b-186">W **szablony** okienku wybierz **zainstalowane szablony** i rozwiń węzeł C#.</span><span class="sxs-lookup"><span data-stu-id="79d3b-186">In the **Templates** pane, select **Installed Templates** and expand the C# node.</span></span> <span data-ttu-id="79d3b-187">W obszarze C#, wybierz **kod**.</span><span class="sxs-lookup"><span data-stu-id="79d3b-187">Under C#, select **Code**.</span></span> <span data-ttu-id="79d3b-188">Na liście szablony kodu, wybierz **interfejsu**.</span><span class="sxs-lookup"><span data-stu-id="79d3b-188">In the list of code templates, select **Interface**.</span></span> <span data-ttu-id="79d3b-189">Nazwa interfejsu &quot;IProductRepository&quot;.</span><span class="sxs-lookup"><span data-stu-id="79d3b-189">Name the interface &quot;IProductRepository&quot;.</span></span>

![](creating-a-web-api-that-supports-crud-operations/_static/image5.png)

<span data-ttu-id="79d3b-190">Dodaj następujące wdrożenia:</span><span class="sxs-lookup"><span data-stu-id="79d3b-190">Add the following implementation:</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample2.cs)]

<span data-ttu-id="79d3b-191">Teraz Dodaj kolejną klasę do folderu modeli o nazwie &quot;ProductRepository.&quot; Ta klasa będzie implementowany `IProductRespository` interfejsu.</span><span class="sxs-lookup"><span data-stu-id="79d3b-191">Now add another class to the Models folder, named &quot;ProductRepository.&quot; This class will implement the `IProductRespository` interface.</span></span> <span data-ttu-id="79d3b-192">Dodaj następujące wdrożenia:</span><span class="sxs-lookup"><span data-stu-id="79d3b-192">Add the following implementation:</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample3.cs)]

<span data-ttu-id="79d3b-193">Repozytorium przechowuje listy w pamięci lokalnej.</span><span class="sxs-lookup"><span data-stu-id="79d3b-193">The repository keeps the list in local memory.</span></span> <span data-ttu-id="79d3b-194">Jest to samouczek, ale w rzeczywistej aplikacji będzie przechowywać dane zewnętrznie, albo bazy danych lub w magazynie w chmurze.</span><span class="sxs-lookup"><span data-stu-id="79d3b-194">This is OK for a tutorial, but in a real application, you would store the data externally, either a database or in cloud storage.</span></span> <span data-ttu-id="79d3b-195">Wzorzec repozytorium ułatwi Zmień implementację później.</span><span class="sxs-lookup"><span data-stu-id="79d3b-195">The repository pattern will make it easier to change the implementation later.</span></span>

## <a name="adding-a-web-api-controller"></a><span data-ttu-id="79d3b-196">Dodawanie kontrolera interfejsu API sieci Web</span><span class="sxs-lookup"><span data-stu-id="79d3b-196">Adding a Web API Controller</span></span>

<span data-ttu-id="79d3b-197">Użytkownicy mający doświadczenie z platformą ASP.NET MVC, następnie znasz już kontrolerów.</span><span class="sxs-lookup"><span data-stu-id="79d3b-197">If you have worked with ASP.NET MVC, then you are already familiar with controllers.</span></span> <span data-ttu-id="79d3b-198">W interfejsie API sieci Web ASP.NET *kontrolera* jest klasa, która obsługuje żądania HTTP od klienta.</span><span class="sxs-lookup"><span data-stu-id="79d3b-198">In ASP.NET Web API, a *controller* is a class that handles HTTP requests from the client.</span></span> <span data-ttu-id="79d3b-199">Kreator nowego projektu utworzone dwa kontrolery automatycznie podczas tworzenia projektu.</span><span class="sxs-lookup"><span data-stu-id="79d3b-199">The New Project wizard created two controllers for you when it created the project.</span></span> <span data-ttu-id="79d3b-200">Aby je wyświetlić, rozwiń folder kontrolerów w Eksploratorze rozwiązań.</span><span class="sxs-lookup"><span data-stu-id="79d3b-200">To see them, expand the Controllers folder in Solution Explorer.</span></span>

- <span data-ttu-id="79d3b-201">HomeController jest tradycyjnych kontrolera ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="79d3b-201">HomeController is a traditional ASP.NET MVC controller.</span></span> <span data-ttu-id="79d3b-202">Jest odpowiedzialny za obsługująca stron HTML dla lokacji i nie jest bezpośrednio powiązana z naszych interfejsu API sieci web.</span><span class="sxs-lookup"><span data-stu-id="79d3b-202">It is responsible for serving HTML pages for the site, and is not directly related to our web API.</span></span>
- <span data-ttu-id="79d3b-203">ValuesController jest przykład WebAPI kontrolera.</span><span class="sxs-lookup"><span data-stu-id="79d3b-203">ValuesController is an example WebAPI controller.</span></span>

<span data-ttu-id="79d3b-204">Przejdź dalej i usunąć ValuesController, prawym przyciskiem myszy plik w Eksploratorze rozwiązań i wybierając **usunąć.**</span><span class="sxs-lookup"><span data-stu-id="79d3b-204">Go ahead and delete ValuesController, by right-clicking the file in Solution Explorer and selecting **Delete.**</span></span> <span data-ttu-id="79d3b-205">Teraz Dodaj nowy kontroler, w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="79d3b-205">Now add a new controller, as follows:</span></span>

<span data-ttu-id="79d3b-206">W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy folder kontrolery.</span><span class="sxs-lookup"><span data-stu-id="79d3b-206">In **Solution Explorer**, right-click the Controllers folder.</span></span> <span data-ttu-id="79d3b-207">Wybierz **Dodaj** , a następnie wybierz **kontrolera**.</span><span class="sxs-lookup"><span data-stu-id="79d3b-207">Select **Add** and then select **Controller**.</span></span>

![](creating-a-web-api-that-supports-crud-operations/_static/image6.png)

<span data-ttu-id="79d3b-208">W **Dodaj kontroler** kreatora, nazwy kontrolera &quot;ProductsController&quot;.</span><span class="sxs-lookup"><span data-stu-id="79d3b-208">In the **Add Controller** wizard, name the controller &quot;ProductsController&quot;.</span></span> <span data-ttu-id="79d3b-209">W **szablonu** listy rozwijanej wybierz **pusty Kontroler interfejsu API**.</span><span class="sxs-lookup"><span data-stu-id="79d3b-209">In the **Template** drop-down list, select **Empty API Controller**.</span></span> <span data-ttu-id="79d3b-210">Następnie kliknij przycisk **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="79d3b-210">Then click **Add**.</span></span>

![](creating-a-web-api-that-supports-crud-operations/_static/image7.png)

> [!NOTE]
> <span data-ttu-id="79d3b-211">Nie jest konieczne w celu uruchomienia programu contollers folder o nazwie kontrolerów.</span><span class="sxs-lookup"><span data-stu-id="79d3b-211">It is not necessary to put your contollers into a folder named Controllers.</span></span> <span data-ttu-id="79d3b-212">Nazwa folderu nie jest ważna; jest po prostu wygodny sposób organizowania plików źródłowych.</span><span class="sxs-lookup"><span data-stu-id="79d3b-212">The folder name is not important; it is simply a convenient way to organize your source files.</span></span>


<span data-ttu-id="79d3b-213">**Dodaj kontroler** Kreator utworzy plik o nazwie ProductsController.cs w folderze kontrolerów.</span><span class="sxs-lookup"><span data-stu-id="79d3b-213">The **Add Controller** wizard will create a file named ProductsController.cs in the Controllers folder.</span></span> <span data-ttu-id="79d3b-214">Jeśli ten plik nie jest jeszcze otwarty, kliknij dwukrotnie plik, aby go otworzyć.</span><span class="sxs-lookup"><span data-stu-id="79d3b-214">If this file is not open already, double-click the file to open it.</span></span> <span data-ttu-id="79d3b-215">Dodaj następujące **przy użyciu** instrukcji:</span><span class="sxs-lookup"><span data-stu-id="79d3b-215">Add the following **using** statement:</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample4.cs)]

<span data-ttu-id="79d3b-216">Dodaj pola zawierające **IProductRepository** wystąpienia.</span><span class="sxs-lookup"><span data-stu-id="79d3b-216">Add a field that holds an **IProductRepository** instance.</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample5.cs)]

> [!NOTE]
> <span data-ttu-id="79d3b-217">Wywoływanie `new ProductRepository()` w kontrolerze nie jest najlepszym projektu, ponieważ wiąże kontrolera w celu wykonania konkretnego `IProductRepository`.</span><span class="sxs-lookup"><span data-stu-id="79d3b-217">Calling `new ProductRepository()` in the controller is not the best design, because it ties the controller to a particular implementation of `IProductRepository`.</span></span> <span data-ttu-id="79d3b-218">Aby lepszym rozwiązaniem, zobacz [za pomocą mechanizmu rozpoznawania zależności dla interfejsu API sieci Web](../advanced/dependency-injection.md).</span><span class="sxs-lookup"><span data-stu-id="79d3b-218">For a better approach, see [Using the Web API Dependency Resolver](../advanced/dependency-injection.md).</span></span>


## <a name="getting-a-resource"></a><span data-ttu-id="79d3b-219">Pobieranie zasobu</span><span class="sxs-lookup"><span data-stu-id="79d3b-219">Getting a Resource</span></span>

<span data-ttu-id="79d3b-220">Interfejs API ProductStore uwidoczni kilka &quot;odczytu&quot; akcje jako metody HTTP GET.</span><span class="sxs-lookup"><span data-stu-id="79d3b-220">The ProductStore API will expose several &quot;read&quot; actions as HTTP GET methods.</span></span> <span data-ttu-id="79d3b-221">Każda akcja odpowiada metody w `ProductsController` klasy.</span><span class="sxs-lookup"><span data-stu-id="79d3b-221">Each action will correspond to a method in the `ProductsController` class.</span></span>

| <span data-ttu-id="79d3b-222">Akcja</span><span class="sxs-lookup"><span data-stu-id="79d3b-222">Action</span></span> | <span data-ttu-id="79d3b-223">Metoda HTTP</span><span class="sxs-lookup"><span data-stu-id="79d3b-223">HTTP method</span></span> | <span data-ttu-id="79d3b-224">Względny identyfikator URI</span><span class="sxs-lookup"><span data-stu-id="79d3b-224">Relative URI</span></span> |
| --- | --- | --- |
| <span data-ttu-id="79d3b-225">Pobranie listy wszystkich produktów</span><span class="sxs-lookup"><span data-stu-id="79d3b-225">Get a list of all products</span></span> | <span data-ttu-id="79d3b-226">GET</span><span class="sxs-lookup"><span data-stu-id="79d3b-226">GET</span></span> | <span data-ttu-id="79d3b-227">/ api/produktów</span><span class="sxs-lookup"><span data-stu-id="79d3b-227">/api/products</span></span> |
| <span data-ttu-id="79d3b-228">Uzyskiwanie produktu według Identyfikatora</span><span class="sxs-lookup"><span data-stu-id="79d3b-228">Get a product by ID</span></span> | <span data-ttu-id="79d3b-229">GET</span><span class="sxs-lookup"><span data-stu-id="79d3b-229">GET</span></span> | <span data-ttu-id="79d3b-230">/API/produkty/*id*</span><span class="sxs-lookup"><span data-stu-id="79d3b-230">/api/products/*id*</span></span> |
| <span data-ttu-id="79d3b-231">Uzyskiwanie produktu według kategorii</span><span class="sxs-lookup"><span data-stu-id="79d3b-231">Get a product by category</span></span> | <span data-ttu-id="79d3b-232">GET</span><span class="sxs-lookup"><span data-stu-id="79d3b-232">GET</span></span> | <span data-ttu-id="79d3b-233">produkty/api /? kategorii =*kategorii*</span><span class="sxs-lookup"><span data-stu-id="79d3b-233">/api/products?category=*category*</span></span> |

<span data-ttu-id="79d3b-234">Aby uzyskać listę wszystkich produktów, Dodaj tę metodę w celu `ProductsController` klasy:</span><span class="sxs-lookup"><span data-stu-id="79d3b-234">To get the list of all products, add this method to the `ProductsController` class:</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample6.cs)]

<span data-ttu-id="79d3b-235">Nazwa metody rozpoczyna się od &quot;uzyskać&quot;, więc Konwencja mapowania żądania GET.</span><span class="sxs-lookup"><span data-stu-id="79d3b-235">The method name starts with &quot;Get&quot;, so by convention it maps to GET requests.</span></span> <span data-ttu-id="79d3b-236">Ponadto, ponieważ metoda nie ma parametrów, mapowania identyfikatora URI, który nie zawiera  *&quot;identyfikator&quot;*  segment w ścieżce.</span><span class="sxs-lookup"><span data-stu-id="79d3b-236">Also, because the method has no parameters, it maps to a URI that does not contain an *&quot;id&quot;* segment in the path.</span></span>

<span data-ttu-id="79d3b-237">Uzyskanie produktu przez identyfikator, Dodaj tę metodę w celu `ProductsController` klasy:</span><span class="sxs-lookup"><span data-stu-id="79d3b-237">To get a product by ID, add this method to the `ProductsController` class:</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample7.cs)]

<span data-ttu-id="79d3b-238">Ta nazwa metody również rozpoczyna się od &quot;uzyskać&quot;, ale metoda ma parametr o nazwie *identyfikator*. Ten parametr jest mapowany na &quot;identyfikator&quot; segmentu ścieżki identyfikatora URI.</span><span class="sxs-lookup"><span data-stu-id="79d3b-238">This method name also starts with &quot;Get&quot;, but the method has a parameter named *id*. This parameter is mapped to the &quot;id&quot; segment of the URI path.</span></span> <span data-ttu-id="79d3b-239">Platformę ASP.NET Web API automatycznie konwertuje identyfikator prawidłowy typ danych (**int**) dla parametru.</span><span class="sxs-lookup"><span data-stu-id="79d3b-239">The ASP.NET Web API framework automatically converts the ID to the correct data type (**int**) for the parameter.</span></span>

<span data-ttu-id="79d3b-240">Metoda GetProduct zgłasza wyjątek typu **HttpResponseException** Jeśli *identyfikator* jest nieprawidłowy.</span><span class="sxs-lookup"><span data-stu-id="79d3b-240">The GetProduct method throws an exception of type **HttpResponseException** if *id* is not valid.</span></span> <span data-ttu-id="79d3b-241">Ten wyjątek zostanie zamieniona przez platformę na błąd 404 (nie znaleziono).</span><span class="sxs-lookup"><span data-stu-id="79d3b-241">This exception will be translated by the framework into a 404 (Not Found) error.</span></span>

<span data-ttu-id="79d3b-242">Na koniec należy dodać metody do znalezienia produktów według kategorii:</span><span class="sxs-lookup"><span data-stu-id="79d3b-242">Finally, add a method to find products by category:</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample8.cs)]

<span data-ttu-id="79d3b-243">Jeśli identyfikator URI żądania zawiera ciąg zapytania, interfejsu API sieci Web próbuje dopasować parametry na parametry dla metody kontrolera.</span><span class="sxs-lookup"><span data-stu-id="79d3b-243">If the request URI has a query string, Web API tries to match the query parameters to parameters on the controller method.</span></span> <span data-ttu-id="79d3b-244">W związku z tym identyfikatorem URI w postaci "interfejsu api/produktów? kategorii =*kategorii*" przypisze do tej metody.</span><span class="sxs-lookup"><span data-stu-id="79d3b-244">Therefore, a URI of the form "api/products?category=*category*" will map to this method.</span></span>

## <a name="creating-a-resource"></a><span data-ttu-id="79d3b-245">Tworzenie zasobu</span><span class="sxs-lookup"><span data-stu-id="79d3b-245">Creating a Resource</span></span>

<span data-ttu-id="79d3b-246">Następnie dodamy metodę `ProductsController` klasy w celu utworzenia nowego produktu.</span><span class="sxs-lookup"><span data-stu-id="79d3b-246">Next, we'll add a method to the `ProductsController` class to create a new product.</span></span> <span data-ttu-id="79d3b-247">Oto prosty implementacji metody:</span><span class="sxs-lookup"><span data-stu-id="79d3b-247">Here is a simple implementation of the method:</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample9.cs)]

<span data-ttu-id="79d3b-248">Należy uwzględnić dwa elementy o tej metody:</span><span class="sxs-lookup"><span data-stu-id="79d3b-248">Note two things about this method:</span></span>

- <span data-ttu-id="79d3b-249">Nazwa metody rozpoczyna się od &quot;Post... &quot;.</span><span class="sxs-lookup"><span data-stu-id="79d3b-249">The method name starts with &quot;Post...&quot;.</span></span> <span data-ttu-id="79d3b-250">Aby utworzyć nowy produkt, klient wysyła żądanie HTTP POST.</span><span class="sxs-lookup"><span data-stu-id="79d3b-250">To create a new product, the client sends an HTTP POST request.</span></span>
- <span data-ttu-id="79d3b-251">Metoda korzysta z parametru typu produkt.</span><span class="sxs-lookup"><span data-stu-id="79d3b-251">The method takes a parameter of type Product.</span></span> <span data-ttu-id="79d3b-252">W składniku Web API parametry o typach złożonych, są przeprowadzić deserializacji treści żądania.</span><span class="sxs-lookup"><span data-stu-id="79d3b-252">In Web API, parameters with complex types are deserialized from the request body.</span></span> <span data-ttu-id="79d3b-253">W związku z tym oczekujemy klientowi wysłanie zserializowana reprezentacja obiektu produktu w formacie XML lub JSON.</span><span class="sxs-lookup"><span data-stu-id="79d3b-253">Therefore, we expect the client to send a serialized representation of a product object, in either XML or JSON format.</span></span>

<span data-ttu-id="79d3b-254">Ta implementacja będzie działać, ale nie zostało jeszcze zakończone.</span><span class="sxs-lookup"><span data-stu-id="79d3b-254">This implementation will work, but it is not quite complete.</span></span> <span data-ttu-id="79d3b-255">Najlepiej, jeśli chcemy odpowiedzi HTTP są następujące:</span><span class="sxs-lookup"><span data-stu-id="79d3b-255">Ideally, we would like the HTTP response to include the following:</span></span>

- <span data-ttu-id="79d3b-256">**Kod odpowiedzi:** domyślnie przez strukturę interfejsu API sieci Web ustawia kod stanu odpowiedzi 200 (OK).</span><span class="sxs-lookup"><span data-stu-id="79d3b-256">**Response code:** By default, the Web API framework sets the response status code to 200 (OK).</span></span> <span data-ttu-id="79d3b-257">Jednak zgodnie z protokołu HTTP/1.1, gdy żądanie POST powoduje utworzenie zasobu, serwer powinien Odpowiedz, podając stanu 201 (utworzono).</span><span class="sxs-lookup"><span data-stu-id="79d3b-257">But according to the HTTP/1.1 protocol, when a POST request results in the creation of a resource, the server should reply with status 201 (Created).</span></span>
- <span data-ttu-id="79d3b-258">**Lokalizacja:** gdy serwer tworzy zasób, powinny zawierać identyfikatora URI nowego zasobu w nagłówku lokalizacji odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="79d3b-258">**Location:** When the server creates a resource, it should include the URI of the new resource in the Location header of the response.</span></span>

<span data-ttu-id="79d3b-259">ASP.NET Web API ułatwia manipulowania komunikat odpowiedzi HTTP.</span><span class="sxs-lookup"><span data-stu-id="79d3b-259">ASP.NET Web API makes it easy to manipulate the HTTP response message.</span></span> <span data-ttu-id="79d3b-260">Oto ulepszone implementacji:</span><span class="sxs-lookup"><span data-stu-id="79d3b-260">Here is the improved implementation:</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample10.cs)]

<span data-ttu-id="79d3b-261">Należy zauważyć, że typ zwracany metody jest teraz **HttpResponseMessage**.</span><span class="sxs-lookup"><span data-stu-id="79d3b-261">Notice that the method return type is now **HttpResponseMessage**.</span></span> <span data-ttu-id="79d3b-262">Zwracając **HttpResponseMessage** zamiast produktu, można sterować szczegóły komunikatu odpowiedzi HTTP, w tym kod stanu i nagłówek Location.</span><span class="sxs-lookup"><span data-stu-id="79d3b-262">By returning an **HttpResponseMessage** instead of a Product, we can control the details of the HTTP response message, including the status code and the Location header.</span></span>

<span data-ttu-id="79d3b-263">**CreateResponse** metoda tworzy **HttpResponseMessage** i automatycznie zapisuje zserializowana reprezentacja obiektu produktu w treści fo komunikat odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="79d3b-263">The **CreateResponse** method creates an **HttpResponseMessage** and automatically writes a serialized representation of the Product object into the body fo the response message.</span></span>

> [!NOTE]
> <span data-ttu-id="79d3b-264">W tym przykładzie nie można zweryfikować `Product`.</span><span class="sxs-lookup"><span data-stu-id="79d3b-264">This example does not validate the `Product`.</span></span> <span data-ttu-id="79d3b-265">Aby uzyskać informacje o weryfikacji modelu, zobacz [weryfikacji modelu w ASP.NET Web API](../formats-and-model-binding/model-validation-in-aspnet-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="79d3b-265">For information about model validation, see [Model Validation in ASP.NET Web API](../formats-and-model-binding/model-validation-in-aspnet-web-api.md).</span></span>


## <a name="updating-a-resource"></a><span data-ttu-id="79d3b-266">Aktualizowanie zasobu</span><span class="sxs-lookup"><span data-stu-id="79d3b-266">Updating a Resource</span></span>

<span data-ttu-id="79d3b-267">Aktualizowanie produktu za pomocą PUT jest bezpośrednia:</span><span class="sxs-lookup"><span data-stu-id="79d3b-267">Updating a product with PUT is straightforward:</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample11.cs)]

<span data-ttu-id="79d3b-268">Nazwa metody rozpoczyna się od &quot;Put... &quot;, więc interfejsu API sieci Web dopasowuje je do żądania PUT.</span><span class="sxs-lookup"><span data-stu-id="79d3b-268">The method name starts with &quot;Put...&quot;, so Web API matches it to PUT requests.</span></span> <span data-ttu-id="79d3b-269">Metoda przyjmuje dwa parametry: identyfikator produktu i zaktualizowane produktu.</span><span class="sxs-lookup"><span data-stu-id="79d3b-269">The method takes two parameters, the product ID and the updated product.</span></span> <span data-ttu-id="79d3b-270">*Identyfikator* parametru jest pobierana z ścieżka identyfikatora URI i *produktu* parametru jest przeprowadzić deserializacji treści żądania.</span><span class="sxs-lookup"><span data-stu-id="79d3b-270">The *id* parameter is taken from the URI path, and the *product* parameter is deserialized from the request body.</span></span> <span data-ttu-id="79d3b-271">Domyślnie przez platformę ASP.NET Web API przyjmuje typów prostych parametru na podstawie trasy i typów złożonych z treści żądania.</span><span class="sxs-lookup"><span data-stu-id="79d3b-271">By default, the ASP.NET Web API framework takes simple parameter types from the route and complex types from the request body.</span></span>

## <a name="deleting-a-resource"></a><span data-ttu-id="79d3b-272">Usunięcie zasobu</span><span class="sxs-lookup"><span data-stu-id="79d3b-272">Deleting a Resource</span></span>

<span data-ttu-id="79d3b-273">Aby usunąć resourse, zdefiniuj metodę "Usuń...".</span><span class="sxs-lookup"><span data-stu-id="79d3b-273">To delete a resourse, define a "Delete..." method.</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample12.cs)]

<span data-ttu-id="79d3b-274">Jeśli żądanie usunięcia zakończy się powodzeniem, może on zwrócić stanu 200 (OK) z treści jednostki opisujące stan; Stan 202 (zaakceptowane), jeśli usunięcie jest nadal oczekujące; Stan lub 204 (bez zawartości) z nie treści jednostki.</span><span class="sxs-lookup"><span data-stu-id="79d3b-274">If a DELETE request succeeds, it can return status 200 (OK) with an entity-body that describes the status; status 202 (Accepted) if the deletion is still pending; or status 204 (No Content) with no entity body.</span></span> <span data-ttu-id="79d3b-275">W takim przypadku `DeleteProduct` metoda ma `void` typ zwracany, więc interfejsu API sieci Web platformy ASP.NET automatycznie przekłada to stan kod 204 (bez zawartości).</span><span class="sxs-lookup"><span data-stu-id="79d3b-275">In this case, the `DeleteProduct` method has a `void` return type, so ASP.NET Web API automatically translates this into status code 204 (No Content).</span></span>
