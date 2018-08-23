---
uid: mvc/overview/older-versions-1/models-data/displaying-a-table-of-database-data-vb
title: Wyświetlanie tabeli danych bazy danych (VB) | Dokumentacja firmy Microsoft
author: microsoft
description: W tym samouczku pokazują I, wyświetlany jest zestaw rekordów bazy danych na dwa sposoby. Czy mogę Pokaż dwie metody formatowania zestaw rekordów bazy danych w formacie HTML danych...
ms.author: riande
ms.date: 10/07/2008
ms.assetid: 5bb4587f-5bcd-44f5-b368-3c1709162b35
msc.legacyurl: /mvc/overview/older-versions-1/models-data/displaying-a-table-of-database-data-vb
msc.type: authoredcontent
ms.openlocfilehash: d96f574c9284ab259b8733b3b8109ecd0b689aa8
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/16/2018
ms.locfileid: "41756469"
---
<a name="displaying-a-table-of-database-data-vb"></a><span data-ttu-id="a6f2a-104">Wyświetlanie tabeli danych bazy danych (VB)</span><span class="sxs-lookup"><span data-stu-id="a6f2a-104">Displaying a Table of Database Data (VB)</span></span>
====================
<span data-ttu-id="a6f2a-105">przez [firmy Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="a6f2a-105">by [Microsoft](https://github.com/microsoft)</span></span>

[<span data-ttu-id="a6f2a-106">Pobierz plik PDF</span><span class="sxs-lookup"><span data-stu-id="a6f2a-106">Download PDF</span></span>](http://download.microsoft.com/download/1/1/f/11f721aa-d749-4ed7-bb89-a681b68894e6/ASPNET_MVC_Tutorial_11_VB.pdf)

> <span data-ttu-id="a6f2a-107">W tym samouczku pokazują I, wyświetlany jest zestaw rekordów bazy danych na dwa sposoby.</span><span class="sxs-lookup"><span data-stu-id="a6f2a-107">In this tutorial, I demonstrate two methods of displaying a set of database records.</span></span> <span data-ttu-id="a6f2a-108">Czy mogę wyświetlić dwie metody formatowania zestaw rekordów bazy danych w tabeli HTML.</span><span class="sxs-lookup"><span data-stu-id="a6f2a-108">I show two methods of formatting a set of database records in an HTML table.</span></span> <span data-ttu-id="a6f2a-109">Po pierwsze I pokazują, jak można sformatować rekordów bazy danych bezpośrednio z poziomu widoku.</span><span class="sxs-lookup"><span data-stu-id="a6f2a-109">First, I show how you can format the database records directly within a view.</span></span> <span data-ttu-id="a6f2a-110">Następnie I pokazują, jak można korzystać z zalet częściowych podczas formatowania rekordów bazy danych.</span><span class="sxs-lookup"><span data-stu-id="a6f2a-110">Next, I demonstrate how you can take advantage of partials when formatting database records.</span></span>


<span data-ttu-id="a6f2a-111">Celem tego samouczka jest wyjaśniają, jak można wyświetlić tabeli HTML danych bazy danych w aplikacji ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="a6f2a-111">The goal of this tutorial is to explain how you can display an HTML table of database data in an ASP.NET MVC application.</span></span> <span data-ttu-id="a6f2a-112">Po pierwsze dowiesz się, jak używać narzędzia do tworzenia szkieletów zawarte w Visual Studio do generowania widoku, który automatycznie wyświetla zestaw rekordów.</span><span class="sxs-lookup"><span data-stu-id="a6f2a-112">First, you learn how to use the scaffolding tools included in Visual Studio to generate a view that displays a set of records automatically.</span></span> <span data-ttu-id="a6f2a-113">Następnie dowiesz się, jak używać częściowym jako szablon, podczas formatowania rekordów bazy danych.</span><span class="sxs-lookup"><span data-stu-id="a6f2a-113">Next, you learn how to use a partial as a template when formatting database records.</span></span>

## <a name="create-the-model-classes"></a><span data-ttu-id="a6f2a-114">Tworzenie klas modelu</span><span class="sxs-lookup"><span data-stu-id="a6f2a-114">Create the Model Classes</span></span>

<span data-ttu-id="a6f2a-115">Firma Microsoft zamierza wyświetlić zestaw rekordów z tabeli bazy danych filmów.</span><span class="sxs-lookup"><span data-stu-id="a6f2a-115">We are going to display the set of records from the Movies database table.</span></span> <span data-ttu-id="a6f2a-116">Tabela bazy danych filmów zawiera następujące kolumny:</span><span class="sxs-lookup"><span data-stu-id="a6f2a-116">The Movies database table contains the following columns:</span></span>

<a id="0.4_table01"></a>


| <span data-ttu-id="a6f2a-117">**Nazwa kolumny**</span><span class="sxs-lookup"><span data-stu-id="a6f2a-117">**Column Name**</span></span> | <span data-ttu-id="a6f2a-118">**Typ danych**</span><span class="sxs-lookup"><span data-stu-id="a6f2a-118">**Data Type**</span></span> | <span data-ttu-id="a6f2a-119">**Zezwalaj na wartości null**</span><span class="sxs-lookup"><span data-stu-id="a6f2a-119">**Allow Nulls**</span></span> |
| --- | --- | --- |
| <span data-ttu-id="a6f2a-120">Id</span><span class="sxs-lookup"><span data-stu-id="a6f2a-120">Id</span></span> | <span data-ttu-id="a6f2a-121">int</span><span class="sxs-lookup"><span data-stu-id="a6f2a-121">Int</span></span> | <span data-ttu-id="a6f2a-122">False</span><span class="sxs-lookup"><span data-stu-id="a6f2a-122">False</span></span> |
| <span data-ttu-id="a6f2a-123">Tytuł</span><span class="sxs-lookup"><span data-stu-id="a6f2a-123">Title</span></span> | <span data-ttu-id="a6f2a-124">Nvarchar(200)</span><span class="sxs-lookup"><span data-stu-id="a6f2a-124">Nvarchar(200)</span></span> | <span data-ttu-id="a6f2a-125">False</span><span class="sxs-lookup"><span data-stu-id="a6f2a-125">False</span></span> |
| <span data-ttu-id="a6f2a-126">Dyrektor ds.</span><span class="sxs-lookup"><span data-stu-id="a6f2a-126">Director</span></span> | <span data-ttu-id="a6f2a-127">NVarchar(50)</span><span class="sxs-lookup"><span data-stu-id="a6f2a-127">NVarchar(50)</span></span> | <span data-ttu-id="a6f2a-128">False</span><span class="sxs-lookup"><span data-stu-id="a6f2a-128">False</span></span> |
| <span data-ttu-id="a6f2a-129">DateReleased</span><span class="sxs-lookup"><span data-stu-id="a6f2a-129">DateReleased</span></span> | <span data-ttu-id="a6f2a-130">DataGodzina</span><span class="sxs-lookup"><span data-stu-id="a6f2a-130">DateTime</span></span> | <span data-ttu-id="a6f2a-131">False</span><span class="sxs-lookup"><span data-stu-id="a6f2a-131">False</span></span> |


<span data-ttu-id="a6f2a-132">Aby móc przedstawić w tabeli filmów w naszej aplikacji ASP.NET MVC, należy utworzyć klasę modelu.</span><span class="sxs-lookup"><span data-stu-id="a6f2a-132">In order to represent the Movies table in our ASP.NET MVC application, we need to create a model class.</span></span> <span data-ttu-id="a6f2a-133">W tym samouczku używamy Microsoft Entity Framework do tworzenia klas w naszym modelu.</span><span class="sxs-lookup"><span data-stu-id="a6f2a-133">In this tutorial, we use the Microsoft Entity Framework to create our model classes.</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="a6f2a-134">W tym samouczku używamy Microsoft Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="a6f2a-134">In this tutorial, we use the Microsoft Entity Framework.</span></span> <span data-ttu-id="a6f2a-135">Jednak ważne jest zrozumienie, czy można użyć szereg różnych technologie do interakcji z bazą danych z aplikacji ASP.NET MVC, w tym LINQ to SQL i NHibernate, ADO.NET.</span><span class="sxs-lookup"><span data-stu-id="a6f2a-135">However, it is important to understand that you can use a variety of different technologies to interact with a database from an ASP.NET MVC application including LINQ to SQL, NHibernate, or ADO.NET.</span></span>


<span data-ttu-id="a6f2a-136">Wykonaj następujące kroki, aby uruchomić Kreator modelu danych jednostki:</span><span class="sxs-lookup"><span data-stu-id="a6f2a-136">Follow these steps to launch the Entity Data Model Wizard:</span></span>

1. <span data-ttu-id="a6f2a-137">Kliknij prawym przyciskiem myszy folderu modeli w oknie Eksploratora rozwiązań i wybierz opcję menu **Dodaj, nowy element**.</span><span class="sxs-lookup"><span data-stu-id="a6f2a-137">Right-click the Models folder in the Solution Explorer window and the select the menu option **Add, New Item**.</span></span>
2. <span data-ttu-id="a6f2a-138">Wybierz **danych** kategorii, a następnie wybierz **ADO.NET Entity Data Model** szablonu.</span><span class="sxs-lookup"><span data-stu-id="a6f2a-138">Select the **Data** category and select the **ADO.NET Entity Data Model** template.</span></span>
3. <span data-ttu-id="a6f2a-139">Nadaj nazwę modelu danych *MoviesDBModel.edmx* i kliknij przycisk **Dodaj** przycisku.</span><span class="sxs-lookup"><span data-stu-id="a6f2a-139">Give your data model the name *MoviesDBModel.edmx* and click the **Add** button.</span></span>

<span data-ttu-id="a6f2a-140">Po kliknięciu przycisku Dodaj zostanie wyświetlony Kreator modelu Entity Data Model (patrz rysunek 1).</span><span class="sxs-lookup"><span data-stu-id="a6f2a-140">After you click the Add button, the Entity Data Model Wizard appears (see Figure 1).</span></span> <span data-ttu-id="a6f2a-141">Wykonaj następujące kroki, aby zakończyć działanie kreatora:</span><span class="sxs-lookup"><span data-stu-id="a6f2a-141">Follow these steps to complete the wizard:</span></span>

1. <span data-ttu-id="a6f2a-142">W **wybierz zawartość modelu** kroku, wybierz pozycję **Generuj z bazy danych** opcji.</span><span class="sxs-lookup"><span data-stu-id="a6f2a-142">In the **Choose Model Contents** step, select the **Generate from database** option.</span></span>
2. <span data-ttu-id="a6f2a-143">W **wybierz połączenie danych** kroku, należy użyć *MoviesDB.mdf* połączenia danych i nazwę *MoviesDBEntities* dla ustawień połączenia.</span><span class="sxs-lookup"><span data-stu-id="a6f2a-143">In the **Choose Your Data Connection** step, use the *MoviesDB.mdf* data connection and the name *MoviesDBEntities* for the connection settings.</span></span> <span data-ttu-id="a6f2a-144">Kliknij przycisk **dalej** przycisku.</span><span class="sxs-lookup"><span data-stu-id="a6f2a-144">Click the **Next** button.</span></span>
3. <span data-ttu-id="a6f2a-145">W **wybierz obiekty bazy danych** kroku, rozwiń węzeł tabele, wybierz tabelę filmów.</span><span class="sxs-lookup"><span data-stu-id="a6f2a-145">In the **Choose Your Database Objects** step, expand the Tables node, select the Movies table.</span></span> <span data-ttu-id="a6f2a-146">Wprowadź przestrzeń nazw *modeli* i kliknij przycisk **Zakończ** przycisku.</span><span class="sxs-lookup"><span data-stu-id="a6f2a-146">Enter the namespace *Models* and click the **Finish** button.</span></span>


<span data-ttu-id="a6f2a-147">[![Tworzenie zapytań LINQ do klas SQL](displaying-a-table-of-database-data-vb/_static/image1.jpg)](displaying-a-table-of-database-data-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="a6f2a-147">[![Creating LINQ to SQL classes](displaying-a-table-of-database-data-vb/_static/image1.jpg)](displaying-a-table-of-database-data-vb/_static/image1.png)</span></span>

<span data-ttu-id="a6f2a-148">**Rysunek 01**: Tworzenie klasy programu LINQ to SQL ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](displaying-a-table-of-database-data-vb/_static/image2.png))</span><span class="sxs-lookup"><span data-stu-id="a6f2a-148">**Figure 01**: Creating LINQ to SQL classes ([Click to view full-size image](displaying-a-table-of-database-data-vb/_static/image2.png))</span></span>


<span data-ttu-id="a6f2a-149">Po zakończeniu działania Kreator modelu Entity Data Model, zostanie otwarty projektant modelu danych jednostki.</span><span class="sxs-lookup"><span data-stu-id="a6f2a-149">After you complete the Entity Data Model Wizard, the Entity Data Model Designer opens.</span></span> <span data-ttu-id="a6f2a-150">Projektant powinien być wyświetlany jednostki filmy (patrz rysunek 2).</span><span class="sxs-lookup"><span data-stu-id="a6f2a-150">The Designer should display the Movies entity (see Figure 2).</span></span>


<span data-ttu-id="a6f2a-151">[![Projektant modelu danych jednostki](displaying-a-table-of-database-data-vb/_static/image2.jpg)](displaying-a-table-of-database-data-vb/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="a6f2a-151">[![The Entity Data Model Designer](displaying-a-table-of-database-data-vb/_static/image2.jpg)](displaying-a-table-of-database-data-vb/_static/image3.png)</span></span>

<span data-ttu-id="a6f2a-152">**Rysunek 02**: projektancie Entity Data Model ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](displaying-a-table-of-database-data-vb/_static/image4.png))</span><span class="sxs-lookup"><span data-stu-id="a6f2a-152">**Figure 02**: The Entity Data Model Designer ([Click to view full-size image](displaying-a-table-of-database-data-vb/_static/image4.png))</span></span>


<span data-ttu-id="a6f2a-153">Należy wprowadzić zmianę jeden, przed kontynuowaniem.</span><span class="sxs-lookup"><span data-stu-id="a6f2a-153">We need to make one change before we continue.</span></span> <span data-ttu-id="a6f2a-154">Kreator modelu Entity Data generuje klasę modelu o nazwie *filmy* reprezentujący tabelę bazy danych filmów.</span><span class="sxs-lookup"><span data-stu-id="a6f2a-154">The Entity Data Wizard generates a model class named *Movies* that represents the Movies database table.</span></span> <span data-ttu-id="a6f2a-155">Ponieważ będziemy korzystać do reprezentowania filmu konkretnej klasy filmy, należy zmodyfikować nazwę klasy, która ma być *filmu* zamiast *filmy* (pojedynczej zamiast liczba mnoga).</span><span class="sxs-lookup"><span data-stu-id="a6f2a-155">Because we'll use the Movies class to represent a particular movie, we need to modify the name of the class to be *Movie* instead of *Movies* (singular rather than plural).</span></span>

<span data-ttu-id="a6f2a-156">Kliknij dwukrotnie nazwę klasy na powierzchni projektowej i Zmień nazwę klasy z filmy filmu.</span><span class="sxs-lookup"><span data-stu-id="a6f2a-156">Double-click the name of the class on the designer surface and change the name of the class from Movies to Movie.</span></span> <span data-ttu-id="a6f2a-157">Po wprowadzeniu tej zmiany, kliknij przycisk **Zapisz** przycisk (ikona dysku), aby wygenerować klasę filmu.</span><span class="sxs-lookup"><span data-stu-id="a6f2a-157">After making this change, click the **Save** button (the icon of the floppy disk) to generate the Movie class.</span></span>

## <a name="create-the-movies-controller"></a><span data-ttu-id="a6f2a-158">Tworzenie kontrolera filmy</span><span class="sxs-lookup"><span data-stu-id="a6f2a-158">Create the Movies Controller</span></span>

<span data-ttu-id="a6f2a-159">Teraz, gdy mamy już sposobem reprezentowania naszych danych bazy danych, możemy utworzyć kontroler, który zwraca kolekcję filmów.</span><span class="sxs-lookup"><span data-stu-id="a6f2a-159">Now that we have a way to represent our database records, we can create a controller that returns the collection of movies.</span></span> <span data-ttu-id="a6f2a-160">W oknie Eksploratora rozwiązań w usłudze Visual Studio kliknij prawym przyciskiem myszy folder kontrolerów, a następnie wybierz opcję menu **Dodaj, kontroler** (zobacz rysunek 3).</span><span class="sxs-lookup"><span data-stu-id="a6f2a-160">Within the Visual Studio Solution Explorer window, right-click the Controllers folder and select the menu option **Add, Controller** (see Figure 3).</span></span>


<span data-ttu-id="a6f2a-161">[![Dodawanie kontrolera Menu](displaying-a-table-of-database-data-vb/_static/image3.jpg)](displaying-a-table-of-database-data-vb/_static/image5.png)</span><span class="sxs-lookup"><span data-stu-id="a6f2a-161">[![The Add Controller Menu](displaying-a-table-of-database-data-vb/_static/image3.jpg)](displaying-a-table-of-database-data-vb/_static/image5.png)</span></span>

<span data-ttu-id="a6f2a-162">**Rysunek 03**: Dodawanie kontrolera Menu ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](displaying-a-table-of-database-data-vb/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="a6f2a-162">**Figure 03**: The Add Controller Menu ([Click to view full-size image](displaying-a-table-of-database-data-vb/_static/image6.png))</span></span>


<span data-ttu-id="a6f2a-163">Gdy **Dodaj kontroler** zostanie wyświetlone okno dialogowe, wprowadź nazwę kontrolera MovieController (zobacz rysunek 4).</span><span class="sxs-lookup"><span data-stu-id="a6f2a-163">When the **Add Controller** dialog appears, enter the controller name MovieController (see Figure 4).</span></span> <span data-ttu-id="a6f2a-164">Kliknij przycisk **Dodaj** przycisk, aby dodać nowy kontroler.</span><span class="sxs-lookup"><span data-stu-id="a6f2a-164">Click the **Add** button to add the new controller.</span></span>


<span data-ttu-id="a6f2a-165">[![Okno dialogowe Dodawanie kontrolera](displaying-a-table-of-database-data-vb/_static/image4.jpg)](displaying-a-table-of-database-data-vb/_static/image7.png)</span><span class="sxs-lookup"><span data-stu-id="a6f2a-165">[![The Add Controller dialog](displaying-a-table-of-database-data-vb/_static/image4.jpg)](displaying-a-table-of-database-data-vb/_static/image7.png)</span></span>

<span data-ttu-id="a6f2a-166">**Rysunek 04**: okno dialogowe Dodawanie kontrolera ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](displaying-a-table-of-database-data-vb/_static/image8.png))</span><span class="sxs-lookup"><span data-stu-id="a6f2a-166">**Figure 04**: The Add Controller dialog([Click to view full-size image](displaying-a-table-of-database-data-vb/_static/image8.png))</span></span>


