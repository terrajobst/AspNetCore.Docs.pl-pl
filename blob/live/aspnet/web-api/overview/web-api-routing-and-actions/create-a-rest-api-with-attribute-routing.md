---
uid: web-api/overview/web-api-routing-and-actions/create-a-rest-api-with-attribute-routing
title: "Tworzenie interfejsu API REST atrybutu routingu w składniku ASP.NET Web API 2 | Dokumentacja firmy Microsoft"
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/26/2013
ms.topic: article
ms.assetid: 23fc77da-2725-4434-99a0-ff872d96336b
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/web-api-routing-and-actions/create-a-rest-api-with-attribute-routing
msc.type: authoredcontent
ms.openlocfilehash: 9ecc233e595716a167ad800a0a21a6162b051648
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
<a name="create-a-rest-api-with-attribute-routing-in-aspnet-web-api-2"></a><span data-ttu-id="06750-102">Tworzenie interfejsu API REST z atrybutem routingu w składniku ASP.NET Web API 2</span><span class="sxs-lookup"><span data-stu-id="06750-102">Create a REST API with Attribute Routing in ASP.NET Web API 2</span></span>
====================
<span data-ttu-id="06750-103">przez [Wasson Jan](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="06750-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="06750-104">Składnik Web API 2 obsługuje nowy typ routingu, nazywany *trasami atrybutów*.</span><span class="sxs-lookup"><span data-stu-id="06750-104">Web API 2 supports a new type of routing, called *attribute routing*.</span></span> <span data-ttu-id="06750-105">Ogólne omówienie trasami atrybutów, zobacz [atrybutu routingu w sieci Web API 2](attribute-routing-in-web-api-2.md).</span><span class="sxs-lookup"><span data-stu-id="06750-105">For a general overview of attribute routing, see [Attribute Routing in Web API 2](attribute-routing-in-web-api-2.md).</span></span> <span data-ttu-id="06750-106">W tym samouczku użyjesz trasami atrybutów do tworzenia interfejsu API REST dla kolekcji książek.</span><span class="sxs-lookup"><span data-stu-id="06750-106">In this tutorial, you will use attribute routing to create a REST API for a collection of books.</span></span> <span data-ttu-id="06750-107">Interfejs API obsługuje następujące akcje:</span><span class="sxs-lookup"><span data-stu-id="06750-107">The API will support the following actions:</span></span>

| <span data-ttu-id="06750-108">Akcja</span><span class="sxs-lookup"><span data-stu-id="06750-108">Action</span></span> | <span data-ttu-id="06750-109">Przykład identyfikatora URI</span><span class="sxs-lookup"><span data-stu-id="06750-109">Example URI</span></span> |
| --- | --- |
| <span data-ttu-id="06750-110">Pobranie listy wszystkich książek.</span><span class="sxs-lookup"><span data-stu-id="06750-110">Get a list of all books.</span></span> | <span data-ttu-id="06750-111">/ api/książek</span><span class="sxs-lookup"><span data-stu-id="06750-111">/api/books</span></span> |
| <span data-ttu-id="06750-112">Pobierz książkę według identyfikatora.</span><span class="sxs-lookup"><span data-stu-id="06750-112">Get a book by ID.</span></span> | <span data-ttu-id="06750-113">/API/Books/1</span><span class="sxs-lookup"><span data-stu-id="06750-113">/api/books/1</span></span> |
| <span data-ttu-id="06750-114">Pobieranie szczegółów książkę.</span><span class="sxs-lookup"><span data-stu-id="06750-114">Get the details of a book.</span></span> | <span data-ttu-id="06750-115">/API/Books/1/details</span><span class="sxs-lookup"><span data-stu-id="06750-115">/api/books/1/details</span></span> |
| <span data-ttu-id="06750-116">Pobierz listę książek według rodzaju.</span><span class="sxs-lookup"><span data-stu-id="06750-116">Get a list of books by genre.</span></span> | <span data-ttu-id="06750-117">/API/Books/fantasy</span><span class="sxs-lookup"><span data-stu-id="06750-117">/api/books/fantasy</span></span> |
| <span data-ttu-id="06750-118">Pobierz listę książek według daty publikacji.</span><span class="sxs-lookup"><span data-stu-id="06750-118">Get a list of books by publication date.</span></span> | <span data-ttu-id="06750-119">/api/books/date/2013/02/16 /API/Books/Date/2013-02-16 (alternatywny)</span><span class="sxs-lookup"><span data-stu-id="06750-119">/api/books/date/2013-02-16 /api/books/date/2013/02/16 (alternate form)</span></span> |
| <span data-ttu-id="06750-120">Pobierz listę książek przez konkretnego autora.</span><span class="sxs-lookup"><span data-stu-id="06750-120">Get a list of books by a particular author.</span></span> | <span data-ttu-id="06750-121">/API/authors/1/Books</span><span class="sxs-lookup"><span data-stu-id="06750-121">/api/authors/1/books</span></span> |

<span data-ttu-id="06750-122">Wszystkie metody są tylko do odczytu (żądania HTTP GET).</span><span class="sxs-lookup"><span data-stu-id="06750-122">All methods are read-only (HTTP GET requests).</span></span>

<span data-ttu-id="06750-123">Dla warstwy danych użyjemy Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="06750-123">For the data layer, we'll use Entity Framework.</span></span> <span data-ttu-id="06750-124">Rejestruje książki będzie zawierać następujące pola:</span><span class="sxs-lookup"><span data-stu-id="06750-124">Book records will have the following fields:</span></span>

- <span data-ttu-id="06750-125">ID</span><span class="sxs-lookup"><span data-stu-id="06750-125">ID</span></span>
- <span data-ttu-id="06750-126">Tytuł</span><span class="sxs-lookup"><span data-stu-id="06750-126">Title</span></span>
- <span data-ttu-id="06750-127">Genre</span><span class="sxs-lookup"><span data-stu-id="06750-127">Genre</span></span>
- <span data-ttu-id="06750-128">Data publikacji</span><span class="sxs-lookup"><span data-stu-id="06750-128">Publication date</span></span>
- <span data-ttu-id="06750-129">Ceny</span><span class="sxs-lookup"><span data-stu-id="06750-129">Price</span></span>
- <span data-ttu-id="06750-130">Opis</span><span class="sxs-lookup"><span data-stu-id="06750-130">Description</span></span>
- <span data-ttu-id="06750-131">Wartość IDAutora (klucz obcy tabeli Autorzy)</span><span class="sxs-lookup"><span data-stu-id="06750-131">AuthorID (foreign key to an Authors table)</span></span>

<span data-ttu-id="06750-132">Jednak większość żądań interfejsu API zwraca podzbiór danych (tytuł, autora i genre).</span><span class="sxs-lookup"><span data-stu-id="06750-132">For most requests, however, the API will return a subset of this data (title, author, and genre).</span></span> <span data-ttu-id="06750-133">Aby uzyskać pełny rekord klienta żądania `/api/books/{id}/details`.</span><span class="sxs-lookup"><span data-stu-id="06750-133">To get the complete record, the client requests `/api/books/{id}/details`.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="06750-134">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="06750-134">Prerequisites</span></span>

<span data-ttu-id="06750-135">[Visual Studio 2017](https://www.visualstudio.com/vs/) Community, Professional lub Enterprise edition.</span><span class="sxs-lookup"><span data-stu-id="06750-135">[Visual Studio 2017](https://www.visualstudio.com/vs/) Community, Professional or Enterprise edition.</span></span>

## <a name="create-the-visual-studio-project"></a><span data-ttu-id="06750-136">Tworzenie projektu programu Visual Studio</span><span class="sxs-lookup"><span data-stu-id="06750-136">Create the Visual Studio Project</span></span>

<span data-ttu-id="06750-137">Rozpocznij od działanie programu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="06750-137">Start by running Visual Studio.</span></span> <span data-ttu-id="06750-138">Z **pliku** menu, wybierz opcję **nowy** , a następnie wybierz **projektu**.</span><span class="sxs-lookup"><span data-stu-id="06750-138">From the **File** menu, select **New** and then select **Project**.</span></span>

<span data-ttu-id="06750-139">W **szablony** okienku wybierz **zainstalowane szablony** i rozwiń **Visual C#** węzła.</span><span class="sxs-lookup"><span data-stu-id="06750-139">In the **Templates** pane, select **Installed Templates** and expand the **Visual C#** node.</span></span> <span data-ttu-id="06750-140">W obszarze **Visual C#**, wybierz pozycję **Web**.</span><span class="sxs-lookup"><span data-stu-id="06750-140">Under **Visual C#**, select **Web**.</span></span> <span data-ttu-id="06750-141">Na liście szablony projektów, wybierz **aplikacji sieci Web programu ASP.NET MVC 4**.</span><span class="sxs-lookup"><span data-stu-id="06750-141">In the list of project templates, select **ASP.NET MVC 4 Web Application**.</span></span> <span data-ttu-id="06750-142">Nazwij projekt &quot;BooksAPI&quot;.</span><span class="sxs-lookup"><span data-stu-id="06750-142">Name the project &quot;BooksAPI&quot;.</span></span>

![](create-a-rest-api-with-attribute-routing/_static/image1.png)

<span data-ttu-id="06750-143">W **nowy projekt ASP.NET** okno dialogowe, wybierz opcję **pusty** szablonu.</span><span class="sxs-lookup"><span data-stu-id="06750-143">In the **New ASP.NET Project** dialog, select the **Empty** template.</span></span> <span data-ttu-id="06750-144">W obszarze ". Dodaj foldery i podstawowe odwołania dla" Wybierz **interfejsu API sieci Web** wyboru.</span><span class="sxs-lookup"><span data-stu-id="06750-144">Under "Add folders and core references for", select the **Web API** checkbox.</span></span> <span data-ttu-id="06750-145">Kliknij przycisk **Utwórz projekt**.</span><span class="sxs-lookup"><span data-stu-id="06750-145">Click **Create Project**.</span></span>

![](create-a-rest-api-with-attribute-routing/_static/image2.png)

<span data-ttu-id="06750-146">Spowoduje to utworzenie szkielet projektu, który jest skonfigurowany do obsługi funkcji interfejsu API sieci Web.</span><span class="sxs-lookup"><span data-stu-id="06750-146">This creates a skeleton project that is configured for Web API functionality.</span></span>

### <a name="domain-models"></a><span data-ttu-id="06750-147">Modele domeny</span><span class="sxs-lookup"><span data-stu-id="06750-147">Domain Models</span></span>

<span data-ttu-id="06750-148">Następnie Dodaj klasy dla modeli domeny.</span><span class="sxs-lookup"><span data-stu-id="06750-148">Next, add classes for domain models.</span></span> <span data-ttu-id="06750-149">W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy folder modeli.</span><span class="sxs-lookup"><span data-stu-id="06750-149">In Solution Explorer, right-click the Models folder.</span></span> <span data-ttu-id="06750-150">Wybierz **Dodaj**, a następnie wybierz pozycję **klasy**.</span><span class="sxs-lookup"><span data-stu-id="06750-150">Select **Add**, then select **Class**.</span></span> <span data-ttu-id="06750-151">Nazwa klasy `Author`.</span><span class="sxs-lookup"><span data-stu-id="06750-151">Name the class `Author`.</span></span>

![](create-a-rest-api-with-attribute-routing/_static/image3.png)

<span data-ttu-id="06750-152">Zastąp kod w Author.cs następujące czynności:</span><span class="sxs-lookup"><span data-stu-id="06750-152">Replace the code in Author.cs with the following:</span></span>

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample1.cs)]

