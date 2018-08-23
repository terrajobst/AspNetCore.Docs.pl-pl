---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-3
title: 'Część 3: Tworzenie kontrolera administratora | Dokumentacja firmy Microsoft'
author: MikeWasson
description: ''
ms.author: riande
ms.date: 07/04/2012
ms.assetid: 6b9ae3c4-0274-4170-a1bb-9df9c546b2a9
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-3
msc.type: authoredcontent
ms.openlocfilehash: 7ad0ec27021514b447e569e479a9e9127e3f75fa
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/16/2018
ms.locfileid: "41757146"
---
<a name="part-3-creating-an-admin-controller"></a><span data-ttu-id="6b0e6-102">Część 3: Tworzenie kontrolera administratora</span><span class="sxs-lookup"><span data-stu-id="6b0e6-102">Part 3: Creating an Admin Controller</span></span>
====================
<span data-ttu-id="6b0e6-103">przez [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="6b0e6-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="6b0e6-104">Pobieranie ukończone projektu</span><span class="sxs-lookup"><span data-stu-id="6b0e6-104">Download Completed Project</span></span>](http://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

## <a name="add-an-admin-controller"></a><span data-ttu-id="6b0e6-105">Dodawanie kontrolera administratora</span><span class="sxs-lookup"><span data-stu-id="6b0e6-105">Add an Admin Controller</span></span>

<span data-ttu-id="6b0e6-106">W tej sekcji dodamy kontroler Web API, który obsługuje CRUD (tworzenia, odczytu, aktualizacji i usuwania) operacje na produkty.</span><span class="sxs-lookup"><span data-stu-id="6b0e6-106">In this section, we'll add a Web API controller that supports CRUD (create, read, update, and delete) operations on products.</span></span> <span data-ttu-id="6b0e6-107">Kontroler będzie używać programu Entity Framework do komunikowania się z warstwą bazy danych.</span><span class="sxs-lookup"><span data-stu-id="6b0e6-107">The controller will use Entity Framework to communicate with the database layer.</span></span> <span data-ttu-id="6b0e6-108">Tylko administratorzy będą mogli korzystać z tego kontrolera.</span><span class="sxs-lookup"><span data-stu-id="6b0e6-108">Only administrators will be able to use this controller.</span></span> <span data-ttu-id="6b0e6-109">Klienci będą uzyskiwać dostęp do produktów, za pomocą innego kontrolera.</span><span class="sxs-lookup"><span data-stu-id="6b0e6-109">Customers will access the products through another controller.</span></span>

<span data-ttu-id="6b0e6-110">W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy folder kontrolerów.</span><span class="sxs-lookup"><span data-stu-id="6b0e6-110">In Solution Explorer, right-click the Controllers folder.</span></span> <span data-ttu-id="6b0e6-111">Wybierz **Dodaj** i następnie **kontrolera**.</span><span class="sxs-lookup"><span data-stu-id="6b0e6-111">Select **Add** and then **Controller**.</span></span>

![](using-web-api-with-entity-framework-part-3/_static/image1.png)

<span data-ttu-id="6b0e6-112">W **Dodaj kontroler** okno dialogowe, nazwy kontrolera `AdminController`.</span><span class="sxs-lookup"><span data-stu-id="6b0e6-112">In the **Add Controller** dialog, name the controller `AdminController`.</span></span> <span data-ttu-id="6b0e6-113">W obszarze **szablonu**, wybierz opcję &quot;Kontroler interfejsu API z akcjami odczytu/zapisu, używający narzędzia Entity Framework&quot;.</span><span class="sxs-lookup"><span data-stu-id="6b0e6-113">Under **Template**, select &quot;API controller with read/write actions, using Entity Framework&quot;.</span></span> <span data-ttu-id="6b0e6-114">W obszarze **klasa modelu**, wybierz pozycję "Product (ProductStore.Models)".</span><span class="sxs-lookup"><span data-stu-id="6b0e6-114">Under **Model class**, select "Product (ProductStore.Models)".</span></span> <span data-ttu-id="6b0e6-115">W obszarze **kontekstu danych**, wybierz opcję "&lt;nowy kontekst danych&gt;".</span><span class="sxs-lookup"><span data-stu-id="6b0e6-115">Under **Data Context**, select "&lt;New Data Context&gt;".</span></span>

![](using-web-api-with-entity-framework-part-3/_static/image2.png)

> [!NOTE]
> <span data-ttu-id="6b0e6-116">Jeśli **klasa modelu** listy rozwijanej nie są wyświetlane wszystkie klasy modelu, upewnij się, skompilowany projekt.</span><span class="sxs-lookup"><span data-stu-id="6b0e6-116">If the **Model class** drop-down does not show any model classes, make sure you compiled the project.</span></span> <span data-ttu-id="6b0e6-117">Entity Framework używa odbicia, więc wymaga skompilowanym zestawie.</span><span class="sxs-lookup"><span data-stu-id="6b0e6-117">Entity Framework uses reflection, so it needs the compiled assembly.</span></span>


<span data-ttu-id="6b0e6-118">Wybieranie "&lt;nowy kontekst danych&gt;" spowoduje otwarcie **nowy kontekst danych** okna dialogowego.</span><span class="sxs-lookup"><span data-stu-id="6b0e6-118">Selecting "&lt;New Data Context&gt;" will open the **New Data Context** dialog.</span></span> <span data-ttu-id="6b0e6-119">Nazwa kontekstu danych `ProductStore.Models.OrdersContext`.</span><span class="sxs-lookup"><span data-stu-id="6b0e6-119">Name the data context `ProductStore.Models.OrdersContext`.</span></span>

![](using-web-api-with-entity-framework-part-3/_static/image3.png)

<span data-ttu-id="6b0e6-120">Kliknij przycisk **OK** odrzucać **nowy kontekst danych** okna dialogowego.</span><span class="sxs-lookup"><span data-stu-id="6b0e6-120">Click **OK** to dismiss the **New Data Context** dialog.</span></span> <span data-ttu-id="6b0e6-121">W **Dodaj kontroler** okno dialogowe, kliknij przycisk **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="6b0e6-121">In the **Add Controller** dialog, click **Add**.</span></span>

<span data-ttu-id="6b0e6-122">Oto, co stało się dodane do projektu:</span><span class="sxs-lookup"><span data-stu-id="6b0e6-122">Here's what got added to the project:</span></span>

- <span data-ttu-id="6b0e6-123">Klasa o nazwie `OrdersContext` który pochodzi od klasy **DbContext**.</span><span class="sxs-lookup"><span data-stu-id="6b0e6-123">A class named `OrdersContext` that derives from **DbContext**.</span></span> <span data-ttu-id="6b0e6-124">Ta klasa dostarcza pośredniczącego między modelami POCO i bazy danych.</span><span class="sxs-lookup"><span data-stu-id="6b0e6-124">This class provides the glue between the POCO models and the database.</span></span>
- <span data-ttu-id="6b0e6-125">Kontroler Web API o nazwie `AdminController`.</span><span class="sxs-lookup"><span data-stu-id="6b0e6-125">A Web API controller named `AdminController`.</span></span> <span data-ttu-id="6b0e6-126">Ten kontroler obsługuje operacje CRUD na `Product` wystąpień.</span><span class="sxs-lookup"><span data-stu-id="6b0e6-126">This controller supports CRUD operations on `Product` instances.</span></span> <span data-ttu-id="6b0e6-127">Używa ona `OrdersContext` klasy do komunikowania się z platformą Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="6b0e6-127">It uses the `OrdersContext` class to communicate with Entity Framework.</span></span>
- <span data-ttu-id="6b0e6-128">Nowe parametry połączenia bazy danych w pliku Web.config.</span><span class="sxs-lookup"><span data-stu-id="6b0e6-128">A new database connection string in the Web.config file.</span></span>

![](using-web-api-with-entity-framework-part-3/_static/image4.png)

<span data-ttu-id="6b0e6-129">Otwórz plik OrdersContext.cs.</span><span class="sxs-lookup"><span data-stu-id="6b0e6-129">Open the OrdersContext.cs file.</span></span> <span data-ttu-id="6b0e6-130">Należy zauważyć, że Konstruktor Określa nazwę parametrów połączenia bazy danych.</span><span class="sxs-lookup"><span data-stu-id="6b0e6-130">Notice that the constructor specifies the name of the database connection string.</span></span> <span data-ttu-id="6b0e6-131">Ta nazwa odnosi się do ciągu połączenia, który został dodany do pliku Web.config.</span><span class="sxs-lookup"><span data-stu-id="6b0e6-131">This name refers to the connection string that was added to Web.config.</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-3/samples/sample1.cs)]

