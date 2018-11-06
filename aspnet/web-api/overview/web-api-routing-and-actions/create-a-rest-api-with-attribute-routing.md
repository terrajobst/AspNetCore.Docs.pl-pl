---
uid: web-api/overview/web-api-routing-and-actions/create-a-rest-api-with-attribute-routing
title: Tworzenie interfejsu API REST przy użyciu atrybutu routingu we wzorcu ASP.NET Web API 2 | Dokumentacja firmy Microsoft
author: MikeWasson
description: ''
ms.author: riande
ms.date: 06/26/2013
ms.assetid: 23fc77da-2725-4434-99a0-ff872d96336b
msc.legacyurl: /web-api/overview/web-api-routing-and-actions/create-a-rest-api-with-attribute-routing
msc.type: authoredcontent
ms.openlocfilehash: 18a44c280e6df1603837938d24d7d639d8c87cc2
ms.sourcegitcommit: 2d3e5422d530203efdaf2014d1d7df31f88d08d0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/05/2018
ms.locfileid: "51021420"
---
<a name="create-a-rest-api-with-attribute-routing-in-aspnet-web-api-2"></a><span data-ttu-id="dbda7-102">Tworzenie interfejsu API REST z atrybutem routingu we wzorcu ASP.NET Web API 2</span><span class="sxs-lookup"><span data-stu-id="dbda7-102">Create a REST API with Attribute Routing in ASP.NET Web API 2</span></span>
====================
<span data-ttu-id="dbda7-103">przez [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="dbda7-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="dbda7-104">Składnik Web API 2 obsługuje nowy typ routingu, o nazwie *trasowanie atrybutów*.</span><span class="sxs-lookup"><span data-stu-id="dbda7-104">Web API 2 supports a new type of routing, called *attribute routing*.</span></span> <span data-ttu-id="dbda7-105">Aby uzyskać ogólne omówienie trasowanie atrybutów, zobacz [atrybut routingu w sieci Web API 2](attribute-routing-in-web-api-2.md).</span><span class="sxs-lookup"><span data-stu-id="dbda7-105">For a general overview of attribute routing, see [Attribute Routing in Web API 2](attribute-routing-in-web-api-2.md).</span></span> <span data-ttu-id="dbda7-106">W tym samouczku użyjesz trasowanie atrybutów do tworzenia interfejsu API REST dla kolekcji książek.</span><span class="sxs-lookup"><span data-stu-id="dbda7-106">In this tutorial, you will use attribute routing to create a REST API for a collection of books.</span></span> <span data-ttu-id="dbda7-107">Interfejs API obsługuje następujące akcje:</span><span class="sxs-lookup"><span data-stu-id="dbda7-107">The API will support the following actions:</span></span>

| <span data-ttu-id="dbda7-108">Akcja</span><span class="sxs-lookup"><span data-stu-id="dbda7-108">Action</span></span> | <span data-ttu-id="dbda7-109">Przykład identyfikatora URI</span><span class="sxs-lookup"><span data-stu-id="dbda7-109">Example URI</span></span> |
| --- | --- |
| <span data-ttu-id="dbda7-110">Zostanie wyświetlona lista wszystkich książek.</span><span class="sxs-lookup"><span data-stu-id="dbda7-110">Get a list of all books.</span></span> | <span data-ttu-id="dbda7-111">/ api/książki</span><span class="sxs-lookup"><span data-stu-id="dbda7-111">/api/books</span></span> |
| <span data-ttu-id="dbda7-112">Pobierz książkę według identyfikatora.</span><span class="sxs-lookup"><span data-stu-id="dbda7-112">Get a book by ID.</span></span> | <span data-ttu-id="dbda7-113">/API/Books/1</span><span class="sxs-lookup"><span data-stu-id="dbda7-113">/api/books/1</span></span> |
| <span data-ttu-id="dbda7-114">Pobieranie szczegółów książki.</span><span class="sxs-lookup"><span data-stu-id="dbda7-114">Get the details of a book.</span></span> | <span data-ttu-id="dbda7-115">/API/Books/1/details</span><span class="sxs-lookup"><span data-stu-id="dbda7-115">/api/books/1/details</span></span> |
| <span data-ttu-id="dbda7-116">Zostanie wyświetlona lista książek według gatunku.</span><span class="sxs-lookup"><span data-stu-id="dbda7-116">Get a list of books by genre.</span></span> | <span data-ttu-id="dbda7-117">/API/Books/fantasy</span><span class="sxs-lookup"><span data-stu-id="dbda7-117">/api/books/fantasy</span></span> |
| <span data-ttu-id="dbda7-118">Zostanie wyświetlona lista książek według daty publikacji.</span><span class="sxs-lookup"><span data-stu-id="dbda7-118">Get a list of books by publication date.</span></span> | <span data-ttu-id="dbda7-119">/api/books/date/2013/02/16 /API/Books/Date/2013-02-16 (alternatywna postać)</span><span class="sxs-lookup"><span data-stu-id="dbda7-119">/api/books/date/2013-02-16 /api/books/date/2013/02/16 (alternate form)</span></span> |
| <span data-ttu-id="dbda7-120">Zostanie wyświetlona lista książek przez określonego autora.</span><span class="sxs-lookup"><span data-stu-id="dbda7-120">Get a list of books by a particular author.</span></span> | <span data-ttu-id="dbda7-121">/API/authors/1/Books</span><span class="sxs-lookup"><span data-stu-id="dbda7-121">/api/authors/1/books</span></span> |

<span data-ttu-id="dbda7-122">Wszystkie metody są tylko do odczytu (żądania HTTP GET).</span><span class="sxs-lookup"><span data-stu-id="dbda7-122">All methods are read-only (HTTP GET requests).</span></span>

<span data-ttu-id="dbda7-123">Dla warstwy danych użyto programu Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="dbda7-123">For the data layer, we'll use Entity Framework.</span></span> <span data-ttu-id="dbda7-124">Rekordy książek będzie zawierać następujące pola:</span><span class="sxs-lookup"><span data-stu-id="dbda7-124">Book records will have the following fields:</span></span>

- <span data-ttu-id="dbda7-125">ID</span><span class="sxs-lookup"><span data-stu-id="dbda7-125">ID</span></span>
- <span data-ttu-id="dbda7-126">Tytuł</span><span class="sxs-lookup"><span data-stu-id="dbda7-126">Title</span></span>
- <span data-ttu-id="dbda7-127">Gatunku</span><span class="sxs-lookup"><span data-stu-id="dbda7-127">Genre</span></span>
- <span data-ttu-id="dbda7-128">Data publikacji</span><span class="sxs-lookup"><span data-stu-id="dbda7-128">Publication date</span></span>
- <span data-ttu-id="dbda7-129">Cena</span><span class="sxs-lookup"><span data-stu-id="dbda7-129">Price</span></span>
- <span data-ttu-id="dbda7-130">Opis</span><span class="sxs-lookup"><span data-stu-id="dbda7-130">Description</span></span>
- <span data-ttu-id="dbda7-131">Wartość IDAutora (klucz obcy tabeli Autorzy)</span><span class="sxs-lookup"><span data-stu-id="dbda7-131">AuthorID (foreign key to an Authors table)</span></span>

<span data-ttu-id="dbda7-132">W większości żądań, natomiast interfejs API zwróci podzbiór danych (tytuł, autor i gatunku).</span><span class="sxs-lookup"><span data-stu-id="dbda7-132">For most requests, however, the API will return a subset of this data (title, author, and genre).</span></span> <span data-ttu-id="dbda7-133">Do żądania get całego rekordu klienta `/api/books/{id}/details`.</span><span class="sxs-lookup"><span data-stu-id="dbda7-133">To get the complete record, the client requests `/api/books/{id}/details`.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="dbda7-134">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="dbda7-134">Prerequisites</span></span>

<span data-ttu-id="dbda7-135">[Program Visual Studio 2017](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017) Community, Professional lub Enterprise edition.</span><span class="sxs-lookup"><span data-stu-id="dbda7-135">[Visual Studio 2017](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017) Community, Professional or Enterprise edition.</span></span>

## <a name="create-the-visual-studio-project"></a><span data-ttu-id="dbda7-136">Tworzenie projektu programu Visual Studio</span><span class="sxs-lookup"><span data-stu-id="dbda7-136">Create the Visual Studio Project</span></span>

<span data-ttu-id="dbda7-137">Rozpocznij od uruchamianie programu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="dbda7-137">Start by running Visual Studio.</span></span> <span data-ttu-id="dbda7-138">Z **pliku** menu, wybierz opcję **New** , a następnie wybierz **projektu**.</span><span class="sxs-lookup"><span data-stu-id="dbda7-138">From the **File** menu, select **New** and then select **Project**.</span></span>

<span data-ttu-id="dbda7-139">Rozwiń **zainstalowane** > **Visual C#** kategorii.</span><span class="sxs-lookup"><span data-stu-id="dbda7-139">Expand the **Installed** > **Visual C#** category.</span></span> <span data-ttu-id="dbda7-140">W obszarze **Visual C#**, wybierz pozycję **Sieć Web**.</span><span class="sxs-lookup"><span data-stu-id="dbda7-140">Under **Visual C#**, select **Web**.</span></span> <span data-ttu-id="dbda7-141">Na liście szablonów projektu wybierz **aplikacji sieci Web platformy ASP.NET (.NET Framework)**.</span><span class="sxs-lookup"><span data-stu-id="dbda7-141">In the list of project templates, select **ASP.NET Web Application (.NET Framework)**.</span></span> <span data-ttu-id="dbda7-142">Nadaj projektowi nazwę &quot;BooksAPI&quot;.</span><span class="sxs-lookup"><span data-stu-id="dbda7-142">Name the project &quot;BooksAPI&quot;.</span></span>

![](create-a-rest-api-with-attribute-routing/_static/image1.png)

<span data-ttu-id="dbda7-143">W **Nowa aplikacja internetowa ASP.NET** okno dialogowe, wybierz opcję **pusty** szablonu.</span><span class="sxs-lookup"><span data-stu-id="dbda7-143">In the **New ASP.NET Web Application** dialog, select the **Empty** template.</span></span> <span data-ttu-id="dbda7-144">W obszarze "Dodaj foldery i podstawowe odwołania dla" Wybierz **interfejsu API sieci Web** pola wyboru.</span><span class="sxs-lookup"><span data-stu-id="dbda7-144">Under "Add folders and core references for", select the **Web API** checkbox.</span></span> <span data-ttu-id="dbda7-145">Kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="dbda7-145">Click **OK**.</span></span>

![](create-a-rest-api-with-attribute-routing/_static/image2.png)

<span data-ttu-id="dbda7-146">Spowoduje to utworzenie szkielet projektu, który jest skonfigurowany do obsługi funkcji interfejsu API sieci Web.</span><span class="sxs-lookup"><span data-stu-id="dbda7-146">This creates a skeleton project that is configured for Web API functionality.</span></span>

### <a name="domain-models"></a><span data-ttu-id="dbda7-147">Modeli domeny</span><span class="sxs-lookup"><span data-stu-id="dbda7-147">Domain Models</span></span>

<span data-ttu-id="dbda7-148">Następnie Dodaj klasy dla modeli domeny.</span><span class="sxs-lookup"><span data-stu-id="dbda7-148">Next, add classes for domain models.</span></span> <span data-ttu-id="dbda7-149">W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy folder Modele.</span><span class="sxs-lookup"><span data-stu-id="dbda7-149">In Solution Explorer, right-click the Models folder.</span></span> <span data-ttu-id="dbda7-150">Wybierz **Dodaj**, a następnie wybierz **klasy**.</span><span class="sxs-lookup"><span data-stu-id="dbda7-150">Select **Add**, then select **Class**.</span></span> <span data-ttu-id="dbda7-151">Nazwa klasy `Author`.</span><span class="sxs-lookup"><span data-stu-id="dbda7-151">Name the class `Author`.</span></span>

![](create-a-rest-api-with-attribute-routing/_static/image3.png)

<span data-ttu-id="dbda7-152">Zastąp kod w Author.cs następujących czynności:</span><span class="sxs-lookup"><span data-stu-id="dbda7-152">Replace the code in Author.cs with the following:</span></span>

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample1.cs)]