<span data-ttu-id="a6f2a-167">Należy zmodyfikować akcję indeks() udostępnianych przez kontroler filmu, tak aby zwraca zestaw rekordów bazy danych.</span><span class="sxs-lookup"><span data-stu-id="a6f2a-167">We need to modify the Index() action exposed by the Movie controller so that it returns the set of database records.</span></span> <span data-ttu-id="a6f2a-168">Tak, aby wyglądało kontrolera w ofercie 1, należy zmodyfikować kontrolera.</span><span class="sxs-lookup"><span data-stu-id="a6f2a-168">Modify the controller so that it looks like the controller in Listing 1.</span></span>

<span data-ttu-id="a6f2a-169">**Wyświetlanie listy 1 – Controllers\MovieController.vb**</span><span class="sxs-lookup"><span data-stu-id="a6f2a-169">**Listing 1 – Controllers\MovieController.vb**</span></span>

[!code-vb[Main](displaying-a-table-of-database-data-vb/samples/sample1.vb)]

<span data-ttu-id="a6f2a-170">W ofercie 1 klasa MoviesDBEntities jest używana do reprezentowania MoviesDB bazy danych.</span><span class="sxs-lookup"><span data-stu-id="a6f2a-170">In Listing 1, the MoviesDBEntities class is used to represent the MoviesDB database.</span></span> <span data-ttu-id="a6f2a-171">Wyrażenie *jednostek. MovieSet.ToList()* zwraca zestaw wszystkich filmów z tabeli bazy danych filmów.</span><span class="sxs-lookup"><span data-stu-id="a6f2a-171">The expression *entities.MovieSet.ToList()* returns the set of all movies from the Movies database table.</span></span>

