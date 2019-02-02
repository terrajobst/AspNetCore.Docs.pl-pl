---
uid: mvc/overview/getting-started/database-first-development/changing-the-database
title: 'Samouczek: Zmień bazę danych dla platformy EF Database First w aplikacji ASP.NET MVC'
description: Ten samouczek koncentruje się na wprowadzanie aktualizacji struktury bazy danych i propagowanie tej zmiany w całej aplikacji sieci web.
author: Rick-Anderson
ms.author: riande
ms.date: 01/28/2019
ms.topic: tutorial
ms.assetid: cfd5c083-a319-482e-8f25-5b38caa93954
msc.legacyurl: /mvc/overview/getting-started/database-first-development/changing-the-database
msc.type: authoredcontent
ms.openlocfilehash: 52cad1120908cf0d4f85770f8e2690f9415c5f56
ms.sourcegitcommit: ed76cc752966c604a795fbc56d5a71d16ded0b58
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/02/2019
ms.locfileid: "55667638"
---
# <a name="tutorial-change-the-database-for-ef-database-first-with-aspnet-mvc-app"></a><span data-ttu-id="b6ed5-103">Samouczek: Zmień bazę danych dla platformy EF Database First w aplikacji ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="b6ed5-103">Tutorial: Change the database for EF Database First with ASP.NET MVC app</span></span>

<span data-ttu-id="b6ed5-104">Za pomocą MVC, platformy Entity Framework i funkcja tworzenia szkieletu ASP.NET, można utworzyć aplikację internetową, która zapewnia interfejs do istniejącej bazy danych.</span><span class="sxs-lookup"><span data-stu-id="b6ed5-104">Using MVC, Entity Framework, and ASP.NET Scaffolding, you can create a web application that provides an interface to an existing database.</span></span> <span data-ttu-id="b6ed5-105">W tej serii samouczków pokazano, jak automatycznie wygenerować kod, który pozwala użytkownikom na wyświetlanie, edytowanie, tworzenie i usuwanie danych, która znajduje się w tabeli bazy danych.</span><span class="sxs-lookup"><span data-stu-id="b6ed5-105">This tutorial series shows you how to automatically generate code that enables users to display, edit, create, and delete data that resides in a database table.</span></span> <span data-ttu-id="b6ed5-106">Wygenerowany kod odnosi się do kolumn w tabeli bazy danych.</span><span class="sxs-lookup"><span data-stu-id="b6ed5-106">The generated code corresponds to the columns in the database table.</span></span>

<span data-ttu-id="b6ed5-107">Ten samouczek koncentruje się na wprowadzanie aktualizacji struktury bazy danych i propagowanie tej zmiany w całej aplikacji sieci web.</span><span class="sxs-lookup"><span data-stu-id="b6ed5-107">This tutorial focuses on making an update to the database structure and propagating that change throughout the web application.</span></span>

<span data-ttu-id="b6ed5-108">W ramach tego samouczka możesz:</span><span class="sxs-lookup"><span data-stu-id="b6ed5-108">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="b6ed5-109">Dodaj kolumnę</span><span class="sxs-lookup"><span data-stu-id="b6ed5-109">Add a column</span></span>
> * <span data-ttu-id="b6ed5-110">Dodaj właściwość do widoków</span><span class="sxs-lookup"><span data-stu-id="b6ed5-110">Add the property to the views</span></span>

## <a name="prerequisites"></a><span data-ttu-id="b6ed5-111">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="b6ed5-111">Prerequisites</span></span>

* [<span data-ttu-id="b6ed5-112">Generowanie widoków</span><span class="sxs-lookup"><span data-stu-id="b6ed5-112">Generating views</span></span>](generating-views.md)

## <a name="add-a-column"></a><span data-ttu-id="b6ed5-113">Dodaj kolumnę</span><span class="sxs-lookup"><span data-stu-id="b6ed5-113">Add a column</span></span>

<span data-ttu-id="b6ed5-114">Jeśli zaktualizujesz strukturę tabeli w bazie danych, należy upewnić się, Twoja zmiana jest propagowana do modelu danych, widoków i kontrolerów.</span><span class="sxs-lookup"><span data-stu-id="b6ed5-114">If you update the structure of a table in your database, you need to ensure that your change is propagated to the data model, views, and controller.</span></span>

