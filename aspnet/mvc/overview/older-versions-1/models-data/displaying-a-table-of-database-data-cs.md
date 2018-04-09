---
uid: mvc/overview/older-versions-1/models-data/displaying-a-table-of-database-data-cs
title: Wyświetlanie tabeli bazy danych (C#) | Dokumentacja firmy Microsoft
author: microsoft
description: W tym samouczku pokazują I wyświetlanie zestaw rekordów bazy danych na dwa sposoby. Wyświetlić dwóch metod formatowania zestaw rekordów bazy danych w formacie HTML ta...
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/07/2008
ms.topic: article
ms.assetid: d6e758b6-6571-484d-a132-34ee6c47747a
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/models-data/displaying-a-table-of-database-data-cs
msc.type: authoredcontent
ms.openlocfilehash: 1d5dc9dd4a82e4577c6c1a3b124d45fef0b0f67c
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
---
<a name="displaying-a-table-of-database-data-c"></a><span data-ttu-id="a7ce6-104">Wyświetlanie tabeli bazy danych (C#)</span><span class="sxs-lookup"><span data-stu-id="a7ce6-104">Displaying a Table of Database Data (C#)</span></span>
====================
<span data-ttu-id="a7ce6-105">przez [firmy Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="a7ce6-105">by [Microsoft](https://github.com/microsoft)</span></span>

[<span data-ttu-id="a7ce6-106">Pobierz plik PDF</span><span class="sxs-lookup"><span data-stu-id="a7ce6-106">Download PDF</span></span>](http://download.microsoft.com/download/1/1/f/11f721aa-d749-4ed7-bb89-a681b68894e6/ASPNET_MVC_Tutorial_11_CS.pdf)

> <span data-ttu-id="a7ce6-107">W tym samouczku pokazują I wyświetlanie zestaw rekordów bazy danych na dwa sposoby.</span><span class="sxs-lookup"><span data-stu-id="a7ce6-107">In this tutorial, I demonstrate two methods of displaying a set of database records.</span></span> <span data-ttu-id="a7ce6-108">Wyświetlić dwóch metod formatowania zestaw rekordów bazy danych w tabeli HTML.</span><span class="sxs-lookup"><span data-stu-id="a7ce6-108">I show two methods of formatting a set of database records in an HTML table.</span></span> <span data-ttu-id="a7ce6-109">Najpierw należy wyświetlić sposób można sformatować rekordów bazy danych bezpośrednio w widoku.</span><span class="sxs-lookup"><span data-stu-id="a7ce6-109">First, I show how you can format the database records directly within a view.</span></span> <span data-ttu-id="a7ce6-110">Następnie I pokazują, jak można korzystać z częściowe podczas formatowania rekordów bazy danych.</span><span class="sxs-lookup"><span data-stu-id="a7ce6-110">Next, I demonstrate how you can take advantage of partials when formatting database records.</span></span>


<span data-ttu-id="a7ce6-111">Celem tego samouczka jest wyjaśnienie, jak można wyświetlić tabeli HTML danych bazy danych w aplikacji platformy ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="a7ce6-111">The goal of this tutorial is to explain how you can display an HTML table of database data in an ASP.NET MVC application.</span></span> <span data-ttu-id="a7ce6-112">Najpierw zostanie przedstawiony sposób korzystania z narzędzi szkieletów uwzględnione w programie Visual Studio do generowania widoku, który automatycznie wyświetla zestaw rekordów.</span><span class="sxs-lookup"><span data-stu-id="a7ce6-112">First, you learn how to use the scaffolding tools included in Visual Studio to generate a view that displays a set of records automatically.</span></span> <span data-ttu-id="a7ce6-113">Następnie zostanie przedstawiony sposób użycia częściowym jako szablon podczas formatowania rekordów bazy danych.</span><span class="sxs-lookup"><span data-stu-id="a7ce6-113">Next, you learn how to use a partial as a template when formatting database records.</span></span>

## <a name="create-the-model-classes"></a><span data-ttu-id="a7ce6-114">Tworzenie klasy modelu</span><span class="sxs-lookup"><span data-stu-id="a7ce6-114">Create the Model Classes</span></span>

<span data-ttu-id="a7ce6-115">Zamierzamy, aby wyświetlić zestaw rekordów z tabeli bazy danych filmów.</span><span class="sxs-lookup"><span data-stu-id="a7ce6-115">We are going to display the set of records from the Movies database table.</span></span> <span data-ttu-id="a7ce6-116">W tabeli bazy danych filmy zawiera następujące kolumny:</span><span class="sxs-lookup"><span data-stu-id="a7ce6-116">The Movies database table contains the following columns:</span></span>

<a id="0.3_table01"></a>


| <span data-ttu-id="a7ce6-117">**Nazwa kolumny**</span><span class="sxs-lookup"><span data-stu-id="a7ce6-117">**Column Name**</span></span> | <span data-ttu-id="a7ce6-118">**Typ danych**</span><span class="sxs-lookup"><span data-stu-id="a7ce6-118">**Data Type**</span></span> | <span data-ttu-id="a7ce6-119">**Dopuszcza wartości null**</span><span class="sxs-lookup"><span data-stu-id="a7ce6-119">**Allow Nulls**</span></span> |
| --- | --- | --- |
| <span data-ttu-id="a7ce6-120">Id</span><span class="sxs-lookup"><span data-stu-id="a7ce6-120">Id</span></span> | <span data-ttu-id="a7ce6-121">int</span><span class="sxs-lookup"><span data-stu-id="a7ce6-121">Int</span></span> | <span data-ttu-id="a7ce6-122">False</span><span class="sxs-lookup"><span data-stu-id="a7ce6-122">False</span></span> |
| <span data-ttu-id="a7ce6-123">Tytuł</span><span class="sxs-lookup"><span data-stu-id="a7ce6-123">Title</span></span> | <span data-ttu-id="a7ce6-124">Nvarchar(200)</span><span class="sxs-lookup"><span data-stu-id="a7ce6-124">Nvarchar(200)</span></span> | <span data-ttu-id="a7ce6-125">False</span><span class="sxs-lookup"><span data-stu-id="a7ce6-125">False</span></span> |
| <span data-ttu-id="a7ce6-126">Dyrektor</span><span class="sxs-lookup"><span data-stu-id="a7ce6-126">Director</span></span> | <span data-ttu-id="a7ce6-127">NVarchar(50)</span><span class="sxs-lookup"><span data-stu-id="a7ce6-127">NVarchar(50)</span></span> | <span data-ttu-id="a7ce6-128">False</span><span class="sxs-lookup"><span data-stu-id="a7ce6-128">False</span></span> |
| <span data-ttu-id="a7ce6-129">DateReleased</span><span class="sxs-lookup"><span data-stu-id="a7ce6-129">DateReleased</span></span> | <span data-ttu-id="a7ce6-130">DataGodzina</span><span class="sxs-lookup"><span data-stu-id="a7ce6-130">DateTime</span></span> | <span data-ttu-id="a7ce6-131">False</span><span class="sxs-lookup"><span data-stu-id="a7ce6-131">False</span></span> |


<span data-ttu-id="a7ce6-132">Aby można było przedstawić tabeli filmów w naszej aplikacji ASP.NET MVC, należy utworzyć klasę modelu.</span><span class="sxs-lookup"><span data-stu-id="a7ce6-132">In order to represent the Movies table in our ASP.NET MVC application, we need to create a model class.</span></span> <span data-ttu-id="a7ce6-133">W tym samouczku używamy Microsoft Entity Framework do tworzenia klas naszych modelu.</span><span class="sxs-lookup"><span data-stu-id="a7ce6-133">In this tutorial, we use the Microsoft Entity Framework to create our model classes.</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="a7ce6-134">W tym samouczku używamy Microsoft Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="a7ce6-134">In this tutorial, we use the Microsoft Entity Framework.</span></span> <span data-ttu-id="a7ce6-135">Jest jednak wziąć pod uwagę, że można użyć szereg różnych technologii wchodzić w interakcje z bazy danych z aplikacji platformy ASP.NET MVC, w tym składniku LINQ to SQL, NHibernate lub ADO.NET.</span><span class="sxs-lookup"><span data-stu-id="a7ce6-135">However, it is important to understand that you can use a variety of different technologies to interact with a database from an ASP.NET MVC application including LINQ to SQL, NHibernate, or ADO.NET.</span></span>


<span data-ttu-id="a7ce6-136">Wykonaj następujące kroki, aby uruchomić Kreatora modelu danych jednostki:</span><span class="sxs-lookup"><span data-stu-id="a7ce6-136">Follow these steps to launch the Entity Data Model Wizard:</span></span>

1. <span data-ttu-id="a7ce6-137">Kliknij prawym przyciskiem myszy folder modeli w oknie Eksploratora rozwiązań i wybierz opcję menu **Dodaj, nowy element**.</span><span class="sxs-lookup"><span data-stu-id="a7ce6-137">Right-click the Models folder in the Solution Explorer window and the select the menu option **Add, New Item**.</span></span>
2. <span data-ttu-id="a7ce6-138">Wybierz **danych** kategorii i wybierz **modelu danych jednostki ADO.NET** szablonu.</span><span class="sxs-lookup"><span data-stu-id="a7ce6-138">Select the **Data** category and select the **ADO.NET Entity Data Model** template.</span></span>
3. <span data-ttu-id="a7ce6-139">Nadaj nazwę modelu danych *MoviesDBModel.edmx* i kliknij przycisk **Dodaj** przycisku.</span><span class="sxs-lookup"><span data-stu-id="a7ce6-139">Give your data model the name *MoviesDBModel.edmx* and click the **Add** button.</span></span>

<span data-ttu-id="a7ce6-140">Po kliknięciu przycisku Dodaj zostanie wyświetlony Kreator modelu danych jednostki (zobacz rysunek 1).</span><span class="sxs-lookup"><span data-stu-id="a7ce6-140">After you click the Add button, the Entity Data Model Wizard appears (see Figure 1).</span></span> <span data-ttu-id="a7ce6-141">Wykonaj następujące kroki, aby zakończyć działanie kreatora:</span><span class="sxs-lookup"><span data-stu-id="a7ce6-141">Follow these steps to complete the wizard:</span></span>

1. <span data-ttu-id="a7ce6-142">W **wybierz zawartość modelu** krok, wybierz opcję **generowania z bazy danych** opcji.</span><span class="sxs-lookup"><span data-stu-id="a7ce6-142">In the **Choose Model Contents** step, select the **Generate from database** option.</span></span>
2. <span data-ttu-id="a7ce6-143">W **wybierz połączenie danych** krok, użyj *MoviesDB.mdf* połączenie danych i nazwy *MoviesDBEntities* dla ustawień połączenia.</span><span class="sxs-lookup"><span data-stu-id="a7ce6-143">In the **Choose Your Data Connection** step, use the *MoviesDB.mdf* data connection and the name *MoviesDBEntities* for the connection settings.</span></span> <span data-ttu-id="a7ce6-144">Kliknij przycisk **dalej** przycisku.</span><span class="sxs-lookup"><span data-stu-id="a7ce6-144">Click the **Next** button.</span></span>
3. <span data-ttu-id="a7ce6-145">W **wybierz obiekty bazy danych użytkownika** krok, rozwiń węzeł tabel, zaznacz tabelę filmów.</span><span class="sxs-lookup"><span data-stu-id="a7ce6-145">In the **Choose Your Database Objects** step, expand the Tables node, select the Movies table.</span></span> <span data-ttu-id="a7ce6-146">Wprowadź przestrzeń nazw *modele* i kliknij przycisk **Zakończ** przycisku.</span><span class="sxs-lookup"><span data-stu-id="a7ce6-146">Enter the namespace *Models* and click the **Finish** button.</span></span>


<span data-ttu-id="a7ce6-147">[![Tworzenie LINQ w klasach SQL](displaying-a-table-of-database-data-cs/_static/image1.jpg)](displaying-a-table-of-database-data-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="a7ce6-147">[![Creating LINQ to SQL classes](displaying-a-table-of-database-data-cs/_static/image1.jpg)](displaying-a-table-of-database-data-cs/_static/image1.png)</span></span>

<span data-ttu-id="a7ce6-148">**Rysunek 01**: Tworzenie klasy LINQ do SQL ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](displaying-a-table-of-database-data-cs/_static/image2.png))</span><span class="sxs-lookup"><span data-stu-id="a7ce6-148">**Figure 01**: Creating LINQ to SQL classes ([Click to view full-size image](displaying-a-table-of-database-data-cs/_static/image2.png))</span></span>


<span data-ttu-id="a7ce6-149">Po zakończeniu pracy Kreatora modelu danych jednostki, zostanie otwarty projektant modelu danych jednostki.</span><span class="sxs-lookup"><span data-stu-id="a7ce6-149">After you complete the Entity Data Model Wizard, the Entity Data Model Designer opens.</span></span> <span data-ttu-id="a7ce6-150">Projektant powinien być wyświetlany jednostki filmów (patrz rysunek 2).</span><span class="sxs-lookup"><span data-stu-id="a7ce6-150">The Designer should display the Movies entity (see Figure 2).</span></span>


<span data-ttu-id="a7ce6-151">[![Projektant modelu danych jednostki](displaying-a-table-of-database-data-cs/_static/image2.jpg)](displaying-a-table-of-database-data-cs/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="a7ce6-151">[![The Entity Data Model Designer](displaying-a-table-of-database-data-cs/_static/image2.jpg)](displaying-a-table-of-database-data-cs/_static/image3.png)</span></span>

<span data-ttu-id="a7ce6-152">**Rysunek 02**: Projektant modelu danych jednostki ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](displaying-a-table-of-database-data-cs/_static/image4.png))</span><span class="sxs-lookup"><span data-stu-id="a7ce6-152">**Figure 02**: The Entity Data Model Designer ([Click to view full-size image](displaying-a-table-of-database-data-cs/_static/image4.png))</span></span>


<span data-ttu-id="a7ce6-153">Musimy upewnić jednej zmiany przed kontynuowaniem.</span><span class="sxs-lookup"><span data-stu-id="a7ce6-153">We need to make one change before we continue.</span></span> <span data-ttu-id="a7ce6-154">Kreator danych jednostek generuje klasę modelu o nazwie *filmy* reprezentujący filmy tabeli bazy danych.</span><span class="sxs-lookup"><span data-stu-id="a7ce6-154">The Entity Data Wizard generates a model class named *Movies* that represents the Movies database table.</span></span> <span data-ttu-id="a7ce6-155">Ponieważ użyjemy klasy filmów do reprezentowania określonego filmu, należy zmodyfikować nazwę klasy, która ma być *film* zamiast *filmy* (pojedynczej zamiast mnogiej).</span><span class="sxs-lookup"><span data-stu-id="a7ce6-155">Because we'll use the Movies class to represent a particular movie, we need to modify the name of the class to be *Movie* instead of *Movies* (singular rather than plural).</span></span>

<span data-ttu-id="a7ce6-156">Kliknij dwukrotnie nazwę klasy na powierzchni projektanta i Zmień nazwę klasy z filmów filmu.</span><span class="sxs-lookup"><span data-stu-id="a7ce6-156">Double-click the name of the class on the designer surface and change the name of the class from Movies to Movie.</span></span> <span data-ttu-id="a7ce6-157">Po wprowadzeniu tej zmiany, kliknij przycisk **zapisać** przycisk (ikona dyskietki) do generowania klasy filmu.</span><span class="sxs-lookup"><span data-stu-id="a7ce6-157">After making this change, click the **Save** button (the icon of the floppy disk) to generate the Movie class.</span></span>

## <a name="create-the-movies-controller"></a><span data-ttu-id="a7ce6-158">Tworzenie kontrolera filmy</span><span class="sxs-lookup"><span data-stu-id="a7ce6-158">Create the Movies Controller</span></span>

<span data-ttu-id="a7ce6-159">Teraz, gdy mamy sposób reprezentacji naszych danych bazy danych można utworzyć kontroler, który zwraca kolekcji filmów.</span><span class="sxs-lookup"><span data-stu-id="a7ce6-159">Now that we have a way to represent our database records, we can create a controller that returns the collection of movies.</span></span> <span data-ttu-id="a7ce6-160">W oknie programu Visual Studio Solution Explorer, kliknij prawym przyciskiem myszy folder kontrolery, a następnie wybierz opcję menu **Dodaj, kontrolera** (patrz rysunek 3).</span><span class="sxs-lookup"><span data-stu-id="a7ce6-160">Within the Visual Studio Solution Explorer window, right-click the Controllers folder and select the menu option **Add, Controller** (see Figure 3).</span></span>


<span data-ttu-id="a7ce6-161">[![Dodawanie Menu kontrolera](displaying-a-table-of-database-data-cs/_static/image3.jpg)](displaying-a-table-of-database-data-cs/_static/image5.png)</span><span class="sxs-lookup"><span data-stu-id="a7ce6-161">[![The Add Controller Menu](displaying-a-table-of-database-data-cs/_static/image3.jpg)](displaying-a-table-of-database-data-cs/_static/image5.png)</span></span>

<span data-ttu-id="a7ce6-162">**Rysunek 03**: Dodaj Menu kontrolera ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](displaying-a-table-of-database-data-cs/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="a7ce6-162">**Figure 03**: The Add Controller Menu ([Click to view full-size image](displaying-a-table-of-database-data-cs/_static/image6.png))</span></span>


<span data-ttu-id="a7ce6-163">Gdy **Dodaj kontroler** zostanie wyświetlone okno dialogowe, wprowadź nazwę kontrolera MovieController (patrz rysunek 4).</span><span class="sxs-lookup"><span data-stu-id="a7ce6-163">When the **Add Controller** dialog appears, enter the controller name MovieController (see Figure 4).</span></span> <span data-ttu-id="a7ce6-164">Kliknij przycisk **Dodaj** przycisk, aby dodać nowy kontroler.</span><span class="sxs-lookup"><span data-stu-id="a7ce6-164">Click the **Add** button to add the new controller.</span></span>


<span data-ttu-id="a7ce6-165">[![Okno dialogowe Dodawanie kontrolera](displaying-a-table-of-database-data-cs/_static/image4.jpg)](displaying-a-table-of-database-data-cs/_static/image7.png)</span><span class="sxs-lookup"><span data-stu-id="a7ce6-165">[![The Add Controller dialog](displaying-a-table-of-database-data-cs/_static/image4.jpg)](displaying-a-table-of-database-data-cs/_static/image7.png)</span></span>

<span data-ttu-id="a7ce6-166">**Rysunek 04**: okno dialogowe Dodaj kontroler ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](displaying-a-table-of-database-data-cs/_static/image8.png))</span><span class="sxs-lookup"><span data-stu-id="a7ce6-166">**Figure 04**: The Add Controller dialog([Click to view full-size image](displaying-a-table-of-database-data-cs/_static/image8.png))</span></span>


<span data-ttu-id="a7ce6-167">Należy zmodyfikować akcję indeks() udostępnianych przez kontrolera film, tak aby zwracało zestaw rekordów bazy danych.</span><span class="sxs-lookup"><span data-stu-id="a7ce6-167">We need to modify the Index() action exposed by the Movie controller so that it returns the set of database records.</span></span> <span data-ttu-id="a7ce6-168">Zmodyfikuj kontrolera wyglądającą jak kontrolera 1 wyświetlania.</span><span class="sxs-lookup"><span data-stu-id="a7ce6-168">Modify the controller so that it looks like the controller in Listing 1.</span></span>

<span data-ttu-id="a7ce6-169">**Wyświetlanie listy 1 – Controllers\MovieController.cs**</span><span class="sxs-lookup"><span data-stu-id="a7ce6-169">**Listing 1 – Controllers\MovieController.cs**</span></span>

[!code-csharp[Main](displaying-a-table-of-database-data-cs/samples/sample1.cs)]

<span data-ttu-id="a7ce6-170">Wyświetlanie listy 1 klasa MoviesDBEntities jest używana do reprezentowania MoviesDB bazy danych.</span><span class="sxs-lookup"><span data-stu-id="a7ce6-170">In Listing 1, the MoviesDBEntities class is used to represent the MoviesDB database.</span></span> <span data-ttu-id="a7ce6-171">Aby korzystać z tej klasy, należy zaimportować przestrzeni nazw MvcApplication1.Models następująco:</span><span class="sxs-lookup"><span data-stu-id="a7ce6-171">To use this class, you need to import the MvcApplication1.Models namespace like this:</span></span>

<span data-ttu-id="a7ce6-172">using MvcApplication1.Models;</span><span class="sxs-lookup"><span data-stu-id="a7ce6-172">using MvcApplication1.Models;</span></span>

<span data-ttu-id="a7ce6-173">Wyrażenie *jednostek. MovieSet.ToList()* zwraca zestaw wszystkich filmów filmy tabeli bazy danych.</span><span class="sxs-lookup"><span data-stu-id="a7ce6-173">The expression *entities.MovieSet.ToList()* returns the set of all movies from the Movies database table.</span></span>

## <a name="create-the-view"></a><span data-ttu-id="a7ce6-174">Tworzenie widoku</span><span class="sxs-lookup"><span data-stu-id="a7ce6-174">Create the View</span></span>

<span data-ttu-id="a7ce6-175">Najprostszym sposobem wyświetlenia zestaw rekordów bazy danych w tabeli HTML jest przeprowadzać szkieletów dostarczane przez program Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="a7ce6-175">The easiest way to display a set of database records in an HTML table is to take advantage of the scaffolding provided by Visual Studio.</span></span>

<span data-ttu-id="a7ce6-176">Tworzenie aplikacji przez wybranie opcji menu **kompilacji Kompiluj rozwiązanie**.</span><span class="sxs-lookup"><span data-stu-id="a7ce6-176">Build your application by selecting the menu option **Build, Build Solution**.</span></span> <span data-ttu-id="a7ce6-177">Należy skompilować aplikację przed otwarciem **Dodaj widok** okna dialogowego lub klas danych nie będzie dłużej wyświetlane w oknie dialogowym.</span><span class="sxs-lookup"><span data-stu-id="a7ce6-177">You must build your application before opening the **Add View** dialog or your data classes won't appear in the dialog.</span></span>

<span data-ttu-id="a7ce6-178">Kliknij prawym przyciskiem myszy działanie indeks() i wybierz opcję menu **Dodaj widok** (patrz rysunek 5).</span><span class="sxs-lookup"><span data-stu-id="a7ce6-178">Right-click the Index() action and select the menu option **Add View** (see Figure 5).</span></span>


<span data-ttu-id="a7ce6-179">[![Dodawanie widoku](displaying-a-table-of-database-data-cs/_static/image5.jpg)](displaying-a-table-of-database-data-cs/_static/image9.png)</span><span class="sxs-lookup"><span data-stu-id="a7ce6-179">[![Adding a view](displaying-a-table-of-database-data-cs/_static/image5.jpg)](displaying-a-table-of-database-data-cs/_static/image9.png)</span></span>

<span data-ttu-id="a7ce6-180">**Rysunek 05**: Dodawanie widoku ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](displaying-a-table-of-database-data-cs/_static/image10.png))</span><span class="sxs-lookup"><span data-stu-id="a7ce6-180">**Figure 05**: Adding a view ([Click to view full-size image](displaying-a-table-of-database-data-cs/_static/image10.png))</span></span>


