---
uid: mvc/overview/getting-started/database-first-development/generating-views
title: 'Samouczek: Generowanie widoków dla platformy EF Database First w aplikacji ASP.NET MVC'
description: Ten artykuł koncentruje się na przy użyciu platformy ASP.NET tworzenie szkieletów widoków i kontrolerów.
author: Rick-Anderson
ms.author: riande
ms.date: 01/23/2019
ms.topic: tutorial
ms.assetid: 669367cf-8e30-4eb6-821d-10a7d9bb906c
msc.legacyurl: /mvc/overview/getting-started/database-first-development/generating-views
msc.type: authoredcontent
ms.openlocfilehash: e1f6646cdf10d293268b92f44b018709e70c0f86
ms.sourcegitcommit: d5223cf6a2cf80b4f5dc54169b0e376d493d2d3a
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/24/2019
ms.locfileid: "54889785"
---
# <a name="tutorial-generate-views-for-ef-database-first-with-aspnet-mvc-app"></a><span data-ttu-id="067cd-103">Samouczek: Generowanie widoków dla platformy EF Database First w aplikacji ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="067cd-103">Tutorial: Generate views for EF Database First with ASP.NET MVC app</span></span>

<span data-ttu-id="067cd-104">Za pomocą MVC, platformy Entity Framework i funkcja tworzenia szkieletu ASP.NET, można utworzyć aplikację internetową, która zapewnia interfejs do istniejącej bazy danych.</span><span class="sxs-lookup"><span data-stu-id="067cd-104">Using MVC, Entity Framework, and ASP.NET Scaffolding, you can create a web application that provides an interface to an existing database.</span></span> <span data-ttu-id="067cd-105">W tej serii samouczków pokazano, jak automatycznie wygenerować kod, który pozwala użytkownikom na wyświetlanie, edytowanie, tworzenie i usuwanie danych, która znajduje się w tabeli bazy danych.</span><span class="sxs-lookup"><span data-stu-id="067cd-105">This tutorial series shows you how to automatically generate code that enables users to display, edit, create, and delete data that resides in a database table.</span></span> <span data-ttu-id="067cd-106">Wygenerowany kod odnosi się do kolumn w tabeli bazy danych.</span><span class="sxs-lookup"><span data-stu-id="067cd-106">The generated code corresponds to the columns in the database table.</span></span>

<span data-ttu-id="067cd-107">Ten artykuł koncentruje się na przy użyciu platformy ASP.NET tworzenie szkieletów widoków i kontrolerów.</span><span class="sxs-lookup"><span data-stu-id="067cd-107">This article focuses on using ASP.NET Scaffolding to generate the controllers and views.</span></span>

<span data-ttu-id="067cd-108">W ramach tego samouczka możesz:</span><span class="sxs-lookup"><span data-stu-id="067cd-108">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="067cd-109">Dodaj szkielet</span><span class="sxs-lookup"><span data-stu-id="067cd-109">Add scaffold</span></span>
> * <span data-ttu-id="067cd-110">Dodawanie łączy do nowych widoków</span><span class="sxs-lookup"><span data-stu-id="067cd-110">Add links to new views</span></span>
> * <span data-ttu-id="067cd-111">Wyświetlanie widoków dla uczniów</span><span class="sxs-lookup"><span data-stu-id="067cd-111">Display student views</span></span>
> * <span data-ttu-id="067cd-112">Wyświetlanie widoków rejestracji</span><span class="sxs-lookup"><span data-stu-id="067cd-112">Display enrollment views</span></span>

## <a name="prerequisite"></a><span data-ttu-id="067cd-113">Wymaganie wstępne</span><span class="sxs-lookup"><span data-stu-id="067cd-113">Prerequisite</span></span>

* [<span data-ttu-id="067cd-114">Tworzenie modeli danych i aplikacji sieci web</span><span class="sxs-lookup"><span data-stu-id="067cd-114">Create the web application and data models</span></span>](creating-the-web-application.md)

## <a name="add-scaffold"></a><span data-ttu-id="067cd-115">Dodaj szkielet</span><span class="sxs-lookup"><span data-stu-id="067cd-115">Add scaffold</span></span>

