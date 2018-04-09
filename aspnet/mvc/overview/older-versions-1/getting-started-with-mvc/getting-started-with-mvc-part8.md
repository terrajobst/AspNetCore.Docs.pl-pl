---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part8
title: Dodawanie kolumny do modelu | Dokumentacja firmy Microsoft
author: shanselman
description: Jest to samouczek początkujących przedstawiający podstawowe informacje o platformie ASP.NET MVC. Utwórz prostą aplikację sieci web odczytuje i zapisuje z bazy danych.
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/14/2010
ms.topic: article
ms.assetid: 7ae696b9-348f-4993-8ebb-a838acbe0c28
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part8
msc.type: authoredcontent
ms.openlocfilehash: 7a0b64ce00fc5ee6d49990f1d4d93a154c467bf5
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
---
<a name="adding-a-column-to-the-model"></a><span data-ttu-id="b960a-104">Dodawanie kolumny do modelu</span><span class="sxs-lookup"><span data-stu-id="b960a-104">Adding a Column to the Model</span></span>
====================
<span data-ttu-id="b960a-105">przez [Scott Hanselman](https://github.com/shanselman)</span><span class="sxs-lookup"><span data-stu-id="b960a-105">by [Scott Hanselman](https://github.com/shanselman)</span></span>

> <span data-ttu-id="b960a-106">Jest to samouczek początkujących przedstawiający podstawowe informacje o platformie ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="b960a-106">This is a beginner tutorial that introduces the basics of ASP.NET MVC.</span></span> <span data-ttu-id="b960a-107">Utworzysz prostą aplikację sieci web odczytuje i zapisuje z bazy danych.</span><span class="sxs-lookup"><span data-stu-id="b960a-107">You'll create a simple web application that reads and writes from a database.</span></span> <span data-ttu-id="b960a-108">Odwiedź stronę [Centrum szkoleniowe programu ASP.NET MVC](../../../index.md) można znaleźć inne platformy ASP.NET MVC, samouczki i przykłady.</span><span class="sxs-lookup"><span data-stu-id="b960a-108">Visit the [ASP.NET MVC learning center](../../../index.md) to find other ASP.NET MVC tutorials and samples.</span></span>


<span data-ttu-id="b960a-109">W tej sekcji zamierzamy przeprowadzenie jak możemy wprowadzić zmiany w schemacie naszej bazie danych i obsługi zmian w naszej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="b960a-109">In this section we are going to walk through how we can make changes to the schema of our database, and handle the changes within our application.</span></span>

<span data-ttu-id="b960a-110">Możemy dodać kolumny "Klasyfikacja" do tabeli filmu.</span><span class="sxs-lookup"><span data-stu-id="b960a-110">Let's add a "Rating" Colum to the Movie table.</span></span> <span data-ttu-id="b960a-111">Wróć do środowiska IDE, a następnie kliknij przycisk pomocy Eksploratora bazy danych.</span><span class="sxs-lookup"><span data-stu-id="b960a-111">Go back to the IDE and click the Database Explorer.</span></span> <span data-ttu-id="b960a-112">W tabeli filmu kliknij prawym przyciskiem myszy i wybierz Otwórz definicję tabeli.</span><span class="sxs-lookup"><span data-stu-id="b960a-112">Right click the Movie table and select Open Table Definition.</span></span>

<span data-ttu-id="b960a-113">Dodaj kolumnę "Klasyfikacji", jak pokazano poniżej.</span><span class="sxs-lookup"><span data-stu-id="b960a-113">Add a "Rating" column as seen below.</span></span> <span data-ttu-id="b960a-114">Ponieważ nie mamy teraz wszystkie klasyfikacje, kolumnie mogą dopuszczać wartości null.</span><span class="sxs-lookup"><span data-stu-id="b960a-114">Since we don't have any Ratings now, the column can allow nulls.</span></span> <span data-ttu-id="b960a-115">Kliknij przycisk Zapisz.</span><span class="sxs-lookup"><span data-stu-id="b960a-115">Click Save.</span></span>

<span data-ttu-id="b960a-116">[![Edytowanie tabeli filmy](getting-started-with-mvc-part8/_static/image2.png)](getting-started-with-mvc-part8/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="b960a-116">[![Editing Movies Table](getting-started-with-mvc-part8/_static/image2.png)](getting-started-with-mvc-part8/_static/image1.png)</span></span>

<span data-ttu-id="b960a-117">Następnie wróć do Eksploratora rozwiązań i otwarcie pliku Movies.edmx (który znajduje się w folderze \Models).</span><span class="sxs-lookup"><span data-stu-id="b960a-117">Next, return to the Solution Explorer and open up the Movies.edmx file (which is in the \Models folder).</span></span> <span data-ttu-id="b960a-118">Kliknij prawym przyciskiem myszy powierzchnię projektu (obszar biały) i wybierz Model aktualizacji z bazy danych.</span><span class="sxs-lookup"><span data-stu-id="b960a-118">Right click on the design surface (the white area) and select Update Model from Database.</span></span>

<span data-ttu-id="b960a-119">[![Filmów - Microsoft Visual Web Developer Express 2010 (11)](getting-started-with-mvc-part8/_static/image4.png)](getting-started-with-mvc-part8/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="b960a-119">[![Movies - Microsoft Visual Web Developer 2010 Express (11)](getting-started-with-mvc-part8/_static/image4.png)](getting-started-with-mvc-part8/_static/image3.png)</span></span>

<span data-ttu-id="b960a-120">Spowoduje to uruchomienie "Kreatora aktualizacji".</span><span class="sxs-lookup"><span data-stu-id="b960a-120">This will launch the "Update Wizard".</span></span> <span data-ttu-id="b960a-121">Kliknij kartę odświeżania w nim, a następnie kliknij przycisk Zakończ.</span><span class="sxs-lookup"><span data-stu-id="b960a-121">Click the Refresh tab within it and click Finish.</span></span> <span data-ttu-id="b960a-122">Klasy modelu nasze filmu zostaną zaktualizowane o nową kolumnę.</span><span class="sxs-lookup"><span data-stu-id="b960a-122">Our Movie model class will then be updated with the new column.</span></span>

![Kreator aktualizacji (2)](getting-started-with-mvc-part8/_static/image5.png)

<span data-ttu-id="b960a-124">Po kliknięciu przycisku Zakończ, można zauważyć, że nowa kolumna klasyfikacji został dodany do filmu jednostki w modelu.</span><span class="sxs-lookup"><span data-stu-id="b960a-124">After clicking Finish, you can see the new Rating Column has been added to the Movie Entity in our model.</span></span>

<span data-ttu-id="b960a-125">[![Film jednostki](getting-started-with-mvc-part8/_static/image7.png)](getting-started-with-mvc-part8/_static/image6.png)</span><span class="sxs-lookup"><span data-stu-id="b960a-125">[![Movie Entity](getting-started-with-mvc-part8/_static/image7.png)](getting-started-with-mvc-part8/_static/image6.png)</span></span>

<span data-ttu-id="b960a-126">Dodaliśmy kolumny w modelu bazy danych, ale widoki nie wiadomo o nim.</span><span class="sxs-lookup"><span data-stu-id="b960a-126">We've added a column in the database model, but the Views don't know about it.</span></span>

## <a name="update-views-with-model-changes"></a><span data-ttu-id="b960a-127">Widoki aktualizacji z zmiany modelu</span><span class="sxs-lookup"><span data-stu-id="b960a-127">Update Views with Model Changes</span></span>

<span data-ttu-id="b960a-128">Istnieje kilka sposobów firma Microsoft może zaktualizować naszych szablonów widoku, aby odzwierciedlić nową kolumnę klasyfikacji.</span><span class="sxs-lookup"><span data-stu-id="b960a-128">There are a few ways we could update our view templates to reflect the new Rating column.</span></span> <span data-ttu-id="b960a-129">Ponieważ utworzyliśmy tych widoków generując za pomocą okna dialogowego Dodaj widok, firma Microsoft może je usunąć i ponownie utwórz je ponownie.</span><span class="sxs-lookup"><span data-stu-id="b960a-129">Since we created these Views by generating them via the Add View dialog, we could delete them and recreate them again.</span></span> <span data-ttu-id="b960a-130">Jednak zwykle osób będzie już zostały wprowadzone zmiany, aby ich wyświetlanie szablonów z początkowej generowania szkieletu i będzie można dodać lub usunąć pola ręcznie, tak jak robiliśmy z polem Identyfikator Utwórz.</span><span class="sxs-lookup"><span data-stu-id="b960a-130">However, typically people will have already made modifications to their View templates from the initial scaffolded generation and will want to add or delete fields manually, just as we did with the ID field for Create.</span></span>

<span data-ttu-id="b960a-131">Otwarcie szablonu \Views\Movies\Index.aspx i Dodaj &lt;th&gt;klasyfikacji&lt;/th&gt; nagłówek tabeli filmu.</span><span class="sxs-lookup"><span data-stu-id="b960a-131">Open up the \Views\Movies\Index.aspx template and add a &lt;th&gt;Rating&lt;/th&gt; to the head of the Movie table.</span></span> <span data-ttu-id="b960a-132">Po dodaniu min po rodzaju.</span><span class="sxs-lookup"><span data-stu-id="b960a-132">I added mine after Genre.</span></span> <span data-ttu-id="b960a-133">Następnie w tym samym położeniu kolumny, ale przedstawione, Dodaj linię do wyjściowego naszej nowej klasyfikacji.</span><span class="sxs-lookup"><span data-stu-id="b960a-133">Then, in the same column position but lower down, add a line to output our new Rating.</span></span>

[!code-aspx[Main](getting-started-with-mvc-part8/samples/sample1.aspx)]

<span data-ttu-id="b960a-134">Nasze ostateczny szablon Index.aspx będzie wyglądać następująco:</span><span class="sxs-lookup"><span data-stu-id="b960a-134">Our final Index.aspx template will look like this:</span></span>

[!code-aspx[Main](getting-started-with-mvc-part8/samples/sample2.aspx)]

<span data-ttu-id="b960a-135">Załóżmy następnie otworzyć szablon \Views\Movies\Create.aspx i Dodaj etykiety i pola tekstowego do naszej nowej właściwości klasyfikacji:</span><span class="sxs-lookup"><span data-stu-id="b960a-135">Let's then open up the \Views\Movies\Create.aspx template and add a Label and Textbox for our new Rating property:</span></span>

[!code-aspx[Main](getting-started-with-mvc-part8/samples/sample3.aspx)]

<span data-ttu-id="b960a-136">Nasze ostateczny szablon Create.aspx będzie wyglądać następująco i Zmieńmy naszej przeglądarki tytuł, jak i pomocniczy &lt;h2&gt; tytuł, aby przypominać "Tworzenie filmu" są nam w tym miejscu!</span><span class="sxs-lookup"><span data-stu-id="b960a-136">Our final Create.aspx template will look like this, and let's change our browser's title and secondary &lt;h2&gt; title to something like "Create a Movie" while we're in here!</span></span>

[!code-aspx[Main](getting-started-with-mvc-part8/samples/sample4.aspx)]

<span data-ttu-id="b960a-137">Uruchom aplikację i teraz już wiem nowego pola w bazie danych dodawanej do tworzenia strony.</span><span class="sxs-lookup"><span data-stu-id="b960a-137">Run your app and now you've got a new field in the database that's been added to the Create page.</span></span> <span data-ttu-id="b960a-138">Dodawanie nowych filmu — tym razem z klasyfikacją — i kliknij przycisk Utwórz.</span><span class="sxs-lookup"><span data-stu-id="b960a-138">Add a new Movie - this time with a Rating - and click Create.</span></span>

<span data-ttu-id="b960a-139">[![Tworzenie filmu — Windows Internet Explorer](getting-started-with-mvc-part8/_static/image9.png)](getting-started-with-mvc-part8/_static/image8.png)</span><span class="sxs-lookup"><span data-stu-id="b960a-139">[![Create a Movie - Windows Internet Explorer](getting-started-with-mvc-part8/_static/image9.png)](getting-started-with-mvc-part8/_static/image8.png)</span></span>

<span data-ttu-id="b960a-140">Po kliknięciu przycisku Utwórz są wysyłane do strony indeksu w przypadku, gdy użytkownik nowych filmu znajduje się nowa kolumna klasyfikacji w bazie danych</span><span class="sxs-lookup"><span data-stu-id="b960a-140">After you click Create, you're sent to the Index page where you new Movie is listed with the new Rating Column in the database</span></span>

<span data-ttu-id="b960a-141">[![Listy filmów - Windows Internet Explorer (12)](getting-started-with-mvc-part8/_static/image11.png)](getting-started-with-mvc-part8/_static/image10.png)</span><span class="sxs-lookup"><span data-stu-id="b960a-141">[![Movie List - Windows Internet Explorer (12)](getting-started-with-mvc-part8/_static/image11.png)](getting-started-with-mvc-part8/_static/image10.png)</span></span>

<span data-ttu-id="b960a-142">W tym samouczku podstawowe otrzymano pracę wprowadzania kontrolerów, kojarzenie ich z widokami i przekazywanie wokół ustalony danych.</span><span class="sxs-lookup"><span data-stu-id="b960a-142">This basic tutorial got you started making Controllers, associating them with Views and passing around hard-coded data.</span></span> <span data-ttu-id="b960a-143">Następnie możemy utworzyć i zaprojektowane bazy danych i umieść niektóre dane w.</span><span class="sxs-lookup"><span data-stu-id="b960a-143">Then we created and designed a Database and put some data it in.</span></span> <span data-ttu-id="b960a-144">Możemy pobrać dane z bazy danych i on wyświetlany w tabeli HTML.</span><span class="sxs-lookup"><span data-stu-id="b960a-144">We retrieved the data from the database and displayed it in an HTML table.</span></span> <span data-ttu-id="b960a-145">Następnie dodano Tworzenie formularza, który zezwala użytkownikowi na dodanie danych do bazy danych się z aplikacji sieci Web.</span><span class="sxs-lookup"><span data-stu-id="b960a-145">Then we added a Create form that let the user add data to the database themselves from within the Web Application.</span></span> <span data-ttu-id="b960a-146">Możemy dodać sprawdzania poprawności, a następnie wprowadzone weryfikacji używany język JavaScript po stronie klienta.</span><span class="sxs-lookup"><span data-stu-id="b960a-146">We added validation, then made the validation use JavaScript on the client-side.</span></span> <span data-ttu-id="b960a-147">Na koniec możemy zmienić bazy danych w celu uwzględnienia nowej kolumny danych, a następnie zaktualizować naszych dwie strony, aby utworzyć i wyświetlić te nowe dane.</span><span class="sxs-lookup"><span data-stu-id="b960a-147">Finally, we changed the database to include a new column of data, then updated our two pages to create and display this new data.</span></span>

<span data-ttu-id="b960a-148">Można teraz zachęca do przejść do poziomu pośredniego z naszym samouczkiem "[magazynu utworów muzycznych MVC](../../older-versions/mvc-music-store/mvc-music-store-part-1.md)" oraz wiele plików wideo i zasobów w [ https://asp.net/mvc ](https://asp.net/mvc) nawet więcej informacji o platformie ASP.NET MVC!</span><span class="sxs-lookup"><span data-stu-id="b960a-148">I now encourage you to move on to our intermediate-level tutorial "[MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md)" as well as the many videos and resources at [https://asp.net/mvc](https://asp.net/mvc) to learn even more about ASP.NET MVC!</span></span>

<span data-ttu-id="b960a-149">Owocnej pracy.</span><span class="sxs-lookup"><span data-stu-id="b960a-149">Enjoy!</span></span>

- <span data-ttu-id="b960a-150">Scott Hanselman - [ http://hanselman.com ](http://hanselman.com) i [ @shanselman ](http://twitter.com/shanselman) w serwisie Twitter.</span><span class="sxs-lookup"><span data-stu-id="b960a-150">Scott Hanselman - [http://hanselman.com](http://hanselman.com) and [@shanselman](http://twitter.com/shanselman) on Twitter.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="b960a-151">Poprzednie</span><span class="sxs-lookup"><span data-stu-id="b960a-151">Previous</span></span>](getting-started-with-mvc-part7.md)