<span data-ttu-id="6b0e6-132">Dodaj następujące właściwości do `OrdersContext` klasy:</span><span class="sxs-lookup"><span data-stu-id="6b0e6-132">Add the following properties to the `OrdersContext` class:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-3/samples/sample2.cs)]

<span data-ttu-id="6b0e6-133">A **DbSet** reprezentuje zestaw jednostek, które mogą być wyszukiwane.</span><span class="sxs-lookup"><span data-stu-id="6b0e6-133">A **DbSet** represents a set of entities that can be queried.</span></span> <span data-ttu-id="6b0e6-134">Oto Pełna lista dla `OrdersContext` klasy:</span><span class="sxs-lookup"><span data-stu-id="6b0e6-134">Here is the complete listing for the `OrdersContext` class:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-3/samples/sample3.cs)]

<span data-ttu-id="6b0e6-135">`AdminController` Klasa definiuje pięć metod, które implementują podstawowych funkcji CRUD.</span><span class="sxs-lookup"><span data-stu-id="6b0e6-135">The `AdminController` class defines five methods that implement basic CRUD functionality.</span></span> <span data-ttu-id="6b0e6-136">Każda metoda odnosi się do identyfikatora URI, które mogą być wywoływane przez klienta:</span><span class="sxs-lookup"><span data-stu-id="6b0e6-136">Each method corresponds to a URI that the client can invoke:</span></span>

