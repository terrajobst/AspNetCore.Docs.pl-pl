---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part1
title: Wprowadzenie do platformy ASP.NET MVC | Dokumentacja firmy Microsoft
author: shanselman
description: Jest to samouczek początkujących przedstawiający podstawowe informacje o platformie ASP.NET MVC. Utwórz prostą aplikację sieci web odczytuje i zapisuje z bazy danych.
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/14/2010
ms.topic: article
ms.assetid: bf4a1c19-0a94-4208-b268-a96ddcf26946
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part1
msc.type: authoredcontent
ms.openlocfilehash: 476d832e389b9b5a26fe2d552ca648c79b100056
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
---
<a name="intro-to-aspnet-mvc"></a><span data-ttu-id="247c4-104">Wprowadzenie do platformy ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="247c4-104">Intro to ASP.NET MVC</span></span>
====================
<span data-ttu-id="247c4-105">przez [Scott Hanselman](https://github.com/shanselman)</span><span class="sxs-lookup"><span data-stu-id="247c4-105">by [Scott Hanselman](https://github.com/shanselman)</span></span>

> > [!NOTE]
> > <span data-ttu-id="247c4-106">Zaktualizowanej wersji, jeśli jest dostępna w tym samouczku [tutaj](../../getting-started/introduction/getting-started.md) przy użyciu [programu Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads).</span><span class="sxs-lookup"><span data-stu-id="247c4-106">An updated version if this tutorial is available [here](../../getting-started/introduction/getting-started.md) using [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads).</span></span> <span data-ttu-id="247c4-107">Nowe samouczku platformy ASP.NET MVC 5, która udostępnia wiele ulepszeń w porównaniu z tego samouczka.</span><span class="sxs-lookup"><span data-stu-id="247c4-107">The new tutorial uses ASP.NET MVC 5, which provides many improvements over this tutorial.</span></span>
> 
> 
> <span data-ttu-id="247c4-108">Jest to samouczek początkujących przedstawiający podstawowe informacje o platformie ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="247c4-108">This is a beginner tutorial that introduces the basics of ASP.NET MVC.</span></span> <span data-ttu-id="247c4-109">Utworzysz prostą aplikację sieci web odczytuje i zapisuje z bazy danych.</span><span class="sxs-lookup"><span data-stu-id="247c4-109">You'll create a simple web application that reads and writes from a database.</span></span> <span data-ttu-id="247c4-110">Odwiedź stronę [Centrum szkoleniowe programu ASP.NET MVC](../../../index.md) można znaleźć inne platformy ASP.NET MVC, samouczki i przykłady.</span><span class="sxs-lookup"><span data-stu-id="247c4-110">Visit the [ASP.NET MVC learning center](../../../index.md) to find other ASP.NET MVC tutorials and samples.</span></span>


<span data-ttu-id="247c4-111">Upewnijmy naszych pierwszej aplikacji sieci Web platformy ASP.NET MVC przy użyciu [Visual Web Developer 2010 Express](https://www.microsoft.com/express/Web/).</span><span class="sxs-lookup"><span data-stu-id="247c4-111">Let's make our first ASP.NET MVC Web Application using [Visual Web Developer 2010 Express](https://www.microsoft.com/express/Web/).</span></span> <span data-ttu-id="247c4-112">Wybierzemy nieco filmu listy aplikacji, która zostanie utworzone i Daj nam listy filmów.</span><span class="sxs-lookup"><span data-stu-id="247c4-112">We'll make a little Movie list application that will let us create and list movies.</span></span>

## <a name="what-youll-build"></a><span data-ttu-id="247c4-113">Jakie będzie kompilacji</span><span class="sxs-lookup"><span data-stu-id="247c4-113">What You'll Build</span></span>

<span data-ttu-id="247c4-114">Poniżej przedstawiono dwa zrzuty ekranu aplikacji, która będzie kompilacji.</span><span class="sxs-lookup"><span data-stu-id="247c4-114">Here are two screenshots of the application you'll build.</span></span> <span data-ttu-id="247c4-115">Będziesz mieć prostą tabelę filmów z różnych kolumn.</span><span class="sxs-lookup"><span data-stu-id="247c4-115">You will have a simple table of movies with various columns.</span></span>

<span data-ttu-id="247c4-116">[![Listy filmów - Windows Internet Explorer (12)](getting-started-with-mvc-part1/_static/image2.png)](getting-started-with-mvc-part1/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="247c4-116">[![Movie List - Windows Internet Explorer (12)](getting-started-with-mvc-part1/_static/image2.png)](getting-started-with-mvc-part1/_static/image1.png)</span></span>

<span data-ttu-id="247c4-117">I należy utworzyć formularza, filmy można dodać do listy.</span><span class="sxs-lookup"><span data-stu-id="247c4-117">And you'll have a Create Form so we can add movies to the list.</span></span>

<span data-ttu-id="247c4-118">[![Tworzenie filmu — Windows Internet Explorer (2)](getting-started-with-mvc-part1/_static/image4.png)](getting-started-with-mvc-part1/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="247c4-118">[![Create a Movie - Windows Internet Explorer (2)](getting-started-with-mvc-part1/_static/image4.png)](getting-started-with-mvc-part1/_static/image3.png)</span></span>

## <a name="skills-youll-learn"></a><span data-ttu-id="247c4-119">Dowiesz się umiejętności</span><span class="sxs-lookup"><span data-stu-id="247c4-119">Skills You'll Learn</span></span>

<span data-ttu-id="247c4-120">Ten samouczek pokazuje podstawowe informacje dotyczące tworzenia aplikacji sieci Web ASP.NET MVC przy użyciu programu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="247c4-120">This tutorial will teach you the basics of building an ASP.NET MVC Web Application using Visual Studio.</span></span> <span data-ttu-id="247c4-121">Dowiesz się:</span><span class="sxs-lookup"><span data-stu-id="247c4-121">You'll learn:</span></span>

- <span data-ttu-id="247c4-122">Jak utworzyć nowy projekt ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="247c4-122">How to create a new ASP.NET MVC Project</span></span>
- <span data-ttu-id="247c4-123">Jak utworzyć nową bazę danych programu SQL Server</span><span class="sxs-lookup"><span data-stu-id="247c4-123">How to create a new Database with SQL Server</span></span>
- <span data-ttu-id="247c4-124">Tworzenie widoków i kontrolerów MVC ASP.NET</span><span class="sxs-lookup"><span data-stu-id="247c4-124">How to create ASP.NET MVC Controllers and Views</span></span>
- <span data-ttu-id="247c4-125">Jak pobrać i wyświetlanie danych</span><span class="sxs-lookup"><span data-stu-id="247c4-125">How to retrieve and display data</span></span>
- <span data-ttu-id="247c4-126">Jak edytować dane i włączyć sprawdzanie poprawności danych</span><span class="sxs-lookup"><span data-stu-id="247c4-126">How to edit data and enable data validation</span></span>
- <span data-ttu-id="247c4-127">Jak zaktualizować schemat bazy danych</span><span class="sxs-lookup"><span data-stu-id="247c4-127">How to update the database schema</span></span>

## <a name="get-started"></a><span data-ttu-id="247c4-128">Rozpocznij</span><span class="sxs-lookup"><span data-stu-id="247c4-128">Get Started</span></span>

<span data-ttu-id="247c4-129">Uruchom za pomocą programu Visual Web Developer 2010 Express (I będzie wywołać ją "VWD" od teraz) i wybierz nowy projekt z ekranu startowego.</span><span class="sxs-lookup"><span data-stu-id="247c4-129">Start by running Visual Web Developer 2010 Express (I'll call it "VWD" from now on) and select New Project from the Start Screen.</span></span>

<span data-ttu-id="247c4-130">Visual Web Developer jest IDE lub zintegrowane środowisko deweloperskie.</span><span class="sxs-lookup"><span data-stu-id="247c4-130">Visual Web Developer is an IDE, or Integrated Developer Environment.</span></span> <span data-ttu-id="247c4-131">Tak samo jak w programie Microsoft Word do zapisywania dokumentów, użyjesz IDE do tworzenia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="247c4-131">Just like you use Microsoft Word to write documents, you'll use an IDE to create applications.</span></span> <span data-ttu-id="247c4-132">Brak narzędzi wzdłuż górnej pokazywanie różnych dostępnych opcji użytkownik, a także menu mógł można również użyć do pliku wybierz | Nowy projekt.</span><span class="sxs-lookup"><span data-stu-id="247c4-132">There's a toolbar along the top showing various options available to you, as well as the menu you could also have used to Select File | New project.</span></span>

<span data-ttu-id="247c4-133">[![Microsoft Visual Web Developer 2010 Express](getting-started-with-mvc-part1/_static/image6.png)](getting-started-with-mvc-part1/_static/image5.png)</span><span class="sxs-lookup"><span data-stu-id="247c4-133">[![Microsoft Visual Web Developer 2010 Express](getting-started-with-mvc-part1/_static/image6.png)](getting-started-with-mvc-part1/_static/image5.png)</span></span>

## <a name="creating-your-first-application"></a><span data-ttu-id="247c4-134">Tworzenie pierwszej aplikacji</span><span class="sxs-lookup"><span data-stu-id="247c4-134">Creating Your First Application</span></span>

<span data-ttu-id="247c4-135">Można tworzyć aplikacje przy użyciu języka Visual Basic lub Visual C#.</span><span class="sxs-lookup"><span data-stu-id="247c4-135">You can create applications using Visual Basic or Visual C#.</span></span> <span data-ttu-id="247c4-136">Teraz, wybierz pozycję Visual C# po lewej stronie a następnie wybierz "Aplikacja sieci Web platformy ASP.NET MVC 2".</span><span class="sxs-lookup"><span data-stu-id="247c4-136">For now, Select Visual C# on the left, then pick "ASP.NET MVC 2 Web Application."</span></span> <span data-ttu-id="247c4-137">Nazwa projektu "Filmy", a następnie kliknij przycisk OK.</span><span class="sxs-lookup"><span data-stu-id="247c4-137">Name your project "Movies" and click OK.</span></span>

<span data-ttu-id="247c4-138">[![Nowy projekt](getting-started-with-mvc-part1/_static/image8.png)](getting-started-with-mvc-part1/_static/image7.png)</span><span class="sxs-lookup"><span data-stu-id="247c4-138">[![New Project](getting-started-with-mvc-part1/_static/image8.png)](getting-started-with-mvc-part1/_static/image7.png)</span></span>

<span data-ttu-id="247c4-139">Po prawej stronie jest Eksploratora rozwiązań przedstawiający wszystkie pliki i foldery w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="247c4-139">On the right-hand side is the Solution Explorer showing all the files and folders in your application.</span></span> <span data-ttu-id="247c4-140">Big okna w środku jest, gdzie edytować kod i spędzają większość czasu.</span><span class="sxs-lookup"><span data-stu-id="247c4-140">The big window in the middle is where you edit your code and spend most of your time.</span></span> <span data-ttu-id="247c4-141">Visual Studio użyty domyślny szablon projektu programu ASP.NET MVC, który właśnie utworzony, więc teraz mieć działającą aplikację bez żadnego działania!</span><span class="sxs-lookup"><span data-stu-id="247c4-141">Visual Studio used a default template for the ASP.NET MVC project you just created, so you have a working application right now without doing anything!</span></span> <span data-ttu-id="247c4-142">Jest to prosty "Witaj świecie!</span><span class="sxs-lookup"><span data-stu-id="247c4-142">This is a simple "Hello World!</span></span> <span data-ttu-id="247c4-143">Projekt który jest dobrym miejscem do uruchomienia dla naszej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="247c4-143">project, and it is a good place to start for our application.</span></span>

<span data-ttu-id="247c4-144">[![Microsoft Visual Web Developer 2010 Express](getting-started-with-mvc-part1/_static/image10.png)](getting-started-with-mvc-part1/_static/image9.png)</span><span class="sxs-lookup"><span data-stu-id="247c4-144">[![Microsoft Visual Web Developer 2010 Express](getting-started-with-mvc-part1/_static/image10.png)](getting-started-with-mvc-part1/_static/image9.png)</span></span>

<span data-ttu-id="247c4-145">Kliknij przycisk "play" na pasku narzędzi.</span><span class="sxs-lookup"><span data-stu-id="247c4-145">Select the "play" button to the toolbar.</span></span>

![Rozpocznij debugowanie](getting-started-with-mvc-part1/_static/image11.png)

<span data-ttu-id="247c4-147">Jest zielona strzałka wskazująca w prawo, program skompilować i uruchomić aplikację w przeglądarce sieci web.</span><span class="sxs-lookup"><span data-stu-id="247c4-147">It's a green arrow pointing to the right that will compile your program and start your application in a web browser.</span></span>

<span data-ttu-id="247c4-148">*Uwaga: Możesz zamiast tego na klawiaturze naciśnij klawisz F5, lub wybrać debugowania -&gt;Rozpocznij debugowanie z menu "Debugowanie".*</span><span class="sxs-lookup"><span data-stu-id="247c4-148">*NOTE: You can instead press F5 on your keyboard, or select Debug-&gt;Start Debugging from the "Debug" menu.*</span></span>

<span data-ttu-id="247c4-149">Spowoduje to Visual Web Developer uruchomić serwer sieci web development i uruchomić aplikację sieci web (nie ma żadnych konfiguracji lub ręczne wykonanie czynności wymagane do włączenia to).</span><span class="sxs-lookup"><span data-stu-id="247c4-149">This will cause Visual Web Developer to start a development web-server and run our web application (there are no configuration or manual steps required to enable this).</span></span> <span data-ttu-id="247c4-150">Następnie zostanie uruchomi przeglądarkę i skonfiguruj go, aby przejść do strony głównej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="247c4-150">It will then launch a browser and configure it to browse the application's home-page.</span></span> <span data-ttu-id="247c4-151">Poniżej zauważyć, że na pasku adresu przeglądarki mówi "localhost", a nie przypominać example.com. Wynika to z localhost zawsze wskazuje na własnym komputerze lokalnym — działającego w takim przypadku aplikacji, którą właśnie utworzony.</span><span class="sxs-lookup"><span data-stu-id="247c4-151">Notice below that the address bar of the browser says "localhost", and not something like example.com. That's because localhost always points to your own local computer - which in this case is running the application we just built.</span></span>

<span data-ttu-id="247c4-152">[![Strona główna](getting-started-with-mvc-part1/_static/image13.png)](getting-started-with-mvc-part1/_static/image12.png)</span><span class="sxs-lookup"><span data-stu-id="247c4-152">[![Home Page](getting-started-with-mvc-part1/_static/image13.png)](getting-started-with-mvc-part1/_static/image12.png)</span></span>

<span data-ttu-id="247c4-153">Poza pole ten szablon domyślny zawiera dwie strony do odwiedzenia i strony logowania podstawowe.</span><span class="sxs-lookup"><span data-stu-id="247c4-153">Out of the box this default template gives you two pages to visit and a basic login page.</span></span> <span data-ttu-id="247c4-154">Teraz zmienić sposób działania tej aplikacji i uzyskiwanie nieco ASP.NET MVC w procesie.</span><span class="sxs-lookup"><span data-stu-id="247c4-154">Let's change how this application works and learn a little bit about ASP.NET MVC in the process.</span></span> <span data-ttu-id="247c4-155">Zamknij przeglądarkę i pozwala zmienić kodu.</span><span class="sxs-lookup"><span data-stu-id="247c4-155">Close your browser and lets change some code.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="247c4-156">Next</span><span class="sxs-lookup"><span data-stu-id="247c4-156">Next</span></span>](getting-started-with-mvc-part2.md)
