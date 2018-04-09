---
uid: mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-entity-framework-scaffolding-and-migrations
title: Platforma ASP.NET MVC 4 Entity Framework rusztowania i migracje | Dokumentacja firmy Microsoft
author: rick-anderson
description: Jeśli znasz metod kontrolera ASP.NET MVC 4 lub została ukończona &quot;pomocników, formularzy i sprawdzania poprawności&quot; laboratorium praktycznego, należy zwrócić uwagę...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2013
ms.topic: article
ms.assetid: 093c1362-f10b-407c-a708-be370f4b62b0
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-entity-framework-scaffolding-and-migrations
msc.type: authoredcontent
ms.openlocfilehash: 548afe1926eed49841251832d54dc213da0cb753
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
---
# <a name="aspnet-mvc-4-entity-framework-scaffolding-and-migrations"></a><span data-ttu-id="7b894-103">Platforma ASP.NET MVC 4 Entity Framework rusztowania i migracji</span><span class="sxs-lookup"><span data-stu-id="7b894-103">ASP.NET MVC 4 Entity Framework Scaffolding and Migrations</span></span>

<span data-ttu-id="7b894-104">Przez [obozów sieci Web Team](https://twitter.com/webcamps)</span><span class="sxs-lookup"><span data-stu-id="7b894-104">By [Web Camps Team](https://twitter.com/webcamps)</span></span>

[<span data-ttu-id="7b894-105">Pobierz obozów sieci Web uczenie Kit</span><span class="sxs-lookup"><span data-stu-id="7b894-105">Download Web Camps Training Kit</span></span>](https://aka.ms/webcamps-training-kit)

<span data-ttu-id="7b894-106">Jeśli znasz metod kontrolera ASP.NET MVC 4 lub została ukończona &quot;pomocników, formularzy i sprawdzania poprawności&quot; laboratorium praktycznego, należy pamiętać, że wiele logiki do tworzenia, aktualizacji, listy i usuń wszelkie dane powtarza się wśród aplikacji.</span><span class="sxs-lookup"><span data-stu-id="7b894-106">If you are familiar with ASP.NET MVC 4 controller methods, or have completed the &quot;Helpers, Forms and Validation&quot; Hands-On lab, you should be aware that many of the logic to create, update, list and remove any data entity it is repeated among the application.</span></span> <span data-ttu-id="7b894-107">Aby nie mówią, że, jeśli model ma kilka klas do manipulowania, będzie prawdopodobnie poświęcić przez dłuższy czas zapisywania metod akcji POST i GET dla każdej operacji jednostki, a także wszystkich widoków.</span><span class="sxs-lookup"><span data-stu-id="7b894-107">Not to mention that, if your model has several classes to manipulate, you will be likely to spend a considerable time writing the POST and GET action methods for each entity operation, as well as each of the views.</span></span>

<span data-ttu-id="7b894-108">W tym laboratorium dowiesz się, jak używać szkieletów ASP.NET MVC 4 mają być automatycznie generowane linia bazowa CRUD aplikacji (tworzenia, odczytu, aktualizacji i usuwania).</span><span class="sxs-lookup"><span data-stu-id="7b894-108">In this lab you will learn how to use the ASP.NET MVC 4 scaffolding to automatically generate the baseline of your application's CRUD (Create, Read, Update and Delete).</span></span> <span data-ttu-id="7b894-109">Uruchamianie z klasy modelu prostego i bez napisania jakiegokolwiek wiersza kodu, utworzysz kontroler, który będzie zawierać wszystkich operacji CRUD, a także wszystkie widoki niezbędne.</span><span class="sxs-lookup"><span data-stu-id="7b894-109">Starting from a simple model class, and, without writing a single line of code, you will create a controller that will contain all the CRUD operations, as well as the all the necessary views.</span></span> <span data-ttu-id="7b894-110">Po tworzenia i uruchamiania prostym rozwiązaniem, konieczne będzie generowany wraz z logikę MVC i widoków do manipulowania danymi bazy danych aplikacji.</span><span class="sxs-lookup"><span data-stu-id="7b894-110">After building and running the simple solution, you will have the application database generated, together with the MVC logic and views for data manipulation.</span></span>

<span data-ttu-id="7b894-111">Ponadto dowiesz się, jak łatwo jest Użyj Entity Framework migracji do przeprowadzania aktualizacji modelu w całej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="7b894-111">In addition, you will learn how easy it is to use Entity Framework Migrations to perform model updates throughout your entire application.</span></span> <span data-ttu-id="7b894-112">Entity Framework migracji będzie można zmodyfikować bazy danych po zmianie modelu prostych kroków.</span><span class="sxs-lookup"><span data-stu-id="7b894-112">Entity Framework Migrations will let you modify your database after the model has changed with simple steps.</span></span> <span data-ttu-id="7b894-113">Wszystkie te na uwadze można do tworzenia i obsługi aplikacji sieci web wydajniej, korzystając z najnowszych funkcji platformy ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="7b894-113">With all these in mind, you will be able to build and maintain web applications more efficiently, taking advantage of the latest features of ASP.NET MVC 4.</span></span>

> [!NOTE]
> <span data-ttu-id="7b894-114">Wszystkie przykładowy kod i fragmenty kodu są uwzględnione w sieci Web obozów zestaw szkoleniowy, pod [wersje Microsoft-Web/WebCampTrainingKit](https://aka.ms/webcamps-training-kit).</span><span class="sxs-lookup"><span data-stu-id="7b894-114">All sample code and snippets are included in the Web Camps Training Kit, available from at [Microsoft-Web/WebCampTrainingKit Releases](https://aka.ms/webcamps-training-kit).</span></span> <span data-ttu-id="7b894-115">Specyficzne dla tego laboratorium projektu jest dostępna na [migracja i ASP.NET MVC 4 Entity Framework szkieletów](https://github.com/Microsoft-Web/HOL-EntityFrameworkScaffoldingAndMigrations).</span><span class="sxs-lookup"><span data-stu-id="7b894-115">The project specific to this lab is available at [ASP.NET MVC 4 Entity Framework Scaffolding and Migrations](https://github.com/Microsoft-Web/HOL-EntityFrameworkScaffoldingAndMigrations).</span></span>

<a id="Objectives"></a>
### <a name="objectives"></a><span data-ttu-id="7b894-116">Cele</span><span class="sxs-lookup"><span data-stu-id="7b894-116">Objectives</span></span>

<span data-ttu-id="7b894-117">W tym laboratorium Hands-On przedstawiono sposób:</span><span class="sxs-lookup"><span data-stu-id="7b894-117">In this Hands-On Lab, you will learn how to:</span></span>

- <span data-ttu-id="7b894-118">Użyj szkieletów ASP.NET dla operacji CRUD w kontrolerów.</span><span class="sxs-lookup"><span data-stu-id="7b894-118">Use ASP.NET scaffolding for CRUD operations in controllers.</span></span>
- <span data-ttu-id="7b894-119">Zmień model bazy danych przy użyciu Entity Framework migracji.</span><span class="sxs-lookup"><span data-stu-id="7b894-119">Change the database model using Entity Framework Migrations.</span></span>

<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a><span data-ttu-id="7b894-120">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="7b894-120">Prerequisites</span></span>

<span data-ttu-id="7b894-121">Musi mieć następujące elementy do przygotowania tego laboratorium:</span><span class="sxs-lookup"><span data-stu-id="7b894-121">You must have the following items to complete this lab:</span></span>

- <span data-ttu-id="7b894-122">[Microsoft Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) lub wyższego poziomu (odczytu [dodatek a.](#AppendixA) instrukcje dotyczące sposobu jego instalacji).</span><span class="sxs-lookup"><span data-stu-id="7b894-122">[Microsoft Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) or superior (read [Appendix A](#AppendixA) for instructions on how to install it).</span></span>

<a id="Setup"></a>

<a id="Setup"></a>
### <a name="setup"></a><span data-ttu-id="7b894-123">Konfiguracja</span><span class="sxs-lookup"><span data-stu-id="7b894-123">Setup</span></span>

<span data-ttu-id="7b894-124">**Instalowanie wstawki kodu**</span><span class="sxs-lookup"><span data-stu-id="7b894-124">**Installing Code Snippets**</span></span>

<span data-ttu-id="7b894-125">Dla wygody taki kod, który będzie zarządzany wzdłuż tego laboratorium jest dostępna jako wstawki kodu programu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="7b894-125">For convenience, much of the code you will be managing along this lab is available as Visual Studio code snippets.</span></span> <span data-ttu-id="7b894-126">Aby zainstalować fragmenty kodu, uruchom **.\Source\Setup\CodeSnippets.vsi** pliku.</span><span class="sxs-lookup"><span data-stu-id="7b894-126">To install the code snippets run **.\Source\Setup\CodeSnippets.vsi** file.</span></span>

<span data-ttu-id="7b894-127">Jeśli nie masz doświadczenia z wstawki programu Visual Studio i chcesz dowiedzieć się, jak ich używać, można odwołać się do dodatku z tego dokumentu &quot; [wstawki kodu za pomocą programu dodatek B:](#AppendixB)&quot;.</span><span class="sxs-lookup"><span data-stu-id="7b894-127">If you are not familiar with the Visual Studio Code Snippets, and want to learn how to use them, you can refer to the appendix from this document &quot;[Appendix B: Using Code Snippets](#AppendixB)&quot;.</span></span>

* * *

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a><span data-ttu-id="7b894-128">Ćwiczenia</span><span class="sxs-lookup"><span data-stu-id="7b894-128">Exercises</span></span>

<span data-ttu-id="7b894-129">Poniższym ćwiczeniu tworzą tego laboratorium Hands-On:</span><span class="sxs-lookup"><span data-stu-id="7b894-129">The following exercise make up this Hands-On Lab:</span></span>

1. [<span data-ttu-id="7b894-130">Przy użyciu funkcji szkieletów platformy ASP.NET MVC 4 z Entity Framework migracji</span><span class="sxs-lookup"><span data-stu-id="7b894-130">Using ASP.NET MVC 4 Scaffolding with Entity Framework Migrations</span></span>](#Exercise1)

> [!NOTE]
> <span data-ttu-id="7b894-131">Towarzyszy tego ćwiczenia **zakończenia** folderu zawierającego wynikowy rozwiązanie, należy uzyskać po zakończeniu wykonywania.</span><span class="sxs-lookup"><span data-stu-id="7b894-131">This exercise is accompanied by an **End** folder containing the resulting solution you should obtain after completing the exercise.</span></span> <span data-ttu-id="7b894-132">Jeśli potrzebujesz dodatkowej pomocy, Praca do wykonywania, można użyć tego rozwiązania jako przewodnika.</span><span class="sxs-lookup"><span data-stu-id="7b894-132">You can use this solution as a guide if you need additional help working through the exercise.</span></span>


<span data-ttu-id="7b894-133">Szacowany czas trwania tego laboratorium: **30 minut**</span><span class="sxs-lookup"><span data-stu-id="7b894-133">Estimated time to complete this lab: **30 minutes**</span></span>

<a id="Exercise1"></a>

<a id="Exercise_1_Using_ASPNET_MVC_4_Scaffolding_with_Entity_Framework_Migrations"></a>
### <a name="exercise-1-using-aspnet-mvc-4-scaffolding-with-entity-framework-migrations"></a><span data-ttu-id="7b894-134">Ćwiczenie 1: Przy użyciu funkcji szkieletów platformy ASP.NET MVC 4 z Entity Framework migracji</span><span class="sxs-lookup"><span data-stu-id="7b894-134">Exercise 1: Using ASP.NET MVC 4 Scaffolding with Entity Framework Migrations</span></span>

<span data-ttu-id="7b894-135">Szkieletów MVC ASP.NET zapewnia możliwość szybkiego generowania operacji CRUD w sposób Zestandaryzowany tworzenia logiki niezbędne, które umożliwia interakcję z warstwy bazy danych aplikacji.</span><span class="sxs-lookup"><span data-stu-id="7b894-135">ASP.NET MVC scaffolding provides a quick way to generate the CRUD operations in a standardized way, creating the necessary logic that lets your application interact with the database layer.</span></span>

<span data-ttu-id="7b894-136">W tym ćwiczeniu dowiesz się, jak używać szkieletów ASP.NET MVC 4 z kodem najpierw utworzyć metody CRUD.</span><span class="sxs-lookup"><span data-stu-id="7b894-136">In this exercise, you will learn how to use ASP.NET MVC 4 scaffolding with code first to create the CRUD methods.</span></span> <span data-ttu-id="7b894-137">Następnie dowiesz jak zaktualizować model zastosowania zmian w bazie danych przy użyciu Entity Framework migracji.</span><span class="sxs-lookup"><span data-stu-id="7b894-137">Then, you will learn how to update your model applying the changes in the database by using Entity Framework Migrations.</span></span>

<a id="Ex1Task1"></a>

<a id="Task_1-_Creating_a_new_ASPNET_MVC_4_project_using_Scaffolding"></a>
#### <a name="task-1--creating-a-new-aspnet-mvc-4-project-using-scaffolding"></a><span data-ttu-id="7b894-138">Zadanie 1 — Tworzenie nowej technologii ASP.NET MVC 4 projektu przy użyciu funkcji szkieletów</span><span class="sxs-lookup"><span data-stu-id="7b894-138">Task 1- Creating a new ASP.NET MVC 4 project using Scaffolding</span></span>

1. <span data-ttu-id="7b894-139">Jeśli nie już otwarty, uruchom **programu Visual Studio 2012**.</span><span class="sxs-lookup"><span data-stu-id="7b894-139">If not already open, start **Visual Studio 2012**.</span></span>
2. <span data-ttu-id="7b894-140">Wybierz **pliku | Nowy projekt**.</span><span class="sxs-lookup"><span data-stu-id="7b894-140">Select **File | New Project**.</span></span> <span data-ttu-id="7b894-141">W nowy projekt okna dialogowego, w obszarze **Visual C# | Web** zaznacz **aplikacji sieci Web programu ASP.NET MVC 4**.</span><span class="sxs-lookup"><span data-stu-id="7b894-141">In the New Project dialog, under the **Visual C# | Web** section, select **ASP.NET MVC 4 Web Application**.</span></span> <span data-ttu-id="7b894-142">Nazwij projekt do **MVC4andEFMigrations** i jako jego lokalizację ustaw **Source\Ex1 UsingMVC4ScaffoldingEFMigrations** folder tego laboratorium.</span><span class="sxs-lookup"><span data-stu-id="7b894-142">Name the project to **MVC4andEFMigrations** and set the location to **Source\Ex1-UsingMVC4ScaffoldingEFMigrations** folder of this lab.</span></span> <span data-ttu-id="7b894-143">Ustaw **Nazwa rozwiązania** do **rozpocząć** i upewnij się, **Utwórz katalog rozwiązania** jest zaznaczony.</span><span class="sxs-lookup"><span data-stu-id="7b894-143">Set the **Solution name** to **Begin** and ensure **Create directory for solution** is checked.</span></span> <span data-ttu-id="7b894-144">Kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="7b894-144">Click **OK**.</span></span>

    <span data-ttu-id="7b894-145">![Okno dialogowe nowego projektu platformy ASP.NET MVC 4](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image1.png "okno dialogowe Nowy projekt ASP.NET MVC 4")</span><span class="sxs-lookup"><span data-stu-id="7b894-145">![New ASP.NET MVC 4 Project Dialog Box](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image1.png "New ASP.NET MVC 4 Project Dialog Box")</span></span>

    <span data-ttu-id="7b894-146">*Okno dialogowe nowego projektu platformy ASP.NET MVC 4*</span><span class="sxs-lookup"><span data-stu-id="7b894-146">*New ASP.NET MVC 4 Project Dialog Box*</span></span>
3. <span data-ttu-id="7b894-147">W **nowy projekt programu ASP.NET MVC 4** wybierz okno dialogowe **aplikacji internetowej** szablonu i upewnij się, że **Razor** jest wybrane **aparat widoku**.</span><span class="sxs-lookup"><span data-stu-id="7b894-147">In the **New ASP.NET MVC 4 Project** dialog box select the **Internet Application** template, and make sure that **Razor** is the selected **View engine**.</span></span> <span data-ttu-id="7b894-148">Kliknij przycisk **OK** Aby utworzyć projekt.</span><span class="sxs-lookup"><span data-stu-id="7b894-148">Click **OK** to create the project.</span></span>

    <span data-ttu-id="7b894-149">![Nowej aplikacji internetowej platformy ASP.NET MVC 4](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image2.png "nowej aplikacji internetowej platformy ASP.NET MVC 4")</span><span class="sxs-lookup"><span data-stu-id="7b894-149">![New ASP.NET MVC 4 Internet Application](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image2.png "New ASP.NET MVC 4 Internet Application")</span></span>

    <span data-ttu-id="7b894-150">*Nowej aplikacji internetowej platformy ASP.NET MVC 4*</span><span class="sxs-lookup"><span data-stu-id="7b894-150">*New ASP.NET MVC 4 Internet Application*</span></span>
4. <span data-ttu-id="7b894-151">W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy **modele** i wybierz **Dodaj | Klasa** utworzyć prostą klasę osoby (POCO).</span><span class="sxs-lookup"><span data-stu-id="7b894-151">In the Solution Explorer, right-click **Models** and select **Add | Class** to create a simple class person (POCO).</span></span> <span data-ttu-id="7b894-152">Nadaj mu nazwę **osoby** i kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="7b894-152">Name it **Person** and click **OK**.</span></span>
5. <span data-ttu-id="7b894-153">Otwórz klasy osoby i Wstaw następujące właściwości.</span><span class="sxs-lookup"><span data-stu-id="7b894-153">Open the Person class and insert the following properties.</span></span>

    <span data-ttu-id="7b894-154">(Fragment - kodu *platformy ASP.NET MVC 4 oraz Entity Framework migracje - właściwości osoby Ex1*)</span><span class="sxs-lookup"><span data-stu-id="7b894-154">(Code Snippet - *ASP.NET MVC 4 and Entity Framework Migrations - Ex1 Person Properties*)</span></span>


~~~
[!code-csharp[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample1.cs)]
~~~
6. <span data-ttu-id="7b894-155">Kliknij przycisk **kompilacji | Tworzenie rozwiązania** Aby zapisać zmiany i skompilować projekt.</span><span class="sxs-lookup"><span data-stu-id="7b894-155">Click **Build | Build Solution** to save the changes and build the project.</span></span>

    <span data-ttu-id="7b894-156">![Tworzenie aplikacji](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image3.png "tworzenie aplikacji")</span><span class="sxs-lookup"><span data-stu-id="7b894-156">![Building the application](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image3.png "Building the application")</span></span>

    <span data-ttu-id="7b894-157">*Tworzenie aplikacji*</span><span class="sxs-lookup"><span data-stu-id="7b894-157">*Building the Application*</span></span>
7. <span data-ttu-id="7b894-158">W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy folder kontrolery, a następnie wybierz **Dodaj | Kontroler**.</span><span class="sxs-lookup"><span data-stu-id="7b894-158">In the Solution Explorer, right-click the controllers folder and select **Add | Controller**.</span></span>
8. <span data-ttu-id="7b894-159">Nazwa kontrolera *PersonController* i ukończyć **opcje szkieletów** z następującymi wartościami.</span><span class="sxs-lookup"><span data-stu-id="7b894-159">Name the controller *PersonController* and complete the **Scaffolding options** with the following values.</span></span>

   1. <span data-ttu-id="7b894-160">W **szablonu** listy rozwijanej wybierz **kontroler MVC z akcjami odczytu/zapisu i widokami używający narzędzia Entity Framework** opcji.</span><span class="sxs-lookup"><span data-stu-id="7b894-160">In the **Template** drop-down list, select the **MVC controller with read/write actions and views, using Entity Framework** option.</span></span>
   2. <span data-ttu-id="7b894-161">W **klasa modelu** listy rozwijanej wybierz **osoby** klasy.</span><span class="sxs-lookup"><span data-stu-id="7b894-161">In the **Model class** drop-down list, select the **Person** class.</span></span>
   3. <span data-ttu-id="7b894-162">W **klasy kontekstu danych** listy, wybierz  **&lt;nowy kontekst danych... &gt;**.</span><span class="sxs-lookup"><span data-stu-id="7b894-162">In the **Data Context class** list, select **&lt;New data context...&gt;**.</span></span> <span data-ttu-id="7b894-163">Wybierz dowolną nazwę, a następnie kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="7b894-163">Choose any name and click **OK**.</span></span>
   4. <span data-ttu-id="7b894-164">W **widoków** rozwijania listy, upewnij się, że **Razor** jest zaznaczone.</span><span class="sxs-lookup"><span data-stu-id="7b894-164">In the **Views** drop-down list, make sure that **Razor** is selected.</span></span>

      <span data-ttu-id="7b894-165">![Dodawanie kontrolera osoby z szkieletów](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image4.png "dodawania kontrolera osoby z szkieletów")</span><span class="sxs-lookup"><span data-stu-id="7b894-165">![Adding the Person controller with scaffolding](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image4.png "Adding the Person controller with scaffolding")</span></span>

      <span data-ttu-id="7b894-166">*Dodawanie kontrolera osoby z szkieletów*</span><span class="sxs-lookup"><span data-stu-id="7b894-166">*Adding the Person controller with scaffolding*</span></span>
9. <span data-ttu-id="7b894-167">Kliknij przycisk **Dodaj** do utworzenia nowego kontrolera dla osoby z szkieletów.</span><span class="sxs-lookup"><span data-stu-id="7b894-167">Click **Add** to create the new controller for Person with scaffolding.</span></span> <span data-ttu-id="7b894-168">Wygenerowane zostały akcji kontrolera, a także widoki.</span><span class="sxs-lookup"><span data-stu-id="7b894-168">You have now generated the controller actions as well as the views.</span></span>

    <span data-ttu-id="7b894-169">![Po utworzeniu kontrolera osoby za pomocą szkieletów](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image5.png "po utworzeniu kontrolera osoby za pomocą szkieletów")</span><span class="sxs-lookup"><span data-stu-id="7b894-169">![After creating the Person controller with scaffolding](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image5.png "After creating the Person controller with scaffolding")</span></span>

    <span data-ttu-id="7b894-170">*Po utworzeniu kontrolera osoby za pomocą szkieletów*</span><span class="sxs-lookup"><span data-stu-id="7b894-170">*After creating the Person controller with scaffolding*</span></span>
10. <span data-ttu-id="7b894-171">Otwórz **PersonController** klasy.</span><span class="sxs-lookup"><span data-stu-id="7b894-171">Open **PersonController** class.</span></span> <span data-ttu-id="7b894-172">Należy zauważyć, że pełna metod akcji CRUD został wygenerowany automatycznie.</span><span class="sxs-lookup"><span data-stu-id="7b894-172">Notice that the full CRUD action methods have been generated automatically.</span></span>

   <span data-ttu-id="7b894-173">![Wewnątrz kontrolera osoby](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image6.png "kontrolera wewnątrz osoby")</span><span class="sxs-lookup"><span data-stu-id="7b894-173">![Inside the Person controller](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image6.png "Inside the Person controller")</span></span>

   <span data-ttu-id="7b894-174">*Wewnątrz kontrolera osoby*</span><span class="sxs-lookup"><span data-stu-id="7b894-174">*Inside the Person controller*</span></span>

<a id="Ex1Task2"></a>

<a id="Task_2-_Running_the_application"></a>
#### <a name="task-2--running-the-application"></a><span data-ttu-id="7b894-175">Zadanie 2 — uruchamianie aplikacji</span><span class="sxs-lookup"><span data-stu-id="7b894-175">Task 2- Running the application</span></span>

<span data-ttu-id="7b894-176">W tym momencie bazy danych nie został jeszcze utworzony.</span><span class="sxs-lookup"><span data-stu-id="7b894-176">At this point, the database is not yet created.</span></span> <span data-ttu-id="7b894-177">W tym zadaniu zostanie Uruchom aplikację po raz pierwszy, a test operacji CRUD.</span><span class="sxs-lookup"><span data-stu-id="7b894-177">In this task, you will run the application for the first time and test the CRUD operations.</span></span> <span data-ttu-id="7b894-178">Bazy danych zostanie utworzona na bieżąco, Code First.</span><span class="sxs-lookup"><span data-stu-id="7b894-178">The database will be created on the fly with Code First.</span></span>

1. <span data-ttu-id="7b894-179">Naciśnij klawisz **F5** do uruchomienia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="7b894-179">Press **F5** to run the application.</span></span>
2. <span data-ttu-id="7b894-180">W przeglądarce, Dodaj **/Person** do adresu URL, aby otworzyć stronę osoby.</span><span class="sxs-lookup"><span data-stu-id="7b894-180">In the browser, add **/Person** to the URL to open the Person page.</span></span>

    <span data-ttu-id="7b894-181">![Najpierw uruchom aplikację](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image7.png "pierwszym uruchomieniu aplikacji")</span><span class="sxs-lookup"><span data-stu-id="7b894-181">![Application first run](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image7.png "Application first run")</span></span>

    <span data-ttu-id="7b894-182">*Aplikację: najpierw uruchom*</span><span class="sxs-lookup"><span data-stu-id="7b894-182">*Application: first run*</span></span>
3. <span data-ttu-id="7b894-183">Teraz zostanie Eksploruj stron osoby i przetestować operacji CRUD.</span><span class="sxs-lookup"><span data-stu-id="7b894-183">You will now explore the Person pages and test the CRUD operations.</span></span>

    1. <span data-ttu-id="7b894-184">Kliknij przycisk **Utwórz nowy** można dodać nowej osoby.</span><span class="sxs-lookup"><span data-stu-id="7b894-184">Click **Create New** to add a new person.</span></span> <span data-ttu-id="7b894-185">Wprowadź imię i nazwisko, a następnie kliknij przycisk **Utwórz**.</span><span class="sxs-lookup"><span data-stu-id="7b894-185">Enter a first name and a last name and click **Create**.</span></span>

        <span data-ttu-id="7b894-186">![Dodawanie nowej osoby](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image8.png "Dodawanie nowej osoby")</span><span class="sxs-lookup"><span data-stu-id="7b894-186">![Adding a new person](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image8.png "Adding a new person")</span></span>

        <span data-ttu-id="7b894-187">*Dodawanie nowej osoby*</span><span class="sxs-lookup"><span data-stu-id="7b894-187">*Adding a new person*</span></span>
    2. <span data-ttu-id="7b894-188">Na liście osoby możesz usunąć, Edytuj lub dodawania elementów.</span><span class="sxs-lookup"><span data-stu-id="7b894-188">In the person's list, you can delete, edit or add items.</span></span>

        <span data-ttu-id="7b894-189">![listy osób](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image9.png "listy osób")</span><span class="sxs-lookup"><span data-stu-id="7b894-189">![person list](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image9.png "person list")</span></span>

        <span data-ttu-id="7b894-190">*Listy osób*</span><span class="sxs-lookup"><span data-stu-id="7b894-190">*Person list*</span></span>
    3. <span data-ttu-id="7b894-191">Kliknij przycisk **szczegóły** można otworzyć szczegółów danej osoby.</span><span class="sxs-lookup"><span data-stu-id="7b894-191">Click **Details** to open the person's details.</span></span>

        <span data-ttu-id="7b894-192">![Szczegóły osoby](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image10.png "szczegóły osoby")</span><span class="sxs-lookup"><span data-stu-id="7b894-192">![Person's details](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image10.png "Person's details")</span></span>

        <span data-ttu-id="7b894-193">*Szczegóły osoby*</span><span class="sxs-lookup"><span data-stu-id="7b894-193">*Person's details*</span></span>
4. <span data-ttu-id="7b894-194">Zamknij przeglądarkę i powrócić do programu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="7b894-194">Close the browser and return to Visual Studio.</span></span> <span data-ttu-id="7b894-195">Zauważ, że utworzono całego CRUD dla obiekt osoby w całej aplikacji — model do widoków — bez konieczności pisania pojedynczy wiersz kodu!</span><span class="sxs-lookup"><span data-stu-id="7b894-195">Notice that you have created the whole CRUD for the person entity throughout your application -from the model to the views- without having to write a single line of code!</span></span>

<a id="Ex1Task3"></a>

<a id="Task_3-_Updating_the_database_using_Entity_Framework_Migrations"></a>
#### <a name="task-3--updating-the-database-using-entity-framework-migrations"></a><span data-ttu-id="7b894-196">Zadanie 3 — aktualizowanie bazy danych używającej Entity Framework migracji</span><span class="sxs-lookup"><span data-stu-id="7b894-196">Task 3- Updating the database using Entity Framework Migrations</span></span>

<span data-ttu-id="7b894-197">To zadanie zaktualizuje bazę danych przy użyciu Entity Framework migracji.</span><span class="sxs-lookup"><span data-stu-id="7b894-197">In this task you will update the database using Entity Framework Migrations.</span></span> <span data-ttu-id="7b894-198">Możesz zauważyć, jak łatwo jest można zmienić modelu na model i uwzględnia zmiany w bazach danych za pomocą funkcji Entity Framework migracji.</span><span class="sxs-lookup"><span data-stu-id="7b894-198">You will discover how easy it is to change the model and reflect the changes in your databases by using the Entity Framework Migrations feature.</span></span>

1. <span data-ttu-id="7b894-199">Otwórz konsolę Menedżera pakietów.</span><span class="sxs-lookup"><span data-stu-id="7b894-199">Open the Package Manager Console.</span></span> <span data-ttu-id="7b894-200">Wybierz **narzędzia | Menedżer pakietów biblioteki | Konsola Menedżera pakietów**.</span><span class="sxs-lookup"><span data-stu-id="7b894-200">Select **Tools | Library Package Manager | Package Manager Console**.</span></span>
2. <span data-ttu-id="7b894-201">W konsoli Menedżera pakietów wprowadź następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="7b894-201">In the Package Manager Console, enter the following command:</span></span>

    <span data-ttu-id="7b894-202">PMC</span><span class="sxs-lookup"><span data-stu-id="7b894-202">PMC</span></span>

    [!code-powershell[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample2.ps1)]

    <span data-ttu-id="7b894-203">![Włączanie migracje](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image11.png "włączenie migracji")</span><span class="sxs-lookup"><span data-stu-id="7b894-203">![Enabling Migrations](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image11.png "Enabling Migrations")</span></span>

    <span data-ttu-id="7b894-204">*Włączanie migracji*</span><span class="sxs-lookup"><span data-stu-id="7b894-204">*Enabling migrations*</span></span>

    <span data-ttu-id="7b894-205">Polecenie Enable-migracji tworzy **migracje** folder zawierający skrypt, aby zainicjować bazy danych.</span><span class="sxs-lookup"><span data-stu-id="7b894-205">The Enable-Migration command creates the **Migrations** folder, which contains a script to initialize the database.</span></span>

    <span data-ttu-id="7b894-206">![Migrations folder](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image12.png "Migrations folder")</span><span class="sxs-lookup"><span data-stu-id="7b894-206">![Migrations folder](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image12.png "Migrations folder")</span></span>

    <span data-ttu-id="7b894-207">*Migrations folder*</span><span class="sxs-lookup"><span data-stu-id="7b894-207">*Migrations folder*</span></span>
3. <span data-ttu-id="7b894-208">Otwórz **Configuration.cs** pliku w folderze migracji.</span><span class="sxs-lookup"><span data-stu-id="7b894-208">Open the **Configuration.cs** file in the Migrations folder.</span></span> <span data-ttu-id="7b894-209">Znajdź Konstruktor klasy i zmień **AutomaticMigrationsEnabled** do wartości *true*.</span><span class="sxs-lookup"><span data-stu-id="7b894-209">Locate the class constructor and change the **AutomaticMigrationsEnabled** value to *true*.</span></span>


~~~
[!code-csharp[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample3.cs)]
~~~
4. <span data-ttu-id="7b894-210">Otwórz klasy osoby i Dodaj atrybut drugie imię.</span><span class="sxs-lookup"><span data-stu-id="7b894-210">Open the Person class and add an attribute for the person's middle name.</span></span> <span data-ttu-id="7b894-211">Z tego nowego atrybutu zmieniasz modelu.</span><span class="sxs-lookup"><span data-stu-id="7b894-211">With this new attribute, you are changing the model.</span></span>


~~~
[!code-csharp[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample4.cs)]
~~~
5. <span data-ttu-id="7b894-212">Wybierz **kompilacji | Tworzenie rozwiązania** menu do skompilowania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="7b894-212">Select **Build | Build Solution** on the menu to build the application.</span></span>

    <span data-ttu-id="7b894-213">![Tworzenie aplikacji](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image13.png "tworzenie aplikacji")</span><span class="sxs-lookup"><span data-stu-id="7b894-213">![Building the application](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image13.png "Building the application")</span></span>

    <span data-ttu-id="7b894-214">*Tworzenie aplikacji*</span><span class="sxs-lookup"><span data-stu-id="7b894-214">*Building the application*</span></span>
6. <span data-ttu-id="7b894-215">W konsoli Menedżera pakietów wprowadź następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="7b894-215">In the Package Manager Console, enter the following command:</span></span>

    <span data-ttu-id="7b894-216">PMC</span><span class="sxs-lookup"><span data-stu-id="7b894-216">PMC</span></span>

    [!code-powershell[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample5.ps1)]

    <span data-ttu-id="7b894-217">To polecenie będzie wyglądać na zmiany w obiektach danych, a następnie dodaj niezbędne polecenia można zmodyfikować odpowiednio bazy danych.</span><span class="sxs-lookup"><span data-stu-id="7b894-217">This command will look for changes in the data objects, and then, it will add the necessary commands to modify the database accordingly.</span></span>

    <span data-ttu-id="7b894-218">![Dodawanie drugie imię](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image14.png "Dodawanie drugie imię")</span><span class="sxs-lookup"><span data-stu-id="7b894-218">![Adding a middle name](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image14.png "Adding a middle name")</span></span>

    <span data-ttu-id="7b894-219">*Dodawanie drugie imię*</span><span class="sxs-lookup"><span data-stu-id="7b894-219">*Adding a middle name*</span></span>
7. <span data-ttu-id="7b894-220">(Opcjonalnie) Można uruchomić następujące polecenie, aby wygenerować skryptu SQL z aktualizacji różnicowych.</span><span class="sxs-lookup"><span data-stu-id="7b894-220">(Optional) You can run the following command to generate a SQL script with the differential update.</span></span> <span data-ttu-id="7b894-221">Umożliwi to należy ręcznie zaktualizować bazę danych (w tym przypadku go nie jest to konieczne), lub Zastosuj zmiany w innych bazach danych:</span><span class="sxs-lookup"><span data-stu-id="7b894-221">This will let you update the database manually (In this case it's not necessary), or apply the changes in other databases:</span></span>

    <span data-ttu-id="7b894-222">PMC</span><span class="sxs-lookup"><span data-stu-id="7b894-222">PMC</span></span>

    [!code-powershell[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample6.ps1)]

    <span data-ttu-id="7b894-223">![Generowanie skryptu SQL](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image15.png "Generowanie skryptu SQL")</span><span class="sxs-lookup"><span data-stu-id="7b894-223">![Generating a SQL script](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image15.png "Generating a SQL script")</span></span>

    <span data-ttu-id="7b894-224">*Generowanie skryptu SQL*</span><span class="sxs-lookup"><span data-stu-id="7b894-224">*Generating a SQL script*</span></span>

    <span data-ttu-id="7b894-225">![Aktualizacja skryptu SQL](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image16.png "aktualizacji skryptu SQL")</span><span class="sxs-lookup"><span data-stu-id="7b894-225">![SQL Script update](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image16.png "SQL Script update")</span></span>

    <span data-ttu-id="7b894-226">*Aktualizacja skryptu SQL*</span><span class="sxs-lookup"><span data-stu-id="7b894-226">*SQL Script update*</span></span>
8. <span data-ttu-id="7b894-227">W konsoli Menedżera pakietów wprowadź następujące polecenie, aby zaktualizować bazę danych:</span><span class="sxs-lookup"><span data-stu-id="7b894-227">In the Package Manager Console, enter the following command to update the database:</span></span>

    <span data-ttu-id="7b894-228">PMC</span><span class="sxs-lookup"><span data-stu-id="7b894-228">PMC</span></span>

    [!code-powershell[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample7.ps1)]

    <span data-ttu-id="7b894-229">![Uaktualnienie bazy danych](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image17.png "aktualizowania bazy danych")</span><span class="sxs-lookup"><span data-stu-id="7b894-229">![Updating the Database](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image17.png "Updating the Database")</span></span>

    <span data-ttu-id="7b894-230">*Uaktualnienie bazy danych*</span><span class="sxs-lookup"><span data-stu-id="7b894-230">*Updating the Database*</span></span>

    <span data-ttu-id="7b894-231">Spowoduje to dodanie **MiddleName** kolumny w **osób** tabeli, aby dopasować bieżącej definicji **osoby** klasy.</span><span class="sxs-lookup"><span data-stu-id="7b894-231">This will add the **MiddleName** column in the **People** table to match the current definition of the **Person** class.</span></span>
9. <span data-ttu-id="7b894-232">Po zaktualizowaniu bazy danych, kliknij prawym przyciskiem myszy folder kontrolera i wybierz **Dodaj | Kontroler** do dodania osoby kontrolera ponownie (razem z takich samych wartości).</span><span class="sxs-lookup"><span data-stu-id="7b894-232">Once the database is updated, right-click the Controller folder and select **Add | Controller** to add the Person controller again (Complete with the same values).</span></span> <span data-ttu-id="7b894-233">Ta operacja spowoduje zaktualizowanie istniejących metod i widoki, dodanie nowego atrybutu.</span><span class="sxs-lookup"><span data-stu-id="7b894-233">This will update the existing methods and views adding the new attribute.</span></span>

    <span data-ttu-id="7b894-234">![Dodawanie aktualizacji kontrolera](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image18.png "Dodawanie aktualizacji kontrolera")</span><span class="sxs-lookup"><span data-stu-id="7b894-234">![Adding a controller update](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image18.png "Adding a controller update")</span></span>

    <span data-ttu-id="7b894-235">*Aktualizowanie kontrolera*</span><span class="sxs-lookup"><span data-stu-id="7b894-235">*Updating the controller*</span></span>
10. <span data-ttu-id="7b894-236">Kliknij przycisk **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="7b894-236">Click **Add**.</span></span> <span data-ttu-id="7b894-237">Następnie wybierz wartości **zastąpić PersonController.cs** i **Zastąp skojarzone widoków** i kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="7b894-237">Then, select the values **Overwrite PersonController.cs** and the **Overwrite associated views** and click **OK**.</span></span>

   ![Dodawanie Zastąp kontrolera](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image19.png)

   <span data-ttu-id="7b894-239">*Aktualizowanie kontrolera*</span><span class="sxs-lookup"><span data-stu-id="7b894-239">*Updating the controller*</span></span>

<a id="Ex1Task4"></a>

<a id="Task4-_Running_the_application"></a>
#### <a name="task4--running-the-application"></a><span data-ttu-id="7b894-240">Task4 - uruchamiania aplikacji</span><span class="sxs-lookup"><span data-stu-id="7b894-240">Task4- Running the application</span></span>

1. <span data-ttu-id="7b894-241">Naciśnij klawisz **F5** do uruchomienia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="7b894-241">Press **F5** to run the application.</span></span>
2. <span data-ttu-id="7b894-242">Otwórz **/Person**.</span><span class="sxs-lookup"><span data-stu-id="7b894-242">Open **/Person**.</span></span> <span data-ttu-id="7b894-243">Zwróć uwagę, że dane została zachowana, podczas gdy drugie imię kolumna została dodana.</span><span class="sxs-lookup"><span data-stu-id="7b894-243">Notice that the data was preserved, while the middle name column was added.</span></span>

    <span data-ttu-id="7b894-244">![Drugie imię dodane](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image20.png "drugie imię dodane")</span><span class="sxs-lookup"><span data-stu-id="7b894-244">![Middle Name added](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image20.png "Middle Name added")</span></span>

    <span data-ttu-id="7b894-245">*Drugie imię dodane*</span><span class="sxs-lookup"><span data-stu-id="7b894-245">*Middle Name added*</span></span>
3. <span data-ttu-id="7b894-246">Jeśli klikniesz przycisk **Edytuj**, można dodać drugie imię do bieżącej osoby.</span><span class="sxs-lookup"><span data-stu-id="7b894-246">If you click **Edit**, you will be able to add a middle name to the current person.</span></span>

    <span data-ttu-id="7b894-247">![Wydanie drugie imię](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image21.png "wydanie drugie imię")</span><span class="sxs-lookup"><span data-stu-id="7b894-247">![Middle Name edition](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image21.png "Middle Name edition")</span></span>

* * *

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a><span data-ttu-id="7b894-248">Podsumowanie</span><span class="sxs-lookup"><span data-stu-id="7b894-248">Summary</span></span>

<span data-ttu-id="7b894-249">W tym laboratorium praktycznego uzyskanych proste kroki umożliwiające utworzenie operacji CRUD z platformy ASP.NET MVC 4 rusztowania, używając dowolnej klasy modelu.</span><span class="sxs-lookup"><span data-stu-id="7b894-249">In this Hands-On lab, you have learned simple steps to create CRUD operations with ASP.NET MVC 4 Scaffolding using any model class.</span></span> <span data-ttu-id="7b894-250">Następnie uzyskanych przeprowadzania aktualizacji pełnego w aplikacji — z bazy danych do widoków — za pomocą Entity Framework migracji.</span><span class="sxs-lookup"><span data-stu-id="7b894-250">Then, you have learned how to perform an end to end update in your application -from the database to the views- by using Entity Framework Migrations.</span></span>

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a><span data-ttu-id="7b894-251">Dodatek A: Instalowanie programu Visual Studio Express 2012 for Web</span><span class="sxs-lookup"><span data-stu-id="7b894-251">Appendix A: Installing Visual Studio Express 2012 for Web</span></span>

<span data-ttu-id="7b894-252">Można zainstalować **Microsoft Visual Studio Express 2012 for Web** lub innym &quot;Express&quot; przy użyciu wersji **[Instalatora platformy sieci Web firmy Microsoft](https://www.microsoft.com/web/downloads/platform.aspx)**.</span><span class="sxs-lookup"><span data-stu-id="7b894-252">You can install **Microsoft Visual Studio Express 2012 for Web** or another &quot;Express&quot; version using the **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**.</span></span> <span data-ttu-id="7b894-253">Poniższe instrukcje przedstawiono czynności wymagane do zainstalowania *programu Visual studio Express 2012 for Web* przy użyciu *Instalatora platformy sieci Web firmy Microsoft*.</span><span class="sxs-lookup"><span data-stu-id="7b894-253">The following instructions guide you through the steps required to install *Visual studio Express 2012 for Web* using *Microsoft Web Platform Installer*.</span></span>

1. <span data-ttu-id="7b894-254">Przejdź do [ [ https://go.microsoft.com/? linkid = 9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169).</span><span class="sxs-lookup"><span data-stu-id="7b894-254">Go to [[https://go.microsoft.com/? linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169).</span></span> <span data-ttu-id="7b894-255">Alternatywnie, jeśli została już zainstalowana Instalatora platformy sieci Web, można otworzyć go i Wyszukaj produkt &quot; <em>programu Visual Studio Express 2012 for Web z zestawem Windows Azure SDK</em>&quot;.</span><span class="sxs-lookup"><span data-stu-id="7b894-255">Alternatively, if you already have installed Web Platform Installer, you can open it and search for the product &quot;<em>Visual Studio Express 2012 for Web with Windows Azure SDK</em>&quot;.</span></span>
2. <span data-ttu-id="7b894-256">Polecenie **teraz zainstalować**.</span><span class="sxs-lookup"><span data-stu-id="7b894-256">Click on **Install Now**.</span></span> <span data-ttu-id="7b894-257">Jeśli nie masz **Instalatora platformy sieci Web** nastąpi przekierowanie do pobrania i zainstalowania go najpierw.</span><span class="sxs-lookup"><span data-stu-id="7b894-257">If you do not have **Web Platform Installer** you will be redirected to download and install it first.</span></span>
3. <span data-ttu-id="7b894-258">Raz **Instalatora platformy sieci Web** jest otwarty, kliknij przycisk **zainstalować** można uruchomić Instalatora.</span><span class="sxs-lookup"><span data-stu-id="7b894-258">Once **Web Platform Installer** is open, click **Install** to start the setup.</span></span>

    <span data-ttu-id="7b894-259">![Instalowanie programu Visual Studio Express](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image22.png "instalacji programu Visual Studio Express")</span><span class="sxs-lookup"><span data-stu-id="7b894-259">![Install Visual Studio Express](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image22.png "Install Visual Studio Express")</span></span>

    <span data-ttu-id="7b894-260">*Instalowanie programu Visual Studio Express*</span><span class="sxs-lookup"><span data-stu-id="7b894-260">*Install Visual Studio Express*</span></span>
4. <span data-ttu-id="7b894-261">Odczytywanie wszystkich produktów licencji i warunków, a następnie kliknij przycisk **akceptuję** aby kontynuować.</span><span class="sxs-lookup"><span data-stu-id="7b894-261">Read all the products' licenses and terms and click **I Accept** to continue.</span></span>

    ![Akceptowanie umowy licencyjnej](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image23.png)

    <span data-ttu-id="7b894-263">*Akceptowanie umowy licencyjnej*</span><span class="sxs-lookup"><span data-stu-id="7b894-263">*Accepting the license terms*</span></span>
5. <span data-ttu-id="7b894-264">Poczekaj na zakończenie procesu pobierania i instalacji.</span><span class="sxs-lookup"><span data-stu-id="7b894-264">Wait until the downloading and installation process completes.</span></span>

    ![Postęp instalacji](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image24.png)

    <span data-ttu-id="7b894-266">*Postęp instalacji*</span><span class="sxs-lookup"><span data-stu-id="7b894-266">*Installation progress*</span></span>
6. <span data-ttu-id="7b894-267">Po zakończeniu instalacji kliknij przycisk **Zakończ**.</span><span class="sxs-lookup"><span data-stu-id="7b894-267">When the installation completes, click **Finish**.</span></span>

    ![Instalacja została zakończona](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image25.png)

    <span data-ttu-id="7b894-269">*Instalacja została zakończona*</span><span class="sxs-lookup"><span data-stu-id="7b894-269">*Installation completed*</span></span>
7. <span data-ttu-id="7b894-270">Kliknij przycisk **zakończenia** aby zamknąć Instalatora platformy sieci Web.</span><span class="sxs-lookup"><span data-stu-id="7b894-270">Click **Exit** to close Web Platform Installer.</span></span>
8. <span data-ttu-id="7b894-271">Aby otworzyć program Visual Studio Express for Web, przejdź do **Start** ekranu i zacznij pisać &quot; **VS Express**&quot;, następnie kliknij polecenie **VS Express for Web** Kafelek.</span><span class="sxs-lookup"><span data-stu-id="7b894-271">To open Visual Studio Express for Web, go to the **Start** screen and start writing &quot;**VS Express**&quot;, then click on the **VS Express for Web** tile.</span></span>

    ![VS Express for Web kafelka](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image26.png)

    <span data-ttu-id="7b894-273">*VS Express for Web kafelka*</span><span class="sxs-lookup"><span data-stu-id="7b894-273">*VS Express for Web tile*</span></span>

<a id="AppendixB"></a>

<a id="Appendix_B_Using_Code_Snippets"></a>
## <a name="appendix-b-using-code-snippets"></a><span data-ttu-id="7b894-274">Dodatek B: korzystania z wstawek kodu</span><span class="sxs-lookup"><span data-stu-id="7b894-274">Appendix B: Using Code Snippets</span></span>

<span data-ttu-id="7b894-275">Wstawki kodu zapewniają całego kodu, które są potrzebne w zasięgu ręki.</span><span class="sxs-lookup"><span data-stu-id="7b894-275">With code snippets, you have all the code you need at your fingertips.</span></span> <span data-ttu-id="7b894-276">Dokument laboratorium informuje o dokładnie po ich użycia, jak pokazano na poniższej ilustracji.</span><span class="sxs-lookup"><span data-stu-id="7b894-276">The lab document will tell you exactly when you can use them, as shown in the following figure.</span></span>

<span data-ttu-id="7b894-277">![Aby wstawić kod do projektu przy użyciu wstawki kodu programu Visual Studio](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image27.png "wstawki kodu za pomocą programu Visual Studio, aby wstawić kod do projektu")</span><span class="sxs-lookup"><span data-stu-id="7b894-277">![Using Visual Studio code snippets to insert code into your project](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image27.png "Using Visual Studio code snippets to insert code into your project")</span></span>

<span data-ttu-id="7b894-278">*Aby wstawić kod do projektu przy użyciu wstawki kodu programu Visual Studio*</span><span class="sxs-lookup"><span data-stu-id="7b894-278">*Using Visual Studio code snippets to insert code into your project*</span></span>

<span data-ttu-id="7b894-279">***Aby dodać fragment kodu za pomocą klawiatury (C# tylko)***</span><span class="sxs-lookup"><span data-stu-id="7b894-279">***To add a code snippet using the keyboard (C# only)***</span></span>

1. <span data-ttu-id="7b894-280">Umieść kursor, w którym chcesz wstawić kod.</span><span class="sxs-lookup"><span data-stu-id="7b894-280">Place the cursor where you would like to insert the code.</span></span>
2. <span data-ttu-id="7b894-281">Zacznij wpisywać nazwę fragment (bez spacji i łączniki).</span><span class="sxs-lookup"><span data-stu-id="7b894-281">Start typing the snippet name (without spaces or hyphens).</span></span>
3. <span data-ttu-id="7b894-282">Obejrzyj jako IntelliSense wyświetla zgodne z nazwami wstawki.</span><span class="sxs-lookup"><span data-stu-id="7b894-282">Watch as IntelliSense displays matching snippets' names.</span></span>
4. <span data-ttu-id="7b894-283">Wybierz prawidłowe fragment (lub zachować wpisywanie do momentu wybrania fragment całą nazwę).</span><span class="sxs-lookup"><span data-stu-id="7b894-283">Select the correct snippet (or keep typing until the entire snippet's name is selected).</span></span>
5. <span data-ttu-id="7b894-284">Naciśnij klawisz Tab dwa razy, aby wstawić fragment kodu w lokalizacji kursora.</span><span class="sxs-lookup"><span data-stu-id="7b894-284">Press the Tab key twice to insert the snippet at the cursor location.</span></span>

<span data-ttu-id="7b894-285">![Rozpocznij wpisywanie nazwy fragment](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image28.png "zacznij wpisywać nazwę wstawki programu")</span><span class="sxs-lookup"><span data-stu-id="7b894-285">![Start typing the snippet name](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image28.png "Start typing the snippet name")</span></span>

<span data-ttu-id="7b894-286">*Rozpocznij wpisywanie nazwy fragment kodu*</span><span class="sxs-lookup"><span data-stu-id="7b894-286">*Start typing the snippet name*</span></span>

<span data-ttu-id="7b894-287">![Naciśnij klawisz Tab, aby wybrać wyróżnione fragment](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image29.png "naciśnij klawisz Tab, aby wybrać wyróżnione fragment kodu")</span><span class="sxs-lookup"><span data-stu-id="7b894-287">![Press Tab to select the highlighted snippet](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image29.png "Press Tab to select the highlighted snippet")</span></span>

<span data-ttu-id="7b894-288">*Naciśnij klawisz Tab, aby wybrać wyróżnione fragment kodu*</span><span class="sxs-lookup"><span data-stu-id="7b894-288">*Press Tab to select the highlighted snippet*</span></span>

<span data-ttu-id="7b894-289">![Ponownie naciśnij klawisz Tab i fragment rozwinie](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image30.png "rozwinie ponownie naciśnij klawisz Tab i wstawki kodu")</span><span class="sxs-lookup"><span data-stu-id="7b894-289">![Press Tab again and the snippet will expand](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image30.png "Press Tab again and the snippet will expand")</span></span>

<span data-ttu-id="7b894-290">*Rozwinie ponownie naciśnij klawisz Tab i wstawki kodu*</span><span class="sxs-lookup"><span data-stu-id="7b894-290">*Press Tab again and the snippet will expand*</span></span>

<span data-ttu-id="7b894-291">***Aby dodać fragment kodu za pomocą myszy (C#, Visual Basic i XML)*** 1.</span><span class="sxs-lookup"><span data-stu-id="7b894-291">***To add a code snippet using the mouse (C#, Visual Basic and XML)*** 1.</span></span> <span data-ttu-id="7b894-292">Kliknij prawym przyciskiem myszy, gdy chcesz wstawić fragment kodu.</span><span class="sxs-lookup"><span data-stu-id="7b894-292">Right-click where you want to insert the code snippet.</span></span>

1. <span data-ttu-id="7b894-293">Wybierz **wstawić fragment** następuje **Moje wstawki kodu**.</span><span class="sxs-lookup"><span data-stu-id="7b894-293">Select **Insert Snippet** followed by **My Code Snippets**.</span></span>
2. <span data-ttu-id="7b894-294">Wybierz odpowiedni fragment z listy, klikając go.</span><span class="sxs-lookup"><span data-stu-id="7b894-294">Pick the relevant snippet from the list, by clicking on it.</span></span>

<span data-ttu-id="7b894-295">![Kliknij prawym przyciskiem myszy, gdy chcesz wstawić fragment kodu i wybierz wstawić fragment](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image31.png "kliknij prawym przyciskiem myszy, gdy chcesz wstawić fragment kodu i wybierz wstawić fragment kodu")</span><span class="sxs-lookup"><span data-stu-id="7b894-295">![Right-click where you want to insert the code snippet and select Insert Snippet](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image31.png "Right-click where you want to insert the code snippet and select Insert Snippet")</span></span>

<span data-ttu-id="7b894-296">*Kliknij prawym przyciskiem myszy, gdy chcesz wstawić fragment kodu i wybierz wstawić fragment kodu*</span><span class="sxs-lookup"><span data-stu-id="7b894-296">*Right-click where you want to insert the code snippet and select Insert Snippet*</span></span>

<span data-ttu-id="7b894-297">![Wybierz odpowiedni fragment z listy, klikając go](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image32.png "wybierz odpowiedni fragment z listy, klikając go")</span><span class="sxs-lookup"><span data-stu-id="7b894-297">![Pick the relevant snippet from the list, by clicking on it](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image32.png "Pick the relevant snippet from the list, by clicking on it")</span></span>

<span data-ttu-id="7b894-298">*Wybierz odpowiedni fragment z listy, klikając go*</span><span class="sxs-lookup"><span data-stu-id="7b894-298">*Pick the relevant snippet from the list, by clicking on it*</span></span>