## <a name="create-the-view"></a><span data-ttu-id="a6f2a-172">Tworzenie widoku</span><span class="sxs-lookup"><span data-stu-id="a6f2a-172">Create the View</span></span>

<span data-ttu-id="a6f2a-173">Najprostszym sposobem wyświetlania zestawu rekordów bazy danych w tabeli HTML jest korzystanie z zalet tworzenia szkieletów udostępniane przez program Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="a6f2a-173">The easiest way to display a set of database records in an HTML table is to take advantage of the scaffolding provided by Visual Studio.</span></span>

<span data-ttu-id="a6f2a-174">Kompiluj aplikację, wybierając opcję menu **twórz, Kompiluj rozwiązanie**.</span><span class="sxs-lookup"><span data-stu-id="a6f2a-174">Build your application by selecting the menu option **Build, Build Solution**.</span></span> <span data-ttu-id="a6f2a-175">Należy skompilować aplikację przed otwarciem **Dodaj widok** okna dialogowego lub klas usługi danych nie będzie wyświetlane w oknie dialogowym.</span><span class="sxs-lookup"><span data-stu-id="a6f2a-175">You must build your application before opening the **Add View** dialog or your data classes won't appear in the dialog.</span></span>

<span data-ttu-id="a6f2a-176">Kliknij prawym przyciskiem myszy działanie indeks() i wybierz opcję menu **Dodaj widok** (zobacz rysunek 5).</span><span class="sxs-lookup"><span data-stu-id="a6f2a-176">Right-click the Index() action and select the menu option **Add View** (see Figure 5).</span></span>