<span data-ttu-id="06750-153">Teraz Dodaj kolejną klasę o nazwie `Book`.</span><span class="sxs-lookup"><span data-stu-id="06750-153">Now add another class named `Book`.</span></span>

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample2.cs)]

### <a name="add-a-web-api-controller"></a><span data-ttu-id="06750-154">Dodawanie kontrolera interfejsu API sieci Web</span><span class="sxs-lookup"><span data-stu-id="06750-154">Add a Web API Controller</span></span>

<span data-ttu-id="06750-155">W tym kroku zostanie dodany kontroler Web API, który używa programu Entity Framework jako warstwa danych.</span><span class="sxs-lookup"><span data-stu-id="06750-155">In this step, we'll add a Web API controller that uses Entity Framework as the data layer.</span></span>

<span data-ttu-id="06750-156">Naciśnij kombinację klawiszy CTRL+SHIFT+B. Projekt zostanie skompilowany.</span><span class="sxs-lookup"><span data-stu-id="06750-156">Press CTRL+SHIFT+B to build the project.</span></span> <span data-ttu-id="06750-157">Entity Framework używa odbicia do odnajdywania właściwości modeli, dzięki czemu wymaga skompilowanym zestawie utworzyć schemat bazy danych.</span><span class="sxs-lookup"><span data-stu-id="06750-157">Entity Framework uses reflection to discover the properties of the models, so it requires a compiled assembly to create the database schema.</span></span>