<span data-ttu-id="b6ed5-115">W tym samouczku dodasz nową kolumnę do tabeli dla uczniów, aby zarejestrować drugie imię studenta.</span><span class="sxs-lookup"><span data-stu-id="b6ed5-115">For this tutorial, you will add a new column to the Student table to record the middle name of the student.</span></span> <span data-ttu-id="b6ed5-116">Aby dodać tę kolumnę, otwórz projekt bazy danych, a następnie otwórz plik Student.sql.</span><span class="sxs-lookup"><span data-stu-id="b6ed5-116">To add this column, open the database project, and open the Student.sql file.</span></span> <span data-ttu-id="b6ed5-117">Za pomocą projektanta lub kodu T-SQL, Dodaj kolumnę o nazwie **MiddleName** , jest NVARCHAR(50) i dopuszcza wartości NULL.</span><span class="sxs-lookup"><span data-stu-id="b6ed5-117">Through either the designer or the T-SQL code, add a column named **MiddleName** that is an NVARCHAR(50) and allows NULL values.</span></span>

<span data-ttu-id="b6ed5-118">Wdróż tę zmianę z lokalną bazą danych, uruchamiając projekt bazy danych (lub F5).</span><span class="sxs-lookup"><span data-stu-id="b6ed5-118">Deploy this change to your local database by starting your database project (or F5).</span></span> <span data-ttu-id="b6ed5-119">Nowe pole jest dodawane do tabeli.</span><span class="sxs-lookup"><span data-stu-id="b6ed5-119">The new field is added to the table.</span></span> <span data-ttu-id="b6ed5-120">Jeśli nie ma go w Eksploratorze obiektów SQL Server, kliknij przycisk Odśwież w okienku.</span><span class="sxs-lookup"><span data-stu-id="b6ed5-120">If you do not see it in the SQL Server Object Explorer, click the Refresh button in the pane.</span></span>

![Pokaż nowej kolumny](changing-the-database/_static/image2.png)

<span data-ttu-id="b6ed5-122">Nowa kolumna istnieje w tabeli bazy danych, ale ona obecnie nie istnieje w klasie modelu danych.</span><span class="sxs-lookup"><span data-stu-id="b6ed5-122">The new column exists in the database table, but it does not currently exist in the data model class.</span></span> <span data-ttu-id="b6ed5-123">Należy zaktualizować model w celu uwzględnienia nowej kolumny.</span><span class="sxs-lookup"><span data-stu-id="b6ed5-123">You must update the model to include your new column.</span></span> <span data-ttu-id="b6ed5-124">W **modeli** folder, otwórz **ContosoModel.edmx** plik, aby wyświetlić diagram modelu.</span><span class="sxs-lookup"><span data-stu-id="b6ed5-124">In the **Models** folder, open the **ContosoModel.edmx** file to display the model diagram.</span></span> <span data-ttu-id="b6ed5-125">Zwróć uwagę, model dla uczniów nie zawiera właściwości MiddleName.</span><span class="sxs-lookup"><span data-stu-id="b6ed5-125">Notice that the Student model does not contain the MiddleName property.</span></span> <span data-ttu-id="b6ed5-126">Kliknij prawym przyciskiem myszy w dowolnym miejscu na powierzchni projektowej, a następnie wybierz pozycję **Model aktualizacji z bazy danych**.</span><span class="sxs-lookup"><span data-stu-id="b6ed5-126">Right-click anywhere on the design surface, and select **Update Model from Database**.</span></span>

<span data-ttu-id="b6ed5-127">W Kreatorze aktualizacji wybierz **Odśwież** , a następnie wybierz pozycję **tabel** > **dbo** > **uczniów**.</span><span class="sxs-lookup"><span data-stu-id="b6ed5-127">In the Update Wizard, select the **Refresh** tab and then select **Tables** > **dbo** > **Student**.</span></span> <span data-ttu-id="b6ed5-128">Kliknij przycisk **Zakończ**.</span><span class="sxs-lookup"><span data-stu-id="b6ed5-128">Click **Finish**.</span></span>

<span data-ttu-id="b6ed5-129">Po zakończeniu procesu aktualizacji diagram zawiera nowe **MiddleName** właściwości.</span><span class="sxs-lookup"><span data-stu-id="b6ed5-129">After the update process is finished, the database diagram includes the new **MiddleName** property.</span></span> <span data-ttu-id="b6ed5-130">Zapisz **ContosoModel.edmx** pliku.</span><span class="sxs-lookup"><span data-stu-id="b6ed5-130">Save the **ContosoModel.edmx** file.</span></span> <span data-ttu-id="b6ed5-131">Musisz zapisać ten plik nowej właściwości są propagowane do **Student.cs** klasy.</span><span class="sxs-lookup"><span data-stu-id="b6ed5-131">You must save this file for the new property to be propagated to the **Student.cs** class.</span></span> <span data-ttu-id="b6ed5-132">Zostały teraz zaktualizowane bazy danych i modelu.</span><span class="sxs-lookup"><span data-stu-id="b6ed5-132">You have now updated the database and the model.</span></span>

