---
uid: mvc/overview/older-versions-1/models-data/displaying-a-table-of-database-data-cs
title: Wyświetlanie tabeli danych bazy danych (C#) | Dokumentacja firmy Microsoft
author: microsoft
description: W tym samouczku pokazują I, wyświetlany jest zestaw rekordów bazy danych na dwa sposoby. Czy mogę Pokaż dwie metody formatowania zestaw rekordów bazy danych w formacie HTML danych...
ms.author: riande
ms.date: 10/07/2008
ms.assetid: d6e758b6-6571-484d-a132-34ee6c47747a
msc.legacyurl: /mvc/overview/older-versions-1/models-data/displaying-a-table-of-database-data-cs
msc.type: authoredcontent
ms.openlocfilehash: d0d3f6a574a4b923d5da73ccb2ab3bfbd6f305ef
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/16/2018
ms.locfileid: "41753529"
---
<a name="displaying-a-table-of-database-data-c"></a><span data-ttu-id="f2251-104">Wyświetlanie tabeli danych bazy danych (C#)</span><span class="sxs-lookup"><span data-stu-id="f2251-104">Displaying a Table of Database Data (C#)</span></span>
====================
<span data-ttu-id="f2251-105">przez [firmy Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="f2251-105">by [Microsoft](https://github.com/microsoft)</span></span>

[<span data-ttu-id="f2251-106">Pobierz plik PDF</span><span class="sxs-lookup"><span data-stu-id="f2251-106">Download PDF</span></span>](http://download.microsoft.com/download/1/1/f/11f721aa-d749-4ed7-bb89-a681b68894e6/ASPNET_MVC_Tutorial_11_CS.pdf)

> <span data-ttu-id="f2251-107">W tym samouczku pokazują I, wyświetlany jest zestaw rekordów bazy danych na dwa sposoby.</span><span class="sxs-lookup"><span data-stu-id="f2251-107">In this tutorial, I demonstrate two methods of displaying a set of database records.</span></span> <span data-ttu-id="f2251-108">Czy mogę wyświetlić dwie metody formatowania zestaw rekordów bazy danych w tabeli HTML.</span><span class="sxs-lookup"><span data-stu-id="f2251-108">I show two methods of formatting a set of database records in an HTML table.</span></span> <span data-ttu-id="f2251-109">Po pierwsze I pokazują, jak można sformatować rekordów bazy danych bezpośrednio z poziomu widoku.</span><span class="sxs-lookup"><span data-stu-id="f2251-109">First, I show how you can format the database records directly within a view.</span></span> <span data-ttu-id="f2251-110">Następnie I pokazują, jak można korzystać z zalet częściowych podczas formatowania rekordów bazy danych.</span><span class="sxs-lookup"><span data-stu-id="f2251-110">Next, I demonstrate how you can take advantage of partials when formatting database records.</span></span>


<span data-ttu-id="f2251-111">Celem tego samouczka jest wyjaśniają, jak można wyświetlić tabeli HTML danych bazy danych w aplikacji ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="f2251-111">The goal of this tutorial is to explain how you can display an HTML table of database data in an ASP.NET MVC application.</span></span> <span data-ttu-id="f2251-112">Po pierwsze dowiesz się, jak używać narzędzia do tworzenia szkieletów zawarte w Visual Studio do generowania widoku, który automatycznie wyświetla zestaw rekordów.</span><span class="sxs-lookup"><span data-stu-id="f2251-112">First, you learn how to use the scaffolding tools included in Visual Studio to generate a view that displays a set of records automatically.</span></span> <span data-ttu-id="f2251-113">Następnie dowiesz się, jak używać częściowym jako szablon, podczas formatowania rekordów bazy danych.</span><span class="sxs-lookup"><span data-stu-id="f2251-113">Next, you learn how to use a partial as a template when formatting database records.</span></span>

## <a name="create-the-model-classes"></a><span data-ttu-id="f2251-114">Tworzenie klas modelu</span><span class="sxs-lookup"><span data-stu-id="f2251-114">Create the Model Classes</span></span>

<span data-ttu-id="f2251-115">Firma Microsoft zamierza wyświetlić zestaw rekordów z tabeli bazy danych filmów.</span><span class="sxs-lookup"><span data-stu-id="f2251-115">We are going to display the set of records from the Movies database table.</span></span> <span data-ttu-id="f2251-116">Tabela bazy danych filmów zawiera następujące kolumny:</span><span class="sxs-lookup"><span data-stu-id="f2251-116">The Movies database table contains the following columns:</span></span>

<a id="0.3_table01"></a>


| <span data-ttu-id="f2251-117">**Nazwa kolumny**</span><span class="sxs-lookup"><span data-stu-id="f2251-117">**Column Name**</span></span> | <span data-ttu-id="f2251-118">**Typ danych**</span><span class="sxs-lookup"><span data-stu-id="f2251-118">**Data Type**</span></span> | <span data-ttu-id="f2251-119">**Zezwalaj na wartości null**</span><span class="sxs-lookup"><span data-stu-id="f2251-119">**Allow Nulls**</span></span> |
| --- | --- | --- |
| <span data-ttu-id="f2251-120">Id</span><span class="sxs-lookup"><span data-stu-id="f2251-120">Id</span></span> | <span data-ttu-id="f2251-121">int</span><span class="sxs-lookup"><span data-stu-id="f2251-121">Int</span></span> | <span data-ttu-id="f2251-122">False</span><span class="sxs-lookup"><span data-stu-id="f2251-122">False</span></span> |
| <span data-ttu-id="f2251-123">Tytuł</span><span class="sxs-lookup"><span data-stu-id="f2251-123">Title</span></span> | <span data-ttu-id="f2251-124">Nvarchar(200)</span><span class="sxs-lookup"><span data-stu-id="f2251-124">Nvarchar(200)</span></span> | <span data-ttu-id="f2251-125">False</span><span class="sxs-lookup"><span data-stu-id="f2251-125">False</span></span> |
| <span data-ttu-id="f2251-126">Dyrektor ds.</span><span class="sxs-lookup"><span data-stu-id="f2251-126">Director</span></span> | <span data-ttu-id="f2251-127">NVarchar(50)</span><span class="sxs-lookup"><span data-stu-id="f2251-127">NVarchar(50)</span></span> | <span data-ttu-id="f2251-128">False</span><span class="sxs-lookup"><span data-stu-id="f2251-128">False</span></span> |
| <span data-ttu-id="f2251-129">DateReleased</span><span class="sxs-lookup"><span data-stu-id="f2251-129">DateReleased</span></span> | <span data-ttu-id="f2251-130">DataGodzina</span><span class="sxs-lookup"><span data-stu-id="f2251-130">DateTime</span></span> | <span data-ttu-id="f2251-131">False</span><span class="sxs-lookup"><span data-stu-id="f2251-131">False</span></span> |


<span data-ttu-id="f2251-132">Aby móc przedstawić w tabeli filmów w naszej aplikacji ASP.NET MVC, należy utworzyć klasę modelu.</span><span class="sxs-lookup"><span data-stu-id="f2251-132">In order to represent the Movies table in our ASP.NET MVC application, we need to create a model class.</span></span> <span data-ttu-id="f2251-133">W tym samouczku używamy Microsoft Entity Framework do tworzenia klas w naszym modelu.</span><span class="sxs-lookup"><span data-stu-id="f2251-133">In this tutorial, we use the Microsoft Entity Framework to create our model classes.</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="f2251-134">W tym samouczku używamy Microsoft Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="f2251-134">In this tutorial, we use the Microsoft Entity Framework.</span></span> <span data-ttu-id="f2251-135">Jednak ważne jest zrozumienie, czy można użyć szereg różnych technologie do interakcji z bazą danych z aplikacji ASP.NET MVC, w tym LINQ to SQL i NHibernate, ADO.NET.</span><span class="sxs-lookup"><span data-stu-id="f2251-135">However, it is important to understand that you can use a variety of different technologies to interact with a database from an ASP.NET MVC application including LINQ to SQL, NHibernate, or ADO.NET.</span></span>


<span data-ttu-id="f2251-136">Wykonaj następujące kroki, aby uruchomić Kreator modelu danych jednostki:</span><span class="sxs-lookup"><span data-stu-id="f2251-136">Follow these steps to launch the Entity Data Model Wizard:</span></span>

1. <span data-ttu-id="f2251-137">Kliknij prawym przyciskiem myszy folderu modeli w oknie Eksploratora rozwiązań i wybierz opcję menu **Dodaj, nowy element**.</span><span class="sxs-lookup"><span data-stu-id="f2251-137">Right-click the Models folder in the Solution Explorer window and the select the menu option **Add, New Item**.</span></span>
2. <span data-ttu-id="f2251-138">Wybierz **danych** kategorii, a następnie wybierz **ADO.NET Entity Data Model** szablonu.</span><span class="sxs-lookup"><span data-stu-id="f2251-138">Select the **Data** category and select the **ADO.NET Entity Data Model** template.</span></span>
3. <span data-ttu-id="f2251-139">Nadaj nazwę modelu danych *MoviesDBModel.edmx* i kliknij przycisk **Dodaj** przycisku.</span><span class="sxs-lookup"><span data-stu-id="f2251-139">Give your data model the name *MoviesDBModel.edmx* and click the **Add** button.</span></span>

<span data-ttu-id="f2251-140">Po kliknięciu przycisku Dodaj zostanie wyświetlony Kreator modelu Entity Data Model (patrz rysunek 1).</span><span class="sxs-lookup"><span data-stu-id="f2251-140">After you click the Add button, the Entity Data Model Wizard appears (see Figure 1).</span></span> <span data-ttu-id="f2251-141">Wykonaj następujące kroki, aby zakończyć działanie kreatora:</span><span class="sxs-lookup"><span data-stu-id="f2251-141">Follow these steps to complete the wizard:</span></span>

1. <span data-ttu-id="f2251-142">W **wybierz zawartość modelu** kroku, wybierz pozycję **Generuj z bazy danych** opcji.</span><span class="sxs-lookup"><span data-stu-id="f2251-142">In the **Choose Model Contents** step, select the **Generate from database** option.</span></span>
2. <span data-ttu-id="f2251-143">W **wybierz połączenie danych** kroku, należy użyć *MoviesDB.mdf* połączenia danych i nazwę *MoviesDBEntities* dla ustawień połączenia.</span><span class="sxs-lookup"><span data-stu-id="f2251-143">In the **Choose Your Data Connection** step, use the *MoviesDB.mdf* data connection and the name *MoviesDBEntities* for the connection settings.</span></span> <span data-ttu-id="f2251-144">Kliknij przycisk **dalej** przycisku.</span><span class="sxs-lookup"><span data-stu-id="f2251-144">Click the **Next** button.</span></span>
3. <span data-ttu-id="f2251-145">W **wybierz obiekty bazy danych** kroku, rozwiń węzeł tabele, wybierz tabelę filmów.</span><span class="sxs-lookup"><span data-stu-id="f2251-145">In the **Choose Your Database Objects** step, expand the Tables node, select the Movies table.</span></span> <span data-ttu-id="f2251-146">Wprowadź przestrzeń nazw *modeli* i kliknij przycisk **Zakończ** przycisku.</span><span class="sxs-lookup"><span data-stu-id="f2251-146">Enter the namespace *Models* and click the **Finish** button.</span></span>


<span data-ttu-id="f2251-147">[![Tworzenie zapytań LINQ do klas SQL](displaying-a-table-of-database-data-cs/_static/image1.jpg)](displaying-a-table-of-database-data-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="f2251-147">[![Creating LINQ to SQL classes](displaying-a-table-of-database-data-cs/_static/image1.jpg)](displaying-a-table-of-database-data-cs/_static/image1.png)</span></span>

<span data-ttu-id="f2251-148">**Rysunek 01**: Tworzenie klasy programu LINQ to SQL ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](displaying-a-table-of-database-data-cs/_static/image2.png))</span><span class="sxs-lookup"><span data-stu-id="f2251-148">**Figure 01**: Creating LINQ to SQL classes ([Click to view full-size image](displaying-a-table-of-database-data-cs/_static/image2.png))</span></span>


<span data-ttu-id="f2251-149">Po zakończeniu działania Kreator modelu Entity Data Model, zostanie otwarty projektant modelu danych jednostki.</span><span class="sxs-lookup"><span data-stu-id="f2251-149">After you complete the Entity Data Model Wizard, the Entity Data Model Designer opens.</span></span> <span data-ttu-id="f2251-150">Projektant powinien być wyświetlany jednostki filmy (patrz rysunek 2).</span><span class="sxs-lookup"><span data-stu-id="f2251-150">The Designer should display the Movies entity (see Figure 2).</span></span>


<span data-ttu-id="f2251-151">[![Projektant modelu danych jednostki](displaying-a-table-of-database-data-cs/_static/image2.jpg)](displaying-a-table-of-database-data-cs/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="f2251-151">[![The Entity Data Model Designer](displaying-a-table-of-database-data-cs/_static/image2.jpg)](displaying-a-table-of-database-data-cs/_static/image3.png)</span></span>

<span data-ttu-id="f2251-152">**Rysunek 02**: projektancie Entity Data Model ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](displaying-a-table-of-database-data-cs/_static/image4.png))</span><span class="sxs-lookup"><span data-stu-id="f2251-152">**Figure 02**: The Entity Data Model Designer ([Click to view full-size image](displaying-a-table-of-database-data-cs/_static/image4.png))</span></span>


<span data-ttu-id="f2251-153">Należy wprowadzić zmianę jeden, przed kontynuowaniem.</span><span class="sxs-lookup"><span data-stu-id="f2251-153">We need to make one change before we continue.</span></span> <span data-ttu-id="f2251-154">Kreator modelu Entity Data generuje klasę modelu o nazwie *filmy* reprezentujący tabelę bazy danych filmów.</span><span class="sxs-lookup"><span data-stu-id="f2251-154">The Entity Data Wizard generates a model class named *Movies* that represents the Movies database table.</span></span> <span data-ttu-id="f2251-155">Ponieważ będziemy korzystać do reprezentowania filmu konkretnej klasy filmy, należy zmodyfikować nazwę klasy, która ma być *filmu* zamiast *filmy* (pojedynczej zamiast liczba mnoga).</span><span class="sxs-lookup"><span data-stu-id="f2251-155">Because we'll use the Movies class to represent a particular movie, we need to modify the name of the class to be *Movie* instead of *Movies* (singular rather than plural).</span></span>

<span data-ttu-id="f2251-156">Kliknij dwukrotnie nazwę klasy na powierzchni projektowej i Zmień nazwę klasy z filmy filmu.</span><span class="sxs-lookup"><span data-stu-id="f2251-156">Double-click the name of the class on the designer surface and change the name of the class from Movies to Movie.</span></span> <span data-ttu-id="f2251-157">Po wprowadzeniu tej zmiany, kliknij przycisk **Zapisz** przycisk (ikona dysku), aby wygenerować klasę filmu.</span><span class="sxs-lookup"><span data-stu-id="f2251-157">After making this change, click the **Save** button (the icon of the floppy disk) to generate the Movie class.</span></span>

## <a name="create-the-movies-controller"></a><span data-ttu-id="f2251-158">Tworzenie kontrolera filmy</span><span class="sxs-lookup"><span data-stu-id="f2251-158">Create the Movies Controller</span></span>

<span data-ttu-id="f2251-159">Teraz, gdy mamy już sposobem reprezentowania naszych danych bazy danych, możemy utworzyć kontroler, który zwraca kolekcję filmów.</span><span class="sxs-lookup"><span data-stu-id="f2251-159">Now that we have a way to represent our database records, we can create a controller that returns the collection of movies.</span></span> <span data-ttu-id="f2251-160">W oknie Eksploratora rozwiązań w usłudze Visual Studio kliknij prawym przyciskiem myszy folder kontrolerów, a następnie wybierz opcję menu **Dodaj, kontroler** (zobacz rysunek 3).</span><span class="sxs-lookup"><span data-stu-id="f2251-160">Within the Visual Studio Solution Explorer window, right-click the Controllers folder and select the menu option **Add, Controller** (see Figure 3).</span></span>


<span data-ttu-id="f2251-161">[![Dodawanie kontrolera Menu](displaying-a-table-of-database-data-cs/_static/image3.jpg)](displaying-a-table-of-database-data-cs/_static/image5.png)</span><span class="sxs-lookup"><span data-stu-id="f2251-161">[![The Add Controller Menu](displaying-a-table-of-database-data-cs/_static/image3.jpg)](displaying-a-table-of-database-data-cs/_static/image5.png)</span></span>

<span data-ttu-id="f2251-162">**Rysunek 03**: Dodawanie kontrolera Menu ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](displaying-a-table-of-database-data-cs/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="f2251-162">**Figure 03**: The Add Controller Menu ([Click to view full-size image](displaying-a-table-of-database-data-cs/_static/image6.png))</span></span>


<span data-ttu-id="f2251-163">Gdy **Dodaj kontroler** zostanie wyświetlone okno dialogowe, wprowadź nazwę kontrolera MovieController (zobacz rysunek 4).</span><span class="sxs-lookup"><span data-stu-id="f2251-163">When the **Add Controller** dialog appears, enter the controller name MovieController (see Figure 4).</span></span> <span data-ttu-id="f2251-164">Kliknij przycisk **Dodaj** przycisk, aby dodać nowy kontroler.</span><span class="sxs-lookup"><span data-stu-id="f2251-164">Click the **Add** button to add the new controller.</span></span>


<span data-ttu-id="f2251-165">[![Okno dialogowe Dodawanie kontrolera](displaying-a-table-of-database-data-cs/_static/image4.jpg)](displaying-a-table-of-database-data-cs/_static/image7.png)</span><span class="sxs-lookup"><span data-stu-id="f2251-165">[![The Add Controller dialog](displaying-a-table-of-database-data-cs/_static/image4.jpg)](displaying-a-table-of-database-data-cs/_static/image7.png)</span></span>

<span data-ttu-id="f2251-166">**Rysunek 04**: okno dialogowe Dodawanie kontrolera ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](displaying-a-table-of-database-data-cs/_static/image8.png))</span><span class="sxs-lookup"><span data-stu-id="f2251-166">**Figure 04**: The Add Controller dialog([Click to view full-size image](displaying-a-table-of-database-data-cs/_static/image8.png))</span></span>


<span data-ttu-id="f2251-167">Należy zmodyfikować akcję indeks() udostępnianych przez kontroler filmu, tak aby zwraca zestaw rekordów bazy danych.</span><span class="sxs-lookup"><span data-stu-id="f2251-167">We need to modify the Index() action exposed by the Movie controller so that it returns the set of database records.</span></span> <span data-ttu-id="f2251-168">Tak, aby wyglądało kontrolera w ofercie 1, należy zmodyfikować kontrolera.</span><span class="sxs-lookup"><span data-stu-id="f2251-168">Modify the controller so that it looks like the controller in Listing 1.</span></span>

<span data-ttu-id="f2251-169">**Wyświetlanie listy 1 – Controllers\MovieController.cs**</span><span class="sxs-lookup"><span data-stu-id="f2251-169">**Listing 1 – Controllers\MovieController.cs**</span></span>

[!code-csharp[Main](displaying-a-table-of-database-data-cs/samples/sample1.cs)]

<span data-ttu-id="f2251-170">W ofercie 1 klasa MoviesDBEntities jest używana do reprezentowania MoviesDB bazy danych.</span><span class="sxs-lookup"><span data-stu-id="f2251-170">In Listing 1, the MoviesDBEntities class is used to represent the MoviesDB database.</span></span> <span data-ttu-id="f2251-171">Aby użyć tej klasy, należy zaimportować obszar nazw MvcApplication1.Models następująco:</span><span class="sxs-lookup"><span data-stu-id="f2251-171">To use this class, you need to import the MvcApplication1.Models namespace like this:</span></span>

<span data-ttu-id="f2251-172">using MvcApplication1.Models;</span><span class="sxs-lookup"><span data-stu-id="f2251-172">using MvcApplication1.Models;</span></span>

<span data-ttu-id="f2251-173">Wyrażenie *jednostek. MovieSet.ToList()* zwraca zestaw wszystkich filmów z tabeli bazy danych filmów.</span><span class="sxs-lookup"><span data-stu-id="f2251-173">The expression *entities.MovieSet.ToList()* returns the set of all movies from the Movies database table.</span></span>

## <a name="create-the-view"></a><span data-ttu-id="f2251-174">Tworzenie widoku</span><span class="sxs-lookup"><span data-stu-id="f2251-174">Create the View</span></span>

<span data-ttu-id="f2251-175">Najprostszym sposobem wyświetlania zestawu rekordów bazy danych w tabeli HTML jest korzystanie z zalet tworzenia szkieletów udostępniane przez program Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="f2251-175">The easiest way to display a set of database records in an HTML table is to take advantage of the scaffolding provided by Visual Studio.</span></span>

<span data-ttu-id="f2251-176">Kompiluj aplikację, wybierając opcję menu **twórz, Kompiluj rozwiązanie**.</span><span class="sxs-lookup"><span data-stu-id="f2251-176">Build your application by selecting the menu option **Build, Build Solution**.</span></span> <span data-ttu-id="f2251-177">Należy skompilować aplikację przed otwarciem **Dodaj widok** okna dialogowego lub klas usługi danych nie będzie wyświetlane w oknie dialogowym.</span><span class="sxs-lookup"><span data-stu-id="f2251-177">You must build your application before opening the **Add View** dialog or your data classes won't appear in the dialog.</span></span>

<span data-ttu-id="f2251-178">Kliknij prawym przyciskiem myszy działanie indeks() i wybierz opcję menu **Dodaj widok** (zobacz rysunek 5).</span><span class="sxs-lookup"><span data-stu-id="f2251-178">Right-click the Index() action and select the menu option **Add View** (see Figure 5).</span></span>


<span data-ttu-id="f2251-179">[![Dodawanie widoku](displaying-a-table-of-database-data-cs/_static/image5.jpg)](displaying-a-table-of-database-data-cs/_static/image9.png)</span><span class="sxs-lookup"><span data-stu-id="f2251-179">[![Adding a view](displaying-a-table-of-database-data-cs/_static/image5.jpg)](displaying-a-table-of-database-data-cs/_static/image9.png)</span></span>

<span data-ttu-id="f2251-180">**Rysunek 05**: Dodawanie widoku ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](displaying-a-table-of-database-data-cs/_static/image10.png))</span><span class="sxs-lookup"><span data-stu-id="f2251-180">**Figure 05**: Adding a view ([Click to view full-size image](displaying-a-table-of-database-data-cs/_static/image10.png))</span></span>


