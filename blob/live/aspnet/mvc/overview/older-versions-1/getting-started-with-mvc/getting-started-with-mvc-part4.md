---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part4
title: Tworzenie bazy danych | Dokumentacja firmy Microsoft
author: shanselman
description: "Jest to samouczek początkujących przedstawiający podstawowe informacje o platformie ASP.NET MVC. Utworzysz prostą aplikację sieci web odczytuje i zapisuje z bazy danych."
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/14/2010
ms.topic: article
ms.assetid: 742df67f-484d-4ef3-af6b-8c791e556b43
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part4
msc.type: authoredcontent
ms.openlocfilehash: 6a9617b8056f883d6be2547588b13063a58abd0e
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
<a name="creating-a-database"></a><span data-ttu-id="bbfc4-104">Tworzenie bazy danych</span><span class="sxs-lookup"><span data-stu-id="bbfc4-104">Creating a Database</span></span>
====================
<span data-ttu-id="bbfc4-105">przez [Scott Hanselman](https://github.com/shanselman)</span><span class="sxs-lookup"><span data-stu-id="bbfc4-105">by [Scott Hanselman](https://github.com/shanselman)</span></span>

> <span data-ttu-id="bbfc4-106">Jest to samouczek początkujących przedstawiający podstawowe informacje o platformie ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="bbfc4-106">This is a beginner tutorial that introduces the basics of ASP.NET MVC.</span></span> <span data-ttu-id="bbfc4-107">Utworzysz prostą aplikację sieci web odczytuje i zapisuje z bazy danych.</span><span class="sxs-lookup"><span data-stu-id="bbfc4-107">You'll create a simple web application that reads and writes from a database.</span></span> <span data-ttu-id="bbfc4-108">Odwiedź stronę [Centrum szkoleniowe programu ASP.NET MVC](../../../index.md) można znaleźć inne platformy ASP.NET MVC, samouczki i przykłady.</span><span class="sxs-lookup"><span data-stu-id="bbfc4-108">Visit the [ASP.NET MVC learning center](../../../index.md) to find other ASP.NET MVC tutorials and samples.</span></span>


<span data-ttu-id="bbfc4-109">W tej sekcji zamierzamy utworzyć nowe SQL Express bazy danych, która zostanie użyta do przechowywania i pobierania danych filmu.</span><span class="sxs-lookup"><span data-stu-id="bbfc4-109">In this section we are going to create a new SQL Express database that we'll use to store and retrieve our movie data.</span></span> <span data-ttu-id="bbfc4-110">W ramach programu Visual Web Developer środowiska IDE zaznacz widok | Eksploratora serwera.</span><span class="sxs-lookup"><span data-stu-id="bbfc4-110">From within the Visual Web Developer IDE, select View | Server Explorer.</span></span> <span data-ttu-id="bbfc4-111">Kliknij prawym przyciskiem myszy połączenia danych, a następnie kliknij przycisk Dodaj połączenia...</span><span class="sxs-lookup"><span data-stu-id="bbfc4-111">Right click on Data Connections and click Add Connection...</span></span>

![AddConnection](getting-started-with-mvc-part4/_static/image1.png)

<span data-ttu-id="bbfc4-113">W oknie dialogowym Wybierz źródło danych wybierz pozycję Microsoft SQL Server i wybierz Kontynuuj.</span><span class="sxs-lookup"><span data-stu-id="bbfc4-113">In the Choose Data Source dialog, select Microsoft SQL Server and select Continue.</span></span>

![](getting-started-with-mvc-part4/_static/image2.png)

<span data-ttu-id="bbfc4-114">W oknie dialogowym Dodawanie połączenia, wprowadź ". \SQLEXPRESS" dla nazwy serwera, a następnie wprowadź nazwę nowej bazy danych "Filmy".</span><span class="sxs-lookup"><span data-stu-id="bbfc4-114">In the Add Connection dialog, enter ".\SQLEXPRESS" for your Server Name, and enter "Movies" as the name for your new database.</span></span>

<span data-ttu-id="bbfc4-115">[![Dodaj połączenie, okno dialogowe](getting-started-with-mvc-part4/_static/image4.png)](getting-started-with-mvc-part4/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="bbfc4-115">[![Add Connection dialog](getting-started-with-mvc-part4/_static/image4.png)](getting-started-with-mvc-part4/_static/image3.png)</span></span>

<span data-ttu-id="bbfc4-116">Kliknij przycisk OK, a użytkownik zostanie zapytany, jeśli chcesz utworzyć tę bazę danych.</span><span class="sxs-lookup"><span data-stu-id="bbfc4-116">Click OK and you'll be asked if you want to create that database.</span></span> <span data-ttu-id="bbfc4-117">Kliknij przycisk Tak.</span><span class="sxs-lookup"><span data-stu-id="bbfc4-117">Select yes.</span></span>

<span data-ttu-id="bbfc4-118">[![Tworzenie filmów?](getting-started-with-mvc-part4/_static/image6.png)](getting-started-with-mvc-part4/_static/image5.png)</span><span class="sxs-lookup"><span data-stu-id="bbfc4-118">[![Create Movies?](getting-started-with-mvc-part4/_static/image6.png)](getting-started-with-mvc-part4/_static/image5.png)</span></span>

<span data-ttu-id="bbfc4-119">Teraz otrzymasz pustej bazy danych w Eksploratorze serwera.</span><span class="sxs-lookup"><span data-stu-id="bbfc4-119">Now you've got an empty database in Server Explorer.</span></span>

![Dodaj nową tabelę](getting-started-with-mvc-part4/_static/image7.png)

<span data-ttu-id="bbfc4-121">Kliknij prawym przyciskiem myszy w tabelach, a następnie kliknij przycisk Dodaj tabelę.</span><span class="sxs-lookup"><span data-stu-id="bbfc4-121">Right click on Tables and click Add Table.</span></span> <span data-ttu-id="bbfc4-122">Pojawi się projektanta tabel.</span><span class="sxs-lookup"><span data-stu-id="bbfc4-122">The Table Designer will appear.</span></span> <span data-ttu-id="bbfc4-123">Dodawanie kolumn dla identyfikatora, tytułu, ReleaseDate, Genre i cenę.</span><span class="sxs-lookup"><span data-stu-id="bbfc4-123">Add columns for Id, Title, ReleaseDate, Genre, and Price.</span></span> <span data-ttu-id="bbfc4-124">Kliknij prawym przyciskiem myszy w kolumnie identyfikator i kliknij przycisk Ustaw klucz podstawowy.</span><span class="sxs-lookup"><span data-stu-id="bbfc4-124">Right click on the ID column and click set Primary Key.</span></span> <span data-ttu-id="bbfc4-125">Oto wygląda jak Moje obszary projektu.</span><span class="sxs-lookup"><span data-stu-id="bbfc4-125">Here's what my design areas looks like.</span></span>

<span data-ttu-id="bbfc4-126">[![Edytor tabel bazy danych](getting-started-with-mvc-part4/_static/image9.png)](getting-started-with-mvc-part4/_static/image8.png)</span><span class="sxs-lookup"><span data-stu-id="bbfc4-126">[![Database Table Editor](getting-started-with-mvc-part4/_static/image9.png)](getting-started-with-mvc-part4/_static/image8.png)</span></span>

<span data-ttu-id="bbfc4-127">Ponadto wybierz w kolumnie identyfikator i w obszarze poniżej właściwości kolumny Zmień "Specyfikacji tożsamości" na "Tak".</span><span class="sxs-lookup"><span data-stu-id="bbfc4-127">Also, select the Id column and under Column Properties below change "Identity Specification" to "Yes."</span></span>

<span data-ttu-id="bbfc4-128">[![IsIdentity - właściwości kolumny](getting-started-with-mvc-part4/_static/image11.png)](getting-started-with-mvc-part4/_static/image10.png)</span><span class="sxs-lookup"><span data-stu-id="bbfc4-128">[![IsIdentity - Column Properties](getting-started-with-mvc-part4/_static/image11.png)](getting-started-with-mvc-part4/_static/image10.png)</span></span>

<span data-ttu-id="bbfc4-129">Po skonfigurowaniu pracy, kliknij ikonę Zapisz na pasku narzędzi lub wybierz plik | Z menu i nazwy tabeli "**film**" (w liczbie pojedynczej).</span><span class="sxs-lookup"><span data-stu-id="bbfc4-129">When you've got it done, click the Save icon in the toolbar or select File | Save from the menu, and name your table "**Movie**" (singular).</span></span> <span data-ttu-id="bbfc4-130">Mamy bazę danych i tabeli!</span><span class="sxs-lookup"><span data-stu-id="bbfc4-130">We've got a database and a table!</span></span>

<span data-ttu-id="bbfc4-131">[![Wybierz nazwę](getting-started-with-mvc-part4/_static/image13.png)](getting-started-with-mvc-part4/_static/image12.png)</span><span class="sxs-lookup"><span data-stu-id="bbfc4-131">[![Choose Name](getting-started-with-mvc-part4/_static/image13.png)](getting-started-with-mvc-part4/_static/image12.png)</span></span>

<span data-ttu-id="bbfc4-132">Wróć do Eksploratora serwera i tabeli filmu kliknij prawym przyciskiem myszy, a następnie wybierz opcję "Pokaż dane tabeli".</span><span class="sxs-lookup"><span data-stu-id="bbfc4-132">Go back to Server Explorer and right click the Movie table, then select "Show Table Data."</span></span> <span data-ttu-id="bbfc4-133">Wprowadź kilka filmów, więc niektóre dane nie naszej bazie danych.</span><span class="sxs-lookup"><span data-stu-id="bbfc4-133">Enter a few movies so our database has some data.</span></span>

<span data-ttu-id="bbfc4-134">[![Edytowanie tabeli bazy danych](getting-started-with-mvc-part4/_static/image15.png)](getting-started-with-mvc-part4/_static/image14.png)</span><span class="sxs-lookup"><span data-stu-id="bbfc4-134">[![Database Table Editing](getting-started-with-mvc-part4/_static/image15.png)](getting-started-with-mvc-part4/_static/image14.png)</span></span>

## <a name="creating-a-model"></a><span data-ttu-id="bbfc4-135">Tworzenie modelu</span><span class="sxs-lookup"><span data-stu-id="bbfc4-135">Creating a Model</span></span>

<span data-ttu-id="bbfc4-136">Teraz, wrócić do Eksploratora rozwiązań po prawej stronie IDE i kliknij prawym przyciskiem folder modeli i wybierz opcję Dodaj | Nowy element.</span><span class="sxs-lookup"><span data-stu-id="bbfc4-136">Now, switch back to the Solution Explorer on the right hand side of the IDE and right-click on the Models folder and select Add | New Item.</span></span>

<span data-ttu-id="bbfc4-137">[![addnewmodelitem](getting-started-with-mvc-part4/_static/image17.png)](getting-started-with-mvc-part4/_static/image16.png)</span><span class="sxs-lookup"><span data-stu-id="bbfc4-137">[![addnewmodelitem](getting-started-with-mvc-part4/_static/image17.png)](getting-started-with-mvc-part4/_static/image16.png)</span></span>

<span data-ttu-id="bbfc4-138">Zamierzamy utworzyć modelu jednostki z naszej nowej bazy danych.</span><span class="sxs-lookup"><span data-stu-id="bbfc4-138">We're going to create an Entity Model from our new database.</span></span> <span data-ttu-id="bbfc4-139">Spowoduje to dodanie zestaw klas naszych projekt, który ułatwia firmie Microsoft w celu wykonywania zapytań i manipulowanie danymi w naszej bazie danych.</span><span class="sxs-lookup"><span data-stu-id="bbfc4-139">This will add a set of classes to our project that makes it easy for us to query and manipulate the data within our database.</span></span> <span data-ttu-id="bbfc4-140">Wybierz węzeł danych po lewej stronie okna dialogowego, a następnie wybierz szablon elementu modelu danych jednostki ADO.NET.</span><span class="sxs-lookup"><span data-stu-id="bbfc4-140">Select the Data node on the left-hand side of the dialog, and then select the ADO.NET Entity Data Model item template.</span></span> <span data-ttu-id="bbfc4-141">Nadaj mu nazwę Movies.edmx.</span><span class="sxs-lookup"><span data-stu-id="bbfc4-141">Name it Movies.edmx.</span></span>

<span data-ttu-id="bbfc4-142">[![AddNewDataModel](getting-started-with-mvc-part4/_static/image19.png)](getting-started-with-mvc-part4/_static/image18.png)</span><span class="sxs-lookup"><span data-stu-id="bbfc4-142">[![AddNewDataModel](getting-started-with-mvc-part4/_static/image19.png)](getting-started-with-mvc-part4/_static/image18.png)</span></span>

<span data-ttu-id="bbfc4-143">Kliknij przycisk "Dodaj".</span><span class="sxs-lookup"><span data-stu-id="bbfc4-143">Click the "Add" button.</span></span> <span data-ttu-id="bbfc4-144">Następnie uruchamiają "kreatora modelu danych jednostki".</span><span class="sxs-lookup"><span data-stu-id="bbfc4-144">This will then launch the "Entity Data Model Wizard".</span></span>

<span data-ttu-id="bbfc4-145">W oknie dialogowym Nowy, które pojawia się wybierz opcję generowania z bazy danych.</span><span class="sxs-lookup"><span data-stu-id="bbfc4-145">In the new dialog that pops up, select Generate from Database.</span></span> <span data-ttu-id="bbfc4-146">Ponieważ właśnie wprowadziliśmy bazy danych, potrzebujemy tylko Opisz programu Entity Framework w naszej nowej bazy danych i tabeli.</span><span class="sxs-lookup"><span data-stu-id="bbfc4-146">Since we've just made a database, we'll only need to tell the Entity Framework about our new database and its table.</span></span> <span data-ttu-id="bbfc4-147">Kliknij przycisk Dalej, aby zapisać naszych połączenia z bazą danych w konfiguracji naszej aplikacji sieci web.</span><span class="sxs-lookup"><span data-stu-id="bbfc4-147">Click Next to save our database connection in our web application's configuration.</span></span> <span data-ttu-id="bbfc4-148">Sprawdź teraz, tabele i filmu wyboru i kliknij przycisk Zakończ.</span><span class="sxs-lookup"><span data-stu-id="bbfc4-148">Now, check the Tables and Movie checkbox and click Finish.</span></span>

<span data-ttu-id="bbfc4-149">[![Kreator modelu danych jednostki](getting-started-with-mvc-part4/_static/image21.png)](getting-started-with-mvc-part4/_static/image20.png)</span><span class="sxs-lookup"><span data-stu-id="bbfc4-149">[![Entity Data Model Wizard](getting-started-with-mvc-part4/_static/image21.png)](getting-started-with-mvc-part4/_static/image20.png)</span></span>

<span data-ttu-id="bbfc4-150">Po wprowadzeniu Zobacz nasze nową tabelę filmu Entity Framework Designer i uzyskać do niego dostęp z kodu.</span><span class="sxs-lookup"><span data-stu-id="bbfc4-150">Now we can see our new Movie table in the Entity Framework Designer and access it from code.</span></span>

<span data-ttu-id="bbfc4-151">[![Filmów - Microsoft Visual Web Developer 2010 Express.](getting-started-with-mvc-part4/_static/image23.png)](getting-started-with-mvc-part4/_static/image22.png)</span><span class="sxs-lookup"><span data-stu-id="bbfc4-151">[![Movies - Microsoft Visual Web Developer 2010 Express](getting-started-with-mvc-part4/_static/image23.png)](getting-started-with-mvc-part4/_static/image22.png)</span></span>

<span data-ttu-id="bbfc4-152">Na powierzchni projektowej widać klasy "Filmu".</span><span class="sxs-lookup"><span data-stu-id="bbfc4-152">On the design surface you can see a "Movie" class.</span></span> <span data-ttu-id="bbfc4-153">Ta klasa mapowany na tabelę "Filmu" w naszej bazie danych i każdej właściwości w niej mapowanym na kolumnę z tabeli.</span><span class="sxs-lookup"><span data-stu-id="bbfc4-153">This class maps to the "Movie" table in our database, and each property within it maps to a column with the table.</span></span> <span data-ttu-id="bbfc4-154">Każde wystąpienie klasy "Filmu" odpowiada wiersza w tabeli "Filmu".</span><span class="sxs-lookup"><span data-stu-id="bbfc4-154">Each instance of a "Movie" class will correspond to a row within the "Movie" table.</span></span>

<span data-ttu-id="bbfc4-155">Jeśli nie lubisz domyślnie i mapowanie konwencje używane przez program Entity Framework, można użyć programu Entity Framework designer można zmienić lub dostosować je.</span><span class="sxs-lookup"><span data-stu-id="bbfc4-155">If you don't like the default naming and mapping conventions used by the Entity Framework, you can use the Entity Framework designer to change or customize them.</span></span> <span data-ttu-id="bbfc4-156">Dla tej aplikacji będzie możemy użyć wartości domyślnych i zapisać go jako — jest.</span><span class="sxs-lookup"><span data-stu-id="bbfc4-156">For this application we'll use the defaults and just save the file as-is.</span></span>

<span data-ttu-id="bbfc4-157">Teraz załóżmy działają niektóre dane!</span><span class="sxs-lookup"><span data-stu-id="bbfc4-157">Now, let's work with some real data!</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="bbfc4-158">[Poprzednie](getting-started-with-mvc-part3.md)
[dalej](getting-started-with-mvc-part5.md)</span><span class="sxs-lookup"><span data-stu-id="bbfc4-158">[Previous](getting-started-with-mvc-part3.md)
[Next](getting-started-with-mvc-part5.md)</span></span>