| <span data-ttu-id="6b0e6-137">Metoda kontrolera</span><span class="sxs-lookup"><span data-stu-id="6b0e6-137">Controller Method</span></span> | <span data-ttu-id="6b0e6-138">Opis</span><span class="sxs-lookup"><span data-stu-id="6b0e6-138">Description</span></span> | <span data-ttu-id="6b0e6-139">Identyfikator URI</span><span class="sxs-lookup"><span data-stu-id="6b0e6-139">URI</span></span> | <span data-ttu-id="6b0e6-140">Metoda HTTP</span><span class="sxs-lookup"><span data-stu-id="6b0e6-140">HTTP Method</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="6b0e6-141">GetProducts</span><span class="sxs-lookup"><span data-stu-id="6b0e6-141">GetProducts</span></span> | <span data-ttu-id="6b0e6-142">Pobiera wszystkie produkty.</span><span class="sxs-lookup"><span data-stu-id="6b0e6-142">Gets all products.</span></span> | <span data-ttu-id="6b0e6-143">Interfejs API/produktów</span><span class="sxs-lookup"><span data-stu-id="6b0e6-143">api/products</span></span> | <span data-ttu-id="6b0e6-144">POBIERZ</span><span class="sxs-lookup"><span data-stu-id="6b0e6-144">GET</span></span> |
| <span data-ttu-id="6b0e6-145">GetProduct</span><span class="sxs-lookup"><span data-stu-id="6b0e6-145">GetProduct</span></span> | <span data-ttu-id="6b0e6-146">Umożliwia znalezienie produktu według identyfikatora.</span><span class="sxs-lookup"><span data-stu-id="6b0e6-146">Finds a product by ID.</span></span> | <span data-ttu-id="6b0e6-147">InterfejsAPI/produkty/*identyfikator*</span><span class="sxs-lookup"><span data-stu-id="6b0e6-147">api/products/*id*</span></span> | <span data-ttu-id="6b0e6-148">POBIERZ</span><span class="sxs-lookup"><span data-stu-id="6b0e6-148">GET</span></span> |
| <span data-ttu-id="6b0e6-149">PutProduct</span><span class="sxs-lookup"><span data-stu-id="6b0e6-149">PutProduct</span></span> | <span data-ttu-id="6b0e6-150">Aktualizacje produktu.</span><span class="sxs-lookup"><span data-stu-id="6b0e6-150">Updates a product.</span></span> | <span data-ttu-id="6b0e6-151">InterfejsAPI/produkty/*identyfikator*</span><span class="sxs-lookup"><span data-stu-id="6b0e6-151">api/products/*id*</span></span> | <span data-ttu-id="6b0e6-152">PUT</span><span class="sxs-lookup"><span data-stu-id="6b0e6-152">PUT</span></span> |
| <span data-ttu-id="6b0e6-153">PostProduct</span><span class="sxs-lookup"><span data-stu-id="6b0e6-153">PostProduct</span></span> | <span data-ttu-id="6b0e6-154">Tworzy nowy produkt.</span><span class="sxs-lookup"><span data-stu-id="6b0e6-154">Creates a new product.</span></span> | <span data-ttu-id="6b0e6-155">Interfejs API/produktów</span><span class="sxs-lookup"><span data-stu-id="6b0e6-155">api/products</span></span> | <span data-ttu-id="6b0e6-156">POST</span><span class="sxs-lookup"><span data-stu-id="6b0e6-156">POST</span></span> |
| <span data-ttu-id="6b0e6-157">DeleteProduct</span><span class="sxs-lookup"><span data-stu-id="6b0e6-157">DeleteProduct</span></span> | <span data-ttu-id="6b0e6-158">Usuwa produkt.</span><span class="sxs-lookup"><span data-stu-id="6b0e6-158">Deletes a product.</span></span> | <span data-ttu-id="6b0e6-159">InterfejsAPI/produkty/*identyfikator*</span><span class="sxs-lookup"><span data-stu-id="6b0e6-159">api/products/*id*</span></span> | <span data-ttu-id="6b0e6-160">DELETE</span><span class="sxs-lookup"><span data-stu-id="6b0e6-160">DELETE</span></span> |

<span data-ttu-id="6b0e6-161">Każda metoda wywoła `OrdersContext` do wykonywania zapytań w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="6b0e6-161">Each method calls into `OrdersContext` to query the database.</span></span> <span data-ttu-id="6b0e6-162">Wywołanie metody, które zmodyfikować kolekcji (PUT, POST i DELETE) `db.SaveChanges` aby utrwalić zmiany w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="6b0e6-162">The methods that modify the collection (PUT, POST, and DELETE) call `db.SaveChanges` to persist the changes to the database.</span></span> <span data-ttu-id="6b0e6-163">Kontrolery są tworzone na żądanie HTTP i następnie usunięty tak jest zachować zmiany, zanim metoda zwraca.</span><span class="sxs-lookup"><span data-stu-id="6b0e6-163">Controllers are created per HTTP request and then disposed, so it is necessary to persist changes before a method returns.</span></span>

## <a name="add-a-database-initializer"></a><span data-ttu-id="6b0e6-164">Dodaj inicjator bazy danych</span><span class="sxs-lookup"><span data-stu-id="6b0e6-164">Add a Database Initializer</span></span>

<span data-ttu-id="6b0e6-165">Entity Framework ma ładny funkcja, która umożliwia wypełnienie bazy danych podczas uruchamiania i automatycznie odtworzy bazy danych, po każdym wprowadzeniu zmiany modeli.</span><span class="sxs-lookup"><span data-stu-id="6b0e6-165">Entity Framework has a nice feature that lets you populate the database on startup, and automatically recreate the database whenever the models change.</span></span> <span data-ttu-id="6b0e6-166">Ta funkcja jest przydatna podczas tworzenia aplikacji, ponieważ zawsze masz dane testowe nawet wtedy, gdy zmienisz modeli.</span><span class="sxs-lookup"><span data-stu-id="6b0e6-166">This feature is useful during development, because you always have some test data, even if you change the models.</span></span>

<span data-ttu-id="6b0e6-167">W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy folderu modeli, a następnie utwórz nową klasę o nazwie `OrdersContextInitializer`.</span><span class="sxs-lookup"><span data-stu-id="6b0e6-167">In Solution Explorer, right-click the Models folder and create a new class named `OrdersContextInitializer`.</span></span> <span data-ttu-id="6b0e6-168">Wklej następującą implementacją:</span><span class="sxs-lookup"><span data-stu-id="6b0e6-168">Paste in the following implementation:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-3/samples/sample4.cs)]

<span data-ttu-id="6b0e6-169">Przez dziedziczenie z **DropCreateDatabaseIfModelChanges** klasy, kolejne parametry programu Entity Framework w celu porzucenia bazy danych zawsze wtedy, gdy zmodyfikujemy klasy modelu.</span><span class="sxs-lookup"><span data-stu-id="6b0e6-169">By inheriting from the **DropCreateDatabaseIfModelChanges** class, we are telling Entity Framework to drop the database whenever we modify the model classes.</span></span> <span data-ttu-id="6b0e6-170">Gdy Entity Framework tworzy (lub odtwarza) bazy danych, wywołuje **inicjatora** metodę, aby wypełnić tabele.</span><span class="sxs-lookup"><span data-stu-id="6b0e6-170">When Entity Framework creates (or recreates) the database, it calls the **Seed** method to populate the tables.</span></span> <span data-ttu-id="6b0e6-171">Używamy **inicjatora** metoda umożliwiająca dodanie niektórych produktów w przykładzie oraz przykładowe zamówienie.</span><span class="sxs-lookup"><span data-stu-id="6b0e6-171">We use the **Seed** method to add some example products plus an example order.</span></span>

<span data-ttu-id="6b0e6-172">Ta funkcja to idealne narzędzie do testowania, ale nie należy używać **DropCreateDatabaseIfModelChanges** klasy w środowisku produkcyjnym, ponieważ może utracić dane, jeśli ktoś zmienia klasę modelu.</span><span class="sxs-lookup"><span data-stu-id="6b0e6-172">This feature is great for testing, but don't use the **DropCreateDatabaseIfModelChanges** class in production,, because you could lose your data if someone changes a model class.</span></span>

<span data-ttu-id="6b0e6-173">Następnie otwórz plik Global.asax i Dodaj następujący kod do **aplikacji\_Start** metody:</span><span class="sxs-lookup"><span data-stu-id="6b0e6-173">Next, open Global.asax and add the following code to the **Application\_Start** method:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-3/samples/sample5.cs)]

## <a name="send-a-request-to-the-controller"></a><span data-ttu-id="6b0e6-174">Wyślij żądanie do kontrolera</span><span class="sxs-lookup"><span data-stu-id="6b0e6-174">Send a Request to the Controller</span></span>

<span data-ttu-id="6b0e6-175">Na tym etapie firma Microsoft nie zostały zapisane jakiegokolwiek kodu klienta, ale można wywołać interfejsu API za pomocą przeglądarki sieci web lub debugowania HTTP narzędzia, takie jak sieci web [Fiddler](http://www.fiddler2.com/fiddler2/).</span><span class="sxs-lookup"><span data-stu-id="6b0e6-175">At this point, we haven't written any client code, but you can invoke the web API using a web browser or an HTTP debugging tool such as [Fiddler](http://www.fiddler2.com/fiddler2/).</span></span> <span data-ttu-id="6b0e6-176">W programie Visual Studio naciśnij klawisz F5, aby rozpocząć debugowanie.</span><span class="sxs-lookup"><span data-stu-id="6b0e6-176">In Visual Studio, press F5 to start debugging.</span></span> <span data-ttu-id="6b0e6-177">Spowoduje to otwarcie przeglądarki sieci web do `http://localhost:*portnum*/`, gdzie *portnum* niektóre numer portu to.</span><span class="sxs-lookup"><span data-stu-id="6b0e6-177">Your web browser will open to `http://localhost:*portnum*/`, where *portnum* is some port number.</span></span>

