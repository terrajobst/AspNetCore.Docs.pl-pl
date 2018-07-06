---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-3
title: 'Część 3: Widoki i modele widoków | Dokumentacja firmy Microsoft'
author: jongalloway
description: W tej serii samouczków szczegółowo opisuje wszystkie etapy, tworzenie przykładowej aplikacji platformy ASP.NET MVC Music Store. Część 3 obejmuje, widoki i modele widoków.
ms.author: aspnetcontent
ms.date: 04/21/2011
ms.assetid: 94297aa0-1f2d-4d72-bbcb-63f64653e0c0
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-3
msc.type: authoredcontent
ms.openlocfilehash: 8fd89c2a448877bf13a7828f545ffcd400f63bb1
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/05/2018
ms.locfileid: "37837413"
---
<a name="part-3-views-and-viewmodels"></a><span data-ttu-id="8f841-104">Część 3: Widoki i modele widoków</span><span class="sxs-lookup"><span data-stu-id="8f841-104">Part 3: Views and ViewModels</span></span>
====================
<span data-ttu-id="8f841-105">przez [Galloway'em Jon](https://github.com/jongalloway)</span><span class="sxs-lookup"><span data-stu-id="8f841-105">by [Jon Galloway](https://github.com/jongalloway)</span></span>

> <span data-ttu-id="8f841-106">MVC Music Store jest aplikacją z samouczka, który wprowadzono i opisano krok po kroku, jak używać platformy ASP.NET MVC i programu Visual Studio do tworzenia aplikacji internetowych.</span><span class="sxs-lookup"><span data-stu-id="8f841-106">The MVC Music Store is a tutorial application that introduces and explains step-by-step how to use ASP.NET MVC and Visual Studio for web development.</span></span>  
>   
> <span data-ttu-id="8f841-107">MVC Music Store jest uproszczone przykładową implementację magazynu sprzedaje utworów muzycznych albumy online, która implementuje podstawowej witryny administracji, logowania użytkownika i funkcje koszyka zakupów.</span><span class="sxs-lookup"><span data-stu-id="8f841-107">The MVC Music Store is a lightweight sample store implementation which sells music albums online, and implements basic site administration, user sign-in, and shopping cart functionality.</span></span>  
>   
> <span data-ttu-id="8f841-108">W tej serii samouczków szczegółowo opisuje wszystkie etapy, tworzenie przykładowej aplikacji platformy ASP.NET MVC Music Store.</span><span class="sxs-lookup"><span data-stu-id="8f841-108">This tutorial series details all of the steps taken to build the ASP.NET MVC Music Store sample application.</span></span> <span data-ttu-id="8f841-109">Część 3 obejmuje, widoki i modele widoków.</span><span class="sxs-lookup"><span data-stu-id="8f841-109">Part 3 covers Views and ViewModels.</span></span>


<span data-ttu-id="8f841-110">Do tej pory firma Microsoft została właśnie zostało zwracanie ciągi z akcji kontrolera.</span><span class="sxs-lookup"><span data-stu-id="8f841-110">So far we've just been returning strings from controller actions.</span></span> <span data-ttu-id="8f841-111">Jest to dobre rozwiązanie sposób poznać działanie kontrolerów, ale to, że nie będzie sposób tworzenia aplikacji sieci web rzeczywistych.</span><span class="sxs-lookup"><span data-stu-id="8f841-111">That's a nice way to get an idea of how controllers work, but it's not how you'd want to build a real web application.</span></span> <span data-ttu-id="8f841-112">Firma Microsoft zamierza mają lepszy sposób generują kod HTML do przeglądarki, odwiedź naszą witrynę — jeden w którym firma Microsoft może korzystać z plików szablonu można łatwo dostosować zawartość HTML przesyła z powrotem.</span><span class="sxs-lookup"><span data-stu-id="8f841-112">We are going to want a better way to generate HTML back to browsers visiting our site – one where we can use template files to more easily customize the HTML content send back.</span></span> <span data-ttu-id="8f841-113">To dokładnie wykonaj widoków.</span><span class="sxs-lookup"><span data-stu-id="8f841-113">That's exactly what Views do.</span></span>

## <a name="adding-a-view-template"></a><span data-ttu-id="8f841-114">Dodawanie szablonu widoku</span><span class="sxs-lookup"><span data-stu-id="8f841-114">Adding a View template</span></span>

<span data-ttu-id="8f841-115">Aby użyć szablonu widoku, firma Microsoft będzie zmienić metodę indeksu HomeController zwracać element ActionResult i jego zwracają View(), takich jak poniżej:</span><span class="sxs-lookup"><span data-stu-id="8f841-115">To use a view-template, we'll change the HomeController Index method to return an ActionResult, and have it return View(), like below:</span></span>

[!code-csharp[Main](mvc-music-store-part-3/samples/sample1.cs)]

<span data-ttu-id="8f841-116">Powyższych zmian wskazuje na to, a nie zwrócił ciągu, chcemy zamiast tego użyj "View", aby wygenerować ponownie wynik.</span><span class="sxs-lookup"><span data-stu-id="8f841-116">The above change indicates that instead of returned a string, we instead want to use a "View" to generate a result back.</span></span>

<span data-ttu-id="8f841-117">Teraz dodamy odpowiedni szablon widoku nasze projektu.</span><span class="sxs-lookup"><span data-stu-id="8f841-117">We'll now add an appropriate View template to our project.</span></span> <span data-ttu-id="8f841-118">W tym celu firma Microsoft będzie umieść kursor tekstu w metodzie akcji indeksu, a następnie kliknij prawym przyciskiem myszy i wybierz pozycję "Dodaj widok".</span><span class="sxs-lookup"><span data-stu-id="8f841-118">To do this we'll position the text cursor within the Index action method, then right-click and select "Add View".</span></span> <span data-ttu-id="8f841-119">Zostanie wyświetlone okno dialogowe dodawania widoku:</span><span class="sxs-lookup"><span data-stu-id="8f841-119">This will bring up the Add View dialog:</span></span>

<span data-ttu-id="8f841-120">![](mvc-music-store-part-3/_static/image1.jpg)![](mvc-music-store-part-3/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="8f841-120">![](mvc-music-store-part-3/_static/image1.jpg) ![](mvc-music-store-part-3/_static/image1.png)</span></span>

<span data-ttu-id="8f841-121">Okno dialogowe "Dodaj widok" pozwala szybko i łatwo wygenerować plik szablonu widoku.</span><span class="sxs-lookup"><span data-stu-id="8f841-121">The "Add View" dialog allows us to quickly and easily generate View template files.</span></span> <span data-ttu-id="8f841-122">Domyślnie "Dodaj widok" okno dialogowe wstępnie wypełnia nazwę Wyświetl szablon do tworzenia, tak aby była zgodna z metody akcji, która będzie go używać.</span><span class="sxs-lookup"><span data-stu-id="8f841-122">By default the "Add View" dialog pre-populates the name of the View template to create so that it matches the action method that will use it.</span></span> <span data-ttu-id="8f841-123">Ponieważ użyliśmy menu kontekstowego "Dodaj widok" w ramach metody akcji indeks() naszych HomeController, okno dialogowe "View Dodaj" powyżej ma "Index" jako nazwy widoku wstępnie wypełnione domyślnie.</span><span class="sxs-lookup"><span data-stu-id="8f841-123">Because we used the "Add View" context menu within the Index() action method of our HomeController, the "Add View" dialog above has "Index" as the view name pre-populated by default.</span></span> <span data-ttu-id="8f841-124">Nie potrzebujemy zmienić dowolne z opcjami w tym oknie dialogowym, więc kliknij przycisk Dodaj.</span><span class="sxs-lookup"><span data-stu-id="8f841-124">We don't need to change any of the options on this dialog, so click the Add button.</span></span>

<span data-ttu-id="8f841-125">Gdy firma Microsoft kliknij przycisk Dodaj, Visual Web Developer utworzy nowy Index.cshtml, Wyświetl szablon dla nas w katalogu \Views\Home tworzenia folderu, jeśli jeszcze nie istnieje.</span><span class="sxs-lookup"><span data-stu-id="8f841-125">When we click the Add button, Visual Web Developer will create a new Index.cshtml view template for us in the \Views\Home directory, creating the folder if doesn't already exist.</span></span>

![](mvc-music-store-part-3/_static/image2.png)

<span data-ttu-id="8f841-126">Nazwy i lokalizacji folderu pliku "Index.cshtml" ważne jest i jest zgodna z konwencjami nazewnictwa platformy ASP.NET MVC domyślne.</span><span class="sxs-lookup"><span data-stu-id="8f841-126">The name and folder location of the "Index.cshtml" file is important, and follows the default ASP.NET MVC naming conventions.</span></span> <span data-ttu-id="8f841-127">Nazwa katalogu, \Views\Home, odpowiada kontroler - o nazwie HomeController.</span><span class="sxs-lookup"><span data-stu-id="8f841-127">The directory name, \Views\Home, matches the controller - which is named HomeController.</span></span> <span data-ttu-id="8f841-128">Nazwa szablonu widoku indeksu, pasuje do metody akcji kontrolera, które będą wyświetlane w widoku.</span><span class="sxs-lookup"><span data-stu-id="8f841-128">The view template name, Index, matches the controller action method which will be displaying the view.</span></span>

<span data-ttu-id="8f841-129">ASP.NET MVC pozwala uniknąć konieczności jawnego określania nazwy lub lokalizacji szablonu widoku, gdy używamy tę konwencję nazewnictwa w celu zwrócenia widoku.</span><span class="sxs-lookup"><span data-stu-id="8f841-129">ASP.NET MVC allows us to avoid having to explicitly specify the name or location of a view template when we use this naming convention to return a view.</span></span> <span data-ttu-id="8f841-130">Renderowanie zostanie domyślnie przeprowadzone szablon widoku \Views\Home\Index.cshtml podczas pisania kodu, takich jak poniżej w ramach naszych HomeController:</span><span class="sxs-lookup"><span data-stu-id="8f841-130">It will by default render the \Views\Home\Index.cshtml view template when we write code like below within our HomeController:</span></span>

[!code-csharp[Main](mvc-music-store-part-3/samples/sample2.cs)]

<span data-ttu-id="8f841-131">Visual Web Developer utworzony i otwarty "Index.cshtml" Wyświetl szablon, po możemy kliknięto przycisk "Dodaj" w oknie dialogowym "Dodaj widok".</span><span class="sxs-lookup"><span data-stu-id="8f841-131">Visual Web Developer created and opened the "Index.cshtml" view template after we clicked the "Add" button within the "Add View" dialog.</span></span> <span data-ttu-id="8f841-132">Poniżej przedstawiono zawartość Index.cshtml.</span><span class="sxs-lookup"><span data-stu-id="8f841-132">The contents of Index.cshtml are shown below.</span></span>

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample3.cshtml)]

