---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-3
title: 'Część 3: Tworzenie kontrolera Admin | Dokumentacja firmy Microsoft'
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/04/2012
ms.topic: article
ms.assetid: 6b9ae3c4-0274-4170-a1bb-9df9c546b2a9
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-3
msc.type: authoredcontent
ms.openlocfilehash: 588d9d1b5d27759692cd840faabf2c3549c309d6
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
---
<a name="part-3-creating-an-admin-controller"></a><span data-ttu-id="f8d82-102">Część 3: Tworzenie kontrolera administratora</span><span class="sxs-lookup"><span data-stu-id="f8d82-102">Part 3: Creating an Admin Controller</span></span>
====================
<span data-ttu-id="f8d82-103">przez [Wasson Jan](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="f8d82-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="f8d82-104">Pobieranie ukończone projektu</span><span class="sxs-lookup"><span data-stu-id="f8d82-104">Download Completed Project</span></span>](http://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

## <a name="add-an-admin-controller"></a><span data-ttu-id="f8d82-105">Dodawanie kontrolera administratora</span><span class="sxs-lookup"><span data-stu-id="f8d82-105">Add an Admin Controller</span></span>

<span data-ttu-id="f8d82-106">W tej sekcji dodamy kontrolera interfejsu API sieci Web, który obsługuje CRUD (tworzenia, odczytu, aktualizacji i usuwania) operacje na produkty.</span><span class="sxs-lookup"><span data-stu-id="f8d82-106">In this section, we'll add a Web API controller that supports CRUD (create, read, update, and delete) operations on products.</span></span> <span data-ttu-id="f8d82-107">Kontroler użyje Entity Framework do komunikowania się z warstwy bazy danych.</span><span class="sxs-lookup"><span data-stu-id="f8d82-107">The controller will use Entity Framework to communicate with the database layer.</span></span> <span data-ttu-id="f8d82-108">Tylko administratorzy będą mogli używać tego kontrolera.</span><span class="sxs-lookup"><span data-stu-id="f8d82-108">Only administrators will be able to use this controller.</span></span> <span data-ttu-id="f8d82-109">Klienci będą uzyskiwać dostęp do produktów za pomocą innego kontrolera.</span><span class="sxs-lookup"><span data-stu-id="f8d82-109">Customers will access the products through another controller.</span></span>

<span data-ttu-id="f8d82-110">W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy folder kontrolery.</span><span class="sxs-lookup"><span data-stu-id="f8d82-110">In Solution Explorer, right-click the Controllers folder.</span></span> <span data-ttu-id="f8d82-111">Wybierz **dodać** , a następnie **kontrolera**.</span><span class="sxs-lookup"><span data-stu-id="f8d82-111">Select **Add** and then **Controller**.</span></span>

![](using-web-api-with-entity-framework-part-3/_static/image1.png)

<span data-ttu-id="f8d82-112">W **Dodaj kontroler** okna dialogowego, nazwy kontrolera `AdminController`.</span><span class="sxs-lookup"><span data-stu-id="f8d82-112">In the **Add Controller** dialog, name the controller `AdminController`.</span></span> <span data-ttu-id="f8d82-113">W obszarze **szablonu**, wybierz pozycję &quot;Kontroler interfejsu API z akcjami odczytu/zapisu, przy użyciu programu Entity Framework&quot;.</span><span class="sxs-lookup"><span data-stu-id="f8d82-113">Under **Template**, select &quot;API controller with read/write actions, using Entity Framework&quot;.</span></span> <span data-ttu-id="f8d82-114">W obszarze **klasa modelu**, wybierz "Produktu (ProductStore.Models)".</span><span class="sxs-lookup"><span data-stu-id="f8d82-114">Under **Model class**, select "Product (ProductStore.Models)".</span></span> <span data-ttu-id="f8d82-115">W obszarze **kontekstu danych**, wybierz pozycję "&lt;nowy kontekst danych&gt;".</span><span class="sxs-lookup"><span data-stu-id="f8d82-115">Under **Data Context**, select "&lt;New Data Context&gt;".</span></span>

![](using-web-api-with-entity-framework-part-3/_static/image2.png)

> [!NOTE]
> <span data-ttu-id="f8d82-116">Jeśli **klasa modelu** listy rozwijanej nie są wyświetlane wszystkie klasy modeli, upewnij się, że można skompilować projekt.</span><span class="sxs-lookup"><span data-stu-id="f8d82-116">If the **Model class** drop-down does not show any model classes, make sure you compiled the project.</span></span> <span data-ttu-id="f8d82-117">Entity Framework używa odbicia, a więc musi skompilowanego zestawu.</span><span class="sxs-lookup"><span data-stu-id="f8d82-117">Entity Framework uses reflection, so it needs the compiled assembly.</span></span>


<span data-ttu-id="f8d82-118">Wybieranie "&lt;nowy kontekst danych&gt;" spowoduje otwarcie **nowy kontekst danych** okna dialogowego.</span><span class="sxs-lookup"><span data-stu-id="f8d82-118">Selecting "&lt;New Data Context&gt;" will open the **New Data Context** dialog.</span></span> <span data-ttu-id="f8d82-119">Nazwa kontekstu danych `ProductStore.Models.OrdersContext`.</span><span class="sxs-lookup"><span data-stu-id="f8d82-119">Name the data context `ProductStore.Models.OrdersContext`.</span></span>

![](using-web-api-with-entity-framework-part-3/_static/image3.png)

<span data-ttu-id="f8d82-120">Kliknij przycisk **OK** aby odrzucić **nowy kontekst danych** okna dialogowego.</span><span class="sxs-lookup"><span data-stu-id="f8d82-120">Click **OK** to dismiss the **New Data Context** dialog.</span></span> <span data-ttu-id="f8d82-121">W **Dodaj kontroler** okna dialogowego, kliknij przycisk **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="f8d82-121">In the **Add Controller** dialog, click **Add**.</span></span>

<span data-ttu-id="f8d82-122">Oto, co zostały dodane do projektu:</span><span class="sxs-lookup"><span data-stu-id="f8d82-122">Here's what got added to the project:</span></span>

- <span data-ttu-id="f8d82-123">Klasa o nazwie `OrdersContext` która pochodzi z **DbContext**.</span><span class="sxs-lookup"><span data-stu-id="f8d82-123">A class named `OrdersContext` that derives from **DbContext**.</span></span> <span data-ttu-id="f8d82-124">Ta klasa udostępnia sklejki między modelami POCO i bazy danych.</span><span class="sxs-lookup"><span data-stu-id="f8d82-124">This class provides the glue between the POCO models and the database.</span></span>
- <span data-ttu-id="f8d82-125">Kontroler interfejsu API sieci Web o nazwie `AdminController`.</span><span class="sxs-lookup"><span data-stu-id="f8d82-125">A Web API controller named `AdminController`.</span></span> <span data-ttu-id="f8d82-126">Ten kontroler obsługuje operacje CRUD na `Product` wystąpień.</span><span class="sxs-lookup"><span data-stu-id="f8d82-126">This controller supports CRUD operations on `Product` instances.</span></span> <span data-ttu-id="f8d82-127">Używa `OrdersContext` klasy do komunikowania się z programu Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="f8d82-127">It uses the `OrdersContext` class to communicate with Entity Framework.</span></span>
- <span data-ttu-id="f8d82-128">Nowe parametry połączenia bazy danych w pliku Web.config.</span><span class="sxs-lookup"><span data-stu-id="f8d82-128">A new database connection string in the Web.config file.</span></span>

![](using-web-api-with-entity-framework-part-3/_static/image4.png)

<span data-ttu-id="f8d82-129">Otwórz plik OrdersContext.cs.</span><span class="sxs-lookup"><span data-stu-id="f8d82-129">Open the OrdersContext.cs file.</span></span> <span data-ttu-id="f8d82-130">Zwróć uwagę, że Konstruktor Określa nazwę parametrów połączenia bazy danych.</span><span class="sxs-lookup"><span data-stu-id="f8d82-130">Notice that the constructor specifies the name of the database connection string.</span></span> <span data-ttu-id="f8d82-131">Ta nazwa odwołuje się do ciągu połączenia, który został dodany do pliku Web.config.</span><span class="sxs-lookup"><span data-stu-id="f8d82-131">This name refers to the connection string that was added to Web.config.</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-3/samples/sample1.cs)]