<span data-ttu-id="f2251-181">W **Dodaj widok** okno dialogowe, zaznacz pola wyboru **utworzyć widok silnie typizowane**.</span><span class="sxs-lookup"><span data-stu-id="f2251-181">In the **Add View** dialog, check the checkbox labeled **Create a strongly-typed view**.</span></span> <span data-ttu-id="f2251-182">Wybierz klasę filmu jako **wyświetlić klasy danych**.</span><span class="sxs-lookup"><span data-stu-id="f2251-182">Select the Movie class as the **view data class**.</span></span> <span data-ttu-id="f2251-183">Wybierz *listy* jako **wyświetlanie zawartości** (patrz rysunek 6).</span><span class="sxs-lookup"><span data-stu-id="f2251-183">Select *List* as the **view content** (see Figure 6).</span></span> <span data-ttu-id="f2251-184">Zaznaczenie tych opcji spowoduje wygenerowanie silnie typizowane widoku, który wyświetla listę filmów.</span><span class="sxs-lookup"><span data-stu-id="f2251-184">Selecting these options will generate a strongly-typed view that displays a list of movies.</span></span>


<span data-ttu-id="f2251-185">[![Okno dialogowe dodawania widoku](displaying-a-table-of-database-data-cs/_static/image6.jpg)](displaying-a-table-of-database-data-cs/_static/image11.png)</span><span class="sxs-lookup"><span data-stu-id="f2251-185">[![The Add View dialog](displaying-a-table-of-database-data-cs/_static/image6.jpg)](displaying-a-table-of-database-data-cs/_static/image11.png)</span></span>

