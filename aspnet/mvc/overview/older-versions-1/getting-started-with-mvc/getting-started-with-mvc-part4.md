---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part4
title: Tworzenie bazy danych | Dokumentacja firmy Microsoft
author: shanselman
description: Jest to samouczek dla początkujących, która przedstawia podstawy platformy ASP.NET MVC. Utwórz prostą aplikację sieci web wykonującej Odczyt i zapis z bazy danych.
ms.author: aspnetcontent
ms.date: 08/14/2010
ms.assetid: 742df67f-484d-4ef3-af6b-8c791e556b43
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part4
msc.type: authoredcontent
ms.openlocfilehash: fff974edfaffeb164879c10b8e70bd3482c47554
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/05/2018
ms.locfileid: "37813763"
---
<a name="creating-a-database"></a><span data-ttu-id="b4abe-104">Tworzenie bazy danych</span><span class="sxs-lookup"><span data-stu-id="b4abe-104">Creating a Database</span></span>
====================
<span data-ttu-id="b4abe-105">przez [Scotta Hanselmana](https://github.com/shanselman)</span><span class="sxs-lookup"><span data-stu-id="b4abe-105">by [Scott Hanselman](https://github.com/shanselman)</span></span>

> <span data-ttu-id="b4abe-106">Jest to samouczek dla początkujących, która przedstawia podstawy platformy ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="b4abe-106">This is a beginner tutorial that introduces the basics of ASP.NET MVC.</span></span> <span data-ttu-id="b4abe-107">Utworzysz prostą aplikację sieci web wykonującej Odczyt i zapis z bazy danych.</span><span class="sxs-lookup"><span data-stu-id="b4abe-107">You'll create a simple web application that reads and writes from a database.</span></span> <span data-ttu-id="b4abe-108">Odwiedź stronę [Centrum szkoleniowe programu ASP.NET MVC](../../../index.md) można znaleźć inne platformy ASP.NET MVC, samouczków i przykładów.</span><span class="sxs-lookup"><span data-stu-id="b4abe-108">Visit the [ASP.NET MVC learning center](../../../index.md) to find other ASP.NET MVC tutorials and samples.</span></span>


<span data-ttu-id="b4abe-109">W tej sekcji użyjemy do utworzenia nowego programu SQL Express bazy danych, która użyjemy do przechowywania i pobierania danych filmu.</span><span class="sxs-lookup"><span data-stu-id="b4abe-109">In this section we are going to create a new SQL Express database that we'll use to store and retrieve our movie data.</span></span> <span data-ttu-id="b4abe-110">Z poziomu programu Visual Web Developer środowiska IDE, wybierz widok | Eksploratora serwera.</span><span class="sxs-lookup"><span data-stu-id="b4abe-110">From within the Visual Web Developer IDE, select View | Server Explorer.</span></span> <span data-ttu-id="b4abe-111">Kliknij prawym przyciskiem myszy połączeń danych, a następnie kliknij przycisk Dodaj połączenie...</span><span class="sxs-lookup"><span data-stu-id="b4abe-111">Right click on Data Connections and click Add Connection...</span></span>

![AddConnection](getting-started-with-mvc-part4/_static/image1.png)

<span data-ttu-id="b4abe-113">W oknie dialogowym Wybierz źródło danych wybierz program Microsoft SQL Server, a następnie wybierz pozycję Kontynuuj.</span><span class="sxs-lookup"><span data-stu-id="b4abe-113">In the Choose Data Source dialog, select Microsoft SQL Server and select Continue.</span></span>

![](getting-started-with-mvc-part4/_static/image2.png)

<span data-ttu-id="b4abe-114">W oknie dialogowym Dodaj połączenie, wprowadź ". \SQLEXPRESS" dla nazwy serwera, a następnie wprowadź "Filmy" jako nazwę nowej bazy danych.</span><span class="sxs-lookup"><span data-stu-id="b4abe-114">In the Add Connection dialog, enter ".\SQLEXPRESS" for your Server Name, and enter "Movies" as the name for your new database.</span></span>

<span data-ttu-id="b4abe-115">[![Dodaj okno dialogowe połączenia](getting-started-with-mvc-part4/_static/image4.png)](getting-started-with-mvc-part4/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="b4abe-115">[![Add Connection dialog](getting-started-with-mvc-part4/_static/image4.png)](getting-started-with-mvc-part4/_static/image3.png)</span></span>

<span data-ttu-id="b4abe-116">Kliknij przycisk OK, a użytkownik zostanie zapytany, jeśli chcesz utworzyć tę bazę danych.</span><span class="sxs-lookup"><span data-stu-id="b4abe-116">Click OK and you'll be asked if you want to create that database.</span></span> <span data-ttu-id="b4abe-117">Kliknij przycisk Tak.</span><span class="sxs-lookup"><span data-stu-id="b4abe-117">Select yes.</span></span>

<span data-ttu-id="b4abe-118">[![Utwórz filmy?](getting-started-with-mvc-part4/_static/image6.png)](getting-started-with-mvc-part4/_static/image5.png)</span><span class="sxs-lookup"><span data-stu-id="b4abe-118">[![Create Movies?](getting-started-with-mvc-part4/_static/image6.png)](getting-started-with-mvc-part4/_static/image5.png)</span></span>

<span data-ttu-id="b4abe-119">Teraz masz pustej bazy danych w Eksploratorze serwera.</span><span class="sxs-lookup"><span data-stu-id="b4abe-119">Now you've got an empty database in Server Explorer.</span></span>

![Dodaj nową tabelę](getting-started-with-mvc-part4/_static/image7.png)

<span data-ttu-id="b4abe-121">Kliknij prawym przyciskiem myszy w tabelach, a następnie kliknij przycisk Dodaj tabelę.</span><span class="sxs-lookup"><span data-stu-id="b4abe-121">Right click on Tables and click Add Table.</span></span> <span data-ttu-id="b4abe-122">Pojawi się okno projektanta tabel.</span><span class="sxs-lookup"><span data-stu-id="b4abe-122">The Table Designer will appear.</span></span> <span data-ttu-id="b4abe-123">Dodaj kolumny Identyfikator, tytuł, ReleaseDate, gatunku i ceny.</span><span class="sxs-lookup"><span data-stu-id="b4abe-123">Add columns for Id, Title, ReleaseDate, Genre, and Price.</span></span> <span data-ttu-id="b4abe-124">Kliknij prawym przyciskiem myszy kolumny Identyfikatora, a następnie kliknij polecenie Ustaw klucz podstawowy.</span><span class="sxs-lookup"><span data-stu-id="b4abe-124">Right click on the ID column and click set Primary Key.</span></span> <span data-ttu-id="b4abe-125">Oto mój projekt obszary wygląda podobnie.</span><span class="sxs-lookup"><span data-stu-id="b4abe-125">Here's what my design areas looks like.</span></span>

<span data-ttu-id="b4abe-126">[![Edytor tabel bazy danych](getting-started-with-mvc-part4/_static/image9.png)](getting-started-with-mvc-part4/_static/image8.png)</span><span class="sxs-lookup"><span data-stu-id="b4abe-126">[![Database Table Editor](getting-started-with-mvc-part4/_static/image9.png)](getting-started-with-mvc-part4/_static/image8.png)</span></span>

<span data-ttu-id="b4abe-127">Ponadto wybierz kolumny identyfikatora, a w obszarze poniżej właściwości kolumny zmiany "Specyfikacja tożsamości", "Yes".</span><span class="sxs-lookup"><span data-stu-id="b4abe-127">Also, select the Id column and under Column Properties below change "Identity Specification" to "Yes."</span></span>

<span data-ttu-id="b4abe-128">[![IsIdentity — właściwości kolumny](getting-started-with-mvc-part4/_static/image11.png)](getting-started-with-mvc-part4/_static/image10.png)</span><span class="sxs-lookup"><span data-stu-id="b4abe-128">[![IsIdentity - Column Properties](getting-started-with-mvc-part4/_static/image11.png)](getting-started-with-mvc-part4/_static/image10.png)</span></span>

<span data-ttu-id="b4abe-129">Jeśli właśnie tak zrobić, kliknij ikonę Zapisz na pasku narzędzi lub wybierz plik | Zapisz z menu, a nazwa tabeli "**filmu**" (pojedynczą).</span><span class="sxs-lookup"><span data-stu-id="b4abe-129">When you've got it done, click the Save icon in the toolbar or select File | Save from the menu, and name your table "**Movie**" (singular).</span></span> <span data-ttu-id="b4abe-130">Gdy mamy już bazę danych i tabelę!</span><span class="sxs-lookup"><span data-stu-id="b4abe-130">We've got a database and a table!</span></span>

<span data-ttu-id="b4abe-131">[![Wybierz nazwę](getting-started-with-mvc-part4/_static/image13.png)](getting-started-with-mvc-part4/_static/image12.png)</span><span class="sxs-lookup"><span data-stu-id="b4abe-131">[![Choose Name](getting-started-with-mvc-part4/_static/image13.png)](getting-started-with-mvc-part4/_static/image12.png)</span></span>

<span data-ttu-id="b4abe-132">Wróć do Eksploratora serwera i kliknij prawym przyciskiem myszy tabelę filmu, a następnie wybierz opcję "Pokaż dane tabeli".</span><span class="sxs-lookup"><span data-stu-id="b4abe-132">Go back to Server Explorer and right click the Movie table, then select "Show Table Data."</span></span> <span data-ttu-id="b4abe-133">Wprowadź kilka filmów, więc niektóre dane nie naszej bazie danych.</span><span class="sxs-lookup"><span data-stu-id="b4abe-133">Enter a few movies so our database has some data.</span></span>

<span data-ttu-id="b4abe-134">[![Edytowanie tabel bazy danych](getting-started-with-mvc-part4/_static/image15.png)](getting-started-with-mvc-part4/_static/image14.png)</span><span class="sxs-lookup"><span data-stu-id="b4abe-134">[![Database Table Editing](getting-started-with-mvc-part4/_static/image15.png)](getting-started-with-mvc-part4/_static/image14.png)</span></span>

## <a name="creating-a-model"></a><span data-ttu-id="b4abe-135">Tworzenie modelu</span><span class="sxs-lookup"><span data-stu-id="b4abe-135">Creating a Model</span></span>

<span data-ttu-id="b4abe-136">Teraz przejdź z powrotem do Eksploratora rozwiązań na po prawej stronie okna środowiska IDE programu i kliknij prawym przyciskiem myszy w folderze modeli i wybierz opcję Dodaj | Nowy element.</span><span class="sxs-lookup"><span data-stu-id="b4abe-136">Now, switch back to the Solution Explorer on the right hand side of the IDE and right-click on the Models folder and select Add | New Item.</span></span>

<span data-ttu-id="b4abe-137">[![addnewmodelitem](getting-started-with-mvc-part4/_static/image17.png)](getting-started-with-mvc-part4/_static/image16.png)</span><span class="sxs-lookup"><span data-stu-id="b4abe-137">[![addnewmodelitem](getting-started-with-mvc-part4/_static/image17.png)](getting-started-with-mvc-part4/_static/image16.png)</span></span>

<span data-ttu-id="b4abe-138">Zamierzamy utworzyć Model jednostki z naszej nowej bazy danych.</span><span class="sxs-lookup"><span data-stu-id="b4abe-138">We're going to create an Entity Model from our new database.</span></span> <span data-ttu-id="b4abe-139">Zestaw klas, spowoduje to dodanie do naszego projektu, który ułatwia to firmie Microsoft w celu wykonywania zapytań i manipulowania danymi w naszej bazie danych.</span><span class="sxs-lookup"><span data-stu-id="b4abe-139">This will add a set of classes to our project that makes it easy for us to query and manipulate the data within our database.</span></span> <span data-ttu-id="b4abe-140">Wybierz węzeł danych po lewej stronie okna dialogowego, a następnie wybierz szablon elementu ADO.NET Entity Data Model.</span><span class="sxs-lookup"><span data-stu-id="b4abe-140">Select the Data node on the left-hand side of the dialog, and then select the ADO.NET Entity Data Model item template.</span></span> <span data-ttu-id="b4abe-141">Nadaj mu nazwę Movies.edmx.</span><span class="sxs-lookup"><span data-stu-id="b4abe-141">Name it Movies.edmx.</span></span>

<span data-ttu-id="b4abe-142">[![AddNewDataModel](getting-started-with-mvc-part4/_static/image19.png)](getting-started-with-mvc-part4/_static/image18.png)</span><span class="sxs-lookup"><span data-stu-id="b4abe-142">[![AddNewDataModel](getting-started-with-mvc-part4/_static/image19.png)](getting-started-with-mvc-part4/_static/image18.png)</span></span>

<span data-ttu-id="b4abe-143">Kliknij przycisk "Dodaj".</span><span class="sxs-lookup"><span data-stu-id="b4abe-143">Click the "Add" button.</span></span> <span data-ttu-id="b4abe-144">Spowoduje to uruchomienie "Kreator modelu danych jednostki".</span><span class="sxs-lookup"><span data-stu-id="b4abe-144">This will then launch the "Entity Data Model Wizard".</span></span>

<span data-ttu-id="b4abe-145">W oknie dialogowym Nowy, które się pojawi wybierz pozycję Generuj z bazy danych.</span><span class="sxs-lookup"><span data-stu-id="b4abe-145">In the new dialog that pops up, select Generate from Database.</span></span> <span data-ttu-id="b4abe-146">Ponieważ właśnie wprowadziliśmy bazę danych, będziemy potrzebować tylko mówić Entity Framework na temat naszej nowej bazy danych i jego tabeli.</span><span class="sxs-lookup"><span data-stu-id="b4abe-146">Since we've just made a database, we'll only need to tell the Entity Framework about our new database and its table.</span></span> <span data-ttu-id="b4abe-147">Kliknij obok pozycji Zapisz naszych połączenia z bazą danych w konfiguracji naszej aplikacji sieci web.</span><span class="sxs-lookup"><span data-stu-id="b4abe-147">Click Next to save our database connection in our web application's configuration.</span></span> <span data-ttu-id="b4abe-148">Sprawdź tabele i film pole wyboru, a następnie kliknij przycisk Zakończ.</span><span class="sxs-lookup"><span data-stu-id="b4abe-148">Now, check the Tables and Movie checkbox and click Finish.</span></span>

<span data-ttu-id="b4abe-149">[![Kreator modelu Entity Data Model](getting-started-with-mvc-part4/_static/image21.png)](getting-started-with-mvc-part4/_static/image20.png)</span><span class="sxs-lookup"><span data-stu-id="b4abe-149">[![Entity Data Model Wizard](getting-started-with-mvc-part4/_static/image21.png)](getting-started-with-mvc-part4/_static/image20.png)</span></span>

<span data-ttu-id="b4abe-150">Teraz możemy Zobacz naszej nowej tabeli filmu w Entity Framework Designer i uzyskać do niego dostęp z poziomu kodu.</span><span class="sxs-lookup"><span data-stu-id="b4abe-150">Now we can see our new Movie table in the Entity Framework Designer and access it from code.</span></span>

<span data-ttu-id="b4abe-151">[![Filmy — Microsoft Visual Web Developer 2010 Express](getting-started-with-mvc-part4/_static/image23.png)](getting-started-with-mvc-part4/_static/image22.png)</span><span class="sxs-lookup"><span data-stu-id="b4abe-151">[![Movies - Microsoft Visual Web Developer 2010 Express](getting-started-with-mvc-part4/_static/image23.png)](getting-started-with-mvc-part4/_static/image22.png)</span></span>

<span data-ttu-id="b4abe-152">Na powierzchni projektowej widać klasy "Filmu".</span><span class="sxs-lookup"><span data-stu-id="b4abe-152">On the design surface you can see a "Movie" class.</span></span> <span data-ttu-id="b4abe-153">Ta klasa jest mapowany do tabeli "Filmu" w naszej bazie danych i każdej właściwości w nim mapuje kolumną z tabeli.</span><span class="sxs-lookup"><span data-stu-id="b4abe-153">This class maps to the "Movie" table in our database, and each property within it maps to a column with the table.</span></span> <span data-ttu-id="b4abe-154">Każde wystąpienie klasy "Filmu" odpowiada wiersz w tabeli "Filmu".</span><span class="sxs-lookup"><span data-stu-id="b4abe-154">Each instance of a "Movie" class will correspond to a row within the "Movie" table.</span></span>

<span data-ttu-id="b4abe-155">Jeśli nie potrzebujesz domyślne nazewnictwa i Konwencji używanych przez program Entity Framework mapowania, może używać Projektanta Entity Framework, zmienić lub dostosować je.</span><span class="sxs-lookup"><span data-stu-id="b4abe-155">If you don't like the default naming and mapping conventions used by the Entity Framework, you can use the Entity Framework designer to change or customize them.</span></span> <span data-ttu-id="b4abe-156">Dla tej aplikacji będzie używane wartości domyślne i mogą po prostu zapisz plik jako-to.</span><span class="sxs-lookup"><span data-stu-id="b4abe-156">For this application we'll use the defaults and just save the file as-is.</span></span>

<span data-ttu-id="b4abe-157">Teraz Użyjmy niektórych rzeczywistych danych!</span><span class="sxs-lookup"><span data-stu-id="b4abe-157">Now, let's work with some real data!</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="b4abe-158">[Poprzednie](getting-started-with-mvc-part3.md)
> [dalej](getting-started-with-mvc-part5.md)</span><span class="sxs-lookup"><span data-stu-id="b4abe-158">[Previous](getting-started-with-mvc-part3.md)
[Next](getting-started-with-mvc-part5.md)</span></span>