<span data-ttu-id="a6f2a-177">[![Dodawanie widoku](displaying-a-table-of-database-data-vb/_static/image5.jpg)](displaying-a-table-of-database-data-vb/_static/image9.png)</span><span class="sxs-lookup"><span data-stu-id="a6f2a-177">[![Adding a view](displaying-a-table-of-database-data-vb/_static/image5.jpg)](displaying-a-table-of-database-data-vb/_static/image9.png)</span></span>

<span data-ttu-id="a6f2a-178">**Rysunek 05**: Dodawanie widoku ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](displaying-a-table-of-database-data-vb/_static/image10.png))</span><span class="sxs-lookup"><span data-stu-id="a6f2a-178">**Figure 05**: Adding a view ([Click to view full-size image](displaying-a-table-of-database-data-vb/_static/image10.png))</span></span>


<span data-ttu-id="a6f2a-179">W **Dodaj widok** okno dialogowe, zaznacz pola wyboru **utworzyć widok silnie typizowane**.</span><span class="sxs-lookup"><span data-stu-id="a6f2a-179">In the **Add View** dialog, check the checkbox labeled **Create a strongly-typed view**.</span></span> <span data-ttu-id="a6f2a-180">Wybierz klasę filmu jako **wyświetlić klasy danych**.</span><span class="sxs-lookup"><span data-stu-id="a6f2a-180">Select the Movie class as the **view data class**.</span></span> <span data-ttu-id="a6f2a-181">Wybierz *listy* jako **wyświetlanie zawartości** (patrz rysunek 6).</span><span class="sxs-lookup"><span data-stu-id="a6f2a-181">Select *List* as the **view content** (see Figure 6).</span></span> <span data-ttu-id="a6f2a-182">Zaznaczenie tych opcji spowoduje wygenerowanie silnie typizowane widoku, który wyświetla listę filmów.</span><span class="sxs-lookup"><span data-stu-id="a6f2a-182">Selecting these options will generate a strongly-typed view that displays a list of movies.</span></span>