<span data-ttu-id="f2251-186">**Rysunek 06**: okno dialogowe Dodawanie widoku ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](displaying-a-table-of-database-data-cs/_static/image12.png))</span><span class="sxs-lookup"><span data-stu-id="f2251-186">**Figure 06**: The Add View dialog([Click to view full-size image](displaying-a-table-of-database-data-cs/_static/image12.png))</span></span>


<span data-ttu-id="f2251-187">Po kliknięciu **Dodaj** przycisk, widok w ofercie 2 jest generowany automatycznie.</span><span class="sxs-lookup"><span data-stu-id="f2251-187">After you click the **Add** button, the view in Listing 2 is generated automatically.</span></span> <span data-ttu-id="f2251-188">Ten widok zawiera kod wymagany do iterowania po kolekcji filmów i wyświetlane poszczególne właściwości filmu.</span><span class="sxs-lookup"><span data-stu-id="f2251-188">This view contains the code required to iterate through the collection of movies and display each of the properties of a movie.</span></span>

<span data-ttu-id="f2251-189">**Wyświetlanie listy 2 — Views\Movie\Index.aspx**</span><span class="sxs-lookup"><span data-stu-id="f2251-189">**Listing 2 – Views\Movie\Index.aspx**</span></span>

[!code-aspx[Main](displaying-a-table-of-database-data-cs/samples/sample2.aspx)]