<span data-ttu-id="a7ce6-181">W **Dodaj widok** okno dialogowe, zaznacz pole wyboru z etykietą **utworzyć widok jednoznacznie**.</span><span class="sxs-lookup"><span data-stu-id="a7ce6-181">In the **Add View** dialog, check the checkbox labeled **Create a strongly-typed view**.</span></span> <span data-ttu-id="a7ce6-182">Wybierz klasę filmu jako **wyświetlić klasy danych**.</span><span class="sxs-lookup"><span data-stu-id="a7ce6-182">Select the Movie class as the **view data class**.</span></span> <span data-ttu-id="a7ce6-183">Wybierz *listy* jako **wyświetlać zawartość** (patrz rysunek 6).</span><span class="sxs-lookup"><span data-stu-id="a7ce6-183">Select *List* as the **view content** (see Figure 6).</span></span> <span data-ttu-id="a7ce6-184">Wybranie opcji wygeneruje silnie typizowanym widoku, który wyświetla listę filmów.</span><span class="sxs-lookup"><span data-stu-id="a7ce6-184">Selecting these options will generate a strongly-typed view that displays a list of movies.</span></span>


<span data-ttu-id="a7ce6-185">[![Okno dialogowe dodawania widoku](displaying-a-table-of-database-data-cs/_static/image6.jpg)](displaying-a-table-of-database-data-cs/_static/image11.png)</span><span class="sxs-lookup"><span data-stu-id="a7ce6-185">[![The Add View dialog](displaying-a-table-of-database-data-cs/_static/image6.jpg)](displaying-a-table-of-database-data-cs/_static/image11.png)</span></span>

