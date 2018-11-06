---
uid: mvc/overview/getting-started/database-first-development/creating-the-web-application
title: 'EF bazy danych, najpierw z platformą ASP.NET MVC: tworzenie modeli danych i aplikacji sieci Web | Dokumentacja firmy Microsoft'
author: Rick-Anderson
description: Za pomocą MVC, platformy Entity Framework i funkcja tworzenia szkieletu ASP.NET, można utworzyć aplikację internetową, która zapewnia interfejs do istniejącej bazy danych. Ten samouczek seri...
ms.author: riande
ms.date: 10/01/2014
ms.assetid: bc8f2bd5-ff57-4dcd-8418-a5bd517d8953
msc.legacyurl: /mvc/overview/getting-started/database-first-development/creating-the-web-application
msc.type: authoredcontent
ms.openlocfilehash: 6679b61326bd016481d96a4b5d58ec006f86b633
ms.sourcegitcommit: 2d3e5422d530203efdaf2014d1d7df31f88d08d0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/05/2018
ms.locfileid: "51020800"
---
<a name="ef-database-first-with-aspnet-mvc-creating-the-web-application-and-data-models"></a><span data-ttu-id="cc49b-104">EF bazy danych, najpierw z platformą ASP.NET MVC: tworzenie modeli danych i aplikacji sieci Web</span><span class="sxs-lookup"><span data-stu-id="cc49b-104">EF Database First with ASP.NET MVC: Creating the Web Application and Data Models</span></span>
====================
<span data-ttu-id="cc49b-105">przez [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="cc49b-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="cc49b-106">Za pomocą MVC, platformy Entity Framework i funkcja tworzenia szkieletu ASP.NET, można utworzyć aplikację internetową, która zapewnia interfejs do istniejącej bazy danych.</span><span class="sxs-lookup"><span data-stu-id="cc49b-106">Using MVC, Entity Framework, and ASP.NET Scaffolding, you can create a web application that provides an interface to an existing database.</span></span> <span data-ttu-id="cc49b-107">W tej serii samouczków pokazano, jak automatycznie wygenerować kod, który pozwala użytkownikom na wyświetlanie, edytowanie, tworzenie i usuwanie danych, która znajduje się w tabeli bazy danych.</span><span class="sxs-lookup"><span data-stu-id="cc49b-107">This tutorial series shows you how to automatically generate code that enables users to display, edit, create, and delete data that resides in a database table.</span></span> <span data-ttu-id="cc49b-108">Wygenerowany kod odnosi się do kolumn w tabeli bazy danych.</span><span class="sxs-lookup"><span data-stu-id="cc49b-108">The generated code corresponds to the columns in the database table.</span></span>
> 
> <span data-ttu-id="cc49b-109">Ta część serii koncentruje się na temat tworzenia aplikacji sieci web i generowania modeli danych, w oparciu o tabelach bazy danych.</span><span class="sxs-lookup"><span data-stu-id="cc49b-109">This part of the series focuses on creating the web application, and generating the data models based on your database tables.</span></span>


## <a name="create-a-new-aspnet-web-application"></a><span data-ttu-id="cc49b-110">Utwórz nową aplikację sieci Web platformy ASP.NET</span><span class="sxs-lookup"><span data-stu-id="cc49b-110">Create a new ASP.NET Web Application</span></span>

<span data-ttu-id="cc49b-111">W nowym rozwiązaniu lub tego samego rozwiązania co projekt bazy danych, Utwórz nowy projekt w programie Visual Studio i wybierz **aplikacji sieci Web ASP.NET** szablonu.</span><span class="sxs-lookup"><span data-stu-id="cc49b-111">In either a new solution or the same solution as the database project, create a new project in Visual Studio and select the **ASP.NET Web Application** template.</span></span> <span data-ttu-id="cc49b-112">Nadaj projektowi nazwę **ContosoSite**.</span><span class="sxs-lookup"><span data-stu-id="cc49b-112">Name the project **ContosoSite**.</span></span>

![Tworzenie projektu](creating-the-web-application/_static/image1.png)

<span data-ttu-id="cc49b-114">Kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="cc49b-114">Click **OK**.</span></span>

<span data-ttu-id="cc49b-115">W oknie Nowy projekt ASP.NET wybierz **MVC** szablonu.</span><span class="sxs-lookup"><span data-stu-id="cc49b-115">In the New ASP.NET Project window, select the **MVC** template.</span></span> <span data-ttu-id="cc49b-116">Możesz wyczyścić **Hostuj w chmurze** opcji teraz, ponieważ wdroży aplikację w chmurze później.</span><span class="sxs-lookup"><span data-stu-id="cc49b-116">You can clear the **Host in the cloud** option for now because you will deploy the application to the cloud later.</span></span> <span data-ttu-id="cc49b-117">Kliknij przycisk **OK** do tworzenia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="cc49b-117">Click **OK** to create the application.</span></span>

![Wybierz szablon mvc](creating-the-web-application/_static/image2.png)

<span data-ttu-id="cc49b-119">Projekt zostanie utworzony przy użyciu domyślnych plików i folderów.</span><span class="sxs-lookup"><span data-stu-id="cc49b-119">The project is created with the default files and folders.</span></span>

<span data-ttu-id="cc49b-120">W tym samouczku użyjesz programu Entity Framework 6.</span><span class="sxs-lookup"><span data-stu-id="cc49b-120">In this tutorial, you will use Entity Framework 6.</span></span> <span data-ttu-id="cc49b-121">W projekcie za pomocą okna Zarządzanie pakietami NuGet, należy dokładnie wersją programu Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="cc49b-121">You can double-check the version of Entity Framework in your project through the Manage NuGet Packages window.</span></span> <span data-ttu-id="cc49b-122">Jeśli to konieczne, zaktualizuj swoją wersję programu Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="cc49b-122">If necessary, update your version of Entity Framework.</span></span>

![Pokaż wersję](creating-the-web-application/_static/image3.png)

## <a name="generate-the-models"></a><span data-ttu-id="cc49b-124">Generowanie modelu</span><span class="sxs-lookup"><span data-stu-id="cc49b-124">Generate the models</span></span>

<span data-ttu-id="cc49b-125">Teraz utworzysz modeli Entity Framework z tabel bazy danych.</span><span class="sxs-lookup"><span data-stu-id="cc49b-125">You will now create Entity Framework models from the database tables.</span></span> <span data-ttu-id="cc49b-126">Te modele są klas, które będą używane do pracy z danymi.</span><span class="sxs-lookup"><span data-stu-id="cc49b-126">These models are classes that you will use to work with the data.</span></span> <span data-ttu-id="cc49b-127">Każdy model odzwierciedla tabeli w bazie danych i zawiera właściwości, które odnoszą się do kolumn w tabeli.</span><span class="sxs-lookup"><span data-stu-id="cc49b-127">Each model mirrors a table in the database and contains properties that correspond to the columns in the table.</span></span>

<span data-ttu-id="cc49b-128">Kliknij prawym przyciskiem myszy **modeli** folder, a następnie wybierz **Dodaj** i **nowy element**.</span><span class="sxs-lookup"><span data-stu-id="cc49b-128">Right-click the **Models** folder, and select **Add** and **New Item**.</span></span>

![Dodaj nowy element](creating-the-web-application/_static/image4.png)

<span data-ttu-id="cc49b-130">W oknie Dodaj nowy element, wybierz **danych** w okienku po lewej stronie i **ADO.NET Entity Data Model** spośród opcji w środkowym okienku.</span><span class="sxs-lookup"><span data-stu-id="cc49b-130">In the Add New Item window, select **Data** in the left pane and **ADO.NET Entity Data Model** from the options in the center pane.</span></span> <span data-ttu-id="cc49b-131">Nadaj nowemu plikowi modelu **ContosoModel**.</span><span class="sxs-lookup"><span data-stu-id="cc49b-131">Name the new model file **ContosoModel**.</span></span>

![Tworzenie modelu](creating-the-web-application/_static/image5.png)

<span data-ttu-id="cc49b-133">Kliknij przycisk **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="cc49b-133">Click **Add**.</span></span>

<span data-ttu-id="cc49b-134">W kreatorze modelu danych jednostki, wybierz **projektancie platformy EF z bazy danych**.</span><span class="sxs-lookup"><span data-stu-id="cc49b-134">In the Entity Data Model Wizard, select **EF Designer from database**.</span></span>

![Generuj z bazy danych](creating-the-web-application/_static/image6.png)

<span data-ttu-id="cc49b-136">Kliknij przycisk **Dalej**.</span><span class="sxs-lookup"><span data-stu-id="cc49b-136">Click **Next**.</span></span>

<span data-ttu-id="cc49b-137">Jeśli masz połączenia z bazą danych zdefiniowanych w środowisku deweloperskim, może zostać wyświetlony jeden z tych połączeń wstępnie wybrana.</span><span class="sxs-lookup"><span data-stu-id="cc49b-137">If you have database connections defined within your development environment, you may see one of these connections pre-selected.</span></span> <span data-ttu-id="cc49b-138">Jednak należy utworzyć nowe połączenie z bazą danych utworzoną w pierwszej części tego samouczka.</span><span class="sxs-lookup"><span data-stu-id="cc49b-138">However, you want to create a new connection to the database you created in the first part of this tutorial.</span></span> <span data-ttu-id="cc49b-139">Kliknij przycisk **nowe połączenie** przycisku.</span><span class="sxs-lookup"><span data-stu-id="cc49b-139">Click the **New Connection** button.</span></span>

![nawiązać połączenie z bazą danych](creating-the-web-application/_static/image7.png)

<span data-ttu-id="cc49b-141">W oknie dialogowym właściwości połączenia należy podać nazwę serwera lokalnego, w którym utworzono bazę danych (w tym przypadku **\ProjectsV12 (localdb)**).</span><span class="sxs-lookup"><span data-stu-id="cc49b-141">In the Connection Properties window, provide the name of the local server where your database was created (in this case **(localdb)\ProjectsV12**).</span></span> <span data-ttu-id="cc49b-142">Po podaniu nazwy serwera, należy wybrać ContosoUniversityData z dostępnych baz danych.</span><span class="sxs-lookup"><span data-stu-id="cc49b-142">After providing the server name, select the ContosoUniversityData from the available databases.</span></span>

![Ustawianie właściwości połączenia](creating-the-web-application/_static/image8.png)

<span data-ttu-id="cc49b-144">Kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="cc49b-144">Click **OK**.</span></span>

<span data-ttu-id="cc49b-145">Właściwości połączenia poprawne są teraz wyświetlane.</span><span class="sxs-lookup"><span data-stu-id="cc49b-145">The correct connection properties are now displayed.</span></span> <span data-ttu-id="cc49b-146">Można użyć domyślnej nazwy dla połączenia w pliku Web.Config</span><span class="sxs-lookup"><span data-stu-id="cc49b-146">You can use the default name for connection in the Web.Config file</span></span>

![Ustawienia połączenia](creating-the-web-application/_static/image9.png)

<span data-ttu-id="cc49b-148">Kliknij przycisk **Dalej**.</span><span class="sxs-lookup"><span data-stu-id="cc49b-148">Click **Next**.</span></span>

<span data-ttu-id="cc49b-149">Wybierz **tabel** do generowania modeli wszystkie trzy tabele.</span><span class="sxs-lookup"><span data-stu-id="cc49b-149">Select **Tables** to generate models for all three tables.</span></span>

![Wybierz tabele](creating-the-web-application/_static/image10.png)

<span data-ttu-id="cc49b-151">Kliknij przycisk **Zakończ**.</span><span class="sxs-lookup"><span data-stu-id="cc49b-151">Click **Finish**.</span></span>

<span data-ttu-id="cc49b-152">Jeśli pojawi się ostrzeżenie o zabezpieczeniach, wybierz opcję **OK** kontynuowanie działania w szablonie.</span><span class="sxs-lookup"><span data-stu-id="cc49b-152">If you receive a security warning, select **OK** to continue running the template.</span></span>

<span data-ttu-id="cc49b-153">Modele są generowane na podstawie tabel bazy danych i wyświetlony diagram pokazuje właściwości i relacje między tabelami.</span><span class="sxs-lookup"><span data-stu-id="cc49b-153">The models are generated from the database tables, and a diagram is displayed that shows the properties and relationships between the tables.</span></span>

![Diagram modelu](creating-the-web-application/_static/image11.png)

<span data-ttu-id="cc49b-155">Folder modeli teraz zawiera wiele nowych plików związanych z modeli, które zostały wygenerowane z bazy danych.</span><span class="sxs-lookup"><span data-stu-id="cc49b-155">The Models folder now includes many new files related to the models that were generated from the database.</span></span>

![Pokaż nowe pliki modelu](creating-the-web-application/_static/image12.png)

<span data-ttu-id="cc49b-157">**ContosoModel.Context.cs** plik zawiera klasę dziedziczącą po **DbContext** klasy, a także właściwości dla każdej klasy modelu, który odnosi się do tabeli bazy danych.</span><span class="sxs-lookup"><span data-stu-id="cc49b-157">The **ContosoModel.Context.cs** file contains a class that derives from the **DbContext** class, and provides a property for each model class that corresponds to a database table.</span></span> <span data-ttu-id="cc49b-158">**Course.cs**, **Enrollment.cs**, i **Student.cs** pliki zawierają klasy modeli, które reprezentują tabele bazy danych.</span><span class="sxs-lookup"><span data-stu-id="cc49b-158">The **Course.cs**, **Enrollment.cs**, and **Student.cs** files contain the model classes that represent the databases tables.</span></span> <span data-ttu-id="cc49b-159">Użyjesz klasy modelu i klasy kontekstu podczas pracy z funkcją szkieletów.</span><span class="sxs-lookup"><span data-stu-id="cc49b-159">You will use both the context class and the model classes when working with scaffolding.</span></span>

<span data-ttu-id="cc49b-160">Przed kontynuowaniem za pomocą tego samouczka, skompiluj projekt.</span><span class="sxs-lookup"><span data-stu-id="cc49b-160">Before proceeding with this tutorial, build the project.</span></span> <span data-ttu-id="cc49b-161">W następnej sekcji spowoduje wygenerowanie kodu opartego na modeli danych, ale ta sekcja nie będzie działać, jeśli projekt nie został zbudowany.</span><span class="sxs-lookup"><span data-stu-id="cc49b-161">In the next section, you will generate code based on the data models, but that section will not work if the project has not been built.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="cc49b-162">[Poprzednie](setting-up-database.md)
> [dalej](generating-views.md)</span><span class="sxs-lookup"><span data-stu-id="cc49b-162">[Previous](setting-up-database.md)
[Next](generating-views.md)</span></span>
