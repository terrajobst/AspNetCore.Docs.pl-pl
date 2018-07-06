---
uid: mvc/overview/getting-started/database-first-development/generating-views
title: 'EF bazy danych, najpierw z platformą ASP.NET MVC: generowanie widoków | Dokumentacja firmy Microsoft'
author: tfitzmac
description: Za pomocą MVC, platformy Entity Framework i funkcja tworzenia szkieletu ASP.NET, można utworzyć aplikację internetową, która zapewnia interfejs do istniejącej bazy danych. Ten samouczek seri...
ms.author: aspnetcontent
ms.date: 12/29/2014
ms.assetid: 669367cf-8e30-4eb6-821d-10a7d9bb906c
msc.legacyurl: /mvc/overview/getting-started/database-first-development/generating-views
msc.type: authoredcontent
ms.openlocfilehash: 9c85b330ac415d8c73d58b31a5108197b754669f
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/05/2018
ms.locfileid: "37835331"
---
<a name="ef-database-first-with-aspnet-mvc-generating-views"></a><span data-ttu-id="0cbc1-104">EF bazy danych, najpierw z platformą ASP.NET MVC: generowanie widoków</span><span class="sxs-lookup"><span data-stu-id="0cbc1-104">EF Database First with ASP.NET MVC: Generating Views</span></span>
====================
<span data-ttu-id="0cbc1-105">przez [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="0cbc1-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="0cbc1-106">Za pomocą MVC, platformy Entity Framework i funkcja tworzenia szkieletu ASP.NET, można utworzyć aplikację internetową, która zapewnia interfejs do istniejącej bazy danych.</span><span class="sxs-lookup"><span data-stu-id="0cbc1-106">Using MVC, Entity Framework, and ASP.NET Scaffolding, you can create a web application that provides an interface to an existing database.</span></span> <span data-ttu-id="0cbc1-107">W tej serii samouczków pokazano, jak automatycznie wygenerować kod, który pozwala użytkownikom na wyświetlanie, edytowanie, tworzenie i usuwanie danych, która znajduje się w tabeli bazy danych.</span><span class="sxs-lookup"><span data-stu-id="0cbc1-107">This tutorial series shows you how to automatically generate code that enables users to display, edit, create, and delete data that resides in a database table.</span></span> <span data-ttu-id="0cbc1-108">Wygenerowany kod odnosi się do kolumn w tabeli bazy danych.</span><span class="sxs-lookup"><span data-stu-id="0cbc1-108">The generated code corresponds to the columns in the database table.</span></span>
> 
> <span data-ttu-id="0cbc1-109">Ta część serii koncentruje się na przy użyciu platformy ASP.NET tworzenie szkieletów widoków i kontrolerów.</span><span class="sxs-lookup"><span data-stu-id="0cbc1-109">This part of the series focuses on using ASP.NET Scaffolding to generate the controllers and views.</span></span>


## <a name="add-scaffold"></a><span data-ttu-id="0cbc1-110">Dodaj szkielet</span><span class="sxs-lookup"><span data-stu-id="0cbc1-110">Add scaffold</span></span>

<span data-ttu-id="0cbc1-111">Jesteś gotowy do generowania kodu, który zapewni operacji danych w warstwie standardowa dla klasy modelu.</span><span class="sxs-lookup"><span data-stu-id="0cbc1-111">You are ready to generate code that will provide standard data operations for the model classes.</span></span> <span data-ttu-id="0cbc1-112">Możesz dodać kod przez dodawanie elementu szkieletu.</span><span class="sxs-lookup"><span data-stu-id="0cbc1-112">You add the code by adding a scaffold item.</span></span> <span data-ttu-id="0cbc1-113">Istnieje wiele opcji dla typu tworzenia szkieletów, które można dodać; w tym samouczku szkieletu obejmuje kontrolera i widoki, które odnoszą się do modeli dla uczniów i rejestracji, utworzony w poprzedniej sekcji.</span><span class="sxs-lookup"><span data-stu-id="0cbc1-113">There are many options for the type of scaffolding you can add; in this tutorial, the scaffold will include a controller and views that correspond to the Student and Enrollment models you created in the previous section.</span></span>

<span data-ttu-id="0cbc1-114">Aby zachować spójność w projekcie, dodasz nowy kontroler do istniejącej **kontrolerów** folderu.</span><span class="sxs-lookup"><span data-stu-id="0cbc1-114">To maintain consistency in your project, you will add the new controller to the existing **Controllers** folder.</span></span> <span data-ttu-id="0cbc1-115">Kliknij prawym przyciskiem myszy **kontrolerów** folder, a następnie wybierz **Dodaj** — **nowy element szkieletu**.</span><span class="sxs-lookup"><span data-stu-id="0cbc1-115">Right-click the **Controllers** folder, and select **Add** – **New Scaffolded Item**.</span></span>

![Dodaj szkielet](generating-views/_static/image1.png)

<span data-ttu-id="0cbc1-117">Wybierz **kontroler MVC 5 z widokami używający narzędzia Entity Framework** opcji.</span><span class="sxs-lookup"><span data-stu-id="0cbc1-117">Select the **MVC 5 Controller with views, using Entity Framework** option.</span></span> <span data-ttu-id="0cbc1-118">Ta opcja spowoduje wygenerowanie kontrolera i widoki dla aktualizowanie, usuwanie, tworzenie i wyświetlanie danych w modelu.</span><span class="sxs-lookup"><span data-stu-id="0cbc1-118">This option will generate the controller and views for updating, deleting, creating and displaying the data in your model.</span></span>

![Dodaj kontroler mvc](generating-views/_static/image2.png)

<span data-ttu-id="0cbc1-120">Wybierz **uczniów** dla klasy modelu, a następnie wybierz **ContosoUniversityEntities** dla klasy kontekstu.</span><span class="sxs-lookup"><span data-stu-id="0cbc1-120">Select **Student** for the model class, and select the **ContosoUniversityEntities** for the context class.</span></span> <span data-ttu-id="0cbc1-121">Zachowaj nazwę kontrolera jako **StudentsController**,</span><span class="sxs-lookup"><span data-stu-id="0cbc1-121">Keep the controller name as **StudentsController**,</span></span>

![Określ kontroler](generating-views/_static/image3.png)

<span data-ttu-id="0cbc1-123">Kliknij przycisk **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="0cbc1-123">Click **Add**.</span></span>

<span data-ttu-id="0cbc1-124">Jeśli otrzymasz komunikat o błędzie, może to być, ponieważ nie skompilowano projekt w poprzedniej sekcji.</span><span class="sxs-lookup"><span data-stu-id="0cbc1-124">If you receive an error, it may be because you did not build the project in the previous section.</span></span> <span data-ttu-id="0cbc1-125">Jeśli tak, spróbuj skompilować projekt, a następnie ponownie Dodaj element szkieletowy.</span><span class="sxs-lookup"><span data-stu-id="0cbc1-125">If so, try building the project, and then add the scaffolded item again.</span></span>

<span data-ttu-id="0cbc1-126">Po zakończeniu procedury generowania kodu zobaczysz nowy kontroler i widoków w projekcie.</span><span class="sxs-lookup"><span data-stu-id="0cbc1-126">After the code generation process is complete, you will see a new controller and views in your project.</span></span>

![Pokaż widoki](generating-views/_static/image4.png)

<span data-ttu-id="0cbc1-128">Ponownie wykonaj te same czynności, ale Dodaj szkielet dla klasy rejestracji.</span><span class="sxs-lookup"><span data-stu-id="0cbc1-128">Perform the same steps again, but add a scaffold for the Enrollment class.</span></span> <span data-ttu-id="0cbc1-129">Po zakończeniu powinien mieć **EnrollmentsController.cs** plików i folderów w obszarze **widoków** o nazwie **rejestracje** o tworzenie, usuwanie, szczegółowe informacje, edycji i indeksu Widoki.</span><span class="sxs-lookup"><span data-stu-id="0cbc1-129">When finished, you should have an **EnrollmentsController.cs** file, and a folder under **Views** named **Enrollments** with the Create, Delete, Details, Edit and Index views.</span></span>

![Pokaż widoki](generating-views/_static/image5.png)

## <a name="add-links-to-new-views"></a><span data-ttu-id="0cbc1-131">Dodawanie łączy do nowych widoków</span><span class="sxs-lookup"><span data-stu-id="0cbc1-131">Add links to new views</span></span>

<span data-ttu-id="0cbc1-132">Aby ułatwić przejście do nowych widoków, można dodać kilka hiperłącza do widoków indeksu dla uczniów i rejestracji.</span><span class="sxs-lookup"><span data-stu-id="0cbc1-132">To make it easier for you to navigate to your new views, you can add a couple of hyperlinks to the Index views for students and enrollments.</span></span> <span data-ttu-id="0cbc1-133">Otwórz plik w rozmiarze **Views/Home/Index.cshtml**, który jest stroną główną witryny.</span><span class="sxs-lookup"><span data-stu-id="0cbc1-133">Open the file at **Views/Home/Index.cshtml**, which is the home page for your site.</span></span> <span data-ttu-id="0cbc1-134">Dodaj następujący kod poniżej jumbotron.</span><span class="sxs-lookup"><span data-stu-id="0cbc1-134">Add the following code below the jumbotron.</span></span>

[!code-cshtml[Main](generating-views/samples/sample1.cshtml)]

<span data-ttu-id="0cbc1-135">W przypadku metody ActionLink pierwszy parametr jest tekst do wyświetlenia w linku.</span><span class="sxs-lookup"><span data-stu-id="0cbc1-135">For the ActionLink method, the first parameter is the text to display in the link.</span></span> <span data-ttu-id="0cbc1-136">Drugi parametr jest to akcja, a trzeci parametr jest nazwa kontrolera.</span><span class="sxs-lookup"><span data-stu-id="0cbc1-136">The second parameter is the action and the third parameter is the name of the controller.</span></span> <span data-ttu-id="0cbc1-137">Na przykład pierwszy link wskazuje akcji indeksu w StudentsController.</span><span class="sxs-lookup"><span data-stu-id="0cbc1-137">For example, the first link points to the Index action in StudentsController.</span></span> <span data-ttu-id="0cbc1-138">Rzeczywiste hiperłącze jest zbudowany z tych wartości.</span><span class="sxs-lookup"><span data-stu-id="0cbc1-138">The actual hyperlink is constructed from these values.</span></span> <span data-ttu-id="0cbc1-139">Pierwszy link ostatecznie przejście do **Index.cshtml** plików w ramach **widoków/uczniów** folderu.</span><span class="sxs-lookup"><span data-stu-id="0cbc1-139">The first link ultimately takes users to the **Index.cshtml** file within the **Views/Students** folder.</span></span>

## <a name="display-student-views"></a><span data-ttu-id="0cbc1-140">Wyświetlanie widoków dla uczniów</span><span class="sxs-lookup"><span data-stu-id="0cbc1-140">Display student views</span></span>

<span data-ttu-id="0cbc1-141">Zweryfikuje kod poprawnie dodany do projektu wyświetla listę uczniów i umożliwia użytkownikom edytowanie, tworzenie lub usuwanie rekordów dla uczniów w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="0cbc1-141">You will verify that the code added to your project correctly displays a list of the students, and enables users to edit, create, or delete the student records in the database.</span></span>

<span data-ttu-id="0cbc1-142">Kliknij prawym przyciskiem myszy **Views/Home/Index.cshtml** pliku, a następnie wybierz **Pokaż w przeglądarce**.</span><span class="sxs-lookup"><span data-stu-id="0cbc1-142">Right-click the **Views/Home/Index.cshtml** file, and select **View in Browser**.</span></span> <span data-ttu-id="0cbc1-143">Na tej stronie kliknij łącze do listy studentów.</span><span class="sxs-lookup"><span data-stu-id="0cbc1-143">On this page, click the link for the list of students.</span></span>

![](generating-views/_static/image6.png)

<span data-ttu-id="0cbc1-144">Na tej stronie zostanie wyświetlona lista studentów i łącza, aby zmodyfikować te dane.</span><span class="sxs-lookup"><span data-stu-id="0cbc1-144">On this page, notice the list of the students and links to modify this data.</span></span>

![listy studentów](generating-views/_static/image7.png)

<span data-ttu-id="0cbc1-146">Kliknij przycisk **Utwórz nowy** łącze i podanie kilku wartości dotyczących nowego studenta.</span><span class="sxs-lookup"><span data-stu-id="0cbc1-146">Click the **Create New** link and provide some values for a new student.</span></span>

![Tworzenie nowego studenta](generating-views/_static/image8.png)

<span data-ttu-id="0cbc1-148">Kliknij przycisk **Utwórz**i zwróć uwagę, dodaniu nowego studenta do listy.</span><span class="sxs-lookup"><span data-stu-id="0cbc1-148">Click **Create**, and notice the new student is added to your list.</span></span>

![Lista z nowego studenta](generating-views/_static/image9.png)

<span data-ttu-id="0cbc1-150">Wybierz **Edytuj** połączyć, a następnie zmienić niektóre z wartości dla uczniów lub studentów.</span><span class="sxs-lookup"><span data-stu-id="0cbc1-150">Select the **Edit** link, and change some of the values for a student.</span></span>

![Edytowanie ucznia](generating-views/_static/image10.png)

<span data-ttu-id="0cbc1-152">Kliknij przycisk **Zapisz**i zwróć uwagę, rekord dla uczniów został zmieniony.</span><span class="sxs-lookup"><span data-stu-id="0cbc1-152">Click **Save**, and notice the student record has been changed.</span></span>

<span data-ttu-id="0cbc1-153">Na koniec wybierz pozycję **Usuń** link i upewnij się, że chcesz usunąć rekord, klikając **Usuń** przycisku.</span><span class="sxs-lookup"><span data-stu-id="0cbc1-153">Finally, select the **Delete** link and confirm that you want to delete the record by clicking the **Delete** button.</span></span>

![Usuń ucznia](generating-views/_static/image11.png)

<span data-ttu-id="0cbc1-155">Bez pisania żadnego kodu, można dodać widoków, które wykonują typowe operacje na danych w tabeli dla uczniów.</span><span class="sxs-lookup"><span data-stu-id="0cbc1-155">Without writing any code, you have added views that perform common operations on the data in the Student table.</span></span>

<span data-ttu-id="0cbc1-156">Być może zauważono, że tekst etykiety dla pola jest oparty na właściwość bazy danych (takich jak **LastName**) który nie jest zawsze mają być wyświetlane na stronie sieci web.</span><span class="sxs-lookup"><span data-stu-id="0cbc1-156">You may have noticed that the text label for a field is based on the database property (such as **LastName**) which is not necessarily what you want to display on the web page.</span></span> <span data-ttu-id="0cbc1-157">Na przykład, lepiej jest etykieta **nazwisko**.</span><span class="sxs-lookup"><span data-stu-id="0cbc1-157">For example, you may prefer the label to be **Last Name**.</span></span> <span data-ttu-id="0cbc1-158">Naprawi ten problem wyświetlaną w dalszej części tego samouczka.</span><span class="sxs-lookup"><span data-stu-id="0cbc1-158">You will fix this display issue later in the tutorial.</span></span>

## <a name="display-enrollment-views"></a><span data-ttu-id="0cbc1-159">Wyświetlanie widoków rejestracji</span><span class="sxs-lookup"><span data-stu-id="0cbc1-159">Display enrollment views</span></span>

<span data-ttu-id="0cbc1-160">Baza danych zawiera relację jeden do wielu między tabelami, studentów i rejestracji i relacji jeden do wielu między tabelami kursu i rejestracji.</span><span class="sxs-lookup"><span data-stu-id="0cbc1-160">Your database includes a one-to-many relationship between the Student and Enrollment tables, and a one-to-many relationship between the Course and Enrollment tables.</span></span> <span data-ttu-id="0cbc1-161">Widoki dla rejestracji prawidłowo obsługiwać te relacje.</span><span class="sxs-lookup"><span data-stu-id="0cbc1-161">The views for Enrollment correctly handle these relationships.</span></span> <span data-ttu-id="0cbc1-162">Przejdź do strony głównej witryny i wybierz **listę rejestracje** link a następnie **Utwórz nowy** łącza.</span><span class="sxs-lookup"><span data-stu-id="0cbc1-162">Navigate to the home page for your site and select the **List of enrollments** link and then the **Create New** link.</span></span> <span data-ttu-id="0cbc1-163">W widoku wyświetlane formularz służący do tworzenia nowego rekordu rejestracji.</span><span class="sxs-lookup"><span data-stu-id="0cbc1-163">The view displays a form for creating a new enrollment record.</span></span> <span data-ttu-id="0cbc1-164">W szczególności zwróć uwagę, że formularz zawiera dwie listy rozwijane, które są wypełnione wartościami z powiązanych tabel.</span><span class="sxs-lookup"><span data-stu-id="0cbc1-164">In particular, notice that the form contains two drop-down lists that are populated with values from the related tables.</span></span>

![Tworzenie rejestracji](generating-views/_static/image12.png)

<span data-ttu-id="0cbc1-166">Ponadto sprawdzanie poprawności podanych wartości jest automatycznie stosowana zależności dla typu danych pola.</span><span class="sxs-lookup"><span data-stu-id="0cbc1-166">Furthermore, validation of the provided values is automatically applied based on the data type of the field.</span></span> <span data-ttu-id="0cbc1-167">Klasy korporacyjnej musi zawierać cyfry, dzięki czemu jest wyświetlany komunikat o błędzie, jeśli zostanie podjęta próba zapewniają niezgodną wartość.</span><span class="sxs-lookup"><span data-stu-id="0cbc1-167">Grade requires a number, so an error message is displayed if you try to provide an incompatible value.</span></span>

![komunikat sprawdzania poprawności](generating-views/_static/image13.png)

<span data-ttu-id="0cbc1-169">Upewnieniu się, że widoki generowane automatycznie umożliwiają użytkownikom pracę z danymi w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="0cbc1-169">You have verified that the automatically-generated views enable users to work with the data in the database.</span></span> <span data-ttu-id="0cbc1-170">W następnym samouczku z tej serii możesz zaktualizować bazę danych i wprowadzić odpowiednie zmiany w aplikacji sieci web.</span><span class="sxs-lookup"><span data-stu-id="0cbc1-170">In the next tutorial in this series, you will update the database and make the corresponding changes in the web application.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="0cbc1-171">[Poprzednie](creating-the-web-application.md)
> [dalej](changing-the-database.md)</span><span class="sxs-lookup"><span data-stu-id="0cbc1-171">[Previous](creating-the-web-application.md)
[Next](changing-the-database.md)</span></span>