<span data-ttu-id="a7ce6-186">**Rysunek 06**: okno dialogowe dodawania widoku ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](displaying-a-table-of-database-data-cs/_static/image12.png))</span><span class="sxs-lookup"><span data-stu-id="a7ce6-186">**Figure 06**: The Add View dialog([Click to view full-size image](displaying-a-table-of-database-data-cs/_static/image12.png))</span></span>


<span data-ttu-id="a7ce6-187">Po kliknięciu **Dodaj** przycisk, Widok wyświetlania 2 jest generowany automatycznie.</span><span class="sxs-lookup"><span data-stu-id="a7ce6-187">After you click the **Add** button, the view in Listing 2 is generated automatically.</span></span> <span data-ttu-id="a7ce6-188">Ten widok zawiera kod wymagany do iterowania po kolekcji filmów i wyświetlania każdej z właściwości filmu.</span><span class="sxs-lookup"><span data-stu-id="a7ce6-188">This view contains the code required to iterate through the collection of movies and display each of the properties of a movie.</span></span>

<span data-ttu-id="a7ce6-189">**Wyświetlanie listy 2 — Views\Movie\Index.aspx**</span><span class="sxs-lookup"><span data-stu-id="a7ce6-189">**Listing 2 – Views\Movie\Index.aspx**</span></span>