<span data-ttu-id="f8d82-132">Dodaj następujące właściwości `OrdersContext` klasy:</span><span class="sxs-lookup"><span data-stu-id="f8d82-132">Add the following properties to the `OrdersContext` class:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-3/samples/sample2.cs)]

<span data-ttu-id="f8d82-133">A **DbSet** reprezentuje zestaw jednostek, które można wyszukiwać.</span><span class="sxs-lookup"><span data-stu-id="f8d82-133">A **DbSet** represents a set of entities that can be queried.</span></span> <span data-ttu-id="f8d82-134">Poniżej przedstawiono pełną listę `OrdersContext` klasy:</span><span class="sxs-lookup"><span data-stu-id="f8d82-134">Here is the complete listing for the `OrdersContext` class:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-3/samples/sample3.cs)]

<span data-ttu-id="f8d82-135">`AdminController` Klasa definiuje pięć metod, które implementują podstawowe funkcje CRUD.</span><span class="sxs-lookup"><span data-stu-id="f8d82-135">The `AdminController` class defines five methods that implement basic CRUD functionality.</span></span> <span data-ttu-id="f8d82-136">Każda metoda odnosi się do identyfikatora URI, który można wywołać klienta:</span><span class="sxs-lookup"><span data-stu-id="f8d82-136">Each method corresponds to a URI that the client can invoke:</span></span>

