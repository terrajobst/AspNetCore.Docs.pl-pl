---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3
title: Wprowadzenie do platformy ASP.NET MVC 3 (C#) | Dokumentacja firmy Microsoft
author: Rick-Anderson
description: Ta seria samouczków obejmuje podstawy tworzenia aplikacji sieci Web platformy ASP.NET MVC przy użyciu programu Microsoft Visual Web Developer 2010 Express Service Pack 1, czyli...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/12/2011
ms.topic: article
ms.assetid: 86a80b35-88bd-4b7c-bd58-f6e7997197d4
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3
msc.type: authoredcontent
ms.openlocfilehash: d4cda748638a6396750fe2ef7713dd0b2a4555d6
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37379866"
---
<a name="intro-to-aspnet-mvc-3-c"></a><span data-ttu-id="faf66-103">Wprowadzenie do platformy ASP.NET MVC 3 (C#)</span><span class="sxs-lookup"><span data-stu-id="faf66-103">Intro to ASP.NET MVC 3 (C#)</span></span>
====================
<span data-ttu-id="faf66-104">Przez [Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="faf66-104">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

> > [!NOTE]
> > <span data-ttu-id="faf66-105">Jest dostępna zaktualizowana wersja tego samouczka [tutaj](../../../getting-started/introduction/getting-started.md) używającej platformy ASP.NET MVC 5 i Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="faf66-105">An updated version of this tutorial is available [here](../../../getting-started/introduction/getting-started.md) that uses ASP.NET MVC 5 and Visual Studio 2013.</span></span> <span data-ttu-id="faf66-106">Jest bardziej bezpieczne, łatwiej wykonać i pokazuje więcej funkcji.</span><span class="sxs-lookup"><span data-stu-id="faf66-106">It's more secure, much simpler to follow and demonstrates more features.</span></span>
> 
> 
> <span data-ttu-id="faf66-107">Ta seria samouczków obejmuje podstawy tworzenia aplikacji sieci Web platformy ASP.NET MVC przy użyciu Microsoft Visual Web Developer 2010 Express Service Pack 1, która jest bezpłatna wersja programu Microsoft Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="faf66-107">This tutorial will teach you the basics of building an ASP.NET MVC Web application using Microsoft Visual Web Developer 2010 Express Service Pack 1, which is a free version of Microsoft Visual Studio.</span></span> <span data-ttu-id="faf66-108">Przed rozpoczęciem upewnij się, że po zainstalowaniu wymagań wstępnych wymienionych poniżej.</span><span class="sxs-lookup"><span data-stu-id="faf66-108">Before you start, make sure you've installed the prerequisites listed below.</span></span> <span data-ttu-id="faf66-109">Możesz zainstalować wszystkie z nich, klikając poniższe łącze: [Instalatora platformy sieci Web](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack).</span><span class="sxs-lookup"><span data-stu-id="faf66-109">You can install all of them by clicking the following link: [Web Platform Installer](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack).</span></span> <span data-ttu-id="faf66-110">Alternatywnie można indywidualnie zainstalować wymagania wstępne, korzystając z następujących linków:</span><span class="sxs-lookup"><span data-stu-id="faf66-110">Alternatively, you can individually install the prerequisites using the following links:</span></span>
> 
> - [<span data-ttu-id="faf66-111">Visual Studio Web Developer Express z dodatkiem SP1 wymagań wstępnych.</span><span class="sxs-lookup"><span data-stu-id="faf66-111">Visual Studio Web Developer Express SP1 prerequisites</span></span>](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [<span data-ttu-id="faf66-112">Program ASP.NET MVC 3 Tools Update</span><span class="sxs-lookup"><span data-stu-id="faf66-112">ASP.NET MVC 3 Tools Update</span></span>](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - <span data-ttu-id="faf66-113">[SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(Obsługa środowiska uruchomieniowego i narzędzi)</span><span class="sxs-lookup"><span data-stu-id="faf66-113">[SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(runtime + tools support)</span></span>
> 
> <span data-ttu-id="faf66-114">Jeśli używasz programu Visual Studio 2010 zamiast Visual Web Developer 2010, zainstaluj wymagania wstępne, klikając poniższe łącze: [wymagania wstępne programu Visual Studio 2010](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).</span><span class="sxs-lookup"><span data-stu-id="faf66-114">If you're using Visual Studio 2010 instead of Visual Web Developer 2010, install the prerequisites by clicking the following link: [Visual Studio 2010 prerequisites](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).</span></span>
> 
> <span data-ttu-id="faf66-115">Projekt Visual Web Developer, przy użyciu kodu źródłowego języka C# jest dostępny powiązany z tym tematem.</span><span class="sxs-lookup"><span data-stu-id="faf66-115">A Visual Web Developer project with C# source code is available to accompany this topic.</span></span> <span data-ttu-id="faf66-116">[Pobierz wersję języka C#](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098).</span><span class="sxs-lookup"><span data-stu-id="faf66-116">[Download the C# version](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098).</span></span> <span data-ttu-id="faf66-117">Jeśli wolisz języka Visual Basic, przełącz się do [wersji języka Visual Basic](../vb/intro-to-aspnet-mvc-3.md) po ukończeniu tego samouczka.</span><span class="sxs-lookup"><span data-stu-id="faf66-117">If you prefer Visual Basic, switch to the [Visual Basic version](../vb/intro-to-aspnet-mvc-3.md) of this tutorial.</span></span>


## <a name="what-youll-build"></a><span data-ttu-id="faf66-118">Jakie będziesz tworzyć</span><span class="sxs-lookup"><span data-stu-id="faf66-118">What You'll Build</span></span>

<span data-ttu-id="faf66-119">Będzie implementowana prostą aplikację listy filmów, która obsługuje tworzenia, edytowania i wyświetlania listy filmów z bazy danych.</span><span class="sxs-lookup"><span data-stu-id="faf66-119">You'll implement a simple movie-listing application that supports creating, editing, and listing movies from a database.</span></span> <span data-ttu-id="faf66-120">Poniżej przedstawiono dwa zrzuty ekranu aplikacji, którą utworzysz.</span><span class="sxs-lookup"><span data-stu-id="faf66-120">Below are two screenshots of the application you'll build.</span></span> <span data-ttu-id="faf66-121">Zawiera ona strona, która wyświetla listę filmów z bazy danych:</span><span class="sxs-lookup"><span data-stu-id="faf66-121">It includes a page that displays a list of movies from a database:</span></span>

![MoviesWithVariousSm](intro-to-aspnet-mvc-3/_static/image1.png)

<span data-ttu-id="faf66-123">Aplikacja umożliwia także dodawanie, edytowanie i usuwanie filmów, a także znaleźć w szczegółach dotyczących poszczególnych z nich.</span><span class="sxs-lookup"><span data-stu-id="faf66-123">The application also lets you add, edit, and delete movies, as well as see details about individual ones.</span></span> <span data-ttu-id="faf66-124">Wszystkie scenariusze wprowadzania danych obejmują sprawdzanie poprawności, aby upewnić się, że dane przechowywane w bazie danych są prawidłowe.</span><span class="sxs-lookup"><span data-stu-id="faf66-124">All data-entry scenarios include validation to ensure that the data stored in the database is correct.</span></span>

![](intro-to-aspnet-mvc-3/_static/image2.png)

## <a name="skills-youll-learn"></a><span data-ttu-id="faf66-125">Umiejętności, których dowiesz się</span><span class="sxs-lookup"><span data-stu-id="faf66-125">Skills You'll Learn</span></span>

<span data-ttu-id="faf66-126">Oto, dowiesz się:</span><span class="sxs-lookup"><span data-stu-id="faf66-126">Here's what you'll learn:</span></span>

- <span data-ttu-id="faf66-127">Jak utworzyć nowy projekt ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="faf66-127">How to create a new ASP.NET MVC project.</span></span>
- <span data-ttu-id="faf66-128">Jak utworzyć platformy ASP.NET MVC, widoków i kontrolerów.</span><span class="sxs-lookup"><span data-stu-id="faf66-128">How to create ASP.NET MVC controllers and views.</span></span>
- <span data-ttu-id="faf66-129">Jak utworzyć nową bazę danych przy użyciu modelu Entity Framework Code First.</span><span class="sxs-lookup"><span data-stu-id="faf66-129">How to create a new database using the Entity Framework Code First paradigm.</span></span>
- <span data-ttu-id="faf66-130">Jak pobrać i wyświetlić dane.</span><span class="sxs-lookup"><span data-stu-id="faf66-130">How to retrieve and display data.</span></span>
- <span data-ttu-id="faf66-131">Jak edytować dane i włączyć sprawdzanie poprawności danych.</span><span class="sxs-lookup"><span data-stu-id="faf66-131">How to edit data and enable data validation.</span></span>

## <a name="getting-started"></a><span data-ttu-id="faf66-132">Wprowadzenie</span><span class="sxs-lookup"><span data-stu-id="faf66-132">Getting Started</span></span>

<span data-ttu-id="faf66-133">Uruchom za pomocą programu Visual Web Developer 2010 Express ("Visual Web Developer" skrócie) i wybierz **nowy projekt** z **Start** strony.</span><span class="sxs-lookup"><span data-stu-id="faf66-133">Start by running Visual Web Developer 2010 Express ("Visual Web Developer" for short) and select **New Project** from the **Start** page.</span></span>

<span data-ttu-id="faf66-134">Visual Web Developer jest środowiskiem IDE lub zintegrowanego środowiska programistycznego.</span><span class="sxs-lookup"><span data-stu-id="faf66-134">Visual Web Developer is an IDE, or integrated development environment.</span></span> <span data-ttu-id="faf66-135">Tak samo jak w programie Microsoft Word do zapisywania dokumentów, użyjesz środowisko IDE do tworzenia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="faf66-135">Just like you use Microsoft Word to write documents, you'll use an IDE to create applications.</span></span> <span data-ttu-id="faf66-136">W Visual Web Developer jest narzędzi wzdłuż górnej przedstawiający różne opcje dostępne dla Ciebie.</span><span class="sxs-lookup"><span data-stu-id="faf66-136">In Visual Web Developer there's a toolbar along the top showing various options available to you.</span></span> <span data-ttu-id="faf66-137">Istnieje również menu, który udostępnia inny sposób wykonywania zadań w środowisku IDE.</span><span class="sxs-lookup"><span data-stu-id="faf66-137">There's also a menu that provides another way to perform tasks in the IDE.</span></span> <span data-ttu-id="faf66-138">(Na przykład, zamiast zaznaczania **nowy projekt** z **Start** strony, można użyć menu i wybrać **pliku** &gt; **nowy projekt**.)</span><span class="sxs-lookup"><span data-stu-id="faf66-138">(For example, instead of selecting **New Project** from the **Start** page, you can use the menu and select **File** &gt; **New Project**.)</span></span>

[![](intro-to-aspnet-mvc-3/_static/image4.png)](intro-to-aspnet-mvc-3/_static/image3.png)

## <a name="creating-your-first-application"></a><span data-ttu-id="faf66-139">Tworzenie pierwszej aplikacji</span><span class="sxs-lookup"><span data-stu-id="faf66-139">Creating Your First Application</span></span>

<span data-ttu-id="faf66-140">Można tworzyć aplikacje przy użyciu języka Visual Basic lub Visual C# jako języka programowania.</span><span class="sxs-lookup"><span data-stu-id="faf66-140">You can create applications using either Visual Basic or Visual C# as the programming language.</span></span> <span data-ttu-id="faf66-141">Wybierz pozycję Visual C# po lewej stronie, a następnie wybierz pozycję **aplikacji sieci Web programu ASP.NET MVC 3**.</span><span class="sxs-lookup"><span data-stu-id="faf66-141">Select Visual C# on the left and then select **ASP.NET MVC 3 Web Application**.</span></span> <span data-ttu-id="faf66-142">Nazwij swój projekt "MvcMovie", a następnie kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="faf66-142">Name your project "MvcMovie" and then click **OK**.</span></span> <span data-ttu-id="faf66-143">(Jeśli wolisz języka Visual Basic, przełącz się do [wersji języka Visual Basic](../vb/intro-to-aspnet-mvc-3.md) po ukończeniu tego samouczka.)</span><span class="sxs-lookup"><span data-stu-id="faf66-143">(If you prefer Visual Basic, switch to the [Visual Basic version](../vb/intro-to-aspnet-mvc-3.md) of this tutorial.)</span></span>

![](intro-to-aspnet-mvc-3/_static/image5.png)

<span data-ttu-id="faf66-144">W **nowego projektu programu ASP.NET MVC 3** okno dialogowe, wybierz opcję **aplikacji internetowej**.</span><span class="sxs-lookup"><span data-stu-id="faf66-144">In the **New ASP.NET MVC 3 Project** dialog box, select **Internet Application**.</span></span> <span data-ttu-id="faf66-145">Sprawdź **HTML5 użycia znaczników** i pozostawić **Razor** jako domyślny aparat widoku.</span><span class="sxs-lookup"><span data-stu-id="faf66-145">Check **Use HTML5 markup** and leave **Razor** as the default view engine.</span></span>

![](intro-to-aspnet-mvc-3/_static/image6.png)

<span data-ttu-id="faf66-146">Kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="faf66-146">Click **OK**.</span></span> <span data-ttu-id="faf66-147">Visual Web Developer używać szablonu domyślnego dla projektu platformy ASP.NET MVC, który został utworzony, więc teraz utworzono działającą aplikację bez żadnego działania!</span><span class="sxs-lookup"><span data-stu-id="faf66-147">Visual Web Developer used a default template for the ASP.NET MVC project you just created, so you have a working application right now without doing anything!</span></span> <span data-ttu-id="faf66-148">Jest to proste "Hello World!"</span><span class="sxs-lookup"><span data-stu-id="faf66-148">This is a simple "Hello World!"</span></span> <span data-ttu-id="faf66-149">Projekt, a jego dobrym miejscem, aby uruchomić aplikację.</span><span class="sxs-lookup"><span data-stu-id="faf66-149">project, and it's a good place to start your application.</span></span>

[![](intro-to-aspnet-mvc-3/_static/image8.png)](intro-to-aspnet-mvc-3/_static/image7.png)

<span data-ttu-id="faf66-150">Z **debugowania** menu, wybierz opcję **Rozpocznij debugowanie**.</span><span class="sxs-lookup"><span data-stu-id="faf66-150">From the **Debug** menu, select **Start Debugging**.</span></span>

![](intro-to-aspnet-mvc-3/_static/image9.png)

<span data-ttu-id="faf66-151">Zauważ, że skrót klawiaturowy, aby rozpocząć debugowanie F5.</span><span class="sxs-lookup"><span data-stu-id="faf66-151">Notice that the keyboard shortcut to start debugging is F5.</span></span>

<span data-ttu-id="faf66-152">F5 powoduje, że Visual Web Developer uruchomić serwera wdrożeniowego sieci web i uruchomić aplikację sieci web.</span><span class="sxs-lookup"><span data-stu-id="faf66-152">F5 causes Visual Web Developer to start a development web server and run your web application.</span></span> <span data-ttu-id="faf66-153">Visual Web Developer następnie otworzy w przeglądarce i otwiera strony głównej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="faf66-153">Visual Web Developer then launches a browser and opens the application's home page.</span></span> <span data-ttu-id="faf66-154">Należy zauważyć, że na pasku adresu przeglądarki mówi `localhost` i nie mielibyśmy mieć czegoś podobnego `example.com`.</span><span class="sxs-lookup"><span data-stu-id="faf66-154">Notice that the address bar of the browser says `localhost` and not something like `example.com`.</span></span> <span data-ttu-id="faf66-155">To dlatego, że `localhost` zawsze wskazuje własnego komputera lokalnego, co w tym przypadku jest uruchomiona aplikacja właśnie zbudowany.</span><span class="sxs-lookup"><span data-stu-id="faf66-155">That's because `localhost` always points to your own local computer, which in this case is running the application you just built.</span></span> <span data-ttu-id="faf66-156">Po uruchomieniu projektu sieci web dla programu Visual Web Developer losowy port jest używany dla serwera sieci web.</span><span class="sxs-lookup"><span data-stu-id="faf66-156">When Visual Web Developer runs a web project, a random port is used for the web server.</span></span> <span data-ttu-id="faf66-157">Na poniższej ilustracji liczba losowy port jest 43246.</span><span class="sxs-lookup"><span data-stu-id="faf66-157">In the image below, the random port number is 43246.</span></span> <span data-ttu-id="faf66-158">Po uruchomieniu aplikacji, najprawdopodobniej znajdziesz inny numer portu.</span><span class="sxs-lookup"><span data-stu-id="faf66-158">When you run the application, you'll probably see a different port number.</span></span>

![](intro-to-aspnet-mvc-3/_static/image10.png)

<span data-ttu-id="faf66-159">Gotową do tego szablonu domyślnego zapewnia dwie strony, aby odwiedzić i strony logowania podstawowe.</span><span class="sxs-lookup"><span data-stu-id="faf66-159">Right out of the box this default template gives you two pages to visit and a basic login page.</span></span> <span data-ttu-id="faf66-160">Następnym krokiem jest zmienić sposób działania tej aplikacji i Poznaj nieco platformy ASP.NET MVC w procesie.</span><span class="sxs-lookup"><span data-stu-id="faf66-160">The next step is to change how this application works and learn a little bit about ASP.NET MVC in the process.</span></span> <span data-ttu-id="faf66-161">Zamknij przeglądarkę i zmienimy kodu.</span><span class="sxs-lookup"><span data-stu-id="faf66-161">Close your browser and let's change some code.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="faf66-162">Next</span><span class="sxs-lookup"><span data-stu-id="faf66-162">Next</span></span>](adding-a-controller.md)
