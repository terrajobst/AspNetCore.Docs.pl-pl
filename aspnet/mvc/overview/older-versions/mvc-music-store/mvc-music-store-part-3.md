---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-3
title: 'Część 3: Widoki i ViewModels | Dokumentacja firmy Microsoft'
author: jongalloway
description: Ten samouczek serii zawiera szczegóły dotyczące wszystkich kroków tworzenia przykładowej aplikacji ASP.NET MVC utworów muzycznych magazynu. Część 3 zawiera widoki ViewModels.
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/21/2011
ms.topic: article
ms.assetid: 94297aa0-1f2d-4d72-bbcb-63f64653e0c0
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-3
msc.type: authoredcontent
ms.openlocfilehash: 497c2898db2e03b0650982c3ad1e6b5ac5ca639d
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
---
<a name="part-3-views-and-viewmodels"></a><span data-ttu-id="bbfee-104">Część 3: Widoki i ViewModels</span><span class="sxs-lookup"><span data-stu-id="bbfee-104">Part 3: Views and ViewModels</span></span>
====================
<span data-ttu-id="bbfee-105">przez [Galloway Jan](https://github.com/jongalloway)</span><span class="sxs-lookup"><span data-stu-id="bbfee-105">by [Jon Galloway](https://github.com/jongalloway)</span></span>

> <span data-ttu-id="bbfee-106">Magazyn utworów muzycznych MVC jest samouczek aplikacji, które wprowadzono i opisano krok po kroku, jak używać do tworzenia aplikacji sieci web platformy ASP.NET MVC i Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="bbfee-106">The MVC Music Store is a tutorial application that introduces and explains step-by-step how to use ASP.NET MVC and Visual Studio for web development.</span></span>  
>   
> <span data-ttu-id="bbfee-107">Magazyn utworów muzycznych MVC jest implementacja magazynu lekkie próbki, co sprzedaje albumów muzycznych w trybie online i implementuje podstawowej witryny administracji, logowania użytkownika i funkcje koszyka zakupów.</span><span class="sxs-lookup"><span data-stu-id="bbfee-107">The MVC Music Store is a lightweight sample store implementation which sells music albums online, and implements basic site administration, user sign-in, and shopping cart functionality.</span></span>  
>   
> <span data-ttu-id="bbfee-108">Ten samouczek serii zawiera szczegóły dotyczące wszystkich kroków tworzenia przykładowej aplikacji ASP.NET MVC utworów muzycznych magazynu.</span><span class="sxs-lookup"><span data-stu-id="bbfee-108">This tutorial series details all of the steps taken to build the ASP.NET MVC Music Store sample application.</span></span> <span data-ttu-id="bbfee-109">Część 3 zawiera widoki ViewModels.</span><span class="sxs-lookup"><span data-stu-id="bbfee-109">Part 3 covers Views and ViewModels.</span></span>


<span data-ttu-id="bbfee-110">Do tej pory możemy został właśnie zostały zwracanie ciągów z akcji kontrolera.</span><span class="sxs-lookup"><span data-stu-id="bbfee-110">So far we've just been returning strings from controller actions.</span></span> <span data-ttu-id="bbfee-111">To dobry sposób, aby uzyskać informacje o tym, jak działają kontrolery, ale jest nie będzie sposób tworzenia aplikacji sieci web rzeczywistych.</span><span class="sxs-lookup"><span data-stu-id="bbfee-111">That's a nice way to get an idea of how controllers work, but it's not how you'd want to build a real web application.</span></span> <span data-ttu-id="bbfee-112">Zamierzamy mają lepszym sposobem generują kod HTML do przeglądarek odwiedzający naszej witrynie — co gdzie możemy użyć plików szablonów można łatwo dostosować zawartość HTML odesłania.</span><span class="sxs-lookup"><span data-stu-id="bbfee-112">We are going to want a better way to generate HTML back to browsers visiting our site – one where we can use template files to more easily customize the HTML content send back.</span></span> <span data-ttu-id="bbfee-113">To dokładnie czy widoków.</span><span class="sxs-lookup"><span data-stu-id="bbfee-113">That's exactly what Views do.</span></span>

## <a name="adding-a-view-template"></a><span data-ttu-id="bbfee-114">Dodawanie szablonu widoku</span><span class="sxs-lookup"><span data-stu-id="bbfee-114">Adding a View template</span></span>

<span data-ttu-id="bbfee-115">Aby użyć szablonu widoku, firma Microsoft będzie zmienić metodę HomeController indeks, aby zwracać element ActionResult, a jego zwracać View(), podobnie jak poniżej:</span><span class="sxs-lookup"><span data-stu-id="bbfee-115">To use a view-template, we'll change the HomeController Index method to return an ActionResult, and have it return View(), like below:</span></span>

[!code-csharp[Main](mvc-music-store-part-3/samples/sample1.cs)]

<span data-ttu-id="bbfee-116">Zmiany powyżej wskazuje, a nie zwrócił ciągu, chcemy zamiast tego użyj "Widok" wynik ponownie wygenerować.</span><span class="sxs-lookup"><span data-stu-id="bbfee-116">The above change indicates that instead of returned a string, we instead want to use a "View" to generate a result back.</span></span>

<span data-ttu-id="bbfee-117">Teraz dodamy odpowiedni szablon widoku do naszej projektu.</span><span class="sxs-lookup"><span data-stu-id="bbfee-117">We'll now add an appropriate View template to our project.</span></span> <span data-ttu-id="bbfee-118">W tym celu firma Microsoft będzie umieść kursor tekstu w metodzie akcji indeksu, a następnie kliknij prawym przyciskiem myszy i wybierz polecenie "Dodaj widok".</span><span class="sxs-lookup"><span data-stu-id="bbfee-118">To do this we'll position the text cursor within the Index action method, then right-click and select "Add View".</span></span> <span data-ttu-id="bbfee-119">Zostanie wyświetlone okno dialogowe dodawania widoku:</span><span class="sxs-lookup"><span data-stu-id="bbfee-119">This will bring up the Add View dialog:</span></span>

<span data-ttu-id="bbfee-120">![](mvc-music-store-part-3/_static/image1.jpg)![](mvc-music-store-part-3/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="bbfee-120">![](mvc-music-store-part-3/_static/image1.jpg) ![](mvc-music-store-part-3/_static/image1.png)</span></span>

<span data-ttu-id="bbfee-121">Okno dialogowe "Dodaj widok" pozwala szybko i łatwo wygenerować pliki szablonów widoku.</span><span class="sxs-lookup"><span data-stu-id="bbfee-121">The "Add View" dialog allows us to quickly and easily generate View template files.</span></span> <span data-ttu-id="bbfee-122">Domyślnie "Dodaj widok" okna dialogowego wstępnie wypełnia nazwę szablonu widoku do tworzenia, tak aby był zgodny z metody akcji, która będzie go używać.</span><span class="sxs-lookup"><span data-stu-id="bbfee-122">By default the "Add View" dialog pre-populates the name of the View template to create so that it matches the action method that will use it.</span></span> <span data-ttu-id="bbfee-123">Ponieważ użyliśmy menu kontekstowe "Dodaj widok" w metodzie akcji indeks() naszych HomeController powyżej okna dialogowego "Dodaj widok" ma "Index" jako nazwy widoku wstępne wypełnianie domyślnie.</span><span class="sxs-lookup"><span data-stu-id="bbfee-123">Because we used the "Add View" context menu within the Index() action method of our HomeController, the "Add View" dialog above has "Index" as the view name pre-populated by default.</span></span> <span data-ttu-id="bbfee-124">Nie należy zmienić opcje w tym oknie dialogowym, więc kliknij przycisk Dodaj.</span><span class="sxs-lookup"><span data-stu-id="bbfee-124">We don't need to change any of the options on this dialog, so click the Add button.</span></span>

<span data-ttu-id="bbfee-125">Kliknięcie przycisku Dodaj, Visual Web Developer utworzy nowy Index.cshtml, Wyświetl szablon firmie Microsoft w katalogu \Views\Home tworzenia folderu, jeśli jeszcze nie istnieje.</span><span class="sxs-lookup"><span data-stu-id="bbfee-125">When we click the Add button, Visual Web Developer will create a new Index.cshtml view template for us in the \Views\Home directory, creating the folder if doesn't already exist.</span></span>

![](mvc-music-store-part-3/_static/image2.png)

<span data-ttu-id="bbfee-126">Nazwy i lokalizacji folderu pliku "Index.cshtml" jest ważna i następuje domyślnych konwencji nazewnictwa platformy ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="bbfee-126">The name and folder location of the "Index.cshtml" file is important, and follows the default ASP.NET MVC naming conventions.</span></span> <span data-ttu-id="bbfee-127">Nazwa katalogu, \Views\Home, odpowiada kontroler - o nazwie HomeController.</span><span class="sxs-lookup"><span data-stu-id="bbfee-127">The directory name, \Views\Home, matches the controller - which is named HomeController.</span></span> <span data-ttu-id="bbfee-128">Nazwa szablonu widoku indeksu, odpowiada metoda akcji kontrolera, które będą wyświetlane w widoku.</span><span class="sxs-lookup"><span data-stu-id="bbfee-128">The view template name, Index, matches the controller action method which will be displaying the view.</span></span>

<span data-ttu-id="bbfee-129">ASP.NET MVC pozwala uniknąć konieczności jawnego określania nazwy lub lokalizacji szablonu widoku, gdy jest używany do zwrócenia widoku tę konwencję nazewnictwa.</span><span class="sxs-lookup"><span data-stu-id="bbfee-129">ASP.NET MVC allows us to avoid having to explicitly specify the name or location of a view template when we use this naming convention to return a view.</span></span> <span data-ttu-id="bbfee-130">Go będzie domyślnie renderowania szablonu widoku \Views\Home\Index.cshtml pisząc poniższy kod podobnie jak w naszych HomeController:</span><span class="sxs-lookup"><span data-stu-id="bbfee-130">It will by default render the \Views\Home\Index.cshtml view template when we write code like below within our HomeController:</span></span>

[!code-csharp[Main](mvc-music-store-part-3/samples/sample2.cs)]

<span data-ttu-id="bbfee-131">Visual Web Developer utworzony i otworzyć szablon widoku "Index.cshtml", po możemy kliknięty przycisk "Dodaj" w oknie dialogowym "Dodaj widok".</span><span class="sxs-lookup"><span data-stu-id="bbfee-131">Visual Web Developer created and opened the "Index.cshtml" view template after we clicked the "Add" button within the "Add View" dialog.</span></span> <span data-ttu-id="bbfee-132">Poniżej przedstawiono zawartość Index.cshtml.</span><span class="sxs-lookup"><span data-stu-id="bbfee-132">The contents of Index.cshtml are shown below.</span></span>

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample3.cshtml)]

