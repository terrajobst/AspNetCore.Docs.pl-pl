---
uid: web-api/overview/web-api-routing-and-actions/create-a-rest-api-with-attribute-routing
title: Tworzenie interfejsu API REST przy użyciu atrybutu routingu we wzorcu ASP.NET Web API 2 | Dokumentacja firmy Microsoft
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/26/2013
ms.topic: article
ms.assetid: 23fc77da-2725-4434-99a0-ff872d96336b
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/web-api-routing-and-actions/create-a-rest-api-with-attribute-routing
msc.type: authoredcontent
ms.openlocfilehash: 20538348504427c30d5d75705271a5c3c3c2c171
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37362223"
---
<a name="create-a-rest-api-with-attribute-routing-in-aspnet-web-api-2"></a><span data-ttu-id="88c19-102">Tworzenie interfejsu API REST z atrybutem routingu we wzorcu ASP.NET Web API 2</span><span class="sxs-lookup"><span data-stu-id="88c19-102">Create a REST API with Attribute Routing in ASP.NET Web API 2</span></span>
====================
<span data-ttu-id="88c19-103">przez [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="88c19-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="88c19-104">Składnik Web API 2 obsługuje nowy typ routingu, o nazwie *trasowanie atrybutów*.</span><span class="sxs-lookup"><span data-stu-id="88c19-104">Web API 2 supports a new type of routing, called *attribute routing*.</span></span> <span data-ttu-id="88c19-105">Aby uzyskać ogólne omówienie trasowanie atrybutów, zobacz [atrybut routingu w sieci Web API 2](attribute-routing-in-web-api-2.md).</span><span class="sxs-lookup"><span data-stu-id="88c19-105">For a general overview of attribute routing, see [Attribute Routing in Web API 2](attribute-routing-in-web-api-2.md).</span></span> <span data-ttu-id="88c19-106">W tym samouczku użyjesz trasowanie atrybutów do tworzenia interfejsu API REST dla kolekcji książek.</span><span class="sxs-lookup"><span data-stu-id="88c19-106">In this tutorial, you will use attribute routing to create a REST API for a collection of books.</span></span> <span data-ttu-id="88c19-107">Interfejs API obsługuje następujące akcje:</span><span class="sxs-lookup"><span data-stu-id="88c19-107">The API will support the following actions:</span></span>

| <span data-ttu-id="88c19-108">Akcja</span><span class="sxs-lookup"><span data-stu-id="88c19-108">Action</span></span> | <span data-ttu-id="88c19-109">Przykład identyfikatora URI</span><span class="sxs-lookup"><span data-stu-id="88c19-109">Example URI</span></span> |
| --- | --- |
| <span data-ttu-id="88c19-110">Zostanie wyświetlona lista wszystkich książek.</span><span class="sxs-lookup"><span data-stu-id="88c19-110">Get a list of all books.</span></span> | <span data-ttu-id="88c19-111">/ api/książki</span><span class="sxs-lookup"><span data-stu-id="88c19-111">/api/books</span></span> |
| <span data-ttu-id="88c19-112">Pobierz książkę według identyfikatora.</span><span class="sxs-lookup"><span data-stu-id="88c19-112">Get a book by ID.</span></span> | <span data-ttu-id="88c19-113">/API/Books/1</span><span class="sxs-lookup"><span data-stu-id="88c19-113">/api/books/1</span></span> |
| <span data-ttu-id="88c19-114">Pobieranie szczegółów książki.</span><span class="sxs-lookup"><span data-stu-id="88c19-114">Get the details of a book.</span></span> | <span data-ttu-id="88c19-115">/API/Books/1/details</span><span class="sxs-lookup"><span data-stu-id="88c19-115">/api/books/1/details</span></span> |
| <span data-ttu-id="88c19-116">Zostanie wyświetlona lista książek według gatunku.</span><span class="sxs-lookup"><span data-stu-id="88c19-116">Get a list of books by genre.</span></span> | <span data-ttu-id="88c19-117">/API/Books/fantasy</span><span class="sxs-lookup"><span data-stu-id="88c19-117">/api/books/fantasy</span></span> |
| <span data-ttu-id="88c19-118">Zostanie wyświetlona lista książek według daty publikacji.</span><span class="sxs-lookup"><span data-stu-id="88c19-118">Get a list of books by publication date.</span></span> | <span data-ttu-id="88c19-119">/api/books/date/2013/02/16 /API/Books/Date/2013-02-16 (alternatywna postać)</span><span class="sxs-lookup"><span data-stu-id="88c19-119">/api/books/date/2013-02-16 /api/books/date/2013/02/16 (alternate form)</span></span> |
| <span data-ttu-id="88c19-120">Zostanie wyświetlona lista książek przez określonego autora.</span><span class="sxs-lookup"><span data-stu-id="88c19-120">Get a list of books by a particular author.</span></span> | <span data-ttu-id="88c19-121">/API/authors/1/Books</span><span class="sxs-lookup"><span data-stu-id="88c19-121">/api/authors/1/books</span></span> |

<span data-ttu-id="88c19-122">Wszystkie metody są tylko do odczytu (żądania HTTP GET).</span><span class="sxs-lookup"><span data-stu-id="88c19-122">All methods are read-only (HTTP GET requests).</span></span>

<span data-ttu-id="88c19-123">Dla warstwy danych użyto programu Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="88c19-123">For the data layer, we'll use Entity Framework.</span></span> <span data-ttu-id="88c19-124">Rekordy książek będzie zawierać następujące pola:</span><span class="sxs-lookup"><span data-stu-id="88c19-124">Book records will have the following fields:</span></span>

- <span data-ttu-id="88c19-125">ID</span><span class="sxs-lookup"><span data-stu-id="88c19-125">ID</span></span>
- <span data-ttu-id="88c19-126">Tytuł</span><span class="sxs-lookup"><span data-stu-id="88c19-126">Title</span></span>
- <span data-ttu-id="88c19-127">Gatunku</span><span class="sxs-lookup"><span data-stu-id="88c19-127">Genre</span></span>
- <span data-ttu-id="88c19-128">Data publikacji</span><span class="sxs-lookup"><span data-stu-id="88c19-128">Publication date</span></span>
- <span data-ttu-id="88c19-129">Cena</span><span class="sxs-lookup"><span data-stu-id="88c19-129">Price</span></span>
- <span data-ttu-id="88c19-130">Opis</span><span class="sxs-lookup"><span data-stu-id="88c19-130">Description</span></span>
- <span data-ttu-id="88c19-131">Wartość IDAutora (klucz obcy tabeli Autorzy)</span><span class="sxs-lookup"><span data-stu-id="88c19-131">AuthorID (foreign key to an Authors table)</span></span>

<span data-ttu-id="88c19-132">W większości żądań, natomiast interfejs API zwróci podzbiór danych (tytuł, autor i gatunku).</span><span class="sxs-lookup"><span data-stu-id="88c19-132">For most requests, however, the API will return a subset of this data (title, author, and genre).</span></span> <span data-ttu-id="88c19-133">Do żądania get całego rekordu klienta `/api/books/{id}/details`.</span><span class="sxs-lookup"><span data-stu-id="88c19-133">To get the complete record, the client requests `/api/books/{id}/details`.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="88c19-134">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="88c19-134">Prerequisites</span></span>

<span data-ttu-id="88c19-135">[Program Visual Studio 2017](https://www.visualstudio.com/vs/) Community, Professional lub Enterprise edition.</span><span class="sxs-lookup"><span data-stu-id="88c19-135">[Visual Studio 2017](https://www.visualstudio.com/vs/) Community, Professional or Enterprise edition.</span></span>

## <a name="create-the-visual-studio-project"></a><span data-ttu-id="88c19-136">Tworzenie projektu programu Visual Studio</span><span class="sxs-lookup"><span data-stu-id="88c19-136">Create the Visual Studio Project</span></span>

<span data-ttu-id="88c19-137">Rozpocznij od uruchamianie programu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="88c19-137">Start by running Visual Studio.</span></span> <span data-ttu-id="88c19-138">Z **pliku** menu, wybierz opcję **New** , a następnie wybierz **projektu**.</span><span class="sxs-lookup"><span data-stu-id="88c19-138">From the **File** menu, select **New** and then select **Project**.</span></span>

<span data-ttu-id="88c19-139">W **szablony** okienku wybierz **zainstalowane szablony** i rozwiń **Visual C#** węzła.</span><span class="sxs-lookup"><span data-stu-id="88c19-139">In the **Templates** pane, select **Installed Templates** and expand the **Visual C#** node.</span></span> <span data-ttu-id="88c19-140">W obszarze **Visual C#**, wybierz opcję **Web**.</span><span class="sxs-lookup"><span data-stu-id="88c19-140">Under **Visual C#**, select **Web**.</span></span> <span data-ttu-id="88c19-141">Na liście szablonów projektu wybierz **aplikacji sieci Web programu ASP.NET MVC 4**.</span><span class="sxs-lookup"><span data-stu-id="88c19-141">In the list of project templates, select **ASP.NET MVC 4 Web Application**.</span></span> <span data-ttu-id="88c19-142">Nadaj projektowi nazwę &quot;BooksAPI&quot;.</span><span class="sxs-lookup"><span data-stu-id="88c19-142">Name the project &quot;BooksAPI&quot;.</span></span>

![](create-a-rest-api-with-attribute-routing/_static/image1.png)

<span data-ttu-id="88c19-143">W **nowy projekt ASP.NET** okno dialogowe, wybierz opcję **pusty** szablonu.</span><span class="sxs-lookup"><span data-stu-id="88c19-143">In the **New ASP.NET Project** dialog, select the **Empty** template.</span></span> <span data-ttu-id="88c19-144">W obszarze "Dodaj foldery i podstawowe odwołania dla" Wybierz **interfejsu API sieci Web** pola wyboru.</span><span class="sxs-lookup"><span data-stu-id="88c19-144">Under "Add folders and core references for", select the **Web API** checkbox.</span></span> <span data-ttu-id="88c19-145">Kliknij przycisk **Utwórz projekt**.</span><span class="sxs-lookup"><span data-stu-id="88c19-145">Click **Create Project**.</span></span>

![](create-a-rest-api-with-attribute-routing/_static/image2.png)

<span data-ttu-id="88c19-146">Spowoduje to utworzenie szkielet projektu, który jest skonfigurowany do obsługi funkcji interfejsu API sieci Web.</span><span class="sxs-lookup"><span data-stu-id="88c19-146">This creates a skeleton project that is configured for Web API functionality.</span></span>

### <a name="domain-models"></a><span data-ttu-id="88c19-147">Modeli domeny</span><span class="sxs-lookup"><span data-stu-id="88c19-147">Domain Models</span></span>

<span data-ttu-id="88c19-148">Następnie Dodaj klasy dla modeli domeny.</span><span class="sxs-lookup"><span data-stu-id="88c19-148">Next, add classes for domain models.</span></span> <span data-ttu-id="88c19-149">W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy folderu modeli.</span><span class="sxs-lookup"><span data-stu-id="88c19-149">In Solution Explorer, right-click the Models folder.</span></span> <span data-ttu-id="88c19-150">Wybierz **Dodaj**, a następnie wybierz **klasy**.</span><span class="sxs-lookup"><span data-stu-id="88c19-150">Select **Add**, then select **Class**.</span></span> <span data-ttu-id="88c19-151">Nazwa klasy `Author`.</span><span class="sxs-lookup"><span data-stu-id="88c19-151">Name the class `Author`.</span></span>

![](create-a-rest-api-with-attribute-routing/_static/image3.png)

<span data-ttu-id="88c19-152">Zastąp kod w Author.cs następujących czynności:</span><span class="sxs-lookup"><span data-stu-id="88c19-152">Replace the code in Author.cs with the following:</span></span>

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample1.cs)]

