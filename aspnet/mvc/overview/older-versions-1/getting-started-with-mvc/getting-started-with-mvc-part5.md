---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part5
title: Uzyskiwanie dostępu do danych modelu za pomocą kontrolera | Dokumentacja firmy Microsoft
author: shanselman
description: Jest to samouczek dla początkujących, która przedstawia podstawy platformy ASP.NET MVC. Utwórz prostą aplikację sieci web wykonującej Odczyt i zapis z bazy danych.
ms.author: aspnetcontent
ms.date: 08/14/2010
ms.assetid: 004703cd-e0e9-4ba7-9974-1b0475c71222
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part5
msc.type: authoredcontent
ms.openlocfilehash: 556eed77cbd9e81c0a2a1334bb0a8ee56abafd34
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/05/2018
ms.locfileid: "37815009"
---
<a name="accessing-your-models-data-from-a-controller"></a><span data-ttu-id="4ddfc-104">Uzyskiwanie dostępu do danych modelu za pomocą kontrolera</span><span class="sxs-lookup"><span data-stu-id="4ddfc-104">Accessing your Model's Data from a Controller</span></span>
====================
<span data-ttu-id="4ddfc-105">przez [Scotta Hanselmana](https://github.com/shanselman)</span><span class="sxs-lookup"><span data-stu-id="4ddfc-105">by [Scott Hanselman](https://github.com/shanselman)</span></span>

> <span data-ttu-id="4ddfc-106">Jest to samouczek dla początkujących, która przedstawia podstawy platformy ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="4ddfc-106">This is a beginner tutorial that introduces the basics of ASP.NET MVC.</span></span> <span data-ttu-id="4ddfc-107">Utworzysz prostą aplikację sieci web wykonującej Odczyt i zapis z bazy danych.</span><span class="sxs-lookup"><span data-stu-id="4ddfc-107">You'll create a simple web application that reads and writes from a database.</span></span> <span data-ttu-id="4ddfc-108">Odwiedź stronę [Centrum szkoleniowe programu ASP.NET MVC](../../../index.md) można znaleźć inne platformy ASP.NET MVC, samouczków i przykładów.</span><span class="sxs-lookup"><span data-stu-id="4ddfc-108">Visit the [ASP.NET MVC learning center](../../../index.md) to find other ASP.NET MVC tutorials and samples.</span></span>


<span data-ttu-id="4ddfc-109">W tej sekcji użyjemy Utwórz nową klasę MoviesController i pisanie kodu, który pobiera dane filmów i wyświetla go do przeglądarki przy użyciu szablonu widoku.</span><span class="sxs-lookup"><span data-stu-id="4ddfc-109">In this section we are going to create a new MoviesController class, and write some code that retrieves our Movie data and displays it back to the browser using a View template.</span></span>

<span data-ttu-id="4ddfc-110">Kliknij prawym przyciskiem folder kontrolery i upewnij MoviesController nowe.</span><span class="sxs-lookup"><span data-stu-id="4ddfc-110">Right click on the Controllers folder and make a new MoviesController.</span></span>

<span data-ttu-id="4ddfc-111">[![Dodawanie kontrolera](getting-started-with-mvc-part5/_static/image2.png)](getting-started-with-mvc-part5/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="4ddfc-111">[![Add Controller](getting-started-with-mvc-part5/_static/image2.png)](getting-started-with-mvc-part5/_static/image1.png)</span></span>

<span data-ttu-id="4ddfc-112">Spowoduje to utworzenie nowego pliku "MoviesController.cs" poniżej naszej folder \Controllers w projekcie.</span><span class="sxs-lookup"><span data-stu-id="4ddfc-112">This will create a new "MoviesController.cs" file underneath our \Controllers folder within our project.</span></span> <span data-ttu-id="4ddfc-113">Zaktualizujmy MovieController można pobrać listy filmów z naszym nowo wypełniania bazy danych.</span><span class="sxs-lookup"><span data-stu-id="4ddfc-113">Let's update the MovieController to retrieve the list of movies from our newly populated database.</span></span>

[!code-csharp[Main](getting-started-with-mvc-part5/samples/sample1.cs)]

<span data-ttu-id="4ddfc-114">Będziemy działają zapytanie LINQ, tak, aby tylko Pobieramy filmy wydana po lata 1984 roku.</span><span class="sxs-lookup"><span data-stu-id="4ddfc-114">We are performing a LINQ query so that we only retrieve movies released after the summer of 1984.</span></span> <span data-ttu-id="4ddfc-115">Będziemy potrzebować szablon widoku do renderowania tej listy filmów z powrotem, więc kliknij prawym przyciskiem myszy metodę i wybierz pozycję Dodaj widok, aby go utworzyć.</span><span class="sxs-lookup"><span data-stu-id="4ddfc-115">We'll need a View template to render this list of movies back, so right-click in the method and select Add View to create it.</span></span>

<span data-ttu-id="4ddfc-116">W oknie dialogowym Dodaj widok firma Microsoft będzie wskazywać, przechodzi listy&lt;Movies.Models.Movie&gt; do naszych szablonu widoku.</span><span class="sxs-lookup"><span data-stu-id="4ddfc-116">Within the Add View dialog we'll indicate that we are passing a List&lt;Movies.Models.Movie&gt; to our View template.</span></span> <span data-ttu-id="4ddfc-117">W przeciwieństwie do poprzednich razy możemy użyć okno dialogowe dodawania widoku i wybrał opcję utworzenia "Pusty" szablon tym razem, firma Microsoft będzie wskazują, że chcemy, aby automatycznie "tworzenia szkieletu" Wyświetl szablon dla nas z niektórych domyślnej zawartości programu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="4ddfc-117">Unlike the previous times we used the Add View dialog and chose to create an "Empty" template, this time we'll indicate that we want Visual Studio to automatically "scaffold" a view template for us with some default content.</span></span> <span data-ttu-id="4ddfc-118">Możemy to zrobić, wybierając pozycję "List" elementu "menu Widok zawartości listy rozwijanej.</span><span class="sxs-lookup"><span data-stu-id="4ddfc-118">We'll do this by selecting the "List" item within the "View content dropdown menu.</span></span>

<span data-ttu-id="4ddfc-119">Należy pamiętać, że jeśli masz utworzony nowej klasy, należy skompilować aplikację, aby wyświetlane w oknie dialogowym Dodawanie widoku.</span><span class="sxs-lookup"><span data-stu-id="4ddfc-119">Remember, when you have a created a new class you'll need to compile your application for it to show up in the Add View Dialog.</span></span>

![Dodawanie widoku](getting-started-with-mvc-part5/_static/image3.png)

<span data-ttu-id="4ddfc-121">Kliknij przycisk Dodaj, a system automatycznie wygeneruje kod dla widoku dla nas wyświetlanych z naszej listy filmów.</span><span class="sxs-lookup"><span data-stu-id="4ddfc-121">Click add and the system will automatically generate the code for a View for us that displays our list of movies.</span></span> <span data-ttu-id="4ddfc-122">Jest to dobry moment, aby zmienić &lt;h2&gt; nagłówek na wartość podobną "Moja lista filmu", takie jak były wykonywane wcześniej przy użyciu widoku Hello World.</span><span class="sxs-lookup"><span data-stu-id="4ddfc-122">This is a good time to change the &lt;h2&gt; heading to something like "My Movie List" like we did earlier with the Hello World view.</span></span>

<span data-ttu-id="4ddfc-123">[![Filmy — Microsoft Visual Web Developer 2010 Express](getting-started-with-mvc-part5/_static/image5.png)](getting-started-with-mvc-part5/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="4ddfc-123">[![Movies - Microsoft Visual Web Developer 2010 Express](getting-started-with-mvc-part5/_static/image5.png)](getting-started-with-mvc-part5/_static/image4.png)</span></span>

<span data-ttu-id="4ddfc-124">Uruchom aplikację, a następnie odwiedź /Movies na pasku adresu.</span><span class="sxs-lookup"><span data-stu-id="4ddfc-124">Run your application and visit /Movies in the address bar.</span></span> <span data-ttu-id="4ddfc-125">Firma Microsoft została teraz pobierane są dane z bazy danych przy użyciu podstawowego zapytania wewnątrz kontrolera i zwrócone dane do widoku, który zna filmy.</span><span class="sxs-lookup"><span data-stu-id="4ddfc-125">Now we've retrieved data from the database using a basic query inside the Controller and returned the data to a View that knows about Movies.</span></span> <span data-ttu-id="4ddfc-126">Ten widok, a następnie uruchamia się za pośrednictwem listy filmów i tworzy tabelę danych dla nas.</span><span class="sxs-lookup"><span data-stu-id="4ddfc-126">That View then spins through the list of Movies and creates a table of data for us.</span></span>

<span data-ttu-id="4ddfc-127">[![Lista filmu — Windows Internet Explorer](getting-started-with-mvc-part5/_static/image7.png)](getting-started-with-mvc-part5/_static/image6.png)</span><span class="sxs-lookup"><span data-stu-id="4ddfc-127">[![Movie List - Windows Internet Explorer](getting-started-with-mvc-part5/_static/image7.png)](getting-started-with-mvc-part5/_static/image6.png)</span></span>

<span data-ttu-id="4ddfc-128">Firma Microsoft nie będzie zaimplementowana funkcji edycji, szczegółów i usuwania, z tą aplikacją — więc nie potrzebujemy łącza domyślny szablon szkieletu utworzony dla nas.</span><span class="sxs-lookup"><span data-stu-id="4ddfc-128">We won't be implementing Edit, Details and Delete functionality with this application - so we don't need the default links that the scaffold template created for us.</span></span> <span data-ttu-id="4ddfc-129">Otwórz plik /Movies/Index.aspx i usuń je.</span><span class="sxs-lookup"><span data-stu-id="4ddfc-129">Open up the /Movies/Index.aspx file and remove them.</span></span>

<span data-ttu-id="4ddfc-130">Poniżej przedstawiono kod źródłowy dla naszych zaktualizowany szablon widoku jak powinien wyglądać po możemy wprowadzić te zmiany:</span><span class="sxs-lookup"><span data-stu-id="4ddfc-130">Here is the source code for what our updated View template should look like once we make these changes:</span></span>

[!code-aspx[Main](getting-started-with-mvc-part5/samples/sample2.aspx)]

<span data-ttu-id="4ddfc-131">Łączy, które firma Microsoft nie będzie już konieczne, która zostanie utworzona, więc usuniemy je w tym przykładzie</span><span class="sxs-lookup"><span data-stu-id="4ddfc-131">It's creating links that we won't need, so we'll delete them for this example.</span></span> <span data-ttu-id="4ddfc-132">Zostanie zachowana, aby nasze Utwórz nowe łącze, ponieważ to jest dalej!</span><span class="sxs-lookup"><span data-stu-id="4ddfc-132">We will keep our Create New link though, as that's next!</span></span> <span data-ttu-id="4ddfc-133">Oto jak wygląda naszą aplikację z tej kolumny usunięte.</span><span class="sxs-lookup"><span data-stu-id="4ddfc-133">Here's what our app looks like with that column removed.</span></span>

<span data-ttu-id="4ddfc-134">[![Lista filmu — Windows Internet Explorer](getting-started-with-mvc-part5/_static/image9.png)](getting-started-with-mvc-part5/_static/image8.png)</span><span class="sxs-lookup"><span data-stu-id="4ddfc-134">[![Movie List - Windows Internet Explorer](getting-started-with-mvc-part5/_static/image9.png)](getting-started-with-mvc-part5/_static/image8.png)</span></span>

<span data-ttu-id="4ddfc-135">W efekcie powstał prostą listę danych filmu.</span><span class="sxs-lookup"><span data-stu-id="4ddfc-135">We now have a simple listing of our movie data.</span></span> <span data-ttu-id="4ddfc-136">Jednak po kliknięciu przycisku "Utwórz nowy" link przejdziemy błąd, ponieważ nie jest podłączany!</span><span class="sxs-lookup"><span data-stu-id="4ddfc-136">However, if we click the "Create New" link, we'll get an error as it's not hooked up!</span></span> <span data-ttu-id="4ddfc-137">Przejdźmy implementuje metody tworzenia akcji i umożliwić użytkownikowi wprowadzanie nowych filmów w naszej bazie danych.</span><span class="sxs-lookup"><span data-stu-id="4ddfc-137">Let's implement a Create Action method and enable a user to enter new movies in our database.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="4ddfc-138">[Poprzednie](getting-started-with-mvc-part4.md)
> [dalej](getting-started-with-mvc-part6.md)</span><span class="sxs-lookup"><span data-stu-id="4ddfc-138">[Previous](getting-started-with-mvc-part4.md)
[Next](getting-started-with-mvc-part6.md)</span></span>