<span data-ttu-id="dbda7-153">Teraz Dodaj klasę o nazwie `Book`.</span><span class="sxs-lookup"><span data-stu-id="dbda7-153">Now add another class named `Book`.</span></span>

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample2.cs)]

### <a name="add-a-web-api-controller"></a><span data-ttu-id="dbda7-154">Dodaj Kontroler interfejsu API sieci Web</span><span class="sxs-lookup"><span data-stu-id="dbda7-154">Add a Web API Controller</span></span>

<span data-ttu-id="dbda7-155">W tym kroku dodasz kontroler internetowego interfejsu API, który używa programu Entity Framework jako warstwa danych.</span><span class="sxs-lookup"><span data-stu-id="dbda7-155">In this step, we'll add a Web API controller that uses Entity Framework as the data layer.</span></span>

<span data-ttu-id="dbda7-156">Naciśnij kombinację klawiszy CTRL+SHIFT+B. Projekt zostanie skompilowany.</span><span class="sxs-lookup"><span data-stu-id="dbda7-156">Press CTRL+SHIFT+B to build the project.</span></span> <span data-ttu-id="dbda7-157">Entity Framework używa odbicia, aby odnaleźć właściwości te modele, dzięki czemu wymaga skompilowanego zestawu w celu utworzenia schematu bazy danych.</span><span class="sxs-lookup"><span data-stu-id="dbda7-157">Entity Framework uses reflection to discover the properties of the models, so it requires a compiled assembly to create the database schema.</span></span>