<span data-ttu-id="88c19-153">Teraz Dodaj klasę o nazwie `Book`.</span><span class="sxs-lookup"><span data-stu-id="88c19-153">Now add another class named `Book`.</span></span>

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample2.cs)]

### <a name="add-a-web-api-controller"></a><span data-ttu-id="88c19-154">Dodaj Kontroler interfejsu API sieci Web</span><span class="sxs-lookup"><span data-stu-id="88c19-154">Add a Web API Controller</span></span>

<span data-ttu-id="88c19-155">W tym kroku dodasz kontroler internetowego interfejsu API, który używa programu Entity Framework jako warstwa danych.</span><span class="sxs-lookup"><span data-stu-id="88c19-155">In this step, we'll add a Web API controller that uses Entity Framework as the data layer.</span></span>

<span data-ttu-id="88c19-156">Naciśnij kombinację klawiszy CTRL+SHIFT+B. Projekt zostanie skompilowany.</span><span class="sxs-lookup"><span data-stu-id="88c19-156">Press CTRL+SHIFT+B to build the project.</span></span> <span data-ttu-id="88c19-157">Entity Framework używa odbicia, aby odnaleźć właściwości te modele, dzięki czemu wymaga skompilowanego zestawu w celu utworzenia schematu bazy danych.</span><span class="sxs-lookup"><span data-stu-id="88c19-157">Entity Framework uses reflection to discover the properties of the models, so it requires a compiled assembly to create the database schema.</span></span>