<span data-ttu-id="bbfee-133">Ten widok jest przy użyciu składni Razor, które jest bardziej zwięzłe niż aparat widoku formularzy sieci Web, które są używane w poprzednich wersjach programu ASP.NET MVC i formularzy sieci Web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="bbfee-133">This view is using the Razor syntax, which is more concise than the Web Forms view engine used in ASP.NET Web Forms and previous versions of ASP.NET MVC.</span></span> <span data-ttu-id="bbfee-134">Aparat widoku formularzy sieci Web są nadal dostępne w programie ASP.NET MVC 3, ale wielu deweloperów znaleźć aparatu widoku Razor naprawdę również pasujące rozwoju platformy ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="bbfee-134">The Web Forms view engine is still available in ASP.NET MVC 3, but many developers find that the Razor view engine fits ASP.NET MVC development really well.</span></span>

<span data-ttu-id="bbfee-135">Pierwsze trzy wiersze ustawić przy użyciu ViewBag.Title tytuł strony.</span><span class="sxs-lookup"><span data-stu-id="bbfee-135">The first three lines set the page title using ViewBag.Title.</span></span> <span data-ttu-id="bbfee-136">Firma Microsoft będzie Sprawdź jak to działa bardziej szczegółowo wkrótce, ale najpierw umożliwia aktualizacji tekst nagłówka i Wyświetl stronę.</span><span class="sxs-lookup"><span data-stu-id="bbfee-136">We'll look at how this works in more detail soon, but first let's update the text heading text and view the page.</span></span> <span data-ttu-id="bbfee-137">Aktualizacja &lt;h2&gt; tag znaczy "ten jest strona główna", jak pokazano poniżej.</span><span class="sxs-lookup"><span data-stu-id="bbfee-137">Update the &lt;h2&gt; tag to say "This is the Home Page" as shown below.</span></span>

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample4.cshtml)]

<span data-ttu-id="bbfee-138">Uruchomiona pokazuje aplikacji, że nasze nowy tekst jest widoczna na stronie głównej.</span><span class="sxs-lookup"><span data-stu-id="bbfee-138">Running the application shows that our new text is visible on the home page.</span></span>

![](mvc-music-store-part-3/_static/image3.png)