<span data-ttu-id="a6f2a-183">[![Okno dialogowe dodawania widoku](displaying-a-table-of-database-data-vb/_static/image6.jpg)](displaying-a-table-of-database-data-vb/_static/image11.png)</span><span class="sxs-lookup"><span data-stu-id="a6f2a-183">[![The Add View dialog](displaying-a-table-of-database-data-vb/_static/image6.jpg)](displaying-a-table-of-database-data-vb/_static/image11.png)</span></span>

<span data-ttu-id="a6f2a-184">**Rysunek 06**: okno dialogowe Dodawanie widoku ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](displaying-a-table-of-database-data-vb/_static/image12.png))</span><span class="sxs-lookup"><span data-stu-id="a6f2a-184">**Figure 06**: The Add View dialog([Click to view full-size image](displaying-a-table-of-database-data-vb/_static/image12.png))</span></span>


<span data-ttu-id="a6f2a-185">Po kliknięciu **Dodaj** przycisk, widok w ofercie 2 jest generowany automatycznie.</span><span class="sxs-lookup"><span data-stu-id="a6f2a-185">After you click the **Add** button, the view in Listing 2 is generated automatically.</span></span> <span data-ttu-id="a6f2a-186">Ten widok zawiera kod wymagany do iterowania po kolekcji filmów i wyświetlane poszczególne właściwości filmu.</span><span class="sxs-lookup"><span data-stu-id="a6f2a-186">This view contains the code required to iterate through the collection of movies and display each of the properties of a movie.</span></span>