[!code-aspx[Main](displaying-a-table-of-database-data-cs/samples/sample2.aspx)]

<span data-ttu-id="a7ce6-190">Można uruchomić aplikację, wybierając opcję menu **debugowania i Rozpocznij debugowanie** (lub naciśnięcie klawisza F5).</span><span class="sxs-lookup"><span data-stu-id="a7ce6-190">You can run the application by selecting the menu option **Debug, Start Debugging** (or hitting the F5 key).</span></span> <span data-ttu-id="a7ce6-191">Uruchamianie aplikacji uruchamia program Internet Explorer.</span><span class="sxs-lookup"><span data-stu-id="a7ce6-191">Running the application launches Internet Explorer.</span></span> <span data-ttu-id="a7ce6-192">Jeśli przejdziesz do adresu URL /Movie zostanie wyświetlone strony na rysunku 7.</span><span class="sxs-lookup"><span data-stu-id="a7ce6-192">If you navigate to the /Movie URL then you'll see the page in Figure 7.</span></span>


<span data-ttu-id="a7ce6-193">[![Spis filmy](displaying-a-table-of-database-data-cs/_static/image7.jpg)](displaying-a-table-of-database-data-cs/_static/image13.png)</span><span class="sxs-lookup"><span data-stu-id="a7ce6-193">[![A table of movies](displaying-a-table-of-database-data-cs/_static/image7.jpg)](displaying-a-table-of-database-data-cs/_static/image13.png)</span></span>

