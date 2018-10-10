---
uid: mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-entity-framework-scaffolding-and-migrations
title: Platforma ASP.NET MVC 4 Entity Framework szkieletu i migracje | Dokumentacja firmy Microsoft
author: rick-anderson
description: Jeśli dopiero zaczynasz z metodami kontrolera platformy ASP.NET MVC 4 lub została ukończona &quot;pomocnicy, formularze i Walidacja&quot; praktycznym laboratorium, należy zwrócić uwagę...
ms.author: riande
ms.date: 02/18/2013
ms.assetid: 093c1362-f10b-407c-a708-be370f4b62b0
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-entity-framework-scaffolding-and-migrations
msc.type: authoredcontent
ms.openlocfilehash: bfb1edfcb756706e44126e7e96803bd2e9ce99fb
ms.sourcegitcommit: a4dcca4f1cb81227c5ed3c92dc0e28be6e99447b
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/10/2018
ms.locfileid: "48911022"
---
# <a name="aspnet-mvc-4-entity-framework-scaffolding-and-migrations"></a><span data-ttu-id="56d0f-103">Platforma ASP.NET MVC 4 Entity Framework szkieletu i migracje</span><span class="sxs-lookup"><span data-stu-id="56d0f-103">ASP.NET MVC 4 Entity Framework Scaffolding and Migrations</span></span>

