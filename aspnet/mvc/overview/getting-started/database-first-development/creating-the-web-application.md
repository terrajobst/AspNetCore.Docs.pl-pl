---
uid: mvc/overview/getting-started/database-first-development/creating-the-web-application
title: 'Samouczek: Tworzenie aplikacji sieci Web i modeli danych na platformie EF bazy danych pierwszy z platformą ASP.NET MVC'
description: Ten artykuł koncentruje się na temat tworzenia aplikacji sieci web i generowania modeli danych, w oparciu o tabelach bazy danych.
author: Rick-Anderson
ms.author: riande
ms.date: 01/23/2019
ms.topic: tutorial
ms.assetid: bc8f2bd5-ff57-4dcd-8418-a5bd517d8953
msc.legacyurl: /mvc/overview/getting-started/database-first-development/creating-the-web-application
msc.type: authoredcontent
ms.openlocfilehash: 095d355866c9ab8fba3759f3e05e2a521992f3d6
ms.sourcegitcommit: d5223cf6a2cf80b4f5dc54169b0e376d493d2d3a
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/24/2019
ms.locfileid: "54889772"
---
# <a name="tutorial-create-the-the-web-application-and-data-models-for-ef-database-first-with-aspnet-mvc"></a><span data-ttu-id="15f8a-103">Samouczek: Tworzenie aplikacji sieci Web i modeli danych na platformie EF bazy danych pierwszy z platformą ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="15f8a-103">Tutorial: Create the the Web Application and Data Models for EF Database First with ASP.NET MVC</span></span>

 <span data-ttu-id="15f8a-104">Za pomocą MVC, platformy Entity Framework i funkcja tworzenia szkieletu ASP.NET, można utworzyć aplikację internetową, która zapewnia interfejs do istniejącej bazy danych.</span><span class="sxs-lookup"><span data-stu-id="15f8a-104">Using MVC, Entity Framework, and ASP.NET Scaffolding, you can create a web application that provides an interface to an existing database.</span></span> <span data-ttu-id="15f8a-105">W tej serii samouczków pokazano, jak automatycznie wygenerować kod, który pozwala użytkownikom na wyświetlanie, edytowanie, tworzenie i usuwanie danych, która znajduje się w tabeli bazy danych.</span><span class="sxs-lookup"><span data-stu-id="15f8a-105">This tutorial series shows you how to automatically generate code that enables users to display, edit, create, and delete data that resides in a database table.</span></span> <span data-ttu-id="15f8a-106">Wygenerowany kod odnosi się do kolumn w tabeli bazy danych.</span><span class="sxs-lookup"><span data-stu-id="15f8a-106">The generated code corresponds to the columns in the database table.</span></span>

<span data-ttu-id="15f8a-107">Ten artykuł koncentruje się na temat tworzenia aplikacji sieci web i generowania modeli danych, w oparciu o tabelach bazy danych.</span><span class="sxs-lookup"><span data-stu-id="15f8a-107">This article focuses on creating the web application, and generating the data models based on your database tables.</span></span>

<span data-ttu-id="15f8a-108">W ramach tego samouczka możesz:</span><span class="sxs-lookup"><span data-stu-id="15f8a-108">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="15f8a-109">Tworzenie aplikacji sieci web ASP.NET</span><span class="sxs-lookup"><span data-stu-id="15f8a-109">Create an ASP.NET web app</span></span>
> * <span data-ttu-id="15f8a-110">Generowanie modelu</span><span class="sxs-lookup"><span data-stu-id="15f8a-110">Generate the models</span></span>

## <a name="prerequisites"></a><span data-ttu-id="15f8a-111">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="15f8a-111">Prerequisites</span></span>

* [<span data-ttu-id="15f8a-112">Wprowadzenie do First w programie Entity Framework 6 bazy danych za pomocą MVC 5</span><span class="sxs-lookup"><span data-stu-id="15f8a-112">Getting started with Entity Framework 6 Database First using MVC 5</span></span>](setting-up-database.md)

## <a name="create-an-aspnet-web-app"></a><span data-ttu-id="15f8a-113">Tworzenie aplikacji sieci web ASP.NET</span><span class="sxs-lookup"><span data-stu-id="15f8a-113">Create an ASP.NET web app</span></span>