<span data-ttu-id="067cd-116">Jesteś gotowy do generowania kodu, który zapewni operacji danych w warstwie standardowa dla klasy modelu.</span><span class="sxs-lookup"><span data-stu-id="067cd-116">You are ready to generate code that will provide standard data operations for the model classes.</span></span> <span data-ttu-id="067cd-117">Możesz dodać kod przez dodawanie elementu szkieletu.</span><span class="sxs-lookup"><span data-stu-id="067cd-117">You add the code by adding a scaffold item.</span></span> <span data-ttu-id="067cd-118">Istnieje wiele opcji dla typu tworzenia szkieletów, które można dodać; w tym samouczku szkieletu obejmuje kontrolera i widoki, które odnoszą się do modeli dla uczniów i rejestracji, utworzony w poprzedniej sekcji.</span><span class="sxs-lookup"><span data-stu-id="067cd-118">There are many options for the type of scaffolding you can add; in this tutorial, the scaffold will include a controller and views that correspond to the Student and Enrollment models you created in the previous section.</span></span>

<span data-ttu-id="067cd-119">Aby zachować spójność w projekcie, dodasz nowy kontroler do istniejącej **kontrolerów** folderu.</span><span class="sxs-lookup"><span data-stu-id="067cd-119">To maintain consistency in your project, you will add the new controller to the existing **Controllers** folder.</span></span> <span data-ttu-id="067cd-120">Kliknij prawym przyciskiem myszy **kontrolerów** folder, a następnie wybierz **Dodaj** > **nowy element szkieletu**.</span><span class="sxs-lookup"><span data-stu-id="067cd-120">Right-click the **Controllers** folder, and select **Add** > **New Scaffolded Item**.</span></span>

<span data-ttu-id="067cd-121">Wybierz **kontroler MVC 5 z widokami używający narzędzia Entity Framework** opcji.</span><span class="sxs-lookup"><span data-stu-id="067cd-121">Select the **MVC 5 Controller with views, using Entity Framework** option.</span></span> <span data-ttu-id="067cd-122">Ta opcja spowoduje wygenerowanie kontrolera i widoki dla aktualizowanie, usuwanie, tworzenie i wyświetlanie danych w modelu.</span><span class="sxs-lookup"><span data-stu-id="067cd-122">This option will generate the controller and views for updating, deleting, creating and displaying the data in your model.</span></span>

![Dodaj kontroler mvc](generating-views/_static/image2.png)

<span data-ttu-id="067cd-124">Wybierz **uczniów (ContosoSite.Models)** klasy modelu i wybierz **ContosoUniversityDataEntities (ContosoSite.Models)** dla klasy kontekstu.</span><span class="sxs-lookup"><span data-stu-id="067cd-124">Select **Student (ContosoSite.Models)** for the model class and select the **ContosoUniversityDataEntities (ContosoSite.Models)** for the context class.</span></span> <span data-ttu-id="067cd-125">Zachowaj nazwę kontrolera jako **StudentsController**.</span><span class="sxs-lookup"><span data-stu-id="067cd-125">Keep the controller name as **StudentsController**.</span></span>

<span data-ttu-id="067cd-126">Kliknij przycisk **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="067cd-126">Click **Add**.</span></span>

<span data-ttu-id="067cd-127">Jeśli otrzymasz komunikat o błędzie, może to być, ponieważ nie skompilowano projekt w poprzedniej sekcji.</span><span class="sxs-lookup"><span data-stu-id="067cd-127">If you receive an error, it may be because you did not build the project in the previous section.</span></span> <span data-ttu-id="067cd-128">Jeśli tak, spróbuj skompilować projekt, a następnie ponownie Dodaj element szkieletowy.</span><span class="sxs-lookup"><span data-stu-id="067cd-128">If so, try building the project, and then add the scaffolded item again.</span></span>

<span data-ttu-id="067cd-129">Po zakończeniu procedury generowania kodu zobaczysz nowy kontroler i widoków w swoim projekcie **kontrolerów** i **widoków** > **studentów** folderów .</span><span class="sxs-lookup"><span data-stu-id="067cd-129">After the code generation process is complete, you will see a new controller and views in your project's **Controllers** and **Views** > **Students** folders.</span></span>