<span data-ttu-id="8f841-133">Ten widok jest przy użyciu składni Razor, który jest bardziej zwięzłe niż aparat widoku formularzy sieci Web, które są używane w formularzach sieci Web platformy ASP.NET i wcześniejszych wersjach programu ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="8f841-133">This view is using the Razor syntax, which is more concise than the Web Forms view engine used in ASP.NET Web Forms and previous versions of ASP.NET MVC.</span></span> <span data-ttu-id="8f841-134">Aparat widoku w formularzach sieci Web jest nadal dostępny w programie ASP.NET MVC 3, ale wielu programistów uważa, aparat widoku Razor bardzo dobrze pasuje rozwoju platformy ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="8f841-134">The Web Forms view engine is still available in ASP.NET MVC 3, but many developers find that the Razor view engine fits ASP.NET MVC development really well.</span></span>

<span data-ttu-id="8f841-135">Pierwsze trzy wiersze Ustaw tytuł strony, przy użyciu ViewBag.Title.</span><span class="sxs-lookup"><span data-stu-id="8f841-135">The first three lines set the page title using ViewBag.Title.</span></span> <span data-ttu-id="8f841-136">Firma Microsoft będzie wyglądać jak to działa bardziej szczegółowo wkrótce, ale najpierw Przyjrzyjmy aktualizacji tekst nagłówka tekst i wyświetlić stronę.</span><span class="sxs-lookup"><span data-stu-id="8f841-136">We'll look at how this works in more detail soon, but first let's update the text heading text and view the page.</span></span> <span data-ttu-id="8f841-137">Aktualizacja &lt;h2&gt; tagu "ten to Home Page" powiedzieć, jak pokazano poniżej.</span><span class="sxs-lookup"><span data-stu-id="8f841-137">Update the &lt;h2&gt; tag to say "This is the Home Page" as shown below.</span></span>

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample4.cshtml)]

