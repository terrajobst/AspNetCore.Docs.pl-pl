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
ms.openlocfilehash: 243db3708ac4311d423c4c137f503f072f5553e6
ms.sourcegitcommit: 7ac15eaae20b6d70e65f3650af050a7880115cbf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/02/2018
---
# <a name="aspnet-mvc-4-helpers-forms-and-validation"></a><span data-ttu-id="92a11-104">Pomocników platformy ASP.NET MVC 4, formularzy i sprawdzania poprawności</span><span class="sxs-lookup"><span data-stu-id="92a11-104">ASP.NET MVC 4 Helpers, Forms and Validation</span></span>

<span data-ttu-id="92a11-105">Przez [obozów sieci Web Team](https://twitter.com/webcamps)</span><span class="sxs-lookup"><span data-stu-id="92a11-105">By [Web Camps Team](https://twitter.com/webcamps)</span></span>

[<span data-ttu-id="92a11-106">Pobierz obozów sieci Web uczenie Kit</span><span class="sxs-lookup"><span data-stu-id="92a11-106">Download Web Camps Training Kit</span></span>](https://aka.ms/webcamps-training-kit)

<span data-ttu-id="92a11-107">W **ASP.NET MVC 4 modeli i dostęp do danych** laboratorium Hands-on nastąpiło ładowanie i wyświetlanie danych z bazy danych.</span><span class="sxs-lookup"><span data-stu-id="92a11-107">In **ASP.NET MVC 4 Models and Data Access** Hands-on Lab, you have been loading and displaying data from the database.</span></span> <span data-ttu-id="92a11-108">W tym laboratorium Hands-on doda do **sklep muzyczny** aplikacji możliwość edytowania tych danych.</span><span class="sxs-lookup"><span data-stu-id="92a11-108">In this Hands-on Lab, you will add to the **Music Store** application the ability to edit that data.</span></span>

<span data-ttu-id="92a11-109">Tego celu pamiętać najpierw utworzysz kontroler, który będzie obsługiwać albumy akcje tworzenia, odczytu, aktualizacji i usuwania (CRUD).</span><span class="sxs-lookup"><span data-stu-id="92a11-109">With that goal in mind, you will first create the controller that will support the Create, Read, Update and Delete (CRUD) actions of albums.</span></span> <span data-ttu-id="92a11-110">Wygeneruje szablonu widoku indeksu, korzystając z funkcji szkieletów ASP.NET MVC do wyświetlania właściwości albumów w tabeli HTML.</span><span class="sxs-lookup"><span data-stu-id="92a11-110">You will generate an Index View template taking advantage of ASP.NET MVC's scaffolding feature to display the albums' properties in an HTML table.</span></span> <span data-ttu-id="92a11-111">Aby zwiększyć ten widok, zostaną dodane niestandardowe pomocnika kodu HTML, który obetnie długie opisy.</span><span class="sxs-lookup"><span data-stu-id="92a11-111">To enhance that view, you will add a custom HTML helper that will truncate long descriptions.</span></span>

<span data-ttu-id="92a11-112">Następnie możesz spowoduje to dodanie edycji i Utwórz widoki, które pozwoli instrukcja alter albumów w bazie danych za pomocą formularza elementy, takie jak listę rozwijaną.</span><span class="sxs-lookup"><span data-stu-id="92a11-112">Afterwards, you will add the Edit and Create Views that will let you alter the albums in the database, with the help of form elements like dropdowns.</span></span>

<span data-ttu-id="92a11-113">Ponadto umożliwi użytkownikowi usunięcie albumu i również uniemożliwi ich wprowadzania danych niewłaściwy przez sprawdzanie poprawności danych wejściowych.</span><span class="sxs-lookup"><span data-stu-id="92a11-113">Lastly, you will let users delete an album and also you will prevent them from entering wrong data by validating their input.</span></span>

<span data-ttu-id="92a11-114">W tym laboratorium Hands-on zakłada mieć podstawową wiedzę na temat **ASP.NET MVC**.</span><span class="sxs-lookup"><span data-stu-id="92a11-114">This Hands-on Lab assumes you have basic knowledge of **ASP.NET MVC**.</span></span> <span data-ttu-id="92a11-115">Jeśli nie używasz **ASP.NET MVC** przed, zalecamy zapoznać się z **podstawowe informacje na temat platformy ASP.NET MVC** Hands-on laboratorium.</span><span class="sxs-lookup"><span data-stu-id="92a11-115">If you have not used **ASP.NET MVC** before, we recommend you to go over **ASP.NET MVC Fundamentals** Hands-on Lab.</span></span>

<span data-ttu-id="92a11-116">W tym laboratorium przeprowadzi Cię przez rozszerzenia oraz nowe funkcje opisane wcześniej przez zastosowanie drobne zmiany do przykładowej aplikacji sieci Web w folderze źródłowym.</span><span class="sxs-lookup"><span data-stu-id="92a11-116">This lab walks you through the enhancements and new features previously described by applying minor changes to a sample Web application provided in the Source folder.</span></span>

> [!NOTE]
> <span data-ttu-id="92a11-117">Wszystkie przykładowy kod i fragmenty kodu są uwzględnione w sieci Web obozów zestaw szkoleniowy, dostępne pod adresem [wersje Microsoft-Web/WebCampTrainingKit](https://aka.ms/webcamps-training-kit).</span><span class="sxs-lookup"><span data-stu-id="92a11-117">All sample code and snippets are included in the Web Camps Training Kit, available at [Microsoft-Web/WebCampTrainingKit Releases](https://aka.ms/webcamps-training-kit).</span></span> <span data-ttu-id="92a11-118">Specyficzne dla tego laboratorium projektu jest dostępna na [pomocników programu ASP.NET MVC 4, formularzy i sprawdzania poprawności](https://github.com/Microsoft-Web/HOL-MVC4HelpersFormsAndValidation).</span><span class="sxs-lookup"><span data-stu-id="92a11-118">The project specific to this lab is available at [ASP.NET MVC 4 Helpers, Forms and Validation](https://github.com/Microsoft-Web/HOL-MVC4HelpersFormsAndValidation).</span></span>

<a id="Objectives"></a>
### <a name="objectives"></a><span data-ttu-id="92a11-119">Cele</span><span class="sxs-lookup"><span data-stu-id="92a11-119">Objectives</span></span>

<span data-ttu-id="92a11-120">W tym laboratorium Hands-On przedstawiono sposób:</span><span class="sxs-lookup"><span data-stu-id="92a11-120">In this Hands-On Lab, you will learn how to:</span></span>

- <span data-ttu-id="92a11-121">Tworzenie kontrolera w celu obsługi operacji CRUD</span><span class="sxs-lookup"><span data-stu-id="92a11-121">Create a controller to support CRUD operations</span></span>
- <span data-ttu-id="92a11-122">Generowanie widoku indeksu, aby wyświetlić właściwości jednostki tabeli HTML</span><span class="sxs-lookup"><span data-stu-id="92a11-122">Generate an Index View to display entity properties in an HTML table</span></span>
- <span data-ttu-id="92a11-123">Dodaj niestandardowy pomocnika kodu HTML</span><span class="sxs-lookup"><span data-stu-id="92a11-123">Add a custom HTML helper</span></span>
- <span data-ttu-id="92a11-124">Tworzenie i dostosowywanie Edytowanie widoku</span><span class="sxs-lookup"><span data-stu-id="92a11-124">Create and customize an Edit View</span></span>
- <span data-ttu-id="92a11-125">Rozróżnienie metod akcji, które reagują na HTTP GET lub POST protokołu HTTP wywołania</span><span class="sxs-lookup"><span data-stu-id="92a11-125">Differentiate between action methods that react to either HTTP-GET or HTTP-POST calls</span></span>
- <span data-ttu-id="92a11-126">Dodawanie i dostosowywanie Utwórz widok</span><span class="sxs-lookup"><span data-stu-id="92a11-126">Add and customize a Create View</span></span>
- <span data-ttu-id="92a11-127">Dojście usunięcie jednostki</span><span class="sxs-lookup"><span data-stu-id="92a11-127">Handle the deletion of an entity</span></span>
- <span data-ttu-id="92a11-128">Sprawdzanie poprawności danych wejściowych użytkownika</span><span class="sxs-lookup"><span data-stu-id="92a11-128">Validate user input</span></span>

<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a><span data-ttu-id="92a11-129">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="92a11-129">Prerequisites</span></span>

<span data-ttu-id="92a11-130">Musi mieć następujące elementy do przygotowania tego laboratorium:</span><span class="sxs-lookup"><span data-stu-id="92a11-130">You must have the following items to complete this lab:</span></span>

- <span data-ttu-id="92a11-131">[Microsoft Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) lub wyższego poziomu (odczytu [dodatek a.](#AppendixA) instrukcje dotyczące sposobu jego instalacji).</span><span class="sxs-lookup"><span data-stu-id="92a11-131">[Microsoft Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) or superior (read [Appendix A](#AppendixA) for instructions on how to install it).</span></span>

<a id="Setup"></a>

<a id="Setup"></a>
### <a name="setup"></a><span data-ttu-id="92a11-132">Konfiguracja</span><span class="sxs-lookup"><span data-stu-id="92a11-132">Setup</span></span>

<span data-ttu-id="92a11-133">**Instalowanie wstawki kodu**</span><span class="sxs-lookup"><span data-stu-id="92a11-133">**Installing Code Snippets**</span></span>

<span data-ttu-id="92a11-134">Dla wygody taki kod, który będzie zarządzany wzdłuż tego laboratorium jest dostępna jako wstawki kodu programu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="92a11-134">For convenience, much of the code you will be managing along this lab is available as Visual Studio code snippets.</span></span> <span data-ttu-id="92a11-135">Aby zainstalować fragmenty kodu, uruchom **.\Source\Setup\CodeSnippets.vsi** pliku.</span><span class="sxs-lookup"><span data-stu-id="92a11-135">To install the code snippets run **.\Source\Setup\CodeSnippets.vsi** file.</span></span>

<span data-ttu-id="92a11-136">Jeśli nie masz doświadczenia z wstawki programu Visual Studio i chcesz dowiedzieć się, jak ich używać, można odwołać się do dodatku z tego dokumentu &quot; [wstawki kodu za pomocą programu dodatek B:](#AppendixB)&quot;.</span><span class="sxs-lookup"><span data-stu-id="92a11-136">If you are not familiar with the Visual Studio Code Snippets, and want to learn how to use them, you can refer to the appendix from this document &quot;[Appendix B: Using Code Snippets](#AppendixB)&quot;.</span></span>

* * *

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a><span data-ttu-id="92a11-137">Ćwiczenia</span><span class="sxs-lookup"><span data-stu-id="92a11-137">Exercises</span></span>

<span data-ttu-id="92a11-138">Następujące ćwiczeń tworzą tego laboratorium Hands-On:</span><span class="sxs-lookup"><span data-stu-id="92a11-138">The following exercises make up this Hands-On Lab:</span></span>

1. [<span data-ttu-id="92a11-139">Tworzenie kontrolera Menedżer magazynu i jego widoku indeksu</span><span class="sxs-lookup"><span data-stu-id="92a11-139">Creating the Store Manager controller and its Index view</span></span>](#Exercise1)
2. [<span data-ttu-id="92a11-140">Dodawanie pomocnika kodu HTML</span><span class="sxs-lookup"><span data-stu-id="92a11-140">Adding an HTML Helper</span></span>](#Exercise2)
3. [<span data-ttu-id="92a11-141">Tworzenie widoku edycji</span><span class="sxs-lookup"><span data-stu-id="92a11-141">Creating the Edit View</span></span>](#Exercise3)
4. [<span data-ttu-id="92a11-142">Dodawanie widoku Create</span><span class="sxs-lookup"><span data-stu-id="92a11-142">Adding a Create View</span></span>](#Exercise4)
5. [<span data-ttu-id="92a11-143">Obsługa usuwania</span><span class="sxs-lookup"><span data-stu-id="92a11-143">Handling Deletion</span></span>](#Exercise5)
6. [<span data-ttu-id="92a11-144">Dodawanie weryfikacji</span><span class="sxs-lookup"><span data-stu-id="92a11-144">Adding Validation</span></span>](#Exercise6)
7. [<span data-ttu-id="92a11-145">Przy użyciu jQuery dyskretny kod po stronie klienta</span><span class="sxs-lookup"><span data-stu-id="92a11-145">Using Unobtrusive jQuery at Client Side</span></span>](#Exercise7)

> [!NOTE]
> <span data-ttu-id="92a11-146">Towarzyszy każdego wykonywania **zakończenia** folderu zawierającego wynikowy rozwiązanie, należy uzyskać po wykonaniu ćwiczeniach.</span><span class="sxs-lookup"><span data-stu-id="92a11-146">Each exercise is accompanied by an **End** folder containing the resulting solution you should obtain after completing the exercises.</span></span> <span data-ttu-id="92a11-147">Jeśli potrzebujesz dodatkowej pomocy pracuje nad ćwiczeniami, można użyć tego rozwiązania jako przewodnika.</span><span class="sxs-lookup"><span data-stu-id="92a11-147">You can use this solution as a guide if you need additional help working through the exercises.</span></span>


<span data-ttu-id="92a11-148">Szacowany czas trwania tego laboratorium: **60 minut**</span><span class="sxs-lookup"><span data-stu-id="92a11-148">Estimated time to complete this lab: **60 minutes**</span></span>

<a id="Exercise1"></a>

<a id="Exercise_1_Creating_the_Store_Manager_controller_and_its_Index_view"></a>
### <a name="exercise-1-creating-the-store-manager-controller-and-its-index-view"></a><span data-ttu-id="92a11-149">Ćwiczenie 1: Tworzenie kontrolera Menedżer magazynu i jego widoku indeksu</span><span class="sxs-lookup"><span data-stu-id="92a11-149">Exercise 1: Creating the Store Manager controller and its Index view</span></span>

<span data-ttu-id="92a11-150">W tym ćwiczeniu dowiesz sposób tworzenia nowego kontrolera w celu obsługi operacji CRUD, można dostosować jego indeksu metody akcji, aby zwrócić listę albumów z bazy danych i na koniec generowania szablon widoku indeksu wykorzystaniu szkieletów ASP.NET MVC Funkcja, aby wyświetlić właściwości albumów w tabeli HTML.</span><span class="sxs-lookup"><span data-stu-id="92a11-150">In this exercise, you will learn how to create a new controller to support CRUD operations, customize its Index action method to return a list of albums from the database and finally generating an Index View template taking advantage of ASP.NET MVC's scaffolding feature to display the albums' properties in an HTML table.</span></span>

<a id="Ex1Task1"></a>

<a id="Task_1_-_Creating_the_StoreManagerController"></a>
#### <a name="task-1---creating-the-storemanagercontroller"></a><span data-ttu-id="92a11-151">Zadanie 1 — Tworzenie StoreManagerController</span><span class="sxs-lookup"><span data-stu-id="92a11-151">Task 1 - Creating the StoreManagerController</span></span>

<span data-ttu-id="92a11-152">W ramach tego zadania spowoduje utworzenie nowego kontrolera o nazwie **StoreManagerController** do obsługi operacji CRUD.</span><span class="sxs-lookup"><span data-stu-id="92a11-152">In this task, you will create a new controller called **StoreManagerController** to support CRUD operations.</span></span>

1. <span data-ttu-id="92a11-153">Otwórz **rozpocząć** rozwiązania, znajdujących się na **źródło/Ex1-CreatingTheStoreManagerController/Begin/** folderu.</span><span class="sxs-lookup"><span data-stu-id="92a11-153">Open the **Begin** solution located at **Source/Ex1-CreatingTheStoreManagerController/Begin/** folder.</span></span>

    1. <span data-ttu-id="92a11-154">Należy pobrać niektórych brakujących pakietów NuGet aby kontynuować.</span><span class="sxs-lookup"><span data-stu-id="92a11-154">You will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="92a11-155">Aby to zrobić, kliknij przycisk **projektu** menu i wybierz **Zarządzaj pakietami NuGet**.</span><span class="sxs-lookup"><span data-stu-id="92a11-155">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
    2. <span data-ttu-id="92a11-156">W **Zarządzaj pakietami NuGet** okna dialogowego, kliknij przycisk **przywrócić** celu pobieranie brakujących pakietów.</span><span class="sxs-lookup"><span data-stu-id="92a11-156">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
    3. <span data-ttu-id="92a11-157">Na koniec Skompiluj rozwiązanie, klikając **kompilacji** | **Kompiluj rozwiązanie**.</span><span class="sxs-lookup"><span data-stu-id="92a11-157">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="92a11-158">Jedną z zalet przy użyciu narzędzia NuGet jest, że nie masz do wysłania wszystkich bibliotek w projekcie, zmniejszenie jego rozmiar projektu.</span><span class="sxs-lookup"><span data-stu-id="92a11-158">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="92a11-159">Narzędzia Power NuGet określając wersje pakietów w pliku Packages.config, będzie można pobrać wymaganych bibliotek podczas pierwszego uruchomienia projektu.</span><span class="sxs-lookup"><span data-stu-id="92a11-159">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="92a11-160">Jest to, dlaczego konieczne będzie wykonanie tych kroków, po otwarciu istniejącego rozwiązania z tego laboratorium.</span><span class="sxs-lookup"><span data-stu-id="92a11-160">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="92a11-161">Dodaj nowy kontroler.</span><span class="sxs-lookup"><span data-stu-id="92a11-161">Add a new controller.</span></span> <span data-ttu-id="92a11-162">Aby to zrobić, kliknij prawym przyciskiem myszy **kontrolerów** folder w Eksploratorze rozwiązań wybierz **Dodaj** , a następnie **kontrolera** polecenia.</span><span class="sxs-lookup"><span data-stu-id="92a11-162">To do this, right-click the **Controllers** folder within the Solution Explorer, select **Add** and then the **Controller** command.</span></span> <span data-ttu-id="92a11-163">Zmień **kontrolera** **nazwa** do **StoreManagerController** i upewnij się, że opcja **kontroler MVC z akcjami odczytu/zapisu pusty**jest zaznaczone.</span><span class="sxs-lookup"><span data-stu-id="92a11-163">Change the **Controller** **Name** to **StoreManagerController** and make sure the option **MVC controller with empty read/write actions** is selected.</span></span> <span data-ttu-id="92a11-164">Kliknij przycisk **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="92a11-164">Click **Add**.</span></span>

    <span data-ttu-id="92a11-165">![Dodaj kontroler w oknie dialogowym](aspnet-mvc-4-helpers-forms-and-validation/_static/image1.png "Dodaj kontroler w oknie dialogowym")</span><span class="sxs-lookup"><span data-stu-id="92a11-165">![Add controller dialog](aspnet-mvc-4-helpers-forms-and-validation/_static/image1.png "Add controller dialog")</span></span>

    <span data-ttu-id="92a11-166">*Dodawanie kontrolera okna dialogowego*</span><span class="sxs-lookup"><span data-stu-id="92a11-166">*Add Controller Dialog*</span></span>

    <span data-ttu-id="92a11-167">Nowa klasa kontrolera jest generowany.</span><span class="sxs-lookup"><span data-stu-id="92a11-167">A new Controller class is generated.</span></span> <span data-ttu-id="92a11-168">Ponieważ wskazane Dodaj akcje dla odczytu/zapisu, metod klasy zastępczej dla osób, typowych operacji CRUD są tworzone z komentarze TODO wypełnione, monitowanie Dołącz logikę określonych aplikacji.</span><span class="sxs-lookup"><span data-stu-id="92a11-168">Since you indicated to add actions for read/write, stub methods for those, common CRUD actions are created with TODO comments filled in, prompting to include the application specific logic.</span></span>

<a id="Ex1Task2"></a>

<a id="Task_2_-_Customizing_the_StoreManager_Index"></a>
#### <a name="task-2---customizing-the-storemanager-index"></a><span data-ttu-id="92a11-169">Zadanie 2 — dostosowanie indeksu StoreManager</span><span class="sxs-lookup"><span data-stu-id="92a11-169">Task 2 - Customizing the StoreManager Index</span></span>

<span data-ttu-id="92a11-170">W tym zadaniu zostanie dostosować indeksu StoreManager metody akcji do zwrócenia widoku z listy albumy z bazy danych.</span><span class="sxs-lookup"><span data-stu-id="92a11-170">In this task, you will customize the StoreManager Index action method to return a View with the list of albums from the database.</span></span>

1. <span data-ttu-id="92a11-171">W klasie StoreManagerController, Dodaj następujący *przy użyciu* dyrektywy.</span><span class="sxs-lookup"><span data-stu-id="92a11-171">In the StoreManagerController class, add the following *using* directives.</span></span>

    <span data-ttu-id="92a11-172">(Fragment - kodu *ASP.NET MVC 4 pomocników i formularzy i sprawdzanie poprawności — Ex1 przy użyciu MvcMusicStore*)</span><span class="sxs-lookup"><span data-stu-id="92a11-172">(Code Snippet - *ASP.NET MVC 4 Helpers and Forms and Validation - Ex1 using MvcMusicStore*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample1.cs)]
2. <span data-ttu-id="92a11-173">Dodawanie pola do **StoreManagerController** do przechowywania wystąpienia **MusicStoreEntities.**</span><span class="sxs-lookup"><span data-stu-id="92a11-173">Add a field to the **StoreManagerController** to hold an instance of **MusicStoreEntities.**</span></span>

    <span data-ttu-id="92a11-174">(Fragment - kodu *pomocników platformy ASP.NET MVC 4 oraz formularzy i sprawdzanie poprawności — Ex1 MusicStoreEntities*)</span><span class="sxs-lookup"><span data-stu-id="92a11-174">(Code Snippet - *ASP.NET MVC 4 Helpers and Forms and Validation - Ex1 MusicStoreEntities*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample2.cs)]
3. <span data-ttu-id="92a11-175">Implementuje akcji indeksu StoreManagerController do zwrócenia widoku z listy Albumy.</span><span class="sxs-lookup"><span data-stu-id="92a11-175">Implement the StoreManagerController Index action to return a View with the list of albums.</span></span>

    <span data-ttu-id="92a11-176">Logika akcji kontrolera jest bardzo podobny do akcji indeksu StoreController zapisane wcześniej.</span><span class="sxs-lookup"><span data-stu-id="92a11-176">The Controller action logic will be very similar to the StoreController's Index action written earlier.</span></span> <span data-ttu-id="92a11-177">Użyj rozszerzenia LINQ, można pobrać wszystkich albumów, w tym informacje o rodzaju i wykonawcy do wyświetlenia.</span><span class="sxs-lookup"><span data-stu-id="92a11-177">Use LINQ to retrieve all albums, including Genre and Artist information for display.</span></span>

    <span data-ttu-id="92a11-178">(Fragment - kodu *pomocników platformy ASP.NET MVC 4 oraz formularzy i sprawdzanie poprawności — indeks StoreManagerController Ex1*)</span><span class="sxs-lookup"><span data-stu-id="92a11-178">(Code Snippet - *ASP.NET MVC 4 Helpers and Forms and Validation - Ex1 StoreManagerController Index*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample3.cs)]

<a id="Ex1Task3"></a>

<a id="strongTask_3_-_Creating_the_Index_Viewstrong"></a>
#### <a name="task-3---creating-the-index-view"></a><span data-ttu-id="92a11-179">Zadanie 3 — Tworzenie widoku indeksu</span><span class="sxs-lookup"><span data-stu-id="92a11-179">Task 3 - Creating the Index View</span></span>

<span data-ttu-id="92a11-180">W ramach tego zadania spowoduje utworzenie szablonu widoku indeksu do wyświetlenia na liście Albumy zwrócony przez **StoreManager** kontrolera.</span><span class="sxs-lookup"><span data-stu-id="92a11-180">In this task, you will create the Index View template to display the list of albums returned by the **StoreManager** Controller.</span></span>

1. <span data-ttu-id="92a11-181">Przed utworzeniem nowego szablonu widoku, należy skompilować projekt, aby **dialogowe dodawania widoku** zna **albumu** klasy do użycia.</span><span class="sxs-lookup"><span data-stu-id="92a11-181">Before creating the new View template, you should build the project so that the **Add View Dialog** knows about the **Album** class to use.</span></span> <span data-ttu-id="92a11-182">Wybierz **kompilacji | Tworzenie MvcMusicStore** Aby skompilować projekt.</span><span class="sxs-lookup"><span data-stu-id="92a11-182">Select **Build | Build MvcMusicStore** to build the project.</span></span>
2. <span data-ttu-id="92a11-183">Kliknij prawym przyciskiem myszy wewnątrz **indeksu** metody akcji i wybierz **Dodaj widok**.</span><span class="sxs-lookup"><span data-stu-id="92a11-183">Right-click inside the **Index** action method and select **Add View**.</span></span> <span data-ttu-id="92a11-184">Zostanie wyświetlone okno **Dodaj widok** okna dialogowego.</span><span class="sxs-lookup"><span data-stu-id="92a11-184">This will bring up the **Add View** dialog.</span></span>

    <span data-ttu-id="92a11-185">![Dodaj widok](aspnet-mvc-4-helpers-forms-and-validation/_static/image2.png "Dodawanie widoku")</span><span class="sxs-lookup"><span data-stu-id="92a11-185">![Add view](aspnet-mvc-4-helpers-forms-and-validation/_static/image2.png "Add view")</span></span>

    <span data-ttu-id="92a11-186">*Dodawanie widoku z wewnątrz Index — metoda*</span><span class="sxs-lookup"><span data-stu-id="92a11-186">*Adding a View from within the Index method*</span></span>
3. <span data-ttu-id="92a11-187">W oknie dialogowym Dodaj widok, sprawdź, czy nazwa widoku jest **indeksu**.</span><span class="sxs-lookup"><span data-stu-id="92a11-187">In the Add View dialog, verify that the View Name is **Index**.</span></span> <span data-ttu-id="92a11-188">Wybierz **utworzyć widok jednoznacznie** i wybrać **albumu (MvcMusicStore.Models)** z **klasa modelu** listy rozwijanej.</span><span class="sxs-lookup"><span data-stu-id="92a11-188">Select the **Create a strongly-typed view** option, and select **Album (MvcMusicStore.Models)** from the **Model class** drop-down.</span></span> <span data-ttu-id="92a11-189">Wybierz **listy** z **szablonu szkieletu** listy rozwijanej.</span><span class="sxs-lookup"><span data-stu-id="92a11-189">Select **List** from the **Scaffold template** drop-down.</span></span> <span data-ttu-id="92a11-190">Pozostaw **aparat widoku** do **Razor** i innych pól z ich domyślną wartość, a następnie kliknij przycisk **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="92a11-190">Leave the **View engine** to **Razor** and the other fields with their default value and then click **Add**.</span></span>

    <span data-ttu-id="92a11-191">![Dodawanie widok indeksu](aspnet-mvc-4-helpers-forms-and-validation/_static/image3.png "Dodawanie widok indeksu")</span><span class="sxs-lookup"><span data-stu-id="92a11-191">![Adding an index view](aspnet-mvc-4-helpers-forms-and-validation/_static/image3.png "Adding an index view")</span></span>

    <span data-ttu-id="92a11-192">*Dodawanie widok indeksu*</span><span class="sxs-lookup"><span data-stu-id="92a11-192">*Adding an Index View*</span></span>

<a id="Ex1Task4"></a>

<a id="Task_4_-_Customizing_the_scaffold_of_the_Index_View"></a>
#### <a name="task-4---customizing-the-scaffold-of-the-index-view"></a><span data-ttu-id="92a11-193">Zadanie 4 — Dostosowywanie szkieletu widoku indeksu</span><span class="sxs-lookup"><span data-stu-id="92a11-193">Task 4 - Customizing the scaffold of the Index View</span></span>

<span data-ttu-id="92a11-194">W tym zadaniu zostanie dostosowana prostego szablonu Widok utworzony za pomocą funkcji szkieletów ASP.NET MVC ona żądane pola.</span><span class="sxs-lookup"><span data-stu-id="92a11-194">In this task, you will adjust the simple View template created with ASP.NET MVC scaffolding feature to have it display the fields you want.</span></span>

> [!NOTE]
> <span data-ttu-id="92a11-195">**Szkieletów** wsparcia w ramach platformy ASP.NET MVC generuje prosty szablon widoku, który wyświetla wszystkie pola w modelu albumu.</span><span class="sxs-lookup"><span data-stu-id="92a11-195">The **scaffolding** support within ASP.NET MVC generates a simple View template which lists all fields in the Album model.</span></span> <span data-ttu-id="92a11-196">**Tworzenia szkieletu** umożliwia szybkie rozpoczęcie pracy na silnie typizowanego widoku: zamiast ręcznie zapisać szablon widoku, tworzenia szkieletu szybko generuje szablon domyślny, a następnie można zmodyfikować wygenerowanego kodu.</span><span class="sxs-lookup"><span data-stu-id="92a11-196">**Scaffolding** provides a quick way to get started on a strongly typed view: rather than having to write the View template manually, scaffolding quickly generates a default template and then you can modify the generated code.</span></span>


1. <span data-ttu-id="92a11-197">Przejrzyj kod utworzony.</span><span class="sxs-lookup"><span data-stu-id="92a11-197">Review the code created.</span></span> <span data-ttu-id="92a11-198">Generowanie listy pól będzie częścią następujące tabeli HTML, który **szkieletów** używanej do wyświetlania danych tabelarycznych.</span><span class="sxs-lookup"><span data-stu-id="92a11-198">The generated list of fields will be part of the following HTML table that **Scaffolding** is using for displaying tabular data.</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample4.cshtml)]
2. <span data-ttu-id="92a11-199">Zastąp  **&lt;tabeli&gt;**  kodu z następującym kodem, które mają być wyświetlane tylko **Genre**, **wykonawcy**, **tytuł**, i **cen** pola.</span><span class="sxs-lookup"><span data-stu-id="92a11-199">Replace the **&lt;table&gt;** code with the following code to display only the **Genre**, **Artist**, **Album Title**, and **Price** fields.</span></span> <span data-ttu-id="92a11-200">Spowoduje to usunięcie **AlbumId** i **albumów graficznych URL** kolumn.</span><span class="sxs-lookup"><span data-stu-id="92a11-200">This deletes the **AlbumId** and **Album Art URL** columns.</span></span> <span data-ttu-id="92a11-201">Ponadto zmienia GenreId i ArtistId kolumny do wyświetlenia ich właściwości klasy połączonego **Artist.Name** i **Genre.Name**i usuwa **szczegóły** łącza.</span><span class="sxs-lookup"><span data-stu-id="92a11-201">Also, it changes GenreId and ArtistId columns to display their linked class properties of **Artist.Name** and **Genre.Name**, and removes the **Details** link.</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample5.cshtml)]
3. <span data-ttu-id="92a11-202">Zmień poniższe opisy.</span><span class="sxs-lookup"><span data-stu-id="92a11-202">Change the following descriptions.</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample6.cshtml)]

<a id="Ex1Task5"></a>

<a id="Task_5_-_Running_the_Application"></a>
#### <a name="task-5---running-the-application"></a><span data-ttu-id="92a11-203">Zadanie 5 działania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="92a11-203">Task 5 - Running the Application</span></span>

<span data-ttu-id="92a11-204">W tym zadaniu zostanie sprawdzić, czy **StoreManager** **indeksu** szablon widoku zostanie wyświetlona lista albumów zgodnie z projektem z poprzednich kroków.</span><span class="sxs-lookup"><span data-stu-id="92a11-204">In this task, you will test that the **StoreManager** **Index** View template displays a list of albums according to the design of the previous steps.</span></span>

1. <span data-ttu-id="92a11-205">Naciśnij klawisz **F5** do uruchomienia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="92a11-205">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="92a11-206">Na stronie głównej datę rozpoczęcia projektu.</span><span class="sxs-lookup"><span data-stu-id="92a11-206">The project starts in the Home page.</span></span> <span data-ttu-id="92a11-207">Zmień adres URL do **/StoreManager** Aby sprawdzić, czy zostanie wyświetlona lista albumy, przedstawiający ich **tytuł**, **wykonawcy** i **Genre**.</span><span class="sxs-lookup"><span data-stu-id="92a11-207">Change the URL to **/StoreManager** to verify that a list of albums is displayed, showing their **Title**, **Artist** and **Genre**.</span></span>

    <span data-ttu-id="92a11-208">![Przeglądanie listy albumów](aspnet-mvc-4-helpers-forms-and-validation/_static/image4.png "przeglądania listy albumów")</span><span class="sxs-lookup"><span data-stu-id="92a11-208">![Browsing the list of albums](aspnet-mvc-4-helpers-forms-and-validation/_static/image4.png "Browsing the list of albums")</span></span>

    <span data-ttu-id="92a11-209">*Przeglądanie listy albumów*</span><span class="sxs-lookup"><span data-stu-id="92a11-209">*Browsing the list of albums*</span></span>

<a id="Exercise2"></a>

<a id="Exercise_2_Adding_an_HTML_Helper"></a>
### <a name="exercise-2-adding-an-html-helper"></a><span data-ttu-id="92a11-210">Ćwiczenie 2: Dodawanie pomocnika kodu HTML</span><span class="sxs-lookup"><span data-stu-id="92a11-210">Exercise 2: Adding an HTML Helper</span></span>

<span data-ttu-id="92a11-211">Strona indeksu StoreManager ma jeden potencjalny problem: tytułu i nazwy wykonawcy właściwości mogą jednocześnie być wystarczająco długie, by wyrzucanie formatowanie tabeli.</span><span class="sxs-lookup"><span data-stu-id="92a11-211">The StoreManager Index page has one potential issue: Title and Artist Name properties can both be long enough to throw off the table formatting.</span></span> <span data-ttu-id="92a11-212">W tym ćwiczeniu dowiesz się, jak dodać niestandardowe pomocnika kodu HTML do obcięcia tekst.</span><span class="sxs-lookup"><span data-stu-id="92a11-212">In this exercise you will learn how to add a custom HTML helper to truncate that text.</span></span>

<span data-ttu-id="92a11-213">Na poniższej ilustracji widać, jak zmienić format ze względu na długość tekstu użycia rozmiar małych przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="92a11-213">In the following figure, you can see how the format is modified because of the length of the text when you use a small browser size.</span></span>

<span data-ttu-id="92a11-214">![Nie przeglądania listy albumów z obcięte tekst](aspnet-mvc-4-helpers-forms-and-validation/_static/image5.png "nie przeglądania listy albumów z obcięte tekstu")</span><span class="sxs-lookup"><span data-stu-id="92a11-214">![Browsing the list of Albums with not truncated text](aspnet-mvc-4-helpers-forms-and-validation/_static/image5.png "Browsing the list of Albums with not truncated text")</span></span>

<span data-ttu-id="92a11-215">*Nie przeglądania listy albumów z obcięte tekstu*</span><span class="sxs-lookup"><span data-stu-id="92a11-215">*Browsing the list of Albums with not truncated text*</span></span>

<a id="Ex2Task1"></a>

<a id="Task_1_-_Extending_the_HTML_Helper"></a>
#### <a name="task-1---extending-the-html-helper"></a><span data-ttu-id="92a11-216">Zadanie 1 - rozszerzanie pomocnika kodu HTML</span><span class="sxs-lookup"><span data-stu-id="92a11-216">Task 1 - Extending the HTML Helper</span></span>

<span data-ttu-id="92a11-217">To zadanie spowoduje dodanie nowej metody **Truncate** do **HTML** obiektu w widoków MVC ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="92a11-217">In this task, you will add a new method **Truncate** to the **HTML** object exposed within ASP.NET MVC Views.</span></span> <span data-ttu-id="92a11-218">Aby to zrobić, zostaną zaimplementowane **— metoda rozszerzenia** do wbudowanej **System.Web.Mvc.HtmlHelper** klasy przez platformę ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="92a11-218">To do this, you will implement an **extension method** to the built-in **System.Web.Mvc.HtmlHelper** class provided by ASP.NET MVC.</span></span>

> [!NOTE]
> <span data-ttu-id="92a11-219">Aby dowiedzieć się więcej o **metody rozszerzenia**, odwiedź ten artykuł w witrynie msdn.</span><span class="sxs-lookup"><span data-stu-id="92a11-219">To read more about **Extension Methods**, please visit this msdn article.</span></span> <span data-ttu-id="92a11-220">[https://msdn.microsoft.com/library/bb383977.aspx](https://msdn.microsoft.com/library/bb383977.aspx).</span><span class="sxs-lookup"><span data-stu-id="92a11-220">[https://msdn.microsoft.com/library/bb383977.aspx](https://msdn.microsoft.com/library/bb383977.aspx).</span></span>


1. <span data-ttu-id="92a11-221">Otwórz **rozpocząć** rozwiązania, znajdujących się na **źródło/Ex2-AddingAnHTMLHelper/Begin/** folderu.</span><span class="sxs-lookup"><span data-stu-id="92a11-221">Open the **Begin** solution located at **Source/Ex2-AddingAnHTMLHelper/Begin/** folder.</span></span> <span data-ttu-id="92a11-222">W przeciwnym razie możesz nadal korzystać **zakończenia** uzyskane rozwiązanie, wykonując poprzednim ćwiczeniu.</span><span class="sxs-lookup"><span data-stu-id="92a11-222">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

    1. <span data-ttu-id="92a11-223">Po otwarciu dostarczonych **rozpocząć** rozwiązania, musisz pobrać niektórych brakujących pakietów NuGet aby kontynuować.</span><span class="sxs-lookup"><span data-stu-id="92a11-223">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="92a11-224">Aby to zrobić, kliknij przycisk **projektu** menu i wybierz **Zarządzaj pakietami NuGet**.</span><span class="sxs-lookup"><span data-stu-id="92a11-224">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
    2. <span data-ttu-id="92a11-225">W **Zarządzaj pakietami NuGet** okna dialogowego, kliknij przycisk **przywrócić** celu pobieranie brakujących pakietów.</span><span class="sxs-lookup"><span data-stu-id="92a11-225">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
    3. <span data-ttu-id="92a11-226">Na koniec Skompiluj rozwiązanie, klikając **kompilacji** | **Kompiluj rozwiązanie**.</span><span class="sxs-lookup"><span data-stu-id="92a11-226">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="92a11-227">Jedną z zalet przy użyciu narzędzia NuGet jest, że nie masz do wysłania wszystkich bibliotek w projekcie, zmniejszenie jego rozmiar projektu.</span><span class="sxs-lookup"><span data-stu-id="92a11-227">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="92a11-228">Narzędzia Power NuGet określając wersje pakietów w pliku Packages.config, będzie można pobrać wymaganych bibliotek podczas pierwszego uruchomienia projektu.</span><span class="sxs-lookup"><span data-stu-id="92a11-228">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="92a11-229">Jest to, dlaczego konieczne będzie wykonanie tych kroków, po otwarciu istniejącego rozwiązania z tego laboratorium.</span><span class="sxs-lookup"><span data-stu-id="92a11-229">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="92a11-230">Otwórz widok indeksu StoreManager firmy.</span><span class="sxs-lookup"><span data-stu-id="92a11-230">Open StoreManager's Index View.</span></span> <span data-ttu-id="92a11-231">Aby to zrobić, w Eksploratorze rozwiązań rozwiń **widoków** folder, a następnie **StoreManager** , a następnie otwórz **Index.cshtml** pliku.</span><span class="sxs-lookup"><span data-stu-id="92a11-231">To do this, in the Solution Explorer expand the **Views** folder, then the **StoreManager** and open the **Index.cshtml** file.</span></span>
3. <span data-ttu-id="92a11-232">Dodaj następujący kod poniżej  **@model**  dyrektywy do definiowania **Truncate** metody pomocnika.</span><span class="sxs-lookup"><span data-stu-id="92a11-232">Add the following code below the **@model** directive to define the **Truncate** helper method.</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample7.cshtml)]

<a id="Ex2Task2"></a>

<a id="Task_2_-_Truncating_Text_in_the_Page"></a>
#### <a name="task-2---truncating-text-in-the-page"></a><span data-ttu-id="92a11-233">Zadanie 2 — obcinanie tekstu na stronie</span><span class="sxs-lookup"><span data-stu-id="92a11-233">Task 2 - Truncating Text in the Page</span></span>

<span data-ttu-id="92a11-234">W tym zadaniu użyjesz **Truncate** metody obcięcia tekst w szablonie widoku.</span><span class="sxs-lookup"><span data-stu-id="92a11-234">In this task, you will use the **Truncate** method to truncate the text in the View template.</span></span>

1. <span data-ttu-id="92a11-235">Otwórz widok indeksu StoreManager firmy.</span><span class="sxs-lookup"><span data-stu-id="92a11-235">Open StoreManager's Index View.</span></span> <span data-ttu-id="92a11-236">Aby to zrobić, w Eksploratorze rozwiązań rozwiń **widoków** folder, a następnie **StoreManager** , a następnie otwórz **Index.cshtml** pliku.</span><span class="sxs-lookup"><span data-stu-id="92a11-236">To do this, in the Solution Explorer expand the **Views** folder, then the **StoreManager** and open the **Index.cshtml** file.</span></span>
2. <span data-ttu-id="92a11-237">Zamień wiersze, które zawierają **nazwy wykonawcy** i albumu **tytuł**.</span><span class="sxs-lookup"><span data-stu-id="92a11-237">Replace the lines that show the **Artist Name** and Album's **Title**.</span></span> <span data-ttu-id="92a11-238">Aby to zrobić, Zamień następujące wiersze.</span><span class="sxs-lookup"><span data-stu-id="92a11-238">To do this, replace the following lines.</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample8.cshtml)]

<a id="Ex2Task3"></a>

<a id="Task_3_-_Running_the_Application"></a>
#### <a name="task-3---running-the-application"></a><span data-ttu-id="92a11-239">Zadanie 3 — uruchamianie aplikacji</span><span class="sxs-lookup"><span data-stu-id="92a11-239">Task 3 - Running the Application</span></span>

<span data-ttu-id="92a11-240">W tym zadaniu zostanie sprawdzić, czy **StoreManager** **indeksu** Wyświetl szablon obcina tytułu i nazwy wykonawcy albumu.</span><span class="sxs-lookup"><span data-stu-id="92a11-240">In this task, you will test that the **StoreManager** **Index** View template truncates the Album's Title and Artist Name.</span></span>

1. <span data-ttu-id="92a11-241">Naciśnij klawisz **F5** do uruchomienia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="92a11-241">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="92a11-242">Na stronie głównej datę rozpoczęcia projektu.</span><span class="sxs-lookup"><span data-stu-id="92a11-242">The project starts in the Home page.</span></span> <span data-ttu-id="92a11-243">Zmień adres URL do **/StoreManager** zweryfikowanie, że long teksty w **tytuł** i **wykonawcy** kolumny są obcinane.</span><span class="sxs-lookup"><span data-stu-id="92a11-243">Change the URL to **/StoreManager** to verify that long texts in the **Title** and **Artist** column are truncated.</span></span>

    <span data-ttu-id="92a11-244">![Obcięty nazwy tytułów i artystów](aspnet-mvc-4-helpers-forms-and-validation/_static/image6.png "obcięty tytułów i artystów nazwy")</span><span class="sxs-lookup"><span data-stu-id="92a11-244">![Truncated titles and artists names](aspnet-mvc-4-helpers-forms-and-validation/_static/image6.png "Truncated titles and artists names")</span></span>

    <span data-ttu-id="92a11-245">*Skrócona tytułów i nazwy wykonawców*</span><span class="sxs-lookup"><span data-stu-id="92a11-245">*Truncated Titles and Artist Names*</span></span>

<a id="Exercise3"></a>

<a id="Exercise_3_Creating_the_Edit_View"></a>
### <a name="exercise-3-creating-the-edit-view"></a><span data-ttu-id="92a11-246">Ćwiczenie 3: Tworzenie widoku edycji</span><span class="sxs-lookup"><span data-stu-id="92a11-246">Exercise 3: Creating the Edit View</span></span>

<span data-ttu-id="92a11-247">W tym ćwiczeniu dowiesz się, jak utworzyć formularz umożliwia menedżerów magazynu do edycji albumu.</span><span class="sxs-lookup"><span data-stu-id="92a11-247">In this exercise, you will learn how to create a form to allow store managers to edit an Album.</span></span> <span data-ttu-id="92a11-248">Będzie przeglądania **/StoreManager/Edit/id** adresu URL (**identyfikator** jest unikatowy identyfikator albumy, aby edytować), dzięki czemu wywołanie HTTP GET do serwera.</span><span class="sxs-lookup"><span data-stu-id="92a11-248">They will browse the **/StoreManager/Edit/id** URL (**id** being the unique id of the album to edit), thus making an HTTP-GET call to the server.</span></span>

<span data-ttu-id="92a11-249">Metody akcji kontrolera Edytuj będzie pobrać odpowiednią Album z bazy danych, Utwórz **StoreManagerViewModel** obiekt, aby hermetyzować go (wraz z listy artystów i Genres), a następnie przekazać go do szablonu widoku w celu renderowanie strony HTML do użytkownika.</span><span class="sxs-lookup"><span data-stu-id="92a11-249">The Controller Edit action method will retrieve the appropriate Album from the database, create a **StoreManagerViewModel** object to encapsulate it (along with a list of Artists and Genres), and then pass it off to a View template to render the HTML page back to the user.</span></span> <span data-ttu-id="92a11-250">Ta strona będzie zawierać  **&lt;formularza&gt;**  element z pola tekstowe i listę rozwijaną do edycji właściwości albumu.</span><span class="sxs-lookup"><span data-stu-id="92a11-250">This page will contain a **&lt;form&gt;** element with textboxes and dropdowns for editing the Album properties.</span></span>

<span data-ttu-id="92a11-251">Po aktualizacji albumu wartości formularza i klika użytkownika **zapisać** przycisku zmiany są przesyłane za pośrednictwem HTTP POST wywołania zwrotnego **/StoreManager/Edit/id**. Mimo że adres URL jest taka sama jak w ostatnim wywołaniu, ASP.NET MVC określi, że teraz POST protokołu HTTP, a w związku z tym wykonuje różne metody akcji edycji (jeden ozdobione **[HttpPost]**).</span><span class="sxs-lookup"><span data-stu-id="92a11-251">Once the user updates the Album form values and clicks the **Save** button, the changes are submitted via an HTTP-POST call back to **/StoreManager/Edit/id**. Although the URL remains the same as in the last call, ASP.NET MVC identifies that this time it is an HTTP-POST and therefore executes a different Edit action method (one decorated with **[HttpPost]**).</span></span>

<a id="Ex3Task1"></a>

<a id="Task_1_-_Implementing_the_HTTP-GET_Edit_Action_Method"></a>
#### <a name="task-1---implementing-the-http-get-edit-action-method"></a><span data-ttu-id="92a11-252">Zadanie 1 - implementacja metody akcji HTTP GET edycji</span><span class="sxs-lookup"><span data-stu-id="92a11-252">Task 1 - Implementing the HTTP-GET Edit Action Method</span></span>

<span data-ttu-id="92a11-253">To zadanie będzie implementowany wersji HTTP GET metody akcji edycji można pobrać odpowiednią Album z bazy danych, a także listę wszystkich Genres i artystów.</span><span class="sxs-lookup"><span data-stu-id="92a11-253">In this task, you will implement the HTTP-GET version of the Edit action method to retrieve the appropriate Album from the database, as well as a list of all Genres and Artists.</span></span> <span data-ttu-id="92a11-254">Go będzie spakować dane do **StoreManagerViewModel** zdefiniowane w ostatnim kroku, który następnie zostanie przekazany do szablonu widoku do renderowania odpowiedzi z obiektu.</span><span class="sxs-lookup"><span data-stu-id="92a11-254">It will package this data up into the **StoreManagerViewModel** object defined in the last step, which will then be passed to a View template to render the response with.</span></span>

1. <span data-ttu-id="92a11-255">Otwórz **rozpocząć** rozwiązania, znajdujących się na **źródło/Ex3-CreatingTheEditView/Begin/** folderu.</span><span class="sxs-lookup"><span data-stu-id="92a11-255">Open the **Begin** solution located at **Source/Ex3-CreatingTheEditView/Begin/** folder.</span></span> <span data-ttu-id="92a11-256">W przeciwnym razie możesz nadal korzystać **zakończenia** uzyskane rozwiązanie, wykonując poprzednim ćwiczeniu.</span><span class="sxs-lookup"><span data-stu-id="92a11-256">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

    1. <span data-ttu-id="92a11-257">Po otwarciu dostarczonych **rozpocząć** rozwiązania, musisz pobrać niektórych brakujących pakietów NuGet aby kontynuować.</span><span class="sxs-lookup"><span data-stu-id="92a11-257">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="92a11-258">Aby to zrobić, kliknij przycisk **projektu** menu i wybierz **Zarządzaj pakietami NuGet**.</span><span class="sxs-lookup"><span data-stu-id="92a11-258">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
    2. <span data-ttu-id="92a11-259">W **Zarządzaj pakietami NuGet** okna dialogowego, kliknij przycisk **przywrócić** celu pobieranie brakujących pakietów.</span><span class="sxs-lookup"><span data-stu-id="92a11-259">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
    3. <span data-ttu-id="92a11-260">Na koniec Skompiluj rozwiązanie, klikając **kompilacji** | **Kompiluj rozwiązanie**.</span><span class="sxs-lookup"><span data-stu-id="92a11-260">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="92a11-261">Jedną z zalet przy użyciu narzędzia NuGet jest, że nie masz do wysłania wszystkich bibliotek w projekcie, zmniejszenie jego rozmiar projektu.</span><span class="sxs-lookup"><span data-stu-id="92a11-261">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="92a11-262">Narzędzia Power NuGet określając wersje pakietów w pliku Packages.config, będzie można pobrać wymaganych bibliotek podczas pierwszego uruchomienia projektu.</span><span class="sxs-lookup"><span data-stu-id="92a11-262">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="92a11-263">Jest to, dlaczego konieczne będzie wykonanie tych kroków, po otwarciu istniejącego rozwiązania z tego laboratorium.</span><span class="sxs-lookup"><span data-stu-id="92a11-263">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="92a11-264">Otwórz **StoreManagerController** klasy.</span><span class="sxs-lookup"><span data-stu-id="92a11-264">Open the **StoreManagerController** class.</span></span> <span data-ttu-id="92a11-265">Aby to zrobić, rozwiń węzeł **kontrolerów** folder i kliknij dwukrotnie **StoreManagerController.cs**.</span><span class="sxs-lookup"><span data-stu-id="92a11-265">To do this, expand the **Controllers** folder and double-click **StoreManagerController.cs**.</span></span>
3. <span data-ttu-id="92a11-266">Zastąp **Edytuj HTTP GET** metodę akcji za pomocą następujący kod, aby pobrać odpowiednią **albumu** , jak również **Genres** i **artystów**listy.</span><span class="sxs-lookup"><span data-stu-id="92a11-266">Replace the **HTTP-GET Edit** action method with the following code to retrieve the appropriate **Album** as well as the **Genres** and **Artists** lists.</span></span>

    <span data-ttu-id="92a11-267">(Fragment - kodu *ASP.NET MVC 4 pomocników i formularzy i sprawdzanie poprawności — Akcja Ex3 StoreManagerController HTTP GET Edytuj*)</span><span class="sxs-lookup"><span data-stu-id="92a11-267">(Code Snippet - *ASP.NET MVC 4 Helpers and Forms and Validation - Ex3 StoreManagerController HTTP-GET Edit action*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample9.cs)]

    > [!NOTE]
    > <span data-ttu-id="92a11-268">Używasz **System.Web.Mvc** **SelectList** dla artystów i Genres zamiast **system.Collections.Generic —** listy.</span><span class="sxs-lookup"><span data-stu-id="92a11-268">You are using **System.Web.Mvc** **SelectList** for Artists and Genres instead of the **System.Collections.Generic** List.</span></span>
    > 
    > <span data-ttu-id="92a11-269">**SelectList** bardziej przejrzysty sposób wypełnić listę rozwijaną HTML i zarządzanie nimi elementów, jak bieżące zaznaczenie.</span><span class="sxs-lookup"><span data-stu-id="92a11-269">**SelectList** is a cleaner way to populate HTML dropdowns and manage things like current selection.</span></span> <span data-ttu-id="92a11-270">Tworzenie wystąpień i nowsze, konfigurując te obiekty ViewModel w akcji kontrolera spowoduje, że scenariusza formularza edycji oczyszczarki.</span><span class="sxs-lookup"><span data-stu-id="92a11-270">Instantiating and later setting up these ViewModel objects in the controller action will make the Edit form scenario cleaner.</span></span>

<a id="Ex3Task2"></a>

<a id="Task_2_-_Creating_the_Edit_View"></a>
#### <a name="task-2---creating-the-edit-view"></a><span data-ttu-id="92a11-271">Zadanie 2 — Tworzenie widoku edycji</span><span class="sxs-lookup"><span data-stu-id="92a11-271">Task 2 - Creating the Edit View</span></span>

<span data-ttu-id="92a11-272">W ramach tego zadania spowoduje utworzenie szablonu Edytuj widok, który później będzie mógł wyświetlić właściwości albumu.</span><span class="sxs-lookup"><span data-stu-id="92a11-272">In this task, you will create an Edit View template that will later display the album properties.</span></span>

1. <span data-ttu-id="92a11-273">Utwórz widok edycji.</span><span class="sxs-lookup"><span data-stu-id="92a11-273">Create the Edit View.</span></span> <span data-ttu-id="92a11-274">Aby to zrobić, kliknij prawym przyciskiem myszy wewnątrz **Edytuj** metody akcji i wybierz **Dodaj widok**.</span><span class="sxs-lookup"><span data-stu-id="92a11-274">To do this, right-click inside the **Edit** action method and select **Add View**.</span></span>
2. <span data-ttu-id="92a11-275">W oknie dialogowym Dodaj widok, sprawdź, czy nazwa widoku jest **Edytuj**.</span><span class="sxs-lookup"><span data-stu-id="92a11-275">In the Add View dialog, verify that the View Name is **Edit**.</span></span> <span data-ttu-id="92a11-276">Sprawdź **utworzyć widok jednoznacznie** wyboru i wybierz **albumu (MvcMusicStore.Models)** z **wyświetlić klasy danych** listy rozwijanej.</span><span class="sxs-lookup"><span data-stu-id="92a11-276">Check the **Create a strongly-typed view** checkbox and select **Album (MvcMusicStore.Models)** from the **View data class** drop-down.</span></span> <span data-ttu-id="92a11-277">Wybierz **Edytuj** z **szablonu szkieletu** listy rozwijanej.</span><span class="sxs-lookup"><span data-stu-id="92a11-277">Select **Edit** from the **Scaffold template** drop-down.</span></span> <span data-ttu-id="92a11-278">Pozostaw innych pól ich wartości domyślne, a następnie kliknij przycisk **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="92a11-278">Leave the other fields with their default value and then click **Add**.</span></span>

    <span data-ttu-id="92a11-279">![Dodawanie widoku edycji](aspnet-mvc-4-helpers-forms-and-validation/_static/image7.png "Dodawanie widoku edycji")</span><span class="sxs-lookup"><span data-stu-id="92a11-279">![Adding an Edit view](aspnet-mvc-4-helpers-forms-and-validation/_static/image7.png "Adding an Edit view")</span></span>

    <span data-ttu-id="92a11-280">*Dodawanie widoku edycji*</span><span class="sxs-lookup"><span data-stu-id="92a11-280">*Adding an Edit view*</span></span>

<a id="Ex3Task3"></a>

<a id="Task_3_-_Running_the_Application"></a>
#### <a name="task-3---running-the-application"></a><span data-ttu-id="92a11-281">Zadanie 3 — uruchamianie aplikacji</span><span class="sxs-lookup"><span data-stu-id="92a11-281">Task 3 - Running the Application</span></span>

<span data-ttu-id="92a11-282">W tym zadaniu zostanie sprawdzić, czy **StoreManager** **edytować** strona widoku są wyświetlane wartości właściwości albumu przekazany jako parametr.</span><span class="sxs-lookup"><span data-stu-id="92a11-282">In this task, you will test that the **StoreManager** **Edit** View page displays the properties' values for the album passed as parameter.</span></span>

1. <span data-ttu-id="92a11-283">Naciśnij klawisz **F5** do uruchomienia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="92a11-283">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="92a11-284">Na stronie głównej datę rozpoczęcia projektu.</span><span class="sxs-lookup"><span data-stu-id="92a11-284">The project starts in the Home page.</span></span> <span data-ttu-id="92a11-285">Zmień adres URL do **/StoreManager/Edit/1** Aby sprawdzić, czy są wyświetlane wartości właściwości dla albumu przekazany.</span><span class="sxs-lookup"><span data-stu-id="92a11-285">Change the URL to **/StoreManager/Edit/1** to verify that the properties' values for the album passed are displayed.</span></span>

    <span data-ttu-id="92a11-286">![Przeglądanie widoku edycji albumu](aspnet-mvc-4-helpers-forms-and-validation/_static/image8.png "przeglądanie albumu widoku edycji")</span><span class="sxs-lookup"><span data-stu-id="92a11-286">![Browsing Album's Edit View](aspnet-mvc-4-helpers-forms-and-validation/_static/image8.png "Browsing Album's Edit View")</span></span>

    <span data-ttu-id="92a11-287">*Przeglądanie widoku edycji albumu*</span><span class="sxs-lookup"><span data-stu-id="92a11-287">*Browsing Album's Edit view*</span></span>

<a id="Ex3Task4"></a>

<a id="Task_4_-_Implementing_drop-downs_on_the_Album_Editor_Template"></a>
#### <a name="task-4---implementing-drop-downs-on-the-album-editor-template"></a><span data-ttu-id="92a11-288">Zadanie 4 — wdrażanie listach rozwijanych w szablonie albumu edytora</span><span class="sxs-lookup"><span data-stu-id="92a11-288">Task 4 - Implementing drop-downs on the Album Editor Template</span></span>

<span data-ttu-id="92a11-289">W tym zadaniu dodasz listach rozwijanych do Wyświetl szablon utworzony w ostatnim zadaniem, dzięki czemu użytkownik może wybrać z listy artystów i Genres.</span><span class="sxs-lookup"><span data-stu-id="92a11-289">In this task, you will add drop-downs to the View template created in the last task, so that the user can select from a list of Artists and Genres.</span></span>

1. <span data-ttu-id="92a11-290">Zamień wszystkie **albumu** fieldset kod następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="92a11-290">Replace all the **Album** fieldset code with the following:</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample10.cshtml)]

    > [!NOTE]
    > <span data-ttu-id="92a11-291">**Html.DropDownList** pomocnika został dodany do listy rozwijane wyboru artystów i Genres renderowania.</span><span class="sxs-lookup"><span data-stu-id="92a11-291">An **Html.DropDownList** helper has been added to render drop-downs for choosing Artists and Genres.</span></span> <span data-ttu-id="92a11-292">Parametry przekazywane do **Html.DropDownList** są:</span><span class="sxs-lookup"><span data-stu-id="92a11-292">The parameters passed to **Html.DropDownList** are:</span></span>
    > 
    > 1. <span data-ttu-id="92a11-293">Nazwa pola formularza (**&quot;ArtistId&quot;**).</span><span class="sxs-lookup"><span data-stu-id="92a11-293">The name of the form field (**&quot;ArtistId&quot;**).</span></span>
    > 2. <span data-ttu-id="92a11-294">**SelectList** wartości dla listy rozwijanej.</span><span class="sxs-lookup"><span data-stu-id="92a11-294">The **SelectList** of values for the drop-down.</span></span>

<a id="Ex3Task5"></a>

<a id="Task_5_-_Running_the_Application"></a>
#### <a name="task-5---running-the-application"></a><span data-ttu-id="92a11-295">Zadanie 5 działania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="92a11-295">Task 5 - Running the Application</span></span>

<span data-ttu-id="92a11-296">W tym zadaniu zostanie sprawdzić, czy **StoreManager** **Edytuj** listach rozwijanych zamiast wykonawcy i identyfikator Genre pól tekstowych zostanie wyświetlona strona widoku.</span><span class="sxs-lookup"><span data-stu-id="92a11-296">In this task, you will test that the **StoreManager** **Edit** View page displays drop-downs instead of Artist and Genre ID text fields.</span></span>

1. <span data-ttu-id="92a11-297">Naciśnij klawisz **F5** do uruchomienia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="92a11-297">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="92a11-298">Na stronie głównej datę rozpoczęcia projektu.</span><span class="sxs-lookup"><span data-stu-id="92a11-298">The project starts in the Home page.</span></span> <span data-ttu-id="92a11-299">Zmień adres URL do **/StoreManager/Edit/1** można zweryfikować Wyświetla listach rozwijanych zamiast wykonawcy i identyfikator Genre pól tekstowych.</span><span class="sxs-lookup"><span data-stu-id="92a11-299">Change the URL to **/StoreManager/Edit/1** to verify that it displays drop-downs instead of Artist and Genre ID text fields.</span></span>

    <span data-ttu-id="92a11-300">![Przeglądanie albumu Edytowanie widoku z listy rozwijane](aspnet-mvc-4-helpers-forms-and-validation/_static/image9.png "przeglądanie albumu Edytowanie widoku z listy rozwijane")</span><span class="sxs-lookup"><span data-stu-id="92a11-300">![Browsing Album's Edit View with drop downs](aspnet-mvc-4-helpers-forms-and-validation/_static/image9.png "Browsing Album's Edit View with drop downs")</span></span>

    <span data-ttu-id="92a11-301">*Widok edycji przeglądania albumu, tym razem z list rozwijanych*</span><span class="sxs-lookup"><span data-stu-id="92a11-301">*Browsing Album's Edit view, this time with dropdowns*</span></span>

<a id="Ex3Task6"></a>

<a id="Task_6_-_Implementing_the_HTTP-POST_Edit_action_method"></a>
#### <a name="task-6---implementing-the-http-post-edit-action-method"></a><span data-ttu-id="92a11-302">Zadanie 6 - implementacja metody akcji POST protokołu HTTP, Edytuj</span><span class="sxs-lookup"><span data-stu-id="92a11-302">Task 6 - Implementing the HTTP-POST Edit action method</span></span>

<span data-ttu-id="92a11-303">Teraz, edytowanie widoku wyświetlane zgodnie z oczekiwaniami, należy wdrożyć metody akcji Edytuj POST protokołu HTTP, aby zapisać zmiany wprowadzone do albumu.</span><span class="sxs-lookup"><span data-stu-id="92a11-303">Now that the Edit View displays as expected, you need to implement the HTTP-POST Edit Action method to save the changes made to the Album.</span></span>

1. <span data-ttu-id="92a11-304">Zamknij przeglądarkę, jeśli to konieczne, aby powrócić do okna programu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="92a11-304">Close the browser if needed, to return to the Visual Studio window.</span></span> <span data-ttu-id="92a11-305">Otwórz **StoreManagerController** z **kontrolerów** folderu.</span><span class="sxs-lookup"><span data-stu-id="92a11-305">Open **StoreManagerController** from the **Controllers** folder.</span></span>
2. <span data-ttu-id="92a11-306">Zastąp **Edytuj HTTP POST** kod do metody akcji z następującymi (należy pamiętać, że metoda, którą należy zastąpić przeciążone wersji, która otrzymuje dwa parametry):</span><span class="sxs-lookup"><span data-stu-id="92a11-306">Replace **HTTP-POST Edit** action method code with the following (note that the method that must be replaced is overloaded version that receives two parameters):</span></span>

    <span data-ttu-id="92a11-307">(Fragment - kodu *ASP.NET MVC 4 pomocników i formularzy i sprawdzanie poprawności — Edytuj HTTP POST StoreManagerController Ex3 akcji*)</span><span class="sxs-lookup"><span data-stu-id="92a11-307">(Code Snippet - *ASP.NET MVC 4 Helpers and Forms and Validation - Ex3 StoreManagerController HTTP-POST Edit action*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample11.cs)]

    > [!NOTE]
    > <span data-ttu-id="92a11-308">Ta metoda zostanie wykonany, gdy użytkownik kliknie **zapisać** przycisk widoku i wykonuje HTTP POST wartości formularza do serwera zachowywał je w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="92a11-308">This method will be executed when the user clicks the **Save** button of the View and performs an HTTP-POST of the form values back to the server to persist them in the database.</span></span> <span data-ttu-id="92a11-309">Dekorator **[HttpPost]** wskazuje, czy można użyć metody w tych scenariuszach POST protokołu HTTP.</span><span class="sxs-lookup"><span data-stu-id="92a11-309">The decorator **[HttpPost]** indicates that the method should be used for those HTTP-POST scenarios.</span></span> <span data-ttu-id="92a11-310">Metoda korzysta z **albumu** obiektu.</span><span class="sxs-lookup"><span data-stu-id="92a11-310">The method takes an **Album** object.</span></span> <span data-ttu-id="92a11-311">ASP.NET MVC zostanie automatycznie utworzony obiekt Album z ogłaszanymi &lt;formularza&gt; wartości.</span><span class="sxs-lookup"><span data-stu-id="92a11-311">ASP.NET MVC will automatically create the Album object from the posted &lt;form&gt; values.</span></span>
    > 
    > <span data-ttu-id="92a11-312">Metoda zostaną wykonane następujące kroki:</span><span class="sxs-lookup"><span data-stu-id="92a11-312">The method will perform these steps:</span></span>
    > 
    > 1. <span data-ttu-id="92a11-313">Jeśli model jest prawidłowy:</span><span class="sxs-lookup"><span data-stu-id="92a11-313">If model is valid:</span></span>
    > 
    >     1. <span data-ttu-id="92a11-314">Zaktualizuj wpis albumu w kontekście oznaczyć je jako zmodyfikowanego obiektu.</span><span class="sxs-lookup"><span data-stu-id="92a11-314">Update the album entry in the context to mark it as a modified object.</span></span>
    >     2. <span data-ttu-id="92a11-315">Zapisz zmiany i Przekierowanie do widoku indeksu.</span><span class="sxs-lookup"><span data-stu-id="92a11-315">Save the changes and redirect to the index view.</span></span>
    > 2. <span data-ttu-id="92a11-316">Jeśli model nie jest prawidłowy, wypełnia obiekt ViewBag z **GenreId** i **ArtistId**, wówczas zostanie zwrócone widok z odebranego obiektu albumy, aby zezwolić użytkownikowi wykonać wszelkie wymagane aktualizacji.</span><span class="sxs-lookup"><span data-stu-id="92a11-316">If the model is not valid, it will populate the ViewBag with the **GenreId** and **ArtistId**, then it will return the view with the received Album object to allow the user perform any required update.</span></span>

<a id="Ex3Task7"></a>

<a id="Task_7_-_Running_the_Application"></a>
#### <a name="task-7---running-the-application"></a><span data-ttu-id="92a11-317">Zadanie 7 - uruchamiania aplikacji</span><span class="sxs-lookup"><span data-stu-id="92a11-317">Task 7 - Running the Application</span></span>

<span data-ttu-id="92a11-318">W tym zadaniu zostanie sprawdzić, czy **edycji StoreManager** — Wyświetl stronę faktycznie zapisuje zaktualizowanych danych albumu w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="92a11-318">In this task, you will test that the **StoreManager Edit** View page actually saves the updated Album data in the database.</span></span>

1. <span data-ttu-id="92a11-319">Naciśnij klawisz **F5** do uruchomienia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="92a11-319">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="92a11-320">Na stronie głównej datę rozpoczęcia projektu.</span><span class="sxs-lookup"><span data-stu-id="92a11-320">The project starts in the Home page.</span></span> <span data-ttu-id="92a11-321">Zmień adres URL do **/StoreManager/Edit/1**.</span><span class="sxs-lookup"><span data-stu-id="92a11-321">Change the URL to **/StoreManager/Edit/1**.</span></span> <span data-ttu-id="92a11-322">Zmień tytuł albumu do **obciążenia** i wybierz polecenie **zapisać**.</span><span class="sxs-lookup"><span data-stu-id="92a11-322">Change the Album title to **Load** and click on **Save**.</span></span> <span data-ttu-id="92a11-323">Sprawdź, czy tytuł albumu rzeczywiste zmiany na liście albumów.</span><span class="sxs-lookup"><span data-stu-id="92a11-323">Verify that album's title actually changed in the list of albums.</span></span>

    <span data-ttu-id="92a11-324">![Aktualizowanie albumu](aspnet-mvc-4-helpers-forms-and-validation/_static/image10.png "aktualizowanie albumu")</span><span class="sxs-lookup"><span data-stu-id="92a11-324">![Updating an album](aspnet-mvc-4-helpers-forms-and-validation/_static/image10.png "Updating an album")</span></span>

    <span data-ttu-id="92a11-325">*Aktualizowanie albumu*</span><span class="sxs-lookup"><span data-stu-id="92a11-325">*Updating an Album*</span></span>

<a id="Exercise4"></a>

<a id="Exercise_4_Adding_a_Create_View"></a>
### <a name="exercise-4-adding-a-create-view"></a><span data-ttu-id="92a11-326">Ćwiczenie 4: Dodawanie widoku Create</span><span class="sxs-lookup"><span data-stu-id="92a11-326">Exercise 4: Adding a Create View</span></span>

<span data-ttu-id="92a11-327">Teraz, gdy **StoreManagerController** obsługuje **Edytuj** możliwości, w tym ćwiczeniu dowiesz się jak dodać szablon Utwórz widok pozwala przechowywać menedżerowie dodawać nowe albumów do aplikacji.</span><span class="sxs-lookup"><span data-stu-id="92a11-327">Now that the **StoreManagerController** supports the **Edit** ability, in this exercise you will learn how to add a Create View template to let store managers add new Albums to the application.</span></span>

<span data-ttu-id="92a11-328">Tak jak w przypadku funkcji Edytuj, zostaną zaimplementowane scenariusz Utwórz za pomocą dwóch osobnych metod w ramach **StoreManagerController** klasy:</span><span class="sxs-lookup"><span data-stu-id="92a11-328">Like you did with the Edit functionality, you will implement the Create scenario using two separate methods within the **StoreManagerController** class:</span></span>

1. <span data-ttu-id="92a11-329">Jedna metoda akcji zostanie wyświetlony pusty formularz menedżerów magazynu najpierw odwiedź **/StoreManager/Utwórz** adresu URL.</span><span class="sxs-lookup"><span data-stu-id="92a11-329">One action method will display an empty form when store managers first visit the **/StoreManager/Create** URL.</span></span>
2. <span data-ttu-id="92a11-330">Druga metoda akcji obsłuży scenariusza gdzie kliknie Menedżer magazynu **zapisać** przycisk w formularzu i przesyła wartości z powrotem do **/StoreManager/Utwórz** adresem URL HTTP POST.</span><span class="sxs-lookup"><span data-stu-id="92a11-330">A second action method will handle the scenario where the store manager clicks the **Save** button within the form and submits the values back to the **/StoreManager/Create** URL as an HTTP-POST.</span></span>

<a id="Ex4Task1"></a>

<a id="Task_1_-_Implementing_the_HTTP-GET_Create_action_method"></a>
#### <a name="task-1---implementing-the-http-get-create-action-method"></a><span data-ttu-id="92a11-331">Zadanie 1 - implementacja metody akcji tworzenia HTTP GET</span><span class="sxs-lookup"><span data-stu-id="92a11-331">Task 1 - Implementing the HTTP-GET Create action method</span></span>

<span data-ttu-id="92a11-332">W tym zadaniu wdroży wersji HTTP GET, utwórz metody akcji, aby pobrać listę wszystkich Genres i artystów, spakować dane do **StoreManagerViewModel** obiektu, który następnie zostanie przekazany do szablonu widoku.</span><span class="sxs-lookup"><span data-stu-id="92a11-332">In this task, you will implement the HTTP-GET version of the Create action method to retrieve a list of all Genres and Artists, package this data up into a **StoreManagerViewModel** object, which will then be passed to a View template.</span></span>

1. <span data-ttu-id="92a11-333">Otwórz **rozpocząć** rozwiązania, znajdujących się na **źródło/Ex4-AddingACreateView/Begin/** folderu.</span><span class="sxs-lookup"><span data-stu-id="92a11-333">Open the **Begin** solution located at **Source/Ex4-AddingACreateView/Begin/** folder.</span></span> <span data-ttu-id="92a11-334">W przeciwnym razie możesz nadal korzystać **zakończenia** uzyskane rozwiązanie, wykonując poprzednim ćwiczeniu.</span><span class="sxs-lookup"><span data-stu-id="92a11-334">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

    1. <span data-ttu-id="92a11-335">Po otwarciu dostarczonych **rozpocząć** rozwiązania, musisz pobrać niektórych brakujących pakietów NuGet aby kontynuować.</span><span class="sxs-lookup"><span data-stu-id="92a11-335">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="92a11-336">Aby to zrobić, kliknij przycisk **projektu** menu i wybierz **Zarządzaj pakietami NuGet**.</span><span class="sxs-lookup"><span data-stu-id="92a11-336">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
    2. <span data-ttu-id="92a11-337">W **Zarządzaj pakietami NuGet** okna dialogowego, kliknij przycisk **przywrócić** celu pobieranie brakujących pakietów.</span><span class="sxs-lookup"><span data-stu-id="92a11-337">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
    3. <span data-ttu-id="92a11-338">Na koniec Skompiluj rozwiązanie, klikając **kompilacji** | **Kompiluj rozwiązanie**.</span><span class="sxs-lookup"><span data-stu-id="92a11-338">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="92a11-339">Jedną z zalet przy użyciu narzędzia NuGet jest, że nie masz do wysłania wszystkich bibliotek w projekcie, zmniejszenie jego rozmiar projektu.</span><span class="sxs-lookup"><span data-stu-id="92a11-339">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="92a11-340">Narzędzia Power NuGet określając wersje pakietów w pliku Packages.config, będzie można pobrać wymaganych bibliotek podczas pierwszego uruchomienia projektu.</span><span class="sxs-lookup"><span data-stu-id="92a11-340">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="92a11-341">Jest to, dlaczego konieczne będzie wykonanie tych kroków, po otwarciu istniejącego rozwiązania z tego laboratorium.</span><span class="sxs-lookup"><span data-stu-id="92a11-341">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="92a11-342">Otwórz **StoreManagerController** klasy.</span><span class="sxs-lookup"><span data-stu-id="92a11-342">Open **StoreManagerController** class.</span></span> <span data-ttu-id="92a11-343">Aby to zrobić, rozwiń węzeł **kontrolerów** folder i kliknij dwukrotnie **StoreManagerController.cs**.</span><span class="sxs-lookup"><span data-stu-id="92a11-343">To do this, expand the **Controllers** folder and double-click **StoreManagerController.cs**.</span></span>
3. <span data-ttu-id="92a11-344">Zastąp **Utwórz** kod metody akcji z następujących czynności:</span><span class="sxs-lookup"><span data-stu-id="92a11-344">Replace the **Create** action method code with the following:</span></span>

    <span data-ttu-id="92a11-345">(Fragment - kodu *ASP.NET MVC 4 pomocników i formularzy i sprawdzanie poprawności — utworzenie HTTP GET StoreManagerController Ex4*)</span><span class="sxs-lookup"><span data-stu-id="92a11-345">(Code Snippet - *ASP.NET MVC 4 Helpers and Forms and Validation - Ex4 StoreManagerController HTTP-GET Create action*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample12.cs)]

<a id="Ex4Task2"></a>

<a id="Task_2_-_Adding_the_Create_View"></a>
#### <a name="task-2---adding-the-create-view"></a><span data-ttu-id="92a11-346">Zadanie 2 — Dodawanie widoku Create</span><span class="sxs-lookup"><span data-stu-id="92a11-346">Task 2 - Adding the Create View</span></span>

<span data-ttu-id="92a11-347">To zadanie spowoduje dodanie szablonu tworzenia widoku, który wyświetli nowy formularz albumu (pusta).</span><span class="sxs-lookup"><span data-stu-id="92a11-347">In this task, you will add the Create View template that will display a new (empty) Album form.</span></span>

1. <span data-ttu-id="92a11-348">Kliknij prawym przyciskiem myszy wewnątrz **Utwórz** metody akcji i wybierz **Dodaj widok**.</span><span class="sxs-lookup"><span data-stu-id="92a11-348">Right-click inside the **Create** action method and select **Add View**.</span></span> <span data-ttu-id="92a11-349">Zostanie wyświetlone okno dialogowe dodawania widoku.</span><span class="sxs-lookup"><span data-stu-id="92a11-349">This will bring up the Add View dialog.</span></span>
2. <span data-ttu-id="92a11-350">W oknie dialogowym Dodaj widok, sprawdź, czy nazwa widoku jest **Utwórz**.</span><span class="sxs-lookup"><span data-stu-id="92a11-350">In the Add View dialog, verify that the View Name is **Create**.</span></span> <span data-ttu-id="92a11-351">Wybierz **utworzyć widok jednoznacznie** opcję i zaznacz **albumu (MvcMusicStore.Models)** z **klasa modelu** listy rozwijanej i **Utwórz** z **szablonu szkieletu** listy rozwijanej.</span><span class="sxs-lookup"><span data-stu-id="92a11-351">Select the **Create a strongly-typed view** option and select **Album (MvcMusicStore.Models)** from the **Model class** drop-down and **Create** from the **Scaffold template** drop-down.</span></span> <span data-ttu-id="92a11-352">Pozostaw innych pól ich wartości domyślne, a następnie kliknij przycisk **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="92a11-352">Leave the other fields with their default value and then click **Add**.</span></span>

    <span data-ttu-id="92a11-353">![Dodawanie widoku create](aspnet-mvc-4-helpers-forms-and-validation/_static/image11.png "Dodawanie a-create-view.png")</span><span class="sxs-lookup"><span data-stu-id="92a11-353">![Adding a create view](aspnet-mvc-4-helpers-forms-and-validation/_static/image11.png "adding-a-create-view.png")</span></span>

    <span data-ttu-id="92a11-354">*Dodawanie widoku Create*</span><span class="sxs-lookup"><span data-stu-id="92a11-354">*Adding the Create View*</span></span>
3. <span data-ttu-id="92a11-355">Aktualizacja **GenreId** i **ArtistId** pola, aby użyć listy rozwijanej, jak pokazano poniżej:</span><span class="sxs-lookup"><span data-stu-id="92a11-355">Update the **GenreId** and **ArtistId** fields to use a drop-down list as shown below:</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample13.cshtml)]

<a id="Ex4Task3"></a>

<a id="Task_3_-_Running_the_Application"></a>
#### <a name="task-3---running-the-application"></a><span data-ttu-id="92a11-356">Zadanie 3 — uruchamianie aplikacji</span><span class="sxs-lookup"><span data-stu-id="92a11-356">Task 3 - Running the Application</span></span>

<span data-ttu-id="92a11-357">W tym zadaniu zostanie sprawdzić, czy **StoreManager** **Utwórz** pusty formularz albumu zostanie wyświetlona strona widoku.</span><span class="sxs-lookup"><span data-stu-id="92a11-357">In this task, you will test that the **StoreManager** **Create** View page displays an empty Album form.</span></span>

1. <span data-ttu-id="92a11-358">Naciśnij klawisz **F5** do uruchomienia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="92a11-358">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="92a11-359">Na stronie głównej datę rozpoczęcia projektu.</span><span class="sxs-lookup"><span data-stu-id="92a11-359">The project starts in the Home page.</span></span> <span data-ttu-id="92a11-360">Zmień adres URL do **/StoreManager/utworzyć**.</span><span class="sxs-lookup"><span data-stu-id="92a11-360">Change the URL to **/StoreManager/Create**.</span></span> <span data-ttu-id="92a11-361">Sprawdź, czy pusty formularz jest wyświetlany do wypełniania nowe właściwości albumu.</span><span class="sxs-lookup"><span data-stu-id="92a11-361">Verify that an empty form is displayed for filling the new Album properties.</span></span>

    <span data-ttu-id="92a11-362">![Utwórz widok z pusty formularz](aspnet-mvc-4-helpers-forms-and-validation/_static/image12.png "Utwórz widok z pusty formularz")</span><span class="sxs-lookup"><span data-stu-id="92a11-362">![Create View with an empty form](aspnet-mvc-4-helpers-forms-and-validation/_static/image12.png "Create View with an empty form")</span></span>

    <span data-ttu-id="92a11-363">*Utwórz widok z pusty formularz*</span><span class="sxs-lookup"><span data-stu-id="92a11-363">*Create View with an empty form*</span></span>

<a id="Ex4Task4"></a>

<a id="Task_4_-_Implementing_the_HTTP-POST_Create_Action_Method"></a>
#### <a name="task-4---implementing-the-http-post-create-action-method"></a><span data-ttu-id="92a11-364">Zadanie 4 — wdrażanie POST protokołu HTTP, utwórz metody akcji</span><span class="sxs-lookup"><span data-stu-id="92a11-364">Task 4 - Implementing the HTTP-POST Create Action Method</span></span>

<span data-ttu-id="92a11-365">W tym zadaniu wdroży wersji HTTP POST Utwórz metody akcji, która będzie wywoływana, gdy użytkownik kliknie **zapisać** przycisku.</span><span class="sxs-lookup"><span data-stu-id="92a11-365">In this task, you will implement the HTTP-POST version of the Create action method that will be invoked when a user clicks the **Save** button.</span></span> <span data-ttu-id="92a11-366">Metoda należy zapisać nowy album w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="92a11-366">The method should save the new album in the database.</span></span>

1. <span data-ttu-id="92a11-367">Zamknij przeglądarkę, jeśli to konieczne, aby powrócić do okna programu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="92a11-367">Close the browser if needed, to return to the Visual Studio window.</span></span> <span data-ttu-id="92a11-368">Otwórz **StoreManagerController** klasy.</span><span class="sxs-lookup"><span data-stu-id="92a11-368">Open **StoreManagerController** class.</span></span> <span data-ttu-id="92a11-369">Aby to zrobić, rozwiń węzeł **kontrolerów** folder i kliknij dwukrotnie **StoreManagerController.cs**.</span><span class="sxs-lookup"><span data-stu-id="92a11-369">To do this, expand the **Controllers** folder and double-click **StoreManagerController.cs**.</span></span>
2. <span data-ttu-id="92a11-370">Zastąp **POST protokołu HTTP, Utwórz** kod metody akcji z następujących czynności:</span><span class="sxs-lookup"><span data-stu-id="92a11-370">Replace **HTTP-POST Create** action method code with the following:</span></span>

    <span data-ttu-id="92a11-371">(Fragment - kodu *ASP.NET MVC 4 pomocników i formularzy i sprawdzanie poprawności — Ex4 StoreManagerController HTTP POST tworzenie akcji*)</span><span class="sxs-lookup"><span data-stu-id="92a11-371">(Code Snippet - *ASP.NET MVC 4 Helpers and Forms and Validation - Ex4 StoreManagerController HTTP- POST Create action*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample14.cs)]

    > [!NOTE]
    > <span data-ttu-id="92a11-372">Akcja Utwórz jest bardzo podobne do poprzedniej edycji metody akcji, ale zamiast ustawienia obiektu, ponieważ zmodyfikowane, jego jest dodawany do kontekstu.</span><span class="sxs-lookup"><span data-stu-id="92a11-372">The Create action is pretty similar to the previous Edit action method but instead of setting the object as modified, it is being added to the context.</span></span>

<a id="Ex4Task5"></a>

<a id="Task_5_-_Running_the_Application"></a>
#### <a name="task-5---running-the-application"></a><span data-ttu-id="92a11-373">Zadanie 5 działania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="92a11-373">Task 5 - Running the Application</span></span>

<span data-ttu-id="92a11-374">W tym zadaniu zostanie sprawdzić, czy **StoreManager utworzyć** strony widoku umożliwia utworzenie nowego albumu, a następnie przekierowuje do widoku indeksu StoreManager.</span><span class="sxs-lookup"><span data-stu-id="92a11-374">In this task, you will test that the **StoreManager Create** View page lets you create a new Album and then redirects to the StoreManager Index View.</span></span>

1. <span data-ttu-id="92a11-375">Naciśnij klawisz **F5** do uruchomienia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="92a11-375">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="92a11-376">Na stronie głównej datę rozpoczęcia projektu.</span><span class="sxs-lookup"><span data-stu-id="92a11-376">The project starts in the Home page.</span></span> <span data-ttu-id="92a11-377">Zmień adres URL do **/StoreManager/utworzyć**.</span><span class="sxs-lookup"><span data-stu-id="92a11-377">Change the URL to **/StoreManager/Create**.</span></span> <span data-ttu-id="92a11-378">Należy wypełnić wszystkie pola formularza z danymi dla nowego albumu, tak jak na poniższej ilustracji:</span><span class="sxs-lookup"><span data-stu-id="92a11-378">Fill all the form fields with data for a new Album, like the one in the following figure:</span></span>

    <span data-ttu-id="92a11-379">![Tworzenie albumu](aspnet-mvc-4-helpers-forms-and-validation/_static/image13.png "tworzenia albumu")</span><span class="sxs-lookup"><span data-stu-id="92a11-379">![Creating an Album](aspnet-mvc-4-helpers-forms-and-validation/_static/image13.png "Creating an Album")</span></span>

    <span data-ttu-id="92a11-380">*Tworzenie albumu*</span><span class="sxs-lookup"><span data-stu-id="92a11-380">*Creating an Album*</span></span>
3. <span data-ttu-id="92a11-381">Sprawdź, czy następuje przekierowanie do widoku indeksu StoreManager, która obejmuje nowy Album właśnie utworzony.</span><span class="sxs-lookup"><span data-stu-id="92a11-381">Verify that you get redirected to the StoreManager Index View that includes the new Album just created.</span></span>

    <span data-ttu-id="92a11-382">![Nowy Album utworzony](aspnet-mvc-4-helpers-forms-and-validation/_static/image14.png "nowy Album utworzone")</span><span class="sxs-lookup"><span data-stu-id="92a11-382">![New Album Created](aspnet-mvc-4-helpers-forms-and-validation/_static/image14.png "New Album Created")</span></span>

    <span data-ttu-id="92a11-383">*Nowy Album utworzone*</span><span class="sxs-lookup"><span data-stu-id="92a11-383">*New Album Created*</span></span>

<a id="Exercise5"></a>

<a id="Exercise_5_Handling_Deletion"></a>
### <a name="exercise-5-handling-deletion"></a><span data-ttu-id="92a11-384">Ćwiczenie 5: Obsługa usuwania</span><span class="sxs-lookup"><span data-stu-id="92a11-384">Exercise 5: Handling Deletion</span></span>

<span data-ttu-id="92a11-385">Możliwość usunięcia albumów nie została jeszcze zaimplementowana.</span><span class="sxs-lookup"><span data-stu-id="92a11-385">The ability to delete albums is not yet implemented.</span></span> <span data-ttu-id="92a11-386">To jest tym ćwiczeniu będzie o.</span><span class="sxs-lookup"><span data-stu-id="92a11-386">This is what this exercise will be about.</span></span> <span data-ttu-id="92a11-387">Tak jak wcześniej, zostanie wdrożyć scenariusz usuwania za pomocą dwóch osobnych metod w ramach **StoreManagerController** klasy:</span><span class="sxs-lookup"><span data-stu-id="92a11-387">Like before, you will implement the Delete scenario using two separate methods within the **StoreManagerController** class:</span></span>

1. <span data-ttu-id="92a11-388">Jedna metoda akcji spowoduje wyświetlenie formularza potwierdzenia</span><span class="sxs-lookup"><span data-stu-id="92a11-388">One action method will display a confirmation form</span></span>
2. <span data-ttu-id="92a11-389">Druga metoda akcji obsłuży przesyłania formularza</span><span class="sxs-lookup"><span data-stu-id="92a11-389">A second action method will handle the form submission</span></span>

<a id="Ex5Task1"></a>

<a id="Task_1_-_Implementing_the_HTTP-GET_Delete_Action_Method"></a>
#### <a name="task-1---implementing-the-http-get-delete-action-method"></a><span data-ttu-id="92a11-390">Zadanie 1 - implementacja metody akcji Delete HTTP GET</span><span class="sxs-lookup"><span data-stu-id="92a11-390">Task 1 - Implementing the HTTP-GET Delete Action Method</span></span>

<span data-ttu-id="92a11-391">W tym zadaniu zaimplementowaniem wersji HTTP GET metody akcji usuwania można pobrać informacji o albumu.</span><span class="sxs-lookup"><span data-stu-id="92a11-391">In this task, you will implement the HTTP-GET version of the Delete action method to retrieve the album's information.</span></span>

1. <span data-ttu-id="92a11-392">Otwórz **rozpocząć** rozwiązania, znajdujących się na **źródło/Ex5-HandlingDeletion/Begin/** folderu.</span><span class="sxs-lookup"><span data-stu-id="92a11-392">Open the **Begin** solution located at **Source/Ex5-HandlingDeletion/Begin/** folder.</span></span> <span data-ttu-id="92a11-393">W przeciwnym razie możesz nadal korzystać **zakończenia** uzyskane rozwiązanie, wykonując poprzednim ćwiczeniu.</span><span class="sxs-lookup"><span data-stu-id="92a11-393">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

    1. <span data-ttu-id="92a11-394">Po otwarciu dostarczonych **rozpocząć** rozwiązania, musisz pobrać niektórych brakujących pakietów NuGet aby kontynuować.</span><span class="sxs-lookup"><span data-stu-id="92a11-394">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="92a11-395">Aby to zrobić, kliknij przycisk **projektu** menu i wybierz **Zarządzaj pakietami NuGet**.</span><span class="sxs-lookup"><span data-stu-id="92a11-395">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
    2. <span data-ttu-id="92a11-396">W **Zarządzaj pakietami NuGet** okna dialogowego, kliknij przycisk **przywrócić** celu pobieranie brakujących pakietów.</span><span class="sxs-lookup"><span data-stu-id="92a11-396">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
    3. <span data-ttu-id="92a11-397">Na koniec Skompiluj rozwiązanie, klikając **kompilacji** | **Kompiluj rozwiązanie**.</span><span class="sxs-lookup"><span data-stu-id="92a11-397">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="92a11-398">Jedną z zalet przy użyciu narzędzia NuGet jest, że nie masz do wysłania wszystkich bibliotek w projekcie, zmniejszenie jego rozmiar projektu.</span><span class="sxs-lookup"><span data-stu-id="92a11-398">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="92a11-399">Narzędzia Power NuGet określając wersje pakietów w pliku Packages.config, będzie można pobrać wymaganych bibliotek podczas pierwszego uruchomienia projektu.</span><span class="sxs-lookup"><span data-stu-id="92a11-399">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="92a11-400">Jest to, dlaczego konieczne będzie wykonanie tych kroków, po otwarciu istniejącego rozwiązania z tego laboratorium.</span><span class="sxs-lookup"><span data-stu-id="92a11-400">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="92a11-401">Otwórz **StoreManagerController** klasy.</span><span class="sxs-lookup"><span data-stu-id="92a11-401">Open **StoreManagerController** class.</span></span> <span data-ttu-id="92a11-402">Aby to zrobić, rozwiń węzeł **kontrolerów** folder i kliknij dwukrotnie **StoreManagerController.cs**.</span><span class="sxs-lookup"><span data-stu-id="92a11-402">To do this, expand the **Controllers** folder and double-click **StoreManagerController.cs**.</span></span>
3. <span data-ttu-id="92a11-403">Akcja usuwania kontrolera jest dokładnie taka sama poprzedniej akcji kontrolera szczegóły magazynu: wysyła zapytanie **albumu** obiektów z bazy danych przy użyciu **identyfikator** podany adres URL i zwraca odpowiednie **widoku**.</span><span class="sxs-lookup"><span data-stu-id="92a11-403">The Delete controller action is exactly the same as the previous Store Details controller action: it queries the **album** object from the database using the **id** provided in the URL and returns the appropriate **View**.</span></span> <span data-ttu-id="92a11-404">Aby to zrobić, Zamień HTTP GET **usunąć** kod metody akcji z następujących czynności:</span><span class="sxs-lookup"><span data-stu-id="92a11-404">To do this, replace the HTTP-GET **Delete** action method code with the following:</span></span>

    <span data-ttu-id="92a11-405">(Fragment - kodu *ASP.NET MVC 4 pomocników i formularzy i sprawdzanie poprawności — Ex5 obsługi usunięcia HTTP GET akcję usunięcia*)</span><span class="sxs-lookup"><span data-stu-id="92a11-405">(Code Snippet - *ASP.NET MVC 4 Helpers and Forms and Validation - Ex5 Handling Deletion HTTP-GET Delete action*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample15.cs)]
4. <span data-ttu-id="92a11-406">Kliknij prawym przyciskiem myszy wewnątrz **usunąć** metody akcji i wybierz **Dodaj widok**.</span><span class="sxs-lookup"><span data-stu-id="92a11-406">Right-click inside the **Delete** action method and select **Add View**.</span></span> <span data-ttu-id="92a11-407">Zostanie wyświetlone okno dialogowe dodawania widoku.</span><span class="sxs-lookup"><span data-stu-id="92a11-407">This will bring up the Add View dialog.</span></span>
5. <span data-ttu-id="92a11-408">W oknie dialogowym Dodaj widok, sprawdź, czy nazwa widoku jest **usunąć**.</span><span class="sxs-lookup"><span data-stu-id="92a11-408">In the Add View dialog, verify that the View name is **Delete**.</span></span> <span data-ttu-id="92a11-409">Wybierz **utworzyć widok jednoznacznie** opcję i zaznacz **albumu (MvcMusicStore.Models)** z **klasa modelu** listy rozwijanej.</span><span class="sxs-lookup"><span data-stu-id="92a11-409">Select the **Create a strongly-typed view** option and select **Album (MvcMusicStore.Models)** from the **Model class** drop-down.</span></span> <span data-ttu-id="92a11-410">Wybierz **usunąć** z **szablonu szkieletu** listy rozwijanej.</span><span class="sxs-lookup"><span data-stu-id="92a11-410">Select **Delete** from the **Scaffold template** drop-down.</span></span> <span data-ttu-id="92a11-411">Pozostaw innych pól ich wartości domyślne, a następnie kliknij przycisk **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="92a11-411">Leave the other fields with their default value and then click **Add**.</span></span>

    <span data-ttu-id="92a11-412">![Dodawanie widoku Delete](aspnet-mvc-4-helpers-forms-and-validation/_static/image15.png "Dodawanie widoku Delete")</span><span class="sxs-lookup"><span data-stu-id="92a11-412">![Adding a Delete View](aspnet-mvc-4-helpers-forms-and-validation/_static/image15.png "Adding a Delete View")</span></span>

    <span data-ttu-id="92a11-413">*Dodawanie widoku Delete*</span><span class="sxs-lookup"><span data-stu-id="92a11-413">*Adding a Delete View*</span></span>
6. <span data-ttu-id="92a11-414">Usuń szablon pokazuje wszystkie pola z modelu.</span><span class="sxs-lookup"><span data-stu-id="92a11-414">The Delete template shows all the fields from the model.</span></span> <span data-ttu-id="92a11-415">Zostaną wyświetlone tylko tytuł albumu.</span><span class="sxs-lookup"><span data-stu-id="92a11-415">You will show only the album's title.</span></span> <span data-ttu-id="92a11-416">Aby to zrobić, Zamień zawartość widoku z następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="92a11-416">To do this, replace the content of the view with the following code:</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample16.cshtml)]

<a id="Ex05Task2"></a>

<a id="Task_2_-_Running_the_Application"></a>
#### <a name="task-2---running-the-application"></a><span data-ttu-id="92a11-417">Zadanie 2 — w uruchomionej aplikacji</span><span class="sxs-lookup"><span data-stu-id="92a11-417">Task 2 - Running the Application</span></span>

<span data-ttu-id="92a11-418">W tym zadaniu zostanie sprawdzić, czy **StoreManager** **usunąć** potwierdzenie usunięcia formularza zostanie wyświetlona strona widoku.</span><span class="sxs-lookup"><span data-stu-id="92a11-418">In this task, you will test that the **StoreManager** **Delete** View page displays a confirmation deletion form.</span></span>

1. <span data-ttu-id="92a11-419">Naciśnij klawisz **F5** do uruchomienia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="92a11-419">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="92a11-420">Na stronie głównej datę rozpoczęcia projektu.</span><span class="sxs-lookup"><span data-stu-id="92a11-420">The project starts in the Home page.</span></span> <span data-ttu-id="92a11-421">Zmień adres URL do **/StoreManager**.</span><span class="sxs-lookup"><span data-stu-id="92a11-421">Change the URL to **/StoreManager**.</span></span> <span data-ttu-id="92a11-422">Wybierz albumów można usunąć, klikając **usunąć** i upewnij się, że nowy widok została przekazana.</span><span class="sxs-lookup"><span data-stu-id="92a11-422">Select one album to delete by clicking **Delete** and verify that the new view is uploaded.</span></span>

    <span data-ttu-id="92a11-423">![Usuwanie albumu](aspnet-mvc-4-helpers-forms-and-validation/_static/image16.png "Usuwanie albumu")</span><span class="sxs-lookup"><span data-stu-id="92a11-423">![Deleting an Album](aspnet-mvc-4-helpers-forms-and-validation/_static/image16.png "Deleting an Album")</span></span>

    <span data-ttu-id="92a11-424">*Usuwanie albumu*</span><span class="sxs-lookup"><span data-stu-id="92a11-424">*Deleting an Album*</span></span>

<a id="Ex05Task3"></a>

<a id="Task_3-_Implementing_the_HTTP-POST_Delete_Action_Method"></a>
#### <a name="task-3--implementing-the-http-post-delete-action-method"></a><span data-ttu-id="92a11-425">Zadanie 3 — implementacja metody akcji POST protokołu HTTP Delete</span><span class="sxs-lookup"><span data-stu-id="92a11-425">Task 3- Implementing the HTTP-POST Delete Action Method</span></span>

<span data-ttu-id="92a11-426">W tym zadaniu wdroży wersji HTTP POST metody akcji usuwania, która będzie wywoływana, gdy użytkownik kliknie **usunąć** przycisku.</span><span class="sxs-lookup"><span data-stu-id="92a11-426">In this task, you will implement the HTTP-POST version of the Delete action method that will be invoked when a user clicks the **Delete** button.</span></span> <span data-ttu-id="92a11-427">Metoda, należy usunąć albumu w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="92a11-427">The method should delete the album in the database.</span></span>

1. <span data-ttu-id="92a11-428">Zamknij przeglądarkę, jeśli to konieczne, aby powrócić do okna programu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="92a11-428">Close the browser if needed, to return to the Visual Studio window.</span></span> <span data-ttu-id="92a11-429">Otwórz **StoreManagerController** klasy.</span><span class="sxs-lookup"><span data-stu-id="92a11-429">Open **StoreManagerController** class.</span></span> <span data-ttu-id="92a11-430">Aby to zrobić, rozwiń węzeł **kontrolerów** folder i kliknij dwukrotnie **StoreManagerController.cs**.</span><span class="sxs-lookup"><span data-stu-id="92a11-430">To do this, expand the **Controllers** folder and double-click **StoreManagerController.cs**.</span></span>
2. <span data-ttu-id="92a11-431">Zastąp **usunąć HTTP POST** kod metody akcji z następujących czynności:</span><span class="sxs-lookup"><span data-stu-id="92a11-431">Replace **HTTP-POST Delete** action method code with the following:</span></span>

    <span data-ttu-id="92a11-432">(Fragment - kodu *ASP.NET MVC 4 pomocników i formularzy i sprawdzanie poprawności — Ex5 obsługi usunięcia HTTP POST akcji*)</span><span class="sxs-lookup"><span data-stu-id="92a11-432">(Code Snippet - *ASP.NET MVC 4 Helpers and Forms and Validation - Ex5 Handling Deletion HTTP-POST Delete action*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample17.cs)]

<a id="Ex5Task4"></a>

<a id="Task_4_-_Running_the_Application"></a>
#### <a name="task-4---running-the-application"></a><span data-ttu-id="92a11-433">Zadanie 4 — uruchamianie aplikacji</span><span class="sxs-lookup"><span data-stu-id="92a11-433">Task 4 - Running the Application</span></span>

<span data-ttu-id="92a11-434">W tym zadaniu zostanie sprawdzić, czy **StoreManager Delete** strony widoku umożliwia usuwanie albumu, a następnie przekierowuje do widoku indeksu StoreManager.</span><span class="sxs-lookup"><span data-stu-id="92a11-434">In this task, you will test that the **StoreManager Delete** View page lets you delete an Album and then redirects to the StoreManager Index View.</span></span>

1. <span data-ttu-id="92a11-435">Naciśnij klawisz **F5** do uruchomienia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="92a11-435">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="92a11-436">Na stronie głównej datę rozpoczęcia projektu.</span><span class="sxs-lookup"><span data-stu-id="92a11-436">The project starts in the Home page.</span></span> <span data-ttu-id="92a11-437">Zmień adres URL do **/StoreManager**.</span><span class="sxs-lookup"><span data-stu-id="92a11-437">Change the URL to **/StoreManager**.</span></span> <span data-ttu-id="92a11-438">Wybierz albumów można usunąć, klikając **usunąć.**</span><span class="sxs-lookup"><span data-stu-id="92a11-438">Select one album to delete by clicking **Delete.**</span></span> <span data-ttu-id="92a11-439">Potwierdź decyzję, klikając **usunąć** przycisk:</span><span class="sxs-lookup"><span data-stu-id="92a11-439">Confirm the deletion by clicking **Delete** button:</span></span>

    <span data-ttu-id="92a11-440">![Usuwanie albumu](aspnet-mvc-4-helpers-forms-and-validation/_static/image17.png "Usuwanie albumu")</span><span class="sxs-lookup"><span data-stu-id="92a11-440">![Deleting an Album](aspnet-mvc-4-helpers-forms-and-validation/_static/image17.png "Deleting an Album")</span></span>

    <span data-ttu-id="92a11-441">*Usuwanie albumu*</span><span class="sxs-lookup"><span data-stu-id="92a11-441">*Deleting an Album*</span></span>
3. <span data-ttu-id="92a11-442">Sprawdź, czy album została usunięta, ponieważ nie ma w **indeksu** strony.</span><span class="sxs-lookup"><span data-stu-id="92a11-442">Verify that the album was deleted since it does not appear in the **Index** page.</span></span>

<a id="Exercise6"></a>

<a id="Exercise_6_Adding_Validation"></a>
### <a name="exercise-6-adding-validation"></a><span data-ttu-id="92a11-443">Ćwiczenie 6: Dodawanie walidacji</span><span class="sxs-lookup"><span data-stu-id="92a11-443">Exercise 6: Adding Validation</span></span>

<span data-ttu-id="92a11-444">Obecnie tworzenie i edytowanie formularzy, które zostały spełnione, nie należy wykonywać dowolny rodzaj sprawdzania poprawności.</span><span class="sxs-lookup"><span data-stu-id="92a11-444">Currently, the Create and Edit forms you have in place do not perform any kind of validation.</span></span> <span data-ttu-id="92a11-445">Jeśli użytkownik opuści wymaganego pola puste lub typ litery w polu Cena, zostanie wyświetlony błąd będzie się z bazy danych.</span><span class="sxs-lookup"><span data-stu-id="92a11-445">If the user leaves a required field blank or type letters in the price field, the first error you will get will be from the database.</span></span>

<span data-ttu-id="92a11-446">Sprawdzanie poprawności można dodać do aplikacji przez dodanie do własnej klasy modelu adnotacji danych.</span><span class="sxs-lookup"><span data-stu-id="92a11-446">You can add validation to the application by adding Data Annotations to your model class.</span></span> <span data-ttu-id="92a11-447">Adnotacje danych umożliwiają zawierająca opis reguły, które chcesz zastosować do właściwości modelu i ASP.NET MVC zajmie się wymuszanie i komunikatem odpowiednie dla użytkowników.</span><span class="sxs-lookup"><span data-stu-id="92a11-447">Data Annotations allow describing the rules you want applied to your model properties, and ASP.NET MVC will take care of enforcing and displaying appropriate message to users.</span></span>

<a id="Ex06Task1"></a>

<a id="Task_1_-_Adding_Data_Annotations"></a>
#### <a name="task-1---adding-data-annotations"></a><span data-ttu-id="92a11-448">Zadanie 1 — Dodawanie adnotacji danych</span><span class="sxs-lookup"><span data-stu-id="92a11-448">Task 1 - Adding Data Annotations</span></span>

<span data-ttu-id="92a11-449">To zadanie spowoduje dodanie adnotacji danych do modelu albumy, aby ułatwić tworzenie i edytowanie strony ekranu komunikatów dotyczących sprawdzania poprawności, gdy jest to konieczne.</span><span class="sxs-lookup"><span data-stu-id="92a11-449">In this task, you will add Data Annotations to the Album Model that will make the Create and Edit page display validation messages when appropriate.</span></span>

<span data-ttu-id="92a11-450">Proste klasy modelu, dodawanie adnotacji danych tylko odbywa się przez dodanie **przy użyciu** instrukcji dla **System.ComponentModel.DataAnnotation**, następnie umieszczenie **[wymagane]**atrybutu na odpowiednie właściwości.</span><span class="sxs-lookup"><span data-stu-id="92a11-450">For a simple Model class, adding a Data Annotation is just handled by adding a **using** statement for **System.ComponentModel.DataAnnotation**, then placing a **[Required]** attribute on the appropriate properties.</span></span> <span data-ttu-id="92a11-451">Poniższy przykład spowodowałoby **nazwa** właściwości wymaganego pola w widoku.</span><span class="sxs-lookup"><span data-stu-id="92a11-451">The following example would make the **Name** property a required field in the View.</span></span>

[!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample18.cs)]

<span data-ttu-id="92a11-452">To nieco bardziej złożone w przypadkach, takich jak ta aplikacja gdzie jest generowany modelu danych jednostki.</span><span class="sxs-lookup"><span data-stu-id="92a11-452">This is a little more complex in cases like this application where the Entity Data Model is generated.</span></span> <span data-ttu-id="92a11-453">Jeśli dodano adnotacji danych bezpośrednio do klasy modeli, będą one zastąpione po zaktualizowaniu modelu z bazy danych.</span><span class="sxs-lookup"><span data-stu-id="92a11-453">If you added Data Annotations directly to the model classes, they would be overwritten if you update the model from the database.</span></span> <span data-ttu-id="92a11-454">Zamiast tego możesz wprowadzić korzystanie z klasy częściowe metadanych, które będą istnieć do przechowywania adnotacje i są skojarzone z tym modelem klasy przy użyciu **[MetadataType]** atrybutu.</span><span class="sxs-lookup"><span data-stu-id="92a11-454">Instead, you can make use of metadata partial classes which will exist to hold the annotations and are associated with the model classes using the **[MetadataType]** attribute.</span></span>

1. <span data-ttu-id="92a11-455">Otwórz **rozpocząć** rozwiązania, znajdujących się na **źródło/Ex6-AddingValidation/Begin/** folderu.</span><span class="sxs-lookup"><span data-stu-id="92a11-455">Open the **Begin** solution located at **Source/Ex6-AddingValidation/Begin/** folder.</span></span> <span data-ttu-id="92a11-456">W przeciwnym razie możesz nadal korzystać **zakończenia** uzyskane rozwiązanie, wykonując poprzednim ćwiczeniu.</span><span class="sxs-lookup"><span data-stu-id="92a11-456">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

    1. <span data-ttu-id="92a11-457">Po otwarciu dostarczonych **rozpocząć** rozwiązania, musisz pobrać niektórych brakujących pakietów NuGet aby kontynuować.</span><span class="sxs-lookup"><span data-stu-id="92a11-457">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="92a11-458">Aby to zrobić, kliknij przycisk **projektu** menu i wybierz **Zarządzaj pakietami NuGet**.</span><span class="sxs-lookup"><span data-stu-id="92a11-458">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
    2. <span data-ttu-id="92a11-459">W **Zarządzaj pakietami NuGet** okna dialogowego, kliknij przycisk **przywrócić** celu pobieranie brakujących pakietów.</span><span class="sxs-lookup"><span data-stu-id="92a11-459">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
    3. <span data-ttu-id="92a11-460">Na koniec Skompiluj rozwiązanie, klikając **kompilacji** | **Kompiluj rozwiązanie**.</span><span class="sxs-lookup"><span data-stu-id="92a11-460">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="92a11-461">Jedną z zalet przy użyciu narzędzia NuGet jest, że nie masz do wysłania wszystkich bibliotek w projekcie, zmniejszenie jego rozmiar projektu.</span><span class="sxs-lookup"><span data-stu-id="92a11-461">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="92a11-462">Narzędzia Power NuGet określając wersje pakietów w pliku Packages.config, będzie można pobrać wymaganych bibliotek podczas pierwszego uruchomienia projektu.</span><span class="sxs-lookup"><span data-stu-id="92a11-462">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="92a11-463">Jest to, dlaczego konieczne będzie wykonanie tych kroków, po otwarciu istniejącego rozwiązania z tego laboratorium.</span><span class="sxs-lookup"><span data-stu-id="92a11-463">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="92a11-464">Otwórz **Album.cs** z **modele** folderu.</span><span class="sxs-lookup"><span data-stu-id="92a11-464">Open the **Album.cs** from the **Models** folder.</span></span>
3. <span data-ttu-id="92a11-465">Zastąp **Album.cs** zawartości ze wyróżniony kod, aby wyglądały one podobnie do następującej:</span><span class="sxs-lookup"><span data-stu-id="92a11-465">Replace **Album.cs** content with the highlighted code, so that it looks like the following:</span></span>

    > [!NOTE]
    > <span data-ttu-id="92a11-466">Wiersz **[DisplayFormat(ConvertEmptyStringToNull=false)]** wskazuje, czy puste ciągi z modelu nie będą konwertowane do wartości null po zaktualizowaniu pole danych w źródle danych.</span><span class="sxs-lookup"><span data-stu-id="92a11-466">The line **[DisplayFormat(ConvertEmptyStringToNull=false)]** indicates that empty strings from the model won't be converted to null when the data field is updated in the data source.</span></span> <span data-ttu-id="92a11-467">To ustawienie można będzie zapobiec Wystąpił wyjątek podczas programu Entity Framework przed adnotacji danych sprawdza poprawność pól przypisuje wartości null do modelu.</span><span class="sxs-lookup"><span data-stu-id="92a11-467">This setting will avoid an exception when the Entity Framework assigns null values to the model before Data Annotation validates the fields.</span></span>

    <span data-ttu-id="92a11-468">(Fragment - kodu *ASP.NET MVC 4 pomocników i formularzy i sprawdzanie poprawności — częściowej klasy metadanych albumu Ex6*)</span><span class="sxs-lookup"><span data-stu-id="92a11-468">(Code Snippet - *ASP.NET MVC 4 Helpers and Forms and Validation - Ex6 Album metadata partial class*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample19.cs)]

    > [!NOTE]
    > <span data-ttu-id="92a11-469">To **albumu** ma częściowej klasy **MetadataType** atrybut, który wskazuje **AlbumMetaData** klasy dla adnotacji danych.</span><span class="sxs-lookup"><span data-stu-id="92a11-469">This **Album** partial class has a **MetadataType** attribute which points to the **AlbumMetaData** class for the Data Annotations.</span></span> <span data-ttu-id="92a11-470">Oto niektóre atrybuty adnotacji danych używanego dla adnotacji modelu albumu:</span><span class="sxs-lookup"><span data-stu-id="92a11-470">These are some of the Data Annotation attributes you are using to annotate the Album model:</span></span>
    > 
    > - <span data-ttu-id="92a11-471">Niewymagana — wskazuje, że właściwość jest polem obowiązkowym</span><span class="sxs-lookup"><span data-stu-id="92a11-471">Required - Indicates that the property is a required field</span></span>
    > - <span data-ttu-id="92a11-472">Nazwa wyświetlana — Określa tekst, który ma być używany z pola formularza i komunikatów dotyczących sprawdzania poprawności</span><span class="sxs-lookup"><span data-stu-id="92a11-472">DisplayName - Defines the text to be used on form fields and validation messages</span></span>
    > - <span data-ttu-id="92a11-473">DisplayFormat — określa sposób wyświetlania i sformatowane pola danych.</span><span class="sxs-lookup"><span data-stu-id="92a11-473">DisplayFormat - Specifies how data fields are displayed and formatted.</span></span>
    > - <span data-ttu-id="92a11-474">StringLength — określa maksymalną długość pola ciągu</span><span class="sxs-lookup"><span data-stu-id="92a11-474">StringLength - Defines a maximum length for a string field</span></span>
    > - <span data-ttu-id="92a11-475">Zakres — zapewnia maksymalne i minimalne wartości pola numerycznego</span><span class="sxs-lookup"><span data-stu-id="92a11-475">Range - Gives a maximum and minimum value for a numeric field</span></span>
    > - <span data-ttu-id="92a11-476">ScaffoldColumn — umożliwia ukrywanie pól z edytora formularzy</span><span class="sxs-lookup"><span data-stu-id="92a11-476">ScaffoldColumn - Allows hiding fields from editor forms</span></span>

<a id="Ex06Task2"></a>

<a id="Task_2_-_Running_the_Application"></a>
#### <a name="task-2---running-the-application"></a><span data-ttu-id="92a11-477">Zadanie 2 — w uruchomionej aplikacji</span><span class="sxs-lookup"><span data-stu-id="92a11-477">Task 2 - Running the Application</span></span>

<span data-ttu-id="92a11-478">To zadanie będzie testowane czy tworzenie i edytowanie strony weryfikacji pól, przy użyciu nazw wyświetlanych w ostatnim zadaniem.</span><span class="sxs-lookup"><span data-stu-id="92a11-478">In this task, you will test that the Create and Edit pages validate fields, using the display names chosen in the last task.</span></span>

1. <span data-ttu-id="92a11-479">Naciśnij klawisz **F5** do uruchomienia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="92a11-479">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="92a11-480">Na stronie głównej datę rozpoczęcia projektu.</span><span class="sxs-lookup"><span data-stu-id="92a11-480">The project starts in the Home page.</span></span> <span data-ttu-id="92a11-481">Zmień adres URL do **/StoreManager/utworzyć**.</span><span class="sxs-lookup"><span data-stu-id="92a11-481">Change the URL to **/StoreManager/Create**.</span></span> <span data-ttu-id="92a11-482">Sprawdź, czy nazwy wyświetlane są zgodne z typami klasy częściowej (takich jak **albumów graficznych URL** zamiast **AlbumArtUrl**)</span><span class="sxs-lookup"><span data-stu-id="92a11-482">Verify that the display names match the ones in the partial class (like **Album Art URL** instead of **AlbumArtUrl**)</span></span>
3. <span data-ttu-id="92a11-483">Kliknij przycisk **Utwórz**, bez wypełniania formularza.</span><span class="sxs-lookup"><span data-stu-id="92a11-483">Click **Create**, without filling the form.</span></span> <span data-ttu-id="92a11-484">Sprawdź, czy możesz uzyskać odpowiednie komunikatów dotyczących sprawdzania poprawności.</span><span class="sxs-lookup"><span data-stu-id="92a11-484">Verify that you get the corresponding validation messages.</span></span>

    <span data-ttu-id="92a11-485">![Zweryfikowany pola na stronie Utwórz](aspnet-mvc-4-helpers-forms-and-validation/_static/image18.png "zweryfikowane pola na stronie Utwórz")</span><span class="sxs-lookup"><span data-stu-id="92a11-485">![Validated fields in the Create page](aspnet-mvc-4-helpers-forms-and-validation/_static/image18.png "Validated fields in the Create page")</span></span>

    <span data-ttu-id="92a11-486">*Sprawdzone pola na stronie Utwórz*</span><span class="sxs-lookup"><span data-stu-id="92a11-486">*Validated fields in the Create page*</span></span>
4. <span data-ttu-id="92a11-487">Możesz sprawdzić, czy występuje takie same z **Edytuj** strony.</span><span class="sxs-lookup"><span data-stu-id="92a11-487">You can verify that the same occurs with the **Edit** page.</span></span> <span data-ttu-id="92a11-488">Zmień adres URL do **/StoreManager/Edit/1** i upewnij się, że nazwy wyświetlane są zgodne z typami klasy częściowej (takich jak **albumów graficznych URL** zamiast **AlbumArtUrl**).</span><span class="sxs-lookup"><span data-stu-id="92a11-488">Change the URL to **/StoreManager/Edit/1** and verify that the display names match the ones in the partial class (like **Album Art URL** instead of **AlbumArtUrl**).</span></span> <span data-ttu-id="92a11-489">Pusty **tytuł** i **cen** pola i kliknij przycisk **zapisać**.</span><span class="sxs-lookup"><span data-stu-id="92a11-489">Empty the **Title** and **Price** fields and click **Save**.</span></span> <span data-ttu-id="92a11-490">Sprawdź, czy możesz uzyskać odpowiednie komunikatów dotyczących sprawdzania poprawności.</span><span class="sxs-lookup"><span data-stu-id="92a11-490">Verify that you get the corresponding validation messages.</span></span>

    ![Sprawdzone pola na stronie edycji](aspnet-mvc-4-helpers-forms-and-validation/_static/image19.png)

    <span data-ttu-id="92a11-492">Sprawdzone pola na stronie edycji</span><span class="sxs-lookup"><span data-stu-id="92a11-492">*Validated fields in the Edit page*</span></span>

<a id="Exercise7"></a>

<a id="Exercise_7_Using_Unobtrusive_jQuery_at_Client_Side"></a>
### <a name="exercise-7-using-unobtrusive-jquery-at-client-side"></a><span data-ttu-id="92a11-493">Ćwiczenie 7: Przy użyciu jQuery dyskretny kod po stronie klienta</span><span class="sxs-lookup"><span data-stu-id="92a11-493">Exercise 7: Using Unobtrusive jQuery at Client Side</span></span>

<span data-ttu-id="92a11-494">W tym ćwiczeniu dowiesz się, jak włączyć dotyczącą weryfikacji jQuery MVC 4 dyskretny kod po stronie klienta.</span><span class="sxs-lookup"><span data-stu-id="92a11-494">In this exercise, you will learn how to enable MVC 4 Unobtrusive jQuery validation at client side.</span></span>

> [!NOTE]
> <span data-ttu-id="92a11-495">Dyskretnego kodu jQuery używa prefiksu danych ajax JavaScript do wywołania metody akcji na serwer, a nie emisji intrusively wbudowane skrypty klienta.</span><span class="sxs-lookup"><span data-stu-id="92a11-495">The Unobtrusive jQuery uses data-ajax prefix JavaScript to invoke action methods on the server rather than intrusively emitting inline client scripts.</span></span>


<a id="Ex7Task1"></a>

<a id="Task_1_-_Running_the_Application_before_Enabling_Unobtrusive_jQuery"></a>
#### <a name="task-1---running-the-application-before-enabling-unobtrusive-jquery"></a><span data-ttu-id="92a11-496">Zadanie 1 - uruchamiania aplikacji przed włączeniem dyskretnego kodu jQuery</span><span class="sxs-lookup"><span data-stu-id="92a11-496">Task 1 - Running the Application before Enabling Unobtrusive jQuery</span></span>

<span data-ttu-id="92a11-497">W ramach tego zadania należy uruchomić aplikację przed tym jQuery, aby porównać oba modele sprawdzania poprawności.</span><span class="sxs-lookup"><span data-stu-id="92a11-497">In this task, you will run the application before including jQuery in order to compare both validation models.</span></span>

1. <span data-ttu-id="92a11-498">Otwórz **rozpocząć** rozwiązania, znajdujących się na **źródło/Ex7-UnobtrusivejQueryValidation/Begin/** folderu.</span><span class="sxs-lookup"><span data-stu-id="92a11-498">Open the **Begin** solution located at **Source/Ex7-UnobtrusivejQueryValidation/Begin/** folder.</span></span> <span data-ttu-id="92a11-499">W przeciwnym razie możesz nadal korzystać **zakończenia** uzyskane rozwiązanie, wykonując poprzednim ćwiczeniu.</span><span class="sxs-lookup"><span data-stu-id="92a11-499">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

    1. <span data-ttu-id="92a11-500">Po otwarciu dostarczonych **rozpocząć** rozwiązania, musisz pobrać niektórych brakujących pakietów NuGet aby kontynuować.</span><span class="sxs-lookup"><span data-stu-id="92a11-500">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="92a11-501">Aby to zrobić, kliknij przycisk **projektu** menu i wybierz **Zarządzaj pakietami NuGet**.</span><span class="sxs-lookup"><span data-stu-id="92a11-501">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
    2. <span data-ttu-id="92a11-502">W **Zarządzaj pakietami NuGet** okna dialogowego, kliknij przycisk **przywrócić** celu pobieranie brakujących pakietów.</span><span class="sxs-lookup"><span data-stu-id="92a11-502">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
    3. <span data-ttu-id="92a11-503">Na koniec Skompiluj rozwiązanie, klikając **kompilacji** | **Kompiluj rozwiązanie**.</span><span class="sxs-lookup"><span data-stu-id="92a11-503">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="92a11-504">Jedną z zalet przy użyciu narzędzia NuGet jest, że nie masz do wysłania wszystkich bibliotek w projekcie, zmniejszenie jego rozmiar projektu.</span><span class="sxs-lookup"><span data-stu-id="92a11-504">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="92a11-505">Narzędzia Power NuGet określając wersje pakietów w pliku Packages.config, będzie można pobrać wymaganych bibliotek podczas pierwszego uruchomienia projektu.</span><span class="sxs-lookup"><span data-stu-id="92a11-505">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="92a11-506">Jest to, dlaczego konieczne będzie wykonanie tych kroków, po otwarciu istniejącego rozwiązania z tego laboratorium.</span><span class="sxs-lookup"><span data-stu-id="92a11-506">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="92a11-507">Naciśnij klawisz **F5** do uruchomienia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="92a11-507">Press **F5** to run the application.</span></span>
3. <span data-ttu-id="92a11-508">Na stronie głównej datę rozpoczęcia projektu.</span><span class="sxs-lookup"><span data-stu-id="92a11-508">The project starts in the Home page.</span></span> <span data-ttu-id="92a11-509">Przeglądaj **/StoreManager/Utwórz** i kliknij przycisk **Utwórz** bez wypełniania formularza, aby sprawdzić, czy możesz uzyskać komunikatów dotyczących sprawdzania poprawności:</span><span class="sxs-lookup"><span data-stu-id="92a11-509">Browse **/StoreManager/Create** and click **Create** without filling the form to verify that you get validation messages:</span></span>

    <span data-ttu-id="92a11-510">![Sprawdzanie poprawności klienta wyłączone](aspnet-mvc-4-helpers-forms-and-validation/_static/image20.png "weryfikacji klienta wyłączone")</span><span class="sxs-lookup"><span data-stu-id="92a11-510">![Client validation disabled](aspnet-mvc-4-helpers-forms-and-validation/_static/image20.png "Client validation disabled")</span></span>

    <span data-ttu-id="92a11-511">*Sprawdzanie poprawności klienta wyłączone*</span><span class="sxs-lookup"><span data-stu-id="92a11-511">*Client validation disabled*</span></span>
4. <span data-ttu-id="92a11-512">W przeglądarce otwórz kod źródłowy HTML:</span><span class="sxs-lookup"><span data-stu-id="92a11-512">In the browser, open the HTML source code:</span></span>

    [!code-html[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample20.html)]

<a id="Ex7Task2"></a>

<a id="Task_2_-_Enabling_Unobtrusive_Client_Validation"></a>
#### <a name="task-2---enabling-unobtrusive-client-validation"></a><span data-ttu-id="92a11-513">Zadanie 2 — Włączanie sprawdzania poprawności dyskretnego kodu klienta</span><span class="sxs-lookup"><span data-stu-id="92a11-513">Task 2 - Enabling Unobtrusive Client Validation</span></span>

<span data-ttu-id="92a11-514">To zadanie spowoduje włączenie jQuery **sprawdzania poprawności dyskretnego kodu klienta** z **Web.config** pliku, który jest domyślnie ustawiony na wartość false w wszystkie nowe projekty programu ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="92a11-514">In this task, you will enable jQuery **unobtrusive client validation** from **Web.config** file, which is by default set to false in all new ASP.NET MVC 4 projects.</span></span> <span data-ttu-id="92a11-515">Ponadto należy dodać się, że niezbędne skrypty odwołania, aby jQuery pracy dyskretny kod weryfikacji klienta.</span><span class="sxs-lookup"><span data-stu-id="92a11-515">Additionally, you will add the necessary scripts references to make jQuery Unobtrusive Client Validation work.</span></span>

1. <span data-ttu-id="92a11-516">Otwórz **Web.Config** plików w katalogu głównym projektu i upewnij się, że **ClientValidationEnabled** i **UnobtrusiveJavaScriptEnabled** klucze wartości są ustawiane na **true**.</span><span class="sxs-lookup"><span data-stu-id="92a11-516">Open **Web.Config** file at project root, and make sure that the **ClientValidationEnabled** and **UnobtrusiveJavaScriptEnabled** keys values are set to **true**.</span></span>

    [!code-xml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample21.xml)]

    > [!NOTE]
    > <span data-ttu-id="92a11-517">Można także włączyć sprawdzanie poprawności klienta przez kod w Global.asax.cs, aby uzyskać takie same wyniki:</span><span class="sxs-lookup"><span data-stu-id="92a11-517">You can also enable client validation by code at Global.asax.cs to get the same results:</span></span>
    > 
    > <span data-ttu-id="92a11-518">**HtmlHelper.ClientValidationEnabled = true;**</span><span class="sxs-lookup"><span data-stu-id="92a11-518">**HtmlHelper.ClientValidationEnabled = true;**</span></span>
    > 
    > <span data-ttu-id="92a11-519">Ponadto można przypisać atrybutu ClientValidationEnabled w żadnym kontrolerze mają zachowania niestandardowego.</span><span class="sxs-lookup"><span data-stu-id="92a11-519">Additionally, you can assign ClientValidationEnabled attribute into any controller to have a custom behavior.</span></span>
2. <span data-ttu-id="92a11-520">Otwórz **Create.cshtml** w **Views\StoreManager**.</span><span class="sxs-lookup"><span data-stu-id="92a11-520">Open **Create.cshtml** at **Views\StoreManager**.</span></span>
3. <span data-ttu-id="92a11-521">Upewnij się, że następujące pliki skryptu **jquery.validate** i **jquery.validate.unobtrusive**, odwołuje się widok poprzez &quot; **~/bundles/jqueryval** &quot; pakietu.</span><span class="sxs-lookup"><span data-stu-id="92a11-521">Make sure the following script files, **jquery.validate** and **jquery.validate.unobtrusive**, are referenced in the view throught the &quot;**~/bundles/jqueryval**&quot; bundle.</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample22.cshtml)]

    > [!NOTE]
    > <span data-ttu-id="92a11-522">Te biblioteki jQuery zostaną uwzględnione w nowych projektów MVC 4.</span><span class="sxs-lookup"><span data-stu-id="92a11-522">All these jQuery libraries are included in MVC 4 new projects.</span></span> <span data-ttu-id="92a11-523">Można znaleźć więcej bibliotek w **/skrypty** folderu projektu należy.</span><span class="sxs-lookup"><span data-stu-id="92a11-523">You can find more libraries in the **/Scripts** folder of you project.</span></span>
    > 
    > <span data-ttu-id="92a11-524">Aby tej weryfikacji pracy biblioteki, musisz dodać odwołanie do biblioteki framework jQuery.</span><span class="sxs-lookup"><span data-stu-id="92a11-524">In order to make this validation libraries work, you need to add a reference to the jQuery framework library.</span></span> <span data-ttu-id="92a11-525">Ponieważ to odwołanie zostało już dodane na  **\_Layout.cshtml** plików, nie trzeba go dodać w tego widoku.</span><span class="sxs-lookup"><span data-stu-id="92a11-525">Since this reference is already added in the **\_Layout.cshtml** file, you do not need to add it in this particular view.</span></span>

<a id="Ex7Task3"></a>

<a id="Task_3_-_Running_the_Application_Using_Unobtrusive_jQuery_Validation"></a>
#### <a name="task-3---running-the-application-using-unobtrusive-jquery-validation"></a><span data-ttu-id="92a11-526">Zadanie 3 — uruchomiona dyskretnego kodu przy użyciu aplikacji jQuery sprawdzania poprawności</span><span class="sxs-lookup"><span data-stu-id="92a11-526">Task 3 - Running the Application Using Unobtrusive jQuery Validation</span></span>

<span data-ttu-id="92a11-527">W tym zadaniu zostanie sprawdzić, czy **StoreManager** utworzyć widok szablonu przeprowadza weryfikacji po stronie klienta przy użyciu biblioteki jQuery podczas tworzenia nowego albumu.</span><span class="sxs-lookup"><span data-stu-id="92a11-527">In this task, you will test that the **StoreManager** create view template performs client side validation using jQuery libraries when the user creates a new album.</span></span>

1. <span data-ttu-id="92a11-528">Naciśnij klawisz **F5** do uruchomienia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="92a11-528">Press **F5** to run the application.</span></span>
2. <span data-ttu-id="92a11-529">Na stronie głównej datę rozpoczęcia projektu.</span><span class="sxs-lookup"><span data-stu-id="92a11-529">The project starts in the Home page.</span></span> <span data-ttu-id="92a11-530">Przeglądaj **/StoreManager/Utwórz** i kliknij przycisk **Utwórz** bez wypełniania formularza, aby sprawdzić, czy możesz uzyskać komunikatów dotyczących sprawdzania poprawności:</span><span class="sxs-lookup"><span data-stu-id="92a11-530">Browse **/StoreManager/Create** and click **Create** without filling the form to verify that you get validation messages:</span></span>

    <span data-ttu-id="92a11-531">![Sprawdzanie poprawności klienta z jQuery włączone](aspnet-mvc-4-helpers-forms-and-validation/_static/image21.png "weryfikacji klienta z jQuery włączone")</span><span class="sxs-lookup"><span data-stu-id="92a11-531">![Client validation with jQuery enabled](aspnet-mvc-4-helpers-forms-and-validation/_static/image21.png "Client validation with jQuery enabled")</span></span>

    <span data-ttu-id="92a11-532">*Sprawdzanie poprawności klienta z jQuery włączone*</span><span class="sxs-lookup"><span data-stu-id="92a11-532">*Client validation with jQuery enabled*</span></span>
3. <span data-ttu-id="92a11-533">W przeglądarce otwórz kod źródłowy Utwórz widok:</span><span class="sxs-lookup"><span data-stu-id="92a11-533">In the browser, open the source code for Create view:</span></span>

    [!code-html[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample23.html)]

    > [!NOTE]
    > <span data-ttu-id="92a11-534">Dla każdej reguły weryfikacji klienta jQuery dyskretny kod dodaje atrybut z danymi-val -*rulename*=&quot;*komunikat*&quot;.</span><span class="sxs-lookup"><span data-stu-id="92a11-534">For each client validation rule, Unobtrusive jQuery adds an attribute with data-val-*rulename*=&quot;*message*&quot;.</span></span> <span data-ttu-id="92a11-535">Poniżej znajduje się lista znaczników tej Unobtrusive jQuery wstawia do pola wejściowego html do sprawdzania poprawności klienta:</span><span class="sxs-lookup"><span data-stu-id="92a11-535">Below is a list of tags that Unobtrusive jQuery inserts into the html input field to perform client validation:</span></span>
    > 
    > - <span data-ttu-id="92a11-536">Val danych</span><span class="sxs-lookup"><span data-stu-id="92a11-536">Data-val</span></span>
    > - <span data-ttu-id="92a11-537">Data-val-number</span><span class="sxs-lookup"><span data-stu-id="92a11-537">Data-val-number</span></span>
    > - <span data-ttu-id="92a11-538">Zakres danych val</span><span class="sxs-lookup"><span data-stu-id="92a11-538">Data-val-range</span></span>
    > - <span data-ttu-id="92a11-539">Dane val zakresu min / danych val zakresu max.</span><span class="sxs-lookup"><span data-stu-id="92a11-539">Data-val-range-min / Data-val-range-max</span></span>
    > - <span data-ttu-id="92a11-540">Wymagane wartości danych</span><span class="sxs-lookup"><span data-stu-id="92a11-540">Data-val-required</span></span>
    > - <span data-ttu-id="92a11-541">Data-val-length</span><span class="sxs-lookup"><span data-stu-id="92a11-541">Data-val-length</span></span>
    > - <span data-ttu-id="92a11-542">Dane val długość max / danych val długość min</span><span class="sxs-lookup"><span data-stu-id="92a11-542">Data-val-length-max / Data-val-length-min</span></span>
    > 
    > <span data-ttu-id="92a11-543">Wszystkie wartości są wypełniane modelu **adnotacji danych elementu**.</span><span class="sxs-lookup"><span data-stu-id="92a11-543">All the data values are filled with model **Data Annotation**.</span></span> <span data-ttu-id="92a11-544">Następnie całą logikę, która działa po stronie serwera może działać po stronie klienta.</span><span class="sxs-lookup"><span data-stu-id="92a11-544">Then, all the logic that works at server side can be run at client side.</span></span> <span data-ttu-id="92a11-545">Na przykład atrybut cen ma następujące adnotacji danych w modelu:</span><span class="sxs-lookup"><span data-stu-id="92a11-545">For example, Price attribute has the following data annotation in the model:</span></span>
    > 
    > [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample24.cs)]
    > 
    > <span data-ttu-id="92a11-546">Po zakończeniu korzystania z dyskretnego kodu jQuery jest wygenerowanego kodu:</span><span class="sxs-lookup"><span data-stu-id="92a11-546">After using Unobtrusive jQuery, the generated code is:</span></span>
    >  
    > [!code-html[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample25.html)]

* * *

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a><span data-ttu-id="92a11-547">Podsumowanie</span><span class="sxs-lookup"><span data-stu-id="92a11-547">Summary</span></span>

<span data-ttu-id="92a11-548">Wykonując tego laboratorium Hands-On zapoznaniu umożliwienie użytkownikom na zmianę danych przechowywanych w bazie danych przy użyciu następujących czynności:</span><span class="sxs-lookup"><span data-stu-id="92a11-548">By completing this Hands-On Lab you have learned how to enable users to change the data stored in the database with the use of the following:</span></span>

- <span data-ttu-id="92a11-549">Indeks, tworzenia, edytowania, usuwania, takich jak akcji kontrolera</span><span class="sxs-lookup"><span data-stu-id="92a11-549">Controller actions like Index, Create, Edit, Delete</span></span>
- <span data-ttu-id="92a11-550">Funkcja szkieletów ASP.NET MVC do wyświetlania właściwości tabeli HTML</span><span class="sxs-lookup"><span data-stu-id="92a11-550">ASP.NET MVC's scaffolding feature for displaying properties in an HTML table</span></span>
- <span data-ttu-id="92a11-551">Środowisko niestandardowych pomocników HTML zwiększające użytkownika</span><span class="sxs-lookup"><span data-stu-id="92a11-551">Custom HTML helpers to improve user experience</span></span>
- <span data-ttu-id="92a11-552">Metody akcji, które reagują na HTTP GET lub POST protokołu HTTP wywołania</span><span class="sxs-lookup"><span data-stu-id="92a11-552">Action methods that react to either HTTP-GET or HTTP-POST calls</span></span>
- <span data-ttu-id="92a11-553">Szablon udostępniony edytora dla podobnych szablonów widoków, takich jak tworzenie i edytowanie</span><span class="sxs-lookup"><span data-stu-id="92a11-553">A shared editor template for similar View templates like Create and Edit</span></span>
- <span data-ttu-id="92a11-554">Formularz elementów, takich jak listy rozwijane</span><span class="sxs-lookup"><span data-stu-id="92a11-554">Form elements like drop-downs</span></span>
- <span data-ttu-id="92a11-555">Adnotacje danych na potrzeby sprawdzania poprawności modelu</span><span class="sxs-lookup"><span data-stu-id="92a11-555">Data annotations for Model validation</span></span>
- <span data-ttu-id="92a11-556">Weryfikacja po stronie klienta za pomocą biblioteki dyskretnego kodu jQuery</span><span class="sxs-lookup"><span data-stu-id="92a11-556">Client Side Validation using jQuery Unobtrusive library</span></span>

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a><span data-ttu-id="92a11-557">Dodatek A: Instalowanie programu Visual Studio Express 2012 for Web</span><span class="sxs-lookup"><span data-stu-id="92a11-557">Appendix A: Installing Visual Studio Express 2012 for Web</span></span>

<span data-ttu-id="92a11-558">Można zainstalować **Microsoft Visual Studio Express 2012 for Web** lub innym &quot;Express&quot; przy użyciu wersji  **[Instalatora platformy sieci Web firmy Microsoft](https://www.microsoft.com/web/downloads/platform.aspx)** .</span><span class="sxs-lookup"><span data-stu-id="92a11-558">You can install **Microsoft Visual Studio Express 2012 for Web** or another &quot;Express&quot; version using the **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**.</span></span> <span data-ttu-id="92a11-559">Poniższe instrukcje przedstawiono czynności wymagane do zainstalowania *programu Visual studio Express 2012 for Web* przy użyciu *Instalatora platformy sieci Web firmy Microsoft*.</span><span class="sxs-lookup"><span data-stu-id="92a11-559">The following instructions guide you through the steps required to install *Visual studio Express 2012 for Web* using *Microsoft Web Platform Installer*.</span></span>

1. <span data-ttu-id="92a11-560">Przejdź do [ [https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169).</span><span class="sxs-lookup"><span data-stu-id="92a11-560">Go to [[https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169).</span></span> <span data-ttu-id="92a11-561">Alternatywnie, jeśli została już zainstalowana Instalatora platformy sieci Web, można otworzyć go i Wyszukaj produkt &quot; *programu Visual Studio Express 2012 for Web z zestawem Windows Azure SDK*&quot;.</span><span class="sxs-lookup"><span data-stu-id="92a11-561">Alternatively, if you already have installed Web Platform Installer, you can open it and search for the product &quot;*Visual Studio Express 2012 for Web with Windows Azure SDK*&quot;.</span></span>
2. <span data-ttu-id="92a11-562">Polecenie **teraz zainstalować**.</span><span class="sxs-lookup"><span data-stu-id="92a11-562">Click on **Install Now**.</span></span> <span data-ttu-id="92a11-563">Jeśli nie masz **Instalatora platformy sieci Web** nastąpi przekierowanie do pobrania i zainstalowania go najpierw.</span><span class="sxs-lookup"><span data-stu-id="92a11-563">If you do not have **Web Platform Installer** you will be redirected to download and install it first.</span></span>
3. <span data-ttu-id="92a11-564">Raz **Instalatora platformy sieci Web** jest otwarty, kliknij przycisk **zainstalować** można uruchomić Instalatora.</span><span class="sxs-lookup"><span data-stu-id="92a11-564">Once **Web Platform Installer** is open, click **Install** to start the setup.</span></span>

    <span data-ttu-id="92a11-565">![Instalowanie programu Visual Studio Express](aspnet-mvc-4-helpers-forms-and-validation/_static/image22.png "instalacji programu Visual Studio Express")</span><span class="sxs-lookup"><span data-stu-id="92a11-565">![Install Visual Studio Express](aspnet-mvc-4-helpers-forms-and-validation/_static/image22.png "Install Visual Studio Express")</span></span>

    <span data-ttu-id="92a11-566">*Instalowanie programu Visual Studio Express*</span><span class="sxs-lookup"><span data-stu-id="92a11-566">*Install Visual Studio Express*</span></span>
4. <span data-ttu-id="92a11-567">Odczytywanie wszystkich produktów licencji i warunków, a następnie kliknij przycisk **akceptuję** aby kontynuować.</span><span class="sxs-lookup"><span data-stu-id="92a11-567">Read all the products' licenses and terms and click **I Accept** to continue.</span></span>

    ![Akceptowanie umowy licencyjnej](aspnet-mvc-4-helpers-forms-and-validation/_static/image23.png)

    <span data-ttu-id="92a11-569">*Akceptowanie umowy licencyjnej*</span><span class="sxs-lookup"><span data-stu-id="92a11-569">*Accepting the license terms*</span></span>
5. <span data-ttu-id="92a11-570">Poczekaj na zakończenie procesu pobierania i instalacji.</span><span class="sxs-lookup"><span data-stu-id="92a11-570">Wait until the downloading and installation process completes.</span></span>

    ![Postęp instalacji](aspnet-mvc-4-helpers-forms-and-validation/_static/image24.png)

    <span data-ttu-id="92a11-572">*Postęp instalacji*</span><span class="sxs-lookup"><span data-stu-id="92a11-572">*Installation progress*</span></span>
6. <span data-ttu-id="92a11-573">Po zakończeniu instalacji kliknij przycisk **Zakończ**.</span><span class="sxs-lookup"><span data-stu-id="92a11-573">When the installation completes, click **Finish**.</span></span>

    ![Instalacja została zakończona](aspnet-mvc-4-helpers-forms-and-validation/_static/image25.png)

    <span data-ttu-id="92a11-575">*Instalacja została zakończona*</span><span class="sxs-lookup"><span data-stu-id="92a11-575">*Installation completed*</span></span>
7. <span data-ttu-id="92a11-576">Kliknij przycisk **zakończenia** aby zamknąć Instalatora platformy sieci Web.</span><span class="sxs-lookup"><span data-stu-id="92a11-576">Click **Exit** to close Web Platform Installer.</span></span>
8. <span data-ttu-id="92a11-577">Aby otworzyć program Visual Studio Express for Web, przejdź do **Start** ekranu i zacznij pisać &quot; **VS Express**&quot;, następnie kliknij polecenie **VS Express for Web** Kafelek.</span><span class="sxs-lookup"><span data-stu-id="92a11-577">To open Visual Studio Express for Web, go to the **Start** screen and start writing &quot;**VS Express**&quot;, then click on the **VS Express for Web** tile.</span></span>

    ![VS Express for Web kafelka](aspnet-mvc-4-helpers-forms-and-validation/_static/image26.png)

    <span data-ttu-id="92a11-579">*VS Express for Web kafelka*</span><span class="sxs-lookup"><span data-stu-id="92a11-579">*VS Express for Web tile*</span></span>

<a id="AppendixB"></a>

<a id="Appendix_B_Using_Code_Snippets"></a>
## <a name="appendix-b-using-code-snippets"></a><span data-ttu-id="92a11-580">Dodatek B: korzystania z wstawek kodu</span><span class="sxs-lookup"><span data-stu-id="92a11-580">Appendix B: Using Code Snippets</span></span>

<span data-ttu-id="92a11-581">Wstawki kodu zapewniają całego kodu, które są potrzebne w zasięgu ręki.</span><span class="sxs-lookup"><span data-stu-id="92a11-581">With code snippets, you have all the code you need at your fingertips.</span></span> <span data-ttu-id="92a11-582">Dokument laboratorium informuje o dokładnie po ich użycia, jak pokazano na poniższej ilustracji.</span><span class="sxs-lookup"><span data-stu-id="92a11-582">The lab document will tell you exactly when you can use them, as shown in the following figure.</span></span>

<span data-ttu-id="92a11-583">![Aby wstawić kod do projektu przy użyciu wstawki kodu programu Visual Studio](aspnet-mvc-4-helpers-forms-and-validation/_static/image27.png "wstawki kodu za pomocą programu Visual Studio, aby wstawić kod do projektu")</span><span class="sxs-lookup"><span data-stu-id="92a11-583">![Using Visual Studio code snippets to insert code into your project](aspnet-mvc-4-helpers-forms-and-validation/_static/image27.png "Using Visual Studio code snippets to insert code into your project")</span></span>

<span data-ttu-id="92a11-584">*Aby wstawić kod do projektu przy użyciu wstawki kodu programu Visual Studio*</span><span class="sxs-lookup"><span data-stu-id="92a11-584">*Using Visual Studio code snippets to insert code into your project*</span></span>

<span data-ttu-id="92a11-585">***Aby dodać fragment kodu za pomocą klawiatury (C# tylko)***</span><span class="sxs-lookup"><span data-stu-id="92a11-585">***To add a code snippet using the keyboard (C# only)***</span></span>

1. <span data-ttu-id="92a11-586">Umieść kursor, w którym chcesz wstawić kod.</span><span class="sxs-lookup"><span data-stu-id="92a11-586">Place the cursor where you would like to insert the code.</span></span>
2. <span data-ttu-id="92a11-587">Zacznij wpisywać nazwę fragment (bez spacji i łączniki).</span><span class="sxs-lookup"><span data-stu-id="92a11-587">Start typing the snippet name (without spaces or hyphens).</span></span>
3. <span data-ttu-id="92a11-588">Obejrzyj jako IntelliSense wyświetla zgodne z nazwami wstawki.</span><span class="sxs-lookup"><span data-stu-id="92a11-588">Watch as IntelliSense displays matching snippets' names.</span></span>
4. <span data-ttu-id="92a11-589">Wybierz prawidłowe fragment (lub zachować wpisywanie do momentu wybrania fragment całą nazwę).</span><span class="sxs-lookup"><span data-stu-id="92a11-589">Select the correct snippet (or keep typing until the entire snippet's name is selected).</span></span>
5. <span data-ttu-id="92a11-590">Naciśnij klawisz Tab dwa razy, aby wstawić fragment kodu w lokalizacji kursora.</span><span class="sxs-lookup"><span data-stu-id="92a11-590">Press the Tab key twice to insert the snippet at the cursor location.</span></span>

<span data-ttu-id="92a11-591">![Rozpocznij wpisywanie nazwy fragment](aspnet-mvc-4-helpers-forms-and-validation/_static/image28.png "zacznij wpisywać nazwę wstawki programu")</span><span class="sxs-lookup"><span data-stu-id="92a11-591">![Start typing the snippet name](aspnet-mvc-4-helpers-forms-and-validation/_static/image28.png "Start typing the snippet name")</span></span>

<span data-ttu-id="92a11-592">*Rozpocznij wpisywanie nazwy fragment kodu*</span><span class="sxs-lookup"><span data-stu-id="92a11-592">*Start typing the snippet name*</span></span>

<span data-ttu-id="92a11-593">![Naciśnij klawisz Tab, aby wybrać wyróżnione fragment](aspnet-mvc-4-helpers-forms-and-validation/_static/image29.png "naciśnij klawisz Tab, aby wybrać wyróżnione fragment kodu")</span><span class="sxs-lookup"><span data-stu-id="92a11-593">![Press Tab to select the highlighted snippet](aspnet-mvc-4-helpers-forms-and-validation/_static/image29.png "Press Tab to select the highlighted snippet")</span></span>

<span data-ttu-id="92a11-594">*Naciśnij klawisz Tab, aby wybrać wyróżnione fragment kodu*</span><span class="sxs-lookup"><span data-stu-id="92a11-594">*Press Tab to select the highlighted snippet*</span></span>

<span data-ttu-id="92a11-595">![Ponownie naciśnij klawisz Tab i fragment rozwinie](aspnet-mvc-4-helpers-forms-and-validation/_static/image30.png "rozwinie ponownie naciśnij klawisz Tab i wstawki kodu")</span><span class="sxs-lookup"><span data-stu-id="92a11-595">![Press Tab again and the snippet will expand](aspnet-mvc-4-helpers-forms-and-validation/_static/image30.png "Press Tab again and the snippet will expand")</span></span>

<span data-ttu-id="92a11-596">*Rozwinie ponownie naciśnij klawisz Tab i wstawki kodu*</span><span class="sxs-lookup"><span data-stu-id="92a11-596">*Press Tab again and the snippet will expand*</span></span>

<span data-ttu-id="92a11-597">***Aby dodać fragment kodu za pomocą myszy (C#, Visual Basic i XML)*** 1.</span><span class="sxs-lookup"><span data-stu-id="92a11-597">***To add a code snippet using the mouse (C#, Visual Basic and XML)*** 1.</span></span> <span data-ttu-id="92a11-598">Kliknij prawym przyciskiem myszy, gdy chcesz wstawić fragment kodu.</span><span class="sxs-lookup"><span data-stu-id="92a11-598">Right-click where you want to insert the code snippet.</span></span>

1. <span data-ttu-id="92a11-599">Wybierz **wstawić fragment** następuje **Moje wstawki kodu**.</span><span class="sxs-lookup"><span data-stu-id="92a11-599">Select **Insert Snippet** followed by **My Code Snippets**.</span></span>
2. <span data-ttu-id="92a11-600">Wybierz odpowiedni fragment z listy, klikając go.</span><span class="sxs-lookup"><span data-stu-id="92a11-600">Pick the relevant snippet from the list, by clicking on it.</span></span>

<span data-ttu-id="92a11-601">![Kliknij prawym przyciskiem myszy, gdy chcesz wstawić fragment kodu i wybierz wstawić fragment](aspnet-mvc-4-helpers-forms-and-validation/_static/image31.png "kliknij prawym przyciskiem myszy, gdy chcesz wstawić fragment kodu i wybierz wstawić fragment kodu")</span><span class="sxs-lookup"><span data-stu-id="92a11-601">![Right-click where you want to insert the code snippet and select Insert Snippet](aspnet-mvc-4-helpers-forms-and-validation/_static/image31.png "Right-click where you want to insert the code snippet and select Insert Snippet")</span></span>

<span data-ttu-id="92a11-602">*Kliknij prawym przyciskiem myszy, gdy chcesz wstawić fragment kodu i wybierz wstawić fragment kodu*</span><span class="sxs-lookup"><span data-stu-id="92a11-602">*Right-click where you want to insert the code snippet and select Insert Snippet*</span></span>

<span data-ttu-id="92a11-603">![Wybierz odpowiedni fragment z listy, klikając go](aspnet-mvc-4-helpers-forms-and-validation/_static/image32.png "wybierz odpowiedni fragment z listy, klikając go")</span><span class="sxs-lookup"><span data-stu-id="92a11-603">![Pick the relevant snippet from the list, by clicking on it](aspnet-mvc-4-helpers-forms-and-validation/_static/image32.png "Pick the relevant snippet from the list, by clicking on it")</span></span>

<span data-ttu-id="92a11-604">*Wybierz odpowiedni fragment z listy, klikając go*</span><span class="sxs-lookup"><span data-stu-id="92a11-604">*Pick the relevant snippet from the list, by clicking on it*</span></span>