<span data-ttu-id="a7ce6-194">**Rysunek 07**: Tabela filmów ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](displaying-a-table-of-database-data-cs/_static/image14.png))</span><span class="sxs-lookup"><span data-stu-id="a7ce6-194">**Figure 07**: A table of movies([Click to view full-size image](displaying-a-table-of-database-data-cs/_static/image14.png))</span></span>


<span data-ttu-id="a7ce6-195">Jeśli nie lubisz niczego o wygląd siatki rekordów bazy danych na rysunku 7 można po prostu zmodyfikuj widok indeksu.</span><span class="sxs-lookup"><span data-stu-id="a7ce6-195">If you don't like anything about the appearance of the grid of database records in Figure 7 then you can simply modify the Index view.</span></span> <span data-ttu-id="a7ce6-196">Na przykład można zmienić *DateReleased* nagłówka do *Data wydania* przez zmodyfikowanie widoku indeksu.</span><span class="sxs-lookup"><span data-stu-id="a7ce6-196">For example, you can change the *DateReleased* header to *Date Released* by modifying the Index view.</span></span>

## <a name="create-a-template-with-a-partial"></a><span data-ttu-id="a7ce6-197">Utwórz szablon z częściowym</span><span class="sxs-lookup"><span data-stu-id="a7ce6-197">Create a Template with a Partial</span></span>

