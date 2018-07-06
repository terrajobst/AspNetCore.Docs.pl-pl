---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-2
title: Dodawanie modeli i kontrolerów | Dokumentacja firmy Microsoft
author: MikeWasson
description: ''
ms.author: aspnetcontent
ms.date: 06/16/2014
ms.assetid: 88908ff8-51a9-40eb-931c-a8139128b680
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-2
msc.type: authoredcontent
ms.openlocfilehash: 31e0fbf65c74a18677588cfa412dc34f7b25f78f
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/05/2018
ms.locfileid: "37806361"
---
<a name="add-models-and-controllers"></a><span data-ttu-id="b9e83-102">Dodawanie modeli i kontrolerów</span><span class="sxs-lookup"><span data-stu-id="b9e83-102">Add Models and Controllers</span></span>
====================
<span data-ttu-id="b9e83-103">przez [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="b9e83-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="b9e83-104">Pobieranie ukończone projektu</span><span class="sxs-lookup"><span data-stu-id="b9e83-104">Download Completed Project</span></span>](https://github.com/MikeWasson/BookService)

<span data-ttu-id="b9e83-105">W tej sekcji dodasz klasy modeli, które definiują jednostek bazy danych.</span><span class="sxs-lookup"><span data-stu-id="b9e83-105">In this section, you will add model classes that define the database entities.</span></span> <span data-ttu-id="b9e83-106">Następnie dodasz kontrolerów internetowych interfejsów API, które wykonują operacje CRUD na tych jednostkach.</span><span class="sxs-lookup"><span data-stu-id="b9e83-106">Then you will add Web API controllers that perform CRUD operations on those entities.</span></span>

## <a name="add-model-classes"></a><span data-ttu-id="b9e83-107">Dodawanie klasy modelu</span><span class="sxs-lookup"><span data-stu-id="b9e83-107">Add Model Classes</span></span>

<span data-ttu-id="b9e83-108">W tym samouczku utworzymy bazy danych przy użyciu podejścia "Code First", aby Entity Framework (EF).</span><span class="sxs-lookup"><span data-stu-id="b9e83-108">In this tutorial, we'll create the database by using the "Code First" approach to Entity Framework (EF).</span></span> <span data-ttu-id="b9e83-109">Code First zapisu klas języka C#, które odnoszą się do tabel bazy danych i EF tworzy bazę danych.</span><span class="sxs-lookup"><span data-stu-id="b9e83-109">With Code First, you write C# classes that correspond to database tables, and EF creates the database.</span></span> <span data-ttu-id="b9e83-110">(Aby uzyskać więcej informacji, zobacz [podejścia do projektowania struktury jednostki](https://msdn.microsoft.com/library/ms178359%28v=vs.110%29.aspx#dbfmfcf).)</span><span class="sxs-lookup"><span data-stu-id="b9e83-110">(For more information, see [Entity Framework Development Approaches](https://msdn.microsoft.com/library/ms178359%28v=vs.110%29.aspx#dbfmfcf).)</span></span>

<span data-ttu-id="b9e83-111">Rozpoczniemy pracę przez zdefiniowanie naszych obiektów domeny jako POCOs (stary zwykły obiektów CLR).</span><span class="sxs-lookup"><span data-stu-id="b9e83-111">We start by defining our domain objects as POCOs (plain-old CLR objects).</span></span> <span data-ttu-id="b9e83-112">Utworzymy POCOs następujące:</span><span class="sxs-lookup"><span data-stu-id="b9e83-112">We will create the following POCOs:</span></span>

- <span data-ttu-id="b9e83-113">Autor</span><span class="sxs-lookup"><span data-stu-id="b9e83-113">Author</span></span>
- <span data-ttu-id="b9e83-114">Książki</span><span class="sxs-lookup"><span data-stu-id="b9e83-114">Book</span></span>

<span data-ttu-id="b9e83-115">W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy kliknij folder modeli.</span><span class="sxs-lookup"><span data-stu-id="b9e83-115">In Solution Explorer, right click the Models folder.</span></span> <span data-ttu-id="b9e83-116">Wybierz **Dodaj**, a następnie wybierz **klasy**.</span><span class="sxs-lookup"><span data-stu-id="b9e83-116">Select **Add**, then select **Class**.</span></span> <span data-ttu-id="b9e83-117">Nazwa klasy `Author`.</span><span class="sxs-lookup"><span data-stu-id="b9e83-117">Name the class `Author`.</span></span>

![](part-2/_static/image1.png)

<span data-ttu-id="b9e83-118">Zamień wszystkie schematyczny kod w Author.cs następujący kod.</span><span class="sxs-lookup"><span data-stu-id="b9e83-118">Replace all of the boilerplate code in Author.cs with the following code.</span></span>

[!code-csharp[Main](part-2/samples/sample1.cs)]

<span data-ttu-id="b9e83-119">Dodaj klasę o nazwie `Book`, używając następującego kodu.</span><span class="sxs-lookup"><span data-stu-id="b9e83-119">Add another class named `Book`, with the following code.</span></span>

[!code-csharp[Main](part-2/samples/sample2.cs)]

<span data-ttu-id="b9e83-120">Entity Framework użyje tych modeli do tworzenia tabel bazy danych.</span><span class="sxs-lookup"><span data-stu-id="b9e83-120">Entity Framework will use these models to create database tables.</span></span> <span data-ttu-id="b9e83-121">Dla każdego modelu `Id` Właściwości staną się kolumna klucza podstawowego w tabeli bazy danych.</span><span class="sxs-lookup"><span data-stu-id="b9e83-121">For each model, the `Id` property will become the primary key column of the database table.</span></span>

<span data-ttu-id="b9e83-122">W klasie książki `AuthorId` definiuje klucz obcy do `Author` tabeli.</span><span class="sxs-lookup"><span data-stu-id="b9e83-122">In the Book class, the `AuthorId` defines a foreign key into the `Author` table.</span></span> <span data-ttu-id="b9e83-123">(Dla uproszczenia I jestem przy założeniu, że poszczególne książki ma jednego autora.) Klasa książki zawiera także właściwości nawigacji w celu powiązane `Author`.</span><span class="sxs-lookup"><span data-stu-id="b9e83-123">(For simplicity, I'm assuming that each book has a single author.) The book class also contains a navigation property to the related `Author`.</span></span> <span data-ttu-id="b9e83-124">Właściwość nawigacji umożliwia dostęp do odnośnych `Author` w kodzie.</span><span class="sxs-lookup"><span data-stu-id="b9e83-124">You can use the navigation property to access the related `Author` in code.</span></span> <span data-ttu-id="b9e83-125">Więcej informacji na temat właściwości nawigacji, część 4, mówię [Obsługa relacji jednostek](part-4.md).</span><span class="sxs-lookup"><span data-stu-id="b9e83-125">I say more about navigation properties in part 4, [Handling Entity Relations](part-4.md).</span></span>

## <a name="add-web-api-controllers"></a><span data-ttu-id="b9e83-126">Dodaj kontrolery interfejsu API sieci Web</span><span class="sxs-lookup"><span data-stu-id="b9e83-126">Add Web API Controllers</span></span>

<span data-ttu-id="b9e83-127">W tej sekcji dodamy kontrolerów internetowych interfejsów API, które obsługują operacje CRUD (tworzenia, odczytu, aktualizacji i usuwania).</span><span class="sxs-lookup"><span data-stu-id="b9e83-127">In this section, we'll add Web API controllers that support CRUD operations (create, read, update, and delete).</span></span> <span data-ttu-id="b9e83-128">Kontrolery będzie używać programu Entity Framework do komunikowania się z warstwą bazy danych.</span><span class="sxs-lookup"><span data-stu-id="b9e83-128">The controllers will use Entity Framework to communicate with the database layer.</span></span>

<span data-ttu-id="b9e83-129">Po pierwsze można usunąć pliku Controllers/ValuesController.cs.</span><span class="sxs-lookup"><span data-stu-id="b9e83-129">First, you can delete the file Controllers/ValuesController.cs.</span></span> <span data-ttu-id="b9e83-130">Ten plik zawiera przykład kontroler interfejsu API sieci Web, ale nie są potrzebne w tym samouczku.</span><span class="sxs-lookup"><span data-stu-id="b9e83-130">This file contains an example Web API controller, but you don't need it for this tutorial.</span></span>

![](part-2/_static/image2.png)

<span data-ttu-id="b9e83-131">Następnie Skompiluj projekt.</span><span class="sxs-lookup"><span data-stu-id="b9e83-131">Next, build the project.</span></span> <span data-ttu-id="b9e83-132">Tworzenie szkieletu interfejsu API sieci Web używa odbicia, można znaleźć klasy modeli, dlatego potrzebuje skompilowanym zestawie.</span><span class="sxs-lookup"><span data-stu-id="b9e83-132">The Web API scaffolding uses reflection to find the model classes, so it needs the compiled assembly.</span></span>

<span data-ttu-id="b9e83-133">W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy folder kontrolerów.</span><span class="sxs-lookup"><span data-stu-id="b9e83-133">In Solution Explorer, right-click the Controllers folder.</span></span> <span data-ttu-id="b9e83-134">Wybierz **Dodaj**, a następnie wybierz **kontrolera**.</span><span class="sxs-lookup"><span data-stu-id="b9e83-134">Select **Add**, then select **Controller**.</span></span>

![](part-2/_static/image3.png)

<span data-ttu-id="b9e83-135">W **Dodawanie szkieletu** okno dialogowe, wybierz opcję "Web API 2 kontroler z akcjami używający narzędzia Entity Framework".</span><span class="sxs-lookup"><span data-stu-id="b9e83-135">In the **Add Scaffold** dialog, select "Web API 2 Controller with actions, using Entity Framework".</span></span> <span data-ttu-id="b9e83-136">Kliknij przycisk **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="b9e83-136">Click **Add**.</span></span>

![](part-2/_static/image4.png)

<span data-ttu-id="b9e83-137">W **Dodaj kontroler** okna dialogowego, wykonaj następujące czynności:</span><span class="sxs-lookup"><span data-stu-id="b9e83-137">In the **Add Controller** dialog, do the following:</span></span>

1. <span data-ttu-id="b9e83-138">W **klasa modelu** listy rozwijanej wybierz `Author` klasy.</span><span class="sxs-lookup"><span data-stu-id="b9e83-138">In the **Model class** dropdown, select the `Author` class.</span></span> <span data-ttu-id="b9e83-139">(Jeśli widzisz go na liście rozwijanej, upewnij się, że utworzony projekt.)</span><span class="sxs-lookup"><span data-stu-id="b9e83-139">(If you don't see it listed in the dropdown, make sure that you built the project.)</span></span>
2. <span data-ttu-id="b9e83-140">Sprawdź "Użyj asynchronicznych akcji kontrolera".</span><span class="sxs-lookup"><span data-stu-id="b9e83-140">Check "Use async controller actions".</span></span>
3. <span data-ttu-id="b9e83-141">Pozostaw nazwę kontrolera &quot;AuthorsController&quot;.</span><span class="sxs-lookup"><span data-stu-id="b9e83-141">Leave the controller name as &quot;AuthorsController&quot;.</span></span>
4. <span data-ttu-id="b9e83-142">Kliknij przycisk plus (+) znajdujący się obok **klasa kontekstu danych**.</span><span class="sxs-lookup"><span data-stu-id="b9e83-142">Click plus (+) button next to **Data Context Class**.</span></span>

![](part-2/_static/image5.png)

<span data-ttu-id="b9e83-143">W **nowy kontekst danych** okno dialogowe, pozostaw nazwę domyślną, a następnie kliknij przycisk **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="b9e83-143">In the **New Data Context** dialog, leave the default name and click **Add**.</span></span>

![](part-2/_static/image6.png)

<span data-ttu-id="b9e83-144">Kliknij przycisk **Dodaj** do ukończenia **Dodaj kontroler** okna dialogowego.</span><span class="sxs-lookup"><span data-stu-id="b9e83-144">Click **Add** to complete the **Add Controller** dialog.</span></span> <span data-ttu-id="b9e83-145">Okno dialogowe powoduje dodanie dwóch klas do projektu:</span><span class="sxs-lookup"><span data-stu-id="b9e83-145">The dialog adds two classes to your project:</span></span>

- <span data-ttu-id="b9e83-146">`AuthorsController` Definiuje kontrolera interfejsu API sieci Web.</span><span class="sxs-lookup"><span data-stu-id="b9e83-146">`AuthorsController` defines a Web API controller.</span></span> <span data-ttu-id="b9e83-147">Kontroler implementuje interfejs API REST, używanego przez klientów w celu wykonywania operacji CRUD na liście autorów.</span><span class="sxs-lookup"><span data-stu-id="b9e83-147">The controller implements the REST API that clients use to perform CRUD operations on the list of authors.</span></span>
- <span data-ttu-id="b9e83-148">`BookServiceContext` zarządza obiekty jednostki w czasie wykonywania, zawierającą wypełnianie obiektów przy użyciu danych z bazy danych, śledzenie zmian i przechowywanie danych w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="b9e83-148">`BookServiceContext` manages entity objects during run time, which includes populating objects with data from a database, change tracking, and persisting data to the database.</span></span> <span data-ttu-id="b9e83-149">Dziedziczy `DbContext`.</span><span class="sxs-lookup"><span data-stu-id="b9e83-149">It inherits from `DbContext`.</span></span>

![](part-2/_static/image7.png)

<span data-ttu-id="b9e83-150">W tym momencie ponownie skompiluj projekt.</span><span class="sxs-lookup"><span data-stu-id="b9e83-150">At this point, build the project again.</span></span> <span data-ttu-id="b9e83-151">Teraz przechodzić przez te same kroki, aby dodać Kontroler interfejsu API dla `Book` jednostek.</span><span class="sxs-lookup"><span data-stu-id="b9e83-151">Now go through the same steps to add an API controller for `Book` entities.</span></span> <span data-ttu-id="b9e83-152">Tym razem wybierz pozycję `Book` klasy modelu, a następnie wybierz istniejący `BookServiceContext` klasy dla klasy kontekstu danych.</span><span class="sxs-lookup"><span data-stu-id="b9e83-152">This time, select `Book` for the model class, and select the existing `BookServiceContext` class for the data context class.</span></span> <span data-ttu-id="b9e83-153">(Nie twórz nowy kontekst danych). Kliknij przycisk **Dodaj** można dodać kontrolera.</span><span class="sxs-lookup"><span data-stu-id="b9e83-153">(Don't create a new data context.) Click **Add** to add the controller.</span></span>

![](part-2/_static/image8.png)

> [!div class="step-by-step"]
> <span data-ttu-id="b9e83-154">[Poprzednie](part-1.md)
> [dalej](part-3.md)</span><span class="sxs-lookup"><span data-stu-id="b9e83-154">[Previous](part-1.md)
[Next](part-3.md)</span></span>
