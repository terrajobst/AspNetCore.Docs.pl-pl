---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part1
title: Wprowadzenie do platformy ASP.NET MVC | Dokumentacja firmy Microsoft
author: shanselman
description: Jest to samouczek dla początkujących, która przedstawia podstawy platformy ASP.NET MVC. Utwórz prostą aplikację sieci web wykonującej Odczyt i zapis z bazy danych.
ms.author: aspnetcontent
ms.date: 08/14/2010
ms.assetid: bf4a1c19-0a94-4208-b268-a96ddcf26946
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part1
msc.type: authoredcontent
ms.openlocfilehash: ee39cf78835767d73bd9a1d4c9a2c4ca75aca5fa
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/05/2018
ms.locfileid: "37808868"
---
<a name="intro-to-aspnet-mvc"></a><span data-ttu-id="55a09-104">Wprowadzenie do platformy ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="55a09-104">Intro to ASP.NET MVC</span></span>
====================
<span data-ttu-id="55a09-105">przez [Scotta Hanselmana](https://github.com/shanselman)</span><span class="sxs-lookup"><span data-stu-id="55a09-105">by [Scott Hanselman](https://github.com/shanselman)</span></span>

> > [!NOTE]
> > <span data-ttu-id="55a09-106">Zaktualizowanej wersji, jeśli w tym samouczku jest dostępny [tutaj](../../getting-started/introduction/getting-started.md) przy użyciu [programu Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads).</span><span class="sxs-lookup"><span data-stu-id="55a09-106">An updated version if this tutorial is available [here](../../getting-started/introduction/getting-started.md) using [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads).</span></span> <span data-ttu-id="55a09-107">Nowe samouczku ASP.NET MVC 5, która udostępnia wiele ulepszeń w porównaniu z tego samouczka.</span><span class="sxs-lookup"><span data-stu-id="55a09-107">The new tutorial uses ASP.NET MVC 5, which provides many improvements over this tutorial.</span></span>
> 
> 
> <span data-ttu-id="55a09-108">Jest to samouczek dla początkujących, która przedstawia podstawy platformy ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="55a09-108">This is a beginner tutorial that introduces the basics of ASP.NET MVC.</span></span> <span data-ttu-id="55a09-109">Utworzysz prostą aplikację sieci web wykonującej Odczyt i zapis z bazy danych.</span><span class="sxs-lookup"><span data-stu-id="55a09-109">You'll create a simple web application that reads and writes from a database.</span></span> <span data-ttu-id="55a09-110">Odwiedź stronę [Centrum szkoleniowe programu ASP.NET MVC](../../../index.md) można znaleźć inne platformy ASP.NET MVC, samouczków i przykładów.</span><span class="sxs-lookup"><span data-stu-id="55a09-110">Visit the [ASP.NET MVC learning center](../../../index.md) to find other ASP.NET MVC tutorials and samples.</span></span>


<span data-ttu-id="55a09-111">Upewnijmy się, w naszej pierwszej aplikacji sieci Web programu ASP.NET MVC przy użyciu [Visual Web Developer 2010 Express](https://www.microsoft.com/express/Web/).</span><span class="sxs-lookup"><span data-stu-id="55a09-111">Let's make our first ASP.NET MVC Web Application using [Visual Web Developer 2010 Express](https://www.microsoft.com/express/Web/).</span></span> <span data-ttu-id="55a09-112">Wybierzemy nieco aplikacji listy filmów, która będzie Daj nam tworzenie i wyświetlanie listy filmów.</span><span class="sxs-lookup"><span data-stu-id="55a09-112">We'll make a little Movie list application that will let us create and list movies.</span></span>

## <a name="what-youll-build"></a><span data-ttu-id="55a09-113">Jakie będziesz tworzyć</span><span class="sxs-lookup"><span data-stu-id="55a09-113">What You'll Build</span></span>

<span data-ttu-id="55a09-114">Poniżej przedstawiono dwa zrzuty ekranu aplikacji, którą utworzysz.</span><span class="sxs-lookup"><span data-stu-id="55a09-114">Here are two screenshots of the application you'll build.</span></span> <span data-ttu-id="55a09-115">Będziesz mieć prostą tabelę filmów z różnych kolumn.</span><span class="sxs-lookup"><span data-stu-id="55a09-115">You will have a simple table of movies with various columns.</span></span>

<span data-ttu-id="55a09-116">[![Lista filmu — Windows Internet Explorer (12)](getting-started-with-mvc-part1/_static/image2.png)](getting-started-with-mvc-part1/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="55a09-116">[![Movie List - Windows Internet Explorer (12)](getting-started-with-mvc-part1/_static/image2.png)](getting-started-with-mvc-part1/_static/image1.png)</span></span>

<span data-ttu-id="55a09-117">I należy utworzyć formularz, abyśmy mogli dodać do listy filmów.</span><span class="sxs-lookup"><span data-stu-id="55a09-117">And you'll have a Create Form so we can add movies to the list.</span></span>

<span data-ttu-id="55a09-118">[![Utwórz film — Windows Internet Explorer (2)](getting-started-with-mvc-part1/_static/image4.png)](getting-started-with-mvc-part1/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="55a09-118">[![Create a Movie - Windows Internet Explorer (2)](getting-started-with-mvc-part1/_static/image4.png)](getting-started-with-mvc-part1/_static/image3.png)</span></span>

## <a name="skills-youll-learn"></a><span data-ttu-id="55a09-119">Umiejętności, których dowiesz się</span><span class="sxs-lookup"><span data-stu-id="55a09-119">Skills You'll Learn</span></span>

<span data-ttu-id="55a09-120">Ta seria samouczków obejmuje podstawy tworzenia aplikacji sieci Web ASP.NET MVC przy użyciu programu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="55a09-120">This tutorial will teach you the basics of building an ASP.NET MVC Web Application using Visual Studio.</span></span> <span data-ttu-id="55a09-121">Dowiesz się:</span><span class="sxs-lookup"><span data-stu-id="55a09-121">You'll learn:</span></span>

- <span data-ttu-id="55a09-122">Tworzenie nowego projektu MVC ASP.NET</span><span class="sxs-lookup"><span data-stu-id="55a09-122">How to create a new ASP.NET MVC Project</span></span>
- <span data-ttu-id="55a09-123">Jak utworzyć nową bazę danych za pomocą programu SQL Server</span><span class="sxs-lookup"><span data-stu-id="55a09-123">How to create a new Database with SQL Server</span></span>
- <span data-ttu-id="55a09-124">Sposób tworzenia widoków i kontrolerów MVC ASP.NET</span><span class="sxs-lookup"><span data-stu-id="55a09-124">How to create ASP.NET MVC Controllers and Views</span></span>
- <span data-ttu-id="55a09-125">Sposób pobierania i wyświetlania danych</span><span class="sxs-lookup"><span data-stu-id="55a09-125">How to retrieve and display data</span></span>
- <span data-ttu-id="55a09-126">Jak edytować dane i włączyć sprawdzanie poprawności danych</span><span class="sxs-lookup"><span data-stu-id="55a09-126">How to edit data and enable data validation</span></span>
- <span data-ttu-id="55a09-127">Jak zaktualizować schemat bazy danych</span><span class="sxs-lookup"><span data-stu-id="55a09-127">How to update the database schema</span></span>

## <a name="get-started"></a><span data-ttu-id="55a09-128">Rozpocznij</span><span class="sxs-lookup"><span data-stu-id="55a09-128">Get Started</span></span>

<span data-ttu-id="55a09-129">Rozpocznij, korzystając z polecenia programu Visual Web Developer 2010 Express (będzie nazywam "VWD" od teraz) i wybierz pozycję Nowy projekt ekranu startowego.</span><span class="sxs-lookup"><span data-stu-id="55a09-129">Start by running Visual Web Developer 2010 Express (I'll call it "VWD" from now on) and select New Project from the Start Screen.</span></span>

<span data-ttu-id="55a09-130">Visual Web Developer jest środowiskiem IDE lub zintegrowanego środowiska deweloperskiego.</span><span class="sxs-lookup"><span data-stu-id="55a09-130">Visual Web Developer is an IDE, or Integrated Developer Environment.</span></span> <span data-ttu-id="55a09-131">Tak samo jak w programie Microsoft Word do zapisywania dokumentów, użyjesz środowisko IDE do tworzenia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="55a09-131">Just like you use Microsoft Word to write documents, you'll use an IDE to create applications.</span></span> <span data-ttu-id="55a09-132">Brak narzędzi wzdłuż górnej przedstawiający różne opcje dostępne do można, a także menu mógł możesz również użyć do wybierz plik | Nowy projekt.</span><span class="sxs-lookup"><span data-stu-id="55a09-132">There's a toolbar along the top showing various options available to you, as well as the menu you could also have used to Select File | New project.</span></span>

<span data-ttu-id="55a09-133">[![Microsoft Visual Web Developer 2010 Express](getting-started-with-mvc-part1/_static/image6.png)](getting-started-with-mvc-part1/_static/image5.png)</span><span class="sxs-lookup"><span data-stu-id="55a09-133">[![Microsoft Visual Web Developer 2010 Express](getting-started-with-mvc-part1/_static/image6.png)](getting-started-with-mvc-part1/_static/image5.png)</span></span>

## <a name="creating-your-first-application"></a><span data-ttu-id="55a09-134">Tworzenie pierwszej aplikacji</span><span class="sxs-lookup"><span data-stu-id="55a09-134">Creating Your First Application</span></span>

<span data-ttu-id="55a09-135">Można tworzyć aplikacje przy użyciu języka Visual Basic lub Visual C#.</span><span class="sxs-lookup"><span data-stu-id="55a09-135">You can create applications using Visual Basic or Visual C#.</span></span> <span data-ttu-id="55a09-136">Teraz, wybierz pozycję Visual C# po lewej stronie a następnie wybierz "Aplikacja sieci Web ASP.NET MVC 2".</span><span class="sxs-lookup"><span data-stu-id="55a09-136">For now, Select Visual C# on the left, then pick "ASP.NET MVC 2 Web Application."</span></span> <span data-ttu-id="55a09-137">Nazwij swój projekt "Filmy", a następnie kliknij przycisk OK.</span><span class="sxs-lookup"><span data-stu-id="55a09-137">Name your project "Movies" and click OK.</span></span>

<span data-ttu-id="55a09-138">[![Nowy projekt](getting-started-with-mvc-part1/_static/image8.png)](getting-started-with-mvc-part1/_static/image7.png)</span><span class="sxs-lookup"><span data-stu-id="55a09-138">[![New Project](getting-started-with-mvc-part1/_static/image8.png)](getting-started-with-mvc-part1/_static/image7.png)</span></span>

<span data-ttu-id="55a09-139">Po prawej stronie jest Eksploratora rozwiązań przedstawiający wszystkie pliki i foldery w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="55a09-139">On the right-hand side is the Solution Explorer showing all the files and folders in your application.</span></span> <span data-ttu-id="55a09-140">Duże okno w środku jest umożliwiającym edytowanie kodu i spędzają większość czasu.</span><span class="sxs-lookup"><span data-stu-id="55a09-140">The big window in the middle is where you edit your code and spend most of your time.</span></span> <span data-ttu-id="55a09-141">Program Visual Studio ulegał szablonu domyślnego dla projektu platformy ASP.NET MVC, który został utworzony, więc teraz utworzono działającą aplikację bez żadnego działania!</span><span class="sxs-lookup"><span data-stu-id="55a09-141">Visual Studio used a default template for the ASP.NET MVC project you just created, so you have a working application right now without doing anything!</span></span> <span data-ttu-id="55a09-142">Jest to prosty tekst "Hello World!</span><span class="sxs-lookup"><span data-stu-id="55a09-142">This is a simple "Hello World!</span></span> <span data-ttu-id="55a09-143">Projekt i jest dobrym miejscem do uruchomienia dla naszej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="55a09-143">project, and it is a good place to start for our application.</span></span>

<span data-ttu-id="55a09-144">[![Microsoft Visual Web Developer 2010 Express](getting-started-with-mvc-part1/_static/image10.png)](getting-started-with-mvc-part1/_static/image9.png)</span><span class="sxs-lookup"><span data-stu-id="55a09-144">[![Microsoft Visual Web Developer 2010 Express](getting-started-with-mvc-part1/_static/image10.png)](getting-started-with-mvc-part1/_static/image9.png)</span></span>

<span data-ttu-id="55a09-145">Wybierz przycisk "odtwarzania" na pasku narzędzi.</span><span class="sxs-lookup"><span data-stu-id="55a09-145">Select the "play" button to the toolbar.</span></span>

![Rozpocznij debugowanie](getting-started-with-mvc-part1/_static/image11.png)

<span data-ttu-id="55a09-147">Jest zielona strzałka w prawo, który będzie kompilujesz program i uruchomić aplikację w przeglądarce sieci web.</span><span class="sxs-lookup"><span data-stu-id="55a09-147">It's a green arrow pointing to the right that will compile your program and start your application in a web browser.</span></span>

<span data-ttu-id="55a09-148">*Uwaga: Możesz zamiast tego na klawiaturze naciśnij klawisz F5 lub wybierz debugowania -&gt;Rozpocznij debugowanie z menu "Debugowanie".*</span><span class="sxs-lookup"><span data-stu-id="55a09-148">*NOTE: You can instead press F5 on your keyboard, or select Debug-&gt;Start Debugging from the "Debug" menu.*</span></span>

<span data-ttu-id="55a09-149">Spowoduje to Visual Web Developer uruchomić serwer sieci web development i uruchomić aplikację sieci web (nie ma żadnych konfiguracji lub wymagane umożliwiające wykonanie tej czynności ręczne).</span><span class="sxs-lookup"><span data-stu-id="55a09-149">This will cause Visual Web Developer to start a development web-server and run our web application (there are no configuration or manual steps required to enable this).</span></span> <span data-ttu-id="55a09-150">Zostanie następnie uruchomi przeglądarkę i skonfiguruj ją, aby przejść do strony głównej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="55a09-150">It will then launch a browser and configure it to browse the application's home-page.</span></span> <span data-ttu-id="55a09-151">Zwróć uwagę, poniżej, na pasku adresu przeglądarki jest wyświetlany komunikat "localhost", a nie podobny do example.com.</span><span class="sxs-lookup"><span data-stu-id="55a09-151">Notice below that the address bar of the browser says "localhost", and not something like example.com.</span></span> <span data-ttu-id="55a09-152">Wynika to z localhost zawsze wskazuje na swoim komputerze lokalnym — działającego w takim przypadku aplikacji, którą właśnie utworzyliśmy.</span><span class="sxs-lookup"><span data-stu-id="55a09-152">That's because localhost always points to your own local computer - which in this case is running the application we just built.</span></span>

<span data-ttu-id="55a09-153">[![Strona główna](getting-started-with-mvc-part1/_static/image13.png)](getting-started-with-mvc-part1/_static/image12.png)</span><span class="sxs-lookup"><span data-stu-id="55a09-153">[![Home Page](getting-started-with-mvc-part1/_static/image13.png)](getting-started-with-mvc-part1/_static/image12.png)</span></span>

<span data-ttu-id="55a09-154">Gotowych ten szablon domyślny zawiera dwie strony do odwiedzenia i strony logowania podstawowe.</span><span class="sxs-lookup"><span data-stu-id="55a09-154">Out of the box this default template gives you two pages to visit and a basic login page.</span></span> <span data-ttu-id="55a09-155">Możemy zmienić sposób działania tej aplikacji i Poznaj nieco platformy ASP.NET MVC w procesie.</span><span class="sxs-lookup"><span data-stu-id="55a09-155">Let's change how this application works and learn a little bit about ASP.NET MVC in the process.</span></span> <span data-ttu-id="55a09-156">Zamknij przeglądarkę i umożliwia zmiany kodu.</span><span class="sxs-lookup"><span data-stu-id="55a09-156">Close your browser and lets change some code.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="55a09-157">Next</span><span class="sxs-lookup"><span data-stu-id="55a09-157">Next</span></span>](getting-started-with-mvc-part2.md)