<span data-ttu-id="f2251-190">Możesz uruchomić aplikację, wybierając opcję menu **debugowania i Rozpocznij debugowanie** (lub naciskając klawisz F5).</span><span class="sxs-lookup"><span data-stu-id="f2251-190">You can run the application by selecting the menu option **Debug, Start Debugging** (or hitting the F5 key).</span></span> <span data-ttu-id="f2251-191">Uruchomiona jest aplikacja uruchomi program Internet Explorer.</span><span class="sxs-lookup"><span data-stu-id="f2251-191">Running the application launches Internet Explorer.</span></span> <span data-ttu-id="f2251-192">Jeśli przejdziesz do adresu URL /Movie, a następnie zobaczysz stronę na rysunku 7.</span><span class="sxs-lookup"><span data-stu-id="f2251-192">If you navigate to the /Movie URL then you'll see the page in Figure 7.</span></span>


<span data-ttu-id="f2251-193">[![Tabelę filmy](displaying-a-table-of-database-data-cs/_static/image7.jpg)](displaying-a-table-of-database-data-cs/_static/image13.png)</span><span class="sxs-lookup"><span data-stu-id="f2251-193">[![A table of movies](displaying-a-table-of-database-data-cs/_static/image7.jpg)](displaying-a-table-of-database-data-cs/_static/image13.png)</span></span>