<span data-ttu-id="dbda7-158">W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy folder kontrolerów.</span><span class="sxs-lookup"><span data-stu-id="dbda7-158">In Solution Explorer, right-click the Controllers folder.</span></span> <span data-ttu-id="dbda7-159">Wybierz **Dodaj**, a następnie wybierz **kontrolera**.</span><span class="sxs-lookup"><span data-stu-id="dbda7-159">Select **Add**, then select **Controller**.</span></span>

![](create-a-rest-api-with-attribute-routing/_static/image4.png)

<span data-ttu-id="dbda7-160">W **Dodawanie szkieletu** okno dialogowe, wybierz opcję **kontroler internetowego interfejsu API 2 z akcjami używający narzędzia Entity Framework**.</span><span class="sxs-lookup"><span data-stu-id="dbda7-160">In the **Add Scaffold** dialog, select **Web API 2 Controller with actions, using Entity Framework**.</span></span>

[![](create-a-rest-api-with-attribute-routing/_static/image6.png)](create-a-rest-api-with-attribute-routing/_static/image5.png)

<span data-ttu-id="dbda7-161">W **Dodaj kontroler** okna dialogowego, aby uzyskać **nazwy kontrolera**, wprowadź &quot;BooksController&quot;.</span><span class="sxs-lookup"><span data-stu-id="dbda7-161">In the **Add Controller** dialog, for **Controller name**, enter &quot;BooksController&quot;.</span></span> <span data-ttu-id="dbda7-162">Wybierz &quot;używać asynchronicznych akcji kontrolera&quot; pola wyboru.</span><span class="sxs-lookup"><span data-stu-id="dbda7-162">Select the &quot;Use async controller actions&quot; checkbox.</span></span> <span data-ttu-id="dbda7-163">Aby uzyskać **klasa modelu**, wybierz opcję &quot;książki&quot;.</span><span class="sxs-lookup"><span data-stu-id="dbda7-163">For **Model class**, select &quot;Book&quot;.</span></span> <span data-ttu-id="dbda7-164">(Jeśli nie widzisz `Book` klasy na liście rozwijanej, upewnij się, że utworzony projekt.) Następnie kliknij przycisk "+".</span><span class="sxs-lookup"><span data-stu-id="dbda7-164">(If you don't see the `Book` class listed in the dropdown, make sure that you built the project.) Then click the "+" button.</span></span>

![](create-a-rest-api-with-attribute-routing/_static/image7.png)

<span data-ttu-id="dbda7-165">Kliknij przycisk **Dodaj** w **nowy kontekst danych** okna dialogowego.</span><span class="sxs-lookup"><span data-stu-id="dbda7-165">Click **Add** in the **New Data Context** dialog.</span></span>

![](create-a-rest-api-with-attribute-routing/_static/image8.png)

<span data-ttu-id="dbda7-166">Kliknij przycisk **Dodaj** w **Dodaj kontroler** okna dialogowego.</span><span class="sxs-lookup"><span data-stu-id="dbda7-166">Click **Add** in the **Add Controller** dialog.</span></span> <span data-ttu-id="dbda7-167">Szkieletu Dodaje klasę o nazwie `BooksController` definiujący Kontroler interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="dbda7-167">The scaffolding adds a class named `BooksController` that defines the API controller.</span></span> <span data-ttu-id="dbda7-168">Dodaje również klasę o nazwie `BooksAPIContext` w folderze modeli, która definiuje kontekst danych Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="dbda7-168">It also adds a class named `BooksAPIContext` in the Models folder, which defines the data context for Entity Framework.</span></span>

![](create-a-rest-api-with-attribute-routing/_static/image9.png)

### <a name="seed-the-database"></a><span data-ttu-id="dbda7-169">Inicjowanie bazy danych</span><span class="sxs-lookup"><span data-stu-id="dbda7-169">Seed the Database</span></span>

<span data-ttu-id="dbda7-170">Wybierz z menu narzędzia **Menedżera pakietów NuGet**, a następnie wybierz pozycję **Konsola Menedżera pakietów**.</span><span class="sxs-lookup"><span data-stu-id="dbda7-170">From the Tools menu, select **NuGet Package Manager**, and then select **Package Manager Console**.</span></span>

<span data-ttu-id="dbda7-171">W oknie Konsola Menedżera pakietów wprowadź następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="dbda7-171">In the Package Manager Console window, enter the following command:</span></span>

[!code-powershell[Main](create-a-rest-api-with-attribute-routing/samples/sample3.ps1)]

<span data-ttu-id="dbda7-172">To polecenie tworzy folder migracje i dodaje nowy plik kodu o nazwie Configuration.cs.</span><span class="sxs-lookup"><span data-stu-id="dbda7-172">This command creates a Migrations folder and adds a new code file named Configuration.cs.</span></span> <span data-ttu-id="dbda7-173">Otwórz ten plik i Dodaj następujący kod do `Configuration.Seed` metody.</span><span class="sxs-lookup"><span data-stu-id="dbda7-173">Open this file and add the following code to the `Configuration.Seed` method.</span></span>

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample4.cs)]

