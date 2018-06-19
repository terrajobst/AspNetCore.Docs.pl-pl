---
uid: mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-models-and-data-access
title: Modele platformy ASP.NET MVC 4 oraz dostęp do danych | Dokumentacja firmy Microsoft
author: rick-anderson
description: 'Uwaga: W tym laboratorium Hands-on przyjęto założenie, że masz podstawową wiedzę na temat platformy ASP.NET MVC. Jeśli nie użyto programu ASP.NET MVC przed, zalecamy zapoznać się z platformy ASP.NET MVC 4...'
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2013
ms.topic: article
ms.assetid: 634ea84b-f904-4afe-b71b-49cccef4d9cc
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-models-and-data-access
msc.type: authoredcontent
ms.openlocfilehash: 88b3316b116962dd35031f4b971dbfe31ed0e010
ms.sourcegitcommit: 3a893ae05f010656d99d6ddf55e82f1b5b6933bc
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/18/2018
ms.locfileid: "34306741"
---
# <a name="aspnet-mvc-4-models-and-data-access"></a><span data-ttu-id="8bfca-104">Modele platformy ASP.NET MVC 4 oraz dostęp do danych</span><span class="sxs-lookup"><span data-stu-id="8bfca-104">ASP.NET MVC 4 Models and Data Access</span></span>

