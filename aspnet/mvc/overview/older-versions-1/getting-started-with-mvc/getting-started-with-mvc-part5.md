---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part5
title: "Uzyskiwanie dostępu do modelu danych z kontrolera | Dokumentacja firmy Microsoft"
author: shanselman
description: "Jest to samouczek początkujących przedstawiający podstawowe informacje o platformie ASP.NET MVC. Utwórz prostą aplikację sieci web odczytuje i zapisuje z bazy danych."
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/14/2010
ms.topic: article
ms.assetid: 004703cd-e0e9-4ba7-9974-1b0475c71222
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part5
msc.type: authoredcontent
ms.openlocfilehash: cf81d351aef45af3640f5d113eb3619911e03606
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/30/2018
---
<a name="accessing-your-models-data-from-a-controller"></a><span data-ttu-id="72375-104">Uzyskiwanie dostępu do modelu danych z kontrolera</span><span class="sxs-lookup"><span data-stu-id="72375-104">Accessing your Model's Data from a Controller</span></span>
====================
<span data-ttu-id="72375-105">przez [Scott Hanselman](https://github.com/shanselman)</span><span class="sxs-lookup"><span data-stu-id="72375-105">by [Scott Hanselman](https://github.com/shanselman)</span></span>

> <span data-ttu-id="72375-106">Jest to samouczek początkujących przedstawiający podstawowe informacje o platformie ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="72375-106">This is a beginner tutorial that introduces the basics of ASP.NET MVC.</span></span> <span data-ttu-id="72375-107">Utworzysz prostą aplikację sieci web odczytuje i zapisuje z bazy danych.</span><span class="sxs-lookup"><span data-stu-id="72375-107">You'll create a simple web application that reads and writes from a database.</span></span> <span data-ttu-id="72375-108">Odwiedź stronę [Centrum szkoleniowe programu ASP.NET MVC](../../../index.md) można znaleźć inne platformy ASP.NET MVC, samouczki i przykłady.</span><span class="sxs-lookup"><span data-stu-id="72375-108">Visit the [ASP.NET MVC learning center](../../../index.md) to find other ASP.NET MVC tutorials and samples.</span></span>


<span data-ttu-id="72375-109">W tej sekcji zamierzamy Utwórz nową klasę MoviesController i napisanie kodu, pobiera naszych danych filmu i wyświetla je do przeglądarki przy użyciu szablonu widoku.</span><span class="sxs-lookup"><span data-stu-id="72375-109">In this section we are going to create a new MoviesController class, and write some code that retrieves our Movie data and displays it back to the browser using a View template.</span></span>

<span data-ttu-id="72375-110">Kliknij prawym przyciskiem myszy folder kontrolery i utworzyć nowy MoviesController.</span><span class="sxs-lookup"><span data-stu-id="72375-110">Right click on the Controllers folder and make a new MoviesController.</span></span>

<span data-ttu-id="72375-111">[![Dodawanie kontrolera](getting-started-with-mvc-part5/_static/image2.png)](getting-started-with-mvc-part5/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="72375-111">[![Add Controller](getting-started-with-mvc-part5/_static/image2.png)](getting-started-with-mvc-part5/_static/image1.png)</span></span>

<span data-ttu-id="72375-112">Spowoduje to utworzenie nowego pliku "MoviesController.cs" poniżej naszych folderu \Controllers w ramach naszych projektu.</span><span class="sxs-lookup"><span data-stu-id="72375-112">This will create a new "MoviesController.cs" file underneath our \Controllers folder within our project.</span></span> <span data-ttu-id="72375-113">Teraz zaktualizuj MovieController można pobrać listy filmów z naszych nowo wypełnione bazy danych.</span><span class="sxs-lookup"><span data-stu-id="72375-113">Let's update the MovieController to retrieve the list of movies from our newly populated database.</span></span>

[!code-csharp[Main](getting-started-with-mvc-part5/samples/sample1.cs)]

<span data-ttu-id="72375-114">Firma Microsoft wykonywania zapytań LINQ, aby tylko pobieranie filmów wydaną po lata 1984.</span><span class="sxs-lookup"><span data-stu-id="72375-114">We are performing a LINQ query so that we only retrieve movies released after the summer of 1984.</span></span> <span data-ttu-id="72375-115">Potrzebujemy szablonu widoku do renderowania tej listy filmów z powrotem, więc kliknij prawym przyciskiem myszy w metodzie i wybierz opcję Dodaj widok, aby go utworzyć.</span><span class="sxs-lookup"><span data-stu-id="72375-115">We'll need a View template to render this list of movies back, so right-click in the method and select Add View to create it.</span></span>

<span data-ttu-id="72375-116">W oknie dialogowym Dodawanie widoku firma Microsoft będzie wskazywać, przekazywane listy&lt;Movies.Models.Movie&gt; do naszej szablonu widoku.</span><span class="sxs-lookup"><span data-stu-id="72375-116">Within the Add View dialog we'll indicate that we are passing a List&lt;Movies.Models.Movie&gt; to our View template.</span></span> <span data-ttu-id="72375-117">W przeciwieństwie do poprzednich razy możemy użyć okna dialogowego Dodawanie widoku i wybrał opcję utworzenia szablonu "Pusty" Firma Microsoft będzie wskazują, że teraz chcemy, aby Visual Studio, aby automatycznie "utworzyć szkielet" Wyświetl szablon dla nas z zawartością niektóre domyślne.</span><span class="sxs-lookup"><span data-stu-id="72375-117">Unlike the previous times we used the Add View dialog and chose to create an "Empty" template, this time we'll indicate that we want Visual Studio to automatically "scaffold" a view template for us with some default content.</span></span> <span data-ttu-id="72375-118">Firma Microsoft będzie to zrobić, wybierając element "List" w "menu rozwijanym zawartości widoku.</span><span class="sxs-lookup"><span data-stu-id="72375-118">We'll do this by selecting the "List" item within the "View content dropdown menu.</span></span>

<span data-ttu-id="72375-119">Należy pamiętać, że jeśli masz utworzony nową klasę należy skompilować aplikację dla niego były wyświetlane w oknie dialogowym Dodawanie widoku.</span><span class="sxs-lookup"><span data-stu-id="72375-119">Remember, when you have a created a new class you'll need to compile your application for it to show up in the Add View Dialog.</span></span>

![Dodaj widok](getting-started-with-mvc-part5/_static/image3.png)

<span data-ttu-id="72375-121">Kliknij przycisk Dodaj, a system automatycznie generuje kod dla widoku firmie Microsoft, które wyświetla naszej listy filmów.</span><span class="sxs-lookup"><span data-stu-id="72375-121">Click add and the system will automatically generate the code for a View for us that displays our list of movies.</span></span> <span data-ttu-id="72375-122">Jest to odpowiedni moment, aby zmienić &lt;h2&gt; nagłówek podobną "Moje listy filmów", jak robiliśmy wcześniej z widokiem Hello World.</span><span class="sxs-lookup"><span data-stu-id="72375-122">This is a good time to change the &lt;h2&gt; heading to something like "My Movie List" like we did earlier with the Hello World view.</span></span>

<span data-ttu-id="72375-123">[![Filmów - Microsoft Visual Web Developer 2010 Express.](getting-started-with-mvc-part5/_static/image5.png)](getting-started-with-mvc-part5/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="72375-123">[![Movies - Microsoft Visual Web Developer 2010 Express](getting-started-with-mvc-part5/_static/image5.png)](getting-started-with-mvc-part5/_static/image4.png)</span></span>

<span data-ttu-id="72375-124">Uruchom aplikację, a następnie odwiedź /Movies na pasku adresu.</span><span class="sxs-lookup"><span data-stu-id="72375-124">Run your application and visit /Movies in the address bar.</span></span> <span data-ttu-id="72375-125">Wprowadzeniu teraz pobrać danych z bazy danych przy użyciu zapytania podstawowego wewnątrz kontrolera i zwrócone dane do widoku, który zna filmów.</span><span class="sxs-lookup"><span data-stu-id="72375-125">Now we've retrieved data from the database using a basic query inside the Controller and returned the data to a View that knows about Movies.</span></span> <span data-ttu-id="72375-126">Ten widok, a następnie obraca za pośrednictwem listy filmów i tworzy tabelę danych firmie Microsoft.</span><span class="sxs-lookup"><span data-stu-id="72375-126">That View then spins through the list of Movies and creates a table of data for us.</span></span>

<span data-ttu-id="72375-127">[![Listy filmów - Windows Internet Explorer](getting-started-with-mvc-part5/_static/image7.png)](getting-started-with-mvc-part5/_static/image6.png)</span><span class="sxs-lookup"><span data-stu-id="72375-127">[![Movie List - Windows Internet Explorer](getting-started-with-mvc-part5/_static/image7.png)](getting-started-with-mvc-part5/_static/image6.png)</span></span>

<span data-ttu-id="72375-128">Firma Microsoft nie będzie można Implementowanie funkcje edycji, szczegóły i Usuń z tej aplikacji — potrzebujemy nie domyślnych łączy utworzony szablon szkieletu firmie Microsoft.</span><span class="sxs-lookup"><span data-stu-id="72375-128">We won't be implementing Edit, Details and Delete functionality with this application - so we don't need the default links that the scaffold template created for us.</span></span> <span data-ttu-id="72375-129">Otwarcie pliku /Movies/Index.aspx i usuń je.</span><span class="sxs-lookup"><span data-stu-id="72375-129">Open up the /Movies/Index.aspx file and remove them.</span></span>

<span data-ttu-id="72375-130">Oto co powinna wyglądać naszych zaktualizowany szablon widoku po możemy wprowadzić te zmiany kodu źródłowego:</span><span class="sxs-lookup"><span data-stu-id="72375-130">Here is the source code for what our updated View template should look like once we make these changes:</span></span>

[!code-aspx[Main](getting-started-with-mvc-part5/samples/sample2.aspx)]

<span data-ttu-id="72375-131">Tworzenia łącza, które nie będą potrzebne, a więc będzie usunąć je w tym przykładzie.</span><span class="sxs-lookup"><span data-stu-id="72375-131">It's creating links that we won't need, so we'll delete them for this example.</span></span> <span data-ttu-id="72375-132">Firma Microsoft zachowa naszych utworzyć nowy link, jak jest to dalej!</span><span class="sxs-lookup"><span data-stu-id="72375-132">We will keep our Create New link though, as that's next!</span></span> <span data-ttu-id="72375-133">Oto, jak wygląda aplikacji z tą kolumną usunięte.</span><span class="sxs-lookup"><span data-stu-id="72375-133">Here's what our app looks like with that column removed.</span></span>

<span data-ttu-id="72375-134">[![Listy filmów - Windows Internet Explorer](getting-started-with-mvc-part5/_static/image9.png)](getting-started-with-mvc-part5/_static/image8.png)</span><span class="sxs-lookup"><span data-stu-id="72375-134">[![Movie List - Windows Internet Explorer](getting-started-with-mvc-part5/_static/image9.png)](getting-started-with-mvc-part5/_static/image8.png)</span></span>

<span data-ttu-id="72375-135">Mamy teraz proste wykaz danych filmu.</span><span class="sxs-lookup"><span data-stu-id="72375-135">We now have a simple listing of our movie data.</span></span> <span data-ttu-id="72375-136">Jednak po kliknięciu pozycji "Utwórz nowy" łącza, firma Microsoft będzie wyświetlany komunikat o błędzie jako nie jest podłączonymi!</span><span class="sxs-lookup"><span data-stu-id="72375-136">However, if we click the "Create New" link, we'll get an error as it's not hooked up!</span></span> <span data-ttu-id="72375-137">Umożliwia implementuje metody tworzenia akcji i umożliwić użytkownikom wprowadzanie nowości w naszej bazie danych.</span><span class="sxs-lookup"><span data-stu-id="72375-137">Let's implement a Create Action method and enable a user to enter new movies in our database.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="72375-138">[Poprzednie](getting-started-with-mvc-part4.md)
[dalej](getting-started-with-mvc-part6.md)</span><span class="sxs-lookup"><span data-stu-id="72375-138">[Previous](getting-started-with-mvc-part4.md)
[Next](getting-started-with-mvc-part6.md)</span></span>
