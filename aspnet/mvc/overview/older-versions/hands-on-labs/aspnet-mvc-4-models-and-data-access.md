---
uid: mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-models-and-data-access
title: "Modele platformy ASP.NET MVC 4 oraz dostęp do danych | Dokumentacja firmy Microsoft"
author: rick-anderson
description: "Uwaga: W tym laboratorium Hands-on przyjęto założenie, że masz podstawową wiedzę na temat platformy ASP.NET MVC. Jeśli nie użyto programu ASP.NET MVC przed, zalecamy zapoznać się z platformy ASP.NET MVC 4..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2013
ms.topic: article
ms.assetid: 634ea84b-f904-4afe-b71b-49cccef4d9cc
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-models-and-data-access
msc.type: authoredcontent
ms.openlocfilehash: 076fa87eff140a3e7ff6855e4876abac40419c57
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/24/2018
---
<a name="aspnet-mvc-4-models-and-data-access"></a><span data-ttu-id="3ba90-104">Modele platformy ASP.NET MVC 4 oraz dostęp do danych</span><span class="sxs-lookup"><span data-stu-id="3ba90-104">ASP.NET MVC 4 Models and Data Access</span></span>
====================
<span data-ttu-id="3ba90-105">przez [obozów sieci Web Team](https://twitter.com/webcamps)</span><span class="sxs-lookup"><span data-stu-id="3ba90-105">by [Web Camps Team](https://twitter.com/webcamps)</span></span>

> [!NOTE]
> <span data-ttu-id="3ba90-106">W tym laboratorium Hands-on zakłada mieć podstawową wiedzę na temat **ASP.NET MVC**.</span><span class="sxs-lookup"><span data-stu-id="3ba90-106">This Hands-on Lab assumes you have basic knowledge of **ASP.NET MVC**.</span></span> <span data-ttu-id="3ba90-107">Jeśli nie używasz **ASP.NET MVC** przed, zalecamy zapoznać się z **podstawowe informacje na temat platformy ASP.NET MVC 4** Hands-on laboratorium.</span><span class="sxs-lookup"><span data-stu-id="3ba90-107">If you have not used **ASP.NET MVC** before, we recommend you to go over **ASP.NET MVC 4 Fundamentals** Hands-on Lab.</span></span>
> 
> <span data-ttu-id="3ba90-108">W tym laboratorium przeprowadzi Cię przez rozszerzenia oraz nowe funkcje opisane wcześniej przez zastosowanie drobne zmiany do przykładowej aplikacji sieci Web w folderze źródłowym.</span><span class="sxs-lookup"><span data-stu-id="3ba90-108">This lab walks you through the enhancements and new features previously described by applying minor changes to a sample Web application provided in the Source folder.</span></span>
> 
> <span data-ttu-id="3ba90-109">Wszystkie przykładowy kod i fragmenty kodu są uwzględnione w sieci Web obozów zestaw szkoleniowy, dostępne pod adresem [https://www.microsoft.com/download/29843](https://www.microsoft.com/download/29843).</span><span class="sxs-lookup"><span data-stu-id="3ba90-109">All sample code and snippets are included in the Web Camps Training Kit, available at [https://www.microsoft.com/download/29843](https://www.microsoft.com/download/29843).</span></span>


<span data-ttu-id="3ba90-110">W **podstawowe informacje na temat platformy ASP.NET MVC** laboratorium Hands-on można mieć zostały przekazywanie ustalony danych z kontrolerów do widoku szablonów.</span><span class="sxs-lookup"><span data-stu-id="3ba90-110">In **ASP.NET MVC Fundamentals** Hands-on Lab, you have been passing hard-coded data from the Controllers to the View templates.</span></span> <span data-ttu-id="3ba90-111">Jednak aby można było tworzyć rzeczywista aplikacji sieci Web, możesz chcieć użyć rzeczywistych bazy danych.</span><span class="sxs-lookup"><span data-stu-id="3ba90-111">But, in order to build a real Web application, you might want to use a real database.</span></span>

<span data-ttu-id="3ba90-112">W tym laboratorium Hands-on będzie pokazują, jak korzystać z aparatu bazy danych w celu przechowywania i pobierania danych potrzebnych do aplikacji Sklep utworów muzycznych.</span><span class="sxs-lookup"><span data-stu-id="3ba90-112">This Hands-on Lab will show you how to use a database engine in order to store and retrieve the data needed for the Music Store application.</span></span> <span data-ttu-id="3ba90-113">Do wykonania, które będzie rozpoczynać się od istniejącej bazy danych i tworzenia modelu Entity Data Model z niego.</span><span class="sxs-lookup"><span data-stu-id="3ba90-113">To accomplish that, you will start with an existing database and create the Entity Data Model from it.</span></span> <span data-ttu-id="3ba90-114">W tym laboratorium będzie spełniać **Database First** podejście, jak również **Code First** podejście.</span><span class="sxs-lookup"><span data-stu-id="3ba90-114">Throughout this lab, you will meet the **Database First** approach as well as the **Code First** approach.</span></span>

<span data-ttu-id="3ba90-115">Jednak umożliwia także **Model First** podejścia, utwórz ten sam model przy użyciu narzędzia, a następnie wygeneruj bazy danych z niego.</span><span class="sxs-lookup"><span data-stu-id="3ba90-115">However, you can also use the **Model First** approach, create the same model using the tools, and then generate the database from it.</span></span>

<span data-ttu-id="3ba90-116">![Pierwszy vs bazy danych. Model pierwszy](aspnet-mvc-4-models-and-data-access/_static/image1.png "bazy danych pierwszej wersji programu vs. Najpierw modelu")</span><span class="sxs-lookup"><span data-stu-id="3ba90-116">![Database First vs. Model First](aspnet-mvc-4-models-and-data-access/_static/image1.png "Database First vs. Model First")</span></span>

<span data-ttu-id="3ba90-117">*Pierwszy vs bazy danych. Najpierw modelu*</span><span class="sxs-lookup"><span data-stu-id="3ba90-117">*Database First vs. Model First*</span></span>

<span data-ttu-id="3ba90-118">Po wygenerowaniu modelu, można utworzyć odpowiednie korekty w StoreController dostarczanie widoków magazynu danych pobranych z bazy danych, zamiast ustalony danych.</span><span class="sxs-lookup"><span data-stu-id="3ba90-118">After generating the Model, you will make the proper adjustments in the StoreController to provide the Store Views with the data taken from the database, instead of using hard-coded data.</span></span> <span data-ttu-id="3ba90-119">Nie należy wprowadzać żadnych zmian widoku szablonów ponieważ StoreController będzie zwracać tego samego ViewModels szablony widoku, mimo że teraz dane będą pobierane z bazy danych.</span><span class="sxs-lookup"><span data-stu-id="3ba90-119">You will not need to make any change to the View templates because the StoreController will be returning the same ViewModels to the View templates, although this time the data will come from the database.</span></span>

<span data-ttu-id="3ba90-120">**Pierwszym sposobem kodu**</span><span class="sxs-lookup"><span data-stu-id="3ba90-120">**The Code First Approach**</span></span>

<span data-ttu-id="3ba90-121">Code First podejście pozwala zdefiniować modelu z kodu bez generowania klasy, które zwykle są powiązane z architekturą.</span><span class="sxs-lookup"><span data-stu-id="3ba90-121">The Code First approach allows us to define the model from the code without generating classes that are generally coupled with the framework.</span></span>

<span data-ttu-id="3ba90-122">W kodzie, najpierw obiekty modelu są zdefiniowane z POCOs, &quot;zwykły stare obiekty CLR&quot;.</span><span class="sxs-lookup"><span data-stu-id="3ba90-122">In code first, model objects are defined with POCOs, &quot;Plain Old CLR Objects&quot;.</span></span> <span data-ttu-id="3ba90-123">POCOs są proste klasy zwykły, nie dziedziczenia, które nie implementują interfejsy.</span><span class="sxs-lookup"><span data-stu-id="3ba90-123">POCOs are simple plain classes that have no inheritance and do not implement interfaces.</span></span> <span data-ttu-id="3ba90-124">Firma Microsoft może automatycznie generować bazy danych z ich lub możemy Użyj istniejącej bazy danych i Generowanie mapowania klasy z kodu.</span><span class="sxs-lookup"><span data-stu-id="3ba90-124">We can automatically generate the database from them, or we can use an existing database and generate the class mapping from the code.</span></span>

<span data-ttu-id="3ba90-125">Korzyści wynikające ze stosowania tego podejścia jest modelu pozostaje niezależnie od framework trwałości (w tym przypadku Entity Framework), zgodnie z klasy POCOs nie są połączone z framework mapowania.</span><span class="sxs-lookup"><span data-stu-id="3ba90-125">The benefits of using this approach is that the Model remains independent from the persistence framework (in this case, Entity Framework), as the POCOs classes are not coupled with the mapping framework.</span></span>

> [!NOTE]
> <span data-ttu-id="3ba90-126">To laboratorium jest oparte na programie ASP.NET MVC 4 i wersji magazynu utworów muzycznych przykładowej aplikacji dostosowanej zminimalizowany, aby dopasować tylko te funkcje, które są wyświetlane w tym laboratorium Hands-On.</span><span class="sxs-lookup"><span data-stu-id="3ba90-126">This Lab is based on ASP.NET MVC 4 and a version of the Music Store sample application customized and minimized to fit only the features shown in this Hands-On Lab.</span></span>
> 
> <span data-ttu-id="3ba90-127">Jeśli chcesz zapoznać się z całym **sklep muzyczny** samouczek aplikacji można znaleźć w [MVC utworów muzycznych magazynu](https://github.com/evilDave/MVC-Music-Store).</span><span class="sxs-lookup"><span data-stu-id="3ba90-127">If you wish to explore the whole **Music Store** tutorial application you can find it in [MVC-Music-Store](https://github.com/evilDave/MVC-Music-Store).</span></span>


<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a><span data-ttu-id="3ba90-128">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="3ba90-128">Prerequisites</span></span>

<span data-ttu-id="3ba90-129">Musi mieć następujące elementy do przygotowania tego laboratorium:</span><span class="sxs-lookup"><span data-stu-id="3ba90-129">You must have the following items to complete this lab:</span></span>

- <span data-ttu-id="3ba90-130">[Microsoft Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) lub wyższego poziomu (odczytu [dodatek a.](#AppendixA) instrukcje dotyczące sposobu jego instalacji).</span><span class="sxs-lookup"><span data-stu-id="3ba90-130">[Microsoft Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) or superior (read [Appendix A](#AppendixA) for instructions on how to install it).</span></span>

<a id="Setup"></a>

<a id="Setup"></a>
### <a name="setup"></a><span data-ttu-id="3ba90-131">Konfiguracja</span><span class="sxs-lookup"><span data-stu-id="3ba90-131">Setup</span></span>

<span data-ttu-id="3ba90-132">**Instalowanie wstawki kodu**</span><span class="sxs-lookup"><span data-stu-id="3ba90-132">**Installing Code Snippets**</span></span>

<span data-ttu-id="3ba90-133">Dla wygody taki kod, który będzie zarządzany wzdłuż tego laboratorium jest dostępna jako wstawki kodu programu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="3ba90-133">For convenience, much of the code you will be managing along this lab is available as Visual Studio code snippets.</span></span> <span data-ttu-id="3ba90-134">Aby zainstalować fragmenty kodu, uruchom **.\Source\Setup\CodeSnippets.vsi** pliku.</span><span class="sxs-lookup"><span data-stu-id="3ba90-134">To install the code snippets run **.\Source\Setup\CodeSnippets.vsi** file.</span></span>

<span data-ttu-id="3ba90-135">Jeśli nie masz doświadczenia z wstawki programu Visual Studio i chcesz dowiedzieć się, jak ich używać, można odwołać się do dodatku z tego dokumentu &quot; [wstawki kodu za pomocą programu dodatek C:](#AppendixC)&quot;.</span><span class="sxs-lookup"><span data-stu-id="3ba90-135">If you are not familiar with the Visual Studio Code Snippets, and want to learn how to use them, you can refer to the appendix from this document &quot;[Appendix C: Using Code Snippets](#AppendixC)&quot;.</span></span>

* * *

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a><span data-ttu-id="3ba90-136">Ćwiczenia</span><span class="sxs-lookup"><span data-stu-id="3ba90-136">Exercises</span></span>

<span data-ttu-id="3ba90-137">W tym laboratorium Hands-on składa się przez następujących czynnościach:</span><span class="sxs-lookup"><span data-stu-id="3ba90-137">This Hands-on Lab is comprised by the following exercises:</span></span>

1. [<span data-ttu-id="3ba90-138">Ćwiczenie 1: Dodawanie bazy danych</span><span class="sxs-lookup"><span data-stu-id="3ba90-138">Exercise 1: Adding a Database</span></span>](#Exercise1)
2. [<span data-ttu-id="3ba90-139">Ćwiczenie 2: Utworzenie bazy danych za pomocą funkcji Code First</span><span class="sxs-lookup"><span data-stu-id="3ba90-139">Exercise 2: Creating a Database using Code First</span></span>](#Exercise2)
3. [<span data-ttu-id="3ba90-140">Ćwiczenie 3: Zapytanie bazy danych z parametrami</span><span class="sxs-lookup"><span data-stu-id="3ba90-140">Exercise 3: Querying the Database with Parameters</span></span>](#Exercise3)

> [!NOTE]
> <span data-ttu-id="3ba90-141">Towarzyszy każdego wykonywania **zakończenia** folderu zawierającego wynikowy rozwiązanie, należy uzyskać po wykonaniu ćwiczeniach.</span><span class="sxs-lookup"><span data-stu-id="3ba90-141">Each exercise is accompanied by an **End** folder containing the resulting solution you should obtain after completing the exercises.</span></span> <span data-ttu-id="3ba90-142">Jeśli potrzebujesz dodatkowej pomocy pracuje nad ćwiczeniami, można użyć tego rozwiązania jako przewodnika.</span><span class="sxs-lookup"><span data-stu-id="3ba90-142">You can use this solution as a guide if you need additional help working through the exercises.</span></span>


<span data-ttu-id="3ba90-143">Szacowany czas trwania tego laboratorium: **35 minut**.</span><span class="sxs-lookup"><span data-stu-id="3ba90-143">Estimated time to complete this lab: **35 minutes**.</span></span>

<a id="Exercise1"></a>

<a id="Exercise_1_Adding_a_Database"></a>
### <a name="exercise-1-adding-a-database"></a><span data-ttu-id="3ba90-144">Ćwiczenie 1: Dodawanie bazy danych</span><span class="sxs-lookup"><span data-stu-id="3ba90-144">Exercise 1: Adding a Database</span></span>

<span data-ttu-id="3ba90-145">W tym ćwiczeniu dowiesz się, jak dodać bazy danych z tabel aplikacji MusicStore do rozwiązania, aby można było korzystać z jego dane.</span><span class="sxs-lookup"><span data-stu-id="3ba90-145">In this exercise, you will learn how to add a database with the tables of the MusicStore application to the solution in order to consume its data.</span></span> <span data-ttu-id="3ba90-146">Po bazy danych jest generowany z modelem i dodany do rozwiązania, należy zmodyfikować klasy StoreController dostarczanie szablon widoku danych pobranych z bazy danych, zamiast zakodowanych wartości.</span><span class="sxs-lookup"><span data-stu-id="3ba90-146">Once the database is generated with the model, and added to the solution, you will modify the StoreController class to provide the View template with the data taken from the database, instead of using hard-coded values.</span></span>

<a id="Ex1Task1"></a>

<a id="Task_1_-_Adding_a_Database"></a>
#### <a name="task-1---adding-a-database"></a><span data-ttu-id="3ba90-147">Zadanie 1 — Dodawanie bazy danych</span><span class="sxs-lookup"><span data-stu-id="3ba90-147">Task 1 - Adding a Database</span></span>

<span data-ttu-id="3ba90-148">To zadanie spowoduje dodanie już utworzono bazę danych z głównych tabel aplikacji MusicStore do rozwiązania.</span><span class="sxs-lookup"><span data-stu-id="3ba90-148">In this task, you will add an already created database with the main tables of the MusicStore application to the solution.</span></span>

1. <span data-ttu-id="3ba90-149">Otwórz **rozpocząć** rozwiązania, znajdujących się na **źródło/Ex1-AddingADatabaseDBFirst/Begin/** folderu.</span><span class="sxs-lookup"><span data-stu-id="3ba90-149">Open the **Begin** solution located at **Source/Ex1-AddingADatabaseDBFirst/Begin/** folder.</span></span>

    1. <span data-ttu-id="3ba90-150">Należy pobrać niektórych brakujących pakietów NuGet aby kontynuować.</span><span class="sxs-lookup"><span data-stu-id="3ba90-150">You will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="3ba90-151">Aby to zrobić, kliknij przycisk **projektu** menu i wybierz **Zarządzaj pakietami NuGet**.</span><span class="sxs-lookup"><span data-stu-id="3ba90-151">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
    2. <span data-ttu-id="3ba90-152">W **Zarządzaj pakietami NuGet** okna dialogowego, kliknij przycisk **przywrócić** celu pobieranie brakujących pakietów.</span><span class="sxs-lookup"><span data-stu-id="3ba90-152">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
    3. <span data-ttu-id="3ba90-153">Na koniec Skompiluj rozwiązanie, klikając **kompilacji** | **Kompiluj rozwiązanie**.</span><span class="sxs-lookup"><span data-stu-id="3ba90-153">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="3ba90-154">Jedną z zalet przy użyciu narzędzia NuGet jest, że nie masz do wysłania wszystkich bibliotek w projekcie, zmniejszenie jego rozmiar projektu.</span><span class="sxs-lookup"><span data-stu-id="3ba90-154">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="3ba90-155">Narzędzia Power NuGet określając wersje pakietów w pliku Packages.config, będzie można pobrać wymaganych bibliotek podczas pierwszego uruchomienia projektu.</span><span class="sxs-lookup"><span data-stu-id="3ba90-155">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="3ba90-156">Jest to, dlaczego konieczne będzie wykonanie tych kroków, po otwarciu istniejącego rozwiązania z tego laboratorium.</span><span class="sxs-lookup"><span data-stu-id="3ba90-156">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="3ba90-157">Dodaj **MvcMusicStore** pliku bazy danych.</span><span class="sxs-lookup"><span data-stu-id="3ba90-157">Add **MvcMusicStore** database file.</span></span> <span data-ttu-id="3ba90-158">W tym laboratorium Hands-on użyje utworzone bazę danych o nazwie **MvcMusicStore.mdf**.</span><span class="sxs-lookup"><span data-stu-id="3ba90-158">In this Hands-on Lab, you will use an already created database called **MvcMusicStore.mdf**.</span></span> <span data-ttu-id="3ba90-159">W tym celu kliknij prawym przyciskiem myszy **aplikacji\_danych** folderu, wskaż **Dodaj** , a następnie kliknij przycisk **istniejący element**.</span><span class="sxs-lookup"><span data-stu-id="3ba90-159">To do that, right-click **App\_Data** folder, point to **Add** and then click **Existing Item**.</span></span> <span data-ttu-id="3ba90-160">Przejdź do **\Source\Assets** i wybierz **MvcMusicStore.mdf** pliku.</span><span class="sxs-lookup"><span data-stu-id="3ba90-160">Browse to **\Source\Assets** and select the **MvcMusicStore.mdf** file.</span></span>

    <span data-ttu-id="3ba90-161">![Dodawanie istniejącego elementu](aspnet-mvc-4-models-and-data-access/_static/image2.png "Dodawanie istniejącego elementu")</span><span class="sxs-lookup"><span data-stu-id="3ba90-161">![Adding an Existing Item](aspnet-mvc-4-models-and-data-access/_static/image2.png "Adding an Existing Item")</span></span>

    <span data-ttu-id="3ba90-162">*Dodawanie istniejącego elementu*</span><span class="sxs-lookup"><span data-stu-id="3ba90-162">*Adding an Existing Item*</span></span>

    <span data-ttu-id="3ba90-163">![Plik bazy danych MvcMusicStore.mdf](aspnet-mvc-4-models-and-data-access/_static/image3.png "MvcMusicStore.mdf pliku bazy danych")</span><span class="sxs-lookup"><span data-stu-id="3ba90-163">![MvcMusicStore.mdf database file](aspnet-mvc-4-models-and-data-access/_static/image3.png "MvcMusicStore.mdf database file")</span></span>

    <span data-ttu-id="3ba90-164">*MvcMusicStore.mdf pliku bazy danych*</span><span class="sxs-lookup"><span data-stu-id="3ba90-164">*MvcMusicStore.mdf database file*</span></span>

    <span data-ttu-id="3ba90-165">Bazy danych dodano do projektu.</span><span class="sxs-lookup"><span data-stu-id="3ba90-165">The database has been added to the project.</span></span> <span data-ttu-id="3ba90-166">Nawet wtedy, gdy baza danych znajduje się wewnątrz rozwiązania, możesz znaleźć, a następnie go zaktualizować, ponieważ została ona hostowana na serwerze z inną bazą danych.</span><span class="sxs-lookup"><span data-stu-id="3ba90-166">Even when the database is located inside the solution, you can query and update it as it was hosted in a different database server.</span></span>

    <span data-ttu-id="3ba90-167">![Baza danych MvcMusicStore w Eksploratorze rozwiązań](aspnet-mvc-4-models-and-data-access/_static/image4.png "MvcMusicStore bazy danych, w Eksploratorze rozwiązań")</span><span class="sxs-lookup"><span data-stu-id="3ba90-167">![MvcMusicStore database in Solution Explorer](aspnet-mvc-4-models-and-data-access/_static/image4.png "MvcMusicStore database in Solution Explorer")</span></span>

    <span data-ttu-id="3ba90-168">*Baza danych MvcMusicStore w Eksploratorze rozwiązań*</span><span class="sxs-lookup"><span data-stu-id="3ba90-168">*MvcMusicStore database in Solution Explorer*</span></span>
3. <span data-ttu-id="3ba90-169">Sprawdź połączenie z bazą danych.</span><span class="sxs-lookup"><span data-stu-id="3ba90-169">Verify the connection to the database.</span></span> <span data-ttu-id="3ba90-170">Aby to zrobić, kliknij dwukrotnie **MvcMusicStore.mdf** ustanowić połączenie.</span><span class="sxs-lookup"><span data-stu-id="3ba90-170">To do this, double-click **MvcMusicStore.mdf** to establish a connection.</span></span>

    <span data-ttu-id="3ba90-171">![Łączenie z MvcMusicStore.mdf](aspnet-mvc-4-models-and-data-access/_static/image5.png "nawiązywania MvcMusicStore.mdf")</span><span class="sxs-lookup"><span data-stu-id="3ba90-171">![Connecting to MvcMusicStore.mdf](aspnet-mvc-4-models-and-data-access/_static/image5.png "Connecting to MvcMusicStore.mdf")</span></span>

    <span data-ttu-id="3ba90-172">*Łączenie z MvcMusicStore.mdf*</span><span class="sxs-lookup"><span data-stu-id="3ba90-172">*Connecting to MvcMusicStore.mdf*</span></span>

<a id="Ex1Task2"></a>

<a id="Task_2_-_Creating_a_Data_Model"></a>
#### <a name="task-2---creating-a-data-model"></a><span data-ttu-id="3ba90-173">Zadanie 2 — Tworzenie modelu danych</span><span class="sxs-lookup"><span data-stu-id="3ba90-173">Task 2 - Creating a Data Model</span></span>

<span data-ttu-id="3ba90-174">W ramach tego zadania spowoduje utworzenie modelu danych do interakcji z dodanym w poprzednim zadaniu bazy danych.</span><span class="sxs-lookup"><span data-stu-id="3ba90-174">In this task, you will create a data model to interact with the database added in the previous task.</span></span>

1. <span data-ttu-id="3ba90-175">Tworzenie modelu danych, która będzie reprezentowała bazy danych.</span><span class="sxs-lookup"><span data-stu-id="3ba90-175">Create a data model that will represent the database.</span></span> <span data-ttu-id="3ba90-176">Aby to zrobić, w Eksploratorze rozwiązań kliknij prawym przyciskiem myszy **modele** folderu, wskaż **Dodaj** , a następnie kliknij przycisk **nowy element**.</span><span class="sxs-lookup"><span data-stu-id="3ba90-176">To do this, in Solution Explorer right-click the **Models** folder, point to **Add** and then click **New Item**.</span></span> <span data-ttu-id="3ba90-177">W **Dodaj nowy element** okno dialogowe, wybierz opcję **danych** szablonu, a następnie **modelu danych jednostki ADO.NET** elementu.</span><span class="sxs-lookup"><span data-stu-id="3ba90-177">In the **Add New Item** dialog, select the **Data** template and then the **ADO.NET Entity Data Model** item.</span></span> <span data-ttu-id="3ba90-178">Zmień nazwę modelu danych, aby **StoreDB.edmx** i kliknij przycisk **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="3ba90-178">Change the data model name to **StoreDB.edmx** and click **Add**.</span></span>

    <span data-ttu-id="3ba90-179">![Dodawanie modelu danych jednostki ADO.NET StoreDB](aspnet-mvc-4-models-and-data-access/_static/image6.png "Dodawanie modelu danych jednostki ADO.NET StoreDB")</span><span class="sxs-lookup"><span data-stu-id="3ba90-179">![Adding the StoreDB ADO.NET Entity Data Model](aspnet-mvc-4-models-and-data-access/_static/image6.png "Adding the StoreDB ADO.NET Entity Data Model")</span></span>

    <span data-ttu-id="3ba90-180">*Dodawanie modelu danych jednostki ADO.NET StoreDB*</span><span class="sxs-lookup"><span data-stu-id="3ba90-180">*Adding the StoreDB ADO.NET Entity Data Model*</span></span>
2. <span data-ttu-id="3ba90-181">**Kreatora modelu danych jednostki** będą wyświetlane.</span><span class="sxs-lookup"><span data-stu-id="3ba90-181">The **Entity Data Model Wizard** will appear.</span></span> <span data-ttu-id="3ba90-182">Ten kreator przeprowadzi Cię przez tworzenie warstwy modelu.</span><span class="sxs-lookup"><span data-stu-id="3ba90-182">This wizard will guide you through the creation of the model layer.</span></span> <span data-ttu-id="3ba90-183">Ponieważ modelu należy tworzyć w oparciu o istniejących recentyl bazy danych dodane, wybierz opcję **generowania z bazy danych** i kliknij przycisk **dalej**.</span><span class="sxs-lookup"><span data-stu-id="3ba90-183">Since the model should be created based on the existing database recentyl added, select **Generate from database** and click **Next**.</span></span>

    <span data-ttu-id="3ba90-184">![Wybieranie zawartość modelu](aspnet-mvc-4-models-and-data-access/_static/image7.png "Wybieranie zawartość modelu")</span><span class="sxs-lookup"><span data-stu-id="3ba90-184">![Choosing the model content](aspnet-mvc-4-models-and-data-access/_static/image7.png "Choosing the model content")</span></span>

    <span data-ttu-id="3ba90-185">*Wybieranie modelu zawartości*</span><span class="sxs-lookup"><span data-stu-id="3ba90-185">*Choosing the model content*</span></span>
3. <span data-ttu-id="3ba90-186">Ponieważ są generowania modelu z bazy danych, należy określić połączenie.</span><span class="sxs-lookup"><span data-stu-id="3ba90-186">Since you are generating a model from a database, you will need to specify the connection to use.</span></span> <span data-ttu-id="3ba90-187">Kliknij przycisk **nowe połączenie**.</span><span class="sxs-lookup"><span data-stu-id="3ba90-187">Click **New Connection**.</span></span>
4. <span data-ttu-id="3ba90-188">Wybierz **plik bazy danych programu Microsoft SQL Server** i kliknij przycisk **Kontynuuj**.</span><span class="sxs-lookup"><span data-stu-id="3ba90-188">Select **Microsoft SQL Server Database File** and click **Continue**.</span></span>

    <span data-ttu-id="3ba90-189">![Wybierz źródło danych](aspnet-mvc-4-models-and-data-access/_static/image8.png "wybierz źródło danych")</span><span class="sxs-lookup"><span data-stu-id="3ba90-189">![Choose data source](aspnet-mvc-4-models-and-data-access/_static/image8.png "Choose data source")</span></span>

    <span data-ttu-id="3ba90-190">*Wybierz źródło danych w oknie dialogowym*</span><span class="sxs-lookup"><span data-stu-id="3ba90-190">*Choose data source dialog*</span></span>
5. <span data-ttu-id="3ba90-191">Kliknij przycisk **Przeglądaj** i wybierz bazę danych **MvcMusicStore.mdf** zlokalizowanego w **aplikacji\_danych** folder i kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="3ba90-191">Click **Browse** and select the database **MvcMusicStore.mdf** you located in the **App\_Data** folder and click **OK**.</span></span>

    <span data-ttu-id="3ba90-192">![Właściwości połączenia](aspnet-mvc-4-models-and-data-access/_static/image9.png "właściwości połączenia")</span><span class="sxs-lookup"><span data-stu-id="3ba90-192">![Connection properties](aspnet-mvc-4-models-and-data-access/_static/image9.png "Connection properties")</span></span>

    <span data-ttu-id="3ba90-193">*Właściwości połączenia*</span><span class="sxs-lookup"><span data-stu-id="3ba90-193">*Connection properties*</span></span>
6. <span data-ttu-id="3ba90-194">Wygenerowana klasa powinna mieć taką samą nazwę jak parametry połączenia jednostki, zmienia jego nazwę, aby **MusicStoreEntities** i kliknij przycisk **dalej**.</span><span class="sxs-lookup"><span data-stu-id="3ba90-194">The generated class should have the same name as the entity connection string, so change its name to **MusicStoreEntities** and click **Next**.</span></span>

    <span data-ttu-id="3ba90-195">![Wybieranie połączenia danych](aspnet-mvc-4-models-and-data-access/_static/image10.png "Wybieranie połączenia danych")</span><span class="sxs-lookup"><span data-stu-id="3ba90-195">![Choosing the data connection](aspnet-mvc-4-models-and-data-access/_static/image10.png "Choosing the data connection")</span></span>

    <span data-ttu-id="3ba90-196">*Wybieranie połączenia danych*</span><span class="sxs-lookup"><span data-stu-id="3ba90-196">*Choosing the data connection*</span></span>
7. <span data-ttu-id="3ba90-197">Wybierz obiekty bazy danych do użycia.</span><span class="sxs-lookup"><span data-stu-id="3ba90-197">Choose the database objects to use.</span></span> <span data-ttu-id="3ba90-198">Modelu jednostki będzie używać tylko tabele bazy danych, wybierz **tabel** opcję i upewnij się, że **Dołącz kolumny klucza obcego do modelu** i **Pluralize lub nadaj wygenerowany nazwy obiektów** również są zaznaczone opcje.</span><span class="sxs-lookup"><span data-stu-id="3ba90-198">As the Entity Model will use just the database's tables, select the **Tables** option, and make sure that the **Include foreign key columns in the model** and **Pluralize or singularize generated object names** options are also selected.</span></span> <span data-ttu-id="3ba90-199">Zmień Namespace modelu do **MvcMusicStore.Model** i kliknij przycisk **Zakończ**.</span><span class="sxs-lookup"><span data-stu-id="3ba90-199">Change the Model Namespace to **MvcMusicStore.Model** and click **Finish**.</span></span>

    <span data-ttu-id="3ba90-200">![Wybieranie obiektów bazy danych](aspnet-mvc-4-models-and-data-access/_static/image11.png "Wybieranie obiektów bazy danych")</span><span class="sxs-lookup"><span data-stu-id="3ba90-200">![Choosing the database objects](aspnet-mvc-4-models-and-data-access/_static/image11.png "Choosing the database objects")</span></span>

    <span data-ttu-id="3ba90-201">*Wybieranie obiektów bazy danych*</span><span class="sxs-lookup"><span data-stu-id="3ba90-201">*Choosing the database objects*</span></span>

    > [!NOTE]
    > <span data-ttu-id="3ba90-202">Okno dialogowe ostrzeżenia zabezpieczeń jest wyświetlany, kliknij przycisk **OK** uruchomić szablon i Generuj klasy dla jednostek w modelu.</span><span class="sxs-lookup"><span data-stu-id="3ba90-202">If a Security Warning dialog is shown, click **OK** to run the template and generate the classes for the model entities.</span></span>
8. <span data-ttu-id="3ba90-203">Diagram jednostek dla bazy danych będą wyświetlane, gdy zostanie utworzony oddzielny klasy, która mapuje każdej tabeli w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="3ba90-203">An entity diagram for the database will appear, while a separate class that maps each table to the database will be created.</span></span> <span data-ttu-id="3ba90-204">Na przykład **albumów** tabeli będą reprezentowane przez **albumu** klasy, w której każda kolumna w tabeli przypisze do właściwości klasy.</span><span class="sxs-lookup"><span data-stu-id="3ba90-204">For example, the **Albums** table will be represented by an **Album** class, where each column in the table will map to a class property.</span></span> <span data-ttu-id="3ba90-205">Pozwoli to zapytanie i pracować z obiektami, które reprezentują wierszy w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="3ba90-205">This will allow you to query and work with objects that represent rows in the database.</span></span>

    <span data-ttu-id="3ba90-206">![Diagram jednostek](aspnet-mvc-4-models-and-data-access/_static/image12.png "diagram jednostek")</span><span class="sxs-lookup"><span data-stu-id="3ba90-206">![Entity diagram](aspnet-mvc-4-models-and-data-access/_static/image12.png "Entity diagram")</span></span>

    <span data-ttu-id="3ba90-207">*Diagram jednostek*</span><span class="sxs-lookup"><span data-stu-id="3ba90-207">*Entity diagram*</span></span>

> [!NOTE]
> <span data-ttu-id="3ba90-208">Szablony T4 (.TT —) uruchom kod można wygenerować klas jednostek i spowoduje zastąpienie istniejących klas o takiej samej nazwie.</span><span class="sxs-lookup"><span data-stu-id="3ba90-208">The T4 templates (.tt) run code to generate the entities classes and will overwrite the existing classes with the same name.</span></span> <span data-ttu-id="3ba90-209">W tym przykładzie klasy &quot;albumu&quot;, &quot;Genre&quot; i &quot;wykonawcy&quot; zostały zastąpione wygenerowanego kodu.</span><span class="sxs-lookup"><span data-stu-id="3ba90-209">In this example, the classes &quot;Album&quot;, &quot;Genre&quot; and &quot;Artist&quot; were overwritten with the generated code.</span></span>


<a id="Ex1Task3"></a>

<a id="Task_3_-_Building_the_Application"></a>
#### <a name="task-3---building-the-application"></a><span data-ttu-id="3ba90-210">Zadanie 3 — Tworzenie aplikacji</span><span class="sxs-lookup"><span data-stu-id="3ba90-210">Task 3 - Building the Application</span></span>

<span data-ttu-id="3ba90-211">W tym zadaniu można będzie sprawdzać, mimo że generowania modelu zostały usunięte **albumu**, **Genre** i **wykonawcy** klasy modelu projektu tworzy pomyślnie przy użyciu nowe klasy modelu danych.</span><span class="sxs-lookup"><span data-stu-id="3ba90-211">In this task, you will check that, although the model generation have removed the **Album**, **Genre** and **Artist** model classes, the project builds successfully by using the new data model classes.</span></span>

1. <span data-ttu-id="3ba90-212">Skompiluj projekt, wybierając **kompilacji** element menu, a następnie **kompilacji MvcMusicStore**.</span><span class="sxs-lookup"><span data-stu-id="3ba90-212">Build the project by selecting the **Build** menu item and then **Build MvcMusicStore**.</span></span>

    <span data-ttu-id="3ba90-213">![Tworzenie projektu](aspnet-mvc-4-models-and-data-access/_static/image13.png "skompilowanie projektu")</span><span class="sxs-lookup"><span data-stu-id="3ba90-213">![Building the project](aspnet-mvc-4-models-and-data-access/_static/image13.png "Building the project")</span></span>

    <span data-ttu-id="3ba90-214">*Tworzenie projektu*</span><span class="sxs-lookup"><span data-stu-id="3ba90-214">*Building the project*</span></span>
2. <span data-ttu-id="3ba90-215">Projekt tworzy się pomyślnie.</span><span class="sxs-lookup"><span data-stu-id="3ba90-215">The project builds successfully.</span></span> <span data-ttu-id="3ba90-216">Dlaczego nadal działa?</span><span class="sxs-lookup"><span data-stu-id="3ba90-216">Why does it still work?</span></span> <span data-ttu-id="3ba90-217">To działa, ponieważ tabele bazy danych ma pola, które zawierają właściwości, które były używane w klasach usuniętych **albumu** i **Genre**.</span><span class="sxs-lookup"><span data-stu-id="3ba90-217">It works because the database tables have fields that include the properties that you were using in the removed classes **Album** and **Genre**.</span></span>

    <span data-ttu-id="3ba90-218">![Kompilacje zakończyło się pomyślnie](aspnet-mvc-4-models-and-data-access/_static/image14.png "kompilacji zakończyło się pomyślnie")</span><span class="sxs-lookup"><span data-stu-id="3ba90-218">![Builds succeeded](aspnet-mvc-4-models-and-data-access/_static/image14.png "Builds succeeded")</span></span>

    <span data-ttu-id="3ba90-219">*Kompilacje powiodło się.*</span><span class="sxs-lookup"><span data-stu-id="3ba90-219">*Builds succeeded*</span></span>
3. <span data-ttu-id="3ba90-220">Gdy Projektant wyświetla jednostek w formacie diagramu, są one naprawdę klas C#.</span><span class="sxs-lookup"><span data-stu-id="3ba90-220">While the designer displays the entities in a diagram format, they are really C# classes.</span></span> <span data-ttu-id="3ba90-221">Rozwiń węzeł **StoreDB.edmx** węzła w Eksploratorze rozwiązań, a następnie **StoreDB.tt**, pojawi się nowe jednostki wygenerowany.</span><span class="sxs-lookup"><span data-stu-id="3ba90-221">Expand the **StoreDB.edmx** node in the Solution Explorer and then **StoreDB.tt**, you will see the new generated entities.</span></span>

    <span data-ttu-id="3ba90-222">![Pliki generowane](aspnet-mvc-4-models-and-data-access/_static/image15.png "wygenerowanych plików")</span><span class="sxs-lookup"><span data-stu-id="3ba90-222">![Generated files](aspnet-mvc-4-models-and-data-access/_static/image15.png "Generated files")</span></span>

    <span data-ttu-id="3ba90-223">*Wygenerowane pliki*</span><span class="sxs-lookup"><span data-stu-id="3ba90-223">*Generated files*</span></span>

<a id="Ex1Task4"></a>

<a id="Task_4_-_Querying_the_Database"></a>
#### <a name="task-4---querying-the-database"></a><span data-ttu-id="3ba90-224">Zadanie 4 — kwerendy bazy danych</span><span class="sxs-lookup"><span data-stu-id="3ba90-224">Task 4 - Querying the Database</span></span>

<span data-ttu-id="3ba90-225">To zadanie spowoduje zaktualizowanie klasy StoreController tak, aby zamiast dane zapisane na stałe będzie zapytania bazy danych do pobrania informacji.</span><span class="sxs-lookup"><span data-stu-id="3ba90-225">In this task, you will update the StoreController class so that, instead of using hardcoded data, it will query the database to retrieve the information.</span></span>

1. <span data-ttu-id="3ba90-226">Otwórz **Controllers\StoreController.cs** i dodaj następujące pole do klasy, aby pomieścić wystąpienia **MusicStoreEntities** klasę o nazwie **storeDB**:</span><span class="sxs-lookup"><span data-stu-id="3ba90-226">Open **Controllers\StoreController.cs** and add the following field to the class to hold an instance of the **MusicStoreEntities** class, named **storeDB**:</span></span>

    <span data-ttu-id="3ba90-227">(Fragment - kodu *modeli i dostęp do danych - Ex1 storeDB*)</span><span class="sxs-lookup"><span data-stu-id="3ba90-227">(Code Snippet - *Models And Data Access - Ex1 storeDB*)</span></span>


    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample1.cs)]
2. <span data-ttu-id="3ba90-228">**MusicStoreEntities** klasy udostępnia właściwości kolekcji dla każdej tabeli w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="3ba90-228">The **MusicStoreEntities** class exposes a collection property for each table in the database.</span></span> <span data-ttu-id="3ba90-229">Aktualizacja **Przeglądaj** metody akcji można pobrać określonego rodzaju ze wszystkimi **albumów**.</span><span class="sxs-lookup"><span data-stu-id="3ba90-229">Update **Browse** action method to retrieve a Genre with all of the **Albums**.</span></span>

    <span data-ttu-id="3ba90-230">(Fragment - kodu *modeli i dostęp do danych - przeglądania magazynu Ex1*)</span><span class="sxs-lookup"><span data-stu-id="3ba90-230">(Code Snippet - *Models And Data Access - Ex1 Store Browse*)</span></span>


    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample2.cs)]

    > [!NOTE]
    > <span data-ttu-id="3ba90-231">W przypadku korzystania z możliwości .NET o nazwie **LINQ** (zapytanie o języku zintegrowanym) do zapisania wyrażeń jednoznacznie zapytania względem tych kolekcje — co spowoduje wykonanie kodu w bazie danych i zwracać obiekty można umieszczonych przed.</span><span class="sxs-lookup"><span data-stu-id="3ba90-231">You are using a capability of .NET called **LINQ** (language-integrated query) to write strongly-typed query expressions against these collections - which will execute code against the database and return objects that you can program against.</span></span>
    > 
    > <span data-ttu-id="3ba90-232">Aby uzyskać więcej informacji na temat LINQ, odwiedź stronę [witrynę msdn](https://msdn.microsoft.com/library/bb397926&amp;#040;v=vs.110&amp;#041;.aspx).</span><span class="sxs-lookup"><span data-stu-id="3ba90-232">For more information about LINQ, please visit the [msdn site](https://msdn.microsoft.com/library/bb397926&amp;#040;v=vs.110&amp;#041;.aspx).</span></span>
3. <span data-ttu-id="3ba90-233">Aktualizacja **indeksu** metody akcji, aby pobrać wszystkie genres.</span><span class="sxs-lookup"><span data-stu-id="3ba90-233">Update **Index** action method to retrieve all the genres.</span></span>

    <span data-ttu-id="3ba90-234">(Fragment - kodu *modeli i Data Access — indeks magazynu Ex1*)</span><span class="sxs-lookup"><span data-stu-id="3ba90-234">(Code Snippet - *Models And Data Access - Ex1 Store Index*)</span></span>


    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample3.cs)]
4. <span data-ttu-id="3ba90-235">Aktualizacja **indeksu** metody akcji, aby pobrać wszystkie genres i przekształcenie kolekcji na listę.</span><span class="sxs-lookup"><span data-stu-id="3ba90-235">Update **Index** action method to retrieve all the genres and transform the collection to a list.</span></span>

    <span data-ttu-id="3ba90-236">(Fragment - kodu *modeli i dostęp do danych - GenreMenu magazynu Ex1*)</span><span class="sxs-lookup"><span data-stu-id="3ba90-236">(Code Snippet - *Models And Data Access - Ex1 Store GenreMenu*)</span></span>


    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample4.cs)]

<a id="Ex1Task5"></a>

<a id="Task_5_-_Running_the_Application"></a>
#### <a name="task-5---running-the-application"></a><span data-ttu-id="3ba90-237">Zadanie 5 działania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="3ba90-237">Task 5 - Running the Application</span></span>

<span data-ttu-id="3ba90-238">To zadanie będzie sprawdzać, czy strona indeksu magazynu wyświetli Genres przechowywane w bazie danych, zamiast ustalony te.</span><span class="sxs-lookup"><span data-stu-id="3ba90-238">In this task, you will check that the Store Index page will now display the Genres stored in the database instead of the hardcoded ones.</span></span> <span data-ttu-id="3ba90-239">Nie istnieje potrzeba zmiany szablonu widoku, ponieważ **StoreController** zwraca tej samej jednostki jak poprzednio, ale tym razem dane będą pobierane z bazy danych.</span><span class="sxs-lookup"><span data-stu-id="3ba90-239">There is no need to change the View template because the **StoreController** is returning the same entities as before, although this time the data will come from the database.</span></span>

1. <span data-ttu-id="3ba90-240">Ponowne kompilowanie rozwiązania i naciśnij klawisz **F5** do uruchomienia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="3ba90-240">Rebuild the solution and press **F5** to run the Application.</span></span>
2. <span data-ttu-id="3ba90-241">Na stronie głównej datę rozpoczęcia projektu.</span><span class="sxs-lookup"><span data-stu-id="3ba90-241">The project starts in the Home page.</span></span> <span data-ttu-id="3ba90-242">Sprawdź, czy menu **Genres** nie jest już listy zapisane na stałe, a dane bezpośrednio są pobierane z bazy danych.</span><span class="sxs-lookup"><span data-stu-id="3ba90-242">Verify that the menu of **Genres** is no longer a hardcoded list, and the data is directly retrieved from the database.</span></span>

    ![BrowsingGenresFromDataBase](aspnet-mvc-4-models-and-data-access/_static/image16.png)

    <span data-ttu-id="3ba90-244">*Przeglądanie Genres z bazy danych*</span><span class="sxs-lookup"><span data-stu-id="3ba90-244">*Browsing Genres from the database*</span></span>
3. <span data-ttu-id="3ba90-245">Teraz przejdź do dowolnego rodzaju i sprawdź, czy albumów zostają wypełnione z bazy danych.</span><span class="sxs-lookup"><span data-stu-id="3ba90-245">Now browse to any genre and verify the albums are populated from database.</span></span>

    <span data-ttu-id="3ba90-246">![Przeglądanie albumów z bazy danych](aspnet-mvc-4-models-and-data-access/_static/image17.png "przeglądanie albumów z bazy danych")</span><span class="sxs-lookup"><span data-stu-id="3ba90-246">![Browsing Albums from the database](aspnet-mvc-4-models-and-data-access/_static/image17.png "Browsing Albums from the database")</span></span>

    <span data-ttu-id="3ba90-247">*Przeglądanie albumów z bazy danych*</span><span class="sxs-lookup"><span data-stu-id="3ba90-247">*Browsing Albums from the database*</span></span>

<a id="Exercise2"></a>

<a id="Exercise_2_Creating_a_Database_Using_Code_First"></a>
### <a name="exercise-2-creating-a-database-using-code-first"></a><span data-ttu-id="3ba90-248">Ćwiczenie 2: Utworzenie bazy danych najpierw przy użyciu kodu</span><span class="sxs-lookup"><span data-stu-id="3ba90-248">Exercise 2: Creating a Database Using Code First</span></span>

<span data-ttu-id="3ba90-249">W tym ćwiczeniu dowiesz się, jak użyć metody Code First, aby utworzyć bazę danych z tabel MusicStore aplikacji oraz jak dostępu do danych.</span><span class="sxs-lookup"><span data-stu-id="3ba90-249">In this exercise, you will learn how to use the Code First approach to create a database with the tables of the MusicStore application, and how to access its data.</span></span>

<span data-ttu-id="3ba90-250">Po wygenerowaniu modelu, należy zmodyfikować StoreController dostarczanie szablon widoku danych z bazy danych, zamiast wartości zapisane na stałe.</span><span class="sxs-lookup"><span data-stu-id="3ba90-250">Once the model is generated, you will modify the StoreController to provide the View template with the data taken from the database, instead of using hardcoded values.</span></span>

> [!NOTE]
> <span data-ttu-id="3ba90-251">Jeśli ukończono ćwiczenie 1 i pracowali już z bazy danych pierwszego podejścia, będzie teraz Dowiedz się jak uzyskać takie same wyniki z innego procesu.</span><span class="sxs-lookup"><span data-stu-id="3ba90-251">If you have completed Exercise 1 and have already worked with the Database First approach, you will now learn how to get the same results with a different process.</span></span> <span data-ttu-id="3ba90-252">Zadania, które są wspólne dla wykonywania 1 została oznaczona ułatwi Twojej odczytu.</span><span class="sxs-lookup"><span data-stu-id="3ba90-252">The tasks that are in common with Exercise 1 have been marked to make your reading easier.</span></span> <span data-ttu-id="3ba90-253">Jeśli nie została ukończona wykonywania 1, ale chcesz dowiedzieć się podejście Code First, możesz rozpocząć od tego ćwiczenia i uzyskać pełny zakres tego tematu.</span><span class="sxs-lookup"><span data-stu-id="3ba90-253">If you have not completed Exercise 1 but would like to learn the Code First approach, you can start from this exercise and get a full coverage of the topic.</span></span>


<a id="Ex2Task1"></a>

<a id="Task_1_-_Populating_Sample_Data"></a>
#### <a name="task-1---populating-sample-data"></a><span data-ttu-id="3ba90-254">Zadanie 1 - wypełnianie przykładowe dane</span><span class="sxs-lookup"><span data-stu-id="3ba90-254">Task 1 - Populating Sample Data</span></span>

<span data-ttu-id="3ba90-255">W tym zadaniu zostanie Wypełnianie bazy danych z przykładowymi danymi tworzonemu początkowemu przy użyciu pierwszej kodu.</span><span class="sxs-lookup"><span data-stu-id="3ba90-255">In this task, you will populate the database with sample data when it is intially created using Code-First.</span></span>

1. <span data-ttu-id="3ba90-256">Otwórz **rozpocząć** rozwiązania, znajdujących się na **źródło/Ex2-CreatingADatabaseCodeFirst/Begin/** folderu.</span><span class="sxs-lookup"><span data-stu-id="3ba90-256">Open the **Begin** solution located at **Source/Ex2-CreatingADatabaseCodeFirst/Begin/** folder.</span></span> <span data-ttu-id="3ba90-257">W przeciwnym razie możesz nadal korzystać **zakończenia** uzyskane rozwiązanie, wykonując poprzednim ćwiczeniu.</span><span class="sxs-lookup"><span data-stu-id="3ba90-257">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

    1. <span data-ttu-id="3ba90-258">Po otwarciu dostarczonych **rozpocząć** rozwiązania, musisz pobrać niektórych brakujących pakietów NuGet aby kontynuować.</span><span class="sxs-lookup"><span data-stu-id="3ba90-258">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="3ba90-259">Aby to zrobić, kliknij przycisk **projektu** menu i wybierz **Zarządzaj pakietami NuGet**.</span><span class="sxs-lookup"><span data-stu-id="3ba90-259">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
    2. <span data-ttu-id="3ba90-260">W **Zarządzaj pakietami NuGet** okna dialogowego, kliknij przycisk **przywrócić** celu pobieranie brakujących pakietów.</span><span class="sxs-lookup"><span data-stu-id="3ba90-260">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
    3. <span data-ttu-id="3ba90-261">Na koniec Skompiluj rozwiązanie, klikając **kompilacji** | **Kompiluj rozwiązanie**.</span><span class="sxs-lookup"><span data-stu-id="3ba90-261">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="3ba90-262">Jedną z zalet przy użyciu narzędzia NuGet jest, że nie masz do wysłania wszystkich bibliotek w projekcie, zmniejszenie jego rozmiar projektu.</span><span class="sxs-lookup"><span data-stu-id="3ba90-262">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="3ba90-263">Narzędzia Power NuGet określając wersje pakietów w pliku Packages.config, będzie można pobrać wymaganych bibliotek podczas pierwszego uruchomienia projektu.</span><span class="sxs-lookup"><span data-stu-id="3ba90-263">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="3ba90-264">Jest to, dlaczego konieczne będzie wykonanie tych kroków, po otwarciu istniejącego rozwiązania z tego laboratorium.</span><span class="sxs-lookup"><span data-stu-id="3ba90-264">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="3ba90-265">Dodaj **SampleData.cs** pliku **modele** folderu.</span><span class="sxs-lookup"><span data-stu-id="3ba90-265">Add the **SampleData.cs** file to the **Models** folder.</span></span> <span data-ttu-id="3ba90-266">W tym celu kliknij prawym przyciskiem myszy **modele** folderu, wskaż **Dodaj** , a następnie kliknij przycisk **istniejący element**.</span><span class="sxs-lookup"><span data-stu-id="3ba90-266">To do that, right-click **Models** folder, point to **Add** and then click **Existing Item**.</span></span> <span data-ttu-id="3ba90-267">Przejdź do **\Source\Assets** i wybierz **SampleData.cs** pliku.</span><span class="sxs-lookup"><span data-stu-id="3ba90-267">Browse to **\Source\Assets** and select the **SampleData.cs** file.</span></span>

    <span data-ttu-id="3ba90-268">![Przykładowe dane wypełnić kodu](aspnet-mvc-4-models-and-data-access/_static/image18.png "przykładowych danych wypełnić kodu")</span><span class="sxs-lookup"><span data-stu-id="3ba90-268">![Sample data populate code](aspnet-mvc-4-models-and-data-access/_static/image18.png "Sample data populate code")</span></span>

    <span data-ttu-id="3ba90-269">*Przykładowe dane wypełnić kodu*</span><span class="sxs-lookup"><span data-stu-id="3ba90-269">*Sample data populate code*</span></span>
3. <span data-ttu-id="3ba90-270">Otwórz **Global.asax.cs** pliku i dodaj następującą *przy użyciu* instrukcje.</span><span class="sxs-lookup"><span data-stu-id="3ba90-270">Open the **Global.asax.cs** file and add the following *using* statements.</span></span>

    <span data-ttu-id="3ba90-271">(Fragment - kodu *modeli i Data Access — deklaracje Using Ex2 globalne Asax*)</span><span class="sxs-lookup"><span data-stu-id="3ba90-271">(Code Snippet - *Models And Data Access - Ex2 Global Asax Usings*)</span></span>


    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample5.cs)]
4. <span data-ttu-id="3ba90-272">W **aplikacji\_Start()** metody Dodaj następujący wiersz do ustawienia inicjatora bazy danych.</span><span class="sxs-lookup"><span data-stu-id="3ba90-272">In the **Application\_Start()** method add the following line to set the database initializer.</span></span>

    <span data-ttu-id="3ba90-273">(Fragment - kodu *modeli i dostęp do danych - globalne Asax SetInitializer Ex2*)</span><span class="sxs-lookup"><span data-stu-id="3ba90-273">(Code Snippet - *Models And Data Access - Ex2 Global Asax SetInitializer*)</span></span>


    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample6.cs)]

<a id="Ex2Task2"></a>

<a id="Task_2_-_Configuring_the_connection_to_the_Database"></a>
#### <a name="task-2---configuring-the-connection-to-the-database"></a><span data-ttu-id="3ba90-274">Zadanie 2 — Konfigurowanie połączenia z bazą danych</span><span class="sxs-lookup"><span data-stu-id="3ba90-274">Task 2 - Configuring the connection to the Database</span></span>

<span data-ttu-id="3ba90-275">Teraz, gdy masz już dodany bazy danych do naszej projektu, będzie zapisywać **Web.config** pliku parametrów połączenia.</span><span class="sxs-lookup"><span data-stu-id="3ba90-275">Now that you have already added a database to our project, you will write in the **Web.config** file the connection string.</span></span>

1. <span data-ttu-id="3ba90-276">Dodaj parametry połączenia w **Web.config**. Aby to zrobić, otwórz **Web.config** w głównym projektu i Zastąp ciąg połączenia o nazwie parametru DefaultConnection z tego wiersza w  **&lt;connectionStrings&gt;**  sekcji:</span><span class="sxs-lookup"><span data-stu-id="3ba90-276">Add a connection string at **Web.config**. To do that, open **Web.config** at project root and replace the connection string named DefaultConnection with this line in the **&lt;connectionStrings&gt;** section:</span></span>

    <span data-ttu-id="3ba90-277">![Lokalizacja pliku Web.config](aspnet-mvc-4-models-and-data-access/_static/image19.png "lokalizację pliku Web.config")</span><span class="sxs-lookup"><span data-stu-id="3ba90-277">![Web.config file location](aspnet-mvc-4-models-and-data-access/_static/image19.png "Web.config file location")</span></span>

    <span data-ttu-id="3ba90-278">*Lokalizacja pliku Web.config*</span><span class="sxs-lookup"><span data-stu-id="3ba90-278">*Web.config file location*</span></span>


    [!code-xml[Main](aspnet-mvc-4-models-and-data-access/samples/sample7.xml)]

<a id="Ex2Task3"></a>

<a id="Task_3_-_Working_with_the_Model"></a>
#### <a name="task-3---working-with-the-model"></a><span data-ttu-id="3ba90-279">Zadanie 3 — Praca z modelu</span><span class="sxs-lookup"><span data-stu-id="3ba90-279">Task 3 - Working with the Model</span></span>

<span data-ttu-id="3ba90-280">Teraz, gdy skonfigurowano już połączenie z bazą danych, zostaną połączone modelu z tabel bazy danych.</span><span class="sxs-lookup"><span data-stu-id="3ba90-280">Now that you have already configured the connection to the database, you will link the model with the database tables.</span></span> <span data-ttu-id="3ba90-281">W ramach tego zadania spowoduje utworzenie klasy, który będzie połączony z bazą danych z Code First.</span><span class="sxs-lookup"><span data-stu-id="3ba90-281">In this task, you will create a class that will be linked to the database with Code First.</span></span> <span data-ttu-id="3ba90-282">Należy pamiętać, że istnieje istniejących klasy modelu POCO, które powinno zostać zmodyfikowane w.</span><span class="sxs-lookup"><span data-stu-id="3ba90-282">Remember that there is an existent POCO model class that should be modified.</span></span>

> [!NOTE]
> <span data-ttu-id="3ba90-283">Jeśli ukończono ćwiczenie 1 można zauważyć, że ten krok został wykonany przez kreatora.</span><span class="sxs-lookup"><span data-stu-id="3ba90-283">If you completed Exercise 1, you will note that this step was performed by a wizard.</span></span> <span data-ttu-id="3ba90-284">Wykonując Code First, ręcznie utworzysz klasy, które zostaną połączone obiekty danych.</span><span class="sxs-lookup"><span data-stu-id="3ba90-284">By doing Code First, you will manually create classes that will be linked to data entities.</span></span>


1. <span data-ttu-id="3ba90-285">Otwórz klasy modelu POCO **Genre** z **modele** folderu projektu i zawiera identyfikatora.</span><span class="sxs-lookup"><span data-stu-id="3ba90-285">Open the POCO model class **Genre** from **Models** project folder and include an ID.</span></span> <span data-ttu-id="3ba90-286">Użyj właściwość o nazwie **GenreId**.</span><span class="sxs-lookup"><span data-stu-id="3ba90-286">Use an int property with the name **GenreId**.</span></span>

    <span data-ttu-id="3ba90-287">(Fragment - kodu *modeli i dostęp do danych - Genre pierwszy kod Ex2*)</span><span class="sxs-lookup"><span data-stu-id="3ba90-287">(Code Snippet - *Models And Data Access - Ex2 Code First Genre*)</span></span>


    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample8.cs)]

    > [!NOTE]
    > <span data-ttu-id="3ba90-288">Aby pracować z konwencjami Code First, klasa Genre musi mieć właściwości klucza podstawowego, który będzie wykrywane automatycznie.</span><span class="sxs-lookup"><span data-stu-id="3ba90-288">To work with Code First conventions, the class Genre must have a primary key property that will be automatically detected.</span></span>
    > 
    > <span data-ttu-id="3ba90-289">Możesz przeczytać więcej na temat Konwencji pierwszy kod w tym [artykuł w witrynie msdn](https://msdn.microsoft.com/library/hh161541&amp;#040;v=vs.103&amp;#041;.aspx).</span><span class="sxs-lookup"><span data-stu-id="3ba90-289">You can read more about Code First Conventions in this [msdn article](https://msdn.microsoft.com/library/hh161541&amp;#040;v=vs.103&amp;#041;.aspx).</span></span>
2. <span data-ttu-id="3ba90-290">Teraz Otwórz klasy modelu POCO **albumu** z **modele** folderu projektu i zawierają kluczy obcych, Utwórz właściwości o nazwach **GenreId** i  **ArtistId**.</span><span class="sxs-lookup"><span data-stu-id="3ba90-290">Now, open the POCO model class **Album** from **Models** project folder and include the foreign keys, create properties with the names **GenreId** and **ArtistId**.</span></span> <span data-ttu-id="3ba90-291">Ta klasa już **GenreId** klucza podstawowego.</span><span class="sxs-lookup"><span data-stu-id="3ba90-291">This class already have the **GenreId** for the primary key.</span></span>

    <span data-ttu-id="3ba90-292">(Fragment - kodu *modeli i dostęp do danych - Ex2 kod pierwszego albumu*)</span><span class="sxs-lookup"><span data-stu-id="3ba90-292">(Code Snippet - *Models And Data Access - Ex2 Code First Album*)</span></span>


    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample9.cs)]
3. <span data-ttu-id="3ba90-293">Otwórz klasy modelu POCO **wykonawcy** i obejmują **ArtistId** właściwości.</span><span class="sxs-lookup"><span data-stu-id="3ba90-293">Open the POCO model class **Artist** and include the **ArtistId** property.</span></span>

    <span data-ttu-id="3ba90-294">(Fragment - kodu *modeli i dostęp do danych - wykonawcy pierwszy kod Ex2*)</span><span class="sxs-lookup"><span data-stu-id="3ba90-294">(Code Snippet - *Models And Data Access - Ex2 Code First Artist*)</span></span>


    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample10.cs)]
4. <span data-ttu-id="3ba90-295">Kliknij prawym przyciskiem myszy **modele** folderu projektu i wybierz **Dodaj | Klasa**.</span><span class="sxs-lookup"><span data-stu-id="3ba90-295">Right-click the **Models** project folder and select **Add | Class**.</span></span> <span data-ttu-id="3ba90-296">Nadaj nazwę plikowi **MusicStoreEntities.cs**.</span><span class="sxs-lookup"><span data-stu-id="3ba90-296">Name the file **MusicStoreEntities.cs**.</span></span> <span data-ttu-id="3ba90-297">Następnie kliknij przycisk **Dodaj.**</span><span class="sxs-lookup"><span data-stu-id="3ba90-297">Then, click **Add.**</span></span>

    <span data-ttu-id="3ba90-298">![Dodawanie klasy](aspnet-mvc-4-models-and-data-access/_static/image20.png "Dodawanie klasy")</span><span class="sxs-lookup"><span data-stu-id="3ba90-298">![Adding a class](aspnet-mvc-4-models-and-data-access/_static/image20.png "Adding a class")</span></span>

    <span data-ttu-id="3ba90-299">*Dodawanie nowego elementu*</span><span class="sxs-lookup"><span data-stu-id="3ba90-299">*Adding a new item*</span></span>

    <span data-ttu-id="3ba90-300">![Dodawanie class2](aspnet-mvc-4-models-and-data-access/_static/image21.png "Dodawanie class2")</span><span class="sxs-lookup"><span data-stu-id="3ba90-300">![Adding a class2](aspnet-mvc-4-models-and-data-access/_static/image21.png "Adding a class2")</span></span>

    <span data-ttu-id="3ba90-301">*Dodawanie klasy*</span><span class="sxs-lookup"><span data-stu-id="3ba90-301">*Adding a class*</span></span>
5. <span data-ttu-id="3ba90-302">Otwórz właśnie utworzony, klasa **MusicStoreEntities.cs**i uwzględnić przestrzenie nazw **System.Data.Entity** i **System.Data.Entity.Infrastructure**.</span><span class="sxs-lookup"><span data-stu-id="3ba90-302">Open the class you have just created, **MusicStoreEntities.cs**, and include the namespaces **System.Data.Entity** and **System.Data.Entity.Infrastructure**.</span></span>


    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample11.cs)]
6. <span data-ttu-id="3ba90-303">Zastąp deklaracji klasy, aby rozszerzyć **DbContext** klasy: deklarowanie publiczny **DBSet** i zastąpienia **OnModelCreating** — metoda.</span><span class="sxs-lookup"><span data-stu-id="3ba90-303">Replace the class declaration to extend the **DbContext** class: declare a public **DBSet** and override **OnModelCreating** method.</span></span> <span data-ttu-id="3ba90-304">Po wykonaniu tego kroku wystąpi klasy domeny, która połączy modelu za pomocą programu Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="3ba90-304">After this step you will get a domain class that will link your model with the Entity Framework.</span></span> <span data-ttu-id="3ba90-305">Aby to zrobić, Zastąp kod klasy następujące czynności:</span><span class="sxs-lookup"><span data-stu-id="3ba90-305">In order to do that, replace the class code with the following:</span></span>

    <span data-ttu-id="3ba90-306">(Fragment - kodu *modeli i dostęp do danych - MusicStoreEntities pierwszy kod Ex2*)</span><span class="sxs-lookup"><span data-stu-id="3ba90-306">(Code Snippet - *Models And Data Access - Ex2 Code First MusicStoreEntities*)</span></span>


    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample12.cs)]

    > [!NOTE]
    > <span data-ttu-id="3ba90-307">Z programu Entity Framework **DbContext** i **DBSet** można zbadać klasy POCO Genre.</span><span class="sxs-lookup"><span data-stu-id="3ba90-307">With Entity Framework **DbContext** and **DBSet** you will be able to query the POCO class Genre.</span></span> <span data-ttu-id="3ba90-308">Rozszerzając **OnModelCreating** metody, określasz w **kod** jak Genre zostaną zmapowane do tabeli bazy danych.</span><span class="sxs-lookup"><span data-stu-id="3ba90-308">By extending **OnModelCreating** method, you are specifying in the **code** how Genre will be mapped to a database table.</span></span> <span data-ttu-id="3ba90-309">Więcej informacji na temat obiektu DBContext i DBSet można znaleźć w tym artykule w witrynie msdn: [łącza](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=vs.103).aspx)</span><span class="sxs-lookup"><span data-stu-id="3ba90-309">You can find more information about DBContext and DBSet in this msdn article: [link](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=vs.103).aspx)</span></span>

<a id="Ex2Task4"></a>

<a id="Task_4_-_Querying_the_Database"></a>
#### <a name="task-4---querying-the-database"></a><span data-ttu-id="3ba90-310">Zadanie 4 — kwerendy bazy danych</span><span class="sxs-lookup"><span data-stu-id="3ba90-310">Task 4 - Querying the Database</span></span>

<span data-ttu-id="3ba90-311">To zadanie spowoduje zaktualizowanie klasy StoreController tak, aby zamiast dane zapisane na stałe, pobierze go z bazy danych.</span><span class="sxs-lookup"><span data-stu-id="3ba90-311">In this task, you will update the StoreController class so that, instead of using hardcoded data, it will retrieve it from the database.</span></span>

> [!NOTE]
> <span data-ttu-id="3ba90-312">To zadanie jest wspólne ćwiczenie 1.</span><span class="sxs-lookup"><span data-stu-id="3ba90-312">This task is in common with Exercise 1.</span></span>
> 
> <span data-ttu-id="3ba90-313">Jeśli ukończono ćwiczenie 1 można zauważyć następujące kroki są takie same, w obu podejść (najpierw bazy danych lub najpierw Code).</span><span class="sxs-lookup"><span data-stu-id="3ba90-313">If you completed Exercise 1 you will note these steps are the same in both approaches (Database first or Code first).</span></span> <span data-ttu-id="3ba90-314">Różnią się one w jaki sposób dane są połączone z modelem dostęp do obiektów danych jest jednak jeszcze przezroczysty z kontrolera.</span><span class="sxs-lookup"><span data-stu-id="3ba90-314">They are different in how the data is linked with the model, but the access to data entities is yet transparent from the controller.</span></span>


1. <span data-ttu-id="3ba90-315">Otwórz **Controllers\StoreController.cs** i dodaj następujące pole do klasy, aby pomieścić wystąpienia **MusicStoreEntities** klasę o nazwie **storeDB**:</span><span class="sxs-lookup"><span data-stu-id="3ba90-315">Open **Controllers\StoreController.cs** and add the following field to the class to hold an instance of the **MusicStoreEntities** class, named **storeDB**:</span></span>

    <span data-ttu-id="3ba90-316">(Fragment - kodu *modeli i dostęp do danych - Ex1 storeDB*)</span><span class="sxs-lookup"><span data-stu-id="3ba90-316">(Code Snippet - *Models And Data Access - Ex1 storeDB*)</span></span>


    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample13.cs)]
2. <span data-ttu-id="3ba90-317">**MusicStoreEntities** klasy udostępnia właściwości kolekcji dla każdej tabeli w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="3ba90-317">The **MusicStoreEntities** class exposes a collection property for each table in the database.</span></span> <span data-ttu-id="3ba90-318">Aktualizacja **Przeglądaj** metody akcji można pobrać określonego rodzaju ze wszystkimi **albumów**.</span><span class="sxs-lookup"><span data-stu-id="3ba90-318">Update **Browse** action method to retrieve a Genre with all of the **Albums**.</span></span>

    <span data-ttu-id="3ba90-319">(Fragment - kodu *modeli i dostęp do danych - przeglądania magazynu Ex2*)</span><span class="sxs-lookup"><span data-stu-id="3ba90-319">(Code Snippet - *Models And Data Access - Ex2 Store Browse*)</span></span>


    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample14.cs)]

    > [!NOTE]
    > <span data-ttu-id="3ba90-320">W przypadku korzystania z możliwości .NET o nazwie **LINQ** (zapytanie o języku zintegrowanym) do zapisania wyrażeń jednoznacznie zapytania względem tych kolekcje — co spowoduje wykonanie kodu w bazie danych i zwracać obiekty można umieszczonych przed.</span><span class="sxs-lookup"><span data-stu-id="3ba90-320">You are using a capability of .NET called **LINQ** (language-integrated query) to write strongly-typed query expressions against these collections - which will execute code against the database and return objects that you can program against.</span></span>
    > 
    > <span data-ttu-id="3ba90-321">Aby uzyskać więcej informacji na temat LINQ, odwiedź stronę [witrynę msdn](https://msdn.microsoft.com/library/bb397926(v=vs.110).aspx).</span><span class="sxs-lookup"><span data-stu-id="3ba90-321">For more information about LINQ, please visit the [msdn site](https://msdn.microsoft.com/library/bb397926(v=vs.110).aspx).</span></span>
3. <span data-ttu-id="3ba90-322">Aktualizacja **indeksu** metody akcji, aby pobrać wszystkie genres.</span><span class="sxs-lookup"><span data-stu-id="3ba90-322">Update **Index** action method to retrieve all the genres.</span></span>

    <span data-ttu-id="3ba90-323">(Fragment - kodu *modeli i Data Access — indeks magazynu Ex2*)</span><span class="sxs-lookup"><span data-stu-id="3ba90-323">(Code Snippet - *Models And Data Access - Ex2 Store Index*)</span></span>


    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample15.cs)]
4. <span data-ttu-id="3ba90-324">Aktualizacja **indeksu** metody akcji, aby pobrać wszystkie genres i przekształcenie kolekcji na listę.</span><span class="sxs-lookup"><span data-stu-id="3ba90-324">Update **Index** action method to retrieve all the genres and transform the collection to a list.</span></span>

    <span data-ttu-id="3ba90-325">(Fragment - kodu *modeli i dostęp do danych - GenreMenu magazynu Ex2*)</span><span class="sxs-lookup"><span data-stu-id="3ba90-325">(Code Snippet - *Models And Data Access - Ex2 Store GenreMenu*)</span></span>


    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample16.cs)]

<a id="Ex2Task5"></a>

<a id="Task_5_-_Running_the_Application"></a>
#### <a name="task-5---running-the-application"></a><span data-ttu-id="3ba90-326">Zadanie 5 działania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="3ba90-326">Task 5 - Running the Application</span></span>

<span data-ttu-id="3ba90-327">To zadanie będzie sprawdzać, czy strona indeksu magazynu wyświetli Genres przechowywane w bazie danych, zamiast ustalony te.</span><span class="sxs-lookup"><span data-stu-id="3ba90-327">In this task, you will check that the Store Index page will now display the Genres stored in the database instead of the hardcoded ones.</span></span> <span data-ttu-id="3ba90-328">Nie istnieje potrzeba zmiany szablonu widoku, ponieważ **StoreController** zwraca takie same **StoreIndexViewModel** jak poprzednio, ale tym razem dane będą pobierane z bazy danych.</span><span class="sxs-lookup"><span data-stu-id="3ba90-328">There is no need to change the View template because the **StoreController** is returning the same **StoreIndexViewModel** as before, but this time the data will come from the database.</span></span>

1. <span data-ttu-id="3ba90-329">Ponowne kompilowanie rozwiązania i naciśnij klawisz **F5** do uruchomienia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="3ba90-329">Rebuild the solution and press **F5** to run the Application.</span></span>
2. <span data-ttu-id="3ba90-330">Na stronie głównej datę rozpoczęcia projektu.</span><span class="sxs-lookup"><span data-stu-id="3ba90-330">The project starts in the Home page.</span></span> <span data-ttu-id="3ba90-331">Sprawdź, czy menu **Genres** nie jest już listy zapisane na stałe, a dane bezpośrednio są pobierane z bazy danych.</span><span class="sxs-lookup"><span data-stu-id="3ba90-331">Verify that the menu of **Genres** is no longer a hardcoded list, and the data is directly retrieved from the database.</span></span>

    ![BrowsingGenresFromDataBase](aspnet-mvc-4-models-and-data-access/_static/image22.png)

    <span data-ttu-id="3ba90-333">*Przeglądanie Genres z bazy danych*</span><span class="sxs-lookup"><span data-stu-id="3ba90-333">*Browsing Genres from the database*</span></span>
3. <span data-ttu-id="3ba90-334">Teraz przejdź do dowolnego rodzaju i sprawdź, czy albumów zostają wypełnione z bazy danych.</span><span class="sxs-lookup"><span data-stu-id="3ba90-334">Now browse to any genre and verify the albums are populated from database.</span></span>

    <span data-ttu-id="3ba90-335">![Przeglądanie albumów z bazy danych](aspnet-mvc-4-models-and-data-access/_static/image23.png "przeglądanie albumów z bazy danych")</span><span class="sxs-lookup"><span data-stu-id="3ba90-335">![Browsing Albums from the database](aspnet-mvc-4-models-and-data-access/_static/image23.png "Browsing Albums from the database")</span></span>

    <span data-ttu-id="3ba90-336">*Przeglądanie albumów z bazy danych*</span><span class="sxs-lookup"><span data-stu-id="3ba90-336">*Browsing Albums from the database*</span></span>

<a id="Exercise3"></a>

<a id="Exercise_3_Querying_the_Database_with_Parameters"></a>
### <a name="exercise-3-querying-the-database-with-parameters"></a><span data-ttu-id="3ba90-337">Ćwiczenie 3: Zapytanie bazy danych z parametrami</span><span class="sxs-lookup"><span data-stu-id="3ba90-337">Exercise 3: Querying the Database with Parameters</span></span>

<span data-ttu-id="3ba90-338">W tym ćwiczeniu dowiesz się, jak wykonać zapytanie dotyczące bazy danych przy użyciu parametrów i sposobu użycia, kształtowania wyników zapytania, funkcja, która ogranicza numerów bazy danych uzyskuje dostęp do pobierania danych w bardziej wydajny sposób.</span><span class="sxs-lookup"><span data-stu-id="3ba90-338">In this exercise, you will learn how to query the database using parameters, and how to use Query Result Shaping, a feature that reduces the number database accesses retrieving data in a more efficient way.</span></span>

> [!NOTE]
> <span data-ttu-id="3ba90-339">Aby uzyskać więcej informacji na kształtowania wyników zapytania, można znaleźć w następującej [artykuł w witrynie msdn](https://msdn.microsoft.com/library/bb896272&amp;#040;v=vs.100&amp;#041;.aspx).</span><span class="sxs-lookup"><span data-stu-id="3ba90-339">For further information on Query Result Shaping, visit the following [msdn article](https://msdn.microsoft.com/library/bb896272&amp;#040;v=vs.100&amp;#041;.aspx).</span></span>


<a id="Ex3Task1"></a>

<a id="Task_1_-_Modifying_StoreController_to_Retrieve_Albums_from_Database"></a>
#### <a name="task-1---modifying-storecontroller-to-retrieve-albums-from-database"></a><span data-ttu-id="3ba90-340">Zadanie 1 - modyfikowanie StoreController pobrać albumów z bazy danych</span><span class="sxs-lookup"><span data-stu-id="3ba90-340">Task 1 - Modifying StoreController to Retrieve Albums from Database</span></span>

<span data-ttu-id="3ba90-341">W tym zadaniu zostanie zmieniony **StoreController** klasy dostęp do bazy danych można pobrać albumów z określonego rodzaju.</span><span class="sxs-lookup"><span data-stu-id="3ba90-341">In this task, you will change the **StoreController** class to access the database to retrieve albums from a specific genre.</span></span>

1. <span data-ttu-id="3ba90-342">Otwórz **rozpocząć** rozwiązania, znajdujących się na **Source\Ex3 QueryingTheDatabaseWithParametersCodeFirst\Begin** folderu, jeśli chcesz użyć pierwszej kodu metody lub **Source\ Ex3 QueryingTheDatabaseWithParametersDBFirst\Begin** folderu, jeśli chcesz użyć metody pierwszej bazy danych.</span><span class="sxs-lookup"><span data-stu-id="3ba90-342">Open the **Begin** solution located at the **Source\Ex3-QueryingTheDatabaseWithParametersCodeFirst\Begin** folder if you want to use Code-First approach or **Source\Ex3-QueryingTheDatabaseWithParametersDBFirst\Begin** folder if you want to use Database-First approach.</span></span> <span data-ttu-id="3ba90-343">W przeciwnym razie możesz nadal korzystać **zakończenia** uzyskane rozwiązanie, wykonując poprzednim ćwiczeniu.</span><span class="sxs-lookup"><span data-stu-id="3ba90-343">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

    1. <span data-ttu-id="3ba90-344">Po otwarciu dostarczonych **rozpocząć** rozwiązania, musisz pobrać niektórych brakujących pakietów NuGet aby kontynuować.</span><span class="sxs-lookup"><span data-stu-id="3ba90-344">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="3ba90-345">Aby to zrobić, kliknij przycisk **projektu** menu i wybierz **Zarządzaj pakietami NuGet**.</span><span class="sxs-lookup"><span data-stu-id="3ba90-345">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
    2. <span data-ttu-id="3ba90-346">W **Zarządzaj pakietami NuGet** okna dialogowego, kliknij przycisk **przywrócić** celu pobieranie brakujących pakietów.</span><span class="sxs-lookup"><span data-stu-id="3ba90-346">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
    3. <span data-ttu-id="3ba90-347">Na koniec Skompiluj rozwiązanie, klikając **kompilacji** | **Kompiluj rozwiązanie**.</span><span class="sxs-lookup"><span data-stu-id="3ba90-347">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="3ba90-348">Jedną z zalet przy użyciu narzędzia NuGet jest, że nie masz do wysłania wszystkich bibliotek w projekcie, zmniejszenie jego rozmiar projektu.</span><span class="sxs-lookup"><span data-stu-id="3ba90-348">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="3ba90-349">Narzędzia Power NuGet określając wersje pakietów w pliku Packages.config, będzie można pobrać wymaganych bibliotek podczas pierwszego uruchomienia projektu.</span><span class="sxs-lookup"><span data-stu-id="3ba90-349">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="3ba90-350">Jest to, dlaczego konieczne będzie wykonanie tych kroków, po otwarciu istniejącego rozwiązania z tego laboratorium.</span><span class="sxs-lookup"><span data-stu-id="3ba90-350">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="3ba90-351">Otwórz **StoreController** klasę, aby zmienić **Przeglądaj** metody akcji.</span><span class="sxs-lookup"><span data-stu-id="3ba90-351">Open the **StoreController** class to change the **Browse** action method.</span></span> <span data-ttu-id="3ba90-352">Aby to zrobić, w **Eksploratora rozwiązań**, rozwiń węzeł **kontrolerów** folder i kliknij dwukrotnie **StoreController.cs**.</span><span class="sxs-lookup"><span data-stu-id="3ba90-352">To do this, in the **Solution Explorer**, expand the **Controllers** folder and double-click **StoreController.cs**.</span></span>
3. <span data-ttu-id="3ba90-353">Zmień **Przeglądaj** metody akcji można pobrać albumów dla określonego rodzaju.</span><span class="sxs-lookup"><span data-stu-id="3ba90-353">Change the **Browse** action method to retrieve albums for a specific genre.</span></span> <span data-ttu-id="3ba90-354">Aby to zrobić, Zastąp następujący kod:</span><span class="sxs-lookup"><span data-stu-id="3ba90-354">To do this, replace the following code:</span></span>

    <span data-ttu-id="3ba90-355">(Fragment - kodu *modeli i dostęp do danych - Ex3 StoreController BrowseMethod*)</span><span class="sxs-lookup"><span data-stu-id="3ba90-355">(Code Snippet - *Models And Data Access - Ex3 StoreController BrowseMethod*)</span></span>


    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample17.cs)]

    > [!NOTE]
    > <span data-ttu-id="3ba90-356">Aby wypełnić kolekcji jednostki, należy użyć **Include** metodę, aby określić ma zostać pobrane za albumów.</span><span class="sxs-lookup"><span data-stu-id="3ba90-356">To populate a collection of the entity, you need to use the **Include** method to specify you want to retrieve the albums too.</span></span> <span data-ttu-id="3ba90-357">Można użyć. **Single()** rozszerzenia LINQ, ponieważ w takim przypadku oczekuje tylko jednego rodzaju albumu.</span><span class="sxs-lookup"><span data-stu-id="3ba90-357">You can use the .**Single()** extension in LINQ because in this case only one genre is expected for an album.</span></span> <span data-ttu-id="3ba90-358">**Single()** metoda przyjmuje jako parametr w takim przypadku określa pojedynczy obiekt Genre w taki sposób, że jego nazwa jest zgodna z wartością zdefiniowane wyrażenia Lambda.</span><span class="sxs-lookup"><span data-stu-id="3ba90-358">The **Single()** method takes a Lambda expression as a parameter, which in this case specifies a single Genre object such that its name matches the value defined.</span></span>
    > 
    > <span data-ttu-id="3ba90-359">Będzie korzystać z funkcji, które można określić innych powiązanych jednostek, który ma być również załadowany po pobraniu obiektu Genre.</span><span class="sxs-lookup"><span data-stu-id="3ba90-359">You will take advantage of a feature that allows you to indicate other related entities you want loaded as well when the Genre object is retrieved.</span></span> <span data-ttu-id="3ba90-360">Ta funkcja jest nazywana **kształtowania wynik zapytania**i pozwala zmniejszyć liczbę potrzebnych do dostępu do bazy danych można pobrać informacji o.</span><span class="sxs-lookup"><span data-stu-id="3ba90-360">This feature is called **Query Result Shaping**, and enables you to reduce the number of times needed to access the database to retrieve information.</span></span> <span data-ttu-id="3ba90-361">W tym scenariuszu można wstępnie pobrać albumów dla rodzaju pobierania.</span><span class="sxs-lookup"><span data-stu-id="3ba90-361">In this scenario, you will want to pre-fetch the Albums for the Genre you retrieve.</span></span>
    > 
    > <span data-ttu-id="3ba90-362">Zapytanie zawiera **Genres.Include (&quot;albumów&quot;)** aby wskazać, że ma również albumów pokrewne.</span><span class="sxs-lookup"><span data-stu-id="3ba90-362">The query includes **Genres.Include(&quot;Albums&quot;)** to indicate that you want related albums as well.</span></span> <span data-ttu-id="3ba90-363">To spowoduje większą wydajność aplikacji, ponieważ będą pobierane dane zarówno Genre, jak i albumu w żądaniu pojedynczej bazy danych.</span><span class="sxs-lookup"><span data-stu-id="3ba90-363">This will result in a more efficient application, since it will retrieve both Genre and Album data in a single database request.</span></span>

<a id="Ex3Task2"></a>

<a id="Task_2_-_Running_the_Application"></a>
#### <a name="task-2---running-the-application"></a><span data-ttu-id="3ba90-364">Zadanie 2 — w uruchomionej aplikacji</span><span class="sxs-lookup"><span data-stu-id="3ba90-364">Task 2 - Running the Application</span></span>

<span data-ttu-id="3ba90-365">W tym zadaniu zostanie Uruchom aplikację i pobrać albumów określonego rodzaju z bazy danych.</span><span class="sxs-lookup"><span data-stu-id="3ba90-365">In this task, you will run the application and retrieve albums of a specific genre from the database.</span></span>

1. <span data-ttu-id="3ba90-366">Naciśnij klawisz **F5** do uruchomienia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="3ba90-366">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="3ba90-367">Na stronie głównej datę rozpoczęcia projektu.</span><span class="sxs-lookup"><span data-stu-id="3ba90-367">The project starts in the Home page.</span></span> <span data-ttu-id="3ba90-368">Zmień adres URL do **/magazynu/Przeglądaj? genre = Pop** można zweryfikować, że wyniki są pobierane z bazy danych.</span><span class="sxs-lookup"><span data-stu-id="3ba90-368">Change the URL to **/Store/Browse?genre=Pop** to verify that the results are being retrieved from the database.</span></span>

    <span data-ttu-id="3ba90-369">![Przeglądanie według rodzaju](aspnet-mvc-4-models-and-data-access/_static/image24.png "przeglądanie według rodzaju")</span><span class="sxs-lookup"><span data-stu-id="3ba90-369">![Browsing by genre](aspnet-mvc-4-models-and-data-access/_static/image24.png "Browsing by genre")</span></span>

    <span data-ttu-id="3ba90-370">*Przeglądanie/magazynu/Przeglądaj? genre = Pop*</span><span class="sxs-lookup"><span data-stu-id="3ba90-370">*Browsing /Store/Browse?genre=Pop*</span></span>

<a id="Ex3Task3"></a>

<a id="Task_3_-_Accessing_Albums_by_Id"></a>
#### <a name="task-3---accessing-albums-by-id"></a><span data-ttu-id="3ba90-371">Zadanie 3. uzyskiwanie dostępu do albumów według identyfikatora</span><span class="sxs-lookup"><span data-stu-id="3ba90-371">Task 3 - Accessing Albums by Id</span></span>

<span data-ttu-id="3ba90-372">W tym zadaniu zostanie powtórzony poprzedniej procedury, aby uzyskać albumów według ich identyfikatorów.</span><span class="sxs-lookup"><span data-stu-id="3ba90-372">In this task, you will repeat the previous procedure to get albums by their Id.</span></span>

1. <span data-ttu-id="3ba90-373">Zamknij przeglądarkę, jeśli to konieczne, aby powrócić do programu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="3ba90-373">Close the browser if needed, to return to Visual Studio.</span></span> <span data-ttu-id="3ba90-374">Otwórz **StoreController** klasę, aby zmienić **szczegóły** metody akcji.</span><span class="sxs-lookup"><span data-stu-id="3ba90-374">Open the **StoreController** class to change the **Details** action method.</span></span> <span data-ttu-id="3ba90-375">Aby to zrobić, w **Eksploratora rozwiązań**, rozwiń węzeł **kontrolerów** folder i kliknij dwukrotnie **StoreController.cs**.</span><span class="sxs-lookup"><span data-stu-id="3ba90-375">To do this, in the **Solution Explorer**, expand the **Controllers** folder and double-click **StoreController.cs**.</span></span>
2. <span data-ttu-id="3ba90-376">Zmień **szczegóły** na podstawie metody akcji można pobrać szczegółów albumów ich **identyfikator**. Aby to zrobić, Zastąp następujący kod:</span><span class="sxs-lookup"><span data-stu-id="3ba90-376">Change the **Details** action method to retrieve albums details based on their **Id**. To do this, replace the following code:</span></span>

    <span data-ttu-id="3ba90-377">(Fragment - kodu *modeli i dostęp do danych - Ex3 StoreController DetailsMethod*)</span><span class="sxs-lookup"><span data-stu-id="3ba90-377">(Code Snippet - *Models And Data Access - Ex3 StoreController DetailsMethod*)</span></span>


    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample18.cs)]

<a id="Ex3Task4"></a>

<a id="Task_4_-_Running_the_Application"></a>
#### <a name="task-4---running-the-application"></a><span data-ttu-id="3ba90-378">Zadanie 4 — uruchamianie aplikacji</span><span class="sxs-lookup"><span data-stu-id="3ba90-378">Task 4 - Running the Application</span></span>

<span data-ttu-id="3ba90-379">W tym zadaniu zostanie uruchomić aplikację w przeglądarce sieci web i Uzyskiwanie szczegółów albumu według ich identyfikatorów.</span><span class="sxs-lookup"><span data-stu-id="3ba90-379">In this task, you will run the Application in a web browser and obtain album details by their Id.</span></span>

1. <span data-ttu-id="3ba90-380">Naciśnij klawisz **F5** do uruchomienia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="3ba90-380">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="3ba90-381">Na stronie głównej datę rozpoczęcia projektu.</span><span class="sxs-lookup"><span data-stu-id="3ba90-381">The project starts in the Home page.</span></span> <span data-ttu-id="3ba90-382">Zmień adres URL do **/Store/Details/51** lub gatunki Przeglądaj i wybierz album, aby zweryfikować, że wyniki są pobierane z bazy danych.</span><span class="sxs-lookup"><span data-stu-id="3ba90-382">Change the URL to **/Store/Details/51** or browse the genres and select an album to verify that the results are being retrieved from the database.</span></span>

    <span data-ttu-id="3ba90-383">![Przeglądanie szczegółów](aspnet-mvc-4-models-and-data-access/_static/image25.png "przeglądania szczegółów")</span><span class="sxs-lookup"><span data-stu-id="3ba90-383">![Browsing Details](aspnet-mvc-4-models-and-data-access/_static/image25.png "Browsing Details")</span></span>

    <span data-ttu-id="3ba90-384">*Przeglądanie /Store/Details/51*</span><span class="sxs-lookup"><span data-stu-id="3ba90-384">*Browsing /Store/Details/51*</span></span>

> [!NOTE]
> <span data-ttu-id="3ba90-385">Ponadto można wdrożyć tę aplikację systemu Windows Azure Web Sites następujących [dodatek B: publikowania aplikacji ASP.NET MVC 4 przy użyciu narzędzia Web Deploy](#AppendixB).</span><span class="sxs-lookup"><span data-stu-id="3ba90-385">Additionally, you can deploy this application to Windows Azure Web Sites following [Appendix B: Publishing an ASP.NET MVC 4 Application using Web Deploy](#AppendixB).</span></span>


* * *

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a><span data-ttu-id="3ba90-386">Podsumowanie</span><span class="sxs-lookup"><span data-stu-id="3ba90-386">Summary</span></span>

<span data-ttu-id="3ba90-387">Poprzez wykonanie tego laboratorium Hands-on, kiedy znasz już podstawy ASP.NET MVC modeli i dostęp do danych, za pomocą **Database First** podejście, jak również **Code First** podejścia:</span><span class="sxs-lookup"><span data-stu-id="3ba90-387">By completing this Hands-on Lab you have learned the fundamentals of ASP.NET MVC Models and Data Access, using the **Database First** approach as well as the **Code First** Approach:</span></span>

- <span data-ttu-id="3ba90-388">Jak dodać bazy danych do rozwiązania, aby można było korzystać z jego danych</span><span class="sxs-lookup"><span data-stu-id="3ba90-388">How to add a database to the solution in order to consume its data</span></span>
- <span data-ttu-id="3ba90-389">Jak zaktualizować kontrolerów dostarczanie szablonów widoków danych z bazy danych zamiast ustalony</span><span class="sxs-lookup"><span data-stu-id="3ba90-389">How to update Controllers to provide View templates with the data taken from the database instead of hard-coded one</span></span>
- <span data-ttu-id="3ba90-390">Jak wykonać zapytanie dotyczące bazy danych przy użyciu parametrów</span><span class="sxs-lookup"><span data-stu-id="3ba90-390">How to query the database using parameters</span></span>
- <span data-ttu-id="3ba90-391">Jak używać zapytania wynik Przekształcanie, funkcja, która ogranicza liczbę uzyskuje dostęp do bazy danych, pobieranie danych w bardziej wydajny sposób</span><span class="sxs-lookup"><span data-stu-id="3ba90-391">How to use the Query Result Shaping, a feature that reduces the number of database accesses, retrieving data in a more efficient way</span></span>
- <span data-ttu-id="3ba90-392">Sposób korzystania z bazy danych imiona i Code First zbliża się do programu Microsoft Entity Framework do połączenia z modelem bazy danych</span><span class="sxs-lookup"><span data-stu-id="3ba90-392">How to use both Database First and Code First approaches in Microsoft Entity Framework to link the database with the model</span></span>

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a><span data-ttu-id="3ba90-393">Dodatek A: Instalowanie programu Visual Studio Express 2012 for Web</span><span class="sxs-lookup"><span data-stu-id="3ba90-393">Appendix A: Installing Visual Studio Express 2012 for Web</span></span>

<span data-ttu-id="3ba90-394">Można zainstalować **Microsoft Visual Studio Express 2012 for Web** lub innym &quot;Express&quot; przy użyciu wersji  **[Instalatora platformy sieci Web firmy Microsoft](https://www.microsoft.com/web/downloads/platform.aspx)** .</span><span class="sxs-lookup"><span data-stu-id="3ba90-394">You can install **Microsoft Visual Studio Express 2012 for Web** or another &quot;Express&quot; version using the **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**.</span></span> <span data-ttu-id="3ba90-395">Poniższe instrukcje przedstawiono czynności wymagane do zainstalowania *programu Visual studio Express 2012 for Web* przy użyciu *Instalatora platformy sieci Web firmy Microsoft*.</span><span class="sxs-lookup"><span data-stu-id="3ba90-395">The following instructions guide you through the steps required to install *Visual studio Express 2012 for Web* using *Microsoft Web Platform Installer*.</span></span>

1. <span data-ttu-id="3ba90-396">Przejdź do [ [https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169).</span><span class="sxs-lookup"><span data-stu-id="3ba90-396">Go to [[https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169).</span></span> <span data-ttu-id="3ba90-397">Alternatywnie, jeśli została już zainstalowana Instalatora platformy sieci Web, można otworzyć go i Wyszukaj produkt &quot; *programu Visual Studio Express 2012 for Web z zestawem Windows Azure SDK*&quot;.</span><span class="sxs-lookup"><span data-stu-id="3ba90-397">Alternatively, if you already have installed Web Platform Installer, you can open it and search for the product &quot;*Visual Studio Express 2012 for Web with Windows Azure SDK*&quot;.</span></span>
2. <span data-ttu-id="3ba90-398">Polecenie **teraz zainstalować**.</span><span class="sxs-lookup"><span data-stu-id="3ba90-398">Click on **Install Now**.</span></span> <span data-ttu-id="3ba90-399">Jeśli nie masz **Instalatora platformy sieci Web** nastąpi przekierowanie do pobrania i zainstalowania go najpierw.</span><span class="sxs-lookup"><span data-stu-id="3ba90-399">If you do not have **Web Platform Installer** you will be redirected to download and install it first.</span></span>
3. <span data-ttu-id="3ba90-400">Raz **Instalatora platformy sieci Web** jest otwarty, kliknij przycisk **zainstalować** można uruchomić Instalatora.</span><span class="sxs-lookup"><span data-stu-id="3ba90-400">Once **Web Platform Installer** is open, click **Install** to start the setup.</span></span>

    <span data-ttu-id="3ba90-401">![Instalowanie programu Visual Studio Express](aspnet-mvc-4-models-and-data-access/_static/image26.png "instalacji programu Visual Studio Express")</span><span class="sxs-lookup"><span data-stu-id="3ba90-401">![Install Visual Studio Express](aspnet-mvc-4-models-and-data-access/_static/image26.png "Install Visual Studio Express")</span></span>

    <span data-ttu-id="3ba90-402">*Instalowanie programu Visual Studio Express*</span><span class="sxs-lookup"><span data-stu-id="3ba90-402">*Install Visual Studio Express*</span></span>
4. <span data-ttu-id="3ba90-403">Odczytywanie wszystkich produktów licencji i warunków, a następnie kliknij przycisk **akceptuję** aby kontynuować.</span><span class="sxs-lookup"><span data-stu-id="3ba90-403">Read all the products' licenses and terms and click **I Accept** to continue.</span></span>

    ![Akceptowanie umowy licencyjnej](aspnet-mvc-4-models-and-data-access/_static/image27.png)

    <span data-ttu-id="3ba90-405">*Akceptowanie umowy licencyjnej*</span><span class="sxs-lookup"><span data-stu-id="3ba90-405">*Accepting the license terms*</span></span>
5. <span data-ttu-id="3ba90-406">Poczekaj na zakończenie procesu pobierania i instalacji.</span><span class="sxs-lookup"><span data-stu-id="3ba90-406">Wait until the downloading and installation process completes.</span></span>

    ![Postęp instalacji](aspnet-mvc-4-models-and-data-access/_static/image28.png)

    <span data-ttu-id="3ba90-408">*Postęp instalacji*</span><span class="sxs-lookup"><span data-stu-id="3ba90-408">*Installation progress*</span></span>
6. <span data-ttu-id="3ba90-409">Po zakończeniu instalacji kliknij przycisk **Zakończ**.</span><span class="sxs-lookup"><span data-stu-id="3ba90-409">When the installation completes, click **Finish**.</span></span>

    ![Instalacja została zakończona](aspnet-mvc-4-models-and-data-access/_static/image29.png)

    <span data-ttu-id="3ba90-411">*Instalacja została zakończona*</span><span class="sxs-lookup"><span data-stu-id="3ba90-411">*Installation completed*</span></span>
7. <span data-ttu-id="3ba90-412">Kliknij przycisk **zakończenia** aby zamknąć Instalatora platformy sieci Web.</span><span class="sxs-lookup"><span data-stu-id="3ba90-412">Click **Exit** to close Web Platform Installer.</span></span>
8. <span data-ttu-id="3ba90-413">Aby otworzyć program Visual Studio Express for Web, przejdź do **Start** ekranu i zacznij pisać &quot; **VS Express**&quot;, następnie kliknij polecenie **VS Express for Web** Kafelek.</span><span class="sxs-lookup"><span data-stu-id="3ba90-413">To open Visual Studio Express for Web, go to the **Start** screen and start writing &quot;**VS Express**&quot;, then click on the **VS Express for Web** tile.</span></span>

    ![VS Express for Web kafelka](aspnet-mvc-4-models-and-data-access/_static/image30.png)

    <span data-ttu-id="3ba90-415">*VS Express for Web kafelka*</span><span class="sxs-lookup"><span data-stu-id="3ba90-415">*VS Express for Web tile*</span></span>

<a id="AppendixB"></a>

<a id="Appendix_B_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
## <a name="appendix-b-publishing-an-aspnet-mvc-4-application-using-web-deploy"></a><span data-ttu-id="3ba90-416">Dodatek B: publikowania aplikacji ASP.NET MVC 4 przy użyciu narzędzia Web Deploy</span><span class="sxs-lookup"><span data-stu-id="3ba90-416">Appendix B: Publishing an ASP.NET MVC 4 Application using Web Deploy</span></span>

<span data-ttu-id="3ba90-417">Ten dodatek opisano sposób tworzenia nowej witryny sieci web z portalu zarządzania pakietu Windows Azure i publikowanie aplikacji, uzyskane wykonując laboratorium, korzystając z funkcji publikowania narzędzia Web Deploy dostarczane przez Windows Azure.</span><span class="sxs-lookup"><span data-stu-id="3ba90-417">This appendix will show you how to create a new web site from the Windows Azure Management Portal and publish the application you obtained by following the lab, taking advantage of the Web Deploy publishing feature provided by Windows Azure.</span></span>

<a id="ApxBTask1"></a>

<a id="Task_1_-_Creating_a_New_Web_Site_from_the_Windows_Azure_Portal"></a>
#### <a name="task-1---creating-a-new-web-site-from-the-windows-azure-portal"></a><span data-ttu-id="3ba90-418">Zadanie 1 — Tworzenie nowej witryny sieci Web systemu Windows portalu Azure</span><span class="sxs-lookup"><span data-stu-id="3ba90-418">Task 1 - Creating a New Web Site from the Windows Azure Portal</span></span>

1. <span data-ttu-id="3ba90-419">Przejdź do [portalu zarządzania pakietu Windows Azure](https://manage.windowsazure.com/) i zaloguj się przy użyciu poświadczeń Microsoft skojarzonych z Twoją subskrypcją.</span><span class="sxs-lookup"><span data-stu-id="3ba90-419">Go to the [Windows Azure Management Portal](https://manage.windowsazure.com/) and sign in using the Microsoft credentials associated with your subscription.</span></span>

    > [!NOTE]
    > <span data-ttu-id="3ba90-420">Z systemu Windows Azure można udostępniać 10 witryn sieci Web platformy ASP.NET bezpłatnie i następnie Skaluj w miarę zwiększania się ruchu.</span><span class="sxs-lookup"><span data-stu-id="3ba90-420">With Windows Azure you can host 10 ASP.NET Web Sites for free and then scale as your traffic grows.</span></span> <span data-ttu-id="3ba90-421">Możesz utworzyć konto [tutaj](http://aka.ms/aspnet-hol-azure).</span><span class="sxs-lookup"><span data-stu-id="3ba90-421">You can sign up [here](http://aka.ms/aspnet-hol-azure).</span></span>

    <span data-ttu-id="3ba90-422">![Zaloguj się do portalu Windows Azure](aspnet-mvc-4-models-and-data-access/_static/image31.png "Zaloguj się do portalu Windows Azure")</span><span class="sxs-lookup"><span data-stu-id="3ba90-422">![Log on to Windows Azure portal](aspnet-mvc-4-models-and-data-access/_static/image31.png "Log on to Windows Azure portal")</span></span>

    <span data-ttu-id="3ba90-423">*Zaloguj się do portalu zarządzania platformy Azure z systemem Windows*</span><span class="sxs-lookup"><span data-stu-id="3ba90-423">*Log on to Windows Azure Management Portal*</span></span>
2. <span data-ttu-id="3ba90-424">Kliknij przycisk **nowy** na pasku poleceń.</span><span class="sxs-lookup"><span data-stu-id="3ba90-424">Click **New** on the command bar.</span></span>

    <span data-ttu-id="3ba90-425">![Tworzenie nowej witryny sieci Web](aspnet-mvc-4-models-and-data-access/_static/image32.png "tworzenia nowej witryny sieci Web")</span><span class="sxs-lookup"><span data-stu-id="3ba90-425">![Creating a new Web Site](aspnet-mvc-4-models-and-data-access/_static/image32.png "Creating a new Web Site")</span></span>

    <span data-ttu-id="3ba90-426">*Tworzenie nowej witryny sieci Web*</span><span class="sxs-lookup"><span data-stu-id="3ba90-426">*Creating a new Web Site*</span></span>
3. <span data-ttu-id="3ba90-427">Kliknij przycisk **obliczeniowe** | **witryny sieci Web**.</span><span class="sxs-lookup"><span data-stu-id="3ba90-427">Click **Compute** | **Web Site**.</span></span> <span data-ttu-id="3ba90-428">Następnie wybierz **szybkie tworzenie** opcji.</span><span class="sxs-lookup"><span data-stu-id="3ba90-428">Then select **Quick Create** option.</span></span> <span data-ttu-id="3ba90-429">Podaj dostępny adres URL dla nowej witryny sieci web, a następnie kliknij przycisk **tworzenie witryny sieci Web**.</span><span class="sxs-lookup"><span data-stu-id="3ba90-429">Provide an available URL for the new web site and click **Create Web Site**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="3ba90-430">Witryny sieci Web systemu Windows Azure jest hostem dla aplikacji sieci web w chmurze, które można kontrolować i zarządzanie nimi.</span><span class="sxs-lookup"><span data-stu-id="3ba90-430">A Windows Azure Web Site is the host for a web application running in the cloud that you can control and manage.</span></span> <span data-ttu-id="3ba90-431">Opcja szybkie tworzenie umożliwia wdrażanie ukończonej aplikacji sieci web do systemu Windows Azure witryny internetowej z spoza portalu.</span><span class="sxs-lookup"><span data-stu-id="3ba90-431">The Quick Create option allows you to deploy a completed web application to the Windows Azure Web Site from outside the portal.</span></span> <span data-ttu-id="3ba90-432">Nie obejmuje kroki konfigurowania bazy danych.</span><span class="sxs-lookup"><span data-stu-id="3ba90-432">It does not include steps for setting up a database.</span></span>

    <span data-ttu-id="3ba90-433">![Tworzenie nowej witryny sieci Web przy użyciu szybkie tworzenie](aspnet-mvc-4-models-and-data-access/_static/image33.png "tworzenia nowej witryny sieci Web przy użyciu szybkie tworzenie")</span><span class="sxs-lookup"><span data-stu-id="3ba90-433">![Creating a new Web Site using Quick Create](aspnet-mvc-4-models-and-data-access/_static/image33.png "Creating a new Web Site using Quick Create")</span></span>

    <span data-ttu-id="3ba90-434">*Tworzenie nowej witryny sieci Web przy użyciu szybkie tworzenie*</span><span class="sxs-lookup"><span data-stu-id="3ba90-434">*Creating a new Web Site using Quick Create*</span></span>
4. <span data-ttu-id="3ba90-435">Poczekaj na nowe **witryny sieci Web** jest tworzony.</span><span class="sxs-lookup"><span data-stu-id="3ba90-435">Wait until the new **Web Site** is created.</span></span>
5. <span data-ttu-id="3ba90-436">Po utworzeniu witryny sieci Web kliknij łącze w obszarze **adres URL** kolumny.</span><span class="sxs-lookup"><span data-stu-id="3ba90-436">Once the Web Site is created click the link under the **URL** column.</span></span> <span data-ttu-id="3ba90-437">Sprawdź, czy działa nowej witryny sieci Web.</span><span class="sxs-lookup"><span data-stu-id="3ba90-437">Check that the new Web Site is working.</span></span>

    <span data-ttu-id="3ba90-438">![Przeglądanie do nowej witryny sieci web](aspnet-mvc-4-models-and-data-access/_static/image34.png "przeglądanie do nowej witryny sieci web")</span><span class="sxs-lookup"><span data-stu-id="3ba90-438">![Browsing to the new web site](aspnet-mvc-4-models-and-data-access/_static/image34.png "Browsing to the new web site")</span></span>

    <span data-ttu-id="3ba90-439">*Przeglądanie do nowej witryny sieci web*</span><span class="sxs-lookup"><span data-stu-id="3ba90-439">*Browsing to the new web site*</span></span>

    <span data-ttu-id="3ba90-440">![Witryna sieci Web działa](aspnet-mvc-4-models-and-data-access/_static/image35.png "uruchamiania witryny sieci Web")</span><span class="sxs-lookup"><span data-stu-id="3ba90-440">![Web site running](aspnet-mvc-4-models-and-data-access/_static/image35.png "Web site running")</span></span>

    <span data-ttu-id="3ba90-441">*Witryna sieci Web uruchomiona*</span><span class="sxs-lookup"><span data-stu-id="3ba90-441">*Web site running*</span></span>
6. <span data-ttu-id="3ba90-442">Wróć do portalu i kliknij nazwę witryny sieci web w obszarze **nazwa** kolumny do wyświetlenia strony zarządzania.</span><span class="sxs-lookup"><span data-stu-id="3ba90-442">Go back to the portal and click the name of the web site under the **Name** column to display the management pages.</span></span>

    <span data-ttu-id="3ba90-443">![Otwieranie stron witryny sieci web zarządzania](aspnet-mvc-4-models-and-data-access/_static/image36.png "otwieranie stron zarządzania witryny sieci web")</span><span class="sxs-lookup"><span data-stu-id="3ba90-443">![Opening the web site management pages](aspnet-mvc-4-models-and-data-access/_static/image36.png "Opening the web site management pages")</span></span>

    <span data-ttu-id="3ba90-444">*Otwieranie stron zarządzania witryny sieci Web*</span><span class="sxs-lookup"><span data-stu-id="3ba90-444">*Opening the Web Site management pages*</span></span>
7. <span data-ttu-id="3ba90-445">W **pulpitu nawigacyjnego** w obszarze **szybkiego dostępu** kliknij **pobieranie profilu publikowania** łącza.</span><span class="sxs-lookup"><span data-stu-id="3ba90-445">In the **Dashboard** page, under the **quick glance** section, click the **Download publish profile** link.</span></span>

    > [!NOTE]
    > <span data-ttu-id="3ba90-446">*Profilu publikowania* zawiera wszystkie informacje wymagane do publikowania aplikacji sieci web do witryny sieci Web systemu Windows Azure dla każdej metody włączone publikacji.</span><span class="sxs-lookup"><span data-stu-id="3ba90-446">The *publish profile* contains all of the information required to publish a web application to a Windows Azure website for each enabled publication method.</span></span> <span data-ttu-id="3ba90-447">Profil publikowania zawiera adresy URL, poświadczenia użytkownika i parametry bazy danych wymagane do nawiązania połączenia i uwierzytelniania dla każdego z punktów końcowych, dla których włączono metoda publikacji.</span><span class="sxs-lookup"><span data-stu-id="3ba90-447">The publish profile contains the URLs, user credentials and database strings required to connect to and authenticate against each of the endpoints for which a publication method is enabled.</span></span> <span data-ttu-id="3ba90-448">**Microsoft WebMatrix 2**, **Microsoft Visual Studio Express for Web** i **programu Microsoft Visual Studio 2012** obsługują odczytywanie publikowanie profile do zautomatyzowania te programy Publikowanie aplikacji sieci web do witryn sieci Web systemu Windows Azure.</span><span class="sxs-lookup"><span data-stu-id="3ba90-448">**Microsoft WebMatrix 2**, **Microsoft Visual Studio Express for Web** and **Microsoft Visual Studio 2012** support reading publish profiles to automate configuration of these programs for publishing web applications to Windows Azure websites.</span></span>

    <span data-ttu-id="3ba90-449">![Pobieranie witryny sieci web profilu publikowania](aspnet-mvc-4-models-and-data-access/_static/image37.png "pobierania witryny sieci web profilu publikowania")</span><span class="sxs-lookup"><span data-stu-id="3ba90-449">![Downloading the web site publish profile](aspnet-mvc-4-models-and-data-access/_static/image37.png "Downloading the web site publish profile")</span></span>

    <span data-ttu-id="3ba90-450">*Pobieranie witryny sieci Web profilu publikowania*</span><span class="sxs-lookup"><span data-stu-id="3ba90-450">*Downloading the Web Site publish profile*</span></span>
8. <span data-ttu-id="3ba90-451">Pobierz profil publikowania w znanej lokalizacji.</span><span class="sxs-lookup"><span data-stu-id="3ba90-451">Download the publish profile file to a known location.</span></span> <span data-ttu-id="3ba90-452">Dodatkowo w tym ćwiczeniu zobaczysz jak opublikować aplikację sieci web do witryny sieci Web systemu Windows Azure w programie Visual Studio przy użyciu tego pliku.</span><span class="sxs-lookup"><span data-stu-id="3ba90-452">Further in this exercise you will see how to use this file to publish a web application to a Windows Azure Web Sites from Visual Studio.</span></span>

    <span data-ttu-id="3ba90-453">![Zapisywanie pliku profilu publikowania](aspnet-mvc-4-models-and-data-access/_static/image38.png "zapisywanie profilu publikowania")</span><span class="sxs-lookup"><span data-stu-id="3ba90-453">![Saving the publish profile file](aspnet-mvc-4-models-and-data-access/_static/image38.png "Saving the publish profile")</span></span>

    <span data-ttu-id="3ba90-454">*Zapisywanie pliku profilu publikowania*</span><span class="sxs-lookup"><span data-stu-id="3ba90-454">*Saving the publish profile file*</span></span>

<a id="ApxBTask2"></a>

<a id="Task_2_-_Configuring_the_Database_Server"></a>
#### <a name="task-2---configuring-the-database-server"></a><span data-ttu-id="3ba90-455">Zadanie 2 — Konfigurowanie serwera bazy danych</span><span class="sxs-lookup"><span data-stu-id="3ba90-455">Task 2 - Configuring the Database Server</span></span>

<span data-ttu-id="3ba90-456">Jeśli aplikacja korzysta z programu SQL Server baz danych, należy utworzyć serwer bazy danych SQL.</span><span class="sxs-lookup"><span data-stu-id="3ba90-456">If your application makes use of SQL Server databases you will need to create a SQL Database server.</span></span> <span data-ttu-id="3ba90-457">Jeśli chcesz wdrożyć prostą aplikację, która nie korzysta z programu SQL Server może pominąć to zadanie.</span><span class="sxs-lookup"><span data-stu-id="3ba90-457">If you want to deploy a simple application that does not use SQL Server you might skip this task.</span></span>

1. <span data-ttu-id="3ba90-458">Będzie potrzebny serwer bazy danych SQL do przechowywania bazy danych aplikacji.</span><span class="sxs-lookup"><span data-stu-id="3ba90-458">You will need a SQL Database server for storing the application database.</span></span> <span data-ttu-id="3ba90-459">Można wyświetlić serwery bazy danych SQL z subskrypcji usługi Windows Azure Management Portal pod adresem **baz danych Sql** | **serwerów** | **serwera Pulpit nawigacyjny**.</span><span class="sxs-lookup"><span data-stu-id="3ba90-459">You can view the SQL Database servers from your subscription in the Windows Azure Management portal at **Sql Databases** | **Servers** | **Server's Dashboard**.</span></span> <span data-ttu-id="3ba90-460">Jeśli nie masz serwer, który został utworzony, można utworzyć przy użyciu jednego **Dodaj** przycisk paska poleceń.</span><span class="sxs-lookup"><span data-stu-id="3ba90-460">If you do not have a server created, you can create one using the **Add** button on the command bar.</span></span> <span data-ttu-id="3ba90-461">Zwróć uwagę na **nazwę serwera i adres URL, nazwę logowania administratora i hasła**, jak będą używane w następnego zadania.</span><span class="sxs-lookup"><span data-stu-id="3ba90-461">Take note of the **server name and URL, administrator login name and password**, as you will use them in the next tasks.</span></span> <span data-ttu-id="3ba90-462">Nie należy tworzyć bazy danych jeszcze, jako zostaną utworzone w późniejszym terminie.</span><span class="sxs-lookup"><span data-stu-id="3ba90-462">Do not create the database yet, as it will be created in a later stage.</span></span>

    <span data-ttu-id="3ba90-463">![Pulpit nawigacyjny serwera bazy danych SQL](aspnet-mvc-4-models-and-data-access/_static/image39.png "pulpitu nawigacyjnego serwera bazy danych SQL")</span><span class="sxs-lookup"><span data-stu-id="3ba90-463">![SQL Database Server Dashboard](aspnet-mvc-4-models-and-data-access/_static/image39.png "SQL Database Server Dashboard")</span></span>

    <span data-ttu-id="3ba90-464">*Pulpit nawigacyjny serwera bazy danych SQL*</span><span class="sxs-lookup"><span data-stu-id="3ba90-464">*SQL Database Server Dashboard*</span></span>
2. <span data-ttu-id="3ba90-465">W następnym zadaniem Testuj połączenie z bazą danych z programu Visual Studio z tego powodu należy uwzględnić lokalny adres IP serwera liście **dozwolone adresy IP**.</span><span class="sxs-lookup"><span data-stu-id="3ba90-465">In the next task you will test the database connection from Visual Studio, for that reason you need to include your local IP address in the server's list of **Allowed IP Addresses**.</span></span> <span data-ttu-id="3ba90-466">Aby to zrobić, kliknij przycisk **Konfiguruj**, wybierz adres IP z **bieżącego adresu IP klienta** i wklej go na **początkowy adres IP** i **końcowy adres IP** pola tekstowe i kliknij przycisk ![add-client-ip-address-ok-button](aspnet-mvc-4-models-and-data-access/_static/image40.png) przycisku.</span><span class="sxs-lookup"><span data-stu-id="3ba90-466">To do that, click **Configure**, select the IP address from **Current Client IP Address** and paste it on the **Start IP Address** and **End IP Address** text boxes and click the ![add-client-ip-address-ok-button](aspnet-mvc-4-models-and-data-access/_static/image40.png) button.</span></span>

    ![Dodawanie adresu IP klienta](aspnet-mvc-4-models-and-data-access/_static/image41.png)

    <span data-ttu-id="3ba90-468">*Dodawanie adresu IP klienta*</span><span class="sxs-lookup"><span data-stu-id="3ba90-468">*Adding Client IP Address*</span></span>
3. <span data-ttu-id="3ba90-469">Raz **adres IP klienta** jest dodawany do dozwolonych adresów IP kliknij na **zapisać** o potwierdzenie zmian.</span><span class="sxs-lookup"><span data-stu-id="3ba90-469">Once the **Client IP Address** is added to the allowed IP addresses list, click on **Save** to confirm the changes.</span></span>

    ![Potwierdzenie zmian](aspnet-mvc-4-models-and-data-access/_static/image42.png)

    <span data-ttu-id="3ba90-471">*Potwierdzenie zmian*</span><span class="sxs-lookup"><span data-stu-id="3ba90-471">*Confirm Changes*</span></span>

<a id="ApxBTask3"></a>

<a id="Task_3_-_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
#### <a name="task-3---publishing-an-aspnet-mvc-4-application-using-web-deploy"></a><span data-ttu-id="3ba90-472">Zadanie 3 - publikowania aplikacji ASP.NET MVC 4 przy użyciu narzędzia Web Deploy</span><span class="sxs-lookup"><span data-stu-id="3ba90-472">Task 3 - Publishing an ASP.NET MVC 4 Application using Web Deploy</span></span>

1. <span data-ttu-id="3ba90-473">Wróć do rozwiązania ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="3ba90-473">Go back to the ASP.NET MVC 4 solution.</span></span> <span data-ttu-id="3ba90-474">W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy projekt witryny sieci web i wybierz **publikowania**.</span><span class="sxs-lookup"><span data-stu-id="3ba90-474">In the **Solution Explorer**, right-click the web site project and select **Publish**.</span></span>

    <span data-ttu-id="3ba90-475">![Publikowanie aplikacji](aspnet-mvc-4-models-and-data-access/_static/image43.png "publikowania aplikacji")</span><span class="sxs-lookup"><span data-stu-id="3ba90-475">![Publishing the Application](aspnet-mvc-4-models-and-data-access/_static/image43.png "Publishing the Application")</span></span>

    <span data-ttu-id="3ba90-476">*Publikowanie witryny sieci web*</span><span class="sxs-lookup"><span data-stu-id="3ba90-476">*Publishing the web site*</span></span>
2. <span data-ttu-id="3ba90-477">Zaimportuj profil publikowania, zapisana w pierwszym zadaniu.</span><span class="sxs-lookup"><span data-stu-id="3ba90-477">Import the publish profile you saved in the first task.</span></span>

    <span data-ttu-id="3ba90-478">![Importowanie profilu publikowania](aspnet-mvc-4-models-and-data-access/_static/image44.png "Importowanie profilu publikowania")</span><span class="sxs-lookup"><span data-stu-id="3ba90-478">![Importing the publish profile](aspnet-mvc-4-models-and-data-access/_static/image44.png "Importing the publish profile")</span></span>

    <span data-ttu-id="3ba90-479">*Importowanie profilu publikowania*</span><span class="sxs-lookup"><span data-stu-id="3ba90-479">*Importing publish profile*</span></span>
3. <span data-ttu-id="3ba90-480">Kliknij przycisk **Weryfikacja połączenia z**.</span><span class="sxs-lookup"><span data-stu-id="3ba90-480">Click **Validate Connection**.</span></span> <span data-ttu-id="3ba90-481">Po zakończeniu sprawdzania kliknij **dalej**.</span><span class="sxs-lookup"><span data-stu-id="3ba90-481">Once Validation is complete click **Next**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="3ba90-482">Zakończeniu sprawdzania poprawności, gdy zostanie wyświetlony zielony znacznik wyboru są wyświetlane obok przycisku sprawdzania poprawności połączenia.</span><span class="sxs-lookup"><span data-stu-id="3ba90-482">Validation is complete once you see a green checkmark appear next to the Validate Connection button.</span></span>

    <span data-ttu-id="3ba90-483">![Sprawdzanie poprawności połączenia](aspnet-mvc-4-models-and-data-access/_static/image45.png "sprawdzanie poprawności połączenia")</span><span class="sxs-lookup"><span data-stu-id="3ba90-483">![Validating connection](aspnet-mvc-4-models-and-data-access/_static/image45.png "Validating connection")</span></span>

    <span data-ttu-id="3ba90-484">*Sprawdzanie poprawności połączenia*</span><span class="sxs-lookup"><span data-stu-id="3ba90-484">*Validating connection*</span></span>
4. <span data-ttu-id="3ba90-485">W **ustawienia** w obszarze **baz danych** sekcji, kliknij przycisk Dalej, aby textbox połączenia bazy danych (tj. **połączenia DefaultConnection**).</span><span class="sxs-lookup"><span data-stu-id="3ba90-485">In the **Settings** page, under the **Databases** section, click the button next to your database connection's textbox (i.e. **DefaultConnection**).</span></span>

    <span data-ttu-id="3ba90-486">![Konfiguracja narzędzia Web deploy](aspnet-mvc-4-models-and-data-access/_static/image46.png "Konfiguracja narzędzia Web deploy")</span><span class="sxs-lookup"><span data-stu-id="3ba90-486">![Web deploy configuration](aspnet-mvc-4-models-and-data-access/_static/image46.png "Web deploy configuration")</span></span>

    <span data-ttu-id="3ba90-487">*Konfiguracja narzędzia Web deploy*</span><span class="sxs-lookup"><span data-stu-id="3ba90-487">*Web deploy configuration*</span></span>
5. <span data-ttu-id="3ba90-488">Skonfiguruj połączenie z bazą danych w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="3ba90-488">Configure the database connection as follows:</span></span>

    - <span data-ttu-id="3ba90-489">W **nazwy serwera** wpisz swoją bazą danych SQL server adresu URL przy użyciu *tcp:* prefiks.</span><span class="sxs-lookup"><span data-stu-id="3ba90-489">In the **Server name** type your SQL Database server URL using the *tcp:* prefix.</span></span>
    - <span data-ttu-id="3ba90-490">W **nazwy użytkownika** wpisz nazwę logowania administratora serwera.</span><span class="sxs-lookup"><span data-stu-id="3ba90-490">In **User name** type your server administrator login name.</span></span>
    - <span data-ttu-id="3ba90-491">W **hasło** wpisz hasło logowania administratora serwera.</span><span class="sxs-lookup"><span data-stu-id="3ba90-491">In **Password** type your server administrator login password.</span></span>
    - <span data-ttu-id="3ba90-492">Wpisz nazwę nowej bazy danych.</span><span class="sxs-lookup"><span data-stu-id="3ba90-492">Type a new database name.</span></span>

    <span data-ttu-id="3ba90-493">![Konfigurowanie parametrów połączenia z lokalizacją docelową](aspnet-mvc-4-models-and-data-access/_static/image47.png "Konfigurowanie parametrów połączenia z lokalizacją docelową")</span><span class="sxs-lookup"><span data-stu-id="3ba90-493">![Configuring destination connection string](aspnet-mvc-4-models-and-data-access/_static/image47.png "Configuring destination connection string")</span></span>

    <span data-ttu-id="3ba90-494">*Konfigurowanie parametrów połączenia z lokalizacją docelową*</span><span class="sxs-lookup"><span data-stu-id="3ba90-494">*Configuring destination connection string*</span></span>
6. <span data-ttu-id="3ba90-495">Następnie kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="3ba90-495">Then click **OK**.</span></span> <span data-ttu-id="3ba90-496">Po wyświetleniu monitu można utworzyć bazy danych kliknij **tak**.</span><span class="sxs-lookup"><span data-stu-id="3ba90-496">When prompted to create the database click **Yes**.</span></span>

    <span data-ttu-id="3ba90-497">![Tworzenie bazy danych](aspnet-mvc-4-models-and-data-access/_static/image48.png "tworzenie parametry bazy danych")</span><span class="sxs-lookup"><span data-stu-id="3ba90-497">![Creating the database](aspnet-mvc-4-models-and-data-access/_static/image48.png "Creating the database string")</span></span>

    <span data-ttu-id="3ba90-498">*Tworzenie bazy danych*</span><span class="sxs-lookup"><span data-stu-id="3ba90-498">*Creating the database*</span></span>
7. <span data-ttu-id="3ba90-499">Ciągu połączenia używanego do łączenia z bazą danych SQL w systemie Windows Azure jest wyświetlany w pole tekstowe domyślne połączenie.</span><span class="sxs-lookup"><span data-stu-id="3ba90-499">The connection string you will use to connect to SQL Database in Windows Azure is shown within Default Connection textbox.</span></span> <span data-ttu-id="3ba90-500">Następnie kliknij przycisk **Dalej**.</span><span class="sxs-lookup"><span data-stu-id="3ba90-500">Then click **Next**.</span></span>

    <span data-ttu-id="3ba90-501">![Parametry połączenia wskazujące bazę danych SQL](aspnet-mvc-4-models-and-data-access/_static/image49.png "ciąg połączenia wskazujące bazę danych SQL")</span><span class="sxs-lookup"><span data-stu-id="3ba90-501">![Connection string pointing to SQL Database](aspnet-mvc-4-models-and-data-access/_static/image49.png "Connection string pointing to SQL Database")</span></span>

    <span data-ttu-id="3ba90-502">*Parametry połączenia wskazujące bazę danych SQL*</span><span class="sxs-lookup"><span data-stu-id="3ba90-502">*Connection string pointing to SQL Database*</span></span>
8. <span data-ttu-id="3ba90-503">W **Podgląd** kliknij przycisk **publikowania**.</span><span class="sxs-lookup"><span data-stu-id="3ba90-503">In the **Preview** page, click **Publish**.</span></span>

    <span data-ttu-id="3ba90-504">![Publikowanie aplikacji sieci web](aspnet-mvc-4-models-and-data-access/_static/image50.png "publikowania aplikacji sieci web")</span><span class="sxs-lookup"><span data-stu-id="3ba90-504">![Publishing the web application](aspnet-mvc-4-models-and-data-access/_static/image50.png "Publishing the web application")</span></span>

    <span data-ttu-id="3ba90-505">*Publikowanie aplikacji sieci web*</span><span class="sxs-lookup"><span data-stu-id="3ba90-505">*Publishing the web application*</span></span>
9. <span data-ttu-id="3ba90-506">Po zakończeniu procesu publikowania domyślnej przeglądarce otworzy opublikowanej witryny sieci web.</span><span class="sxs-lookup"><span data-stu-id="3ba90-506">Once the publishing process finishes, your default browser will open the published web site.</span></span>

<a id="AppendixC"></a>

<a id="Appendix_C_Using_Code_Snippets"></a>
## <a name="appendix-c-using-code-snippets"></a><span data-ttu-id="3ba90-507">Dodatek C: korzystania z wstawek kodu</span><span class="sxs-lookup"><span data-stu-id="3ba90-507">Appendix C: Using Code Snippets</span></span>

<span data-ttu-id="3ba90-508">Wstawki kodu zapewniają całego kodu, które są potrzebne w zasięgu ręki.</span><span class="sxs-lookup"><span data-stu-id="3ba90-508">With code snippets, you have all the code you need at your fingertips.</span></span> <span data-ttu-id="3ba90-509">Dokument laboratorium informuje o dokładnie po ich użycia, jak pokazano na poniższej ilustracji.</span><span class="sxs-lookup"><span data-stu-id="3ba90-509">The lab document will tell you exactly when you can use them, as shown in the following figure.</span></span>

<span data-ttu-id="3ba90-510">![Aby wstawić kod do projektu przy użyciu wstawki kodu programu Visual Studio](aspnet-mvc-4-models-and-data-access/_static/image51.png "wstawki kodu za pomocą programu Visual Studio, aby wstawić kod do projektu")</span><span class="sxs-lookup"><span data-stu-id="3ba90-510">![Using Visual Studio code snippets to insert code into your project](aspnet-mvc-4-models-and-data-access/_static/image51.png "Using Visual Studio code snippets to insert code into your project")</span></span>

<span data-ttu-id="3ba90-511">*Aby wstawić kod do projektu przy użyciu wstawki kodu programu Visual Studio*</span><span class="sxs-lookup"><span data-stu-id="3ba90-511">*Using Visual Studio code snippets to insert code into your project*</span></span>

<span data-ttu-id="3ba90-512">***Aby dodać fragment kodu za pomocą klawiatury (C# tylko)***</span><span class="sxs-lookup"><span data-stu-id="3ba90-512">***To add a code snippet using the keyboard (C# only)***</span></span>

1. <span data-ttu-id="3ba90-513">Umieść kursor, w którym chcesz wstawić kod.</span><span class="sxs-lookup"><span data-stu-id="3ba90-513">Place the cursor where you would like to insert the code.</span></span>
2. <span data-ttu-id="3ba90-514">Zacznij wpisywać nazwę fragment (bez spacji i łączniki).</span><span class="sxs-lookup"><span data-stu-id="3ba90-514">Start typing the snippet name (without spaces or hyphens).</span></span>
3. <span data-ttu-id="3ba90-515">Obejrzyj jako IntelliSense wyświetla zgodne z nazwami wstawki.</span><span class="sxs-lookup"><span data-stu-id="3ba90-515">Watch as IntelliSense displays matching snippets' names.</span></span>
4. <span data-ttu-id="3ba90-516">Wybierz prawidłowe fragment (lub zachować wpisywanie do momentu wybrania fragment całą nazwę).</span><span class="sxs-lookup"><span data-stu-id="3ba90-516">Select the correct snippet (or keep typing until the entire snippet's name is selected).</span></span>
5. <span data-ttu-id="3ba90-517">Naciśnij klawisz Tab dwa razy, aby wstawić fragment kodu w lokalizacji kursora.</span><span class="sxs-lookup"><span data-stu-id="3ba90-517">Press the Tab key twice to insert the snippet at the cursor location.</span></span>

<span data-ttu-id="3ba90-518">![Rozpocznij wpisywanie nazwy fragment](aspnet-mvc-4-models-and-data-access/_static/image52.png "zacznij wpisywać nazwę wstawki programu")</span><span class="sxs-lookup"><span data-stu-id="3ba90-518">![Start typing the snippet name](aspnet-mvc-4-models-and-data-access/_static/image52.png "Start typing the snippet name")</span></span>

<span data-ttu-id="3ba90-519">*Rozpocznij wpisywanie nazwy fragment kodu*</span><span class="sxs-lookup"><span data-stu-id="3ba90-519">*Start typing the snippet name*</span></span>

<span data-ttu-id="3ba90-520">![Naciśnij klawisz Tab, aby wybrać wyróżnione fragment](aspnet-mvc-4-models-and-data-access/_static/image53.png "naciśnij klawisz Tab, aby wybrać wyróżnione fragment kodu")</span><span class="sxs-lookup"><span data-stu-id="3ba90-520">![Press Tab to select the highlighted snippet](aspnet-mvc-4-models-and-data-access/_static/image53.png "Press Tab to select the highlighted snippet")</span></span>

<span data-ttu-id="3ba90-521">*Naciśnij klawisz Tab, aby wybrać wyróżnione fragment kodu*</span><span class="sxs-lookup"><span data-stu-id="3ba90-521">*Press Tab to select the highlighted snippet*</span></span>

<span data-ttu-id="3ba90-522">![Ponownie naciśnij klawisz Tab i fragment rozwinie](aspnet-mvc-4-models-and-data-access/_static/image54.png "rozwinie ponownie naciśnij klawisz Tab i wstawki kodu")</span><span class="sxs-lookup"><span data-stu-id="3ba90-522">![Press Tab again and the snippet will expand](aspnet-mvc-4-models-and-data-access/_static/image54.png "Press Tab again and the snippet will expand")</span></span>

<span data-ttu-id="3ba90-523">*Rozwinie ponownie naciśnij klawisz Tab i wstawki kodu*</span><span class="sxs-lookup"><span data-stu-id="3ba90-523">*Press Tab again and the snippet will expand*</span></span>

<span data-ttu-id="3ba90-524">***Aby dodać fragment kodu za pomocą myszy (C#, Visual Basic i XML)*** 1.</span><span class="sxs-lookup"><span data-stu-id="3ba90-524">***To add a code snippet using the mouse (C#, Visual Basic and XML)*** 1.</span></span> <span data-ttu-id="3ba90-525">Kliknij prawym przyciskiem myszy, gdy chcesz wstawić fragment kodu.</span><span class="sxs-lookup"><span data-stu-id="3ba90-525">Right-click where you want to insert the code snippet.</span></span>

1. <span data-ttu-id="3ba90-526">Wybierz **wstawić fragment** następuje **Moje wstawki kodu**.</span><span class="sxs-lookup"><span data-stu-id="3ba90-526">Select **Insert Snippet** followed by **My Code Snippets**.</span></span>
2. <span data-ttu-id="3ba90-527">Wybierz odpowiedni fragment z listy, klikając go.</span><span class="sxs-lookup"><span data-stu-id="3ba90-527">Pick the relevant snippet from the list, by clicking on it.</span></span>

<span data-ttu-id="3ba90-528">![Kliknij prawym przyciskiem myszy, gdy chcesz wstawić fragment kodu i wybierz wstawić fragment](aspnet-mvc-4-models-and-data-access/_static/image55.png "kliknij prawym przyciskiem myszy, gdy chcesz wstawić fragment kodu i wybierz wstawić fragment kodu")</span><span class="sxs-lookup"><span data-stu-id="3ba90-528">![Right-click where you want to insert the code snippet and select Insert Snippet](aspnet-mvc-4-models-and-data-access/_static/image55.png "Right-click where you want to insert the code snippet and select Insert Snippet")</span></span>

<span data-ttu-id="3ba90-529">*Kliknij prawym przyciskiem myszy, gdy chcesz wstawić fragment kodu i wybierz wstawić fragment kodu*</span><span class="sxs-lookup"><span data-stu-id="3ba90-529">*Right-click where you want to insert the code snippet and select Insert Snippet*</span></span>

<span data-ttu-id="3ba90-530">![Wybierz odpowiedni fragment z listy, klikając go](aspnet-mvc-4-models-and-data-access/_static/image56.png "wybierz odpowiedni fragment z listy, klikając go")</span><span class="sxs-lookup"><span data-stu-id="3ba90-530">![Pick the relevant snippet from the list, by clicking on it](aspnet-mvc-4-models-and-data-access/_static/image56.png "Pick the relevant snippet from the list, by clicking on it")</span></span>

<span data-ttu-id="3ba90-531">*Wybierz odpowiedni fragment z listy, klikając go*</span><span class="sxs-lookup"><span data-stu-id="3ba90-531">*Pick the relevant snippet from the list, by clicking on it*</span></span>
