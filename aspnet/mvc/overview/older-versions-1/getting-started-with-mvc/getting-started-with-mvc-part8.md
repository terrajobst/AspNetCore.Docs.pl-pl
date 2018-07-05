---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part8
title: Dodawanie kolumny do modelu | Dokumentacja firmy Microsoft
author: shanselman
description: Jest to samouczek dla początkujących, która przedstawia podstawy platformy ASP.NET MVC. Utwórz prostą aplikację sieci web wykonującej Odczyt i zapis z bazy danych.
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/14/2010
ms.topic: article
ms.assetid: 7ae696b9-348f-4993-8ebb-a838acbe0c28
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part8
msc.type: authoredcontent
ms.openlocfilehash: 241f9108394561fecb15db5b6efa2661fdb128d6
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37361763"
---
<a name="adding-a-column-to-the-model"></a><span data-ttu-id="c9481-104">Dodawanie kolumny do modelu</span><span class="sxs-lookup"><span data-stu-id="c9481-104">Adding a Column to the Model</span></span>
====================
<span data-ttu-id="c9481-105">przez [Scotta Hanselmana](https://github.com/shanselman)</span><span class="sxs-lookup"><span data-stu-id="c9481-105">by [Scott Hanselman](https://github.com/shanselman)</span></span>

> <span data-ttu-id="c9481-106">Jest to samouczek dla początkujących, która przedstawia podstawy platformy ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="c9481-106">This is a beginner tutorial that introduces the basics of ASP.NET MVC.</span></span> <span data-ttu-id="c9481-107">Utworzysz prostą aplikację sieci web wykonującej Odczyt i zapis z bazy danych.</span><span class="sxs-lookup"><span data-stu-id="c9481-107">You'll create a simple web application that reads and writes from a database.</span></span> <span data-ttu-id="c9481-108">Odwiedź stronę [Centrum szkoleniowe programu ASP.NET MVC](../../../index.md) można znaleźć inne platformy ASP.NET MVC, samouczków i przykładów.</span><span class="sxs-lookup"><span data-stu-id="c9481-108">Visit the [ASP.NET MVC learning center](../../../index.md) to find other ASP.NET MVC tutorials and samples.</span></span>


<span data-ttu-id="c9481-109">W tej sekcji użyjemy opisano jak firma Microsoft może dokonać zmian schematu w naszej bazie danych i obsługi zmian w naszej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="c9481-109">In this section we are going to walk through how we can make changes to the schema of our database, and handle the changes within our application.</span></span>

<span data-ttu-id="c9481-110">Dodajmy "Ocena" kolumny do tabeli filmu.</span><span class="sxs-lookup"><span data-stu-id="c9481-110">Let's add a "Rating" Colum to the Movie table.</span></span> <span data-ttu-id="c9481-111">Wróć do IDE, a następnie kliknij pozycję Eksplorator bazy danych.</span><span class="sxs-lookup"><span data-stu-id="c9481-111">Go back to the IDE and click the Database Explorer.</span></span> <span data-ttu-id="c9481-112">Kliknij prawym przyciskiem myszy tabelę filmu, a następnie wybierz Otwórz definicję tabeli.</span><span class="sxs-lookup"><span data-stu-id="c9481-112">Right click the Movie table and select Open Table Definition.</span></span>

<span data-ttu-id="c9481-113">Dodaj kolumnę "Ocena", jak pokazano poniżej.</span><span class="sxs-lookup"><span data-stu-id="c9481-113">Add a "Rating" column as seen below.</span></span> <span data-ttu-id="c9481-114">Ponieważ firma Microsoft nie mają teraz wszystkie klasyfikacje kolumny można zezwolić na wartości null.</span><span class="sxs-lookup"><span data-stu-id="c9481-114">Since we don't have any Ratings now, the column can allow nulls.</span></span> <span data-ttu-id="c9481-115">Kliknij przycisk Zapisz.</span><span class="sxs-lookup"><span data-stu-id="c9481-115">Click Save.</span></span>

<span data-ttu-id="c9481-116">[![Edytowanie tabel filmy](getting-started-with-mvc-part8/_static/image2.png)](getting-started-with-mvc-part8/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="c9481-116">[![Editing Movies Table](getting-started-with-mvc-part8/_static/image2.png)](getting-started-with-mvc-part8/_static/image1.png)</span></span>

<span data-ttu-id="c9481-117">Następnie wróć do Eksploratora rozwiązań i Otwórz plik Movies.edmx (która znajduje się w folderze \Models).</span><span class="sxs-lookup"><span data-stu-id="c9481-117">Next, return to the Solution Explorer and open up the Movies.edmx file (which is in the \Models folder).</span></span> <span data-ttu-id="c9481-118">Kliknij prawym przyciskiem myszy na powierzchni projektowej (białego obszaru) i wybierz Model aktualizacji z bazy danych.</span><span class="sxs-lookup"><span data-stu-id="c9481-118">Right click on the design surface (the white area) and select Update Model from Database.</span></span>

<span data-ttu-id="c9481-119">[![Filmy — Microsoft Visual Web Developer 2010 Express (11)](getting-started-with-mvc-part8/_static/image4.png)](getting-started-with-mvc-part8/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="c9481-119">[![Movies - Microsoft Visual Web Developer 2010 Express (11)](getting-started-with-mvc-part8/_static/image4.png)](getting-started-with-mvc-part8/_static/image3.png)</span></span>

<span data-ttu-id="c9481-120">Spowoduje to uruchomienie kreatora"aktualizacji".</span><span class="sxs-lookup"><span data-stu-id="c9481-120">This will launch the "Update Wizard".</span></span> <span data-ttu-id="c9481-121">Kliknij kartę odświeżania w nim, a następnie kliknij przycisk Zakończ.</span><span class="sxs-lookup"><span data-stu-id="c9481-121">Click the Refresh tab within it and click Finish.</span></span> <span data-ttu-id="c9481-122">Klasa modelu nasz film zostaną zaktualizowane przy użyciu nowej kolumny.</span><span class="sxs-lookup"><span data-stu-id="c9481-122">Our Movie model class will then be updated with the new column.</span></span>

![Kreator aktualizacji (2)](getting-started-with-mvc-part8/_static/image5.png)

<span data-ttu-id="c9481-124">Po kliknięciu przycisku Zakończ, zobaczysz, że nowa kolumna Ocena został dodany do jednostki filmu w naszym modelu.</span><span class="sxs-lookup"><span data-stu-id="c9481-124">After clicking Finish, you can see the new Rating Column has been added to the Movie Entity in our model.</span></span>

<span data-ttu-id="c9481-125">[![Jednostki filmu](getting-started-with-mvc-part8/_static/image7.png)](getting-started-with-mvc-part8/_static/image6.png)</span><span class="sxs-lookup"><span data-stu-id="c9481-125">[![Movie Entity](getting-started-with-mvc-part8/_static/image7.png)](getting-started-with-mvc-part8/_static/image6.png)</span></span>

<span data-ttu-id="c9481-126">Dodaliśmy kolumny w modelu bazy danych, ale widoków nie wiadomo o nim.</span><span class="sxs-lookup"><span data-stu-id="c9481-126">We've added a column in the database model, but the Views don't know about it.</span></span>

## <a name="update-views-with-model-changes"></a><span data-ttu-id="c9481-127">Zaktualizuj widoków, wprowadzając zmiany w modelu</span><span class="sxs-lookup"><span data-stu-id="c9481-127">Update Views with Model Changes</span></span>

<span data-ttu-id="c9481-128">Istnieje kilka sposobów aktualizujemy nasze szablony widoku, aby odzwierciedlić nową kolumnę klasyfikacji.</span><span class="sxs-lookup"><span data-stu-id="c9481-128">There are a few ways we could update our view templates to reflect the new Rating column.</span></span> <span data-ttu-id="c9481-129">Ponieważ utworzyliśmy te widoki, generując je za pomocą okna dialogowego Dodaj widok, można je usunąć i ponownie utwórz je ponownie.</span><span class="sxs-lookup"><span data-stu-id="c9481-129">Since we created these Views by generating them via the Add View dialog, we could delete them and recreate them again.</span></span> <span data-ttu-id="c9481-130">Jednak zazwyczaj osób będzie zostały już wykonane modyfikacje ich Przeglądanie szablonów początkowej generowania szkieletu i trzeba dodać lub usunąć pola, ręcznie, tak jak robiliśmy z polem Identyfikator tworzenia.</span><span class="sxs-lookup"><span data-stu-id="c9481-130">However, typically people will have already made modifications to their View templates from the initial scaffolded generation and will want to add or delete fields manually, just as we did with the ID field for Create.</span></span>

<span data-ttu-id="c9481-131">Otwórz szablon \Views\Movies\Index.aspx i Dodaj &lt;th&gt;ocena&lt;/th&gt; porównanie tabeli filmu.</span><span class="sxs-lookup"><span data-stu-id="c9481-131">Open up the \Views\Movies\Index.aspx template and add a &lt;th&gt;Rating&lt;/th&gt; to the head of the Movie table.</span></span> <span data-ttu-id="c9481-132">Po dodaniu wydobywanie po gatunku.</span><span class="sxs-lookup"><span data-stu-id="c9481-132">I added mine after Genre.</span></span> <span data-ttu-id="c9481-133">Następnie w tym samym położeniu kolumny, ale pod nią, Dodaj wiersz w danych wyjściowych naszych Nowa klasyfikacja.</span><span class="sxs-lookup"><span data-stu-id="c9481-133">Then, in the same column position but lower down, add a line to output our new Rating.</span></span>

[!code-aspx[Main](getting-started-with-mvc-part8/samples/sample1.aspx)]

<span data-ttu-id="c9481-134">Ostateczny szablon Index.aspx będzie wyglądać następująco:</span><span class="sxs-lookup"><span data-stu-id="c9481-134">Our final Index.aspx template will look like this:</span></span>

[!code-aspx[Main](getting-started-with-mvc-part8/samples/sample2.aspx)]

<span data-ttu-id="c9481-135">Możemy następnie otworzyć szablon \Views\Movies\Create.aspx i dodawanie etykiety i pole tekstowe dla naszej nowej właściwości klasyfikacji:</span><span class="sxs-lookup"><span data-stu-id="c9481-135">Let's then open up the \Views\Movies\Create.aspx template and add a Label and Textbox for our new Rating property:</span></span>

[!code-aspx[Main](getting-started-with-mvc-part8/samples/sample3.aspx)]

<span data-ttu-id="c9481-136">Nasze ostateczny szablon Create.aspx będzie wyglądać następująco, a Zmieńmy tytuł i pomocniczy naszej przeglądarki &lt;h2&gt; tytułu na wartość podobną "Tworzenie filmu" gdy mamy tutaj!</span><span class="sxs-lookup"><span data-stu-id="c9481-136">Our final Create.aspx template will look like this, and let's change our browser's title and secondary &lt;h2&gt; title to something like "Create a Movie" while we're in here!</span></span>

[!code-aspx[Main](getting-started-with-mvc-part8/samples/sample4.aspx)]

<span data-ttu-id="c9481-137">Uruchom aplikację, a teraz masz nowe pole w bazie danych zostały dodane do tworzenia strony.</span><span class="sxs-lookup"><span data-stu-id="c9481-137">Run your app and now you've got a new field in the database that's been added to the Create page.</span></span> <span data-ttu-id="c9481-138">Dodaj nowy film — tym razem z oceną — i kliknij przycisk Utwórz.</span><span class="sxs-lookup"><span data-stu-id="c9481-138">Add a new Movie - this time with a Rating - and click Create.</span></span>

<span data-ttu-id="c9481-139">[![Utwórz film — Windows Internet Explorer](getting-started-with-mvc-part8/_static/image9.png)](getting-started-with-mvc-part8/_static/image8.png)</span><span class="sxs-lookup"><span data-stu-id="c9481-139">[![Create a Movie - Windows Internet Explorer](getting-started-with-mvc-part8/_static/image9.png)](getting-started-with-mvc-part8/_static/image8.png)</span></span>

<span data-ttu-id="c9481-140">Po kliknięciu pozycji Utwórz nastąpiło do strony indeksu w przypadku, gdy użytkownik nowego filmu znajduje się nowa kolumna oceny w bazie danych</span><span class="sxs-lookup"><span data-stu-id="c9481-140">After you click Create, you're sent to the Index page where you new Movie is listed with the new Rating Column in the database</span></span>

<span data-ttu-id="c9481-141">[![Lista filmu — Windows Internet Explorer (12)](getting-started-with-mvc-part8/_static/image11.png)](getting-started-with-mvc-part8/_static/image10.png)</span><span class="sxs-lookup"><span data-stu-id="c9481-141">[![Movie List - Windows Internet Explorer (12)](getting-started-with-mvc-part8/_static/image11.png)](getting-started-with-mvc-part8/_static/image10.png)</span></span>

<span data-ttu-id="c9481-142">W tym samouczku podstawowe stało się ułatwiające rozpoczęcie pracy, dzięki czemu kontrolerów, skojarzenie ich z widoków i przekazanie wokół zakodowanych danych.</span><span class="sxs-lookup"><span data-stu-id="c9481-142">This basic tutorial got you started making Controllers, associating them with Views and passing around hard-coded data.</span></span> <span data-ttu-id="c9481-143">Następnie możemy utworzone i przeznaczone do bazy danych niektóre dane umieszczane w.</span><span class="sxs-lookup"><span data-stu-id="c9481-143">Then we created and designed a Database and put some data it in.</span></span> <span data-ttu-id="c9481-144">Firma Microsoft może pobrać dane z bazy danych i on wyświetlany w tabeli HTML.</span><span class="sxs-lookup"><span data-stu-id="c9481-144">We retrieved the data from the database and displayed it in an HTML table.</span></span> <span data-ttu-id="c9481-145">Następnie dodaliśmy Utwórz formularz, który umożliwia użytkownikowi dodawanie danych do bazy danych samodzielnie z poziomu aplikacji sieci Web.</span><span class="sxs-lookup"><span data-stu-id="c9481-145">Then we added a Create form that let the user add data to the database themselves from within the Web Application.</span></span> <span data-ttu-id="c9481-146">Możemy dodać sprawdzanie poprawności, a następnie wykonywane sprawdzanie poprawności używany język JavaScript po stronie klienta.</span><span class="sxs-lookup"><span data-stu-id="c9481-146">We added validation, then made the validation use JavaScript on the client-side.</span></span> <span data-ttu-id="c9481-147">Na koniec możemy zmienić bazę danych, aby uwzględnić nową kolumnę danych, a następnie zaktualizowane nasze dwie strony, aby utworzyć i wyświetlić te nowe dane.</span><span class="sxs-lookup"><span data-stu-id="c9481-147">Finally, we changed the database to include a new column of data, then updated our two pages to create and display this new data.</span></span>

<span data-ttu-id="c9481-148">Teraz zachęcam Cię do przejdziemy do Nasz samouczek dotyczący poziomu pośredniego "[MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md)", a także wiele filmów wideo i zasobów w [ https://asp.net/mvc ](https://asp.net/mvc) się jeszcze bardziej poświęcone platformie ASP.NET MVC!</span><span class="sxs-lookup"><span data-stu-id="c9481-148">I now encourage you to move on to our intermediate-level tutorial "[MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md)" as well as the many videos and resources at [https://asp.net/mvc](https://asp.net/mvc) to learn even more about ASP.NET MVC!</span></span>

<span data-ttu-id="c9481-149">Ciesz się!</span><span class="sxs-lookup"><span data-stu-id="c9481-149">Enjoy!</span></span>

- <span data-ttu-id="c9481-150">Scott Hanselman — [ http://hanselman.com ](http://hanselman.com) i [ @shanselman ](http://twitter.com/shanselman) w serwisie Twitter.</span><span class="sxs-lookup"><span data-stu-id="c9481-150">Scott Hanselman - [http://hanselman.com](http://hanselman.com) and [@shanselman](http://twitter.com/shanselman) on Twitter.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="c9481-151">Poprzednie</span><span class="sxs-lookup"><span data-stu-id="c9481-151">Previous</span></span>](getting-started-with-mvc-part7.md)