<span data-ttu-id="88c19-158">W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy folder kontrolerów.</span><span class="sxs-lookup"><span data-stu-id="88c19-158">In Solution Explorer, right-click the Controllers folder.</span></span> <span data-ttu-id="88c19-159">Wybierz **Dodaj**, a następnie wybierz **kontrolera**.</span><span class="sxs-lookup"><span data-stu-id="88c19-159">Select **Add**, then select **Controller**.</span></span>

![](create-a-rest-api-with-attribute-routing/_static/image4.png)

<span data-ttu-id="88c19-160">W **Dodawanie szkieletu** okno dialogowe, wybierz opcję "Web API 2 z akcjami odczytu/zapisu, kontroler, używający narzędzia Entity Framework."</span><span class="sxs-lookup"><span data-stu-id="88c19-160">In the **Add Scaffold** dialog, select "Web API 2 Controller with read/write actions, using Entity Framework."</span></span>

[![](create-a-rest-api-with-attribute-routing/_static/image6.png)](create-a-rest-api-with-attribute-routing/_static/image5.png)

<span data-ttu-id="88c19-161">W **Dodaj kontroler** okna dialogowego, aby uzyskać **nazwy kontrolera**, wprowadź &quot;BooksController&quot;.</span><span class="sxs-lookup"><span data-stu-id="88c19-161">In the **Add Controller** dialog, for **Controller name**, enter &quot;BooksController&quot;.</span></span> <span data-ttu-id="88c19-162">Wybierz &quot;używać asynchronicznych akcji kontrolera&quot; pola wyboru.</span><span class="sxs-lookup"><span data-stu-id="88c19-162">Select the &quot;Use async controller actions&quot; checkbox.</span></span> <span data-ttu-id="88c19-163">Aby uzyskać **klasa modelu**, wybierz opcję &quot;książki&quot;.</span><span class="sxs-lookup"><span data-stu-id="88c19-163">For **Model class**, select &quot;Book&quot;.</span></span> <span data-ttu-id="88c19-164">(Jeśli nie widzisz `Book` klasy na liście rozwijanej, upewnij się, że utworzony projekt.) Następnie kliknij przycisk "+".</span><span class="sxs-lookup"><span data-stu-id="88c19-164">(If you don't see the `Book` class listed in the dropdown, make sure that you built the project.) Then click the "+" button.</span></span>

![](create-a-rest-api-with-attribute-routing/_static/image7.png)

<span data-ttu-id="88c19-165">Kliknij przycisk **Dodaj** w **nowy kontekst danych** okna dialogowego.</span><span class="sxs-lookup"><span data-stu-id="88c19-165">Click **Add** in the **New Data Context** dialog.</span></span>

![](create-a-rest-api-with-attribute-routing/_static/image8.png)

<span data-ttu-id="88c19-166">Kliknij przycisk **Dodaj** w **Dodaj kontroler** okna dialogowego.</span><span class="sxs-lookup"><span data-stu-id="88c19-166">Click **Add** in the **Add Controller** dialog.</span></span> <span data-ttu-id="88c19-167">Szkieletu Dodaje klasę o nazwie `BooksController` definiujący Kontroler interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="88c19-167">The scaffolding adds a class named `BooksController` that defines the API controller.</span></span> <span data-ttu-id="88c19-168">Dodaje również klasę o nazwie `BooksAPIContext` w folderze modeli, która definiuje kontekst danych Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="88c19-168">It also adds a class named `BooksAPIContext` in the Models folder, which defines the data context for Entity Framework.</span></span>

![](create-a-rest-api-with-attribute-routing/_static/image9.png)

### <a name="seed-the-database"></a><span data-ttu-id="88c19-169">Inicjowanie bazy danych</span><span class="sxs-lookup"><span data-stu-id="88c19-169">Seed the Database</span></span>

<span data-ttu-id="88c19-170">Wybierz z menu narzędzia **Menedżer pakietów biblioteki**, a następnie wybierz pozycję **Konsola Menedżera pakietów**.</span><span class="sxs-lookup"><span data-stu-id="88c19-170">From the Tools menu, select **Library Package Manager**, and then select **Package Manager Console**.</span></span>

<span data-ttu-id="88c19-171">W oknie Konsola Menedżera pakietów wprowadź następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="88c19-171">In the Package Manager Console window, enter the following command:</span></span>

[!code-powershell[Main](create-a-rest-api-with-attribute-routing/samples/sample3.ps1)]

<span data-ttu-id="88c19-172">To polecenie tworzy folder migracje i dodaje nowy plik kodu o nazwie Configuration.cs.</span><span class="sxs-lookup"><span data-stu-id="88c19-172">This command creates a Migrations folder and adds a new code file named Configuration.cs.</span></span> <span data-ttu-id="88c19-173">Otwórz ten plik i Dodaj następujący kod do `Configuration.Seed` metody.</span><span class="sxs-lookup"><span data-stu-id="88c19-173">Open this file and add the following code to the `Configuration.Seed` method.</span></span>

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample4.cs)]