<span data-ttu-id="15f8a-114">W nowym rozwiązaniu lub tego samego rozwiązania co projekt bazy danych, Utwórz nowy projekt w programie Visual Studio i wybierz **aplikacji sieci Web ASP.NET** szablonu.</span><span class="sxs-lookup"><span data-stu-id="15f8a-114">In either a new solution or the same solution as the database project, create a new project in Visual Studio and select the **ASP.NET Web Application** template.</span></span> <span data-ttu-id="15f8a-115">Nadaj projektowi nazwę **ContosoSite**.</span><span class="sxs-lookup"><span data-stu-id="15f8a-115">Name the project **ContosoSite**.</span></span>

![Tworzenie projektu](creating-the-web-application/_static/image1.png)

<span data-ttu-id="15f8a-117">Kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="15f8a-117">Click **OK**.</span></span>

<span data-ttu-id="15f8a-118">W oknie Nowy projekt ASP.NET wybierz **MVC** szablonu.</span><span class="sxs-lookup"><span data-stu-id="15f8a-118">In the New ASP.NET Project window, select the **MVC** template.</span></span> <span data-ttu-id="15f8a-119">Możesz wyczyścić **Hostuj w chmurze** opcji teraz, ponieważ wdroży aplikację w chmurze później.</span><span class="sxs-lookup"><span data-stu-id="15f8a-119">You can clear the **Host in the cloud** option for now because you will deploy the application to the cloud later.</span></span> <span data-ttu-id="15f8a-120">Kliknij przycisk **OK** do tworzenia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="15f8a-120">Click **OK** to create the application.</span></span>

<span data-ttu-id="15f8a-121">Projekt zostanie utworzony przy użyciu domyślnych plików i folderów.</span><span class="sxs-lookup"><span data-stu-id="15f8a-121">The project is created with the default files and folders.</span></span>

<span data-ttu-id="15f8a-122">W tym samouczku użyjesz programu Entity Framework 6.</span><span class="sxs-lookup"><span data-stu-id="15f8a-122">In this tutorial, you will use Entity Framework 6.</span></span> <span data-ttu-id="15f8a-123">W projekcie za pomocą okna Zarządzanie pakietami NuGet, należy dokładnie wersją programu Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="15f8a-123">You can double-check the version of Entity Framework in your project through the Manage NuGet Packages window.</span></span> <span data-ttu-id="15f8a-124">Jeśli to konieczne, zaktualizuj swoją wersję programu Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="15f8a-124">If necessary, update your version of Entity Framework.</span></span>

![Pokaż wersję](creating-the-web-application/_static/image3.png)

## <a name="generate-the-models"></a><span data-ttu-id="15f8a-126">Generowanie modelu</span><span class="sxs-lookup"><span data-stu-id="15f8a-126">Generate the models</span></span>

<span data-ttu-id="15f8a-127">Teraz utworzysz modeli Entity Framework z tabel bazy danych.</span><span class="sxs-lookup"><span data-stu-id="15f8a-127">You will now create Entity Framework models from the database tables.</span></span> <span data-ttu-id="15f8a-128">Te modele są klas, które będą używane do pracy z danymi.</span><span class="sxs-lookup"><span data-stu-id="15f8a-128">These models are classes that you will use to work with the data.</span></span> <span data-ttu-id="15f8a-129">Każdy model odzwierciedla tabeli w bazie danych i zawiera właściwości, które odnoszą się do kolumn w tabeli.</span><span class="sxs-lookup"><span data-stu-id="15f8a-129">Each model mirrors a table in the database and contains properties that correspond to the columns in the table.</span></span>

<span data-ttu-id="15f8a-130">Kliknij prawym przyciskiem myszy **modeli** folder, a następnie wybierz **Dodaj** i **nowy element**.</span><span class="sxs-lookup"><span data-stu-id="15f8a-130">Right-click the **Models** folder, and select **Add** and **New Item**.</span></span>