<span data-ttu-id="06750-158">W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy folder kontrolery.</span><span class="sxs-lookup"><span data-stu-id="06750-158">In Solution Explorer, right-click the Controllers folder.</span></span> <span data-ttu-id="06750-159">Wybierz **Dodaj**, a następnie wybierz pozycję **kontrolera**.</span><span class="sxs-lookup"><span data-stu-id="06750-159">Select **Add**, then select **Controller**.</span></span>

![](create-a-rest-api-with-attribute-routing/_static/image4.png)

<span data-ttu-id="06750-160">W **Dodawanie szkieletu** okno dialogowe, wybierz opcję "składnika Web API 2 z akcjami odczytu/zapisu, kontroler, przy użyciu programu Entity Framework."</span><span class="sxs-lookup"><span data-stu-id="06750-160">In the **Add Scaffold** dialog, select "Web API 2 Controller with read/write actions, using Entity Framework."</span></span>

[![](create-a-rest-api-with-attribute-routing/_static/image6.png)](create-a-rest-api-with-attribute-routing/_static/image5.png)

<span data-ttu-id="06750-161">W **Dodaj kontroler** okna dialogowego, aby uzyskać **nazwy kontrolera**, wprowadź &quot;BooksController&quot;.</span><span class="sxs-lookup"><span data-stu-id="06750-161">In the **Add Controller** dialog, for **Controller name**, enter &quot;BooksController&quot;.</span></span> <span data-ttu-id="06750-162">Wybierz &quot;używać asynchronicznych akcji kontrolera&quot; wyboru.</span><span class="sxs-lookup"><span data-stu-id="06750-162">Select the &quot;Use async controller actions&quot; checkbox.</span></span> <span data-ttu-id="06750-163">Aby uzyskać **klasa modelu**, wybierz pozycję &quot;książki&quot;.</span><span class="sxs-lookup"><span data-stu-id="06750-163">For **Model class**, select &quot;Book&quot;.</span></span> <span data-ttu-id="06750-164">(Jeśli nie widzisz `Book` klasy wyświetlane na liście rozwijanej, upewnij się, że utworzono projekt.) Następnie kliknij przycisk "+".</span><span class="sxs-lookup"><span data-stu-id="06750-164">(If you don't see the `Book` class listed in the dropdown, make sure that you built the project.) Then click the "+" button.</span></span>

![](create-a-rest-api-with-attribute-routing/_static/image7.png)

<span data-ttu-id="06750-165">Kliknij przycisk **Dodaj** w **nowy kontekst danych** okna dialogowego.</span><span class="sxs-lookup"><span data-stu-id="06750-165">Click **Add** in the **New Data Context** dialog.</span></span>

![](create-a-rest-api-with-attribute-routing/_static/image8.png)

<span data-ttu-id="06750-166">Kliknij przycisk **Dodaj** w **Dodaj kontroler** okna dialogowego.</span><span class="sxs-lookup"><span data-stu-id="06750-166">Click **Add** in the **Add Controller** dialog.</span></span> <span data-ttu-id="06750-167">Rusztowania Dodaje klasę o nazwie `BooksController` definiuje kontrolera interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="06750-167">The scaffolding adds a class named `BooksController` that defines the API controller.</span></span> <span data-ttu-id="06750-168">Dodaje również klasę o nazwie `BooksAPIContext` w folderze modeli, który definiuje kontekstu danych Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="06750-168">It also adds a class named `BooksAPIContext` in the Models folder, which defines the data context for Entity Framework.</span></span>

![](create-a-rest-api-with-attribute-routing/_static/image9.png)

### <a name="seed-the-database"></a><span data-ttu-id="06750-169">Inicjatora bazy danych</span><span class="sxs-lookup"><span data-stu-id="06750-169">Seed the Database</span></span>

<span data-ttu-id="06750-170">Wybierz z menu narzędzia **Menedżer pakietów biblioteki**, a następnie wybierz **Konsola Menedżera pakietów**.</span><span class="sxs-lookup"><span data-stu-id="06750-170">From the Tools menu, select **Library Package Manager**, and then select **Package Manager Console**.</span></span>

<span data-ttu-id="06750-171">W oknie Konsola Menedżera pakietów wprowadź następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="06750-171">In the Package Manager Console window, enter the following command:</span></span>

[!code-powershell[Main](create-a-rest-api-with-attribute-routing/samples/sample3.ps1)]

<span data-ttu-id="06750-172">To polecenie tworzy Migrations folder i dodaje nowy plik kodu o nazwie Configuration.cs.</span><span class="sxs-lookup"><span data-stu-id="06750-172">This command creates a Migrations folder and adds a new code file named Configuration.cs.</span></span> <span data-ttu-id="06750-173">Otwórz ten plik i Dodaj następujący kod do `Configuration.Seed` metody.</span><span class="sxs-lookup"><span data-stu-id="06750-173">Open this file and add the following code to the `Configuration.Seed` method.</span></span>

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample4.cs)]