<span data-ttu-id="88c19-174">W oknie Konsola Menedżera pakietów wpisz następujące polecenia.</span><span class="sxs-lookup"><span data-stu-id="88c19-174">In the Package Manager Console window, type the following commands.</span></span>

[!code-powershell[Main](create-a-rest-api-with-attribute-routing/samples/sample5.ps1)]

<span data-ttu-id="88c19-175">Te polecenia Utwórz lokalnej bazy danych i wywołaj metodę inicjatora do wypełniania bazy danych.</span><span class="sxs-lookup"><span data-stu-id="88c19-175">These commands create a local database and invoke the Seed method to populate the database.</span></span>

![](create-a-rest-api-with-attribute-routing/_static/image10.png)

## <a name="add-dto-classes"></a><span data-ttu-id="88c19-176">Dodaj obiekt DTO klasy</span><span class="sxs-lookup"><span data-stu-id="88c19-176">Add DTO Classes</span></span>

<span data-ttu-id="88c19-177">Uruchom aplikację teraz, Wyślij żądanie Pobierz do /api/books/1 odpowiedź wygląda podobnie do następującego.</span><span class="sxs-lookup"><span data-stu-id="88c19-177">If you run the application now and send a GET request to /api/books/1, the response looks similar to the following.</span></span> <span data-ttu-id="88c19-178">(Po dodaniu wcięć dla czytelności.)</span><span class="sxs-lookup"><span data-stu-id="88c19-178">(I added indentation for readability.)</span></span>