<span data-ttu-id="067cd-130">Ponownie wykonaj te same czynności, ale Dodaj szkielet dla **rejestracji** klasy.</span><span class="sxs-lookup"><span data-stu-id="067cd-130">Perform the same steps again, but add a scaffold for the **Enrollment** class.</span></span> <span data-ttu-id="067cd-131">Po zakończeniu będziesz mieć **EnrollmentsController.cs** plików i folderów w obszarze **widoków** o nazwie **rejestracje** z widokami Create, Delete, szczegółowe informacje, edycji i indeksu.</span><span class="sxs-lookup"><span data-stu-id="067cd-131">When finished, you have an **EnrollmentsController.cs** file, and a folder under **Views** named **Enrollments** with the Create, Delete, Details, Edit and Index views.</span></span>

## <a name="add-links-to-new-views"></a><span data-ttu-id="067cd-132">Dodawanie łączy do nowych widoków</span><span class="sxs-lookup"><span data-stu-id="067cd-132">Add links to new views</span></span>

<span data-ttu-id="067cd-133">Aby ułatwić przejście do nowych widoków, można dodać kilka hiperłącza do widoków indeksu dla uczniów i rejestracji.</span><span class="sxs-lookup"><span data-stu-id="067cd-133">To make it easier for you to navigate to your new views, you can add a couple of hyperlinks to the Index views for students and enrollments.</span></span> <span data-ttu-id="067cd-134">Otwórz plik w rozmiarze **widoków** > **macierzystego** > *Index.cshtml*, który jest stroną główną witryny.</span><span class="sxs-lookup"><span data-stu-id="067cd-134">Open the file at **Views** > **Home** > *Index.cshtml*, which is the home page for your site.</span></span> <span data-ttu-id="067cd-135">Dodaj następujący kod poniżej jumbotron.</span><span class="sxs-lookup"><span data-stu-id="067cd-135">Add the following code below the jumbotron.</span></span>

[!code-cshtml[Main](generating-views/samples/sample1.cshtml)]

<span data-ttu-id="067cd-136">W przypadku metody ActionLink pierwszy parametr jest tekst do wyświetlenia w linku.</span><span class="sxs-lookup"><span data-stu-id="067cd-136">For the ActionLink method, the first parameter is the text to display in the link.</span></span> <span data-ttu-id="067cd-137">Drugi parametr jest to akcja, a trzeci parametr jest nazwa kontrolera.</span><span class="sxs-lookup"><span data-stu-id="067cd-137">The second parameter is the action and the third parameter is the name of the controller.</span></span> <span data-ttu-id="067cd-138">Na przykład pierwszy link wskazuje akcji indeksu w StudentsController.</span><span class="sxs-lookup"><span data-stu-id="067cd-138">For example, the first link points to the Index action in StudentsController.</span></span> <span data-ttu-id="067cd-139">Rzeczywiste hiperłącze jest zbudowany z tych wartości.</span><span class="sxs-lookup"><span data-stu-id="067cd-139">The actual hyperlink is constructed from these values.</span></span> <span data-ttu-id="067cd-140">Pierwszy link ostatecznie przejście do **Index.cshtml** plików w ramach **widoków/uczniów** folderu.</span><span class="sxs-lookup"><span data-stu-id="067cd-140">The first link ultimately takes users to the **Index.cshtml** file within the **Views/Students** folder.</span></span>

## <a name="display-student-views"></a><span data-ttu-id="067cd-141">Wyświetlanie widoków dla uczniów</span><span class="sxs-lookup"><span data-stu-id="067cd-141">Display student views</span></span>

<span data-ttu-id="067cd-142">Zweryfikuje kod poprawnie dodany do projektu wyświetla listę uczniów i umożliwia użytkownikom edytowanie, tworzenie lub usuwanie rekordów dla uczniów w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="067cd-142">You will verify that the code added to your project correctly displays a list of the students, and enables users to edit, create, or delete the student records in the database.</span></span>