<span data-ttu-id="dbda7-174">W oknie Konsola Menedżera pakietów wpisz następujące polecenia.</span><span class="sxs-lookup"><span data-stu-id="dbda7-174">In the Package Manager Console window, type the following commands.</span></span>

[!code-powershell[Main](create-a-rest-api-with-attribute-routing/samples/sample5.ps1)]

<span data-ttu-id="dbda7-175">Te polecenia Utwórz lokalnej bazy danych i wywołaj metodę inicjatora do wypełniania bazy danych.</span><span class="sxs-lookup"><span data-stu-id="dbda7-175">These commands create a local database and invoke the Seed method to populate the database.</span></span>

![](create-a-rest-api-with-attribute-routing/_static/image10.png)

## <a name="add-dto-classes"></a><span data-ttu-id="dbda7-176">Dodaj obiekt DTO klasy</span><span class="sxs-lookup"><span data-stu-id="dbda7-176">Add DTO Classes</span></span>

<span data-ttu-id="dbda7-177">Uruchom aplikację teraz, Wyślij żądanie Pobierz do /api/books/1 odpowiedź wygląda podobnie do następującego.</span><span class="sxs-lookup"><span data-stu-id="dbda7-177">If you run the application now and send a GET request to /api/books/1, the response looks similar to the following.</span></span> <span data-ttu-id="dbda7-178">(Po dodaniu wcięć dla czytelności.)</span><span class="sxs-lookup"><span data-stu-id="dbda7-178">(I added indentation for readability.)</span></span>