[!code-json[Main](create-a-rest-api-with-attribute-routing/samples/sample6.json)]

<span data-ttu-id="88c19-179">Zamiast tego chcę tego żądania, aby zwrócić podzbiór pól.</span><span class="sxs-lookup"><span data-stu-id="88c19-179">Instead, I want this request to return a subset of the fields.</span></span> <span data-ttu-id="88c19-180">Ponadto powinna być zwraca imię i nazwisko autora, a nie identyfikatora autora.</span><span class="sxs-lookup"><span data-stu-id="88c19-180">Also, I want it to return the author's name, rather than the author ID.</span></span> <span data-ttu-id="88c19-181">Aby to osiągnąć, zmodyfikujemy metody kontrolera, aby zwrócić *obiekt transferu danych* (DTO) zamiast modelu platformy EF.</span><span class="sxs-lookup"><span data-stu-id="88c19-181">To accomplish this, we'll modify the controller methods to return a *data transfer object* (DTO) instead of the EF model.</span></span> <span data-ttu-id="88c19-182">Obiekt DTO jest obiektem, który jest przeznaczony tylko do przesyłania danych.</span><span class="sxs-lookup"><span data-stu-id="88c19-182">A DTO is an object that is designed only to carry data.</span></span>

<span data-ttu-id="88c19-183">W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy projekt i wybierz **Dodaj** | **nowy Folder**.</span><span class="sxs-lookup"><span data-stu-id="88c19-183">In Solution Explorer, right-click the project and select **Add** | **New Folder**.</span></span> <span data-ttu-id="88c19-184">Nazwa folderu &quot;dto&quot;.</span><span class="sxs-lookup"><span data-stu-id="88c19-184">Name the folder &quot;DTOs&quot;.</span></span> <span data-ttu-id="88c19-185">Dodaj klasę o nazwie `BookDto` do folderu dto, przy użyciu następujących definicji:</span><span class="sxs-lookup"><span data-stu-id="88c19-185">Add a class named `BookDto` to the DTOs folder, with the following definition:</span></span>

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample7.cs)]

<span data-ttu-id="88c19-186">Dodaj klasę o nazwie `BookDetailDto`.</span><span class="sxs-lookup"><span data-stu-id="88c19-186">Add another class named `BookDetailDto`.</span></span>

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample8.cs)]

<span data-ttu-id="88c19-187">Następnie zaktualizuj `BooksController` klasy w celu zwracania `BookDto` wystąpień.</span><span class="sxs-lookup"><span data-stu-id="88c19-187">Next, update the `BooksController` class to return `BookDto` instances.</span></span> <span data-ttu-id="88c19-188">Użyjemy [Queryable.Select](https://msdn.microsoft.com/library/system.linq.queryable.select.aspx) metody do projektu `Book` wystąpień do `BookDto` wystąpień.</span><span class="sxs-lookup"><span data-stu-id="88c19-188">We'll use the [Queryable.Select](https://msdn.microsoft.com/library/system.linq.queryable.select.aspx) method to project `Book` instances to `BookDto` instances.</span></span> <span data-ttu-id="88c19-189">Oto zaktualizowany kod do klasy kontrolera.</span><span class="sxs-lookup"><span data-stu-id="88c19-189">Here is the updated code for the controller class.</span></span>

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample9.cs)]

> [!NOTE]
> <span data-ttu-id="88c19-190">Po usunięciu `PutBook`, `PostBook`, i `DeleteBook` metod, ponieważ nie są one potrzebne w tym samouczku.</span><span class="sxs-lookup"><span data-stu-id="88c19-190">I deleted the `PutBook`, `PostBook`, and `DeleteBook` methods, because they aren't needed for this tutorial.</span></span>


<span data-ttu-id="88c19-191">Teraz uruchom aplikację, żądanie /api/books/1 treść odpowiedzi powinna wyglądać następująco:</span><span class="sxs-lookup"><span data-stu-id="88c19-191">Now if you run the application and request /api/books/1, the response body should look like this:</span></span>

[!code-json[Main](create-a-rest-api-with-attribute-routing/samples/sample10.json)]

## <a name="add-route-attributes"></a><span data-ttu-id="88c19-192">Dodawanie atrybutów trasy</span><span class="sxs-lookup"><span data-stu-id="88c19-192">Add Route Attributes</span></span>