<span data-ttu-id="6b0e6-178">Wyślij żądanie HTTP do "`http://localhost:*portnum*/api/admin`.</span><span class="sxs-lookup"><span data-stu-id="6b0e6-178">Send an HTTP request to "`http://localhost:*portnum*/api/admin`.</span></span> <span data-ttu-id="6b0e6-179">Pierwsze żądanie może być powolne, ponieważ Entify Framework musi utworzyć i Inicjowanie bazy danych.</span><span class="sxs-lookup"><span data-stu-id="6b0e6-179">The first request may be slow to complete, because Entify Framework needs to create and seed the database.</span></span> <span data-ttu-id="6b0e6-180">Odpowiedź powinna coś podobnego do następującego:</span><span class="sxs-lookup"><span data-stu-id="6b0e6-180">The response should something similar to the following:</span></span>

[!code-console[Main](using-web-api-with-entity-framework-part-3/samples/sample6.cmd)]

> [!div class="step-by-step"]
> <span data-ttu-id="6b0e6-181">[Poprzednie](using-web-api-with-entity-framework-part-2.md)
> [dalej](using-web-api-with-entity-framework-part-4.md)</span><span class="sxs-lookup"><span data-stu-id="6b0e6-181">[Previous](using-web-api-with-entity-framework-part-2.md)
[Next](using-web-api-with-entity-framework-part-4.md)</span></span>
