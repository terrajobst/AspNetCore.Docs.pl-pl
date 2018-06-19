---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-1
title: 'Część 1: Omówienie i plik -> Nowy projekt | Dokumentacja firmy Microsoft'
author: jongalloway
description: Ten samouczek serii zawiera szczegóły dotyczące wszystkich kroków tworzenia przykładowej aplikacji ASP.NET MVC utworów muzycznych magazynu. Część 1 obejmuje przegląd oraz plik -> Nowy projekt.
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/21/2011
ms.topic: article
ms.assetid: bd356ca3-5bdb-4067-9dac-c9e9923a86e8
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-1
msc.type: authoredcontent
ms.openlocfilehash: 2082927d18c95563893da199d60347fa15952446
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
ms.locfileid: "30868974"
---
<a name="part-1-overview-and-file-new-project"></a><span data-ttu-id="3e9f0-104">Część 1: Omówienie i plik -> Nowy projekt</span><span class="sxs-lookup"><span data-stu-id="3e9f0-104">Part 1: Overview and File->New Project</span></span>
====================
<span data-ttu-id="3e9f0-105">przez [Galloway Jan](https://github.com/jongalloway)</span><span class="sxs-lookup"><span data-stu-id="3e9f0-105">by [Jon Galloway](https://github.com/jongalloway)</span></span>

> <span data-ttu-id="3e9f0-106">Magazyn utworów muzycznych MVC jest samouczek aplikacji, które wprowadzono i opisano krok po kroku, jak używać do tworzenia aplikacji sieci web platformy ASP.NET MVC i Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="3e9f0-106">The MVC Music Store is a tutorial application that introduces and explains step-by-step how to use ASP.NET MVC and Visual Studio for web development.</span></span>  
>   
> <span data-ttu-id="3e9f0-107">Magazyn utworów muzycznych MVC jest implementacja magazynu lekkie próbki, co sprzedaje albumów muzycznych w trybie online i implementuje podstawowej witryny administracji, logowania użytkownika i funkcje koszyka zakupów.</span><span class="sxs-lookup"><span data-stu-id="3e9f0-107">The MVC Music Store is a lightweight sample store implementation which sells music albums online, and implements basic site administration, user sign-in, and shopping cart functionality.</span></span>  
>   
> <span data-ttu-id="3e9f0-108">Ten samouczek serii zawiera szczegóły dotyczące wszystkich kroków tworzenia przykładowej aplikacji ASP.NET MVC utworów muzycznych magazynu.</span><span class="sxs-lookup"><span data-stu-id="3e9f0-108">This tutorial series details all of the steps taken to build the ASP.NET MVC Music Store sample application.</span></span> <span data-ttu-id="3e9f0-109">Część 1 zawiera omówienie i pliku -&gt;nowy projekt.</span><span class="sxs-lookup"><span data-stu-id="3e9f0-109">Part 1 covers Overview and File-&gt;New Project.</span></span>


## <a name="overview"></a><span data-ttu-id="3e9f0-110">Omówienie</span><span class="sxs-lookup"><span data-stu-id="3e9f0-110">Overview</span></span>

<span data-ttu-id="3e9f0-111">Magazyn utworów muzycznych MVC jest samouczek aplikacji, które wprowadzono i opisano krok po kroku, jak używać do tworzenia aplikacji sieci web platformy ASP.NET MVC i Visual Web Developer.</span><span class="sxs-lookup"><span data-stu-id="3e9f0-111">The MVC Music Store is a tutorial application that introduces and explains step-by-step how to use ASP.NET MVC and Visual Web Developer for web development.</span></span> <span data-ttu-id="3e9f0-112">Firma Microsoft będzie powoli uruchamianie, środowisko rozwoju poziomu sieci web dla początkujących użytkowników jest poprawny.</span><span class="sxs-lookup"><span data-stu-id="3e9f0-112">We'll be starting slowly, so beginner level web development experience is okay.</span></span>

<span data-ttu-id="3e9f0-113">Aplikacji, które firma Microsoft będzie zbudować jest magazynem muzyka proste.</span><span class="sxs-lookup"><span data-stu-id="3e9f0-113">The application we'll be building is a simple music store.</span></span> <span data-ttu-id="3e9f0-114">Istnieją trzy główne części do aplikacji: zakupów, wyewidencjonowania i administracji.</span><span class="sxs-lookup"><span data-stu-id="3e9f0-114">There are three main parts to the application: shopping, checkout, and administration.</span></span>

![](mvc-music-store-part-1/_static/image1.jpg)

<span data-ttu-id="3e9f0-115">Odwiedzający mogą przeglądać albumy według rodzaju:</span><span class="sxs-lookup"><span data-stu-id="3e9f0-115">Visitors can browse Albums by Genre:</span></span>

![](mvc-music-store-part-1/_static/image2.jpg)

<span data-ttu-id="3e9f0-116">Mogą wyświetlać jednego albumu i dodaj go do ich koszyka:</span><span class="sxs-lookup"><span data-stu-id="3e9f0-116">They can view a single album and add it to their cart:</span></span>

![](mvc-music-store-part-1/_static/image3.jpg)

<span data-ttu-id="3e9f0-117">Mogą one przejrzeć ich koszyka, usuwając wszystkie elementy, które nie są już potrzebne:</span><span class="sxs-lookup"><span data-stu-id="3e9f0-117">They can review their cart, removing any items they no longer want:</span></span>

![](mvc-music-store-part-1/_static/image4.jpg)

<span data-ttu-id="3e9f0-118">Przejściem do wyewidencjonowania wyświetli monit o ich zalogować się lub zarejestrować się na koncie użytkownika.</span><span class="sxs-lookup"><span data-stu-id="3e9f0-118">Proceeding to Checkout will prompt them to login or register for a user account.</span></span>

![](mvc-music-store-part-1/_static/image1.png)

![](mvc-music-store-part-1/_static/image2.png)

<span data-ttu-id="3e9f0-119">Po utworzeniu konta ich zakończeniu kolejności, wypełniając informacji wysyłki i płatności.</span><span class="sxs-lookup"><span data-stu-id="3e9f0-119">After creating an account, they can complete the order by filling out shipping and payment information.</span></span> <span data-ttu-id="3e9f0-120">Aby zachować proste rzeczy, prowadzimy niesamowite podwyższania poziomu: wszystko, co jest bezpłatne, jeśli użytkownik podał kod promocyjny "Wolne"!</span><span class="sxs-lookup"><span data-stu-id="3e9f0-120">To keep things simple, we're running an amazing promotion: everything's free if they enter promotion code "FREE"!</span></span>

![](mvc-music-store-part-1/_static/image5.jpg)

<span data-ttu-id="3e9f0-121">Po określeniu kolejności, zobaczy ekran potwierdzenia prosty:</span><span class="sxs-lookup"><span data-stu-id="3e9f0-121">After ordering, they see a simple confirmation screen:</span></span>

![](mvc-music-store-part-1/_static/image6.jpg)

<span data-ttu-id="3e9f0-122">Oprócz klienta faceing stron firma Microsoft będzie także kompilacji sekcji administratora, zawierający listę albumy, z których administratorzy mogą tworzyć, edycji i Usuń albumów:</span><span class="sxs-lookup"><span data-stu-id="3e9f0-122">In addition to customer-faceing pages, we'll also build an administrator section that shows a list of albums from which Administrators can Create, Edit, and Delete albums:</span></span>

![](mvc-music-store-part-1/_static/image7.jpg)

## <a name="1-file--gt-new-project"></a><span data-ttu-id="3e9f0-123">1. Plik —&gt; nowy projekt</span><span class="sxs-lookup"><span data-stu-id="3e9f0-123">1. File -&gt; New Project</span></span>

### <a name="installing-the-software"></a><span data-ttu-id="3e9f0-124">Instalowanie oprogramowania</span><span class="sxs-lookup"><span data-stu-id="3e9f0-124">Installing the software</span></span>

<span data-ttu-id="3e9f0-125">W tym samouczku rozpocznie się przez utworzenie nowego projektu platformy ASP.NET MVC 3 przy użyciu wolnego Visual Web Developer 2010 Express (który jest wolny), a następnie stopniowo dodamy funkcje tworzenia kompletna aplikacja działa.</span><span class="sxs-lookup"><span data-stu-id="3e9f0-125">This tutorial will begin by creating a new ASP.NET MVC 3 project using the free Visual Web Developer 2010 Express (which is free), and then we'll incrementally add features to create a complete functioning application.</span></span> <span data-ttu-id="3e9f0-126">Przeglądania omówione zostaną następujące czynności dostępu do bazy danych, scenariuszy Księgowanie formularza, sprawdzanie poprawności danych, za pomocą stron wzorcowych w spójne strony układu, za pomocą interfejsu AJAX dla strony aktualizacji i sprawdzania poprawności, dane logowania użytkownika i.</span><span class="sxs-lookup"><span data-stu-id="3e9f0-126">Along the way, we'll cover database access, form posting scenarios, data validation, using master pages for consistent page layout, using AJAX for page updates and validation, user login, and more.</span></span>

<span data-ttu-id="3e9f0-127">Można wykonać krok po kroku, można również pobrać ukończona aplikacja z [MVC utworów muzycznych magazynu](https://github.com/evilDave/MVC-Music-Store).</span><span class="sxs-lookup"><span data-stu-id="3e9f0-127">You can follow along step by step, or you can download the completed application from [MVC-Music-Store](https://github.com/evilDave/MVC-Music-Store).</span></span>

<span data-ttu-id="3e9f0-128">Dodatek SP1 dla programu Visual Studio 2010 lub Visual Web Developer 2010 Express z dodatkiem SP1 (bezpłatna wersja programu Visual Studio 2010) służy do tworzenia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="3e9f0-128">You can use either Visual Studio 2010 SP1 or Visual Web Developer 2010 Express SP1 (a free version of Visual Studio 2010) to build the application.</span></span> <span data-ttu-id="3e9f0-129">Będziemy używać programu SQL Server Compact (również bezpłatnie) do obsługi bazy danych.</span><span class="sxs-lookup"><span data-stu-id="3e9f0-129">We'll be using the SQL Server Compact (also free) to host the database.</span></span> <span data-ttu-id="3e9f0-130">Przed rozpoczęciem upewnij się, że po zainstalowaniu wymagania wstępne wymienione poniżej.</span><span class="sxs-lookup"><span data-stu-id="3e9f0-130">Before you start, make sure you've installed the prerequisites listed below.</span></span>


- <span data-ttu-id="3e9f0-131">[Wymagania wstępne programu visual Studio Web Developer Express z dodatkiem SP1]</span><span class="sxs-lookup"><span data-stu-id="3e9f0-131">[Visual Studio Web Developer Express SP1 prerequisites]</span></span>
- <span data-ttu-id="3e9f0-132">[Aktualizacji narzędzi programu ASP.NET MVC 3]</span><span class="sxs-lookup"><span data-stu-id="3e9f0-132">[ASP.NET MVC 3 Tools Update]</span></span>
- <span data-ttu-id="3e9f0-133">[SQL Server Compact 4.0] - tym obsługi zarówno środowiska uruchomieniowego i narzędzia</span><span class="sxs-lookup"><span data-stu-id="3e9f0-133">[SQL Server Compact 4.0] - including both runtime and tools support</span></span>


### <a name="creating-a-new-aspnet-mvc-3-project"></a><span data-ttu-id="3e9f0-134">Tworzenie nowego projektu platformy ASP.NET MVC 3</span><span class="sxs-lookup"><span data-stu-id="3e9f0-134">Creating a new ASP.NET MVC 3 project</span></span>

<span data-ttu-id="3e9f0-135">Zaczniemy, wybierając "Nowego projektu" z menu Plik programu Visual Web Developer.</span><span class="sxs-lookup"><span data-stu-id="3e9f0-135">We'll start by selecting "New Project" from the File menu in Visual Web Developer.</span></span> <span data-ttu-id="3e9f0-136">Spowoduje to wyświetlenie okna dialogowego Nowy projekt.</span><span class="sxs-lookup"><span data-stu-id="3e9f0-136">This brings up the New Project dialog.</span></span>

![](mvc-music-store-part-1/_static/image5.png)

<span data-ttu-id="3e9f0-137">Wybierzemy Visual C# —&gt; szablonów sieci Web grupę po lewej stronie, a następnie wybierz szablon "Platformy ASP.NET MVC 3 aplikacji sieci Web" w środkowej kolumnie.</span><span class="sxs-lookup"><span data-stu-id="3e9f0-137">We'll select the Visual C# -&gt; Web Templates group on the left, then choose the "ASP.NET MVC 3 Web Application" template in the center column.</span></span> <span data-ttu-id="3e9f0-138">Nazwa projektu MvcMusicStore i naciśnij przycisk OK.</span><span class="sxs-lookup"><span data-stu-id="3e9f0-138">Name your project MvcMusicStore and press the OK button.</span></span>

![](mvc-music-store-part-1/_static/image8.jpg)

<span data-ttu-id="3e9f0-139">Spowoduje to wyświetlenie dodatkowej okno dialogowe, które pozwala nam w tworzeniu niektórych MVC ustawienia specyficzne dla naszych projektu.</span><span class="sxs-lookup"><span data-stu-id="3e9f0-139">This will display a secondary dialog which allows us to make some MVC specific settings for our project.</span></span> <span data-ttu-id="3e9f0-140">Wybierz następujące pozycje:</span><span class="sxs-lookup"><span data-stu-id="3e9f0-140">Select the following:</span></span>

<span data-ttu-id="3e9f0-141">Projekt szablonu — wybierz opcję Pusta</span><span class="sxs-lookup"><span data-stu-id="3e9f0-141">Project Template - select Empty</span></span>

<span data-ttu-id="3e9f0-142">Wyświetl aparat — Wybieranie Razor</span><span class="sxs-lookup"><span data-stu-id="3e9f0-142">View Engine - select Razor</span></span>

<span data-ttu-id="3e9f0-143">Użyj znaczników semantyki HTML5 - zaznaczone</span><span class="sxs-lookup"><span data-stu-id="3e9f0-143">Use HTML5 semantic markup - checked</span></span>

<span data-ttu-id="3e9f0-144">Sprawdź, czy ustawienia są w sposób przedstawiony poniżej, a następnie naciśnij przycisk OK.</span><span class="sxs-lookup"><span data-stu-id="3e9f0-144">Verify that your settings are as shown below, then press the OK button.</span></span>

![](mvc-music-store-part-1/_static/image9.jpg)

<span data-ttu-id="3e9f0-145">Spowoduje to utworzenie naszych projektu.</span><span class="sxs-lookup"><span data-stu-id="3e9f0-145">This will create our project.</span></span> <span data-ttu-id="3e9f0-146">Spójrzmy na foldery, które zostały dodane do naszej aplikacji w Eksploratorze rozwiązań po prawej stronie.</span><span class="sxs-lookup"><span data-stu-id="3e9f0-146">Let's take a look at the folders that have been added to our application in the Solution Explorer on the right side.</span></span>

![](mvc-music-store-part-1/_static/image10.jpg)

<span data-ttu-id="3e9f0-147">Szablon pusty MVC 3 nie jest pusty — dodaje strukturę folderów podstawowe:</span><span class="sxs-lookup"><span data-stu-id="3e9f0-147">The Empty MVC 3 template isn't completely empty – it adds a basic folder structure:</span></span>

![](mvc-music-store-part-1/_static/image6.png)

<span data-ttu-id="3e9f0-148">ASP.NET MVC korzysta z niektóre podstawowe konwencje nazewnictwa dla nazwy folderu:</span><span class="sxs-lookup"><span data-stu-id="3e9f0-148">ASP.NET MVC makes use of some basic naming conventions for folder names:</span></span>

| <span data-ttu-id="3e9f0-149">**Folder**</span><span class="sxs-lookup"><span data-stu-id="3e9f0-149">**Folder**</span></span> | <span data-ttu-id="3e9f0-150">**Cel**</span><span class="sxs-lookup"><span data-stu-id="3e9f0-150">**Purpose**</span></span> |
| --- | --- |
| <span data-ttu-id="3e9f0-151">**/ Kontrolerów**</span><span class="sxs-lookup"><span data-stu-id="3e9f0-151">**/Controllers**</span></span> | <span data-ttu-id="3e9f0-152">Kontrolery odpowiedzieć na dane wejściowe z przeglądarki, czynności, jakie należy wykonać, a odpowiedź zwrócona do użytkownika.</span><span class="sxs-lookup"><span data-stu-id="3e9f0-152">Controllers respond to input from the browser, decide what to do with it, and return response to the user.</span></span> |
| <span data-ttu-id="3e9f0-153">**/ Widoków**</span><span class="sxs-lookup"><span data-stu-id="3e9f0-153">**/Views**</span></span> | <span data-ttu-id="3e9f0-154">Widoki przechowywania naszym szablony interfejsu użytkownika</span><span class="sxs-lookup"><span data-stu-id="3e9f0-154">Views hold our UI templates</span></span> |
| <span data-ttu-id="3e9f0-155">**/ Modeli**</span><span class="sxs-lookup"><span data-stu-id="3e9f0-155">**/Models**</span></span> | <span data-ttu-id="3e9f0-156">Modele przechowywania i manipulowanie danymi</span><span class="sxs-lookup"><span data-stu-id="3e9f0-156">Models hold and manipulate data</span></span> |
| <span data-ttu-id="3e9f0-157">**/Content**</span><span class="sxs-lookup"><span data-stu-id="3e9f0-157">**/Content**</span></span> | <span data-ttu-id="3e9f0-158">Ten folder przechowuje naszych obrazów, CSS i innej zawartości statycznej</span><span class="sxs-lookup"><span data-stu-id="3e9f0-158">This folder holds our images, CSS, and any other static content</span></span> |
| <span data-ttu-id="3e9f0-159">**/ Skryptów**</span><span class="sxs-lookup"><span data-stu-id="3e9f0-159">**/Scripts**</span></span> | <span data-ttu-id="3e9f0-160">Ten folder przechowuje plików JavaScript</span><span class="sxs-lookup"><span data-stu-id="3e9f0-160">This folder holds our JavaScript files</span></span> |

<span data-ttu-id="3e9f0-161">Te foldery są zawarte nawet w przypadku aplikacji pusty ASP.NET MVC, ponieważ platforma ASP.NET MVC domyślnie korzysta z podejścia "Konwencji za pośrednictwem konfiguracji" i sprawia, że niektóre założenia domyślne oparte na konwencji nazewnictwa folderu.</span><span class="sxs-lookup"><span data-stu-id="3e9f0-161">These folders are included even in an Empty ASP.NET MVC application because the ASP.NET MVC framework by default uses a "convention over configuration" approach and makes some default assumptions based on folder naming conventions.</span></span> <span data-ttu-id="3e9f0-162">Na przykład kontrolery szukają widoków w folderze Widoki domyślnie bez konieczności jawnego określania to w kodzie.</span><span class="sxs-lookup"><span data-stu-id="3e9f0-162">For instance, controllers look for views in the Views folder by default without you having to explicitly specify this in your code.</span></span> <span data-ttu-id="3e9f0-163">Spełni domyślnych Konwencji zmniejsza ilość kodu, należy napisać, i może również ułatwić inni deweloperzy zrozumienie projektu.</span><span class="sxs-lookup"><span data-stu-id="3e9f0-163">Sticking with the default conventions reduces the amount of code you need to write, and can also make it easier for other developers to understand your project.</span></span> <span data-ttu-id="3e9f0-164">Wyjaśniamy tych konwencji więcej, jak mamy utworzyć w naszej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="3e9f0-164">We'll explain these conventions more as we build our application.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="3e9f0-165">Next</span><span class="sxs-lookup"><span data-stu-id="3e9f0-165">Next</span></span>](mvc-music-store-part-2.md)