<span data-ttu-id="a6f2a-187">**Wyświetlanie listy 2 — Views\Movie\Index.aspx**</span><span class="sxs-lookup"><span data-stu-id="a6f2a-187">**Listing 2 – Views\Movie\Index.aspx**</span></span>

[!code-aspx[Main](displaying-a-table-of-database-data-vb/samples/sample2.aspx)]

<span data-ttu-id="a6f2a-188">Możesz uruchomić aplikację, wybierając opcję menu **debugowania i Rozpocznij debugowanie** (lub naciskając klawisz F5).</span><span class="sxs-lookup"><span data-stu-id="a6f2a-188">You can run the application by selecting the menu option **Debug, Start Debugging** (or hitting the F5 key).</span></span> <span data-ttu-id="a6f2a-189">Uruchomiona jest aplikacja uruchomi program Internet Explorer.</span><span class="sxs-lookup"><span data-stu-id="a6f2a-189">Running the application launches Internet Explorer.</span></span> <span data-ttu-id="a6f2a-190">Jeśli przejdziesz do adresu URL /Movie, a następnie zobaczysz stronę na rysunku 7.</span><span class="sxs-lookup"><span data-stu-id="a6f2a-190">If you navigate to the /Movie URL then you'll see the page in Figure 7.</span></span>


<span data-ttu-id="a6f2a-191">[![Tabelę filmy](displaying-a-table-of-database-data-vb/_static/image7.jpg)](displaying-a-table-of-database-data-vb/_static/image13.png)</span><span class="sxs-lookup"><span data-stu-id="a6f2a-191">[![A table of movies](displaying-a-table-of-database-data-vb/_static/image7.jpg)](displaying-a-table-of-database-data-vb/_static/image13.png)</span></span>

<span data-ttu-id="a6f2a-192">**Rysunek 07**: tabelę filmy ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](displaying-a-table-of-database-data-vb/_static/image14.png))</span><span class="sxs-lookup"><span data-stu-id="a6f2a-192">**Figure 07**: A table of movies([Click to view full-size image](displaying-a-table-of-database-data-vb/_static/image14.png))</span></span>


<span data-ttu-id="a6f2a-193">Jeśli nie potrzebujesz nic o wyglądzie siatki rekordów bazy danych na rysunku 7 można po prostu zmodyfikuj widok indeksu.</span><span class="sxs-lookup"><span data-stu-id="a6f2a-193">If you don't like anything about the appearance of the grid of database records in Figure 7 then you can simply modify the Index view.</span></span> <span data-ttu-id="a6f2a-194">Na przykład można zmienić *DateReleased* nagłówka do *Data wydania* przez zmodyfikowanie widoku indeksu.</span><span class="sxs-lookup"><span data-stu-id="a6f2a-194">For example, you can change the *DateReleased* header to *Date Released* by modifying the Index view.</span></span>