<span data-ttu-id="06750-174">W oknie Konsola Menedżera pakietów wpisz następujące polecenia.</span><span class="sxs-lookup"><span data-stu-id="06750-174">In the Package Manager Console window, type the following commands.</span></span>

[!code-powershell[Main](create-a-rest-api-with-attribute-routing/samples/sample5.ps1)]

<span data-ttu-id="06750-175">Te polecenia Utwórz lokalną bazę danych i wywołania metody inicjatora do wypełniania bazy danych.</span><span class="sxs-lookup"><span data-stu-id="06750-175">These commands create a local database and invoke the Seed method to populate the database.</span></span>

![](create-a-rest-api-with-attribute-routing/_static/image10.png)

## <a name="add-dto-classes"></a><span data-ttu-id="06750-176">Dodawanie klasy DTO</span><span class="sxs-lookup"><span data-stu-id="06750-176">Add DTO Classes</span></span>

<span data-ttu-id="06750-177">Uruchom aplikację teraz, Wyślij żądanie GET do /api/books/1 odpowiedzi wygląda podobnie do następującego.</span><span class="sxs-lookup"><span data-stu-id="06750-177">If you run the application now and send a GET request to /api/books/1, the response looks similar to the following.</span></span> <span data-ttu-id="06750-178">(Po dodaniu wcięcie dla czytelności.)</span><span class="sxs-lookup"><span data-stu-id="06750-178">(I added indentation for readability.)</span></span>