[!code-json[Main](create-a-rest-api-with-attribute-routing/samples/sample6.json)]

<span data-ttu-id="dbda7-179">Zamiast tego chcę tego żądania, aby zwrócić podzbiór pól.</span><span class="sxs-lookup"><span data-stu-id="dbda7-179">Instead, I want this request to return a subset of the fields.</span></span> <span data-ttu-id="dbda7-180">Ponadto powinna być zwraca imię i nazwisko autora, a nie identyfikatora autora.</span><span class="sxs-lookup"><span data-stu-id="dbda7-180">Also, I want it to return the author's name, rather than the author ID.</span></span> <span data-ttu-id="dbda7-181">Aby to osiągnąć, zmodyfikujemy metody kontrolera, aby zwrócić *obiekt transferu danych* (DTO) zamiast modelu platformy EF.</span><span class="sxs-lookup"><span data-stu-id="dbda7-181">To accomplish this, we'll modify the controller methods to return a *data transfer object* (DTO) instead of the EF model.</span></span> <span data-ttu-id="dbda7-182">Obiekt DTO jest obiektem, który jest przeznaczony tylko do przesyłania danych.</span><span class="sxs-lookup"><span data-stu-id="dbda7-182">A DTO is an object that is designed only to carry data.</span></span>

<span data-ttu-id="dbda7-183">W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy projekt i wybierz **Dodaj** | **nowy Folder**.</span><span class="sxs-lookup"><span data-stu-id="dbda7-183">In Solution Explorer, right-click the project and select **Add** | **New Folder**.</span></span> <span data-ttu-id="dbda7-184">Nazwa folderu &quot;dto&quot;.</span><span class="sxs-lookup"><span data-stu-id="dbda7-184">Name the folder &quot;DTOs&quot;.</span></span> <span data-ttu-id="dbda7-185">Dodaj klasę o nazwie `BookDto` do folderu dto, przy użyciu następujących definicji:</span><span class="sxs-lookup"><span data-stu-id="dbda7-185">Add a class named `BookDto` to the DTOs folder, with the following definition:</span></span>

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample7.cs)]

<span data-ttu-id="dbda7-186">Dodaj klasę o nazwie `BookDetailDto`.</span><span class="sxs-lookup"><span data-stu-id="dbda7-186">Add another class named `BookDetailDto`.</span></span>

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample8.cs)]

<span data-ttu-id="dbda7-187">Następnie zaktualizuj `BooksController` klasy w celu zwracania `BookDto` wystąpień.</span><span class="sxs-lookup"><span data-stu-id="dbda7-187">Next, update the `BooksController` class to return `BookDto` instances.</span></span> <span data-ttu-id="dbda7-188">Użyjemy [Queryable.Select](https://msdn.microsoft.com/library/system.linq.queryable.select.aspx) metody do projektu `Book` wystąpień do `BookDto` wystąpień.</span><span class="sxs-lookup"><span data-stu-id="dbda7-188">We'll use the [Queryable.Select](https://msdn.microsoft.com/library/system.linq.queryable.select.aspx) method to project `Book` instances to `BookDto` instances.</span></span> <span data-ttu-id="dbda7-189">Oto zaktualizowany kod do klasy kontrolera.</span><span class="sxs-lookup"><span data-stu-id="dbda7-189">Here is the updated code for the controller class.</span></span>

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample9.cs)]

> [!NOTE]
> <span data-ttu-id="dbda7-190">Po usunięciu `PutBook`, `PostBook`, i `DeleteBook` metod, ponieważ nie są one potrzebne w tym samouczku.</span><span class="sxs-lookup"><span data-stu-id="dbda7-190">I deleted the `PutBook`, `PostBook`, and `DeleteBook` methods, because they aren't needed for this tutorial.</span></span>


<span data-ttu-id="dbda7-191">Teraz uruchom aplikację, żądanie /api/books/1 treść odpowiedzi powinna wyglądać następująco:</span><span class="sxs-lookup"><span data-stu-id="dbda7-191">Now if you run the application and request /api/books/1, the response body should look like this:</span></span>

[!code-json[Main](create-a-rest-api-with-attribute-routing/samples/sample10.json)]

## <a name="add-route-attributes"></a><span data-ttu-id="dbda7-192">Dodawanie atrybutów trasy</span><span class="sxs-lookup"><span data-stu-id="dbda7-192">Add Route Attributes</span></span>