<span data-ttu-id="b6ed5-133">Skompiluj rozwiązanie.</span><span class="sxs-lookup"><span data-stu-id="b6ed5-133">Build the solution.</span></span>

## <a name="add-the-property-to-the-views"></a><span data-ttu-id="b6ed5-134">Dodaj właściwość do widoków</span><span class="sxs-lookup"><span data-stu-id="b6ed5-134">Add the property to the views</span></span>

<span data-ttu-id="b6ed5-135">Niestety widoki nadal nie zawierają nową właściwość.</span><span class="sxs-lookup"><span data-stu-id="b6ed5-135">Unfortunately, the views still do not contain the new property.</span></span> <span data-ttu-id="b6ed5-136">Aby zaktualizować widoki dostępne są dwie opcje — można ponownie wygenerować widoki dodając ponownie funkcją szkieletów dla klasy dla uczniów, lub można ręcznie dodawać nowych właściwości do istniejących widoków.</span><span class="sxs-lookup"><span data-stu-id="b6ed5-136">To update the views you have two options - you can either re-generate the views by once again adding scaffolding for the Student class, or you can manually add the new property to your existing views.</span></span> <span data-ttu-id="b6ed5-137">W tym samouczku dodasz szkieletu ponownie, ponieważ nie wprowadzono zmian dostosowane widoki generowane automatycznie.</span><span class="sxs-lookup"><span data-stu-id="b6ed5-137">In this tutorial, you will add the scaffolding again because you have not made any customized changes to the automatically-generated views.</span></span> <span data-ttu-id="b6ed5-138">Należy rozważyć, ręcznie dodając właściwość, jeśli wprowadzono zmiany do widoków i nie chcesz utracić te zmiany.</span><span class="sxs-lookup"><span data-stu-id="b6ed5-138">You might consider manually adding the property when you have made changes to the views and do not want to lose those changes.</span></span>

<span data-ttu-id="b6ed5-139">Aby upewnić się, widoki są ponownie tworzone, Usuń **studentów** folderze **widoków**i Usuń **StudentsController**.</span><span class="sxs-lookup"><span data-stu-id="b6ed5-139">To ensure the views are re-created, delete the **Students** folder under **Views**, and delete the **StudentsController**.</span></span> <span data-ttu-id="b6ed5-140">Następnie kliknij prawym przyciskiem myszy **kontrolerów** folderze i Dodaj funkcją szkieletów dla **uczniów** modelu.</span><span class="sxs-lookup"><span data-stu-id="b6ed5-140">Then, right-click the **Controllers** folder and add scaffolding for the **Student** model.</span></span> <span data-ttu-id="b6ed5-141">Ponownie, nazwy kontrolera **StudentsController**.</span><span class="sxs-lookup"><span data-stu-id="b6ed5-141">Again, name the controller **StudentsController**.</span></span> <span data-ttu-id="b6ed5-142">Wybierz pozycję **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="b6ed5-142">Select **Add**.</span></span>

<span data-ttu-id="b6ed5-143">Ponownie skompiluj rozwiązanie.</span><span class="sxs-lookup"><span data-stu-id="b6ed5-143">Build the solution again.</span></span> <span data-ttu-id="b6ed5-144">Widoki zawierają teraz właściwość MiddleName.</span><span class="sxs-lookup"><span data-stu-id="b6ed5-144">The views now contain the MiddleName property.</span></span>

![Pokaż drugie imię](changing-the-database/_static/image5.png)

## <a name="next-steps"></a><span data-ttu-id="b6ed5-146">Następne kroki</span><span class="sxs-lookup"><span data-stu-id="b6ed5-146">Next steps</span></span>

<span data-ttu-id="b6ed5-147">W ramach tego samouczka możesz:</span><span class="sxs-lookup"><span data-stu-id="b6ed5-147">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="b6ed5-148">Dodano kolumnę</span><span class="sxs-lookup"><span data-stu-id="b6ed5-148">Added a column</span></span>
> * <span data-ttu-id="b6ed5-149">Dodaje właściwość do widoków</span><span class="sxs-lookup"><span data-stu-id="b6ed5-149">Added the property to the views</span></span>

<span data-ttu-id="b6ed5-150">Przejdź do następnego samouczka, aby dowiedzieć się, jak dostosować widok do wyświetlania szczegółów rekordu dla uczniów.</span><span class="sxs-lookup"><span data-stu-id="b6ed5-150">Advance to the next tutorial to learn how to customize the view for showing details about a student record.</span></span>
> [!div class="nextstepaction"]
> [<span data-ttu-id="b6ed5-151">Dostosowywanie widoku</span><span class="sxs-lookup"><span data-stu-id="b6ed5-151">Customize a view</span></span>](customizing-a-view.md)