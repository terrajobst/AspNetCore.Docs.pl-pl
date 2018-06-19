---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3
title: Wprowadzenie do platformy ASP.NET MVC 3 (C#) | Dokumentacja firmy Microsoft
author: Rick-Anderson
description: Ten samouczek pokazuje podstawowe informacje dotyczące tworzenia aplikacji sieci Web programu ASP.NET MVC przy użyciu Microsoft Visual Web Developer 2010 Express Service Pack 1, która jest...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/12/2011
ms.topic: article
ms.assetid: 86a80b35-88bd-4b7c-bd58-f6e7997197d4
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3
msc.type: authoredcontent
ms.openlocfilehash: 9b965df6175051a084de35627160161c116be42d
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
ms.locfileid: "30868932"
---
<a name="intro-to-aspnet-mvc-3-c"></a><span data-ttu-id="b5c6b-103">Wprowadzenie do platformy ASP.NET MVC 3 (C#)</span><span class="sxs-lookup"><span data-stu-id="b5c6b-103">Intro to ASP.NET MVC 3 (C#)</span></span>
====================
<span data-ttu-id="b5c6b-104">przez [Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="b5c6b-104">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

> > [!NOTE]
> > <span data-ttu-id="b5c6b-105">Dostępna jest zaktualizowana wersja tego samouczka [tutaj](../../../getting-started/introduction/getting-started.md) używającej platformy ASP.NET MVC 5 i Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="b5c6b-105">An updated version of this tutorial is available [here](../../../getting-started/introduction/getting-started.md) that uses ASP.NET MVC 5 and Visual Studio 2013.</span></span> <span data-ttu-id="b5c6b-106">Jest bardziej bezpieczne, łatwiej wykonać i pokazuje więcej funkcji.</span><span class="sxs-lookup"><span data-stu-id="b5c6b-106">It's more secure, much simpler to follow and demonstrates more features.</span></span>
> 
> 
> <span data-ttu-id="b5c6b-107">Ten samouczek pokazuje podstawowe informacje dotyczące tworzenia aplikacji sieci Web programu ASP.NET MVC przy użyciu Microsoft Visual Web Developer 2010 Express Service Pack 1, która jest bezpłatna wersja programu Microsoft Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="b5c6b-107">This tutorial will teach you the basics of building an ASP.NET MVC Web application using Microsoft Visual Web Developer 2010 Express Service Pack 1, which is a free version of Microsoft Visual Studio.</span></span> <span data-ttu-id="b5c6b-108">Przed rozpoczęciem upewnij się, że po zainstalowaniu wymagania wstępne wymienione poniżej.</span><span class="sxs-lookup"><span data-stu-id="b5c6b-108">Before you start, make sure you've installed the prerequisites listed below.</span></span> <span data-ttu-id="b5c6b-109">Można zainstalować wszystkie z nich, klikając poniższe łącze: [Instalatora platformy sieci Web](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack).</span><span class="sxs-lookup"><span data-stu-id="b5c6b-109">You can install all of them by clicking the following link: [Web Platform Installer](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack).</span></span> <span data-ttu-id="b5c6b-110">Alternatywnie można zainstalować oddzielnie wymagania wstępne, korzystając z następujących linków:</span><span class="sxs-lookup"><span data-stu-id="b5c6b-110">Alternatively, you can individually install the prerequisites using the following links:</span></span>
> 
> - [<span data-ttu-id="b5c6b-111">Visual Studio Web Developer Express z dodatkiem SP1 wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="b5c6b-111">Visual Studio Web Developer Express SP1 prerequisites</span></span>](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [<span data-ttu-id="b5c6b-112">Aktualizacji narzędzi programu ASP.NET MVC 3</span><span class="sxs-lookup"><span data-stu-id="b5c6b-112">ASP.NET MVC 3 Tools Update</span></span>](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - <span data-ttu-id="b5c6b-113">[SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(środowisko uruchomieniowe + narzędzia pomocy technicznej)</span><span class="sxs-lookup"><span data-stu-id="b5c6b-113">[SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(runtime + tools support)</span></span>
> 
> <span data-ttu-id="b5c6b-114">Jeśli używasz programu Visual Studio 2010, zamiast Visual Web Developer 2010, zainstaluj wymagania wstępne, klikając poniższe łącze: [wymagania wstępne programu Visual Studio 2010](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).</span><span class="sxs-lookup"><span data-stu-id="b5c6b-114">If you're using Visual Studio 2010 instead of Visual Web Developer 2010, install the prerequisites by clicking the following link: [Visual Studio 2010 prerequisites](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).</span></span>
> 
> <span data-ttu-id="b5c6b-115">Projekt Visual Web Developer z kodu źródłowego C# jest dostępna powiązany z tym tematem.</span><span class="sxs-lookup"><span data-stu-id="b5c6b-115">A Visual Web Developer project with C# source code is available to accompany this topic.</span></span> <span data-ttu-id="b5c6b-116">[Pobierz wersję języka C#](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098).</span><span class="sxs-lookup"><span data-stu-id="b5c6b-116">[Download the C# version](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098).</span></span> <span data-ttu-id="b5c6b-117">Jeśli wolisz Visual Basic, przełącz się do [wersji Visual Basic](../vb/intro-to-aspnet-mvc-3.md) tego samouczka.</span><span class="sxs-lookup"><span data-stu-id="b5c6b-117">If you prefer Visual Basic, switch to the [Visual Basic version](../vb/intro-to-aspnet-mvc-3.md) of this tutorial.</span></span>


## <a name="what-youll-build"></a><span data-ttu-id="b5c6b-118">Jakie będzie kompilacji</span><span class="sxs-lookup"><span data-stu-id="b5c6b-118">What You'll Build</span></span>

<span data-ttu-id="b5c6b-119">Będzie implementuje prostą aplikację listy filmów, która obsługuje tworzenie, edytowanie i wyświetlanie filmów z bazy danych.</span><span class="sxs-lookup"><span data-stu-id="b5c6b-119">You'll implement a simple movie-listing application that supports creating, editing, and listing movies from a database.</span></span> <span data-ttu-id="b5c6b-120">Poniżej przedstawiono dwa zrzuty ekranu aplikacji, która będzie kompilacji.</span><span class="sxs-lookup"><span data-stu-id="b5c6b-120">Below are two screenshots of the application you'll build.</span></span> <span data-ttu-id="b5c6b-121">Obejmuje on strona, wyświetlająca listę filmów z bazy danych:</span><span class="sxs-lookup"><span data-stu-id="b5c6b-121">It includes a page that displays a list of movies from a database:</span></span>

![MoviesWithVariousSm](intro-to-aspnet-mvc-3/_static/image1.png)

<span data-ttu-id="b5c6b-123">Aplikacja umożliwia także dodawania, edytowania i usuwania, filmów, a także szczegółowe informacje można znaleźć o pojedyncze pliki.</span><span class="sxs-lookup"><span data-stu-id="b5c6b-123">The application also lets you add, edit, and delete movies, as well as see details about individual ones.</span></span> <span data-ttu-id="b5c6b-124">Wszystkie scenariusze wprowadzania danych sprawdzania poprawności, aby upewnić się, że dane przechowywane w bazie danych są prawidłowe.</span><span class="sxs-lookup"><span data-stu-id="b5c6b-124">All data-entry scenarios include validation to ensure that the data stored in the database is correct.</span></span>

![](intro-to-aspnet-mvc-3/_static/image2.png)

## <a name="skills-youll-learn"></a><span data-ttu-id="b5c6b-125">Dowiesz się umiejętności</span><span class="sxs-lookup"><span data-stu-id="b5c6b-125">Skills You'll Learn</span></span>

<span data-ttu-id="b5c6b-126">Oto dowiesz się:</span><span class="sxs-lookup"><span data-stu-id="b5c6b-126">Here's what you'll learn:</span></span>

- <span data-ttu-id="b5c6b-127">Jak utworzyć nowy projekt ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="b5c6b-127">How to create a new ASP.NET MVC project.</span></span>
- <span data-ttu-id="b5c6b-128">Jak utworzyć ASP.NET MVC, widoków i kontrolerów.</span><span class="sxs-lookup"><span data-stu-id="b5c6b-128">How to create ASP.NET MVC controllers and views.</span></span>
- <span data-ttu-id="b5c6b-129">Jak utworzyć nową bazę danych przy użyciu modelu Entity Framework Code First.</span><span class="sxs-lookup"><span data-stu-id="b5c6b-129">How to create a new database using the Entity Framework Code First paradigm.</span></span>
- <span data-ttu-id="b5c6b-130">Sposób pobierania i wyświetlania danych.</span><span class="sxs-lookup"><span data-stu-id="b5c6b-130">How to retrieve and display data.</span></span>
- <span data-ttu-id="b5c6b-131">Jak edytować dane i włączyć sprawdzanie poprawności danych.</span><span class="sxs-lookup"><span data-stu-id="b5c6b-131">How to edit data and enable data validation.</span></span>

## <a name="getting-started"></a><span data-ttu-id="b5c6b-132">Wprowadzenie</span><span class="sxs-lookup"><span data-stu-id="b5c6b-132">Getting Started</span></span>

<span data-ttu-id="b5c6b-133">Zacznij od uruchomienia programu Visual Web Developer 2010 Express ("Visual Web Developer" skrócie) i wybierz **nowy projekt** z **Start** strony.</span><span class="sxs-lookup"><span data-stu-id="b5c6b-133">Start by running Visual Web Developer 2010 Express ("Visual Web Developer" for short) and select **New Project** from the **Start** page.</span></span>

<span data-ttu-id="b5c6b-134">Visual Web Developer jest IDE lub zintegrowane środowisko deweloperskie.</span><span class="sxs-lookup"><span data-stu-id="b5c6b-134">Visual Web Developer is an IDE, or integrated development environment.</span></span> <span data-ttu-id="b5c6b-135">Tak samo jak w programie Microsoft Word do zapisywania dokumentów, użyjesz IDE do tworzenia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="b5c6b-135">Just like you use Microsoft Word to write documents, you'll use an IDE to create applications.</span></span> <span data-ttu-id="b5c6b-136">Visual Web Developer ma narzędzi wzdłuż górnej pokazywanie różnych dostępnych opcji.</span><span class="sxs-lookup"><span data-stu-id="b5c6b-136">In Visual Web Developer there's a toolbar along the top showing various options available to you.</span></span> <span data-ttu-id="b5c6b-137">Istnieje również menu, która umożliwia innym sposobem wykonania zadania w środowisku IDE.</span><span class="sxs-lookup"><span data-stu-id="b5c6b-137">There's also a menu that provides another way to perform tasks in the IDE.</span></span> <span data-ttu-id="b5c6b-138">(Na przykład zamiast zaznaczania **nowy projekt** z **Start** strony, można skorzystać z menu i wybierz **pliku** &gt; **nowy projekt**.)</span><span class="sxs-lookup"><span data-stu-id="b5c6b-138">(For example, instead of selecting **New Project** from the **Start** page, you can use the menu and select **File** &gt; **New Project**.)</span></span>

[![](intro-to-aspnet-mvc-3/_static/image4.png)](intro-to-aspnet-mvc-3/_static/image3.png)

## <a name="creating-your-first-application"></a><span data-ttu-id="b5c6b-139">Tworzenie pierwszej aplikacji</span><span class="sxs-lookup"><span data-stu-id="b5c6b-139">Creating Your First Application</span></span>

<span data-ttu-id="b5c6b-140">Można tworzyć aplikacje przy użyciu języka Visual Basic lub Visual C# jako języka programowania.</span><span class="sxs-lookup"><span data-stu-id="b5c6b-140">You can create applications using either Visual Basic or Visual C# as the programming language.</span></span> <span data-ttu-id="b5c6b-141">Wybierz Visual C# po lewej stronie, a następnie wybierz **aplikacji sieci Web programu ASP.NET MVC 3**.</span><span class="sxs-lookup"><span data-stu-id="b5c6b-141">Select Visual C# on the left and then select **ASP.NET MVC 3 Web Application**.</span></span> <span data-ttu-id="b5c6b-142">Nazwa projektu "MvcMovie", a następnie kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="b5c6b-142">Name your project "MvcMovie" and then click **OK**.</span></span> <span data-ttu-id="b5c6b-143">(Jeśli wolisz Visual Basic, przełącz się do [wersji Visual Basic](../vb/intro-to-aspnet-mvc-3.md) tego samouczka.)</span><span class="sxs-lookup"><span data-stu-id="b5c6b-143">(If you prefer Visual Basic, switch to the [Visual Basic version](../vb/intro-to-aspnet-mvc-3.md) of this tutorial.)</span></span>

![](intro-to-aspnet-mvc-3/_static/image5.png)

<span data-ttu-id="b5c6b-144">W **nowy projekt programu ASP.NET MVC 3** okno dialogowe, wybierz opcję **aplikacji internetowej**.</span><span class="sxs-lookup"><span data-stu-id="b5c6b-144">In the **New ASP.NET MVC 3 Project** dialog box, select **Internet Application**.</span></span> <span data-ttu-id="b5c6b-145">Sprawdź **HTML5 użycia znaczników** i pozostawić **Razor** jako domyślny aparat widoku.</span><span class="sxs-lookup"><span data-stu-id="b5c6b-145">Check **Use HTML5 markup** and leave **Razor** as the default view engine.</span></span>

![](intro-to-aspnet-mvc-3/_static/image6.png)

<span data-ttu-id="b5c6b-146">Kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="b5c6b-146">Click **OK**.</span></span> <span data-ttu-id="b5c6b-147">Visual Web Developer używany szablon domyślny projektu programu ASP.NET MVC, który właśnie utworzony, więc w tej chwili oferuje działającą aplikację bez żadnego działania!</span><span class="sxs-lookup"><span data-stu-id="b5c6b-147">Visual Web Developer used a default template for the ASP.NET MVC project you just created, so you have a working application right now without doing anything!</span></span> <span data-ttu-id="b5c6b-148">Jest to prosty "Witaj świecie!"</span><span class="sxs-lookup"><span data-stu-id="b5c6b-148">This is a simple "Hello World!"</span></span> <span data-ttu-id="b5c6b-149">Projekt, a jego dobrym miejscem do uruchomienia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="b5c6b-149">project, and it's a good place to start your application.</span></span>

[![](intro-to-aspnet-mvc-3/_static/image8.png)](intro-to-aspnet-mvc-3/_static/image7.png)

<span data-ttu-id="b5c6b-150">Z **debugowania** menu, wybierz opcję **Rozpocznij debugowanie**.</span><span class="sxs-lookup"><span data-stu-id="b5c6b-150">From the **Debug** menu, select **Start Debugging**.</span></span>

![](intro-to-aspnet-mvc-3/_static/image9.png)

<span data-ttu-id="b5c6b-151">Zwróć uwagę, że skrót klawiaturowy można rozpocząć debugowania jest F5.</span><span class="sxs-lookup"><span data-stu-id="b5c6b-151">Notice that the keyboard shortcut to start debugging is F5.</span></span>

<span data-ttu-id="b5c6b-152">F5 powoduje, że Visual Web Developer uruchomić serwera wdrożeniowego sieci web i uruchom aplikację sieci web.</span><span class="sxs-lookup"><span data-stu-id="b5c6b-152">F5 causes Visual Web Developer to start a development web server and run your web application.</span></span> <span data-ttu-id="b5c6b-153">Visual Web Developer następnie uruchamia przeglądarkę i spowoduje otwarcie strony głównej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="b5c6b-153">Visual Web Developer then launches a browser and opens the application's home page.</span></span> <span data-ttu-id="b5c6b-154">Należy zauważyć, że na pasku adresu przeglądarki mówi `localhost` i nie coś, takich jak `example.com`.</span><span class="sxs-lookup"><span data-stu-id="b5c6b-154">Notice that the address bar of the browser says `localhost` and not something like `example.com`.</span></span> <span data-ttu-id="b5c6b-155">Jest to spowodowane tym `localhost` zawsze wskazuje na własnym komputerze lokalnym, działającego w takim przypadku aplikacji zostały utworzone.</span><span class="sxs-lookup"><span data-stu-id="b5c6b-155">That's because `localhost` always points to your own local computer, which in this case is running the application you just built.</span></span> <span data-ttu-id="b5c6b-156">Po uruchomieniu projektu sieci web programu Visual Web Developer losowy port jest używany przez serwer sieci web.</span><span class="sxs-lookup"><span data-stu-id="b5c6b-156">When Visual Web Developer runs a web project, a random port is used for the web server.</span></span> <span data-ttu-id="b5c6b-157">Na poniższej ilustracji numer portu losowe jest 43246.</span><span class="sxs-lookup"><span data-stu-id="b5c6b-157">In the image below, the random port number is 43246.</span></span> <span data-ttu-id="b5c6b-158">Po uruchomieniu aplikacji widoczny będzie prawdopodobnie inny numer portu.</span><span class="sxs-lookup"><span data-stu-id="b5c6b-158">When you run the application, you'll probably see a different port number.</span></span>

![](intro-to-aspnet-mvc-3/_static/image10.png)

<span data-ttu-id="b5c6b-159">Dodatkowych zabiegów ten szablon domyślny zapewnia dwie strony, aby odwiedzić i strony logowania podstawowe.</span><span class="sxs-lookup"><span data-stu-id="b5c6b-159">Right out of the box this default template gives you two pages to visit and a basic login page.</span></span> <span data-ttu-id="b5c6b-160">Następnym krokiem jest zmienić sposób działania tej aplikacji i Poznaj nieco ASP.NET MVC w procesie.</span><span class="sxs-lookup"><span data-stu-id="b5c6b-160">The next step is to change how this application works and learn a little bit about ASP.NET MVC in the process.</span></span> <span data-ttu-id="b5c6b-161">Zamknij przeglądarkę i Zmieńmy kodu.</span><span class="sxs-lookup"><span data-stu-id="b5c6b-161">Close your browser and let's change some code.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="b5c6b-162">Next</span><span class="sxs-lookup"><span data-stu-id="b5c6b-162">Next</span></span>](adding-a-controller.md)