<span data-ttu-id="a7ce6-198">Jeśli widok pobiera zbyt skomplikowane, jest dobrym rozwiązaniem jest rozpocząć dzielenie widoku na częściowe.</span><span class="sxs-lookup"><span data-stu-id="a7ce6-198">When a view gets too complicated, it is a good idea to start breaking the view into partials.</span></span> <span data-ttu-id="a7ce6-199">Przy użyciu częściowe umożliwia łatwiejsze do zrozumienia i obsługa widoków.</span><span class="sxs-lookup"><span data-stu-id="a7ce6-199">Using partials makes your views easier to understand and maintain.</span></span> <span data-ttu-id="a7ce6-200">Utworzymy częściowym który możemy użyć jako szablon do formatowania każdego filmu rekordów bazy danych.</span><span class="sxs-lookup"><span data-stu-id="a7ce6-200">We'll create a partial that we can use as a template to format each of the movie database records.</span></span>

<span data-ttu-id="a7ce6-201">Wykonaj następujące kroki, aby utworzyć częściowego:</span><span class="sxs-lookup"><span data-stu-id="a7ce6-201">Follow these steps to create the partial:</span></span>

1. <span data-ttu-id="a7ce6-202">Kliknij prawym przyciskiem myszy Views\Movie folder i wybierz opcję menu **Dodaj widok**.</span><span class="sxs-lookup"><span data-stu-id="a7ce6-202">Right-click the Views\Movie folder and select the menu option **Add View**.</span></span>
2. <span data-ttu-id="a7ce6-203">Zaznacz pole wyboru z etykietą *tworzenia widoku częściowego (.ascx)*.</span><span class="sxs-lookup"><span data-stu-id="a7ce6-203">Check the checkbox labeled *Create a partial view (.ascx)*.</span></span>
3. <span data-ttu-id="a7ce6-204">Nazwa częściowego *MovieTemplate*.</span><span class="sxs-lookup"><span data-stu-id="a7ce6-204">Name the partial *MovieTemplate*.</span></span>
4. <span data-ttu-id="a7ce6-205">Zaznacz pole wyboru z etykietą **utworzyć widok jednoznacznie**.</span><span class="sxs-lookup"><span data-stu-id="a7ce6-205">Check the checkbox labeled **Create a strongly-typed view**.</span></span>
5. <span data-ttu-id="a7ce6-206">Wybierz film jako *wyświetlić klasy danych*.</span><span class="sxs-lookup"><span data-stu-id="a7ce6-206">Select Movie as the *view data class*.</span></span>
6. <span data-ttu-id="a7ce6-207">Wybierz opcję Pusta jako *wyświetlać zawartość*.</span><span class="sxs-lookup"><span data-stu-id="a7ce6-207">Select Empty as the *view content*.</span></span>
7. <span data-ttu-id="a7ce6-208">Kliknij przycisk **Dodaj** przycisk, aby dodać częściowego do projektu.</span><span class="sxs-lookup"><span data-stu-id="a7ce6-208">Click the **Add** button to add the partial to your project.</span></span>

