---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-2
title: "Dodaj modele i kontrolerów | Dokumentacja firmy Microsoft"
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/16/2014
ms.topic: article
ms.assetid: 88908ff8-51a9-40eb-931c-a8139128b680
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-2
msc.type: authoredcontent
ms.openlocfilehash: b75eae11fd99b60864256f79d4770a3007487964
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
<a name="add-models-and-controllers"></a><span data-ttu-id="940cc-102">Dodaj modele i kontrolerów</span><span class="sxs-lookup"><span data-stu-id="940cc-102">Add Models and Controllers</span></span>
====================
<span data-ttu-id="940cc-103">przez [Wasson Jan](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="940cc-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="940cc-104">Pobieranie ukończone projektu</span><span class="sxs-lookup"><span data-stu-id="940cc-104">Download Completed Project</span></span>](https://github.com/MikeWasson/BookService)

<span data-ttu-id="940cc-105">W tej sekcji dodasz klasy modeli, które definiują jednostki bazy danych.</span><span class="sxs-lookup"><span data-stu-id="940cc-105">In this section, you will add model classes that define the database entities.</span></span> <span data-ttu-id="940cc-106">Następnie należy dodać kontrolerów interfejsu API sieci Web, które wykonują operacje CRUD na tych jednostek.</span><span class="sxs-lookup"><span data-stu-id="940cc-106">Then you will add Web API controllers that perform CRUD operations on those entities.</span></span>

## <a name="add-model-classes"></a><span data-ttu-id="940cc-107">Dodawanie klasy modeli</span><span class="sxs-lookup"><span data-stu-id="940cc-107">Add Model Classes</span></span>

<span data-ttu-id="940cc-108">W tym samouczku utworzymy bazy danych przy użyciu "Code First" podejście do Entity Framework (EF).</span><span class="sxs-lookup"><span data-stu-id="940cc-108">In this tutorial, we'll create the database by using the "Code First" approach to Entity Framework (EF).</span></span> <span data-ttu-id="940cc-109">Code First zapisu klas C#, które odpowiadają w tabelach bazy danych i EF utworzy bazę danych.</span><span class="sxs-lookup"><span data-stu-id="940cc-109">With Code First, you write C# classes that correspond to database tables, and EF creates the database.</span></span> <span data-ttu-id="940cc-110">(Aby uzyskać więcej informacji, zobacz [Entity Framework programowanie podejścia](https://msdn.microsoft.com/en-us/library/ms178359%28v=vs.110%29.aspx#dbfmfcf).)</span><span class="sxs-lookup"><span data-stu-id="940cc-110">(For more information, see [Entity Framework Development Approaches](https://msdn.microsoft.com/en-us/library/ms178359%28v=vs.110%29.aspx#dbfmfcf).)</span></span>

<span data-ttu-id="940cc-111">Firma Microsoft Rozpocznij od zdefiniowania naszych obiektów domeny jako POCOs (stary zwykły obiektów CLR).</span><span class="sxs-lookup"><span data-stu-id="940cc-111">We start by defining our domain objects as POCOs (plain-old CLR objects).</span></span> <span data-ttu-id="940cc-112">Utworzymy POCOs następujące:</span><span class="sxs-lookup"><span data-stu-id="940cc-112">We will create the following POCOs:</span></span>

- <span data-ttu-id="940cc-113">Autor</span><span class="sxs-lookup"><span data-stu-id="940cc-113">Author</span></span>
- <span data-ttu-id="940cc-114">Podręcznik</span><span class="sxs-lookup"><span data-stu-id="940cc-114">Book</span></span>

<span data-ttu-id="940cc-115">W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy kliknij folder modeli.</span><span class="sxs-lookup"><span data-stu-id="940cc-115">In Solution Explorer, right click the Models folder.</span></span> <span data-ttu-id="940cc-116">Wybierz **Dodaj**, a następnie wybierz pozycję **klasy**.</span><span class="sxs-lookup"><span data-stu-id="940cc-116">Select **Add**, then select **Class**.</span></span> <span data-ttu-id="940cc-117">Nazwa klasy `Author`.</span><span class="sxs-lookup"><span data-stu-id="940cc-117">Name the class `Author`.</span></span>

![](part-2/_static/image1.png)

<span data-ttu-id="940cc-118">Zamień wszystkie schematyczny kod w Author.cs poniższy kod.</span><span class="sxs-lookup"><span data-stu-id="940cc-118">Replace all of the boilerplate code in Author.cs with the following code.</span></span>

[!code-csharp[Main](part-2/samples/sample1.cs)]

<span data-ttu-id="940cc-119">Dodaj kolejną klasę o nazwie `Book`, z następującym kodem.</span><span class="sxs-lookup"><span data-stu-id="940cc-119">Add another class named `Book`, with the following code.</span></span>

[!code-csharp[Main](part-2/samples/sample2.cs)]

<span data-ttu-id="940cc-120">Entity Framework użyje tych modeli w celu tworzenia tabel bazy danych.</span><span class="sxs-lookup"><span data-stu-id="940cc-120">Entity Framework will use these models to create database tables.</span></span> <span data-ttu-id="940cc-121">Dla każdego modelu `Id` Właściwości staną się kolumna klucza podstawowego tabeli bazy danych.</span><span class="sxs-lookup"><span data-stu-id="940cc-121">For each model, the `Id` property will become the primary key column of the database table.</span></span>

<span data-ttu-id="940cc-122">W klasie książki `AuthorId` definiuje klucza obcego do `Author` tabeli.</span><span class="sxs-lookup"><span data-stu-id="940cc-122">In the Book class, the `AuthorId` defines a foreign key into the `Author` table.</span></span> <span data-ttu-id="940cc-123">(Dla uproszczenia I używam przy założeniu, że każdy książki ma jednego autora.) Klasa book zawiera także właściwości nawigacji do pokrewnych `Author`.</span><span class="sxs-lookup"><span data-stu-id="940cc-123">(For simplicity, I'm assuming that each book has a single author.) The book class also contains a navigation property to the related `Author`.</span></span> <span data-ttu-id="940cc-124">Właściwość nawigacji umożliwia dostęp do pokrewny `Author` w kodzie.</span><span class="sxs-lookup"><span data-stu-id="940cc-124">You can use the navigation property to access the related `Author` in code.</span></span> <span data-ttu-id="940cc-125">Coś więcej o właściwości nawigacji w część 4, [relacjami jednostek obsługi](part-4.md).</span><span class="sxs-lookup"><span data-stu-id="940cc-125">I say more about navigation properties in part 4, [Handling Entity Relations](part-4.md).</span></span>

## <a name="add-web-api-controllers"></a><span data-ttu-id="940cc-126">Dodaj kontrolery interfejsu API sieci Web</span><span class="sxs-lookup"><span data-stu-id="940cc-126">Add Web API Controllers</span></span>

<span data-ttu-id="940cc-127">W tej sekcji dodamy kontrolery interfejsu API sieci Web, które obsługują operacje CRUD (tworzenia, odczytu, aktualizacji i usuwania).</span><span class="sxs-lookup"><span data-stu-id="940cc-127">In this section, we'll add Web API controllers that support CRUD operations (create, read, update, and delete).</span></span> <span data-ttu-id="940cc-128">Kontrolery użyje Entity Framework do komunikowania się z warstwy bazy danych.</span><span class="sxs-lookup"><span data-stu-id="940cc-128">The controllers will use Entity Framework to communicate with the database layer.</span></span>

<span data-ttu-id="940cc-129">Po pierwsze można usunąć pliku Controllers/ValuesController.cs.</span><span class="sxs-lookup"><span data-stu-id="940cc-129">First, you can delete the file Controllers/ValuesController.cs.</span></span> <span data-ttu-id="940cc-130">Ten plik zawiera przykład kontroler interfejsu API sieci Web, ale nie będzie potrzebny w tym samouczku.</span><span class="sxs-lookup"><span data-stu-id="940cc-130">This file contains an example Web API controller, but you don't need it for this tutorial.</span></span>

![](part-2/_static/image2.png)

<span data-ttu-id="940cc-131">Następnie należy skompilować projekt.</span><span class="sxs-lookup"><span data-stu-id="940cc-131">Next, build the project.</span></span> <span data-ttu-id="940cc-132">Funkcja szkieletów interfejsu API sieci Web używa odbicia można znaleźć klasy modeli, dlatego potrzebuje skompilowanego zestawu.</span><span class="sxs-lookup"><span data-stu-id="940cc-132">The Web API scaffolding uses reflection to find the model classes, so it needs the compiled assembly.</span></span>

<span data-ttu-id="940cc-133">W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy folder kontrolery.</span><span class="sxs-lookup"><span data-stu-id="940cc-133">In Solution Explorer, right-click the Controllers folder.</span></span> <span data-ttu-id="940cc-134">Wybierz **Dodaj**, a następnie wybierz pozycję **kontrolera**.</span><span class="sxs-lookup"><span data-stu-id="940cc-134">Select **Add**, then select **Controller**.</span></span>

![](part-2/_static/image3.png)

<span data-ttu-id="940cc-135">W **Dodawanie szkieletu** okno dialogowe, wybierz opcję "składnika Web API 2 kontroler z akcjami używający narzędzia Entity Framework".</span><span class="sxs-lookup"><span data-stu-id="940cc-135">In the **Add Scaffold** dialog, select "Web API 2 Controller with actions, using Entity Framework".</span></span> <span data-ttu-id="940cc-136">Kliknij przycisk **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="940cc-136">Click **Add**.</span></span>

![](part-2/_static/image4.png)

<span data-ttu-id="940cc-137">W **Dodaj kontroler** okna dialogowego, wykonaj następujące czynności:</span><span class="sxs-lookup"><span data-stu-id="940cc-137">In the **Add Controller** dialog, do the following:</span></span>

1. <span data-ttu-id="940cc-138">W **klasa modelu** listy rozwijanej wybierz `Author` klasy.</span><span class="sxs-lookup"><span data-stu-id="940cc-138">In the **Model class** dropdown, select the `Author` class.</span></span> <span data-ttu-id="940cc-139">(Jeśli nie widzisz wyświetlane na liście rozwijanej, upewnij się, że zostały utworzone w projekcie.)</span><span class="sxs-lookup"><span data-stu-id="940cc-139">(If you don't see it listed in the dropdown, make sure that you built the project.)</span></span>
2. <span data-ttu-id="940cc-140">Sprawdź "Użyj asynchronicznych akcji kontrolera".</span><span class="sxs-lookup"><span data-stu-id="940cc-140">Check "Use async controller actions".</span></span>
3. <span data-ttu-id="940cc-141">Pozostaw nazwę kontrolera jako &quot;AuthorsController&quot;.</span><span class="sxs-lookup"><span data-stu-id="940cc-141">Leave the controller name as &quot;AuthorsController&quot;.</span></span>
4. <span data-ttu-id="940cc-142">Kliknij przycisk plus (+) obok przycisku **klasa kontekstu danych**.</span><span class="sxs-lookup"><span data-stu-id="940cc-142">Click plus (+) button next to **Data Context Class**.</span></span>

![](part-2/_static/image5.png)

<span data-ttu-id="940cc-143">W **nowy kontekst danych** okna dialogowego, pozostaw nazwę domyślną, a następnie kliknij przycisk **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="940cc-143">In the **New Data Context** dialog, leave the default name and click **Add**.</span></span>

![](part-2/_static/image6.png)

<span data-ttu-id="940cc-144">Kliknij przycisk **Dodaj** do ukończenia **Dodaj kontroler** okna dialogowego.</span><span class="sxs-lookup"><span data-stu-id="940cc-144">Click **Add** to complete the **Add Controller** dialog.</span></span> <span data-ttu-id="940cc-145">Okno dialogowe dodaje dwie klasy do projektu:</span><span class="sxs-lookup"><span data-stu-id="940cc-145">The dialog adds two classes to your project:</span></span>

- <span data-ttu-id="940cc-146">`AuthorsController`Definiuje kontrolera interfejsu API sieci Web.</span><span class="sxs-lookup"><span data-stu-id="940cc-146">`AuthorsController` defines a Web API controller.</span></span> <span data-ttu-id="940cc-147">Kontroler implementuje interfejs API REST, używanego przez klientów w celu wykonywania operacji CRUD na liście autorów.</span><span class="sxs-lookup"><span data-stu-id="940cc-147">The controller implements the REST API that clients use to perform CRUD operations on the list of authors.</span></span>
- <span data-ttu-id="940cc-148">`BookServiceContext`zarządza obiekty obiektów w czasie wykonywania, obejmujące wypełnianie obiekty z danymi z bazy danych, śledzenie zmian i trwałych danych do bazy danych.</span><span class="sxs-lookup"><span data-stu-id="940cc-148">`BookServiceContext` manages entity objects during run time, which includes populating objects with data from a database, change tracking, and persisting data to the database.</span></span> <span data-ttu-id="940cc-149">Dziedziczy on z `DbContext`.</span><span class="sxs-lookup"><span data-stu-id="940cc-149">It inherits from `DbContext`.</span></span>

![](part-2/_static/image7.png)

<span data-ttu-id="940cc-150">W tym momencie ponownie skompilować projekt.</span><span class="sxs-lookup"><span data-stu-id="940cc-150">At this point, build the project again.</span></span> <span data-ttu-id="940cc-151">Teraz przejdź przez te same kroki, aby dodać Kontroler interfejsu API dla `Book` jednostek.</span><span class="sxs-lookup"><span data-stu-id="940cc-151">Now go through the same steps to add an API controller for `Book` entities.</span></span> <span data-ttu-id="940cc-152">Teraz, wybierz opcję `Book` dla klasy modelu i wybierz istniejącą `BookServiceContext` klasy dla klasy kontekstu danych.</span><span class="sxs-lookup"><span data-stu-id="940cc-152">This time, select `Book` for the model class, and select the existing `BookServiceContext` class for the data context class.</span></span> <span data-ttu-id="940cc-153">(Nie tworzy nowy kontekst danych). Kliknij przycisk **Dodaj** Aby dodać kontroler.</span><span class="sxs-lookup"><span data-stu-id="940cc-153">(Don't create a new data context.) Click **Add** to add the controller.</span></span>

![](part-2/_static/image8.png)

>[!div class="step-by-step"]
<span data-ttu-id="940cc-154">[Poprzednie](part-1.md)
[dalej](part-3.md)</span><span class="sxs-lookup"><span data-stu-id="940cc-154">[Previous](part-1.md)
[Next](part-3.md)</span></span>