## <a name="using-a-layout-for-common-site-elements"></a><span data-ttu-id="bbfee-139">Przy użyciu układu dla wspólnych elementów lokacji</span><span class="sxs-lookup"><span data-stu-id="bbfee-139">Using a Layout for common site elements</span></span>

<span data-ttu-id="bbfee-140">Większość witryn sieci Web ma zawartości, które są współużytkowane przez wiele stron: nawigacji stopki, obrazy logo, odwołania do arkusza stylów, itp. Aparat widoku Razor ułatwia to zarządzanie przy użyciu strony o nazwie \_Layout.cshtml, który automatycznie została utworzona dla nas znajdujące się w folderze/widoków/Shared.</span><span class="sxs-lookup"><span data-stu-id="bbfee-140">Most websites have content which is shared between many pages: navigation, footers, logo images, stylesheet references, etc. The Razor view engine makes this easy to manage using a page called \_Layout.cshtml which has automatically been created for us inside the /Views/Shared folder.</span></span>

![](mvc-music-store-part-3/_static/image4.png)

<span data-ttu-id="bbfee-141">Kliknij dwukrotnie ten folder, aby wyświetlić zawartość, które są wyświetlane poniżej.</span><span class="sxs-lookup"><span data-stu-id="bbfee-141">Double-click on this folder to view the contents, which are shown below.</span></span>

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample5.cshtml)]

<span data-ttu-id="bbfee-142">Zawartość z naszych poszczególnych widoków będą wyświetlane przez @RenderBodypolecenia () oraz typowe zawartość, która chcemy się pojawiają się poza tym, które można dodać do \_Layout.cshtml znaczników.</span><span class="sxs-lookup"><span data-stu-id="bbfee-142">The content from our individual views will be displayed by the @RenderBody() command, and any common content that we want to appear outside of that can be added to the \_Layout.cshtml markup.</span></span> <span data-ttu-id="bbfee-143">Chcemy naszego sklepu utworów muzycznych MVC aby wspólnej nagłówek z łączami do naszej obszar strony głównej i zapisać na wszystkich stron w witrynie, więc dodamy który do szablonu bezpośrednio powyżej @RenderBodyinstrukcji ().</span><span class="sxs-lookup"><span data-stu-id="bbfee-143">We'll want our MVC Music Store to have a common header with links to our Home page and Store area on all pages in the site, so we'll add that to the template directly above that @RenderBody() statement.</span></span>

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample6.cshtml)]

## <a name="updating-the-stylesheet"></a><span data-ttu-id="bbfee-144">Aktualizowanie arkusza stylów</span><span class="sxs-lookup"><span data-stu-id="bbfee-144">Updating the StyleSheet</span></span>

<span data-ttu-id="bbfee-145">Pusty szablon projektu zawiera plik CSS bardzo prostsze, zawierający tylko stylów używany do wyświetlania komunikatów dotyczących sprawdzania poprawności.</span><span class="sxs-lookup"><span data-stu-id="bbfee-145">The empty project template includes a very streamlined CSS file which just includes styles used to display validation messages.</span></span> <span data-ttu-id="bbfee-146">Nasze projektanta udostępnił niektóre dodatkowe CSS i obrazy do definiowania wyglądu i działania dla witryny, więc dodamy znajdującymi się teraz.</span><span class="sxs-lookup"><span data-stu-id="bbfee-146">Our designer has provided some additional CSS and images to define the look and feel for our site, so we'll add those in now.</span></span>

