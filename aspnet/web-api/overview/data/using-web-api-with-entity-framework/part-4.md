---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-4
title: Obsługa relacji jednostek | Dokumentacja firmy Microsoft
author: MikeWasson
description: ''
ms.author: riande
ms.date: 06/16/2014
ms.assetid: d2f5710c-23c7-40a5-9cd9-5d0516570cba
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-4
msc.type: authoredcontent
ms.openlocfilehash: 5d05a2e5d4380a15078317545325bd20fde3f83c
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/16/2018
ms.locfileid: "41755384"
---
<a name="handling-entity-relations"></a><span data-ttu-id="d4a0c-102">Obsługa relacji jednostek</span><span class="sxs-lookup"><span data-stu-id="d4a0c-102">Handling Entity Relations</span></span>
====================
<span data-ttu-id="d4a0c-103">przez [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="d4a0c-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="d4a0c-104">Pobieranie ukończone projektu</span><span class="sxs-lookup"><span data-stu-id="d4a0c-104">Download Completed Project</span></span>](https://github.com/MikeWasson/BookService)

<span data-ttu-id="d4a0c-105">W tej sekcji opisano niektóre szczegóły, jak EF ładuje powiązanych jednostek i sposób obsługi właściwości nawigacji cykliczne w klasach modeli.</span><span class="sxs-lookup"><span data-stu-id="d4a0c-105">This section describes some details of how EF loads related entities, and how to handle circular navigation properties in your model classes.</span></span> <span data-ttu-id="d4a0c-106">(Ta sekcja zawiera wiedzę i nie jest wymagane do ukończenia tego samouczka.</span><span class="sxs-lookup"><span data-stu-id="d4a0c-106">(This section provides background knowledge, and is not required to complete the tutorial.</span></span> <span data-ttu-id="d4a0c-107">Jeśli wolisz, przejdź do [część 5.](part-5.md).)</span><span class="sxs-lookup"><span data-stu-id="d4a0c-107">If you prefer, skip to [Part 5.](part-5.md).)</span></span>

## <a name="eager-loading-versus-lazy-loading"></a><span data-ttu-id="d4a0c-108">Eager, ładowania i ładowania z opóźnieniem</span><span class="sxs-lookup"><span data-stu-id="d4a0c-108">Eager Loading versus Lazy Loading</span></span>

<span data-ttu-id="d4a0c-109">Przy użyciu programu EF z relacyjnej bazy danych, jest ważne, aby zrozumieć, jak EF ładuje powiązanych danych.</span><span class="sxs-lookup"><span data-stu-id="d4a0c-109">When using EF with a relational database, it's important to understand how EF loads related data.</span></span>

<span data-ttu-id="d4a0c-110">Jest to również przydatne wyświetlić zapytania SQL, które generuje EF.</span><span class="sxs-lookup"><span data-stu-id="d4a0c-110">It's also useful to see the SQL queries that EF generates.</span></span> <span data-ttu-id="d4a0c-111">Śledzenie SQL, Dodaj następujący wiersz kodu, aby `BookServiceContext` Konstruktor:</span><span class="sxs-lookup"><span data-stu-id="d4a0c-111">To trace the SQL, add the following line of code to the `BookServiceContext` constructor:</span></span>

[!code-csharp[Main](part-4/samples/sample1.cs)]

<span data-ttu-id="d4a0c-112">Jeśli wyślesz żądanie GET do /api/books zwraca JSON podobne do następującego:</span><span class="sxs-lookup"><span data-stu-id="d4a0c-112">If you send a GET request to /api/books, it returns JSON like the following:</span></span>

[!code-console[Main](part-4/samples/sample2.cmd)]

<span data-ttu-id="d4a0c-113">Widać, że właściwość Autor ma wartość null, mimo że książka zawiera prawidłową wartość IDAutora.</span><span class="sxs-lookup"><span data-stu-id="d4a0c-113">You can see that the Author property is null, even though the book contains a valid AuthorId.</span></span> <span data-ttu-id="d4a0c-114">Wynika to z programów EF nie trwa ładowanie powiązanych jednostek autora.</span><span class="sxs-lookup"><span data-stu-id="d4a0c-114">That's because EF is not loading the related Author entities.</span></span> <span data-ttu-id="d4a0c-115">Zapytanie SQL w dzienniku śledzenia potwierdza to:</span><span class="sxs-lookup"><span data-stu-id="d4a0c-115">The trace log of the SQL query confirms this:</span></span>

[!code-console[Main](part-4/samples/sample3.sql)]

<span data-ttu-id="d4a0c-116">Instrukcja SELECT przyjmuje z tabeli książki, a nie odwołuje się do tabeli autora.</span><span class="sxs-lookup"><span data-stu-id="d4a0c-116">The SELECT statement takes from the Books table, and does not reference the Author table.</span></span>

<span data-ttu-id="d4a0c-117">Odwołanie, Oto metoda `BooksController` klasę, która zwraca listę książki.</span><span class="sxs-lookup"><span data-stu-id="d4a0c-117">For reference, here is the method in the `BooksController` class that returns the list of books.</span></span>

[!code-csharp[Main](part-4/samples/sample4.cs)]

<span data-ttu-id="d4a0c-118">Zobaczmy, jak firma Microsoft może zwracać autora jako część danych JSON.</span><span class="sxs-lookup"><span data-stu-id="d4a0c-118">Let's see how we can return the Author as part of the JSON data.</span></span> <span data-ttu-id="d4a0c-119">Istnieją trzy sposoby ładowanie powiązanych danych platformy Entity Framework: wczesne ładowanie, powolne ładowanie i jawne ładowanie.</span><span class="sxs-lookup"><span data-stu-id="d4a0c-119">There are three ways to load related data in Entity Framework: eager loading, lazy loading, and explicit loading.</span></span> <span data-ttu-id="d4a0c-120">Ma wad i zalet z każdą z technik, więc jest ważne zrozumieć, jak działają.</span><span class="sxs-lookup"><span data-stu-id="d4a0c-120">There are trade-offs with each technique, so it's important to understand how they work.</span></span>

### <a name="eager-loading"></a><span data-ttu-id="d4a0c-121">Wczesne ładowanie</span><span class="sxs-lookup"><span data-stu-id="d4a0c-121">Eager Loading</span></span>

<span data-ttu-id="d4a0c-122">Za pomocą *wczesne ładowanie*, EF ładuje powiązanych jednostek jako część zapytania początkowej bazy danych.</span><span class="sxs-lookup"><span data-stu-id="d4a0c-122">With *eager loading*, EF loads related entities as part of the initial database query.</span></span> <span data-ttu-id="d4a0c-123">Aby wykonać wczesne ładowanie, użyj **System.Data.Entity.Include** — metoda rozszerzenia.</span><span class="sxs-lookup"><span data-stu-id="d4a0c-123">To perform eager loading, use the **System.Data.Entity.Include** extension method.</span></span>

[!code-csharp[Main](part-4/samples/sample5.cs)]

<span data-ttu-id="d4a0c-124">Informuje EF, aby dołączyć dane autora w zapytaniu.</span><span class="sxs-lookup"><span data-stu-id="d4a0c-124">This tells EF to include the Author data in the query.</span></span> <span data-ttu-id="d4a0c-125">Jeśli chcesz wprowadzić tę zmianę i uruchomić aplikację, dane JSON wygląda teraz następująco:</span><span class="sxs-lookup"><span data-stu-id="d4a0c-125">If you make this change and run the app, now the JSON data looks like this:</span></span>

[!code-console[Main](part-4/samples/sample6.cmd)]

<span data-ttu-id="d4a0c-126">W dzienniku śledzenia pokazuje, że EF wykonywane sprzężenia w tabelach książki i autora.</span><span class="sxs-lookup"><span data-stu-id="d4a0c-126">The trace log shows that EF performed a join on the Book and Author tables.</span></span>

[!code-console[Main](part-4/samples/sample7.cmd)]

### <a name="lazy-loading"></a><span data-ttu-id="d4a0c-127">Ładowanie z opóźnieniem</span><span class="sxs-lookup"><span data-stu-id="d4a0c-127">Lazy Loading</span></span>

<span data-ttu-id="d4a0c-128">Przy użyciu ładowania z opóźnieniem EF automatycznie ładuje powiązanej jednostki, kiedy jest wyłuskiwany właściwości nawigacji dla danej jednostki.</span><span class="sxs-lookup"><span data-stu-id="d4a0c-128">With lazy loading, EF automatically loads a related entity when the navigation property for that entity is dereferenced.</span></span> <span data-ttu-id="d4a0c-129">Aby włączyć powolne ładowanie, Tworzenie wirtualnego właściwości nawigacji.</span><span class="sxs-lookup"><span data-stu-id="d4a0c-129">To enable lazy loading, make the navigation property virtual.</span></span> <span data-ttu-id="d4a0c-130">Na przykład w klasie książki:</span><span class="sxs-lookup"><span data-stu-id="d4a0c-130">For example, in the Book class:</span></span>

[!code-csharp[Main](part-4/samples/sample8.cs?highlight=6)]

<span data-ttu-id="d4a0c-131">Teraz należy wziąć pod uwagę następujący kod:</span><span class="sxs-lookup"><span data-stu-id="d4a0c-131">Now consider the following code:</span></span>

[!code-csharp[Main](part-4/samples/sample9.cs)]

<span data-ttu-id="d4a0c-132">Po włączeniu ładowania z opóźnieniem, uzyskiwanie dostępu do `Author` właściwość `books[0]` powoduje, że EF dla autora kwerendy bazy danych.</span><span class="sxs-lookup"><span data-stu-id="d4a0c-132">When lazy loading is enabled, accessing the `Author` property on `books[0]` causes EF to query the database for the author.</span></span>

<span data-ttu-id="d4a0c-133">Powolne ładowanie wymaga wielu rund bazy danych, ponieważ EF wysyła zapytanie każdorazowo pobiera powiązanej jednostki.</span><span class="sxs-lookup"><span data-stu-id="d4a0c-133">Lazy loading requires multiple database trips, because EF sends a query each time it retrieves a related entity.</span></span> <span data-ttu-id="d4a0c-134">Ogólnie rzecz biorąc ma powolne ładowanie wyłączone dla obiektów, które można serializować.</span><span class="sxs-lookup"><span data-stu-id="d4a0c-134">Generally, you want lazy loading disabled for objects that you serialize.</span></span> <span data-ttu-id="d4a0c-135">Serializator musi odczytywać wszystkie właściwości w modelu, który wyzwala ładowanie powiązanych jednostek.</span><span class="sxs-lookup"><span data-stu-id="d4a0c-135">The serializer has to read all of the properties on the model, which triggers loading the related entities.</span></span> <span data-ttu-id="d4a0c-136">Na przykład poniżej przedstawiono zapytania SQL podczas EF serializuje listy książek przy użyciu ładowania z opóźnieniem włączone.</span><span class="sxs-lookup"><span data-stu-id="d4a0c-136">For example, here are the SQL queries when EF serializes the list of books with lazy loading enabled.</span></span> <span data-ttu-id="d4a0c-137">Aby zobaczyć, że EF sprawia, że trzy oddzielne zapytania dla trzech autorów.</span><span class="sxs-lookup"><span data-stu-id="d4a0c-137">You can see that EF makes three separate queries for the three authors.</span></span>

[!code-console[Main](part-4/samples/sample10.sql)]

<span data-ttu-id="d4a0c-138">Czas, kiedy warto korzystać z opóźnieniem ładowania nadal istnieją.</span><span class="sxs-lookup"><span data-stu-id="d4a0c-138">There are still times when you might want to use lazy loading.</span></span> <span data-ttu-id="d4a0c-139">Wczesne ładowanie może spowodować EF do generowania bardzo złożone sprzężenia.</span><span class="sxs-lookup"><span data-stu-id="d4a0c-139">Eager loading can cause EF to generate a very complex join.</span></span> <span data-ttu-id="d4a0c-140">Lub możesz potrzebować powiązanych jednostek dla małego podzbioru danych i powolne ładowanie będzie bardziej wydajne.</span><span class="sxs-lookup"><span data-stu-id="d4a0c-140">Or you might need related entities for a small subset of the data, and lazy loading would be more efficient.</span></span>

<span data-ttu-id="d4a0c-141">Jednym ze sposobów, aby uniknąć problemów z serializacji jest do wykonywania serializacji obiektów transferu danych (dto) zamiast obiektów jednostek.</span><span class="sxs-lookup"><span data-stu-id="d4a0c-141">One way to avoid serialization problems is to serialize data transfer objects (DTOs) instead of entity objects.</span></span> <span data-ttu-id="d4a0c-142">Zaprezentuję, to podejście w dalszej części tego artykułu.</span><span class="sxs-lookup"><span data-stu-id="d4a0c-142">I'll show this approach later in the article.</span></span>

### <a name="explicit-loading"></a><span data-ttu-id="d4a0c-143">Jawne ładowanie</span><span class="sxs-lookup"><span data-stu-id="d4a0c-143">Explicit Loading</span></span>

<span data-ttu-id="d4a0c-144">Jawne ładowanie jest podobny do ładowania z opóźnieniem, chyba że jawnie uzyskać powiązanych danych w kodzie nie będzie automatycznie gdy uzyskujesz dostęp do właściwości nawigacji.</span><span class="sxs-lookup"><span data-stu-id="d4a0c-144">Explicit loading is similar to lazy loading, except that you explicitly get the related data in code; it doesn't happen automatically when you access a navigation property.</span></span> <span data-ttu-id="d4a0c-145">Jawne ładowanie daje większą kontrolę nad tym, kiedy ładowanie powiązanych danych, ale wymaga dodatkowego kodu.</span><span class="sxs-lookup"><span data-stu-id="d4a0c-145">Explicit loading gives you more control over when to load related data, but requires extra code.</span></span> <span data-ttu-id="d4a0c-146">Aby uzyskać więcej informacji na temat jawne ładowanie zobacz [ładowanie powiązanych jednostek](https://msdn.microsoft.com/data/jj574232#explicit).</span><span class="sxs-lookup"><span data-stu-id="d4a0c-146">For more information about explicit loading, see [Loading Related Entities](https://msdn.microsoft.com/data/jj574232#explicit).</span></span>

## <a name="navigation-properties-and-circular-references"></a><span data-ttu-id="d4a0c-147">Właściwości nawigacji i odwołania cykliczne</span><span class="sxs-lookup"><span data-stu-id="d4a0c-147">Navigation Properties and Circular References</span></span>

<span data-ttu-id="d4a0c-148">Po zdefiniowaniu książki i tworzenie modeli I na jest zdefiniowana właściwość nawigacji `Book` klasy relacji autor książki, ale nie mogę definiują właściwości nawigacji w drugą stronę.</span><span class="sxs-lookup"><span data-stu-id="d4a0c-148">When I defined the Book and Author models, I defined a navigation property on the `Book` class for the Book-Author relationship, but I did not define a navigation property in the other direction.</span></span>

<span data-ttu-id="d4a0c-149">Co się stanie po dodaniu odpowiednią właściwość nawigacji do `Author` klasy?</span><span class="sxs-lookup"><span data-stu-id="d4a0c-149">What happens if you add the corresponding navigation property to the `Author` class?</span></span>

[!code-csharp[Main](part-4/samples/sample11.cs?highlight=7)]

<span data-ttu-id="d4a0c-150">Niestety spowoduje to utworzenie problem podczas serializacji modeli.</span><span class="sxs-lookup"><span data-stu-id="d4a0c-150">Unfortunately, this creates a problem when you serialize the models.</span></span> <span data-ttu-id="d4a0c-151">Jeśli załadujesz powiązanych danych, tworzy wykres obiektu cykliczne.</span><span class="sxs-lookup"><span data-stu-id="d4a0c-151">If you load the related data, it creates a circular object graph.</span></span>

![](part-4/_static/image1.png)

<span data-ttu-id="d4a0c-152">Próba serializacji grafu przez program formatujący JSON lub XML zgłosi wyjątek.</span><span class="sxs-lookup"><span data-stu-id="d4a0c-152">When the JSON or XML formatter tries to serialize the graph, it will throw an exception.</span></span> <span data-ttu-id="d4a0c-153">Dwa elementy formatujące generują komunikaty o wyjątkach różne.</span><span class="sxs-lookup"><span data-stu-id="d4a0c-153">The two formatters throw different exception messages.</span></span> <span data-ttu-id="d4a0c-154">Oto przykład dla elementu formatującego JSON:</span><span class="sxs-lookup"><span data-stu-id="d4a0c-154">Here is an example for the JSON formatter:</span></span>

[!code-console[Main](part-4/samples/sample12.cmd)]

<span data-ttu-id="d4a0c-155">Poniżej przedstawiono program formatujący kod XML:</span><span class="sxs-lookup"><span data-stu-id="d4a0c-155">Here is the XML formatter:</span></span>

[!code-xml[Main](part-4/samples/sample13.xml)]

<span data-ttu-id="d4a0c-156">Jedno rozwiązanie ma używać dto, zawierające w następnej sekcji.</span><span class="sxs-lookup"><span data-stu-id="d4a0c-156">One solution is to use DTOs, which I describe in the next section.</span></span> <span data-ttu-id="d4a0c-157">Alternatywnie można skonfigurować formatami JSON i XML elementy formatujące do obsługi cykle wykresu.</span><span class="sxs-lookup"><span data-stu-id="d4a0c-157">Alternatively, you can configure the JSON and XML formatters to handle graph cycles.</span></span> <span data-ttu-id="d4a0c-158">Aby uzyskać więcej informacji, zobacz [obsługi odwołań cyklicznych obiektu](../../formats-and-model-binding/json-and-xml-serialization.md#handling_circular_object_references).</span><span class="sxs-lookup"><span data-stu-id="d4a0c-158">For more information, see [Handling Circular Object References](../../formats-and-model-binding/json-and-xml-serialization.md#handling_circular_object_references).</span></span>

<span data-ttu-id="d4a0c-159">Na potrzeby tego samouczka nie ma potrzeby `Author.Book` właściwość nawigacji, dzięki czemu możesz pozostawić.</span><span class="sxs-lookup"><span data-stu-id="d4a0c-159">For this tutorial, you don't need the `Author.Book` navigation property, so you can leave it out.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="d4a0c-160">[Poprzednie](part-3.md)
> [dalej](part-5.md)</span><span class="sxs-lookup"><span data-stu-id="d4a0c-160">[Previous](part-3.md)
[Next](part-5.md)</span></span>