<span data-ttu-id="8f841-138">Uruchamianie aplikacji pokazuje, że nasz nowy tekst jest widoczna na stronie głównej.</span><span class="sxs-lookup"><span data-stu-id="8f841-138">Running the application shows that our new text is visible on the home page.</span></span>

![](mvc-music-store-part-3/_static/image3.png)

## <a name="using-a-layout-for-common-site-elements"></a><span data-ttu-id="8f841-139">Przy użyciu układu dla wspólnych elementów witryny</span><span class="sxs-lookup"><span data-stu-id="8f841-139">Using a Layout for common site elements</span></span>

<span data-ttu-id="8f841-140">Większość witryn sieci Web mają zawartość, która jest współużytkowana przez wiele stron: nawigacji stopkach stron, obrazów logo, odwołania do arkusza stylów, itp. Aparat widoku Razor sprawia, że jest to łatwa w zarządzaniu przy użyciu strony o nazwie \_Layout.cshtml, który został automatycznie utworzony dla nas znajdujące się w folderze/widoków/Shared.</span><span class="sxs-lookup"><span data-stu-id="8f841-140">Most websites have content which is shared between many pages: navigation, footers, logo images, stylesheet references, etc. The Razor view engine makes this easy to manage using a page called \_Layout.cshtml which has automatically been created for us inside the /Views/Shared folder.</span></span>

![](mvc-music-store-part-3/_static/image4.png)

<span data-ttu-id="8f841-141">Kliknij dwukrotnie ten folder, aby wyświetlić jego zawartość, poniżej.</span><span class="sxs-lookup"><span data-stu-id="8f841-141">Double-click on this folder to view the contents, which are shown below.</span></span>

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample5.cshtml)]

<span data-ttu-id="8f841-142">Zawartość z naszych pojedyncze widoki, które będą wyświetlane przez @RenderBodypolecenia () oraz typowe zawartość, która ma znajdować się poza, można dodać do \_Layout.cshtml znaczników.</span><span class="sxs-lookup"><span data-stu-id="8f841-142">The content from our individual views will be displayed by the @RenderBody() command, and any common content that we want to appear outside of that can be added to the \_Layout.cshtml markup.</span></span> <span data-ttu-id="8f841-143">Chcemy, aby nasze Store utworów muzycznych MVC aby wspólnej nagłówek wraz z łączami do naszej strony głównej i Store obszaru na wszystkich stronach w witrynie, dlatego dodamy, szablon bezpośrednio powyżej @RenderBodyinstrukcji ().</span><span class="sxs-lookup"><span data-stu-id="8f841-143">We'll want our MVC Music Store to have a common header with links to our Home page and Store area on all pages in the site, so we'll add that to the template directly above that @RenderBody() statement.</span></span>

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample6.cshtml)]

## <a name="updating-the-stylesheet"></a><span data-ttu-id="8f841-144">Aktualizowanie arkusza stylów</span><span class="sxs-lookup"><span data-stu-id="8f841-144">Updating the StyleSheet</span></span>

<span data-ttu-id="8f841-145">Szablonu pusty projekt zawiera bardzo usprawnione pliku CSS, która zawiera tylko style, które umożliwia wyświetlenie komunikatów dotyczących sprawdzania poprawności.</span><span class="sxs-lookup"><span data-stu-id="8f841-145">The empty project template includes a very streamlined CSS file which just includes styles used to display validation messages.</span></span> <span data-ttu-id="8f841-146">Nasze projektanta udostępnił niektóre dodatkowe CSS i obrazów do definiowania wygląd i działanie dla naszej witrynie, dlatego dodamy te teraz.</span><span class="sxs-lookup"><span data-stu-id="8f841-146">Our designer has provided some additional CSS and images to define the look and feel for our site, so we'll add those in now.</span></span>