| <span data-ttu-id="f8d82-137">Kontroler — metoda</span><span class="sxs-lookup"><span data-stu-id="f8d82-137">Controller Method</span></span> | <span data-ttu-id="f8d82-138">Opis</span><span class="sxs-lookup"><span data-stu-id="f8d82-138">Description</span></span> | <span data-ttu-id="f8d82-139">Identyfikator URI</span><span class="sxs-lookup"><span data-stu-id="f8d82-139">URI</span></span> | <span data-ttu-id="f8d82-140">Metoda HTTP</span><span class="sxs-lookup"><span data-stu-id="f8d82-140">HTTP Method</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="f8d82-141">GetProducts</span><span class="sxs-lookup"><span data-stu-id="f8d82-141">GetProducts</span></span> | <span data-ttu-id="f8d82-142">Pobiera wszystkie produkty.</span><span class="sxs-lookup"><span data-stu-id="f8d82-142">Gets all products.</span></span> | <span data-ttu-id="f8d82-143">Interfejs API/produktów</span><span class="sxs-lookup"><span data-stu-id="f8d82-143">api/products</span></span> | <span data-ttu-id="f8d82-144">GET</span><span class="sxs-lookup"><span data-stu-id="f8d82-144">GET</span></span> |
| <span data-ttu-id="f8d82-145">GetProduct</span><span class="sxs-lookup"><span data-stu-id="f8d82-145">GetProduct</span></span> | <span data-ttu-id="f8d82-146">Wyszukuje produktu według identyfikatora.</span><span class="sxs-lookup"><span data-stu-id="f8d82-146">Finds a product by ID.</span></span> | <span data-ttu-id="f8d82-147">produkty/API/*id*</span><span class="sxs-lookup"><span data-stu-id="f8d82-147">api/products/*id*</span></span> | <span data-ttu-id="f8d82-148">GET</span><span class="sxs-lookup"><span data-stu-id="f8d82-148">GET</span></span> |
| <span data-ttu-id="f8d82-149">PutProduct</span><span class="sxs-lookup"><span data-stu-id="f8d82-149">PutProduct</span></span> | <span data-ttu-id="f8d82-150">Aktualizacje produktu.</span><span class="sxs-lookup"><span data-stu-id="f8d82-150">Updates a product.</span></span> | <span data-ttu-id="f8d82-151">produkty/API/*id*</span><span class="sxs-lookup"><span data-stu-id="f8d82-151">api/products/*id*</span></span> | <span data-ttu-id="f8d82-152">UMIEŚĆ</span><span class="sxs-lookup"><span data-stu-id="f8d82-152">PUT</span></span> |
| <span data-ttu-id="f8d82-153">PostProduct</span><span class="sxs-lookup"><span data-stu-id="f8d82-153">PostProduct</span></span> | <span data-ttu-id="f8d82-154">Tworzy nowego produktu.</span><span class="sxs-lookup"><span data-stu-id="f8d82-154">Creates a new product.</span></span> | <span data-ttu-id="f8d82-155">Interfejs API/produktów</span><span class="sxs-lookup"><span data-stu-id="f8d82-155">api/products</span></span> | <span data-ttu-id="f8d82-156">POST</span><span class="sxs-lookup"><span data-stu-id="f8d82-156">POST</span></span> |
| <span data-ttu-id="f8d82-157">DeleteProduct</span><span class="sxs-lookup"><span data-stu-id="f8d82-157">DeleteProduct</span></span> | <span data-ttu-id="f8d82-158">Usuwa produktu.</span><span class="sxs-lookup"><span data-stu-id="f8d82-158">Deletes a product.</span></span> | <span data-ttu-id="f8d82-159">produkty/API/*id*</span><span class="sxs-lookup"><span data-stu-id="f8d82-159">api/products/*id*</span></span> | <span data-ttu-id="f8d82-160">DELETE</span><span class="sxs-lookup"><span data-stu-id="f8d82-160">DELETE</span></span> |

<span data-ttu-id="f8d82-161">Każda metoda wywołuje do `OrdersContext` aby w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="f8d82-161">Each method calls into `OrdersContext` to query the database.</span></span> <span data-ttu-id="f8d82-162">Wywołanie metody, które modyfikują kolekcji (PUT, POST i DELETE) `db.SaveChanges` do utrwalania zmian w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="f8d82-162">The methods that modify the collection (PUT, POST, and DELETE) call `db.SaveChanges` to persist the changes to the database.</span></span> <span data-ttu-id="f8d82-163">Kontrolery są tworzone na żądanie HTTP i następnie usunięty, dlatego należy zachować zmiany przed metoda zwraca.</span><span class="sxs-lookup"><span data-stu-id="f8d82-163">Controllers are created per HTTP request and then disposed, so it is necessary to persist changes before a method returns.</span></span>

## <a name="add-a-database-initializer"></a><span data-ttu-id="f8d82-164">Dodaj inicjatora bazy danych</span><span class="sxs-lookup"><span data-stu-id="f8d82-164">Add a Database Initializer</span></span>

<span data-ttu-id="f8d82-165">Entity Framework ma nieuprzywilejowany funkcja, która umożliwia wypełnienie bazy danych podczas uruchamiania i automatycznie ponownie utworzyć bazy danych, zmianie modeli.</span><span class="sxs-lookup"><span data-stu-id="f8d82-165">Entity Framework has a nice feature that lets you populate the database on startup, and automatically recreate the database whenever the models change.</span></span> <span data-ttu-id="f8d82-166">Ta funkcja jest przydatna podczas tworzenia, ponieważ zawsze dane testowe, nawet w przypadku zmiany modelu.</span><span class="sxs-lookup"><span data-stu-id="f8d82-166">This feature is useful during development, because you always have some test data, even if you change the models.</span></span>

<span data-ttu-id="f8d82-167">W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy folder modeli i Utwórz nową klasę o nazwie `OrdersContextInitializer`.</span><span class="sxs-lookup"><span data-stu-id="f8d82-167">In Solution Explorer, right-click the Models folder and create a new class named `OrdersContextInitializer`.</span></span> <span data-ttu-id="f8d82-168">Wklej następujący implementacji:</span><span class="sxs-lookup"><span data-stu-id="f8d82-168">Paste in the following implementation:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-3/samples/sample4.cs)]

<span data-ttu-id="f8d82-169">Poprzez dziedziczenie z **DropCreateDatabaseIfModelChanges** klasy, firma Microsoft informuje Entity Framework do usunięcia bazy danych, gdy firma Microsoft zmodyfikować klasy modelu.</span><span class="sxs-lookup"><span data-stu-id="f8d82-169">By inheriting from the **DropCreateDatabaseIfModelChanges** class, we are telling Entity Framework to drop the database whenever we modify the model classes.</span></span> <span data-ttu-id="f8d82-170">Kiedy Entity Framework utworzy (lub odtwarza) bazy danych, wywołuje **inicjatora** metodę, aby wypełnić tabel.</span><span class="sxs-lookup"><span data-stu-id="f8d82-170">When Entity Framework creates (or recreates) the database, it calls the **Seed** method to populate the tables.</span></span> <span data-ttu-id="f8d82-171">Używamy **inicjatora** metody w celu dodania przykład niektórych produktów, oraz przykład zamówienia.</span><span class="sxs-lookup"><span data-stu-id="f8d82-171">We use the **Seed** method to add some example products plus an example order.</span></span>

<span data-ttu-id="f8d82-172">Ta funkcja stanowi doskonałe rozwiązanie do testowania, ale nie należy używać **DropCreateDatabaseIfModelChanges** klasy w środowisku produkcyjnym, ponieważ zmiana klasę modelu może spowodować utratę danych.</span><span class="sxs-lookup"><span data-stu-id="f8d82-172">This feature is great for testing, but don't use the **DropCreateDatabaseIfModelChanges** class in production,, because you could lose your data if someone changes a model class.</span></span>

<span data-ttu-id="f8d82-173">Następnie otwórz plik Global.asax i Dodaj następujący kod do **aplikacji\_Start** metody:</span><span class="sxs-lookup"><span data-stu-id="f8d82-173">Next, open Global.asax and add the following code to the **Application\_Start** method:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-3/samples/sample5.cs)]

## <a name="send-a-request-to-the-controller"></a><span data-ttu-id="f8d82-174">Wyślij żądanie do kontrolera</span><span class="sxs-lookup"><span data-stu-id="f8d82-174">Send a Request to the Controller</span></span>

<span data-ttu-id="f8d82-175">Na tym etapie firma Microsoft nie zostały zapisane żadnego kodu klienta, ale można wywołać interfejsu API przy użyciu przeglądarki sieci web lub debugowania HTTP takich jak narzędzia sieci web [Fiddler](http://www.fiddler2.com/fiddler2/).</span><span class="sxs-lookup"><span data-stu-id="f8d82-175">At this point, we haven't written any client code, but you can invoke the web API using a web browser or an HTTP debugging tool such as [Fiddler](http://www.fiddler2.com/fiddler2/).</span></span> <span data-ttu-id="f8d82-176">W programie Visual Studio naciśnij klawisz F5, aby rozpocząć debugowania.</span><span class="sxs-lookup"><span data-stu-id="f8d82-176">In Visual Studio, press F5 to start debugging.</span></span> <span data-ttu-id="f8d82-177">Zostanie otwarta w przeglądarce sieci web `http://localhost:*portnum*/`, gdzie *portnum* niektórych numer portu.</span><span class="sxs-lookup"><span data-stu-id="f8d82-177">Your web browser will open to `http://localhost:*portnum*/`, where *portnum* is some port number.</span></span>

