---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-4
title: Obsługa relacjami jednostek | Dokumentacja firmy Microsoft
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/16/2014
ms.topic: article
ms.assetid: d2f5710c-23c7-40a5-9cd9-5d0516570cba
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-4
msc.type: authoredcontent
ms.openlocfilehash: 3c82724739b8ccb7c6b13788a5420af1e61c990b
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
---
<a name="handling-entity-relations"></a><span data-ttu-id="90b59-102">Obsługa relacjami jednostek</span><span class="sxs-lookup"><span data-stu-id="90b59-102">Handling Entity Relations</span></span>
====================
<span data-ttu-id="90b59-103">przez [Wasson Jan](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="90b59-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="90b59-104">Pobieranie ukończone projektu</span><span class="sxs-lookup"><span data-stu-id="90b59-104">Download Completed Project</span></span>](https://github.com/MikeWasson/BookService)

<span data-ttu-id="90b59-105">W tej sekcji opisano niektóre szczegółowe informacje, jak EF ładuje powiązanych jednostek i sposób obsługi właściwości nawigacji cykliczne w klasach modeli.</span><span class="sxs-lookup"><span data-stu-id="90b59-105">This section describes some details of how EF loads related entities, and how to handle circular navigation properties in your model classes.</span></span> <span data-ttu-id="90b59-106">(Ta sekcja zawiera tła wiedzy i nie jest wymagane do ukończenia tego samouczka.</span><span class="sxs-lookup"><span data-stu-id="90b59-106">(This section provides background knowledge, and is not required to complete the tutorial.</span></span> <span data-ttu-id="90b59-107">Jeśli wolisz, przejdź do [część 5.](part-5.md).)</span><span class="sxs-lookup"><span data-stu-id="90b59-107">If you prefer, skip to [Part 5.](part-5.md).)</span></span>

## <a name="eager-loading-versus-lazy-loading"></a><span data-ttu-id="90b59-108">Eager ładowania i opóźnionego ładowania</span><span class="sxs-lookup"><span data-stu-id="90b59-108">Eager Loading versus Lazy Loading</span></span>

<span data-ttu-id="90b59-109">Jeśli używasz EF z relacyjnej bazy danych, jest zrozumieć, jak EF ładuje dane dotyczące.</span><span class="sxs-lookup"><span data-stu-id="90b59-109">When using EF with a relational database, it's important to understand how EF loads related data.</span></span>

<span data-ttu-id="90b59-110">Jest również zobaczyć zapytań SQL, które generuje EF.</span><span class="sxs-lookup"><span data-stu-id="90b59-110">It's also useful to see the SQL queries that EF generates.</span></span> <span data-ttu-id="90b59-111">Śledzenie SQL, Dodaj następujący wiersz kodu w celu `BookServiceContext` konstruktora:</span><span class="sxs-lookup"><span data-stu-id="90b59-111">To trace the SQL, add the following line of code to the `BookServiceContext` constructor:</span></span>

[!code-csharp[Main](part-4/samples/sample1.cs)]

<span data-ttu-id="90b59-112">Po wysłaniu żądania GET do /api/books zwraca JSON podobne do poniższych:</span><span class="sxs-lookup"><span data-stu-id="90b59-112">If you send a GET request to /api/books, it returns JSON like the following:</span></span>

[!code-console[Main](part-4/samples/sample2.cmd)]

<span data-ttu-id="90b59-113">Widać, właściwość Autor jest pusty, nawet jeśli książce zawiera prawidłową wartość IDAutora.</span><span class="sxs-lookup"><span data-stu-id="90b59-113">You can see that the Author property is null, even though the book contains a valid AuthorId.</span></span> <span data-ttu-id="90b59-114">Wynika to z EF nie ładuje powiązanych jednostek autora.</span><span class="sxs-lookup"><span data-stu-id="90b59-114">That's because EF is not loading the related Author entities.</span></span> <span data-ttu-id="90b59-115">Dziennik śledzenia zapytania SQL potwierdza to:</span><span class="sxs-lookup"><span data-stu-id="90b59-115">The trace log of the SQL query confirms this:</span></span>

[!code-console[Main](part-4/samples/sample3.sql)]

<span data-ttu-id="90b59-116">Instrukcja SELECT przyjmuje z tabeli książki i nie odwołuje się do tabeli autora.</span><span class="sxs-lookup"><span data-stu-id="90b59-116">The SELECT statement takes from the Books table, and does not reference the Author table.</span></span>

<span data-ttu-id="90b59-117">Odwołanie, w tym miejscu jest metoda `BooksController` klasy, która zwraca listy książek.</span><span class="sxs-lookup"><span data-stu-id="90b59-117">For reference, here is the method in the `BooksController` class that returns the list of books.</span></span>

[!code-csharp[Main](part-4/samples/sample4.cs)]

<span data-ttu-id="90b59-118">Zobaczmy, jak zostanie zwrócona autora w ramach dane JSON.</span><span class="sxs-lookup"><span data-stu-id="90b59-118">Let's see how we can return the Author as part of the JSON data.</span></span> <span data-ttu-id="90b59-119">Istnieją trzy sposoby załadować powiązanych danych w programie Entity Framework: wczesny ładowania opóźnionego ładowania i jawnego ładowania.</span><span class="sxs-lookup"><span data-stu-id="90b59-119">There are three ways to load related data in Entity Framework: eager loading, lazy loading, and explicit loading.</span></span> <span data-ttu-id="90b59-120">Istnieją kompromis każdego technika, dlatego ważne jest, aby zrozumieć, jak działają.</span><span class="sxs-lookup"><span data-stu-id="90b59-120">There are trade-offs with each technique, so it's important to understand how they work.</span></span>

### <a name="eager-loading"></a><span data-ttu-id="90b59-121">Ładowanie wczesny</span><span class="sxs-lookup"><span data-stu-id="90b59-121">Eager Loading</span></span>

<span data-ttu-id="90b59-122">Z *wczesny ładowania*, EF ładuje powiązanych jednostek jako część zapytania początkowej bazy danych.</span><span class="sxs-lookup"><span data-stu-id="90b59-122">With *eager loading*, EF loads related entities as part of the initial database query.</span></span> <span data-ttu-id="90b59-123">Aby przeprowadzić ładowanie wczesny, użyj **System.Data.Entity.Include** — metoda rozszerzenia.</span><span class="sxs-lookup"><span data-stu-id="90b59-123">To perform eager loading, use the **System.Data.Entity.Include** extension method.</span></span>

[!code-csharp[Main](part-4/samples/sample5.cs)]

<span data-ttu-id="90b59-124">Ta wartość informuje EF, aby dołączyć dane autora w zapytaniu.</span><span class="sxs-lookup"><span data-stu-id="90b59-124">This tells EF to include the Author data in the query.</span></span> <span data-ttu-id="90b59-125">Jeśli chcesz wprowadzić tę zmianę i uruchomić aplikację, teraz danych JSON wygląda następująco:</span><span class="sxs-lookup"><span data-stu-id="90b59-125">If you make this change and run the app, now the JSON data looks like this:</span></span>

[!code-console[Main](part-4/samples/sample6.cmd)]

<span data-ttu-id="90b59-126">Dziennik śledzenia pokazuje EF wykonanie sprzężenia w tabelach książki i autora.</span><span class="sxs-lookup"><span data-stu-id="90b59-126">The trace log shows that EF performed a join on the Book and Author tables.</span></span>

[!code-console[Main](part-4/samples/sample7.cmd)]

### <a name="lazy-loading"></a><span data-ttu-id="90b59-127">Powolne ładowanie</span><span class="sxs-lookup"><span data-stu-id="90b59-127">Lazy Loading</span></span>

<span data-ttu-id="90b59-128">Z opóźnieniem ładowania EF automatycznie ładuje obiektu pokrewnego, gdy jest wyłuskiwany właściwości nawigacji dla danej jednostki.</span><span class="sxs-lookup"><span data-stu-id="90b59-128">With lazy loading, EF automatically loads a related entity when the navigation property for that entity is dereferenced.</span></span> <span data-ttu-id="90b59-129">Aby włączyć ładowanie opóźnieniem, Tworzenie wirtualnego właściwości nawigacji.</span><span class="sxs-lookup"><span data-stu-id="90b59-129">To enable lazy loading, make the navigation property virtual.</span></span> <span data-ttu-id="90b59-130">Na przykład w klasie książki:</span><span class="sxs-lookup"><span data-stu-id="90b59-130">For example, in the Book class:</span></span>

[!code-csharp[Main](part-4/samples/sample8.cs?highlight=6)]

<span data-ttu-id="90b59-131">Teraz Rozważmy następujący kod:</span><span class="sxs-lookup"><span data-stu-id="90b59-131">Now consider the following code:</span></span>

[!code-csharp[Main](part-4/samples/sample9.cs)]

<span data-ttu-id="90b59-132">Podczas ładowania opóźnieniem jest włączone, dostęp do `Author` właściwość `books[0]` powoduje, że EF dla autora kwerendy bazy danych.</span><span class="sxs-lookup"><span data-stu-id="90b59-132">When lazy loading is enabled, accessing the `Author` property on `books[0]` causes EF to query the database for the author.</span></span>

<span data-ttu-id="90b59-133">Powolne ładowanie wymaga wiele rund bazy danych, ponieważ EF wysyła zapytanie zawsze pobiera powiązanej jednostki.</span><span class="sxs-lookup"><span data-stu-id="90b59-133">Lazy loading requires multiple database trips, because EF sends a query each time it retrieves a related entity.</span></span> <span data-ttu-id="90b59-134">Ogólnie rzecz biorąc ma powolne ładowanie wyłączone dla obiektów, które można serializować.</span><span class="sxs-lookup"><span data-stu-id="90b59-134">Generally, you want lazy loading disabled for objects that you serialize.</span></span> <span data-ttu-id="90b59-135">Serializator musi odczytywać wszystkie właściwości w modelu, który wyzwala ładowanie powiązanych jednostek.</span><span class="sxs-lookup"><span data-stu-id="90b59-135">The serializer has to read all of the properties on the model, which triggers loading the related entities.</span></span> <span data-ttu-id="90b59-136">Na przykład poniżej przedstawiono zapytania SQL podczas EF serializuje listy książek z opóźnieniem ładowania włączone.</span><span class="sxs-lookup"><span data-stu-id="90b59-136">For example, here are the SQL queries when EF serializes the list of books with lazy loading enabled.</span></span> <span data-ttu-id="90b59-137">Widać, że EF udostępnia trzy oddzielne zapytania dla trzech autorów.</span><span class="sxs-lookup"><span data-stu-id="90b59-137">You can see that EF makes three separate queries for the three authors.</span></span>

[!code-console[Main](part-4/samples/sample10.sql)]

<span data-ttu-id="90b59-138">Nadal istnieją razy podczas możesz chcieć użyć opóźnionego ładowania.</span><span class="sxs-lookup"><span data-stu-id="90b59-138">There are still times when you might want to use lazy loading.</span></span> <span data-ttu-id="90b59-139">Ładowanie wczesny może spowodować EF do generowania bardzo złożonych sprzężenia.</span><span class="sxs-lookup"><span data-stu-id="90b59-139">Eager loading can cause EF to generate a very complex join.</span></span> <span data-ttu-id="90b59-140">Powiązanych jednostek mogą być wymagane do małego podzbioru danych i bardziej efektywne byłoby opóźnionego ładowania.</span><span class="sxs-lookup"><span data-stu-id="90b59-140">Or you might need related entities for a small subset of the data, and lazy loading would be more efficient.</span></span>

<span data-ttu-id="90b59-141">Jest jednym ze sposobów uniknąć problemów serializacji do serializacji obiektów transfer danych (DTOs) zamiast obiektów jednostek.</span><span class="sxs-lookup"><span data-stu-id="90b59-141">One way to avoid serialization problems is to serialize data transfer objects (DTOs) instead of entity objects.</span></span> <span data-ttu-id="90b59-142">Ta metoda będzie wyświetlić w dalszej części tego artykułu.</span><span class="sxs-lookup"><span data-stu-id="90b59-142">I'll show this approach later in the article.</span></span>

### <a name="explicit-loading"></a><span data-ttu-id="90b59-143">Ładowanie jawne</span><span class="sxs-lookup"><span data-stu-id="90b59-143">Explicit Loading</span></span>

<span data-ttu-id="90b59-144">Jawne ładowania jest podobny do opóźnionego ładowania, z wyjątkiem jawnie pobrać powiązanych danych w kodzie; go nie jest realizowane automatycznie podczas dostępu do właściwości nawigacji.</span><span class="sxs-lookup"><span data-stu-id="90b59-144">Explicit loading is similar to lazy loading, except that you explicitly get the related data in code; it doesn't happen automatically when you access a navigation property.</span></span> <span data-ttu-id="90b59-145">Ładowanie jawne zapewnia deweloperom większą kontrolę nad tym, kiedy można załadować dane dotyczące, ale wymaga dodatkowego kodu.</span><span class="sxs-lookup"><span data-stu-id="90b59-145">Explicit loading gives you more control over when to load related data, but requires extra code.</span></span> <span data-ttu-id="90b59-146">Aby uzyskać więcej informacji na temat jawnego ładowania, zobacz [ładowanie powiązanych jednostek](https://msdn.microsoft.com/data/jj574232#explicit).</span><span class="sxs-lookup"><span data-stu-id="90b59-146">For more information about explicit loading, see [Loading Related Entities](https://msdn.microsoft.com/data/jj574232#explicit).</span></span>

## <a name="navigation-properties-and-circular-references"></a><span data-ttu-id="90b59-147">Właściwości nawigacji oraz odwołania cykliczne</span><span class="sxs-lookup"><span data-stu-id="90b59-147">Navigation Properties and Circular References</span></span>

<span data-ttu-id="90b59-148">Po zdefiniowaniu I modele książki i autora, na jest zdefiniowana właściwość nawigacji `Book` klasy relacji Autor księgi, ale nie zdefiniowano I właściwości nawigacji w innym kierunku.</span><span class="sxs-lookup"><span data-stu-id="90b59-148">When I defined the Book and Author models, I defined a navigation property on the `Book` class for the Book-Author relationship, but I did not define a navigation property in the other direction.</span></span>

<span data-ttu-id="90b59-149">Co się stanie po dodaniu odpowiednią właściwość nawigacji do `Author` klasy?</span><span class="sxs-lookup"><span data-stu-id="90b59-149">What happens if you add the corresponding navigation property to the `Author` class?</span></span>

[!code-csharp[Main](part-4/samples/sample11.cs?highlight=7)]

<span data-ttu-id="90b59-150">Niestety tworzy to problem podczas serializacji modeli.</span><span class="sxs-lookup"><span data-stu-id="90b59-150">Unfortunately, this creates a problem when you serialize the models.</span></span> <span data-ttu-id="90b59-151">Po załadowaniu powiązanych danych tworzy wykres obiektu cykliczne.</span><span class="sxs-lookup"><span data-stu-id="90b59-151">If you load the related data, it creates a circular object graph.</span></span>

![](part-4/_static/image1.png)

<span data-ttu-id="90b59-152">Próba serializować wykresu przez program formatujący JSON i XML zgłosi wyjątek.</span><span class="sxs-lookup"><span data-stu-id="90b59-152">When the JSON or XML formatter tries to serialize the graph, it will throw an exception.</span></span> <span data-ttu-id="90b59-153">Dwa elementy formatujące generują komunikaty o różnych wyjątku.</span><span class="sxs-lookup"><span data-stu-id="90b59-153">The two formatters throw different exception messages.</span></span> <span data-ttu-id="90b59-154">Oto przykład dla elementu formatującego JSON:</span><span class="sxs-lookup"><span data-stu-id="90b59-154">Here is an example for the JSON formatter:</span></span>

[!code-console[Main](part-4/samples/sample12.cmd)]

<span data-ttu-id="90b59-155">Oto element formatujący XML:</span><span class="sxs-lookup"><span data-stu-id="90b59-155">Here is the XML formatter:</span></span>

[!code-xml[Main](part-4/samples/sample13.xml)]

<span data-ttu-id="90b59-156">Rozwiązanie polega na Użyj DTOs, opisujące w następnej sekcji.</span><span class="sxs-lookup"><span data-stu-id="90b59-156">One solution is to use DTOs, which I describe in the next section.</span></span> <span data-ttu-id="90b59-157">Alternatywnie można skonfigurować JSON i XML programów formatujących do obsługi cykle wykresu.</span><span class="sxs-lookup"><span data-stu-id="90b59-157">Alternatively, you can configure the JSON and XML formatters to handle graph cycles.</span></span> <span data-ttu-id="90b59-158">Aby uzyskać więcej informacji, zobacz [obsługi odwołań cyklicznych obiektu](../../formats-and-model-binding/json-and-xml-serialization.md#handling_circular_object_references).</span><span class="sxs-lookup"><span data-stu-id="90b59-158">For more information, see [Handling Circular Object References](../../formats-and-model-binding/json-and-xml-serialization.md#handling_circular_object_references).</span></span>

<span data-ttu-id="90b59-159">W tym samouczku, nie potrzebujesz `Author.Book` właściwość nawigacji, więc możesz go Opuść.</span><span class="sxs-lookup"><span data-stu-id="90b59-159">For this tutorial, you don't need the `Author.Book` navigation property, so you can leave it out.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="90b59-160">[Poprzednie](part-3.md)
> [dalej](part-5.md)</span><span class="sxs-lookup"><span data-stu-id="90b59-160">[Previous](part-3.md)
[Next](part-5.md)</span></span>