<span data-ttu-id="15f8a-131">W oknie Dodaj nowy element, wybierz **danych** w okienku po lewej stronie i **ADO.NET Entity Data Model** spośród opcji w środkowym okienku.</span><span class="sxs-lookup"><span data-stu-id="15f8a-131">In the Add New Item window, select **Data** in the left pane and **ADO.NET Entity Data Model** from the options in the center pane.</span></span> <span data-ttu-id="15f8a-132">Nadaj nowemu plikowi modelu **ContosoModel**.</span><span class="sxs-lookup"><span data-stu-id="15f8a-132">Name the new model file **ContosoModel**.</span></span>

<span data-ttu-id="15f8a-133">Kliknij przycisk **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="15f8a-133">Click **Add**.</span></span>

<span data-ttu-id="15f8a-134">W kreatorze modelu danych jednostki, wybierz **projektancie platformy EF z bazy danych**.</span><span class="sxs-lookup"><span data-stu-id="15f8a-134">In the Entity Data Model Wizard, select **EF Designer from database**.</span></span>

<span data-ttu-id="15f8a-135">Kliknij przycisk **Dalej**.</span><span class="sxs-lookup"><span data-stu-id="15f8a-135">Click **Next**.</span></span>

<span data-ttu-id="15f8a-136">Jeśli masz połączenia z bazą danych zdefiniowanych w środowisku deweloperskim, może zostać wyświetlony jeden z tych połączeń wstępnie wybrana.</span><span class="sxs-lookup"><span data-stu-id="15f8a-136">If you have database connections defined within your development environment, you may see one of these connections pre-selected.</span></span> <span data-ttu-id="15f8a-137">Jednak należy utworzyć nowe połączenie z bazą danych utworzoną w pierwszej części tego samouczka.</span><span class="sxs-lookup"><span data-stu-id="15f8a-137">However, you want to create a new connection to the database you created in the first part of this tutorial.</span></span> <span data-ttu-id="15f8a-138">Kliknij przycisk **nowe połączenie** przycisku.</span><span class="sxs-lookup"><span data-stu-id="15f8a-138">Click the **New Connection** button.</span></span>

<span data-ttu-id="15f8a-139">W oknie dialogowym właściwości połączenia należy podać nazwę serwera lokalnego, w którym utworzono bazę danych (w tym przypadku **\Projects13 (localdb)**).</span><span class="sxs-lookup"><span data-stu-id="15f8a-139">In the Connection Properties window, provide the name of the local server where your database was created (in this case **(localdb)\Projects13**).</span></span> <span data-ttu-id="15f8a-140">Po podaniu nazwy serwera, należy wybrać ContosoUniversityData z dostępnych baz danych.</span><span class="sxs-lookup"><span data-stu-id="15f8a-140">After providing the server name, select the ContosoUniversityData from the available databases.</span></span>

![Ustawianie właściwości połączenia](creating-the-web-application/_static/image8.png)

<span data-ttu-id="15f8a-142">Kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="15f8a-142">Click **OK**.</span></span>

<span data-ttu-id="15f8a-143">Właściwości połączenia poprawne są teraz wyświetlane.</span><span class="sxs-lookup"><span data-stu-id="15f8a-143">The correct connection properties are now displayed.</span></span> <span data-ttu-id="15f8a-144">Można użyć domyślnej nazwy dla połączenia w pliku Web.Config.</span><span class="sxs-lookup"><span data-stu-id="15f8a-144">You can use the default name for connection in the Web.Config file.</span></span>

<span data-ttu-id="15f8a-145">Kliknij przycisk **Dalej**.</span><span class="sxs-lookup"><span data-stu-id="15f8a-145">Click **Next**.</span></span>

<span data-ttu-id="15f8a-146">Wybierz najnowszą wersję programu Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="15f8a-146">Select the latest version of Entity Framework.</span></span>

<span data-ttu-id="15f8a-147">Kliknij przycisk **Dalej**.</span><span class="sxs-lookup"><span data-stu-id="15f8a-147">Click **Next**.</span></span>

<span data-ttu-id="15f8a-148">Wybierz **tabel** do generowania modeli wszystkie trzy tabele.</span><span class="sxs-lookup"><span data-stu-id="15f8a-148">Select **Tables** to generate models for all three tables.</span></span>