<span data-ttu-id="dbda7-193">Firma Microsoft będzie następnie przekonwertować kontroler trasowanie atrybutów.</span><span class="sxs-lookup"><span data-stu-id="dbda7-193">Next, we'll convert the controller to use attribute routing.</span></span> <span data-ttu-id="dbda7-194">Najpierw dodaj **RoutePrefix** atrybutu do kontrolera.</span><span class="sxs-lookup"><span data-stu-id="dbda7-194">First, add a **RoutePrefix** attribute to the controller.</span></span> <span data-ttu-id="dbda7-195">Ten atrybut definiuje początkowe segmentów identyfikator URI dla wszystkich metod na tym kontrolerze.</span><span class="sxs-lookup"><span data-stu-id="dbda7-195">This attribute defines the initial URI segments for all methods on this controller.</span></span>

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample11.cs?highlight=1)]

<span data-ttu-id="dbda7-196">Następnie dodaj **[trasy]** atrybuty do akcji kontrolera, w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="dbda7-196">Then add **[Route]** attributes to the controller actions, as follows:</span></span>

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample12.cs?highlight=1,7)]

<span data-ttu-id="dbda7-197">Szablon trasy dla każdej metody kontrolera jest prefiksem, a także ciąg określony w **trasy** atrybutu.</span><span class="sxs-lookup"><span data-stu-id="dbda7-197">The route template for each controller method is the prefix plus the string specified in the **Route** attribute.</span></span> <span data-ttu-id="dbda7-198">Dla `GetBook` , szablon trasy zawiera ciągu sparametryzowanego &quot;{identyfikator: int}&quot;, która pasuje, jeśli segmentem identyfikatora URI zawiera wartość całkowitą.</span><span class="sxs-lookup"><span data-stu-id="dbda7-198">For the `GetBook` method, the route template includes the parameterized string &quot;{id:int}&quot;, which matches if the URI segment contains an integer value.</span></span>

| <span data-ttu-id="dbda7-199">Metoda</span><span class="sxs-lookup"><span data-stu-id="dbda7-199">Method</span></span> | <span data-ttu-id="dbda7-200">Szablon trasy</span><span class="sxs-lookup"><span data-stu-id="dbda7-200">Route Template</span></span> | <span data-ttu-id="dbda7-201">Przykład identyfikatora URI</span><span class="sxs-lookup"><span data-stu-id="dbda7-201">Example URI</span></span> |
| --- | --- | --- |
| `GetBooks` | <span data-ttu-id="dbda7-202">"interfejs api/books"</span><span class="sxs-lookup"><span data-stu-id="dbda7-202">"api/books"</span></span> | `http://localhost/api/books` |
| `GetBook` | <span data-ttu-id="dbda7-203">"interfejs api/książki / {identyfikator: int}"</span><span class="sxs-lookup"><span data-stu-id="dbda7-203">"api/books/{id:int}"</span></span> | `http://localhost/api/books/5` |

## <a name="get-book-details"></a><span data-ttu-id="dbda7-204">Pobieranie szczegółów książki</span><span class="sxs-lookup"><span data-stu-id="dbda7-204">Get Book Details</span></span>

<span data-ttu-id="dbda7-205">Aby uzyskać szczegółowe informacje z książki, klient wyśle żądanie GET w celu `/api/books/{id}/details`, gdzie *{id}* to identyfikator książki.</span><span class="sxs-lookup"><span data-stu-id="dbda7-205">To get book details, the client will send a GET request to `/api/books/{id}/details`, where *{id}* is the ID of the book.</span></span>

<span data-ttu-id="dbda7-206">Dodaj następującą metodę do `BooksController` klasy.</span><span class="sxs-lookup"><span data-stu-id="dbda7-206">Add the following method to the `BooksController` class.</span></span>

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample13.cs)]

<span data-ttu-id="dbda7-207">Jeśli żądania `/api/books/1/details`, odpowiedź wygląda następująco:</span><span class="sxs-lookup"><span data-stu-id="dbda7-207">If you request `/api/books/1/details`, the response looks like this:</span></span>

[!code-json[Main](create-a-rest-api-with-attribute-routing/samples/sample14.json)]

## <a name="get-books-by-genre"></a><span data-ttu-id="dbda7-208">Pobieranie książki według gatunku</span><span class="sxs-lookup"><span data-stu-id="dbda7-208">Get Books By Genre</span></span>

<span data-ttu-id="dbda7-209">Aby uzyskać listę książek określonego rodzaju, klient wyśle żądanie GET w celu `/api/books/genre`, gdzie *gatunku* to nazwa tego gatunku.</span><span class="sxs-lookup"><span data-stu-id="dbda7-209">To get a list of books in a specific genre, the client will send a GET request to `/api/books/genre`, where *genre* is the name of the genre.</span></span> <span data-ttu-id="dbda7-210">(Na przykład `/api/books/fantasy`.)</span><span class="sxs-lookup"><span data-stu-id="dbda7-210">(For example, `/api/books/fantasy`.)</span></span>

<span data-ttu-id="dbda7-211">Dodaj następującą metodę do `BooksController`.</span><span class="sxs-lookup"><span data-stu-id="dbda7-211">Add the following method to `BooksController`.</span></span>

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample15.cs)]