[!code-json[Main](create-a-rest-api-with-attribute-routing/samples/sample6.json)]

<span data-ttu-id="06750-179">Zamiast tego ma zwracają podzbiór pola to żądanie.</span><span class="sxs-lookup"><span data-stu-id="06750-179">Instead, I want this request to return a subset of the fields.</span></span> <span data-ttu-id="06750-180">Ponadto ma być zwracany imię i nazwisko autora, zamiast identyfikatora autora.</span><span class="sxs-lookup"><span data-stu-id="06750-180">Also, I want it to return the author's name, rather than the author ID.</span></span> <span data-ttu-id="06750-181">W tym celu możemy zmodyfikować metod kontrolera do zwrócenia *obiektu transferu danych* (DTO) zamiast EF modelu.</span><span class="sxs-lookup"><span data-stu-id="06750-181">To accomplish this, we'll modify the controller methods to return a *data transfer object* (DTO) instead of the EF model.</span></span> <span data-ttu-id="06750-182">DTO jest obiekt, który jest przeznaczony tylko do przesyłania danych.</span><span class="sxs-lookup"><span data-stu-id="06750-182">A DTO is an object that is designed only to carry data.</span></span>

<span data-ttu-id="06750-183">W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy projekt i wybierz **Dodaj** | **nowy Folder**.</span><span class="sxs-lookup"><span data-stu-id="06750-183">In Solution Explorer, right-click the project and select **Add** | **New Folder**.</span></span> <span data-ttu-id="06750-184">Nazwa folderu &quot;DTOs&quot;.</span><span class="sxs-lookup"><span data-stu-id="06750-184">Name the folder &quot;DTOs&quot;.</span></span> <span data-ttu-id="06750-185">Dodaj klasę o nazwie `BookDto` do folderu DTOs z następującej definicji:</span><span class="sxs-lookup"><span data-stu-id="06750-185">Add a class named `BookDto` to the DTOs folder, with the following definition:</span></span>

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample7.cs)]

<span data-ttu-id="06750-186">Dodaj kolejną klasę o nazwie `BookDetailDto`.</span><span class="sxs-lookup"><span data-stu-id="06750-186">Add another class named `BookDetailDto`.</span></span>

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample8.cs)]

<span data-ttu-id="06750-187">Następnie zaktualizuj `BooksController` służącą do zwracania `BookDto` wystąpień.</span><span class="sxs-lookup"><span data-stu-id="06750-187">Next, update the `BooksController` class to return `BookDto` instances.</span></span> <span data-ttu-id="06750-188">Użyjemy [Queryable.Select](https://msdn.microsoft.com/en-us/library/system.linq.queryable.select.aspx) metody do projektu `Book` wystąpień do `BookDto` wystąpień.</span><span class="sxs-lookup"><span data-stu-id="06750-188">We'll use the [Queryable.Select](https://msdn.microsoft.com/en-us/library/system.linq.queryable.select.aspx) method to project `Book` instances to `BookDto` instances.</span></span> <span data-ttu-id="06750-189">Oto zaktualizowanego kodu do klasy kontrolera.</span><span class="sxs-lookup"><span data-stu-id="06750-189">Here is the updated code for the controller class.</span></span>

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample9.cs)]

> [!NOTE]
> <span data-ttu-id="06750-190">Po usunięciu `PutBook`, `PostBook`, i `DeleteBook` metody, ponieważ nie są one potrzebne w tym samouczku.</span><span class="sxs-lookup"><span data-stu-id="06750-190">I deleted the `PutBook`, `PostBook`, and `DeleteBook` methods, because they aren't needed for this tutorial.</span></span>


<span data-ttu-id="06750-191">Teraz uruchom aplikację, żądania /api/books/1 treść odpowiedzi powinna wyglądać następująco:</span><span class="sxs-lookup"><span data-stu-id="06750-191">Now if you run the application and request /api/books/1, the response body should look like this:</span></span>

[!code-json[Main](create-a-rest-api-with-attribute-routing/samples/sample10.json)]

## <a name="add-route-attributes"></a><span data-ttu-id="06750-192">Dodawanie tras atrybutów</span><span class="sxs-lookup"><span data-stu-id="06750-192">Add Route Attributes</span></span>

<span data-ttu-id="06750-193">Następnie firma Microsoft będzie przekonwertować kontrolera, aby korzystać z routingu atrybutu.</span><span class="sxs-lookup"><span data-stu-id="06750-193">Next, we'll convert the controller to use attribute routing.</span></span> <span data-ttu-id="06750-194">Najpierw dodaj **RoutePrefix** atrybutu do kontrolera.</span><span class="sxs-lookup"><span data-stu-id="06750-194">First, add a **RoutePrefix** attribute to the controller.</span></span> <span data-ttu-id="06750-195">Ten atrybut definiuje początkowej segmentów identyfikatora URI dla wszystkich metod na tym kontrolerze.</span><span class="sxs-lookup"><span data-stu-id="06750-195">This attribute defines the initial URI segments for all methods on this controller.</span></span>

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample11.cs?highlight=1)]

