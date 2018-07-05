---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-1
title: 'Część 1: Omówienie i plik -> Nowy projekt | Dokumentacja firmy Microsoft'
author: jongalloway
description: W tej serii samouczków szczegółowo opisuje wszystkie etapy, tworzenie przykładowej aplikacji platformy ASP.NET MVC Music Store. Część 1 obejmuje omówienie i plik -> Nowy projekt.
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/21/2011
ms.topic: article
ms.assetid: bd356ca3-5bdb-4067-9dac-c9e9923a86e8
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-1
msc.type: authoredcontent
ms.openlocfilehash: c03b62db2227c167c68ca5cf8174e4322658d39d
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37377892"
---
<a name="part-1-overview-and-file-new-project"></a><span data-ttu-id="33fa8-104">Część 1: Omówienie i plik -> Nowy projekt</span><span class="sxs-lookup"><span data-stu-id="33fa8-104">Part 1: Overview and File->New Project</span></span>
====================
<span data-ttu-id="33fa8-105">przez [Galloway'em Jon](https://github.com/jongalloway)</span><span class="sxs-lookup"><span data-stu-id="33fa8-105">by [Jon Galloway](https://github.com/jongalloway)</span></span>

> <span data-ttu-id="33fa8-106">MVC Music Store jest aplikacją z samouczka, który wprowadzono i opisano krok po kroku, jak używać platformy ASP.NET MVC i programu Visual Studio do tworzenia aplikacji internetowych.</span><span class="sxs-lookup"><span data-stu-id="33fa8-106">The MVC Music Store is a tutorial application that introduces and explains step-by-step how to use ASP.NET MVC and Visual Studio for web development.</span></span>  
>   
> <span data-ttu-id="33fa8-107">MVC Music Store jest uproszczone przykładową implementację magazynu sprzedaje utworów muzycznych albumy online, która implementuje podstawowej witryny administracji, logowania użytkownika i funkcje koszyka zakupów.</span><span class="sxs-lookup"><span data-stu-id="33fa8-107">The MVC Music Store is a lightweight sample store implementation which sells music albums online, and implements basic site administration, user sign-in, and shopping cart functionality.</span></span>  
>   
> <span data-ttu-id="33fa8-108">W tej serii samouczków szczegółowo opisuje wszystkie etapy, tworzenie przykładowej aplikacji platformy ASP.NET MVC Music Store.</span><span class="sxs-lookup"><span data-stu-id="33fa8-108">This tutorial series details all of the steps taken to build the ASP.NET MVC Music Store sample application.</span></span> <span data-ttu-id="33fa8-109">Część 1 obejmuje przegląd i plikach&gt;nowy projekt.</span><span class="sxs-lookup"><span data-stu-id="33fa8-109">Part 1 covers Overview and File-&gt;New Project.</span></span>


## <a name="overview"></a><span data-ttu-id="33fa8-110">Omówienie</span><span class="sxs-lookup"><span data-stu-id="33fa8-110">Overview</span></span>

<span data-ttu-id="33fa8-111">MVC Music Store jest aplikacją z samouczka, który wprowadza i opisano krok po kroku na potrzeby tworzenia aplikacji sieci web platformy ASP.NET MVC i Visual Web Developer.</span><span class="sxs-lookup"><span data-stu-id="33fa8-111">The MVC Music Store is a tutorial application that introduces and explains step-by-step how to use ASP.NET MVC and Visual Web Developer for web development.</span></span> <span data-ttu-id="33fa8-112">Firma Microsoft będzie uruchamiana powoli, a więc środowisko programowania dla początkujących poziomu sieci web jest poprawny.</span><span class="sxs-lookup"><span data-stu-id="33fa8-112">We'll be starting slowly, so beginner level web development experience is okay.</span></span>

<span data-ttu-id="33fa8-113">Aplikacja, którą tworzymy będzie jest proste music store —.</span><span class="sxs-lookup"><span data-stu-id="33fa8-113">The application we'll be building is a simple music store.</span></span> <span data-ttu-id="33fa8-114">Istnieją trzy główne fragmenty aplikacji: zakupów, finalizacja zakupu i administracyjnej.</span><span class="sxs-lookup"><span data-stu-id="33fa8-114">There are three main parts to the application: shopping, checkout, and administration.</span></span>

![](mvc-music-store-part-1/_static/image1.jpg)

<span data-ttu-id="33fa8-115">Goście mogą przeglądać albumów według gatunku:</span><span class="sxs-lookup"><span data-stu-id="33fa8-115">Visitors can browse Albums by Genre:</span></span>

![](mvc-music-store-part-1/_static/image2.jpg)

<span data-ttu-id="33fa8-116">Mogą wyświetlać jednego albumu i dodać go do koszyka ich:</span><span class="sxs-lookup"><span data-stu-id="33fa8-116">They can view a single album and add it to their cart:</span></span>

![](mvc-music-store-part-1/_static/image3.jpg)

<span data-ttu-id="33fa8-117">Mogą przeglądać ich koszyka, usuwając wszystkie elementy, które nie są już potrzebne:</span><span class="sxs-lookup"><span data-stu-id="33fa8-117">They can review their cart, removing any items they no longer want:</span></span>

![](mvc-music-store-part-1/_static/image4.jpg)

<span data-ttu-id="33fa8-118">Przejściem do wyewidencjonowania spowoduje wyświetlenie monitu o zalogowanie lub Zarejestruj się, aby konto użytkownika.</span><span class="sxs-lookup"><span data-stu-id="33fa8-118">Proceeding to Checkout will prompt them to login or register for a user account.</span></span>

![](mvc-music-store-part-1/_static/image1.png)

![](mvc-music-store-part-1/_static/image2.png)

<span data-ttu-id="33fa8-119">Po utworzeniu konta usługi, możliwości ich ukończenia zamówienia, wypełniając informacji wysyłki i płatności.</span><span class="sxs-lookup"><span data-stu-id="33fa8-119">After creating an account, they can complete the order by filling out shipping and payment information.</span></span> <span data-ttu-id="33fa8-120">Aby zachować ich prostotę, prowadzimy wspaniałe podwyższania poziomu: wszystko, co jest bezpłatne, jeśli użytkownik podał kod promocyjny "BEZPŁATNY"!</span><span class="sxs-lookup"><span data-stu-id="33fa8-120">To keep things simple, we're running an amazing promotion: everything's free if they enter promotion code "FREE"!</span></span>

![](mvc-music-store-part-1/_static/image5.jpg)

<span data-ttu-id="33fa8-121">Po określeniu kolejności, zobaczy ekran potwierdzenia proste:</span><span class="sxs-lookup"><span data-stu-id="33fa8-121">After ordering, they see a simple confirmation screen:</span></span>

![](mvc-music-store-part-1/_static/image6.jpg)

<span data-ttu-id="33fa8-122">Oprócz stron faceing klienta firma Microsoft będzie również tworzenie sekcji administratora, który wyświetla listę albumy, z których administratorzy mogą tworzyć, edytować i Usuń albumów:</span><span class="sxs-lookup"><span data-stu-id="33fa8-122">In addition to customer-faceing pages, we'll also build an administrator section that shows a list of albums from which Administrators can Create, Edit, and Delete albums:</span></span>

![](mvc-music-store-part-1/_static/image7.jpg)

## <a name="1-file--gt-new-project"></a><span data-ttu-id="33fa8-123">1. Plik —&gt; nowy projekt</span><span class="sxs-lookup"><span data-stu-id="33fa8-123">1. File -&gt; New Project</span></span>

### <a name="installing-the-software"></a><span data-ttu-id="33fa8-124">Instalowanie oprogramowania</span><span class="sxs-lookup"><span data-stu-id="33fa8-124">Installing the software</span></span>

<span data-ttu-id="33fa8-125">Ten samouczek rozpoczyna się przez utworzenie nowego projektu ASP.NET MVC 3, przy użyciu bezpłatnego programu Visual Web Developer 2010 Express (która jest bezpłatna), a następnie stopniowo dodamy funkcje umożliwiające tworzenie kompletna aplikacja działa.</span><span class="sxs-lookup"><span data-stu-id="33fa8-125">This tutorial will begin by creating a new ASP.NET MVC 3 project using the free Visual Web Developer 2010 Express (which is free), and then we'll incrementally add features to create a complete functioning application.</span></span> <span data-ttu-id="33fa8-126">Po drodze omówimy dostępu do bazy danych, scenariusze ogłaszania formularza i sprawdzanie poprawności danych, za pomocą stron wzorcowych w układu strony spójne, za pomocą interfejsu AJAX do aktualizacji strony i sprawdzania poprawności, dane logowania użytkownika i innych.</span><span class="sxs-lookup"><span data-stu-id="33fa8-126">Along the way, we'll cover database access, form posting scenarios, data validation, using master pages for consistent page layout, using AJAX for page updates and validation, user login, and more.</span></span>

<span data-ttu-id="33fa8-127">Można wykonać krok po kroku, można również pobrać gotową aplikację z [MVC — muzyka-Store](https://github.com/evilDave/MVC-Music-Store).</span><span class="sxs-lookup"><span data-stu-id="33fa8-127">You can follow along step by step, or you can download the completed application from [MVC-Music-Store](https://github.com/evilDave/MVC-Music-Store).</span></span>

<span data-ttu-id="33fa8-128">Aby skompilować aplikację, można użyć programu Visual Studio 2010 z dodatkiem SP1 lub Visual Web Developer 2010 Express z dodatkiem SP1 (bezpłatna wersja programu Visual Studio 2010).</span><span class="sxs-lookup"><span data-stu-id="33fa8-128">You can use either Visual Studio 2010 SP1 or Visual Web Developer 2010 Express SP1 (a free version of Visual Studio 2010) to build the application.</span></span> <span data-ttu-id="33fa8-129">Będziemy używać programu SQL Server Compact (również wersja bezpłatna) do hostowania bazy danych.</span><span class="sxs-lookup"><span data-stu-id="33fa8-129">We'll be using the SQL Server Compact (also free) to host the database.</span></span> <span data-ttu-id="33fa8-130">Przed rozpoczęciem upewnij się, że po zainstalowaniu wymagań wstępnych wymienionych poniżej.</span><span class="sxs-lookup"><span data-stu-id="33fa8-130">Before you start, make sure you've installed the prerequisites listed below.</span></span>


- <span data-ttu-id="33fa8-131">[Wymagania wstępne programu visual Studio Web Developer Express z dodatkiem SP1]</span><span class="sxs-lookup"><span data-stu-id="33fa8-131">[Visual Studio Web Developer Express SP1 prerequisites]</span></span>
- <span data-ttu-id="33fa8-132">[Program ASP.NET MVC 3 Tools Update]</span><span class="sxs-lookup"><span data-stu-id="33fa8-132">[ASP.NET MVC 3 Tools Update]</span></span>
- <span data-ttu-id="33fa8-133">[SQL Server Compact 4.0] — łącznie z obsługą środowiska uruchomieniowego i narzędzi</span><span class="sxs-lookup"><span data-stu-id="33fa8-133">[SQL Server Compact 4.0] - including both runtime and tools support</span></span>


### <a name="creating-a-new-aspnet-mvc-3-project"></a><span data-ttu-id="33fa8-134">Tworzenie nowego projektu ASP.NET MVC 3</span><span class="sxs-lookup"><span data-stu-id="33fa8-134">Creating a new ASP.NET MVC 3 project</span></span>

<span data-ttu-id="33fa8-135">Zaczniemy, wybierając pozycję "Nowy projekt" w menu Plik w Visual Web Developer.</span><span class="sxs-lookup"><span data-stu-id="33fa8-135">We'll start by selecting "New Project" from the File menu in Visual Web Developer.</span></span> <span data-ttu-id="33fa8-136">Wyświetlenie okna dialogowego Nowy projekt.</span><span class="sxs-lookup"><span data-stu-id="33fa8-136">This brings up the New Project dialog.</span></span>

![](mvc-music-store-part-1/_static/image5.png)

<span data-ttu-id="33fa8-137">Wybierzemy Visual C# —&gt; szablonów sieci Web grupę po lewej stronie, a następnie wybierz szablon "Platformy ASP.NET MVC 3 aplikacja sieci Web" w środkowej kolumnie.</span><span class="sxs-lookup"><span data-stu-id="33fa8-137">We'll select the Visual C# -&gt; Web Templates group on the left, then choose the "ASP.NET MVC 3 Web Application" template in the center column.</span></span> <span data-ttu-id="33fa8-138">Nazwij swój projekt MvcMusicStore i naciśnij przycisk OK.</span><span class="sxs-lookup"><span data-stu-id="33fa8-138">Name your project MvcMusicStore and press the OK button.</span></span>

![](mvc-music-store-part-1/_static/image8.jpg)

<span data-ttu-id="33fa8-139">Spowoduje to wyświetlenie dodatkowych okna dialogowego, co pozwala nam w niektórych określonych ustawień MVC naszego projektu.</span><span class="sxs-lookup"><span data-stu-id="33fa8-139">This will display a secondary dialog which allows us to make some MVC specific settings for our project.</span></span> <span data-ttu-id="33fa8-140">Wybierz jedną z poniższych:</span><span class="sxs-lookup"><span data-stu-id="33fa8-140">Select the following:</span></span>

<span data-ttu-id="33fa8-141">Projekt szablonu — wybierz pusty</span><span class="sxs-lookup"><span data-stu-id="33fa8-141">Project Template - select Empty</span></span>

<span data-ttu-id="33fa8-142">Wyświetl aparatu — Wybieranie Razor</span><span class="sxs-lookup"><span data-stu-id="33fa8-142">View Engine - select Razor</span></span>

<span data-ttu-id="33fa8-143">Użyj HTML5 znaczników semantycznych - zaznaczone</span><span class="sxs-lookup"><span data-stu-id="33fa8-143">Use HTML5 semantic markup - checked</span></span>

<span data-ttu-id="33fa8-144">Sprawdź, czy ustawienia są, jak pokazano poniżej, a następnie naciśnij przycisk OK.</span><span class="sxs-lookup"><span data-stu-id="33fa8-144">Verify that your settings are as shown below, then press the OK button.</span></span>

![](mvc-music-store-part-1/_static/image9.jpg)

<span data-ttu-id="33fa8-145">Spowoduje to utworzenie projekcie.</span><span class="sxs-lookup"><span data-stu-id="33fa8-145">This will create our project.</span></span> <span data-ttu-id="33fa8-146">Przyjrzyjmy się w folderach, które zostały dodane do naszej aplikacji w Eksploratorze rozwiązań po prawej stronie.</span><span class="sxs-lookup"><span data-stu-id="33fa8-146">Let's take a look at the folders that have been added to our application in the Solution Explorer on the right side.</span></span>

![](mvc-music-store-part-1/_static/image10.jpg)

<span data-ttu-id="33fa8-147">Szablonu pusty MVC 3 nie jest całkowicie pusty — dodaje strukturę folderów podstawowe:</span><span class="sxs-lookup"><span data-stu-id="33fa8-147">The Empty MVC 3 template isn't completely empty – it adds a basic folder structure:</span></span>

![](mvc-music-store-part-1/_static/image6.png)

<span data-ttu-id="33fa8-148">ASP.NET MVC sprawia, że użycie niektóre podstawowe konwencje nazewnictwa dla nazw folderów:</span><span class="sxs-lookup"><span data-stu-id="33fa8-148">ASP.NET MVC makes use of some basic naming conventions for folder names:</span></span>

| <span data-ttu-id="33fa8-149">**Folder**</span><span class="sxs-lookup"><span data-stu-id="33fa8-149">**Folder**</span></span> | <span data-ttu-id="33fa8-150">**Cel**</span><span class="sxs-lookup"><span data-stu-id="33fa8-150">**Purpose**</span></span> |
| --- | --- |
| <span data-ttu-id="33fa8-151">**/ Kontrolerów**</span><span class="sxs-lookup"><span data-stu-id="33fa8-151">**/Controllers**</span></span> | <span data-ttu-id="33fa8-152">Kontrolery odpowiedzieć na dane wejściowe z przeglądarki, podejmowania decyzji o korzystają z nich, a następnie zwraca odpowiedź do użytkownika.</span><span class="sxs-lookup"><span data-stu-id="33fa8-152">Controllers respond to input from the browser, decide what to do with it, and return response to the user.</span></span> |
| <span data-ttu-id="33fa8-153">**/ Widoków**</span><span class="sxs-lookup"><span data-stu-id="33fa8-153">**/Views**</span></span> | <span data-ttu-id="33fa8-154">Widoki przechowywania nasze szablony interfejsu użytkownika</span><span class="sxs-lookup"><span data-stu-id="33fa8-154">Views hold our UI templates</span></span> |
| <span data-ttu-id="33fa8-155">**/ Modeli**</span><span class="sxs-lookup"><span data-stu-id="33fa8-155">**/Models**</span></span> | <span data-ttu-id="33fa8-156">Modele przechowywania i manipulowanie danymi</span><span class="sxs-lookup"><span data-stu-id="33fa8-156">Models hold and manipulate data</span></span> |
| <span data-ttu-id="33fa8-157">**/Content**</span><span class="sxs-lookup"><span data-stu-id="33fa8-157">**/Content**</span></span> | <span data-ttu-id="33fa8-158">Ten folder przechowuje nasze obrazy, CSS i innej zawartości statycznej</span><span class="sxs-lookup"><span data-stu-id="33fa8-158">This folder holds our images, CSS, and any other static content</span></span> |
| <span data-ttu-id="33fa8-159">**/ Skryptów**</span><span class="sxs-lookup"><span data-stu-id="33fa8-159">**/Scripts**</span></span> | <span data-ttu-id="33fa8-160">Ten folder zawiera nasz plików JavaScript</span><span class="sxs-lookup"><span data-stu-id="33fa8-160">This folder holds our JavaScript files</span></span> |

<span data-ttu-id="33fa8-161">Te foldery znajdują się nawet w przypadku aplikacji pusty ASP.NET MVC, ponieważ platforma ASP.NET MVC, domyślnie korzysta z metody "Konwencji za pośrednictwem konfiguracji" i zakłada pewne domyślne oparte na konwencjach nazewnictwa folderów.</span><span class="sxs-lookup"><span data-stu-id="33fa8-161">These folders are included even in an Empty ASP.NET MVC application because the ASP.NET MVC framework by default uses a "convention over configuration" approach and makes some default assumptions based on folder naming conventions.</span></span> <span data-ttu-id="33fa8-162">Na przykład kontrolery szukają widoków w folderze Widoki domyślnie bez konieczności jawnego określania to w kodzie.</span><span class="sxs-lookup"><span data-stu-id="33fa8-162">For instance, controllers look for views in the Views folder by default without you having to explicitly specify this in your code.</span></span> <span data-ttu-id="33fa8-163">Wyobrazić przy użyciu domyślnych Konwencji zmniejsza ilość kodu, należy napisać, i może również ułatwić innym deweloperom zrozumienie projektu.</span><span class="sxs-lookup"><span data-stu-id="33fa8-163">Sticking with the default conventions reduces the amount of code you need to write, and can also make it easier for other developers to understand your project.</span></span> <span data-ttu-id="33fa8-164">Wyjaśnimy tych konwencji więcej, jak możemy tworzyć naszej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="33fa8-164">We'll explain these conventions more as we build our application.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="33fa8-165">Next</span><span class="sxs-lookup"><span data-stu-id="33fa8-165">Next</span></span>](mvc-music-store-part-2.md)