<span data-ttu-id="8f841-147">Zaktualizowany plik CSS i obrazów, które są uwzględnione w zawartości katalogu Assets.zip MvcMusicStore, który znajduje się w temacie [MVC — muzyka-Store](https://github.com/evilDave/MVC-Music-Store).</span><span class="sxs-lookup"><span data-stu-id="8f841-147">The updated CSS file and Images are included in the Content directory of MvcMusicStore-Assets.zip which is available at [MVC-Music-Store](https://github.com/evilDave/MVC-Music-Store).</span></span> <span data-ttu-id="8f841-148">Utworzymy wybierz obie z nich w Eksploratorze Windows i upuścić je do folderu zawartości Nasze rozwiązanie programu Visual Web Developer, jak pokazano poniżej:</span><span class="sxs-lookup"><span data-stu-id="8f841-148">We'll select both of them in Windows Explorer and drop them into our Solution's Content folder in Visual Web Developer, as shown below:</span></span>

![](mvc-music-store-part-3/_static/image5.png)

<span data-ttu-id="8f841-149">Zostanie wyświetlony monit o potwierdzenie, czy chcesz zastąpić istniejący plik Site.css.</span><span class="sxs-lookup"><span data-stu-id="8f841-149">You'll be asked to confirm if you want to overwrite the existing Site.css file.</span></span> <span data-ttu-id="8f841-150">Kliknij przycisk Tak.</span><span class="sxs-lookup"><span data-stu-id="8f841-150">Click Yes.</span></span>

![](mvc-music-store-part-3/_static/image6.png)

<span data-ttu-id="8f841-151">Folder zawartości Twojej aplikacji będą wyświetlane w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="8f841-151">The Content folder of your application will now appear as follows:</span></span>

![](mvc-music-store-part-3/_static/image7.png)

<span data-ttu-id="8f841-152">Teraz możemy uruchomić aplikację i zobacz, jak wyglądają nasze zmiany na stronie głównej.</span><span class="sxs-lookup"><span data-stu-id="8f841-152">Now let's run the application and see how our changes look on the Home page.</span></span>

![](mvc-music-store-part-3/_static/image8.png)

- <span data-ttu-id="8f841-153">Podsumujmy, co zostało zmienione: metody akcji indeksu HomeController znalezione i wyświetlane \Views\Home\Index.cshtmlView szablonu, mimo że naszego kodu o nazwie "View() zwrotu", ponieważ szablon widoku, a następnie standardowej konwencji nazewnictwa.</span><span class="sxs-lookup"><span data-stu-id="8f841-153">Let's review what's changed: The HomeController's Index action method found and displayed the \Views\Home\Index.cshtmlView template, even though our code called "return View()", because our View template followed the standard naming convention.</span></span>
- <span data-ttu-id="8f841-154">Strona główna wyświetla prosty komunikat powitalny, który jest zdefiniowany w szablonie widoku \Views\Home\Index.cshtml.</span><span class="sxs-lookup"><span data-stu-id="8f841-154">The Home Page is displaying a simple welcome message that is defined within the \Views\Home\Index.cshtml view template.</span></span>
- <span data-ttu-id="8f841-155">Strona główna używa naszej \_Layout.cshtml szablonu, a zatem komunikat powitalny znajduje się w układzie standardowej witrynie HTML.</span><span class="sxs-lookup"><span data-stu-id="8f841-155">The Home Page is using our \_Layout.cshtml template, and so the welcome message is contained within the standard site HTML layout.</span></span>

## <a name="using-a-model-to-pass-information-to-our-view"></a><span data-ttu-id="8f841-156">Przy użyciu modelu do przekazywania informacji do naszych widoku</span><span class="sxs-lookup"><span data-stu-id="8f841-156">Using a Model to pass information to our View</span></span>

<span data-ttu-id="8f841-157">Wyświetl szablon, który po prostu wyświetla zapisane na stałe HTML nie będzie bardzo interesujące, witryny sieci web.</span><span class="sxs-lookup"><span data-stu-id="8f841-157">A View template that just displays hardcoded HTML isn't going to make a very interesting web site.</span></span> <span data-ttu-id="8f841-158">Aby utworzyć dynamiczne witryny sieci web, będzie zamiast tego chcemy do przekazywania informacji z naszych akcji kontrolera do naszych szablonów widoku.</span><span class="sxs-lookup"><span data-stu-id="8f841-158">To create a dynamic web site, we'll instead want to pass information from our controller actions to our view templates.</span></span>

<span data-ttu-id="8f841-159">Wzorzec Model-View-Controller termin, który Model, który odwołuje się do obiektów, zawierające dane w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="8f841-159">In the Model-View-Controller pattern, the term Model refers to objects which represent the data in the application.</span></span> <span data-ttu-id="8f841-160">Często obiekty modelu odnoszą się do tabel w bazie danych, ale nie muszą oni.</span><span class="sxs-lookup"><span data-stu-id="8f841-160">Often, model objects correspond to tables in your database, but they don't have to.</span></span>

<span data-ttu-id="8f841-161">Metody akcji kontrolera, które zwracają element ActionResult, można przekazać obiekt modelu widoku.</span><span class="sxs-lookup"><span data-stu-id="8f841-161">Controller action methods which return an ActionResult can pass a model object to the view.</span></span> <span data-ttu-id="8f841-162">Dzięki temu kontroler na klarownie spakowanie wszystkie informacje niezbędne do generowania odpowiedzi, a następnie przekazał tych informacji do szablonu widoku na potrzeby generowania odpowiednich odpowiedzi HTML.</span><span class="sxs-lookup"><span data-stu-id="8f841-162">This allows a Controller to cleanly package up all the information needed to generate a response, and then pass this information off to a View template to use to generate the appropriate HTML response.</span></span> <span data-ttu-id="8f841-163">Jest to łatwiej zrozumieć, obserwując go w działaniu, więc zaczynajmy.</span><span class="sxs-lookup"><span data-stu-id="8f841-163">This is easiest to understand by seeing it in action, so let's get started.</span></span>

<span data-ttu-id="8f841-164">Najpierw utworzymy niektóre klasy modelu do reprezentowania gatunki i albumów w naszym Sklepie.</span><span class="sxs-lookup"><span data-stu-id="8f841-164">First we'll create some Model classes to represent Genres and Albums within our store.</span></span> <span data-ttu-id="8f841-165">Zacznijmy od utworzenia klasy gatunku.</span><span class="sxs-lookup"><span data-stu-id="8f841-165">Let's start by creating a Genre class.</span></span> <span data-ttu-id="8f841-166">Kliknij prawym przyciskiem myszy folder "Modele" w obrębie projektu, wybierz opcję "Dodaj klasę" i nazwij plik "Genre.cs".</span><span class="sxs-lookup"><span data-stu-id="8f841-166">Right-click the "Models" folder within your project, choose the "Add Class" option, and name the file "Genre.cs".</span></span>

![](mvc-music-store-part-3/_static/image2.jpg)

![](mvc-music-store-part-3/_static/image9.png)

<span data-ttu-id="8f841-167">Następnie dodaj publicznego ciągu nazwy właściwości do klasy, która została utworzona:</span><span class="sxs-lookup"><span data-stu-id="8f841-167">Then add a public string Name property to the class that was created:</span></span>

[!code-csharp[Main](mvc-music-store-part-3/samples/sample7.cs)]

<span data-ttu-id="8f841-168">*Uwaga: W przypadku zastanawiasz się {Pobierz; Ustaw;} osiągnął notacji korzystanie z języka C# na automatycznie implementowane właściwości funkcji. To daje nam korzyści wynikające z właściwością bez konieczności NAS zadeklarować pole zapasowe.*</span><span class="sxs-lookup"><span data-stu-id="8f841-168">*Note: In case you're wondering, the { get; set; } notation is making use of C#'s auto-implemented properties feature. This gives us the benefits of a property without requiring us to declare a backing field.*</span></span>

<span data-ttu-id="8f841-169">Następnie wykonaj te same kroki, aby utworzyć klasę albumu (o nazwie Album.cs), która ma tytuł i właściwości gatunku:</span><span class="sxs-lookup"><span data-stu-id="8f841-169">Next, follow the same steps to create an Album class (named Album.cs) that has a Title and a Genre property:</span></span>

[!code-csharp[Main](mvc-music-store-part-3/samples/sample8.cs)]

<span data-ttu-id="8f841-170">Teraz możemy zmodyfikować StoreController używać widoków, wyświetlające informacji dynamicznych z nasz Model.</span><span class="sxs-lookup"><span data-stu-id="8f841-170">Now we can modify the StoreController to use Views which display dynamic information from our Model.</span></span> <span data-ttu-id="8f841-171">Jeśli — dla celów demonstracyjnych teraz — firma Microsoft o nazwie naszych albumów na podstawie Identyfikatora żądania, firma Microsoft można wyświetlić te informacje, tak jak w poniższym widoku.</span><span class="sxs-lookup"><span data-stu-id="8f841-171">If - for demonstration purposes right now - we named our Albums based on the request ID, we could display that information as in the view below.</span></span>

![](mvc-music-store-part-3/_static/image10.png)

<span data-ttu-id="8f841-172">Zaczniemy, zmieniając akcji Store Details, aby pokazywała informacje dotyczące jednego albumu.</span><span class="sxs-lookup"><span data-stu-id="8f841-172">We'll start by changing the Store Details action so it shows the information for a single album.</span></span> <span data-ttu-id="8f841-173">Dodaj instrukcję "using" na początku **StoreControllers** klasy, aby uwzględnić przestrzeń nazw MvcMusicStore.Models, dzięki czemu nie trzeba wpisać MvcMusicStore.Models.Album za każdym razem, gdy chcemy użyć klasy albumu.</span><span class="sxs-lookup"><span data-stu-id="8f841-173">Add a "using" statement to the top of the **StoreControllers** class to include the MvcMusicStore.Models namespace, so we don't need to type MvcMusicStore.Models.Album every time we want to use the album class.</span></span> <span data-ttu-id="8f841-174">Sekcja "dyrektywy Using" tej klasy powinien teraz wyglądać jak poniżej.</span><span class="sxs-lookup"><span data-stu-id="8f841-174">The "usings" section of that class should now appear as below.</span></span>

[!code-csharp[Main](mvc-music-store-part-3/samples/sample9.cs)]

<span data-ttu-id="8f841-175">Następnie zaktualizujemy szczegóły akcji kontrolera, aby funkcja zwraca element ActionResult, a nie w ciągu, ile My mieliśmy metodą indeksu HomeController.</span><span class="sxs-lookup"><span data-stu-id="8f841-175">Next, we'll update the Details controller action so that it returns an ActionResult rather than a string, as we did with the HomeController's Index method.</span></span>

[!code-csharp[Main](mvc-music-store-part-3/samples/sample10.cs)]

<span data-ttu-id="8f841-176">Teraz możemy zmodyfikować logiki, aby zwrócić obiekt albumu do widoku.</span><span class="sxs-lookup"><span data-stu-id="8f841-176">Now we can modify the logic to return an Album object to the view.</span></span> <span data-ttu-id="8f841-177">W dalszej części tego samouczka firma Microsoft będzie można pobierania danych z bazy danych — ale dla obecnie firma Microsoft użyje "fikcyjny dane" Aby rozpocząć pracę.</span><span class="sxs-lookup"><span data-stu-id="8f841-177">Later in this tutorial we will be retrieving the data from a database – but for right now we will use "dummy data" to get started.</span></span>

[!code-csharp[Main](mvc-music-store-part-3/samples/sample11.cs)]

<span data-ttu-id="8f841-178">*Uwaga: Jeżeli jesteś zaznajomiony z C#, możesz może przyjąć, że za pomocą var oznacza, że nasza zmienna album z późnym wiązaniem. Nie jest prawidłowe — kompilator języka C# używa wnioskowanie typów w oparciu o co możemy przypisanie do zmiennej w celu określenia tej album jest typu albumu i kompilowanie zmiennej lokalnej albumu jako typu fotograficzne, dzięki czemu możemy sprawdzanie w czasie kompilacji i Edytor kodu programu Visual Studio Pomoc techniczna.*</span><span class="sxs-lookup"><span data-stu-id="8f841-178">*Note: If you're unfamiliar with C#, you may assume that using var means that our album variable is late-bound. That's not correct – the C# compiler is using type-inference based on what we're assigning to the variable to determine that album is of type Album and compiling the local album variable as an Album type, so we get compile-time checking and Visual Studio code-editor support.*</span></span>

<span data-ttu-id="8f841-179">Teraz Utwórz szablon widoku, który używa naszej albumu do generowania odpowiedzi HTML.</span><span class="sxs-lookup"><span data-stu-id="8f841-179">Let's now create a View template that uses our Album to generate an HTML response.</span></span> <span data-ttu-id="8f841-180">Zanim do tego należy skompilować projekt, aby poinformować okno dialogowe dodawania widoku o nasze nowo utworzonej klasie albumu.</span><span class="sxs-lookup"><span data-stu-id="8f841-180">Before we do that we need to build the project so that the Add View dialog knows about our newly created Album class.</span></span> <span data-ttu-id="8f841-181">Można tworzyć projekt, wybierając Debug⇨Build MvcMusicStore elementu menu (jako dodatkowe ćwiczenie można Użyj skrótu Ctrl-Shift-B do skompilowania projektu).</span><span class="sxs-lookup"><span data-stu-id="8f841-181">You can build the project by selecting the Debug⇨Build MvcMusicStore menu item (for extra credit, you can use the Ctrl-Shift-B shortcut to build the project).</span></span>

![](mvc-music-store-part-3/_static/image11.png)

<span data-ttu-id="8f841-182">Skoro już skonfigurowaliśmy naszych klasy pomocnicze jesteśmy gotowi do tworzenia szablonu widoku.</span><span class="sxs-lookup"><span data-stu-id="8f841-182">Now that we've set up our supporting classes, we're ready to build our View template.</span></span> <span data-ttu-id="8f841-183">Kliknij prawym przyciskiem myszy wewnątrz metody szczegółowe informacje, a następnie wybierz pozycję "Dodaj widok..." z menu kontekstowego.</span><span class="sxs-lookup"><span data-stu-id="8f841-183">Right-click within the Details method and select "Add View…" from the context menu.</span></span>

![](mvc-music-store-part-3/_static/image12.png)

<span data-ttu-id="8f841-184">Zamierzamy utworzyć nowy szablon widoku jak zrobiliśmy przed z HomeController.</span><span class="sxs-lookup"><span data-stu-id="8f841-184">We are going to create a new View template like we did before with the HomeController.</span></span> <span data-ttu-id="8f841-185">Ponieważ tworzymy z StoreController go zostanie domyślnie wygenerowany plik \Views\Store\Index.cshtml.</span><span class="sxs-lookup"><span data-stu-id="8f841-185">Because we are creating it from the StoreController it will by default be generated in a \Views\Store\Index.cshtml file.</span></span>

<span data-ttu-id="8f841-186">W przeciwieństwie do wcześniej, użyjemy zaznacz pole wyboru "Utwórz silnie typizowanego" widoku.</span><span class="sxs-lookup"><span data-stu-id="8f841-186">Unlike before, we are going to check the "Create a strongly-typed" view checkbox.</span></span> <span data-ttu-id="8f841-187">Następnie użyjemy wybierz nasze klasę "Albumu" w ciągu "Widok danych class" listy downlist.</span><span class="sxs-lookup"><span data-stu-id="8f841-187">We are then going to select our "Album" class within the "View data-class" drop-downlist.</span></span> <span data-ttu-id="8f841-188">Spowoduje to okno dialogowe "Dodaj widok", aby utworzyć szablon widoku, który oczekuje, że albumu obiektów, które zostaną przekazane do go do użycia.</span><span class="sxs-lookup"><span data-stu-id="8f841-188">This will cause the "Add View" dialog to create a View template that expects that an Album object will be passed to it to use.</span></span>

![](mvc-music-store-part-3/_static/image13.png)

<span data-ttu-id="8f841-189">Po kliknięciu przycisku "Dodaj" nasze \Views\Store\Details.cshtml Wyświetl szablon zostanie utworzony, zawierający poniższy kod.</span><span class="sxs-lookup"><span data-stu-id="8f841-189">When we click the "Add" button our \Views\Store\Details.cshtml View template will be created, containing the following code.</span></span>

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample12.cshtml)]