<span data-ttu-id="067cd-143">Kliknij prawym przyciskiem myszy **widoków** > **Home** > *Index.cshtml* pliku, a następnie wybierz **Pokaż w przeglądarce**.</span><span class="sxs-lookup"><span data-stu-id="067cd-143">Right-click the **Views** > **Home** > *Index.cshtml* file, and select **View in Browser**.</span></span> <span data-ttu-id="067cd-144">Na stronie głównej aplikacji wybierz **listę uczniów**.</span><span class="sxs-lookup"><span data-stu-id="067cd-144">On the application home page, select **List of students**.</span></span>

![](generating-views/_static/image6.png)

<span data-ttu-id="067cd-145">Na **indeksu** strony, zwróć uwagę na listę uczniów i łącza, aby zmodyfikować te dane.</span><span class="sxs-lookup"><span data-stu-id="067cd-145">On the **Index** page, notice the list of the students and links to modify this data.</span></span> <span data-ttu-id="067cd-146">Wybierz **Utwórz nowy** łącze i podanie kilku wartości dotyczących nowego studenta.</span><span class="sxs-lookup"><span data-stu-id="067cd-146">Select the **Create New** link and provide some values for a new student.</span></span> <span data-ttu-id="067cd-147">Kliknij przycisk **Utwórz**i zwróć uwagę, dodaniu nowego studenta do listy.</span><span class="sxs-lookup"><span data-stu-id="067cd-147">Click **Create**, and notice the new student is added to your list.</span></span>

<span data-ttu-id="067cd-148">Po powrocie **indeksu** wybierz opcję **Edytuj** połączyć, a następnie zmienić niektóre z wartości dla uczniów lub studentów.</span><span class="sxs-lookup"><span data-stu-id="067cd-148">Back on the **Index** page, select the **Edit** link, and change some of the values for a student.</span></span> <span data-ttu-id="067cd-149">Kliknij przycisk **Zapisz**i zwróć uwagę, rekord dla uczniów został zmieniony.</span><span class="sxs-lookup"><span data-stu-id="067cd-149">Click **Save**, and notice the student record has been changed.</span></span>

<span data-ttu-id="067cd-150">Na koniec wybierz pozycję **Usuń** link i upewnij się, że chcesz usunąć rekord, klikając **Usuń** przycisku.</span><span class="sxs-lookup"><span data-stu-id="067cd-150">Finally, select the **Delete** link and confirm that you want to delete the record by clicking the **Delete** button.</span></span>

<span data-ttu-id="067cd-151">Bez pisania żadnego kodu, można dodać widoków, które wykonują typowe operacje na danych w tabeli dla uczniów.</span><span class="sxs-lookup"><span data-stu-id="067cd-151">Without writing any code, you have added views that perform common operations on the data in the Student table.</span></span>

<span data-ttu-id="067cd-152">Być może zauważono, że tekst etykiety dla pola jest oparty na właściwość bazy danych (takich jak **LastName**) który nie jest zawsze mają być wyświetlane na stronie sieci web.</span><span class="sxs-lookup"><span data-stu-id="067cd-152">You may have noticed that the text label for a field is based on the database property (such as **LastName**) which is not necessarily what you want to display on the web page.</span></span> <span data-ttu-id="067cd-153">Na przykład, lepiej jest etykieta **nazwisko**.</span><span class="sxs-lookup"><span data-stu-id="067cd-153">For example, you may prefer the label to be **Last Name**.</span></span> <span data-ttu-id="067cd-154">Naprawi ten problem wyświetlaną w dalszej części tego samouczka.</span><span class="sxs-lookup"><span data-stu-id="067cd-154">You will fix this display issue later in the tutorial.</span></span>

## <a name="display-enrollment-views"></a><span data-ttu-id="067cd-155">Wyświetlanie widoków rejestracji</span><span class="sxs-lookup"><span data-stu-id="067cd-155">Display enrollment views</span></span>

<span data-ttu-id="067cd-156">Baza danych zawiera relację jeden do wielu między tabelami, studentów i rejestracji i relacji jeden do wielu między tabelami kursu i rejestracji.</span><span class="sxs-lookup"><span data-stu-id="067cd-156">Your database includes a one-to-many relationship between the Student and Enrollment tables, and a one-to-many relationship between the Course and Enrollment tables.</span></span> <span data-ttu-id="067cd-157">Widoki dla rejestracji prawidłowo obsługiwać te relacje.</span><span class="sxs-lookup"><span data-stu-id="067cd-157">The views for Enrollment correctly handle these relationships.</span></span> <span data-ttu-id="067cd-158">Przejdź do strony głównej witryny i wybierz **listę rejestracje** link a następnie **Utwórz nowy** łącza.</span><span class="sxs-lookup"><span data-stu-id="067cd-158">Navigate to the home page for your site and select the **List of enrollments** link and then the **Create New** link.</span></span>

