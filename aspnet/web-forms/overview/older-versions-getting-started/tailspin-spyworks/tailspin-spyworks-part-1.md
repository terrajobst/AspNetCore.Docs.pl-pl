---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-1
title: 'Część 1: Plik -> Nowy projekt | Dokumentacja firmy Microsoft'
author: JoeStagner
description: Ta seria samouczek zawiera szczegóły dotyczące wszystkich kroków kompilacji Tailspin Spyworks przykładowej aplikacji. Część 1 zawiera przegląd/nowy plik projektu.
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/21/2010
ms.topic: article
ms.assetid: 15d4652b-d5aa-4172-b186-2c7f96ba316d
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-1
msc.type: authoredcontent
ms.openlocfilehash: a1b9681516e626b6a0eec420b168a74e05d88afb
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
ms.locfileid: "30892468"
---
<a name="part-1-file--new-project"></a><span data-ttu-id="91a96-104">Część 1: Plik -> Nowy projekt</span><span class="sxs-lookup"><span data-stu-id="91a96-104">Part 1: File-> New Project</span></span>
====================
<span data-ttu-id="91a96-105">przez [Stagner Jan](https://github.com/JoeStagner)</span><span class="sxs-lookup"><span data-stu-id="91a96-105">by [Joe Stagner](https://github.com/JoeStagner)</span></span>

> <span data-ttu-id="91a96-106">Tailspin Spyworks pokazano, jak bardzo proste jest tworzenie zaawansowanych, skalowalnych aplikacji dla platformy .NET.</span><span class="sxs-lookup"><span data-stu-id="91a96-106">Tailspin Spyworks demonstrates how extraordinarily simple it is to create powerful, scalable applications for the .NET platform.</span></span> <span data-ttu-id="91a96-107">Przedstawia on poza jak nowe, fantastyczne funkcje programu ASP.NET 4 do tworzenia sklepu online, łącznie z zakupów, wyewidencjonowania i administracji.</span><span class="sxs-lookup"><span data-stu-id="91a96-107">It shows off how to use the great new features in ASP.NET 4 to build an online store, including shopping, checkout, and administration.</span></span>
> 
> <span data-ttu-id="91a96-108">Ta seria samouczek zawiera szczegóły dotyczące wszystkich kroków kompilacji Tailspin Spyworks przykładowej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="91a96-108">This tutorial series details all of the steps taken to build the Tailspin Spyworks sample application.</span></span> <span data-ttu-id="91a96-109">Część 1 zawiera przegląd/nowy plik projektu.</span><span class="sxs-lookup"><span data-stu-id="91a96-109">Part 1 covers Overview and File/New Project.</span></span>


## <a id="_Toc260221666"></a>  <span data-ttu-id="91a96-110">Omówienie</span><span class="sxs-lookup"><span data-stu-id="91a96-110">Overview</span></span>

<span data-ttu-id="91a96-111">W tym samouczku jest wprowadzenie do formularzy sieci Web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="91a96-111">This tutorial is an introduction to ASP.NET WebForms.</span></span> <span data-ttu-id="91a96-112">Firma Microsoft będzie powoli uruchamianie, środowisko rozwoju poziomu sieci web dla początkujących użytkowników jest poprawny.</span><span class="sxs-lookup"><span data-stu-id="91a96-112">We'll be starting slowly, so beginner level web development experience is okay.</span></span>

<span data-ttu-id="91a96-113">Aplikacji, które firma Microsoft będzie zbudować jest proste sklepu online.</span><span class="sxs-lookup"><span data-stu-id="91a96-113">The application we'll be building is a simple on-line store.</span></span>

![](tailspin-spyworks-part-1/_static/image1.jpg)


<span data-ttu-id="91a96-114">Odwiedzający mogą przeglądać produkty według kategorii:</span><span class="sxs-lookup"><span data-stu-id="91a96-114">Visitors can browse Products by Category:</span></span>

![](tailspin-spyworks-part-1/_static/image2.jpg)

<span data-ttu-id="91a96-115">Mogą wyświetlać jeden produkt i dodać go do ich koszyka:</span><span class="sxs-lookup"><span data-stu-id="91a96-115">They can view a single product and add it to their cart:</span></span>

![](tailspin-spyworks-part-1/_static/image3.jpg)

<span data-ttu-id="91a96-116">Mogą one przejrzeć ich koszyka, usuwając wszystkie elementy, które nie są już potrzebne:</span><span class="sxs-lookup"><span data-stu-id="91a96-116">They can review their cart, removing any items they no longer want:</span></span>

![](tailspin-spyworks-part-1/_static/image4.jpg)

<span data-ttu-id="91a96-117">Przejściem do wyewidencjonowania wyświetli monit o ich do</span><span class="sxs-lookup"><span data-stu-id="91a96-117">Proceeding to Checkout will prompt them to</span></span>

![](tailspin-spyworks-part-1/_static/image5.jpg)

![](tailspin-spyworks-part-1/_static/image6.jpg)

<span data-ttu-id="91a96-118">Po określeniu kolejności, zobaczy ekran potwierdzenia prosty:</span><span class="sxs-lookup"><span data-stu-id="91a96-118">After ordering, they see a simple confirmation screen:</span></span>

![](tailspin-spyworks-part-1/_static/image7.jpg)


<span data-ttu-id="91a96-119">Rozpocznie się przez utworzenie nowego projektu formularzy sieci Web ASP.NET w programie Visual Studio 2010, a następnie stopniowo dodamy funkcje tworzenia kompletna aplikacja działa.</span><span class="sxs-lookup"><span data-stu-id="91a96-119">We'll begin by creating a new ASP.NET WebForms project in Visual Studio 2010, and we'll incrementally add features to create a complete functioning application.</span></span> <span data-ttu-id="91a96-120">Przeglądania omówione zostaną następujące czynności dostępu do bazy danych, widoki listy i siatki, strony aktualizacji danych, sprawdzanie poprawności danych, za pomocą stron wzorcowych układ strony spójne, AJAX, weryfikacji i użytkownika członkostwa.</span><span class="sxs-lookup"><span data-stu-id="91a96-120">Along the way, we'll cover database access, list and grid views, data update pages, data validation, using master pages for consistent page layout, AJAX, validation, user membership, and more.</span></span>

<span data-ttu-id="91a96-121">Można wykonać krok po kroku, można również pobrać ukończona aplikacja z [http://tailspinspyworks.codeplex.com/](http://tailspinspyworks.codeplex.com/)</span><span class="sxs-lookup"><span data-stu-id="91a96-121">You can follow along step by step, or you can download the completed application from [http://tailspinspyworks.codeplex.com/](http://tailspinspyworks.codeplex.com/)</span></span>

<span data-ttu-id="91a96-122">Możesz użyć programu Visual Studio 2010 lub wolnego Visual Web Developer 2010 z [ https://www.microsoft.com/express/Web/ ](https://www.microsoft.com/express/Web/).</span><span class="sxs-lookup"><span data-stu-id="91a96-122">You can use either Visual Studio 2010 or the free Visual Web Developer 2010 from [https://www.microsoft.com/express/Web/](https://www.microsoft.com/express/Web/).</span></span> <span data-ttu-id="91a96-123">Do skompilowania aplikacji, można użyć programu SQL Server lub wolnego programu SQL Server Express do hosta bazy danych.</span><span class="sxs-lookup"><span data-stu-id="91a96-123">To build the application, you can use either SQL Server or the free SQL Server Express to host the database.</span></span>

## <a id="_Toc260221667"></a>  <span data-ttu-id="91a96-124">Plik / nowy projekt</span><span class="sxs-lookup"><span data-stu-id="91a96-124">File / New Project</span></span>

<span data-ttu-id="91a96-125">Zaczniemy, wybierając nowy projekt z menu Plik w programie Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="91a96-125">We'll start by selecting the New Project from the File menu in Visual Studio.</span></span> <span data-ttu-id="91a96-126">Spowoduje to wyświetlenie okna dialogowego Nowy projekt.</span><span class="sxs-lookup"><span data-stu-id="91a96-126">This brings up the New Project dialog.</span></span>

![](tailspin-spyworks-part-1/_static/image8.jpg)

<span data-ttu-id="91a96-127">Wybierzemy Visual C# / szablonów sieci Web grupy po lewej stronie, a następnie wybierz szablon "Aplikacja sieci Web ASP.NET" w środkowej kolumnie.</span><span class="sxs-lookup"><span data-stu-id="91a96-127">We'll select the Visual C# / Web Templates group on the left, and then choose the "ASP.NET Web Application" template in the center column.</span></span> <span data-ttu-id="91a96-128">Nazwa projektu TailspinSpyworks i naciśnij przycisk OK.</span><span class="sxs-lookup"><span data-stu-id="91a96-128">Name your project TailspinSpyworks and press the OK button.</span></span>

![](tailspin-spyworks-part-1/_static/image9.jpg)

<span data-ttu-id="91a96-129">Spowoduje to utworzenie naszych projektu.</span><span class="sxs-lookup"><span data-stu-id="91a96-129">This will create our project.</span></span> <span data-ttu-id="91a96-130">Spójrzmy na foldery, które są uwzględnione w naszej aplikacji w Eksploratorze rozwiązań po prawej stronie.</span><span class="sxs-lookup"><span data-stu-id="91a96-130">Let's take a look at the folders that are included in our application in the Solution Explorer on the right side.</span></span>

![](tailspin-spyworks-part-1/_static/image10.jpg)

<span data-ttu-id="91a96-131">Puste rozwiązanie nie jest pusty — dodaje strukturę folderów podstawowe:</span><span class="sxs-lookup"><span data-stu-id="91a96-131">The Empty Solution isn't completely empty – it adds a basic folder structure:</span></span>

![](tailspin-spyworks-part-1/_static/image1.png)

<span data-ttu-id="91a96-132">Należy pamiętać, konwencje implementowane przez domyślny szablon projektu programu ASP.NET 4.</span><span class="sxs-lookup"><span data-stu-id="91a96-132">Note the conventions implemented by the ASP.NET 4 default project template.</span></span>

- <span data-ttu-id="91a96-133">Folder "Konto" implementuje podstawowy interfejs użytkownika dla środowiska ASP. Podsystem członkostwa w sieci.</span><span class="sxs-lookup"><span data-stu-id="91a96-133">The "Account" folder implements a basic user interface for ASP.NET's membership subsystem.</span></span>
- <span data-ttu-id="91a96-134">Folder "Skryptów" służy jako repozytorium plików JavaScript po stronie klienta i podstawowe jQuery js pliki są udostępniane domyślnie.</span><span class="sxs-lookup"><span data-stu-id="91a96-134">The "Scripts" folder serves as the repository for client side JavaScript files and the core jQuery .js files are made available by default.</span></span>
- <span data-ttu-id="91a96-135">Folder "Style" jest używany do organizowania elementów wizualnych naszych witryny sieci web (arkusze stylów CSS)</span><span class="sxs-lookup"><span data-stu-id="91a96-135">The "Styles" folder is used to organize our web site visuals (CSS Style Sheets)</span></span>

<span data-ttu-id="91a96-136">Gdy firma Microsoft naciśnij klawisz F5, aby uruchomić aplikację i renderowania strony default.aspx przedstawiono poniżej.</span><span class="sxs-lookup"><span data-stu-id="91a96-136">When we press F5 to run our application and render the default.aspx page we see the following.</span></span>

![](tailspin-spyworks-part-1/_static/image11.jpg)

<span data-ttu-id="91a96-137">Naszym pierwszym rozszerzenie aplikacji należy zastąpić plik Style.css z szablonu domyślnego WebForms klas CSS i powiązane pliki obrazów renderowanych visual asthetics, interesujące dla aplikacji Tailspin Spyworks.</span><span class="sxs-lookup"><span data-stu-id="91a96-137">Our first application enhancement will be to replace the Style.css file from the default WebForms template with the CSS classes and associated image files that will render the visual asthetics that we want for our Tailspin Spyworks application.</span></span>

<span data-ttu-id="91a96-138">Po wykonaniu tej czynności naszą stronę default.aspx renderuje się następująco.</span><span class="sxs-lookup"><span data-stu-id="91a96-138">After doing so our default.aspx page renders like this.</span></span>

![](tailspin-spyworks-part-1/_static/image12.jpg)

<span data-ttu-id="91a96-139">Zwróć uwagę, linki obraz u góry rogu strony i elementy menu, które zostały dodane do strony wzorcowej.</span><span class="sxs-lookup"><span data-stu-id="91a96-139">Notice the image links at the top right of the page and the menu items that have been added to the master page.</span></span> <span data-ttu-id="91a96-140">Tylko łącza "Logowania" i "Konto" wskaż stron, które istnieją (generowane przez szablon domyślny) i pozostałej części strony, które wprowadzimy jak mamy utworzyć w naszej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="91a96-140">Only the "Sign In" and "Account" links point to pages that exist (generated by the default template) and the rest of the pages we will implement as we build our application.</span></span>

<span data-ttu-id="91a96-141">Chcemy też przenosić do katalogu stylów strony wzorcowej.</span><span class="sxs-lookup"><span data-stu-id="91a96-141">We're also going to relocate the Master Page to the Styles directory.</span></span> <span data-ttu-id="91a96-142">Chociaż jest to tylko preferencję go może ułatwić nieco Jeśli firma zdecyduje się upewnij "skinable" naszej aplikacji w przyszłości.</span><span class="sxs-lookup"><span data-stu-id="91a96-142">Though this is only a preference it may make things a little easier if we decide to make our application "skinable" in the future.</span></span>

<span data-ttu-id="91a96-143">Po wykonaniu, to trzeba będzie zmienić strony wzorcowej odwołań w wszystkie pliki aspx generowane przez domyślne strony formularzy sieci Web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="91a96-143">After doing this we'll need to change the master page references in all the .aspx files generated by the default ASP.NET WebForms pages.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="91a96-144">Next</span><span class="sxs-lookup"><span data-stu-id="91a96-144">Next</span></span>](tailspin-spyworks-part-2.md)