<span data-ttu-id="dbda7-212">W tym miejscu możemy definiowania tras, która zawiera parametr {gatunku} w szablon identyfikatora URI.</span><span class="sxs-lookup"><span data-stu-id="dbda7-212">Here we are defining a route that contains a {genre} parameter in the URI template.</span></span> <span data-ttu-id="dbda7-213">Zauważ, że interfejs API sieci Web rozróżnienie tych dwóch identyfikatorów i kierowania ich do różnych metod:</span><span class="sxs-lookup"><span data-stu-id="dbda7-213">Notice that Web API is able to distinguish these two URIs and route them to different methods:</span></span>

`/api/books/1`

`/api/books/fantasy`

<span data-ttu-id="dbda7-214">To dlatego, że `GetBook` metoda zawiera ograniczenie, że segment "id" musi być wartością całkowitą z zakresu:</span><span class="sxs-lookup"><span data-stu-id="dbda7-214">That's because the `GetBook` method includes a constraint that the "id" segment must be an integer value:</span></span>

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample16.cs?highlight=1)]

<span data-ttu-id="dbda7-215">Jeśli żądania /api/books/fantasy odpowiedź wygląda następująco:</span><span class="sxs-lookup"><span data-stu-id="dbda7-215">If you request /api/books/fantasy, the response looks like this:</span></span>

`[ { "Title": "Midnight Rain", "Author": "Ralls, Kim", "Genre": "Fantasy" }, { "Title": "Maeve Ascendant", "Author": "Corets, Eva", "Genre": "Fantasy" }, { "Title": "The Sundered Grail", "Author": "Corets, Eva", "Genre": "Fantasy" } ]`

## <a name="get-books-by-author"></a><span data-ttu-id="dbda7-216">Pobieranie książki według autora</span><span class="sxs-lookup"><span data-stu-id="dbda7-216">Get Books By Author</span></span>

<span data-ttu-id="dbda7-217">Aby uzyskać listę książek dla danego autora, klient wyśle żądanie GET w celu `/api/authors/id/books`, gdzie *identyfikator* to identyfikator autora.</span><span class="sxs-lookup"><span data-stu-id="dbda7-217">To get a list of a books for a particular author, the client will send a GET request to `/api/authors/id/books`, where *id* is the ID of the author.</span></span>

<span data-ttu-id="dbda7-218">Dodaj następującą metodę do `BooksController`.</span><span class="sxs-lookup"><span data-stu-id="dbda7-218">Add the following method to `BooksController`.</span></span>

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample17.cs)]

<span data-ttu-id="dbda7-219">W tym przykładzie jest interesująca ponieważ &quot;książki&quot; jest traktowane zasobu podrzędnego &quot;autorzy&quot;.</span><span class="sxs-lookup"><span data-stu-id="dbda7-219">This example is interesting because &quot;books&quot; is treated a child resource of &quot;authors&quot;.</span></span> <span data-ttu-id="dbda7-220">Ten wzorzec jest bardzo częsty w interfejsów API RESTful.</span><span class="sxs-lookup"><span data-stu-id="dbda7-220">This pattern is quite common in RESTful APIs.</span></span>

<span data-ttu-id="dbda7-221">Tyldy (~) w szablonie trasy zastępuje prefiks trasy w **RoutePrefix** atrybutu.</span><span class="sxs-lookup"><span data-stu-id="dbda7-221">The tilde (~) in the route template overrides the route prefix in the **RoutePrefix** attribute.</span></span>

## <a name="get-books-by-publication-date"></a><span data-ttu-id="dbda7-222">Pobieranie książki według: Data publikacji</span><span class="sxs-lookup"><span data-stu-id="dbda7-222">Get Books By Publication Date</span></span>

<span data-ttu-id="dbda7-223">Aby uzyskać listę książki według daty publikacji, klient wyśle żądanie GET w celu `/api/books/date/yyyy-mm-dd`, gdzie *rrrr mm-dd* to dzień.</span><span class="sxs-lookup"><span data-stu-id="dbda7-223">To get a list of books by publication date, the client will send a GET request to `/api/books/date/yyyy-mm-dd`, where *yyyy-mm-dd* is the date.</span></span>

<span data-ttu-id="dbda7-224">Oto jeden ze sposobów, aby to zrobić:</span><span class="sxs-lookup"><span data-stu-id="dbda7-224">Here is one way to do this:</span></span>

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample18.cs)]

<span data-ttu-id="dbda7-225">`{pubdate:datetime}` Ograniczony jest parametr, aby dopasować **daty/godziny** wartość.</span><span class="sxs-lookup"><span data-stu-id="dbda7-225">The `{pubdate:datetime}` parameter is constrained to match a **DateTime** value.</span></span> <span data-ttu-id="dbda7-226">Ta funkcja działa, ale jest faktycznie mniej ograniczająca niż prosimy o poświęcenie.</span><span class="sxs-lookup"><span data-stu-id="dbda7-226">This works, but it's actually more permissive than we'd like.</span></span> <span data-ttu-id="dbda7-227">Na przykład następujące identyfikatory URI, również będą zgodne trasy:</span><span class="sxs-lookup"><span data-stu-id="dbda7-227">For example, these URIs will also match the route:</span></span>