<span data-ttu-id="067cd-159">W widoku wyświetlane formularz służący do tworzenia nowego rekordu rejestracji.</span><span class="sxs-lookup"><span data-stu-id="067cd-159">The view displays a form for creating a new enrollment record.</span></span> <span data-ttu-id="067cd-160">W szczególności zwróć uwagę, że formularz zawiera **CourseID** listy rozwijanej i **StudentID** listy rozwijanej.</span><span class="sxs-lookup"><span data-stu-id="067cd-160">In particular, notice that the form contains a **CourseID** drop-down list and a **StudentID** drop-down list.</span></span> <span data-ttu-id="067cd-161">Oba są wypełnione wartościami z powiązanych tabel.</span><span class="sxs-lookup"><span data-stu-id="067cd-161">Both are populated with values from the related tables.</span></span>

<span data-ttu-id="067cd-162">Ponadto sprawdzanie poprawności podanych wartości jest automatycznie stosowana zależności dla typu danych pola.</span><span class="sxs-lookup"><span data-stu-id="067cd-162">Furthermore, validation of the provided values is automatically applied based on the data type of the field.</span></span> <span data-ttu-id="067cd-163">**Klasa** musi zawierać cyfry, dzięki czemu jest wyświetlany komunikat o błędzie, jeśli zostanie podjęta próba zapewniają niezgodną wartość: *Pola klasy korporacyjnej musi być liczbą.*</span><span class="sxs-lookup"><span data-stu-id="067cd-163">**Grade** requires a number, so an error message is displayed if you try to provide an incompatible value: *The field Grade must be a number.*</span></span>

<span data-ttu-id="067cd-164">Upewnieniu się, że widoki generowane automatycznie umożliwiają użytkownikom pracę z danymi w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="067cd-164">You have verified that the automatically-generated views enable users to work with the data in the database.</span></span> <span data-ttu-id="067cd-165">W następnym samouczku z tej serii możesz zaktualizować bazę danych i wprowadzić odpowiednie zmiany w aplikacji sieci web.</span><span class="sxs-lookup"><span data-stu-id="067cd-165">In the next tutorial in this series, you will update the database and make the corresponding changes in the web application.</span></span>

## <a name="next-steps"></a><span data-ttu-id="067cd-166">Następne kroki</span><span class="sxs-lookup"><span data-stu-id="067cd-166">Next steps</span></span>

<span data-ttu-id="067cd-167">W ramach tego samouczka możesz:</span><span class="sxs-lookup"><span data-stu-id="067cd-167">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="067cd-168">Dodano szkieletu</span><span class="sxs-lookup"><span data-stu-id="067cd-168">Added scaffold</span></span>
> * <span data-ttu-id="067cd-169">Dodano linki do nowych widoków</span><span class="sxs-lookup"><span data-stu-id="067cd-169">Added links to new views</span></span>
> * <span data-ttu-id="067cd-170">Widoki wyświetlanych dla uczniów</span><span class="sxs-lookup"><span data-stu-id="067cd-170">Displayed student views</span></span>
> * <span data-ttu-id="067cd-171">Widoki wyświetlane rejestracji</span><span class="sxs-lookup"><span data-stu-id="067cd-171">Displayed enrollment views</span></span>

<span data-ttu-id="067cd-172">Przejdź do następnego artykułu, aby dowiedzieć się, jak zmienić bazę danych programu.</span><span class="sxs-lookup"><span data-stu-id="067cd-172">Advance to the next article to learn how to change the database.</span></span>
> [!div class="nextstepaction"]
> [<span data-ttu-id="067cd-173">Zmień bazę danych</span><span class="sxs-lookup"><span data-stu-id="067cd-173">Change the database</span></span>](changing-the-database.md)