<span data-ttu-id="88c19-193">Firma Microsoft będzie następnie przekonwertować kontroler trasowanie atrybutów.</span><span class="sxs-lookup"><span data-stu-id="88c19-193">Next, we'll convert the controller to use attribute routing.</span></span> <span data-ttu-id="88c19-194">Najpierw dodaj **RoutePrefix** atrybutu do kontrolera.</span><span class="sxs-lookup"><span data-stu-id="88c19-194">First, add a **RoutePrefix** attribute to the controller.</span></span> <span data-ttu-id="88c19-195">Ten atrybut definiuje początkowe segmentów identyfikator URI dla wszystkich metod na tym kontrolerze.</span><span class="sxs-lookup"><span data-stu-id="88c19-195">This attribute defines the initial URI segments for all methods on this controller.</span></span>

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample11.cs?highlight=1)]

<span data-ttu-id="88c19-196">Następnie dodaj **[trasy]** atrybuty do akcji kontrolera, w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="88c19-196">Then add **[Route]** attributes to the controller actions, as follows:</span></span>

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample12.cs?highlight=1,7)]

<span data-ttu-id="88c19-197">Szablon trasy dla każdej metody kontrolera jest prefiksem, a także ciąg określony w **trasy** atrybutu.</span><span class="sxs-lookup"><span data-stu-id="88c19-197">The route template for each controller method is the prefix plus the string specified in the **Route** attribute.</span></span> <span data-ttu-id="88c19-198">Dla `GetBook` , szablon trasy zawiera ciągu sparametryzowanego &quot;{identyfikator: int}&quot;, która pasuje, jeśli segmentem identyfikatora URI zawiera wartość całkowitą.</span><span class="sxs-lookup"><span data-stu-id="88c19-198">For the `GetBook` method, the route template includes the parameterized string &quot;{id:int}&quot;, which matches if the URI segment contains an integer value.</span></span>

| <span data-ttu-id="88c19-199">Metoda</span><span class="sxs-lookup"><span data-stu-id="88c19-199">Method</span></span> | <span data-ttu-id="88c19-200">Szablon trasy</span><span class="sxs-lookup"><span data-stu-id="88c19-200">Route Template</span></span> | <span data-ttu-id="88c19-201">Przykład identyfikatora URI</span><span class="sxs-lookup"><span data-stu-id="88c19-201">Example URI</span></span> |
| --- | --- | --- |
| `GetBooks` | <span data-ttu-id="88c19-202">"interfejs api/books"</span><span class="sxs-lookup"><span data-stu-id="88c19-202">"api/books"</span></span> | `http://localhost/api/books` |
| `GetBook` | <span data-ttu-id="88c19-203">"interfejs api/książki / {identyfikator: int}"</span><span class="sxs-lookup"><span data-stu-id="88c19-203">"api/books/{id:int}"</span></span> | `http://localhost/api/books/5` |

## <a name="get-book-details"></a><span data-ttu-id="88c19-204">Pobieranie szczegółów książki</span><span class="sxs-lookup"><span data-stu-id="88c19-204">Get Book Details</span></span>

<span data-ttu-id="88c19-205">Aby uzyskać szczegółowe informacje z książki, klient wyśle żądanie GET w celu `/api/books/{id}/details`, gdzie *{id}* to identyfikator książki.</span><span class="sxs-lookup"><span data-stu-id="88c19-205">To get book details, the client will send a GET request to `/api/books/{id}/details`, where *{id}* is the ID of the book.</span></span>

<span data-ttu-id="88c19-206">Dodaj następującą metodę do `BooksController` klasy.</span><span class="sxs-lookup"><span data-stu-id="88c19-206">Add the following method to the `BooksController` class.</span></span>

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample13.cs)]

<span data-ttu-id="88c19-207">Jeśli żądania `/api/books/1/details`, odpowiedź wygląda następująco:</span><span class="sxs-lookup"><span data-stu-id="88c19-207">If you request `/api/books/1/details`, the response looks like this:</span></span>

[!code-json[Main](create-a-rest-api-with-attribute-routing/samples/sample14.json)]

## <a name="get-books-by-genre"></a><span data-ttu-id="88c19-208">Pobieranie książki według gatunku</span><span class="sxs-lookup"><span data-stu-id="88c19-208">Get Books By Genre</span></span>

<span data-ttu-id="88c19-209">Aby uzyskać listę książek określonego rodzaju, klient wyśle żądanie GET w celu `/api/books/genre`, gdzie *gatunku* to nazwa tego gatunku.</span><span class="sxs-lookup"><span data-stu-id="88c19-209">To get a list of books in a specific genre, the client will send a GET request to `/api/books/genre`, where *genre* is the name of the genre.</span></span> <span data-ttu-id="88c19-210">(Na przykład `/api/books/fantasy`.)</span><span class="sxs-lookup"><span data-stu-id="88c19-210">(For example, `/api/books/fantasy`.)</span></span>

<span data-ttu-id="88c19-211">Dodaj następującą metodę do `BooksController`.</span><span class="sxs-lookup"><span data-stu-id="88c19-211">Add the following method to `BooksController`.</span></span>

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample15.cs)]