<span data-ttu-id="06750-196">Następnie dodaj **[trasy]** atrybuty do akcji kontrolera, w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="06750-196">Then add **[Route]** attributes to the controller actions, as follows:</span></span>

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample12.cs?highlight=1,7)]

<span data-ttu-id="06750-197">Szablon trasy dla każdej metody kontrolera jest prefiksem plus ciąg określony w **trasy** atrybutu.</span><span class="sxs-lookup"><span data-stu-id="06750-197">The route template for each controller method is the prefix plus the string specified in the **Route** attribute.</span></span> <span data-ttu-id="06750-198">Dla `GetBook` metoda, szablon trasy zawiera ciągu sparametryzowanego &quot;{identyfikator: int}&quot;, zgodnej, jeśli segment identyfikator URI zawiera wartość całkowitą.</span><span class="sxs-lookup"><span data-stu-id="06750-198">For the `GetBook` method, the route template includes the parameterized string &quot;{id:int}&quot;, which matches if the URI segment contains an integer value.</span></span>

| <span data-ttu-id="06750-199">Metoda</span><span class="sxs-lookup"><span data-stu-id="06750-199">Method</span></span> | <span data-ttu-id="06750-200">Szablon trasy</span><span class="sxs-lookup"><span data-stu-id="06750-200">Route Template</span></span> | <span data-ttu-id="06750-201">Przykład identyfikatora URI</span><span class="sxs-lookup"><span data-stu-id="06750-201">Example URI</span></span> |
| --- | --- | --- |
| `GetBooks` | <span data-ttu-id="06750-202">"interfejsu api/books"</span><span class="sxs-lookup"><span data-stu-id="06750-202">"api/books"</span></span> | `http://localhost/api/books` |
| `GetBook` | <span data-ttu-id="06750-203">"książek/api / {identyfikator: int}"</span><span class="sxs-lookup"><span data-stu-id="06750-203">"api/books/{id:int}"</span></span> | `http://localhost/api/books/5` |

## <a name="get-book-details"></a><span data-ttu-id="06750-204">Uzyskiwanie szczegółowych informacji książki</span><span class="sxs-lookup"><span data-stu-id="06750-204">Get Book Details</span></span>

<span data-ttu-id="06750-205">Aby uzyskać szczegóły książki, klient wyśle żądanie GET `/api/books/{id}/details`, gdzie *{id}* jest Identyfikatorem książki.</span><span class="sxs-lookup"><span data-stu-id="06750-205">To get book details, the client will send a GET request to `/api/books/{id}/details`, where *{id}* is the ID of the book.</span></span>

<span data-ttu-id="06750-206">Dodaj następującą metodę do `BooksController` klasy.</span><span class="sxs-lookup"><span data-stu-id="06750-206">Add the following method to the `BooksController` class.</span></span>

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample13.cs)]

<span data-ttu-id="06750-207">Jeśli żądanie zostało wysłane `/api/books/1/details`, odpowiedź wygląda następująco:</span><span class="sxs-lookup"><span data-stu-id="06750-207">If you request `/api/books/1/details`, the response looks like this:</span></span>

[!code-json[Main](create-a-rest-api-with-attribute-routing/samples/sample14.json)]

## <a name="get-books-by-genre"></a><span data-ttu-id="06750-208">Pobierz książek według rodzaju</span><span class="sxs-lookup"><span data-stu-id="06750-208">Get Books By Genre</span></span>

<span data-ttu-id="06750-209">Aby uzyskać listę książek określonego rodzaju, klient wyśle żądanie GET `/api/books/genre`, gdzie *genre* jest nazwą rodzaju.</span><span class="sxs-lookup"><span data-stu-id="06750-209">To get a list of books in a specific genre, the client will send a GET request to `/api/books/genre`, where *genre* is the name of the genre.</span></span> <span data-ttu-id="06750-210">(Na przykład `/get/books/fantasy`.)</span><span class="sxs-lookup"><span data-stu-id="06750-210">(For example, `/get/books/fantasy`.)</span></span>

<span data-ttu-id="06750-211">Dodaj następującą metodę do `BooksController`.</span><span class="sxs-lookup"><span data-stu-id="06750-211">Add the following method to `BooksController`.</span></span>

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample15.cs)]

