---
uid: mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-models-and-data-access
title: ASP.NET MVC 4 — modele i dostęp do danych | Dokumentacja firmy Microsoft
author: rick-anderson
description: 'Uwaga: W tym laboratorium praktyczne przyjęto założenie, że masz podstawową wiedzę na temat platformy ASP.NET MVC. Jeśli nie używasz platformy ASP.NET MVC przed, zalecamy zapoznać się z platformy ASP.NET MVC 4...'
ms.author: aspnetcontent
ms.date: 02/18/2013
ms.assetid: 634ea84b-f904-4afe-b71b-49cccef4d9cc
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-models-and-data-access
msc.type: authoredcontent
ms.openlocfilehash: bbb726f9f4abb2f9c72153a8c7986c5c83a9a840
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/05/2018
ms.locfileid: "37818375"
---
# <a name="aspnet-mvc-4-models-and-data-access"></a><span data-ttu-id="ef39b-104">ASP.NET MVC 4 — modele i dostęp do danych</span><span class="sxs-lookup"><span data-stu-id="ef39b-104">ASP.NET MVC 4 Models and Data Access</span></span>

<span data-ttu-id="ef39b-105">Przez [Camp w sieci Web zespołu](https://twitter.com/webcamps)</span><span class="sxs-lookup"><span data-stu-id="ef39b-105">By [Web Camps Team](https://twitter.com/webcamps)</span></span>

[<span data-ttu-id="ef39b-106">Pobierz Camp Web szkolenia Kit</span><span class="sxs-lookup"><span data-stu-id="ef39b-106">Download Web Camps Training Kit</span></span>](https://aka.ms/webcamps-training-kit)

<span data-ttu-id="ef39b-107">W tym laboratorium praktyczne przyjęto założenie, masz podstawową wiedzę na temat **platformy ASP.NET MVC**.</span><span class="sxs-lookup"><span data-stu-id="ef39b-107">This Hands-on Lab assumes you have basic knowledge of **ASP.NET MVC**.</span></span> <span data-ttu-id="ef39b-108">Jeśli nie używasz **platformy ASP.NET MVC** , zalecamy zapoznać się z **podstawowe informacje dotyczące programu ASP.NET MVC 4** warsztatów.</span><span class="sxs-lookup"><span data-stu-id="ef39b-108">If you have not used **ASP.NET MVC** before, we recommend you to go over **ASP.NET MVC 4 Fundamentals** Hands-on Lab.</span></span>

<span data-ttu-id="ef39b-109">W tym laboratorium przeprowadzi Cię przez ulepszeń i nowych funkcji, które opisano wcześniej, stosując drobne zmiany do przykładowej aplikacji sieci Web, pod warunkiem w folderze źródłowym.</span><span class="sxs-lookup"><span data-stu-id="ef39b-109">This lab walks you through the enhancements and new features previously described by applying minor changes to a sample Web application provided in the Source folder.</span></span>

> [!NOTE]
> <span data-ttu-id="ef39b-110">Wszystkie przykładowy kod i fragmenty kodu są uwzględnione w sieci Web Camp zestaw szkoleniowy, dostępne pod adresem [wersji Microsoft — w sieci Web/WebCampTrainingKit](https://aka.ms/webcamps-training-kit).</span><span class="sxs-lookup"><span data-stu-id="ef39b-110">All sample code and snippets are included in the Web Camps Training Kit, available at [Microsoft-Web/WebCampTrainingKit Releases](https://aka.ms/webcamps-training-kit).</span></span> <span data-ttu-id="ef39b-111">Projekt specyficzne dla tego laboratorium znajduje się w temacie [platformy ASP.NET MVC 4 modele i dostęp do danych](https://github.com/Microsoft-Web/HOL-MVC4ModelsAndDataAccess).</span><span class="sxs-lookup"><span data-stu-id="ef39b-111">The project specific to this lab is available at [ASP.NET MVC 4 Models and Data Access](https://github.com/Microsoft-Web/HOL-MVC4ModelsAndDataAccess).</span></span>

<span data-ttu-id="ef39b-112">W **podstawowe informacje dotyczące platformy ASP.NET MVC** warsztatów, możesz mieć zostały przekazywania zakodowanych danych z kontrolerów do Przeglądanie szablonów.</span><span class="sxs-lookup"><span data-stu-id="ef39b-112">In **ASP.NET MVC Fundamentals** Hands-on Lab, you have been passing hard-coded data from the Controllers to the View templates.</span></span> <span data-ttu-id="ef39b-113">Jednak aby można było tworzenie rzeczywistych aplikacji sieci Web, możesz chcieć użyć rzeczywista baza danych.</span><span class="sxs-lookup"><span data-stu-id="ef39b-113">But, in order to build a real Web application, you might want to use a real database.</span></span>

<span data-ttu-id="ef39b-114">W tym laboratorium praktyczne opisano, jak za pomocą aparatu bazy danych, aby można było przechowywać i pobierać dane potrzebne do aplikacji Music Store.</span><span class="sxs-lookup"><span data-stu-id="ef39b-114">This Hands-on Lab will show you how to use a database engine in order to store and retrieve the data needed for the Music Store application.</span></span> <span data-ttu-id="ef39b-115">To zrobić, możesz uruchomić przy użyciu istniejącej bazy danych i utworzone na podstawie modelu Entity Data Model.</span><span class="sxs-lookup"><span data-stu-id="ef39b-115">To accomplish that, you will start with an existing database and create the Entity Data Model from it.</span></span> <span data-ttu-id="ef39b-116">W tym środowisku laboratoryjnym będzie spełniać **Database First** podejście, jak również **Code First** podejście.</span><span class="sxs-lookup"><span data-stu-id="ef39b-116">Throughout this lab, you will meet the **Database First** approach as well as the **Code First** approach.</span></span>

<span data-ttu-id="ef39b-117">Jednak możesz również użyć **pierwszy Model** podejście, utwórz ten sam model przy użyciu narzędzi, a następnie wygeneruj bazy danych z niego.</span><span class="sxs-lookup"><span data-stu-id="ef39b-117">However, you can also use the **Model First** approach, create the same model using the tools, and then generate the database from it.</span></span>

<span data-ttu-id="ef39b-118">![Vs pierwszej bazy danych. Pierwszy model](aspnet-mvc-4-models-and-data-access/_static/image1.png "Database First w programie vs. Najpierw modelu")</span><span class="sxs-lookup"><span data-stu-id="ef39b-118">![Database First vs. Model First](aspnet-mvc-4-models-and-data-access/_static/image1.png "Database First vs. Model First")</span></span>

<span data-ttu-id="ef39b-119">*Vs pierwszej bazy danych. Najpierw modelu*</span><span class="sxs-lookup"><span data-stu-id="ef39b-119">*Database First vs. Model First*</span></span>

<span data-ttu-id="ef39b-120">Po wygenerowaniu modelu, wprowadzisz odpowiednie korekty w StoreController zapewnienie widoków Store przy użyciu danych pobranych z bazy danych, zamiast korzystać z zakodowanych danych.</span><span class="sxs-lookup"><span data-stu-id="ef39b-120">After generating the Model, you will make the proper adjustments in the StoreController to provide the Store Views with the data taken from the database, instead of using hard-coded data.</span></span> <span data-ttu-id="ef39b-121">Nie należy wprowadzać zmian do szablonów widoku ponieważ StoreController będzie zwracać ten sam modele widoków do Przeglądanie szablonów, mimo że w tej chwili dane będą pochodzić z bazy danych.</span><span class="sxs-lookup"><span data-stu-id="ef39b-121">You will not need to make any change to the View templates because the StoreController will be returning the same ViewModels to the View templates, although this time the data will come from the database.</span></span>

<span data-ttu-id="ef39b-122">**Pierwszym sposobem kodu**</span><span class="sxs-lookup"><span data-stu-id="ef39b-122">**The Code First Approach**</span></span>

<span data-ttu-id="ef39b-123">Podejścia Code First pozwala zdefiniować model z kodu bez generowania klasy, które są zazwyczaj powiązane z framework.</span><span class="sxs-lookup"><span data-stu-id="ef39b-123">The Code First approach allows us to define the model from the code without generating classes that are generally coupled with the framework.</span></span>

<span data-ttu-id="ef39b-124">W kodzie, najpierw obiekty modelu są definiowane za pomocą POCOs, &quot;zwykłych starych obiektów CLR&quot;.</span><span class="sxs-lookup"><span data-stu-id="ef39b-124">In code first, model objects are defined with POCOs, &quot;Plain Old CLR Objects&quot;.</span></span> <span data-ttu-id="ef39b-125">POCOs są proste zwykły klasy, które nie dziedziczenia i nie należy implementować interfejsy.</span><span class="sxs-lookup"><span data-stu-id="ef39b-125">POCOs are simple plain classes that have no inheritance and do not implement interfaces.</span></span> <span data-ttu-id="ef39b-126">Firma Microsoft może automatycznie generować bazy danych z nich lub możemy Użyj istniejącej bazy danych i wygenerować mapowania klasy z kodu.</span><span class="sxs-lookup"><span data-stu-id="ef39b-126">We can automatically generate the database from them, or we can use an existing database and generate the class mapping from the code.</span></span>

<span data-ttu-id="ef39b-127">Korzyści z używania tej metody jest modelu pozostają niezależne od framework trwałości (w tym przypadku Entity Framework), zgodnie z klasy POCOs nie są powiązane z architekturą mapowania.</span><span class="sxs-lookup"><span data-stu-id="ef39b-127">The benefits of using this approach is that the Model remains independent from the persistence framework (in this case, Entity Framework), as the POCOs classes are not coupled with the mapping framework.</span></span>

> [!NOTE]
> <span data-ttu-id="ef39b-128">To laboratorium jest oparte na programie ASP.NET MVC 4 i wersję aplikacji przykładowej Music Store dostosowanej zminimalizowane, aby dopasować tylko te funkcje, które są wyświetlane w tym laboratorium praktyczne.</span><span class="sxs-lookup"><span data-stu-id="ef39b-128">This Lab is based on ASP.NET MVC 4 and a version of the Music Store sample application customized and minimized to fit only the features shown in this Hands-On Lab.</span></span>
> 
> <span data-ttu-id="ef39b-129">Jeśli chcesz zapoznać się z całym **Music Store** aplikacją z samouczka można znaleźć w [MVC — muzyka-Store](https://github.com/evilDave/MVC-Music-Store).</span><span class="sxs-lookup"><span data-stu-id="ef39b-129">If you wish to explore the whole **Music Store** tutorial application you can find it in [MVC-Music-Store](https://github.com/evilDave/MVC-Music-Store).</span></span>


<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a><span data-ttu-id="ef39b-130">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="ef39b-130">Prerequisites</span></span>

<span data-ttu-id="ef39b-131">Należy dysponować następującymi elementami do przygotowania tego laboratorium:</span><span class="sxs-lookup"><span data-stu-id="ef39b-131">You must have the following items to complete this lab:</span></span>

- <span data-ttu-id="ef39b-132">[Microsoft Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) lub wyższego poziomu (odczyt [dodatek a.](#AppendixA) instrukcje dotyczące sposobu jego instalacji).</span><span class="sxs-lookup"><span data-stu-id="ef39b-132">[Microsoft Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) or superior (read [Appendix A](#AppendixA) for instructions on how to install it).</span></span>

<a id="Setup"></a>

<a id="Setup"></a>
### <a name="setup"></a><span data-ttu-id="ef39b-133">Konfiguracja</span><span class="sxs-lookup"><span data-stu-id="ef39b-133">Setup</span></span>

<span data-ttu-id="ef39b-134">**Instalowanie fragmentów kodu**</span><span class="sxs-lookup"><span data-stu-id="ef39b-134">**Installing Code Snippets**</span></span>

<span data-ttu-id="ef39b-135">Dla wygody większość kodu, który będzie zarządzany wzdłuż tego laboratorium jest dostępna jako fragmenty kodu programu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="ef39b-135">For convenience, much of the code you will be managing along this lab is available as Visual Studio code snippets.</span></span> <span data-ttu-id="ef39b-136">Aby zainstalować fragmenty kodu, uruchom **.\Source\Setup\CodeSnippets.vsi** pliku.</span><span class="sxs-lookup"><span data-stu-id="ef39b-136">To install the code snippets run **.\Source\Setup\CodeSnippets.vsi** file.</span></span>

<span data-ttu-id="ef39b-137">Jeśli nie jesteś zaznajomiony z fragmentów kodu w usłudze Visual Studio i chcesz dowiedzieć się, jak z nich korzystać, możesz zapoznać się z dodatku z tego dokumentu &quot; [dodatek C: za pomocą wstawek kodu programu](#AppendixC)&quot;.</span><span class="sxs-lookup"><span data-stu-id="ef39b-137">If you are not familiar with the Visual Studio Code Snippets, and want to learn how to use them, you can refer to the appendix from this document &quot;[Appendix C: Using Code Snippets](#AppendixC)&quot;.</span></span>

* * *

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a><span data-ttu-id="ef39b-138">Ćwiczenia</span><span class="sxs-lookup"><span data-stu-id="ef39b-138">Exercises</span></span>

<span data-ttu-id="ef39b-139">W tym laboratorium praktyczne składa się przez następujących czynnościach:</span><span class="sxs-lookup"><span data-stu-id="ef39b-139">This Hands-on Lab is comprised by the following exercises:</span></span>

1. [<span data-ttu-id="ef39b-140">Ćwiczenie 1: Dodawanie bazy danych</span><span class="sxs-lookup"><span data-stu-id="ef39b-140">Exercise 1: Adding a Database</span></span>](#Exercise1)
2. [<span data-ttu-id="ef39b-141">Ćwiczenie 2: Tworzenie bazy danych za pomocą funkcji Code First</span><span class="sxs-lookup"><span data-stu-id="ef39b-141">Exercise 2: Creating a Database using Code First</span></span>](#Exercise2)
3. [<span data-ttu-id="ef39b-142">Ćwiczenie 3: Zapytanie do bazy danych z parametrami</span><span class="sxs-lookup"><span data-stu-id="ef39b-142">Exercise 3: Querying the Database with Parameters</span></span>](#Exercise3)

> [!NOTE]
> <span data-ttu-id="ef39b-143">Towarzyszy każdego wykonywania **zakończenia** folderu zawierającego wynikowy rozwiązania, należy uzyskać po ukończeniu ćwiczenia.</span><span class="sxs-lookup"><span data-stu-id="ef39b-143">Each exercise is accompanied by an **End** folder containing the resulting solution you should obtain after completing the exercises.</span></span> <span data-ttu-id="ef39b-144">Jeśli potrzebujesz dodatkowej pomocy ćwiczeń opisanych w dalszej, można użyć tego rozwiązania jako wskazówki.</span><span class="sxs-lookup"><span data-stu-id="ef39b-144">You can use this solution as a guide if you need additional help working through the exercises.</span></span>


<span data-ttu-id="ef39b-145">Szacowany czas do ukończenia tego laboratorium: **35 minut**.</span><span class="sxs-lookup"><span data-stu-id="ef39b-145">Estimated time to complete this lab: **35 minutes**.</span></span>

<a id="Exercise1"></a>

<a id="Exercise_1_Adding_a_Database"></a>
### <a name="exercise-1-adding-a-database"></a><span data-ttu-id="ef39b-146">Ćwiczenie 1: Dodawanie bazy danych</span><span class="sxs-lookup"><span data-stu-id="ef39b-146">Exercise 1: Adding a Database</span></span>

<span data-ttu-id="ef39b-147">W tym ćwiczeniu dowiesz się, jak dodać bazę danych przy użyciu tabel aplikacji MusicStore do rozwiązania w celu korzystania z danych.</span><span class="sxs-lookup"><span data-stu-id="ef39b-147">In this exercise, you will learn how to add a database with the tables of the MusicStore application to the solution in order to consume its data.</span></span> <span data-ttu-id="ef39b-148">Gdy baza danych jest generowane przy użyciu modelu i dodany do rozwiązania, należy zmodyfikować klasy StoreController, aby udostępnić szablon widoku danych pobranych z bazy danych, zamiast używać zakodowanych wartości.</span><span class="sxs-lookup"><span data-stu-id="ef39b-148">Once the database is generated with the model, and added to the solution, you will modify the StoreController class to provide the View template with the data taken from the database, instead of using hard-coded values.</span></span>

<a id="Ex1Task1"></a>

<a id="Task_1_-_Adding_a_Database"></a>
#### <a name="task-1---adding-a-database"></a><span data-ttu-id="ef39b-149">Zadanie 1 — Dodawanie bazy danych</span><span class="sxs-lookup"><span data-stu-id="ef39b-149">Task 1 - Adding a Database</span></span>

<span data-ttu-id="ef39b-150">W tym zadaniu należy dodać już utworzoną bazę danych z głównego tabelami aplikacji MusicStore do rozwiązania.</span><span class="sxs-lookup"><span data-stu-id="ef39b-150">In this task, you will add an already created database with the main tables of the MusicStore application to the solution.</span></span>

1. <span data-ttu-id="ef39b-151">Otwórz **rozpocząć** rozwiązania znajdujący się w **źródło/Ex1-AddingADatabaseDBFirst/rozpoczęcia/** folderu.</span><span class="sxs-lookup"><span data-stu-id="ef39b-151">Open the **Begin** solution located at **Source/Ex1-AddingADatabaseDBFirst/Begin/** folder.</span></span>

   1. <span data-ttu-id="ef39b-152">Musisz pobrać niektóre brakujące pakiety NuGet przed kontynuowaniem.</span><span class="sxs-lookup"><span data-stu-id="ef39b-152">You will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="ef39b-153">Aby to zrobić, kliknij przycisk **projektu** menu, a następnie wybierz **Zarządzaj pakietami NuGet**.</span><span class="sxs-lookup"><span data-stu-id="ef39b-153">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="ef39b-154">W **Zarządzaj pakietami NuGet** okno dialogowe, kliknij przycisk **przywrócić** Aby pobrać brakujące pakiety.</span><span class="sxs-lookup"><span data-stu-id="ef39b-154">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="ef39b-155">Na koniec Skompiluj rozwiązanie, klikając **kompilacji** | **Kompiluj rozwiązanie**.</span><span class="sxs-lookup"><span data-stu-id="ef39b-155">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="ef39b-156">Jedną z zalet za pomocą narzędzia NuGet jest, że nie masz do wysłania wszystkie biblioteki w projekcie, zmniejszenie rozmiaru projektu.</span><span class="sxs-lookup"><span data-stu-id="ef39b-156">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="ef39b-157">Za pomocą narzędzi NuGet Power Tools, określając wersji pakietu w pliku Packages.config można pobrać wymaganych bibliotek podczas pierwszego uruchomienia projektu.</span><span class="sxs-lookup"><span data-stu-id="ef39b-157">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="ef39b-158">Jest to, dlaczego należy uruchomić następujące kroki, po otwarciu istniejącego rozwiązania, w tym środowisku laboratoryjnym.</span><span class="sxs-lookup"><span data-stu-id="ef39b-158">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="ef39b-159">Dodaj **MvcMusicStore** plik bazy danych.</span><span class="sxs-lookup"><span data-stu-id="ef39b-159">Add **MvcMusicStore** database file.</span></span> <span data-ttu-id="ef39b-160">W tym laboratorium praktyczne użyje zostały już utworzone bazy danych o nazwie **MvcMusicStore.mdf**.</span><span class="sxs-lookup"><span data-stu-id="ef39b-160">In this Hands-on Lab, you will use an already created database called **MvcMusicStore.mdf**.</span></span> <span data-ttu-id="ef39b-161">Aby to zrobić, kliknij prawym przyciskiem myszy **aplikacji\_danych** folderu, wskaż **Dodaj** a następnie kliknij przycisk **istniejący element**.</span><span class="sxs-lookup"><span data-stu-id="ef39b-161">To do that, right-click **App\_Data** folder, point to **Add** and then click **Existing Item**.</span></span> <span data-ttu-id="ef39b-162">Przejdź do **\Source\Assets** i wybierz **MvcMusicStore.mdf** pliku.</span><span class="sxs-lookup"><span data-stu-id="ef39b-162">Browse to **\Source\Assets** and select the **MvcMusicStore.mdf** file.</span></span>

    <span data-ttu-id="ef39b-163">![Dodawanie istniejącego elementu](aspnet-mvc-4-models-and-data-access/_static/image2.png "Dodawanie istniejącego elementu")</span><span class="sxs-lookup"><span data-stu-id="ef39b-163">![Adding an Existing Item](aspnet-mvc-4-models-and-data-access/_static/image2.png "Adding an Existing Item")</span></span>

    <span data-ttu-id="ef39b-164">*Dodawanie istniejącego elementu*</span><span class="sxs-lookup"><span data-stu-id="ef39b-164">*Adding an Existing Item*</span></span>

    <span data-ttu-id="ef39b-165">![Plik bazy danych MvcMusicStore.mdf](aspnet-mvc-4-models-and-data-access/_static/image3.png "MvcMusicStore.mdf plik bazy danych")</span><span class="sxs-lookup"><span data-stu-id="ef39b-165">![MvcMusicStore.mdf database file](aspnet-mvc-4-models-and-data-access/_static/image3.png "MvcMusicStore.mdf database file")</span></span>

    <span data-ttu-id="ef39b-166">*Plik bazy danych MvcMusicStore.mdf*</span><span class="sxs-lookup"><span data-stu-id="ef39b-166">*MvcMusicStore.mdf database file*</span></span>

    <span data-ttu-id="ef39b-167">Baza danych została dodana do projektu.</span><span class="sxs-lookup"><span data-stu-id="ef39b-167">The database has been added to the project.</span></span> <span data-ttu-id="ef39b-168">Nawet wtedy, gdy baza danych znajduje się wewnątrz rozwiązania, możesz zapytania i aktualizować w miarę była hostowana na serwerze innej bazy danych.</span><span class="sxs-lookup"><span data-stu-id="ef39b-168">Even when the database is located inside the solution, you can query and update it as it was hosted in a different database server.</span></span>

    <span data-ttu-id="ef39b-169">![MvcMusicStore bazy danych, w Eksploratorze rozwiązań](aspnet-mvc-4-models-and-data-access/_static/image4.png "MvcMusicStore bazy danych, w Eksploratorze rozwiązań")</span><span class="sxs-lookup"><span data-stu-id="ef39b-169">![MvcMusicStore database in Solution Explorer](aspnet-mvc-4-models-and-data-access/_static/image4.png "MvcMusicStore database in Solution Explorer")</span></span>

    <span data-ttu-id="ef39b-170">*MvcMusicStore bazy danych, w Eksploratorze rozwiązań*</span><span class="sxs-lookup"><span data-stu-id="ef39b-170">*MvcMusicStore database in Solution Explorer*</span></span>
3. <span data-ttu-id="ef39b-171">Sprawdź połączenie z bazą danych.</span><span class="sxs-lookup"><span data-stu-id="ef39b-171">Verify the connection to the database.</span></span> <span data-ttu-id="ef39b-172">Aby to zrobić, kliknij dwukrotnie **MvcMusicStore.mdf** nawiązać połączenie.</span><span class="sxs-lookup"><span data-stu-id="ef39b-172">To do this, double-click **MvcMusicStore.mdf** to establish a connection.</span></span>

    <span data-ttu-id="ef39b-173">![Nawiązywanie połączenia z MvcMusicStore.mdf](aspnet-mvc-4-models-and-data-access/_static/image5.png "nawiązywania połączenia z MvcMusicStore.mdf")</span><span class="sxs-lookup"><span data-stu-id="ef39b-173">![Connecting to MvcMusicStore.mdf](aspnet-mvc-4-models-and-data-access/_static/image5.png "Connecting to MvcMusicStore.mdf")</span></span>

    <span data-ttu-id="ef39b-174">*Nawiązywanie połączenia z MvcMusicStore.mdf*</span><span class="sxs-lookup"><span data-stu-id="ef39b-174">*Connecting to MvcMusicStore.mdf*</span></span>

<a id="Ex1Task2"></a>

<a id="Task_2_-_Creating_a_Data_Model"></a>
#### <a name="task-2---creating-a-data-model"></a><span data-ttu-id="ef39b-175">Zadanie 2 — Tworzenie modelu danych</span><span class="sxs-lookup"><span data-stu-id="ef39b-175">Task 2 - Creating a Data Model</span></span>

<span data-ttu-id="ef39b-176">W tym zadaniu utworzysz model danych do interakcji z bazy danych dodane w poprzednim zadaniu.</span><span class="sxs-lookup"><span data-stu-id="ef39b-176">In this task, you will create a data model to interact with the database added in the previous task.</span></span>

1. <span data-ttu-id="ef39b-177">Tworzenie modelu danych, która będzie reprezentowała bazy danych.</span><span class="sxs-lookup"><span data-stu-id="ef39b-177">Create a data model that will represent the database.</span></span> <span data-ttu-id="ef39b-178">Aby to zrobić, w Eksploratorze rozwiązań kliknij prawym przyciskiem myszy **modeli** folderu, wskaż **Dodaj** a następnie kliknij przycisk **nowy element**.</span><span class="sxs-lookup"><span data-stu-id="ef39b-178">To do this, in Solution Explorer right-click the **Models** folder, point to **Add** and then click **New Item**.</span></span> <span data-ttu-id="ef39b-179">W **Dodaj nowy element** okno dialogowe, wybierz opcję **danych** szablonu i następnie **ADO.NET Entity Data Model** elementu.</span><span class="sxs-lookup"><span data-stu-id="ef39b-179">In the **Add New Item** dialog, select the **Data** template and then the **ADO.NET Entity Data Model** item.</span></span> <span data-ttu-id="ef39b-180">Zmień nazwę modelu danych, aby **StoreDB.edmx** i kliknij przycisk **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="ef39b-180">Change the data model name to **StoreDB.edmx** and click **Add**.</span></span>

    <span data-ttu-id="ef39b-181">![Dodawanie modelu danych jednostki ADO.NET StoreDB](aspnet-mvc-4-models-and-data-access/_static/image6.png "Dodawanie modelu danych jednostki ADO.NET StoreDB")</span><span class="sxs-lookup"><span data-stu-id="ef39b-181">![Adding the StoreDB ADO.NET Entity Data Model](aspnet-mvc-4-models-and-data-access/_static/image6.png "Adding the StoreDB ADO.NET Entity Data Model")</span></span>

    <span data-ttu-id="ef39b-182">*Dodawanie modelu danych jednostki ADO.NET StoreDB*</span><span class="sxs-lookup"><span data-stu-id="ef39b-182">*Adding the StoreDB ADO.NET Entity Data Model*</span></span>
2. <span data-ttu-id="ef39b-183">**Kreator modelu Entity Data Model** będą wyświetlane.</span><span class="sxs-lookup"><span data-stu-id="ef39b-183">The **Entity Data Model Wizard** will appear.</span></span> <span data-ttu-id="ef39b-184">Ten kreator przeprowadzi Cię przez tworzenie warstwy modelu.</span><span class="sxs-lookup"><span data-stu-id="ef39b-184">This wizard will guide you through the creation of the model layer.</span></span> <span data-ttu-id="ef39b-185">Ponieważ modelu należy tworzyć w oparciu o istniejące recentyl bazy danych dodane, wybierz opcję **Generuj z bazy danych** i kliknij przycisk **dalej**.</span><span class="sxs-lookup"><span data-stu-id="ef39b-185">Since the model should be created based on the existing database recentyl added, select **Generate from database** and click **Next**.</span></span>

    <span data-ttu-id="ef39b-186">![Wybieranie zawartości modelu](aspnet-mvc-4-models-and-data-access/_static/image7.png "wybierania zawartości modelu")</span><span class="sxs-lookup"><span data-stu-id="ef39b-186">![Choosing the model content](aspnet-mvc-4-models-and-data-access/_static/image7.png "Choosing the model content")</span></span>

    <span data-ttu-id="ef39b-187">*Wybieranie modelu zawartości*</span><span class="sxs-lookup"><span data-stu-id="ef39b-187">*Choosing the model content*</span></span>
3. <span data-ttu-id="ef39b-188">Ponieważ model jest generowany z bazy danych, należy określić tego połączenia.</span><span class="sxs-lookup"><span data-stu-id="ef39b-188">Since you are generating a model from a database, you will need to specify the connection to use.</span></span> <span data-ttu-id="ef39b-189">Kliknij przycisk **nowe połączenie**.</span><span class="sxs-lookup"><span data-stu-id="ef39b-189">Click **New Connection**.</span></span>
4. <span data-ttu-id="ef39b-190">Wybierz **plik bazy danych programu Microsoft SQL Server** i kliknij przycisk **Kontynuuj**.</span><span class="sxs-lookup"><span data-stu-id="ef39b-190">Select **Microsoft SQL Server Database File** and click **Continue**.</span></span>

    <span data-ttu-id="ef39b-191">![Wybierz źródło danych](aspnet-mvc-4-models-and-data-access/_static/image8.png "wybierz źródło danych")</span><span class="sxs-lookup"><span data-stu-id="ef39b-191">![Choose data source](aspnet-mvc-4-models-and-data-access/_static/image8.png "Choose data source")</span></span>

    <span data-ttu-id="ef39b-192">*Wybierz źródło danych w oknie dialogowym*</span><span class="sxs-lookup"><span data-stu-id="ef39b-192">*Choose data source dialog*</span></span>
5. <span data-ttu-id="ef39b-193">Kliknij przycisk **Przeglądaj** i wybierz bazę danych **MvcMusicStore.mdf** zlokalizowanego w **aplikacji\_danych** folder i kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="ef39b-193">Click **Browse** and select the database **MvcMusicStore.mdf** you located in the **App\_Data** folder and click **OK**.</span></span>

    <span data-ttu-id="ef39b-194">![Właściwości połączenia](aspnet-mvc-4-models-and-data-access/_static/image9.png "właściwości połączenia")</span><span class="sxs-lookup"><span data-stu-id="ef39b-194">![Connection properties](aspnet-mvc-4-models-and-data-access/_static/image9.png "Connection properties")</span></span>

    <span data-ttu-id="ef39b-195">*Właściwości połączenia*</span><span class="sxs-lookup"><span data-stu-id="ef39b-195">*Connection properties*</span></span>
6. <span data-ttu-id="ef39b-196">Wygenerowana klasa powinna mieć taką samą nazwę jak parametry połączenia obiektu, więc zmień jego nazwę na **MusicStoreEntities** i kliknij przycisk **dalej**.</span><span class="sxs-lookup"><span data-stu-id="ef39b-196">The generated class should have the same name as the entity connection string, so change its name to **MusicStoreEntities** and click **Next**.</span></span>

    <span data-ttu-id="ef39b-197">![Wybieranie połączenia danych](aspnet-mvc-4-models-and-data-access/_static/image10.png "Wybieranie połączenia danych")</span><span class="sxs-lookup"><span data-stu-id="ef39b-197">![Choosing the data connection](aspnet-mvc-4-models-and-data-access/_static/image10.png "Choosing the data connection")</span></span>

    <span data-ttu-id="ef39b-198">*Wybieranie połączenia danych*</span><span class="sxs-lookup"><span data-stu-id="ef39b-198">*Choosing the data connection*</span></span>
7. <span data-ttu-id="ef39b-199">Wybierz obiekty bazy danych do użycia.</span><span class="sxs-lookup"><span data-stu-id="ef39b-199">Choose the database objects to use.</span></span> <span data-ttu-id="ef39b-200">Zgodnie z modelem jednostki będą używane tylko tabele bazy danych, wybierz **tabel** opcji i upewnij się, że **zawierać kolumny klucza obcego w modelu** i **Pluralize lub końcówek wygenerowany nazwy obiektów** również są zaznaczone opcje.</span><span class="sxs-lookup"><span data-stu-id="ef39b-200">As the Entity Model will use just the database's tables, select the **Tables** option, and make sure that the **Include foreign key columns in the model** and **Pluralize or singularize generated object names** options are also selected.</span></span> <span data-ttu-id="ef39b-201">Zmień Namespace modelu do **MvcMusicStore.Model** i kliknij przycisk **Zakończ**.</span><span class="sxs-lookup"><span data-stu-id="ef39b-201">Change the Model Namespace to **MvcMusicStore.Model** and click **Finish**.</span></span>

    <span data-ttu-id="ef39b-202">![Wybieranie obiektów bazy danych](aspnet-mvc-4-models-and-data-access/_static/image11.png "wybierania obiektów bazy danych")</span><span class="sxs-lookup"><span data-stu-id="ef39b-202">![Choosing the database objects](aspnet-mvc-4-models-and-data-access/_static/image11.png "Choosing the database objects")</span></span>

    <span data-ttu-id="ef39b-203">*Wybieranie obiektów bazy danych*</span><span class="sxs-lookup"><span data-stu-id="ef39b-203">*Choosing the database objects*</span></span>

    > [!NOTE]
    > <span data-ttu-id="ef39b-204">Jeśli zostanie wyświetlone okno dialogowe z ostrzeżeniem zabezpieczeń, kliknij przycisk **OK** do uruchamiania szablonu i Generuj klasy dla jednostki modelu.</span><span class="sxs-lookup"><span data-stu-id="ef39b-204">If a Security Warning dialog is shown, click **OK** to run the template and generate the classes for the model entities.</span></span>
8. <span data-ttu-id="ef39b-205">Diagram jednostek bazy danych pojawi się i gdy osobna klasę, która mapuje każdej tabeli w bazie danych zostanie utworzony.</span><span class="sxs-lookup"><span data-stu-id="ef39b-205">An entity diagram for the database will appear, while a separate class that maps each table to the database will be created.</span></span> <span data-ttu-id="ef39b-206">Na przykład **albumów** tabeli będą reprezentowane przez **albumu** klasy, w której każda kolumna w tabeli będzie zmapowana do właściwości klasy.</span><span class="sxs-lookup"><span data-stu-id="ef39b-206">For example, the **Albums** table will be represented by an **Album** class, where each column in the table will map to a class property.</span></span> <span data-ttu-id="ef39b-207">Pozwoli to zapytanie i Praca z obiektami, które reprezentują wierszy w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="ef39b-207">This will allow you to query and work with objects that represent rows in the database.</span></span>

    <span data-ttu-id="ef39b-208">![Diagram jednostek](aspnet-mvc-4-models-and-data-access/_static/image12.png "diagram jednostki")</span><span class="sxs-lookup"><span data-stu-id="ef39b-208">![Entity diagram](aspnet-mvc-4-models-and-data-access/_static/image12.png "Entity diagram")</span></span>

    <span data-ttu-id="ef39b-209">*Diagram jednostek*</span><span class="sxs-lookup"><span data-stu-id="ef39b-209">*Entity diagram*</span></span>

    > [!NOTE]
    > <span data-ttu-id="ef39b-210">Szablony T4 (.tt) uruchamiać kod służący do generowania klasy jednostek i spowoduje zastąpienie istniejących klas o takiej samej nazwie.</span><span class="sxs-lookup"><span data-stu-id="ef39b-210">The T4 templates (.tt) run code to generate the entities classes and will overwrite the existing classes with the same name.</span></span> <span data-ttu-id="ef39b-211">W tym przykładzie klasy &quot;albumu&quot;, &quot;gatunku&quot; i &quot;wykonawcy&quot; zostały zastąpione wygenerowanego kodu.</span><span class="sxs-lookup"><span data-stu-id="ef39b-211">In this example, the classes &quot;Album&quot;, &quot;Genre&quot; and &quot;Artist&quot; were overwritten with the generated code.</span></span>


<a id="Ex1Task3"></a>

<a id="Task_3_-_Building_the_Application"></a>
#### <a name="task-3---building-the-application"></a><span data-ttu-id="ef39b-212">Zadanie 3 — Tworzenie aplikacji</span><span class="sxs-lookup"><span data-stu-id="ef39b-212">Task 3 - Building the Application</span></span>

<span data-ttu-id="ef39b-213">W tym zadaniu można sprawdzi, mimo że generowania modelu zostały usunięte **albumu**, **gatunku** i **wykonawcy** klasy modeli, projekt zostanie skompilowany poprawnie przy użyciu nowe klasy modelu danych.</span><span class="sxs-lookup"><span data-stu-id="ef39b-213">In this task, you will check that, although the model generation have removed the **Album**, **Genre** and **Artist** model classes, the project builds successfully by using the new data model classes.</span></span>

1. <span data-ttu-id="ef39b-214">Skompiluj projekt, wybierając **kompilacji** element menu i następnie **kompilacji MvcMusicStore**.</span><span class="sxs-lookup"><span data-stu-id="ef39b-214">Build the project by selecting the **Build** menu item and then **Build MvcMusicStore**.</span></span>

    <span data-ttu-id="ef39b-215">![Tworzenie projektu](aspnet-mvc-4-models-and-data-access/_static/image13.png "kompilowania projektu")</span><span class="sxs-lookup"><span data-stu-id="ef39b-215">![Building the project](aspnet-mvc-4-models-and-data-access/_static/image13.png "Building the project")</span></span>

    <span data-ttu-id="ef39b-216">*Tworzenie projektu*</span><span class="sxs-lookup"><span data-stu-id="ef39b-216">*Building the project*</span></span>
2. <span data-ttu-id="ef39b-217">Projekt zostanie skompilowany poprawnie.</span><span class="sxs-lookup"><span data-stu-id="ef39b-217">The project builds successfully.</span></span> <span data-ttu-id="ef39b-218">Dlaczego to nadal działa?</span><span class="sxs-lookup"><span data-stu-id="ef39b-218">Why does it still work?</span></span> <span data-ttu-id="ef39b-219">To działa, ponieważ tabele bazy danych ma pola, które zawierają właściwości, które były używane w klasach usunięto **albumu** i **gatunku**.</span><span class="sxs-lookup"><span data-stu-id="ef39b-219">It works because the database tables have fields that include the properties that you were using in the removed classes **Album** and **Genre**.</span></span>

    <span data-ttu-id="ef39b-220">![Kompilacje, które zakończyło się pomyślnie](aspnet-mvc-4-models-and-data-access/_static/image14.png "kompilacji zakończyło się pomyślnie")</span><span class="sxs-lookup"><span data-stu-id="ef39b-220">![Builds succeeded](aspnet-mvc-4-models-and-data-access/_static/image14.png "Builds succeeded")</span></span>

    <span data-ttu-id="ef39b-221">*Kompilacje, które zakończyło się pomyślnie*</span><span class="sxs-lookup"><span data-stu-id="ef39b-221">*Builds succeeded*</span></span>
3. <span data-ttu-id="ef39b-222">Gdy Projektant wyświetla obiekty w formacie diagram, są one naprawdę klas języka C#.</span><span class="sxs-lookup"><span data-stu-id="ef39b-222">While the designer displays the entities in a diagram format, they are really C# classes.</span></span> <span data-ttu-id="ef39b-223">Rozwiń **StoreDB.edmx** węzła w Eksploratorze rozwiązań i następnie **StoreDB.tt**, zobaczysz nowe jednostki wygenerowany.</span><span class="sxs-lookup"><span data-stu-id="ef39b-223">Expand the **StoreDB.edmx** node in the Solution Explorer and then **StoreDB.tt**, you will see the new generated entities.</span></span>

    <span data-ttu-id="ef39b-224">![Wygenerowane pliki](aspnet-mvc-4-models-and-data-access/_static/image15.png "wygenerowanych plików")</span><span class="sxs-lookup"><span data-stu-id="ef39b-224">![Generated files](aspnet-mvc-4-models-and-data-access/_static/image15.png "Generated files")</span></span>

    <span data-ttu-id="ef39b-225">*Wygenerowane pliki*</span><span class="sxs-lookup"><span data-stu-id="ef39b-225">*Generated files*</span></span>

<a id="Ex1Task4"></a>

<a id="Task_4_-_Querying_the_Database"></a>
#### <a name="task-4---querying-the-database"></a><span data-ttu-id="ef39b-226">Zadanie 4 — zapytanie do bazy danych</span><span class="sxs-lookup"><span data-stu-id="ef39b-226">Task 4 - Querying the Database</span></span>

<span data-ttu-id="ef39b-227">W tym zadaniu zostanie zaktualizować klasy StoreController tak, aby zamiast korzystać z danych zapisane na stałe, zostanie wykonane zapytanie względem bazy danych, aby pobrać informacje.</span><span class="sxs-lookup"><span data-stu-id="ef39b-227">In this task, you will update the StoreController class so that, instead of using hardcoded data, it will query the database to retrieve the information.</span></span>

1. <span data-ttu-id="ef39b-228">Otwórz **Controllers\StoreController.cs** i dodaj następujące pola do klasy, aby pomieścić wystąpienie **MusicStoreEntities** klasę o nazwie **storeDB**:</span><span class="sxs-lookup"><span data-stu-id="ef39b-228">Open **Controllers\StoreController.cs** and add the following field to the class to hold an instance of the **MusicStoreEntities** class, named **storeDB**:</span></span>

    <span data-ttu-id="ef39b-229">(Code Snippet — *modele i dostęp do danych — Ex1 storeDB*)</span><span class="sxs-lookup"><span data-stu-id="ef39b-229">(Code Snippet - *Models And Data Access - Ex1 storeDB*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample1.cs)]
2. <span data-ttu-id="ef39b-230">**MusicStoreEntities** klasa udostępnia właściwości kolekcji dla każdej tabeli w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="ef39b-230">The **MusicStoreEntities** class exposes a collection property for each table in the database.</span></span> <span data-ttu-id="ef39b-231">Aktualizacja **Przeglądaj** metody akcji, które można pobrać określonego rodzaju ze wszystkimi **albumów**.</span><span class="sxs-lookup"><span data-stu-id="ef39b-231">Update **Browse** action method to retrieve a Genre with all of the **Albums**.</span></span>

    <span data-ttu-id="ef39b-232">(Code Snippet — *modele i dostęp do danych — Przeglądaj Store Ex1*)</span><span class="sxs-lookup"><span data-stu-id="ef39b-232">(Code Snippet - *Models And Data Access - Ex1 Store Browse*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample2.cs)]

    > [!NOTE]
    > <span data-ttu-id="ef39b-233">W przypadku korzystania z możliwości platformy .NET o nazwie **LINQ** (zapytanie o języku zintegrowanym) do pisania wyrażeń zapytań silnie typizowane względem te kolekcje — co spowoduje wykonanie kodu w bazie danych i zwracają obiekty, że można programować względem.</span><span class="sxs-lookup"><span data-stu-id="ef39b-233">You are using a capability of .NET called **LINQ** (language-integrated query) to write strongly-typed query expressions against these collections - which will execute code against the database and return objects that you can program against.</span></span>
    > 
    > <span data-ttu-id="ef39b-234">Aby uzyskać więcej informacji na temat LINQ, odwiedź [witryny msdn](https://msdn.microsoft.com/library/bb397926&amp;#040;v=vs.110&amp;#041;.aspx).</span><span class="sxs-lookup"><span data-stu-id="ef39b-234">For more information about LINQ, please visit the [msdn site](https://msdn.microsoft.com/library/bb397926&amp;#040;v=vs.110&amp;#041;.aspx).</span></span>
3. <span data-ttu-id="ef39b-235">Aktualizacja **indeksu** metody akcji, aby pobrać wszystkie gatunki.</span><span class="sxs-lookup"><span data-stu-id="ef39b-235">Update **Index** action method to retrieve all the genres.</span></span>

    <span data-ttu-id="ef39b-236">(Code Snippet — *modele i dostęp do danych — indeks Store Ex1*)</span><span class="sxs-lookup"><span data-stu-id="ef39b-236">(Code Snippet - *Models And Data Access - Ex1 Store Index*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample3.cs)]
4. <span data-ttu-id="ef39b-237">Aktualizacja **indeksu** metody akcji, aby pobrać wszystkie gatunki i Przekształć kolekcji na listę.</span><span class="sxs-lookup"><span data-stu-id="ef39b-237">Update **Index** action method to retrieve all the genres and transform the collection to a list.</span></span>

    <span data-ttu-id="ef39b-238">(Code Snippet — *modele i dostęp do danych — Ex1 Store GenreMenu*)</span><span class="sxs-lookup"><span data-stu-id="ef39b-238">(Code Snippet - *Models And Data Access - Ex1 Store GenreMenu*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample4.cs)]

<a id="Ex1Task5"></a>

<a id="Task_5_-_Running_the_Application"></a>
#### <a name="task-5---running-the-application"></a><span data-ttu-id="ef39b-239">Zadanie 5 działania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="ef39b-239">Task 5 - Running the Application</span></span>

<span data-ttu-id="ef39b-240">W ramach tego zadania będzie sprawdzać strony indeksu Store będą teraz wyświetlane gatunki przechowywanych w bazie danych, zamiast zakodowaną z nich.</span><span class="sxs-lookup"><span data-stu-id="ef39b-240">In this task, you will check that the Store Index page will now display the Genres stored in the database instead of the hardcoded ones.</span></span> <span data-ttu-id="ef39b-241">Nie ma potrzeby zmiany szablon widoku, ponieważ **StoreController** zwraca tych samych jednostek tak jak poprzednio, ale tym razem dane będą pochodzić z bazy danych.</span><span class="sxs-lookup"><span data-stu-id="ef39b-241">There is no need to change the View template because the **StoreController** is returning the same entities as before, although this time the data will come from the database.</span></span>

1. <span data-ttu-id="ef39b-242">Ponownie skompiluj rozwiązanie, a następnie naciśnij klawisz **F5** do uruchomienia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="ef39b-242">Rebuild the solution and press **F5** to run the Application.</span></span>
2. <span data-ttu-id="ef39b-243">Data rozpoczęcia projektu na stronie głównej.</span><span class="sxs-lookup"><span data-stu-id="ef39b-243">The project starts in the Home page.</span></span> <span data-ttu-id="ef39b-244">Upewnij się, że menu **gatunki** nie jest już listę zapisane na stałe, a dane bezpośrednio są pobierane z bazy danych.</span><span class="sxs-lookup"><span data-stu-id="ef39b-244">Verify that the menu of **Genres** is no longer a hardcoded list, and the data is directly retrieved from the database.</span></span>

    ![BrowsingGenresFromDataBase](aspnet-mvc-4-models-and-data-access/_static/image16.png)

    <span data-ttu-id="ef39b-246">*Przeglądanie gatunki z bazy danych*</span><span class="sxs-lookup"><span data-stu-id="ef39b-246">*Browsing Genres from the database*</span></span>
3. <span data-ttu-id="ef39b-247">Teraz przejdź do dowolnego rodzaju i sprawdź, czy albumów zostają wypełnione z bazy danych.</span><span class="sxs-lookup"><span data-stu-id="ef39b-247">Now browse to any genre and verify the albums are populated from database.</span></span>

    <span data-ttu-id="ef39b-248">![Przeglądanie albumów z bazy danych](aspnet-mvc-4-models-and-data-access/_static/image17.png "przeglądania albumów z bazy danych")</span><span class="sxs-lookup"><span data-stu-id="ef39b-248">![Browsing Albums from the database](aspnet-mvc-4-models-and-data-access/_static/image17.png "Browsing Albums from the database")</span></span>

    <span data-ttu-id="ef39b-249">*Przeglądanie albumów z bazy danych*</span><span class="sxs-lookup"><span data-stu-id="ef39b-249">*Browsing Albums from the database*</span></span>

<a id="Exercise2"></a>

<a id="Exercise_2_Creating_a_Database_Using_Code_First"></a>
### <a name="exercise-2-creating-a-database-using-code-first"></a><span data-ttu-id="ef39b-250">Ćwiczenie 2: Tworzenie bazy danych przy użyciu kodu najpierw</span><span class="sxs-lookup"><span data-stu-id="ef39b-250">Exercise 2: Creating a Database Using Code First</span></span>

<span data-ttu-id="ef39b-251">W tym ćwiczeniu dowiesz się, jak utworzyć bazę danych z tabelami aplikacji MusicStore za pomocą podejścia Code First i jak uzyskać dostęp do swoich danych.</span><span class="sxs-lookup"><span data-stu-id="ef39b-251">In this exercise, you will learn how to use the Code First approach to create a database with the tables of the MusicStore application, and how to access its data.</span></span>

<span data-ttu-id="ef39b-252">Po wygenerowaniu modelu zmodyfikujesz StoreController, aby zapewnić wyświetlanie szablonu z danymi z bazy danych, a nie przy użyciu wartości zapisane na stałe.</span><span class="sxs-lookup"><span data-stu-id="ef39b-252">Once the model is generated, you will modify the StoreController to provide the View template with the data taken from the database, instead of using hardcoded values.</span></span>

> [!NOTE]
> <span data-ttu-id="ef39b-253">Jeśli ukończono wykonywanie 1 i już mający doświadczenie z podejścia Database First, będzie teraz dowiesz się, jak uzyskać te same wyniki za pomocą innego procesu.</span><span class="sxs-lookup"><span data-stu-id="ef39b-253">If you have completed Exercise 1 and have already worked with the Database First approach, you will now learn how to get the same results with a different process.</span></span> <span data-ttu-id="ef39b-254">Aby ułatwić odczytywanie Twojego zostały oznaczone zadania, które są wspólne dla ćwiczenie 1.</span><span class="sxs-lookup"><span data-stu-id="ef39b-254">The tasks that are in common with Exercise 1 have been marked to make your reading easier.</span></span> <span data-ttu-id="ef39b-255">Jeśli nie zostały wypełnione wykonywania 1, ale chcesz dowiedzieć się podejścia Code First, możesz rozpocząć od tego ćwiczenia i uzyskać pełne tego tematu.</span><span class="sxs-lookup"><span data-stu-id="ef39b-255">If you have not completed Exercise 1 but would like to learn the Code First approach, you can start from this exercise and get a full coverage of the topic.</span></span>


<a id="Ex2Task1"></a>

<a id="Task_1_-_Populating_Sample_Data"></a>
#### <a name="task-1---populating-sample-data"></a><span data-ttu-id="ef39b-256">Zadanie 1 — wypełniania przykładowych danych</span><span class="sxs-lookup"><span data-stu-id="ef39b-256">Task 1 - Populating Sample Data</span></span>

<span data-ttu-id="ef39b-257">W ramach tego zadania spowoduje wypełnienie bazy danych z przykładowymi danymi tworzonemu zasobowi początkowemu przy użyciu najpierw kod.</span><span class="sxs-lookup"><span data-stu-id="ef39b-257">In this task, you will populate the database with sample data when it is intially created using Code-First.</span></span>

1. <span data-ttu-id="ef39b-258">Otwórz **rozpocząć** rozwiązania znajdujący się w **źródło/Ex2-CreatingADatabaseCodeFirst/rozpoczęcia/** folderu.</span><span class="sxs-lookup"><span data-stu-id="ef39b-258">Open the **Begin** solution located at **Source/Ex2-CreatingADatabaseCodeFirst/Begin/** folder.</span></span> <span data-ttu-id="ef39b-259">W przeciwnym razie będziesz nadal korzystać z **zakończenia** rozwiązania uzyskać, wykonując poprzednim ćwiczeniu.</span><span class="sxs-lookup"><span data-stu-id="ef39b-259">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

   1. <span data-ttu-id="ef39b-260">Jeśli został otwarty dołączonym **rozpocząć** rozwiązania, należy pobrać niektóre brakujące pakiety NuGet przed kontynuowaniem.</span><span class="sxs-lookup"><span data-stu-id="ef39b-260">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="ef39b-261">Aby to zrobić, kliknij przycisk **projektu** menu, a następnie wybierz **Zarządzaj pakietami NuGet**.</span><span class="sxs-lookup"><span data-stu-id="ef39b-261">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="ef39b-262">W **Zarządzaj pakietami NuGet** okno dialogowe, kliknij przycisk **przywrócić** Aby pobrać brakujące pakiety.</span><span class="sxs-lookup"><span data-stu-id="ef39b-262">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="ef39b-263">Na koniec Skompiluj rozwiązanie, klikając **kompilacji** | **Kompiluj rozwiązanie**.</span><span class="sxs-lookup"><span data-stu-id="ef39b-263">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="ef39b-264">Jedną z zalet za pomocą narzędzia NuGet jest, że nie masz do wysłania wszystkie biblioteki w projekcie, zmniejszenie rozmiaru projektu.</span><span class="sxs-lookup"><span data-stu-id="ef39b-264">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="ef39b-265">Za pomocą narzędzi NuGet Power Tools, określając wersji pakietu w pliku Packages.config można pobrać wymaganych bibliotek podczas pierwszego uruchomienia projektu.</span><span class="sxs-lookup"><span data-stu-id="ef39b-265">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="ef39b-266">Jest to, dlaczego należy uruchomić następujące kroki, po otwarciu istniejącego rozwiązania, w tym środowisku laboratoryjnym.</span><span class="sxs-lookup"><span data-stu-id="ef39b-266">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="ef39b-267">Dodaj **SampleData.cs** plik **modeli** folderu.</span><span class="sxs-lookup"><span data-stu-id="ef39b-267">Add the **SampleData.cs** file to the **Models** folder.</span></span> <span data-ttu-id="ef39b-268">Aby to zrobić, kliknij prawym przyciskiem myszy **modeli** folderu, wskaż **Dodaj** a następnie kliknij przycisk **istniejący element**.</span><span class="sxs-lookup"><span data-stu-id="ef39b-268">To do that, right-click **Models** folder, point to **Add** and then click **Existing Item**.</span></span> <span data-ttu-id="ef39b-269">Przejdź do **\Source\Assets** i wybierz **SampleData.cs** pliku.</span><span class="sxs-lookup"><span data-stu-id="ef39b-269">Browse to **\Source\Assets** and select the **SampleData.cs** file.</span></span>

    <span data-ttu-id="ef39b-270">![Przykładowe dane Wypełnij kodu](aspnet-mvc-4-models-and-data-access/_static/image18.png "przykładowych danych wypełnienia kodem")</span><span class="sxs-lookup"><span data-stu-id="ef39b-270">![Sample data populate code](aspnet-mvc-4-models-and-data-access/_static/image18.png "Sample data populate code")</span></span>

    <span data-ttu-id="ef39b-271">*Przykładowe dane Wypełnij kodu*</span><span class="sxs-lookup"><span data-stu-id="ef39b-271">*Sample data populate code*</span></span>
3. <span data-ttu-id="ef39b-272">Otwórz **Global.asax.cs** pliku i Dodaj następujący kod *przy użyciu* instrukcji.</span><span class="sxs-lookup"><span data-stu-id="ef39b-272">Open the **Global.asax.cs** file and add the following *using* statements.</span></span>

    <span data-ttu-id="ef39b-273">(Code Snippet — *modele i dostęp do danych — deklaracje Using Ex2 globalnego Asax*)</span><span class="sxs-lookup"><span data-stu-id="ef39b-273">(Code Snippet - *Models And Data Access - Ex2 Global Asax Usings*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample5.cs)]
4. <span data-ttu-id="ef39b-274">W **aplikacji\_Start()** metoda Dodaj poniższy wiersz, aby ustawić inicjatora bazy danych.</span><span class="sxs-lookup"><span data-stu-id="ef39b-274">In the **Application\_Start()** method add the following line to set the database initializer.</span></span>

    <span data-ttu-id="ef39b-275">(Code Snippet — *modele i dostęp do danych — globalne Asax SetInitializer Ex2*)</span><span class="sxs-lookup"><span data-stu-id="ef39b-275">(Code Snippet - *Models And Data Access - Ex2 Global Asax SetInitializer*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample6.cs)]

<a id="Ex2Task2"></a>

<a id="Task_2_-_Configuring_the_connection_to_the_Database"></a>
#### <a name="task-2---configuring-the-connection-to-the-database"></a><span data-ttu-id="ef39b-276">Zadanie 2 — Konfigurowanie połączenia z bazą danych</span><span class="sxs-lookup"><span data-stu-id="ef39b-276">Task 2 - Configuring the connection to the Database</span></span>

<span data-ttu-id="ef39b-277">Skoro już dodano bazę danych do naszego projektu, trzeba napisać **Web.config** pliku parametrów połączenia.</span><span class="sxs-lookup"><span data-stu-id="ef39b-277">Now that you have already added a database to our project, you will write in the **Web.config** file the connection string.</span></span>

1. <span data-ttu-id="ef39b-278">Dodaj parametry połączenia w **Web.config**. Aby to zrobić, otwórz **Web.config** w katalogu głównym projektu i Zastąp parametry połączenia o nazwie DefaultConnection ten wiersz w **&lt;connectionStrings&gt;** sekcji:</span><span class="sxs-lookup"><span data-stu-id="ef39b-278">Add a connection string at **Web.config**. To do that, open **Web.config** at project root and replace the connection string named DefaultConnection with this line in the **&lt;connectionStrings&gt;** section:</span></span>

    <span data-ttu-id="ef39b-279">![Lokalizacja pliku Web.config](aspnet-mvc-4-models-and-data-access/_static/image19.png "lokalizacji pliku Web.config")</span><span class="sxs-lookup"><span data-stu-id="ef39b-279">![Web.config file location](aspnet-mvc-4-models-and-data-access/_static/image19.png "Web.config file location")</span></span>

    <span data-ttu-id="ef39b-280">*Lokalizacja pliku Web.config*</span><span class="sxs-lookup"><span data-stu-id="ef39b-280">*Web.config file location*</span></span>

    [!code-xml[Main](aspnet-mvc-4-models-and-data-access/samples/sample7.xml)]

<a id="Ex2Task3"></a>

<a id="Task_3_-_Working_with_the_Model"></a>
#### <a name="task-3---working-with-the-model"></a><span data-ttu-id="ef39b-281">Zadanie 3 — Praca z modelem</span><span class="sxs-lookup"><span data-stu-id="ef39b-281">Task 3 - Working with the Model</span></span>

<span data-ttu-id="ef39b-282">Teraz, gdy skonfigurowano już połączenie z bazą danych, połączysz modelu z tabel bazy danych.</span><span class="sxs-lookup"><span data-stu-id="ef39b-282">Now that you have already configured the connection to the database, you will link the model with the database tables.</span></span> <span data-ttu-id="ef39b-283">W tym zadaniu utworzysz klasy, które zostanie połączone z bazą danych przy użyciu Code First.</span><span class="sxs-lookup"><span data-stu-id="ef39b-283">In this task, you will create a class that will be linked to the database with Code First.</span></span> <span data-ttu-id="ef39b-284">Należy pamiętać, że jest celowe klasy modelu POCO powinien być modyfikowany.</span><span class="sxs-lookup"><span data-stu-id="ef39b-284">Remember that there is an existent POCO model class that should be modified.</span></span>

> [!NOTE]
> <span data-ttu-id="ef39b-285">Jeśli ukończono wykonywanie 1 można zauważyć, że ten krok został wykonany przez kreatora.</span><span class="sxs-lookup"><span data-stu-id="ef39b-285">If you completed Exercise 1, you will note that this step was performed by a wizard.</span></span> <span data-ttu-id="ef39b-286">Wykonanie tych czynności Code First, ręcznie utworzysz klas, które zostaną połączone z jednostkami danych.</span><span class="sxs-lookup"><span data-stu-id="ef39b-286">By doing Code First, you will manually create classes that will be linked to data entities.</span></span>

1. <span data-ttu-id="ef39b-287">Otwórz klasę modelu POCO **gatunku** z **modeli** folderu projektu i zawierać tego identyfikatora.</span><span class="sxs-lookup"><span data-stu-id="ef39b-287">Open the POCO model class **Genre** from **Models** project folder and include an ID.</span></span> <span data-ttu-id="ef39b-288">Użyj właściwości int o nazwie **GenreId**.</span><span class="sxs-lookup"><span data-stu-id="ef39b-288">Use an int property with the name **GenreId**.</span></span>

    <span data-ttu-id="ef39b-289">(Code Snippet — *modele i dostęp do danych — gatunku pierwszy kod Ex2*)</span><span class="sxs-lookup"><span data-stu-id="ef39b-289">(Code Snippet - *Models And Data Access - Ex2 Code First Genre*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample8.cs)]

    > [!NOTE]
    > <span data-ttu-id="ef39b-290">Aby pracować z Konwencji Code First, klasa gatunku musi mieć właściwość klucza podstawowego, który zostanie wykryty automatycznie.</span><span class="sxs-lookup"><span data-stu-id="ef39b-290">To work with Code First conventions, the class Genre must have a primary key property that will be automatically detected.</span></span>
    > 
    > <span data-ttu-id="ef39b-291">Więcej informacji dotyczących Konwencji pierwsze kodu, w tym [artykuł w witrynie msdn](https://msdn.microsoft.com/library/hh161541&amp;#040;v=vs.103&amp;#041;.aspx).</span><span class="sxs-lookup"><span data-stu-id="ef39b-291">You can read more about Code First Conventions in this [msdn article](https://msdn.microsoft.com/library/hh161541&amp;#040;v=vs.103&amp;#041;.aspx).</span></span>
2. <span data-ttu-id="ef39b-292">Teraz Otwórz klasę modelu POCO **albumu** z **modeli** folderu projektu i zawierają kluczy obcych, tworzenie właściwości z nazwami **GenreId** i  **ArtistId**.</span><span class="sxs-lookup"><span data-stu-id="ef39b-292">Now, open the POCO model class **Album** from **Models** project folder and include the foreign keys, create properties with the names **GenreId** and **ArtistId**.</span></span> <span data-ttu-id="ef39b-293">Ta klasa jeszcze **GenreId** dla klucza podstawowego.</span><span class="sxs-lookup"><span data-stu-id="ef39b-293">This class already have the **GenreId** for the primary key.</span></span>

    <span data-ttu-id="ef39b-294">(Code Snippet — *modele i dostęp do danych — Ex2 kodu pierwszego albumu*)</span><span class="sxs-lookup"><span data-stu-id="ef39b-294">(Code Snippet - *Models And Data Access - Ex2 Code First Album*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample9.cs)]
3. <span data-ttu-id="ef39b-295">Otwórz klasę modelu POCO **wykonawcy** i obejmują **ArtistId** właściwości.</span><span class="sxs-lookup"><span data-stu-id="ef39b-295">Open the POCO model class **Artist** and include the **ArtistId** property.</span></span>

    <span data-ttu-id="ef39b-296">(Code Snippet — *modele i dostęp do danych — wykonawcy pierwszy kod Ex2*)</span><span class="sxs-lookup"><span data-stu-id="ef39b-296">(Code Snippet - *Models And Data Access - Ex2 Code First Artist*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample10.cs)]
4. <span data-ttu-id="ef39b-297">Kliknij prawym przyciskiem myszy **modeli** folderu projektu i wybierz pozycję **Dodaj | Klasa**.</span><span class="sxs-lookup"><span data-stu-id="ef39b-297">Right-click the **Models** project folder and select **Add | Class**.</span></span> <span data-ttu-id="ef39b-298">Nadaj plikowi nazwę **MusicStoreEntities.cs**.</span><span class="sxs-lookup"><span data-stu-id="ef39b-298">Name the file **MusicStoreEntities.cs**.</span></span> <span data-ttu-id="ef39b-299">Następnie kliknij przycisk **Dodaj.**</span><span class="sxs-lookup"><span data-stu-id="ef39b-299">Then, click **Add.**</span></span>

    <span data-ttu-id="ef39b-300">![Dodawanie klasy](aspnet-mvc-4-models-and-data-access/_static/image20.png "Dodawanie klasy")</span><span class="sxs-lookup"><span data-stu-id="ef39b-300">![Adding a class](aspnet-mvc-4-models-and-data-access/_static/image20.png "Adding a class")</span></span>

    <span data-ttu-id="ef39b-301">*Dodawanie nowego elementu*</span><span class="sxs-lookup"><span data-stu-id="ef39b-301">*Adding a new item*</span></span>

    <span data-ttu-id="ef39b-302">![Dodawanie klasa2](aspnet-mvc-4-models-and-data-access/_static/image21.png "Dodawanie klasa2")</span><span class="sxs-lookup"><span data-stu-id="ef39b-302">![Adding a class2](aspnet-mvc-4-models-and-data-access/_static/image21.png "Adding a class2")</span></span>

    <span data-ttu-id="ef39b-303">*Dodawanie klasy*</span><span class="sxs-lookup"><span data-stu-id="ef39b-303">*Adding a class*</span></span>
5. <span data-ttu-id="ef39b-304">Otwórz klasę właśnie utworzony, **MusicStoreEntities.cs**i zawierać przestrzenie nazw **System.Data.Entity** i **System.Data.Entity.Infrastructure**.</span><span class="sxs-lookup"><span data-stu-id="ef39b-304">Open the class you have just created, **MusicStoreEntities.cs**, and include the namespaces **System.Data.Entity** and **System.Data.Entity.Infrastructure**.</span></span>

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample11.cs)]
6. <span data-ttu-id="ef39b-305">Zastąp deklaracji klasy, aby rozszerzyć **DbContext** klasy: deklarowanie publiczny **DBSet** i zastąpić **OnModelCreating** metody.</span><span class="sxs-lookup"><span data-stu-id="ef39b-305">Replace the class declaration to extend the **DbContext** class: declare a public **DBSet** and override **OnModelCreating** method.</span></span> <span data-ttu-id="ef39b-306">Po wykonaniu tego kroku wystąpi klasę domeny, która połączy modelu za pomocą programu Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="ef39b-306">After this step you will get a domain class that will link your model with the Entity Framework.</span></span> <span data-ttu-id="ef39b-307">Aby to zrobić, Zastąp kod klasy następujących czynności:</span><span class="sxs-lookup"><span data-stu-id="ef39b-307">In order to do that, replace the class code with the following:</span></span>

    <span data-ttu-id="ef39b-308">(Code Snippet — *modele i dostęp do danych — MusicStoreEntities pierwszy kod Ex2*)</span><span class="sxs-lookup"><span data-stu-id="ef39b-308">(Code Snippet - *Models And Data Access - Ex2 Code First MusicStoreEntities*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample12.cs)]

> [!NOTE]
> <span data-ttu-id="ef39b-309">Za pomocą platformy Entity Framework **DbContext** i **DBSet** można zbadać klasy POCO gatunku.</span><span class="sxs-lookup"><span data-stu-id="ef39b-309">With Entity Framework **DbContext** and **DBSet** you will be able to query the POCO class Genre.</span></span> <span data-ttu-id="ef39b-310">Rozszerzając **OnModelCreating** metody, określasz w **kodu** odwzorowania gatunku do tabeli bazy danych.</span><span class="sxs-lookup"><span data-stu-id="ef39b-310">By extending **OnModelCreating** method, you are specifying in the **code** how Genre will be mapped to a database table.</span></span> <span data-ttu-id="ef39b-311">Więcej informacji na temat typu DBContext i DBSet można znaleźć w artykule msdn: [łącza](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=vs.103).aspx)</span><span class="sxs-lookup"><span data-stu-id="ef39b-311">You can find more information about DBContext and DBSet in this msdn article: [link](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=vs.103).aspx)</span></span>

<a id="Ex2Task4"></a>

<a id="Task_4_-_Querying_the_Database"></a>
#### <a name="task-4---querying-the-database"></a><span data-ttu-id="ef39b-312">Zadanie 4 — zapytanie do bazy danych</span><span class="sxs-lookup"><span data-stu-id="ef39b-312">Task 4 - Querying the Database</span></span>

<span data-ttu-id="ef39b-313">W tym zadaniu zostanie zaktualizować klasy StoreController, tak, aby zamiast korzystać z danych zapisane na stałe, pobierze go z bazy danych.</span><span class="sxs-lookup"><span data-stu-id="ef39b-313">In this task, you will update the StoreController class so that, instead of using hardcoded data, it will retrieve it from the database.</span></span>

> [!NOTE]
> <span data-ttu-id="ef39b-314">To zadanie jest wspólnych ćwiczenie 1.</span><span class="sxs-lookup"><span data-stu-id="ef39b-314">This task is in common with Exercise 1.</span></span>
> 
> <span data-ttu-id="ef39b-315">Jeśli wykonano 1 ćwiczenia można zauważyć te kroki są takie same w obu metod (najpierw bazy danych lub najpierw Code).</span><span class="sxs-lookup"><span data-stu-id="ef39b-315">If you completed Exercise 1 you will note these steps are the same in both approaches (Database first or Code first).</span></span> <span data-ttu-id="ef39b-316">Są one różne, w jaki sposób dane są połączone za pomocą modelu, ale dostęp do jednostek danych jest jeszcze przezroczyste z kontrolera.</span><span class="sxs-lookup"><span data-stu-id="ef39b-316">They are different in how the data is linked with the model, but the access to data entities is yet transparent from the controller.</span></span>


1. <span data-ttu-id="ef39b-317">Otwórz **Controllers\StoreController.cs** i dodaj następujące pola do klasy, aby pomieścić wystąpienie **MusicStoreEntities** klasę o nazwie **storeDB**:</span><span class="sxs-lookup"><span data-stu-id="ef39b-317">Open **Controllers\StoreController.cs** and add the following field to the class to hold an instance of the **MusicStoreEntities** class, named **storeDB**:</span></span>

    <span data-ttu-id="ef39b-318">(Code Snippet — *modele i dostęp do danych — Ex1 storeDB*)</span><span class="sxs-lookup"><span data-stu-id="ef39b-318">(Code Snippet - *Models And Data Access - Ex1 storeDB*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample13.cs)]
2. <span data-ttu-id="ef39b-319">**MusicStoreEntities** klasa udostępnia właściwości kolekcji dla każdej tabeli w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="ef39b-319">The **MusicStoreEntities** class exposes a collection property for each table in the database.</span></span> <span data-ttu-id="ef39b-320">Aktualizacja **Przeglądaj** metody akcji, które można pobrać określonego rodzaju ze wszystkimi **albumów**.</span><span class="sxs-lookup"><span data-stu-id="ef39b-320">Update **Browse** action method to retrieve a Genre with all of the **Albums**.</span></span>

    <span data-ttu-id="ef39b-321">(Code Snippet — *modele i dostęp do danych — Przeglądaj Store Ex2*)</span><span class="sxs-lookup"><span data-stu-id="ef39b-321">(Code Snippet - *Models And Data Access - Ex2 Store Browse*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample14.cs)]

    > [!NOTE]
    > <span data-ttu-id="ef39b-322">W przypadku korzystania z możliwości platformy .NET o nazwie **LINQ** (zapytanie o języku zintegrowanym) do pisania wyrażeń zapytań silnie typizowane względem te kolekcje — co spowoduje wykonanie kodu w bazie danych i zwracają obiekty, że można programować względem.</span><span class="sxs-lookup"><span data-stu-id="ef39b-322">You are using a capability of .NET called **LINQ** (language-integrated query) to write strongly-typed query expressions against these collections - which will execute code against the database and return objects that you can program against.</span></span>
    > 
    > <span data-ttu-id="ef39b-323">Aby uzyskać więcej informacji na temat LINQ, odwiedź [witryny msdn](https://msdn.microsoft.com/library/bb397926(v=vs.110).aspx).</span><span class="sxs-lookup"><span data-stu-id="ef39b-323">For more information about LINQ, please visit the [msdn site](https://msdn.microsoft.com/library/bb397926(v=vs.110).aspx).</span></span>
3. <span data-ttu-id="ef39b-324">Aktualizacja **indeksu** metody akcji, aby pobrać wszystkie gatunki.</span><span class="sxs-lookup"><span data-stu-id="ef39b-324">Update **Index** action method to retrieve all the genres.</span></span>

    <span data-ttu-id="ef39b-325">(Code Snippet — *modele i dostęp do danych — indeks Store Ex2*)</span><span class="sxs-lookup"><span data-stu-id="ef39b-325">(Code Snippet - *Models And Data Access - Ex2 Store Index*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample15.cs)]
4. <span data-ttu-id="ef39b-326">Aktualizacja **indeksu** metody akcji, aby pobrać wszystkie gatunki i Przekształć kolekcji na listę.</span><span class="sxs-lookup"><span data-stu-id="ef39b-326">Update **Index** action method to retrieve all the genres and transform the collection to a list.</span></span>

    <span data-ttu-id="ef39b-327">(Code Snippet — *modele i dostęp do danych — Ex2 Store GenreMenu*)</span><span class="sxs-lookup"><span data-stu-id="ef39b-327">(Code Snippet - *Models And Data Access - Ex2 Store GenreMenu*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample16.cs)]

<a id="Ex2Task5"></a>

<a id="Task_5_-_Running_the_Application"></a>
#### <a name="task-5---running-the-application"></a><span data-ttu-id="ef39b-328">Zadanie 5 działania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="ef39b-328">Task 5 - Running the Application</span></span>

<span data-ttu-id="ef39b-329">W ramach tego zadania będzie sprawdzać strony indeksu Store będą teraz wyświetlane gatunki przechowywanych w bazie danych, zamiast zakodowaną z nich.</span><span class="sxs-lookup"><span data-stu-id="ef39b-329">In this task, you will check that the Store Index page will now display the Genres stored in the database instead of the hardcoded ones.</span></span> <span data-ttu-id="ef39b-330">Nie ma potrzeby zmiany szablon widoku, ponieważ **StoreController** zwraca takie same **StoreIndexViewModel** tak jak poprzednio, ale tym razem, dane będą pochodzić z bazy danych.</span><span class="sxs-lookup"><span data-stu-id="ef39b-330">There is no need to change the View template because the **StoreController** is returning the same **StoreIndexViewModel** as before, but this time the data will come from the database.</span></span>

1. <span data-ttu-id="ef39b-331">Ponownie skompiluj rozwiązanie, a następnie naciśnij klawisz **F5** do uruchomienia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="ef39b-331">Rebuild the solution and press **F5** to run the Application.</span></span>
2. <span data-ttu-id="ef39b-332">Data rozpoczęcia projektu na stronie głównej.</span><span class="sxs-lookup"><span data-stu-id="ef39b-332">The project starts in the Home page.</span></span> <span data-ttu-id="ef39b-333">Upewnij się, że menu **gatunki** nie jest już listę zapisane na stałe, a dane bezpośrednio są pobierane z bazy danych.</span><span class="sxs-lookup"><span data-stu-id="ef39b-333">Verify that the menu of **Genres** is no longer a hardcoded list, and the data is directly retrieved from the database.</span></span>

    ![BrowsingGenresFromDataBase](aspnet-mvc-4-models-and-data-access/_static/image22.png)

    <span data-ttu-id="ef39b-335">*Przeglądanie gatunki z bazy danych*</span><span class="sxs-lookup"><span data-stu-id="ef39b-335">*Browsing Genres from the database*</span></span>
3. <span data-ttu-id="ef39b-336">Teraz przejdź do dowolnego rodzaju i sprawdź, czy albumów zostają wypełnione z bazy danych.</span><span class="sxs-lookup"><span data-stu-id="ef39b-336">Now browse to any genre and verify the albums are populated from database.</span></span>

    <span data-ttu-id="ef39b-337">![Przeglądanie albumów z bazy danych](aspnet-mvc-4-models-and-data-access/_static/image23.png "przeglądania albumów z bazy danych")</span><span class="sxs-lookup"><span data-stu-id="ef39b-337">![Browsing Albums from the database](aspnet-mvc-4-models-and-data-access/_static/image23.png "Browsing Albums from the database")</span></span>

    <span data-ttu-id="ef39b-338">*Przeglądanie albumów z bazy danych*</span><span class="sxs-lookup"><span data-stu-id="ef39b-338">*Browsing Albums from the database*</span></span>

<a id="Exercise3"></a>

<a id="Exercise_3_Querying_the_Database_with_Parameters"></a>
### <a name="exercise-3-querying-the-database-with-parameters"></a><span data-ttu-id="ef39b-339">Ćwiczenie 3: Zapytanie do bazy danych z parametrami</span><span class="sxs-lookup"><span data-stu-id="ef39b-339">Exercise 3: Querying the Database with Parameters</span></span>

<span data-ttu-id="ef39b-340">W tym ćwiczeniu dowiesz się, jak wykonać zapytanie bazy danych przy użyciu parametrów i sposobu używania kształtowania wyników zapytania, funkcja, która zmniejsza się liczba bazy danych uzyskuje dostęp do pobierania danych w bardziej efektywny sposób.</span><span class="sxs-lookup"><span data-stu-id="ef39b-340">In this exercise, you will learn how to query the database using parameters, and how to use Query Result Shaping, a feature that reduces the number database accesses retrieving data in a more efficient way.</span></span>

> [!NOTE]
> <span data-ttu-id="ef39b-341">Więcej informacji na temat kształtowania wyników kwerendy, można znaleźć w następującej [artykuł w witrynie msdn](https://msdn.microsoft.com/library/bb896272&amp;#040;v=vs.100&amp;#041;.aspx).</span><span class="sxs-lookup"><span data-stu-id="ef39b-341">For further information on Query Result Shaping, visit the following [msdn article](https://msdn.microsoft.com/library/bb896272&amp;#040;v=vs.100&amp;#041;.aspx).</span></span>

<a id="Ex3Task1"></a>

<a id="Task_1_-_Modifying_StoreController_to_Retrieve_Albums_from_Database"></a>
#### <a name="task-1---modifying-storecontroller-to-retrieve-albums-from-database"></a><span data-ttu-id="ef39b-342">Zadanie 1 — modyfikowanie StoreController pobrać albumów z bazy danych</span><span class="sxs-lookup"><span data-stu-id="ef39b-342">Task 1 - Modifying StoreController to Retrieve Albums from Database</span></span>

<span data-ttu-id="ef39b-343">W ramach tego zadania zostanie zmieniony **StoreController** klasy dostępu do bazy danych można pobrać ze zdjęciami z określonego rodzaju.</span><span class="sxs-lookup"><span data-stu-id="ef39b-343">In this task, you will change the **StoreController** class to access the database to retrieve albums from a specific genre.</span></span>

1. <span data-ttu-id="ef39b-344">Otwórz **rozpocząć** rozwiązania znajdujący się w **Source\Ex3 QueryingTheDatabaseWithParametersCodeFirst\Begin** folderu, jeśli chcesz użyć podejścia najpierw kod lub **Source\ Ex3 QueryingTheDatabaseWithParametersDBFirst\Begin** folderu, jeśli chcesz użyć podejścia bazy danych.</span><span class="sxs-lookup"><span data-stu-id="ef39b-344">Open the **Begin** solution located at the **Source\Ex3-QueryingTheDatabaseWithParametersCodeFirst\Begin** folder if you want to use Code-First approach or **Source\Ex3-QueryingTheDatabaseWithParametersDBFirst\Begin** folder if you want to use Database-First approach.</span></span> <span data-ttu-id="ef39b-345">W przeciwnym razie będziesz nadal korzystać z **zakończenia** rozwiązania uzyskać, wykonując poprzednim ćwiczeniu.</span><span class="sxs-lookup"><span data-stu-id="ef39b-345">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

   1. <span data-ttu-id="ef39b-346">Jeśli został otwarty dołączonym **rozpocząć** rozwiązania, należy pobrać niektóre brakujące pakiety NuGet przed kontynuowaniem.</span><span class="sxs-lookup"><span data-stu-id="ef39b-346">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="ef39b-347">Aby to zrobić, kliknij przycisk **projektu** menu, a następnie wybierz **Zarządzaj pakietami NuGet**.</span><span class="sxs-lookup"><span data-stu-id="ef39b-347">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="ef39b-348">W **Zarządzaj pakietami NuGet** okno dialogowe, kliknij przycisk **przywrócić** Aby pobrać brakujące pakiety.</span><span class="sxs-lookup"><span data-stu-id="ef39b-348">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="ef39b-349">Na koniec Skompiluj rozwiązanie, klikając **kompilacji** | **Kompiluj rozwiązanie**.</span><span class="sxs-lookup"><span data-stu-id="ef39b-349">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="ef39b-350">Jedną z zalet za pomocą narzędzia NuGet jest, że nie masz do wysłania wszystkie biblioteki w projekcie, zmniejszenie rozmiaru projektu.</span><span class="sxs-lookup"><span data-stu-id="ef39b-350">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="ef39b-351">Za pomocą narzędzi NuGet Power Tools, określając wersji pakietu w pliku Packages.config można pobrać wymaganych bibliotek podczas pierwszego uruchomienia projektu.</span><span class="sxs-lookup"><span data-stu-id="ef39b-351">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="ef39b-352">Jest to, dlaczego należy uruchomić następujące kroki, po otwarciu istniejącego rozwiązania, w tym środowisku laboratoryjnym.</span><span class="sxs-lookup"><span data-stu-id="ef39b-352">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="ef39b-353">Otwórz **StoreController** klasy, aby zmienić **Przeglądaj** metody akcji.</span><span class="sxs-lookup"><span data-stu-id="ef39b-353">Open the **StoreController** class to change the **Browse** action method.</span></span> <span data-ttu-id="ef39b-354">Aby to zrobić, w **Eksploratora rozwiązań**, rozwiń węzeł **kontrolerów** folder i kliknij dwukrotnie plik **StoreController.cs**.</span><span class="sxs-lookup"><span data-stu-id="ef39b-354">To do this, in the **Solution Explorer**, expand the **Controllers** folder and double-click **StoreController.cs**.</span></span>
3. <span data-ttu-id="ef39b-355">Zmiana **Przeglądaj** metody akcji, które można pobrać albumów dla określonego rodzaju.</span><span class="sxs-lookup"><span data-stu-id="ef39b-355">Change the **Browse** action method to retrieve albums for a specific genre.</span></span> <span data-ttu-id="ef39b-356">Aby to zrobić, Zastąp następujący kod:</span><span class="sxs-lookup"><span data-stu-id="ef39b-356">To do this, replace the following code:</span></span>

    <span data-ttu-id="ef39b-357">(Code Snippet — *modele i dostęp do danych — Ex3 StoreController BrowseMethod*)</span><span class="sxs-lookup"><span data-stu-id="ef39b-357">(Code Snippet - *Models And Data Access - Ex3 StoreController BrowseMethod*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample17.cs)]

> [!NOTE]
> <span data-ttu-id="ef39b-358">Aby wypełnić kolekcji jednostki, musisz użyć **Include** metodę, aby określić, ma zostać pobrane za albumów.</span><span class="sxs-lookup"><span data-stu-id="ef39b-358">To populate a collection of the entity, you need to use the **Include** method to specify you want to retrieve the albums too.</span></span> <span data-ttu-id="ef39b-359">Można użyć. **Funkcja Single()** rozszerzenia w składniku LINQ, ponieważ w tym przypadku tylko jeden gatunku oczekuje albumu.</span><span class="sxs-lookup"><span data-stu-id="ef39b-359">You can use the .**Single()** extension in LINQ because in this case only one genre is expected for an album.</span></span> <span data-ttu-id="ef39b-360">**Funkcja Single()** metoda przyjmuje wyrażenia Lambda jako parametru, w tym przypadku określa pojedynczy obiekt gatunku w taki sposób, że jego nazwa jest zgodna wartość zdefiniowana.</span><span class="sxs-lookup"><span data-stu-id="ef39b-360">The **Single()** method takes a Lambda expression as a parameter, which in this case specifies a single Genre object such that its name matches the value defined.</span></span>
> 
> <span data-ttu-id="ef39b-361">Będzie korzystać z funkcji, które można określić innych powiązanych jednostek, który ma być również załadowany po pobraniu obiektu gatunku.</span><span class="sxs-lookup"><span data-stu-id="ef39b-361">You will take advantage of a feature that allows you to indicate other related entities you want loaded as well when the Genre object is retrieved.</span></span> <span data-ttu-id="ef39b-362">Ta funkcja jest nazywana **kształtowania wynik kwerendy**i pozwala zmniejszyć liczbę potrzebnych do dostępu do bazy danych, aby pobrać informacje.</span><span class="sxs-lookup"><span data-stu-id="ef39b-362">This feature is called **Query Result Shaping**, and enables you to reduce the number of times needed to access the database to retrieve information.</span></span> <span data-ttu-id="ef39b-363">W tym scenariuszu można wstępnie pobrać albumów dla tego gatunku, możesz pobrać.</span><span class="sxs-lookup"><span data-stu-id="ef39b-363">In this scenario, you will want to pre-fetch the Albums for the Genre you retrieve.</span></span>
> 
> <span data-ttu-id="ef39b-364">Zapytanie zawiera **Genres.Include (&quot;albumów&quot;)** aby wskazać, że ma także powiązane ze zdjęciami.</span><span class="sxs-lookup"><span data-stu-id="ef39b-364">The query includes **Genres.Include(&quot;Albums&quot;)** to indicate that you want related albums as well.</span></span> <span data-ttu-id="ef39b-365">To spowoduje aplikacji bardziej efektywne, ponieważ pobierze dane gatunku i albumów w żądaniu pojedynczej bazy danych.</span><span class="sxs-lookup"><span data-stu-id="ef39b-365">This will result in a more efficient application, since it will retrieve both Genre and Album data in a single database request.</span></span>

<a id="Ex3Task2"></a>

<a id="Task_2_-_Running_the_Application"></a>
#### <a name="task-2---running-the-application"></a><span data-ttu-id="ef39b-366">Zadanie 2 — uruchamianie aplikacji</span><span class="sxs-lookup"><span data-stu-id="ef39b-366">Task 2 - Running the Application</span></span>

<span data-ttu-id="ef39b-367">W ramach tego zadania spowoduje uruchomienia aplikacji i pobrać albumów określonego rodzaju z bazy danych.</span><span class="sxs-lookup"><span data-stu-id="ef39b-367">In this task, you will run the application and retrieve albums of a specific genre from the database.</span></span>

1. <span data-ttu-id="ef39b-368">Naciśnij klawisz **F5** do uruchomienia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="ef39b-368">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="ef39b-369">Data rozpoczęcia projektu na stronie głównej.</span><span class="sxs-lookup"><span data-stu-id="ef39b-369">The project starts in the Home page.</span></span> <span data-ttu-id="ef39b-370">Zmień adres URL do **/Store/przeglądania? gatunku = Pop** Aby sprawdzić, czy wyniki są pobierane z bazy danych.</span><span class="sxs-lookup"><span data-stu-id="ef39b-370">Change the URL to **/Store/Browse?genre=Pop** to verify that the results are being retrieved from the database.</span></span>

    <span data-ttu-id="ef39b-371">![Przeglądanie według gatunku](aspnet-mvc-4-models-and-data-access/_static/image24.png "przeglądanie według gatunku")</span><span class="sxs-lookup"><span data-stu-id="ef39b-371">![Browsing by genre](aspnet-mvc-4-models-and-data-access/_static/image24.png "Browsing by genre")</span></span>

    <span data-ttu-id="ef39b-372">*Przeglądanie/Store/przeglądania? gatunku = Pop*</span><span class="sxs-lookup"><span data-stu-id="ef39b-372">*Browsing /Store/Browse?genre=Pop*</span></span>

<a id="Ex3Task3"></a>

<a id="Task_3_-_Accessing_Albums_by_Id"></a>
#### <a name="task-3---accessing-albums-by-id"></a><span data-ttu-id="ef39b-373">Zadanie 3. uzyskiwanie dostępu do albumów według identyfikatora</span><span class="sxs-lookup"><span data-stu-id="ef39b-373">Task 3 - Accessing Albums by Id</span></span>

<span data-ttu-id="ef39b-374">W tym zadaniu zostanie Powtórz poprzedniej procedury, aby uzyskać albumów według ich identyfikatorów.</span><span class="sxs-lookup"><span data-stu-id="ef39b-374">In this task, you will repeat the previous procedure to get albums by their Id.</span></span>

1. <span data-ttu-id="ef39b-375">Zamknij przeglądarkę, jeśli to konieczne, aby powrócić do programu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="ef39b-375">Close the browser if needed, to return to Visual Studio.</span></span> <span data-ttu-id="ef39b-376">Otwórz **StoreController** klasy, aby zmienić **szczegóły** metody akcji.</span><span class="sxs-lookup"><span data-stu-id="ef39b-376">Open the **StoreController** class to change the **Details** action method.</span></span> <span data-ttu-id="ef39b-377">Aby to zrobić, w **Eksploratora rozwiązań**, rozwiń węzeł **kontrolerów** folder i kliknij dwukrotnie plik **StoreController.cs**.</span><span class="sxs-lookup"><span data-stu-id="ef39b-377">To do this, in the **Solution Explorer**, expand the **Controllers** folder and double-click **StoreController.cs**.</span></span>
2. <span data-ttu-id="ef39b-378">Zmiana **szczegóły** na podstawie metody akcji, które można pobrać szczegółów albumów ich **identyfikator**. Aby to zrobić, Zastąp następujący kod:</span><span class="sxs-lookup"><span data-stu-id="ef39b-378">Change the **Details** action method to retrieve albums details based on their **Id**. To do this, replace the following code:</span></span>

    <span data-ttu-id="ef39b-379">(Code Snippet — *modele i dostęp do danych — Ex3 StoreController DetailsMethod*)</span><span class="sxs-lookup"><span data-stu-id="ef39b-379">(Code Snippet - *Models And Data Access - Ex3 StoreController DetailsMethod*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample18.cs)]

<a id="Ex3Task4"></a>

<a id="Task_4_-_Running_the_Application"></a>
#### <a name="task-4---running-the-application"></a><span data-ttu-id="ef39b-380">Zadanie 4 — uruchamianie aplikacji</span><span class="sxs-lookup"><span data-stu-id="ef39b-380">Task 4 - Running the Application</span></span>

<span data-ttu-id="ef39b-381">W ramach tego zadania spowoduje uruchomienia aplikacji w przeglądarce sieci web i Uzyskiwanie szczegółów albumu według ich identyfikatorów.</span><span class="sxs-lookup"><span data-stu-id="ef39b-381">In this task, you will run the Application in a web browser and obtain album details by their Id.</span></span>

1. <span data-ttu-id="ef39b-382">Naciśnij klawisz **F5** do uruchomienia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="ef39b-382">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="ef39b-383">Data rozpoczęcia projektu na stronie głównej.</span><span class="sxs-lookup"><span data-stu-id="ef39b-383">The project starts in the Home page.</span></span> <span data-ttu-id="ef39b-384">Zmień adres URL do **/Store/Details/51** lub gatunki Przeglądaj i wybierz pozycję album, aby sprawdzić, czy wyniki są pobierane z bazy danych.</span><span class="sxs-lookup"><span data-stu-id="ef39b-384">Change the URL to **/Store/Details/51** or browse the genres and select an album to verify that the results are being retrieved from the database.</span></span>

    <span data-ttu-id="ef39b-385">![Przeglądanie szczegółów](aspnet-mvc-4-models-and-data-access/_static/image25.png "Przeglądanie szczegółowe informacje")</span><span class="sxs-lookup"><span data-stu-id="ef39b-385">![Browsing Details](aspnet-mvc-4-models-and-data-access/_static/image25.png "Browsing Details")</span></span>

    <span data-ttu-id="ef39b-386">*Przeglądanie /Store/Details/51*</span><span class="sxs-lookup"><span data-stu-id="ef39b-386">*Browsing /Store/Details/51*</span></span>

> [!NOTE]
> <span data-ttu-id="ef39b-387">Ponadto można wdrożyć tę aplikację do witryny sieci Web systemu Windows Azure następujące [dodatek B: publikowania aplikacji ASP.NET MVC 4 za pomocą narzędzia Web Deploy](#AppendixB).</span><span class="sxs-lookup"><span data-stu-id="ef39b-387">Additionally, you can deploy this application to Windows Azure Web Sites following [Appendix B: Publishing an ASP.NET MVC 4 Application using Web Deploy](#AppendixB).</span></span>

* * *

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a><span data-ttu-id="ef39b-388">Podsumowanie</span><span class="sxs-lookup"><span data-stu-id="ef39b-388">Summary</span></span>

<span data-ttu-id="ef39b-389">Przez ukończenie tego laboratorium praktyczne, kiedy znasz już podstawy platformy ASP.NET MVC modele i dostęp do danych, przy użyciu **Database First** podejście, jak również **Code First** podejścia:</span><span class="sxs-lookup"><span data-stu-id="ef39b-389">By completing this Hands-on Lab you have learned the fundamentals of ASP.NET MVC Models and Data Access, using the **Database First** approach as well as the **Code First** Approach:</span></span>

- <span data-ttu-id="ef39b-390">Jak dodać bazę danych do rozwiązania w celu korzystania z danych</span><span class="sxs-lookup"><span data-stu-id="ef39b-390">How to add a database to the solution in order to consume its data</span></span>
- <span data-ttu-id="ef39b-391">Jak zaktualizować kontrolerów, aby zapewnić wyświetlanie szablonów przy użyciu danych pobranych z bazy danych zamiast zakodowanych</span><span class="sxs-lookup"><span data-stu-id="ef39b-391">How to update Controllers to provide View templates with the data taken from the database instead of hard-coded one</span></span>
- <span data-ttu-id="ef39b-392">Tworzenie zapytań względem bazy danych przy użyciu parametrów</span><span class="sxs-lookup"><span data-stu-id="ef39b-392">How to query the database using parameters</span></span>
- <span data-ttu-id="ef39b-393">Jak używać zapytania wynik kształtowania, funkcja, która zmniejsza liczbę uzyskuje dostęp do bazy danych, pobieranie danych w sposób bardziej efektywny</span><span class="sxs-lookup"><span data-stu-id="ef39b-393">How to use the Query Result Shaping, a feature that reduces the number of database accesses, retrieving data in a more efficient way</span></span>
- <span data-ttu-id="ef39b-394">Sposób użycia podejścia Database First i Code First platformy Entity Framework Microsoft połączyć bazę danych przy użyciu modelu</span><span class="sxs-lookup"><span data-stu-id="ef39b-394">How to use both Database First and Code First approaches in Microsoft Entity Framework to link the database with the model</span></span>

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a><span data-ttu-id="ef39b-395">Dodatek A: Instalowanie programu Visual Studio Express 2012 for Web</span><span class="sxs-lookup"><span data-stu-id="ef39b-395">Appendix A: Installing Visual Studio Express 2012 for Web</span></span>

<span data-ttu-id="ef39b-396">Możesz zainstalować **programu Microsoft Visual Studio Express 2012 for Web** lub inne &quot;Express&quot; przy użyciu wersji **[Instalatora platformy sieci Web firmy Microsoft](https://www.microsoft.com/web/downloads/platform.aspx)**.</span><span class="sxs-lookup"><span data-stu-id="ef39b-396">You can install **Microsoft Visual Studio Express 2012 for Web** or another &quot;Express&quot; version using the **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**.</span></span> <span data-ttu-id="ef39b-397">Poniższe wskazówki ułatwiają kroki wymagane do zainstalowania *programu Visual studio Express 2012 for Web* przy użyciu *Instalatora platformy sieci Web firmy Microsoft*.</span><span class="sxs-lookup"><span data-stu-id="ef39b-397">The following instructions guide you through the steps required to install *Visual studio Express 2012 for Web* using *Microsoft Web Platform Installer*.</span></span>

1. <span data-ttu-id="ef39b-398">Przejdź do [ [ https://go.microsoft.com/?linkid=9810169 ](https://go.microsoft.com/?linkid=9810169) ](https://go.microsoft.com/?linkid=9810169).</span><span class="sxs-lookup"><span data-stu-id="ef39b-398">Go to [[https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169).</span></span> <span data-ttu-id="ef39b-399">Alternatywnie, jeśli już zainstalowano Instalatora platformy sieci Web, możesz otworzyć go i Wyszukaj produkt &quot; <em>Visual Studio Express 2012 for Web z zestawem Windows Azure SDK</em>&quot;.</span><span class="sxs-lookup"><span data-stu-id="ef39b-399">Alternatively, if you already have installed Web Platform Installer, you can open it and search for the product &quot;<em>Visual Studio Express 2012 for Web with Windows Azure SDK</em>&quot;.</span></span>
2. <span data-ttu-id="ef39b-400">Kliknij pozycję **Zainstaluj teraz**.</span><span class="sxs-lookup"><span data-stu-id="ef39b-400">Click on **Install Now**.</span></span> <span data-ttu-id="ef39b-401">Jeśli nie masz **Instalatora platformy sieci Web** nastąpi przekierowanie do pobrania i zainstalowania go najpierw.</span><span class="sxs-lookup"><span data-stu-id="ef39b-401">If you do not have **Web Platform Installer** you will be redirected to download and install it first.</span></span>
3. <span data-ttu-id="ef39b-402">Raz **Instalatora platformy sieci Web** jest otwarty, kliknij przycisk **zainstalować** można uruchomić Instalatora.</span><span class="sxs-lookup"><span data-stu-id="ef39b-402">Once **Web Platform Installer** is open, click **Install** to start the setup.</span></span>

    <span data-ttu-id="ef39b-403">![Zainstaluj program Visual Studio Express](aspnet-mvc-4-models-and-data-access/_static/image26.png "zainstalować program Visual Studio Express")</span><span class="sxs-lookup"><span data-stu-id="ef39b-403">![Install Visual Studio Express](aspnet-mvc-4-models-and-data-access/_static/image26.png "Install Visual Studio Express")</span></span>

    <span data-ttu-id="ef39b-404">*Zainstaluj program Visual Studio Express*</span><span class="sxs-lookup"><span data-stu-id="ef39b-404">*Install Visual Studio Express*</span></span>
4. <span data-ttu-id="ef39b-405">Odczytywanie wszystkich produktów licencji oraz warunki, a następnie kliknij przycisk **akceptuję** aby kontynuować.</span><span class="sxs-lookup"><span data-stu-id="ef39b-405">Read all the products' licenses and terms and click **I Accept** to continue.</span></span>

    ![Akceptowanie umowy licencyjnej](aspnet-mvc-4-models-and-data-access/_static/image27.png)

    <span data-ttu-id="ef39b-407">*Akceptowanie umowy licencyjnej*</span><span class="sxs-lookup"><span data-stu-id="ef39b-407">*Accepting the license terms*</span></span>
5. <span data-ttu-id="ef39b-408">Zaczekaj, aż do zakończenia procesu pobierania i instalacji.</span><span class="sxs-lookup"><span data-stu-id="ef39b-408">Wait until the downloading and installation process completes.</span></span>

    ![Postęp instalacji](aspnet-mvc-4-models-and-data-access/_static/image28.png)

    <span data-ttu-id="ef39b-410">*Postęp instalacji*</span><span class="sxs-lookup"><span data-stu-id="ef39b-410">*Installation progress*</span></span>
6. <span data-ttu-id="ef39b-411">Po zakończeniu instalacji kliknij przycisk **Zakończ**.</span><span class="sxs-lookup"><span data-stu-id="ef39b-411">When the installation completes, click **Finish**.</span></span>

    ![Instalacja została zakończona](aspnet-mvc-4-models-and-data-access/_static/image29.png)

    <span data-ttu-id="ef39b-413">*Instalacja została zakończona*</span><span class="sxs-lookup"><span data-stu-id="ef39b-413">*Installation completed*</span></span>
7. <span data-ttu-id="ef39b-414">Kliknij przycisk **zakończenia** zamknąć Instalatora platformy sieci Web.</span><span class="sxs-lookup"><span data-stu-id="ef39b-414">Click **Exit** to close Web Platform Installer.</span></span>
8. <span data-ttu-id="ef39b-415">Aby otworzyć program Visual Studio Express for Web, przejdź do **Start** ekranu, a następnie zacznij pisać &quot; **VS Express**&quot;, następnie kliknij pozycję **VS Express for Web** Kafelek.</span><span class="sxs-lookup"><span data-stu-id="ef39b-415">To open Visual Studio Express for Web, go to the **Start** screen and start writing &quot;**VS Express**&quot;, then click on the **VS Express for Web** tile.</span></span>

    ![VS Express for Web kafelka](aspnet-mvc-4-models-and-data-access/_static/image30.png)

    <span data-ttu-id="ef39b-417">*VS Express for Web kafelka*</span><span class="sxs-lookup"><span data-stu-id="ef39b-417">*VS Express for Web tile*</span></span>

<a id="AppendixB"></a>

<a id="Appendix_B_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
## <a name="appendix-b-publishing-an-aspnet-mvc-4-application-using-web-deploy"></a><span data-ttu-id="ef39b-418">Dodatek B: publikowania aplikacji ASP.NET MVC 4 za pomocą narzędzia Web Deploy</span><span class="sxs-lookup"><span data-stu-id="ef39b-418">Appendix B: Publishing an ASP.NET MVC 4 Application using Web Deploy</span></span>

<span data-ttu-id="ef39b-419">Ten dodatek będzie pokazują, jak utworzyć nową witrynę sieci web w portalu zarządzania pakietu Windows Azure i publikowanie aplikacji, uzyskany postępując zgodnie z laboratorium, korzystając z zalet funkcji publikowania narzędzia Web Deploy oferowanego przez system Windows Azure.</span><span class="sxs-lookup"><span data-stu-id="ef39b-419">This appendix will show you how to create a new web site from the Windows Azure Management Portal and publish the application you obtained by following the lab, taking advantage of the Web Deploy publishing feature provided by Windows Azure.</span></span>

<a id="ApxBTask1"></a>

<a id="Task_1_-_Creating_a_New_Web_Site_from_the_Windows_Azure_Portal"></a>
#### <a name="task-1---creating-a-new-web-site-from-the-windows-azure-portal"></a><span data-ttu-id="ef39b-420">Zadanie 1. Tworzenie nowej witryny sieci Web z Windows Azure Portal</span><span class="sxs-lookup"><span data-stu-id="ef39b-420">Task 1 - Creating a New Web Site from the Windows Azure Portal</span></span>

1. <span data-ttu-id="ef39b-421">Przejdź do [portalu zarządzania pakietu Windows Azure](https://manage.windowsazure.com/) i zaloguj się przy użyciu poświadczeń firmy Microsoft, powiązaną z Twoją subskrypcją.</span><span class="sxs-lookup"><span data-stu-id="ef39b-421">Go to the [Windows Azure Management Portal](https://manage.windowsazure.com/) and sign in using the Microsoft credentials associated with your subscription.</span></span>

    > [!NOTE]
    > <span data-ttu-id="ef39b-422">Platforma Windows Azure można bezpłatny hosting 10 witryn sieci Web platformy ASP.NET i skalowanie w miarę wzrostu ruchu.</span><span class="sxs-lookup"><span data-stu-id="ef39b-422">With Windows Azure you can host 10 ASP.NET Web Sites for free and then scale as your traffic grows.</span></span> <span data-ttu-id="ef39b-423">Możesz zarejestrować się [tutaj](http://aka.ms/aspnet-hol-azure).</span><span class="sxs-lookup"><span data-stu-id="ef39b-423">You can sign up [here](http://aka.ms/aspnet-hol-azure).</span></span>

    <span data-ttu-id="ef39b-424">![Zaloguj się do portalu usługi Windows Azure](aspnet-mvc-4-models-and-data-access/_static/image31.png "Zaloguj się do portalu usługi Windows Azure")</span><span class="sxs-lookup"><span data-stu-id="ef39b-424">![Log on to Windows Azure portal](aspnet-mvc-4-models-and-data-access/_static/image31.png "Log on to Windows Azure portal")</span></span>

    <span data-ttu-id="ef39b-425">*Zaloguj się do portalu zarządzania systemu Azure Windows*</span><span class="sxs-lookup"><span data-stu-id="ef39b-425">*Log on to Windows Azure Management Portal*</span></span>
2. <span data-ttu-id="ef39b-426">Kliknij przycisk **New** na pasku poleceń.</span><span class="sxs-lookup"><span data-stu-id="ef39b-426">Click **New** on the command bar.</span></span>

    <span data-ttu-id="ef39b-427">![Tworzenie nowej witryny sieci Web](aspnet-mvc-4-models-and-data-access/_static/image32.png "tworzenia nowej witryny sieci Web")</span><span class="sxs-lookup"><span data-stu-id="ef39b-427">![Creating a new Web Site](aspnet-mvc-4-models-and-data-access/_static/image32.png "Creating a new Web Site")</span></span>

    <span data-ttu-id="ef39b-428">*Tworzenie nowej witryny sieci Web*</span><span class="sxs-lookup"><span data-stu-id="ef39b-428">*Creating a new Web Site*</span></span>
3. <span data-ttu-id="ef39b-429">Kliknij przycisk **obliczenia** | **witryny sieci Web**.</span><span class="sxs-lookup"><span data-stu-id="ef39b-429">Click **Compute** | **Web Site**.</span></span> <span data-ttu-id="ef39b-430">Następnie wybierz pozycję **szybkie tworzenie** opcji.</span><span class="sxs-lookup"><span data-stu-id="ef39b-430">Then select **Quick Create** option.</span></span> <span data-ttu-id="ef39b-431">Podaj adres URL dostępny dla nowej witryny sieci web, a następnie kliknij przycisk **Utwórz witrynę sieci Web**.</span><span class="sxs-lookup"><span data-stu-id="ef39b-431">Provide an available URL for the new web site and click **Create Web Site**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="ef39b-432">Witryny sieci Web Windows Azure jest hostem dla aplikacji sieci web działające w chmurze, które można kontrolować i zarządzać nimi.</span><span class="sxs-lookup"><span data-stu-id="ef39b-432">A Windows Azure Web Site is the host for a web application running in the cloud that you can control and manage.</span></span> <span data-ttu-id="ef39b-433">Opcja szybkie tworzenie umożliwia wdrażanie ukończonej aplikacji sieci web do Windows Azure witryny sieci Web z poza portalem.</span><span class="sxs-lookup"><span data-stu-id="ef39b-433">The Quick Create option allows you to deploy a completed web application to the Windows Azure Web Site from outside the portal.</span></span> <span data-ttu-id="ef39b-434">Nie obejmuje kroki konfigurowania bazy danych.</span><span class="sxs-lookup"><span data-stu-id="ef39b-434">It does not include steps for setting up a database.</span></span>

    <span data-ttu-id="ef39b-435">![Tworzenie nowej witryny sieci Web przy użyciu szybkiego tworzenia](aspnet-mvc-4-models-and-data-access/_static/image33.png "tworzenia nowej witryny sieci Web przy użyciu szybkie tworzenie")</span><span class="sxs-lookup"><span data-stu-id="ef39b-435">![Creating a new Web Site using Quick Create](aspnet-mvc-4-models-and-data-access/_static/image33.png "Creating a new Web Site using Quick Create")</span></span>

    <span data-ttu-id="ef39b-436">*Tworzenie nowej witryny sieci Web przy użyciu szybkie tworzenie*</span><span class="sxs-lookup"><span data-stu-id="ef39b-436">*Creating a new Web Site using Quick Create*</span></span>
4. <span data-ttu-id="ef39b-437">Poczekaj, aż nowe **witryny sieci Web** zostanie utworzony.</span><span class="sxs-lookup"><span data-stu-id="ef39b-437">Wait until the new **Web Site** is created.</span></span>
5. <span data-ttu-id="ef39b-438">Po utworzeniu witryny sieci Web kliknij link w obszarze **adresu URL** kolumny.</span><span class="sxs-lookup"><span data-stu-id="ef39b-438">Once the Web Site is created click the link under the **URL** column.</span></span> <span data-ttu-id="ef39b-439">Sprawdź, czy działa nową witrynę sieci Web.</span><span class="sxs-lookup"><span data-stu-id="ef39b-439">Check that the new Web Site is working.</span></span>

    <span data-ttu-id="ef39b-440">![Przejście do nowej witryny sieci web](aspnet-mvc-4-models-and-data-access/_static/image34.png "przejście do nowej witryny sieci web")</span><span class="sxs-lookup"><span data-stu-id="ef39b-440">![Browsing to the new web site](aspnet-mvc-4-models-and-data-access/_static/image34.png "Browsing to the new web site")</span></span>

    <span data-ttu-id="ef39b-441">*Przejście do nowej witryny sieci web*</span><span class="sxs-lookup"><span data-stu-id="ef39b-441">*Browsing to the new web site*</span></span>

    <span data-ttu-id="ef39b-442">![Witryna sieci Web działa](aspnet-mvc-4-models-and-data-access/_static/image35.png "witryna sieci Web działa")</span><span class="sxs-lookup"><span data-stu-id="ef39b-442">![Web site running](aspnet-mvc-4-models-and-data-access/_static/image35.png "Web site running")</span></span>

    <span data-ttu-id="ef39b-443">*Witryna sieci Web działa*</span><span class="sxs-lookup"><span data-stu-id="ef39b-443">*Web site running*</span></span>
6. <span data-ttu-id="ef39b-444">Wróć do portalu i kliknij nazwę witryny sieci web w obszarze **nazwa** kolumny do wyświetlenia strony zarządzania.</span><span class="sxs-lookup"><span data-stu-id="ef39b-444">Go back to the portal and click the name of the web site under the **Name** column to display the management pages.</span></span>

    <span data-ttu-id="ef39b-445">![Otwieranie strony zarządzania witryny sieci web](aspnet-mvc-4-models-and-data-access/_static/image36.png "otwieranie stron zarządzania witryny sieci web")</span><span class="sxs-lookup"><span data-stu-id="ef39b-445">![Opening the web site management pages](aspnet-mvc-4-models-and-data-access/_static/image36.png "Opening the web site management pages")</span></span>

    <span data-ttu-id="ef39b-446">*Otwieranie strony zarządzania witryny sieci Web*</span><span class="sxs-lookup"><span data-stu-id="ef39b-446">*Opening the Web Site management pages*</span></span>
7. <span data-ttu-id="ef39b-447">W **pulpit nawigacyjny** w obszarze **Przegląd** kliknij **Pobierz profil publikowania** łącza.</span><span class="sxs-lookup"><span data-stu-id="ef39b-447">In the **Dashboard** page, under the **quick glance** section, click the **Download publish profile** link.</span></span>

    > [!NOTE]
    > <span data-ttu-id="ef39b-448">*Profil publikowania* zawiera wszystkie informacje wymagane do publikowania aplikacji sieci web w witrynie sieci Web usługi Windows Azure dla poszczególnych metod włączone publikacji.</span><span class="sxs-lookup"><span data-stu-id="ef39b-448">The *publish profile* contains all of the information required to publish a web application to a Windows Azure website for each enabled publication method.</span></span> <span data-ttu-id="ef39b-449">Profil publikowania zawiera adresy URL, poświadczenia użytkownika i parametry bazy danych wymagane do łączenia się i uwierzytelnianie w odniesieniu do każdego z punktów końcowych, dla których włączono metoda publikacji.</span><span class="sxs-lookup"><span data-stu-id="ef39b-449">The publish profile contains the URLs, user credentials and database strings required to connect to and authenticate against each of the endpoints for which a publication method is enabled.</span></span> <span data-ttu-id="ef39b-450">**Microsoft WebMatrix 2**, **Microsoft Visual Studio Express for Web** i **Microsoft Visual Studio 2012** Obsługa odczytywania profili publikowania do automatyzowania konfiguracji tych programów dla Publikowanie aplikacji sieci web w usłudze Windows Azure websites.</span><span class="sxs-lookup"><span data-stu-id="ef39b-450">**Microsoft WebMatrix 2**, **Microsoft Visual Studio Express for Web** and **Microsoft Visual Studio 2012** support reading publish profiles to automate configuration of these programs for publishing web applications to Windows Azure websites.</span></span>

    <span data-ttu-id="ef39b-451">![Pobieranie witryny sieci web profil publikowania](aspnet-mvc-4-models-and-data-access/_static/image37.png "pobierania witryny sieci web profilu publikowania")</span><span class="sxs-lookup"><span data-stu-id="ef39b-451">![Downloading the web site publish profile](aspnet-mvc-4-models-and-data-access/_static/image37.png "Downloading the web site publish profile")</span></span>

    <span data-ttu-id="ef39b-452">*Pobieranie witryny sieci Web profilu publikowania*</span><span class="sxs-lookup"><span data-stu-id="ef39b-452">*Downloading the Web Site publish profile*</span></span>
8. <span data-ttu-id="ef39b-453">Pobierz plik profilu publikowania w znanej lokalizacji.</span><span class="sxs-lookup"><span data-stu-id="ef39b-453">Download the publish profile file to a known location.</span></span> <span data-ttu-id="ef39b-454">Dodatkowo w tym ćwiczeniu zobaczysz jak opublikować aplikację sieci web do Windows Azure Web Sites z programu Visual Studio za pomocą tego pliku.</span><span class="sxs-lookup"><span data-stu-id="ef39b-454">Further in this exercise you will see how to use this file to publish a web application to a Windows Azure Web Sites from Visual Studio.</span></span>

    <span data-ttu-id="ef39b-455">![Zapisywanie pliku profilu publikowania](aspnet-mvc-4-models-and-data-access/_static/image38.png "zapisywanie profilu publikowania")</span><span class="sxs-lookup"><span data-stu-id="ef39b-455">![Saving the publish profile file](aspnet-mvc-4-models-and-data-access/_static/image38.png "Saving the publish profile")</span></span>

    <span data-ttu-id="ef39b-456">*Zapisywanie pliku profilu publikowania*</span><span class="sxs-lookup"><span data-stu-id="ef39b-456">*Saving the publish profile file*</span></span>

<a id="ApxBTask2"></a>

<a id="Task_2_-_Configuring_the_Database_Server"></a>
#### <a name="task-2---configuring-the-database-server"></a><span data-ttu-id="ef39b-457">Zadanie 2 — Konfigurowanie serwera bazy danych</span><span class="sxs-lookup"><span data-stu-id="ef39b-457">Task 2 - Configuring the Database Server</span></span>

<span data-ttu-id="ef39b-458">Jeśli aplikacja korzysta z programu SQL Server baz danych, musisz utworzyć serwer bazy danych SQL.</span><span class="sxs-lookup"><span data-stu-id="ef39b-458">If your application makes use of SQL Server databases you will need to create a SQL Database server.</span></span> <span data-ttu-id="ef39b-459">Jeśli chcesz wdrożyć prostą aplikację, która nie korzysta z programu SQL Server może pominąć to zadanie.</span><span class="sxs-lookup"><span data-stu-id="ef39b-459">If you want to deploy a simple application that does not use SQL Server you might skip this task.</span></span>

1. <span data-ttu-id="ef39b-460">Należy serwera usługi SQL Database do przechowywania bazy danych aplikacji.</span><span class="sxs-lookup"><span data-stu-id="ef39b-460">You will need a SQL Database server for storing the application database.</span></span> <span data-ttu-id="ef39b-461">Możesz wyświetlić serwery baz danych SQL z subskrypcji w portalu zarządzania Azure Windows pod **baz danych Sql** | **serwerów** | **serwera Pulpit nawigacyjny**.</span><span class="sxs-lookup"><span data-stu-id="ef39b-461">You can view the SQL Database servers from your subscription in the Windows Azure Management portal at **Sql Databases** | **Servers** | **Server's Dashboard**.</span></span> <span data-ttu-id="ef39b-462">Jeśli nie masz serwer, który został utworzony, można utworzyć ją przy użyciu **Dodaj** przycisk na pasku poleceń.</span><span class="sxs-lookup"><span data-stu-id="ef39b-462">If you do not have a server created, you can create one using the **Add** button on the command bar.</span></span> <span data-ttu-id="ef39b-463">Zwróć uwagę na **nazwę serwera i adres URL, nazwę logowania administratora i hasła**, ponieważ będziesz ich używać w kolejne zadania podrzędne.</span><span class="sxs-lookup"><span data-stu-id="ef39b-463">Take note of the **server name and URL, administrator login name and password**, as you will use them in the next tasks.</span></span> <span data-ttu-id="ef39b-464">Nie należy tworzyć bazy danych, ponieważ zostanie on utworzony w późniejszym terminie.</span><span class="sxs-lookup"><span data-stu-id="ef39b-464">Do not create the database yet, as it will be created in a later stage.</span></span>

    <span data-ttu-id="ef39b-465">![Pulpit nawigacyjny z serwera bazy danych SQL](aspnet-mvc-4-models-and-data-access/_static/image39.png "pulpitu nawigacyjnego serwera bazy danych SQL")</span><span class="sxs-lookup"><span data-stu-id="ef39b-465">![SQL Database Server Dashboard](aspnet-mvc-4-models-and-data-access/_static/image39.png "SQL Database Server Dashboard")</span></span>

    <span data-ttu-id="ef39b-466">*Pulpit nawigacyjny z serwera bazy danych SQL*</span><span class="sxs-lookup"><span data-stu-id="ef39b-466">*SQL Database Server Dashboard*</span></span>
2. <span data-ttu-id="ef39b-467">W ramach następnego zadania spowoduje przetestowanie połączenia z bazą danych z programu Visual Studio, dlatego trzeba uwzględnić adres IP lokalnego serwera liście **dozwolone adresy IP**.</span><span class="sxs-lookup"><span data-stu-id="ef39b-467">In the next task you will test the database connection from Visual Studio, for that reason you need to include your local IP address in the server's list of **Allowed IP Addresses**.</span></span> <span data-ttu-id="ef39b-468">Aby to zrobić, kliknij przycisk **Konfiguruj**, wybierz adres IP z **bieżący adres IP klienta** i wklej go na **początkowy adres IP** i **końcowy adres IP** pola tekstowe i kliknij przycisk ![add-client-ip-address-ok-button](aspnet-mvc-4-models-and-data-access/_static/image40.png) przycisku.</span><span class="sxs-lookup"><span data-stu-id="ef39b-468">To do that, click **Configure**, select the IP address from **Current Client IP Address** and paste it on the **Start IP Address** and **End IP Address** text boxes and click the ![add-client-ip-address-ok-button](aspnet-mvc-4-models-and-data-access/_static/image40.png) button.</span></span>

    ![Dodawanie adresu IP klienta](aspnet-mvc-4-models-and-data-access/_static/image41.png)

    <span data-ttu-id="ef39b-470">*Dodawanie adresu IP klienta*</span><span class="sxs-lookup"><span data-stu-id="ef39b-470">*Adding Client IP Address*</span></span>
3. <span data-ttu-id="ef39b-471">Raz **adres IP klienta** jest dodawany do dozwolonych adresów IP listy, kliknij pozycję **Zapisz** aby potwierdzić zmiany.</span><span class="sxs-lookup"><span data-stu-id="ef39b-471">Once the **Client IP Address** is added to the allowed IP addresses list, click on **Save** to confirm the changes.</span></span>

    ![Potwierdź zmiany](aspnet-mvc-4-models-and-data-access/_static/image42.png)

    <span data-ttu-id="ef39b-473">*Potwierdź zmiany*</span><span class="sxs-lookup"><span data-stu-id="ef39b-473">*Confirm Changes*</span></span>

<a id="ApxBTask3"></a>

<a id="Task_3_-_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
#### <a name="task-3---publishing-an-aspnet-mvc-4-application-using-web-deploy"></a><span data-ttu-id="ef39b-474">Zadanie 3 — Publikowanie aplikacji ASP.NET MVC 4 za pomocą narzędzia Web Deploy</span><span class="sxs-lookup"><span data-stu-id="ef39b-474">Task 3 - Publishing an ASP.NET MVC 4 Application using Web Deploy</span></span>

1. <span data-ttu-id="ef39b-475">Wróć do rozwiązania ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="ef39b-475">Go back to the ASP.NET MVC 4 solution.</span></span> <span data-ttu-id="ef39b-476">W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy projekt witryny sieci web i wybierz **Publikuj**.</span><span class="sxs-lookup"><span data-stu-id="ef39b-476">In the **Solution Explorer**, right-click the web site project and select **Publish**.</span></span>

    <span data-ttu-id="ef39b-477">![Publikowanie aplikacji](aspnet-mvc-4-models-and-data-access/_static/image43.png "publikowania aplikacji")</span><span class="sxs-lookup"><span data-stu-id="ef39b-477">![Publishing the Application](aspnet-mvc-4-models-and-data-access/_static/image43.png "Publishing the Application")</span></span>

    <span data-ttu-id="ef39b-478">*Publikowanie witryny sieci web*</span><span class="sxs-lookup"><span data-stu-id="ef39b-478">*Publishing the web site*</span></span>
2. <span data-ttu-id="ef39b-479">Importuj profil publikowania, zapisaną w pierwszym zadaniem.</span><span class="sxs-lookup"><span data-stu-id="ef39b-479">Import the publish profile you saved in the first task.</span></span>

    <span data-ttu-id="ef39b-480">![Trwa importowanie profilu publikowania](aspnet-mvc-4-models-and-data-access/_static/image44.png "importowania profilu publikowania")</span><span class="sxs-lookup"><span data-stu-id="ef39b-480">![Importing the publish profile](aspnet-mvc-4-models-and-data-access/_static/image44.png "Importing the publish profile")</span></span>

    <span data-ttu-id="ef39b-481">*Importowanie profilu publikowania*</span><span class="sxs-lookup"><span data-stu-id="ef39b-481">*Importing publish profile*</span></span>
3. <span data-ttu-id="ef39b-482">Kliknij przycisk **sprawdzanie poprawności połączenia**.</span><span class="sxs-lookup"><span data-stu-id="ef39b-482">Click **Validate Connection**.</span></span> <span data-ttu-id="ef39b-483">Po zakończeniu sprawdzania kliknij **dalej**.</span><span class="sxs-lookup"><span data-stu-id="ef39b-483">Once Validation is complete click **Next**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="ef39b-484">Sprawdzanie poprawności zostało ukończone, gdy zostanie wyświetlony z zielonym znacznikiem wyboru pojawiają się obok przycisku Waliduj połączenie.</span><span class="sxs-lookup"><span data-stu-id="ef39b-484">Validation is complete once you see a green checkmark appear next to the Validate Connection button.</span></span>

    <span data-ttu-id="ef39b-485">![Sprawdzanie poprawności połączenia](aspnet-mvc-4-models-and-data-access/_static/image45.png "sprawdzanie poprawności połączenia")</span><span class="sxs-lookup"><span data-stu-id="ef39b-485">![Validating connection](aspnet-mvc-4-models-and-data-access/_static/image45.png "Validating connection")</span></span>

    <span data-ttu-id="ef39b-486">*Sprawdzanie poprawności połączenia*</span><span class="sxs-lookup"><span data-stu-id="ef39b-486">*Validating connection*</span></span>
4. <span data-ttu-id="ef39b-487">W **ustawienia** w obszarze **baz danych** sekcji, kliknij przycisk obok pola tekstowego połączenia bazy danych (czyli **DefaultConnection**).</span><span class="sxs-lookup"><span data-stu-id="ef39b-487">In the **Settings** page, under the **Databases** section, click the button next to your database connection's textbox (i.e. **DefaultConnection**).</span></span>

    <span data-ttu-id="ef39b-488">![Konfiguracja narzędzia Web deploy](aspnet-mvc-4-models-and-data-access/_static/image46.png "Konfiguracja narzędzia Web deploy")</span><span class="sxs-lookup"><span data-stu-id="ef39b-488">![Web deploy configuration](aspnet-mvc-4-models-and-data-access/_static/image46.png "Web deploy configuration")</span></span>

    <span data-ttu-id="ef39b-489">*Konfiguracja narzędzia Web deploy*</span><span class="sxs-lookup"><span data-stu-id="ef39b-489">*Web deploy configuration*</span></span>
5. <span data-ttu-id="ef39b-490">Skonfiguruj połączenie z bazą danych w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="ef39b-490">Configure the database connection as follows:</span></span>

   - <span data-ttu-id="ef39b-491">W **nazwy serwera** wpisz swoją bazę danych SQL server adresu URL przy użyciu *tcp:* prefiks.</span><span class="sxs-lookup"><span data-stu-id="ef39b-491">In the **Server name** type your SQL Database server URL using the *tcp:* prefix.</span></span>
   - <span data-ttu-id="ef39b-492">W **nazwa_użytkownika** wpisz nazwę logowania administratora serwera.</span><span class="sxs-lookup"><span data-stu-id="ef39b-492">In **User name** type your server administrator login name.</span></span>
   - <span data-ttu-id="ef39b-493">W **hasło** wpisz hasło logowania administratora serwera.</span><span class="sxs-lookup"><span data-stu-id="ef39b-493">In **Password** type your server administrator login password.</span></span>
   - <span data-ttu-id="ef39b-494">Wpisz nazwę nowej bazy danych.</span><span class="sxs-lookup"><span data-stu-id="ef39b-494">Type a new database name.</span></span>

     <span data-ttu-id="ef39b-495">![Konfigurowanie parametrów połączenia z docelowym](aspnet-mvc-4-models-and-data-access/_static/image47.png "Konfigurowanie parametrów połączenia z docelowym")</span><span class="sxs-lookup"><span data-stu-id="ef39b-495">![Configuring destination connection string](aspnet-mvc-4-models-and-data-access/_static/image47.png "Configuring destination connection string")</span></span>

     <span data-ttu-id="ef39b-496">*Konfigurowanie parametrów połączenia z docelowym*</span><span class="sxs-lookup"><span data-stu-id="ef39b-496">*Configuring destination connection string*</span></span>
6. <span data-ttu-id="ef39b-497">Następnie kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="ef39b-497">Then click **OK**.</span></span> <span data-ttu-id="ef39b-498">Po wyświetleniu monitu, aby utworzyć kliknij bazę danych **tak**.</span><span class="sxs-lookup"><span data-stu-id="ef39b-498">When prompted to create the database click **Yes**.</span></span>

    <span data-ttu-id="ef39b-499">![Tworzenie bazy danych](aspnet-mvc-4-models-and-data-access/_static/image48.png "Tworzenie parametrów bazy danych")</span><span class="sxs-lookup"><span data-stu-id="ef39b-499">![Creating the database](aspnet-mvc-4-models-and-data-access/_static/image48.png "Creating the database string")</span></span>

    <span data-ttu-id="ef39b-500">*Tworzenie bazy danych*</span><span class="sxs-lookup"><span data-stu-id="ef39b-500">*Creating the database*</span></span>
7. <span data-ttu-id="ef39b-501">Parametry połączenia, które będą używane do łączenia z bazą danych SQL na platformie Windows Azure jest wyświetlana w polu tekstowym połączenia domyślne.</span><span class="sxs-lookup"><span data-stu-id="ef39b-501">The connection string you will use to connect to SQL Database in Windows Azure is shown within Default Connection textbox.</span></span> <span data-ttu-id="ef39b-502">Następnie kliknij przycisk **Dalej**.</span><span class="sxs-lookup"><span data-stu-id="ef39b-502">Then click **Next**.</span></span>

    <span data-ttu-id="ef39b-503">![Ciąg połączenia wskazujący bazę danych SQL](aspnet-mvc-4-models-and-data-access/_static/image49.png "ciąg połączenia wskazujący bazę danych SQL")</span><span class="sxs-lookup"><span data-stu-id="ef39b-503">![Connection string pointing to SQL Database](aspnet-mvc-4-models-and-data-access/_static/image49.png "Connection string pointing to SQL Database")</span></span>

    <span data-ttu-id="ef39b-504">*Ciąg połączenia wskazujący bazę danych SQL*</span><span class="sxs-lookup"><span data-stu-id="ef39b-504">*Connection string pointing to SQL Database*</span></span>
8. <span data-ttu-id="ef39b-505">W **Podgląd** kliknij **Publikuj**.</span><span class="sxs-lookup"><span data-stu-id="ef39b-505">In the **Preview** page, click **Publish**.</span></span>

    <span data-ttu-id="ef39b-506">![Publikowanie aplikacji sieci web](aspnet-mvc-4-models-and-data-access/_static/image50.png "publikowania aplikacji sieci web")</span><span class="sxs-lookup"><span data-stu-id="ef39b-506">![Publishing the web application](aspnet-mvc-4-models-and-data-access/_static/image50.png "Publishing the web application")</span></span>

    <span data-ttu-id="ef39b-507">*Publikowanie aplikacji sieci web*</span><span class="sxs-lookup"><span data-stu-id="ef39b-507">*Publishing the web application*</span></span>
9. <span data-ttu-id="ef39b-508">Po zakończeniu procesu publikowania domyślnej przeglądarce otworzy opublikowanej witryny sieci web.</span><span class="sxs-lookup"><span data-stu-id="ef39b-508">Once the publishing process finishes, your default browser will open the published web site.</span></span>

<a id="AppendixC"></a>

<a id="Appendix_C_Using_Code_Snippets"></a>
## <a name="appendix-c-using-code-snippets"></a><span data-ttu-id="ef39b-509">Dodatek C: korzystania z fragmentów kodu</span><span class="sxs-lookup"><span data-stu-id="ef39b-509">Appendix C: Using Code Snippets</span></span>

<span data-ttu-id="ef39b-510">Za pomocą wstawek kodu masz kod, czego potrzebujesz w zasięgu ręki.</span><span class="sxs-lookup"><span data-stu-id="ef39b-510">With code snippets, you have all the code you need at your fingertips.</span></span> <span data-ttu-id="ef39b-511">Dokument laboratorium informuje o dokładnie podczas korzystania z nich, jak pokazano na poniższej ilustracji.</span><span class="sxs-lookup"><span data-stu-id="ef39b-511">The lab document will tell you exactly when you can use them, as shown in the following figure.</span></span>

<span data-ttu-id="ef39b-512">![Za pomocą wstawek kodu programu Visual Studio, aby wstawić kod do projektu](aspnet-mvc-4-models-and-data-access/_static/image51.png "fragmenty kodu za pomocą programu Visual Studio, aby wstawić kod do projektu")</span><span class="sxs-lookup"><span data-stu-id="ef39b-512">![Using Visual Studio code snippets to insert code into your project](aspnet-mvc-4-models-and-data-access/_static/image51.png "Using Visual Studio code snippets to insert code into your project")</span></span>

<span data-ttu-id="ef39b-513">*Za pomocą wstawek kodu programu Visual Studio, aby wstawić kod do projektu*</span><span class="sxs-lookup"><span data-stu-id="ef39b-513">*Using Visual Studio code snippets to insert code into your project*</span></span>

<span data-ttu-id="ef39b-514">***Aby dodać wstawki kodu za pomocą klawiatury (tylko język C#)***</span><span class="sxs-lookup"><span data-stu-id="ef39b-514">***To add a code snippet using the keyboard (C# only)***</span></span>

1. <span data-ttu-id="ef39b-515">Umieść kursor, w którym chcesz wstawić kod.</span><span class="sxs-lookup"><span data-stu-id="ef39b-515">Place the cursor where you would like to insert the code.</span></span>
2. <span data-ttu-id="ef39b-516">Rozpocznij wpisywanie nazwy fragmentu kodu (bez spacji i łączniki).</span><span class="sxs-lookup"><span data-stu-id="ef39b-516">Start typing the snippet name (without spaces or hyphens).</span></span>
3. <span data-ttu-id="ef39b-517">Obejrzyj, jak technologia IntelliSense wyświetla zgodne z nazwami fragmenty kodu.</span><span class="sxs-lookup"><span data-stu-id="ef39b-517">Watch as IntelliSense displays matching snippets' names.</span></span>
4. <span data-ttu-id="ef39b-518">Wybierz prawidłowy fragment kodu lub pisz dalej do momentu wybrania debugowanej nazwy całego fragmentu kodu.</span><span class="sxs-lookup"><span data-stu-id="ef39b-518">Select the correct snippet (or keep typing until the entire snippet's name is selected).</span></span>
5. <span data-ttu-id="ef39b-519">Naciśnij klawisz Tab dwa razy, aby wstawić fragment kodu w lokalizacji kursora.</span><span class="sxs-lookup"><span data-stu-id="ef39b-519">Press the Tab key twice to insert the snippet at the cursor location.</span></span>

<span data-ttu-id="ef39b-520">![Rozpocznij wpisywanie nazwy fragmentu kodu](aspnet-mvc-4-models-and-data-access/_static/image52.png "Rozpocznij wpisywanie nazwy fragmentu kodu")</span><span class="sxs-lookup"><span data-stu-id="ef39b-520">![Start typing the snippet name](aspnet-mvc-4-models-and-data-access/_static/image52.png "Start typing the snippet name")</span></span>

<span data-ttu-id="ef39b-521">*Rozpocznij wpisywanie nazwy fragmentu kodu*</span><span class="sxs-lookup"><span data-stu-id="ef39b-521">*Start typing the snippet name*</span></span>

<span data-ttu-id="ef39b-522">![Naciśnij klawisz Tab, aby zaznaczyć fragment wyróżnione](aspnet-mvc-4-models-and-data-access/_static/image53.png "naciśnij klawisz Tab, aby wybrać wyróżnione fragmentu kodu")</span><span class="sxs-lookup"><span data-stu-id="ef39b-522">![Press Tab to select the highlighted snippet](aspnet-mvc-4-models-and-data-access/_static/image53.png "Press Tab to select the highlighted snippet")</span></span>

<span data-ttu-id="ef39b-523">*Naciśnij klawisz Tab, aby wybrać wyróżnione fragment kodu*</span><span class="sxs-lookup"><span data-stu-id="ef39b-523">*Press Tab to select the highlighted snippet*</span></span>

<span data-ttu-id="ef39b-524">![Ponownie naciśnij klawisz Tab i fragment kodu zostanie rozwinięte](aspnet-mvc-4-models-and-data-access/_static/image54.png "rozwinie ponownie naciśnij klawisz Tab i fragment kodu")</span><span class="sxs-lookup"><span data-stu-id="ef39b-524">![Press Tab again and the snippet will expand](aspnet-mvc-4-models-and-data-access/_static/image54.png "Press Tab again and the snippet will expand")</span></span>

<span data-ttu-id="ef39b-525">*Ponownie naciśnij klawisz Tab i fragment kodu zostanie rozwinięte.*</span><span class="sxs-lookup"><span data-stu-id="ef39b-525">*Press Tab again and the snippet will expand*</span></span>

<span data-ttu-id="ef39b-526">***Aby dodać wstawki kodu za pomocą myszy (C#, Visual Basic i XML)*** 1.</span><span class="sxs-lookup"><span data-stu-id="ef39b-526">***To add a code snippet using the mouse (C#, Visual Basic and XML)*** 1.</span></span> <span data-ttu-id="ef39b-527">Kliknij prawym przyciskiem myszy, gdy chcesz wstawić fragment kodu.</span><span class="sxs-lookup"><span data-stu-id="ef39b-527">Right-click where you want to insert the code snippet.</span></span>

1. <span data-ttu-id="ef39b-528">Wybierz **Wstaw fragment kodu** następuje **Moje fragmenty kodu**.</span><span class="sxs-lookup"><span data-stu-id="ef39b-528">Select **Insert Snippet** followed by **My Code Snippets**.</span></span>
2. <span data-ttu-id="ef39b-529">Wybierz odpowiedni fragment z listy, klikając go.</span><span class="sxs-lookup"><span data-stu-id="ef39b-529">Pick the relevant snippet from the list, by clicking on it.</span></span>

<span data-ttu-id="ef39b-530">![Kliknij prawym przyciskiem myszy, które chcesz wstawić fragment kodu i wybierz opcję Wstaw fragment kodu](aspnet-mvc-4-models-and-data-access/_static/image55.png "kliknij prawym przyciskiem myszy, które chcesz wstawić fragment kodu i wybierz opcję Wstaw fragment kodu")</span><span class="sxs-lookup"><span data-stu-id="ef39b-530">![Right-click where you want to insert the code snippet and select Insert Snippet](aspnet-mvc-4-models-and-data-access/_static/image55.png "Right-click where you want to insert the code snippet and select Insert Snippet")</span></span>

<span data-ttu-id="ef39b-531">*Kliknij prawym przyciskiem myszy, które chcesz wstawić fragment kodu i wybierz opcję Wstaw fragment kodu*</span><span class="sxs-lookup"><span data-stu-id="ef39b-531">*Right-click where you want to insert the code snippet and select Insert Snippet*</span></span>

<span data-ttu-id="ef39b-532">![Wybierz odpowiedni fragment z listy, klikając go](aspnet-mvc-4-models-and-data-access/_static/image56.png "wybierz odpowiedni fragment z listy, klikając go")</span><span class="sxs-lookup"><span data-stu-id="ef39b-532">![Pick the relevant snippet from the list, by clicking on it](aspnet-mvc-4-models-and-data-access/_static/image56.png "Pick the relevant snippet from the list, by clicking on it")</span></span>

<span data-ttu-id="ef39b-533">*Wybierz odpowiedni fragment z listy, klikając go*</span><span class="sxs-lookup"><span data-stu-id="ef39b-533">*Pick the relevant snippet from the list, by clicking on it*</span></span>