`/api/books/date/Thu, 01 May 2008`

`/api/books/date/2000-12-16T00:00:00`

<span data-ttu-id="dbda7-228">Nie ma problem z zezwoleniem na te identyfikatory URI.</span><span class="sxs-lookup"><span data-stu-id="dbda7-228">There's nothing wrong with allowing these URIs.</span></span> <span data-ttu-id="dbda7-229">Może jednak ograniczyć trasy do określonego formatu, Dodawanie ograniczenia wyrażenia regularnego do szablonu trasy:</span><span class="sxs-lookup"><span data-stu-id="dbda7-229">However, you can restrict the route to a particular format by adding a regular-expression constraint to the route template:</span></span>

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample19.cs?highlight=1)]

<span data-ttu-id="dbda7-230">Teraz tylko daty w postaci &quot;rrrr mm-dd&quot; będą zgodne.</span><span class="sxs-lookup"><span data-stu-id="dbda7-230">Now only dates in the form &quot;yyyy-mm-dd&quot; will match.</span></span> <span data-ttu-id="dbda7-231">Należy zauważyć, że nie używamy wyrażenia regularnego można zweryfikować, że mamy rzeczywiste daty.</span><span class="sxs-lookup"><span data-stu-id="dbda7-231">Notice that we don't use the regex to validate that we got a real date.</span></span> <span data-ttu-id="dbda7-232">Który odbywa się w przypadku interfejsu API sieci Web podejmuje próbę przekonwertowania segmentem identyfikatora URI do **daty/godziny** wystąpienia.</span><span class="sxs-lookup"><span data-stu-id="dbda7-232">That is handled when Web API tries to convert the URI segment into a **DateTime** instance.</span></span> <span data-ttu-id="dbda7-233">Nieprawidłowa data, takie jak "2012-47-99" zakończy się niepowodzeniem ma zostać przekonwertowany, a klient otrzyma błąd 404.</span><span class="sxs-lookup"><span data-stu-id="dbda7-233">An invalid date such as '2012-47-99' will fail to be converted, and the client will get a 404 error.</span></span>

<span data-ttu-id="dbda7-234">Może również obsługiwać separator ukośnika (`/api/books/date/yyyy/mm/dd`) przez dodanie innej **[trasy]** atrybut o inne wyrażenie regularne.</span><span class="sxs-lookup"><span data-stu-id="dbda7-234">You can also support a slash separator (`/api/books/date/yyyy/mm/dd`) by adding another **[Route]** attribute with a different regex.</span></span>

[!code-html[Main](create-a-rest-api-with-attribute-routing/samples/sample20.html)]

<span data-ttu-id="dbda7-235">Brak subtelne, ale ważne szczegóły poniżej.</span><span class="sxs-lookup"><span data-stu-id="dbda7-235">There is a subtle but important detail here.</span></span> <span data-ttu-id="dbda7-236">Drugi szablon trasy zawiera symbol wieloznaczny (\*) na początku parametru {pubdate}:</span><span class="sxs-lookup"><span data-stu-id="dbda7-236">The second route template has a wildcard character (\*) at the start of the {pubdate} parameter:</span></span>

[!code-json[Main](create-a-rest-api-with-attribute-routing/samples/sample21.json)]

<span data-ttu-id="dbda7-237">Informuje aparat routingu, że {pubdate} powinien być zgodny z pozostałą część identyfikatora URI.</span><span class="sxs-lookup"><span data-stu-id="dbda7-237">This tells the routing engine that {pubdate} should match the rest of the URI.</span></span> <span data-ttu-id="dbda7-238">Domyślnie parametrem szablonu dopasowuje pojedynczy segment identyfikatora URI.</span><span class="sxs-lookup"><span data-stu-id="dbda7-238">By default, a template parameter matches a single URI segment.</span></span> <span data-ttu-id="dbda7-239">W tym przypadku chcemy {pubdate} obejmować wiele segmentów identyfikatora URI:</span><span class="sxs-lookup"><span data-stu-id="dbda7-239">In this case, we want {pubdate} to span several URI segments:</span></span>

`/api/books/date/2013/06/17`

## <a name="controller-code"></a><span data-ttu-id="dbda7-240">Kod kontrolera</span><span class="sxs-lookup"><span data-stu-id="dbda7-240">Controller Code</span></span>

<span data-ttu-id="dbda7-241">Oto kompletny kod dla klasy BooksController.</span><span class="sxs-lookup"><span data-stu-id="dbda7-241">Here is the complete code for the BooksController class.</span></span>

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample22.cs)]

## <a name="summary"></a><span data-ttu-id="dbda7-242">Podsumowanie</span><span class="sxs-lookup"><span data-stu-id="dbda7-242">Summary</span></span>

<span data-ttu-id="dbda7-243">Trasowanie atrybutów zapewnia więcej kontroli i większą elastyczność podczas projektowania identyfikatorów URI dla interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="dbda7-243">Attribute routing gives you more control and greater flexibility when designing the URIs for your API.</span></span>