<span data-ttu-id="15f8a-149">Kliknij przycisk **Zakończ**.</span><span class="sxs-lookup"><span data-stu-id="15f8a-149">Click **Finish**.</span></span>

<span data-ttu-id="15f8a-150">Jeśli pojawi się ostrzeżenie o zabezpieczeniach, wybierz opcję **OK** kontynuowanie działania w szablonie.</span><span class="sxs-lookup"><span data-stu-id="15f8a-150">If you receive a security warning, select **OK** to continue running the template.</span></span>

<span data-ttu-id="15f8a-151">Modele są generowane na podstawie tabel bazy danych i wyświetlony diagram pokazuje właściwości i relacje między tabelami.</span><span class="sxs-lookup"><span data-stu-id="15f8a-151">The models are generated from the database tables, and a diagram is displayed that shows the properties and relationships between the tables.</span></span>

![Diagram modelu](creating-the-web-application/_static/image11.png)

<span data-ttu-id="15f8a-153">Folder modeli teraz zawiera wiele nowych plików związanych z modeli, które zostały wygenerowane z bazy danych.</span><span class="sxs-lookup"><span data-stu-id="15f8a-153">The Models folder now includes many new files related to the models that were generated from the database.</span></span>

<span data-ttu-id="15f8a-154">**ContosoModel.Context.cs** plik zawiera klasę dziedziczącą po **DbContext** klasy, a także właściwości dla każdej klasy modelu, który odnosi się do tabeli bazy danych.</span><span class="sxs-lookup"><span data-stu-id="15f8a-154">The **ContosoModel.Context.cs** file contains a class that derives from the **DbContext** class, and provides a property for each model class that corresponds to a database table.</span></span> <span data-ttu-id="15f8a-155">**Course.cs**, **Enrollment.cs**, i **Student.cs** pliki zawierają klasy modeli, które reprezentują tabele bazy danych.</span><span class="sxs-lookup"><span data-stu-id="15f8a-155">The **Course.cs**, **Enrollment.cs**, and **Student.cs** files contain the model classes that represent the databases tables.</span></span> <span data-ttu-id="15f8a-156">Użyjesz klasy modelu i klasy kontekstu podczas pracy z funkcją szkieletów.</span><span class="sxs-lookup"><span data-stu-id="15f8a-156">You will use both the context class and the model classes when working with scaffolding.</span></span>

<span data-ttu-id="15f8a-157">Przed kontynuowaniem za pomocą tego samouczka, skompiluj projekt.</span><span class="sxs-lookup"><span data-stu-id="15f8a-157">Before proceeding with this tutorial, build the project.</span></span> <span data-ttu-id="15f8a-158">W następnej sekcji spowoduje wygenerowanie kodu opartego na modeli danych, ale ta sekcja nie będzie działać, jeśli projekt nie został zbudowany.</span><span class="sxs-lookup"><span data-stu-id="15f8a-158">In the next section, you will generate code based on the data models, but that section will not work if the project has not been built.</span></span>

## <a name="next-steps"></a><span data-ttu-id="15f8a-159">Następne kroki</span><span class="sxs-lookup"><span data-stu-id="15f8a-159">Next steps</span></span>

<span data-ttu-id="15f8a-160">W ramach tego samouczka możesz:</span><span class="sxs-lookup"><span data-stu-id="15f8a-160">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="15f8a-161">Utworzona aplikacja internetowa platformy ASP.NET</span><span class="sxs-lookup"><span data-stu-id="15f8a-161">Created an ASP.NET web app</span></span>
> * <span data-ttu-id="15f8a-162">Wygenerowany modele</span><span class="sxs-lookup"><span data-stu-id="15f8a-162">Generated the models</span></span>

<span data-ttu-id="15f8a-163">Przejdź do następnego artykułu, aby dowiedzieć się, jak utworzyć wygenerowanie kodu opartego na modeli danych.</span><span class="sxs-lookup"><span data-stu-id="15f8a-163">Advance to the next article to learn how to create generate code based on the data models.</span></span>
> [!div class="nextstepaction"]
> [<span data-ttu-id="15f8a-164">Generowanie widoków</span><span class="sxs-lookup"><span data-stu-id="15f8a-164">Generating views</span></span>](generating-views.md)