<span data-ttu-id="f2251-194">**Rysunek 07**: tabelę filmy ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](displaying-a-table-of-database-data-cs/_static/image14.png))</span><span class="sxs-lookup"><span data-stu-id="f2251-194">**Figure 07**: A table of movies([Click to view full-size image](displaying-a-table-of-database-data-cs/_static/image14.png))</span></span>


<span data-ttu-id="f2251-195">Jeśli nie potrzebujesz nic o wyglądzie siatki rekordów bazy danych na rysunku 7 można po prostu zmodyfikuj widok indeksu.</span><span class="sxs-lookup"><span data-stu-id="f2251-195">If you don't like anything about the appearance of the grid of database records in Figure 7 then you can simply modify the Index view.</span></span> <span data-ttu-id="f2251-196">Na przykład można zmienić *DateReleased* nagłówka do *Data wydania* przez zmodyfikowanie widoku indeksu.</span><span class="sxs-lookup"><span data-stu-id="f2251-196">For example, you can change the *DateReleased* header to *Date Released* by modifying the Index view.</span></span>

## <a name="create-a-template-with-a-partial"></a><span data-ttu-id="f2251-197">Tworzenie szablonu z częściowym</span><span class="sxs-lookup"><span data-stu-id="f2251-197">Create a Template with a Partial</span></span>