<span data-ttu-id="a7ce6-209">Po wykonaniu tych kroków należy zmodyfikować MovieTemplate częściowego, aby wyglądały jak lista 3.</span><span class="sxs-lookup"><span data-stu-id="a7ce6-209">After you complete these steps, modify the MovieTemplate partial to look like Listing 3.</span></span>

<span data-ttu-id="a7ce6-210">**Wyświetlanie listy 3 — Views\Movie\MovieTemplate.ascx**</span><span class="sxs-lookup"><span data-stu-id="a7ce6-210">**Listing 3 – Views\Movie\MovieTemplate.ascx**</span></span>

[!code-aspx[Main](displaying-a-table-of-database-data-cs/samples/sample3.aspx)]

<span data-ttu-id="a7ce6-211">Partial wyświetlania 3 zawiera szablon w pojedynczym wierszu rekordów.</span><span class="sxs-lookup"><span data-stu-id="a7ce6-211">The partial in Listing 3 contains a template for a single row of records.</span></span>

<span data-ttu-id="a7ce6-212">Zmodyfikowany widok indeksu listę 4 korzysta z częściowa MovieTemplate.</span><span class="sxs-lookup"><span data-stu-id="a7ce6-212">The modified Index view in Listing 4 uses the MovieTemplate partial.</span></span>