<span data-ttu-id="88c19-212">W tym miejscu możemy definiowania tras, która zawiera parametr {gatunku} w szablon identyfikatora URI.</span><span class="sxs-lookup"><span data-stu-id="88c19-212">Here we are defining a route that contains a {genre} parameter in the URI template.</span></span> <span data-ttu-id="88c19-213">Zauważ, że interfejs API sieci Web rozróżnienie tych dwóch identyfikatorów i kierowania ich do różnych metod:</span><span class="sxs-lookup"><span data-stu-id="88c19-213">Notice that Web API is able to distinguish these two URIs and route them to different methods:</span></span>

`/api/books/1`

`/api/books/fantasy`

<span data-ttu-id="88c19-214">To dlatego, że `GetBook` metoda zawiera ograniczenie, że segment "id" musi być wartością całkowitą z zakresu:</span><span class="sxs-lookup"><span data-stu-id="88c19-214">That's because the `GetBook` method includes a constraint that the "id" segment must be an integer value:</span></span>

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample16.cs?highlight=1)]

<span data-ttu-id="88c19-215">Jeśli żądania /api/books/fantasy odpowiedź wygląda następująco:</span><span class="sxs-lookup"><span data-stu-id="88c19-215">If you request /api/books/fantasy, the response looks like this:</span></span>

`[ { "Title": "Midnight Rain", "Author": "Ralls, Kim", "Genre": "Fantasy" }, { "Title": "Maeve Ascendant", "Author": "Corets, Eva", "Genre": "Fantasy" }, { "Title": "The Sundered Grail", "Author": "Corets, Eva", "Genre": "Fantasy" } ]`

## <a name="get-books-by-author"></a><span data-ttu-id="88c19-216">Pobieranie książki według autora</span><span class="sxs-lookup"><span data-stu-id="88c19-216">Get Books By Author</span></span>

<span data-ttu-id="88c19-217">Aby uzyskać listę książek dla danego autora, klient wyśle żądanie GET w celu `/api/authors/id/books`, gdzie *identyfikator* to identyfikator autora.</span><span class="sxs-lookup"><span data-stu-id="88c19-217">To get a list of a books for a particular author, the client will send a GET request to `/api/authors/id/books`, where *id* is the ID of the author.</span></span>

<span data-ttu-id="88c19-218">Dodaj następującą metodę do `BooksController`.</span><span class="sxs-lookup"><span data-stu-id="88c19-218">Add the following method to `BooksController`.</span></span>

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample17.cs)]

<span data-ttu-id="88c19-219">W tym przykładzie jest interesująca ponieważ &quot;książki&quot; jest traktowane zasobu podrzędnego &quot;autorzy&quot;.</span><span class="sxs-lookup"><span data-stu-id="88c19-219">This example is interesting because &quot;books&quot; is treated a child resource of &quot;authors&quot;.</span></span> <span data-ttu-id="88c19-220">Ten wzorzec jest bardzo częsty w interfejsów API RESTful.</span><span class="sxs-lookup"><span data-stu-id="88c19-220">This pattern is quite common in RESTful APIs.</span></span>

<span data-ttu-id="88c19-221">Tyldy (~) w szablonie trasy zastępuje prefiks trasy w **RoutePrefix** atrybutu.</span><span class="sxs-lookup"><span data-stu-id="88c19-221">The tilde (~) in the route template overrides the route prefix in the **RoutePrefix** attribute.</span></span>

## <a name="get-books-by-publication-date"></a><span data-ttu-id="88c19-222">Pobieranie książki według: Data publikacji</span><span class="sxs-lookup"><span data-stu-id="88c19-222">Get Books By Publication Date</span></span>

<span data-ttu-id="88c19-223">Aby uzyskać listę książki według daty publikacji, klient wyśle żądanie GET w celu `/api/books/date/yyyy-mm-dd`, gdzie *rrrr mm-dd* to dzień.</span><span class="sxs-lookup"><span data-stu-id="88c19-223">To get a list of books by publication date, the client will send a GET request to `/api/books/date/yyyy-mm-dd`, where *yyyy-mm-dd* is the date.</span></span>

<span data-ttu-id="88c19-224">Oto jeden ze sposobów, aby to zrobić:</span><span class="sxs-lookup"><span data-stu-id="88c19-224">Here is one way to do this:</span></span>

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample18.cs)]

<span data-ttu-id="88c19-225">`{pubdate:datetime}` Ograniczony jest parametr, aby dopasować **daty/godziny** wartość.</span><span class="sxs-lookup"><span data-stu-id="88c19-225">The `{pubdate:datetime}` parameter is constrained to match a **DateTime** value.</span></span> <span data-ttu-id="88c19-226">Ta funkcja działa, ale jest faktycznie mniej ograniczająca niż prosimy o poświęcenie.</span><span class="sxs-lookup"><span data-stu-id="88c19-226">This works, but it's actually more permissive than we'd like.</span></span> <span data-ttu-id="88c19-227">Na przykład następujące identyfikatory URI, również będą zgodne trasy:</span><span class="sxs-lookup"><span data-stu-id="88c19-227">For example, these URIs will also match the route:</span></span>