<span data-ttu-id="bbfee-147">Zaktualizowany plik CSS i obrazy znajdują się w katalogu zawartości Assets.zip MvcMusicStore, która jest dostępna w [MVC utworów muzycznych magazynu](https://github.com/evilDave/MVC-Music-Store).</span><span class="sxs-lookup"><span data-stu-id="bbfee-147">The updated CSS file and Images are included in the Content directory of MvcMusicStore-Assets.zip which is available at [MVC-Music-Store](https://github.com/evilDave/MVC-Music-Store).</span></span> <span data-ttu-id="bbfee-148">Firma Microsoft zostaną wybrane oba w Eksploratorze Windows i upuść je do folderu zawartości naszych rozwiązań programu Visual Web Developer, jak pokazano poniżej:</span><span class="sxs-lookup"><span data-stu-id="bbfee-148">We'll select both of them in Windows Explorer and drop them into our Solution's Content folder in Visual Web Developer, as shown below:</span></span>

![](mvc-music-store-part-3/_static/image5.png)

<span data-ttu-id="bbfee-149">Zostanie wyświetlony monit o potwierdzenie, czy chcesz zastąpić istniejący plik Site.css.</span><span class="sxs-lookup"><span data-stu-id="bbfee-149">You'll be asked to confirm if you want to overwrite the existing Site.css file.</span></span> <span data-ttu-id="bbfee-150">Kliknij przycisk Tak.</span><span class="sxs-lookup"><span data-stu-id="bbfee-150">Click Yes.</span></span>

![](mvc-music-store-part-3/_static/image6.png)

<span data-ttu-id="bbfee-151">Folder zawartości aplikacji pojawi się w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="bbfee-151">The Content folder of your application will now appear as follows:</span></span>

![](mvc-music-store-part-3/_static/image7.png)

<span data-ttu-id="bbfee-152">Teraz załóżmy Uruchom aplikację i sprawdzić wygląd naszego zmiany na stronie głównej.</span><span class="sxs-lookup"><span data-stu-id="bbfee-152">Now let's run the application and see how our changes look on the Home page.</span></span>

![](mvc-music-store-part-3/_static/image8.png)

- <span data-ttu-id="bbfee-153">Umożliwia przeglądanie, co zostało zmienione: metody akcji indeksu HomeController znaleziono i wyświetlane szablonu \Views\Home\Index.cshtmlView, mimo że naszego kodu nazywany "return View()", ponieważ nasze Wyświetl szablon, a następnie standardowej konwencji nazewnictwa.</span><span class="sxs-lookup"><span data-stu-id="bbfee-153">Let's review what's changed: The HomeController's Index action method found and displayed the \Views\Home\Index.cshtmlView template, even though our code called "return View()", because our View template followed the standard naming convention.</span></span>
- <span data-ttu-id="bbfee-154">Strona główna wyświetla prosty komunikat powitalny, który jest zdefiniowany w szablonie \Views\Home\Index.cshtml widoku.</span><span class="sxs-lookup"><span data-stu-id="bbfee-154">The Home Page is displaying a simple welcome message that is defined within the \Views\Home\Index.cshtml view template.</span></span>
- <span data-ttu-id="bbfee-155">Używa strony głównej naszych \_Layout.cshtml szablonu, co komunikat powitalny jest zawarty w układu standardowe witryny HTML.</span><span class="sxs-lookup"><span data-stu-id="bbfee-155">The Home Page is using our \_Layout.cshtml template, and so the welcome message is contained within the standard site HTML layout.</span></span>

## <a name="using-a-model-to-pass-information-to-our-view"></a><span data-ttu-id="bbfee-156">Przy użyciu modelu do przekazywania informacji do naszej widoku</span><span class="sxs-lookup"><span data-stu-id="bbfee-156">Using a Model to pass information to our View</span></span>

<span data-ttu-id="bbfee-157">Szablon widoku, który wyświetla tylko zapisane na stałe HTML nie będzie bardzo interesujące witryny sieci web.</span><span class="sxs-lookup"><span data-stu-id="bbfee-157">A View template that just displays hardcoded HTML isn't going to make a very interesting web site.</span></span> <span data-ttu-id="bbfee-158">Do tworzenia dynamicznych witryn sieci web, będzie zamiast tego chcemy do przekazywania informacji z naszych akcji kontrolera naszym szablony widoku.</span><span class="sxs-lookup"><span data-stu-id="bbfee-158">To create a dynamic web site, we'll instead want to pass information from our controller actions to our view templates.</span></span>

<span data-ttu-id="bbfee-159">We wzorcu Model-View-Controller określenie modelu odwołuje się do obiektów, które reprezentują danych w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="bbfee-159">In the Model-View-Controller pattern, the term Model refers to objects which represent the data in the application.</span></span> <span data-ttu-id="bbfee-160">Często obiekty modelu odpowiadają tabel bazy danych, ale nie muszą oni.</span><span class="sxs-lookup"><span data-stu-id="bbfee-160">Often, model objects correspond to tables in your database, but they don't have to.</span></span>

<span data-ttu-id="bbfee-161">Metody akcji kontrolera, zwracające element ActionResult można przekazać obiekt modelu do widoku.</span><span class="sxs-lookup"><span data-stu-id="bbfee-161">Controller action methods which return an ActionResult can pass a model object to the view.</span></span> <span data-ttu-id="bbfee-162">Dzięki temu kontroler testu pakietu wszystkie informacje potrzebne do generowania odpowiedzi, a następnie przekazać te informacje do szablonu widoku używany do generowania odpowiednich odpowiedzi HTML.</span><span class="sxs-lookup"><span data-stu-id="bbfee-162">This allows a Controller to cleanly package up all the information needed to generate a response, and then pass this information off to a View template to use to generate the appropriate HTML response.</span></span> <span data-ttu-id="bbfee-163">To jest łatwiej zrozumieć, sprawdzając w akcji, więc Zacznijmy od początku.</span><span class="sxs-lookup"><span data-stu-id="bbfee-163">This is easiest to understand by seeing it in action, so let's get started.</span></span>

<span data-ttu-id="bbfee-164">Najpierw utworzymy niektóre klasy modelu do reprezentowania Genres i albumów w naszym magazynie.</span><span class="sxs-lookup"><span data-stu-id="bbfee-164">First we'll create some Model classes to represent Genres and Albums within our store.</span></span> <span data-ttu-id="bbfee-165">Zacznijmy od utworzenia klasy rodzaju.</span><span class="sxs-lookup"><span data-stu-id="bbfee-165">Let's start by creating a Genre class.</span></span> <span data-ttu-id="bbfee-166">Kliknij prawym przyciskiem myszy folder "Modele" w projekcie, wybierz opcję "Dodaj klasę" i nazwę pliku "Genre.cs".</span><span class="sxs-lookup"><span data-stu-id="bbfee-166">Right-click the "Models" folder within your project, choose the "Add Class" option, and name the file "Genre.cs".</span></span>

![](mvc-music-store-part-3/_static/image2.jpg)

![](mvc-music-store-part-3/_static/image9.png)

<span data-ttu-id="bbfee-167">Następnie dodaj ciąg publicznego nazwa właściwości do klasy, która została utworzona:</span><span class="sxs-lookup"><span data-stu-id="bbfee-167">Then add a public string Name property to the class that was created:</span></span>

[!code-csharp[Main](mvc-music-store-part-3/samples/sample7.cs)]

<span data-ttu-id="bbfee-168">*Uwaga: W przypadku, że zastanawiasz się {get; Ustaw;} notacji jest wprowadzenie funkcji C# dla właściwości zaimplementowane automatycznie. Dzięki temu nam korzyści wynikające z właściwością bez konieczności nam zadeklarować polem zapasowym.*</span><span class="sxs-lookup"><span data-stu-id="bbfee-168">*Note: In case you're wondering, the { get; set; } notation is making use of C#'s auto-implemented properties feature. This gives us the benefits of a property without requiring us to declare a backing field.*</span></span>

<span data-ttu-id="bbfee-169">Następnie wykonaj te same czynności, aby utworzyć klasę albumu (o nazwie Album.cs), tytuł i właściwości Genre:</span><span class="sxs-lookup"><span data-stu-id="bbfee-169">Next, follow the same steps to create an Album class (named Album.cs) that has a Title and a Genre property:</span></span>

[!code-csharp[Main](mvc-music-store-part-3/samples/sample8.cs)]

<span data-ttu-id="bbfee-170">Teraz możemy zmodyfikować StoreController, aby móc używać widoków, które zawierają informacje o dynamicznej z modelu.</span><span class="sxs-lookup"><span data-stu-id="bbfee-170">Now we can modify the StoreController to use Views which display dynamic information from our Model.</span></span> <span data-ttu-id="bbfee-171">Jeśli - dla celów demonstracyjnych teraz — nasze konto nazywa się naszego albumów na podstawie Identyfikatora żądania, można wyświetlić te informacje, jak w poniższym widoku.</span><span class="sxs-lookup"><span data-stu-id="bbfee-171">If - for demonstration purposes right now - we named our Albums based on the request ID, we could display that information as in the view below.</span></span>

![](mvc-music-store-part-3/_static/image10.png)

<span data-ttu-id="bbfee-172">Zaczniemy przez zmianę akcji szczegóły magazynu, dzięki czemu zawiera informacje dotyczące jednego albumu.</span><span class="sxs-lookup"><span data-stu-id="bbfee-172">We'll start by changing the Store Details action so it shows the information for a single album.</span></span> <span data-ttu-id="bbfee-173">Dodaj instrukcję "przy użyciu" na początku **StoreControllers** klasy do uwzględnienia MvcMusicStore.Models przestrzeni nazw, dlatego nie należy do typu MvcMusicStore.Models.Album za każdym razem, gdy chcemy użyć klasy albumu.</span><span class="sxs-lookup"><span data-stu-id="bbfee-173">Add a "using" statement to the top of the **StoreControllers** class to include the MvcMusicStore.Models namespace, so we don't need to type MvcMusicStore.Models.Album every time we want to use the album class.</span></span> <span data-ttu-id="bbfee-174">"Deklaracje Using" części tej klasy, powinien zostać wyświetlony jak poniżej.</span><span class="sxs-lookup"><span data-stu-id="bbfee-174">The "usings" section of that class should now appear as below.</span></span>

[!code-csharp[Main](mvc-music-store-part-3/samples/sample9.cs)]

<span data-ttu-id="bbfee-175">Następnie będziemy informować szczegóły akcji kontrolera, aby zwraca element ActionResult, a nie typu string, jak robiliśmy metodą HomeController indeksu.</span><span class="sxs-lookup"><span data-stu-id="bbfee-175">Next, we'll update the Details controller action so that it returns an ActionResult rather than a string, as we did with the HomeController's Index method.</span></span>

[!code-csharp[Main](mvc-music-store-part-3/samples/sample10.cs)]

<span data-ttu-id="bbfee-176">Teraz możemy zmodyfikować logiki, aby powrócić do widoku obiektu albumu.</span><span class="sxs-lookup"><span data-stu-id="bbfee-176">Now we can modify the logic to return an Album object to the view.</span></span> <span data-ttu-id="bbfee-177">W dalszej części tego samouczka firma Microsoft będzie można pobierania danych z bazy danych — ale dla teraz używamy "fikcyjny danych" Aby rozpocząć pracę.</span><span class="sxs-lookup"><span data-stu-id="bbfee-177">Later in this tutorial we will be retrieving the data from a database – but for right now we will use "dummy data" to get started.</span></span>

[!code-csharp[Main](mvc-music-store-part-3/samples/sample11.cs)]

<span data-ttu-id="bbfee-178">*Uwaga: Jeśli znasz C#, możesz może założyć, że przy użyciu var oznacza, że nasze zmiennej albumu późnym wiązaniem. To nie jest prawidłowe — kompilatora C# używa wnioskowanie na podstawie co możemy jest przypisanie do zmiennej do określenia tej album jest typu albumu i kompilowanie zmiennej lokalnej albumu jako typ albumy, uzyskujemy sprawdzanie kompilacji i edytora kodu programu Visual Studio Obsługa.*</span><span class="sxs-lookup"><span data-stu-id="bbfee-178">*Note: If you're unfamiliar with C#, you may assume that using var means that our album variable is late-bound. That's not correct – the C# compiler is using type-inference based on what we're assigning to the variable to determine that album is of type Album and compiling the local album variable as an Album type, so we get compile-time checking and Visual Studio code-editor support.*</span></span>

<span data-ttu-id="bbfee-179">Teraz Utwórzmy szablonu widoku, który korzysta z naszych albumu do generowania odpowiedzi HTML.</span><span class="sxs-lookup"><span data-stu-id="bbfee-179">Let's now create a View template that uses our Album to generate an HTML response.</span></span> <span data-ttu-id="bbfee-180">Zanim przejdziemy należy skompilować projekt, dzięki czemu okno dialogowe dodawania widoku zna naszych nowo utworzonej klasy albumu.</span><span class="sxs-lookup"><span data-stu-id="bbfee-180">Before we do that we need to build the project so that the Add View dialog knows about our newly created Album class.</span></span> <span data-ttu-id="bbfee-181">Można utworzyć projekt, wybierając Debug⇨Build MvcMusicStore elementu menu (dla dodatkowe środki, można użyć skrótu Ctrl-Shift-B Aby skompilować projekt).</span><span class="sxs-lookup"><span data-stu-id="bbfee-181">You can build the project by selecting the Debug⇨Build MvcMusicStore menu item (for extra credit, you can use the Ctrl-Shift-B shortcut to build the project).</span></span>

![](mvc-music-store-part-3/_static/image11.png)

<span data-ttu-id="bbfee-182">Teraz, gdy skonfigurowaliśmy naszej klasy obsługi już wszystko gotowe do tworzenia szablonu naszych widoku.</span><span class="sxs-lookup"><span data-stu-id="bbfee-182">Now that we've set up our supporting classes, we're ready to build our View template.</span></span> <span data-ttu-id="bbfee-183">Kliknij prawym przyciskiem myszy w metodzie szczegółowe informacje i wybierz opcję "Dodaj widok..."</span><span class="sxs-lookup"><span data-stu-id="bbfee-183">Right-click within the Details method and select "Add View…"</span></span> <span data-ttu-id="bbfee-184">z menu kontekstowego.</span><span class="sxs-lookup"><span data-stu-id="bbfee-184">from the context menu.</span></span>

![](mvc-music-store-part-3/_static/image12.png)

<span data-ttu-id="bbfee-185">Zamierzamy utworzyć nowy szablon widoku jak robiliśmy przed z HomeController.</span><span class="sxs-lookup"><span data-stu-id="bbfee-185">We are going to create a new View template like we did before with the HomeController.</span></span> <span data-ttu-id="bbfee-186">Ponieważ firma Microsoft tworzysz z StoreController go domyślnie generowania pliku \Views\Store\Index.cshtml.</span><span class="sxs-lookup"><span data-stu-id="bbfee-186">Because we are creating it from the StoreController it will by default be generated in a \Views\Store\Index.cshtml file.</span></span>

<span data-ttu-id="bbfee-187">W odróżnieniu od przed, zamierzamy wyboru "Utwórz silnie typizowanego" w widoku.</span><span class="sxs-lookup"><span data-stu-id="bbfee-187">Unlike before, we are going to check the "Create a strongly-typed" view checkbox.</span></span> <span data-ttu-id="bbfee-188">Następnie zamierzamy Wybierz klasy Nasze "Album" w "Klasy danych widoku" upuszczania downlist.</span><span class="sxs-lookup"><span data-stu-id="bbfee-188">We are then going to select our "Album" class within the "View data-class" drop-downlist.</span></span> <span data-ttu-id="bbfee-189">To spowoduje, że okno dialogowe "Dodaj widok", aby utworzyć szablon widoku, który oczekuje, że Album obiektu zostaną przekazane do go do użycia.</span><span class="sxs-lookup"><span data-stu-id="bbfee-189">This will cause the "Add View" dialog to create a View template that expects that an Album object will be passed to it to use.</span></span>

![](mvc-music-store-part-3/_static/image13.png)

<span data-ttu-id="bbfee-190">Po kliknięciu przycisku przycisk "Dodaj" naszych \Views\Store\Details.cshtml widok będzie można utworzyć szablonu, zawierający następujący kod.</span><span class="sxs-lookup"><span data-stu-id="bbfee-190">When we click the "Add" button our \Views\Store\Details.cshtml View template will be created, containing the following code.</span></span>

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample12.cshtml)]