<span data-ttu-id="56d0f-104">Przez [Camp w sieci Web zespołu](https://twitter.com/webcamps)</span><span class="sxs-lookup"><span data-stu-id="56d0f-104">By [Web Camps Team](https://twitter.com/webcamps)</span></span>

[<span data-ttu-id="56d0f-105">Pobierz Camp Web szkolenia Kit</span><span class="sxs-lookup"><span data-stu-id="56d0f-105">Download Web Camps Training Kit</span></span>](https://aka.ms/webcamps-training-kit)

<span data-ttu-id="56d0f-106">Jeśli dopiero zaczynasz z metodami kontrolera platformy ASP.NET MVC 4 lub została ukończona &quot;pomocnicy, formularze i Walidacja&quot; praktycznym laboratorium, należy pamiętać, że wiele logiki, aby utworzyć, zaktualizować, listy i Usuń dowolnej jednostki danych powtarza się wśród aplikacji.</span><span class="sxs-lookup"><span data-stu-id="56d0f-106">If you are familiar with ASP.NET MVC 4 controller methods, or have completed the &quot;Helpers, Forms and Validation&quot; Hands-On lab, you should be aware that many of the logic to create, update, list and remove any data entity it is repeated among the application.</span></span> <span data-ttu-id="56d0f-107">Aby mówią, że, jeśli model zawiera kilka klas do manipulowania, nie będzie prawdopodobnie poświęcać wiele czasu Pisanie metod akcji POST i GET dla każdej operacji podmiotu, a także każdego z widoków.</span><span class="sxs-lookup"><span data-stu-id="56d0f-107">Not to mention that, if your model has several classes to manipulate, you will be likely to spend a considerable time writing the POST and GET action methods for each entity operation, as well as each of the views.</span></span>

<span data-ttu-id="56d0f-108">W tym laboratorium dowiesz się, jak używać szkieletu ASP.NET MVC 4 do automatycznego generowania linię bazową aplikacji CRUD (tworzenia, odczytu, aktualizowania lub usuwania).</span><span class="sxs-lookup"><span data-stu-id="56d0f-108">In this lab you will learn how to use the ASP.NET MVC 4 scaffolding to automatically generate the baseline of your application's CRUD (Create, Read, Update and Delete).</span></span> <span data-ttu-id="56d0f-109">Uruchamianie z klasy prosty model i bez konieczności pisania nawet jednego wiersza kodu, utworzysz kontroler, który będzie zawierać wszystkich operacji CRUD, a także wszystkie potrzeby widoków.</span><span class="sxs-lookup"><span data-stu-id="56d0f-109">Starting from a simple model class, and, without writing a single line of code, you will create a controller that will contain all the CRUD operations, as well as the all the necessary views.</span></span> <span data-ttu-id="56d0f-110">Po kompilowania i uruchamiania prostego rozwiązania, będziesz mieć wygenerowane, wraz z logikę MVC i widoków manipulowanie danymi bazy danych aplikacji.</span><span class="sxs-lookup"><span data-stu-id="56d0f-110">After building and running the simple solution, you will have the application database generated, together with the MVC logic and views for data manipulation.</span></span>

<span data-ttu-id="56d0f-111">Ponadto dowiesz się, jak łatwe jest korzystanie z migracją architektury jednostek do przeprowadzania aktualizacji modelu w całej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="56d0f-111">In addition, you will learn how easy it is to use Entity Framework Migrations to perform model updates throughout your entire application.</span></span> <span data-ttu-id="56d0f-112">Migracją architektury jednostek będzie można zmodyfikować bazy danych, po zmianie modelu przy użyciu prostych krokach.</span><span class="sxs-lookup"><span data-stu-id="56d0f-112">Entity Framework Migrations will let you modify your database after the model has changed with simple steps.</span></span> <span data-ttu-id="56d0f-113">Wszystkie te należy pamiętać można do tworzenia i konserwowania aplikacji sieci web bardziej wydajnie, korzystając z zalet najnowszych funkcji platformy ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="56d0f-113">With all these in mind, you will be able to build and maintain web applications more efficiently, taking advantage of the latest features of ASP.NET MVC 4.</span></span>

> [!NOTE]
> <span data-ttu-id="56d0f-114">Wszystkie przykładowy kod i fragmenty są uwzględnione w sieci Web Camp zestaw szkoleniowy, pod [wersji Microsoft — w sieci Web/WebCampTrainingKit](https://aka.ms/webcamps-training-kit).</span><span class="sxs-lookup"><span data-stu-id="56d0f-114">All sample code and snippets are included in the Web Camps Training Kit, available from at [Microsoft-Web/WebCampTrainingKit Releases](https://aka.ms/webcamps-training-kit).</span></span> <span data-ttu-id="56d0f-115">Projekt specyficzne dla tego laboratorium znajduje się w temacie [platformy ASP.NET MVC 4 Entity Framework szkieletu i migracje](https://github.com/Microsoft-Web/HOL-EntityFrameworkScaffoldingAndMigrations).</span><span class="sxs-lookup"><span data-stu-id="56d0f-115">The project specific to this lab is available at [ASP.NET MVC 4 Entity Framework Scaffolding and Migrations](https://github.com/Microsoft-Web/HOL-EntityFrameworkScaffoldingAndMigrations).</span></span>

<a id="Objectives"></a>
### <a name="objectives"></a><span data-ttu-id="56d0f-116">Cele</span><span class="sxs-lookup"><span data-stu-id="56d0f-116">Objectives</span></span>

<span data-ttu-id="56d0f-117">W tym laboratorium praktyczne dowiesz się jak:</span><span class="sxs-lookup"><span data-stu-id="56d0f-117">In this Hands-On Lab, you will learn how to:</span></span>

- <span data-ttu-id="56d0f-118">Funkcja tworzenia szkieletu ASP.NET na użytek operacji CRUD w kontrolerów.</span><span class="sxs-lookup"><span data-stu-id="56d0f-118">Use ASP.NET scaffolding for CRUD operations in controllers.</span></span>
- <span data-ttu-id="56d0f-119">Zmień model bazy danych przy użyciu migracją architektury jednostek.</span><span class="sxs-lookup"><span data-stu-id="56d0f-119">Change the database model using Entity Framework Migrations.</span></span>

<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a><span data-ttu-id="56d0f-120">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="56d0f-120">Prerequisites</span></span>

<span data-ttu-id="56d0f-121">Należy dysponować następującymi elementami do przygotowania tego laboratorium:</span><span class="sxs-lookup"><span data-stu-id="56d0f-121">You must have the following items to complete this lab:</span></span>

- <span data-ttu-id="56d0f-122">[Microsoft Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) lub wyższego poziomu (odczyt [dodatek a.](#AppendixA) instrukcje dotyczące sposobu jego instalacji).</span><span class="sxs-lookup"><span data-stu-id="56d0f-122">[Microsoft Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) or superior (read [Appendix A](#AppendixA) for instructions on how to install it).</span></span>

<a id="Setup"></a>

<a id="Setup"></a>
### <a name="setup"></a><span data-ttu-id="56d0f-123">Konfiguracja</span><span class="sxs-lookup"><span data-stu-id="56d0f-123">Setup</span></span>

<span data-ttu-id="56d0f-124">**Instalowanie fragmentów kodu**</span><span class="sxs-lookup"><span data-stu-id="56d0f-124">**Installing Code Snippets**</span></span>

<span data-ttu-id="56d0f-125">Dla wygody większość kodu, który będzie zarządzany wzdłuż tego laboratorium jest dostępna jako fragmenty kodu programu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="56d0f-125">For convenience, much of the code you will be managing along this lab is available as Visual Studio code snippets.</span></span> <span data-ttu-id="56d0f-126">Aby zainstalować fragmenty kodu, uruchom **.\Source\Setup\CodeSnippets.vsi** pliku.</span><span class="sxs-lookup"><span data-stu-id="56d0f-126">To install the code snippets run **.\Source\Setup\CodeSnippets.vsi** file.</span></span>

<span data-ttu-id="56d0f-127">Jeśli nie jesteś zaznajomiony z fragmentów kodu w usłudze Visual Studio i chcesz dowiedzieć się, jak z nich korzystać, możesz zapoznać się z dodatku z tego dokumentu &quot; [dodatek B: za pomocą wstawek kodu programu](#AppendixB)&quot;.</span><span class="sxs-lookup"><span data-stu-id="56d0f-127">If you are not familiar with the Visual Studio Code Snippets, and want to learn how to use them, you can refer to the appendix from this document &quot;[Appendix B: Using Code Snippets](#AppendixB)&quot;.</span></span>

* * *

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a><span data-ttu-id="56d0f-128">Ćwiczenia</span><span class="sxs-lookup"><span data-stu-id="56d0f-128">Exercises</span></span>

<span data-ttu-id="56d0f-129">Poniższym ćwiczeniu tworzą tego laboratorium praktyczne:</span><span class="sxs-lookup"><span data-stu-id="56d0f-129">The following exercise make up this Hands-On Lab:</span></span>

1. [<span data-ttu-id="56d0f-130">Za pomocą platformy ASP.NET MVC 4 szkieletu z migracją architektury jednostek</span><span class="sxs-lookup"><span data-stu-id="56d0f-130">Using ASP.NET MVC 4 Scaffolding with Entity Framework Migrations</span></span>](#Exercise1)

> [!NOTE]
> <span data-ttu-id="56d0f-131">Dołączone w tym ćwiczeniu **zakończenia** folderu zawierającego wynikowy rozwiązania, należy uzyskać po zakończeniu wykonywania.</span><span class="sxs-lookup"><span data-stu-id="56d0f-131">This exercise is accompanied by an **End** folder containing the resulting solution you should obtain after completing the exercise.</span></span> <span data-ttu-id="56d0f-132">Jeśli potrzebujesz dodatkowej pomocy, w tym ćwiczeniu, można użyć tego rozwiązania jako wskazówki.</span><span class="sxs-lookup"><span data-stu-id="56d0f-132">You can use this solution as a guide if you need additional help working through the exercise.</span></span>


<span data-ttu-id="56d0f-133">Szacowany czas do ukończenia tego laboratorium: **30 minut**</span><span class="sxs-lookup"><span data-stu-id="56d0f-133">Estimated time to complete this lab: **30 minutes**</span></span>

<a id="Exercise1"></a>

<a id="Exercise_1_Using_ASPNET_MVC_4_Scaffolding_with_Entity_Framework_Migrations"></a>
### <a name="exercise-1-using-aspnet-mvc-4-scaffolding-with-entity-framework-migrations"></a><span data-ttu-id="56d0f-134">Ćwiczenie 1: Szkieletów platformy ASP.NET MVC 4 z migracją architektury jednostek</span><span class="sxs-lookup"><span data-stu-id="56d0f-134">Exercise 1: Using ASP.NET MVC 4 Scaffolding with Entity Framework Migrations</span></span>

<span data-ttu-id="56d0f-135">Funkcja tworzenia szkieletu ASP.NET MVC umożliwia szybkie generowanie operacji CRUD w sposób Zestandaryzowany tworzenia niezbędne logikę, która umożliwia interakcję z warstwą bazy danych aplikacji.</span><span class="sxs-lookup"><span data-stu-id="56d0f-135">ASP.NET MVC scaffolding provides a quick way to generate the CRUD operations in a standardized way, creating the necessary logic that lets your application interact with the database layer.</span></span>

<span data-ttu-id="56d0f-136">W tym ćwiczeniu dowiesz się, jak za pomocą tworzenia szkieletu ASP.NET MVC 4 przy użyciu kodu najpierw utworzyć metody CRUD.</span><span class="sxs-lookup"><span data-stu-id="56d0f-136">In this exercise, you will learn how to use ASP.NET MVC 4 scaffolding with code first to create the CRUD methods.</span></span> <span data-ttu-id="56d0f-137">Następnie zostanie dowiesz się, jak zaktualizować model zastosowania zmian w bazie danych, korzystając z migracją architektury jednostek.</span><span class="sxs-lookup"><span data-stu-id="56d0f-137">Then, you will learn how to update your model applying the changes in the database by using Entity Framework Migrations.</span></span>

<a id="Ex1Task1"></a>

<a id="Task_1-_Creating_a_new_ASPNET_MVC_4_project_using_Scaffolding"></a>
#### <a name="task-1--creating-a-new-aspnet-mvc-4-project-using-scaffolding"></a><span data-ttu-id="56d0f-138">Zadanie 1 — Tworzenie nowej platformy ASP.NET MVC 4 projektu szkieletów</span><span class="sxs-lookup"><span data-stu-id="56d0f-138">Task 1- Creating a new ASP.NET MVC 4 project using Scaffolding</span></span>

1. <span data-ttu-id="56d0f-139">Jeśli nie już jest otwarty, uruchom **programu Visual Studio 2012**.</span><span class="sxs-lookup"><span data-stu-id="56d0f-139">If not already open, start **Visual Studio 2012**.</span></span>
2. <span data-ttu-id="56d0f-140">Wybierz **pliku | Nowy projekt**.</span><span class="sxs-lookup"><span data-stu-id="56d0f-140">Select **File | New Project**.</span></span> <span data-ttu-id="56d0f-141">W nowym projekcie okno dialogowe, w obszarze **Visual C# | Web** zaznacz **aplikacji sieci Web programu ASP.NET MVC 4**.</span><span class="sxs-lookup"><span data-stu-id="56d0f-141">In the New Project dialog, under the **Visual C# | Web** section, select **ASP.NET MVC 4 Web Application**.</span></span> <span data-ttu-id="56d0f-142">Nazwij projekt, aby **MVC4andEFMigrations** i Ustaw lokalizację na **Source\Ex1 UsingMVC4ScaffoldingEFMigrations** folder w tym laboratorium.</span><span class="sxs-lookup"><span data-stu-id="56d0f-142">Name the project to **MVC4andEFMigrations** and set the location to **Source\Ex1-UsingMVC4ScaffoldingEFMigrations** folder of this lab.</span></span> <span data-ttu-id="56d0f-143">Ustaw **Nazwa rozwiązania** do **rozpocząć** i upewnij się, **Utwórz katalog rozwiązania** jest zaznaczone.</span><span class="sxs-lookup"><span data-stu-id="56d0f-143">Set the **Solution name** to **Begin** and ensure **Create directory for solution** is checked.</span></span> <span data-ttu-id="56d0f-144">Kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="56d0f-144">Click **OK**.</span></span>

    <span data-ttu-id="56d0f-145">![Okno dialogowe Nowy projekt ASP.NET MVC 4](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image1.png "okno dialogowe Nowy projekt ASP.NET MVC 4")</span><span class="sxs-lookup"><span data-stu-id="56d0f-145">![New ASP.NET MVC 4 Project Dialog Box](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image1.png "New ASP.NET MVC 4 Project Dialog Box")</span></span>

    <span data-ttu-id="56d0f-146">*Okno dialogowe Nowy projekt ASP.NET MVC 4*</span><span class="sxs-lookup"><span data-stu-id="56d0f-146">*New ASP.NET MVC 4 Project Dialog Box*</span></span>
3. <span data-ttu-id="56d0f-147">W **nowego projektu programu ASP.NET MVC 4** wybierz okno dialogowe **aplikacji internetowej** szablonu i upewnij się, że **Razor** jest wybrane **aparat widoku**.</span><span class="sxs-lookup"><span data-stu-id="56d0f-147">In the **New ASP.NET MVC 4 Project** dialog box select the **Internet Application** template, and make sure that **Razor** is the selected **View engine**.</span></span> <span data-ttu-id="56d0f-148">Kliknij przycisk **OK** do tworzenia projektu.</span><span class="sxs-lookup"><span data-stu-id="56d0f-148">Click **OK** to create the project.</span></span>

    <span data-ttu-id="56d0f-149">![Nowej aplikacji internetowej platformy ASP.NET MVC 4](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image2.png "nowej aplikacji internetowej platformy ASP.NET MVC 4")</span><span class="sxs-lookup"><span data-stu-id="56d0f-149">![New ASP.NET MVC 4 Internet Application](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image2.png "New ASP.NET MVC 4 Internet Application")</span></span>

    <span data-ttu-id="56d0f-150">*Nowej aplikacji internetowej platformy ASP.NET MVC 4*</span><span class="sxs-lookup"><span data-stu-id="56d0f-150">*New ASP.NET MVC 4 Internet Application*</span></span>
4. <span data-ttu-id="56d0f-151">W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy **modeli** i wybierz **Dodaj | Klasa** utworzyć prostą klasę osoby (POCO).</span><span class="sxs-lookup"><span data-stu-id="56d0f-151">In the Solution Explorer, right-click **Models** and select **Add | Class** to create a simple class person (POCO).</span></span> <span data-ttu-id="56d0f-152">Nadaj mu nazwę **osoby** i kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="56d0f-152">Name it **Person** and click **OK**.</span></span>
5. <span data-ttu-id="56d0f-153">Otwórz klasę osoby i Wstaw następujące właściwości.</span><span class="sxs-lookup"><span data-stu-id="56d0f-153">Open the Person class and insert the following properties.</span></span>

    <span data-ttu-id="56d0f-154">(Code Snippet — *platformy ASP.NET MVC 4 i migracją architektury jednostek — właściwości osoby Ex1*)</span><span class="sxs-lookup"><span data-stu-id="56d0f-154">(Code Snippet - *ASP.NET MVC 4 and Entity Framework Migrations - Ex1 Person Properties*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample1.cs)]
6. <span data-ttu-id="56d0f-155">Kliknij przycisk **kompilacji | Tworzenie rozwiązania** Aby zapisać zmiany i skompiluj projekt.</span><span class="sxs-lookup"><span data-stu-id="56d0f-155">Click **Build | Build Solution** to save the changes and build the project.</span></span>

    <span data-ttu-id="56d0f-156">![Tworzenie aplikacji](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image3.png "Kompilowanie aplikacji")</span><span class="sxs-lookup"><span data-stu-id="56d0f-156">![Building the application](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image3.png "Building the application")</span></span>

    <span data-ttu-id="56d0f-157">*Tworzenie aplikacji*</span><span class="sxs-lookup"><span data-stu-id="56d0f-157">*Building the Application*</span></span>
7. <span data-ttu-id="56d0f-158">W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy folder kontrolerów, a następnie wybierz **Dodaj | Kontroler**.</span><span class="sxs-lookup"><span data-stu-id="56d0f-158">In the Solution Explorer, right-click the controllers folder and select **Add | Controller**.</span></span>
8. <span data-ttu-id="56d0f-159">Nazwa kontrolera *PersonController* i ukończyć **opcji tworzenia szkieletu** z następującymi wartościami.</span><span class="sxs-lookup"><span data-stu-id="56d0f-159">Name the controller *PersonController* and complete the **Scaffolding options** with the following values.</span></span>

   1. <span data-ttu-id="56d0f-160">W **szablonu** listy rozwijanej wybierz **kontroler MVC z akcjami odczytu/zapisu i widokami używający narzędzia Entity Framework** opcji.</span><span class="sxs-lookup"><span data-stu-id="56d0f-160">In the **Template** drop-down list, select the **MVC controller with read/write actions and views, using Entity Framework** option.</span></span>
   2. <span data-ttu-id="56d0f-161">W **klasa modelu** listy rozwijanej wybierz **osoby** klasy.</span><span class="sxs-lookup"><span data-stu-id="56d0f-161">In the **Model class** drop-down list, select the **Person** class.</span></span>
   3. <span data-ttu-id="56d0f-162">W **klasy kontekstu danych** listy wybierz  **&lt;nowy kontekst danych... &gt;**.</span><span class="sxs-lookup"><span data-stu-id="56d0f-162">In the **Data Context class** list, select **&lt;New data context...&gt;**.</span></span> <span data-ttu-id="56d0f-163">Wybierz dowolną nazwę, a następnie kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="56d0f-163">Choose any name and click **OK**.</span></span>
   4. <span data-ttu-id="56d0f-164">W **widoków** listy rozwijanej liście, upewnij się, że **Razor** jest zaznaczone.</span><span class="sxs-lookup"><span data-stu-id="56d0f-164">In the **Views** drop-down list, make sure that **Razor** is selected.</span></span>

      <span data-ttu-id="56d0f-165">![Dodawanie kontrolera osoby za pomocą tworzenia szkieletów](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image4.png "dodawania kontrolera osoby za pomocą tworzenia szkieletów")</span><span class="sxs-lookup"><span data-stu-id="56d0f-165">![Adding the Person controller with scaffolding](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image4.png "Adding the Person controller with scaffolding")</span></span>

      <span data-ttu-id="56d0f-166">*Dodawanie kontrolera osoby za pomocą tworzenia szkieletów*</span><span class="sxs-lookup"><span data-stu-id="56d0f-166">*Adding the Person controller with scaffolding*</span></span>
9. <span data-ttu-id="56d0f-167">Kliknij przycisk **Dodaj** do utworzenia nowego kontrolera dla osoby za pomocą tworzenia szkieletów.</span><span class="sxs-lookup"><span data-stu-id="56d0f-167">Click **Add** to create the new controller for Person with scaffolding.</span></span> <span data-ttu-id="56d0f-168">Masz teraz generowane akcji kontrolera, a także widoki.</span><span class="sxs-lookup"><span data-stu-id="56d0f-168">You have now generated the controller actions as well as the views.</span></span>

    <span data-ttu-id="56d0f-169">![Po utworzeniu kontroler osoby za pomocą tworzenia szkieletów](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image5.png "po utworzeniu kontroler osoby za pomocą tworzenia szkieletów")</span><span class="sxs-lookup"><span data-stu-id="56d0f-169">![After creating the Person controller with scaffolding](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image5.png "After creating the Person controller with scaffolding")</span></span>

    <span data-ttu-id="56d0f-170">*Po utworzeniu kontroler osoby za pomocą tworzenia szkieletów*</span><span class="sxs-lookup"><span data-stu-id="56d0f-170">*After creating the Person controller with scaffolding*</span></span>
10. <span data-ttu-id="56d0f-171">Otwórz **PersonController** klasy.</span><span class="sxs-lookup"><span data-stu-id="56d0f-171">Open **PersonController** class.</span></span> <span data-ttu-id="56d0f-172">Należy zauważyć, że pełna metod akcji CRUD został wygenerowany automatycznie.</span><span class="sxs-lookup"><span data-stu-id="56d0f-172">Notice that the full CRUD action methods have been generated automatically.</span></span>

   <span data-ttu-id="56d0f-173">![Wewnątrz kontrolera osoby](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image6.png "kontrolera wewnątrz osoby")</span><span class="sxs-lookup"><span data-stu-id="56d0f-173">![Inside the Person controller](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image6.png "Inside the Person controller")</span></span>

   <span data-ttu-id="56d0f-174">*Wewnątrz kontrolera osoby*</span><span class="sxs-lookup"><span data-stu-id="56d0f-174">*Inside the Person controller*</span></span>

<a id="Ex1Task2"></a>

<a id="Task_2-_Running_the_application"></a>
#### <a name="task-2--running-the-application"></a><span data-ttu-id="56d0f-175">Zadanie 2 — uruchamianie aplikacji</span><span class="sxs-lookup"><span data-stu-id="56d0f-175">Task 2- Running the application</span></span>

<span data-ttu-id="56d0f-176">W tym momencie baza danych nie został jeszcze utworzony.</span><span class="sxs-lookup"><span data-stu-id="56d0f-176">At this point, the database is not yet created.</span></span> <span data-ttu-id="56d0f-177">W ramach tego zadania możesz uruchomić aplikację po raz pierwszy i testowania operacji CRUD.</span><span class="sxs-lookup"><span data-stu-id="56d0f-177">In this task, you will run the application for the first time and test the CRUD operations.</span></span> <span data-ttu-id="56d0f-178">Baza danych zostanie utworzona na bieżąco Code First.</span><span class="sxs-lookup"><span data-stu-id="56d0f-178">The database will be created on the fly with Code First.</span></span>

1. <span data-ttu-id="56d0f-179">Naciśnij klawisz **F5** do uruchomienia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="56d0f-179">Press **F5** to run the application.</span></span>
2. <span data-ttu-id="56d0f-180">W przeglądarce, należy dodać **/Person** do adresu URL, aby otworzyć stronę osoby.</span><span class="sxs-lookup"><span data-stu-id="56d0f-180">In the browser, add **/Person** to the URL to open the Person page.</span></span>

    <span data-ttu-id="56d0f-181">![Pierwsze uruchomienie aplikacji](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image7.png "pierwszego uruchomienia aplikacji")</span><span class="sxs-lookup"><span data-stu-id="56d0f-181">![Application first run](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image7.png "Application first run")</span></span>

    <span data-ttu-id="56d0f-182">*Aplikacji: pierwszym uruchomieniu*</span><span class="sxs-lookup"><span data-stu-id="56d0f-182">*Application: first run*</span></span>
3. <span data-ttu-id="56d0f-183">Teraz będzie przeglądanie stron osoby i testowania operacji CRUD.</span><span class="sxs-lookup"><span data-stu-id="56d0f-183">You will now explore the Person pages and test the CRUD operations.</span></span>

    1. <span data-ttu-id="56d0f-184">Kliknij przycisk **Utwórz nową** na dodawanie nowej osoby.</span><span class="sxs-lookup"><span data-stu-id="56d0f-184">Click **Create New** to add a new person.</span></span> <span data-ttu-id="56d0f-185">Wprowadź imię i nazwisko, a następnie kliknij przycisk **Utwórz**.</span><span class="sxs-lookup"><span data-stu-id="56d0f-185">Enter a first name and a last name and click **Create**.</span></span>

        <span data-ttu-id="56d0f-186">![Dodawanie nowej osoby](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image8.png "Dodawanie nowej osoby")</span><span class="sxs-lookup"><span data-stu-id="56d0f-186">![Adding a new person](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image8.png "Adding a new person")</span></span>

        <span data-ttu-id="56d0f-187">*Dodawanie nowej osoby*</span><span class="sxs-lookup"><span data-stu-id="56d0f-187">*Adding a new person*</span></span>
    2. <span data-ttu-id="56d0f-188">Na liście osoby możesz usunąć, Edytuj lub Dodaj elementy.</span><span class="sxs-lookup"><span data-stu-id="56d0f-188">In the person's list, you can delete, edit or add items.</span></span>

        <span data-ttu-id="56d0f-189">![listy osób](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image9.png "listy osób")</span><span class="sxs-lookup"><span data-stu-id="56d0f-189">![person list](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image9.png "person list")</span></span>

        <span data-ttu-id="56d0f-190">*Listy osób*</span><span class="sxs-lookup"><span data-stu-id="56d0f-190">*Person list*</span></span>
    3. <span data-ttu-id="56d0f-191">Kliknij przycisk **szczegóły** można otworzyć szczegóły tej osoby.</span><span class="sxs-lookup"><span data-stu-id="56d0f-191">Click **Details** to open the person's details.</span></span>

        <span data-ttu-id="56d0f-192">![Szczegóły osoby](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image10.png "szczegółów osoby")</span><span class="sxs-lookup"><span data-stu-id="56d0f-192">![Person's details](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image10.png "Person's details")</span></span>

        <span data-ttu-id="56d0f-193">*Szczegóły tej osoby*</span><span class="sxs-lookup"><span data-stu-id="56d0f-193">*Person's details*</span></span>
4. <span data-ttu-id="56d0f-194">Zamknij przeglądarkę i wróć do programu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="56d0f-194">Close the browser and return to Visual Studio.</span></span> <span data-ttu-id="56d0f-195">Zwróć uwagę, czy utworzono całej operacji CRUD dla jednostki osoby w całej aplikacji — od modelu do widoków — bez konieczności pisania nawet wiersza kodu!</span><span class="sxs-lookup"><span data-stu-id="56d0f-195">Notice that you have created the whole CRUD for the person entity throughout your application -from the model to the views- without having to write a single line of code!</span></span>

<a id="Ex1Task3"></a>

<a id="Task_3-_Updating_the_database_using_Entity_Framework_Migrations"></a>
#### <a name="task-3--updating-the-database-using-entity-framework-migrations"></a><span data-ttu-id="56d0f-196">Zadanie 3 — aktualizowanie bazy danych, korzystając z migracją architektury jednostek</span><span class="sxs-lookup"><span data-stu-id="56d0f-196">Task 3- Updating the database using Entity Framework Migrations</span></span>

<span data-ttu-id="56d0f-197">To zadanie zaktualizuje bazy danych, korzystając z migracją architektury jednostek.</span><span class="sxs-lookup"><span data-stu-id="56d0f-197">In this task you will update the database using Entity Framework Migrations.</span></span> <span data-ttu-id="56d0f-198">Wykryje to, jak łatwo zmienić model i uwzględnia zmiany w bazach danych przy użyciu funkcji migracją architektury jednostek.</span><span class="sxs-lookup"><span data-stu-id="56d0f-198">You will discover how easy it is to change the model and reflect the changes in your databases by using the Entity Framework Migrations feature.</span></span>

1. <span data-ttu-id="56d0f-199">Otwórz konsolę Menedżera pakietów.</span><span class="sxs-lookup"><span data-stu-id="56d0f-199">Open the Package Manager Console.</span></span> <span data-ttu-id="56d0f-200">Wybierz **narzędzia** > **Menedżera pakietów NuGet** > **Konsola Menedżera pakietów**.</span><span class="sxs-lookup"><span data-stu-id="56d0f-200">Select **Tools** > **NuGet Package Manager** > **Package Manager Console**.</span></span>
2. <span data-ttu-id="56d0f-201">W konsoli Menedżera pakietów wpisz następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="56d0f-201">In the Package Manager Console, enter the following command:</span></span>

    <span data-ttu-id="56d0f-202">PMC</span><span class="sxs-lookup"><span data-stu-id="56d0f-202">PMC</span></span>

    [!code-powershell[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample2.ps1)]

    <span data-ttu-id="56d0f-203">![Włączanie migracji](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image11.png "Włączanie migracji")</span><span class="sxs-lookup"><span data-stu-id="56d0f-203">![Enabling Migrations](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image11.png "Enabling Migrations")</span></span>

    <span data-ttu-id="56d0f-204">*Włączanie migracji*</span><span class="sxs-lookup"><span data-stu-id="56d0f-204">*Enabling migrations*</span></span>

    <span data-ttu-id="56d0f-205">Polecenie Włączanie migracji tworzy **migracje** folder, który zawiera skrypt, aby zainicjować bazy danych.</span><span class="sxs-lookup"><span data-stu-id="56d0f-205">The Enable-Migration command creates the **Migrations** folder, which contains a script to initialize the database.</span></span>

    <span data-ttu-id="56d0f-206">![Folder migracje](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image12.png "migracji folderu")</span><span class="sxs-lookup"><span data-stu-id="56d0f-206">![Migrations folder](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image12.png "Migrations folder")</span></span>

    <span data-ttu-id="56d0f-207">*Folder migracji*</span><span class="sxs-lookup"><span data-stu-id="56d0f-207">*Migrations folder*</span></span>
3. <span data-ttu-id="56d0f-208">Otwórz **Configuration.cs** pliku w folderze migracji.</span><span class="sxs-lookup"><span data-stu-id="56d0f-208">Open the **Configuration.cs** file in the Migrations folder.</span></span> <span data-ttu-id="56d0f-209">Znajdź Konstruktor klasy i zmień **AutomaticMigrationsEnabled** wartość *true*.</span><span class="sxs-lookup"><span data-stu-id="56d0f-209">Locate the class constructor and change the **AutomaticMigrationsEnabled** value to *true*.</span></span>

    [!code-csharp[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample3.cs)]
4. <span data-ttu-id="56d0f-210">Otwórz klasę osoby, a następnie dodaj atrybut drugie imię.</span><span class="sxs-lookup"><span data-stu-id="56d0f-210">Open the Person class and add an attribute for the person's middle name.</span></span> <span data-ttu-id="56d0f-211">Przy użyciu tego nowego atrybutu w przypadku zmiany modelu.</span><span class="sxs-lookup"><span data-stu-id="56d0f-211">With this new attribute, you are changing the model.</span></span>

    [!code-csharp[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample4.cs)]
5. <span data-ttu-id="56d0f-212">Wybierz **kompilacji | Tworzenie rozwiązania** menu, aby skompilować aplikację.</span><span class="sxs-lookup"><span data-stu-id="56d0f-212">Select **Build | Build Solution** on the menu to build the application.</span></span>

    <span data-ttu-id="56d0f-213">![Tworzenie aplikacji](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image13.png "Kompilowanie aplikacji")</span><span class="sxs-lookup"><span data-stu-id="56d0f-213">![Building the application](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image13.png "Building the application")</span></span>

    <span data-ttu-id="56d0f-214">*Tworzenie aplikacji*</span><span class="sxs-lookup"><span data-stu-id="56d0f-214">*Building the application*</span></span>
6. <span data-ttu-id="56d0f-215">W konsoli Menedżera pakietów wpisz następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="56d0f-215">In the Package Manager Console, enter the following command:</span></span>

    <span data-ttu-id="56d0f-216">PMC</span><span class="sxs-lookup"><span data-stu-id="56d0f-216">PMC</span></span>

    [!code-powershell[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample5.ps1)]

    <span data-ttu-id="56d0f-217">To polecenie będzie szukał zmiany w obiektach danych, a następnie dodaj wymagane polecenia, aby odpowiednio zmodyfikować bazy danych.</span><span class="sxs-lookup"><span data-stu-id="56d0f-217">This command will look for changes in the data objects, and then, it will add the necessary commands to modify the database accordingly.</span></span>

    <span data-ttu-id="56d0f-218">![Dodawanie drugiego imienia](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image14.png "Dodawanie drugie imię")</span><span class="sxs-lookup"><span data-stu-id="56d0f-218">![Adding a middle name](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image14.png "Adding a middle name")</span></span>

    <span data-ttu-id="56d0f-219">*Dodawanie drugie imię*</span><span class="sxs-lookup"><span data-stu-id="56d0f-219">*Adding a middle name*</span></span>
7. <span data-ttu-id="56d0f-220">(Opcjonalnie) Można uruchomić następujące polecenie, aby wygenerować skrypt SQL z zastosowaniem aktualizacji różnicowych.</span><span class="sxs-lookup"><span data-stu-id="56d0f-220">(Optional) You can run the following command to generate a SQL script with the differential update.</span></span> <span data-ttu-id="56d0f-221">Dzięki temu możesz ręcznie zaktualizować bazę danych (w tym przypadku go nie jest to konieczne), lub Zastosuj zmiany w innych bazach danych:</span><span class="sxs-lookup"><span data-stu-id="56d0f-221">This will let you update the database manually (In this case it's not necessary), or apply the changes in other databases:</span></span>

    <span data-ttu-id="56d0f-222">PMC</span><span class="sxs-lookup"><span data-stu-id="56d0f-222">PMC</span></span>

    [!code-powershell[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample6.ps1)]

    <span data-ttu-id="56d0f-223">![Generowanie skryptu SQL](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image15.png "Generowanie skryptu SQL")</span><span class="sxs-lookup"><span data-stu-id="56d0f-223">![Generating a SQL script](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image15.png "Generating a SQL script")</span></span>

    <span data-ttu-id="56d0f-224">*Generowanie skryptu SQL*</span><span class="sxs-lookup"><span data-stu-id="56d0f-224">*Generating a SQL script*</span></span>

    <span data-ttu-id="56d0f-225">![Skrypt SQL aktualizacji](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image16.png "aktualizacji skrypt SQL")</span><span class="sxs-lookup"><span data-stu-id="56d0f-225">![SQL Script update](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image16.png "SQL Script update")</span></span>

    <span data-ttu-id="56d0f-226">*Aktualizacja skrypt SQL*</span><span class="sxs-lookup"><span data-stu-id="56d0f-226">*SQL Script update*</span></span>
8. <span data-ttu-id="56d0f-227">W konsoli Menedżera pakietów wpisz następujące polecenie, aby zaktualizować bazę danych:</span><span class="sxs-lookup"><span data-stu-id="56d0f-227">In the Package Manager Console, enter the following command to update the database:</span></span>

    <span data-ttu-id="56d0f-228">PMC</span><span class="sxs-lookup"><span data-stu-id="56d0f-228">PMC</span></span>

    [!code-powershell[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample7.ps1)]

    <span data-ttu-id="56d0f-229">![Aktualizowanie bazy danych](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image17.png "zaktualizowanie bazy danych")</span><span class="sxs-lookup"><span data-stu-id="56d0f-229">![Updating the Database](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image17.png "Updating the Database")</span></span>

    <span data-ttu-id="56d0f-230">*Aktualizowanie bazy danych*</span><span class="sxs-lookup"><span data-stu-id="56d0f-230">*Updating the Database*</span></span>

    <span data-ttu-id="56d0f-231">Spowoduje to dodanie **MiddleName** kolumny w **osób** tabelę do dopasowania bieżącej definicji **osoby** klasy.</span><span class="sxs-lookup"><span data-stu-id="56d0f-231">This will add the **MiddleName** column in the **People** table to match the current definition of the **Person** class.</span></span>
9. <span data-ttu-id="56d0f-232">Po zaktualizowaniu bazy danych, kliknij prawym przyciskiem myszy folder kontrolera i wybierz **Dodaj | Kontroler** Aby dodać osobę kontrolera ponownie (Zakończ z wartości).</span><span class="sxs-lookup"><span data-stu-id="56d0f-232">Once the database is updated, right-click the Controller folder and select **Add | Controller** to add the Person controller again (Complete with the same values).</span></span> <span data-ttu-id="56d0f-233">Spowoduje to zaktualizowanie istniejących metod i widoki, dodawanie nowego atrybutu.</span><span class="sxs-lookup"><span data-stu-id="56d0f-233">This will update the existing methods and views adding the new attribute.</span></span>

    <span data-ttu-id="56d0f-234">![Dodawanie aktualizacji kontrolera](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image18.png "dodanie aktualizacji kontrolera")</span><span class="sxs-lookup"><span data-stu-id="56d0f-234">![Adding a controller update](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image18.png "Adding a controller update")</span></span>

    <span data-ttu-id="56d0f-235">*Aktualizacja kontrolera*</span><span class="sxs-lookup"><span data-stu-id="56d0f-235">*Updating the controller*</span></span>
10. <span data-ttu-id="56d0f-236">Kliknij przycisk **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="56d0f-236">Click **Add**.</span></span> <span data-ttu-id="56d0f-237">Następnie wybierz wartości **zastąpić PersonController.cs** i **Zastąp skojarzone widoków** i kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="56d0f-237">Then, select the values **Overwrite PersonController.cs** and the **Overwrite associated views** and click **OK**.</span></span>

   ![Dodawanie Zastąp kontrolera](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image19.png)

   <span data-ttu-id="56d0f-239">*Aktualizacja kontrolera*</span><span class="sxs-lookup"><span data-stu-id="56d0f-239">*Updating the controller*</span></span>

<a id="Ex1Task4"></a>

<a id="Task4-_Running_the_application"></a>
#### <a name="task4--running-the-application"></a><span data-ttu-id="56d0f-240">Task4 — uruchamianie aplikacji</span><span class="sxs-lookup"><span data-stu-id="56d0f-240">Task4- Running the application</span></span>

1. <span data-ttu-id="56d0f-241">Naciśnij klawisz **F5** do uruchomienia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="56d0f-241">Press **F5** to run the application.</span></span>
2. <span data-ttu-id="56d0f-242">Otwórz **/Person**.</span><span class="sxs-lookup"><span data-stu-id="56d0f-242">Open **/Person**.</span></span> <span data-ttu-id="56d0f-243">Należy zauważyć, że dane została zachowana, gdy została dodana kolumna drugie imię.</span><span class="sxs-lookup"><span data-stu-id="56d0f-243">Notice that the data was preserved, while the middle name column was added.</span></span>

    <span data-ttu-id="56d0f-244">![Drugie imię dodano](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image20.png "drugie imię dodane")</span><span class="sxs-lookup"><span data-stu-id="56d0f-244">![Middle Name added](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image20.png "Middle Name added")</span></span>

    <span data-ttu-id="56d0f-245">*Drugie imię dodane*</span><span class="sxs-lookup"><span data-stu-id="56d0f-245">*Middle Name added*</span></span>
3. <span data-ttu-id="56d0f-246">Jeśli klikniesz **Edytuj**, można dodać nazwę środka bieżącego osobie.</span><span class="sxs-lookup"><span data-stu-id="56d0f-246">If you click **Edit**, you will be able to add a middle name to the current person.</span></span>

    <span data-ttu-id="56d0f-247">![Wydanie drugie imię](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image21.png "wydanie drugie imię")</span><span class="sxs-lookup"><span data-stu-id="56d0f-247">![Middle Name edition](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image21.png "Middle Name edition")</span></span>

* * *

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a><span data-ttu-id="56d0f-248">Podsumowanie</span><span class="sxs-lookup"><span data-stu-id="56d0f-248">Summary</span></span>

<span data-ttu-id="56d0f-249">W tym laboratorium praktycznego wiesz proste kroki, aby utworzyć operacji CRUD, za pomocą platformy ASP.NET MVC 4 tworzenia szkieletów przy użyciu dowolnej klasy modelu.</span><span class="sxs-lookup"><span data-stu-id="56d0f-249">In this Hands-On lab, you have learned simple steps to create CRUD operations with ASP.NET MVC 4 Scaffolding using any model class.</span></span> <span data-ttu-id="56d0f-250">Następnie mają przedstawiono sposób przeprowadzenia aktualizacji typu end to end w aplikacji — od bazy danych do widoków — korzystając z migracją architektury jednostek.</span><span class="sxs-lookup"><span data-stu-id="56d0f-250">Then, you have learned how to perform an end to end update in your application -from the database to the views- by using Entity Framework Migrations.</span></span>

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a><span data-ttu-id="56d0f-251">Dodatek A: Instalowanie programu Visual Studio Express 2012 for Web</span><span class="sxs-lookup"><span data-stu-id="56d0f-251">Appendix A: Installing Visual Studio Express 2012 for Web</span></span>

<span data-ttu-id="56d0f-252">Możesz zainstalować **programu Microsoft Visual Studio Express 2012 for Web** lub inne &quot;Express&quot; przy użyciu wersji **[Instalatora platformy sieci Web firmy Microsoft](https://www.microsoft.com/web/downloads/platform.aspx)**.</span><span class="sxs-lookup"><span data-stu-id="56d0f-252">You can install **Microsoft Visual Studio Express 2012 for Web** or another &quot;Express&quot; version using the **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**.</span></span> <span data-ttu-id="56d0f-253">Poniższe wskazówki ułatwiają kroki wymagane do zainstalowania *programu Visual studio Express 2012 for Web* przy użyciu *Instalatora platformy sieci Web firmy Microsoft*.</span><span class="sxs-lookup"><span data-stu-id="56d0f-253">The following instructions guide you through the steps required to install *Visual studio Express 2012 for Web* using *Microsoft Web Platform Installer*.</span></span>

1. <span data-ttu-id="56d0f-254">Przejdź do [ [ https://go.microsoft.com/? linkid = 9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169).</span><span class="sxs-lookup"><span data-stu-id="56d0f-254">Go to [[https://go.microsoft.com/? linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169).</span></span> <span data-ttu-id="56d0f-255">Alternatywnie, jeśli już zainstalowano Instalatora platformy sieci Web, możesz otworzyć go i Wyszukaj produkt &quot; <em>Visual Studio Express 2012 for Web z zestawem Windows Azure SDK</em>&quot;.</span><span class="sxs-lookup"><span data-stu-id="56d0f-255">Alternatively, if you already have installed Web Platform Installer, you can open it and search for the product &quot;<em>Visual Studio Express 2012 for Web with Windows Azure SDK</em>&quot;.</span></span>
2. <span data-ttu-id="56d0f-256">Kliknij pozycję **Zainstaluj teraz**.</span><span class="sxs-lookup"><span data-stu-id="56d0f-256">Click on **Install Now**.</span></span> <span data-ttu-id="56d0f-257">Jeśli nie masz **Instalatora platformy sieci Web** nastąpi przekierowanie do pobrania i zainstalowania go najpierw.</span><span class="sxs-lookup"><span data-stu-id="56d0f-257">If you do not have **Web Platform Installer** you will be redirected to download and install it first.</span></span>
3. <span data-ttu-id="56d0f-258">Raz **Instalatora platformy sieci Web** jest otwarty, kliknij przycisk **zainstalować** można uruchomić Instalatora.</span><span class="sxs-lookup"><span data-stu-id="56d0f-258">Once **Web Platform Installer** is open, click **Install** to start the setup.</span></span>

    <span data-ttu-id="56d0f-259">![Zainstaluj program Visual Studio Express](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image22.png "zainstalować program Visual Studio Express")</span><span class="sxs-lookup"><span data-stu-id="56d0f-259">![Install Visual Studio Express](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image22.png "Install Visual Studio Express")</span></span>

    <span data-ttu-id="56d0f-260">*Zainstaluj program Visual Studio Express*</span><span class="sxs-lookup"><span data-stu-id="56d0f-260">*Install Visual Studio Express*</span></span>
4. <span data-ttu-id="56d0f-261">Odczytywanie wszystkich produktów licencji oraz warunki, a następnie kliknij przycisk **akceptuję** aby kontynuować.</span><span class="sxs-lookup"><span data-stu-id="56d0f-261">Read all the products' licenses and terms and click **I Accept** to continue.</span></span>

    ![Akceptowanie umowy licencyjnej](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image23.png)

    <span data-ttu-id="56d0f-263">*Akceptowanie umowy licencyjnej*</span><span class="sxs-lookup"><span data-stu-id="56d0f-263">*Accepting the license terms*</span></span>
5. <span data-ttu-id="56d0f-264">Zaczekaj, aż do zakończenia procesu pobierania i instalacji.</span><span class="sxs-lookup"><span data-stu-id="56d0f-264">Wait until the downloading and installation process completes.</span></span>

    ![Postęp instalacji](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image24.png)

    <span data-ttu-id="56d0f-266">*Postęp instalacji*</span><span class="sxs-lookup"><span data-stu-id="56d0f-266">*Installation progress*</span></span>
6. <span data-ttu-id="56d0f-267">Po zakończeniu instalacji kliknij przycisk **Zakończ**.</span><span class="sxs-lookup"><span data-stu-id="56d0f-267">When the installation completes, click **Finish**.</span></span>

    ![Instalacja została zakończona](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image25.png)

    <span data-ttu-id="56d0f-269">*Instalacja została zakończona*</span><span class="sxs-lookup"><span data-stu-id="56d0f-269">*Installation completed*</span></span>
7. <span data-ttu-id="56d0f-270">Kliknij przycisk **zakończenia** zamknąć Instalatora platformy sieci Web.</span><span class="sxs-lookup"><span data-stu-id="56d0f-270">Click **Exit** to close Web Platform Installer.</span></span>
8. <span data-ttu-id="56d0f-271">Aby otworzyć program Visual Studio Express for Web, przejdź do **Start** ekranu, a następnie zacznij pisać &quot; **VS Express**&quot;, następnie kliknij pozycję **VS Express for Web** Kafelek.</span><span class="sxs-lookup"><span data-stu-id="56d0f-271">To open Visual Studio Express for Web, go to the **Start** screen and start writing &quot;**VS Express**&quot;, then click on the **VS Express for Web** tile.</span></span>

    ![VS Express for Web kafelka](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image26.png)

    <span data-ttu-id="56d0f-273">*VS Express for Web kafelka*</span><span class="sxs-lookup"><span data-stu-id="56d0f-273">*VS Express for Web tile*</span></span>

<a id="AppendixB"></a>

<a id="Appendix_B_Using_Code_Snippets"></a>
## <a name="appendix-b-using-code-snippets"></a><span data-ttu-id="56d0f-274">Dodatek B: korzystania z fragmentów kodu</span><span class="sxs-lookup"><span data-stu-id="56d0f-274">Appendix B: Using Code Snippets</span></span>

<span data-ttu-id="56d0f-275">Za pomocą wstawek kodu masz kod, czego potrzebujesz w zasięgu ręki.</span><span class="sxs-lookup"><span data-stu-id="56d0f-275">With code snippets, you have all the code you need at your fingertips.</span></span> <span data-ttu-id="56d0f-276">Dokument laboratorium informuje o dokładnie podczas korzystania z nich, jak pokazano na poniższej ilustracji.</span><span class="sxs-lookup"><span data-stu-id="56d0f-276">The lab document will tell you exactly when you can use them, as shown in the following figure.</span></span>

<span data-ttu-id="56d0f-277">![Za pomocą wstawek kodu programu Visual Studio, aby wstawić kod do projektu](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image27.png "fragmenty kodu za pomocą programu Visual Studio, aby wstawić kod do projektu")</span><span class="sxs-lookup"><span data-stu-id="56d0f-277">![Using Visual Studio code snippets to insert code into your project](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image27.png "Using Visual Studio code snippets to insert code into your project")</span></span>

<span data-ttu-id="56d0f-278">*Za pomocą wstawek kodu programu Visual Studio, aby wstawić kod do projektu*</span><span class="sxs-lookup"><span data-stu-id="56d0f-278">*Using Visual Studio code snippets to insert code into your project*</span></span>

<span data-ttu-id="56d0f-279">***Aby dodać wstawki kodu za pomocą klawiatury (tylko język C#)***</span><span class="sxs-lookup"><span data-stu-id="56d0f-279">***To add a code snippet using the keyboard (C# only)***</span></span>

1. <span data-ttu-id="56d0f-280">Umieść kursor, w którym chcesz wstawić kod.</span><span class="sxs-lookup"><span data-stu-id="56d0f-280">Place the cursor where you would like to insert the code.</span></span>
2. <span data-ttu-id="56d0f-281">Rozpocznij wpisywanie nazwy fragmentu kodu (bez spacji i łączniki).</span><span class="sxs-lookup"><span data-stu-id="56d0f-281">Start typing the snippet name (without spaces or hyphens).</span></span>
3. <span data-ttu-id="56d0f-282">Obejrzyj, jak technologia IntelliSense wyświetla zgodne z nazwami fragmenty kodu.</span><span class="sxs-lookup"><span data-stu-id="56d0f-282">Watch as IntelliSense displays matching snippets' names.</span></span>
4. <span data-ttu-id="56d0f-283">Wybierz prawidłowy fragment kodu lub pisz dalej do momentu wybrania debugowanej nazwy całego fragmentu kodu.</span><span class="sxs-lookup"><span data-stu-id="56d0f-283">Select the correct snippet (or keep typing until the entire snippet's name is selected).</span></span>
5. <span data-ttu-id="56d0f-284">Naciśnij klawisz Tab dwa razy, aby wstawić fragment kodu w lokalizacji kursora.</span><span class="sxs-lookup"><span data-stu-id="56d0f-284">Press the Tab key twice to insert the snippet at the cursor location.</span></span>

<span data-ttu-id="56d0f-285">![Rozpocznij wpisywanie nazwy fragmentu kodu](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image28.png "Rozpocznij wpisywanie nazwy fragmentu kodu")</span><span class="sxs-lookup"><span data-stu-id="56d0f-285">![Start typing the snippet name](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image28.png "Start typing the snippet name")</span></span>

<span data-ttu-id="56d0f-286">*Rozpocznij wpisywanie nazwy fragmentu kodu*</span><span class="sxs-lookup"><span data-stu-id="56d0f-286">*Start typing the snippet name*</span></span>

<span data-ttu-id="56d0f-287">![Naciśnij klawisz Tab, aby zaznaczyć fragment wyróżnione](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image29.png "naciśnij klawisz Tab, aby wybrać wyróżnione fragmentu kodu")</span><span class="sxs-lookup"><span data-stu-id="56d0f-287">![Press Tab to select the highlighted snippet](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image29.png "Press Tab to select the highlighted snippet")</span></span>

<span data-ttu-id="56d0f-288">*Naciśnij klawisz Tab, aby wybrać wyróżnione fragment kodu*</span><span class="sxs-lookup"><span data-stu-id="56d0f-288">*Press Tab to select the highlighted snippet*</span></span>

<span data-ttu-id="56d0f-289">![Ponownie naciśnij klawisz Tab i fragment kodu zostanie rozwinięte](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image30.png "rozwinie ponownie naciśnij klawisz Tab i fragment kodu")</span><span class="sxs-lookup"><span data-stu-id="56d0f-289">![Press Tab again and the snippet will expand](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image30.png "Press Tab again and the snippet will expand")</span></span>

<span data-ttu-id="56d0f-290">*Ponownie naciśnij klawisz Tab i fragment kodu zostanie rozwinięte.*</span><span class="sxs-lookup"><span data-stu-id="56d0f-290">*Press Tab again and the snippet will expand*</span></span>

<span data-ttu-id="56d0f-291">***Aby dodać wstawki kodu za pomocą myszy (C#, Visual Basic i XML)*** 1.</span><span class="sxs-lookup"><span data-stu-id="56d0f-291">***To add a code snippet using the mouse (C#, Visual Basic and XML)*** 1.</span></span> <span data-ttu-id="56d0f-292">Kliknij prawym przyciskiem myszy, gdy chcesz wstawić fragment kodu.</span><span class="sxs-lookup"><span data-stu-id="56d0f-292">Right-click where you want to insert the code snippet.</span></span>

1. <span data-ttu-id="56d0f-293">Wybierz **Wstaw fragment kodu** następuje **Moje fragmenty kodu**.</span><span class="sxs-lookup"><span data-stu-id="56d0f-293">Select **Insert Snippet** followed by **My Code Snippets**.</span></span>
2. <span data-ttu-id="56d0f-294">Wybierz odpowiedni fragment z listy, klikając go.</span><span class="sxs-lookup"><span data-stu-id="56d0f-294">Pick the relevant snippet from the list, by clicking on it.</span></span>

<span data-ttu-id="56d0f-295">![Kliknij prawym przyciskiem myszy, które chcesz wstawić fragment kodu i wybierz opcję Wstaw fragment kodu](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image31.png "kliknij prawym przyciskiem myszy, które chcesz wstawić fragment kodu i wybierz opcję Wstaw fragment kodu")</span><span class="sxs-lookup"><span data-stu-id="56d0f-295">![Right-click where you want to insert the code snippet and select Insert Snippet](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image31.png "Right-click where you want to insert the code snippet and select Insert Snippet")</span></span>

<span data-ttu-id="56d0f-296">*Kliknij prawym przyciskiem myszy, które chcesz wstawić fragment kodu i wybierz opcję Wstaw fragment kodu*</span><span class="sxs-lookup"><span data-stu-id="56d0f-296">*Right-click where you want to insert the code snippet and select Insert Snippet*</span></span>

<span data-ttu-id="56d0f-297">![Wybierz odpowiedni fragment z listy, klikając go](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image32.png "wybierz odpowiedni fragment z listy, klikając go")</span><span class="sxs-lookup"><span data-stu-id="56d0f-297">![Pick the relevant snippet from the list, by clicking on it](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image32.png "Pick the relevant snippet from the list, by clicking on it")</span></span>

<span data-ttu-id="56d0f-298">*Wybierz odpowiedni fragment z listy, klikając go*</span><span class="sxs-lookup"><span data-stu-id="56d0f-298">*Pick the relevant snippet from the list, by clicking on it*</span></span>