<span data-ttu-id="8f841-190">Zwróć uwagę, pierwszego wiersza, co oznacza, że ten widok jest silnie typizowaną do klasy Nasze albumu.</span><span class="sxs-lookup"><span data-stu-id="8f841-190">Notice the first line, which indicates that this view is strongly-typed to our Album class.</span></span> <span data-ttu-id="8f841-191">Aparat widoku Razor rozumie, że go został przekazany obiekt albumu, firma Microsoft można łatwo uzyskiwać dostęp do właściwości modelu oraz nawet korzystać z zalet technologii IntelliSense w edytorze programu Visual Web Developer.</span><span class="sxs-lookup"><span data-stu-id="8f841-191">The Razor view engine understands that it has been passed an Album object, so we can easily access model properties and even have the benefit of IntelliSense in the Visual Web Developer editor.</span></span>

<span data-ttu-id="8f841-192">Aktualizacja &lt;h2&gt; tag, aby wyświetlała właściwości Title fotograficzne, modyfikując ten wiersz, aby wyglądają następująco.</span><span class="sxs-lookup"><span data-stu-id="8f841-192">Update the &lt;h2&gt; tag so it displays the Album's Title property by modifying that line to appear as follows.</span></span>

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample13.cshtml)]

<span data-ttu-id="8f841-193">Należy zauważyć, że funkcja IntelliSense jest wyzwalany w przypadku Podaj okres, po @Model — słowo kluczowe, wyświetlanie właściwości i metody, które obsługuje klasa albumu.</span><span class="sxs-lookup"><span data-stu-id="8f841-193">Notice that IntelliSense is triggered when you enter the period after the @Model keyword, showing the properties and methods that the Album class supports.</span></span>