## <a name="create-a-template-with-a-partial"></a><span data-ttu-id="a6f2a-195">Tworzenie szablonu z częściowym</span><span class="sxs-lookup"><span data-stu-id="a6f2a-195">Create a Template with a Partial</span></span>

<span data-ttu-id="a6f2a-196">Jeśli widok pobiera zbyt skomplikowane, jest dobry pomysł, aby uruchomić podzielenie widoku na częściowych.</span><span class="sxs-lookup"><span data-stu-id="a6f2a-196">When a view gets too complicated, it is a good idea to start breaking the view into partials.</span></span> <span data-ttu-id="a6f2a-197">Za pomocą częściowych ułatwia widoków do zrozumienia i utrzymania.</span><span class="sxs-lookup"><span data-stu-id="a6f2a-197">Using partials makes your views easier to understand and maintain.</span></span> <span data-ttu-id="a6f2a-198">Utworzymy częściowym, możemy użyć jako szablonu, aby sformatować listę rekordów bazy danych filmów.</span><span class="sxs-lookup"><span data-stu-id="a6f2a-198">We'll create a partial that we can use as a template to format each of the movie database records.</span></span>

<span data-ttu-id="a6f2a-199">Wykonaj następujące kroki, aby utworzyć częściowego:</span><span class="sxs-lookup"><span data-stu-id="a6f2a-199">Follow these steps to create the partial:</span></span>

1. <span data-ttu-id="a6f2a-200">Kliknij prawym przyciskiem myszy Views\Movie folder i wybierz opcję menu **Dodaj widok**.</span><span class="sxs-lookup"><span data-stu-id="a6f2a-200">Right-click the Views\Movie folder and select the menu option **Add View**.</span></span>
2. <span data-ttu-id="a6f2a-201">Zaznacz pola wyboru *tworzenia widoku częściowego (.ascx)*.</span><span class="sxs-lookup"><span data-stu-id="a6f2a-201">Check the checkbox labeled *Create a partial view (.ascx)*.</span></span>
3. <span data-ttu-id="a6f2a-202">Nadaj nazwę częściowego *MovieTemplate*.</span><span class="sxs-lookup"><span data-stu-id="a6f2a-202">Name the partial *MovieTemplate*.</span></span>
4. <span data-ttu-id="a6f2a-203">Zaznacz pola wyboru **utworzyć widok silnie typizowane**.</span><span class="sxs-lookup"><span data-stu-id="a6f2a-203">Check the checkbox labeled **Create a strongly-typed view**.</span></span>
5. <span data-ttu-id="a6f2a-204">Wybierz filmu jako *wyświetlić klasy danych*.</span><span class="sxs-lookup"><span data-stu-id="a6f2a-204">Select Movie as the *view data class*.</span></span>
6. <span data-ttu-id="a6f2a-205">Wybierz pusty jako *wyświetlanie zawartości*.</span><span class="sxs-lookup"><span data-stu-id="a6f2a-205">Select Empty as the *view content*.</span></span>
7. <span data-ttu-id="a6f2a-206">Kliknij przycisk **Dodaj** przycisk, aby dodać części do projektu.</span><span class="sxs-lookup"><span data-stu-id="a6f2a-206">Click the **Add** button to add the partial to your project.</span></span>

<span data-ttu-id="a6f2a-207">Po wykonaniu tych kroków, należy zmodyfikować MovieTemplate częściowe wyglądać lista 3.</span><span class="sxs-lookup"><span data-stu-id="a6f2a-207">After you complete these steps, modify the MovieTemplate partial to look like Listing 3.</span></span>

<span data-ttu-id="a6f2a-208">**Wyświetlanie listy 3 — Views\Movie\MovieTemplate.ascx**</span><span class="sxs-lookup"><span data-stu-id="a6f2a-208">**Listing 3 – Views\Movie\MovieTemplate.ascx**</span></span>

[!code-aspx[Main](displaying-a-table-of-database-data-vb/samples/sample3.aspx)]

<span data-ttu-id="a6f2a-209">Partial w ofercie 3 zawiera szablon w pojedynczym wierszu rekordów.</span><span class="sxs-lookup"><span data-stu-id="a6f2a-209">The partial in Listing 3 contains a template for a single row of records.</span></span>

<span data-ttu-id="a6f2a-210">Zmodyfikowany widok indeksu w ofercie 4 używa MovieTemplate częściowe.</span><span class="sxs-lookup"><span data-stu-id="a6f2a-210">The modified Index view in Listing 4 uses the MovieTemplate partial.</span></span>