<span data-ttu-id="f2251-198">Jeśli widok pobiera zbyt skomplikowane, jest dobry pomysł, aby uruchomić podzielenie widoku na częściowych.</span><span class="sxs-lookup"><span data-stu-id="f2251-198">When a view gets too complicated, it is a good idea to start breaking the view into partials.</span></span> <span data-ttu-id="f2251-199">Za pomocą częściowych ułatwia widoków do zrozumienia i utrzymania.</span><span class="sxs-lookup"><span data-stu-id="f2251-199">Using partials makes your views easier to understand and maintain.</span></span> <span data-ttu-id="f2251-200">Utworzymy częściowym, możemy użyć jako szablonu, aby sformatować listę rekordów bazy danych filmów.</span><span class="sxs-lookup"><span data-stu-id="f2251-200">We'll create a partial that we can use as a template to format each of the movie database records.</span></span>

<span data-ttu-id="f2251-201">Wykonaj następujące kroki, aby utworzyć częściowego:</span><span class="sxs-lookup"><span data-stu-id="f2251-201">Follow these steps to create the partial:</span></span>

1. <span data-ttu-id="f2251-202">Kliknij prawym przyciskiem myszy Views\Movie folder i wybierz opcję menu **Dodaj widok**.</span><span class="sxs-lookup"><span data-stu-id="f2251-202">Right-click the Views\Movie folder and select the menu option **Add View**.</span></span>
2. <span data-ttu-id="f2251-203">Zaznacz pola wyboru *tworzenia widoku częściowego (.ascx)*.</span><span class="sxs-lookup"><span data-stu-id="f2251-203">Check the checkbox labeled *Create a partial view (.ascx)*.</span></span>
3. <span data-ttu-id="f2251-204">Nadaj nazwę częściowego *MovieTemplate*.</span><span class="sxs-lookup"><span data-stu-id="f2251-204">Name the partial *MovieTemplate*.</span></span>
4. <span data-ttu-id="f2251-205">Zaznacz pola wyboru **utworzyć widok silnie typizowane**.</span><span class="sxs-lookup"><span data-stu-id="f2251-205">Check the checkbox labeled **Create a strongly-typed view**.</span></span>
5. <span data-ttu-id="f2251-206">Wybierz filmu jako *wyświetlić klasy danych*.</span><span class="sxs-lookup"><span data-stu-id="f2251-206">Select Movie as the *view data class*.</span></span>
6. <span data-ttu-id="f2251-207">Wybierz pusty jako *wyświetlanie zawartości*.</span><span class="sxs-lookup"><span data-stu-id="f2251-207">Select Empty as the *view content*.</span></span>
7. <span data-ttu-id="f2251-208">Kliknij przycisk **Dodaj** przycisk, aby dodać części do projektu.</span><span class="sxs-lookup"><span data-stu-id="f2251-208">Click the **Add** button to add the partial to your project.</span></span>