<span data-ttu-id="8bfca-105">Przez [obozów sieci Web Team](https://twitter.com/webcamps)</span><span class="sxs-lookup"><span data-stu-id="8bfca-105">By [Web Camps Team](https://twitter.com/webcamps)</span></span>

[<span data-ttu-id="8bfca-106">Pobierz obozów sieci Web uczenie Kit</span><span class="sxs-lookup"><span data-stu-id="8bfca-106">Download Web Camps Training Kit</span></span>](https://aka.ms/webcamps-training-kit)

<span data-ttu-id="8bfca-107">W tym laboratorium Hands-on zakłada mieć podstawową wiedzę na temat **ASP.NET MVC**.</span><span class="sxs-lookup"><span data-stu-id="8bfca-107">This Hands-on Lab assumes you have basic knowledge of **ASP.NET MVC**.</span></span> <span data-ttu-id="8bfca-108">Jeśli nie używasz **ASP.NET MVC** przed, zalecamy zapoznać się z **podstawowe informacje na temat platformy ASP.NET MVC 4** Hands-on laboratorium.</span><span class="sxs-lookup"><span data-stu-id="8bfca-108">If you have not used **ASP.NET MVC** before, we recommend you to go over **ASP.NET MVC 4 Fundamentals** Hands-on Lab.</span></span>

<span data-ttu-id="8bfca-109">W tym laboratorium przeprowadzi Cię przez rozszerzenia oraz nowe funkcje opisane wcześniej przez zastosowanie drobne zmiany do przykładowej aplikacji sieci Web w folderze źródłowym.</span><span class="sxs-lookup"><span data-stu-id="8bfca-109">This lab walks you through the enhancements and new features previously described by applying minor changes to a sample Web application provided in the Source folder.</span></span>

> [!NOTE]
> <span data-ttu-id="8bfca-110">Wszystkie przykładowy kod i fragmenty kodu są uwzględnione w sieci Web obozów zestaw szkoleniowy, dostępne pod adresem [wersje Microsoft-Web/WebCampTrainingKit](https://aka.ms/webcamps-training-kit).</span><span class="sxs-lookup"><span data-stu-id="8bfca-110">All sample code and snippets are included in the Web Camps Training Kit, available at [Microsoft-Web/WebCampTrainingKit Releases](https://aka.ms/webcamps-training-kit).</span></span> <span data-ttu-id="8bfca-111">Specyficzne dla tego laboratorium projektu jest dostępna na [ASP.NET MVC 4 modeli i dostęp do danych](https://github.com/Microsoft-Web/HOL-MVC4ModelsAndDataAccess).</span><span class="sxs-lookup"><span data-stu-id="8bfca-111">The project specific to this lab is available at [ASP.NET MVC 4 Models and Data Access](https://github.com/Microsoft-Web/HOL-MVC4ModelsAndDataAccess).</span></span>

<span data-ttu-id="8bfca-112">W **podstawowe informacje na temat platformy ASP.NET MVC** laboratorium Hands-on można mieć zostały przekazywanie ustalony danych z kontrolerów do widoku szablonów.</span><span class="sxs-lookup"><span data-stu-id="8bfca-112">In **ASP.NET MVC Fundamentals** Hands-on Lab, you have been passing hard-coded data from the Controllers to the View templates.</span></span> <span data-ttu-id="8bfca-113">Jednak aby można było tworzyć rzeczywista aplikacji sieci Web, możesz chcieć użyć rzeczywistych bazy danych.</span><span class="sxs-lookup"><span data-stu-id="8bfca-113">But, in order to build a real Web application, you might want to use a real database.</span></span>

<span data-ttu-id="8bfca-114">W tym laboratorium Hands-on będzie pokazują, jak korzystać z aparatu bazy danych w celu przechowywania i pobierania danych potrzebnych do aplikacji Sklep utworów muzycznych.</span><span class="sxs-lookup"><span data-stu-id="8bfca-114">This Hands-on Lab will show you how to use a database engine in order to store and retrieve the data needed for the Music Store application.</span></span> <span data-ttu-id="8bfca-115">Do wykonania, które będzie rozpoczynać się od istniejącej bazy danych i tworzenia modelu Entity Data Model z niego.</span><span class="sxs-lookup"><span data-stu-id="8bfca-115">To accomplish that, you will start with an existing database and create the Entity Data Model from it.</span></span> <span data-ttu-id="8bfca-116">W tym laboratorium będzie spełniać **Database First** podejście, jak również **Code First** podejście.</span><span class="sxs-lookup"><span data-stu-id="8bfca-116">Throughout this lab, you will meet the **Database First** approach as well as the **Code First** approach.</span></span>

<span data-ttu-id="8bfca-117">Jednak umożliwia także **Model First** podejścia, utwórz ten sam model przy użyciu narzędzia, a następnie wygeneruj bazy danych z niego.</span><span class="sxs-lookup"><span data-stu-id="8bfca-117">However, you can also use the **Model First** approach, create the same model using the tools, and then generate the database from it.</span></span>

<span data-ttu-id="8bfca-118">![Pierwszy vs bazy danych. Model pierwszy](aspnet-mvc-4-models-and-data-access/_static/image1.png "bazy danych pierwszej wersji programu vs. Najpierw modelu")</span><span class="sxs-lookup"><span data-stu-id="8bfca-118">![Database First vs. Model First](aspnet-mvc-4-models-and-data-access/_static/image1.png "Database First vs. Model First")</span></span>

<span data-ttu-id="8bfca-119">*Pierwszy vs bazy danych. Najpierw modelu*</span><span class="sxs-lookup"><span data-stu-id="8bfca-119">*Database First vs. Model First*</span></span>

<span data-ttu-id="8bfca-120">Po wygenerowaniu modelu, można utworzyć odpowiednie korekty w StoreController dostarczanie widoków magazynu danych pobranych z bazy danych, zamiast ustalony danych.</span><span class="sxs-lookup"><span data-stu-id="8bfca-120">After generating the Model, you will make the proper adjustments in the StoreController to provide the Store Views with the data taken from the database, instead of using hard-coded data.</span></span> <span data-ttu-id="8bfca-121">Nie należy wprowadzać żadnych zmian widoku szablonów ponieważ StoreController będzie zwracać tego samego ViewModels szablony widoku, mimo że teraz dane będą pobierane z bazy danych.</span><span class="sxs-lookup"><span data-stu-id="8bfca-121">You will not need to make any change to the View templates because the StoreController will be returning the same ViewModels to the View templates, although this time the data will come from the database.</span></span>

<span data-ttu-id="8bfca-122">**Pierwszym sposobem kodu**</span><span class="sxs-lookup"><span data-stu-id="8bfca-122">**The Code First Approach**</span></span>

<span data-ttu-id="8bfca-123">Code First podejście pozwala zdefiniować modelu z kodu bez generowania klasy, które zwykle są powiązane z architekturą.</span><span class="sxs-lookup"><span data-stu-id="8bfca-123">The Code First approach allows us to define the model from the code without generating classes that are generally coupled with the framework.</span></span>

<span data-ttu-id="8bfca-124">W kodzie, najpierw obiekty modelu są zdefiniowane z POCOs, &quot;zwykły stare obiekty CLR&quot;.</span><span class="sxs-lookup"><span data-stu-id="8bfca-124">In code first, model objects are defined with POCOs, &quot;Plain Old CLR Objects&quot;.</span></span> <span data-ttu-id="8bfca-125">POCOs są proste klasy zwykły, nie dziedziczenia, które nie implementują interfejsy.</span><span class="sxs-lookup"><span data-stu-id="8bfca-125">POCOs are simple plain classes that have no inheritance and do not implement interfaces.</span></span> <span data-ttu-id="8bfca-126">Firma Microsoft może automatycznie generować bazy danych z ich lub możemy Użyj istniejącej bazy danych i Generowanie mapowania klasy z kodu.</span><span class="sxs-lookup"><span data-stu-id="8bfca-126">We can automatically generate the database from them, or we can use an existing database and generate the class mapping from the code.</span></span>

<span data-ttu-id="8bfca-127">Korzyści wynikające ze stosowania tego podejścia jest modelu pozostaje niezależnie od framework trwałości (w tym przypadku Entity Framework), zgodnie z klasy POCOs nie są połączone z framework mapowania.</span><span class="sxs-lookup"><span data-stu-id="8bfca-127">The benefits of using this approach is that the Model remains independent from the persistence framework (in this case, Entity Framework), as the POCOs classes are not coupled with the mapping framework.</span></span>

> [!NOTE]
> <span data-ttu-id="8bfca-128">To laboratorium jest oparte na programie ASP.NET MVC 4 i wersji magazynu utworów muzycznych przykładowej aplikacji dostosowanej zminimalizowany, aby dopasować tylko te funkcje, które są wyświetlane w tym laboratorium Hands-On.</span><span class="sxs-lookup"><span data-stu-id="8bfca-128">This Lab is based on ASP.NET MVC 4 and a version of the Music Store sample application customized and minimized to fit only the features shown in this Hands-On Lab.</span></span>
> 
> <span data-ttu-id="8bfca-129">Jeśli chcesz zapoznać się z całym **sklep muzyczny** samouczek aplikacji można znaleźć w [MVC utworów muzycznych magazynu](https://github.com/evilDave/MVC-Music-Store).</span><span class="sxs-lookup"><span data-stu-id="8bfca-129">If you wish to explore the whole **Music Store** tutorial application you can find it in [MVC-Music-Store](https://github.com/evilDave/MVC-Music-Store).</span></span>


<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a><span data-ttu-id="8bfca-130">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="8bfca-130">Prerequisites</span></span>

<span data-ttu-id="8bfca-131">Musi mieć następujące elementy do przygotowania tego laboratorium:</span><span class="sxs-lookup"><span data-stu-id="8bfca-131">You must have the following items to complete this lab:</span></span>

- <span data-ttu-id="8bfca-132">[Microsoft Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) lub wyższego poziomu (odczytu [dodatek a.](#AppendixA) instrukcje dotyczące sposobu jego instalacji).</span><span class="sxs-lookup"><span data-stu-id="8bfca-132">[Microsoft Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) or superior (read [Appendix A](#AppendixA) for instructions on how to install it).</span></span>

<a id="Setup"></a>

<a id="Setup"></a>
### <a name="setup"></a><span data-ttu-id="8bfca-133">Konfiguracja</span><span class="sxs-lookup"><span data-stu-id="8bfca-133">Setup</span></span>

<span data-ttu-id="8bfca-134">**Instalowanie wstawki kodu**</span><span class="sxs-lookup"><span data-stu-id="8bfca-134">**Installing Code Snippets**</span></span>

<span data-ttu-id="8bfca-135">Dla wygody taki kod, który będzie zarządzany wzdłuż tego laboratorium jest dostępna jako wstawki kodu programu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="8bfca-135">For convenience, much of the code you will be managing along this lab is available as Visual Studio code snippets.</span></span> <span data-ttu-id="8bfca-136">Aby zainstalować fragmenty kodu, uruchom **.\Source\Setup\CodeSnippets.vsi** pliku.</span><span class="sxs-lookup"><span data-stu-id="8bfca-136">To install the code snippets run **.\Source\Setup\CodeSnippets.vsi** file.</span></span>

<span data-ttu-id="8bfca-137">Jeśli nie masz doświadczenia z wstawki programu Visual Studio i chcesz dowiedzieć się, jak ich używać, można odwołać się do dodatku z tego dokumentu &quot; [wstawki kodu za pomocą programu dodatek C:](#AppendixC)&quot;.</span><span class="sxs-lookup"><span data-stu-id="8bfca-137">If you are not familiar with the Visual Studio Code Snippets, and want to learn how to use them, you can refer to the appendix from this document &quot;[Appendix C: Using Code Snippets](#AppendixC)&quot;.</span></span>

* * *

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a><span data-ttu-id="8bfca-138">Ćwiczenia</span><span class="sxs-lookup"><span data-stu-id="8bfca-138">Exercises</span></span>

<span data-ttu-id="8bfca-139">W tym laboratorium Hands-on składa się przez następujących czynnościach:</span><span class="sxs-lookup"><span data-stu-id="8bfca-139">This Hands-on Lab is comprised by the following exercises:</span></span>

1. [<span data-ttu-id="8bfca-140">Ćwiczenie 1: Dodawanie bazy danych</span><span class="sxs-lookup"><span data-stu-id="8bfca-140">Exercise 1: Adding a Database</span></span>](#Exercise1)
2. [<span data-ttu-id="8bfca-141">Ćwiczenie 2: Utworzenie bazy danych za pomocą funkcji Code First</span><span class="sxs-lookup"><span data-stu-id="8bfca-141">Exercise 2: Creating a Database using Code First</span></span>](#Exercise2)
3. [<span data-ttu-id="8bfca-142">Ćwiczenie 3: Zapytanie bazy danych z parametrami</span><span class="sxs-lookup"><span data-stu-id="8bfca-142">Exercise 3: Querying the Database with Parameters</span></span>](#Exercise3)

> [!NOTE]
> <span data-ttu-id="8bfca-143">Towarzyszy każdego wykonywania **zakończenia** folderu zawierającego wynikowy rozwiązanie, należy uzyskać po wykonaniu ćwiczeniach.</span><span class="sxs-lookup"><span data-stu-id="8bfca-143">Each exercise is accompanied by an **End** folder containing the resulting solution you should obtain after completing the exercises.</span></span> <span data-ttu-id="8bfca-144">Jeśli potrzebujesz dodatkowej pomocy pracuje nad ćwiczeniami, można użyć tego rozwiązania jako przewodnika.</span><span class="sxs-lookup"><span data-stu-id="8bfca-144">You can use this solution as a guide if you need additional help working through the exercises.</span></span>


<span data-ttu-id="8bfca-145">Szacowany czas trwania tego laboratorium: **35 minut**.</span><span class="sxs-lookup"><span data-stu-id="8bfca-145">Estimated time to complete this lab: **35 minutes**.</span></span>

<a id="Exercise1"></a>

<a id="Exercise_1_Adding_a_Database"></a>
### <a name="exercise-1-adding-a-database"></a><span data-ttu-id="8bfca-146">Ćwiczenie 1: Dodawanie bazy danych</span><span class="sxs-lookup"><span data-stu-id="8bfca-146">Exercise 1: Adding a Database</span></span>

<span data-ttu-id="8bfca-147">W tym ćwiczeniu dowiesz się, jak dodać bazy danych z tabel aplikacji MusicStore do rozwiązania, aby można było korzystać z jego dane.</span><span class="sxs-lookup"><span data-stu-id="8bfca-147">In this exercise, you will learn how to add a database with the tables of the MusicStore application to the solution in order to consume its data.</span></span> <span data-ttu-id="8bfca-148">Po bazy danych jest generowany z modelem i dodany do rozwiązania, należy zmodyfikować klasy StoreController dostarczanie szablon widoku danych pobranych z bazy danych, zamiast zakodowanych wartości.</span><span class="sxs-lookup"><span data-stu-id="8bfca-148">Once the database is generated with the model, and added to the solution, you will modify the StoreController class to provide the View template with the data taken from the database, instead of using hard-coded values.</span></span>

<a id="Ex1Task1"></a>

<a id="Task_1_-_Adding_a_Database"></a>
#### <a name="task-1---adding-a-database"></a><span data-ttu-id="8bfca-149">Zadanie 1 — Dodawanie bazy danych</span><span class="sxs-lookup"><span data-stu-id="8bfca-149">Task 1 - Adding a Database</span></span>

<span data-ttu-id="8bfca-150">To zadanie spowoduje dodanie już utworzono bazę danych z głównych tabel aplikacji MusicStore do rozwiązania.</span><span class="sxs-lookup"><span data-stu-id="8bfca-150">In this task, you will add an already created database with the main tables of the MusicStore application to the solution.</span></span>

1. <span data-ttu-id="8bfca-151">Otwórz **rozpocząć** rozwiązania, znajdujących się na **źródło/Ex1-AddingADatabaseDBFirst/Begin/** folderu.</span><span class="sxs-lookup"><span data-stu-id="8bfca-151">Open the **Begin** solution located at **Source/Ex1-AddingADatabaseDBFirst/Begin/** folder.</span></span>

   1. <span data-ttu-id="8bfca-152">Należy pobrać niektórych brakujących pakietów NuGet aby kontynuować.</span><span class="sxs-lookup"><span data-stu-id="8bfca-152">You will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="8bfca-153">Aby to zrobić, kliknij przycisk **projektu** menu i wybierz **Zarządzaj pakietami NuGet**.</span><span class="sxs-lookup"><span data-stu-id="8bfca-153">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="8bfca-154">W **Zarządzaj pakietami NuGet** okna dialogowego, kliknij przycisk **przywrócić** celu pobieranie brakujących pakietów.</span><span class="sxs-lookup"><span data-stu-id="8bfca-154">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="8bfca-155">Na koniec Skompiluj rozwiązanie, klikając **kompilacji** | **Kompiluj rozwiązanie**.</span><span class="sxs-lookup"><span data-stu-id="8bfca-155">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="8bfca-156">Jedną z zalet przy użyciu narzędzia NuGet jest, że nie masz do wysłania wszystkich bibliotek w projekcie, zmniejszenie jego rozmiar projektu.</span><span class="sxs-lookup"><span data-stu-id="8bfca-156">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="8bfca-157">Narzędzia Power NuGet określając wersje pakietów w pliku Packages.config, będzie można pobrać wymaganych bibliotek podczas pierwszego uruchomienia projektu.</span><span class="sxs-lookup"><span data-stu-id="8bfca-157">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="8bfca-158">Jest to, dlaczego konieczne będzie wykonanie tych kroków, po otwarciu istniejącego rozwiązania z tego laboratorium.</span><span class="sxs-lookup"><span data-stu-id="8bfca-158">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="8bfca-159">Dodaj **MvcMusicStore** pliku bazy danych.</span><span class="sxs-lookup"><span data-stu-id="8bfca-159">Add **MvcMusicStore** database file.</span></span> <span data-ttu-id="8bfca-160">W tym laboratorium Hands-on użyje utworzone bazę danych o nazwie **MvcMusicStore.mdf**.</span><span class="sxs-lookup"><span data-stu-id="8bfca-160">In this Hands-on Lab, you will use an already created database called **MvcMusicStore.mdf**.</span></span> <span data-ttu-id="8bfca-161">W tym celu kliknij prawym przyciskiem myszy **aplikacji\_danych** folderu, wskaż **Dodaj** , a następnie kliknij przycisk **istniejący element**.</span><span class="sxs-lookup"><span data-stu-id="8bfca-161">To do that, right-click **App\_Data** folder, point to **Add** and then click **Existing Item**.</span></span> <span data-ttu-id="8bfca-162">Przejdź do **\Source\Assets** i wybierz **MvcMusicStore.mdf** pliku.</span><span class="sxs-lookup"><span data-stu-id="8bfca-162">Browse to **\Source\Assets** and select the **MvcMusicStore.mdf** file.</span></span>

    <span data-ttu-id="8bfca-163">![Dodawanie istniejącego elementu](aspnet-mvc-4-models-and-data-access/_static/image2.png "Dodawanie istniejącego elementu")</span><span class="sxs-lookup"><span data-stu-id="8bfca-163">![Adding an Existing Item](aspnet-mvc-4-models-and-data-access/_static/image2.png "Adding an Existing Item")</span></span>

    <span data-ttu-id="8bfca-164">*Dodawanie istniejącego elementu*</span><span class="sxs-lookup"><span data-stu-id="8bfca-164">*Adding an Existing Item*</span></span>

    <span data-ttu-id="8bfca-165">![Plik bazy danych MvcMusicStore.mdf](aspnet-mvc-4-models-and-data-access/_static/image3.png "MvcMusicStore.mdf pliku bazy danych")</span><span class="sxs-lookup"><span data-stu-id="8bfca-165">![MvcMusicStore.mdf database file](aspnet-mvc-4-models-and-data-access/_static/image3.png "MvcMusicStore.mdf database file")</span></span>

    <span data-ttu-id="8bfca-166">*MvcMusicStore.mdf pliku bazy danych*</span><span class="sxs-lookup"><span data-stu-id="8bfca-166">*MvcMusicStore.mdf database file*</span></span>

    <span data-ttu-id="8bfca-167">Bazy danych dodano do projektu.</span><span class="sxs-lookup"><span data-stu-id="8bfca-167">The database has been added to the project.</span></span> <span data-ttu-id="8bfca-168">Nawet wtedy, gdy baza danych znajduje się wewnątrz rozwiązania, możesz znaleźć, a następnie go zaktualizować, ponieważ została ona hostowana na serwerze z inną bazą danych.</span><span class="sxs-lookup"><span data-stu-id="8bfca-168">Even when the database is located inside the solution, you can query and update it as it was hosted in a different database server.</span></span>

    <span data-ttu-id="8bfca-169">![Baza danych MvcMusicStore w Eksploratorze rozwiązań](aspnet-mvc-4-models-and-data-access/_static/image4.png "MvcMusicStore bazy danych, w Eksploratorze rozwiązań")</span><span class="sxs-lookup"><span data-stu-id="8bfca-169">![MvcMusicStore database in Solution Explorer](aspnet-mvc-4-models-and-data-access/_static/image4.png "MvcMusicStore database in Solution Explorer")</span></span>

    <span data-ttu-id="8bfca-170">*Baza danych MvcMusicStore w Eksploratorze rozwiązań*</span><span class="sxs-lookup"><span data-stu-id="8bfca-170">*MvcMusicStore database in Solution Explorer*</span></span>
3. <span data-ttu-id="8bfca-171">Sprawdź połączenie z bazą danych.</span><span class="sxs-lookup"><span data-stu-id="8bfca-171">Verify the connection to the database.</span></span> <span data-ttu-id="8bfca-172">Aby to zrobić, kliknij dwukrotnie **MvcMusicStore.mdf** ustanowić połączenie.</span><span class="sxs-lookup"><span data-stu-id="8bfca-172">To do this, double-click **MvcMusicStore.mdf** to establish a connection.</span></span>

    <span data-ttu-id="8bfca-173">![Łączenie z MvcMusicStore.mdf](aspnet-mvc-4-models-and-data-access/_static/image5.png "nawiązywania MvcMusicStore.mdf")</span><span class="sxs-lookup"><span data-stu-id="8bfca-173">![Connecting to MvcMusicStore.mdf](aspnet-mvc-4-models-and-data-access/_static/image5.png "Connecting to MvcMusicStore.mdf")</span></span>

    <span data-ttu-id="8bfca-174">*Łączenie z MvcMusicStore.mdf*</span><span class="sxs-lookup"><span data-stu-id="8bfca-174">*Connecting to MvcMusicStore.mdf*</span></span>

<a id="Ex1Task2"></a>

<a id="Task_2_-_Creating_a_Data_Model"></a>
#### <a name="task-2---creating-a-data-model"></a><span data-ttu-id="8bfca-175">Zadanie 2 — Tworzenie modelu danych</span><span class="sxs-lookup"><span data-stu-id="8bfca-175">Task 2 - Creating a Data Model</span></span>

<span data-ttu-id="8bfca-176">W ramach tego zadania spowoduje utworzenie modelu danych do interakcji z dodanym w poprzednim zadaniu bazy danych.</span><span class="sxs-lookup"><span data-stu-id="8bfca-176">In this task, you will create a data model to interact with the database added in the previous task.</span></span>

1. <span data-ttu-id="8bfca-177">Tworzenie modelu danych, która będzie reprezentowała bazy danych.</span><span class="sxs-lookup"><span data-stu-id="8bfca-177">Create a data model that will represent the database.</span></span> <span data-ttu-id="8bfca-178">Aby to zrobić, w Eksploratorze rozwiązań kliknij prawym przyciskiem myszy **modele** folderu, wskaż **Dodaj** , a następnie kliknij przycisk **nowy element**.</span><span class="sxs-lookup"><span data-stu-id="8bfca-178">To do this, in Solution Explorer right-click the **Models** folder, point to **Add** and then click **New Item**.</span></span> <span data-ttu-id="8bfca-179">W **Dodaj nowy element** okno dialogowe, wybierz opcję **danych** szablonu, a następnie **modelu danych jednostki ADO.NET** elementu.</span><span class="sxs-lookup"><span data-stu-id="8bfca-179">In the **Add New Item** dialog, select the **Data** template and then the **ADO.NET Entity Data Model** item.</span></span> <span data-ttu-id="8bfca-180">Zmień nazwę modelu danych, aby **StoreDB.edmx** i kliknij przycisk **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="8bfca-180">Change the data model name to **StoreDB.edmx** and click **Add**.</span></span>

    <span data-ttu-id="8bfca-181">![Dodawanie modelu danych jednostki ADO.NET StoreDB](aspnet-mvc-4-models-and-data-access/_static/image6.png "Dodawanie modelu danych jednostki ADO.NET StoreDB")</span><span class="sxs-lookup"><span data-stu-id="8bfca-181">![Adding the StoreDB ADO.NET Entity Data Model](aspnet-mvc-4-models-and-data-access/_static/image6.png "Adding the StoreDB ADO.NET Entity Data Model")</span></span>

    <span data-ttu-id="8bfca-182">*Dodawanie modelu danych jednostki ADO.NET StoreDB*</span><span class="sxs-lookup"><span data-stu-id="8bfca-182">*Adding the StoreDB ADO.NET Entity Data Model*</span></span>
2. <span data-ttu-id="8bfca-183">**Kreatora modelu danych jednostki** będą wyświetlane.</span><span class="sxs-lookup"><span data-stu-id="8bfca-183">The **Entity Data Model Wizard** will appear.</span></span> <span data-ttu-id="8bfca-184">Ten kreator przeprowadzi Cię przez tworzenie warstwy modelu.</span><span class="sxs-lookup"><span data-stu-id="8bfca-184">This wizard will guide you through the creation of the model layer.</span></span> <span data-ttu-id="8bfca-185">Ponieważ modelu należy tworzyć w oparciu o istniejących recentyl bazy danych dodane, wybierz opcję **generowania z bazy danych** i kliknij przycisk **dalej**.</span><span class="sxs-lookup"><span data-stu-id="8bfca-185">Since the model should be created based on the existing database recentyl added, select **Generate from database** and click **Next**.</span></span>

    <span data-ttu-id="8bfca-186">![Wybieranie zawartość modelu](aspnet-mvc-4-models-and-data-access/_static/image7.png "Wybieranie zawartość modelu")</span><span class="sxs-lookup"><span data-stu-id="8bfca-186">![Choosing the model content](aspnet-mvc-4-models-and-data-access/_static/image7.png "Choosing the model content")</span></span>

    <span data-ttu-id="8bfca-187">*Wybieranie modelu zawartości*</span><span class="sxs-lookup"><span data-stu-id="8bfca-187">*Choosing the model content*</span></span>
3. <span data-ttu-id="8bfca-188">Ponieważ są generowania modelu z bazy danych, należy określić połączenie.</span><span class="sxs-lookup"><span data-stu-id="8bfca-188">Since you are generating a model from a database, you will need to specify the connection to use.</span></span> <span data-ttu-id="8bfca-189">Kliknij przycisk **nowe połączenie**.</span><span class="sxs-lookup"><span data-stu-id="8bfca-189">Click **New Connection**.</span></span>
4. <span data-ttu-id="8bfca-190">Wybierz **plik bazy danych programu Microsoft SQL Server** i kliknij przycisk **Kontynuuj**.</span><span class="sxs-lookup"><span data-stu-id="8bfca-190">Select **Microsoft SQL Server Database File** and click **Continue**.</span></span>

    <span data-ttu-id="8bfca-191">![Wybierz źródło danych](aspnet-mvc-4-models-and-data-access/_static/image8.png "wybierz źródło danych")</span><span class="sxs-lookup"><span data-stu-id="8bfca-191">![Choose data source](aspnet-mvc-4-models-and-data-access/_static/image8.png "Choose data source")</span></span>

    <span data-ttu-id="8bfca-192">*Wybierz źródło danych w oknie dialogowym*</span><span class="sxs-lookup"><span data-stu-id="8bfca-192">*Choose data source dialog*</span></span>
5. <span data-ttu-id="8bfca-193">Kliknij przycisk **Przeglądaj** i wybierz bazę danych **MvcMusicStore.mdf** zlokalizowanego w **aplikacji\_danych** folder i kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="8bfca-193">Click **Browse** and select the database **MvcMusicStore.mdf** you located in the **App\_Data** folder and click **OK**.</span></span>

    <span data-ttu-id="8bfca-194">![Właściwości połączenia](aspnet-mvc-4-models-and-data-access/_static/image9.png "właściwości połączenia")</span><span class="sxs-lookup"><span data-stu-id="8bfca-194">![Connection properties](aspnet-mvc-4-models-and-data-access/_static/image9.png "Connection properties")</span></span>

    <span data-ttu-id="8bfca-195">*Właściwości połączenia*</span><span class="sxs-lookup"><span data-stu-id="8bfca-195">*Connection properties*</span></span>
6. <span data-ttu-id="8bfca-196">Wygenerowana klasa powinna mieć taką samą nazwę jak parametry połączenia jednostki, zmienia jego nazwę, aby **MusicStoreEntities** i kliknij przycisk **dalej**.</span><span class="sxs-lookup"><span data-stu-id="8bfca-196">The generated class should have the same name as the entity connection string, so change its name to **MusicStoreEntities** and click **Next**.</span></span>

    <span data-ttu-id="8bfca-197">![Wybieranie połączenia danych](aspnet-mvc-4-models-and-data-access/_static/image10.png "Wybieranie połączenia danych")</span><span class="sxs-lookup"><span data-stu-id="8bfca-197">![Choosing the data connection](aspnet-mvc-4-models-and-data-access/_static/image10.png "Choosing the data connection")</span></span>

    <span data-ttu-id="8bfca-198">*Wybieranie połączenia danych*</span><span class="sxs-lookup"><span data-stu-id="8bfca-198">*Choosing the data connection*</span></span>
7. <span data-ttu-id="8bfca-199">Wybierz obiekty bazy danych do użycia.</span><span class="sxs-lookup"><span data-stu-id="8bfca-199">Choose the database objects to use.</span></span> <span data-ttu-id="8bfca-200">Modelu jednostki będzie używać tylko tabele bazy danych, wybierz **tabel** opcję i upewnij się, że **Dołącz kolumny klucza obcego do modelu** i **Pluralize lub nadaj wygenerowany nazwy obiektów** również są zaznaczone opcje.</span><span class="sxs-lookup"><span data-stu-id="8bfca-200">As the Entity Model will use just the database's tables, select the **Tables** option, and make sure that the **Include foreign key columns in the model** and **Pluralize or singularize generated object names** options are also selected.</span></span> <span data-ttu-id="8bfca-201">Zmień Namespace modelu do **MvcMusicStore.Model** i kliknij przycisk **Zakończ**.</span><span class="sxs-lookup"><span data-stu-id="8bfca-201">Change the Model Namespace to **MvcMusicStore.Model** and click **Finish**.</span></span>

    <span data-ttu-id="8bfca-202">![Wybieranie obiektów bazy danych](aspnet-mvc-4-models-and-data-access/_static/image11.png "Wybieranie obiektów bazy danych")</span><span class="sxs-lookup"><span data-stu-id="8bfca-202">![Choosing the database objects](aspnet-mvc-4-models-and-data-access/_static/image11.png "Choosing the database objects")</span></span>

    <span data-ttu-id="8bfca-203">*Wybieranie obiektów bazy danych*</span><span class="sxs-lookup"><span data-stu-id="8bfca-203">*Choosing the database objects*</span></span>

    > [!NOTE]
    > <span data-ttu-id="8bfca-204">Okno dialogowe ostrzeżenia zabezpieczeń jest wyświetlany, kliknij przycisk **OK** uruchomić szablon i Generuj klasy dla jednostek w modelu.</span><span class="sxs-lookup"><span data-stu-id="8bfca-204">If a Security Warning dialog is shown, click **OK** to run the template and generate the classes for the model entities.</span></span>
8. <span data-ttu-id="8bfca-205">Diagram jednostek dla bazy danych będą wyświetlane, gdy zostanie utworzony oddzielny klasy, która mapuje każdej tabeli w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="8bfca-205">An entity diagram for the database will appear, while a separate class that maps each table to the database will be created.</span></span> <span data-ttu-id="8bfca-206">Na przykład **albumów** tabeli będą reprezentowane przez **albumu** klasy, w której każda kolumna w tabeli przypisze do właściwości klasy.</span><span class="sxs-lookup"><span data-stu-id="8bfca-206">For example, the **Albums** table will be represented by an **Album** class, where each column in the table will map to a class property.</span></span> <span data-ttu-id="8bfca-207">Pozwoli to zapytanie i pracować z obiektami, które reprezentują wierszy w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="8bfca-207">This will allow you to query and work with objects that represent rows in the database.</span></span>

    <span data-ttu-id="8bfca-208">![Diagram jednostek](aspnet-mvc-4-models-and-data-access/_static/image12.png "diagram jednostek")</span><span class="sxs-lookup"><span data-stu-id="8bfca-208">![Entity diagram](aspnet-mvc-4-models-and-data-access/_static/image12.png "Entity diagram")</span></span>

    <span data-ttu-id="8bfca-209">*Diagram jednostek*</span><span class="sxs-lookup"><span data-stu-id="8bfca-209">*Entity diagram*</span></span>

    > [!NOTE]
    > <span data-ttu-id="8bfca-210">Szablony T4 (.TT —) uruchom kod można wygenerować klas jednostek i spowoduje zastąpienie istniejących klas o takiej samej nazwie.</span><span class="sxs-lookup"><span data-stu-id="8bfca-210">The T4 templates (.tt) run code to generate the entities classes and will overwrite the existing classes with the same name.</span></span> <span data-ttu-id="8bfca-211">W tym przykładzie klasy &quot;albumu&quot;, &quot;Genre&quot; i &quot;wykonawcy&quot; zostały zastąpione wygenerowanego kodu.</span><span class="sxs-lookup"><span data-stu-id="8bfca-211">In this example, the classes &quot;Album&quot;, &quot;Genre&quot; and &quot;Artist&quot; were overwritten with the generated code.</span></span>


<a id="Ex1Task3"></a>

<a id="Task_3_-_Building_the_Application"></a>
#### <a name="task-3---building-the-application"></a><span data-ttu-id="8bfca-212">Zadanie 3 — Tworzenie aplikacji</span><span class="sxs-lookup"><span data-stu-id="8bfca-212">Task 3 - Building the Application</span></span>

<span data-ttu-id="8bfca-213">W tym zadaniu można będzie sprawdzać, mimo że generowania modelu zostały usunięte **albumu**, **Genre** i **wykonawcy** klasy modelu projektu tworzy pomyślnie przy użyciu nowe klasy modelu danych.</span><span class="sxs-lookup"><span data-stu-id="8bfca-213">In this task, you will check that, although the model generation have removed the **Album**, **Genre** and **Artist** model classes, the project builds successfully by using the new data model classes.</span></span>

1. <span data-ttu-id="8bfca-214">Skompiluj projekt, wybierając **kompilacji** element menu, a następnie **kompilacji MvcMusicStore**.</span><span class="sxs-lookup"><span data-stu-id="8bfca-214">Build the project by selecting the **Build** menu item and then **Build MvcMusicStore**.</span></span>

    <span data-ttu-id="8bfca-215">![Tworzenie projektu](aspnet-mvc-4-models-and-data-access/_static/image13.png "skompilowanie projektu")</span><span class="sxs-lookup"><span data-stu-id="8bfca-215">![Building the project](aspnet-mvc-4-models-and-data-access/_static/image13.png "Building the project")</span></span>

    <span data-ttu-id="8bfca-216">*Tworzenie projektu*</span><span class="sxs-lookup"><span data-stu-id="8bfca-216">*Building the project*</span></span>
2. <span data-ttu-id="8bfca-217">Projekt tworzy się pomyślnie.</span><span class="sxs-lookup"><span data-stu-id="8bfca-217">The project builds successfully.</span></span> <span data-ttu-id="8bfca-218">Dlaczego nadal działa?</span><span class="sxs-lookup"><span data-stu-id="8bfca-218">Why does it still work?</span></span> <span data-ttu-id="8bfca-219">To działa, ponieważ tabele bazy danych ma pola, które zawierają właściwości, które były używane w klasach usuniętych **albumu** i **Genre**.</span><span class="sxs-lookup"><span data-stu-id="8bfca-219">It works because the database tables have fields that include the properties that you were using in the removed classes **Album** and **Genre**.</span></span>

    <span data-ttu-id="8bfca-220">![Kompilacje zakończyło się pomyślnie](aspnet-mvc-4-models-and-data-access/_static/image14.png "kompilacji zakończyło się pomyślnie")</span><span class="sxs-lookup"><span data-stu-id="8bfca-220">![Builds succeeded](aspnet-mvc-4-models-and-data-access/_static/image14.png "Builds succeeded")</span></span>

    <span data-ttu-id="8bfca-221">*Kompilacje powiodło się.*</span><span class="sxs-lookup"><span data-stu-id="8bfca-221">*Builds succeeded*</span></span>
3. <span data-ttu-id="8bfca-222">Gdy Projektant wyświetla jednostek w formacie diagramu, są one naprawdę klas C#.</span><span class="sxs-lookup"><span data-stu-id="8bfca-222">While the designer displays the entities in a diagram format, they are really C# classes.</span></span> <span data-ttu-id="8bfca-223">Rozwiń węzeł **StoreDB.edmx** węzła w Eksploratorze rozwiązań, a następnie **StoreDB.tt**, pojawi się nowe jednostki wygenerowany.</span><span class="sxs-lookup"><span data-stu-id="8bfca-223">Expand the **StoreDB.edmx** node in the Solution Explorer and then **StoreDB.tt**, you will see the new generated entities.</span></span>

    <span data-ttu-id="8bfca-224">![Pliki generowane](aspnet-mvc-4-models-and-data-access/_static/image15.png "wygenerowanych plików")</span><span class="sxs-lookup"><span data-stu-id="8bfca-224">![Generated files](aspnet-mvc-4-models-and-data-access/_static/image15.png "Generated files")</span></span>

    <span data-ttu-id="8bfca-225">*Wygenerowane pliki*</span><span class="sxs-lookup"><span data-stu-id="8bfca-225">*Generated files*</span></span>

<a id="Ex1Task4"></a>

<a id="Task_4_-_Querying_the_Database"></a>
#### <a name="task-4---querying-the-database"></a><span data-ttu-id="8bfca-226">Zadanie 4 — kwerendy bazy danych</span><span class="sxs-lookup"><span data-stu-id="8bfca-226">Task 4 - Querying the Database</span></span>

<span data-ttu-id="8bfca-227">To zadanie spowoduje zaktualizowanie klasy StoreController tak, aby zamiast dane zapisane na stałe będzie zapytania bazy danych do pobrania informacji.</span><span class="sxs-lookup"><span data-stu-id="8bfca-227">In this task, you will update the StoreController class so that, instead of using hardcoded data, it will query the database to retrieve the information.</span></span>

1. <span data-ttu-id="8bfca-228">Otwórz **Controllers\StoreController.cs** i dodaj następujące pole do klasy, aby pomieścić wystąpienia **MusicStoreEntities** klasę o nazwie **storeDB**:</span><span class="sxs-lookup"><span data-stu-id="8bfca-228">Open **Controllers\StoreController.cs** and add the following field to the class to hold an instance of the **MusicStoreEntities** class, named **storeDB**:</span></span>

    <span data-ttu-id="8bfca-229">(Fragment - kodu *modeli i dostęp do danych - Ex1 storeDB*)</span><span class="sxs-lookup"><span data-stu-id="8bfca-229">(Code Snippet - *Models And Data Access - Ex1 storeDB*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample1.cs)]
2. <span data-ttu-id="8bfca-230">**MusicStoreEntities** klasy udostępnia właściwości kolekcji dla każdej tabeli w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="8bfca-230">The **MusicStoreEntities** class exposes a collection property for each table in the database.</span></span> <span data-ttu-id="8bfca-231">Aktualizacja **Przeglądaj** metody akcji można pobrać określonego rodzaju ze wszystkimi **albumów**.</span><span class="sxs-lookup"><span data-stu-id="8bfca-231">Update **Browse** action method to retrieve a Genre with all of the **Albums**.</span></span>

    <span data-ttu-id="8bfca-232">(Fragment - kodu *modeli i dostęp do danych - przeglądania magazynu Ex1*)</span><span class="sxs-lookup"><span data-stu-id="8bfca-232">(Code Snippet - *Models And Data Access - Ex1 Store Browse*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample2.cs)]

    > [!NOTE]
    > <span data-ttu-id="8bfca-233">W przypadku korzystania z możliwości .NET o nazwie **LINQ** (zapytanie o języku zintegrowanym) do zapisania wyrażeń jednoznacznie zapytania względem tych kolekcje — co spowoduje wykonanie kodu w bazie danych i zwracać obiekty można umieszczonych przed.</span><span class="sxs-lookup"><span data-stu-id="8bfca-233">You are using a capability of .NET called **LINQ** (language-integrated query) to write strongly-typed query expressions against these collections - which will execute code against the database and return objects that you can program against.</span></span>
    > 
    > <span data-ttu-id="8bfca-234">Aby uzyskać więcej informacji na temat LINQ, odwiedź stronę [witrynę msdn](https://msdn.microsoft.com/library/bb397926&amp;#040;v=vs.110&amp;#041;.aspx).</span><span class="sxs-lookup"><span data-stu-id="8bfca-234">For more information about LINQ, please visit the [msdn site](https://msdn.microsoft.com/library/bb397926&amp;#040;v=vs.110&amp;#041;.aspx).</span></span>
3. <span data-ttu-id="8bfca-235">Aktualizacja **indeksu** metody akcji, aby pobrać wszystkie genres.</span><span class="sxs-lookup"><span data-stu-id="8bfca-235">Update **Index** action method to retrieve all the genres.</span></span>

    <span data-ttu-id="8bfca-236">(Fragment - kodu *modeli i Data Access — indeks magazynu Ex1*)</span><span class="sxs-lookup"><span data-stu-id="8bfca-236">(Code Snippet - *Models And Data Access - Ex1 Store Index*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample3.cs)]
4. <span data-ttu-id="8bfca-237">Aktualizacja **indeksu** metody akcji, aby pobrać wszystkie genres i przekształcenie kolekcji na listę.</span><span class="sxs-lookup"><span data-stu-id="8bfca-237">Update **Index** action method to retrieve all the genres and transform the collection to a list.</span></span>

    <span data-ttu-id="8bfca-238">(Fragment - kodu *modeli i dostęp do danych - GenreMenu magazynu Ex1*)</span><span class="sxs-lookup"><span data-stu-id="8bfca-238">(Code Snippet - *Models And Data Access - Ex1 Store GenreMenu*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample4.cs)]

<a id="Ex1Task5"></a>

<a id="Task_5_-_Running_the_Application"></a>
#### <a name="task-5---running-the-application"></a><span data-ttu-id="8bfca-239">Zadanie 5 działania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="8bfca-239">Task 5 - Running the Application</span></span>

<span data-ttu-id="8bfca-240">To zadanie będzie sprawdzać, czy strona indeksu magazynu wyświetli Genres przechowywane w bazie danych, zamiast ustalony te.</span><span class="sxs-lookup"><span data-stu-id="8bfca-240">In this task, you will check that the Store Index page will now display the Genres stored in the database instead of the hardcoded ones.</span></span> <span data-ttu-id="8bfca-241">Nie istnieje potrzeba zmiany szablonu widoku, ponieważ **StoreController** zwraca tej samej jednostki jak poprzednio, ale tym razem dane będą pobierane z bazy danych.</span><span class="sxs-lookup"><span data-stu-id="8bfca-241">There is no need to change the View template because the **StoreController** is returning the same entities as before, although this time the data will come from the database.</span></span>

1. <span data-ttu-id="8bfca-242">Ponowne kompilowanie rozwiązania i naciśnij klawisz **F5** do uruchomienia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="8bfca-242">Rebuild the solution and press **F5** to run the Application.</span></span>
2. <span data-ttu-id="8bfca-243">Na stronie głównej datę rozpoczęcia projektu.</span><span class="sxs-lookup"><span data-stu-id="8bfca-243">The project starts in the Home page.</span></span> <span data-ttu-id="8bfca-244">Sprawdź, czy menu **Genres** nie jest już listy zapisane na stałe, a dane bezpośrednio są pobierane z bazy danych.</span><span class="sxs-lookup"><span data-stu-id="8bfca-244">Verify that the menu of **Genres** is no longer a hardcoded list, and the data is directly retrieved from the database.</span></span>

    ![BrowsingGenresFromDataBase](aspnet-mvc-4-models-and-data-access/_static/image16.png)

    <span data-ttu-id="8bfca-246">*Przeglądanie Genres z bazy danych*</span><span class="sxs-lookup"><span data-stu-id="8bfca-246">*Browsing Genres from the database*</span></span>
3. <span data-ttu-id="8bfca-247">Teraz przejdź do dowolnego rodzaju i sprawdź, czy albumów zostają wypełnione z bazy danych.</span><span class="sxs-lookup"><span data-stu-id="8bfca-247">Now browse to any genre and verify the albums are populated from database.</span></span>

    <span data-ttu-id="8bfca-248">![Przeglądanie albumów z bazy danych](aspnet-mvc-4-models-and-data-access/_static/image17.png "przeglądanie albumów z bazy danych")</span><span class="sxs-lookup"><span data-stu-id="8bfca-248">![Browsing Albums from the database](aspnet-mvc-4-models-and-data-access/_static/image17.png "Browsing Albums from the database")</span></span>

    <span data-ttu-id="8bfca-249">*Przeglądanie albumów z bazy danych*</span><span class="sxs-lookup"><span data-stu-id="8bfca-249">*Browsing Albums from the database*</span></span>

<a id="Exercise2"></a>

<a id="Exercise_2_Creating_a_Database_Using_Code_First"></a>
### <a name="exercise-2-creating-a-database-using-code-first"></a><span data-ttu-id="8bfca-250">Ćwiczenie 2: Utworzenie bazy danych najpierw przy użyciu kodu</span><span class="sxs-lookup"><span data-stu-id="8bfca-250">Exercise 2: Creating a Database Using Code First</span></span>

<span data-ttu-id="8bfca-251">W tym ćwiczeniu dowiesz się, jak użyć metody Code First, aby utworzyć bazę danych z tabel MusicStore aplikacji oraz jak dostępu do danych.</span><span class="sxs-lookup"><span data-stu-id="8bfca-251">In this exercise, you will learn how to use the Code First approach to create a database with the tables of the MusicStore application, and how to access its data.</span></span>

<span data-ttu-id="8bfca-252">Po wygenerowaniu modelu, należy zmodyfikować StoreController dostarczanie szablon widoku danych z bazy danych, zamiast wartości zapisane na stałe.</span><span class="sxs-lookup"><span data-stu-id="8bfca-252">Once the model is generated, you will modify the StoreController to provide the View template with the data taken from the database, instead of using hardcoded values.</span></span>

> [!NOTE]
> <span data-ttu-id="8bfca-253">Jeśli ukończono ćwiczenie 1 i pracowali już z bazy danych pierwszego podejścia, będzie teraz Dowiedz się jak uzyskać takie same wyniki z innego procesu.</span><span class="sxs-lookup"><span data-stu-id="8bfca-253">If you have completed Exercise 1 and have already worked with the Database First approach, you will now learn how to get the same results with a different process.</span></span> <span data-ttu-id="8bfca-254">Zadania, które są wspólne dla wykonywania 1 została oznaczona ułatwi Twojej odczytu.</span><span class="sxs-lookup"><span data-stu-id="8bfca-254">The tasks that are in common with Exercise 1 have been marked to make your reading easier.</span></span> <span data-ttu-id="8bfca-255">Jeśli nie została ukończona wykonywania 1, ale chcesz dowiedzieć się podejście Code First, możesz rozpocząć od tego ćwiczenia i uzyskać pełny zakres tego tematu.</span><span class="sxs-lookup"><span data-stu-id="8bfca-255">If you have not completed Exercise 1 but would like to learn the Code First approach, you can start from this exercise and get a full coverage of the topic.</span></span>


<a id="Ex2Task1"></a>

<a id="Task_1_-_Populating_Sample_Data"></a>
#### <a name="task-1---populating-sample-data"></a><span data-ttu-id="8bfca-256">Zadanie 1 - wypełnianie przykładowe dane</span><span class="sxs-lookup"><span data-stu-id="8bfca-256">Task 1 - Populating Sample Data</span></span>

<span data-ttu-id="8bfca-257">W tym zadaniu zostanie Wypełnianie bazy danych z przykładowymi danymi tworzonemu początkowemu przy użyciu pierwszej kodu.</span><span class="sxs-lookup"><span data-stu-id="8bfca-257">In this task, you will populate the database with sample data when it is intially created using Code-First.</span></span>

1. <span data-ttu-id="8bfca-258">Otwórz **rozpocząć** rozwiązania, znajdujących się na **źródło/Ex2-CreatingADatabaseCodeFirst/Begin/** folderu.</span><span class="sxs-lookup"><span data-stu-id="8bfca-258">Open the **Begin** solution located at **Source/Ex2-CreatingADatabaseCodeFirst/Begin/** folder.</span></span> <span data-ttu-id="8bfca-259">W przeciwnym razie możesz nadal korzystać **zakończenia** uzyskane rozwiązanie, wykonując poprzednim ćwiczeniu.</span><span class="sxs-lookup"><span data-stu-id="8bfca-259">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

   1. <span data-ttu-id="8bfca-260">Po otwarciu dostarczonych **rozpocząć** rozwiązania, musisz pobrać niektórych brakujących pakietów NuGet aby kontynuować.</span><span class="sxs-lookup"><span data-stu-id="8bfca-260">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="8bfca-261">Aby to zrobić, kliknij przycisk **projektu** menu i wybierz **Zarządzaj pakietami NuGet**.</span><span class="sxs-lookup"><span data-stu-id="8bfca-261">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="8bfca-262">W **Zarządzaj pakietami NuGet** okna dialogowego, kliknij przycisk **przywrócić** celu pobieranie brakujących pakietów.</span><span class="sxs-lookup"><span data-stu-id="8bfca-262">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="8bfca-263">Na koniec Skompiluj rozwiązanie, klikając **kompilacji** | **Kompiluj rozwiązanie**.</span><span class="sxs-lookup"><span data-stu-id="8bfca-263">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="8bfca-264">Jedną z zalet przy użyciu narzędzia NuGet jest, że nie masz do wysłania wszystkich bibliotek w projekcie, zmniejszenie jego rozmiar projektu.</span><span class="sxs-lookup"><span data-stu-id="8bfca-264">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="8bfca-265">Narzędzia Power NuGet określając wersje pakietów w pliku Packages.config, będzie można pobrać wymaganych bibliotek podczas pierwszego uruchomienia projektu.</span><span class="sxs-lookup"><span data-stu-id="8bfca-265">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="8bfca-266">Jest to, dlaczego konieczne będzie wykonanie tych kroków, po otwarciu istniejącego rozwiązania z tego laboratorium.</span><span class="sxs-lookup"><span data-stu-id="8bfca-266">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="8bfca-267">Dodaj **SampleData.cs** pliku **modele** folderu.</span><span class="sxs-lookup"><span data-stu-id="8bfca-267">Add the **SampleData.cs** file to the **Models** folder.</span></span> <span data-ttu-id="8bfca-268">W tym celu kliknij prawym przyciskiem myszy **modele** folderu, wskaż **Dodaj** , a następnie kliknij przycisk **istniejący element**.</span><span class="sxs-lookup"><span data-stu-id="8bfca-268">To do that, right-click **Models** folder, point to **Add** and then click **Existing Item**.</span></span> <span data-ttu-id="8bfca-269">Przejdź do **\Source\Assets** i wybierz **SampleData.cs** pliku.</span><span class="sxs-lookup"><span data-stu-id="8bfca-269">Browse to **\Source\Assets** and select the **SampleData.cs** file.</span></span>

    <span data-ttu-id="8bfca-270">![Przykładowe dane wypełnić kodu](aspnet-mvc-4-models-and-data-access/_static/image18.png "przykładowych danych wypełnić kodu")</span><span class="sxs-lookup"><span data-stu-id="8bfca-270">![Sample data populate code](aspnet-mvc-4-models-and-data-access/_static/image18.png "Sample data populate code")</span></span>

    <span data-ttu-id="8bfca-271">*Przykładowe dane wypełnić kodu*</span><span class="sxs-lookup"><span data-stu-id="8bfca-271">*Sample data populate code*</span></span>
3. <span data-ttu-id="8bfca-272">Otwórz **Global.asax.cs** pliku i dodaj następującą *przy użyciu* instrukcje.</span><span class="sxs-lookup"><span data-stu-id="8bfca-272">Open the **Global.asax.cs** file and add the following *using* statements.</span></span>

    <span data-ttu-id="8bfca-273">(Fragment - kodu *modeli i Data Access — deklaracje Using Ex2 globalne Asax*)</span><span class="sxs-lookup"><span data-stu-id="8bfca-273">(Code Snippet - *Models And Data Access - Ex2 Global Asax Usings*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample5.cs)]
4. <span data-ttu-id="8bfca-274">W **aplikacji\_Start()** metody Dodaj następujący wiersz do ustawienia inicjatora bazy danych.</span><span class="sxs-lookup"><span data-stu-id="8bfca-274">In the **Application\_Start()** method add the following line to set the database initializer.</span></span>

    <span data-ttu-id="8bfca-275">(Fragment - kodu *modeli i dostęp do danych - globalne Asax SetInitializer Ex2*)</span><span class="sxs-lookup"><span data-stu-id="8bfca-275">(Code Snippet - *Models And Data Access - Ex2 Global Asax SetInitializer*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample6.cs)]

<a id="Ex2Task2"></a>

<a id="Task_2_-_Configuring_the_connection_to_the_Database"></a>
#### <a name="task-2---configuring-the-connection-to-the-database"></a><span data-ttu-id="8bfca-276">Zadanie 2 — Konfigurowanie połączenia z bazą danych</span><span class="sxs-lookup"><span data-stu-id="8bfca-276">Task 2 - Configuring the connection to the Database</span></span>

<span data-ttu-id="8bfca-277">Teraz, gdy masz już dodany bazy danych do naszej projektu, będzie zapisywać **Web.config** pliku parametrów połączenia.</span><span class="sxs-lookup"><span data-stu-id="8bfca-277">Now that you have already added a database to our project, you will write in the **Web.config** file the connection string.</span></span>

1. <span data-ttu-id="8bfca-278">Dodaj parametry połączenia w **Web.config**. Aby to zrobić, otwórz **Web.config** w głównym projektu i Zastąp ciąg połączenia o nazwie parametru DefaultConnection z tego wiersza w **&lt;connectionStrings&gt;** sekcji:</span><span class="sxs-lookup"><span data-stu-id="8bfca-278">Add a connection string at **Web.config**. To do that, open **Web.config** at project root and replace the connection string named DefaultConnection with this line in the **&lt;connectionStrings&gt;** section:</span></span>

    <span data-ttu-id="8bfca-279">![Lokalizacja pliku Web.config](aspnet-mvc-4-models-and-data-access/_static/image19.png "lokalizację pliku Web.config")</span><span class="sxs-lookup"><span data-stu-id="8bfca-279">![Web.config file location](aspnet-mvc-4-models-and-data-access/_static/image19.png "Web.config file location")</span></span>

    <span data-ttu-id="8bfca-280">*Lokalizacja pliku Web.config*</span><span class="sxs-lookup"><span data-stu-id="8bfca-280">*Web.config file location*</span></span>

    [!code-xml[Main](aspnet-mvc-4-models-and-data-access/samples/sample7.xml)]

<a id="Ex2Task3"></a>

<a id="Task_3_-_Working_with_the_Model"></a>
#### <a name="task-3---working-with-the-model"></a><span data-ttu-id="8bfca-281">Zadanie 3 — Praca z modelu</span><span class="sxs-lookup"><span data-stu-id="8bfca-281">Task 3 - Working with the Model</span></span>

<span data-ttu-id="8bfca-282">Teraz, gdy skonfigurowano już połączenie z bazą danych, zostaną połączone modelu z tabel bazy danych.</span><span class="sxs-lookup"><span data-stu-id="8bfca-282">Now that you have already configured the connection to the database, you will link the model with the database tables.</span></span> <span data-ttu-id="8bfca-283">W ramach tego zadania spowoduje utworzenie klasy, który będzie połączony z bazą danych z Code First.</span><span class="sxs-lookup"><span data-stu-id="8bfca-283">In this task, you will create a class that will be linked to the database with Code First.</span></span> <span data-ttu-id="8bfca-284">Należy pamiętać, że istnieje istniejących klasy modelu POCO, które powinno zostać zmodyfikowane w.</span><span class="sxs-lookup"><span data-stu-id="8bfca-284">Remember that there is an existent POCO model class that should be modified.</span></span>

> [!NOTE]
> <span data-ttu-id="8bfca-285">Jeśli ukończono ćwiczenie 1 można zauważyć, że ten krok został wykonany przez kreatora.</span><span class="sxs-lookup"><span data-stu-id="8bfca-285">If you completed Exercise 1, you will note that this step was performed by a wizard.</span></span> <span data-ttu-id="8bfca-286">Wykonując Code First, ręcznie utworzysz klasy, które zostaną połączone obiekty danych.</span><span class="sxs-lookup"><span data-stu-id="8bfca-286">By doing Code First, you will manually create classes that will be linked to data entities.</span></span>

1. <span data-ttu-id="8bfca-287">Otwórz klasy modelu POCO **Genre** z **modele** folderu projektu i zawiera identyfikatora.</span><span class="sxs-lookup"><span data-stu-id="8bfca-287">Open the POCO model class **Genre** from **Models** project folder and include an ID.</span></span> <span data-ttu-id="8bfca-288">Użyj właściwość o nazwie **GenreId**.</span><span class="sxs-lookup"><span data-stu-id="8bfca-288">Use an int property with the name **GenreId**.</span></span>

    <span data-ttu-id="8bfca-289">(Fragment - kodu *modeli i dostęp do danych - Genre pierwszy kod Ex2*)</span><span class="sxs-lookup"><span data-stu-id="8bfca-289">(Code Snippet - *Models And Data Access - Ex2 Code First Genre*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample8.cs)]

    > [!NOTE]
    > <span data-ttu-id="8bfca-290">Aby pracować z konwencjami Code First, klasa Genre musi mieć właściwości klucza podstawowego, który będzie wykrywane automatycznie.</span><span class="sxs-lookup"><span data-stu-id="8bfca-290">To work with Code First conventions, the class Genre must have a primary key property that will be automatically detected.</span></span>
    > 
    > <span data-ttu-id="8bfca-291">Możesz przeczytać więcej na temat Konwencji pierwszy kod w tym [artykuł w witrynie msdn](https://msdn.microsoft.com/library/hh161541&amp;#040;v=vs.103&amp;#041;.aspx).</span><span class="sxs-lookup"><span data-stu-id="8bfca-291">You can read more about Code First Conventions in this [msdn article](https://msdn.microsoft.com/library/hh161541&amp;#040;v=vs.103&amp;#041;.aspx).</span></span>
2. <span data-ttu-id="8bfca-292">Teraz Otwórz klasy modelu POCO **albumu** z **modele** folderu projektu i zawierają kluczy obcych, Utwórz właściwości o nazwach **GenreId** i  **ArtistId**.</span><span class="sxs-lookup"><span data-stu-id="8bfca-292">Now, open the POCO model class **Album** from **Models** project folder and include the foreign keys, create properties with the names **GenreId** and **ArtistId**.</span></span> <span data-ttu-id="8bfca-293">Ta klasa już **GenreId** klucza podstawowego.</span><span class="sxs-lookup"><span data-stu-id="8bfca-293">This class already have the **GenreId** for the primary key.</span></span>

    <span data-ttu-id="8bfca-294">(Fragment - kodu *modeli i dostęp do danych - Ex2 kod pierwszego albumu*)</span><span class="sxs-lookup"><span data-stu-id="8bfca-294">(Code Snippet - *Models And Data Access - Ex2 Code First Album*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample9.cs)]
3. <span data-ttu-id="8bfca-295">Otwórz klasy modelu POCO **wykonawcy** i obejmują **ArtistId** właściwości.</span><span class="sxs-lookup"><span data-stu-id="8bfca-295">Open the POCO model class **Artist** and include the **ArtistId** property.</span></span>

    <span data-ttu-id="8bfca-296">(Fragment - kodu *modeli i dostęp do danych - wykonawcy pierwszy kod Ex2*)</span><span class="sxs-lookup"><span data-stu-id="8bfca-296">(Code Snippet - *Models And Data Access - Ex2 Code First Artist*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample10.cs)]
4. <span data-ttu-id="8bfca-297">Kliknij prawym przyciskiem myszy **modele** folderu projektu i wybierz **Dodaj | Klasa**.</span><span class="sxs-lookup"><span data-stu-id="8bfca-297">Right-click the **Models** project folder and select **Add | Class**.</span></span> <span data-ttu-id="8bfca-298">Nadaj nazwę plikowi **MusicStoreEntities.cs**.</span><span class="sxs-lookup"><span data-stu-id="8bfca-298">Name the file **MusicStoreEntities.cs**.</span></span> <span data-ttu-id="8bfca-299">Następnie kliknij przycisk **Dodaj.**</span><span class="sxs-lookup"><span data-stu-id="8bfca-299">Then, click **Add.**</span></span>

    <span data-ttu-id="8bfca-300">![Dodawanie klasy](aspnet-mvc-4-models-and-data-access/_static/image20.png "Dodawanie klasy")</span><span class="sxs-lookup"><span data-stu-id="8bfca-300">![Adding a class](aspnet-mvc-4-models-and-data-access/_static/image20.png "Adding a class")</span></span>

    <span data-ttu-id="8bfca-301">*Dodawanie nowego elementu*</span><span class="sxs-lookup"><span data-stu-id="8bfca-301">*Adding a new item*</span></span>

    <span data-ttu-id="8bfca-302">![Dodawanie class2](aspnet-mvc-4-models-and-data-access/_static/image21.png "Dodawanie class2")</span><span class="sxs-lookup"><span data-stu-id="8bfca-302">![Adding a class2](aspnet-mvc-4-models-and-data-access/_static/image21.png "Adding a class2")</span></span>

    <span data-ttu-id="8bfca-303">*Dodawanie klasy*</span><span class="sxs-lookup"><span data-stu-id="8bfca-303">*Adding a class*</span></span>
5. <span data-ttu-id="8bfca-304">Otwórz właśnie utworzony, klasa **MusicStoreEntities.cs**i uwzględnić przestrzenie nazw **System.Data.Entity** i **System.Data.Entity.Infrastructure**.</span><span class="sxs-lookup"><span data-stu-id="8bfca-304">Open the class you have just created, **MusicStoreEntities.cs**, and include the namespaces **System.Data.Entity** and **System.Data.Entity.Infrastructure**.</span></span>

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample11.cs)]
6. <span data-ttu-id="8bfca-305">Zastąp deklaracji klasy, aby rozszerzyć **DbContext** klasy: deklarowanie publiczny **DBSet** i zastąpienia **OnModelCreating** — metoda.</span><span class="sxs-lookup"><span data-stu-id="8bfca-305">Replace the class declaration to extend the **DbContext** class: declare a public **DBSet** and override **OnModelCreating** method.</span></span> <span data-ttu-id="8bfca-306">Po wykonaniu tego kroku wystąpi klasy domeny, która połączy modelu za pomocą programu Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="8bfca-306">After this step you will get a domain class that will link your model with the Entity Framework.</span></span> <span data-ttu-id="8bfca-307">Aby to zrobić, Zastąp kod klasy następujące czynności:</span><span class="sxs-lookup"><span data-stu-id="8bfca-307">In order to do that, replace the class code with the following:</span></span>

    <span data-ttu-id="8bfca-308">(Fragment - kodu *modeli i dostęp do danych - MusicStoreEntities pierwszy kod Ex2*)</span><span class="sxs-lookup"><span data-stu-id="8bfca-308">(Code Snippet - *Models And Data Access - Ex2 Code First MusicStoreEntities*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample12.cs)]

> [!NOTE]
> <span data-ttu-id="8bfca-309">Z programu Entity Framework **DbContext** i **DBSet** można zbadać klasy POCO Genre.</span><span class="sxs-lookup"><span data-stu-id="8bfca-309">With Entity Framework **DbContext** and **DBSet** you will be able to query the POCO class Genre.</span></span> <span data-ttu-id="8bfca-310">Rozszerzając **OnModelCreating** metody, określasz w **kod** jak Genre zostaną zmapowane do tabeli bazy danych.</span><span class="sxs-lookup"><span data-stu-id="8bfca-310">By extending **OnModelCreating** method, you are specifying in the **code** how Genre will be mapped to a database table.</span></span> <span data-ttu-id="8bfca-311">Więcej informacji na temat obiektu DBContext i DBSet można znaleźć w tym artykule w witrynie msdn: [łącza](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=vs.103).aspx)</span><span class="sxs-lookup"><span data-stu-id="8bfca-311">You can find more information about DBContext and DBSet in this msdn article: [link](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=vs.103).aspx)</span></span>

<a id="Ex2Task4"></a>

<a id="Task_4_-_Querying_the_Database"></a>
#### <a name="task-4---querying-the-database"></a><span data-ttu-id="8bfca-312">Zadanie 4 — kwerendy bazy danych</span><span class="sxs-lookup"><span data-stu-id="8bfca-312">Task 4 - Querying the Database</span></span>

<span data-ttu-id="8bfca-313">To zadanie spowoduje zaktualizowanie klasy StoreController tak, aby zamiast dane zapisane na stałe, pobierze go z bazy danych.</span><span class="sxs-lookup"><span data-stu-id="8bfca-313">In this task, you will update the StoreController class so that, instead of using hardcoded data, it will retrieve it from the database.</span></span>

> [!NOTE]
> <span data-ttu-id="8bfca-314">To zadanie jest wspólne ćwiczenie 1.</span><span class="sxs-lookup"><span data-stu-id="8bfca-314">This task is in common with Exercise 1.</span></span>
> 
> <span data-ttu-id="8bfca-315">Jeśli ukończono ćwiczenie 1 można zauważyć następujące kroki są takie same, w obu podejść (najpierw bazy danych lub najpierw Code).</span><span class="sxs-lookup"><span data-stu-id="8bfca-315">If you completed Exercise 1 you will note these steps are the same in both approaches (Database first or Code first).</span></span> <span data-ttu-id="8bfca-316">Różnią się one w jaki sposób dane są połączone z modelem dostęp do obiektów danych jest jednak jeszcze przezroczysty z kontrolera.</span><span class="sxs-lookup"><span data-stu-id="8bfca-316">They are different in how the data is linked with the model, but the access to data entities is yet transparent from the controller.</span></span>


1. <span data-ttu-id="8bfca-317">Otwórz **Controllers\StoreController.cs** i dodaj następujące pole do klasy, aby pomieścić wystąpienia **MusicStoreEntities** klasę o nazwie **storeDB**:</span><span class="sxs-lookup"><span data-stu-id="8bfca-317">Open **Controllers\StoreController.cs** and add the following field to the class to hold an instance of the **MusicStoreEntities** class, named **storeDB**:</span></span>

    <span data-ttu-id="8bfca-318">(Fragment - kodu *modeli i dostęp do danych - Ex1 storeDB*)</span><span class="sxs-lookup"><span data-stu-id="8bfca-318">(Code Snippet - *Models And Data Access - Ex1 storeDB*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample13.cs)]
2. <span data-ttu-id="8bfca-319">**MusicStoreEntities** klasy udostępnia właściwości kolekcji dla każdej tabeli w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="8bfca-319">The **MusicStoreEntities** class exposes a collection property for each table in the database.</span></span> <span data-ttu-id="8bfca-320">Aktualizacja **Przeglądaj** metody akcji można pobrać określonego rodzaju ze wszystkimi **albumów**.</span><span class="sxs-lookup"><span data-stu-id="8bfca-320">Update **Browse** action method to retrieve a Genre with all of the **Albums**.</span></span>

    <span data-ttu-id="8bfca-321">(Fragment - kodu *modeli i dostęp do danych - przeglądania magazynu Ex2*)</span><span class="sxs-lookup"><span data-stu-id="8bfca-321">(Code Snippet - *Models And Data Access - Ex2 Store Browse*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample14.cs)]

    > [!NOTE]
    > <span data-ttu-id="8bfca-322">W przypadku korzystania z możliwości .NET o nazwie **LINQ** (zapytanie o języku zintegrowanym) do zapisania wyrażeń jednoznacznie zapytania względem tych kolekcje — co spowoduje wykonanie kodu w bazie danych i zwracać obiekty można umieszczonych przed.</span><span class="sxs-lookup"><span data-stu-id="8bfca-322">You are using a capability of .NET called **LINQ** (language-integrated query) to write strongly-typed query expressions against these collections - which will execute code against the database and return objects that you can program against.</span></span>
    > 
    > <span data-ttu-id="8bfca-323">Aby uzyskać więcej informacji na temat LINQ, odwiedź stronę [witrynę msdn](https://msdn.microsoft.com/library/bb397926(v=vs.110).aspx).</span><span class="sxs-lookup"><span data-stu-id="8bfca-323">For more information about LINQ, please visit the [msdn site](https://msdn.microsoft.com/library/bb397926(v=vs.110).aspx).</span></span>
3. <span data-ttu-id="8bfca-324">Aktualizacja **indeksu** metody akcji, aby pobrać wszystkie genres.</span><span class="sxs-lookup"><span data-stu-id="8bfca-324">Update **Index** action method to retrieve all the genres.</span></span>

    <span data-ttu-id="8bfca-325">(Fragment - kodu *modeli i Data Access — indeks magazynu Ex2*)</span><span class="sxs-lookup"><span data-stu-id="8bfca-325">(Code Snippet - *Models And Data Access - Ex2 Store Index*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample15.cs)]
4. <span data-ttu-id="8bfca-326">Aktualizacja **indeksu** metody akcji, aby pobrać wszystkie genres i przekształcenie kolekcji na listę.</span><span class="sxs-lookup"><span data-stu-id="8bfca-326">Update **Index** action method to retrieve all the genres and transform the collection to a list.</span></span>

    <span data-ttu-id="8bfca-327">(Fragment - kodu *modeli i dostęp do danych - GenreMenu magazynu Ex2*)</span><span class="sxs-lookup"><span data-stu-id="8bfca-327">(Code Snippet - *Models And Data Access - Ex2 Store GenreMenu*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample16.cs)]

<a id="Ex2Task5"></a>

<a id="Task_5_-_Running_the_Application"></a>
#### <a name="task-5---running-the-application"></a><span data-ttu-id="8bfca-328">Zadanie 5 działania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="8bfca-328">Task 5 - Running the Application</span></span>

<span data-ttu-id="8bfca-329">To zadanie będzie sprawdzać, czy strona indeksu magazynu wyświetli Genres przechowywane w bazie danych, zamiast ustalony te.</span><span class="sxs-lookup"><span data-stu-id="8bfca-329">In this task, you will check that the Store Index page will now display the Genres stored in the database instead of the hardcoded ones.</span></span> <span data-ttu-id="8bfca-330">Nie istnieje potrzeba zmiany szablonu widoku, ponieważ **StoreController** zwraca takie same **StoreIndexViewModel** jak poprzednio, ale tym razem dane będą pobierane z bazy danych.</span><span class="sxs-lookup"><span data-stu-id="8bfca-330">There is no need to change the View template because the **StoreController** is returning the same **StoreIndexViewModel** as before, but this time the data will come from the database.</span></span>

1. <span data-ttu-id="8bfca-331">Ponowne kompilowanie rozwiązania i naciśnij klawisz **F5** do uruchomienia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="8bfca-331">Rebuild the solution and press **F5** to run the Application.</span></span>
2. <span data-ttu-id="8bfca-332">Na stronie głównej datę rozpoczęcia projektu.</span><span class="sxs-lookup"><span data-stu-id="8bfca-332">The project starts in the Home page.</span></span> <span data-ttu-id="8bfca-333">Sprawdź, czy menu **Genres** nie jest już listy zapisane na stałe, a dane bezpośrednio są pobierane z bazy danych.</span><span class="sxs-lookup"><span data-stu-id="8bfca-333">Verify that the menu of **Genres** is no longer a hardcoded list, and the data is directly retrieved from the database.</span></span>

    ![BrowsingGenresFromDataBase](aspnet-mvc-4-models-and-data-access/_static/image22.png)

    <span data-ttu-id="8bfca-335">*Przeglądanie Genres z bazy danych*</span><span class="sxs-lookup"><span data-stu-id="8bfca-335">*Browsing Genres from the database*</span></span>
3. <span data-ttu-id="8bfca-336">Teraz przejdź do dowolnego rodzaju i sprawdź, czy albumów zostają wypełnione z bazy danych.</span><span class="sxs-lookup"><span data-stu-id="8bfca-336">Now browse to any genre and verify the albums are populated from database.</span></span>

    <span data-ttu-id="8bfca-337">![Przeglądanie albumów z bazy danych](aspnet-mvc-4-models-and-data-access/_static/image23.png "przeglądanie albumów z bazy danych")</span><span class="sxs-lookup"><span data-stu-id="8bfca-337">![Browsing Albums from the database](aspnet-mvc-4-models-and-data-access/_static/image23.png "Browsing Albums from the database")</span></span>

    <span data-ttu-id="8bfca-338">*Przeglądanie albumów z bazy danych*</span><span class="sxs-lookup"><span data-stu-id="8bfca-338">*Browsing Albums from the database*</span></span>

<a id="Exercise3"></a>

<a id="Exercise_3_Querying_the_Database_with_Parameters"></a>
### <a name="exercise-3-querying-the-database-with-parameters"></a><span data-ttu-id="8bfca-339">Ćwiczenie 3: Zapytanie bazy danych z parametrami</span><span class="sxs-lookup"><span data-stu-id="8bfca-339">Exercise 3: Querying the Database with Parameters</span></span>

<span data-ttu-id="8bfca-340">W tym ćwiczeniu dowiesz się, jak wykonać zapytanie dotyczące bazy danych przy użyciu parametrów i sposobu użycia, kształtowania wyników zapytania, funkcja, która ogranicza numerów bazy danych uzyskuje dostęp do pobierania danych w bardziej wydajny sposób.</span><span class="sxs-lookup"><span data-stu-id="8bfca-340">In this exercise, you will learn how to query the database using parameters, and how to use Query Result Shaping, a feature that reduces the number database accesses retrieving data in a more efficient way.</span></span>

> [!NOTE]
> <span data-ttu-id="8bfca-341">Aby uzyskać więcej informacji na kształtowania wyników zapytania, można znaleźć w następującej [artykuł w witrynie msdn](https://msdn.microsoft.com/library/bb896272&amp;#040;v=vs.100&amp;#041;.aspx).</span><span class="sxs-lookup"><span data-stu-id="8bfca-341">For further information on Query Result Shaping, visit the following [msdn article](https://msdn.microsoft.com/library/bb896272&amp;#040;v=vs.100&amp;#041;.aspx).</span></span>

<a id="Ex3Task1"></a>

<a id="Task_1_-_Modifying_StoreController_to_Retrieve_Albums_from_Database"></a>
#### <a name="task-1---modifying-storecontroller-to-retrieve-albums-from-database"></a><span data-ttu-id="8bfca-342">Zadanie 1 - modyfikowanie StoreController pobrać albumów z bazy danych</span><span class="sxs-lookup"><span data-stu-id="8bfca-342">Task 1 - Modifying StoreController to Retrieve Albums from Database</span></span>

<span data-ttu-id="8bfca-343">W tym zadaniu zostanie zmieniony **StoreController** klasy dostęp do bazy danych można pobrać albumów z określonego rodzaju.</span><span class="sxs-lookup"><span data-stu-id="8bfca-343">In this task, you will change the **StoreController** class to access the database to retrieve albums from a specific genre.</span></span>

1. <span data-ttu-id="8bfca-344">Otwórz **rozpocząć** rozwiązania, znajdujących się na **Source\Ex3 QueryingTheDatabaseWithParametersCodeFirst\Begin** folderu, jeśli chcesz użyć pierwszej kodu metody lub **Source\ Ex3 QueryingTheDatabaseWithParametersDBFirst\Begin** folderu, jeśli chcesz użyć metody pierwszej bazy danych.</span><span class="sxs-lookup"><span data-stu-id="8bfca-344">Open the **Begin** solution located at the **Source\Ex3-QueryingTheDatabaseWithParametersCodeFirst\Begin** folder if you want to use Code-First approach or **Source\Ex3-QueryingTheDatabaseWithParametersDBFirst\Begin** folder if you want to use Database-First approach.</span></span> <span data-ttu-id="8bfca-345">W przeciwnym razie możesz nadal korzystać **zakończenia** uzyskane rozwiązanie, wykonując poprzednim ćwiczeniu.</span><span class="sxs-lookup"><span data-stu-id="8bfca-345">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

   1. <span data-ttu-id="8bfca-346">Po otwarciu dostarczonych **rozpocząć** rozwiązania, musisz pobrać niektórych brakujących pakietów NuGet aby kontynuować.</span><span class="sxs-lookup"><span data-stu-id="8bfca-346">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="8bfca-347">Aby to zrobić, kliknij przycisk **projektu** menu i wybierz **Zarządzaj pakietami NuGet**.</span><span class="sxs-lookup"><span data-stu-id="8bfca-347">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="8bfca-348">W **Zarządzaj pakietami NuGet** okna dialogowego, kliknij przycisk **przywrócić** celu pobieranie brakujących pakietów.</span><span class="sxs-lookup"><span data-stu-id="8bfca-348">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="8bfca-349">Na koniec Skompiluj rozwiązanie, klikając **kompilacji** | **Kompiluj rozwiązanie**.</span><span class="sxs-lookup"><span data-stu-id="8bfca-349">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="8bfca-350">Jedną z zalet przy użyciu narzędzia NuGet jest, że nie masz do wysłania wszystkich bibliotek w projekcie, zmniejszenie jego rozmiar projektu.</span><span class="sxs-lookup"><span data-stu-id="8bfca-350">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="8bfca-351">Narzędzia Power NuGet określając wersje pakietów w pliku Packages.config, będzie można pobrać wymaganych bibliotek podczas pierwszego uruchomienia projektu.</span><span class="sxs-lookup"><span data-stu-id="8bfca-351">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="8bfca-352">Jest to, dlaczego konieczne będzie wykonanie tych kroków, po otwarciu istniejącego rozwiązania z tego laboratorium.</span><span class="sxs-lookup"><span data-stu-id="8bfca-352">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="8bfca-353">Otwórz **StoreController** klasę, aby zmienić **Przeglądaj** metody akcji.</span><span class="sxs-lookup"><span data-stu-id="8bfca-353">Open the **StoreController** class to change the **Browse** action method.</span></span> <span data-ttu-id="8bfca-354">Aby to zrobić, w **Eksploratora rozwiązań**, rozwiń węzeł **kontrolerów** folder i kliknij dwukrotnie **StoreController.cs**.</span><span class="sxs-lookup"><span data-stu-id="8bfca-354">To do this, in the **Solution Explorer**, expand the **Controllers** folder and double-click **StoreController.cs**.</span></span>
3. <span data-ttu-id="8bfca-355">Zmień **Przeglądaj** metody akcji można pobrać albumów dla określonego rodzaju.</span><span class="sxs-lookup"><span data-stu-id="8bfca-355">Change the **Browse** action method to retrieve albums for a specific genre.</span></span> <span data-ttu-id="8bfca-356">Aby to zrobić, Zastąp następujący kod:</span><span class="sxs-lookup"><span data-stu-id="8bfca-356">To do this, replace the following code:</span></span>

    <span data-ttu-id="8bfca-357">(Fragment - kodu *modeli i dostęp do danych - Ex3 StoreController BrowseMethod*)</span><span class="sxs-lookup"><span data-stu-id="8bfca-357">(Code Snippet - *Models And Data Access - Ex3 StoreController BrowseMethod*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample17.cs)]

> [!NOTE]
> <span data-ttu-id="8bfca-358">Aby wypełnić kolekcji jednostki, należy użyć **Include** metodę, aby określić ma zostać pobrane za albumów.</span><span class="sxs-lookup"><span data-stu-id="8bfca-358">To populate a collection of the entity, you need to use the **Include** method to specify you want to retrieve the albums too.</span></span> <span data-ttu-id="8bfca-359">Można użyć. **Single()** rozszerzenia LINQ, ponieważ w takim przypadku oczekuje tylko jednego rodzaju albumu.</span><span class="sxs-lookup"><span data-stu-id="8bfca-359">You can use the .**Single()** extension in LINQ because in this case only one genre is expected for an album.</span></span> <span data-ttu-id="8bfca-360">**Single()** metoda przyjmuje jako parametr w takim przypadku określa pojedynczy obiekt Genre w taki sposób, że jego nazwa jest zgodna z wartością zdefiniowane wyrażenia Lambda.</span><span class="sxs-lookup"><span data-stu-id="8bfca-360">The **Single()** method takes a Lambda expression as a parameter, which in this case specifies a single Genre object such that its name matches the value defined.</span></span>
> 
> <span data-ttu-id="8bfca-361">Będzie korzystać z funkcji, które można określić innych powiązanych jednostek, który ma być również załadowany po pobraniu obiektu Genre.</span><span class="sxs-lookup"><span data-stu-id="8bfca-361">You will take advantage of a feature that allows you to indicate other related entities you want loaded as well when the Genre object is retrieved.</span></span> <span data-ttu-id="8bfca-362">Ta funkcja jest nazywana **kształtowania wynik zapytania**i pozwala zmniejszyć liczbę potrzebnych do dostępu do bazy danych można pobrać informacji o.</span><span class="sxs-lookup"><span data-stu-id="8bfca-362">This feature is called **Query Result Shaping**, and enables you to reduce the number of times needed to access the database to retrieve information.</span></span> <span data-ttu-id="8bfca-363">W tym scenariuszu można wstępnie pobrać albumów dla rodzaju pobierania.</span><span class="sxs-lookup"><span data-stu-id="8bfca-363">In this scenario, you will want to pre-fetch the Albums for the Genre you retrieve.</span></span>
> 
> <span data-ttu-id="8bfca-364">Zapytanie zawiera **Genres.Include (&quot;albumów&quot;)** aby wskazać, że ma również albumów pokrewne.</span><span class="sxs-lookup"><span data-stu-id="8bfca-364">The query includes **Genres.Include(&quot;Albums&quot;)** to indicate that you want related albums as well.</span></span> <span data-ttu-id="8bfca-365">To spowoduje większą wydajność aplikacji, ponieważ będą pobierane dane zarówno Genre, jak i albumu w żądaniu pojedynczej bazy danych.</span><span class="sxs-lookup"><span data-stu-id="8bfca-365">This will result in a more efficient application, since it will retrieve both Genre and Album data in a single database request.</span></span>

<a id="Ex3Task2"></a>

<a id="Task_2_-_Running_the_Application"></a>
#### <a name="task-2---running-the-application"></a><span data-ttu-id="8bfca-366">Zadanie 2 — w uruchomionej aplikacji</span><span class="sxs-lookup"><span data-stu-id="8bfca-366">Task 2 - Running the Application</span></span>

<span data-ttu-id="8bfca-367">W tym zadaniu zostanie Uruchom aplikację i pobrać albumów określonego rodzaju z bazy danych.</span><span class="sxs-lookup"><span data-stu-id="8bfca-367">In this task, you will run the application and retrieve albums of a specific genre from the database.</span></span>

1. <span data-ttu-id="8bfca-368">Naciśnij klawisz **F5** do uruchomienia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="8bfca-368">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="8bfca-369">Na stronie głównej datę rozpoczęcia projektu.</span><span class="sxs-lookup"><span data-stu-id="8bfca-369">The project starts in the Home page.</span></span> <span data-ttu-id="8bfca-370">Zmień adres URL do **/magazynu/Przeglądaj? genre = Pop** można zweryfikować, że wyniki są pobierane z bazy danych.</span><span class="sxs-lookup"><span data-stu-id="8bfca-370">Change the URL to **/Store/Browse?genre=Pop** to verify that the results are being retrieved from the database.</span></span>

    <span data-ttu-id="8bfca-371">![Przeglądanie według rodzaju](aspnet-mvc-4-models-and-data-access/_static/image24.png "przeglądanie według rodzaju")</span><span class="sxs-lookup"><span data-stu-id="8bfca-371">![Browsing by genre](aspnet-mvc-4-models-and-data-access/_static/image24.png "Browsing by genre")</span></span>

    <span data-ttu-id="8bfca-372">*Przeglądanie/magazynu/Przeglądaj? genre = Pop*</span><span class="sxs-lookup"><span data-stu-id="8bfca-372">*Browsing /Store/Browse?genre=Pop*</span></span>

<a id="Ex3Task3"></a>

<a id="Task_3_-_Accessing_Albums_by_Id"></a>
#### <a name="task-3---accessing-albums-by-id"></a><span data-ttu-id="8bfca-373">Zadanie 3. uzyskiwanie dostępu do albumów według identyfikatora</span><span class="sxs-lookup"><span data-stu-id="8bfca-373">Task 3 - Accessing Albums by Id</span></span>

<span data-ttu-id="8bfca-374">W tym zadaniu zostanie powtórzony poprzedniej procedury, aby uzyskać albumów według ich identyfikatorów.</span><span class="sxs-lookup"><span data-stu-id="8bfca-374">In this task, you will repeat the previous procedure to get albums by their Id.</span></span>

1. <span data-ttu-id="8bfca-375">Zamknij przeglądarkę, jeśli to konieczne, aby powrócić do programu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="8bfca-375">Close the browser if needed, to return to Visual Studio.</span></span> <span data-ttu-id="8bfca-376">Otwórz **StoreController** klasę, aby zmienić **szczegóły** metody akcji.</span><span class="sxs-lookup"><span data-stu-id="8bfca-376">Open the **StoreController** class to change the **Details** action method.</span></span> <span data-ttu-id="8bfca-377">Aby to zrobić, w **Eksploratora rozwiązań**, rozwiń węzeł **kontrolerów** folder i kliknij dwukrotnie **StoreController.cs**.</span><span class="sxs-lookup"><span data-stu-id="8bfca-377">To do this, in the **Solution Explorer**, expand the **Controllers** folder and double-click **StoreController.cs**.</span></span>
2. <span data-ttu-id="8bfca-378">Zmień **szczegóły** na podstawie metody akcji można pobrać szczegółów albumów ich **identyfikator**. Aby to zrobić, Zastąp następujący kod:</span><span class="sxs-lookup"><span data-stu-id="8bfca-378">Change the **Details** action method to retrieve albums details based on their **Id**. To do this, replace the following code:</span></span>

    <span data-ttu-id="8bfca-379">(Fragment - kodu *modeli i dostęp do danych - Ex3 StoreController DetailsMethod*)</span><span class="sxs-lookup"><span data-stu-id="8bfca-379">(Code Snippet - *Models And Data Access - Ex3 StoreController DetailsMethod*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample18.cs)]

<a id="Ex3Task4"></a>

<a id="Task_4_-_Running_the_Application"></a>
#### <a name="task-4---running-the-application"></a><span data-ttu-id="8bfca-380">Zadanie 4 — uruchamianie aplikacji</span><span class="sxs-lookup"><span data-stu-id="8bfca-380">Task 4 - Running the Application</span></span>

<span data-ttu-id="8bfca-381">W tym zadaniu zostanie uruchomić aplikację w przeglądarce sieci web i Uzyskiwanie szczegółów albumu według ich identyfikatorów.</span><span class="sxs-lookup"><span data-stu-id="8bfca-381">In this task, you will run the Application in a web browser and obtain album details by their Id.</span></span>

1. <span data-ttu-id="8bfca-382">Naciśnij klawisz **F5** do uruchomienia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="8bfca-382">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="8bfca-383">Na stronie głównej datę rozpoczęcia projektu.</span><span class="sxs-lookup"><span data-stu-id="8bfca-383">The project starts in the Home page.</span></span> <span data-ttu-id="8bfca-384">Zmień adres URL do **/Store/Details/51** lub gatunki Przeglądaj i wybierz album, aby zweryfikować, że wyniki są pobierane z bazy danych.</span><span class="sxs-lookup"><span data-stu-id="8bfca-384">Change the URL to **/Store/Details/51** or browse the genres and select an album to verify that the results are being retrieved from the database.</span></span>

    <span data-ttu-id="8bfca-385">![Przeglądanie szczegółów](aspnet-mvc-4-models-and-data-access/_static/image25.png "przeglądania szczegółów")</span><span class="sxs-lookup"><span data-stu-id="8bfca-385">![Browsing Details](aspnet-mvc-4-models-and-data-access/_static/image25.png "Browsing Details")</span></span>

    <span data-ttu-id="8bfca-386">*Przeglądanie /Store/Details/51*</span><span class="sxs-lookup"><span data-stu-id="8bfca-386">*Browsing /Store/Details/51*</span></span>

> [!NOTE]
> <span data-ttu-id="8bfca-387">Ponadto można wdrożyć tę aplikację systemu Windows Azure Web Sites następujących [dodatek B: publikowania aplikacji ASP.NET MVC 4 przy użyciu narzędzia Web Deploy](#AppendixB).</span><span class="sxs-lookup"><span data-stu-id="8bfca-387">Additionally, you can deploy this application to Windows Azure Web Sites following [Appendix B: Publishing an ASP.NET MVC 4 Application using Web Deploy](#AppendixB).</span></span>

* * *

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a><span data-ttu-id="8bfca-388">Podsumowanie</span><span class="sxs-lookup"><span data-stu-id="8bfca-388">Summary</span></span>

<span data-ttu-id="8bfca-389">Poprzez wykonanie tego laboratorium Hands-on, kiedy znasz już podstawy ASP.NET MVC modeli i dostęp do danych, za pomocą **Database First** podejście, jak również **Code First** podejścia:</span><span class="sxs-lookup"><span data-stu-id="8bfca-389">By completing this Hands-on Lab you have learned the fundamentals of ASP.NET MVC Models and Data Access, using the **Database First** approach as well as the **Code First** Approach:</span></span>

- <span data-ttu-id="8bfca-390">Jak dodać bazy danych do rozwiązania, aby można było korzystać z jego danych</span><span class="sxs-lookup"><span data-stu-id="8bfca-390">How to add a database to the solution in order to consume its data</span></span>
- <span data-ttu-id="8bfca-391">Jak zaktualizować kontrolerów dostarczanie szablonów widoków danych z bazy danych zamiast ustalony</span><span class="sxs-lookup"><span data-stu-id="8bfca-391">How to update Controllers to provide View templates with the data taken from the database instead of hard-coded one</span></span>
- <span data-ttu-id="8bfca-392">Jak wykonać zapytanie dotyczące bazy danych przy użyciu parametrów</span><span class="sxs-lookup"><span data-stu-id="8bfca-392">How to query the database using parameters</span></span>
- <span data-ttu-id="8bfca-393">Jak używać zapytania wynik Przekształcanie, funkcja, która ogranicza liczbę uzyskuje dostęp do bazy danych, pobieranie danych w bardziej wydajny sposób</span><span class="sxs-lookup"><span data-stu-id="8bfca-393">How to use the Query Result Shaping, a feature that reduces the number of database accesses, retrieving data in a more efficient way</span></span>
- <span data-ttu-id="8bfca-394">Sposób korzystania z bazy danych imiona i Code First zbliża się do programu Microsoft Entity Framework do połączenia z modelem bazy danych</span><span class="sxs-lookup"><span data-stu-id="8bfca-394">How to use both Database First and Code First approaches in Microsoft Entity Framework to link the database with the model</span></span>

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a><span data-ttu-id="8bfca-395">Dodatek A: Instalowanie programu Visual Studio Express 2012 for Web</span><span class="sxs-lookup"><span data-stu-id="8bfca-395">Appendix A: Installing Visual Studio Express 2012 for Web</span></span>

<span data-ttu-id="8bfca-396">Można zainstalować **Microsoft Visual Studio Express 2012 for Web** lub innym &quot;Express&quot; przy użyciu wersji **[Instalatora platformy sieci Web firmy Microsoft](https://www.microsoft.com/web/downloads/platform.aspx)**.</span><span class="sxs-lookup"><span data-stu-id="8bfca-396">You can install **Microsoft Visual Studio Express 2012 for Web** or another &quot;Express&quot; version using the **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**.</span></span> <span data-ttu-id="8bfca-397">Poniższe instrukcje przedstawiono czynności wymagane do zainstalowania *programu Visual studio Express 2012 for Web* przy użyciu *Instalatora platformy sieci Web firmy Microsoft*.</span><span class="sxs-lookup"><span data-stu-id="8bfca-397">The following instructions guide you through the steps required to install *Visual studio Express 2012 for Web* using *Microsoft Web Platform Installer*.</span></span>

1. <span data-ttu-id="8bfca-398">Przejdź do [ [ https://go.microsoft.com/?linkid=9810169 ](https://go.microsoft.com/?linkid=9810169) ](https://go.microsoft.com/?linkid=9810169).</span><span class="sxs-lookup"><span data-stu-id="8bfca-398">Go to [[https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169).</span></span> <span data-ttu-id="8bfca-399">Alternatywnie, jeśli została już zainstalowana Instalatora platformy sieci Web, można otworzyć go i Wyszukaj produkt &quot; <em>programu Visual Studio Express 2012 for Web z zestawem Windows Azure SDK</em>&quot;.</span><span class="sxs-lookup"><span data-stu-id="8bfca-399">Alternatively, if you already have installed Web Platform Installer, you can open it and search for the product &quot;<em>Visual Studio Express 2012 for Web with Windows Azure SDK</em>&quot;.</span></span>
2. <span data-ttu-id="8bfca-400">Polecenie **teraz zainstalować**.</span><span class="sxs-lookup"><span data-stu-id="8bfca-400">Click on **Install Now**.</span></span> <span data-ttu-id="8bfca-401">Jeśli nie masz **Instalatora platformy sieci Web** nastąpi przekierowanie do pobrania i zainstalowania go najpierw.</span><span class="sxs-lookup"><span data-stu-id="8bfca-401">If you do not have **Web Platform Installer** you will be redirected to download and install it first.</span></span>
3. <span data-ttu-id="8bfca-402">Raz **Instalatora platformy sieci Web** jest otwarty, kliknij przycisk **zainstalować** można uruchomić Instalatora.</span><span class="sxs-lookup"><span data-stu-id="8bfca-402">Once **Web Platform Installer** is open, click **Install** to start the setup.</span></span>

    <span data-ttu-id="8bfca-403">![Instalowanie programu Visual Studio Express](aspnet-mvc-4-models-and-data-access/_static/image26.png "instalacji programu Visual Studio Express")</span><span class="sxs-lookup"><span data-stu-id="8bfca-403">![Install Visual Studio Express](aspnet-mvc-4-models-and-data-access/_static/image26.png "Install Visual Studio Express")</span></span>

    <span data-ttu-id="8bfca-404">*Instalowanie programu Visual Studio Express*</span><span class="sxs-lookup"><span data-stu-id="8bfca-404">*Install Visual Studio Express*</span></span>
4. <span data-ttu-id="8bfca-405">Odczytywanie wszystkich produktów licencji i warunków, a następnie kliknij przycisk **akceptuję** aby kontynuować.</span><span class="sxs-lookup"><span data-stu-id="8bfca-405">Read all the products' licenses and terms and click **I Accept** to continue.</span></span>

    ![Akceptowanie umowy licencyjnej](aspnet-mvc-4-models-and-data-access/_static/image27.png)

    <span data-ttu-id="8bfca-407">*Akceptowanie umowy licencyjnej*</span><span class="sxs-lookup"><span data-stu-id="8bfca-407">*Accepting the license terms*</span></span>
5. <span data-ttu-id="8bfca-408">Poczekaj na zakończenie procesu pobierania i instalacji.</span><span class="sxs-lookup"><span data-stu-id="8bfca-408">Wait until the downloading and installation process completes.</span></span>

    ![Postęp instalacji](aspnet-mvc-4-models-and-data-access/_static/image28.png)

    <span data-ttu-id="8bfca-410">*Postęp instalacji*</span><span class="sxs-lookup"><span data-stu-id="8bfca-410">*Installation progress*</span></span>
6. <span data-ttu-id="8bfca-411">Po zakończeniu instalacji kliknij przycisk **Zakończ**.</span><span class="sxs-lookup"><span data-stu-id="8bfca-411">When the installation completes, click **Finish**.</span></span>

    ![Instalacja została zakończona](aspnet-mvc-4-models-and-data-access/_static/image29.png)

    <span data-ttu-id="8bfca-413">*Instalacja została zakończona*</span><span class="sxs-lookup"><span data-stu-id="8bfca-413">*Installation completed*</span></span>
7. <span data-ttu-id="8bfca-414">Kliknij przycisk **zakończenia** aby zamknąć Instalatora platformy sieci Web.</span><span class="sxs-lookup"><span data-stu-id="8bfca-414">Click **Exit** to close Web Platform Installer.</span></span>
8. <span data-ttu-id="8bfca-415">Aby otworzyć program Visual Studio Express for Web, przejdź do **Start** ekranu i zacznij pisać &quot; **VS Express**&quot;, następnie kliknij polecenie **VS Express for Web** Kafelek.</span><span class="sxs-lookup"><span data-stu-id="8bfca-415">To open Visual Studio Express for Web, go to the **Start** screen and start writing &quot;**VS Express**&quot;, then click on the **VS Express for Web** tile.</span></span>

    ![VS Express for Web kafelka](aspnet-mvc-4-models-and-data-access/_static/image30.png)

    <span data-ttu-id="8bfca-417">*VS Express for Web kafelka*</span><span class="sxs-lookup"><span data-stu-id="8bfca-417">*VS Express for Web tile*</span></span>

<a id="AppendixB"></a>

<a id="Appendix_B_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
## <a name="appendix-b-publishing-an-aspnet-mvc-4-application-using-web-deploy"></a><span data-ttu-id="8bfca-418">Dodatek B: publikowania aplikacji ASP.NET MVC 4 przy użyciu narzędzia Web Deploy</span><span class="sxs-lookup"><span data-stu-id="8bfca-418">Appendix B: Publishing an ASP.NET MVC 4 Application using Web Deploy</span></span>

<span data-ttu-id="8bfca-419">Ten dodatek opisano sposób tworzenia nowej witryny sieci web z portalu zarządzania pakietu Windows Azure i publikowanie aplikacji, uzyskane wykonując laboratorium, korzystając z funkcji publikowania narzędzia Web Deploy dostarczane przez Windows Azure.</span><span class="sxs-lookup"><span data-stu-id="8bfca-419">This appendix will show you how to create a new web site from the Windows Azure Management Portal and publish the application you obtained by following the lab, taking advantage of the Web Deploy publishing feature provided by Windows Azure.</span></span>

<a id="ApxBTask1"></a>

<a id="Task_1_-_Creating_a_New_Web_Site_from_the_Windows_Azure_Portal"></a>
#### <a name="task-1---creating-a-new-web-site-from-the-windows-azure-portal"></a><span data-ttu-id="8bfca-420">Zadanie 1 — Tworzenie nowej witryny sieci Web systemu Windows portalu Azure</span><span class="sxs-lookup"><span data-stu-id="8bfca-420">Task 1 - Creating a New Web Site from the Windows Azure Portal</span></span>

1. <span data-ttu-id="8bfca-421">Przejdź do [portalu zarządzania pakietu Windows Azure](https://manage.windowsazure.com/) i zaloguj się przy użyciu poświadczeń Microsoft skojarzonych z Twoją subskrypcją.</span><span class="sxs-lookup"><span data-stu-id="8bfca-421">Go to the [Windows Azure Management Portal](https://manage.windowsazure.com/) and sign in using the Microsoft credentials associated with your subscription.</span></span>

    > [!NOTE]
    > <span data-ttu-id="8bfca-422">Z systemu Windows Azure można udostępniać 10 witryn sieci Web platformy ASP.NET bezpłatnie i następnie Skaluj w miarę zwiększania się ruchu.</span><span class="sxs-lookup"><span data-stu-id="8bfca-422">With Windows Azure you can host 10 ASP.NET Web Sites for free and then scale as your traffic grows.</span></span> <span data-ttu-id="8bfca-423">Możesz utworzyć konto [tutaj](http://aka.ms/aspnet-hol-azure).</span><span class="sxs-lookup"><span data-stu-id="8bfca-423">You can sign up [here](http://aka.ms/aspnet-hol-azure).</span></span>

    <span data-ttu-id="8bfca-424">![Zaloguj się do portalu Windows Azure](aspnet-mvc-4-models-and-data-access/_static/image31.png "Zaloguj się do portalu Windows Azure")</span><span class="sxs-lookup"><span data-stu-id="8bfca-424">![Log on to Windows Azure portal](aspnet-mvc-4-models-and-data-access/_static/image31.png "Log on to Windows Azure portal")</span></span>

    <span data-ttu-id="8bfca-425">*Zaloguj się do portalu zarządzania platformy Azure z systemem Windows*</span><span class="sxs-lookup"><span data-stu-id="8bfca-425">*Log on to Windows Azure Management Portal*</span></span>
2. <span data-ttu-id="8bfca-426">Kliknij przycisk **nowy** na pasku poleceń.</span><span class="sxs-lookup"><span data-stu-id="8bfca-426">Click **New** on the command bar.</span></span>

    <span data-ttu-id="8bfca-427">![Tworzenie nowej witryny sieci Web](aspnet-mvc-4-models-and-data-access/_static/image32.png "tworzenia nowej witryny sieci Web")</span><span class="sxs-lookup"><span data-stu-id="8bfca-427">![Creating a new Web Site](aspnet-mvc-4-models-and-data-access/_static/image32.png "Creating a new Web Site")</span></span>

    <span data-ttu-id="8bfca-428">*Tworzenie nowej witryny sieci Web*</span><span class="sxs-lookup"><span data-stu-id="8bfca-428">*Creating a new Web Site*</span></span>
3. <span data-ttu-id="8bfca-429">Kliknij przycisk **obliczeniowe** | **witryny sieci Web**.</span><span class="sxs-lookup"><span data-stu-id="8bfca-429">Click **Compute** | **Web Site**.</span></span> <span data-ttu-id="8bfca-430">Następnie wybierz **szybkie tworzenie** opcji.</span><span class="sxs-lookup"><span data-stu-id="8bfca-430">Then select **Quick Create** option.</span></span> <span data-ttu-id="8bfca-431">Podaj dostępny adres URL dla nowej witryny sieci web, a następnie kliknij przycisk **tworzenie witryny sieci Web**.</span><span class="sxs-lookup"><span data-stu-id="8bfca-431">Provide an available URL for the new web site and click **Create Web Site**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="8bfca-432">Witryny sieci Web systemu Windows Azure jest hostem dla aplikacji sieci web w chmurze, które można kontrolować i zarządzanie nimi.</span><span class="sxs-lookup"><span data-stu-id="8bfca-432">A Windows Azure Web Site is the host for a web application running in the cloud that you can control and manage.</span></span> <span data-ttu-id="8bfca-433">Opcja szybkie tworzenie umożliwia wdrażanie ukończonej aplikacji sieci web do systemu Windows Azure witryny internetowej z spoza portalu.</span><span class="sxs-lookup"><span data-stu-id="8bfca-433">The Quick Create option allows you to deploy a completed web application to the Windows Azure Web Site from outside the portal.</span></span> <span data-ttu-id="8bfca-434">Nie obejmuje kroki konfigurowania bazy danych.</span><span class="sxs-lookup"><span data-stu-id="8bfca-434">It does not include steps for setting up a database.</span></span>

    <span data-ttu-id="8bfca-435">![Tworzenie nowej witryny sieci Web przy użyciu szybkie tworzenie](aspnet-mvc-4-models-and-data-access/_static/image33.png "tworzenia nowej witryny sieci Web przy użyciu szybkie tworzenie")</span><span class="sxs-lookup"><span data-stu-id="8bfca-435">![Creating a new Web Site using Quick Create](aspnet-mvc-4-models-and-data-access/_static/image33.png "Creating a new Web Site using Quick Create")</span></span>

    <span data-ttu-id="8bfca-436">*Tworzenie nowej witryny sieci Web przy użyciu szybkie tworzenie*</span><span class="sxs-lookup"><span data-stu-id="8bfca-436">*Creating a new Web Site using Quick Create*</span></span>
4. <span data-ttu-id="8bfca-437">Poczekaj na nowe **witryny sieci Web** jest tworzony.</span><span class="sxs-lookup"><span data-stu-id="8bfca-437">Wait until the new **Web Site** is created.</span></span>
5. <span data-ttu-id="8bfca-438">Po utworzeniu witryny sieci Web kliknij łącze w obszarze **adres URL** kolumny.</span><span class="sxs-lookup"><span data-stu-id="8bfca-438">Once the Web Site is created click the link under the **URL** column.</span></span> <span data-ttu-id="8bfca-439">Sprawdź, czy działa nowej witryny sieci Web.</span><span class="sxs-lookup"><span data-stu-id="8bfca-439">Check that the new Web Site is working.</span></span>

    <span data-ttu-id="8bfca-440">![Przeglądanie do nowej witryny sieci web](aspnet-mvc-4-models-and-data-access/_static/image34.png "przeglądanie do nowej witryny sieci web")</span><span class="sxs-lookup"><span data-stu-id="8bfca-440">![Browsing to the new web site](aspnet-mvc-4-models-and-data-access/_static/image34.png "Browsing to the new web site")</span></span>

    <span data-ttu-id="8bfca-441">*Przeglądanie do nowej witryny sieci web*</span><span class="sxs-lookup"><span data-stu-id="8bfca-441">*Browsing to the new web site*</span></span>

    <span data-ttu-id="8bfca-442">![Witryna sieci Web działa](aspnet-mvc-4-models-and-data-access/_static/image35.png "uruchamiania witryny sieci Web")</span><span class="sxs-lookup"><span data-stu-id="8bfca-442">![Web site running](aspnet-mvc-4-models-and-data-access/_static/image35.png "Web site running")</span></span>

    <span data-ttu-id="8bfca-443">*Witryna sieci Web uruchomiona*</span><span class="sxs-lookup"><span data-stu-id="8bfca-443">*Web site running*</span></span>
6. <span data-ttu-id="8bfca-444">Wróć do portalu i kliknij nazwę witryny sieci web w obszarze **nazwa** kolumny do wyświetlenia strony zarządzania.</span><span class="sxs-lookup"><span data-stu-id="8bfca-444">Go back to the portal and click the name of the web site under the **Name** column to display the management pages.</span></span>

    <span data-ttu-id="8bfca-445">![Otwieranie stron witryny sieci web zarządzania](aspnet-mvc-4-models-and-data-access/_static/image36.png "otwieranie stron zarządzania witryny sieci web")</span><span class="sxs-lookup"><span data-stu-id="8bfca-445">![Opening the web site management pages](aspnet-mvc-4-models-and-data-access/_static/image36.png "Opening the web site management pages")</span></span>

    <span data-ttu-id="8bfca-446">*Otwieranie stron zarządzania witryny sieci Web*</span><span class="sxs-lookup"><span data-stu-id="8bfca-446">*Opening the Web Site management pages*</span></span>
7. <span data-ttu-id="8bfca-447">W **pulpitu nawigacyjnego** w obszarze **szybkiego dostępu** kliknij **pobieranie profilu publikowania** łącza.</span><span class="sxs-lookup"><span data-stu-id="8bfca-447">In the **Dashboard** page, under the **quick glance** section, click the **Download publish profile** link.</span></span>

    > [!NOTE]
    > <span data-ttu-id="8bfca-448">*Profilu publikowania* zawiera wszystkie informacje wymagane do publikowania aplikacji sieci web do witryny sieci Web systemu Windows Azure dla każdej metody włączone publikacji.</span><span class="sxs-lookup"><span data-stu-id="8bfca-448">The *publish profile* contains all of the information required to publish a web application to a Windows Azure website for each enabled publication method.</span></span> <span data-ttu-id="8bfca-449">Profil publikowania zawiera adresy URL, poświadczenia użytkownika i parametry bazy danych wymagane do nawiązania połączenia i uwierzytelniania dla każdego z punktów końcowych, dla których włączono metoda publikacji.</span><span class="sxs-lookup"><span data-stu-id="8bfca-449">The publish profile contains the URLs, user credentials and database strings required to connect to and authenticate against each of the endpoints for which a publication method is enabled.</span></span> <span data-ttu-id="8bfca-450">**Microsoft WebMatrix 2**, **Microsoft Visual Studio Express for Web** i **programu Microsoft Visual Studio 2012** obsługują odczytywanie publikowanie profile do zautomatyzowania te programy Publikowanie aplikacji sieci web do witryn sieci Web systemu Windows Azure.</span><span class="sxs-lookup"><span data-stu-id="8bfca-450">**Microsoft WebMatrix 2**, **Microsoft Visual Studio Express for Web** and **Microsoft Visual Studio 2012** support reading publish profiles to automate configuration of these programs for publishing web applications to Windows Azure websites.</span></span>

    <span data-ttu-id="8bfca-451">![Pobieranie witryny sieci web profilu publikowania](aspnet-mvc-4-models-and-data-access/_static/image37.png "pobierania witryny sieci web profilu publikowania")</span><span class="sxs-lookup"><span data-stu-id="8bfca-451">![Downloading the web site publish profile](aspnet-mvc-4-models-and-data-access/_static/image37.png "Downloading the web site publish profile")</span></span>

    <span data-ttu-id="8bfca-452">*Pobieranie witryny sieci Web profilu publikowania*</span><span class="sxs-lookup"><span data-stu-id="8bfca-452">*Downloading the Web Site publish profile*</span></span>
8. <span data-ttu-id="8bfca-453">Pobierz profil publikowania w znanej lokalizacji.</span><span class="sxs-lookup"><span data-stu-id="8bfca-453">Download the publish profile file to a known location.</span></span> <span data-ttu-id="8bfca-454">Dodatkowo w tym ćwiczeniu zobaczysz jak opublikować aplikację sieci web do witryny sieci Web systemu Windows Azure w programie Visual Studio przy użyciu tego pliku.</span><span class="sxs-lookup"><span data-stu-id="8bfca-454">Further in this exercise you will see how to use this file to publish a web application to a Windows Azure Web Sites from Visual Studio.</span></span>

    <span data-ttu-id="8bfca-455">![Zapisywanie pliku profilu publikowania](aspnet-mvc-4-models-and-data-access/_static/image38.png "zapisywanie profilu publikowania")</span><span class="sxs-lookup"><span data-stu-id="8bfca-455">![Saving the publish profile file](aspnet-mvc-4-models-and-data-access/_static/image38.png "Saving the publish profile")</span></span>

    <span data-ttu-id="8bfca-456">*Zapisywanie pliku profilu publikowania*</span><span class="sxs-lookup"><span data-stu-id="8bfca-456">*Saving the publish profile file*</span></span>

<a id="ApxBTask2"></a>

<a id="Task_2_-_Configuring_the_Database_Server"></a>
#### <a name="task-2---configuring-the-database-server"></a><span data-ttu-id="8bfca-457">Zadanie 2 — Konfigurowanie serwera bazy danych</span><span class="sxs-lookup"><span data-stu-id="8bfca-457">Task 2 - Configuring the Database Server</span></span>

<span data-ttu-id="8bfca-458">Jeśli aplikacja korzysta z programu SQL Server baz danych, należy utworzyć serwer bazy danych SQL.</span><span class="sxs-lookup"><span data-stu-id="8bfca-458">If your application makes use of SQL Server databases you will need to create a SQL Database server.</span></span> <span data-ttu-id="8bfca-459">Jeśli chcesz wdrożyć prostą aplikację, która nie korzysta z programu SQL Server może pominąć to zadanie.</span><span class="sxs-lookup"><span data-stu-id="8bfca-459">If you want to deploy a simple application that does not use SQL Server you might skip this task.</span></span>

1. <span data-ttu-id="8bfca-460">Będzie potrzebny serwer bazy danych SQL do przechowywania bazy danych aplikacji.</span><span class="sxs-lookup"><span data-stu-id="8bfca-460">You will need a SQL Database server for storing the application database.</span></span> <span data-ttu-id="8bfca-461">Można wyświetlić serwery bazy danych SQL z subskrypcji usługi Windows Azure Management Portal pod adresem **baz danych Sql** | **serwerów** | **serwera Pulpit nawigacyjny**.</span><span class="sxs-lookup"><span data-stu-id="8bfca-461">You can view the SQL Database servers from your subscription in the Windows Azure Management portal at **Sql Databases** | **Servers** | **Server's Dashboard**.</span></span> <span data-ttu-id="8bfca-462">Jeśli nie masz serwer, który został utworzony, można utworzyć przy użyciu jednego **Dodaj** przycisk paska poleceń.</span><span class="sxs-lookup"><span data-stu-id="8bfca-462">If you do not have a server created, you can create one using the **Add** button on the command bar.</span></span> <span data-ttu-id="8bfca-463">Zwróć uwagę na **nazwę serwera i adres URL, nazwę logowania administratora i hasła**, jak będą używane w następnego zadania.</span><span class="sxs-lookup"><span data-stu-id="8bfca-463">Take note of the **server name and URL, administrator login name and password**, as you will use them in the next tasks.</span></span> <span data-ttu-id="8bfca-464">Nie należy tworzyć bazy danych jeszcze, jako zostaną utworzone w późniejszym terminie.</span><span class="sxs-lookup"><span data-stu-id="8bfca-464">Do not create the database yet, as it will be created in a later stage.</span></span>

    <span data-ttu-id="8bfca-465">![Pulpit nawigacyjny serwera bazy danych SQL](aspnet-mvc-4-models-and-data-access/_static/image39.png "pulpitu nawigacyjnego serwera bazy danych SQL")</span><span class="sxs-lookup"><span data-stu-id="8bfca-465">![SQL Database Server Dashboard](aspnet-mvc-4-models-and-data-access/_static/image39.png "SQL Database Server Dashboard")</span></span>

    <span data-ttu-id="8bfca-466">*Pulpit nawigacyjny serwera bazy danych SQL*</span><span class="sxs-lookup"><span data-stu-id="8bfca-466">*SQL Database Server Dashboard*</span></span>
2. <span data-ttu-id="8bfca-467">W następnym zadaniem Testuj połączenie z bazą danych z programu Visual Studio z tego powodu należy uwzględnić lokalny adres IP serwera liście **dozwolone adresy IP**.</span><span class="sxs-lookup"><span data-stu-id="8bfca-467">In the next task you will test the database connection from Visual Studio, for that reason you need to include your local IP address in the server's list of **Allowed IP Addresses**.</span></span> <span data-ttu-id="8bfca-468">Aby to zrobić, kliknij przycisk **Konfiguruj**, wybierz adres IP z **bieżącego adresu IP klienta** i wklej go na **początkowy adres IP** i **końcowy adres IP** pola tekstowe i kliknij przycisk ![add-client-ip-address-ok-button](aspnet-mvc-4-models-and-data-access/_static/image40.png) przycisku.</span><span class="sxs-lookup"><span data-stu-id="8bfca-468">To do that, click **Configure**, select the IP address from **Current Client IP Address** and paste it on the **Start IP Address** and **End IP Address** text boxes and click the ![add-client-ip-address-ok-button](aspnet-mvc-4-models-and-data-access/_static/image40.png) button.</span></span>

    ![Dodawanie adresu IP klienta](aspnet-mvc-4-models-and-data-access/_static/image41.png)

    <span data-ttu-id="8bfca-470">*Dodawanie adresu IP klienta*</span><span class="sxs-lookup"><span data-stu-id="8bfca-470">*Adding Client IP Address*</span></span>
3. <span data-ttu-id="8bfca-471">Raz **adres IP klienta** jest dodawany do dozwolonych adresów IP kliknij na **zapisać** o potwierdzenie zmian.</span><span class="sxs-lookup"><span data-stu-id="8bfca-471">Once the **Client IP Address** is added to the allowed IP addresses list, click on **Save** to confirm the changes.</span></span>

    ![Potwierdzenie zmian](aspnet-mvc-4-models-and-data-access/_static/image42.png)

    <span data-ttu-id="8bfca-473">*Potwierdzenie zmian*</span><span class="sxs-lookup"><span data-stu-id="8bfca-473">*Confirm Changes*</span></span>

<a id="ApxBTask3"></a>

<a id="Task_3_-_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
#### <a name="task-3---publishing-an-aspnet-mvc-4-application-using-web-deploy"></a><span data-ttu-id="8bfca-474">Zadanie 3 - publikowania aplikacji ASP.NET MVC 4 przy użyciu narzędzia Web Deploy</span><span class="sxs-lookup"><span data-stu-id="8bfca-474">Task 3 - Publishing an ASP.NET MVC 4 Application using Web Deploy</span></span>

1. <span data-ttu-id="8bfca-475">Wróć do rozwiązania ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="8bfca-475">Go back to the ASP.NET MVC 4 solution.</span></span> <span data-ttu-id="8bfca-476">W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy projekt witryny sieci web i wybierz **publikowania**.</span><span class="sxs-lookup"><span data-stu-id="8bfca-476">In the **Solution Explorer**, right-click the web site project and select **Publish**.</span></span>

    <span data-ttu-id="8bfca-477">![Publikowanie aplikacji](aspnet-mvc-4-models-and-data-access/_static/image43.png "publikowania aplikacji")</span><span class="sxs-lookup"><span data-stu-id="8bfca-477">![Publishing the Application](aspnet-mvc-4-models-and-data-access/_static/image43.png "Publishing the Application")</span></span>

    <span data-ttu-id="8bfca-478">*Publikowanie witryny sieci web*</span><span class="sxs-lookup"><span data-stu-id="8bfca-478">*Publishing the web site*</span></span>
2. <span data-ttu-id="8bfca-479">Zaimportuj profil publikowania, zapisana w pierwszym zadaniu.</span><span class="sxs-lookup"><span data-stu-id="8bfca-479">Import the publish profile you saved in the first task.</span></span>

    <span data-ttu-id="8bfca-480">![Importowanie profilu publikowania](aspnet-mvc-4-models-and-data-access/_static/image44.png "Importowanie profilu publikowania")</span><span class="sxs-lookup"><span data-stu-id="8bfca-480">![Importing the publish profile](aspnet-mvc-4-models-and-data-access/_static/image44.png "Importing the publish profile")</span></span>

    <span data-ttu-id="8bfca-481">*Importowanie profilu publikowania*</span><span class="sxs-lookup"><span data-stu-id="8bfca-481">*Importing publish profile*</span></span>
3. <span data-ttu-id="8bfca-482">Kliknij przycisk **Weryfikacja połączenia z**.</span><span class="sxs-lookup"><span data-stu-id="8bfca-482">Click **Validate Connection**.</span></span> <span data-ttu-id="8bfca-483">Po zakończeniu sprawdzania kliknij **dalej**.</span><span class="sxs-lookup"><span data-stu-id="8bfca-483">Once Validation is complete click **Next**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="8bfca-484">Zakończeniu sprawdzania poprawności, gdy zostanie wyświetlony zielony znacznik wyboru są wyświetlane obok przycisku sprawdzania poprawności połączenia.</span><span class="sxs-lookup"><span data-stu-id="8bfca-484">Validation is complete once you see a green checkmark appear next to the Validate Connection button.</span></span>

    <span data-ttu-id="8bfca-485">![Sprawdzanie poprawności połączenia](aspnet-mvc-4-models-and-data-access/_static/image45.png "sprawdzanie poprawności połączenia")</span><span class="sxs-lookup"><span data-stu-id="8bfca-485">![Validating connection](aspnet-mvc-4-models-and-data-access/_static/image45.png "Validating connection")</span></span>

    <span data-ttu-id="8bfca-486">*Sprawdzanie poprawności połączenia*</span><span class="sxs-lookup"><span data-stu-id="8bfca-486">*Validating connection*</span></span>
4. <span data-ttu-id="8bfca-487">W **ustawienia** w obszarze **baz danych** sekcji, kliknij przycisk Dalej, aby textbox połączenia bazy danych (tj. **połączenia DefaultConnection**).</span><span class="sxs-lookup"><span data-stu-id="8bfca-487">In the **Settings** page, under the **Databases** section, click the button next to your database connection's textbox (i.e. **DefaultConnection**).</span></span>

    <span data-ttu-id="8bfca-488">![Konfiguracja narzędzia Web deploy](aspnet-mvc-4-models-and-data-access/_static/image46.png "Konfiguracja narzędzia Web deploy")</span><span class="sxs-lookup"><span data-stu-id="8bfca-488">![Web deploy configuration](aspnet-mvc-4-models-and-data-access/_static/image46.png "Web deploy configuration")</span></span>

    <span data-ttu-id="8bfca-489">*Konfiguracja narzędzia Web deploy*</span><span class="sxs-lookup"><span data-stu-id="8bfca-489">*Web deploy configuration*</span></span>
5. <span data-ttu-id="8bfca-490">Skonfiguruj połączenie z bazą danych w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="8bfca-490">Configure the database connection as follows:</span></span>

   - <span data-ttu-id="8bfca-491">W **nazwy serwera** wpisz swoją bazą danych SQL server adresu URL przy użyciu *tcp:* prefiks.</span><span class="sxs-lookup"><span data-stu-id="8bfca-491">In the **Server name** type your SQL Database server URL using the *tcp:* prefix.</span></span>
   - <span data-ttu-id="8bfca-492">W **nazwy użytkownika** wpisz nazwę logowania administratora serwera.</span><span class="sxs-lookup"><span data-stu-id="8bfca-492">In **User name** type your server administrator login name.</span></span>
   - <span data-ttu-id="8bfca-493">W **hasło** wpisz hasło logowania administratora serwera.</span><span class="sxs-lookup"><span data-stu-id="8bfca-493">In **Password** type your server administrator login password.</span></span>
   - <span data-ttu-id="8bfca-494">Wpisz nazwę nowej bazy danych.</span><span class="sxs-lookup"><span data-stu-id="8bfca-494">Type a new database name.</span></span>

     <span data-ttu-id="8bfca-495">![Konfigurowanie parametrów połączenia z lokalizacją docelową](aspnet-mvc-4-models-and-data-access/_static/image47.png "Konfigurowanie parametrów połączenia z lokalizacją docelową")</span><span class="sxs-lookup"><span data-stu-id="8bfca-495">![Configuring destination connection string](aspnet-mvc-4-models-and-data-access/_static/image47.png "Configuring destination connection string")</span></span>

     <span data-ttu-id="8bfca-496">*Konfigurowanie parametrów połączenia z lokalizacją docelową*</span><span class="sxs-lookup"><span data-stu-id="8bfca-496">*Configuring destination connection string*</span></span>
6. <span data-ttu-id="8bfca-497">Następnie kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="8bfca-497">Then click **OK**.</span></span> <span data-ttu-id="8bfca-498">Po wyświetleniu monitu można utworzyć bazy danych kliknij **tak**.</span><span class="sxs-lookup"><span data-stu-id="8bfca-498">When prompted to create the database click **Yes**.</span></span>

    <span data-ttu-id="8bfca-499">![Tworzenie bazy danych](aspnet-mvc-4-models-and-data-access/_static/image48.png "tworzenie parametry bazy danych")</span><span class="sxs-lookup"><span data-stu-id="8bfca-499">![Creating the database](aspnet-mvc-4-models-and-data-access/_static/image48.png "Creating the database string")</span></span>

    <span data-ttu-id="8bfca-500">*Tworzenie bazy danych*</span><span class="sxs-lookup"><span data-stu-id="8bfca-500">*Creating the database*</span></span>
7. <span data-ttu-id="8bfca-501">Ciągu połączenia używanego do łączenia z bazą danych SQL w systemie Windows Azure jest wyświetlany w pole tekstowe domyślne połączenie.</span><span class="sxs-lookup"><span data-stu-id="8bfca-501">The connection string you will use to connect to SQL Database in Windows Azure is shown within Default Connection textbox.</span></span> <span data-ttu-id="8bfca-502">Następnie kliknij przycisk **Dalej**.</span><span class="sxs-lookup"><span data-stu-id="8bfca-502">Then click **Next**.</span></span>

    <span data-ttu-id="8bfca-503">![Parametry połączenia wskazujące bazę danych SQL](aspnet-mvc-4-models-and-data-access/_static/image49.png "ciąg połączenia wskazujące bazę danych SQL")</span><span class="sxs-lookup"><span data-stu-id="8bfca-503">![Connection string pointing to SQL Database](aspnet-mvc-4-models-and-data-access/_static/image49.png "Connection string pointing to SQL Database")</span></span>

    <span data-ttu-id="8bfca-504">*Parametry połączenia wskazujące bazę danych SQL*</span><span class="sxs-lookup"><span data-stu-id="8bfca-504">*Connection string pointing to SQL Database*</span></span>
8. <span data-ttu-id="8bfca-505">W **Podgląd** kliknij przycisk **publikowania**.</span><span class="sxs-lookup"><span data-stu-id="8bfca-505">In the **Preview** page, click **Publish**.</span></span>

    <span data-ttu-id="8bfca-506">![Publikowanie aplikacji sieci web](aspnet-mvc-4-models-and-data-access/_static/image50.png "publikowania aplikacji sieci web")</span><span class="sxs-lookup"><span data-stu-id="8bfca-506">![Publishing the web application](aspnet-mvc-4-models-and-data-access/_static/image50.png "Publishing the web application")</span></span>

    <span data-ttu-id="8bfca-507">*Publikowanie aplikacji sieci web*</span><span class="sxs-lookup"><span data-stu-id="8bfca-507">*Publishing the web application*</span></span>
9. <span data-ttu-id="8bfca-508">Po zakończeniu procesu publikowania domyślnej przeglądarce otworzy opublikowanej witryny sieci web.</span><span class="sxs-lookup"><span data-stu-id="8bfca-508">Once the publishing process finishes, your default browser will open the published web site.</span></span>

<a id="AppendixC"></a>

<a id="Appendix_C_Using_Code_Snippets"></a>
## <a name="appendix-c-using-code-snippets"></a><span data-ttu-id="8bfca-509">Dodatek C: korzystania z wstawek kodu</span><span class="sxs-lookup"><span data-stu-id="8bfca-509">Appendix C: Using Code Snippets</span></span>

<span data-ttu-id="8bfca-510">Wstawki kodu zapewniają całego kodu, które są potrzebne w zasięgu ręki.</span><span class="sxs-lookup"><span data-stu-id="8bfca-510">With code snippets, you have all the code you need at your fingertips.</span></span> <span data-ttu-id="8bfca-511">Dokument laboratorium informuje o dokładnie po ich użycia, jak pokazano na poniższej ilustracji.</span><span class="sxs-lookup"><span data-stu-id="8bfca-511">The lab document will tell you exactly when you can use them, as shown in the following figure.</span></span>

<span data-ttu-id="8bfca-512">![Aby wstawić kod do projektu przy użyciu wstawki kodu programu Visual Studio](aspnet-mvc-4-models-and-data-access/_static/image51.png "wstawki kodu za pomocą programu Visual Studio, aby wstawić kod do projektu")</span><span class="sxs-lookup"><span data-stu-id="8bfca-512">![Using Visual Studio code snippets to insert code into your project](aspnet-mvc-4-models-and-data-access/_static/image51.png "Using Visual Studio code snippets to insert code into your project")</span></span>

<span data-ttu-id="8bfca-513">*Aby wstawić kod do projektu przy użyciu wstawki kodu programu Visual Studio*</span><span class="sxs-lookup"><span data-stu-id="8bfca-513">*Using Visual Studio code snippets to insert code into your project*</span></span>

<span data-ttu-id="8bfca-514">***Aby dodać fragment kodu za pomocą klawiatury (C# tylko)***</span><span class="sxs-lookup"><span data-stu-id="8bfca-514">***To add a code snippet using the keyboard (C# only)***</span></span>

1. <span data-ttu-id="8bfca-515">Umieść kursor, w którym chcesz wstawić kod.</span><span class="sxs-lookup"><span data-stu-id="8bfca-515">Place the cursor where you would like to insert the code.</span></span>
2. <span data-ttu-id="8bfca-516">Zacznij wpisywać nazwę fragment (bez spacji i łączniki).</span><span class="sxs-lookup"><span data-stu-id="8bfca-516">Start typing the snippet name (without spaces or hyphens).</span></span>
3. <span data-ttu-id="8bfca-517">Obejrzyj jako IntelliSense wyświetla zgodne z nazwami wstawki.</span><span class="sxs-lookup"><span data-stu-id="8bfca-517">Watch as IntelliSense displays matching snippets' names.</span></span>
4. <span data-ttu-id="8bfca-518">Wybierz prawidłowe fragment (lub zachować wpisywanie do momentu wybrania fragment całą nazwę).</span><span class="sxs-lookup"><span data-stu-id="8bfca-518">Select the correct snippet (or keep typing until the entire snippet's name is selected).</span></span>
5. <span data-ttu-id="8bfca-519">Naciśnij klawisz Tab dwa razy, aby wstawić fragment kodu w lokalizacji kursora.</span><span class="sxs-lookup"><span data-stu-id="8bfca-519">Press the Tab key twice to insert the snippet at the cursor location.</span></span>

<span data-ttu-id="8bfca-520">![Rozpocznij wpisywanie nazwy fragment](aspnet-mvc-4-models-and-data-access/_static/image52.png "zacznij wpisywać nazwę wstawki programu")</span><span class="sxs-lookup"><span data-stu-id="8bfca-520">![Start typing the snippet name](aspnet-mvc-4-models-and-data-access/_static/image52.png "Start typing the snippet name")</span></span>

<span data-ttu-id="8bfca-521">*Rozpocznij wpisywanie nazwy fragment kodu*</span><span class="sxs-lookup"><span data-stu-id="8bfca-521">*Start typing the snippet name*</span></span>

<span data-ttu-id="8bfca-522">![Naciśnij klawisz Tab, aby wybrać wyróżnione fragment](aspnet-mvc-4-models-and-data-access/_static/image53.png "naciśnij klawisz Tab, aby wybrać wyróżnione fragment kodu")</span><span class="sxs-lookup"><span data-stu-id="8bfca-522">![Press Tab to select the highlighted snippet](aspnet-mvc-4-models-and-data-access/_static/image53.png "Press Tab to select the highlighted snippet")</span></span>

<span data-ttu-id="8bfca-523">*Naciśnij klawisz Tab, aby wybrać wyróżnione fragment kodu*</span><span class="sxs-lookup"><span data-stu-id="8bfca-523">*Press Tab to select the highlighted snippet*</span></span>

<span data-ttu-id="8bfca-524">![Ponownie naciśnij klawisz Tab i fragment rozwinie](aspnet-mvc-4-models-and-data-access/_static/image54.png "rozwinie ponownie naciśnij klawisz Tab i wstawki kodu")</span><span class="sxs-lookup"><span data-stu-id="8bfca-524">![Press Tab again and the snippet will expand](aspnet-mvc-4-models-and-data-access/_static/image54.png "Press Tab again and the snippet will expand")</span></span>

<span data-ttu-id="8bfca-525">*Rozwinie ponownie naciśnij klawisz Tab i wstawki kodu*</span><span class="sxs-lookup"><span data-stu-id="8bfca-525">*Press Tab again and the snippet will expand*</span></span>

<span data-ttu-id="8bfca-526">***Aby dodać fragment kodu za pomocą myszy (C#, Visual Basic i XML)*** 1.</span><span class="sxs-lookup"><span data-stu-id="8bfca-526">***To add a code snippet using the mouse (C#, Visual Basic and XML)*** 1.</span></span> <span data-ttu-id="8bfca-527">Kliknij prawym przyciskiem myszy, gdy chcesz wstawić fragment kodu.</span><span class="sxs-lookup"><span data-stu-id="8bfca-527">Right-click where you want to insert the code snippet.</span></span>

1. <span data-ttu-id="8bfca-528">Wybierz **wstawić fragment** następuje **Moje wstawki kodu**.</span><span class="sxs-lookup"><span data-stu-id="8bfca-528">Select **Insert Snippet** followed by **My Code Snippets**.</span></span>
2. <span data-ttu-id="8bfca-529">Wybierz odpowiedni fragment z listy, klikając go.</span><span class="sxs-lookup"><span data-stu-id="8bfca-529">Pick the relevant snippet from the list, by clicking on it.</span></span>

<span data-ttu-id="8bfca-530">![Kliknij prawym przyciskiem myszy, gdy chcesz wstawić fragment kodu i wybierz wstawić fragment](aspnet-mvc-4-models-and-data-access/_static/image55.png "kliknij prawym przyciskiem myszy, gdy chcesz wstawić fragment kodu i wybierz wstawić fragment kodu")</span><span class="sxs-lookup"><span data-stu-id="8bfca-530">![Right-click where you want to insert the code snippet and select Insert Snippet](aspnet-mvc-4-models-and-data-access/_static/image55.png "Right-click where you want to insert the code snippet and select Insert Snippet")</span></span>

<span data-ttu-id="8bfca-531">*Kliknij prawym przyciskiem myszy, gdy chcesz wstawić fragment kodu i wybierz wstawić fragment kodu*</span><span class="sxs-lookup"><span data-stu-id="8bfca-531">*Right-click where you want to insert the code snippet and select Insert Snippet*</span></span>

<span data-ttu-id="8bfca-532">![Wybierz odpowiedni fragment z listy, klikając go](aspnet-mvc-4-models-and-data-access/_static/image56.png "wybierz odpowiedni fragment z listy, klikając go")</span><span class="sxs-lookup"><span data-stu-id="8bfca-532">![Pick the relevant snippet from the list, by clicking on it](aspnet-mvc-4-models-and-data-access/_static/image56.png "Pick the relevant snippet from the list, by clicking on it")</span></span>

<span data-ttu-id="8bfca-533">*Wybierz odpowiedni fragment z listy, klikając go*</span><span class="sxs-lookup"><span data-stu-id="8bfca-533">*Pick the relevant snippet from the list, by clicking on it*</span></span>