<span data-ttu-id="bbfee-191">Zwróć uwagę, pierwszy wiersz, co oznacza, że widok jest silnie typizowaną do klasy Nasze albumu.</span><span class="sxs-lookup"><span data-stu-id="bbfee-191">Notice the first line, which indicates that this view is strongly-typed to our Album class.</span></span> <span data-ttu-id="bbfee-192">Aparat widoku Razor rozumie, że jej został przekazany obiekt albumu możemy łatwo uzyskiwać dostęp do właściwości modelu, a nawet są korzyści IntelliSense w edytorze programu Visual Web Developer.</span><span class="sxs-lookup"><span data-stu-id="bbfee-192">The Razor view engine understands that it has been passed an Album object, so we can easily access model properties and even have the benefit of IntelliSense in the Visual Web Developer editor.</span></span>

<span data-ttu-id="bbfee-193">Aktualizacja &lt;h2&gt; tagów wyświetla przez zmodyfikowanie tego wiersza, aby wyglądają następująco albumu właściwości Title więc.</span><span class="sxs-lookup"><span data-stu-id="bbfee-193">Update the &lt;h2&gt; tag so it displays the Album's Title property by modifying that line to appear as follows.</span></span>

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample13.cshtml)]

<span data-ttu-id="bbfee-194">Zwróć uwagę, że IntelliSense jest wyzwalane po wprowadzeniu okres po @Model — słowo kluczowe, przedstawiający właściwości i metody, które obsługuje klasy albumu.</span><span class="sxs-lookup"><span data-stu-id="bbfee-194">Notice that IntelliSense is triggered when you enter the period after the @Model keyword, showing the properties and methods that the Album class supports.</span></span>