<span data-ttu-id="a6f2a-211">**Wyświetlanie listy 4 — Views\Movie\Index.aspx**</span><span class="sxs-lookup"><span data-stu-id="a6f2a-211">**Listing 4 – Views\Movie\Index.aspx**</span></span>

[!code-aspx[Main](displaying-a-table-of-database-data-vb/samples/sample4.aspx)]

<span data-ttu-id="a6f2a-212">Widok w ofercie 4 zawiera dla każdej pętli, który iteruje po wszystkich filmów.</span><span class="sxs-lookup"><span data-stu-id="a6f2a-212">The view in Listing 4 contains a For Each loop that iterates through all of the movies.</span></span> <span data-ttu-id="a6f2a-213">Dla każdego filmu częściowe MovieTemplate jest używany do formatowania filmu.</span><span class="sxs-lookup"><span data-stu-id="a6f2a-213">For each movie, the MovieTemplate partial is used to format the movie.</span></span> <span data-ttu-id="a6f2a-214">MovieTemplate jest renderowany przez wywołanie metody pomocnika RenderPartial().</span><span class="sxs-lookup"><span data-stu-id="a6f2a-214">The MovieTemplate is rendered by calling the RenderPartial() helper method.</span></span>

<span data-ttu-id="a6f2a-215">Zmodyfikowany widok indeksu powoduje wyświetlenie tej samej tabeli HTML rekordów bazy danych.</span><span class="sxs-lookup"><span data-stu-id="a6f2a-215">The modified Index view renders the very same HTML table of database records.</span></span> <span data-ttu-id="a6f2a-216">Jednak widok został znacznie uproszczony.</span><span class="sxs-lookup"><span data-stu-id="a6f2a-216">However, the view has been greatly simplified.</span></span>


<span data-ttu-id="a6f2a-217">Metoda RenderPartial() jest inne niż większość pozostałych metod pomocniczych, ponieważ nie zwraca ciąg.</span><span class="sxs-lookup"><span data-stu-id="a6f2a-217">The RenderPartial() method is different than most of the other helper methods because it does not return a string.</span></span> <span data-ttu-id="a6f2a-218">W związku z tym, należy wywołać przy użyciu metody RenderPartial() &lt;Html.RenderPartial() %&gt; zamiast &lt;% = Html.RenderPartial() %&gt;.</span><span class="sxs-lookup"><span data-stu-id="a6f2a-218">Therefore, you must call the RenderPartial() method using &lt;% Html.RenderPartial() %&gt; instead of &lt;%= Html.RenderPartial() %&gt;.</span></span>


## <a name="summary"></a><span data-ttu-id="a6f2a-219">Podsumowanie</span><span class="sxs-lookup"><span data-stu-id="a6f2a-219">Summary</span></span>

<span data-ttu-id="a6f2a-220">Celem tego samouczka było wyjaśniają, jak można wyświetlić zestaw rekordów bazy danych w tabeli HTML.</span><span class="sxs-lookup"><span data-stu-id="a6f2a-220">The goal of this tutorial was to explain how you can display a set of database records in an HTML table.</span></span> <span data-ttu-id="a6f2a-221">Po pierwsze wiesz, jak można zwrócić zestaw rekordów bazy danych w ramach akcji kontrolera, wykorzystując Microsoft Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="a6f2a-221">First, you learned how to return a set of database records from a controller action by taking advantage of the Microsoft Entity Framework.</span></span> <span data-ttu-id="a6f2a-222">Następnie omówiono na potrzeby tworzenia szkieletów programu Visual Studio wygenerowania widoku, który automatycznie wyświetla zbiór elementów.</span><span class="sxs-lookup"><span data-stu-id="a6f2a-222">Next, you learned how to use Visual Studio scaffolding to generate a view that displays a collection of items automatically.</span></span> <span data-ttu-id="a6f2a-223">Na koniec pokazaliśmy ci, jak uprościć widok, korzystając z częściowego.</span><span class="sxs-lookup"><span data-stu-id="a6f2a-223">Finally, you learned how to simplify the view by taking advantage of a partial.</span></span> <span data-ttu-id="a6f2a-224">Przedstawiono sposób użycia częściowym jako szablon, dzięki czemu można formatować każdy rekord bazy danych.</span><span class="sxs-lookup"><span data-stu-id="a6f2a-224">You learned how to use a partial as a template so that you can format each database record.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="a6f2a-225">[Poprzednie](creating-model-classes-with-linq-to-sql-vb.md)
> [dalej](performing-simple-validation-vb.md)</span><span class="sxs-lookup"><span data-stu-id="a6f2a-225">[Previous](creating-model-classes-with-linq-to-sql-vb.md)
[Next](performing-simple-validation-vb.md)</span></span>