<span data-ttu-id="f2251-209">Po wykonaniu tych kroków, należy zmodyfikować MovieTemplate częściowe wyglądać lista 3.</span><span class="sxs-lookup"><span data-stu-id="f2251-209">After you complete these steps, modify the MovieTemplate partial to look like Listing 3.</span></span>

<span data-ttu-id="f2251-210">**Wyświetlanie listy 3 — Views\Movie\MovieTemplate.ascx**</span><span class="sxs-lookup"><span data-stu-id="f2251-210">**Listing 3 – Views\Movie\MovieTemplate.ascx**</span></span>

[!code-aspx[Main](displaying-a-table-of-database-data-cs/samples/sample3.aspx)]

<span data-ttu-id="f2251-211">Partial w ofercie 3 zawiera szablon w pojedynczym wierszu rekordów.</span><span class="sxs-lookup"><span data-stu-id="f2251-211">The partial in Listing 3 contains a template for a single row of records.</span></span>

<span data-ttu-id="f2251-212">Zmodyfikowany widok indeksu w ofercie 4 używa MovieTemplate częściowe.</span><span class="sxs-lookup"><span data-stu-id="f2251-212">The modified Index view in Listing 4 uses the MovieTemplate partial.</span></span>

<span data-ttu-id="f2251-213">**Wyświetlanie listy 4 — Views\Movie\Index.aspx**</span><span class="sxs-lookup"><span data-stu-id="f2251-213">**Listing 4 – Views\Movie\Index.aspx**</span></span>