<span data-ttu-id="06750-212">W tym miejscu możemy definiowania trasy, który zawiera parametr {genre} w szablon identyfikatora URI.</span><span class="sxs-lookup"><span data-stu-id="06750-212">Here we are defining a route that contains a {genre} parameter in the URI template.</span></span> <span data-ttu-id="06750-213">Zwróć uwagę, że interfejs API sieci Web jest w stanie rozróżnienia tych dwóch identyfikatorów i kierowania ich do różnych metod:</span><span class="sxs-lookup"><span data-stu-id="06750-213">Notice that Web API is able to distinguish these two URIs and route them to different methods:</span></span>

`/api/books/1`

`/api/books/fantasy`

<span data-ttu-id="06750-214">Jest to spowodowane tym `GetBook` metoda zawiera ograniczenie, że segment "id" musi być liczbą całkowitą:</span><span class="sxs-lookup"><span data-stu-id="06750-214">That's because the `GetBook` method includes a constraint that the "id" segment must be an integer value:</span></span>

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample16.cs?highlight=1)]

<span data-ttu-id="06750-215">Jeśli żądania /api/books/fantasy odpowiedzi wygląda następująco:</span><span class="sxs-lookup"><span data-stu-id="06750-215">If you request /api/books/fantasy, the response looks like this:</span></span>

`[ { "Title": "Midnight Rain", "Author": "Ralls, Kim", "Genre": "Fantasy" }, { "Title": "Maeve Ascendant", "Author": "Corets, Eva", "Genre": "Fantasy" }, { "Title": "The Sundered Grail", "Author": "Corets, Eva", "Genre": "Fantasy" } ]`

## <a name="get-books-by-author"></a><span data-ttu-id="06750-216">Pobierz książek przez autora</span><span class="sxs-lookup"><span data-stu-id="06750-216">Get Books By Author</span></span>

<span data-ttu-id="06750-217">Aby uzyskać listę książek dla konkretnego autora, klient wyśle żądanie GET `/api/authors/id/books`, gdzie *identyfikator* jest Identyfikatorem autora.</span><span class="sxs-lookup"><span data-stu-id="06750-217">To get a list of a books for a particular author, the client will send a GET request to `/api/authors/id/books`, where *id* is the ID of the author.</span></span>

<span data-ttu-id="06750-218">Dodaj następującą metodę do `BooksController`.</span><span class="sxs-lookup"><span data-stu-id="06750-218">Add the following method to `BooksController`.</span></span>

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample17.cs)]

<span data-ttu-id="06750-219">W tym przykładzie jest ciekawe ponieważ &quot;książki&quot; jest traktowane zasobu podrzędnego &quot;autorów&quot;.</span><span class="sxs-lookup"><span data-stu-id="06750-219">This example is interesting because &quot;books&quot; is treated a child resource of &quot;authors&quot;.</span></span> <span data-ttu-id="06750-220">Ten wzorzec jest dość często w interfejsy API RESTful.</span><span class="sxs-lookup"><span data-stu-id="06750-220">This pattern is quite common in RESTful APIs.</span></span>

<span data-ttu-id="06750-221">Prefiks trasy w zastępuje tyldy (~) w szablonie trasy **RoutePrefix** atrybutu.</span><span class="sxs-lookup"><span data-stu-id="06750-221">The tilde (~) in the route template overrides the route prefix in the **RoutePrefix** attribute.</span></span>

## <a name="get-books-by-publication-date"></a><span data-ttu-id="06750-222">Pobierz książek przez Data publikacji</span><span class="sxs-lookup"><span data-stu-id="06750-222">Get Books By Publication Date</span></span>

<span data-ttu-id="06750-223">Aby uzyskać listę książek według daty publikacji, klient wyśle żądanie GET `/api/books/date/yyyy-mm-dd`, gdzie *rrrr mm-dd* jest datą.</span><span class="sxs-lookup"><span data-stu-id="06750-223">To get a list of books by publication date, the client will send a GET request to `/api/books/date/yyyy-mm-dd`, where *yyyy-mm-dd* is the date.</span></span>

<span data-ttu-id="06750-224">Oto jeden ze sposobów:</span><span class="sxs-lookup"><span data-stu-id="06750-224">Here is one way to do this:</span></span>

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample18.cs)]

<span data-ttu-id="06750-225">`{pubdate:datetime}` Parametr jest ograniczony do dopasowania **DateTime** wartość.</span><span class="sxs-lookup"><span data-stu-id="06750-225">The `{pubdate:datetime}` parameter is constrained to match a **DateTime** value.</span></span> <span data-ttu-id="06750-226">To działa, ale jest rzeczywiście mniej ograniczająca niż chcielibyśmy.</span><span class="sxs-lookup"><span data-stu-id="06750-226">This works, but it's actually more permissive than we'd like.</span></span> <span data-ttu-id="06750-227">Na przykład tych identyfikatorów również zgodna trasy:</span><span class="sxs-lookup"><span data-stu-id="06750-227">For example, these URIs will also match the route:</span></span>