<span data-ttu-id="f8d82-178">Wyślij żądanie HTTP skierowane do "`http://localhost:*portnum*/api/admin`.</span><span class="sxs-lookup"><span data-stu-id="f8d82-178">Send an HTTP request to "`http://localhost:*portnum*/api/admin`.</span></span> <span data-ttu-id="f8d82-179">Pierwsze żądanie może być powolne, ponieważ Entify Framework musi utworzyć i inicjatora bazy danych.</span><span class="sxs-lookup"><span data-stu-id="f8d82-179">The first request may be slow to complete, because Entify Framework needs to create and seed the database.</span></span> <span data-ttu-id="f8d82-180">Odpowiedź powinna coś podobnego do następującego:</span><span class="sxs-lookup"><span data-stu-id="f8d82-180">The response should something similar to the following:</span></span>

[!code-console[Main](using-web-api-with-entity-framework-part-3/samples/sample6.cmd)]

> [!div class="step-by-step"]
> <span data-ttu-id="f8d82-181">[Poprzednie](using-web-api-with-entity-framework-part-2.md)
> [dalej](using-web-api-with-entity-framework-part-4.md)</span><span class="sxs-lookup"><span data-stu-id="f8d82-181">[Previous](using-web-api-with-entity-framework-part-2.md)
[Next](using-web-api-with-entity-framework-part-4.md)</span></span>