`/api/books/date/Thu, 01 May 2008`

`/api/books/date/2000-12-16T00:00:00`

<span data-ttu-id="88c19-228">Nie ma problem z zezwoleniem na te identyfikatory URI.</span><span class="sxs-lookup"><span data-stu-id="88c19-228">There's nothing wrong with allowing these URIs.</span></span> <span data-ttu-id="88c19-229">Może jednak ograniczyć trasy do określonego formatu, Dodawanie ograniczenia wyrażenia regularnego do szablonu trasy:</span><span class="sxs-lookup"><span data-stu-id="88c19-229">However, you can restrict the route to a particular format by adding a regular-expression constraint to the route template:</span></span>

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample19.cs?highlight=1)]

<span data-ttu-id="88c19-230">Teraz tylko daty w postaci &quot;rrrr mm-dd&quot; będą zgodne.</span><span class="sxs-lookup"><span data-stu-id="88c19-230">Now only dates in the form &quot;yyyy-mm-dd&quot; will match.</span></span> <span data-ttu-id="88c19-231">Należy zauważyć, że nie używamy wyrażenia regularnego można zweryfikować, że mamy rzeczywiste daty.</span><span class="sxs-lookup"><span data-stu-id="88c19-231">Notice that we don't use the regex to validate that we got a real date.</span></span> <span data-ttu-id="88c19-232">Który odbywa się w przypadku interfejsu API sieci Web podejmuje próbę przekonwertowania segmentem identyfikatora URI do **daty/godziny** wystąpienia.</span><span class="sxs-lookup"><span data-stu-id="88c19-232">That is handled when Web API tries to convert the URI segment into a **DateTime** instance.</span></span> <span data-ttu-id="88c19-233">Nieprawidłowa data, takie jak "2012-47-99" zakończy się niepowodzeniem ma zostać przekonwertowany, a klient otrzyma błąd 404.</span><span class="sxs-lookup"><span data-stu-id="88c19-233">An invalid date such as '2012-47-99' will fail to be converted, and the client will get a 404 error.</span></span>

<span data-ttu-id="88c19-234">Może również obsługiwać separator ukośnika (`/api/books/date/yyyy/mm/dd`) przez dodanie innej **[trasy]** atrybut o inne wyrażenie regularne.</span><span class="sxs-lookup"><span data-stu-id="88c19-234">You can also support a slash separator (`/api/books/date/yyyy/mm/dd`) by adding another **[Route]** attribute with a different regex.</span></span>

[!code-html[Main](create-a-rest-api-with-attribute-routing/samples/sample20.html)]

<span data-ttu-id="88c19-235">Brak subtelne, ale ważne szczegóły poniżej.</span><span class="sxs-lookup"><span data-stu-id="88c19-235">There is a subtle but important detail here.</span></span> <span data-ttu-id="88c19-236">Drugi szablon trasy zawiera symbol wieloznaczny (\*) na początku parametru {pubdate}:</span><span class="sxs-lookup"><span data-stu-id="88c19-236">The second route template has a wildcard character (\*) at the start of the {pubdate} parameter:</span></span>

[!code-json[Main](create-a-rest-api-with-attribute-routing/samples/sample21.json)]

<span data-ttu-id="88c19-237">Informuje aparat routingu, że {pubdate} powinien być zgodny z pozostałą część identyfikatora URI.</span><span class="sxs-lookup"><span data-stu-id="88c19-237">This tells the routing engine that {pubdate} should match the rest of the URI.</span></span> <span data-ttu-id="88c19-238">Domyślnie parametrem szablonu dopasowuje pojedynczy segment identyfikatora URI.</span><span class="sxs-lookup"><span data-stu-id="88c19-238">By default, a template parameter matches a single URI segment.</span></span> <span data-ttu-id="88c19-239">W tym przypadku chcemy {pubdate} obejmować wiele segmentów identyfikatora URI:</span><span class="sxs-lookup"><span data-stu-id="88c19-239">In this case, we want {pubdate} to span several URI segments:</span></span>

`/api/books/date/2013/06/17`

## <a name="controller-code"></a><span data-ttu-id="88c19-240">Kod kontrolera</span><span class="sxs-lookup"><span data-stu-id="88c19-240">Controller Code</span></span>

<span data-ttu-id="88c19-241">Oto kompletny kod dla klasy BooksController.</span><span class="sxs-lookup"><span data-stu-id="88c19-241">Here is the complete code for the BooksController class.</span></span>

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample22.cs)]

## <a name="summary"></a><span data-ttu-id="88c19-242">Podsumowanie</span><span class="sxs-lookup"><span data-stu-id="88c19-242">Summary</span></span>

<span data-ttu-id="88c19-243">Trasowanie atrybutów zapewnia więcej kontroli i większą elastyczność podczas projektowania identyfikatorów URI dla interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="88c19-243">Attribute routing gives you more control and greater flexibility when designing the URIs for your API.</span></span>