<span data-ttu-id="bbfee-195">Umożliwia teraz ponownie uruchomić naszych projektu i odwiedź adres URL Sklepu/szczegóły/5.</span><span class="sxs-lookup"><span data-stu-id="bbfee-195">Let's now re-run our project and visit the /Store/Details/5 URL.</span></span> <span data-ttu-id="bbfee-196">Firma Microsoft zostanie wyświetlone szczegóły albumu podobnie jak poniżej.</span><span class="sxs-lookup"><span data-stu-id="bbfee-196">We'll see details of an Album like below.</span></span>

![](mvc-music-store-part-3/_static/image14.png)

<span data-ttu-id="bbfee-197">Teraz wybierzemy podobne aktualizacji dla metody akcji Przeglądaj magazynu.</span><span class="sxs-lookup"><span data-stu-id="bbfee-197">Now we'll make a similar update for the Store Browse action method.</span></span> <span data-ttu-id="bbfee-198">Zaktualizuj metodę, tak aby zwracało element ActionResult i zmodyfikować logikę metody, aby tworzy nowy obiekt Genre i zwraca go do widoku.</span><span class="sxs-lookup"><span data-stu-id="bbfee-198">Update the method so it returns an ActionResult, and modify the method logic so it creates a new Genre object and returns it to the View.</span></span>

[!code-csharp[Main](mvc-music-store-part-3/samples/sample14.cs)]

<span data-ttu-id="bbfee-199">Kliknij prawym przyciskiem myszy w metodzie Przeglądaj i wybierz opcję "Dodaj widok..."</span><span class="sxs-lookup"><span data-stu-id="bbfee-199">Right-click in the Browse method and select "Add View…"</span></span> <span data-ttu-id="bbfee-200">z menu kontekstowego, a następnie Dodaj widok, który jest jednoznacznie Dodaj silnie typizowaną do klasy Genre.</span><span class="sxs-lookup"><span data-stu-id="bbfee-200">from the context menu, then add a View that is strongly-typed add a strongly typed to the Genre class.</span></span>

![](mvc-music-store-part-3/_static/image15.png)

<span data-ttu-id="bbfee-201">Aktualizacja &lt;h2&gt; elementu w widoku kodu (w /Views/Store/Browse.cshtml) do wyświetlenia informacji o rodzaju.</span><span class="sxs-lookup"><span data-stu-id="bbfee-201">Update the &lt;h2&gt; element in the view code (in /Views/Store/Browse.cshtml) to display the Genre information.</span></span>

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample15.cshtml)]

<span data-ttu-id="bbfee-202">Teraz załóżmy Uruchom ponownie naszych projektu i przejdź do/magazynu/Przeglądaj? Genre = Disco adresu URL.</span><span class="sxs-lookup"><span data-stu-id="bbfee-202">Now let's re-run our project and browse to the /Store/Browse?Genre=Disco URL.</span></span> <span data-ttu-id="bbfee-203">Firma Microsoft będzie wyświetlona strona przeglądania wyświetlony podobnie jak poniżej.</span><span class="sxs-lookup"><span data-stu-id="bbfee-203">We'll see the Browse page displayed like below.</span></span>

![](mvc-music-store-part-3/_static/image16.png)

<span data-ttu-id="bbfee-204">Na koniec upewnijmy nieco bardziej skomplikowane aktualizacji do **przechowywania indeksu** metody akcji i widok, aby wyświetlić listę wszystkich gatunkami muzyki w naszym magazynie.</span><span class="sxs-lookup"><span data-stu-id="bbfee-204">Finally, let's make a slightly more complex update to the **Store Index** action method and view to display a list of all the Genres in our store.</span></span> <span data-ttu-id="bbfee-205">Firma Microsoft to zrobić przy użyciu listy Genres naszych obiektu modelu, a nie tylko jednego rodzaju.</span><span class="sxs-lookup"><span data-stu-id="bbfee-205">We'll do that by using a List of Genres as our model object, rather than just a single Genre.</span></span>

[!code-csharp[Main](mvc-music-store-part-3/samples/sample16.cs)]

<span data-ttu-id="bbfee-206">Kliknij prawym przyciskiem myszy w metodzie akcji indeksu magazynu i wybierz opcję Dodaj widok, wybierz Genre jako klasa modelu, a następnie naciśnij przycisk Dodaj.</span><span class="sxs-lookup"><span data-stu-id="bbfee-206">Right-click in the Store Index action method and select Add View as before, select Genre as the Model class, and press the Add button.</span></span>