<span data-ttu-id="8f841-194">Załóżmy teraz ponownego uruchomienia naszych projektu i skorzystaj z 5-Store/szczegóły adresu URL.</span><span class="sxs-lookup"><span data-stu-id="8f841-194">Let's now re-run our project and visit the /Store/Details/5 URL.</span></span> <span data-ttu-id="8f841-195">Zobaczymy Szczegóły albumu podobnie jak poniżej.</span><span class="sxs-lookup"><span data-stu-id="8f841-195">We'll see details of an Album like below.</span></span>

![](mvc-music-store-part-3/_static/image14.png)

<span data-ttu-id="8f841-196">Teraz wprowadzimy podobne aktualizacji dla metody akcji przeglądania Store.</span><span class="sxs-lookup"><span data-stu-id="8f841-196">Now we'll make a similar update for the Store Browse action method.</span></span> <span data-ttu-id="8f841-197">Zaktualizuj metodę, tak aby zwracało poprawnie element ActionResult, a następnie zmodyfikuj logikę metody, więc tworzy nowy obiekt gatunku i zwraca go do widoku.</span><span class="sxs-lookup"><span data-stu-id="8f841-197">Update the method so it returns an ActionResult, and modify the method logic so it creates a new Genre object and returns it to the View.</span></span>

[!code-csharp[Main](mvc-music-store-part-3/samples/sample14.cs)]

<span data-ttu-id="8f841-198">Kliknij prawym przyciskiem myszy w metodzie przeglądania i wybierz pozycję "Dodaj widok..." z menu kontekstowego, Dodaj widok, który jest silnie typizowane Dodaj silnie typizowaną do klasy gatunku.</span><span class="sxs-lookup"><span data-stu-id="8f841-198">Right-click in the Browse method and select "Add View…" from the context menu, then add a View that is strongly-typed add a strongly typed to the Genre class.</span></span>

![](mvc-music-store-part-3/_static/image15.png)

<span data-ttu-id="8f841-199">Aktualizacja &lt;h2&gt; elementu w widoku kodu (w /Views/Store/Browse.cshtml) do wyświetlania informacji gatunku.</span><span class="sxs-lookup"><span data-stu-id="8f841-199">Update the &lt;h2&gt; element in the view code (in /Views/Store/Browse.cshtml) to display the Genre information.</span></span>

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample15.cshtml)]

<span data-ttu-id="8f841-200">Teraz możemy ponownie uruchomić projekcie i przejdź do/Store/przeglądania? Gatunku = Disco adres URL.</span><span class="sxs-lookup"><span data-stu-id="8f841-200">Now let's re-run our project and browse to the /Store/Browse?Genre=Disco URL.</span></span> <span data-ttu-id="8f841-201">Firma Microsoft zostanie wyświetlona strona przeglądania, wyświetlana, jak poniżej.</span><span class="sxs-lookup"><span data-stu-id="8f841-201">We'll see the Browse page displayed like below.</span></span>

![](mvc-music-store-part-3/_static/image16.png)

<span data-ttu-id="8f841-202">Na koniec upewnijmy się nieco bardziej skomplikowaną aktualizacji **Store indeksu** metody akcji i widok, aby wyświetlić listę wszystkich gatunki w naszym Sklepie.</span><span class="sxs-lookup"><span data-stu-id="8f841-202">Finally, let's make a slightly more complex update to the **Store Index** action method and view to display a list of all the Genres in our store.</span></span> <span data-ttu-id="8f841-203">Możemy to zrobić za pomocą listy gatunki jako naszych obiektu modelu, a nie tylko jednego rodzaju.</span><span class="sxs-lookup"><span data-stu-id="8f841-203">We'll do that by using a List of Genres as our model object, rather than just a single Genre.</span></span>

[!code-csharp[Main](mvc-music-store-part-3/samples/sample16.cs)]