[!code-aspx[Main](displaying-a-table-of-database-data-cs/samples/sample4.aspx)]

<span data-ttu-id="f2251-214">Widok w ofercie 4 zawiera pętlę foreach, która wykonuje iterację przez wszystkie filmy.</span><span class="sxs-lookup"><span data-stu-id="f2251-214">The view in Listing 4 contains a foreach loop that iterates through all of the movies.</span></span> <span data-ttu-id="f2251-215">Dla każdego filmu częściowe MovieTemplate jest używany do formatowania filmu.</span><span class="sxs-lookup"><span data-stu-id="f2251-215">For each movie, the MovieTemplate partial is used to format the movie.</span></span> <span data-ttu-id="f2251-216">MovieTemplate jest renderowany przez wywołanie metody pomocnika RenderPartial().</span><span class="sxs-lookup"><span data-stu-id="f2251-216">The MovieTemplate is rendered by calling the RenderPartial() helper method.</span></span>

<span data-ttu-id="f2251-217">Zmodyfikowany widok indeksu powoduje wyświetlenie tej samej tabeli HTML rekordów bazy danych.</span><span class="sxs-lookup"><span data-stu-id="f2251-217">The modified Index view renders the very same HTML table of database records.</span></span> <span data-ttu-id="f2251-218">Jednak widok został znacznie uproszczony.</span><span class="sxs-lookup"><span data-stu-id="f2251-218">However, the view has been greatly simplified.</span></span>


<span data-ttu-id="f2251-219">Metoda RenderPartial() jest inne niż większość pozostałych metod pomocniczych, ponieważ nie zwraca ciąg.</span><span class="sxs-lookup"><span data-stu-id="f2251-219">The RenderPartial() method is different than most of the other helper methods because it does not return a string.</span></span> <span data-ttu-id="f2251-220">W związku z tym, należy wywołać przy użyciu metody RenderPartial() &lt;% Html.RenderPartial(); %&gt; zamiast &lt;% = Html.RenderPartial(); %&gt;.</span><span class="sxs-lookup"><span data-stu-id="f2251-220">Therefore, you must call the RenderPartial() method using &lt;% Html.RenderPartial(); %&gt; instead of &lt;%= Html.RenderPartial(); %&gt;.</span></span>


## <a name="summary"></a><span data-ttu-id="f2251-221">Podsumowanie</span><span class="sxs-lookup"><span data-stu-id="f2251-221">Summary</span></span>

<span data-ttu-id="f2251-222">Celem tego samouczka było wyjaśniają, jak można wyświetlić zestaw rekordów bazy danych w tabeli HTML.</span><span class="sxs-lookup"><span data-stu-id="f2251-222">The goal of this tutorial was to explain how you can display a set of database records in an HTML table.</span></span> <span data-ttu-id="f2251-223">Po pierwsze wiesz, jak można zwrócić zestaw rekordów bazy danych w ramach akcji kontrolera, wykorzystując Microsoft Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="f2251-223">First, you learned how to return a set of database records from a controller action by taking advantage of the Microsoft Entity Framework.</span></span> <span data-ttu-id="f2251-224">Następnie omówiono na potrzeby tworzenia szkieletów programu Visual Studio wygenerowania widoku, który automatycznie wyświetla zbiór elementów.</span><span class="sxs-lookup"><span data-stu-id="f2251-224">Next, you learned how to use Visual Studio scaffolding to generate a view that displays a collection of items automatically.</span></span> <span data-ttu-id="f2251-225">Na koniec pokazaliśmy ci, jak uprościć widok, korzystając z częściowego.</span><span class="sxs-lookup"><span data-stu-id="f2251-225">Finally, you learned how to simplify the view by taking advantage of a partial.</span></span> <span data-ttu-id="f2251-226">Przedstawiono sposób użycia częściowym jako szablon, dzięki czemu można formatować każdy rekord bazy danych.</span><span class="sxs-lookup"><span data-stu-id="f2251-226">You learned how to use a partial as a template so that you can format each database record.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="f2251-227">[Poprzednie](creating-model-classes-with-linq-to-sql-cs.md)
> [dalej](performing-simple-validation-cs.md)</span><span class="sxs-lookup"><span data-stu-id="f2251-227">[Previous](creating-model-classes-with-linq-to-sql-cs.md)
[Next](performing-simple-validation-cs.md)</span></span>
