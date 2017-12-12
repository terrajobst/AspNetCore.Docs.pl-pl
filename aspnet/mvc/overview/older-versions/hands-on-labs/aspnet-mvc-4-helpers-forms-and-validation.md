---
uid: mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-helpers-forms-and-validation
title: "Pomocników platformy ASP.NET MVC 4, formularzy i sprawdzania poprawności | Dokumentacja firmy Microsoft"
author: rick-anderson
description: "ASP.NET MVC 4 modeli i laboratorium Hands-on dostępu do danych możesz zostały ładowanie i wyświetlanie danych z bazy danych. W tym laboratorium Hands-on doda do..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2013
ms.topic: article
ms.assetid: 187ee9cd-bc70-479b-bfed-f568b8da96eb
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-helpers-forms-and-validation
msc.type: authoredcontent
ms.openlocfilehash: 4dd10430778dc51fef1199315ee02eb2cd4970ba
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
<a name="aspnet-mvc-4-helpers-forms-and-validation"></a><span data-ttu-id="5e9d0-104">Pomocników platformy ASP.NET MVC 4, formularzy i sprawdzania poprawności</span><span class="sxs-lookup"><span data-stu-id="5e9d0-104">ASP.NET MVC 4 Helpers, Forms and Validation</span></span>
====================
<span data-ttu-id="5e9d0-105">przez [obozów sieci Web Team](https://twitter.com/webcamps)</span><span class="sxs-lookup"><span data-stu-id="5e9d0-105">by [Web Camps Team](https://twitter.com/webcamps)</span></span>

> <span data-ttu-id="5e9d0-106">W **ASP.NET MVC 4 modeli i dostęp do danych** laboratorium Hands-on nastąpiło ładowanie i wyświetlanie danych z bazy danych.</span><span class="sxs-lookup"><span data-stu-id="5e9d0-106">In **ASP.NET MVC 4 Models and Data Access** Hands-on Lab, you have been loading and displaying data from the database.</span></span> <span data-ttu-id="5e9d0-107">W tym laboratorium Hands-on doda do **sklep muzyczny** aplikacji możliwość edytowania tych danych.</span><span class="sxs-lookup"><span data-stu-id="5e9d0-107">In this Hands-on Lab, you will add to the **Music Store** application the ability to edit that data.</span></span>
> 
> <span data-ttu-id="5e9d0-108">Tego celu pamiętać najpierw utworzysz kontroler, który będzie obsługiwać albumy akcje tworzenia, odczytu, aktualizacji i usuwania (CRUD).</span><span class="sxs-lookup"><span data-stu-id="5e9d0-108">With that goal in mind, you will first create the controller that will support the Create, Read, Update and Delete (CRUD) actions of albums.</span></span> <span data-ttu-id="5e9d0-109">Wygeneruje szablonu widoku indeksu, korzystając z funkcji szkieletów ASP.NET MVC do wyświetlania właściwości albumów w tabeli HTML.</span><span class="sxs-lookup"><span data-stu-id="5e9d0-109">You will generate an Index View template taking advantage of ASP.NET MVC's scaffolding feature to display the albums' properties in an HTML table.</span></span> <span data-ttu-id="5e9d0-110">Aby zwiększyć ten widok, zostaną dodane niestandardowe pomocnika kodu HTML, który obetnie długie opisy.</span><span class="sxs-lookup"><span data-stu-id="5e9d0-110">To enhance that view, you will add a custom HTML helper that will truncate long descriptions.</span></span>
> 
> <span data-ttu-id="5e9d0-111">Następnie możesz spowoduje to dodanie edycji i Utwórz widoki, które pozwoli instrukcja alter albumów w bazie danych za pomocą formularza elementy, takie jak listę rozwijaną.</span><span class="sxs-lookup"><span data-stu-id="5e9d0-111">Afterwards, you will add the Edit and Create Views that will let you alter the albums in the database, with the help of form elements like dropdowns.</span></span>
> 
> <span data-ttu-id="5e9d0-112">Ponadto umożliwi użytkownikowi usunięcie albumu i również uniemożliwi ich wprowadzania danych niewłaściwy przez sprawdzanie poprawności danych wejściowych.</span><span class="sxs-lookup"><span data-stu-id="5e9d0-112">Lastly, you will let users delete an album and also you will prevent them from entering wrong data by validating their input.</span></span>
> 
> > [!NOTE]
> > <span data-ttu-id="5e9d0-113">W tym laboratorium Hands-on zakłada mieć podstawową wiedzę na temat **ASP.NET MVC**.</span><span class="sxs-lookup"><span data-stu-id="5e9d0-113">This Hands-on Lab assumes you have basic knowledge of **ASP.NET MVC**.</span></span> <span data-ttu-id="5e9d0-114">Jeśli nie używasz **ASP.NET MVC** przed, zalecamy zapoznać się z **podstawowe informacje na temat platformy ASP.NET MVC** Hands-on laboratorium.</span><span class="sxs-lookup"><span data-stu-id="5e9d0-114">If you have not used **ASP.NET MVC** before, we recommend you to go over **ASP.NET MVC Fundamentals** Hands-on Lab.</span></span>
> 
> 
> <span data-ttu-id="5e9d0-115">W tym laboratorium przeprowadzi Cię przez rozszerzenia oraz nowe funkcje opisane wcześniej przez zastosowanie drobne zmiany do przykładowej aplikacji sieci Web w folderze źródłowym.</span><span class="sxs-lookup"><span data-stu-id="5e9d0-115">This lab walks you through the enhancements and new features previously described by applying minor changes to a sample Web application provided in the Source folder.</span></span>
> 
> <span data-ttu-id="5e9d0-116">Wszystkie przykładowy kod i fragmenty kodu są uwzględnione w sieci Web obozów zestaw szkoleniowy, dostępne pod adresem [https://www.microsoft.com/en-us/download/29843](https://www.microsoft.com/en-us/download/29843).</span><span class="sxs-lookup"><span data-stu-id="5e9d0-116">All sample code and snippets are included in the Web Camps Training Kit, available at [https://www.microsoft.com/en-us/download/29843](https://www.microsoft.com/en-us/download/29843).</span></span>


<a id="Objectives"></a>

<a id="Objectives"></a>
### <a name="objectives"></a><span data-ttu-id="5e9d0-117">Cele</span><span class="sxs-lookup"><span data-stu-id="5e9d0-117">Objectives</span></span>

<span data-ttu-id="5e9d0-118">W tym laboratorium Hands-On przedstawiono sposób:</span><span class="sxs-lookup"><span data-stu-id="5e9d0-118">In this Hands-On Lab, you will learn how to:</span></span>

- <span data-ttu-id="5e9d0-119">Tworzenie kontrolera w celu obsługi operacji CRUD</span><span class="sxs-lookup"><span data-stu-id="5e9d0-119">Create a controller to support CRUD operations</span></span>
- <span data-ttu-id="5e9d0-120">Generowanie widoku indeksu, aby wyświetlić właściwości jednostki tabeli HTML</span><span class="sxs-lookup"><span data-stu-id="5e9d0-120">Generate an Index View to display entity properties in an HTML table</span></span>
- <span data-ttu-id="5e9d0-121">Dodaj niestandardowy pomocnika kodu HTML</span><span class="sxs-lookup"><span data-stu-id="5e9d0-121">Add a custom HTML helper</span></span>
- <span data-ttu-id="5e9d0-122">Tworzenie i dostosowywanie Edytowanie widoku</span><span class="sxs-lookup"><span data-stu-id="5e9d0-122">Create and customize an Edit View</span></span>
- <span data-ttu-id="5e9d0-123">Rozróżnienie metod akcji, które reagują na HTTP GET lub POST protokołu HTTP wywołania</span><span class="sxs-lookup"><span data-stu-id="5e9d0-123">Differentiate between action methods that react to either HTTP-GET or HTTP-POST calls</span></span>
- <span data-ttu-id="5e9d0-124">Dodawanie i dostosowywanie Utwórz widok</span><span class="sxs-lookup"><span data-stu-id="5e9d0-124">Add and customize a Create View</span></span>
- <span data-ttu-id="5e9d0-125">Dojście usunięcie jednostki</span><span class="sxs-lookup"><span data-stu-id="5e9d0-125">Handle the deletion of an entity</span></span>
- <span data-ttu-id="5e9d0-126">Sprawdzanie poprawności danych wejściowych użytkownika</span><span class="sxs-lookup"><span data-stu-id="5e9d0-126">Validate user input</span></span>

<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a><span data-ttu-id="5e9d0-127">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="5e9d0-127">Prerequisites</span></span>

<span data-ttu-id="5e9d0-128">Musi mieć następujące elementy do przygotowania tego laboratorium:</span><span class="sxs-lookup"><span data-stu-id="5e9d0-128">You must have the following items to complete this lab:</span></span>

- <span data-ttu-id="5e9d0-129">[Microsoft Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) lub wyższego poziomu (odczytu [dodatek a.](#AppendixA) instrukcje dotyczące sposobu jego instalacji).</span><span class="sxs-lookup"><span data-stu-id="5e9d0-129">[Microsoft Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) or superior (read [Appendix A](#AppendixA) for instructions on how to install it).</span></span>

<a id="Setup"></a>

<a id="Setup"></a>
### <a name="setup"></a><span data-ttu-id="5e9d0-130">Konfiguracja</span><span class="sxs-lookup"><span data-stu-id="5e9d0-130">Setup</span></span>

<span data-ttu-id="5e9d0-131">**Instalowanie wstawki kodu**</span><span class="sxs-lookup"><span data-stu-id="5e9d0-131">**Installing Code Snippets**</span></span>

<span data-ttu-id="5e9d0-132">Dla wygody taki kod, który będzie zarządzany wzdłuż tego laboratorium jest dostępna jako wstawki kodu programu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="5e9d0-132">For convenience, much of the code you will be managing along this lab is available as Visual Studio code snippets.</span></span> <span data-ttu-id="5e9d0-133">Aby zainstalować fragmenty kodu, uruchom **.\Source\Setup\CodeSnippets.vsi** pliku.</span><span class="sxs-lookup"><span data-stu-id="5e9d0-133">To install the code snippets run **.\Source\Setup\CodeSnippets.vsi** file.</span></span>

<span data-ttu-id="5e9d0-134">Jeśli nie masz doświadczenia z wstawki programu Visual Studio i chcesz dowiedzieć się, jak ich używać, można odwołać się do dodatku z tego dokumentu &quot; [wstawki kodu za pomocą programu dodatek B:](#AppendixB)&quot;.</span><span class="sxs-lookup"><span data-stu-id="5e9d0-134">If you are not familiar with the Visual Studio Code Snippets, and want to learn how to use them, you can refer to the appendix from this document &quot;[Appendix B: Using Code Snippets](#AppendixB)&quot;.</span></span>

* * *

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a><span data-ttu-id="5e9d0-135">Ćwiczenia</span><span class="sxs-lookup"><span data-stu-id="5e9d0-135">Exercises</span></span>

<span data-ttu-id="5e9d0-136">Następujące ćwiczeń tworzą tego laboratorium Hands-On:</span><span class="sxs-lookup"><span data-stu-id="5e9d0-136">The following exercises make up this Hands-On Lab:</span></span>

1. [<span data-ttu-id="5e9d0-137">Tworzenie kontrolera Menedżer magazynu i jego widoku indeksu</span><span class="sxs-lookup"><span data-stu-id="5e9d0-137">Creating the Store Manager controller and its Index view</span></span>](#Exercise1)
2. [<span data-ttu-id="5e9d0-138">Dodawanie pomocnika kodu HTML</span><span class="sxs-lookup"><span data-stu-id="5e9d0-138">Adding an HTML Helper</span></span>](#Exercise2)
3. [<span data-ttu-id="5e9d0-139">Tworzenie widoku edycji</span><span class="sxs-lookup"><span data-stu-id="5e9d0-139">Creating the Edit View</span></span>](#Exercise3)
4. [<span data-ttu-id="5e9d0-140">Dodawanie widoku Create</span><span class="sxs-lookup"><span data-stu-id="5e9d0-140">Adding a Create View</span></span>](#Exercise4)
5. [<span data-ttu-id="5e9d0-141">Obsługa usuwania</span><span class="sxs-lookup"><span data-stu-id="5e9d0-141">Handling Deletion</span></span>](#Exercise5)
6. [<span data-ttu-id="5e9d0-142">Dodawanie walidacji</span><span class="sxs-lookup"><span data-stu-id="5e9d0-142">Adding Validation</span></span>](#Exercise6)
7. [<span data-ttu-id="5e9d0-143">Przy użyciu jQuery dyskretny kod po stronie klienta</span><span class="sxs-lookup"><span data-stu-id="5e9d0-143">Using Unobtrusive jQuery at Client Side</span></span>](#Exercise7)

> [!NOTE]
> <span data-ttu-id="5e9d0-144">Towarzyszy każdego wykonywania **zakończenia** folderu zawierającego wynikowy rozwiązanie, należy uzyskać po wykonaniu ćwiczeniach.</span><span class="sxs-lookup"><span data-stu-id="5e9d0-144">Each exercise is accompanied by an **End** folder containing the resulting solution you should obtain after completing the exercises.</span></span> <span data-ttu-id="5e9d0-145">Jeśli potrzebujesz dodatkowej pomocy pracuje nad ćwiczeniami, można użyć tego rozwiązania jako przewodnika.</span><span class="sxs-lookup"><span data-stu-id="5e9d0-145">You can use this solution as a guide if you need additional help working through the exercises.</span></span>


<span data-ttu-id="5e9d0-146">Szacowany czas trwania tego laboratorium: **60 minut**</span><span class="sxs-lookup"><span data-stu-id="5e9d0-146">Estimated time to complete this lab: **60 minutes**</span></span>

<a id="Exercise1"></a>

<a id="Exercise_1_Creating_the_Store_Manager_controller_and_its_Index_view"></a>
### <a name="exercise-1-creating-the-store-manager-controller-and-its-index-view"></a><span data-ttu-id="5e9d0-147">Ćwiczenie 1: Tworzenie kontrolera Menedżer magazynu i jego widoku indeksu</span><span class="sxs-lookup"><span data-stu-id="5e9d0-147">Exercise 1: Creating the Store Manager controller and its Index view</span></span>

<span data-ttu-id="5e9d0-148">W tym ćwiczeniu dowiesz sposób tworzenia nowego kontrolera w celu obsługi operacji CRUD, można dostosować jego indeksu metody akcji, aby zwrócić listę albumów z bazy danych i na koniec generowania szablon widoku indeksu wykorzystaniu szkieletów ASP.NET MVC Funkcja, aby wyświetlić właściwości albumów w tabeli HTML.</span><span class="sxs-lookup"><span data-stu-id="5e9d0-148">In this exercise, you will learn how to create a new controller to support CRUD operations, customize its Index action method to return a list of albums from the database and finally generating an Index View template taking advantage of ASP.NET MVC's scaffolding feature to display the albums' properties in an HTML table.</span></span>

<a id="Ex1Task1"></a>

<a id="Task_1_-_Creating_the_StoreManagerController"></a>
#### <a name="task-1---creating-the-storemanagercontroller"></a><span data-ttu-id="5e9d0-149">Zadanie 1 — Tworzenie StoreManagerController</span><span class="sxs-lookup"><span data-stu-id="5e9d0-149">Task 1 - Creating the StoreManagerController</span></span>

<span data-ttu-id="5e9d0-150">W ramach tego zadania spowoduje utworzenie nowego kontrolera o nazwie **StoreManagerController** do obsługi operacji CRUD.</span><span class="sxs-lookup"><span data-stu-id="5e9d0-150">In this task, you will create a new controller called **StoreManagerController** to support CRUD operations.</span></span>

1. <span data-ttu-id="5e9d0-151">Otwórz **rozpocząć** rozwiązania, znajdujących się na **źródło/Ex1-CreatingTheStoreManagerController/Begin/** folderu.</span><span class="sxs-lookup"><span data-stu-id="5e9d0-151">Open the **Begin** solution located at **Source/Ex1-CreatingTheStoreManagerController/Begin/** folder.</span></span>

    1. <span data-ttu-id="5e9d0-152">Należy pobrać niektórych brakujących pakietów NuGet aby kontynuować.</span><span class="sxs-lookup"><span data-stu-id="5e9d0-152">You will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="5e9d0-153">Aby to zrobić, kliknij przycisk **projektu** menu i wybierz **Zarządzaj pakietami NuGet**.</span><span class="sxs-lookup"><span data-stu-id="5e9d0-153">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
    2. <span data-ttu-id="5e9d0-154">W **Zarządzaj pakietami NuGet** okna dialogowego, kliknij przycisk **przywrócić** celu pobieranie brakujących pakietów.</span><span class="sxs-lookup"><span data-stu-id="5e9d0-154">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
    3. <span data-ttu-id="5e9d0-155">Na koniec Skompiluj rozwiązanie, klikając **kompilacji** | **Kompiluj rozwiązanie**.</span><span class="sxs-lookup"><span data-stu-id="5e9d0-155">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="5e9d0-156">Jedną z zalet przy użyciu narzędzia NuGet jest, że nie masz do wysłania wszystkich bibliotek w projekcie, zmniejszenie jego rozmiar projektu.</span><span class="sxs-lookup"><span data-stu-id="5e9d0-156">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="5e9d0-157">Narzędzia Power NuGet określając wersje pakietów w pliku Packages.config, będzie można pobrać wymaganych bibliotek podczas pierwszego uruchomienia projektu.</span><span class="sxs-lookup"><span data-stu-id="5e9d0-157">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="5e9d0-158">Jest to, dlaczego konieczne będzie wykonanie tych kroków, po otwarciu istniejącego rozwiązania z tego laboratorium.</span><span class="sxs-lookup"><span data-stu-id="5e9d0-158">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="5e9d0-159">Dodaj nowy kontroler.</span><span class="sxs-lookup"><span data-stu-id="5e9d0-159">Add a new controller.</span></span> <span data-ttu-id="5e9d0-160">Aby to zrobić, kliknij prawym przyciskiem myszy **kontrolerów** folder w Eksploratorze rozwiązań wybierz **Dodaj** , a następnie **kontrolera** polecenia.</span><span class="sxs-lookup"><span data-stu-id="5e9d0-160">To do this, right-click the **Controllers** folder within the Solution Explorer, select **Add** and then the **Controller** command.</span></span> <span data-ttu-id="5e9d0-161">Zmień **kontrolera** **nazwa** do **StoreManagerController** i upewnij się, że opcja **kontroler MVC z akcjami odczytu/zapisu pusty**jest zaznaczone.</span><span class="sxs-lookup"><span data-stu-id="5e9d0-161">Change the **Controller** **Name** to **StoreManagerController** and make sure the option **MVC controller with empty read/write actions** is selected.</span></span> <span data-ttu-id="5e9d0-162">Kliknij przycisk **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="5e9d0-162">Click **Add**.</span></span>

    <span data-ttu-id="5e9d0-163">![Dodaj kontroler w oknie dialogowym](aspnet-mvc-4-helpers-forms-and-validation/_static/image1.png "Dodaj kontroler w oknie dialogowym")</span><span class="sxs-lookup"><span data-stu-id="5e9d0-163">![Add controller dialog](aspnet-mvc-4-helpers-forms-and-validation/_static/image1.png "Add controller dialog")</span></span>

    <span data-ttu-id="5e9d0-164">*Dodawanie kontrolera okna dialogowego*</span><span class="sxs-lookup"><span data-stu-id="5e9d0-164">*Add Controller Dialog*</span></span>

    <span data-ttu-id="5e9d0-165">Nowa klasa kontrolera jest generowany.</span><span class="sxs-lookup"><span data-stu-id="5e9d0-165">A new Controller class is generated.</span></span> <span data-ttu-id="5e9d0-166">Ponieważ wskazane Dodaj akcje dla odczytu/zapisu, metod klasy zastępczej dla osób, typowych operacji CRUD są tworzone z komentarze TODO wypełnione, monitowanie Dołącz logikę określonych aplikacji.</span><span class="sxs-lookup"><span data-stu-id="5e9d0-166">Since you indicated to add actions for read/write, stub methods for those, common CRUD actions are created with TODO comments filled in, prompting to include the application specific logic.</span></span>

<a id="Ex1Task2"></a>

<a id="Task_2_-_Customizing_the_StoreManager_Index"></a>
#### <a name="task-2---customizing-the-storemanager-index"></a><span data-ttu-id="5e9d0-167">Zadanie 2 — dostosowanie indeksu StoreManager</span><span class="sxs-lookup"><span data-stu-id="5e9d0-167">Task 2 - Customizing the StoreManager Index</span></span>

<span data-ttu-id="5e9d0-168">W tym zadaniu zostanie dostosować indeksu StoreManager metody akcji do zwrócenia widoku z listy albumy z bazy danych.</span><span class="sxs-lookup"><span data-stu-id="5e9d0-168">In this task, you will customize the StoreManager Index action method to return a View with the list of albums from the database.</span></span>

1. <span data-ttu-id="5e9d0-169">W klasie StoreManagerController, Dodaj następujący *przy użyciu* dyrektywy.</span><span class="sxs-lookup"><span data-stu-id="5e9d0-169">In the StoreManagerController class, add the following *using* directives.</span></span>

    <span data-ttu-id="5e9d0-170">(Fragment - kodu *ASP.NET MVC 4 pomocników i formularzy i sprawdzanie poprawności — Ex1 przy użyciu MvcMusicStore*)</span><span class="sxs-lookup"><span data-stu-id="5e9d0-170">(Code Snippet - *ASP.NET MVC 4 Helpers and Forms and Validation - Ex1 using MvcMusicStore*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample1.cs)]
2. <span data-ttu-id="5e9d0-171">Dodawanie pola do **StoreManagerController** do przechowywania wystąpienia **MusicStoreEntities.**</span><span class="sxs-lookup"><span data-stu-id="5e9d0-171">Add a field to the **StoreManagerController** to hold an instance of **MusicStoreEntities.**</span></span>

    <span data-ttu-id="5e9d0-172">(Fragment - kodu *pomocników platformy ASP.NET MVC 4 oraz formularzy i sprawdzanie poprawności — Ex1 MusicStoreEntities*)</span><span class="sxs-lookup"><span data-stu-id="5e9d0-172">(Code Snippet - *ASP.NET MVC 4 Helpers and Forms and Validation - Ex1 MusicStoreEntities*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample2.cs)]
3. <span data-ttu-id="5e9d0-173">Implementuje akcji indeksu StoreManagerController do zwrócenia widoku z listy Albumy.</span><span class="sxs-lookup"><span data-stu-id="5e9d0-173">Implement the StoreManagerController Index action to return a View with the list of albums.</span></span>

    <span data-ttu-id="5e9d0-174">Logika akcji kontrolera jest bardzo podobny do akcji indeksu StoreController zapisane wcześniej.</span><span class="sxs-lookup"><span data-stu-id="5e9d0-174">The Controller action logic will be very similar to the StoreController's Index action written earlier.</span></span> <span data-ttu-id="5e9d0-175">Użyj rozszerzenia LINQ, można pobrać wszystkich albumów, w tym informacje o rodzaju i wykonawcy do wyświetlenia.</span><span class="sxs-lookup"><span data-stu-id="5e9d0-175">Use LINQ to retrieve all albums, including Genre and Artist information for display.</span></span>

    <span data-ttu-id="5e9d0-176">(Fragment - kodu *pomocników platformy ASP.NET MVC 4 oraz formularzy i sprawdzanie poprawności — indeks StoreManagerController Ex1*)</span><span class="sxs-lookup"><span data-stu-id="5e9d0-176">(Code Snippet - *ASP.NET MVC 4 Helpers and Forms and Validation - Ex1 StoreManagerController Index*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample3.cs)]

<a id="Ex1Task3"></a>

<a id="strongTask_3_-_Creating_the_Index_Viewstrong"></a>
#### <a name="task-3---creating-the-index-view"></a><span data-ttu-id="5e9d0-177">Zadanie 3 — Tworzenie widoku indeksu</span><span class="sxs-lookup"><span data-stu-id="5e9d0-177">Task 3 - Creating the Index View</span></span>

<span data-ttu-id="5e9d0-178">W ramach tego zadania spowoduje utworzenie szablonu widoku indeksu do wyświetlenia na liście Albumy zwrócony przez **StoreManager** kontrolera.</span><span class="sxs-lookup"><span data-stu-id="5e9d0-178">In this task, you will create the Index View template to display the list of albums returned by the **StoreManager** Controller.</span></span>

1. <span data-ttu-id="5e9d0-179">Przed utworzeniem nowego szablonu widoku, należy skompilować projekt, aby **dialogowe dodawania widoku** zna **albumu** klasy do użycia.</span><span class="sxs-lookup"><span data-stu-id="5e9d0-179">Before creating the new View template, you should build the project so that the **Add View Dialog** knows about the **Album** class to use.</span></span> <span data-ttu-id="5e9d0-180">Wybierz **kompilacji | Tworzenie MvcMusicStore** Aby skompilować projekt.</span><span class="sxs-lookup"><span data-stu-id="5e9d0-180">Select **Build | Build MvcMusicStore** to build the project.</span></span>
2. <span data-ttu-id="5e9d0-181">Kliknij prawym przyciskiem myszy wewnątrz **indeksu** metody akcji i wybierz **Dodaj widok**.</span><span class="sxs-lookup"><span data-stu-id="5e9d0-181">Right-click inside the **Index** action method and select **Add View**.</span></span> <span data-ttu-id="5e9d0-182">Zostanie wyświetlone okno **Dodaj widok** okna dialogowego.</span><span class="sxs-lookup"><span data-stu-id="5e9d0-182">This will bring up the **Add View** dialog.</span></span>

    <span data-ttu-id="5e9d0-183">![Dodaj widok](aspnet-mvc-4-helpers-forms-and-validation/_static/image2.png "Dodawanie widoku")</span><span class="sxs-lookup"><span data-stu-id="5e9d0-183">![Add view](aspnet-mvc-4-helpers-forms-and-validation/_static/image2.png "Add view")</span></span>

    <span data-ttu-id="5e9d0-184">*Dodawanie widoku z wewnątrz Index — metoda*</span><span class="sxs-lookup"><span data-stu-id="5e9d0-184">*Adding a View from within the Index method*</span></span>
3. <span data-ttu-id="5e9d0-185">W oknie dialogowym Dodaj widok, sprawdź, czy nazwa widoku jest **indeksu**.</span><span class="sxs-lookup"><span data-stu-id="5e9d0-185">In the Add View dialog, verify that the View Name is **Index**.</span></span> <span data-ttu-id="5e9d0-186">Wybierz **utworzyć widok jednoznacznie** i wybrać **albumu (MvcMusicStore.Models)** z **klasa modelu** listy rozwijanej.</span><span class="sxs-lookup"><span data-stu-id="5e9d0-186">Select the **Create a strongly-typed view** option, and select **Album (MvcMusicStore.Models)** from the **Model class** drop-down.</span></span> <span data-ttu-id="5e9d0-187">Wybierz **listy** z **szablonu szkieletu** listy rozwijanej.</span><span class="sxs-lookup"><span data-stu-id="5e9d0-187">Select **List** from the **Scaffold template** drop-down.</span></span> <span data-ttu-id="5e9d0-188">Pozostaw **aparat widoku** do **Razor** i innych pól z ich domyślną wartość, a następnie kliknij przycisk **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="5e9d0-188">Leave the **View engine** to **Razor** and the other fields with their default value and then click **Add**.</span></span>

    <span data-ttu-id="5e9d0-189">![Dodawanie widok indeksu](aspnet-mvc-4-helpers-forms-and-validation/_static/image3.png "Dodawanie widok indeksu")</span><span class="sxs-lookup"><span data-stu-id="5e9d0-189">![Adding an index view](aspnet-mvc-4-helpers-forms-and-validation/_static/image3.png "Adding an index view")</span></span>

    <span data-ttu-id="5e9d0-190">*Dodawanie widok indeksu*</span><span class="sxs-lookup"><span data-stu-id="5e9d0-190">*Adding an Index View*</span></span>

<a id="Ex1Task4"></a>

<a id="Task_4_-_Customizing_the_scaffold_of_the_Index_View"></a>
#### <a name="task-4---customizing-the-scaffold-of-the-index-view"></a><span data-ttu-id="5e9d0-191">Zadanie 4 — Dostosowywanie szkieletu widoku indeksu</span><span class="sxs-lookup"><span data-stu-id="5e9d0-191">Task 4 - Customizing the scaffold of the Index View</span></span>

<span data-ttu-id="5e9d0-192">W tym zadaniu zostanie dostosowana prostego szablonu Widok utworzony za pomocą funkcji szkieletów ASP.NET MVC ona żądane pola.</span><span class="sxs-lookup"><span data-stu-id="5e9d0-192">In this task, you will adjust the simple View template created with ASP.NET MVC scaffolding feature to have it display the fields you want.</span></span>

> [!NOTE]
> <span data-ttu-id="5e9d0-193">**Szkieletów** wsparcia w ramach platformy ASP.NET MVC generuje prosty szablon widoku, który wyświetla wszystkie pola w modelu albumu.</span><span class="sxs-lookup"><span data-stu-id="5e9d0-193">The **scaffolding** support within ASP.NET MVC generates a simple View template which lists all fields in the Album model.</span></span> <span data-ttu-id="5e9d0-194">**Tworzenia szkieletu** umożliwia szybkie rozpoczęcie pracy na silnie typizowanego widoku: zamiast ręcznie zapisać szablon widoku, tworzenia szkieletu szybko generuje szablon domyślny, a następnie można zmodyfikować wygenerowanego kodu.</span><span class="sxs-lookup"><span data-stu-id="5e9d0-194">**Scaffolding** provides a quick way to get started on a strongly typed view: rather than having to write the View template manually, scaffolding quickly generates a default template and then you can modify the generated code.</span></span>


1. <span data-ttu-id="5e9d0-195">Przejrzyj kod utworzony.</span><span class="sxs-lookup"><span data-stu-id="5e9d0-195">Review the code created.</span></span> <span data-ttu-id="5e9d0-196">Generowanie listy pól będzie częścią następujące tabeli HTML, który **szkieletów** używanej do wyświetlania danych tabelarycznych.</span><span class="sxs-lookup"><span data-stu-id="5e9d0-196">The generated list of fields will be part of the following HTML table that **Scaffolding** is using for displaying tabular data.</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample4.cshtml)]
2. <span data-ttu-id="5e9d0-197">Zastąp  **&lt;tabeli&gt;**  kodu z następującym kodem, które mają być wyświetlane tylko **Genre**, **wykonawcy**, **tytuł**, i **cen** pola.</span><span class="sxs-lookup"><span data-stu-id="5e9d0-197">Replace the **&lt;table&gt;** code with the following code to display only the **Genre**, **Artist**, **Album Title**, and **Price** fields.</span></span> <span data-ttu-id="5e9d0-198">Spowoduje to usunięcie **AlbumId** i **albumów graficznych URL** kolumn.</span><span class="sxs-lookup"><span data-stu-id="5e9d0-198">This deletes the **AlbumId** and **Album Art URL** columns.</span></span> <span data-ttu-id="5e9d0-199">Ponadto zmienia GenreId i ArtistId kolumny do wyświetlenia ich właściwości klasy połączonego **Artist.Name** i **Genre.Name**i usuwa **szczegóły** łącza.</span><span class="sxs-lookup"><span data-stu-id="5e9d0-199">Also, it changes GenreId and ArtistId columns to display their linked class properties of **Artist.Name** and **Genre.Name**, and removes the **Details** link.</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample5.cshtml)]
3. <span data-ttu-id="5e9d0-200">Zmień poniższe opisy.</span><span class="sxs-lookup"><span data-stu-id="5e9d0-200">Change the following descriptions.</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample6.cshtml)]

<a id="Ex1Task5"></a>

<a id="Task_5_-_Running_the_Application"></a>
#### <a name="task-5---running-the-application"></a><span data-ttu-id="5e9d0-201">Zadanie 5 działania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="5e9d0-201">Task 5 - Running the Application</span></span>

<span data-ttu-id="5e9d0-202">W tym zadaniu zostanie sprawdzić, czy **StoreManager** **indeksu** szablon widoku zostanie wyświetlona lista albumów zgodnie z projektem z poprzednich kroków.</span><span class="sxs-lookup"><span data-stu-id="5e9d0-202">In this task, you will test that the **StoreManager** **Index** View template displays a list of albums according to the design of the previous steps.</span></span>

1. <span data-ttu-id="5e9d0-203">Naciśnij klawisz **F5** do uruchomienia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="5e9d0-203">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="5e9d0-204">Na stronie głównej datę rozpoczęcia projektu.</span><span class="sxs-lookup"><span data-stu-id="5e9d0-204">The project starts in the Home page.</span></span> <span data-ttu-id="5e9d0-205">Zmień adres URL do **/StoreManager** Aby sprawdzić, czy zostanie wyświetlona lista albumy, przedstawiający ich **tytuł**, **wykonawcy** i **Genre**.</span><span class="sxs-lookup"><span data-stu-id="5e9d0-205">Change the URL to **/StoreManager** to verify that a list of albums is displayed, showing their **Title**, **Artist** and **Genre**.</span></span>

    <span data-ttu-id="5e9d0-206">![Przeglądanie listy albumów](aspnet-mvc-4-helpers-forms-and-validation/_static/image4.png "przeglądania listy albumów")</span><span class="sxs-lookup"><span data-stu-id="5e9d0-206">![Browsing the list of albums](aspnet-mvc-4-helpers-forms-and-validation/_static/image4.png "Browsing the list of albums")</span></span>

    <span data-ttu-id="5e9d0-207">*Przeglądanie listy albumów*</span><span class="sxs-lookup"><span data-stu-id="5e9d0-207">*Browsing the list of albums*</span></span>

<a id="Exercise2"></a>

<a id="Exercise_2_Adding_an_HTML_Helper"></a>
### <a name="exercise-2-adding-an-html-helper"></a><span data-ttu-id="5e9d0-208">Ćwiczenie 2: Dodawanie pomocnika kodu HTML</span><span class="sxs-lookup"><span data-stu-id="5e9d0-208">Exercise 2: Adding an HTML Helper</span></span>

<span data-ttu-id="5e9d0-209">Strona indeksu StoreManager ma jeden potencjalny problem: tytułu i nazwy wykonawcy właściwości mogą jednocześnie być wystarczająco długie, by wyrzucanie formatowanie tabeli.</span><span class="sxs-lookup"><span data-stu-id="5e9d0-209">The StoreManager Index page has one potential issue: Title and Artist Name properties can both be long enough to throw off the table formatting.</span></span> <span data-ttu-id="5e9d0-210">W tym ćwiczeniu dowiesz się, jak dodać niestandardowe pomocnika kodu HTML do obcięcia tekst.</span><span class="sxs-lookup"><span data-stu-id="5e9d0-210">In this exercise you will learn how to add a custom HTML helper to truncate that text.</span></span>

<span data-ttu-id="5e9d0-211">Na poniższej ilustracji widać, jak zmienić format ze względu na długość tekstu użycia rozmiar małych przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="5e9d0-211">In the following figure, you can see how the format is modified because of the length of the text when you use a small browser size.</span></span>

<span data-ttu-id="5e9d0-212">![Nie przeglądania listy albumów z obcięte tekst](aspnet-mvc-4-helpers-forms-and-validation/_static/image5.png "nie przeglądania listy albumów z obcięte tekstu")</span><span class="sxs-lookup"><span data-stu-id="5e9d0-212">![Browsing the list of Albums with not truncated text](aspnet-mvc-4-helpers-forms-and-validation/_static/image5.png "Browsing the list of Albums with not truncated text")</span></span>

<span data-ttu-id="5e9d0-213">*Nie przeglądania listy albumów z obcięte tekstu*</span><span class="sxs-lookup"><span data-stu-id="5e9d0-213">*Browsing the list of Albums with not truncated text*</span></span>

<a id="Ex2Task1"></a>

<a id="Task_1_-_Extending_the_HTML_Helper"></a>
#### <a name="task-1---extending-the-html-helper"></a><span data-ttu-id="5e9d0-214">Zadanie 1 - rozszerzanie pomocnika kodu HTML</span><span class="sxs-lookup"><span data-stu-id="5e9d0-214">Task 1 - Extending the HTML Helper</span></span>

<span data-ttu-id="5e9d0-215">To zadanie spowoduje dodanie nowej metody **Truncate** do **HTML** obiektu w widoków MVC ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="5e9d0-215">In this task, you will add a new method **Truncate** to the **HTML** object exposed within ASP.NET MVC Views.</span></span> <span data-ttu-id="5e9d0-216">Aby to zrobić, zostaną zaimplementowane **— metoda rozszerzenia** do wbudowanej **System.Web.Mvc.HtmlHelper** klasy przez platformę ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="5e9d0-216">To do this, you will implement an **extension method** to the built-in **System.Web.Mvc.HtmlHelper** class provided by ASP.NET MVC.</span></span>

> [!NOTE]
> <span data-ttu-id="5e9d0-217">Aby dowiedzieć się więcej o **metody rozszerzenia**, odwiedź ten artykuł w witrynie msdn.</span><span class="sxs-lookup"><span data-stu-id="5e9d0-217">To read more about **Extension Methods**, please visit this msdn article.</span></span> <span data-ttu-id="5e9d0-218">[https://msdn.microsoft.com/en-us/library/bb383977.aspx](https://msdn.microsoft.com/en-us/library/bb383977.aspx).</span><span class="sxs-lookup"><span data-stu-id="5e9d0-218">[https://msdn.microsoft.com/en-us/library/bb383977.aspx](https://msdn.microsoft.com/en-us/library/bb383977.aspx).</span></span>


1. <span data-ttu-id="5e9d0-219">Otwórz **rozpocząć** rozwiązania, znajdujących się na **źródło/Ex2-AddingAnHTMLHelper/Begin/** folderu.</span><span class="sxs-lookup"><span data-stu-id="5e9d0-219">Open the **Begin** solution located at **Source/Ex2-AddingAnHTMLHelper/Begin/** folder.</span></span> <span data-ttu-id="5e9d0-220">W przeciwnym razie możesz nadal korzystać **zakończenia** uzyskane rozwiązanie, wykonując poprzednim ćwiczeniu.</span><span class="sxs-lookup"><span data-stu-id="5e9d0-220">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

    1. <span data-ttu-id="5e9d0-221">Po otwarciu dostarczonych **rozpocząć** rozwiązania, musisz pobrać niektórych brakujących pakietów NuGet aby kontynuować.</span><span class="sxs-lookup"><span data-stu-id="5e9d0-221">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="5e9d0-222">Aby to zrobić, kliknij przycisk **projektu** menu i wybierz **Zarządzaj pakietami NuGet**.</span><span class="sxs-lookup"><span data-stu-id="5e9d0-222">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
    2. <span data-ttu-id="5e9d0-223">W **Zarządzaj pakietami NuGet** okna dialogowego, kliknij przycisk **przywrócić** celu pobieranie brakujących pakietów.</span><span class="sxs-lookup"><span data-stu-id="5e9d0-223">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
    3. <span data-ttu-id="5e9d0-224">Na koniec Skompiluj rozwiązanie, klikając **kompilacji** | **Kompiluj rozwiązanie**.</span><span class="sxs-lookup"><span data-stu-id="5e9d0-224">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="5e9d0-225">Jedną z zalet przy użyciu narzędzia NuGet jest, że nie masz do wysłania wszystkich bibliotek w projekcie, zmniejszenie jego rozmiar projektu.</span><span class="sxs-lookup"><span data-stu-id="5e9d0-225">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="5e9d0-226">Narzędzia Power NuGet określając wersje pakietów w pliku Packages.config, będzie można pobrać wymaganych bibliotek podczas pierwszego uruchomienia projektu.</span><span class="sxs-lookup"><span data-stu-id="5e9d0-226">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="5e9d0-227">Jest to, dlaczego konieczne będzie wykonanie tych kroków, po otwarciu istniejącego rozwiązania z tego laboratorium.</span><span class="sxs-lookup"><span data-stu-id="5e9d0-227">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="5e9d0-228">Otwórz widok indeksu StoreManager firmy.</span><span class="sxs-lookup"><span data-stu-id="5e9d0-228">Open StoreManager's Index View.</span></span> <span data-ttu-id="5e9d0-229">Aby to zrobić, w Eksploratorze rozwiązań rozwiń **widoków** folder, a następnie **StoreManager** , a następnie otwórz **Index.cshtml** pliku.</span><span class="sxs-lookup"><span data-stu-id="5e9d0-229">To do this, in the Solution Explorer expand the **Views** folder, then the **StoreManager** and open the **Index.cshtml** file.</span></span>
3. <span data-ttu-id="5e9d0-230">Dodaj następujący kod poniżej  **@model**  dyrektywy do definiowania **Truncate** metody pomocnika.</span><span class="sxs-lookup"><span data-stu-id="5e9d0-230">Add the following code below the **@model** directive to define the **Truncate** helper method.</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample7.cshtml)]

<a id="Ex2Task2"></a>

<a id="Task_2_-_Truncating_Text_in_the_Page"></a>
#### <a name="task-2---truncating-text-in-the-page"></a><span data-ttu-id="5e9d0-231">Zadanie 2 — obcinanie tekstu na stronie</span><span class="sxs-lookup"><span data-stu-id="5e9d0-231">Task 2 - Truncating Text in the Page</span></span>

<span data-ttu-id="5e9d0-232">W tym zadaniu użyjesz **Truncate** metody obcięcia tekst w szablonie widoku.</span><span class="sxs-lookup"><span data-stu-id="5e9d0-232">In this task, you will use the **Truncate** method to truncate the text in the View template.</span></span>

1. <span data-ttu-id="5e9d0-233">Otwórz widok indeksu StoreManager firmy.</span><span class="sxs-lookup"><span data-stu-id="5e9d0-233">Open StoreManager's Index View.</span></span> <span data-ttu-id="5e9d0-234">Aby to zrobić, w Eksploratorze rozwiązań rozwiń **widoków** folder, a następnie **StoreManager** , a następnie otwórz **Index.cshtml** pliku.</span><span class="sxs-lookup"><span data-stu-id="5e9d0-234">To do this, in the Solution Explorer expand the **Views** folder, then the **StoreManager** and open the **Index.cshtml** file.</span></span>
2. <span data-ttu-id="5e9d0-235">Zamień wiersze, które zawierają **nazwy wykonawcy** i albumu **tytuł**.</span><span class="sxs-lookup"><span data-stu-id="5e9d0-235">Replace the lines that show the **Artist Name** and Album's **Title**.</span></span> <span data-ttu-id="5e9d0-236">Aby to zrobić, Zamień następujące wiersze.</span><span class="sxs-lookup"><span data-stu-id="5e9d0-236">To do this, replace the following lines.</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample8.cshtml)]

<a id="Ex2Task3"></a>

<a id="Task_3_-_Running_the_Application"></a>
#### <a name="task-3---running-the-application"></a><span data-ttu-id="5e9d0-237">Zadanie 3 — uruchamianie aplikacji</span><span class="sxs-lookup"><span data-stu-id="5e9d0-237">Task 3 - Running the Application</span></span>

<span data-ttu-id="5e9d0-238">W tym zadaniu zostanie sprawdzić, czy **StoreManager** **indeksu** Wyświetl szablon obcina tytułu i nazwy wykonawcy albumu.</span><span class="sxs-lookup"><span data-stu-id="5e9d0-238">In this task, you will test that the **StoreManager** **Index** View template truncates the Album's Title and Artist Name.</span></span>

1. <span data-ttu-id="5e9d0-239">Naciśnij klawisz **F5** do uruchomienia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="5e9d0-239">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="5e9d0-240">Na stronie głównej datę rozpoczęcia projektu.</span><span class="sxs-lookup"><span data-stu-id="5e9d0-240">The project starts in the Home page.</span></span> <span data-ttu-id="5e9d0-241">Zmień adres URL do **/StoreManager** zweryfikowanie, że long teksty w **tytuł** i **wykonawcy** kolumny są obcinane.</span><span class="sxs-lookup"><span data-stu-id="5e9d0-241">Change the URL to **/StoreManager** to verify that long texts in the **Title** and **Artist** column are truncated.</span></span>

    <span data-ttu-id="5e9d0-242">![Obcięty nazwy tytułów i artystów](aspnet-mvc-4-helpers-forms-and-validation/_static/image6.png "obcięty tytułów i artystów nazwy")</span><span class="sxs-lookup"><span data-stu-id="5e9d0-242">![Truncated titles and artists names](aspnet-mvc-4-helpers-forms-and-validation/_static/image6.png "Truncated titles and artists names")</span></span>

    <span data-ttu-id="5e9d0-243">*Skrócona tytułów i nazwy wykonawców*</span><span class="sxs-lookup"><span data-stu-id="5e9d0-243">*Truncated Titles and Artist Names*</span></span>

<a id="Exercise3"></a>

<a id="Exercise_3_Creating_the_Edit_View"></a>
### <a name="exercise-3-creating-the-edit-view"></a><span data-ttu-id="5e9d0-244">Ćwiczenie 3: Tworzenie widoku edycji</span><span class="sxs-lookup"><span data-stu-id="5e9d0-244">Exercise 3: Creating the Edit View</span></span>

<span data-ttu-id="5e9d0-245">W tym ćwiczeniu dowiesz się, jak utworzyć formularz umożliwia menedżerów magazynu do edycji albumu.</span><span class="sxs-lookup"><span data-stu-id="5e9d0-245">In this exercise, you will learn how to create a form to allow store managers to edit an Album.</span></span> <span data-ttu-id="5e9d0-246">Będzie przeglądania **/StoreManager/Edit/id** adresu URL (**identyfikator** jest unikatowy identyfikator albumy, aby edytować), dzięki czemu wywołanie HTTP GET do serwera.</span><span class="sxs-lookup"><span data-stu-id="5e9d0-246">They will browse the **/StoreManager/Edit/id** URL (**id** being the unique id of the album to edit), thus making an HTTP-GET call to the server.</span></span>

<span data-ttu-id="5e9d0-247">Metody akcji kontrolera Edytuj będzie pobrać odpowiednią Album z bazy danych, Utwórz **StoreManagerViewModel** obiekt, aby hermetyzować go (wraz z listy artystów i Genres), a następnie przekazać go do szablonu widoku w celu renderowanie strony HTML do użytkownika.</span><span class="sxs-lookup"><span data-stu-id="5e9d0-247">The Controller Edit action method will retrieve the appropriate Album from the database, create a **StoreManagerViewModel** object to encapsulate it (along with a list of Artists and Genres), and then pass it off to a View template to render the HTML page back to the user.</span></span> <span data-ttu-id="5e9d0-248">Ta strona będzie zawierać  **&lt;formularza&gt;**  element z pola tekstowe i listę rozwijaną do edycji właściwości albumu.</span><span class="sxs-lookup"><span data-stu-id="5e9d0-248">This page will contain a **&lt;form&gt;** element with textboxes and dropdowns for editing the Album properties.</span></span>

<span data-ttu-id="5e9d0-249">Po aktualizacji albumu wartości formularza i klika użytkownika **zapisać** przycisku zmiany są przesyłane za pośrednictwem HTTP POST wywołania zwrotnego **/StoreManager/Edit/id**. Mimo że adres URL jest taka sama jak w ostatnim wywołaniu, ASP.NET MVC określi, że teraz POST protokołu HTTP, a w związku z tym wykonuje różne metody akcji edycji (jeden ozdobione **[HttpPost]**).</span><span class="sxs-lookup"><span data-stu-id="5e9d0-249">Once the user updates the Album form values and clicks the **Save** button, the changes are submitted via an HTTP-POST call back to **/StoreManager/Edit/id**. Although the URL remains the same as in the last call, ASP.NET MVC identifies that this time it is an HTTP-POST and therefore executes a different Edit action method (one decorated with **[HttpPost]**).</span></span>

<a id="Ex3Task1"></a>

<a id="Task_1_-_Implementing_the_HTTP-GET_Edit_Action_Method"></a>
#### <a name="task-1---implementing-the-http-get-edit-action-method"></a><span data-ttu-id="5e9d0-250">Zadanie 1 - implementacja metody akcji HTTP GET edycji</span><span class="sxs-lookup"><span data-stu-id="5e9d0-250">Task 1 - Implementing the HTTP-GET Edit Action Method</span></span>

<span data-ttu-id="5e9d0-251">To zadanie będzie implementowany wersji HTTP GET metody akcji edycji można pobrać odpowiednią Album z bazy danych, a także listę wszystkich Genres i artystów.</span><span class="sxs-lookup"><span data-stu-id="5e9d0-251">In this task, you will implement the HTTP-GET version of the Edit action method to retrieve the appropriate Album from the database, as well as a list of all Genres and Artists.</span></span> <span data-ttu-id="5e9d0-252">Go będzie spakować dane do **StoreManagerViewModel** zdefiniowane w ostatnim kroku, który następnie zostanie przekazany do szablonu widoku do renderowania odpowiedzi z obiektu.</span><span class="sxs-lookup"><span data-stu-id="5e9d0-252">It will package this data up into the **StoreManagerViewModel** object defined in the last step, which will then be passed to a View template to render the response with.</span></span>

1. <span data-ttu-id="5e9d0-253">Otwórz **rozpocząć** rozwiązania, znajdujących się na **źródło/Ex3-CreatingTheEditView/Begin/** folderu.</span><span class="sxs-lookup"><span data-stu-id="5e9d0-253">Open the **Begin** solution located at **Source/Ex3-CreatingTheEditView/Begin/** folder.</span></span> <span data-ttu-id="5e9d0-254">W przeciwnym razie możesz nadal korzystać **zakończenia** uzyskane rozwiązanie, wykonując poprzednim ćwiczeniu.</span><span class="sxs-lookup"><span data-stu-id="5e9d0-254">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

    1. <span data-ttu-id="5e9d0-255">Po otwarciu dostarczonych **rozpocząć** rozwiązania, musisz pobrać niektórych brakujących pakietów NuGet aby kontynuować.</span><span class="sxs-lookup"><span data-stu-id="5e9d0-255">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="5e9d0-256">Aby to zrobić, kliknij przycisk **projektu** menu i wybierz **Zarządzaj pakietami NuGet**.</span><span class="sxs-lookup"><span data-stu-id="5e9d0-256">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
    2. <span data-ttu-id="5e9d0-257">W **Zarządzaj pakietami NuGet** okna dialogowego, kliknij przycisk **przywrócić** celu pobieranie brakujących pakietów.</span><span class="sxs-lookup"><span data-stu-id="5e9d0-257">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
    3. <span data-ttu-id="5e9d0-258">Na koniec Skompiluj rozwiązanie, klikając **kompilacji** | **Kompiluj rozwiązanie**.</span><span class="sxs-lookup"><span data-stu-id="5e9d0-258">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="5e9d0-259">Jedną z zalet przy użyciu narzędzia NuGet jest, że nie masz do wysłania wszystkich bibliotek w projekcie, zmniejszenie jego rozmiar projektu.</span><span class="sxs-lookup"><span data-stu-id="5e9d0-259">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="5e9d0-260">Narzędzia Power NuGet określając wersje pakietów w pliku Packages.config, będzie można pobrać wymaganych bibliotek podczas pierwszego uruchomienia projektu.</span><span class="sxs-lookup"><span data-stu-id="5e9d0-260">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="5e9d0-261">Jest to, dlaczego konieczne będzie wykonanie tych kroków, po otwarciu istniejącego rozwiązania z tego laboratorium.</span><span class="sxs-lookup"><span data-stu-id="5e9d0-261">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="5e9d0-262">Otwórz **StoreManagerController** klasy.</span><span class="sxs-lookup"><span data-stu-id="5e9d0-262">Open the **StoreManagerController** class.</span></span> <span data-ttu-id="5e9d0-263">Aby to zrobić, rozwiń węzeł **kontrolerów** folder i kliknij dwukrotnie **StoreManagerController.cs**.</span><span class="sxs-lookup"><span data-stu-id="5e9d0-263">To do this, expand the **Controllers** folder and double-click **StoreManagerController.cs**.</span></span>
3. <span data-ttu-id="5e9d0-264">Zastąp **Edytuj HTTP GET** metodę akcji za pomocą następujący kod, aby pobrać odpowiednią **albumu** , jak również **Genres** i **artystów**listy.</span><span class="sxs-lookup"><span data-stu-id="5e9d0-264">Replace the **HTTP-GET Edit** action method with the following code to retrieve the appropriate **Album** as well as the **Genres** and **Artists** lists.</span></span>

    <span data-ttu-id="5e9d0-265">(Fragment - kodu *ASP.NET MVC 4 pomocników i formularzy i sprawdzanie poprawności — Akcja Ex3 StoreManagerController HTTP GET Edytuj*)</span><span class="sxs-lookup"><span data-stu-id="5e9d0-265">(Code Snippet - *ASP.NET MVC 4 Helpers and Forms and Validation - Ex3 StoreManagerController HTTP-GET Edit action*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample9.cs)]

    > [!NOTE]
    > <span data-ttu-id="5e9d0-266">Używasz **System.Web.Mvc** **SelectList** dla artystów i Genres zamiast **system.Collections.Generic —** listy.</span><span class="sxs-lookup"><span data-stu-id="5e9d0-266">You are using **System.Web.Mvc** **SelectList** for Artists and Genres instead of the **System.Collections.Generic** List.</span></span>
    > 
    > <span data-ttu-id="5e9d0-267">**SelectList** bardziej przejrzysty sposób wypełnić listę rozwijaną HTML i zarządzanie nimi elementów, jak bieżące zaznaczenie.</span><span class="sxs-lookup"><span data-stu-id="5e9d0-267">**SelectList** is a cleaner way to populate HTML dropdowns and manage things like current selection.</span></span> <span data-ttu-id="5e9d0-268">Tworzenie wystąpień i nowsze, konfigurując te obiekty ViewModel w akcji kontrolera spowoduje, że scenariusza formularza edycji oczyszczarki.</span><span class="sxs-lookup"><span data-stu-id="5e9d0-268">Instantiating and later setting up these ViewModel objects in the controller action will make the Edit form scenario cleaner.</span></span>

<a id="Ex3Task2"></a>

<a id="Task_2_-_Creating_the_Edit_View"></a>
#### <a name="task-2---creating-the-edit-view"></a><span data-ttu-id="5e9d0-269">Zadanie 2 — Tworzenie widoku edycji</span><span class="sxs-lookup"><span data-stu-id="5e9d0-269">Task 2 - Creating the Edit View</span></span>

<span data-ttu-id="5e9d0-270">W ramach tego zadania spowoduje utworzenie szablonu Edytuj widok, który później będzie mógł wyświetlić właściwości albumu.</span><span class="sxs-lookup"><span data-stu-id="5e9d0-270">In this task, you will create an Edit View template that will later display the album properties.</span></span>

1. <span data-ttu-id="5e9d0-271">Utwórz widok edycji.</span><span class="sxs-lookup"><span data-stu-id="5e9d0-271">Create the Edit View.</span></span> <span data-ttu-id="5e9d0-272">Aby to zrobić, kliknij prawym przyciskiem myszy wewnątrz **Edytuj** metody akcji i wybierz **Dodaj widok**.</span><span class="sxs-lookup"><span data-stu-id="5e9d0-272">To do this, right-click inside the **Edit** action method and select **Add View**.</span></span>
2. <span data-ttu-id="5e9d0-273">W oknie dialogowym Dodaj widok, sprawdź, czy nazwa widoku jest **Edytuj**.</span><span class="sxs-lookup"><span data-stu-id="5e9d0-273">In the Add View dialog, verify that the View Name is **Edit**.</span></span> <span data-ttu-id="5e9d0-274">Sprawdź **utworzyć widok jednoznacznie** wyboru i wybierz **albumu (MvcMusicStore.Models)** z **wyświetlić klasy danych** listy rozwijanej.</span><span class="sxs-lookup"><span data-stu-id="5e9d0-274">Check the **Create a strongly-typed view** checkbox and select **Album (MvcMusicStore.Models)** from the **View data class** drop-down.</span></span> <span data-ttu-id="5e9d0-275">Wybierz **Edytuj** z **szablonu szkieletu** listy rozwijanej.</span><span class="sxs-lookup"><span data-stu-id="5e9d0-275">Select **Edit** from the **Scaffold template** drop-down.</span></span> <span data-ttu-id="5e9d0-276">Pozostaw innych pól ich wartości domyślne, a następnie kliknij przycisk **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="5e9d0-276">Leave the other fields with their default value and then click **Add**.</span></span>

    <span data-ttu-id="5e9d0-277">![Dodawanie widoku edycji](aspnet-mvc-4-helpers-forms-and-validation/_static/image7.png "Dodawanie widoku edycji")</span><span class="sxs-lookup"><span data-stu-id="5e9d0-277">![Adding an Edit view](aspnet-mvc-4-helpers-forms-and-validation/_static/image7.png "Adding an Edit view")</span></span>

    <span data-ttu-id="5e9d0-278">*Dodawanie widoku edycji*</span><span class="sxs-lookup"><span data-stu-id="5e9d0-278">*Adding an Edit view*</span></span>

<a id="Ex3Task3"></a>

<a id="Task_3_-_Running_the_Application"></a>
#### <a name="task-3---running-the-application"></a><span data-ttu-id="5e9d0-279">Zadanie 3 — uruchamianie aplikacji</span><span class="sxs-lookup"><span data-stu-id="5e9d0-279">Task 3 - Running the Application</span></span>

<span data-ttu-id="5e9d0-280">W tym zadaniu zostanie sprawdzić, czy **StoreManager** **edytować** strona widoku są wyświetlane wartości właściwości albumu przekazany jako parametr.</span><span class="sxs-lookup"><span data-stu-id="5e9d0-280">In this task, you will test that the **StoreManager** **Edit** View page displays the properties' values for the album passed as parameter.</span></span>

1. <span data-ttu-id="5e9d0-281">Naciśnij klawisz **F5** do uruchomienia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="5e9d0-281">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="5e9d0-282">Na stronie głównej datę rozpoczęcia projektu.</span><span class="sxs-lookup"><span data-stu-id="5e9d0-282">The project starts in the Home page.</span></span> <span data-ttu-id="5e9d0-283">Zmień adres URL do **/StoreManager/Edit/1** Aby sprawdzić, czy są wyświetlane wartości właściwości dla albumu przekazany.</span><span class="sxs-lookup"><span data-stu-id="5e9d0-283">Change the URL to **/StoreManager/Edit/1** to verify that the properties' values for the album passed are displayed.</span></span>

    <span data-ttu-id="5e9d0-284">![Przeglądanie widoku edycji albumu](aspnet-mvc-4-helpers-forms-and-validation/_static/image8.png "przeglądanie albumu widoku edycji")</span><span class="sxs-lookup"><span data-stu-id="5e9d0-284">![Browsing Album's Edit View](aspnet-mvc-4-helpers-forms-and-validation/_static/image8.png "Browsing Album's Edit View")</span></span>

    <span data-ttu-id="5e9d0-285">*Przeglądanie widoku edycji albumu*</span><span class="sxs-lookup"><span data-stu-id="5e9d0-285">*Browsing Album's Edit view*</span></span>

<a id="Ex3Task4"></a>

<a id="Task_4_-_Implementing_drop-downs_on_the_Album_Editor_Template"></a>
#### <a name="task-4---implementing-drop-downs-on-the-album-editor-template"></a><span data-ttu-id="5e9d0-286">Zadanie 4 — wdrażanie listach rozwijanych w szablonie albumu edytora</span><span class="sxs-lookup"><span data-stu-id="5e9d0-286">Task 4 - Implementing drop-downs on the Album Editor Template</span></span>

<span data-ttu-id="5e9d0-287">W tym zadaniu dodasz listach rozwijanych do Wyświetl szablon utworzony w ostatnim zadaniem, dzięki czemu użytkownik może wybrać z listy artystów i Genres.</span><span class="sxs-lookup"><span data-stu-id="5e9d0-287">In this task, you will add drop-downs to the View template created in the last task, so that the user can select from a list of Artists and Genres.</span></span>

1. <span data-ttu-id="5e9d0-288">Zamień wszystkie **albumu** fieldset kod następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="5e9d0-288">Replace all the **Album** fieldset code with the following:</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample10.cshtml)]

    > [!NOTE]
    > <span data-ttu-id="5e9d0-289">**Html.DropDownList** pomocnika został dodany do listy rozwijane wyboru artystów i Genres renderowania.</span><span class="sxs-lookup"><span data-stu-id="5e9d0-289">An **Html.DropDownList** helper has been added to render drop-downs for choosing Artists and Genres.</span></span> <span data-ttu-id="5e9d0-290">Parametry przekazywane do **Html.DropDownList** są:</span><span class="sxs-lookup"><span data-stu-id="5e9d0-290">The parameters passed to **Html.DropDownList** are:</span></span>
    > 
    > 1. <span data-ttu-id="5e9d0-291">Nazwa pola formularza (**&quot;ArtistId&quot;**).</span><span class="sxs-lookup"><span data-stu-id="5e9d0-291">The name of the form field (**&quot;ArtistId&quot;**).</span></span>
    > 2. <span data-ttu-id="5e9d0-292">**SelectList** wartości dla listy rozwijanej.</span><span class="sxs-lookup"><span data-stu-id="5e9d0-292">The **SelectList** of values for the drop-down.</span></span>

<a id="Ex3Task5"></a>

<a id="Task_5_-_Running_the_Application"></a>
#### <a name="task-5---running-the-application"></a><span data-ttu-id="5e9d0-293">Zadanie 5 działania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="5e9d0-293">Task 5 - Running the Application</span></span>

<span data-ttu-id="5e9d0-294">W tym zadaniu zostanie sprawdzić, czy **StoreManager** **Edytuj** listach rozwijanych zamiast wykonawcy i identyfikator Genre pól tekstowych zostanie wyświetlona strona widoku.</span><span class="sxs-lookup"><span data-stu-id="5e9d0-294">In this task, you will test that the **StoreManager** **Edit** View page displays drop-downs instead of Artist and Genre ID text fields.</span></span>

1. <span data-ttu-id="5e9d0-295">Naciśnij klawisz **F5** do uruchomienia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="5e9d0-295">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="5e9d0-296">Na stronie głównej datę rozpoczęcia projektu.</span><span class="sxs-lookup"><span data-stu-id="5e9d0-296">The project starts in the Home page.</span></span> <span data-ttu-id="5e9d0-297">Zmień adres URL do **/StoreManager/Edit/1** można zweryfikować Wyświetla listach rozwijanych zamiast wykonawcy i identyfikator Genre pól tekstowych.</span><span class="sxs-lookup"><span data-stu-id="5e9d0-297">Change the URL to **/StoreManager/Edit/1** to verify that it displays drop-downs instead of Artist and Genre ID text fields.</span></span>

    <span data-ttu-id="5e9d0-298">![Przeglądanie albumu Edytowanie widoku z listy rozwijane](aspnet-mvc-4-helpers-forms-and-validation/_static/image9.png "przeglądanie albumu Edytowanie widoku z listy rozwijane")</span><span class="sxs-lookup"><span data-stu-id="5e9d0-298">![Browsing Album's Edit View with drop downs](aspnet-mvc-4-helpers-forms-and-validation/_static/image9.png "Browsing Album's Edit View with drop downs")</span></span>

    <span data-ttu-id="5e9d0-299">*Widok edycji przeglądania albumu, tym razem z list rozwijanych*</span><span class="sxs-lookup"><span data-stu-id="5e9d0-299">*Browsing Album's Edit view, this time with dropdowns*</span></span>

<a id="Ex3Task6"></a>

<a id="Task_6_-_Implementing_the_HTTP-POST_Edit_action_method"></a>
#### <a name="task-6---implementing-the-http-post-edit-action-method"></a><span data-ttu-id="5e9d0-300">Zadanie 6 - implementacja metody akcji POST protokołu HTTP, Edytuj</span><span class="sxs-lookup"><span data-stu-id="5e9d0-300">Task 6 - Implementing the HTTP-POST Edit action method</span></span>

<span data-ttu-id="5e9d0-301">Teraz, edytowanie widoku wyświetlane zgodnie z oczekiwaniami, należy wdrożyć metody akcji Edytuj POST protokołu HTTP, aby zapisać zmiany wprowadzone do albumu.</span><span class="sxs-lookup"><span data-stu-id="5e9d0-301">Now that the Edit View displays as expected, you need to implement the HTTP-POST Edit Action method to save the changes made to the Album.</span></span>

1. <span data-ttu-id="5e9d0-302">Zamknij przeglądarkę, jeśli to konieczne, aby powrócić do okna programu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="5e9d0-302">Close the browser if needed, to return to the Visual Studio window.</span></span> <span data-ttu-id="5e9d0-303">Otwórz **StoreManagerController** z **kontrolerów** folderu.</span><span class="sxs-lookup"><span data-stu-id="5e9d0-303">Open **StoreManagerController** from the **Controllers** folder.</span></span>
2. <span data-ttu-id="5e9d0-304">Zastąp **Edytuj HTTP POST** kod do metody akcji z następującymi (należy pamiętać, że metoda, którą należy zastąpić przeciążone wersji, która otrzymuje dwa parametry):</span><span class="sxs-lookup"><span data-stu-id="5e9d0-304">Replace **HTTP-POST Edit** action method code with the following (note that the method that must be replaced is overloaded version that receives two parameters):</span></span>

    <span data-ttu-id="5e9d0-305">(Fragment - kodu *ASP.NET MVC 4 pomocników i formularzy i sprawdzanie poprawności — Edytuj HTTP POST StoreManagerController Ex3 akcji*)</span><span class="sxs-lookup"><span data-stu-id="5e9d0-305">(Code Snippet - *ASP.NET MVC 4 Helpers and Forms and Validation - Ex3 StoreManagerController HTTP-POST Edit action*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample11.cs)]

    > [!NOTE]
    > <span data-ttu-id="5e9d0-306">Ta metoda zostanie wykonany, gdy użytkownik kliknie **zapisać** przycisk widoku i wykonuje HTTP POST wartości formularza do serwera zachowywał je w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="5e9d0-306">This method will be executed when the user clicks the **Save** button of the View and performs an HTTP-POST of the form values back to the server to persist them in the database.</span></span> <span data-ttu-id="5e9d0-307">Dekorator **[HttpPost]** wskazuje, czy można użyć metody w tych scenariuszach POST protokołu HTTP.</span><span class="sxs-lookup"><span data-stu-id="5e9d0-307">The decorator **[HttpPost]** indicates that the method should be used for those HTTP-POST scenarios.</span></span> <span data-ttu-id="5e9d0-308">Metoda korzysta z **albumu** obiektu.</span><span class="sxs-lookup"><span data-stu-id="5e9d0-308">The method takes an **Album** object.</span></span> <span data-ttu-id="5e9d0-309">ASP.NET MVC zostanie automatycznie utworzony obiekt Album z ogłaszanymi &lt;formularza&gt; wartości.</span><span class="sxs-lookup"><span data-stu-id="5e9d0-309">ASP.NET MVC will automatically create the Album object from the posted &lt;form&gt; values.</span></span>
    > 
    > <span data-ttu-id="5e9d0-310">Metoda zostaną wykonane następujące kroki:</span><span class="sxs-lookup"><span data-stu-id="5e9d0-310">The method will perform these steps:</span></span>
    > 
    > 1. <span data-ttu-id="5e9d0-311">Jeśli model jest prawidłowy:</span><span class="sxs-lookup"><span data-stu-id="5e9d0-311">If model is valid:</span></span>
    > 
    >     1. <span data-ttu-id="5e9d0-312">Zaktualizuj wpis albumu w kontekście oznaczyć je jako zmodyfikowanego obiektu.</span><span class="sxs-lookup"><span data-stu-id="5e9d0-312">Update the album entry in the context to mark it as a modified object.</span></span>
    >     2. <span data-ttu-id="5e9d0-313">Zapisz zmiany i Przekierowanie do widoku indeksu.</span><span class="sxs-lookup"><span data-stu-id="5e9d0-313">Save the changes and redirect to the index view.</span></span>
    > 2. <span data-ttu-id="5e9d0-314">Jeśli model nie jest prawidłowy, wypełnia obiekt ViewBag z **GenreId** i **ArtistId**, wówczas zostanie zwrócone widok z odebranego obiektu albumy, aby zezwolić użytkownikowi wykonać wszelkie wymagane aktualizacji.</span><span class="sxs-lookup"><span data-stu-id="5e9d0-314">If the model is not valid, it will populate the ViewBag with the **GenreId** and **ArtistId**, then it will return the view with the received Album object to allow the user perform any required update.</span></span>

<a id="Ex3Task7"></a>

<a id="Task_7_-_Running_the_Application"></a>
#### <a name="task-7---running-the-application"></a><span data-ttu-id="5e9d0-315">Zadanie 7 - uruchamiania aplikacji</span><span class="sxs-lookup"><span data-stu-id="5e9d0-315">Task 7 - Running the Application</span></span>

<span data-ttu-id="5e9d0-316">W tym zadaniu zostanie sprawdzić, czy **edycji StoreManager** — Wyświetl stronę faktycznie zapisuje zaktualizowanych danych albumu w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="5e9d0-316">In this task, you will test that the **StoreManager Edit** View page actually saves the updated Album data in the database.</span></span>

1. <span data-ttu-id="5e9d0-317">Naciśnij klawisz **F5** do uruchomienia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="5e9d0-317">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="5e9d0-318">Na stronie głównej datę rozpoczęcia projektu.</span><span class="sxs-lookup"><span data-stu-id="5e9d0-318">The project starts in the Home page.</span></span> <span data-ttu-id="5e9d0-319">Zmień adres URL do **/StoreManager/Edit/1**.</span><span class="sxs-lookup"><span data-stu-id="5e9d0-319">Change the URL to **/StoreManager/Edit/1**.</span></span> <span data-ttu-id="5e9d0-320">Zmień tytuł albumu do **obciążenia** i wybierz polecenie **zapisać**.</span><span class="sxs-lookup"><span data-stu-id="5e9d0-320">Change the Album title to **Load** and click on **Save**.</span></span> <span data-ttu-id="5e9d0-321">Sprawdź, czy tytuł albumu rzeczywiste zmiany na liście albumów.</span><span class="sxs-lookup"><span data-stu-id="5e9d0-321">Verify that album's title actually changed in the list of albums.</span></span>

    <span data-ttu-id="5e9d0-322">![Aktualizowanie albumu](aspnet-mvc-4-helpers-forms-and-validation/_static/image10.png "aktualizowanie albumu")</span><span class="sxs-lookup"><span data-stu-id="5e9d0-322">![Updating an album](aspnet-mvc-4-helpers-forms-and-validation/_static/image10.png "Updating an album")</span></span>

    <span data-ttu-id="5e9d0-323">*Aktualizowanie albumu*</span><span class="sxs-lookup"><span data-stu-id="5e9d0-323">*Updating an Album*</span></span>

<a id="Exercise4"></a>

<a id="Exercise_4_Adding_a_Create_View"></a>
### <a name="exercise-4-adding-a-create-view"></a><span data-ttu-id="5e9d0-324">Ćwiczenie 4: Dodawanie widoku Create</span><span class="sxs-lookup"><span data-stu-id="5e9d0-324">Exercise 4: Adding a Create View</span></span>

<span data-ttu-id="5e9d0-325">Teraz, gdy **StoreManagerController** obsługuje **Edytuj** możliwości, w tym ćwiczeniu dowiesz się jak dodać szablon Utwórz widok pozwala przechowywać menedżerowie dodawać nowe albumów do aplikacji.</span><span class="sxs-lookup"><span data-stu-id="5e9d0-325">Now that the **StoreManagerController** supports the **Edit** ability, in this exercise you will learn how to add a Create View template to let store managers add new Albums to the application.</span></span>

<span data-ttu-id="5e9d0-326">Tak jak w przypadku funkcji Edytuj, zostaną zaimplementowane scenariusz Utwórz za pomocą dwóch osobnych metod w ramach **StoreManagerController** klasy:</span><span class="sxs-lookup"><span data-stu-id="5e9d0-326">Like you did with the Edit functionality, you will implement the Create scenario using two separate methods within the **StoreManagerController** class:</span></span>

1. <span data-ttu-id="5e9d0-327">Jedna metoda akcji zostanie wyświetlony pusty formularz menedżerów magazynu najpierw odwiedź **/StoreManager/Utwórz** adresu URL.</span><span class="sxs-lookup"><span data-stu-id="5e9d0-327">One action method will display an empty form when store managers first visit the **/StoreManager/Create** URL.</span></span>
2. <span data-ttu-id="5e9d0-328">Druga metoda akcji obsłuży scenariusza gdzie kliknie Menedżer magazynu **zapisać** przycisk w formularzu i przesyła wartości z powrotem do **/StoreManager/Utwórz** adresem URL HTTP POST.</span><span class="sxs-lookup"><span data-stu-id="5e9d0-328">A second action method will handle the scenario where the store manager clicks the **Save** button within the form and submits the values back to the **/StoreManager/Create** URL as an HTTP-POST.</span></span>

<a id="Ex4Task1"></a>

<a id="Task_1_-_Implementing_the_HTTP-GET_Create_action_method"></a>
#### <a name="task-1---implementing-the-http-get-create-action-method"></a><span data-ttu-id="5e9d0-329">Zadanie 1 - implementacja metody akcji tworzenia HTTP GET</span><span class="sxs-lookup"><span data-stu-id="5e9d0-329">Task 1 - Implementing the HTTP-GET Create action method</span></span>

<span data-ttu-id="5e9d0-330">W tym zadaniu wdroży wersji HTTP GET, utwórz metody akcji, aby pobrać listę wszystkich Genres i artystów, spakować dane do **StoreManagerViewModel** obiektu, który następnie zostanie przekazany do szablonu widoku.</span><span class="sxs-lookup"><span data-stu-id="5e9d0-330">In this task, you will implement the HTTP-GET version of the Create action method to retrieve a list of all Genres and Artists, package this data up into a **StoreManagerViewModel** object, which will then be passed to a View template.</span></span>

1. <span data-ttu-id="5e9d0-331">Otwórz **rozpocząć** rozwiązania, znajdujących się na **źródło/Ex4-AddingACreateView/Begin/** folderu.</span><span class="sxs-lookup"><span data-stu-id="5e9d0-331">Open the **Begin** solution located at **Source/Ex4-AddingACreateView/Begin/** folder.</span></span> <span data-ttu-id="5e9d0-332">W przeciwnym razie możesz nadal korzystać **zakończenia** uzyskane rozwiązanie, wykonując poprzednim ćwiczeniu.</span><span class="sxs-lookup"><span data-stu-id="5e9d0-332">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

    1. <span data-ttu-id="5e9d0-333">Po otwarciu dostarczonych **rozpocząć** rozwiązania, musisz pobrać niektórych brakujących pakietów NuGet aby kontynuować.</span><span class="sxs-lookup"><span data-stu-id="5e9d0-333">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="5e9d0-334">Aby to zrobić, kliknij przycisk **projektu** menu i wybierz **Zarządzaj pakietami NuGet**.</span><span class="sxs-lookup"><span data-stu-id="5e9d0-334">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
    2. <span data-ttu-id="5e9d0-335">W **Zarządzaj pakietami NuGet** okna dialogowego, kliknij przycisk **przywrócić** celu pobieranie brakujących pakietów.</span><span class="sxs-lookup"><span data-stu-id="5e9d0-335">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
    3. <span data-ttu-id="5e9d0-336">Na koniec Skompiluj rozwiązanie, klikając **kompilacji** | **Kompiluj rozwiązanie**.</span><span class="sxs-lookup"><span data-stu-id="5e9d0-336">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="5e9d0-337">Jedną z zalet przy użyciu narzędzia NuGet jest, że nie masz do wysłania wszystkich bibliotek w projekcie, zmniejszenie jego rozmiar projektu.</span><span class="sxs-lookup"><span data-stu-id="5e9d0-337">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="5e9d0-338">Narzędzia Power NuGet określając wersje pakietów w pliku Packages.config, będzie można pobrać wymaganych bibliotek podczas pierwszego uruchomienia projektu.</span><span class="sxs-lookup"><span data-stu-id="5e9d0-338">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="5e9d0-339">Jest to, dlaczego konieczne będzie wykonanie tych kroków, po otwarciu istniejącego rozwiązania z tego laboratorium.</span><span class="sxs-lookup"><span data-stu-id="5e9d0-339">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="5e9d0-340">Otwórz **StoreManagerController** klasy.</span><span class="sxs-lookup"><span data-stu-id="5e9d0-340">Open **StoreManagerController** class.</span></span> <span data-ttu-id="5e9d0-341">Aby to zrobić, rozwiń węzeł **kontrolerów** folder i kliknij dwukrotnie **StoreManagerController.cs**.</span><span class="sxs-lookup"><span data-stu-id="5e9d0-341">To do this, expand the **Controllers** folder and double-click **StoreManagerController.cs**.</span></span>
3. <span data-ttu-id="5e9d0-342">Zastąp **Utwórz** kod metody akcji z następujących czynności:</span><span class="sxs-lookup"><span data-stu-id="5e9d0-342">Replace the **Create** action method code with the following:</span></span>

    <span data-ttu-id="5e9d0-343">(Fragment - kodu *ASP.NET MVC 4 pomocników i formularzy i sprawdzanie poprawności — utworzenie HTTP GET StoreManagerController Ex4*)</span><span class="sxs-lookup"><span data-stu-id="5e9d0-343">(Code Snippet - *ASP.NET MVC 4 Helpers and Forms and Validation - Ex4 StoreManagerController HTTP-GET Create action*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample12.cs)]

<a id="Ex4Task2"></a>

<a id="Task_2_-_Adding_the_Create_View"></a>
#### <a name="task-2---adding-the-create-view"></a><span data-ttu-id="5e9d0-344">Zadanie 2 — Dodawanie widoku Create</span><span class="sxs-lookup"><span data-stu-id="5e9d0-344">Task 2 - Adding the Create View</span></span>

<span data-ttu-id="5e9d0-345">To zadanie spowoduje dodanie szablonu tworzenia widoku, który wyświetli nowy formularz albumu (pusta).</span><span class="sxs-lookup"><span data-stu-id="5e9d0-345">In this task, you will add the Create View template that will display a new (empty) Album form.</span></span>

1. <span data-ttu-id="5e9d0-346">Kliknij prawym przyciskiem myszy wewnątrz **Utwórz** metody akcji i wybierz **Dodaj widok**.</span><span class="sxs-lookup"><span data-stu-id="5e9d0-346">Right-click inside the **Create** action method and select **Add View**.</span></span> <span data-ttu-id="5e9d0-347">Zostanie wyświetlone okno dialogowe dodawania widoku.</span><span class="sxs-lookup"><span data-stu-id="5e9d0-347">This will bring up the Add View dialog.</span></span>
2. <span data-ttu-id="5e9d0-348">W oknie dialogowym Dodaj widok, sprawdź, czy nazwa widoku jest **Utwórz**.</span><span class="sxs-lookup"><span data-stu-id="5e9d0-348">In the Add View dialog, verify that the View Name is **Create**.</span></span> <span data-ttu-id="5e9d0-349">Wybierz **utworzyć widok jednoznacznie** opcję i zaznacz **albumu (MvcMusicStore.Models)** z **klasa modelu** listy rozwijanej i **Utwórz** z **szablonu szkieletu** listy rozwijanej.</span><span class="sxs-lookup"><span data-stu-id="5e9d0-349">Select the **Create a strongly-typed view** option and select **Album (MvcMusicStore.Models)** from the **Model class** drop-down and **Create** from the **Scaffold template** drop-down.</span></span> <span data-ttu-id="5e9d0-350">Pozostaw innych pól ich wartości domyślne, a następnie kliknij przycisk **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="5e9d0-350">Leave the other fields with their default value and then click **Add**.</span></span>

    <span data-ttu-id="5e9d0-351">![Dodawanie widoku create](aspnet-mvc-4-helpers-forms-and-validation/_static/image11.png "Dodawanie a-create-view.png")</span><span class="sxs-lookup"><span data-stu-id="5e9d0-351">![Adding a create view](aspnet-mvc-4-helpers-forms-and-validation/_static/image11.png "adding-a-create-view.png")</span></span>

    <span data-ttu-id="5e9d0-352">*Dodawanie widoku Create*</span><span class="sxs-lookup"><span data-stu-id="5e9d0-352">*Adding the Create View*</span></span>
3. <span data-ttu-id="5e9d0-353">Aktualizacja **GenreId** i **ArtistId** pola, aby użyć listy rozwijanej, jak pokazano poniżej:</span><span class="sxs-lookup"><span data-stu-id="5e9d0-353">Update the **GenreId** and **ArtistId** fields to use a drop-down list as shown below:</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample13.cshtml)]

<a id="Ex4Task3"></a>

<a id="Task_3_-_Running_the_Application"></a>
#### <a name="task-3---running-the-application"></a><span data-ttu-id="5e9d0-354">Zadanie 3 — uruchamianie aplikacji</span><span class="sxs-lookup"><span data-stu-id="5e9d0-354">Task 3 - Running the Application</span></span>

<span data-ttu-id="5e9d0-355">W tym zadaniu zostanie sprawdzić, czy **StoreManager** **Utwórz** pusty formularz albumu zostanie wyświetlona strona widoku.</span><span class="sxs-lookup"><span data-stu-id="5e9d0-355">In this task, you will test that the **StoreManager** **Create** View page displays an empty Album form.</span></span>

1. <span data-ttu-id="5e9d0-356">Naciśnij klawisz **F5** do uruchomienia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="5e9d0-356">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="5e9d0-357">Na stronie głównej datę rozpoczęcia projektu.</span><span class="sxs-lookup"><span data-stu-id="5e9d0-357">The project starts in the Home page.</span></span> <span data-ttu-id="5e9d0-358">Zmień adres URL do **/StoreManager/utworzyć**.</span><span class="sxs-lookup"><span data-stu-id="5e9d0-358">Change the URL to **/StoreManager/Create**.</span></span> <span data-ttu-id="5e9d0-359">Sprawdź, czy pusty formularz jest wyświetlany do wypełniania nowe właściwości albumu.</span><span class="sxs-lookup"><span data-stu-id="5e9d0-359">Verify that an empty form is displayed for filling the new Album properties.</span></span>

    <span data-ttu-id="5e9d0-360">![Utwórz widok z pusty formularz](aspnet-mvc-4-helpers-forms-and-validation/_static/image12.png "Utwórz widok z pusty formularz")</span><span class="sxs-lookup"><span data-stu-id="5e9d0-360">![Create View with an empty form](aspnet-mvc-4-helpers-forms-and-validation/_static/image12.png "Create View with an empty form")</span></span>

    <span data-ttu-id="5e9d0-361">*Utwórz widok z pusty formularz*</span><span class="sxs-lookup"><span data-stu-id="5e9d0-361">*Create View with an empty form*</span></span>

<a id="Ex4Task4"></a>

<a id="Task_4_-_Implementing_the_HTTP-POST_Create_Action_Method"></a>
#### <a name="task-4---implementing-the-http-post-create-action-method"></a><span data-ttu-id="5e9d0-362">Zadanie 4 — wdrażanie POST protokołu HTTP, utwórz metody akcji</span><span class="sxs-lookup"><span data-stu-id="5e9d0-362">Task 4 - Implementing the HTTP-POST Create Action Method</span></span>

<span data-ttu-id="5e9d0-363">W tym zadaniu wdroży wersji HTTP POST Utwórz metody akcji, która będzie wywoływana, gdy użytkownik kliknie **zapisać** przycisku.</span><span class="sxs-lookup"><span data-stu-id="5e9d0-363">In this task, you will implement the HTTP-POST version of the Create action method that will be invoked when a user clicks the **Save** button.</span></span> <span data-ttu-id="5e9d0-364">Metoda należy zapisać nowy album w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="5e9d0-364">The method should save the new album in the database.</span></span>

1. <span data-ttu-id="5e9d0-365">Zamknij przeglądarkę, jeśli to konieczne, aby powrócić do okna programu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="5e9d0-365">Close the browser if needed, to return to the Visual Studio window.</span></span> <span data-ttu-id="5e9d0-366">Otwórz **StoreManagerController** klasy.</span><span class="sxs-lookup"><span data-stu-id="5e9d0-366">Open **StoreManagerController** class.</span></span> <span data-ttu-id="5e9d0-367">Aby to zrobić, rozwiń węzeł **kontrolerów** folder i kliknij dwukrotnie **StoreManagerController.cs**.</span><span class="sxs-lookup"><span data-stu-id="5e9d0-367">To do this, expand the **Controllers** folder and double-click **StoreManagerController.cs**.</span></span>
2. <span data-ttu-id="5e9d0-368">Zastąp **POST protokołu HTTP, Utwórz** kod metody akcji z następujących czynności:</span><span class="sxs-lookup"><span data-stu-id="5e9d0-368">Replace **HTTP-POST Create** action method code with the following:</span></span>

    <span data-ttu-id="5e9d0-369">(Fragment - kodu *ASP.NET MVC 4 pomocników i formularzy i sprawdzanie poprawności — Ex4 StoreManagerController HTTP POST tworzenie akcji*)</span><span class="sxs-lookup"><span data-stu-id="5e9d0-369">(Code Snippet - *ASP.NET MVC 4 Helpers and Forms and Validation - Ex4 StoreManagerController HTTP- POST Create action*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample14.cs)]

    > [!NOTE]
    > <span data-ttu-id="5e9d0-370">Akcja Utwórz jest bardzo podobne do poprzedniej edycji metody akcji, ale zamiast ustawienia obiektu, ponieważ zmodyfikowane, jego jest dodawany do kontekstu.</span><span class="sxs-lookup"><span data-stu-id="5e9d0-370">The Create action is pretty similar to the previous Edit action method but instead of setting the object as modified, it is being added to the context.</span></span>

<a id="Ex4Task5"></a>

<a id="Task_5_-_Running_the_Application"></a>
#### <a name="task-5---running-the-application"></a><span data-ttu-id="5e9d0-371">Zadanie 5 działania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="5e9d0-371">Task 5 - Running the Application</span></span>

<span data-ttu-id="5e9d0-372">W tym zadaniu zostanie sprawdzić, czy **StoreManager utworzyć** strony widoku umożliwia utworzenie nowego albumu, a następnie przekierowuje do widoku indeksu StoreManager.</span><span class="sxs-lookup"><span data-stu-id="5e9d0-372">In this task, you will test that the **StoreManager Create** View page lets you create a new Album and then redirects to the StoreManager Index View.</span></span>

1. <span data-ttu-id="5e9d0-373">Naciśnij klawisz **F5** do uruchomienia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="5e9d0-373">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="5e9d0-374">Na stronie głównej datę rozpoczęcia projektu.</span><span class="sxs-lookup"><span data-stu-id="5e9d0-374">The project starts in the Home page.</span></span> <span data-ttu-id="5e9d0-375">Zmień adres URL do **/StoreManager/utworzyć**.</span><span class="sxs-lookup"><span data-stu-id="5e9d0-375">Change the URL to **/StoreManager/Create**.</span></span> <span data-ttu-id="5e9d0-376">Należy wypełnić wszystkie pola formularza z danymi dla nowego albumu, tak jak na poniższej ilustracji:</span><span class="sxs-lookup"><span data-stu-id="5e9d0-376">Fill all the form fields with data for a new Album, like the one in the following figure:</span></span>

    <span data-ttu-id="5e9d0-377">![Tworzenie albumu](aspnet-mvc-4-helpers-forms-and-validation/_static/image13.png "tworzenia albumu")</span><span class="sxs-lookup"><span data-stu-id="5e9d0-377">![Creating an Album](aspnet-mvc-4-helpers-forms-and-validation/_static/image13.png "Creating an Album")</span></span>

    <span data-ttu-id="5e9d0-378">*Tworzenie albumu*</span><span class="sxs-lookup"><span data-stu-id="5e9d0-378">*Creating an Album*</span></span>
3. <span data-ttu-id="5e9d0-379">Sprawdź, czy następuje przekierowanie do widoku indeksu StoreManager, która obejmuje nowy Album właśnie utworzony.</span><span class="sxs-lookup"><span data-stu-id="5e9d0-379">Verify that you get redirected to the StoreManager Index View that includes the new Album just created.</span></span>

    <span data-ttu-id="5e9d0-380">![Nowy Album utworzony](aspnet-mvc-4-helpers-forms-and-validation/_static/image14.png "nowy Album utworzone")</span><span class="sxs-lookup"><span data-stu-id="5e9d0-380">![New Album Created](aspnet-mvc-4-helpers-forms-and-validation/_static/image14.png "New Album Created")</span></span>

    <span data-ttu-id="5e9d0-381">*Nowy Album utworzone*</span><span class="sxs-lookup"><span data-stu-id="5e9d0-381">*New Album Created*</span></span>

<a id="Exercise5"></a>

<a id="Exercise_5_Handling_Deletion"></a>
### <a name="exercise-5-handling-deletion"></a><span data-ttu-id="5e9d0-382">Ćwiczenie 5: Obsługa usuwania</span><span class="sxs-lookup"><span data-stu-id="5e9d0-382">Exercise 5: Handling Deletion</span></span>

<span data-ttu-id="5e9d0-383">Możliwość usunięcia albumów nie została jeszcze zaimplementowana.</span><span class="sxs-lookup"><span data-stu-id="5e9d0-383">The ability to delete albums is not yet implemented.</span></span> <span data-ttu-id="5e9d0-384">To jest tym ćwiczeniu będzie o.</span><span class="sxs-lookup"><span data-stu-id="5e9d0-384">This is what this exercise will be about.</span></span> <span data-ttu-id="5e9d0-385">Tak jak wcześniej, zostanie wdrożyć scenariusz usuwania za pomocą dwóch osobnych metod w ramach **StoreManagerController** klasy:</span><span class="sxs-lookup"><span data-stu-id="5e9d0-385">Like before, you will implement the Delete scenario using two separate methods within the **StoreManagerController** class:</span></span>

1. <span data-ttu-id="5e9d0-386">Jedna metoda akcji spowoduje wyświetlenie formularza potwierdzenia</span><span class="sxs-lookup"><span data-stu-id="5e9d0-386">One action method will display a confirmation form</span></span>
2. <span data-ttu-id="5e9d0-387">Druga metoda akcji obsłuży przesyłania formularza</span><span class="sxs-lookup"><span data-stu-id="5e9d0-387">A second action method will handle the form submission</span></span>

<a id="Ex5Task1"></a>

<a id="Task_1_-_Implementing_the_HTTP-GET_Delete_Action_Method"></a>
#### <a name="task-1---implementing-the-http-get-delete-action-method"></a><span data-ttu-id="5e9d0-388">Zadanie 1 - implementacja metody akcji Delete HTTP GET</span><span class="sxs-lookup"><span data-stu-id="5e9d0-388">Task 1 - Implementing the HTTP-GET Delete Action Method</span></span>

<span data-ttu-id="5e9d0-389">W tym zadaniu zaimplementowaniem wersji HTTP GET metody akcji usuwania można pobrać informacji o albumu.</span><span class="sxs-lookup"><span data-stu-id="5e9d0-389">In this task, you will implement the HTTP-GET version of the Delete action method to retrieve the album's information.</span></span>

1. <span data-ttu-id="5e9d0-390">Otwórz **rozpocząć** rozwiązania, znajdujących się na **źródło/Ex5-HandlingDeletion/Begin/** folderu.</span><span class="sxs-lookup"><span data-stu-id="5e9d0-390">Open the **Begin** solution located at **Source/Ex5-HandlingDeletion/Begin/** folder.</span></span> <span data-ttu-id="5e9d0-391">W przeciwnym razie możesz nadal korzystać **zakończenia** uzyskane rozwiązanie, wykonując poprzednim ćwiczeniu.</span><span class="sxs-lookup"><span data-stu-id="5e9d0-391">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

    1. <span data-ttu-id="5e9d0-392">Po otwarciu dostarczonych **rozpocząć** rozwiązania, musisz pobrać niektórych brakujących pakietów NuGet aby kontynuować.</span><span class="sxs-lookup"><span data-stu-id="5e9d0-392">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="5e9d0-393">Aby to zrobić, kliknij przycisk **projektu** menu i wybierz **Zarządzaj pakietami NuGet**.</span><span class="sxs-lookup"><span data-stu-id="5e9d0-393">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
    2. <span data-ttu-id="5e9d0-394">W **Zarządzaj pakietami NuGet** okna dialogowego, kliknij przycisk **przywrócić** celu pobieranie brakujących pakietów.</span><span class="sxs-lookup"><span data-stu-id="5e9d0-394">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
    3. <span data-ttu-id="5e9d0-395">Na koniec Skompiluj rozwiązanie, klikając **kompilacji** | **Kompiluj rozwiązanie**.</span><span class="sxs-lookup"><span data-stu-id="5e9d0-395">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="5e9d0-396">Jedną z zalet przy użyciu narzędzia NuGet jest, że nie masz do wysłania wszystkich bibliotek w projekcie, zmniejszenie jego rozmiar projektu.</span><span class="sxs-lookup"><span data-stu-id="5e9d0-396">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="5e9d0-397">Narzędzia Power NuGet określając wersje pakietów w pliku Packages.config, będzie można pobrać wymaganych bibliotek podczas pierwszego uruchomienia projektu.</span><span class="sxs-lookup"><span data-stu-id="5e9d0-397">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="5e9d0-398">Jest to, dlaczego konieczne będzie wykonanie tych kroków, po otwarciu istniejącego rozwiązania z tego laboratorium.</span><span class="sxs-lookup"><span data-stu-id="5e9d0-398">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="5e9d0-399">Otwórz **StoreManagerController** klasy.</span><span class="sxs-lookup"><span data-stu-id="5e9d0-399">Open **StoreManagerController** class.</span></span> <span data-ttu-id="5e9d0-400">Aby to zrobić, rozwiń węzeł **kontrolerów** folder i kliknij dwukrotnie **StoreManagerController.cs**.</span><span class="sxs-lookup"><span data-stu-id="5e9d0-400">To do this, expand the **Controllers** folder and double-click **StoreManagerController.cs**.</span></span>
3. <span data-ttu-id="5e9d0-401">Akcja usuwania kontrolera jest dokładnie taka sama poprzedniej akcji kontrolera szczegóły magazynu: wysyła zapytanie **albumu** obiektów z bazy danych przy użyciu **identyfikator** podany adres URL i zwraca odpowiednie **widoku**.</span><span class="sxs-lookup"><span data-stu-id="5e9d0-401">The Delete controller action is exactly the same as the previous Store Details controller action: it queries the **album** object from the database using the **id** provided in the URL and returns the appropriate **View**.</span></span> <span data-ttu-id="5e9d0-402">Aby to zrobić, Zamień HTTP GET **usunąć** kod metody akcji z następujących czynności:</span><span class="sxs-lookup"><span data-stu-id="5e9d0-402">To do this, replace the HTTP-GET **Delete** action method code with the following:</span></span>

    <span data-ttu-id="5e9d0-403">(Fragment - kodu *ASP.NET MVC 4 pomocników i formularzy i sprawdzanie poprawności — Ex5 obsługi usunięcia HTTP GET akcję usunięcia*)</span><span class="sxs-lookup"><span data-stu-id="5e9d0-403">(Code Snippet - *ASP.NET MVC 4 Helpers and Forms and Validation - Ex5 Handling Deletion HTTP-GET Delete action*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample15.cs)]
4. <span data-ttu-id="5e9d0-404">Kliknij prawym przyciskiem myszy wewnątrz **usunąć** metody akcji i wybierz **Dodaj widok**.</span><span class="sxs-lookup"><span data-stu-id="5e9d0-404">Right-click inside the **Delete** action method and select **Add View**.</span></span> <span data-ttu-id="5e9d0-405">Zostanie wyświetlone okno dialogowe dodawania widoku.</span><span class="sxs-lookup"><span data-stu-id="5e9d0-405">This will bring up the Add View dialog.</span></span>
5. <span data-ttu-id="5e9d0-406">W oknie dialogowym Dodaj widok, sprawdź, czy nazwa widoku jest **usunąć**.</span><span class="sxs-lookup"><span data-stu-id="5e9d0-406">In the Add View dialog, verify that the View name is **Delete**.</span></span> <span data-ttu-id="5e9d0-407">Wybierz **utworzyć widok jednoznacznie** opcję i zaznacz **albumu (MvcMusicStore.Models)** z **klasa modelu** listy rozwijanej.</span><span class="sxs-lookup"><span data-stu-id="5e9d0-407">Select the **Create a strongly-typed view** option and select **Album (MvcMusicStore.Models)** from the **Model class** drop-down.</span></span> <span data-ttu-id="5e9d0-408">Wybierz **usunąć** z **szablonu szkieletu** listy rozwijanej.</span><span class="sxs-lookup"><span data-stu-id="5e9d0-408">Select **Delete** from the **Scaffold template** drop-down.</span></span> <span data-ttu-id="5e9d0-409">Pozostaw innych pól ich wartości domyślne, a następnie kliknij przycisk **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="5e9d0-409">Leave the other fields with their default value and then click **Add**.</span></span>

    <span data-ttu-id="5e9d0-410">![Dodawanie widoku Delete](aspnet-mvc-4-helpers-forms-and-validation/_static/image15.png "Dodawanie widoku Delete")</span><span class="sxs-lookup"><span data-stu-id="5e9d0-410">![Adding a Delete View](aspnet-mvc-4-helpers-forms-and-validation/_static/image15.png "Adding a Delete View")</span></span>

    <span data-ttu-id="5e9d0-411">*Dodawanie widoku Delete*</span><span class="sxs-lookup"><span data-stu-id="5e9d0-411">*Adding a Delete View*</span></span>
6. <span data-ttu-id="5e9d0-412">Usuń szablon pokazuje wszystkie pola z modelu.</span><span class="sxs-lookup"><span data-stu-id="5e9d0-412">The Delete template shows all the fields from the model.</span></span> <span data-ttu-id="5e9d0-413">Zostaną wyświetlone tylko tytuł albumu.</span><span class="sxs-lookup"><span data-stu-id="5e9d0-413">You will show only the album's title.</span></span> <span data-ttu-id="5e9d0-414">Aby to zrobić, Zamień zawartość widoku z następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="5e9d0-414">To do this, replace the content of the view with the following code:</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample16.cshtml)]

<a id="Ex05Task2"></a>

<a id="Task_2_-_Running_the_Application"></a>
#### <a name="task-2---running-the-application"></a><span data-ttu-id="5e9d0-415">Zadanie 2 — w uruchomionej aplikacji</span><span class="sxs-lookup"><span data-stu-id="5e9d0-415">Task 2 - Running the Application</span></span>

<span data-ttu-id="5e9d0-416">W tym zadaniu zostanie sprawdzić, czy **StoreManager** **usunąć** potwierdzenie usunięcia formularza zostanie wyświetlona strona widoku.</span><span class="sxs-lookup"><span data-stu-id="5e9d0-416">In this task, you will test that the **StoreManager** **Delete** View page displays a confirmation deletion form.</span></span>

1. <span data-ttu-id="5e9d0-417">Naciśnij klawisz **F5** do uruchomienia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="5e9d0-417">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="5e9d0-418">Na stronie głównej datę rozpoczęcia projektu.</span><span class="sxs-lookup"><span data-stu-id="5e9d0-418">The project starts in the Home page.</span></span> <span data-ttu-id="5e9d0-419">Zmień adres URL do **/StoreManager**.</span><span class="sxs-lookup"><span data-stu-id="5e9d0-419">Change the URL to **/StoreManager**.</span></span> <span data-ttu-id="5e9d0-420">Wybierz albumów można usunąć, klikając **usunąć** i upewnij się, że nowy widok została przekazana.</span><span class="sxs-lookup"><span data-stu-id="5e9d0-420">Select one album to delete by clicking **Delete** and verify that the new view is uploaded.</span></span>

    <span data-ttu-id="5e9d0-421">![Usuwanie albumu](aspnet-mvc-4-helpers-forms-and-validation/_static/image16.png "Usuwanie albumu")</span><span class="sxs-lookup"><span data-stu-id="5e9d0-421">![Deleting an Album](aspnet-mvc-4-helpers-forms-and-validation/_static/image16.png "Deleting an Album")</span></span>

    <span data-ttu-id="5e9d0-422">*Usuwanie albumu*</span><span class="sxs-lookup"><span data-stu-id="5e9d0-422">*Deleting an Album*</span></span>

<a id="Ex05Task3"></a>

<a id="Task_3-_Implementing_the_HTTP-POST_Delete_Action_Method"></a>
#### <a name="task-3--implementing-the-http-post-delete-action-method"></a><span data-ttu-id="5e9d0-423">Zadanie 3 — implementacja metody akcji POST protokołu HTTP Delete</span><span class="sxs-lookup"><span data-stu-id="5e9d0-423">Task 3- Implementing the HTTP-POST Delete Action Method</span></span>

<span data-ttu-id="5e9d0-424">W tym zadaniu wdroży wersji HTTP POST metody akcji usuwania, która będzie wywoływana, gdy użytkownik kliknie **usunąć** przycisku.</span><span class="sxs-lookup"><span data-stu-id="5e9d0-424">In this task, you will implement the HTTP-POST version of the Delete action method that will be invoked when a user clicks the **Delete** button.</span></span> <span data-ttu-id="5e9d0-425">Metoda, należy usunąć albumu w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="5e9d0-425">The method should delete the album in the database.</span></span>

1. <span data-ttu-id="5e9d0-426">Zamknij przeglądarkę, jeśli to konieczne, aby powrócić do okna programu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="5e9d0-426">Close the browser if needed, to return to the Visual Studio window.</span></span> <span data-ttu-id="5e9d0-427">Otwórz **StoreManagerController** klasy.</span><span class="sxs-lookup"><span data-stu-id="5e9d0-427">Open **StoreManagerController** class.</span></span> <span data-ttu-id="5e9d0-428">Aby to zrobić, rozwiń węzeł **kontrolerów** folder i kliknij dwukrotnie **StoreManagerController.cs**.</span><span class="sxs-lookup"><span data-stu-id="5e9d0-428">To do this, expand the **Controllers** folder and double-click **StoreManagerController.cs**.</span></span>
2. <span data-ttu-id="5e9d0-429">Zastąp **usunąć HTTP POST** kod metody akcji z następujących czynności:</span><span class="sxs-lookup"><span data-stu-id="5e9d0-429">Replace **HTTP-POST Delete** action method code with the following:</span></span>

    <span data-ttu-id="5e9d0-430">(Fragment - kodu *ASP.NET MVC 4 pomocników i formularzy i sprawdzanie poprawności — Ex5 obsługi usunięcia HTTP POST akcji*)</span><span class="sxs-lookup"><span data-stu-id="5e9d0-430">(Code Snippet - *ASP.NET MVC 4 Helpers and Forms and Validation - Ex5 Handling Deletion HTTP-POST Delete action*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample17.cs)]

<a id="Ex5Task4"></a>

<a id="Task_4_-_Running_the_Application"></a>
#### <a name="task-4---running-the-application"></a><span data-ttu-id="5e9d0-431">Zadanie 4 — uruchamianie aplikacji</span><span class="sxs-lookup"><span data-stu-id="5e9d0-431">Task 4 - Running the Application</span></span>

<span data-ttu-id="5e9d0-432">W tym zadaniu zostanie sprawdzić, czy **StoreManager Delete** strony widoku umożliwia usuwanie albumu, a następnie przekierowuje do widoku indeksu StoreManager.</span><span class="sxs-lookup"><span data-stu-id="5e9d0-432">In this task, you will test that the **StoreManager Delete** View page lets you delete an Album and then redirects to the StoreManager Index View.</span></span>

1. <span data-ttu-id="5e9d0-433">Naciśnij klawisz **F5** do uruchomienia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="5e9d0-433">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="5e9d0-434">Na stronie głównej datę rozpoczęcia projektu.</span><span class="sxs-lookup"><span data-stu-id="5e9d0-434">The project starts in the Home page.</span></span> <span data-ttu-id="5e9d0-435">Zmień adres URL do **/StoreManager**.</span><span class="sxs-lookup"><span data-stu-id="5e9d0-435">Change the URL to **/StoreManager**.</span></span> <span data-ttu-id="5e9d0-436">Wybierz albumów można usunąć, klikając **usunąć.**</span><span class="sxs-lookup"><span data-stu-id="5e9d0-436">Select one album to delete by clicking **Delete.**</span></span> <span data-ttu-id="5e9d0-437">Potwierdź decyzję, klikając **usunąć** przycisk:</span><span class="sxs-lookup"><span data-stu-id="5e9d0-437">Confirm the deletion by clicking **Delete** button:</span></span>

    <span data-ttu-id="5e9d0-438">![Usuwanie albumu](aspnet-mvc-4-helpers-forms-and-validation/_static/image17.png "Usuwanie albumu")</span><span class="sxs-lookup"><span data-stu-id="5e9d0-438">![Deleting an Album](aspnet-mvc-4-helpers-forms-and-validation/_static/image17.png "Deleting an Album")</span></span>

    <span data-ttu-id="5e9d0-439">*Usuwanie albumu*</span><span class="sxs-lookup"><span data-stu-id="5e9d0-439">*Deleting an Album*</span></span>
3. <span data-ttu-id="5e9d0-440">Sprawdź, czy album została usunięta, ponieważ nie ma w **indeksu** strony.</span><span class="sxs-lookup"><span data-stu-id="5e9d0-440">Verify that the album was deleted since it does not appear in the **Index** page.</span></span>

<a id="Exercise6"></a>

<a id="Exercise_6_Adding_Validation"></a>
### <a name="exercise-6-adding-validation"></a><span data-ttu-id="5e9d0-441">Ćwiczenie 6: Dodawanie walidacji</span><span class="sxs-lookup"><span data-stu-id="5e9d0-441">Exercise 6: Adding Validation</span></span>

<span data-ttu-id="5e9d0-442">Obecnie tworzenie i edytowanie formularzy, które zostały spełnione, nie należy wykonywać dowolny rodzaj sprawdzania poprawności.</span><span class="sxs-lookup"><span data-stu-id="5e9d0-442">Currently, the Create and Edit forms you have in place do not perform any kind of validation.</span></span> <span data-ttu-id="5e9d0-443">Jeśli użytkownik opuści wymaganego pola puste lub typ litery w polu Cena, zostanie wyświetlony błąd będzie się z bazy danych.</span><span class="sxs-lookup"><span data-stu-id="5e9d0-443">If the user leaves a required field blank or type letters in the price field, the first error you will get will be from the database.</span></span>

<span data-ttu-id="5e9d0-444">Sprawdzanie poprawności można dodać do aplikacji przez dodanie do własnej klasy modelu adnotacji danych.</span><span class="sxs-lookup"><span data-stu-id="5e9d0-444">You can add validation to the application by adding Data Annotations to your model class.</span></span> <span data-ttu-id="5e9d0-445">Adnotacje danych umożliwiają zawierająca opis reguły, które chcesz zastosować do właściwości modelu i ASP.NET MVC zajmie się wymuszanie i komunikatem odpowiednie dla użytkowników.</span><span class="sxs-lookup"><span data-stu-id="5e9d0-445">Data Annotations allow describing the rules you want applied to your model properties, and ASP.NET MVC will take care of enforcing and displaying appropriate message to users.</span></span>

<a id="Ex06Task1"></a>

<a id="Task_1_-_Adding_Data_Annotations"></a>
#### <a name="task-1---adding-data-annotations"></a><span data-ttu-id="5e9d0-446">Zadanie 1 — Dodawanie adnotacji danych</span><span class="sxs-lookup"><span data-stu-id="5e9d0-446">Task 1 - Adding Data Annotations</span></span>

<span data-ttu-id="5e9d0-447">To zadanie spowoduje dodanie adnotacji danych do modelu albumy, aby ułatwić tworzenie i edytowanie strony ekranu komunikatów dotyczących sprawdzania poprawności, gdy jest to konieczne.</span><span class="sxs-lookup"><span data-stu-id="5e9d0-447">In this task, you will add Data Annotations to the Album Model that will make the Create and Edit page display validation messages when appropriate.</span></span>

<span data-ttu-id="5e9d0-448">Proste klasy modelu, dodawanie adnotacji danych tylko odbywa się przez dodanie **przy użyciu** instrukcji dla **System.ComponentModel.DataAnnotation**, następnie umieszczenie **[wymagane]**atrybutu na odpowiednie właściwości.</span><span class="sxs-lookup"><span data-stu-id="5e9d0-448">For a simple Model class, adding a Data Annotation is just handled by adding a **using** statement for **System.ComponentModel.DataAnnotation**, then placing a **[Required]** attribute on the appropriate properties.</span></span> <span data-ttu-id="5e9d0-449">Poniższy przykład spowodowałoby **nazwa** właściwości wymaganego pola w widoku.</span><span class="sxs-lookup"><span data-stu-id="5e9d0-449">The following example would make the **Name** property a required field in the View.</span></span>

[!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample18.cs)]

<span data-ttu-id="5e9d0-450">To nieco bardziej złożone w przypadkach, takich jak ta aplikacja gdzie jest generowany modelu danych jednostki.</span><span class="sxs-lookup"><span data-stu-id="5e9d0-450">This is a little more complex in cases like this application where the Entity Data Model is generated.</span></span> <span data-ttu-id="5e9d0-451">Jeśli dodano adnotacji danych bezpośrednio do klasy modeli, będą one zastąpione po zaktualizowaniu modelu z bazy danych.</span><span class="sxs-lookup"><span data-stu-id="5e9d0-451">If you added Data Annotations directly to the model classes, they would be overwritten if you update the model from the database.</span></span> <span data-ttu-id="5e9d0-452">Zamiast tego możesz wprowadzić korzystanie z klasy częściowe metadanych, które będą istnieć do przechowywania adnotacje i są skojarzone z tym modelem klasy przy użyciu **[MetadataType]** atrybutu.</span><span class="sxs-lookup"><span data-stu-id="5e9d0-452">Instead, you can make use of metadata partial classes which will exist to hold the annotations and are associated with the model classes using the **[MetadataType]** attribute.</span></span>

1. <span data-ttu-id="5e9d0-453">Otwórz **rozpocząć** rozwiązania, znajdujących się na **źródło/Ex6-AddingValidation/Begin/** folderu.</span><span class="sxs-lookup"><span data-stu-id="5e9d0-453">Open the **Begin** solution located at **Source/Ex6-AddingValidation/Begin/** folder.</span></span> <span data-ttu-id="5e9d0-454">W przeciwnym razie możesz nadal korzystać **zakończenia** uzyskane rozwiązanie, wykonując poprzednim ćwiczeniu.</span><span class="sxs-lookup"><span data-stu-id="5e9d0-454">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

    1. <span data-ttu-id="5e9d0-455">Po otwarciu dostarczonych **rozpocząć** rozwiązania, musisz pobrać niektórych brakujących pakietów NuGet aby kontynuować.</span><span class="sxs-lookup"><span data-stu-id="5e9d0-455">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="5e9d0-456">Aby to zrobić, kliknij przycisk **projektu** menu i wybierz **Zarządzaj pakietami NuGet**.</span><span class="sxs-lookup"><span data-stu-id="5e9d0-456">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
    2. <span data-ttu-id="5e9d0-457">W **Zarządzaj pakietami NuGet** okna dialogowego, kliknij przycisk **przywrócić** celu pobieranie brakujących pakietów.</span><span class="sxs-lookup"><span data-stu-id="5e9d0-457">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
    3. <span data-ttu-id="5e9d0-458">Na koniec Skompiluj rozwiązanie, klikając **kompilacji** | **Kompiluj rozwiązanie**.</span><span class="sxs-lookup"><span data-stu-id="5e9d0-458">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="5e9d0-459">Jedną z zalet przy użyciu narzędzia NuGet jest, że nie masz do wysłania wszystkich bibliotek w projekcie, zmniejszenie jego rozmiar projektu.</span><span class="sxs-lookup"><span data-stu-id="5e9d0-459">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="5e9d0-460">Narzędzia Power NuGet określając wersje pakietów w pliku Packages.config, będzie można pobrać wymaganych bibliotek podczas pierwszego uruchomienia projektu.</span><span class="sxs-lookup"><span data-stu-id="5e9d0-460">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="5e9d0-461">Jest to, dlaczego konieczne będzie wykonanie tych kroków, po otwarciu istniejącego rozwiązania z tego laboratorium.</span><span class="sxs-lookup"><span data-stu-id="5e9d0-461">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="5e9d0-462">Otwórz **Album.cs** z **modele** folderu.</span><span class="sxs-lookup"><span data-stu-id="5e9d0-462">Open the **Album.cs** from the **Models** folder.</span></span>
3. <span data-ttu-id="5e9d0-463">Zastąp **Album.cs** zawartości ze wyróżniony kod, aby wyglądały one podobnie do następującej:</span><span class="sxs-lookup"><span data-stu-id="5e9d0-463">Replace **Album.cs** content with the highlighted code, so that it looks like the following:</span></span>

    > [!NOTE]
    > <span data-ttu-id="5e9d0-464">Wiersz **[DisplayFormat(ConvertEmptyStringToNull=false)]** wskazuje, czy puste ciągi z modelu nie będą konwertowane do wartości null po zaktualizowaniu pole danych w źródle danych.</span><span class="sxs-lookup"><span data-stu-id="5e9d0-464">The line **[DisplayFormat(ConvertEmptyStringToNull=false)]** indicates that empty strings from the model won't be converted to null when the data field is updated in the data source.</span></span> <span data-ttu-id="5e9d0-465">To ustawienie można będzie zapobiec Wystąpił wyjątek podczas programu Entity Framework przed adnotacji danych sprawdza poprawność pól przypisuje wartości null do modelu.</span><span class="sxs-lookup"><span data-stu-id="5e9d0-465">This setting will avoid an exception when the Entity Framework assigns null values to the model before Data Annotation validates the fields.</span></span>

    <span data-ttu-id="5e9d0-466">(Fragment - kodu *ASP.NET MVC 4 pomocników i formularzy i sprawdzanie poprawności — częściowej klasy metadanych albumu Ex6*)</span><span class="sxs-lookup"><span data-stu-id="5e9d0-466">(Code Snippet - *ASP.NET MVC 4 Helpers and Forms and Validation - Ex6 Album metadata partial class*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample19.cs)]

    > [!NOTE]
    > <span data-ttu-id="5e9d0-467">To **albumu** ma częściowej klasy **MetadataType** atrybut, który wskazuje **AlbumMetaData** klasy dla adnotacji danych.</span><span class="sxs-lookup"><span data-stu-id="5e9d0-467">This **Album** partial class has a **MetadataType** attribute which points to the **AlbumMetaData** class for the Data Annotations.</span></span> <span data-ttu-id="5e9d0-468">Oto niektóre atrybuty adnotacji danych używanego dla adnotacji modelu albumu:</span><span class="sxs-lookup"><span data-stu-id="5e9d0-468">These are some of the Data Annotation attributes you are using to annotate the Album model:</span></span>
    > 
    > - <span data-ttu-id="5e9d0-469">Niewymagana — wskazuje, że właściwość jest polem obowiązkowym</span><span class="sxs-lookup"><span data-stu-id="5e9d0-469">Required - Indicates that the property is a required field</span></span>
    > - <span data-ttu-id="5e9d0-470">Nazwa wyświetlana — Określa tekst, który ma być używany z pola formularza i komunikatów dotyczących sprawdzania poprawności</span><span class="sxs-lookup"><span data-stu-id="5e9d0-470">DisplayName - Defines the text to be used on form fields and validation messages</span></span>
    > - <span data-ttu-id="5e9d0-471">DisplayFormat — określa sposób wyświetlania i sformatowane pola danych.</span><span class="sxs-lookup"><span data-stu-id="5e9d0-471">DisplayFormat - Specifies how data fields are displayed and formatted.</span></span>
    > - <span data-ttu-id="5e9d0-472">StringLength — określa maksymalną długość pola ciągu</span><span class="sxs-lookup"><span data-stu-id="5e9d0-472">StringLength - Defines a maximum length for a string field</span></span>
    > - <span data-ttu-id="5e9d0-473">Zakres — zapewnia maksymalne i minimalne wartości pola numerycznego</span><span class="sxs-lookup"><span data-stu-id="5e9d0-473">Range - Gives a maximum and minimum value for a numeric field</span></span>
    > - <span data-ttu-id="5e9d0-474">ScaffoldColumn — umożliwia ukrywanie pól z edytora formularzy</span><span class="sxs-lookup"><span data-stu-id="5e9d0-474">ScaffoldColumn - Allows hiding fields from editor forms</span></span>

<a id="Ex06Task2"></a>

<a id="Task_2_-_Running_the_Application"></a>
#### <a name="task-2---running-the-application"></a><span data-ttu-id="5e9d0-475">Zadanie 2 — w uruchomionej aplikacji</span><span class="sxs-lookup"><span data-stu-id="5e9d0-475">Task 2 - Running the Application</span></span>

<span data-ttu-id="5e9d0-476">To zadanie będzie testowane czy tworzenie i edytowanie strony weryfikacji pól, przy użyciu nazw wyświetlanych w ostatnim zadaniem.</span><span class="sxs-lookup"><span data-stu-id="5e9d0-476">In this task, you will test that the Create and Edit pages validate fields, using the display names chosen in the last task.</span></span>

1. <span data-ttu-id="5e9d0-477">Naciśnij klawisz **F5** do uruchomienia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="5e9d0-477">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="5e9d0-478">Na stronie głównej datę rozpoczęcia projektu.</span><span class="sxs-lookup"><span data-stu-id="5e9d0-478">The project starts in the Home page.</span></span> <span data-ttu-id="5e9d0-479">Zmień adres URL do **/StoreManager/utworzyć**.</span><span class="sxs-lookup"><span data-stu-id="5e9d0-479">Change the URL to **/StoreManager/Create**.</span></span> <span data-ttu-id="5e9d0-480">Sprawdź, czy nazwy wyświetlane są zgodne z typami klasy częściowej (takich jak **albumów graficznych URL** zamiast **AlbumArtUrl**)</span><span class="sxs-lookup"><span data-stu-id="5e9d0-480">Verify that the display names match the ones in the partial class (like **Album Art URL** instead of **AlbumArtUrl**)</span></span>
3. <span data-ttu-id="5e9d0-481">Kliknij przycisk **Utwórz**, bez wypełniania formularza.</span><span class="sxs-lookup"><span data-stu-id="5e9d0-481">Click **Create**, without filling the form.</span></span> <span data-ttu-id="5e9d0-482">Sprawdź, czy możesz uzyskać odpowiednie komunikatów dotyczących sprawdzania poprawności.</span><span class="sxs-lookup"><span data-stu-id="5e9d0-482">Verify that you get the corresponding validation messages.</span></span>

    <span data-ttu-id="5e9d0-483">![Zweryfikowany pola na stronie Utwórz](aspnet-mvc-4-helpers-forms-and-validation/_static/image18.png "zweryfikowane pola na stronie Utwórz")</span><span class="sxs-lookup"><span data-stu-id="5e9d0-483">![Validated fields in the Create page](aspnet-mvc-4-helpers-forms-and-validation/_static/image18.png "Validated fields in the Create page")</span></span>

    <span data-ttu-id="5e9d0-484">*Sprawdzone pola na stronie Utwórz*</span><span class="sxs-lookup"><span data-stu-id="5e9d0-484">*Validated fields in the Create page*</span></span>
4. <span data-ttu-id="5e9d0-485">Możesz sprawdzić, czy występuje takie same z **Edytuj** strony.</span><span class="sxs-lookup"><span data-stu-id="5e9d0-485">You can verify that the same occurs with the **Edit** page.</span></span> <span data-ttu-id="5e9d0-486">Zmień adres URL do **/StoreManager/Edit/1** i upewnij się, że nazwy wyświetlane są zgodne z typami klasy częściowej (takich jak **albumów graficznych URL** zamiast **AlbumArtUrl**).</span><span class="sxs-lookup"><span data-stu-id="5e9d0-486">Change the URL to **/StoreManager/Edit/1** and verify that the display names match the ones in the partial class (like **Album Art URL** instead of **AlbumArtUrl**).</span></span> <span data-ttu-id="5e9d0-487">Pusty **tytuł** i **cen** pola i kliknij przycisk **zapisać**.</span><span class="sxs-lookup"><span data-stu-id="5e9d0-487">Empty the **Title** and **Price** fields and click **Save**.</span></span> <span data-ttu-id="5e9d0-488">Sprawdź, czy możesz uzyskać odpowiednie komunikatów dotyczących sprawdzania poprawności.</span><span class="sxs-lookup"><span data-stu-id="5e9d0-488">Verify that you get the corresponding validation messages.</span></span>

    ![Sprawdzone pola na stronie edycji](aspnet-mvc-4-helpers-forms-and-validation/_static/image19.png)

    <span data-ttu-id="5e9d0-490">*Sprawdzone pola na stronie edycji*</span><span class="sxs-lookup"><span data-stu-id="5e9d0-490">*Validated fields in the Edit page*</span></span>

<a id="Exercise7"></a>

<a id="Exercise_7_Using_Unobtrusive_jQuery_at_Client_Side"></a>
### <a name="exercise-7-using-unobtrusive-jquery-at-client-side"></a><span data-ttu-id="5e9d0-491">Ćwiczenie 7: Przy użyciu jQuery dyskretny kod po stronie klienta</span><span class="sxs-lookup"><span data-stu-id="5e9d0-491">Exercise 7: Using Unobtrusive jQuery at Client Side</span></span>

<span data-ttu-id="5e9d0-492">W tym ćwiczeniu dowiesz się, jak włączyć dotyczącą weryfikacji jQuery MVC 4 dyskretny kod po stronie klienta.</span><span class="sxs-lookup"><span data-stu-id="5e9d0-492">In this exercise, you will learn how to enable MVC 4 Unobtrusive jQuery validation at client side.</span></span>

> [!NOTE]
> <span data-ttu-id="5e9d0-493">Dyskretnego kodu jQuery używa prefiksu danych ajax JavaScript do wywołania metody akcji na serwer, a nie emisji intrusively wbudowane skrypty klienta.</span><span class="sxs-lookup"><span data-stu-id="5e9d0-493">The Unobtrusive jQuery uses data-ajax prefix JavaScript to invoke action methods on the server rather than intrusively emitting inline client scripts.</span></span>


<a id="Ex7Task1"></a>

<a id="Task_1_-_Running_the_Application_before_Enabling_Unobtrusive_jQuery"></a>
#### <a name="task-1---running-the-application-before-enabling-unobtrusive-jquery"></a><span data-ttu-id="5e9d0-494">Zadanie 1 - uruchamiania aplikacji przed włączeniem dyskretnego kodu jQuery</span><span class="sxs-lookup"><span data-stu-id="5e9d0-494">Task 1 - Running the Application before Enabling Unobtrusive jQuery</span></span>

<span data-ttu-id="5e9d0-495">W ramach tego zadania należy uruchomić aplikację przed tym jQuery, aby porównać oba modele sprawdzania poprawności.</span><span class="sxs-lookup"><span data-stu-id="5e9d0-495">In this task, you will run the application before including jQuery in order to compare both validation models.</span></span>

1. <span data-ttu-id="5e9d0-496">Otwórz **rozpocząć** rozwiązania, znajdujących się na **źródło/Ex7-UnobtrusivejQueryValidation/Begin/** folderu.</span><span class="sxs-lookup"><span data-stu-id="5e9d0-496">Open the **Begin** solution located at **Source/Ex7-UnobtrusivejQueryValidation/Begin/** folder.</span></span> <span data-ttu-id="5e9d0-497">W przeciwnym razie możesz nadal korzystać **zakończenia** uzyskane rozwiązanie, wykonując poprzednim ćwiczeniu.</span><span class="sxs-lookup"><span data-stu-id="5e9d0-497">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

    1. <span data-ttu-id="5e9d0-498">Po otwarciu dostarczonych **rozpocząć** rozwiązania, musisz pobrać niektórych brakujących pakietów NuGet aby kontynuować.</span><span class="sxs-lookup"><span data-stu-id="5e9d0-498">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="5e9d0-499">Aby to zrobić, kliknij przycisk **projektu** menu i wybierz **Zarządzaj pakietami NuGet**.</span><span class="sxs-lookup"><span data-stu-id="5e9d0-499">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
    2. <span data-ttu-id="5e9d0-500">W **Zarządzaj pakietami NuGet** okna dialogowego, kliknij przycisk **przywrócić** celu pobieranie brakujących pakietów.</span><span class="sxs-lookup"><span data-stu-id="5e9d0-500">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
    3. <span data-ttu-id="5e9d0-501">Na koniec Skompiluj rozwiązanie, klikając **kompilacji** | **Kompiluj rozwiązanie**.</span><span class="sxs-lookup"><span data-stu-id="5e9d0-501">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="5e9d0-502">Jedną z zalet przy użyciu narzędzia NuGet jest, że nie masz do wysłania wszystkich bibliotek w projekcie, zmniejszenie jego rozmiar projektu.</span><span class="sxs-lookup"><span data-stu-id="5e9d0-502">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="5e9d0-503">Narzędzia Power NuGet określając wersje pakietów w pliku Packages.config, będzie można pobrać wymaganych bibliotek podczas pierwszego uruchomienia projektu.</span><span class="sxs-lookup"><span data-stu-id="5e9d0-503">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="5e9d0-504">Jest to, dlaczego konieczne będzie wykonanie tych kroków, po otwarciu istniejącego rozwiązania z tego laboratorium.</span><span class="sxs-lookup"><span data-stu-id="5e9d0-504">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="5e9d0-505">Naciśnij klawisz **F5** do uruchomienia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="5e9d0-505">Press **F5** to run the application.</span></span>
3. <span data-ttu-id="5e9d0-506">Na stronie głównej datę rozpoczęcia projektu.</span><span class="sxs-lookup"><span data-stu-id="5e9d0-506">The project starts in the Home page.</span></span> <span data-ttu-id="5e9d0-507">Przeglądaj **/StoreManager/Utwórz** i kliknij przycisk **Utwórz** bez wypełniania formularza, aby sprawdzić, czy możesz uzyskać komunikatów dotyczących sprawdzania poprawności:</span><span class="sxs-lookup"><span data-stu-id="5e9d0-507">Browse **/StoreManager/Create** and click **Create** without filling the form to verify that you get validation messages:</span></span>

    <span data-ttu-id="5e9d0-508">![Sprawdzanie poprawności klienta wyłączone](aspnet-mvc-4-helpers-forms-and-validation/_static/image20.png "weryfikacji klienta wyłączone")</span><span class="sxs-lookup"><span data-stu-id="5e9d0-508">![Client validation disabled](aspnet-mvc-4-helpers-forms-and-validation/_static/image20.png "Client validation disabled")</span></span>

    <span data-ttu-id="5e9d0-509">*Sprawdzanie poprawności klienta wyłączone*</span><span class="sxs-lookup"><span data-stu-id="5e9d0-509">*Client validation disabled*</span></span>
4. <span data-ttu-id="5e9d0-510">W przeglądarce otwórz kod źródłowy HTML:</span><span class="sxs-lookup"><span data-stu-id="5e9d0-510">In the browser, open the HTML source code:</span></span>

    [!code-html[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample20.html)]

<a id="Ex7Task2"></a>

<a id="Task_2_-_Enabling_Unobtrusive_Client_Validation"></a>
#### <a name="task-2---enabling-unobtrusive-client-validation"></a><span data-ttu-id="5e9d0-511">Zadanie 2 — Włączanie sprawdzania poprawności dyskretnego kodu klienta</span><span class="sxs-lookup"><span data-stu-id="5e9d0-511">Task 2 - Enabling Unobtrusive Client Validation</span></span>

<span data-ttu-id="5e9d0-512">To zadanie spowoduje włączenie jQuery **sprawdzania poprawności dyskretnego kodu klienta** z **Web.config** pliku, który jest domyślnie ustawiony na wartość false w wszystkie nowe projekty programu ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="5e9d0-512">In this task, you will enable jQuery **unobtrusive client validation** from **Web.config** file, which is by default set to false in all new ASP.NET MVC 4 projects.</span></span> <span data-ttu-id="5e9d0-513">Ponadto należy dodać się, że niezbędne skrypty odwołania, aby jQuery pracy dyskretny kod weryfikacji klienta.</span><span class="sxs-lookup"><span data-stu-id="5e9d0-513">Additionally, you will add the necessary scripts references to make jQuery Unobtrusive Client Validation work.</span></span>

1. <span data-ttu-id="5e9d0-514">Otwórz **Web.Config** plików w katalogu głównym projektu i upewnij się, że **ClientValidationEnabled** i **UnobtrusiveJavaScriptEnabled** klucze wartości są ustawiane na **true**.</span><span class="sxs-lookup"><span data-stu-id="5e9d0-514">Open **Web.Config** file at project root, and make sure that the **ClientValidationEnabled** and **UnobtrusiveJavaScriptEnabled** keys values are set to **true**.</span></span>

    [!code-xml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample21.xml)]

    > [!NOTE]
    > <span data-ttu-id="5e9d0-515">Można także włączyć sprawdzanie poprawności klienta przez kod w Global.asax.cs, aby uzyskać takie same wyniki:</span><span class="sxs-lookup"><span data-stu-id="5e9d0-515">You can also enable client validation by code at Global.asax.cs to get the same results:</span></span>
    > 
    > <span data-ttu-id="5e9d0-516">**HtmlHelper.ClientValidationEnabled = true;**</span><span class="sxs-lookup"><span data-stu-id="5e9d0-516">**HtmlHelper.ClientValidationEnabled = true;**</span></span>
    > 
    > <span data-ttu-id="5e9d0-517">Ponadto można przypisać atrybutu ClientValidationEnabled w żadnym kontrolerze mają zachowania niestandardowego.</span><span class="sxs-lookup"><span data-stu-id="5e9d0-517">Additionally, you can assign ClientValidationEnabled attribute into any controller to have a custom behavior.</span></span>
2. <span data-ttu-id="5e9d0-518">Otwórz **Create.cshtml** w **Views\StoreManager**.</span><span class="sxs-lookup"><span data-stu-id="5e9d0-518">Open **Create.cshtml** at **Views\StoreManager**.</span></span>
3. <span data-ttu-id="5e9d0-519">Upewnij się, że następujące pliki skryptu **jquery.validate** i **jquery.validate.unobtrusive**, odwołuje się widok poprzez &quot; **~/bundles/jqueryval** &quot; pakietu.</span><span class="sxs-lookup"><span data-stu-id="5e9d0-519">Make sure the following script files, **jquery.validate** and **jquery.validate.unobtrusive**, are referenced in the view throught the &quot;**~/bundles/jqueryval**&quot; bundle.</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample22.cshtml)]

    > [!NOTE]
    > <span data-ttu-id="5e9d0-520">Te biblioteki jQuery zostaną uwzględnione w nowych projektów MVC 4.</span><span class="sxs-lookup"><span data-stu-id="5e9d0-520">All these jQuery libraries are included in MVC 4 new projects.</span></span> <span data-ttu-id="5e9d0-521">Można znaleźć więcej bibliotek w **/skrypty** folderu projektu należy.</span><span class="sxs-lookup"><span data-stu-id="5e9d0-521">You can find more libraries in the **/Scripts** folder of you project.</span></span>
    > 
    > <span data-ttu-id="5e9d0-522">Aby tej weryfikacji pracy biblioteki, musisz dodać odwołanie do biblioteki framework jQuery.</span><span class="sxs-lookup"><span data-stu-id="5e9d0-522">In order to make this validation libraries work, you need to add a reference to the jQuery framework library.</span></span> <span data-ttu-id="5e9d0-523">Ponieważ to odwołanie zostało już dodane na  **\_Layout.cshtml** plików, nie trzeba go dodać w tego widoku.</span><span class="sxs-lookup"><span data-stu-id="5e9d0-523">Since this reference is already added in the **\_Layout.cshtml** file, you do not need to add it in this particular view.</span></span>

<a id="Ex7Task3"></a>

<a id="Task_3_-_Running_the_Application_Using_Unobtrusive_jQuery_Validation"></a>
#### <a name="task-3---running-the-application-using-unobtrusive-jquery-validation"></a><span data-ttu-id="5e9d0-524">Zadanie 3 — uruchomiona dyskretnego kodu przy użyciu aplikacji jQuery sprawdzania poprawności</span><span class="sxs-lookup"><span data-stu-id="5e9d0-524">Task 3 - Running the Application Using Unobtrusive jQuery Validation</span></span>

<span data-ttu-id="5e9d0-525">W tym zadaniu zostanie sprawdzić, czy **StoreManager** utworzyć widok szablonu przeprowadza weryfikacji po stronie klienta przy użyciu biblioteki jQuery podczas tworzenia nowego albumu.</span><span class="sxs-lookup"><span data-stu-id="5e9d0-525">In this task, you will test that the **StoreManager** create view template performs client side validation using jQuery libraries when the user creates a new album.</span></span>

1. <span data-ttu-id="5e9d0-526">Naciśnij klawisz **F5** do uruchomienia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="5e9d0-526">Press **F5** to run the application.</span></span>
2. <span data-ttu-id="5e9d0-527">Na stronie głównej datę rozpoczęcia projektu.</span><span class="sxs-lookup"><span data-stu-id="5e9d0-527">The project starts in the Home page.</span></span> <span data-ttu-id="5e9d0-528">Przeglądaj **/StoreManager/Utwórz** i kliknij przycisk **Utwórz** bez wypełniania formularza, aby sprawdzić, czy możesz uzyskać komunikatów dotyczących sprawdzania poprawności:</span><span class="sxs-lookup"><span data-stu-id="5e9d0-528">Browse **/StoreManager/Create** and click **Create** without filling the form to verify that you get validation messages:</span></span>

    <span data-ttu-id="5e9d0-529">![Sprawdzanie poprawności klienta z jQuery włączone](aspnet-mvc-4-helpers-forms-and-validation/_static/image21.png "weryfikacji klienta z jQuery włączone")</span><span class="sxs-lookup"><span data-stu-id="5e9d0-529">![Client validation with jQuery enabled](aspnet-mvc-4-helpers-forms-and-validation/_static/image21.png "Client validation with jQuery enabled")</span></span>

    <span data-ttu-id="5e9d0-530">*Sprawdzanie poprawności klienta z jQuery włączone*</span><span class="sxs-lookup"><span data-stu-id="5e9d0-530">*Client validation with jQuery enabled*</span></span>
3. <span data-ttu-id="5e9d0-531">W przeglądarce otwórz kod źródłowy Utwórz widok:</span><span class="sxs-lookup"><span data-stu-id="5e9d0-531">In the browser, open the source code for Create view:</span></span>

    [!code-html[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample23.html)]

    > [!NOTE]
    > <span data-ttu-id="5e9d0-532">Dla każdej reguły weryfikacji klienta jQuery dyskretny kod dodaje atrybut z danymi-val -*rulename*=&quot;*komunikat*&quot;.</span><span class="sxs-lookup"><span data-stu-id="5e9d0-532">For each client validation rule, Unobtrusive jQuery adds an attribute with data-val-*rulename*=&quot;*message*&quot;.</span></span> <span data-ttu-id="5e9d0-533">Poniżej znajduje się lista znaczników tej Unobtrusive jQuery wstawia do pola wejściowego html do sprawdzania poprawności klienta:</span><span class="sxs-lookup"><span data-stu-id="5e9d0-533">Below is a list of tags that Unobtrusive jQuery inserts into the html input field to perform client validation:</span></span>
    > 
    > - <span data-ttu-id="5e9d0-534">Val danych</span><span class="sxs-lookup"><span data-stu-id="5e9d0-534">Data-val</span></span>
    > - <span data-ttu-id="5e9d0-535">Dane val numerów</span><span class="sxs-lookup"><span data-stu-id="5e9d0-535">Data-val-number</span></span>
    > - <span data-ttu-id="5e9d0-536">Zakres danych val</span><span class="sxs-lookup"><span data-stu-id="5e9d0-536">Data-val-range</span></span>
    > - <span data-ttu-id="5e9d0-537">Dane val zakresu min / danych val zakresu max.</span><span class="sxs-lookup"><span data-stu-id="5e9d0-537">Data-val-range-min / Data-val-range-max</span></span>
    > - <span data-ttu-id="5e9d0-538">Wymagane wartości danych</span><span class="sxs-lookup"><span data-stu-id="5e9d0-538">Data-val-required</span></span>
    > - <span data-ttu-id="5e9d0-539">Długość danych val</span><span class="sxs-lookup"><span data-stu-id="5e9d0-539">Data-val-length</span></span>
    > - <span data-ttu-id="5e9d0-540">Dane val długość max / danych val długość min</span><span class="sxs-lookup"><span data-stu-id="5e9d0-540">Data-val-length-max / Data-val-length-min</span></span>
    > 
    > <span data-ttu-id="5e9d0-541">Wszystkie wartości są wypełniane modelu **adnotacji danych elementu**.</span><span class="sxs-lookup"><span data-stu-id="5e9d0-541">All the data values are filled with model **Data Annotation**.</span></span> <span data-ttu-id="5e9d0-542">Następnie całą logikę, która działa po stronie serwera może działać po stronie klienta.</span><span class="sxs-lookup"><span data-stu-id="5e9d0-542">Then, all the logic that works at server side can be run at client side.</span></span> <span data-ttu-id="5e9d0-543">Na przykład atrybut cen ma następujące adnotacji danych w modelu:</span><span class="sxs-lookup"><span data-stu-id="5e9d0-543">For example, Price attribute has the following data annotation in the model:</span></span>
    > 
    > [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample24.cs)]
    > 
    > <span data-ttu-id="5e9d0-544">Po zakończeniu korzystania z dyskretnego kodu jQuery jest wygenerowanego kodu:</span><span class="sxs-lookup"><span data-stu-id="5e9d0-544">After using Unobtrusive jQuery, the generated code is:</span></span>
    >  
    > [!code-html[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample25.html)]

* * *

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a><span data-ttu-id="5e9d0-545">Podsumowanie</span><span class="sxs-lookup"><span data-stu-id="5e9d0-545">Summary</span></span>

<span data-ttu-id="5e9d0-546">Wykonując tego laboratorium Hands-On zapoznaniu umożliwienie użytkownikom na zmianę danych przechowywanych w bazie danych przy użyciu następujących czynności:</span><span class="sxs-lookup"><span data-stu-id="5e9d0-546">By completing this Hands-On Lab you have learned how to enable users to change the data stored in the database with the use of the following:</span></span>

- <span data-ttu-id="5e9d0-547">Indeks, tworzenia, edytowania, usuwania, takich jak akcji kontrolera</span><span class="sxs-lookup"><span data-stu-id="5e9d0-547">Controller actions like Index, Create, Edit, Delete</span></span>
- <span data-ttu-id="5e9d0-548">Funkcja szkieletów ASP.NET MVC do wyświetlania właściwości tabeli HTML</span><span class="sxs-lookup"><span data-stu-id="5e9d0-548">ASP.NET MVC's scaffolding feature for displaying properties in an HTML table</span></span>
- <span data-ttu-id="5e9d0-549">Środowisko niestandardowych pomocników HTML zwiększające użytkownika</span><span class="sxs-lookup"><span data-stu-id="5e9d0-549">Custom HTML helpers to improve user experience</span></span>
- <span data-ttu-id="5e9d0-550">Metody akcji, które reagują na HTTP GET lub POST protokołu HTTP wywołania</span><span class="sxs-lookup"><span data-stu-id="5e9d0-550">Action methods that react to either HTTP-GET or HTTP-POST calls</span></span>
- <span data-ttu-id="5e9d0-551">Szablon udostępniony edytora dla podobnych szablonów widoków, takich jak tworzenie i edytowanie</span><span class="sxs-lookup"><span data-stu-id="5e9d0-551">A shared editor template for similar View templates like Create and Edit</span></span>
- <span data-ttu-id="5e9d0-552">Formularz elementów, takich jak listy rozwijane</span><span class="sxs-lookup"><span data-stu-id="5e9d0-552">Form elements like drop-downs</span></span>
- <span data-ttu-id="5e9d0-553">Adnotacje danych na potrzeby sprawdzania poprawności modelu</span><span class="sxs-lookup"><span data-stu-id="5e9d0-553">Data annotations for Model validation</span></span>
- <span data-ttu-id="5e9d0-554">Weryfikacja po stronie klienta za pomocą biblioteki dyskretnego kodu jQuery</span><span class="sxs-lookup"><span data-stu-id="5e9d0-554">Client Side Validation using jQuery Unobtrusive library</span></span>

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a><span data-ttu-id="5e9d0-555">Dodatek A: Instalowanie programu Visual Studio Express 2012 for Web</span><span class="sxs-lookup"><span data-stu-id="5e9d0-555">Appendix A: Installing Visual Studio Express 2012 for Web</span></span>

<span data-ttu-id="5e9d0-556">Można zainstalować **Microsoft Visual Studio Express 2012 for Web** lub innym &quot;Express&quot; przy użyciu wersji  **[Instalatora platformy sieci Web firmy Microsoft](https://www.microsoft.com/web/downloads/platform.aspx)** .</span><span class="sxs-lookup"><span data-stu-id="5e9d0-556">You can install **Microsoft Visual Studio Express 2012 for Web** or another &quot;Express&quot; version using the **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**.</span></span> <span data-ttu-id="5e9d0-557">Poniższe instrukcje przedstawiono czynności wymagane do zainstalowania *programu Visual studio Express 2012 for Web* przy użyciu *Instalatora platformy sieci Web firmy Microsoft*.</span><span class="sxs-lookup"><span data-stu-id="5e9d0-557">The following instructions guide you through the steps required to install *Visual studio Express 2012 for Web* using *Microsoft Web Platform Installer*.</span></span>

1. <span data-ttu-id="5e9d0-558">Przejdź do [ [https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169).</span><span class="sxs-lookup"><span data-stu-id="5e9d0-558">Go to [[https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169).</span></span> <span data-ttu-id="5e9d0-559">Alternatywnie, jeśli została już zainstalowana Instalatora platformy sieci Web, można otworzyć go i Wyszukaj produkt &quot; *programu Visual Studio Express 2012 for Web z zestawem Windows Azure SDK*&quot;.</span><span class="sxs-lookup"><span data-stu-id="5e9d0-559">Alternatively, if you already have installed Web Platform Installer, you can open it and search for the product &quot;*Visual Studio Express 2012 for Web with Windows Azure SDK*&quot;.</span></span>
2. <span data-ttu-id="5e9d0-560">Polecenie **teraz zainstalować**.</span><span class="sxs-lookup"><span data-stu-id="5e9d0-560">Click on **Install Now**.</span></span> <span data-ttu-id="5e9d0-561">Jeśli nie masz **Instalatora platformy sieci Web** nastąpi przekierowanie do pobrania i zainstalowania go najpierw.</span><span class="sxs-lookup"><span data-stu-id="5e9d0-561">If you do not have **Web Platform Installer** you will be redirected to download and install it first.</span></span>
3. <span data-ttu-id="5e9d0-562">Raz **Instalatora platformy sieci Web** jest otwarty, kliknij przycisk **zainstalować** można uruchomić Instalatora.</span><span class="sxs-lookup"><span data-stu-id="5e9d0-562">Once **Web Platform Installer** is open, click **Install** to start the setup.</span></span>

    <span data-ttu-id="5e9d0-563">![Instalowanie programu Visual Studio Express](aspnet-mvc-4-helpers-forms-and-validation/_static/image22.png "instalacji programu Visual Studio Express")</span><span class="sxs-lookup"><span data-stu-id="5e9d0-563">![Install Visual Studio Express](aspnet-mvc-4-helpers-forms-and-validation/_static/image22.png "Install Visual Studio Express")</span></span>

    <span data-ttu-id="5e9d0-564">*Instalowanie programu Visual Studio Express*</span><span class="sxs-lookup"><span data-stu-id="5e9d0-564">*Install Visual Studio Express*</span></span>
4. <span data-ttu-id="5e9d0-565">Odczytywanie wszystkich produktów licencji i warunków, a następnie kliknij przycisk **akceptuję** aby kontynuować.</span><span class="sxs-lookup"><span data-stu-id="5e9d0-565">Read all the products' licenses and terms and click **I Accept** to continue.</span></span>

    ![Akceptowanie umowy licencyjnej](aspnet-mvc-4-helpers-forms-and-validation/_static/image23.png)

    <span data-ttu-id="5e9d0-567">*Akceptowanie umowy licencyjnej*</span><span class="sxs-lookup"><span data-stu-id="5e9d0-567">*Accepting the license terms*</span></span>
5. <span data-ttu-id="5e9d0-568">Poczekaj na zakończenie procesu pobierania i instalacji.</span><span class="sxs-lookup"><span data-stu-id="5e9d0-568">Wait until the downloading and installation process completes.</span></span>

    ![Postęp instalacji](aspnet-mvc-4-helpers-forms-and-validation/_static/image24.png)

    <span data-ttu-id="5e9d0-570">*Postęp instalacji*</span><span class="sxs-lookup"><span data-stu-id="5e9d0-570">*Installation progress*</span></span>
6. <span data-ttu-id="5e9d0-571">Po zakończeniu instalacji kliknij przycisk **Zakończ**.</span><span class="sxs-lookup"><span data-stu-id="5e9d0-571">When the installation completes, click **Finish**.</span></span>

    ![Instalacja została zakończona](aspnet-mvc-4-helpers-forms-and-validation/_static/image25.png)

    <span data-ttu-id="5e9d0-573">*Instalacja została zakończona*</span><span class="sxs-lookup"><span data-stu-id="5e9d0-573">*Installation completed*</span></span>
7. <span data-ttu-id="5e9d0-574">Kliknij przycisk **zakończenia** aby zamknąć Instalatora platformy sieci Web.</span><span class="sxs-lookup"><span data-stu-id="5e9d0-574">Click **Exit** to close Web Platform Installer.</span></span>
8. <span data-ttu-id="5e9d0-575">Aby otworzyć program Visual Studio Express for Web, przejdź do **Start** ekranu i zacznij pisać &quot; **VS Express**&quot;, następnie kliknij polecenie **VS Express for Web** Kafelek.</span><span class="sxs-lookup"><span data-stu-id="5e9d0-575">To open Visual Studio Express for Web, go to the **Start** screen and start writing &quot;**VS Express**&quot;, then click on the **VS Express for Web** tile.</span></span>

    ![VS Express for Web kafelka](aspnet-mvc-4-helpers-forms-and-validation/_static/image26.png)

    <span data-ttu-id="5e9d0-577">*VS Express for Web kafelka*</span><span class="sxs-lookup"><span data-stu-id="5e9d0-577">*VS Express for Web tile*</span></span>

<a id="AppendixB"></a>

<a id="Appendix_B_Using_Code_Snippets"></a>
## <a name="appendix-b-using-code-snippets"></a><span data-ttu-id="5e9d0-578">Dodatek B: korzystania z wstawek kodu</span><span class="sxs-lookup"><span data-stu-id="5e9d0-578">Appendix B: Using Code Snippets</span></span>

<span data-ttu-id="5e9d0-579">Wstawki kodu zapewniają całego kodu, które są potrzebne w zasięgu ręki.</span><span class="sxs-lookup"><span data-stu-id="5e9d0-579">With code snippets, you have all the code you need at your fingertips.</span></span> <span data-ttu-id="5e9d0-580">Dokument laboratorium informuje o dokładnie po ich użycia, jak pokazano na poniższej ilustracji.</span><span class="sxs-lookup"><span data-stu-id="5e9d0-580">The lab document will tell you exactly when you can use them, as shown in the following figure.</span></span>

<span data-ttu-id="5e9d0-581">![Aby wstawić kod do projektu przy użyciu wstawki kodu programu Visual Studio](aspnet-mvc-4-helpers-forms-and-validation/_static/image27.png "wstawki kodu za pomocą programu Visual Studio, aby wstawić kod do projektu")</span><span class="sxs-lookup"><span data-stu-id="5e9d0-581">![Using Visual Studio code snippets to insert code into your project](aspnet-mvc-4-helpers-forms-and-validation/_static/image27.png "Using Visual Studio code snippets to insert code into your project")</span></span>

<span data-ttu-id="5e9d0-582">*Aby wstawić kod do projektu przy użyciu wstawki kodu programu Visual Studio*</span><span class="sxs-lookup"><span data-stu-id="5e9d0-582">*Using Visual Studio code snippets to insert code into your project*</span></span>

<span data-ttu-id="5e9d0-583">***Aby dodać fragment kodu za pomocą klawiatury (C# tylko)***</span><span class="sxs-lookup"><span data-stu-id="5e9d0-583">***To add a code snippet using the keyboard (C# only)***</span></span>

1. <span data-ttu-id="5e9d0-584">Umieść kursor, w którym chcesz wstawić kod.</span><span class="sxs-lookup"><span data-stu-id="5e9d0-584">Place the cursor where you would like to insert the code.</span></span>
2. <span data-ttu-id="5e9d0-585">Zacznij wpisywać nazwę fragment (bez spacji i łączniki).</span><span class="sxs-lookup"><span data-stu-id="5e9d0-585">Start typing the snippet name (without spaces or hyphens).</span></span>
3. <span data-ttu-id="5e9d0-586">Obejrzyj jako IntelliSense wyświetla zgodne z nazwami wstawki.</span><span class="sxs-lookup"><span data-stu-id="5e9d0-586">Watch as IntelliSense displays matching snippets' names.</span></span>
4. <span data-ttu-id="5e9d0-587">Wybierz prawidłowe fragment (lub zachować wpisywanie do momentu wybrania fragment całą nazwę).</span><span class="sxs-lookup"><span data-stu-id="5e9d0-587">Select the correct snippet (or keep typing until the entire snippet's name is selected).</span></span>
5. <span data-ttu-id="5e9d0-588">Naciśnij klawisz Tab dwa razy, aby wstawić fragment kodu w lokalizacji kursora.</span><span class="sxs-lookup"><span data-stu-id="5e9d0-588">Press the Tab key twice to insert the snippet at the cursor location.</span></span>

<span data-ttu-id="5e9d0-589">![Rozpocznij wpisywanie nazwy fragment](aspnet-mvc-4-helpers-forms-and-validation/_static/image28.png "zacznij wpisywać nazwę wstawki programu")</span><span class="sxs-lookup"><span data-stu-id="5e9d0-589">![Start typing the snippet name](aspnet-mvc-4-helpers-forms-and-validation/_static/image28.png "Start typing the snippet name")</span></span>

<span data-ttu-id="5e9d0-590">*Rozpocznij wpisywanie nazwy fragment kodu*</span><span class="sxs-lookup"><span data-stu-id="5e9d0-590">*Start typing the snippet name*</span></span>

<span data-ttu-id="5e9d0-591">![Naciśnij klawisz Tab, aby wybrać wyróżnione fragment](aspnet-mvc-4-helpers-forms-and-validation/_static/image29.png "naciśnij klawisz Tab, aby wybrać wyróżnione fragment kodu")</span><span class="sxs-lookup"><span data-stu-id="5e9d0-591">![Press Tab to select the highlighted snippet](aspnet-mvc-4-helpers-forms-and-validation/_static/image29.png "Press Tab to select the highlighted snippet")</span></span>

<span data-ttu-id="5e9d0-592">*Naciśnij klawisz Tab, aby wybrać wyróżnione fragment kodu*</span><span class="sxs-lookup"><span data-stu-id="5e9d0-592">*Press Tab to select the highlighted snippet*</span></span>

<span data-ttu-id="5e9d0-593">![Ponownie naciśnij klawisz Tab i fragment rozwinie](aspnet-mvc-4-helpers-forms-and-validation/_static/image30.png "rozwinie ponownie naciśnij klawisz Tab i wstawki kodu")</span><span class="sxs-lookup"><span data-stu-id="5e9d0-593">![Press Tab again and the snippet will expand](aspnet-mvc-4-helpers-forms-and-validation/_static/image30.png "Press Tab again and the snippet will expand")</span></span>

<span data-ttu-id="5e9d0-594">*Rozwinie ponownie naciśnij klawisz Tab i wstawki kodu*</span><span class="sxs-lookup"><span data-stu-id="5e9d0-594">*Press Tab again and the snippet will expand*</span></span>

<span data-ttu-id="5e9d0-595">***Aby dodać fragment kodu za pomocą myszy (C#, Visual Basic i XML)*** 1.</span><span class="sxs-lookup"><span data-stu-id="5e9d0-595">***To add a code snippet using the mouse (C#, Visual Basic and XML)*** 1.</span></span> <span data-ttu-id="5e9d0-596">Kliknij prawym przyciskiem myszy, gdy chcesz wstawić fragment kodu.</span><span class="sxs-lookup"><span data-stu-id="5e9d0-596">Right-click where you want to insert the code snippet.</span></span>

1. <span data-ttu-id="5e9d0-597">Wybierz **wstawić fragment** następuje **Moje wstawki kodu**.</span><span class="sxs-lookup"><span data-stu-id="5e9d0-597">Select **Insert Snippet** followed by **My Code Snippets**.</span></span>
2. <span data-ttu-id="5e9d0-598">Wybierz odpowiedni fragment z listy, klikając go.</span><span class="sxs-lookup"><span data-stu-id="5e9d0-598">Pick the relevant snippet from the list, by clicking on it.</span></span>

<span data-ttu-id="5e9d0-599">![Kliknij prawym przyciskiem myszy, gdy chcesz wstawić fragment kodu i wybierz wstawić fragment](aspnet-mvc-4-helpers-forms-and-validation/_static/image31.png "kliknij prawym przyciskiem myszy, gdy chcesz wstawić fragment kodu i wybierz wstawić fragment kodu")</span><span class="sxs-lookup"><span data-stu-id="5e9d0-599">![Right-click where you want to insert the code snippet and select Insert Snippet](aspnet-mvc-4-helpers-forms-and-validation/_static/image31.png "Right-click where you want to insert the code snippet and select Insert Snippet")</span></span>

<span data-ttu-id="5e9d0-600">*Kliknij prawym przyciskiem myszy, gdy chcesz wstawić fragment kodu i wybierz wstawić fragment kodu*</span><span class="sxs-lookup"><span data-stu-id="5e9d0-600">*Right-click where you want to insert the code snippet and select Insert Snippet*</span></span>

<span data-ttu-id="5e9d0-601">![Wybierz odpowiedni fragment z listy, klikając go](aspnet-mvc-4-helpers-forms-and-validation/_static/image32.png "wybierz odpowiedni fragment z listy, klikając go")</span><span class="sxs-lookup"><span data-stu-id="5e9d0-601">![Pick the relevant snippet from the list, by clicking on it](aspnet-mvc-4-helpers-forms-and-validation/_static/image32.png "Pick the relevant snippet from the list, by clicking on it")</span></span>

<span data-ttu-id="5e9d0-602">*Wybierz odpowiedni fragment z listy, klikając go*</span><span class="sxs-lookup"><span data-stu-id="5e9d0-602">*Pick the relevant snippet from the list, by clicking on it*</span></span>