<span data-ttu-id="a7ce6-213">**Wyświetlanie listy 4 — Views\Movie\Index.aspx**</span><span class="sxs-lookup"><span data-stu-id="a7ce6-213">**Listing 4 – Views\Movie\Index.aspx**</span></span>

[!code-aspx[Main](displaying-a-table-of-database-data-cs/samples/sample4.aspx)]

<span data-ttu-id="a7ce6-214">Widok w listę 4 zawiera pętli foreach, który iteruje po wszystkie filmy.</span><span class="sxs-lookup"><span data-stu-id="a7ce6-214">The view in Listing 4 contains a foreach loop that iterates through all of the movies.</span></span> <span data-ttu-id="a7ce6-215">Dla każdego filmu częściowe MovieTemplate jest używany do formatowania filmu.</span><span class="sxs-lookup"><span data-stu-id="a7ce6-215">For each movie, the MovieTemplate partial is used to format the movie.</span></span> <span data-ttu-id="a7ce6-216">MovieTemplate jest renderowany przez wywołanie metody pomocnika RenderPartial().</span><span class="sxs-lookup"><span data-stu-id="a7ce6-216">The MovieTemplate is rendered by calling the RenderPartial() helper method.</span></span>

<span data-ttu-id="a7ce6-217">Zmodyfikowany widok indeksu renderuje w tej samej tabeli HTML rekordów bazy danych.</span><span class="sxs-lookup"><span data-stu-id="a7ce6-217">The modified Index view renders the very same HTML table of database records.</span></span> <span data-ttu-id="a7ce6-218">Jednak widoku znacznie uproszczono.</span><span class="sxs-lookup"><span data-stu-id="a7ce6-218">However, the view has been greatly simplified.</span></span>


<span data-ttu-id="a7ce6-219">Metoda RenderPartial() jest inny niż w przypadku pozostałych metod pomocniczych, ponieważ nie zwraca ciąg.</span><span class="sxs-lookup"><span data-stu-id="a7ce6-219">The RenderPartial() method is different than most of the other helper methods because it does not return a string.</span></span> <span data-ttu-id="a7ce6-220">W związku z tym należy wywołać przy użyciu metody RenderPartial() &lt;% % Html.RenderPartial();&gt; zamiast &lt;% = Html.RenderPartial(); %&gt;.</span><span class="sxs-lookup"><span data-stu-id="a7ce6-220">Therefore, you must call the RenderPartial() method using &lt;% Html.RenderPartial(); %&gt; instead of &lt;%= Html.RenderPartial(); %&gt;.</span></span>


## <a name="summary"></a><span data-ttu-id="a7ce6-221">Podsumowanie</span><span class="sxs-lookup"><span data-stu-id="a7ce6-221">Summary</span></span>

<span data-ttu-id="a7ce6-222">Celem tego samouczka jest wyjaśnienie, jak wyświetlić zestaw rekordów bazy danych w tabeli HTML.</span><span class="sxs-lookup"><span data-stu-id="a7ce6-222">The goal of this tutorial was to explain how you can display a set of database records in an HTML table.</span></span> <span data-ttu-id="a7ce6-223">Najpierw przedstawiono sposób zwracania zestawu rekordów bazy danych z akcji kontrolera, korzystając z programu Entity Framework firmy Microsoft.</span><span class="sxs-lookup"><span data-stu-id="a7ce6-223">First, you learned how to return a set of database records from a controller action by taking advantage of the Microsoft Entity Framework.</span></span> <span data-ttu-id="a7ce6-224">Następnie przedstawiono sposób użycia szkieletów Visual Studio do generowania widoku, który automatycznie wyświetla kolekcję elementów.</span><span class="sxs-lookup"><span data-stu-id="a7ce6-224">Next, you learned how to use Visual Studio scaffolding to generate a view that displays a collection of items automatically.</span></span> <span data-ttu-id="a7ce6-225">Ponadto przedstawiono sposób uproszczenia widoku dzięki wykorzystaniu częściowym.</span><span class="sxs-lookup"><span data-stu-id="a7ce6-225">Finally, you learned how to simplify the view by taking advantage of a partial.</span></span> <span data-ttu-id="a7ce6-226">Przedstawiono sposób użycia częściowym jako szablon, dzięki czemu można sformatować każdego rekordu bazy danych.</span><span class="sxs-lookup"><span data-stu-id="a7ce6-226">You learned how to use a partial as a template so that you can format each database record.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="a7ce6-227">[Poprzednie](creating-model-classes-with-linq-to-sql-cs.md)
> [dalej](performing-simple-validation-cs.md)</span><span class="sxs-lookup"><span data-stu-id="a7ce6-227">[Previous](creating-model-classes-with-linq-to-sql-cs.md)
[Next](performing-simple-validation-cs.md)</span></span>
