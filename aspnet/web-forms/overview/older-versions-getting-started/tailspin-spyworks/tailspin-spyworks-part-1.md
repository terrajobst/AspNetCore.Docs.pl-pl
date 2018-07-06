---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-1
title: 'Część 1: Plik -> Nowy projekt | Dokumentacja firmy Microsoft'
author: JoeStagner
description: W tej serii samouczków zawiera szczegóły wszystkich kroków kompilacji Przykładowa aplikacja Tailspin Spyworks. Część 1 obejmuje omówienie i plik/nowy projekt.
ms.author: aspnetcontent
ms.date: 07/21/2010
ms.assetid: 15d4652b-d5aa-4172-b186-2c7f96ba316d
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-1
msc.type: authoredcontent
ms.openlocfilehash: 1c8ba561745c9c02b5513e326b4739ec97338393
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/05/2018
ms.locfileid: "37825690"
---
<a name="part-1-file--new-project"></a><span data-ttu-id="7d5ac-104">Część 1: Plik -> Nowy projekt</span><span class="sxs-lookup"><span data-stu-id="7d5ac-104">Part 1: File-> New Project</span></span>
====================
<span data-ttu-id="7d5ac-105">przez [Stagner Jan](https://github.com/JoeStagner)</span><span class="sxs-lookup"><span data-stu-id="7d5ac-105">by [Joe Stagner](https://github.com/JoeStagner)</span></span>

> <span data-ttu-id="7d5ac-106">Tailspin Spyworks pokazuje, jak bardzo łatwo jest tworzyć zaawansowane, skalowalne aplikacje dla platformy .NET.</span><span class="sxs-lookup"><span data-stu-id="7d5ac-106">Tailspin Spyworks demonstrates how extraordinarily simple it is to create powerful, scalable applications for the .NET platform.</span></span> <span data-ttu-id="7d5ac-107">Przedstawia on poza sposób użycia wspaniałych nowych funkcjach w ASP.NET 4 do tworzenia sklep online, m.in. zakupy wyewidencjonowanie i Administracja.</span><span class="sxs-lookup"><span data-stu-id="7d5ac-107">It shows off how to use the great new features in ASP.NET 4 to build an online store, including shopping, checkout, and administration.</span></span>
> 
> <span data-ttu-id="7d5ac-108">W tej serii samouczków zawiera szczegóły wszystkich kroków kompilacji Przykładowa aplikacja Tailspin Spyworks.</span><span class="sxs-lookup"><span data-stu-id="7d5ac-108">This tutorial series details all of the steps taken to build the Tailspin Spyworks sample application.</span></span> <span data-ttu-id="7d5ac-109">Część 1 obejmuje omówienie i plik/nowy projekt.</span><span class="sxs-lookup"><span data-stu-id="7d5ac-109">Part 1 covers Overview and File/New Project.</span></span>


## <a id="_Toc260221666"></a>  <span data-ttu-id="7d5ac-110">Przegląd</span><span class="sxs-lookup"><span data-stu-id="7d5ac-110">Overview</span></span>

<span data-ttu-id="7d5ac-111">Ten samouczek stanowi wprowadzenie do formularzy sieci Web platformy ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="7d5ac-111">This tutorial is an introduction to ASP.NET WebForms.</span></span> <span data-ttu-id="7d5ac-112">Firma Microsoft będzie uruchamiana powoli, a więc środowisko programowania dla początkujących poziomu sieci web jest poprawny.</span><span class="sxs-lookup"><span data-stu-id="7d5ac-112">We'll be starting slowly, so beginner level web development experience is okay.</span></span>

<span data-ttu-id="7d5ac-113">Aplikacja, którą tworzymy będzie jest proste magazyn online.</span><span class="sxs-lookup"><span data-stu-id="7d5ac-113">The application we'll be building is a simple on-line store.</span></span>

![](tailspin-spyworks-part-1/_static/image1.jpg)


<span data-ttu-id="7d5ac-114">Goście mogą przeglądać produktów według kategorii:</span><span class="sxs-lookup"><span data-stu-id="7d5ac-114">Visitors can browse Products by Category:</span></span>

![](tailspin-spyworks-part-1/_static/image2.jpg)

<span data-ttu-id="7d5ac-115">Mogą wyświetlać pojedynczego produktu i dodać go do koszyka ich:</span><span class="sxs-lookup"><span data-stu-id="7d5ac-115">They can view a single product and add it to their cart:</span></span>

![](tailspin-spyworks-part-1/_static/image3.jpg)

<span data-ttu-id="7d5ac-116">Mogą przeglądać ich koszyka, usuwając wszystkie elementy, które nie są już potrzebne:</span><span class="sxs-lookup"><span data-stu-id="7d5ac-116">They can review their cart, removing any items they no longer want:</span></span>

![](tailspin-spyworks-part-1/_static/image4.jpg)

<span data-ttu-id="7d5ac-117">Przejściem do wyewidencjonowania spowoduje wyświetlenie monitu o</span><span class="sxs-lookup"><span data-stu-id="7d5ac-117">Proceeding to Checkout will prompt them to</span></span>

![](tailspin-spyworks-part-1/_static/image5.jpg)

![](tailspin-spyworks-part-1/_static/image6.jpg)

<span data-ttu-id="7d5ac-118">Po określeniu kolejności, zobaczy ekran potwierdzenia proste:</span><span class="sxs-lookup"><span data-stu-id="7d5ac-118">After ordering, they see a simple confirmation screen:</span></span>

![](tailspin-spyworks-part-1/_static/image7.jpg)


<span data-ttu-id="7d5ac-119">Firma Microsoft rozpocznie się, tworząc nowy projekt formularzy sieci Web platformy ASP.NET w programie Visual Studio 2010, a przyrostowe dodamy funkcje umożliwiające tworzenie kompletna aplikacja działa.</span><span class="sxs-lookup"><span data-stu-id="7d5ac-119">We'll begin by creating a new ASP.NET WebForms project in Visual Studio 2010, and we'll incrementally add features to create a complete functioning application.</span></span> <span data-ttu-id="7d5ac-120">Po drodze omówimy dostępu do bazy danych, widoku listy i siatki, stron aktualizacji danych, sprawdzanie poprawności danych, za pomocą stron wzorcowych dla układu strony spójne, AJAX, weryfikacji i użytkownika członkostwa.</span><span class="sxs-lookup"><span data-stu-id="7d5ac-120">Along the way, we'll cover database access, list and grid views, data update pages, data validation, using master pages for consistent page layout, AJAX, validation, user membership, and more.</span></span>

<span data-ttu-id="7d5ac-121">Można wykonać krok po kroku, można również pobrać gotową aplikację z [http://tailspinspyworks.codeplex.com/](http://tailspinspyworks.codeplex.com/)</span><span class="sxs-lookup"><span data-stu-id="7d5ac-121">You can follow along step by step, or you can download the completed application from [http://tailspinspyworks.codeplex.com/](http://tailspinspyworks.codeplex.com/)</span></span>

<span data-ttu-id="7d5ac-122">Możesz użyć programu Visual Studio 2010 lub bezpłatnego programu Visual Web Developer 2010 z [ https://www.microsoft.com/express/Web/ ](https://www.microsoft.com/express/Web/).</span><span class="sxs-lookup"><span data-stu-id="7d5ac-122">You can use either Visual Studio 2010 or the free Visual Web Developer 2010 from [https://www.microsoft.com/express/Web/](https://www.microsoft.com/express/Web/).</span></span> <span data-ttu-id="7d5ac-123">Aby skompilować aplikację, można użyć programu SQL Server lub bezpłatnego programu SQL Server Express do hosta bazy danych.</span><span class="sxs-lookup"><span data-stu-id="7d5ac-123">To build the application, you can use either SQL Server or the free SQL Server Express to host the database.</span></span>

## <a id="_Toc260221667"></a>  <span data-ttu-id="7d5ac-124">Plik / nowy projekt</span><span class="sxs-lookup"><span data-stu-id="7d5ac-124">File / New Project</span></span>

<span data-ttu-id="7d5ac-125">Zaczniemy, wybierając nowy projekt z menu Plik w programie Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="7d5ac-125">We'll start by selecting the New Project from the File menu in Visual Studio.</span></span> <span data-ttu-id="7d5ac-126">Wyświetlenie okna dialogowego Nowy projekt.</span><span class="sxs-lookup"><span data-stu-id="7d5ac-126">This brings up the New Project dialog.</span></span>

![](tailspin-spyworks-part-1/_static/image8.jpg)

<span data-ttu-id="7d5ac-127">Wybierzemy Visual C# / szablonów sieci Web grupy po lewej stronie i w środkowej kolumnie Wybierz szablon "Aplikacja sieci Web programu ASP.NET".</span><span class="sxs-lookup"><span data-stu-id="7d5ac-127">We'll select the Visual C# / Web Templates group on the left, and then choose the "ASP.NET Web Application" template in the center column.</span></span> <span data-ttu-id="7d5ac-128">Nazwij swój projekt TailspinSpyworks i naciśnij przycisk OK.</span><span class="sxs-lookup"><span data-stu-id="7d5ac-128">Name your project TailspinSpyworks and press the OK button.</span></span>

![](tailspin-spyworks-part-1/_static/image9.jpg)

<span data-ttu-id="7d5ac-129">Spowoduje to utworzenie projekcie.</span><span class="sxs-lookup"><span data-stu-id="7d5ac-129">This will create our project.</span></span> <span data-ttu-id="7d5ac-130">Spójrzmy na foldery, które znajdują się w naszej aplikacji w Eksploratorze rozwiązań po prawej stronie.</span><span class="sxs-lookup"><span data-stu-id="7d5ac-130">Let's take a look at the folders that are included in our application in the Solution Explorer on the right side.</span></span>

![](tailspin-spyworks-part-1/_static/image10.jpg)

<span data-ttu-id="7d5ac-131">Puste rozwiązanie nie jest całkowicie pusty — dodaje strukturę folderów podstawowe:</span><span class="sxs-lookup"><span data-stu-id="7d5ac-131">The Empty Solution isn't completely empty – it adds a basic folder structure:</span></span>

![](tailspin-spyworks-part-1/_static/image1.png)

<span data-ttu-id="7d5ac-132">Należy pamiętać, konwencje implementowany przez domyślny szablon projektu platformy ASP.NET 4.</span><span class="sxs-lookup"><span data-stu-id="7d5ac-132">Note the conventions implemented by the ASP.NET 4 default project template.</span></span>

- <span data-ttu-id="7d5ac-133">Folder "Konto" implementuje podstawowy interfejs użytkownika dla stron ASP. Podsystem członkostwo w sieci.</span><span class="sxs-lookup"><span data-stu-id="7d5ac-133">The "Account" folder implements a basic user interface for ASP.NET's membership subsystem.</span></span>
- <span data-ttu-id="7d5ac-134">Folder "Skryptów" służy jako repozytorium dla plików JavaScript po stronie klienta i podstawowe pliki .js jQuery są udostępnione domyślnie.</span><span class="sxs-lookup"><span data-stu-id="7d5ac-134">The "Scripts" folder serves as the repository for client side JavaScript files and the core jQuery .js files are made available by default.</span></span>
- <span data-ttu-id="7d5ac-135">Folder "Style" służy do organizowania nasze elementy wizualne witryny sieci web (arkusze stylów CSS)</span><span class="sxs-lookup"><span data-stu-id="7d5ac-135">The "Styles" folder is used to organize our web site visuals (CSS Style Sheets)</span></span>

<span data-ttu-id="7d5ac-136">Po naciśnięciu klawisza F5, aby uruchomić aplikację i renderowania strony default.aspx zobaczymy poniższe.</span><span class="sxs-lookup"><span data-stu-id="7d5ac-136">When we press F5 to run our application and render the default.aspx page we see the following.</span></span>

![](tailspin-spyworks-part-1/_static/image11.jpg)

<span data-ttu-id="7d5ac-137">Naszym pierwszym rozszerzenie aplikacji będzie zastąpić plik Style.css za pomocą domyślnego szablonu formularzy sieci Web przy użyciu klas CSS i powiązane pliki obrazów, które będą renderowane asthetics wizualne, które warto dla naszej aplikacji Tailspin Spyworks.</span><span class="sxs-lookup"><span data-stu-id="7d5ac-137">Our first application enhancement will be to replace the Style.css file from the default WebForms template with the CSS classes and associated image files that will render the visual asthetics that we want for our Tailspin Spyworks application.</span></span>

<span data-ttu-id="7d5ac-138">Po wykonaniu tej czynności powoduje wyświetlenie strony default.aspx następująco.</span><span class="sxs-lookup"><span data-stu-id="7d5ac-138">After doing so our default.aspx page renders like this.</span></span>

![](tailspin-spyworks-part-1/_static/image12.jpg)

<span data-ttu-id="7d5ac-139">Zwróć uwagę, linków obrazów w prawym górnym rogu strony i elementy menu, które zostały dodane do strony wzorcowej.</span><span class="sxs-lookup"><span data-stu-id="7d5ac-139">Notice the image links at the top right of the page and the menu items that have been added to the master page.</span></span> <span data-ttu-id="7d5ac-140">Tylko łącza "Sign In" i "Konto" wskaż stron, które istnieją (wygenerowanych przez szablon domyślny) i pozostałej części strony, które wdrażamy jak nasza aplikacja jest wbudowana.</span><span class="sxs-lookup"><span data-stu-id="7d5ac-140">Only the "Sign In" and "Account" links point to pages that exist (generated by the default template) and the rest of the pages we will implement as we build our application.</span></span>

<span data-ttu-id="7d5ac-141">Przedstawimy również przenosić strony wzorcowej do katalogu style.</span><span class="sxs-lookup"><span data-stu-id="7d5ac-141">We're also going to relocate the Master Page to the Styles directory.</span></span> <span data-ttu-id="7d5ac-142">Chociaż jest to tylko preferencję rozsądne może okazać się elementów była nieco łatwiejsza, jeśli firma zdecyduje się wprowadzić naszej aplikacji w "skinable" w przyszłości.</span><span class="sxs-lookup"><span data-stu-id="7d5ac-142">Though this is only a preference it may make things a little easier if we decide to make our application "skinable" in the future.</span></span>

<span data-ttu-id="7d5ac-143">Po wykonaniu tego musisz zmienić stronę wzorcową odwołań we wszystkich plikach aspx generowane przez domyślne strony formularzy sieci Web platformy ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="7d5ac-143">After doing this we'll need to change the master page references in all the .aspx files generated by the default ASP.NET WebForms pages.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="7d5ac-144">Next</span><span class="sxs-lookup"><span data-stu-id="7d5ac-144">Next</span></span>](tailspin-spyworks-part-2.md)
