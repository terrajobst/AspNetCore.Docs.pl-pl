---
uid: mvc/overview/getting-started/database-first-development/changing-the-database
title: 'EF bazy danych, najpierw z platformą ASP.NET MVC: zmienianie bazy danych | Dokumentacja firmy Microsoft'
author: Rick-Anderson
description: Za pomocą MVC, platformy Entity Framework i funkcja tworzenia szkieletu ASP.NET, można utworzyć aplikację internetową, która zapewnia interfejs do istniejącej bazy danych. Ten samouczek seri...
ms.author: riande
ms.date: 10/01/2014
ms.assetid: cfd5c083-a319-482e-8f25-5b38caa93954
msc.legacyurl: /mvc/overview/getting-started/database-first-development/changing-the-database
msc.type: authoredcontent
ms.openlocfilehash: 8d0eff37ced757cd8be74f8171c9aaa430940010
ms.sourcegitcommit: 2d3e5422d530203efdaf2014d1d7df31f88d08d0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/05/2018
ms.locfileid: "51021277"
---
<a name="ef-database-first-with-aspnet-mvc-changing-the-database"></a><span data-ttu-id="080ba-104">EF bazy danych, najpierw z platformą ASP.NET MVC: zmienianie bazy danych</span><span class="sxs-lookup"><span data-stu-id="080ba-104">EF Database First with ASP.NET MVC: Changing the Database</span></span>
====================
<span data-ttu-id="080ba-105">przez [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="080ba-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="080ba-106">Za pomocą MVC, platformy Entity Framework i funkcja tworzenia szkieletu ASP.NET, można utworzyć aplikację internetową, która zapewnia interfejs do istniejącej bazy danych.</span><span class="sxs-lookup"><span data-stu-id="080ba-106">Using MVC, Entity Framework, and ASP.NET Scaffolding, you can create a web application that provides an interface to an existing database.</span></span> <span data-ttu-id="080ba-107">W tej serii samouczków pokazano, jak automatycznie wygenerować kod, który pozwala użytkownikom na wyświetlanie, edytowanie, tworzenie i usuwanie danych, która znajduje się w tabeli bazy danych.</span><span class="sxs-lookup"><span data-stu-id="080ba-107">This tutorial series shows you how to automatically generate code that enables users to display, edit, create, and delete data that resides in a database table.</span></span> <span data-ttu-id="080ba-108">Wygenerowany kod odnosi się do kolumn w tabeli bazy danych.</span><span class="sxs-lookup"><span data-stu-id="080ba-108">The generated code corresponds to the columns in the database table.</span></span>
> 
> <span data-ttu-id="080ba-109">Ta część serii koncentruje się na wprowadzanie aktualizacji struktury bazy danych i propagowanie tej zmiany w całej aplikacji sieci web.</span><span class="sxs-lookup"><span data-stu-id="080ba-109">This part of the series focuses on making an update to the database structure and propagating that change throughout the web application.</span></span>


## <a name="add-a-column"></a><span data-ttu-id="080ba-110">Dodaj kolumnę</span><span class="sxs-lookup"><span data-stu-id="080ba-110">Add a column</span></span>

<span data-ttu-id="080ba-111">Jeśli zaktualizujesz strukturę tabeli w bazie danych, należy upewnić się, Twoja zmiana jest propagowana do modelu danych, widoków i kontrolerów.</span><span class="sxs-lookup"><span data-stu-id="080ba-111">If you update the structure of a table in your database, you need to ensure that your change is propagated to the data model, views, and controller.</span></span>

<span data-ttu-id="080ba-112">W tym samouczku dodasz nową kolumnę do tabeli dla uczniów, aby zarejestrować drugie imię studenta.</span><span class="sxs-lookup"><span data-stu-id="080ba-112">For this tutorial, you will add a new column to the Student table to record the middle name of the student.</span></span> <span data-ttu-id="080ba-113">Aby dodać tę kolumnę, otwórz projekt bazy danych, a następnie otwórz plik Student.sql.</span><span class="sxs-lookup"><span data-stu-id="080ba-113">To add this column, open the database project, and open the Student.sql file.</span></span> <span data-ttu-id="080ba-114">Za pomocą projektanta lub kodu T-SQL, Dodaj kolumnę o nazwie **MiddleName** , jest NVARCHAR(50) i dopuszcza wartości NULL.</span><span class="sxs-lookup"><span data-stu-id="080ba-114">Through either the designer or the T-SQL code, add a column named **MiddleName** that is an NVARCHAR(50) and allows NULL values.</span></span>

![Dodaj drugie imię](changing-the-database/_static/image1.png)

<span data-ttu-id="080ba-116">Wdróż tę zmianę z lokalną bazą danych, uruchamiając projekt bazy danych (lub F5).</span><span class="sxs-lookup"><span data-stu-id="080ba-116">Deploy this change to your local database by starting your database project (or F5).</span></span> <span data-ttu-id="080ba-117">Nowe pole jest dodawane do tabeli.</span><span class="sxs-lookup"><span data-stu-id="080ba-117">The new field is added to the table.</span></span> <span data-ttu-id="080ba-118">Jeśli nie ma go w Eksploratorze obiektów SQL Server, kliknij przycisk Odśwież w okienku.</span><span class="sxs-lookup"><span data-stu-id="080ba-118">If you do not see it in the SQL Server Object Explorer, click the Refresh button in the pane.</span></span>

![Pokaż nowej kolumny](changing-the-database/_static/image2.png)

<span data-ttu-id="080ba-120">Nowa kolumna istnieje w tabeli bazy danych, ale ona obecnie nie istnieje w klasie modelu danych.</span><span class="sxs-lookup"><span data-stu-id="080ba-120">The new column exists in the database table, but it does not currently exist in the data model class.</span></span> <span data-ttu-id="080ba-121">Należy zaktualizować model w celu uwzględnienia nowej kolumny.</span><span class="sxs-lookup"><span data-stu-id="080ba-121">You must update the model to include your new column.</span></span> <span data-ttu-id="080ba-122">W **modeli** folder, otwórz **ContosoModel.edmx** plik, aby wyświetlić diagram modelu.</span><span class="sxs-lookup"><span data-stu-id="080ba-122">In the **Models** folder, open the **ContosoModel.edmx** file to display the model diagram.</span></span> <span data-ttu-id="080ba-123">Zwróć uwagę, model dla uczniów nie zawiera właściwości MiddleName.</span><span class="sxs-lookup"><span data-stu-id="080ba-123">Notice that the Student model does not contain the MiddleName property.</span></span> <span data-ttu-id="080ba-124">Kliknij prawym przyciskiem myszy w dowolnym miejscu na powierzchni projektowej, a następnie wybierz pozycję **Model aktualizacji z bazy danych**.</span><span class="sxs-lookup"><span data-stu-id="080ba-124">Right-click anywhere on the design surface, and select **Update Model from Database**.</span></span>

![aktualizowanie modelu](changing-the-database/_static/image3.png)

<span data-ttu-id="080ba-126">W Kreatorze aktualizacji wybierz **Odśwież** kartę i **uczniów** tabeli.</span><span class="sxs-lookup"><span data-stu-id="080ba-126">In the Update Wizard, select the **Refresh** tab and the **Student** table.</span></span>

![Kreator aktualizacji](changing-the-database/_static/image4.png)

<span data-ttu-id="080ba-128">Kliknij przycisk **Zakończ**.</span><span class="sxs-lookup"><span data-stu-id="080ba-128">Click **Finish**.</span></span>

<span data-ttu-id="080ba-129">Po zakończeniu procesu aktualizacji diagram zawiera nowe **MiddleName** właściwości.</span><span class="sxs-lookup"><span data-stu-id="080ba-129">After the update process is finished, the database diagram includes the new **MiddleName** property.</span></span> <span data-ttu-id="080ba-130">Zapisz **ContosoModel.edmx** pliku.</span><span class="sxs-lookup"><span data-stu-id="080ba-130">Save the **ContosoModel.edmx** file.</span></span> <span data-ttu-id="080ba-131">Musisz zapisać ten plik nowej właściwości są propagowane do **Student.cs** klasy.</span><span class="sxs-lookup"><span data-stu-id="080ba-131">You must save this file for the new property to be propagated to the **Student.cs** class.</span></span> <span data-ttu-id="080ba-132">Zostały teraz zaktualizowane bazy danych i modelu.</span><span class="sxs-lookup"><span data-stu-id="080ba-132">You have now updated the database and the model.</span></span>

<span data-ttu-id="080ba-133">Skompiluj rozwiązanie.</span><span class="sxs-lookup"><span data-stu-id="080ba-133">Build the solution.</span></span>

<span data-ttu-id="080ba-134">Niestety widoki nadal nie zawierają nową właściwość.</span><span class="sxs-lookup"><span data-stu-id="080ba-134">Unfortunately, the views still do not contain the new property.</span></span> <span data-ttu-id="080ba-135">Aby zaktualizować widoki dostępne są dwie opcje — można ponownie wygenerować widoki dodając ponownie funkcją szkieletów dla klasy dla uczniów, lub można ręcznie dodawać nowych właściwości do istniejących widoków.</span><span class="sxs-lookup"><span data-stu-id="080ba-135">To update the views you have two options - you can either re-generate the views by once again adding scaffolding for the Student class, or you can manually add the new property to your existing views.</span></span> <span data-ttu-id="080ba-136">W tym samouczku dodasz szkieletu ponownie, ponieważ nie wprowadzono zmian dostosowane widoki generowane automatycznie.</span><span class="sxs-lookup"><span data-stu-id="080ba-136">In this tutorial, you will add the scaffolding again because you have not made any customized changes to the automatically-generated views.</span></span> <span data-ttu-id="080ba-137">Należy rozważyć, ręcznie dodając właściwość, jeśli wprowadzono zmiany do widoków i nie chcesz utracić te zmiany.</span><span class="sxs-lookup"><span data-stu-id="080ba-137">You might consider manually adding the property when you have made changes to the views and do not want to lose those changes.</span></span>

<span data-ttu-id="080ba-138">Aby upewnić się, widoki są ponownie tworzone, Usuń **studentów** folderze **widoków**i Usuń **StudentsController**.</span><span class="sxs-lookup"><span data-stu-id="080ba-138">To ensure the views are re-created, delete the **Students** folder under **Views**, and delete the **StudentsController**.</span></span> <span data-ttu-id="080ba-139">Następnie kliknij prawym przyciskiem myszy **kontrolerów** folderze i Dodaj funkcją szkieletów dla **uczniów** modelu.</span><span class="sxs-lookup"><span data-stu-id="080ba-139">Then, right-click the **Controllers** folder and add scaffolding for the **Student** model.</span></span> <span data-ttu-id="080ba-140">Ponownie, nazwy kontrolera **StudentsController**.</span><span class="sxs-lookup"><span data-stu-id="080ba-140">Again, name the controller **StudentsController**.</span></span> <span data-ttu-id="080ba-141">Wybierz **OK**.</span><span class="sxs-lookup"><span data-stu-id="080ba-141">Select **OK**.</span></span>

<span data-ttu-id="080ba-142">Widoki zawierają teraz właściwość MiddleName.</span><span class="sxs-lookup"><span data-stu-id="080ba-142">The views now contain the MiddleName property.</span></span>

![Pokaż drugie imię](changing-the-database/_static/image5.png)

<span data-ttu-id="080ba-144">W następnej sekcji dodasz kod, aby dostosować widok do wyświetlania szczegółów rekordu dla uczniów.</span><span class="sxs-lookup"><span data-stu-id="080ba-144">In the next section, you will add code to customize the view for showing details about a student record.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="080ba-145">[Poprzednie](generating-views.md)
> [dalej](customizing-a-view.md)</span><span class="sxs-lookup"><span data-stu-id="080ba-145">[Previous](generating-views.md)
[Next](customizing-a-view.md)</span></span>