<span data-ttu-id="8f841-204">Kliknij prawym przyciskiem myszy w metodzie akcji indeksu Store i wybierz pozycję Dodaj widok, wybierz gatunku jako klasa modelu, a następnie naciśnij przycisk Dodaj.</span><span class="sxs-lookup"><span data-stu-id="8f841-204">Right-click in the Store Index action method and select Add View as before, select Genre as the Model class, and press the Add button.</span></span>

![](mvc-music-store-part-3/_static/image17.png)

<span data-ttu-id="8f841-205">Pierwsze zmienimy @model deklaracji, aby wskazać, czy widok będzie oczekiwano gatunku kilka obiektów zamiast tylko jeden.</span><span class="sxs-lookup"><span data-stu-id="8f841-205">First we'll change the @model declaration to indicate that the view will be expecting several Genre objects rather than just one.</span></span> <span data-ttu-id="8f841-206">Zmiana w pierwszym wierszu /Store/Index.cshtml do odczytu w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="8f841-206">Change the first line of /Store/Index.cshtml to read as follows:</span></span>

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample17.cshtml)]

<span data-ttu-id="8f841-207">Oznacza to, że będzie on pracować z obiektu modelu, który może zawierać kilka obiektów gatunku aparatu widoku Razor.</span><span class="sxs-lookup"><span data-stu-id="8f841-207">This tells the Razor view engine that it will be working with a model object that can hold several Genre objects.</span></span> <span data-ttu-id="8f841-208">Firma Microsoft korzysta z elementu IEnumerable&lt;gatunku&gt; zamiast listy&lt;gatunku&gt; ponieważ jest bardziej ogólnym, może zmienić później naszych typ modelu do dowolnego typu obiektu, który obsługuje interfejsu IEnumerable.</span><span class="sxs-lookup"><span data-stu-id="8f841-208">We're using an IEnumerable&lt;Genre&gt; rather than a List&lt;Genre&gt; since it's more generic, allowing us to change our model type later to any object type that supports the IEnumerable interface.</span></span>

<span data-ttu-id="8f841-209">Następnie firma Microsoft będzie pętli gatunku obiekty w modelu, jak pokazano w poniższym kodzie ukończone.</span><span class="sxs-lookup"><span data-stu-id="8f841-209">Next, we'll loop through the Genre objects in the model as shown in the completed view code below.</span></span>

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample18.cshtml)]

<span data-ttu-id="8f841-210">Zwróć uwagę, że mamy już pełną obsługą technologii IntelliSense możemy wprowadź ten kod tak, że gdy wpiszesz "@Model."</span><span class="sxs-lookup"><span data-stu-id="8f841-210">Notice that we have full IntelliSense support as we enter this code, so that when we type "@Model."</span></span> <span data-ttu-id="8f841-211">widzimy, wszystkie metody i właściwości obsługiwanych przez elementu IEnumerable typu gatunku.</span><span class="sxs-lookup"><span data-stu-id="8f841-211">we see all methods and properties supported by an IEnumerable of type Genre.</span></span>

![](mvc-music-store-part-3/_static/image18.png)

<span data-ttu-id="8f841-212">W ramach naszych pętlę "foreach" Visual Web Developer wie, że każdy element jest typu gatunku, więc widzimy IntelliSence dla każdego typu gatunku.</span><span class="sxs-lookup"><span data-stu-id="8f841-212">Within our "foreach" loop, Visual Web Developer knows that each item is of type Genre, so we see IntelliSence for each the Genre type.</span></span>

![](mvc-music-store-part-3/_static/image19.png)

<span data-ttu-id="8f841-213">Następnie funkcja tworzenia szkieletu zbadane obiektu gatunku i określić, że będzie mieć właściwości Name, więc w pętli i zapisuje je w. Generuje on również linki Edytuj, szczegóły i Delete do poszczególnych elementów.</span><span class="sxs-lookup"><span data-stu-id="8f841-213">Next, the scaffolding feature examined the Genre object and determined that each will have a Name property, so it loops through and writes them out. It also generates Edit, Details, and Delete links to each individual item.</span></span> <span data-ttu-id="8f841-214">Firma Microsoft będzie zalet, w dalszej części nasz Menedżer magazynu, ale teraz chcemy mieć zamiast prostej listy.</span><span class="sxs-lookup"><span data-stu-id="8f841-214">We'll take advantage of that later in our store manager, but for now we'd like to have a simple list instead.</span></span>

<span data-ttu-id="8f841-215">Gdy firma Microsoft może uruchomić aplikację, a następnie przejdź do/Store, zobaczymy, liczby i lista gatunki jest wyświetlana.</span><span class="sxs-lookup"><span data-stu-id="8f841-215">When we run the application and browse to /Store, we see that both the count and list of Genres is displayed.</span></span>

![](mvc-music-store-part-3/_static/image20.png)

## <a name="adding-links-between-pages"></a><span data-ttu-id="8f841-216">Dodawanie łączy między stronami</span><span class="sxs-lookup"><span data-stu-id="8f841-216">Adding Links between pages</span></span>

<span data-ttu-id="8f841-217">Adresu URL/Store obecnie zawierającego gatunki Wyświetla nazwy gatunku, po prostu jako zwykły tekst.</span><span class="sxs-lookup"><span data-stu-id="8f841-217">Our /Store URL that lists Genres currently lists the Genre names simply as plain text.</span></span> <span data-ttu-id="8f841-218">Wybierzmy tak, aby zamiast zwykłego tekstu mamy zamiast tego gatunku nazwy link na odpowiedni adres URL Store/przeglądania, tak aby kliknięcie określonego rodzaju utworów muzycznych, takie jak "Najdywania" spowoduje przejście do/Store/przeglądania? gatunku = Najdywania adres URL.</span><span class="sxs-lookup"><span data-stu-id="8f841-218">Let's change this so that instead of plain text we instead have the Genre names link to the appropriate /Store/Browse URL, so that clicking on a music genre like "Disco" will navigate to the /Store/Browse?genre=Disco URL.</span></span> <span data-ttu-id="8f841-219">Firma Microsoft można zaktualizować szablon widoku \Views\Store\Index.cshtml te linki przy użyciu kodu takie jak poniżej danych wyjściowych **(nie należy umieszczać w — teraz zamierzamy je poprawić)**:</span><span class="sxs-lookup"><span data-stu-id="8f841-219">We could update our \Views\Store\Index.cshtml View template to output these links using code like below **(don't type this in - we're going to improve on it)**:</span></span>

[!code-html[Main](mvc-music-store-part-3/samples/sample19.html)]