`/api/books/date/Thu, 01 May 2008`

`/api/books/date/2000-12-16T00:00:00`

<span data-ttu-id="06750-228">Nie ma problem z zezwoleniem na tych identyfikatorów.</span><span class="sxs-lookup"><span data-stu-id="06750-228">There's nothing wrong with allowing these URIs.</span></span> <span data-ttu-id="06750-229">Może jednak ograniczyć trasy z formatem określonym przez dodanie ograniczenia wyrażenia regularnego do szablonu trasy:</span><span class="sxs-lookup"><span data-stu-id="06750-229">However, you can restrict the route to a particular format by adding a regular-expression constraint to the route template:</span></span>

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample19.cs?highlight=1)]

<span data-ttu-id="06750-230">Obecnie tylko daty w postaci &quot;rrrr mm-dd&quot; będą zgodne.</span><span class="sxs-lookup"><span data-stu-id="06750-230">Now only dates in the form &quot;yyyy-mm-dd&quot; will match.</span></span> <span data-ttu-id="06750-231">Zwróć uwagę, że nie używamy wyrażenia regularnego do zweryfikowania, że dotarliśmy rzeczywistą datę.</span><span class="sxs-lookup"><span data-stu-id="06750-231">Notice that we don't use the regex to validate that we got a real date.</span></span> <span data-ttu-id="06750-232">Który jest obsługiwane w przypadku interfejsu API sieci Web próbuje przekonwertować segmentem identyfikatora URI do **DateTime** wystąpienia.</span><span class="sxs-lookup"><span data-stu-id="06750-232">That is handled when Web API tries to convert the URI segment into a **DateTime** instance.</span></span> <span data-ttu-id="06750-233">Nieprawidłowa data, takich jak "2012-47-99" zakończy się niepowodzeniem ma zostać przekonwertowane, a klient otrzyma błąd 404.</span><span class="sxs-lookup"><span data-stu-id="06750-233">An invalid date such as '2012-47-99' will fail to be converted, and the client will get a 404 error.</span></span>

<span data-ttu-id="06750-234">Można również obsługiwać separatora ukośnika (`/api/books/date/yyyy/mm/dd`) przez dodanie kolejnego **[trasy]** atrybut o różnych wyrażenia regularnego.</span><span class="sxs-lookup"><span data-stu-id="06750-234">You can also support a slash separator (`/api/books/date/yyyy/mm/dd`) by adding another **[Route]** attribute with a different regex.</span></span>

[!code-html[Main](create-a-rest-api-with-attribute-routing/samples/sample20.html)]

<span data-ttu-id="06750-235">Brak niewielkie, ale ważne szczegóły tutaj.</span><span class="sxs-lookup"><span data-stu-id="06750-235">There is a subtle but important detail here.</span></span> <span data-ttu-id="06750-236">Drugi szablon trasy zawiera symbol wieloznaczny (\*) na początku parametru {pubdate}:</span><span class="sxs-lookup"><span data-stu-id="06750-236">The second route template has a wildcard character (\*) at the start of the {pubdate} parameter:</span></span>

[!code-json[Main](create-a-rest-api-with-attribute-routing/samples/sample21.json)]

<span data-ttu-id="06750-237">Ta wartość informuje aparatu routingu tego {pubdate} powinna być taka sama pozostałej części identyfikatora URI.</span><span class="sxs-lookup"><span data-stu-id="06750-237">This tells the routing engine that {pubdate} should match the rest of the URI.</span></span> <span data-ttu-id="06750-238">Domyślnie parametr szablonu odpowiada jednej segmentem identyfikatora URI.</span><span class="sxs-lookup"><span data-stu-id="06750-238">By default, a template parameter matches a single URI segment.</span></span> <span data-ttu-id="06750-239">W takim przypadku chcemy {pubdate} span kilkoma segmentami identyfikatora URI:</span><span class="sxs-lookup"><span data-stu-id="06750-239">In this case, we want {pubdate} to span several URI segments:</span></span>

`/api/books/date/2013/06/17`

## <a name="controller-code"></a><span data-ttu-id="06750-240">Kod kontrolera</span><span class="sxs-lookup"><span data-stu-id="06750-240">Controller Code</span></span>

<span data-ttu-id="06750-241">W tym miejscu jest kompletny kod dla klasy BooksController.</span><span class="sxs-lookup"><span data-stu-id="06750-241">Here is the complete code for the BooksController class.</span></span>

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample22.cs)]

## <a name="summary"></a><span data-ttu-id="06750-242">Podsumowanie</span><span class="sxs-lookup"><span data-stu-id="06750-242">Summary</span></span>

<span data-ttu-id="06750-243">Atrybut routingu daje więcej kontroli i większą elastyczność podczas projektowania identyfikatorów URI dla interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="06750-243">Attribute routing gives you more control and greater flexibility when designing the URIs for your API.</span></span>
