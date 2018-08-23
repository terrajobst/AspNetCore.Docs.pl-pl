---
uid: mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-helpers-forms-and-validation
title: ASP.NET MVC 4 — pomocnicy, formularze i Walidacja | Dokumentacja firmy Microsoft
author: rick-anderson
description: ASP.NET MVC 4 modeli i danych programu Access warsztatów możesz zostały ładowanie i wyświetlanie danych z bazy danych. W tym laboratorium praktyczne zostanie dodany do...
ms.author: riande
ms.date: 02/18/2013
ms.assetid: 187ee9cd-bc70-479b-bfed-f568b8da96eb
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-helpers-forms-and-validation
msc.type: authoredcontent
ms.openlocfilehash: 8671ae8e9408e6f05135fa27d56480477521c4ba
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/16/2018
ms.locfileid: "41757351"
---
# <a name="aspnet-mvc-4-helpers-forms-and-validation"></a><span data-ttu-id="82686-104">ASP.NET MVC 4 — pomocnicy, formularze i Walidacja</span><span class="sxs-lookup"><span data-stu-id="82686-104">ASP.NET MVC 4 Helpers, Forms and Validation</span></span>

<span data-ttu-id="82686-105">Przez [Camp w sieci Web zespołu](https://twitter.com/webcamps)</span><span class="sxs-lookup"><span data-stu-id="82686-105">By [Web Camps Team](https://twitter.com/webcamps)</span></span>

[<span data-ttu-id="82686-106">Pobierz Camp Web szkolenia Kit</span><span class="sxs-lookup"><span data-stu-id="82686-106">Download Web Camps Training Kit</span></span>](https://aka.ms/webcamps-training-kit)

<span data-ttu-id="82686-107">W **platformy ASP.NET MVC 4 modele i dostęp do danych** warsztatów, nastąpiło ładowanie i wyświetlanie danych z bazy danych.</span><span class="sxs-lookup"><span data-stu-id="82686-107">In **ASP.NET MVC 4 Models and Data Access** Hands-on Lab, you have been loading and displaying data from the database.</span></span> <span data-ttu-id="82686-108">W tym laboratorium praktyczne zostanie dodany do **Music Store** aplikacji możliwość edytowania tych danych.</span><span class="sxs-lookup"><span data-stu-id="82686-108">In this Hands-on Lab, you will add to the **Music Store** application the ability to edit that data.</span></span>

<span data-ttu-id="82686-109">Z tego celu należy pamiętać, najpierw utworzysz kontroler, który będzie obsługiwać operacje tworzenia, odczytu, aktualizowania lub usuwania (CRUD) albumów.</span><span class="sxs-lookup"><span data-stu-id="82686-109">With that goal in mind, you will first create the controller that will support the Create, Read, Update and Delete (CRUD) actions of albums.</span></span> <span data-ttu-id="82686-110">Spowoduje wygenerowanie szablonu widoku indeksu, korzystając z zalet funkcji tworzenia szkieletu ASP.NET MVC do wyświetlania właściwości albumów w tabeli HTML.</span><span class="sxs-lookup"><span data-stu-id="82686-110">You will generate an Index View template taking advantage of ASP.NET MVC's scaffolding feature to display the albums' properties in an HTML table.</span></span> <span data-ttu-id="82686-111">Aby zwiększyć możliwości tego widoku, należy dodać niestandardowego pomocnika HTML, który obetnie długie opisy.</span><span class="sxs-lookup"><span data-stu-id="82686-111">To enhance that view, you will add a custom HTML helper that will truncate long descriptions.</span></span>

<span data-ttu-id="82686-112">Później dodasz, czy edytowanie i tworzenie widoków, które pozwoli zmienić albumów w bazie danych za pomocą formularza elementów, takich jak listy rozwijane.</span><span class="sxs-lookup"><span data-stu-id="82686-112">Afterwards, you will add the Edit and Create Views that will let you alter the albums in the database, with the help of form elements like dropdowns.</span></span>

<span data-ttu-id="82686-113">Ponadto umożliwi użytkownikom usuwanie albumu i również możesz uniemożliwi im wprowadzanie błędnych danych, sprawdzając poprawność danych wejściowych.</span><span class="sxs-lookup"><span data-stu-id="82686-113">Lastly, you will let users delete an album and also you will prevent them from entering wrong data by validating their input.</span></span>

<span data-ttu-id="82686-114">W tym laboratorium praktyczne przyjęto założenie, masz podstawową wiedzę na temat **platformy ASP.NET MVC**.</span><span class="sxs-lookup"><span data-stu-id="82686-114">This Hands-on Lab assumes you have basic knowledge of **ASP.NET MVC**.</span></span> <span data-ttu-id="82686-115">Jeśli nie używasz **platformy ASP.NET MVC** , zalecamy zapoznać się z **podstawowe informacje dotyczące platformy ASP.NET MVC** warsztatów.</span><span class="sxs-lookup"><span data-stu-id="82686-115">If you have not used **ASP.NET MVC** before, we recommend you to go over **ASP.NET MVC Fundamentals** Hands-on Lab.</span></span>

<span data-ttu-id="82686-116">W tym laboratorium przeprowadzi Cię przez ulepszeń i nowych funkcji, które opisano wcześniej, stosując drobne zmiany do przykładowej aplikacji sieci Web, pod warunkiem w folderze źródłowym.</span><span class="sxs-lookup"><span data-stu-id="82686-116">This lab walks you through the enhancements and new features previously described by applying minor changes to a sample Web application provided in the Source folder.</span></span>

> [!NOTE]
> <span data-ttu-id="82686-117">Wszystkie przykładowy kod i fragmenty kodu są uwzględnione w sieci Web Camp zestaw szkoleniowy, dostępne pod adresem [wersji Microsoft — w sieci Web/WebCampTrainingKit](https://aka.ms/webcamps-training-kit).</span><span class="sxs-lookup"><span data-stu-id="82686-117">All sample code and snippets are included in the Web Camps Training Kit, available at [Microsoft-Web/WebCampTrainingKit Releases](https://aka.ms/webcamps-training-kit).</span></span> <span data-ttu-id="82686-118">Projekt specyficzne dla tego laboratorium znajduje się w temacie [platformy ASP.NET MVC 4 pomocnicy, formularze i Walidacja](https://github.com/Microsoft-Web/HOL-MVC4HelpersFormsAndValidation).</span><span class="sxs-lookup"><span data-stu-id="82686-118">The project specific to this lab is available at [ASP.NET MVC 4 Helpers, Forms and Validation](https://github.com/Microsoft-Web/HOL-MVC4HelpersFormsAndValidation).</span></span>

<a id="Objectives"></a>
### <a name="objectives"></a><span data-ttu-id="82686-119">Cele</span><span class="sxs-lookup"><span data-stu-id="82686-119">Objectives</span></span>

<span data-ttu-id="82686-120">W tym laboratorium praktyczne dowiesz się jak:</span><span class="sxs-lookup"><span data-stu-id="82686-120">In this Hands-On Lab, you will learn how to:</span></span>

- <span data-ttu-id="82686-121">Tworzenie kontrolera w celu obsługi operacji CRUD</span><span class="sxs-lookup"><span data-stu-id="82686-121">Create a controller to support CRUD operations</span></span>
- <span data-ttu-id="82686-122">Generowanie widoku indeksu, aby wyświetlić właściwości jednostki w tabeli HTML</span><span class="sxs-lookup"><span data-stu-id="82686-122">Generate an Index View to display entity properties in an HTML table</span></span>
- <span data-ttu-id="82686-123">Dodawanie niestandardowego pomocnika HTML</span><span class="sxs-lookup"><span data-stu-id="82686-123">Add a custom HTML helper</span></span>
- <span data-ttu-id="82686-124">Tworzenie i dostosowywanie widoku Edycja</span><span class="sxs-lookup"><span data-stu-id="82686-124">Create and customize an Edit View</span></span>
- <span data-ttu-id="82686-125">Rozróżnienie metod akcji, które reagują na HTTP GET lub POST protokołu HTTP wywołania</span><span class="sxs-lookup"><span data-stu-id="82686-125">Differentiate between action methods that react to either HTTP-GET or HTTP-POST calls</span></span>
- <span data-ttu-id="82686-126">Dodawanie i dostosowywanie Create View</span><span class="sxs-lookup"><span data-stu-id="82686-126">Add and customize a Create View</span></span>
- <span data-ttu-id="82686-127">Dojście do usuwania jednostki</span><span class="sxs-lookup"><span data-stu-id="82686-127">Handle the deletion of an entity</span></span>
- <span data-ttu-id="82686-128">Weryfikowanie danych wejściowych użytkownika</span><span class="sxs-lookup"><span data-stu-id="82686-128">Validate user input</span></span>

<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a><span data-ttu-id="82686-129">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="82686-129">Prerequisites</span></span>

<span data-ttu-id="82686-130">Należy dysponować następującymi elementami do przygotowania tego laboratorium:</span><span class="sxs-lookup"><span data-stu-id="82686-130">You must have the following items to complete this lab:</span></span>

- <span data-ttu-id="82686-131">[Microsoft Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) lub wyższego poziomu (odczyt [dodatek a.](#AppendixA) instrukcje dotyczące sposobu jego instalacji).</span><span class="sxs-lookup"><span data-stu-id="82686-131">[Microsoft Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) or superior (read [Appendix A](#AppendixA) for instructions on how to install it).</span></span>

<a id="Setup"></a>

<a id="Setup"></a>
### <a name="setup"></a><span data-ttu-id="82686-132">Konfiguracja</span><span class="sxs-lookup"><span data-stu-id="82686-132">Setup</span></span>

<span data-ttu-id="82686-133">**Instalowanie fragmentów kodu**</span><span class="sxs-lookup"><span data-stu-id="82686-133">**Installing Code Snippets**</span></span>

<span data-ttu-id="82686-134">Dla wygody większość kodu, który będzie zarządzany wzdłuż tego laboratorium jest dostępna jako fragmenty kodu programu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="82686-134">For convenience, much of the code you will be managing along this lab is available as Visual Studio code snippets.</span></span> <span data-ttu-id="82686-135">Aby zainstalować fragmenty kodu, uruchom **.\Source\Setup\CodeSnippets.vsi** pliku.</span><span class="sxs-lookup"><span data-stu-id="82686-135">To install the code snippets run **.\Source\Setup\CodeSnippets.vsi** file.</span></span>

<span data-ttu-id="82686-136">Jeśli nie jesteś zaznajomiony z fragmentów kodu w usłudze Visual Studio i chcesz dowiedzieć się, jak z nich korzystać, możesz zapoznać się z dodatku z tego dokumentu &quot; [dodatek B: za pomocą wstawek kodu programu](#AppendixB)&quot;.</span><span class="sxs-lookup"><span data-stu-id="82686-136">If you are not familiar with the Visual Studio Code Snippets, and want to learn how to use them, you can refer to the appendix from this document &quot;[Appendix B: Using Code Snippets](#AppendixB)&quot;.</span></span>

* * *

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a><span data-ttu-id="82686-137">Ćwiczenia</span><span class="sxs-lookup"><span data-stu-id="82686-137">Exercises</span></span>

<span data-ttu-id="82686-138">Następujące ćwiczeń składają się tym warsztatów:</span><span class="sxs-lookup"><span data-stu-id="82686-138">The following exercises make up this Hands-On Lab:</span></span>

1. [<span data-ttu-id="82686-139">Tworzenie kontrolera Store Manager i jej widok indeksu</span><span class="sxs-lookup"><span data-stu-id="82686-139">Creating the Store Manager controller and its Index view</span></span>](#Exercise1)
2. [<span data-ttu-id="82686-140">Dodawanie pomocnika kodu HTML</span><span class="sxs-lookup"><span data-stu-id="82686-140">Adding an HTML Helper</span></span>](#Exercise2)
3. [<span data-ttu-id="82686-141">Tworzenie widoku edycji</span><span class="sxs-lookup"><span data-stu-id="82686-141">Creating the Edit View</span></span>](#Exercise3)
4. [<span data-ttu-id="82686-142">Dodawanie widoku Create</span><span class="sxs-lookup"><span data-stu-id="82686-142">Adding a Create View</span></span>](#Exercise4)
5. [<span data-ttu-id="82686-143">Usuwanie obsługi</span><span class="sxs-lookup"><span data-stu-id="82686-143">Handling Deletion</span></span>](#Exercise5)
6. [<span data-ttu-id="82686-144">Dodawanie weryfikacji</span><span class="sxs-lookup"><span data-stu-id="82686-144">Adding Validation</span></span>](#Exercise6)
7. [<span data-ttu-id="82686-145">Przy użyciu jQuery dyskretny kod po stronie klienta</span><span class="sxs-lookup"><span data-stu-id="82686-145">Using Unobtrusive jQuery at Client Side</span></span>](#Exercise7)

> [!NOTE]
> <span data-ttu-id="82686-146">Towarzyszy każdego wykonywania **zakończenia** folderu zawierającego wynikowy rozwiązania, należy uzyskać po ukończeniu ćwiczenia.</span><span class="sxs-lookup"><span data-stu-id="82686-146">Each exercise is accompanied by an **End** folder containing the resulting solution you should obtain after completing the exercises.</span></span> <span data-ttu-id="82686-147">Jeśli potrzebujesz dodatkowej pomocy ćwiczeń opisanych w dalszej, można użyć tego rozwiązania jako wskazówki.</span><span class="sxs-lookup"><span data-stu-id="82686-147">You can use this solution as a guide if you need additional help working through the exercises.</span></span>


<span data-ttu-id="82686-148">Szacowany czas do ukończenia tego laboratorium: **60 minut**</span><span class="sxs-lookup"><span data-stu-id="82686-148">Estimated time to complete this lab: **60 minutes**</span></span>

<a id="Exercise1"></a>

<a id="Exercise_1_Creating_the_Store_Manager_controller_and_its_Index_view"></a>
### <a name="exercise-1-creating-the-store-manager-controller-and-its-index-view"></a><span data-ttu-id="82686-149">Ćwiczenie 1: Tworzenie kontrolera Store Manager i jej widok indeksu</span><span class="sxs-lookup"><span data-stu-id="82686-149">Exercise 1: Creating the Store Manager controller and its Index view</span></span>

<span data-ttu-id="82686-150">W tym ćwiczeniu zostanie dowiesz się, jak utworzyć nowy kontroler do obsługi operacji CRUD, dostosowywanie jej indeks metody akcji, aby zwrócić listę albumów z bazy danych i na koniec wygenerowanie szablonu widoku indeksu, korzystając z zalet tworzenia szkieletu ASP.NET MVC Funkcja, aby wyświetlić właściwości albumów w tabeli HTML.</span><span class="sxs-lookup"><span data-stu-id="82686-150">In this exercise, you will learn how to create a new controller to support CRUD operations, customize its Index action method to return a list of albums from the database and finally generating an Index View template taking advantage of ASP.NET MVC's scaffolding feature to display the albums' properties in an HTML table.</span></span>

<a id="Ex1Task1"></a>

<a id="Task_1_-_Creating_the_StoreManagerController"></a>
#### <a name="task-1---creating-the-storemanagercontroller"></a><span data-ttu-id="82686-151">Zadanie 1. Tworzenie StoreManagerController</span><span class="sxs-lookup"><span data-stu-id="82686-151">Task 1 - Creating the StoreManagerController</span></span>

<span data-ttu-id="82686-152">W tym zadaniu utworzysz nowy kontroler o nazwie **StoreManagerController** do obsługi operacji CRUD.</span><span class="sxs-lookup"><span data-stu-id="82686-152">In this task, you will create a new controller called **StoreManagerController** to support CRUD operations.</span></span>

1. <span data-ttu-id="82686-153">Otwórz **rozpocząć** rozwiązania znajdujący się w **źródło/Ex1-CreatingTheStoreManagerController/rozpoczęcia/** folderu.</span><span class="sxs-lookup"><span data-stu-id="82686-153">Open the **Begin** solution located at **Source/Ex1-CreatingTheStoreManagerController/Begin/** folder.</span></span>

   1. <span data-ttu-id="82686-154">Musisz pobrać niektóre brakujące pakiety NuGet przed kontynuowaniem.</span><span class="sxs-lookup"><span data-stu-id="82686-154">You will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="82686-155">Aby to zrobić, kliknij przycisk **projektu** menu, a następnie wybierz **Zarządzaj pakietami NuGet**.</span><span class="sxs-lookup"><span data-stu-id="82686-155">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="82686-156">W **Zarządzaj pakietami NuGet** okno dialogowe, kliknij przycisk **przywrócić** Aby pobrać brakujące pakiety.</span><span class="sxs-lookup"><span data-stu-id="82686-156">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="82686-157">Na koniec Skompiluj rozwiązanie, klikając **kompilacji** | **Kompiluj rozwiązanie**.</span><span class="sxs-lookup"><span data-stu-id="82686-157">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="82686-158">Jedną z zalet za pomocą narzędzia NuGet jest, że nie masz do wysłania wszystkie biblioteki w projekcie, zmniejszenie rozmiaru projektu.</span><span class="sxs-lookup"><span data-stu-id="82686-158">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="82686-159">Za pomocą narzędzi NuGet Power Tools, określając wersji pakietu w pliku Packages.config można pobrać wymaganych bibliotek podczas pierwszego uruchomienia projektu.</span><span class="sxs-lookup"><span data-stu-id="82686-159">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="82686-160">Jest to, dlaczego należy uruchomić następujące kroki, po otwarciu istniejącego rozwiązania, w tym środowisku laboratoryjnym.</span><span class="sxs-lookup"><span data-stu-id="82686-160">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="82686-161">Dodaj nowy kontroler.</span><span class="sxs-lookup"><span data-stu-id="82686-161">Add a new controller.</span></span> <span data-ttu-id="82686-162">Aby to zrobić, kliknij prawym przyciskiem myszy **kontrolerów** folder w Eksploratorze rozwiązań, wybierz pozycję **Dodaj** i następnie **kontrolera** polecenia.</span><span class="sxs-lookup"><span data-stu-id="82686-162">To do this, right-click the **Controllers** folder within the Solution Explorer, select **Add** and then the **Controller** command.</span></span> <span data-ttu-id="82686-163">Zmiana **kontrolera** **nazwa** do **StoreManagerController** i upewnić się, że opcja **kontroler MVC z akcjami odczytu/zapisu pusty**jest zaznaczone.</span><span class="sxs-lookup"><span data-stu-id="82686-163">Change the **Controller** **Name** to **StoreManagerController** and make sure the option **MVC controller with empty read/write actions** is selected.</span></span> <span data-ttu-id="82686-164">Kliknij przycisk **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="82686-164">Click **Add**.</span></span>

    <span data-ttu-id="82686-165">![Okno dialogowe Dodawanie kontrolera](aspnet-mvc-4-helpers-forms-and-validation/_static/image1.png "okno dialogowe Dodawanie kontrolera")</span><span class="sxs-lookup"><span data-stu-id="82686-165">![Add controller dialog](aspnet-mvc-4-helpers-forms-and-validation/_static/image1.png "Add controller dialog")</span></span>

    <span data-ttu-id="82686-166">*Dodaj kontroler, okno dialogowe*</span><span class="sxs-lookup"><span data-stu-id="82686-166">*Add Controller Dialog*</span></span>

    <span data-ttu-id="82686-167">Nowa klasa kontrolera jest generowany.</span><span class="sxs-lookup"><span data-stu-id="82686-167">A new Controller class is generated.</span></span> <span data-ttu-id="82686-168">Ponieważ przekazały Dodawanie akcji dla odczytu/zapisu, metody klasy zastępczej dla osób, typowych operacji CRUD są tworzone przy użyciu komentarze TODO wypełnione, monitowania obejmujący logiki określonych aplikacji.</span><span class="sxs-lookup"><span data-stu-id="82686-168">Since you indicated to add actions for read/write, stub methods for those, common CRUD actions are created with TODO comments filled in, prompting to include the application specific logic.</span></span>

<a id="Ex1Task2"></a>

<a id="Task_2_-_Customizing_the_StoreManager_Index"></a>
#### <a name="task-2---customizing-the-storemanager-index"></a><span data-ttu-id="82686-169">Zadanie 2 — Dostosowywanie indeksu StoreManager</span><span class="sxs-lookup"><span data-stu-id="82686-169">Task 2 - Customizing the StoreManager Index</span></span>

<span data-ttu-id="82686-170">To zadanie umożliwia dostosowywanie indeksu StoreManager metoda akcji do zwrócenia widoku z listą albumów z bazy danych.</span><span class="sxs-lookup"><span data-stu-id="82686-170">In this task, you will customize the StoreManager Index action method to return a View with the list of albums from the database.</span></span>

1. <span data-ttu-id="82686-171">W klasie StoreManagerController, Dodaj następujący kod *przy użyciu* dyrektywy.</span><span class="sxs-lookup"><span data-stu-id="82686-171">In the StoreManagerController class, add the following *using* directives.</span></span>

    <span data-ttu-id="82686-172">(Code Snippet — *pomocników programu ASP.NET MVC 4 i formularze i Walidacja - Ex1 przy użyciu MvcMusicStore*)</span><span class="sxs-lookup"><span data-stu-id="82686-172">(Code Snippet - *ASP.NET MVC 4 Helpers and Forms and Validation - Ex1 using MvcMusicStore*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample1.cs)]
2. <span data-ttu-id="82686-173">Dodaj pole do **StoreManagerController** zawierającą wystąpienia **MusicStoreEntities.**</span><span class="sxs-lookup"><span data-stu-id="82686-173">Add a field to the **StoreManagerController** to hold an instance of **MusicStoreEntities.**</span></span>

    <span data-ttu-id="82686-174">(Code Snippet — *pomocników platformy ASP.NET MVC 4 i formularze i Walidacja - Ex1 MusicStoreEntities*)</span><span class="sxs-lookup"><span data-stu-id="82686-174">(Code Snippet - *ASP.NET MVC 4 Helpers and Forms and Validation - Ex1 MusicStoreEntities*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample2.cs)]
3. <span data-ttu-id="82686-175">Implementowanie akcji indeksu StoreManagerController do zwrócenia widoku z listy albumów.</span><span class="sxs-lookup"><span data-stu-id="82686-175">Implement the StoreManagerController Index action to return a View with the list of albums.</span></span>

    <span data-ttu-id="82686-176">Logika akcji kontrolera jest bardzo podobny do StoreController indeksu akcji w przypadku napisanych wcześniej.</span><span class="sxs-lookup"><span data-stu-id="82686-176">The Controller action logic will be very similar to the StoreController's Index action written earlier.</span></span> <span data-ttu-id="82686-177">Użyj programu LINQ do pobrania wszystkich albumów, w tym wykonawcy i gatunku informacje do wyświetlenia.</span><span class="sxs-lookup"><span data-stu-id="82686-177">Use LINQ to retrieve all albums, including Genre and Artist information for display.</span></span>

    <span data-ttu-id="82686-178">(Code Snippet — *pomocników platformy ASP.NET MVC 4 i formularze i Walidacja — indeks StoreManagerController Ex1*)</span><span class="sxs-lookup"><span data-stu-id="82686-178">(Code Snippet - *ASP.NET MVC 4 Helpers and Forms and Validation - Ex1 StoreManagerController Index*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample3.cs)]

<a id="Ex1Task3"></a>

<a id="strongTask_3_-_Creating_the_Index_Viewstrong"></a>
#### <a name="task-3---creating-the-index-view"></a><span data-ttu-id="82686-179">Zadanie 3. Tworzenie widoku indeksu</span><span class="sxs-lookup"><span data-stu-id="82686-179">Task 3 - Creating the Index View</span></span>

<span data-ttu-id="82686-180">W tym zadaniu zostanie utworzony szablon widoku indeksu, aby wyświetlić listę albumów zwrócony przez **StoreManager** kontrolera.</span><span class="sxs-lookup"><span data-stu-id="82686-180">In this task, you will create the Index View template to display the list of albums returned by the **StoreManager** Controller.</span></span>

1. <span data-ttu-id="82686-181">Przed utworzeniem nowego szablonu widoku, należy skompilować projekt, aby **dialogowe dodawania widoku** obsługującemu **albumu** klasy.</span><span class="sxs-lookup"><span data-stu-id="82686-181">Before creating the new View template, you should build the project so that the **Add View Dialog** knows about the **Album** class to use.</span></span> <span data-ttu-id="82686-182">Wybierz **kompilacji | Tworzenie MvcMusicStore** do skompilowania projektu.</span><span class="sxs-lookup"><span data-stu-id="82686-182">Select **Build | Build MvcMusicStore** to build the project.</span></span>
2. <span data-ttu-id="82686-183">Kliknij prawym przyciskiem myszy wewnątrz **indeksu** metody akcji i wybierz **Dodaj widok**.</span><span class="sxs-lookup"><span data-stu-id="82686-183">Right-click inside the **Index** action method and select **Add View**.</span></span> <span data-ttu-id="82686-184">Zostanie wyświetlone okno **Dodaj widok** okna dialogowego.</span><span class="sxs-lookup"><span data-stu-id="82686-184">This will bring up the **Add View** dialog.</span></span>

    <span data-ttu-id="82686-185">![Dodaj widok](aspnet-mvc-4-helpers-forms-and-validation/_static/image2.png "Dodawanie widoku")</span><span class="sxs-lookup"><span data-stu-id="82686-185">![Add view](aspnet-mvc-4-helpers-forms-and-validation/_static/image2.png "Add view")</span></span>

    <span data-ttu-id="82686-186">*Dodawanie widoku z wewnątrz metody indeksu*</span><span class="sxs-lookup"><span data-stu-id="82686-186">*Adding a View from within the Index method*</span></span>
3. <span data-ttu-id="82686-187">W oknie dialogowym Dodaj widok, sprawdź, czy nazwa widoku jest **indeksu**.</span><span class="sxs-lookup"><span data-stu-id="82686-187">In the Add View dialog, verify that the View Name is **Index**.</span></span> <span data-ttu-id="82686-188">Wybierz **utworzyć widok silnie typizowane** opcji, a następnie wybierz **albumu (MvcMusicStore.Models)** z **klasa modelu** listy rozwijanej.</span><span class="sxs-lookup"><span data-stu-id="82686-188">Select the **Create a strongly-typed view** option, and select **Album (MvcMusicStore.Models)** from the **Model class** drop-down.</span></span> <span data-ttu-id="82686-189">Wybierz **listy** z **szablonu szkieletu** listy rozwijanej.</span><span class="sxs-lookup"><span data-stu-id="82686-189">Select **List** from the **Scaffold template** drop-down.</span></span> <span data-ttu-id="82686-190">Pozostaw **aparat widoku** do **Razor** i inne pola z ich domyślną wartość, a następnie kliknij przycisk **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="82686-190">Leave the **View engine** to **Razor** and the other fields with their default value and then click **Add**.</span></span>

    <span data-ttu-id="82686-191">![Dodawanie widoku indeksu](aspnet-mvc-4-helpers-forms-and-validation/_static/image3.png "Dodawanie widoku indeksu")</span><span class="sxs-lookup"><span data-stu-id="82686-191">![Adding an index view](aspnet-mvc-4-helpers-forms-and-validation/_static/image3.png "Adding an index view")</span></span>

    <span data-ttu-id="82686-192">*Dodawanie widoku indeksu*</span><span class="sxs-lookup"><span data-stu-id="82686-192">*Adding an Index View*</span></span>

<a id="Ex1Task4"></a>

<a id="Task_4_-_Customizing_the_scaffold_of_the_Index_View"></a>
#### <a name="task-4---customizing-the-scaffold-of-the-index-view"></a><span data-ttu-id="82686-193">Zadanie 4 — Dostosowywanie szkieletu widoku indeksu</span><span class="sxs-lookup"><span data-stu-id="82686-193">Task 4 - Customizing the scaffold of the Index View</span></span>

<span data-ttu-id="82686-194">W tym zadaniu zostanie dostosowana prostego szablonu Widok utworzony za pomocą funkcji tworzenia szkieletu ASP.NET MVC do jego wyświetlania pól, które chcesz.</span><span class="sxs-lookup"><span data-stu-id="82686-194">In this task, you will adjust the simple View template created with ASP.NET MVC scaffolding feature to have it display the fields you want.</span></span>

> [!NOTE]
> <span data-ttu-id="82686-195">**Tworzenia szkieletów** wsparcie w ramach platformy ASP.NET MVC wygeneruje prostego szablonu widoku, który wyświetla listę wszystkich pól w modelu albumu.</span><span class="sxs-lookup"><span data-stu-id="82686-195">The **scaffolding** support within ASP.NET MVC generates a simple View template which lists all fields in the Album model.</span></span> <span data-ttu-id="82686-196">**Tworzenia szkieletów** umożliwia szybkie rozpoczęcie pracy nad silnie typizowanego widoku: zamiast ręcznie napisać szablon widoku, tworzenia szkieletów szybko generuje domyślny szablon, a następnie można zmodyfikować wygenerowany kod.</span><span class="sxs-lookup"><span data-stu-id="82686-196">**Scaffolding** provides a quick way to get started on a strongly typed view: rather than having to write the View template manually, scaffolding quickly generates a default template and then you can modify the generated code.</span></span>


1. <span data-ttu-id="82686-197">Przejrzyj kod utworzony.</span><span class="sxs-lookup"><span data-stu-id="82686-197">Review the code created.</span></span> <span data-ttu-id="82686-198">Wygenerowany Lista pól będzie częścią następujących tabeli HTML, który **tworzenia szkieletów** używanej do wyświetlania danych tabelarycznych.</span><span class="sxs-lookup"><span data-stu-id="82686-198">The generated list of fields will be part of the following HTML table that **Scaffolding** is using for displaying tabular data.</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample4.cshtml)]
2. <span data-ttu-id="82686-199">Zastąp **&lt;tabeli&gt;** kodu za pomocą następującego kodu, aby wyświetlić tylko **gatunku**, **wykonawcy**, **tytuł**, i **cena** pola.</span><span class="sxs-lookup"><span data-stu-id="82686-199">Replace the **&lt;table&gt;** code with the following code to display only the **Genre**, **Artist**, **Album Title**, and **Price** fields.</span></span> <span data-ttu-id="82686-200">Spowoduje to usunięcie **AlbumId** i **URL grafikę albumu** kolumn.</span><span class="sxs-lookup"><span data-stu-id="82686-200">This deletes the **AlbumId** and **Album Art URL** columns.</span></span> <span data-ttu-id="82686-201">Ponadto zmienia GenreId i ArtistId kolumny do wyświetlenia ich właściwości klasy połączone **Artist.Name** i **Genre.Name**i usuwa **szczegóły** łącza.</span><span class="sxs-lookup"><span data-stu-id="82686-201">Also, it changes GenreId and ArtistId columns to display their linked class properties of **Artist.Name** and **Genre.Name**, and removes the **Details** link.</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample5.cshtml)]
3. <span data-ttu-id="82686-202">Zmień poniższe opisy.</span><span class="sxs-lookup"><span data-stu-id="82686-202">Change the following descriptions.</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample6.cshtml)]

<a id="Ex1Task5"></a>

<a id="Task_5_-_Running_the_Application"></a>
#### <a name="task-5---running-the-application"></a><span data-ttu-id="82686-203">Zadanie 5 działania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="82686-203">Task 5 - Running the Application</span></span>

<span data-ttu-id="82686-204">W tym zadaniu przetestujesz, **StoreManager** **indeksu** Wyświetl szablon zawiera listę albumów zgodnie z projektem w poprzednich krokach.</span><span class="sxs-lookup"><span data-stu-id="82686-204">In this task, you will test that the **StoreManager** **Index** View template displays a list of albums according to the design of the previous steps.</span></span>

1. <span data-ttu-id="82686-205">Naciśnij klawisz **F5** do uruchomienia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="82686-205">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="82686-206">Data rozpoczęcia projektu na stronie głównej.</span><span class="sxs-lookup"><span data-stu-id="82686-206">The project starts in the Home page.</span></span> <span data-ttu-id="82686-207">Zmień adres URL do **/StoreManager** Aby sprawdzić, czy zostanie wyświetlona lista ze zdjęciami, przedstawiający ich **tytuł**, **wykonawcy** i **gatunku**.</span><span class="sxs-lookup"><span data-stu-id="82686-207">Change the URL to **/StoreManager** to verify that a list of albums is displayed, showing their **Title**, **Artist** and **Genre**.</span></span>

    <span data-ttu-id="82686-208">![Przeglądanie listy albumów](aspnet-mvc-4-helpers-forms-and-validation/_static/image4.png "Przeglądanie listy albumów")</span><span class="sxs-lookup"><span data-stu-id="82686-208">![Browsing the list of albums](aspnet-mvc-4-helpers-forms-and-validation/_static/image4.png "Browsing the list of albums")</span></span>

    <span data-ttu-id="82686-209">*Przeglądanie listy albumów*</span><span class="sxs-lookup"><span data-stu-id="82686-209">*Browsing the list of albums*</span></span>

<a id="Exercise2"></a>

<a id="Exercise_2_Adding_an_HTML_Helper"></a>
### <a name="exercise-2-adding-an-html-helper"></a><span data-ttu-id="82686-210">Ćwiczenie 2: Dodawanie pomocnika kodu HTML</span><span class="sxs-lookup"><span data-stu-id="82686-210">Exercise 2: Adding an HTML Helper</span></span>

<span data-ttu-id="82686-211">Strona indeksu StoreManager ma jeden potencjalny problem: tytułu i nazwy wykonawcy właściwości mogą jednocześnie być wystarczająco długi, by wyrzucanie formatowanie tabeli.</span><span class="sxs-lookup"><span data-stu-id="82686-211">The StoreManager Index page has one potential issue: Title and Artist Name properties can both be long enough to throw off the table formatting.</span></span> <span data-ttu-id="82686-212">W tym ćwiczeniu dowiesz się, jak dodać niestandardowe pomocnika kodu HTML do obcinania ten tekst.</span><span class="sxs-lookup"><span data-stu-id="82686-212">In this exercise you will learn how to add a custom HTML helper to truncate that text.</span></span>

<span data-ttu-id="82686-213">Na poniższej ilustracji widać, jak zmienić format ze względu na długość tekstu użycia o rozmiarze małe przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="82686-213">In the following figure, you can see how the format is modified because of the length of the text when you use a small browser size.</span></span>

<span data-ttu-id="82686-214">![Przeglądanie listy albumy ze nie obcięty tekst](aspnet-mvc-4-helpers-forms-and-validation/_static/image5.png "nie Przeglądanie listy albumów z obcięty tekst")</span><span class="sxs-lookup"><span data-stu-id="82686-214">![Browsing the list of Albums with not truncated text](aspnet-mvc-4-helpers-forms-and-validation/_static/image5.png "Browsing the list of Albums with not truncated text")</span></span>

<span data-ttu-id="82686-215">*Przeglądanie listy albumy ze nie obcięty tekst*</span><span class="sxs-lookup"><span data-stu-id="82686-215">*Browsing the list of Albums with not truncated text*</span></span>

<a id="Ex2Task1"></a>

<a id="Task_1_-_Extending_the_HTML_Helper"></a>
#### <a name="task-1---extending-the-html-helper"></a><span data-ttu-id="82686-216">Zadanie 1 — rozszerzając pomocnika kodu HTML</span><span class="sxs-lookup"><span data-stu-id="82686-216">Task 1 - Extending the HTML Helper</span></span>

<span data-ttu-id="82686-217">W tym zadaniu zostanie Dodaj nową metodę **Truncate** do **HTML** obiektu ujawniane w obrębie widoków MVC platformy ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="82686-217">In this task, you will add a new method **Truncate** to the **HTML** object exposed within ASP.NET MVC Views.</span></span> <span data-ttu-id="82686-218">Aby to zrobić, zostaną zaimplementowane **— metoda rozszerzenia** do wbudowanej **System.Web.Mvc.HtmlHelper** klasy dostarczane przez platformę ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="82686-218">To do this, you will implement an **extension method** to the built-in **System.Web.Mvc.HtmlHelper** class provided by ASP.NET MVC.</span></span>

> [!NOTE]
> <span data-ttu-id="82686-219">Aby dowiedzieć się więcej o **metody rozszerzenia**, można znaleźć w tym artykule w witrynie msdn.</span><span class="sxs-lookup"><span data-stu-id="82686-219">To read more about **Extension Methods**, please visit this msdn article.</span></span> <span data-ttu-id="82686-220">[https://msdn.microsoft.com/library/bb383977.aspx](https://msdn.microsoft.com/library/bb383977.aspx).</span><span class="sxs-lookup"><span data-stu-id="82686-220">[https://msdn.microsoft.com/library/bb383977.aspx](https://msdn.microsoft.com/library/bb383977.aspx).</span></span>


1. <span data-ttu-id="82686-221">Otwórz **rozpocząć** rozwiązania znajdujący się w **źródło/Ex2-AddingAnHTMLHelper/rozpoczęcia/** folderu.</span><span class="sxs-lookup"><span data-stu-id="82686-221">Open the **Begin** solution located at **Source/Ex2-AddingAnHTMLHelper/Begin/** folder.</span></span> <span data-ttu-id="82686-222">W przeciwnym razie będziesz nadal korzystać z **zakończenia** rozwiązania uzyskać, wykonując poprzednim ćwiczeniu.</span><span class="sxs-lookup"><span data-stu-id="82686-222">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

   1. <span data-ttu-id="82686-223">Jeśli został otwarty dołączonym **rozpocząć** rozwiązania, należy pobrać niektóre brakujące pakiety NuGet przed kontynuowaniem.</span><span class="sxs-lookup"><span data-stu-id="82686-223">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="82686-224">Aby to zrobić, kliknij przycisk **projektu** menu, a następnie wybierz **Zarządzaj pakietami NuGet**.</span><span class="sxs-lookup"><span data-stu-id="82686-224">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="82686-225">W **Zarządzaj pakietami NuGet** okno dialogowe, kliknij przycisk **przywrócić** Aby pobrać brakujące pakiety.</span><span class="sxs-lookup"><span data-stu-id="82686-225">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="82686-226">Na koniec Skompiluj rozwiązanie, klikając **kompilacji** | **Kompiluj rozwiązanie**.</span><span class="sxs-lookup"><span data-stu-id="82686-226">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="82686-227">Jedną z zalet za pomocą narzędzia NuGet jest, że nie masz do wysłania wszystkie biblioteki w projekcie, zmniejszenie rozmiaru projektu.</span><span class="sxs-lookup"><span data-stu-id="82686-227">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="82686-228">Za pomocą narzędzi NuGet Power Tools, określając wersji pakietu w pliku Packages.config można pobrać wymaganych bibliotek podczas pierwszego uruchomienia projektu.</span><span class="sxs-lookup"><span data-stu-id="82686-228">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="82686-229">Jest to, dlaczego należy uruchomić następujące kroki, po otwarciu istniejącego rozwiązania, w tym środowisku laboratoryjnym.</span><span class="sxs-lookup"><span data-stu-id="82686-229">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="82686-230">Otwórz widok indeksu StoreManager firmy.</span><span class="sxs-lookup"><span data-stu-id="82686-230">Open StoreManager's Index View.</span></span> <span data-ttu-id="82686-231">Aby to zrobić, w Eksploratorze rozwiązań rozwiń **widoków** folder, a następnie **StoreManager** , a następnie otwórz **Index.cshtml** pliku.</span><span class="sxs-lookup"><span data-stu-id="82686-231">To do this, in the Solution Explorer expand the **Views** folder, then the **StoreManager** and open the **Index.cshtml** file.</span></span>
3. <span data-ttu-id="82686-232">Dodaj następujący kod poniżej <strong>@model</strong> dyrektywy do definiowania <strong>Truncate</strong> metody pomocnika.</span><span class="sxs-lookup"><span data-stu-id="82686-232">Add the following code below the <strong>@model</strong> directive to define the <strong>Truncate</strong> helper method.</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample7.cshtml)]

<a id="Ex2Task2"></a>

<a id="Task_2_-_Truncating_Text_in_the_Page"></a>
#### <a name="task-2---truncating-text-in-the-page"></a><span data-ttu-id="82686-233">Zadanie 2 — obcinanie tekstu na stronie</span><span class="sxs-lookup"><span data-stu-id="82686-233">Task 2 - Truncating Text in the Page</span></span>

<span data-ttu-id="82686-234">W tym zadaniu zostanie użyty **Truncate** metody powoduje obcięcie tekstu w szablonie widoku.</span><span class="sxs-lookup"><span data-stu-id="82686-234">In this task, you will use the **Truncate** method to truncate the text in the View template.</span></span>

1. <span data-ttu-id="82686-235">Otwórz widok indeksu StoreManager firmy.</span><span class="sxs-lookup"><span data-stu-id="82686-235">Open StoreManager's Index View.</span></span> <span data-ttu-id="82686-236">Aby to zrobić, w Eksploratorze rozwiązań rozwiń **widoków** folder, a następnie **StoreManager** , a następnie otwórz **Index.cshtml** pliku.</span><span class="sxs-lookup"><span data-stu-id="82686-236">To do this, in the Solution Explorer expand the **Views** folder, then the **StoreManager** and open the **Index.cshtml** file.</span></span>
2. <span data-ttu-id="82686-237">Zamień wiersze, które pokazują **nazwy wykonawcy** i albumu **tytuł**.</span><span class="sxs-lookup"><span data-stu-id="82686-237">Replace the lines that show the **Artist Name** and Album's **Title**.</span></span> <span data-ttu-id="82686-238">Aby to zrobić, należy zastąpić następujące wiersze.</span><span class="sxs-lookup"><span data-stu-id="82686-238">To do this, replace the following lines.</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample8.cshtml)]

<a id="Ex2Task3"></a>

<a id="Task_3_-_Running_the_Application"></a>
#### <a name="task-3---running-the-application"></a><span data-ttu-id="82686-239">Zadanie 3. uruchamianie aplikacji</span><span class="sxs-lookup"><span data-stu-id="82686-239">Task 3 - Running the Application</span></span>

<span data-ttu-id="82686-240">W tym zadaniu przetestujesz, **StoreManager** **indeksu** Wyświetl szablon obcina tytułu i nazwy wykonawcy albumu.</span><span class="sxs-lookup"><span data-stu-id="82686-240">In this task, you will test that the **StoreManager** **Index** View template truncates the Album's Title and Artist Name.</span></span>

1. <span data-ttu-id="82686-241">Naciśnij klawisz **F5** do uruchomienia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="82686-241">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="82686-242">Data rozpoczęcia projektu na stronie głównej.</span><span class="sxs-lookup"><span data-stu-id="82686-242">The project starts in the Home page.</span></span> <span data-ttu-id="82686-243">Zmień adres URL do **/StoreManager** Aby sprawdzić, okres czasu teksty w **tytuł** i **wykonawcy** kolumny są obcinane.</span><span class="sxs-lookup"><span data-stu-id="82686-243">Change the URL to **/StoreManager** to verify that long texts in the **Title** and **Artist** column are truncated.</span></span>

    <span data-ttu-id="82686-244">![Obcięte nazwy tytułów i artystów](aspnet-mvc-4-helpers-forms-and-validation/_static/image6.png "obcięte nazwy tytułów i artystów")</span><span class="sxs-lookup"><span data-stu-id="82686-244">![Truncated titles and artists names](aspnet-mvc-4-helpers-forms-and-validation/_static/image6.png "Truncated titles and artists names")</span></span>

    <span data-ttu-id="82686-245">*Skrócona tytuły i nazwy wykonawców*</span><span class="sxs-lookup"><span data-stu-id="82686-245">*Truncated Titles and Artist Names*</span></span>

<a id="Exercise3"></a>

<a id="Exercise_3_Creating_the_Edit_View"></a>
### <a name="exercise-3-creating-the-edit-view"></a><span data-ttu-id="82686-246">Ćwiczenie 3: Tworzenie widoku edycji</span><span class="sxs-lookup"><span data-stu-id="82686-246">Exercise 3: Creating the Edit View</span></span>

<span data-ttu-id="82686-247">W tym ćwiczeniu dowiesz się, jak utworzyć formularz, aby umożliwić menedżerów store do edycji albumu.</span><span class="sxs-lookup"><span data-stu-id="82686-247">In this exercise, you will learn how to create a form to allow store managers to edit an Album.</span></span> <span data-ttu-id="82686-248">Będzie przeglądania **/StoreManager/Edit/id** adresu URL (**identyfikator** jest unikatowy identyfikator fotograficzne, aby edytować), dlatego wywołania HTTP GET do serwera.</span><span class="sxs-lookup"><span data-stu-id="82686-248">They will browse the **/StoreManager/Edit/id** URL (**id** being the unique id of the album to edit), thus making an HTTP-GET call to the server.</span></span>

<span data-ttu-id="82686-249">Metody akcji kontrolera edycji będzie pobrać odpowiedni Album z bazy danych, tworzenie **StoreManagerViewModel** obiekt do hermetyzacji go (wraz z listy artystów i gatunki), a następnie przekazać go, do widoku szablonu w celu Renderuj stronę HTML do użytkownika.</span><span class="sxs-lookup"><span data-stu-id="82686-249">The Controller Edit action method will retrieve the appropriate Album from the database, create a **StoreManagerViewModel** object to encapsulate it (along with a list of Artists and Genres), and then pass it off to a View template to render the HTML page back to the user.</span></span> <span data-ttu-id="82686-250">Ta strona będzie zawierać **&lt;formularza&gt;** elementu za pomocą pól tekstowych i menu rozwijane na potrzeby edytowania właściwości albumu.</span><span class="sxs-lookup"><span data-stu-id="82686-250">This page will contain a **&lt;form&gt;** element with textboxes and dropdowns for editing the Album properties.</span></span>

<span data-ttu-id="82686-251">Po użytkownik aktualizuje wartości formularza albumu i kliknie **Zapisz** przycisku, zmiany są przesyłane za pośrednictwem metody POST protokołu HTTP wywołania zwrotnego **/StoreManager/Edit/id**. Mimo, że adres URL pozostanie taki sam jak ostatniego wywołania, ASP.NET MVC określi, że teraz jest metodę POST protokołu HTTP i w związku z tym wykonuje różne metody akcji edycji (jeden ozdobione **[HttpPost]**).</span><span class="sxs-lookup"><span data-stu-id="82686-251">Once the user updates the Album form values and clicks the **Save** button, the changes are submitted via an HTTP-POST call back to **/StoreManager/Edit/id**. Although the URL remains the same as in the last call, ASP.NET MVC identifies that this time it is an HTTP-POST and therefore executes a different Edit action method (one decorated with **[HttpPost]**).</span></span>

<a id="Ex3Task1"></a>

<a id="Task_1_-_Implementing_the_HTTP-GET_Edit_Action_Method"></a>
#### <a name="task-1---implementing-the-http-get-edit-action-method"></a><span data-ttu-id="82686-252">Zadanie 1 — implementacja metody akcji edycji HTTP GET</span><span class="sxs-lookup"><span data-stu-id="82686-252">Task 1 - Implementing the HTTP-GET Edit Action Method</span></span>

<span data-ttu-id="82686-253">W tym zadaniu wdroży wersję HTTP GET metody akcji edycji można pobrać odpowiedni Album z bazy danych, a także wykaz wszystkich gatunki i artystów.</span><span class="sxs-lookup"><span data-stu-id="82686-253">In this task, you will implement the HTTP-GET version of the Edit action method to retrieve the appropriate Album from the database, as well as a list of all Genres and Artists.</span></span> <span data-ttu-id="82686-254">Jej będzie spakować te dane do **StoreManagerViewModel** zdefiniowanego w ostatnim kroku, który następnie zostanie przekazany do szablonu widoku do renderowania odpowiedzi z obiektu.</span><span class="sxs-lookup"><span data-stu-id="82686-254">It will package this data up into the **StoreManagerViewModel** object defined in the last step, which will then be passed to a View template to render the response with.</span></span>

1. <span data-ttu-id="82686-255">Otwórz **rozpocząć** rozwiązania znajdujący się w **źródło/Ex3-CreatingTheEditView/rozpoczęcia/** folderu.</span><span class="sxs-lookup"><span data-stu-id="82686-255">Open the **Begin** solution located at **Source/Ex3-CreatingTheEditView/Begin/** folder.</span></span> <span data-ttu-id="82686-256">W przeciwnym razie będziesz nadal korzystać z **zakończenia** rozwiązania uzyskać, wykonując poprzednim ćwiczeniu.</span><span class="sxs-lookup"><span data-stu-id="82686-256">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

   1. <span data-ttu-id="82686-257">Jeśli został otwarty dołączonym **rozpocząć** rozwiązania, należy pobrać niektóre brakujące pakiety NuGet przed kontynuowaniem.</span><span class="sxs-lookup"><span data-stu-id="82686-257">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="82686-258">Aby to zrobić, kliknij przycisk **projektu** menu, a następnie wybierz **Zarządzaj pakietami NuGet**.</span><span class="sxs-lookup"><span data-stu-id="82686-258">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="82686-259">W **Zarządzaj pakietami NuGet** okno dialogowe, kliknij przycisk **przywrócić** Aby pobrać brakujące pakiety.</span><span class="sxs-lookup"><span data-stu-id="82686-259">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="82686-260">Na koniec Skompiluj rozwiązanie, klikając **kompilacji** | **Kompiluj rozwiązanie**.</span><span class="sxs-lookup"><span data-stu-id="82686-260">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="82686-261">Jedną z zalet za pomocą narzędzia NuGet jest, że nie masz do wysłania wszystkie biblioteki w projekcie, zmniejszenie rozmiaru projektu.</span><span class="sxs-lookup"><span data-stu-id="82686-261">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="82686-262">Za pomocą narzędzi NuGet Power Tools, określając wersji pakietu w pliku Packages.config można pobrać wymaganych bibliotek podczas pierwszego uruchomienia projektu.</span><span class="sxs-lookup"><span data-stu-id="82686-262">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="82686-263">Jest to, dlaczego należy uruchomić następujące kroki, po otwarciu istniejącego rozwiązania, w tym środowisku laboratoryjnym.</span><span class="sxs-lookup"><span data-stu-id="82686-263">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="82686-264">Otwórz **StoreManagerController** klasy.</span><span class="sxs-lookup"><span data-stu-id="82686-264">Open the **StoreManagerController** class.</span></span> <span data-ttu-id="82686-265">Aby to zrobić, należy rozwinąć **kontrolerów** folder i kliknij dwukrotnie plik **StoreManagerController.cs**.</span><span class="sxs-lookup"><span data-stu-id="82686-265">To do this, expand the **Controllers** folder and double-click **StoreManagerController.cs**.</span></span>
3. <span data-ttu-id="82686-266">Zastąp **Edytuj HTTP GET** metodę akcji za pomocą następującego kodu, aby pobrać odpowiednią **albumu** , jak również **gatunki** i **artystów**listy.</span><span class="sxs-lookup"><span data-stu-id="82686-266">Replace the **HTTP-GET Edit** action method with the following code to retrieve the appropriate **Album** as well as the **Genres** and **Artists** lists.</span></span>

    <span data-ttu-id="82686-267">(Code Snippet — *pomocników programu ASP.NET MVC 4 i formularze i Walidacja - Ex3 StoreManagerController HTTP GET Edytuj akcję*)</span><span class="sxs-lookup"><span data-stu-id="82686-267">(Code Snippet - *ASP.NET MVC 4 Helpers and Forms and Validation - Ex3 StoreManagerController HTTP-GET Edit action*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample9.cs)]

    > [!NOTE]
    > <span data-ttu-id="82686-268">Używasz **System.Web.Mvc** **SelectList** dla artystów i gatunki zamiast **System.Collections.Generic** listy.</span><span class="sxs-lookup"><span data-stu-id="82686-268">You are using **System.Web.Mvc** **SelectList** for Artists and Genres instead of the **System.Collections.Generic** List.</span></span>
    > 
    > <span data-ttu-id="82686-269">**SelectList** jest bardziej przejrzysty sposób wypełniania list rozwijanych HTML i zarządzanie nimi elementów, takich jak bieżące zaznaczenie.</span><span class="sxs-lookup"><span data-stu-id="82686-269">**SelectList** is a cleaner way to populate HTML dropdowns and manage things like current selection.</span></span> <span data-ttu-id="82686-270">Utworzenie wystąpienia i nowsze, konfigurując tych obiektów ViewModel w akcji kontrolera spowoduje, że scenariusz formularz edycji czyszczenia.</span><span class="sxs-lookup"><span data-stu-id="82686-270">Instantiating and later setting up these ViewModel objects in the controller action will make the Edit form scenario cleaner.</span></span>

<a id="Ex3Task2"></a>

<a id="Task_2_-_Creating_the_Edit_View"></a>
#### <a name="task-2---creating-the-edit-view"></a><span data-ttu-id="82686-271">Zadanie 2 — Tworzenie widoku edycji</span><span class="sxs-lookup"><span data-stu-id="82686-271">Task 2 - Creating the Edit View</span></span>

<span data-ttu-id="82686-272">W tym zadaniu utworzysz szablon widoku Edycja, które później będą wyświetlane, właściwości albumu.</span><span class="sxs-lookup"><span data-stu-id="82686-272">In this task, you will create an Edit View template that will later display the album properties.</span></span>

1. <span data-ttu-id="82686-273">Utwórz widok edycji.</span><span class="sxs-lookup"><span data-stu-id="82686-273">Create the Edit View.</span></span> <span data-ttu-id="82686-274">Aby to zrobić, kliknij prawym przyciskiem myszy wewnątrz **Edytuj** metody akcji i wybierz **Dodaj widok**.</span><span class="sxs-lookup"><span data-stu-id="82686-274">To do this, right-click inside the **Edit** action method and select **Add View**.</span></span>
2. <span data-ttu-id="82686-275">W oknie dialogowym Dodaj widok, sprawdź, czy nazwa widoku jest **Edytuj**.</span><span class="sxs-lookup"><span data-stu-id="82686-275">In the Add View dialog, verify that the View Name is **Edit**.</span></span> <span data-ttu-id="82686-276">Sprawdź **utworzyć widok silnie typizowane** pole wyboru i wybierz **albumu (MvcMusicStore.Models)** z **wyświetlić klasy danych** listy rozwijanej.</span><span class="sxs-lookup"><span data-stu-id="82686-276">Check the **Create a strongly-typed view** checkbox and select **Album (MvcMusicStore.Models)** from the **View data class** drop-down.</span></span> <span data-ttu-id="82686-277">Wybierz **Edytuj** z **szablonu szkieletu** listy rozwijanej.</span><span class="sxs-lookup"><span data-stu-id="82686-277">Select **Edit** from the **Scaffold template** drop-down.</span></span> <span data-ttu-id="82686-278">Pozostaw wartości domyślne w pozostałych polach, a następnie kliknij przycisk **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="82686-278">Leave the other fields with their default value and then click **Add**.</span></span>

    <span data-ttu-id="82686-279">![Dodawanie widoku edycji](aspnet-mvc-4-helpers-forms-and-validation/_static/image7.png "Dodawanie widoku edycji")</span><span class="sxs-lookup"><span data-stu-id="82686-279">![Adding an Edit view](aspnet-mvc-4-helpers-forms-and-validation/_static/image7.png "Adding an Edit view")</span></span>

    <span data-ttu-id="82686-280">*Dodawanie widoku edycji*</span><span class="sxs-lookup"><span data-stu-id="82686-280">*Adding an Edit view*</span></span>

<a id="Ex3Task3"></a>

<a id="Task_3_-_Running_the_Application"></a>
#### <a name="task-3---running-the-application"></a><span data-ttu-id="82686-281">Zadanie 3. uruchamianie aplikacji</span><span class="sxs-lookup"><span data-stu-id="82686-281">Task 3 - Running the Application</span></span>

<span data-ttu-id="82686-282">W tym zadaniu przetestujesz, **StoreManager** **Edytuj** strona widoku są wyświetlane wartości właściwości albumu przekazany jako parametr.</span><span class="sxs-lookup"><span data-stu-id="82686-282">In this task, you will test that the **StoreManager** **Edit** View page displays the properties' values for the album passed as parameter.</span></span>

1. <span data-ttu-id="82686-283">Naciśnij klawisz **F5** do uruchomienia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="82686-283">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="82686-284">Data rozpoczęcia projektu na stronie głównej.</span><span class="sxs-lookup"><span data-stu-id="82686-284">The project starts in the Home page.</span></span> <span data-ttu-id="82686-285">Zmień adres URL do **/StoreManager/Edit/1** Aby sprawdzić, czy wartości właściwości albumu przekazywane są wyświetlane.</span><span class="sxs-lookup"><span data-stu-id="82686-285">Change the URL to **/StoreManager/Edit/1** to verify that the properties' values for the album passed are displayed.</span></span>

    <span data-ttu-id="82686-286">![Przeglądanie widoku edycji albumu](aspnet-mvc-4-helpers-forms-and-validation/_static/image8.png "przeglądania widoku edycji albumu")</span><span class="sxs-lookup"><span data-stu-id="82686-286">![Browsing Album's Edit View](aspnet-mvc-4-helpers-forms-and-validation/_static/image8.png "Browsing Album's Edit View")</span></span>

    <span data-ttu-id="82686-287">*Przeglądanie albumu widoku edycji*</span><span class="sxs-lookup"><span data-stu-id="82686-287">*Browsing Album's Edit view*</span></span>

<a id="Ex3Task4"></a>

<a id="Task_4_-_Implementing_drop-downs_on_the_Album_Editor_Template"></a>
#### <a name="task-4---implementing-drop-downs-on-the-album-editor-template"></a><span data-ttu-id="82686-288">Zadanie 4 — Implementowanie list rozwijanych w szablonie albumu edytora</span><span class="sxs-lookup"><span data-stu-id="82686-288">Task 4 - Implementing drop-downs on the Album Editor Template</span></span>

<span data-ttu-id="82686-289">W tym zadaniu dodasz list rozwijanych do Wyświetl szablon utworzony w poprzednim zadaniu tak, aby użytkownik może wybrać z listy, artystów i gatunki.</span><span class="sxs-lookup"><span data-stu-id="82686-289">In this task, you will add drop-downs to the View template created in the last task, so that the user can select from a list of Artists and Genres.</span></span>

1. <span data-ttu-id="82686-290">Zamień wszystkie **albumu** fieldset kod następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="82686-290">Replace all the **Album** fieldset code with the following:</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample10.cshtml)]

    > [!NOTE]
    > <span data-ttu-id="82686-291">**Html.DropDownList** pomocnika została dodana do listy rozwijane wyboru artystów i gatunki renderowania.</span><span class="sxs-lookup"><span data-stu-id="82686-291">An **Html.DropDownList** helper has been added to render drop-downs for choosing Artists and Genres.</span></span> <span data-ttu-id="82686-292">Parametry przekazywane do **Html.DropDownList** są:</span><span class="sxs-lookup"><span data-stu-id="82686-292">The parameters passed to **Html.DropDownList** are:</span></span>
    > 
    > 1. <span data-ttu-id="82686-293">Nazwa pola formularza (**&quot;ArtistId&quot;**).</span><span class="sxs-lookup"><span data-stu-id="82686-293">The name of the form field (**&quot;ArtistId&quot;**).</span></span>
    > 2. <span data-ttu-id="82686-294">**SelectList** wartości z listy rozwijanej.</span><span class="sxs-lookup"><span data-stu-id="82686-294">The **SelectList** of values for the drop-down.</span></span>

<a id="Ex3Task5"></a>

<a id="Task_5_-_Running_the_Application"></a>
#### <a name="task-5---running-the-application"></a><span data-ttu-id="82686-295">Zadanie 5 działania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="82686-295">Task 5 - Running the Application</span></span>

<span data-ttu-id="82686-296">W tym zadaniu przetestujesz, **StoreManager** **Edytuj** wyświetlona strona widoku listy rozwijane zamiast wykonawców i ID gatunku pól tekstowych.</span><span class="sxs-lookup"><span data-stu-id="82686-296">In this task, you will test that the **StoreManager** **Edit** View page displays drop-downs instead of Artist and Genre ID text fields.</span></span>

1. <span data-ttu-id="82686-297">Naciśnij klawisz **F5** do uruchomienia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="82686-297">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="82686-298">Data rozpoczęcia projektu na stronie głównej.</span><span class="sxs-lookup"><span data-stu-id="82686-298">The project starts in the Home page.</span></span> <span data-ttu-id="82686-299">Zmień adres URL do **/StoreManager/Edit/1** Aby sprawdzić, czy powoduje wyświetlenie listy rozwijane zamiast wykonawców i ID gatunku pól tekstowych.</span><span class="sxs-lookup"><span data-stu-id="82686-299">Change the URL to **/StoreManager/Edit/1** to verify that it displays drop-downs instead of Artist and Genre ID text fields.</span></span>

    <span data-ttu-id="82686-300">![Przeglądanie albumu widok do edycji z listy rozwijane](aspnet-mvc-4-helpers-forms-and-validation/_static/image9.png "widoku Edycja przeglądania Album z listy rozwijane")</span><span class="sxs-lookup"><span data-stu-id="82686-300">![Browsing Album's Edit View with drop downs](aspnet-mvc-4-helpers-forms-and-validation/_static/image9.png "Browsing Album's Edit View with drop downs")</span></span>

    <span data-ttu-id="82686-301">*Widok edycji przeglądania fotograficzne, tym razem z list rozwijanych*</span><span class="sxs-lookup"><span data-stu-id="82686-301">*Browsing Album's Edit view, this time with dropdowns*</span></span>

<a id="Ex3Task6"></a>

<a id="Task_6_-_Implementing_the_HTTP-POST_Edit_action_method"></a>
#### <a name="task-6---implementing-the-http-post-edit-action-method"></a><span data-ttu-id="82686-302">Zadanie 6 — Implementowanie metody akcji POST protokołu HTTP, Edytuj</span><span class="sxs-lookup"><span data-stu-id="82686-302">Task 6 - Implementing the HTTP-POST Edit action method</span></span>

<span data-ttu-id="82686-303">Teraz, gdy edytowanie widoku są wyświetlane zgodnie z oczekiwaniami, należy zaimplementować metodę Edytuj akcję POST protokołu HTTP, aby zapisać zmiany wprowadzone do albumu.</span><span class="sxs-lookup"><span data-stu-id="82686-303">Now that the Edit View displays as expected, you need to implement the HTTP-POST Edit Action method to save the changes made to the Album.</span></span>

1. <span data-ttu-id="82686-304">Zamknij przeglądarkę, jeśli to konieczne, aby powrócić do okna programu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="82686-304">Close the browser if needed, to return to the Visual Studio window.</span></span> <span data-ttu-id="82686-305">Otwórz **StoreManagerController** z **kontrolerów** folderu.</span><span class="sxs-lookup"><span data-stu-id="82686-305">Open **StoreManagerController** from the **Controllers** folder.</span></span>
2. <span data-ttu-id="82686-306">Zastąp **edytować żądania HTTP POST** kod do metody akcji przy użyciu następujących (należy pamiętać, że metoda, którą należy zastąpić przeciążona wersja, która otrzymuje dwa parametry):</span><span class="sxs-lookup"><span data-stu-id="82686-306">Replace **HTTP-POST Edit** action method code with the following (note that the method that must be replaced is overloaded version that receives two parameters):</span></span>

    <span data-ttu-id="82686-307">(Code Snippet — *pomocników programu ASP.NET MVC 4 i formularze i Walidacja - Ex3 StoreManagerController żądania HTTP POST Edytuj akcję*)</span><span class="sxs-lookup"><span data-stu-id="82686-307">(Code Snippet - *ASP.NET MVC 4 Helpers and Forms and Validation - Ex3 StoreManagerController HTTP-POST Edit action*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample11.cs)]

    > [!NOTE]
    > <span data-ttu-id="82686-308">Ta metoda zostanie wykonana, gdy użytkownik kliknie **Zapisz** przycisk widoku i wykonuje metodę POST protokołu HTTP-wartości formularza do serwera w celu utrwalenia ich w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="82686-308">This method will be executed when the user clicks the **Save** button of the View and performs an HTTP-POST of the form values back to the server to persist them in the database.</span></span> <span data-ttu-id="82686-309">Dekorator **[HttpPost]** wskazuje, że metoda powinien być używany dla tych scenariuszy POST protokołu HTTP.</span><span class="sxs-lookup"><span data-stu-id="82686-309">The decorator **[HttpPost]** indicates that the method should be used for those HTTP-POST scenarios.</span></span> <span data-ttu-id="82686-310">Ta metoda przyjmuje **albumu** obiektu.</span><span class="sxs-lookup"><span data-stu-id="82686-310">The method takes an **Album** object.</span></span> <span data-ttu-id="82686-311">ASP.NET MVC automatycznie utworzy obiekt Album z przesłanych &lt;formularza&gt; wartości.</span><span class="sxs-lookup"><span data-stu-id="82686-311">ASP.NET MVC will automatically create the Album object from the posted &lt;form&gt; values.</span></span>
    > 
    > <span data-ttu-id="82686-312">Metoda będzie wykonywać następujące czynności:</span><span class="sxs-lookup"><span data-stu-id="82686-312">The method will perform these steps:</span></span>
    > 
    > 1. <span data-ttu-id="82686-313">Jeśli model jest prawidłowy:</span><span class="sxs-lookup"><span data-stu-id="82686-313">If model is valid:</span></span>
    > 
    >     1. <span data-ttu-id="82686-314">Zaktualizuj wpis albumu w kontekście, aby oznaczyć go jako obiekt zmodyfikowany.</span><span class="sxs-lookup"><span data-stu-id="82686-314">Update the album entry in the context to mark it as a modified object.</span></span>
    >     2. <span data-ttu-id="82686-315">Zapisz zmiany i Przekieruj do widoku indeksu.</span><span class="sxs-lookup"><span data-stu-id="82686-315">Save the changes and redirect to the index view.</span></span>
    > 2. <span data-ttu-id="82686-316">Jeśli model nie jest prawidłowy, spowoduje to wypełnienie elementów ViewBag, za pomocą **GenreId** i **ArtistId**, wówczas zostanie zwrócone widok z odebranego obiektu fotograficzne, aby umożliwić użytkownikowi wykonać dowolne wymagane aktualizacje.</span><span class="sxs-lookup"><span data-stu-id="82686-316">If the model is not valid, it will populate the ViewBag with the **GenreId** and **ArtistId**, then it will return the view with the received Album object to allow the user perform any required update.</span></span>

<a id="Ex3Task7"></a>

<a id="Task_7_-_Running_the_Application"></a>
#### <a name="task-7---running-the-application"></a><span data-ttu-id="82686-317">Zadanie 7. uruchamianie aplikacji</span><span class="sxs-lookup"><span data-stu-id="82686-317">Task 7 - Running the Application</span></span>

<span data-ttu-id="82686-318">W tym zadaniu przetestujesz, **edycji StoreManager** strona widoku faktycznie zapisuje zaktualizowanych danych albumu w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="82686-318">In this task, you will test that the **StoreManager Edit** View page actually saves the updated Album data in the database.</span></span>

1. <span data-ttu-id="82686-319">Naciśnij klawisz **F5** do uruchomienia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="82686-319">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="82686-320">Data rozpoczęcia projektu na stronie głównej.</span><span class="sxs-lookup"><span data-stu-id="82686-320">The project starts in the Home page.</span></span> <span data-ttu-id="82686-321">Zmień adres URL do **/StoreManager/Edit/1**.</span><span class="sxs-lookup"><span data-stu-id="82686-321">Change the URL to **/StoreManager/Edit/1**.</span></span> <span data-ttu-id="82686-322">Zmień tytuł albumu **obciążenia** i kliknij pozycję **Zapisz**.</span><span class="sxs-lookup"><span data-stu-id="82686-322">Change the Album title to **Load** and click on **Save**.</span></span> <span data-ttu-id="82686-323">Upewnij się, że jego tytuł rzeczywiste zmiany na liście albumów.</span><span class="sxs-lookup"><span data-stu-id="82686-323">Verify that album's title actually changed in the list of albums.</span></span>

    <span data-ttu-id="82686-324">![Aktualizowanie albumu](aspnet-mvc-4-helpers-forms-and-validation/_static/image10.png "aktualizowanie albumu")</span><span class="sxs-lookup"><span data-stu-id="82686-324">![Updating an album](aspnet-mvc-4-helpers-forms-and-validation/_static/image10.png "Updating an album")</span></span>

    <span data-ttu-id="82686-325">*Aktualizowanie albumu*</span><span class="sxs-lookup"><span data-stu-id="82686-325">*Updating an Album*</span></span>

<a id="Exercise4"></a>

<a id="Exercise_4_Adding_a_Create_View"></a>
### <a name="exercise-4-adding-a-create-view"></a><span data-ttu-id="82686-326">Ćwiczenie 4: Dodawanie widoku Create</span><span class="sxs-lookup"><span data-stu-id="82686-326">Exercise 4: Adding a Create View</span></span>

<span data-ttu-id="82686-327">Teraz, gdy **StoreManagerController** obsługuje **Edytuj** możliwości, w tym ćwiczeniu zostanie dowiesz się, jak dodać szablon Create View umożliwiające przechowywanie menedżerowie dodawać nowe albumów do aplikacji.</span><span class="sxs-lookup"><span data-stu-id="82686-327">Now that the **StoreManagerController** supports the **Edit** ability, in this exercise you will learn how to add a Create View template to let store managers add new Albums to the application.</span></span>

<span data-ttu-id="82686-328">Jak za pomocą funkcji Edytuj wdroży scenariusz tworzenia przy użyciu dwóch oddzielnych metod w obrębie **StoreManagerController** klasy:</span><span class="sxs-lookup"><span data-stu-id="82686-328">Like you did with the Edit functionality, you will implement the Create scenario using two separate methods within the **StoreManagerController** class:</span></span>

1. <span data-ttu-id="82686-329">Jedną z metod akcji zostanie wyświetlony pusty formularz menedżerów magazynu pierwszej wizyty w witrynie **/StoreManager/tworzenie** adresu URL.</span><span class="sxs-lookup"><span data-stu-id="82686-329">One action method will display an empty form when store managers first visit the **/StoreManager/Create** URL.</span></span>
2. <span data-ttu-id="82686-330">Druga metoda akcji będzie obsługiwać scenariusz, gdzie kliknie Menedżer magazynu **Zapisz** znajdujący się w obrębie formularza i przesyła wartości z powrotem do **/StoreManager/tworzenie** adresu URL jako metodę POST protokołu HTTP.</span><span class="sxs-lookup"><span data-stu-id="82686-330">A second action method will handle the scenario where the store manager clicks the **Save** button within the form and submits the values back to the **/StoreManager/Create** URL as an HTTP-POST.</span></span>

<a id="Ex4Task1"></a>

<a id="Task_1_-_Implementing_the_HTTP-GET_Create_action_method"></a>
#### <a name="task-1---implementing-the-http-get-create-action-method"></a><span data-ttu-id="82686-331">Zadanie 1 — implementacja metody GET protokołu HTTP, Utwórz akcję</span><span class="sxs-lookup"><span data-stu-id="82686-331">Task 1 - Implementing the HTTP-GET Create action method</span></span>

<span data-ttu-id="82686-332">W tym zadaniu wdroży wersję HTTP GET metody akcji Utwórz, aby pobrać listę wszystkich gatunki i artystów, spakować te dane do **StoreManagerViewModel** obiektu, który następnie zostanie przekazany do szablonu widoku.</span><span class="sxs-lookup"><span data-stu-id="82686-332">In this task, you will implement the HTTP-GET version of the Create action method to retrieve a list of all Genres and Artists, package this data up into a **StoreManagerViewModel** object, which will then be passed to a View template.</span></span>

1. <span data-ttu-id="82686-333">Otwórz **rozpocząć** rozwiązania znajdujący się w **źródło/Ex4-AddingACreateView/rozpoczęcia/** folderu.</span><span class="sxs-lookup"><span data-stu-id="82686-333">Open the **Begin** solution located at **Source/Ex4-AddingACreateView/Begin/** folder.</span></span> <span data-ttu-id="82686-334">W przeciwnym razie będziesz nadal korzystać z **zakończenia** rozwiązania uzyskać, wykonując poprzednim ćwiczeniu.</span><span class="sxs-lookup"><span data-stu-id="82686-334">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

   1. <span data-ttu-id="82686-335">Jeśli został otwarty dołączonym **rozpocząć** rozwiązania, należy pobrać niektóre brakujące pakiety NuGet przed kontynuowaniem.</span><span class="sxs-lookup"><span data-stu-id="82686-335">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="82686-336">Aby to zrobić, kliknij przycisk **projektu** menu, a następnie wybierz **Zarządzaj pakietami NuGet**.</span><span class="sxs-lookup"><span data-stu-id="82686-336">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="82686-337">W **Zarządzaj pakietami NuGet** okno dialogowe, kliknij przycisk **przywrócić** Aby pobrać brakujące pakiety.</span><span class="sxs-lookup"><span data-stu-id="82686-337">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="82686-338">Na koniec Skompiluj rozwiązanie, klikając **kompilacji** | **Kompiluj rozwiązanie**.</span><span class="sxs-lookup"><span data-stu-id="82686-338">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="82686-339">Jedną z zalet za pomocą narzędzia NuGet jest, że nie masz do wysłania wszystkie biblioteki w projekcie, zmniejszenie rozmiaru projektu.</span><span class="sxs-lookup"><span data-stu-id="82686-339">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="82686-340">Za pomocą narzędzi NuGet Power Tools, określając wersji pakietu w pliku Packages.config można pobrać wymaganych bibliotek podczas pierwszego uruchomienia projektu.</span><span class="sxs-lookup"><span data-stu-id="82686-340">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="82686-341">Jest to, dlaczego należy uruchomić następujące kroki, po otwarciu istniejącego rozwiązania, w tym środowisku laboratoryjnym.</span><span class="sxs-lookup"><span data-stu-id="82686-341">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="82686-342">Otwórz **StoreManagerController** klasy.</span><span class="sxs-lookup"><span data-stu-id="82686-342">Open **StoreManagerController** class.</span></span> <span data-ttu-id="82686-343">Aby to zrobić, należy rozwinąć **kontrolerów** folder i kliknij dwukrotnie plik **StoreManagerController.cs**.</span><span class="sxs-lookup"><span data-stu-id="82686-343">To do this, expand the **Controllers** folder and double-click **StoreManagerController.cs**.</span></span>
3. <span data-ttu-id="82686-344">Zastąp **Utwórz** kod do metody akcji następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="82686-344">Replace the **Create** action method code with the following:</span></span>

    <span data-ttu-id="82686-345">(Code Snippet — *pomocników programu ASP.NET MVC 4 i formularze i Walidacja - Ex4 StoreManagerController HTTP GET utworzenie*)</span><span class="sxs-lookup"><span data-stu-id="82686-345">(Code Snippet - *ASP.NET MVC 4 Helpers and Forms and Validation - Ex4 StoreManagerController HTTP-GET Create action*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample12.cs)]

<a id="Ex4Task2"></a>

<a id="Task_2_-_Adding_the_Create_View"></a>
#### <a name="task-2---adding-the-create-view"></a><span data-ttu-id="82686-346">Zadanie 2 — Dodawanie widoku Create</span><span class="sxs-lookup"><span data-stu-id="82686-346">Task 2 - Adding the Create View</span></span>

<span data-ttu-id="82686-347">W ramach tego zadania zostaną dodane szablon Utwórz widok, który będzie pojawi się nowy formularz albumu (pusty).</span><span class="sxs-lookup"><span data-stu-id="82686-347">In this task, you will add the Create View template that will display a new (empty) Album form.</span></span>

1. <span data-ttu-id="82686-348">Kliknij prawym przyciskiem myszy wewnątrz **Utwórz** metody akcji i wybierz **Dodaj widok**.</span><span class="sxs-lookup"><span data-stu-id="82686-348">Right-click inside the **Create** action method and select **Add View**.</span></span> <span data-ttu-id="82686-349">Zostanie wyświetlone okno dialogowe dodawania widoku.</span><span class="sxs-lookup"><span data-stu-id="82686-349">This will bring up the Add View dialog.</span></span>
2. <span data-ttu-id="82686-350">W oknie dialogowym Dodaj widok, sprawdź, czy nazwa widoku jest **Utwórz**.</span><span class="sxs-lookup"><span data-stu-id="82686-350">In the Add View dialog, verify that the View Name is **Create**.</span></span> <span data-ttu-id="82686-351">Wybierz **utworzyć widok silnie typizowane** opcji, a następnie wybierz pozycję **albumu (MvcMusicStore.Models)** z **klasa modelu** listy rozwijanej i **Utwórz** z **szablonu szkieletu** listy rozwijanej.</span><span class="sxs-lookup"><span data-stu-id="82686-351">Select the **Create a strongly-typed view** option and select **Album (MvcMusicStore.Models)** from the **Model class** drop-down and **Create** from the **Scaffold template** drop-down.</span></span> <span data-ttu-id="82686-352">Pozostaw wartości domyślne w pozostałych polach, a następnie kliknij przycisk **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="82686-352">Leave the other fields with their default value and then click **Add**.</span></span>

    <span data-ttu-id="82686-353">![Dodawanie widoku Utwórz](aspnet-mvc-4-helpers-forms-and-validation/_static/image11.png "Dodawanie a-create-view.png")</span><span class="sxs-lookup"><span data-stu-id="82686-353">![Adding a create view](aspnet-mvc-4-helpers-forms-and-validation/_static/image11.png "adding-a-create-view.png")</span></span>

    <span data-ttu-id="82686-354">*Dodawanie widoku Create*</span><span class="sxs-lookup"><span data-stu-id="82686-354">*Adding the Create View*</span></span>
3. <span data-ttu-id="82686-355">Aktualizacja **GenreId** i **ArtistId** pola, aby użyć listy rozwijanej, jak pokazano poniżej:</span><span class="sxs-lookup"><span data-stu-id="82686-355">Update the **GenreId** and **ArtistId** fields to use a drop-down list as shown below:</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample13.cshtml)]

<a id="Ex4Task3"></a>

<a id="Task_3_-_Running_the_Application"></a>
#### <a name="task-3---running-the-application"></a><span data-ttu-id="82686-356">Zadanie 3. uruchamianie aplikacji</span><span class="sxs-lookup"><span data-stu-id="82686-356">Task 3 - Running the Application</span></span>

<span data-ttu-id="82686-357">W tym zadaniu przetestujesz, **StoreManager** **Utwórz** widoku strony wyświetli pusty formularz albumu.</span><span class="sxs-lookup"><span data-stu-id="82686-357">In this task, you will test that the **StoreManager** **Create** View page displays an empty Album form.</span></span>

1. <span data-ttu-id="82686-358">Naciśnij klawisz **F5** do uruchomienia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="82686-358">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="82686-359">Data rozpoczęcia projektu na stronie głównej.</span><span class="sxs-lookup"><span data-stu-id="82686-359">The project starts in the Home page.</span></span> <span data-ttu-id="82686-360">Zmień adres URL do **/StoreManager/tworzenie**.</span><span class="sxs-lookup"><span data-stu-id="82686-360">Change the URL to **/StoreManager/Create**.</span></span> <span data-ttu-id="82686-361">Sprawdź, czy pusty formularz jest wyświetlany do wypełniania nowej właściwości albumu.</span><span class="sxs-lookup"><span data-stu-id="82686-361">Verify that an empty form is displayed for filling the new Album properties.</span></span>

    <span data-ttu-id="82686-362">![Utwórz widok przy użyciu pusty formularz](aspnet-mvc-4-helpers-forms-and-validation/_static/image12.png "Create View z pusty formularz")</span><span class="sxs-lookup"><span data-stu-id="82686-362">![Create View with an empty form](aspnet-mvc-4-helpers-forms-and-validation/_static/image12.png "Create View with an empty form")</span></span>

    <span data-ttu-id="82686-363">*Utwórz widok przy użyciu pusty formularz*</span><span class="sxs-lookup"><span data-stu-id="82686-363">*Create View with an empty form*</span></span>

<a id="Ex4Task4"></a>

<a id="Task_4_-_Implementing_the_HTTP-POST_Create_Action_Method"></a>
#### <a name="task-4---implementing-the-http-post-create-action-method"></a><span data-ttu-id="82686-364">Zadanie 4 — Implementowanie POST protokołu HTTP, utwórz metody akcji</span><span class="sxs-lookup"><span data-stu-id="82686-364">Task 4 - Implementing the HTTP-POST Create Action Method</span></span>

<span data-ttu-id="82686-365">W tym zadaniu będą wdrażać wersję POST protokołu HTTP, utwórz metody akcji, która będzie wywołana, gdy użytkownik kliknie **Zapisz** przycisku.</span><span class="sxs-lookup"><span data-stu-id="82686-365">In this task, you will implement the HTTP-POST version of the Create action method that will be invoked when a user clicks the **Save** button.</span></span> <span data-ttu-id="82686-366">Metoda należy zapisać nowy album w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="82686-366">The method should save the new album in the database.</span></span>

1. <span data-ttu-id="82686-367">Zamknij przeglądarkę, jeśli to konieczne, aby powrócić do okna programu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="82686-367">Close the browser if needed, to return to the Visual Studio window.</span></span> <span data-ttu-id="82686-368">Otwórz **StoreManagerController** klasy.</span><span class="sxs-lookup"><span data-stu-id="82686-368">Open **StoreManagerController** class.</span></span> <span data-ttu-id="82686-369">Aby to zrobić, należy rozwinąć **kontrolerów** folder i kliknij dwukrotnie plik **StoreManagerController.cs**.</span><span class="sxs-lookup"><span data-stu-id="82686-369">To do this, expand the **Controllers** folder and double-click **StoreManagerController.cs**.</span></span>
2. <span data-ttu-id="82686-370">Zastąp **POST protokołu HTTP, Utwórz** kod do metody akcji następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="82686-370">Replace **HTTP-POST Create** action method code with the following:</span></span>

    <span data-ttu-id="82686-371">(Code Snippet — *pomocników programu ASP.NET MVC 4 i formularze i Walidacja - Ex4 StoreManagerController HTTP - POST tworzenie akcji*)</span><span class="sxs-lookup"><span data-stu-id="82686-371">(Code Snippet - *ASP.NET MVC 4 Helpers and Forms and Validation - Ex4 StoreManagerController HTTP- POST Create action*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample14.cs)]

    > [!NOTE]
    > <span data-ttu-id="82686-372">Akcja Utwórz jest dość podobny do poprzedniego metody akcji, Edytuj, ale zamiast ustawienia obiektu, ponieważ zmodyfikowane, to są dodawane do kontekstu.</span><span class="sxs-lookup"><span data-stu-id="82686-372">The Create action is pretty similar to the previous Edit action method but instead of setting the object as modified, it is being added to the context.</span></span>

<a id="Ex4Task5"></a>

<a id="Task_5_-_Running_the_Application"></a>
#### <a name="task-5---running-the-application"></a><span data-ttu-id="82686-373">Zadanie 5 działania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="82686-373">Task 5 - Running the Application</span></span>

<span data-ttu-id="82686-374">W tym zadaniu przetestujesz, **tworzenie StoreManager** strona widoku umożliwia utworzenie nowego albumu, a następnie przekierowuje do widoku indeksu StoreManager.</span><span class="sxs-lookup"><span data-stu-id="82686-374">In this task, you will test that the **StoreManager Create** View page lets you create a new Album and then redirects to the StoreManager Index View.</span></span>

1. <span data-ttu-id="82686-375">Naciśnij klawisz **F5** do uruchomienia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="82686-375">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="82686-376">Data rozpoczęcia projektu na stronie głównej.</span><span class="sxs-lookup"><span data-stu-id="82686-376">The project starts in the Home page.</span></span> <span data-ttu-id="82686-377">Zmień adres URL do **/StoreManager/tworzenie**.</span><span class="sxs-lookup"><span data-stu-id="82686-377">Change the URL to **/StoreManager/Create**.</span></span> <span data-ttu-id="82686-378">Wypełnij wszystkie pola formularza z danych dla nowego albumu podobne do pokazanego na poniższym rysunku:</span><span class="sxs-lookup"><span data-stu-id="82686-378">Fill all the form fields with data for a new Album, like the one in the following figure:</span></span>

    <span data-ttu-id="82686-379">![Tworzenie albumu](aspnet-mvc-4-helpers-forms-and-validation/_static/image13.png "tworzeniu albumu")</span><span class="sxs-lookup"><span data-stu-id="82686-379">![Creating an Album](aspnet-mvc-4-helpers-forms-and-validation/_static/image13.png "Creating an Album")</span></span>

    <span data-ttu-id="82686-380">*Tworzenie albumu*</span><span class="sxs-lookup"><span data-stu-id="82686-380">*Creating an Album*</span></span>
3. <span data-ttu-id="82686-381">Sprawdź, czy uzyskać przekierowanie, do widoku indeksu StoreManager, która obejmuje nowy Album właśnie utworzony.</span><span class="sxs-lookup"><span data-stu-id="82686-381">Verify that you get redirected to the StoreManager Index View that includes the new Album just created.</span></span>

    <span data-ttu-id="82686-382">![Nowy Album utworzone](aspnet-mvc-4-helpers-forms-and-validation/_static/image14.png "nowy Album utworzone")</span><span class="sxs-lookup"><span data-stu-id="82686-382">![New Album Created](aspnet-mvc-4-helpers-forms-and-validation/_static/image14.png "New Album Created")</span></span>

    <span data-ttu-id="82686-383">*Nowy Album utworzone*</span><span class="sxs-lookup"><span data-stu-id="82686-383">*New Album Created*</span></span>

<a id="Exercise5"></a>

<a id="Exercise_5_Handling_Deletion"></a>
### <a name="exercise-5-handling-deletion"></a><span data-ttu-id="82686-384">Ćwiczenie 5: Obsługa usuwania</span><span class="sxs-lookup"><span data-stu-id="82686-384">Exercise 5: Handling Deletion</span></span>

<span data-ttu-id="82686-385">Możliwość usunięcia albumów nie została jeszcze zaimplementowana.</span><span class="sxs-lookup"><span data-stu-id="82686-385">The ability to delete albums is not yet implemented.</span></span> <span data-ttu-id="82686-386">Jest to ćwiczenie będzie o.</span><span class="sxs-lookup"><span data-stu-id="82686-386">This is what this exercise will be about.</span></span> <span data-ttu-id="82686-387">Tak jak poprzednio, wdroży scenariusz Delete, przy użyciu dwóch oddzielnych metod w **StoreManagerController** klasy:</span><span class="sxs-lookup"><span data-stu-id="82686-387">Like before, you will implement the Delete scenario using two separate methods within the **StoreManagerController** class:</span></span>

1. <span data-ttu-id="82686-388">Jedną z metod akcji będą wyświetlane w formularzu potwierdzenia</span><span class="sxs-lookup"><span data-stu-id="82686-388">One action method will display a confirmation form</span></span>
2. <span data-ttu-id="82686-389">Druga metoda akcji będzie obsługiwać przesyłania formularza</span><span class="sxs-lookup"><span data-stu-id="82686-389">A second action method will handle the form submission</span></span>

<a id="Ex5Task1"></a>

<a id="Task_1_-_Implementing_the_HTTP-GET_Delete_Action_Method"></a>
#### <a name="task-1---implementing-the-http-get-delete-action-method"></a><span data-ttu-id="82686-390">Zadanie 1 — implementacja metody akcji Delete protokołu HTTP GET</span><span class="sxs-lookup"><span data-stu-id="82686-390">Task 1 - Implementing the HTTP-GET Delete Action Method</span></span>

<span data-ttu-id="82686-391">W tym zadaniu wdroży wersję HTTP GET metody akcji usuwania można pobrać informacji o albumu.</span><span class="sxs-lookup"><span data-stu-id="82686-391">In this task, you will implement the HTTP-GET version of the Delete action method to retrieve the album's information.</span></span>

1. <span data-ttu-id="82686-392">Otwórz **rozpocząć** rozwiązania znajdujący się w **źródło/Ex5-HandlingDeletion/rozpoczęcia/** folderu.</span><span class="sxs-lookup"><span data-stu-id="82686-392">Open the **Begin** solution located at **Source/Ex5-HandlingDeletion/Begin/** folder.</span></span> <span data-ttu-id="82686-393">W przeciwnym razie będziesz nadal korzystać z **zakończenia** rozwiązania uzyskać, wykonując poprzednim ćwiczeniu.</span><span class="sxs-lookup"><span data-stu-id="82686-393">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

   1. <span data-ttu-id="82686-394">Jeśli został otwarty dołączonym **rozpocząć** rozwiązania, należy pobrać niektóre brakujące pakiety NuGet przed kontynuowaniem.</span><span class="sxs-lookup"><span data-stu-id="82686-394">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="82686-395">Aby to zrobić, kliknij przycisk **projektu** menu, a następnie wybierz **Zarządzaj pakietami NuGet**.</span><span class="sxs-lookup"><span data-stu-id="82686-395">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="82686-396">W **Zarządzaj pakietami NuGet** okno dialogowe, kliknij przycisk **przywrócić** Aby pobrać brakujące pakiety.</span><span class="sxs-lookup"><span data-stu-id="82686-396">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="82686-397">Na koniec Skompiluj rozwiązanie, klikając **kompilacji** | **Kompiluj rozwiązanie**.</span><span class="sxs-lookup"><span data-stu-id="82686-397">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="82686-398">Jedną z zalet za pomocą narzędzia NuGet jest, że nie masz do wysłania wszystkie biblioteki w projekcie, zmniejszenie rozmiaru projektu.</span><span class="sxs-lookup"><span data-stu-id="82686-398">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="82686-399">Za pomocą narzędzi NuGet Power Tools, określając wersji pakietu w pliku Packages.config można pobrać wymaganych bibliotek podczas pierwszego uruchomienia projektu.</span><span class="sxs-lookup"><span data-stu-id="82686-399">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="82686-400">Jest to, dlaczego należy uruchomić następujące kroki, po otwarciu istniejącego rozwiązania, w tym środowisku laboratoryjnym.</span><span class="sxs-lookup"><span data-stu-id="82686-400">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="82686-401">Otwórz **StoreManagerController** klasy.</span><span class="sxs-lookup"><span data-stu-id="82686-401">Open **StoreManagerController** class.</span></span> <span data-ttu-id="82686-402">Aby to zrobić, należy rozwinąć **kontrolerów** folder i kliknij dwukrotnie plik **StoreManagerController.cs**.</span><span class="sxs-lookup"><span data-stu-id="82686-402">To do this, expand the **Controllers** folder and double-click **StoreManagerController.cs**.</span></span>
3. <span data-ttu-id="82686-403">Usuwanie akcji kontrolera jest dokładnie taka sama poprzedniej akcji kontrolera szczegóły Store: zapytań **albumu** obiektu z bazy danych przy użyciu **identyfikator** podany adres URL i zwraca odpowiednie **widoku**.</span><span class="sxs-lookup"><span data-stu-id="82686-403">The Delete controller action is exactly the same as the previous Store Details controller action: it queries the **album** object from the database using the **id** provided in the URL and returns the appropriate **View**.</span></span> <span data-ttu-id="82686-404">Aby to zrobić, Zastąp HTTP GET **Usuń** kod do metody akcji następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="82686-404">To do this, replace the HTTP-GET **Delete** action method code with the following:</span></span>

    <span data-ttu-id="82686-405">(Code Snippet — *pomocników programu ASP.NET MVC 4 i formularze i Walidacja - Ex5 obsługi usuwania HTTP GET usunąć akcję*)</span><span class="sxs-lookup"><span data-stu-id="82686-405">(Code Snippet - *ASP.NET MVC 4 Helpers and Forms and Validation - Ex5 Handling Deletion HTTP-GET Delete action*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample15.cs)]
4. <span data-ttu-id="82686-406">Kliknij prawym przyciskiem myszy wewnątrz **Usuń** metody akcji i wybierz **Dodaj widok**.</span><span class="sxs-lookup"><span data-stu-id="82686-406">Right-click inside the **Delete** action method and select **Add View**.</span></span> <span data-ttu-id="82686-407">Zostanie wyświetlone okno dialogowe dodawania widoku.</span><span class="sxs-lookup"><span data-stu-id="82686-407">This will bring up the Add View dialog.</span></span>
5. <span data-ttu-id="82686-408">W oknie dialogowym Dodaj widok, sprawdź, czy nazwa widoku jest **Usuń**.</span><span class="sxs-lookup"><span data-stu-id="82686-408">In the Add View dialog, verify that the View name is **Delete**.</span></span> <span data-ttu-id="82686-409">Wybierz **utworzyć widok silnie typizowane** opcji, a następnie wybierz pozycję **albumu (MvcMusicStore.Models)** z **klasa modelu** listy rozwijanej.</span><span class="sxs-lookup"><span data-stu-id="82686-409">Select the **Create a strongly-typed view** option and select **Album (MvcMusicStore.Models)** from the **Model class** drop-down.</span></span> <span data-ttu-id="82686-410">Wybierz **Usuń** z **szablonu szkieletu** listy rozwijanej.</span><span class="sxs-lookup"><span data-stu-id="82686-410">Select **Delete** from the **Scaffold template** drop-down.</span></span> <span data-ttu-id="82686-411">Pozostaw wartości domyślne w pozostałych polach, a następnie kliknij przycisk **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="82686-411">Leave the other fields with their default value and then click **Add**.</span></span>

    <span data-ttu-id="82686-412">![Dodawanie widoku Delete](aspnet-mvc-4-helpers-forms-and-validation/_static/image15.png "Dodawanie widoku Delete")</span><span class="sxs-lookup"><span data-stu-id="82686-412">![Adding a Delete View](aspnet-mvc-4-helpers-forms-and-validation/_static/image15.png "Adding a Delete View")</span></span>

    <span data-ttu-id="82686-413">*Dodawanie widoku Delete*</span><span class="sxs-lookup"><span data-stu-id="82686-413">*Adding a Delete View*</span></span>
6. <span data-ttu-id="82686-414">Usuń szablon zawiera wszystkie pola z modelu.</span><span class="sxs-lookup"><span data-stu-id="82686-414">The Delete template shows all the fields from the model.</span></span> <span data-ttu-id="82686-415">Zostaną wyświetlone tylko tytuł albumu.</span><span class="sxs-lookup"><span data-stu-id="82686-415">You will show only the album's title.</span></span> <span data-ttu-id="82686-416">Aby to zrobić, Zastąp zawartość widoku z następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="82686-416">To do this, replace the content of the view with the following code:</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample16.cshtml)]

<a id="Ex05Task2"></a>

<a id="Task_2_-_Running_the_Application"></a>
#### <a name="task-2---running-the-application"></a><span data-ttu-id="82686-417">Zadanie 2 — uruchamianie aplikacji</span><span class="sxs-lookup"><span data-stu-id="82686-417">Task 2 - Running the Application</span></span>

<span data-ttu-id="82686-418">W tym zadaniu przetestujesz, **StoreManager** **Usuń** widoku strony wyświetla formularz potwierdzenie usunięcia.</span><span class="sxs-lookup"><span data-stu-id="82686-418">In this task, you will test that the **StoreManager** **Delete** View page displays a confirmation deletion form.</span></span>

1. <span data-ttu-id="82686-419">Naciśnij klawisz **F5** do uruchomienia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="82686-419">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="82686-420">Data rozpoczęcia projektu na stronie głównej.</span><span class="sxs-lookup"><span data-stu-id="82686-420">The project starts in the Home page.</span></span> <span data-ttu-id="82686-421">Zmień adres URL do **/StoreManager**.</span><span class="sxs-lookup"><span data-stu-id="82686-421">Change the URL to **/StoreManager**.</span></span> <span data-ttu-id="82686-422">Wybierz jeden album można usunąć, klikając **Usuń** i sprawdź, czy zostanie przekazany nowy widok.</span><span class="sxs-lookup"><span data-stu-id="82686-422">Select one album to delete by clicking **Delete** and verify that the new view is uploaded.</span></span>

    <span data-ttu-id="82686-423">![Usuwanie albumu](aspnet-mvc-4-helpers-forms-and-validation/_static/image16.png "Usuwanie albumu")</span><span class="sxs-lookup"><span data-stu-id="82686-423">![Deleting an Album](aspnet-mvc-4-helpers-forms-and-validation/_static/image16.png "Deleting an Album")</span></span>

    <span data-ttu-id="82686-424">*Usuwanie albumu*</span><span class="sxs-lookup"><span data-stu-id="82686-424">*Deleting an Album*</span></span>

<a id="Ex05Task3"></a>

<a id="Task_3-_Implementing_the_HTTP-POST_Delete_Action_Method"></a>
#### <a name="task-3--implementing-the-http-post-delete-action-method"></a><span data-ttu-id="82686-425">Zadanie 3 — implementacja metody akcji usuwania POST protokołu HTTP</span><span class="sxs-lookup"><span data-stu-id="82686-425">Task 3- Implementing the HTTP-POST Delete Action Method</span></span>

<span data-ttu-id="82686-426">W tym zadaniu będą wdrażać wersję POST protokołu HTTP, Usuń metody akcji, która będzie wywołana, gdy użytkownik kliknie **Usuń** przycisku.</span><span class="sxs-lookup"><span data-stu-id="82686-426">In this task, you will implement the HTTP-POST version of the Delete action method that will be invoked when a user clicks the **Delete** button.</span></span> <span data-ttu-id="82686-427">Metoda, należy usunąć albumu w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="82686-427">The method should delete the album in the database.</span></span>

1. <span data-ttu-id="82686-428">Zamknij przeglądarkę, jeśli to konieczne, aby powrócić do okna programu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="82686-428">Close the browser if needed, to return to the Visual Studio window.</span></span> <span data-ttu-id="82686-429">Otwórz **StoreManagerController** klasy.</span><span class="sxs-lookup"><span data-stu-id="82686-429">Open **StoreManagerController** class.</span></span> <span data-ttu-id="82686-430">Aby to zrobić, należy rozwinąć **kontrolerów** folder i kliknij dwukrotnie plik **StoreManagerController.cs**.</span><span class="sxs-lookup"><span data-stu-id="82686-430">To do this, expand the **Controllers** folder and double-click **StoreManagerController.cs**.</span></span>
2. <span data-ttu-id="82686-431">Zastąp **POST protokołu HTTP, Usuń** kod do metody akcji następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="82686-431">Replace **HTTP-POST Delete** action method code with the following:</span></span>

    <span data-ttu-id="82686-432">(Code Snippet — *pomocników programu ASP.NET MVC 4 i formularze i Walidacja - Ex5 obsługi usuwania żądania HTTP POST akcji*)</span><span class="sxs-lookup"><span data-stu-id="82686-432">(Code Snippet - *ASP.NET MVC 4 Helpers and Forms and Validation - Ex5 Handling Deletion HTTP-POST Delete action*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample17.cs)]

<a id="Ex5Task4"></a>

<a id="Task_4_-_Running_the_Application"></a>
#### <a name="task-4---running-the-application"></a><span data-ttu-id="82686-433">Zadanie 4 — uruchamianie aplikacji</span><span class="sxs-lookup"><span data-stu-id="82686-433">Task 4 - Running the Application</span></span>

<span data-ttu-id="82686-434">W tym zadaniu przetestujesz, **StoreManager Delete** strona widoku służy do usuwania albumu, a następnie przekierowuje do widoku indeksu StoreManager.</span><span class="sxs-lookup"><span data-stu-id="82686-434">In this task, you will test that the **StoreManager Delete** View page lets you delete an Album and then redirects to the StoreManager Index View.</span></span>

1. <span data-ttu-id="82686-435">Naciśnij klawisz **F5** do uruchomienia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="82686-435">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="82686-436">Data rozpoczęcia projektu na stronie głównej.</span><span class="sxs-lookup"><span data-stu-id="82686-436">The project starts in the Home page.</span></span> <span data-ttu-id="82686-437">Zmień adres URL do **/StoreManager**.</span><span class="sxs-lookup"><span data-stu-id="82686-437">Change the URL to **/StoreManager**.</span></span> <span data-ttu-id="82686-438">Wybierz jeden album można usunąć, klikając **usunąć.**</span><span class="sxs-lookup"><span data-stu-id="82686-438">Select one album to delete by clicking **Delete.**</span></span> <span data-ttu-id="82686-439">Potwierdzenie usunięcia, klikając **Usuń** przycisku:</span><span class="sxs-lookup"><span data-stu-id="82686-439">Confirm the deletion by clicking **Delete** button:</span></span>

    <span data-ttu-id="82686-440">![Usuwanie albumu](aspnet-mvc-4-helpers-forms-and-validation/_static/image17.png "Usuwanie albumu")</span><span class="sxs-lookup"><span data-stu-id="82686-440">![Deleting an Album](aspnet-mvc-4-helpers-forms-and-validation/_static/image17.png "Deleting an Album")</span></span>

    <span data-ttu-id="82686-441">*Usuwanie albumu*</span><span class="sxs-lookup"><span data-stu-id="82686-441">*Deleting an Album*</span></span>
3. <span data-ttu-id="82686-442">Sprawdź, czy album została usunięta, ponieważ nie ma w **indeksu** strony.</span><span class="sxs-lookup"><span data-stu-id="82686-442">Verify that the album was deleted since it does not appear in the **Index** page.</span></span>

<a id="Exercise6"></a>

<a id="Exercise_6_Adding_Validation"></a>
### <a name="exercise-6-adding-validation"></a><span data-ttu-id="82686-443">Ćwiczenie 6: Dodawanie walidacji</span><span class="sxs-lookup"><span data-stu-id="82686-443">Exercise 6: Adding Validation</span></span>

<span data-ttu-id="82686-444">Obecnie tworzyć i edytować formularze, które zostały spełnione, nie należy wykonywać wszelkiego rodzaju sprawdzania poprawności.</span><span class="sxs-lookup"><span data-stu-id="82686-444">Currently, the Create and Edit forms you have in place do not perform any kind of validation.</span></span> <span data-ttu-id="82686-445">Jeśli użytkownik opuści wymagane pole jest puste lub typ liter w polu Cena, pierwszy błąd, który zostanie wyświetlony będzie z bazy danych.</span><span class="sxs-lookup"><span data-stu-id="82686-445">If the user leaves a required field blank or type letters in the price field, the first error you will get will be from the database.</span></span>

<span data-ttu-id="82686-446">Aby dodać sprawdzanie poprawności do aplikacji, dodawanie adnotacji danych do klasy modelu.</span><span class="sxs-lookup"><span data-stu-id="82686-446">You can add validation to the application by adding Data Annotations to your model class.</span></span> <span data-ttu-id="82686-447">Adnotacje danych Zezwalaj, zawierająca opis reguły, które mają stosowane do właściwości modelu i platformy ASP.NET MVC zajmie się wymuszanie i wyświetlania odpowiedni komunikat do użytkowników.</span><span class="sxs-lookup"><span data-stu-id="82686-447">Data Annotations allow describing the rules you want applied to your model properties, and ASP.NET MVC will take care of enforcing and displaying appropriate message to users.</span></span>

<a id="Ex06Task1"></a>

<a id="Task_1_-_Adding_Data_Annotations"></a>
#### <a name="task-1---adding-data-annotations"></a><span data-ttu-id="82686-448">Zadanie 1 — Dodawanie adnotacji danych</span><span class="sxs-lookup"><span data-stu-id="82686-448">Task 1 - Adding Data Annotations</span></span>

<span data-ttu-id="82686-449">W ramach tego zadania zostaną dodane adnotacje danych do modelu fotograficzne, który spowoduje, że strona tworzyć i edytować wyświetlanie komunikatów dotyczących sprawdzania poprawności, gdy jest to konieczne.</span><span class="sxs-lookup"><span data-stu-id="82686-449">In this task, you will add Data Annotations to the Album Model that will make the Create and Edit page display validation messages when appropriate.</span></span>

<span data-ttu-id="82686-450">Proste klasy modelu, dodawanie adnotacji danych po prostu odbywa się przez dodanie **przy użyciu** poufności informacji dotyczące **System.ComponentModel.DataAnnotation**, następnie umieszczenie **[wymagane]** atrybutu odpowiednie właściwości.</span><span class="sxs-lookup"><span data-stu-id="82686-450">For a simple Model class, adding a Data Annotation is just handled by adding a **using** statement for **System.ComponentModel.DataAnnotation**, then placing a **[Required]** attribute on the appropriate properties.</span></span> <span data-ttu-id="82686-451">Poniższy przykład czyniłyby **nazwa** właściwości wymagane pola w widoku.</span><span class="sxs-lookup"><span data-stu-id="82686-451">The following example would make the **Name** property a required field in the View.</span></span>

[!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample18.cs)]

<span data-ttu-id="82686-452">Jest to nieco bardziej złożone, w przypadkach, takich jak tej aplikacji, gdzie jest generowany modelu Entity Data Model.</span><span class="sxs-lookup"><span data-stu-id="82686-452">This is a little more complex in cases like this application where the Entity Data Model is generated.</span></span> <span data-ttu-id="82686-453">Jeśli dodano adnotacje danych bezpośrednio do klasy modeli, ich zostaną zastąpione po aktualizacji modelu z bazy danych.</span><span class="sxs-lookup"><span data-stu-id="82686-453">If you added Data Annotations directly to the model classes, they would be overwritten if you update the model from the database.</span></span> <span data-ttu-id="82686-454">Zamiast tego można tworzyć z metadanych klas częściowych, które będzie istnieć do przechowywania adnotacje i skojarzone z modelu należy używać klas, za pomocą **[MetadataType]** atrybutu.</span><span class="sxs-lookup"><span data-stu-id="82686-454">Instead, you can make use of metadata partial classes which will exist to hold the annotations and are associated with the model classes using the **[MetadataType]** attribute.</span></span>

1. <span data-ttu-id="82686-455">Otwórz **rozpocząć** rozwiązania znajdujący się w **źródło/Ex6-AddingValidation/rozpoczęcia/** folderu.</span><span class="sxs-lookup"><span data-stu-id="82686-455">Open the **Begin** solution located at **Source/Ex6-AddingValidation/Begin/** folder.</span></span> <span data-ttu-id="82686-456">W przeciwnym razie będziesz nadal korzystać z **zakończenia** rozwiązania uzyskać, wykonując poprzednim ćwiczeniu.</span><span class="sxs-lookup"><span data-stu-id="82686-456">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

   1. <span data-ttu-id="82686-457">Jeśli został otwarty dołączonym **rozpocząć** rozwiązania, należy pobrać niektóre brakujące pakiety NuGet przed kontynuowaniem.</span><span class="sxs-lookup"><span data-stu-id="82686-457">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="82686-458">Aby to zrobić, kliknij przycisk **projektu** menu, a następnie wybierz **Zarządzaj pakietami NuGet**.</span><span class="sxs-lookup"><span data-stu-id="82686-458">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="82686-459">W **Zarządzaj pakietami NuGet** okno dialogowe, kliknij przycisk **przywrócić** Aby pobrać brakujące pakiety.</span><span class="sxs-lookup"><span data-stu-id="82686-459">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="82686-460">Na koniec Skompiluj rozwiązanie, klikając **kompilacji** | **Kompiluj rozwiązanie**.</span><span class="sxs-lookup"><span data-stu-id="82686-460">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="82686-461">Jedną z zalet za pomocą narzędzia NuGet jest, że nie masz do wysłania wszystkie biblioteki w projekcie, zmniejszenie rozmiaru projektu.</span><span class="sxs-lookup"><span data-stu-id="82686-461">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="82686-462">Za pomocą narzędzi NuGet Power Tools, określając wersji pakietu w pliku Packages.config można pobrać wymaganych bibliotek podczas pierwszego uruchomienia projektu.</span><span class="sxs-lookup"><span data-stu-id="82686-462">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="82686-463">Jest to, dlaczego należy uruchomić następujące kroki, po otwarciu istniejącego rozwiązania, w tym środowisku laboratoryjnym.</span><span class="sxs-lookup"><span data-stu-id="82686-463">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="82686-464">Otwórz **Album.cs** z **modeli** folderu.</span><span class="sxs-lookup"><span data-stu-id="82686-464">Open the **Album.cs** from the **Models** folder.</span></span>
3. <span data-ttu-id="82686-465">Zastąp **Album.cs** zawartości ze wyróżniony kod, tak że wygląda podobnie do poniższego:</span><span class="sxs-lookup"><span data-stu-id="82686-465">Replace **Album.cs** content with the highlighted code, so that it looks like the following:</span></span>

    > [!NOTE]
    > <span data-ttu-id="82686-466">Wiersz **[DisplayFormat(ConvertEmptyStringToNull=false)]** wskazuje, czy puste ciągi z modelu nie zostaną przekonwertowane na wartości null, po zaktualizowaniu pola danych w źródle danych.</span><span class="sxs-lookup"><span data-stu-id="82686-466">The line **[DisplayFormat(ConvertEmptyStringToNull=false)]** indicates that empty strings from the model won't be converted to null when the data field is updated in the data source.</span></span> <span data-ttu-id="82686-467">To ustawienie pozwoli uniknąć wyjątek, gdy platforma Entity Framework przypisuje wartości null do modelu przed adnotacji danych sprawdza poprawność pól.</span><span class="sxs-lookup"><span data-stu-id="82686-467">This setting will avoid an exception when the Entity Framework assigns null values to the model before Data Annotation validates the fields.</span></span>

    <span data-ttu-id="82686-468">(Code Snippet — *pomocników programu ASP.NET MVC 4 i formularze i Walidacja - klasy częściowej metadanych albumu Ex6*)</span><span class="sxs-lookup"><span data-stu-id="82686-468">(Code Snippet - *ASP.NET MVC 4 Helpers and Forms and Validation - Ex6 Album metadata partial class*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample19.cs)]

    > [!NOTE]
    > <span data-ttu-id="82686-469">To **albumu** częściowa klasa ma **MetadataType** atrybut, który wskazuje na **AlbumMetaData** klasy dla adnotacji danych.</span><span class="sxs-lookup"><span data-stu-id="82686-469">This **Album** partial class has a **MetadataType** attribute which points to the **AlbumMetaData** class for the Data Annotations.</span></span> <span data-ttu-id="82686-470">Oto niektóre atrybuty adnotacji danych, których używasz dodawać adnotacje do modelu albumu:</span><span class="sxs-lookup"><span data-stu-id="82686-470">These are some of the Data Annotation attributes you are using to annotate the Album model:</span></span>
    > 
    > - <span data-ttu-id="82686-471">Niewymagana — wskazuje, że właściwość jest polem wymaganym</span><span class="sxs-lookup"><span data-stu-id="82686-471">Required - Indicates that the property is a required field</span></span>
    > - <span data-ttu-id="82686-472">DisplayName — Określa tekst, który ma być używany na pola formularza i komunikatów dotyczących sprawdzania poprawności</span><span class="sxs-lookup"><span data-stu-id="82686-472">DisplayName - Defines the text to be used on form fields and validation messages</span></span>
    > - <span data-ttu-id="82686-473">DisplayFormat — określa sposób wyświetlania i sformatowane pola danych.</span><span class="sxs-lookup"><span data-stu-id="82686-473">DisplayFormat - Specifies how data fields are displayed and formatted.</span></span>
    > - <span data-ttu-id="82686-474">StringLength - określa maksymalną długość pola ciągu</span><span class="sxs-lookup"><span data-stu-id="82686-474">StringLength - Defines a maximum length for a string field</span></span>
    > - <span data-ttu-id="82686-475">Zakres — zapewnia maksymalne i minimalne wartości pola numerycznego</span><span class="sxs-lookup"><span data-stu-id="82686-475">Range - Gives a maximum and minimum value for a numeric field</span></span>
    > - <span data-ttu-id="82686-476">ScaffoldColumn — umożliwia ukrycie pola w edytorze formularzy</span><span class="sxs-lookup"><span data-stu-id="82686-476">ScaffoldColumn - Allows hiding fields from editor forms</span></span>

<a id="Ex06Task2"></a>

<a id="Task_2_-_Running_the_Application"></a>
#### <a name="task-2---running-the-application"></a><span data-ttu-id="82686-477">Zadanie 2 — uruchamianie aplikacji</span><span class="sxs-lookup"><span data-stu-id="82686-477">Task 2 - Running the Application</span></span>

<span data-ttu-id="82686-478">W tym zadaniu przetestujesz, tworzyć i edytować stron sprawdzanie poprawności pola, przy użyciu nazw wyświetlanych wybrany w poprzednim zadaniu.</span><span class="sxs-lookup"><span data-stu-id="82686-478">In this task, you will test that the Create and Edit pages validate fields, using the display names chosen in the last task.</span></span>

1. <span data-ttu-id="82686-479">Naciśnij klawisz **F5** do uruchomienia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="82686-479">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="82686-480">Data rozpoczęcia projektu na stronie głównej.</span><span class="sxs-lookup"><span data-stu-id="82686-480">The project starts in the Home page.</span></span> <span data-ttu-id="82686-481">Zmień adres URL do **/StoreManager/tworzenie**.</span><span class="sxs-lookup"><span data-stu-id="82686-481">Change the URL to **/StoreManager/Create**.</span></span> <span data-ttu-id="82686-482">Sprawdź, czy nazwy wyświetlane zgodne z typami klasy częściowej (takich jak **URL grafikę albumu** zamiast **AlbumArtUrl**)</span><span class="sxs-lookup"><span data-stu-id="82686-482">Verify that the display names match the ones in the partial class (like **Album Art URL** instead of **AlbumArtUrl**)</span></span>
3. <span data-ttu-id="82686-483">Kliknij przycisk **Utwórz**, bez wypełniania formularza.</span><span class="sxs-lookup"><span data-stu-id="82686-483">Click **Create**, without filling the form.</span></span> <span data-ttu-id="82686-484">Upewnij się, że otrzymasz odpowiednie komunikaty sprawdzania poprawności.</span><span class="sxs-lookup"><span data-stu-id="82686-484">Verify that you get the corresponding validation messages.</span></span>

    <span data-ttu-id="82686-485">![Zweryfikowane pola na stronie Utwórz](aspnet-mvc-4-helpers-forms-and-validation/_static/image18.png "zweryfikowane pola na stronie Utwórz")</span><span class="sxs-lookup"><span data-stu-id="82686-485">![Validated fields in the Create page](aspnet-mvc-4-helpers-forms-and-validation/_static/image18.png "Validated fields in the Create page")</span></span>

    <span data-ttu-id="82686-486">*Sprawdzone pola na stronie Utwórz*</span><span class="sxs-lookup"><span data-stu-id="82686-486">*Validated fields in the Create page*</span></span>
4. <span data-ttu-id="82686-487">Możesz sprawdzić, że takie same odbywa się za pomocą **Edytuj** strony.</span><span class="sxs-lookup"><span data-stu-id="82686-487">You can verify that the same occurs with the **Edit** page.</span></span> <span data-ttu-id="82686-488">Zmień adres URL do **/StoreManager/Edit/1** i sprawdź, czy nazwy wyświetlane zgodne z typami klasy częściowej (takich jak **URL grafikę albumu** zamiast **AlbumArtUrl**).</span><span class="sxs-lookup"><span data-stu-id="82686-488">Change the URL to **/StoreManager/Edit/1** and verify that the display names match the ones in the partial class (like **Album Art URL** instead of **AlbumArtUrl**).</span></span> <span data-ttu-id="82686-489">Pusty **tytuł** i **cena** pola i kliknij przycisk **Zapisz**.</span><span class="sxs-lookup"><span data-stu-id="82686-489">Empty the **Title** and **Price** fields and click **Save**.</span></span> <span data-ttu-id="82686-490">Upewnij się, że otrzymasz odpowiednie komunikaty sprawdzania poprawności.</span><span class="sxs-lookup"><span data-stu-id="82686-490">Verify that you get the corresponding validation messages.</span></span>

    ![Sprawdzone pola na stronie edycji](aspnet-mvc-4-helpers-forms-and-validation/_static/image19.png)

    <span data-ttu-id="82686-492">*Sprawdzone pola na stronie edycji*</span><span class="sxs-lookup"><span data-stu-id="82686-492">*Validated fields in the Edit page*</span></span>

<a id="Exercise7"></a>

<a id="Exercise_7_Using_Unobtrusive_jQuery_at_Client_Side"></a>
### <a name="exercise-7-using-unobtrusive-jquery-at-client-side"></a><span data-ttu-id="82686-493">Ćwiczenie 7: Przy użyciu jQuery dyskretny kod po stronie klienta</span><span class="sxs-lookup"><span data-stu-id="82686-493">Exercise 7: Using Unobtrusive jQuery at Client Side</span></span>

<span data-ttu-id="82686-494">W tym ćwiczeniu dowiesz się, jak włączyć dotyczącą weryfikacji jQuery MVC 4 dyskretny kod po stronie klienta.</span><span class="sxs-lookup"><span data-stu-id="82686-494">In this exercise, you will learn how to enable MVC 4 Unobtrusive jQuery validation at client side.</span></span>

> [!NOTE]
> <span data-ttu-id="82686-495">Dyskretnego kodu jQuery używa prefiksu danych ajax JavaScript do wywołania metody akcji, na serwerze, a nie intrusively emitowanie wbudowanych skryptach klienta.</span><span class="sxs-lookup"><span data-stu-id="82686-495">The Unobtrusive jQuery uses data-ajax prefix JavaScript to invoke action methods on the server rather than intrusively emitting inline client scripts.</span></span>


<a id="Ex7Task1"></a>

<a id="Task_1_-_Running_the_Application_before_Enabling_Unobtrusive_jQuery"></a>
#### <a name="task-1---running-the-application-before-enabling-unobtrusive-jquery"></a><span data-ttu-id="82686-496">Zadanie 1. uruchomienie aplikacji przed włączeniem dyskretnego kodu jQuery</span><span class="sxs-lookup"><span data-stu-id="82686-496">Task 1 - Running the Application before Enabling Unobtrusive jQuery</span></span>

<span data-ttu-id="82686-497">W tym zadaniu należy uruchomić aplikację przed dołączeniem jQuery, aby można było porównać oba modele sprawdzania poprawności.</span><span class="sxs-lookup"><span data-stu-id="82686-497">In this task, you will run the application before including jQuery in order to compare both validation models.</span></span>

1. <span data-ttu-id="82686-498">Otwórz **rozpocząć** rozwiązania znajdujący się w **źródło/Ex7-UnobtrusivejQueryValidation/rozpoczęcia/** folderu.</span><span class="sxs-lookup"><span data-stu-id="82686-498">Open the **Begin** solution located at **Source/Ex7-UnobtrusivejQueryValidation/Begin/** folder.</span></span> <span data-ttu-id="82686-499">W przeciwnym razie będziesz nadal korzystać z **zakończenia** rozwiązania uzyskać, wykonując poprzednim ćwiczeniu.</span><span class="sxs-lookup"><span data-stu-id="82686-499">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

   1. <span data-ttu-id="82686-500">Jeśli został otwarty dołączonym **rozpocząć** rozwiązania, należy pobrać niektóre brakujące pakiety NuGet przed kontynuowaniem.</span><span class="sxs-lookup"><span data-stu-id="82686-500">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="82686-501">Aby to zrobić, kliknij przycisk **projektu** menu, a następnie wybierz **Zarządzaj pakietami NuGet**.</span><span class="sxs-lookup"><span data-stu-id="82686-501">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="82686-502">W **Zarządzaj pakietami NuGet** okno dialogowe, kliknij przycisk **przywrócić** Aby pobrać brakujące pakiety.</span><span class="sxs-lookup"><span data-stu-id="82686-502">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="82686-503">Na koniec Skompiluj rozwiązanie, klikając **kompilacji** | **Kompiluj rozwiązanie**.</span><span class="sxs-lookup"><span data-stu-id="82686-503">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="82686-504">Jedną z zalet za pomocą narzędzia NuGet jest, że nie masz do wysłania wszystkie biblioteki w projekcie, zmniejszenie rozmiaru projektu.</span><span class="sxs-lookup"><span data-stu-id="82686-504">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="82686-505">Za pomocą narzędzi NuGet Power Tools, określając wersji pakietu w pliku Packages.config można pobrać wymaganych bibliotek podczas pierwszego uruchomienia projektu.</span><span class="sxs-lookup"><span data-stu-id="82686-505">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="82686-506">Jest to, dlaczego należy uruchomić następujące kroki, po otwarciu istniejącego rozwiązania, w tym środowisku laboratoryjnym.</span><span class="sxs-lookup"><span data-stu-id="82686-506">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="82686-507">Naciśnij klawisz **F5** do uruchomienia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="82686-507">Press **F5** to run the application.</span></span>
3. <span data-ttu-id="82686-508">Data rozpoczęcia projektu na stronie głównej.</span><span class="sxs-lookup"><span data-stu-id="82686-508">The project starts in the Home page.</span></span> <span data-ttu-id="82686-509">Przeglądaj **/StoreManager/tworzenie** i kliknij przycisk **Utwórz** bez wypełniania formularza, aby zweryfikować, że masz komunikatów dotyczących sprawdzania poprawności:</span><span class="sxs-lookup"><span data-stu-id="82686-509">Browse **/StoreManager/Create** and click **Create** without filling the form to verify that you get validation messages:</span></span>

    <span data-ttu-id="82686-510">![Sprawdzanie poprawności klienta wyłączone](aspnet-mvc-4-helpers-forms-and-validation/_static/image20.png "wyłączyć sprawdzanie poprawności klienta")</span><span class="sxs-lookup"><span data-stu-id="82686-510">![Client validation disabled](aspnet-mvc-4-helpers-forms-and-validation/_static/image20.png "Client validation disabled")</span></span>

    <span data-ttu-id="82686-511">*Sprawdzanie poprawności klienta wyłączone*</span><span class="sxs-lookup"><span data-stu-id="82686-511">*Client validation disabled*</span></span>
4. <span data-ttu-id="82686-512">W przeglądarce otwórz kod źródłowy HTML:</span><span class="sxs-lookup"><span data-stu-id="82686-512">In the browser, open the HTML source code:</span></span>

    [!code-html[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample20.html)]

<a id="Ex7Task2"></a>

<a id="Task_2_-_Enabling_Unobtrusive_Client_Validation"></a>
#### <a name="task-2---enabling-unobtrusive-client-validation"></a><span data-ttu-id="82686-513">Zadanie 2 — Włączanie sprawdzania poprawności dyskretnego kodu klienta</span><span class="sxs-lookup"><span data-stu-id="82686-513">Task 2 - Enabling Unobtrusive Client Validation</span></span>

<span data-ttu-id="82686-514">W ramach tego zadania umożliwi jQuery **sprawdzania poprawności dyskretnego kodu klienta** z **Web.config** pliku, który jest domyślnie ustawiona na wartość false, we wszystkich nowych projektach programu ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="82686-514">In this task, you will enable jQuery **unobtrusive client validation** from **Web.config** file, which is by default set to false in all new ASP.NET MVC 4 projects.</span></span> <span data-ttu-id="82686-515">Ponadto dodasz, że niezbędne skrypty odwołania się jQuery pracy dyskretny kod weryfikacji klienta.</span><span class="sxs-lookup"><span data-stu-id="82686-515">Additionally, you will add the necessary scripts references to make jQuery Unobtrusive Client Validation work.</span></span>

1. <span data-ttu-id="82686-516">Otwórz **Web.Config** plików w katalogu głównym projektu i upewnij się, że **ClientValidationEnabled** i **UnobtrusiveJavaScriptEnabled** kluczy wartości są ustawione na **true**.</span><span class="sxs-lookup"><span data-stu-id="82686-516">Open **Web.Config** file at project root, and make sure that the **ClientValidationEnabled** and **UnobtrusiveJavaScriptEnabled** keys values are set to **true**.</span></span>

    [!code-xml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample21.xml)]

    > [!NOTE]
    > <span data-ttu-id="82686-517">Można także włączyć sprawdzanie poprawności klienta przez kod w Global.asax.cs, aby uzyskać te same wyniki:</span><span class="sxs-lookup"><span data-stu-id="82686-517">You can also enable client validation by code at Global.asax.cs to get the same results:</span></span>
    > 
    > <span data-ttu-id="82686-518">**HtmlHelper.ClientValidationEnabled = true;**</span><span class="sxs-lookup"><span data-stu-id="82686-518">**HtmlHelper.ClientValidationEnabled = true;**</span></span>
    > 
    > <span data-ttu-id="82686-519">Ponadto można przypisać atrybutu ClientValidationEnabled do dowolnego kontrolera, aby określić niestandardowe zachowanie.</span><span class="sxs-lookup"><span data-stu-id="82686-519">Additionally, you can assign ClientValidationEnabled attribute into any controller to have a custom behavior.</span></span>
2. <span data-ttu-id="82686-520">Otwórz **Create.cshtml** na **Views\StoreManager**.</span><span class="sxs-lookup"><span data-stu-id="82686-520">Open **Create.cshtml** at **Views\StoreManager**.</span></span>
3. <span data-ttu-id="82686-521">Upewnij się, że następujące pliki skryptów **jquery.validate** i **jquery.validate.unobtrusive**, do których istnieją odwołania w poprzez wyświetlanie &quot; **~/bundles/jqueryval** &quot; pakietu.</span><span class="sxs-lookup"><span data-stu-id="82686-521">Make sure the following script files, **jquery.validate** and **jquery.validate.unobtrusive**, are referenced in the view throught the &quot;**~/bundles/jqueryval**&quot; bundle.</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample22.cshtml)]

    > [!NOTE]
    > <span data-ttu-id="82686-522">Te biblioteki jQuery są uwzględniane w nowych projektów MVC 4.</span><span class="sxs-lookup"><span data-stu-id="82686-522">All these jQuery libraries are included in MVC 4 new projects.</span></span> <span data-ttu-id="82686-523">Można znaleźć więcej bibliotek w **/skrypty** folderu w projekcie.</span><span class="sxs-lookup"><span data-stu-id="82686-523">You can find more libraries in the **/Scripts** folder of you project.</span></span>
    > 
    > <span data-ttu-id="82686-524">Przed tym sprawdzania poprawności pracy bibliotek, musisz dodać odwołanie do biblioteki framework jQuery.</span><span class="sxs-lookup"><span data-stu-id="82686-524">In order to make this validation libraries work, you need to add a reference to the jQuery framework library.</span></span> <span data-ttu-id="82686-525">Ponieważ ta dokumentacja została już dodana w  **\_Layout.cshtml** pliku, nie trzeba go dodać w tego widoku.</span><span class="sxs-lookup"><span data-stu-id="82686-525">Since this reference is already added in the **\_Layout.cshtml** file, you do not need to add it in this particular view.</span></span>

<a id="Ex7Task3"></a>

<a id="Task_3_-_Running_the_Application_Using_Unobtrusive_jQuery_Validation"></a>
#### <a name="task-3---running-the-application-using-unobtrusive-jquery-validation"></a><span data-ttu-id="82686-526">Zadanie 3 — uruchamianie dyskretnego kodu za pomocą aplikacji jQuery sprawdzania poprawności</span><span class="sxs-lookup"><span data-stu-id="82686-526">Task 3 - Running the Application Using Unobtrusive jQuery Validation</span></span>

<span data-ttu-id="82686-527">W tym zadaniu przetestujesz, **StoreManager** Tworzenie szablonu przeprowadza weryfikacji po stronie klienta przy użyciu biblioteki jQuery, gdy użytkownik tworzy nowy album widoku.</span><span class="sxs-lookup"><span data-stu-id="82686-527">In this task, you will test that the **StoreManager** create view template performs client side validation using jQuery libraries when the user creates a new album.</span></span>

1. <span data-ttu-id="82686-528">Naciśnij klawisz **F5** do uruchomienia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="82686-528">Press **F5** to run the application.</span></span>
2. <span data-ttu-id="82686-529">Data rozpoczęcia projektu na stronie głównej.</span><span class="sxs-lookup"><span data-stu-id="82686-529">The project starts in the Home page.</span></span> <span data-ttu-id="82686-530">Przeglądaj **/StoreManager/tworzenie** i kliknij przycisk **Utwórz** bez wypełniania formularza, aby zweryfikować, że masz komunikatów dotyczących sprawdzania poprawności:</span><span class="sxs-lookup"><span data-stu-id="82686-530">Browse **/StoreManager/Create** and click **Create** without filling the form to verify that you get validation messages:</span></span>

    <span data-ttu-id="82686-531">![Sprawdzanie poprawności klienta przy użyciu jQuery włączone](aspnet-mvc-4-helpers-forms-and-validation/_static/image21.png "sprawdzanie poprawności klienta przy użyciu jQuery włączone")</span><span class="sxs-lookup"><span data-stu-id="82686-531">![Client validation with jQuery enabled](aspnet-mvc-4-helpers-forms-and-validation/_static/image21.png "Client validation with jQuery enabled")</span></span>

    <span data-ttu-id="82686-532">*Sprawdzanie poprawności klienta przy użyciu jQuery włączone*</span><span class="sxs-lookup"><span data-stu-id="82686-532">*Client validation with jQuery enabled*</span></span>
3. <span data-ttu-id="82686-533">W przeglądarce otwórz kod źródłowy Utwórz widok:</span><span class="sxs-lookup"><span data-stu-id="82686-533">In the browser, open the source code for Create view:</span></span>

    [!code-html[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample23.html)]

   > [!NOTE]
   > <span data-ttu-id="82686-534">Dla każdej reguły weryfikacji klienta jQuery dyskretny kod dodaje atrybut z danymi — val —*rulename*=&quot;*komunikat*&quot;.</span><span class="sxs-lookup"><span data-stu-id="82686-534">For each client validation rule, Unobtrusive jQuery adds an attribute with data-val-*rulename*=&quot;*message*&quot;.</span></span> <span data-ttu-id="82686-535">Poniżej przedstawiono listę tagów tego Unobtrusive jQuery wstawia do pola wejściowego html, aby wykonać sprawdzanie poprawności klienta:</span><span class="sxs-lookup"><span data-stu-id="82686-535">Below is a list of tags that Unobtrusive jQuery inserts into the html input field to perform client validation:</span></span>
   > 
   > - <span data-ttu-id="82686-536">Val danych</span><span class="sxs-lookup"><span data-stu-id="82686-536">Data-val</span></span>
   > - <span data-ttu-id="82686-537">Dane val liczb</span><span class="sxs-lookup"><span data-stu-id="82686-537">Data-val-number</span></span>
   > - <span data-ttu-id="82686-538">Dane val zakresu</span><span class="sxs-lookup"><span data-stu-id="82686-538">Data-val-range</span></span>
   > - <span data-ttu-id="82686-539">Data-val zakres min / danych val — — maksimum zakresu</span><span class="sxs-lookup"><span data-stu-id="82686-539">Data-val-range-min / Data-val-range-max</span></span>
   > - <span data-ttu-id="82686-540">Wymagane val dane</span><span class="sxs-lookup"><span data-stu-id="82686-540">Data-val-required</span></span>
   > - <span data-ttu-id="82686-541">Dane o długości val</span><span class="sxs-lookup"><span data-stu-id="82686-541">Data-val-length</span></span>
   > - <span data-ttu-id="82686-542">Data-val długość max / danych val długość min</span><span class="sxs-lookup"><span data-stu-id="82686-542">Data-val-length-max / Data-val-length-min</span></span>
   > 
   > <span data-ttu-id="82686-543">Wszystkie wartości danych są wypełniane przy użyciu modelu **adnotacji danych**.</span><span class="sxs-lookup"><span data-stu-id="82686-543">All the data values are filled with model **Data Annotation**.</span></span> <span data-ttu-id="82686-544">Następnie całą logikę, która działa na po stronie serwera może działać po stronie klienta.</span><span class="sxs-lookup"><span data-stu-id="82686-544">Then, all the logic that works at server side can be run at client side.</span></span> <span data-ttu-id="82686-545">Na przykład cena atrybut ma następujące adnotacji danych w modelu:</span><span class="sxs-lookup"><span data-stu-id="82686-545">For example, Price attribute has the following data annotation in the model:</span></span>
   > 
   > [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample24.cs)]
   > 
   > <span data-ttu-id="82686-546">Po zakończeniu korzystania z dyskretnego kodu jQuery jest wygenerowany kod:</span><span class="sxs-lookup"><span data-stu-id="82686-546">After using Unobtrusive jQuery, the generated code is:</span></span>
   > 
   > [!code-html[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample25.html)]

* * *

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a><span data-ttu-id="82686-547">Podsumowanie</span><span class="sxs-lookup"><span data-stu-id="82686-547">Summary</span></span>

<span data-ttu-id="82686-548">Realizując ten warsztatów wiesz jak umożliwić użytkownikom zmienić dane przechowywane w bazie danych, korzystając z następujących czynności:</span><span class="sxs-lookup"><span data-stu-id="82686-548">By completing this Hands-On Lab you have learned how to enable users to change the data stored in the database with the use of the following:</span></span>

- <span data-ttu-id="82686-549">Indeks, tworzenia, edytowania, usuwania, takie jak akcji kontrolera</span><span class="sxs-lookup"><span data-stu-id="82686-549">Controller actions like Index, Create, Edit, Delete</span></span>
- <span data-ttu-id="82686-550">Funkcja tworzenia szkieletu ASP.NET MVC do wyświetlania właściwości tabeli HTML</span><span class="sxs-lookup"><span data-stu-id="82686-550">ASP.NET MVC's scaffolding feature for displaying properties in an HTML table</span></span>
- <span data-ttu-id="82686-551">Środowisko niestandardowych pomocników HTML w celu zwiększenia użytkownika</span><span class="sxs-lookup"><span data-stu-id="82686-551">Custom HTML helpers to improve user experience</span></span>
- <span data-ttu-id="82686-552">Metody akcji, które reagują na HTTP GET lub POST protokołu HTTP wywołania</span><span class="sxs-lookup"><span data-stu-id="82686-552">Action methods that react to either HTTP-GET or HTTP-POST calls</span></span>
- <span data-ttu-id="82686-553">Szablon udostępniony edytora podobne szablony widoku, takie jak tworzenie i edytowanie</span><span class="sxs-lookup"><span data-stu-id="82686-553">A shared editor template for similar View templates like Create and Edit</span></span>
- <span data-ttu-id="82686-554">Elementy formularza, takie jak listy rozwijane</span><span class="sxs-lookup"><span data-stu-id="82686-554">Form elements like drop-downs</span></span>
- <span data-ttu-id="82686-555">Adnotacje danych na potrzeby weryfikacji modelu</span><span class="sxs-lookup"><span data-stu-id="82686-555">Data annotations for Model validation</span></span>
- <span data-ttu-id="82686-556">Weryfikacja po stronie klienta przy użyciu dyskretnego kodu Biblioteka jQuery</span><span class="sxs-lookup"><span data-stu-id="82686-556">Client Side Validation using jQuery Unobtrusive library</span></span>

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a><span data-ttu-id="82686-557">Dodatek A: Instalowanie programu Visual Studio Express 2012 for Web</span><span class="sxs-lookup"><span data-stu-id="82686-557">Appendix A: Installing Visual Studio Express 2012 for Web</span></span>

<span data-ttu-id="82686-558">Możesz zainstalować **programu Microsoft Visual Studio Express 2012 for Web** lub inne &quot;Express&quot; przy użyciu wersji **[Instalatora platformy sieci Web firmy Microsoft](https://www.microsoft.com/web/downloads/platform.aspx)**.</span><span class="sxs-lookup"><span data-stu-id="82686-558">You can install **Microsoft Visual Studio Express 2012 for Web** or another &quot;Express&quot; version using the **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**.</span></span> <span data-ttu-id="82686-559">Poniższe wskazówki ułatwiają kroki wymagane do zainstalowania *programu Visual studio Express 2012 for Web* przy użyciu *Instalatora platformy sieci Web firmy Microsoft*.</span><span class="sxs-lookup"><span data-stu-id="82686-559">The following instructions guide you through the steps required to install *Visual studio Express 2012 for Web* using *Microsoft Web Platform Installer*.</span></span>

1. <span data-ttu-id="82686-560">Przejdź do [ [ https://go.microsoft.com/?linkid=9810169 ](https://go.microsoft.com/?linkid=9810169) ](https://go.microsoft.com/?linkid=9810169).</span><span class="sxs-lookup"><span data-stu-id="82686-560">Go to [[https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169).</span></span> <span data-ttu-id="82686-561">Alternatywnie, jeśli już zainstalowano Instalatora platformy sieci Web, możesz otworzyć go i Wyszukaj produkt &quot; <em>Visual Studio Express 2012 for Web z zestawem Windows Azure SDK</em>&quot;.</span><span class="sxs-lookup"><span data-stu-id="82686-561">Alternatively, if you already have installed Web Platform Installer, you can open it and search for the product &quot;<em>Visual Studio Express 2012 for Web with Windows Azure SDK</em>&quot;.</span></span>
2. <span data-ttu-id="82686-562">Kliknij pozycję **Zainstaluj teraz**.</span><span class="sxs-lookup"><span data-stu-id="82686-562">Click on **Install Now**.</span></span> <span data-ttu-id="82686-563">Jeśli nie masz **Instalatora platformy sieci Web** nastąpi przekierowanie do pobrania i zainstalowania go najpierw.</span><span class="sxs-lookup"><span data-stu-id="82686-563">If you do not have **Web Platform Installer** you will be redirected to download and install it first.</span></span>
3. <span data-ttu-id="82686-564">Raz **Instalatora platformy sieci Web** jest otwarty, kliknij przycisk **zainstalować** można uruchomić Instalatora.</span><span class="sxs-lookup"><span data-stu-id="82686-564">Once **Web Platform Installer** is open, click **Install** to start the setup.</span></span>

    <span data-ttu-id="82686-565">![Zainstaluj program Visual Studio Express](aspnet-mvc-4-helpers-forms-and-validation/_static/image22.png "zainstalować program Visual Studio Express")</span><span class="sxs-lookup"><span data-stu-id="82686-565">![Install Visual Studio Express](aspnet-mvc-4-helpers-forms-and-validation/_static/image22.png "Install Visual Studio Express")</span></span>

    <span data-ttu-id="82686-566">*Zainstaluj program Visual Studio Express*</span><span class="sxs-lookup"><span data-stu-id="82686-566">*Install Visual Studio Express*</span></span>
4. <span data-ttu-id="82686-567">Odczytywanie wszystkich produktów licencji oraz warunki, a następnie kliknij przycisk **akceptuję** aby kontynuować.</span><span class="sxs-lookup"><span data-stu-id="82686-567">Read all the products' licenses and terms and click **I Accept** to continue.</span></span>

    ![Akceptowanie umowy licencyjnej](aspnet-mvc-4-helpers-forms-and-validation/_static/image23.png)

    <span data-ttu-id="82686-569">*Akceptowanie umowy licencyjnej*</span><span class="sxs-lookup"><span data-stu-id="82686-569">*Accepting the license terms*</span></span>
5. <span data-ttu-id="82686-570">Zaczekaj, aż do zakończenia procesu pobierania i instalacji.</span><span class="sxs-lookup"><span data-stu-id="82686-570">Wait until the downloading and installation process completes.</span></span>

    ![Postęp instalacji](aspnet-mvc-4-helpers-forms-and-validation/_static/image24.png)

    <span data-ttu-id="82686-572">*Postęp instalacji*</span><span class="sxs-lookup"><span data-stu-id="82686-572">*Installation progress*</span></span>
6. <span data-ttu-id="82686-573">Po zakończeniu instalacji kliknij przycisk **Zakończ**.</span><span class="sxs-lookup"><span data-stu-id="82686-573">When the installation completes, click **Finish**.</span></span>

    ![Instalacja została zakończona](aspnet-mvc-4-helpers-forms-and-validation/_static/image25.png)

    <span data-ttu-id="82686-575">*Instalacja została zakończona*</span><span class="sxs-lookup"><span data-stu-id="82686-575">*Installation completed*</span></span>
7. <span data-ttu-id="82686-576">Kliknij przycisk **zakończenia** zamknąć Instalatora platformy sieci Web.</span><span class="sxs-lookup"><span data-stu-id="82686-576">Click **Exit** to close Web Platform Installer.</span></span>
8. <span data-ttu-id="82686-577">Aby otworzyć program Visual Studio Express for Web, przejdź do **Start** ekranu, a następnie zacznij pisać &quot; **VS Express**&quot;, następnie kliknij pozycję **VS Express for Web** Kafelek.</span><span class="sxs-lookup"><span data-stu-id="82686-577">To open Visual Studio Express for Web, go to the **Start** screen and start writing &quot;**VS Express**&quot;, then click on the **VS Express for Web** tile.</span></span>

    ![VS Express for Web kafelka](aspnet-mvc-4-helpers-forms-and-validation/_static/image26.png)

    <span data-ttu-id="82686-579">*VS Express for Web kafelka*</span><span class="sxs-lookup"><span data-stu-id="82686-579">*VS Express for Web tile*</span></span>

<a id="AppendixB"></a>

<a id="Appendix_B_Using_Code_Snippets"></a>
## <a name="appendix-b-using-code-snippets"></a><span data-ttu-id="82686-580">Dodatek B: korzystania z fragmentów kodu</span><span class="sxs-lookup"><span data-stu-id="82686-580">Appendix B: Using Code Snippets</span></span>

<span data-ttu-id="82686-581">Za pomocą wstawek kodu masz kod, czego potrzebujesz w zasięgu ręki.</span><span class="sxs-lookup"><span data-stu-id="82686-581">With code snippets, you have all the code you need at your fingertips.</span></span> <span data-ttu-id="82686-582">Dokument laboratorium informuje o dokładnie podczas korzystania z nich, jak pokazano na poniższej ilustracji.</span><span class="sxs-lookup"><span data-stu-id="82686-582">The lab document will tell you exactly when you can use them, as shown in the following figure.</span></span>

<span data-ttu-id="82686-583">![Za pomocą wstawek kodu programu Visual Studio, aby wstawić kod do projektu](aspnet-mvc-4-helpers-forms-and-validation/_static/image27.png "fragmenty kodu za pomocą programu Visual Studio, aby wstawić kod do projektu")</span><span class="sxs-lookup"><span data-stu-id="82686-583">![Using Visual Studio code snippets to insert code into your project](aspnet-mvc-4-helpers-forms-and-validation/_static/image27.png "Using Visual Studio code snippets to insert code into your project")</span></span>

<span data-ttu-id="82686-584">*Za pomocą wstawek kodu programu Visual Studio, aby wstawić kod do projektu*</span><span class="sxs-lookup"><span data-stu-id="82686-584">*Using Visual Studio code snippets to insert code into your project*</span></span>

<span data-ttu-id="82686-585">***Aby dodać wstawki kodu za pomocą klawiatury (tylko język C#)***</span><span class="sxs-lookup"><span data-stu-id="82686-585">***To add a code snippet using the keyboard (C# only)***</span></span>

1. <span data-ttu-id="82686-586">Umieść kursor, w którym chcesz wstawić kod.</span><span class="sxs-lookup"><span data-stu-id="82686-586">Place the cursor where you would like to insert the code.</span></span>
2. <span data-ttu-id="82686-587">Rozpocznij wpisywanie nazwy fragmentu kodu (bez spacji i łączniki).</span><span class="sxs-lookup"><span data-stu-id="82686-587">Start typing the snippet name (without spaces or hyphens).</span></span>
3. <span data-ttu-id="82686-588">Obejrzyj, jak technologia IntelliSense wyświetla zgodne z nazwami fragmenty kodu.</span><span class="sxs-lookup"><span data-stu-id="82686-588">Watch as IntelliSense displays matching snippets' names.</span></span>
4. <span data-ttu-id="82686-589">Wybierz prawidłowy fragment kodu lub pisz dalej do momentu wybrania debugowanej nazwy całego fragmentu kodu.</span><span class="sxs-lookup"><span data-stu-id="82686-589">Select the correct snippet (or keep typing until the entire snippet's name is selected).</span></span>
5. <span data-ttu-id="82686-590">Naciśnij klawisz Tab dwa razy, aby wstawić fragment kodu w lokalizacji kursora.</span><span class="sxs-lookup"><span data-stu-id="82686-590">Press the Tab key twice to insert the snippet at the cursor location.</span></span>

<span data-ttu-id="82686-591">![Rozpocznij wpisywanie nazwy fragmentu kodu](aspnet-mvc-4-helpers-forms-and-validation/_static/image28.png "Rozpocznij wpisywanie nazwy fragmentu kodu")</span><span class="sxs-lookup"><span data-stu-id="82686-591">![Start typing the snippet name](aspnet-mvc-4-helpers-forms-and-validation/_static/image28.png "Start typing the snippet name")</span></span>

<span data-ttu-id="82686-592">*Rozpocznij wpisywanie nazwy fragmentu kodu*</span><span class="sxs-lookup"><span data-stu-id="82686-592">*Start typing the snippet name*</span></span>

<span data-ttu-id="82686-593">![Naciśnij klawisz Tab, aby zaznaczyć fragment wyróżnione](aspnet-mvc-4-helpers-forms-and-validation/_static/image29.png "naciśnij klawisz Tab, aby wybrać wyróżnione fragmentu kodu")</span><span class="sxs-lookup"><span data-stu-id="82686-593">![Press Tab to select the highlighted snippet](aspnet-mvc-4-helpers-forms-and-validation/_static/image29.png "Press Tab to select the highlighted snippet")</span></span>

<span data-ttu-id="82686-594">*Naciśnij klawisz Tab, aby wybrać wyróżnione fragment kodu*</span><span class="sxs-lookup"><span data-stu-id="82686-594">*Press Tab to select the highlighted snippet*</span></span>

<span data-ttu-id="82686-595">![Ponownie naciśnij klawisz Tab i fragment kodu zostanie rozwinięte](aspnet-mvc-4-helpers-forms-and-validation/_static/image30.png "rozwinie ponownie naciśnij klawisz Tab i fragment kodu")</span><span class="sxs-lookup"><span data-stu-id="82686-595">![Press Tab again and the snippet will expand](aspnet-mvc-4-helpers-forms-and-validation/_static/image30.png "Press Tab again and the snippet will expand")</span></span>

<span data-ttu-id="82686-596">*Ponownie naciśnij klawisz Tab i fragment kodu zostanie rozwinięte.*</span><span class="sxs-lookup"><span data-stu-id="82686-596">*Press Tab again and the snippet will expand*</span></span>

<span data-ttu-id="82686-597">***Aby dodać wstawki kodu za pomocą myszy (C#, Visual Basic i XML)*** 1.</span><span class="sxs-lookup"><span data-stu-id="82686-597">***To add a code snippet using the mouse (C#, Visual Basic and XML)*** 1.</span></span> <span data-ttu-id="82686-598">Kliknij prawym przyciskiem myszy, gdy chcesz wstawić fragment kodu.</span><span class="sxs-lookup"><span data-stu-id="82686-598">Right-click where you want to insert the code snippet.</span></span>

1. <span data-ttu-id="82686-599">Wybierz **Wstaw fragment kodu** następuje **Moje fragmenty kodu**.</span><span class="sxs-lookup"><span data-stu-id="82686-599">Select **Insert Snippet** followed by **My Code Snippets**.</span></span>
2. <span data-ttu-id="82686-600">Wybierz odpowiedni fragment z listy, klikając go.</span><span class="sxs-lookup"><span data-stu-id="82686-600">Pick the relevant snippet from the list, by clicking on it.</span></span>

<span data-ttu-id="82686-601">![Kliknij prawym przyciskiem myszy, które chcesz wstawić fragment kodu i wybierz opcję Wstaw fragment kodu](aspnet-mvc-4-helpers-forms-and-validation/_static/image31.png "kliknij prawym przyciskiem myszy, które chcesz wstawić fragment kodu i wybierz opcję Wstaw fragment kodu")</span><span class="sxs-lookup"><span data-stu-id="82686-601">![Right-click where you want to insert the code snippet and select Insert Snippet](aspnet-mvc-4-helpers-forms-and-validation/_static/image31.png "Right-click where you want to insert the code snippet and select Insert Snippet")</span></span>

<span data-ttu-id="82686-602">*Kliknij prawym przyciskiem myszy, które chcesz wstawić fragment kodu i wybierz opcję Wstaw fragment kodu*</span><span class="sxs-lookup"><span data-stu-id="82686-602">*Right-click where you want to insert the code snippet and select Insert Snippet*</span></span>

<span data-ttu-id="82686-603">![Wybierz odpowiedni fragment z listy, klikając go](aspnet-mvc-4-helpers-forms-and-validation/_static/image32.png "wybierz odpowiedni fragment z listy, klikając go")</span><span class="sxs-lookup"><span data-stu-id="82686-603">![Pick the relevant snippet from the list, by clicking on it](aspnet-mvc-4-helpers-forms-and-validation/_static/image32.png "Pick the relevant snippet from the list, by clicking on it")</span></span>

<span data-ttu-id="82686-604">*Wybierz odpowiedni fragment z listy, klikając go*</span><span class="sxs-lookup"><span data-stu-id="82686-604">*Pick the relevant snippet from the list, by clicking on it*</span></span>