<span data-ttu-id="8f841-220">To działa, ale może prowadzić do problemów później, ponieważ opiera się na ciąg zapisane na stałe.</span><span class="sxs-lookup"><span data-stu-id="8f841-220">That works, but it could lead to trouble later since it relies on a hardcoded string.</span></span> <span data-ttu-id="8f841-221">Na przykład jeśli chcemy Zmień nazwę kontrolera, czy musimy przeszukiwać naszego kodu szuka łączy, które muszą zostać zaktualizowane.</span><span class="sxs-lookup"><span data-stu-id="8f841-221">For instance, if we wanted to rename the Controller, we'd need to search through our code looking for links that need to be updated.</span></span>

<span data-ttu-id="8f841-222">Alternatywne podejście, które możemy użyć jest korzystanie z zalet metodę pomocnika kodu HTML.</span><span class="sxs-lookup"><span data-stu-id="8f841-222">An alternative approach we can use is to take advantage of an HTML Helper method.</span></span> <span data-ttu-id="8f841-223">Platforma ASP.NET MVC zawiera metody pomocnika kodu HTML, które są dostępne z naszego kodu szablonu widoku do wykonywania różnych zadań, tak jak to.</span><span class="sxs-lookup"><span data-stu-id="8f841-223">ASP.NET MVC includes HTML Helper methods which are available from our View template code to perform a variety of common tasks just like this.</span></span> <span data-ttu-id="8f841-224">Metoda pomocnika Html.ActionLink() jest szczególnie przydatne i ułatwia tworzenie HTML &lt;&gt; łączy i dba o irytujące szczegóły, takie jak zagwarantowanie, że ścieżki URL są poprawnie zakodowane w adresie URL.</span><span class="sxs-lookup"><span data-stu-id="8f841-224">The Html.ActionLink() helper method is a particularly useful one, and makes it easy to build HTML &lt;a&gt; links and takes care of annoying details like making sure URL paths are properly URL encoded.</span></span>

<span data-ttu-id="8f841-225">Html.ActionLink() ma kilka przeciążeń różnych, aby umożliwić określenie tylu informacji potrzebnych dla łącza.</span><span class="sxs-lookup"><span data-stu-id="8f841-225">Html.ActionLink() has several different overloads to allow specifying as much information as you need for your links.</span></span> <span data-ttu-id="8f841-226">W najprostszym przypadku podasz, po prostu tekst łącza, a także metoda akcji nastąpić przejście po kliknięciu hiperlinku na komputerze klienckim.</span><span class="sxs-lookup"><span data-stu-id="8f841-226">In the simplest case, you'll supply just the link text and the Action method to go to when the hyperlink is clicked on the client.</span></span> <span data-ttu-id="8f841-227">Na przykład można przejść do "/ Store /" Metoda indeks() na stronie szczegółów Store tekstem link "Przejdź do Store Index" przy użyciu następującego wywołania:</span><span class="sxs-lookup"><span data-stu-id="8f841-227">For example, we can link to "/Store/" Index() method on the Store Details page with the link text "Go to the Store Index" using the following call:</span></span>

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample20.cshtml)]

<span data-ttu-id="8f841-228">*Uwaga: W tym przypadku nie należy określać nazwy kontrolera, ponieważ możemy po prostu ustanawiania innej akcji w obrębie tego samego kontrolera, który jest renderowanie bieżący widok.*</span><span class="sxs-lookup"><span data-stu-id="8f841-228">*Note: In this case, we didn't need to specify the controller name because we're just linking to another action within the same controller that's rendering the current view.*</span></span>

<span data-ttu-id="8f841-229">Nasze łącza do strony Przegląd należy przekazać parametr, jednak dlatego użyjemy innego przeciążenia metody Html.ActionLink, która przyjmuje trzy parametry:</span><span class="sxs-lookup"><span data-stu-id="8f841-229">Our links to the Browse page will need to pass a parameter, though, so we'll use another overload of the Html.ActionLink method that takes three parameters:</span></span>

- 1. <span data-ttu-id="8f841-230">Tekst łącza, co spowoduje wyświetlenie nazwy gatunku</span><span class="sxs-lookup"><span data-stu-id="8f841-230">Link text, which will display the Genre name</span></span>
- 2. <span data-ttu-id="8f841-231">Nazwa akcji kontrolera (Przeglądaj)</span><span class="sxs-lookup"><span data-stu-id="8f841-231">Controller action name (Browse)</span></span>
- 3. <span data-ttu-id="8f841-232">Wartości parametrów trasy, określając nazwę (gatunku) i wartości (nazwa gatunku)</span><span class="sxs-lookup"><span data-stu-id="8f841-232">Route parameter values, specifying both the name (Genre) and the value (Genre name)</span></span>

<span data-ttu-id="8f841-233">Umieszczanie, że wszystko ze sobą, Oto jak będziemy pisać tych łączy do widoku indeksu Store:</span><span class="sxs-lookup"><span data-stu-id="8f841-233">Putting that all together, here's how we'll write those links to the Store Index view:</span></span>

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample21.cshtml)]

<span data-ttu-id="8f841-234">Teraz możemy ponownie uruchom naszych projektów i uzyskać dostępu do adresu URL /Store/ firma Microsoft zostanie wyświetlona lista gatunki.</span><span class="sxs-lookup"><span data-stu-id="8f841-234">Now when we run our project again and access the /Store/ URL we will see a list of genres.</span></span> <span data-ttu-id="8f841-235">Każdego gatunku jest hiperłącze — po kliknięciu potrwa nam naszym/Store/przeglądania? gatunku =*[gatunek]* adresu URL.</span><span class="sxs-lookup"><span data-stu-id="8f841-235">Each genre is a hyperlink – when clicked it will take us to our /Store/Browse?genre=*[genre]* URL.</span></span>

![](mvc-music-store-part-3/_static/image3.jpg)

<span data-ttu-id="8f841-236">Kod HTML dla listy gatunku wygląda następująco:</span><span class="sxs-lookup"><span data-stu-id="8f841-236">The HTML for the genre list looks like this:</span></span>

[!code-html[Main](mvc-music-store-part-3/samples/sample22.html)]


> [!div class="step-by-step"]
> <span data-ttu-id="8f841-237">[Poprzednie](mvc-music-store-part-2.md)
> [dalej](mvc-music-store-part-4.md)</span><span class="sxs-lookup"><span data-stu-id="8f841-237">[Previous](mvc-music-store-part-2.md)
[Next](mvc-music-store-part-4.md)</span></span>