![](mvc-music-store-part-3/_static/image17.png)

<span data-ttu-id="bbfee-207">Najpierw zmienimy @model deklaracji, aby określić, czy widok będzie oczekiwano Genre kilka obiektów zamiast jeden z nich.</span><span class="sxs-lookup"><span data-stu-id="bbfee-207">First we'll change the @model declaration to indicate that the view will be expecting several Genre objects rather than just one.</span></span> <span data-ttu-id="bbfee-208">Zmiana w pierwszym wierszu /Store/Index.cshtml do odczytu w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="bbfee-208">Change the first line of /Store/Index.cshtml to read as follows:</span></span>

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample17.cshtml)]

<span data-ttu-id="bbfee-209">Ta wartość informuje aparatu widoku Razor, który będzie działać z obiektu modelu, który może posiadać kilka obiektów rodzaju.</span><span class="sxs-lookup"><span data-stu-id="bbfee-209">This tells the Razor view engine that it will be working with a model object that can hold several Genre objects.</span></span> <span data-ttu-id="bbfee-210">Firma Microsoft korzysta z interfejsu IEnumerable&lt;Genre&gt; zamiast listy&lt;Genre&gt; ponieważ jest bardziej ogólnym, pozwalając nam można zmienić później naszych typ modelu do dowolnego typu obiektu, który obsługuje interfejsu IEnumerable.</span><span class="sxs-lookup"><span data-stu-id="bbfee-210">We're using an IEnumerable&lt;Genre&gt; rather than a List&lt;Genre&gt; since it's more generic, allowing us to change our model type later to any object type that supports the IEnumerable interface.</span></span>

<span data-ttu-id="bbfee-211">Następnie będzie Pętla przez obiekty Genre w modelu, jak pokazano w poniższym kodzie ukończone.</span><span class="sxs-lookup"><span data-stu-id="bbfee-211">Next, we'll loop through the Genre objects in the model as shown in the completed view code below.</span></span>

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample18.cshtml)]

<span data-ttu-id="bbfee-212">Powiadomienie, że mamy pełną obsługę funkcji IntelliSense jak możemy wprowadź ten kod tak, aby podczas wpisywania możemy "@Model."</span><span class="sxs-lookup"><span data-stu-id="bbfee-212">Notice that we have full IntelliSense support as we enter this code, so that when we type "@Model."</span></span> <span data-ttu-id="bbfee-213">przedstawia wszystkie metody i właściwości obsługiwane przez interfejs IEnumerable typu rodzaju.</span><span class="sxs-lookup"><span data-stu-id="bbfee-213">we see all methods and properties supported by an IEnumerable of type Genre.</span></span>

![](mvc-music-store-part-3/_static/image18.png)

<span data-ttu-id="bbfee-214">W naszym pętli "foreach" Visual Web Developer wie, że każdy element jest typu Genre, tak widzimy IntelliSence dla każdego typu Genre.</span><span class="sxs-lookup"><span data-stu-id="bbfee-214">Within our "foreach" loop, Visual Web Developer knows that each item is of type Genre, so we see IntelliSence for each the Genre type.</span></span>

![](mvc-music-store-part-3/_static/image19.png)

<span data-ttu-id="bbfee-215">Następnie funkcja szkieletów zbadane obiektu Genre i określić, że będzie mieć właściwości Name, pętli i dokonuje zapisu. Generuje on edycji, szczegóły i Usuń linki do każdego elementu.</span><span class="sxs-lookup"><span data-stu-id="bbfee-215">Next, the scaffolding feature examined the Genre object and determined that each will have a Name property, so it loops through and writes them out. It also generates Edit, Details, and Delete links to each individual item.</span></span> <span data-ttu-id="bbfee-216">Firma Microsoft będzie korzystać tego później w naszym Menedżer magazynu, ale teraz chcemy mieć prostą listę zamiast tego.</span><span class="sxs-lookup"><span data-stu-id="bbfee-216">We'll take advantage of that later in our store manager, but for now we'd like to have a simple list instead.</span></span>

<span data-ttu-id="bbfee-217">Gdy firma Microsoft może uruchomić aplikację, a następnie przejdź do/Store, widzimy liczbę i listy gatunkami muzyki jest wyświetlany.</span><span class="sxs-lookup"><span data-stu-id="bbfee-217">When we run the application and browse to /Store, we see that both the count and list of Genres is displayed.</span></span>

![](mvc-music-store-part-3/_static/image20.png)

## <a name="adding-links-between-pages"></a><span data-ttu-id="bbfee-218">Dodawanie łączy między stronami</span><span class="sxs-lookup"><span data-stu-id="bbfee-218">Adding Links between pages</span></span>

<span data-ttu-id="bbfee-219">Nasze adresu URL/Store obecnie wymieniono Genres lista nazw Genre po prostu jako zwykły tekst.</span><span class="sxs-lookup"><span data-stu-id="bbfee-219">Our /Store URL that lists Genres currently lists the Genre names simply as plain text.</span></span> <span data-ttu-id="bbfee-220">Zmieńmy to tak, aby zamiast zwykłego tekstu mamy zamiast tego rodzaju nazwy link odpowiedni adres URL Sklepu/Przeglądaj, dzięki czemu kliknięcie określonego rodzaju muzyki, takie jak "Disco" przejdzie do/magazynu/Przeglądaj? genre = Disco adresu URL.</span><span class="sxs-lookup"><span data-stu-id="bbfee-220">Let's change this so that instead of plain text we instead have the Genre names link to the appropriate /Store/Browse URL, so that clicking on a music genre like "Disco" will navigate to the /Store/Browse?genre=Disco URL.</span></span> <span data-ttu-id="bbfee-221">Firma Microsoft można zaktualizować szablon widoku naszych \Views\Store\Index.cshtml te linki przy użyciu kodu podobnie jak poniżej w danych wyjściowych **(nie wpisz to w — chcemy poprawić na nim)**:</span><span class="sxs-lookup"><span data-stu-id="bbfee-221">We could update our \Views\Store\Index.cshtml View template to output these links using code like below **(don't type this in - we're going to improve on it)**:</span></span>

[!code-html[Main](mvc-music-store-part-3/samples/sample19.html)]

<span data-ttu-id="bbfee-222">Działający, ale połączenie może prowadzić do problemów później, ponieważ zależy od ciągu zapisane na stałe.</span><span class="sxs-lookup"><span data-stu-id="bbfee-222">That works, but it could lead to trouble later since it relies on a hardcoded string.</span></span> <span data-ttu-id="bbfee-223">Na przykład jeśli trzeba zmienić nazwę kontrolera, czy musimy wyszukiwania za pomocą naszego kodu wyszukiwanie linki, które muszą zostać zaktualizowane.</span><span class="sxs-lookup"><span data-stu-id="bbfee-223">For instance, if we wanted to rename the Controller, we'd need to search through our code looking for links that need to be updated.</span></span>

<span data-ttu-id="bbfee-224">Alternatywne podejście, które możemy użyć ma korzystać z metody pomocnika kodu HTML.</span><span class="sxs-lookup"><span data-stu-id="bbfee-224">An alternative approach we can use is to take advantage of an HTML Helper method.</span></span> <span data-ttu-id="bbfee-225">Platforma ASP.NET MVC zawiera metody pomocnika kodu HTML, które są dostępne z naszego kodu szablonu widoku do wykonywania różnych zadań, tak jak to.</span><span class="sxs-lookup"><span data-stu-id="bbfee-225">ASP.NET MVC includes HTML Helper methods which are available from our View template code to perform a variety of common tasks just like this.</span></span> <span data-ttu-id="bbfee-226">Metoda pomocnika Html.ActionLink() jest szczególnie przydatne i ułatwia tworzenie HTML &lt;&gt; łączy i zajmuje się irytujące szczegóły, takie jak upewnić ścieżki adresu URL jest poprawnie zakodowane w adresie URL.</span><span class="sxs-lookup"><span data-stu-id="bbfee-226">The Html.ActionLink() helper method is a particularly useful one, and makes it easy to build HTML &lt;a&gt; links and takes care of annoying details like making sure URL paths are properly URL encoded.</span></span>

<span data-ttu-id="bbfee-227">Html.ActionLink() ma kilka przeciążeń różnych umożliwiają określenie tylu informacji potrzebnych dla łącza.</span><span class="sxs-lookup"><span data-stu-id="bbfee-227">Html.ActionLink() has several different overloads to allow specifying as much information as you need for your links.</span></span> <span data-ttu-id="bbfee-228">Ogólnie rzecz biorąc musisz wpisać tylko tekst łącza, a także metoda akcji nastąpić przejście po kliknięciu hiperlinku na kliencie.</span><span class="sxs-lookup"><span data-stu-id="bbfee-228">In the simplest case, you'll supply just the link text and the Action method to go to when the hyperlink is clicked on the client.</span></span> <span data-ttu-id="bbfee-229">Na przykład firma Microsoft można połączyć się z "/ magazynu /" indeks() metody na stronie Szczegóły magazynu z tekst łącza "Przejdź do magazynu indeksu" przy użyciu następujące wywołanie:</span><span class="sxs-lookup"><span data-stu-id="bbfee-229">For example, we can link to "/Store/" Index() method on the Store Details page with the link text "Go to the Store Index" using the following call:</span></span>

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample20.cshtml)]

<span data-ttu-id="bbfee-230">*Uwaga: W tym przypadku nie należy określać nazwy kontrolera, ponieważ firma Microsoft łączona tylko innego działania w ramach tego samego kontrolera, który renderowania bieżącego widoku.*</span><span class="sxs-lookup"><span data-stu-id="bbfee-230">*Note: In this case, we didn't need to specify the controller name because we're just linking to another action within the same controller that's rendering the current view.*</span></span>

<span data-ttu-id="bbfee-231">Nasze łącza do strony przeglądania należy przekazać parametr, jednak dlatego użyjemy innego przeciążenia metody Html.ActionLink, który przyjmuje trzy parametry:</span><span class="sxs-lookup"><span data-stu-id="bbfee-231">Our links to the Browse page will need to pass a parameter, though, so we'll use another overload of the Html.ActionLink method that takes three parameters:</span></span>

- 1. <span data-ttu-id="bbfee-232">Tekst łącza, która będzie wyświetlana nazwa Genre</span><span class="sxs-lookup"><span data-stu-id="bbfee-232">Link text, which will display the Genre name</span></span>
- 2. <span data-ttu-id="bbfee-233">Nazwa akcji kontrolera (Przeglądaj)</span><span class="sxs-lookup"><span data-stu-id="bbfee-233">Controller action name (Browse)</span></span>
- 3. <span data-ttu-id="bbfee-234">Wartości parametrów trasy, określając zarówno nazwę (Genre), jak i wartości (nazwa Genre)</span><span class="sxs-lookup"><span data-stu-id="bbfee-234">Route parameter values, specifying both the name (Genre) and the value (Genre name)</span></span>

<span data-ttu-id="bbfee-235">Umieszczanie, że wszystkich elementów, Oto jak możemy przedstawiono tworzenie łączy do widoku indeksu magazynu:</span><span class="sxs-lookup"><span data-stu-id="bbfee-235">Putting that all together, here's how we'll write those links to the Store Index view:</span></span>

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample21.cshtml)]

<span data-ttu-id="bbfee-236">Teraz możemy ponownie uruchom naszych projektu i uzyskać dostępu do adresu URL /Store/ możemy zostanie wyświetlona lista gatunkami muzyki.</span><span class="sxs-lookup"><span data-stu-id="bbfee-236">Now when we run our project again and access the /Store/ URL we will see a list of genres.</span></span> <span data-ttu-id="bbfee-237">Każdego rodzaju jest hiperłącze — po kliknięciu potrwa nam naszym magazynie/Przeglądaj? genre =*[genre]* adresu URL.</span><span class="sxs-lookup"><span data-stu-id="bbfee-237">Each genre is a hyperlink – when clicked it will take us to our /Store/Browse?genre=*[genre]* URL.</span></span>

![](mvc-music-store-part-3/_static/image3.jpg)

<span data-ttu-id="bbfee-238">Kod HTML pod kątem listy genre wygląda następująco:</span><span class="sxs-lookup"><span data-stu-id="bbfee-238">The HTML for the genre list looks like this:</span></span>

[!code-html[Main](mvc-music-store-part-3/samples/sample22.html)]


> [!div class="step-by-step"]
> <span data-ttu-id="bbfee-239">[Poprzednie](mvc-music-store-part-2.md)
> [dalej](mvc-music-store-part-4.md)</span><span class="sxs-lookup"><span data-stu-id="bbfee-239">[Previous](mvc-music-store-part-2.md)
[Next](mvc-music-store-part-4.md)</span></span>
