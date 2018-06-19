---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-5
title: 'Część 5: Edycji formularzy i tworzenia szablonów | Dokumentacja firmy Microsoft'
author: jongalloway
description: Ten samouczek serii zawiera szczegóły dotyczące wszystkich kroków tworzenia przykładowej aplikacji ASP.NET MVC utworów muzycznych magazynu. Część 5 obejmuje edycji formularzy oraz tworzenia szablonów.
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/21/2011
ms.topic: article
ms.assetid: 6b09413a-6d6a-425a-87c9-629f91b91b28
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-5
msc.type: authoredcontent
ms.openlocfilehash: d584e614b5a4124044cd9decd2272192ca164643
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
ms.locfileid: "30874915"
---
<a name="part-5-edit-forms-and-templating"></a><span data-ttu-id="196c3-104">Część 5: Formularze edycji i tworzenia szablonów</span><span class="sxs-lookup"><span data-stu-id="196c3-104">Part 5: Edit Forms and Templating</span></span>
====================
<span data-ttu-id="196c3-105">przez [Galloway Jan](https://github.com/jongalloway)</span><span class="sxs-lookup"><span data-stu-id="196c3-105">by [Jon Galloway](https://github.com/jongalloway)</span></span>

> <span data-ttu-id="196c3-106">Magazyn utworów muzycznych MVC jest samouczek aplikacji, które wprowadzono i opisano krok po kroku, jak używać do tworzenia aplikacji sieci web platformy ASP.NET MVC i Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="196c3-106">The MVC Music Store is a tutorial application that introduces and explains step-by-step how to use ASP.NET MVC and Visual Studio for web development.</span></span>  
>   
> <span data-ttu-id="196c3-107">Magazyn utworów muzycznych MVC jest implementacja magazynu lekkie próbki, co sprzedaje albumów muzycznych w trybie online i implementuje podstawowej witryny administracji, logowania użytkownika i funkcje koszyka zakupów.</span><span class="sxs-lookup"><span data-stu-id="196c3-107">The MVC Music Store is a lightweight sample store implementation which sells music albums online, and implements basic site administration, user sign-in, and shopping cart functionality.</span></span>
> 
> <span data-ttu-id="196c3-108">Ten samouczek serii zawiera szczegóły dotyczące wszystkich kroków tworzenia przykładowej aplikacji ASP.NET MVC utworów muzycznych magazynu.</span><span class="sxs-lookup"><span data-stu-id="196c3-108">This tutorial series details all of the steps taken to build the ASP.NET MVC Music Store sample application.</span></span> <span data-ttu-id="196c3-109">Część 5 obejmuje edycji formularzy oraz tworzenia szablonów.</span><span class="sxs-lookup"><span data-stu-id="196c3-109">Part 5 covers Edit Forms and Templating.</span></span>


<span data-ttu-id="196c3-110">W ciągu ostatnich rozdziale nasz zostały ładowania danych z naszej bazie danych i wyświetlanie go.</span><span class="sxs-lookup"><span data-stu-id="196c3-110">In the past chapter, we were loading data from our database and displaying it.</span></span> <span data-ttu-id="196c3-111">W tym rozdziale firma Microsoft będzie także włączyć, edytowanie danych.</span><span class="sxs-lookup"><span data-stu-id="196c3-111">In this chapter, we'll also enable editing the data.</span></span>

## <a name="creating-the-storemanagercontroller"></a><span data-ttu-id="196c3-112">Tworzenie StoreManagerController</span><span class="sxs-lookup"><span data-stu-id="196c3-112">Creating the StoreManagerController</span></span>

<span data-ttu-id="196c3-113">Rozpocznie się przez utworzenie nowego kontrolera o nazwie **StoreManagerController**.</span><span class="sxs-lookup"><span data-stu-id="196c3-113">We'll begin by creating a new controller called **StoreManagerController**.</span></span> <span data-ttu-id="196c3-114">Dla tego kontrolera firma Microsoft będzie można korzystanie z funkcji szkieletów dostępnych w aktualizacji narzędzi programu ASP.NET MVC 3.</span><span class="sxs-lookup"><span data-stu-id="196c3-114">For this controller, we will be taking advantage of the Scaffolding features available in the ASP.NET MVC 3 Tools Update.</span></span> <span data-ttu-id="196c3-115">Ustaw opcje dla okna dialogowego Dodawanie kontrolera, jak pokazano poniżej.</span><span class="sxs-lookup"><span data-stu-id="196c3-115">Set the options for the Add Controller dialog as shown below.</span></span>

![](mvc-music-store-part-5/_static/image1.png)

<span data-ttu-id="196c3-116">Po kliknięciu przycisku Dodaj zobaczysz, że mechanizm szkieletów ASP.NET MVC 3 jest dobrym ilość pracy możesz:</span><span class="sxs-lookup"><span data-stu-id="196c3-116">When you click the Add button, you'll see that the ASP.NET MVC 3 scaffolding mechanism does a good amount of work for you:</span></span>

- <span data-ttu-id="196c3-117">Tworzy nowy StoreManagerController z zmiennej lokalnej programu Entity Framework</span><span class="sxs-lookup"><span data-stu-id="196c3-117">It creates the new StoreManagerController with a local Entity Framework variable</span></span>
- <span data-ttu-id="196c3-118">Dodaje StoreManager folder do folderu widoków projektu</span><span class="sxs-lookup"><span data-stu-id="196c3-118">It adds a StoreManager folder to the project's Views folder</span></span>
- <span data-ttu-id="196c3-119">Dodawany widok Create.cshtml, Delete.cshtml Details.cshtml, Edit.cshtml i Index.cshtml, silnie typizowaną do klasy albumu</span><span class="sxs-lookup"><span data-stu-id="196c3-119">It adds Create.cshtml, Delete.cshtml, Details.cshtml, Edit.cshtml, and Index.cshtml view, strongly typed to the Album class</span></span>

![](mvc-music-store-part-5/_static/image2.png)

<span data-ttu-id="196c3-120">Nowa klasa kontrolera StoreManager obejmuje CRUD (tworzenia, odczytu, aktualizowanie i usuwanie) akcji kontrolera, które wiedzieć, jak pracować z Album klasa modelu i użyć naszego kontekstu programu Entity Framework dla dostępu do bazy danych.</span><span class="sxs-lookup"><span data-stu-id="196c3-120">The new StoreManager controller class includes CRUD (create, read, update, delete) controller actions which know how to work with the Album model class and use our Entity Framework context for database access.</span></span>

## <a name="modifying-a-scaffolded-view"></a><span data-ttu-id="196c3-121">Modyfikowanie szkieletu widoku</span><span class="sxs-lookup"><span data-stu-id="196c3-121">Modifying a Scaffolded View</span></span>

<span data-ttu-id="196c3-122">Należy pamiętać, że podczas ten kod został wygenerowany w firmie Microsoft, jest standardowego kodu platformy ASP.NET MVC, tak jak zostały możemy zapisu w tym samouczku.</span><span class="sxs-lookup"><span data-stu-id="196c3-122">It's important to remember that, while this code was generated for us, it's standard ASP.NET MVC code, just like we've been writing throughout this tutorial.</span></span> <span data-ttu-id="196c3-123">Ma ona przeznaczona do zapisania należy poświęcić czas na zapisywanie schematyczny kod kontrolera i ręczne tworzenie widoków silnie typizowaną, ale nie jest to typ wygenerowanego kodu może przejrzane poprzedzone znakiem kierunek ostrzeżeń w komentarze na temat sposobu nie może zmienić Kod.</span><span class="sxs-lookup"><span data-stu-id="196c3-123">It's intended to save you the time you'd spend on writing boilerplate controller code and creating the strongly typed views manually, but this isn't the kind of generated code you may have seen prefaced with dire warnings in comments about how you mustn't change the code.</span></span> <span data-ttu-id="196c3-124">To jest kod i oczekiwaniami je zmienić.</span><span class="sxs-lookup"><span data-stu-id="196c3-124">This is your code, and you're expected to change it.</span></span>

<span data-ttu-id="196c3-125">Tak Zacznijmy szybkie edytowanie widoku indeksu StoreManager (/ Views/StoreManager/Index.cshtml).</span><span class="sxs-lookup"><span data-stu-id="196c3-125">So, let's start with a quick edit to the StoreManager Index view (/Views/StoreManager/Index.cshtml).</span></span> <span data-ttu-id="196c3-126">Ten widok przedstawia tabeli, które wymieniono albumów w naszym magazynie z edycji / szczegółów / Usuń linki i zawiera właściwości publicznej albumu.</span><span class="sxs-lookup"><span data-stu-id="196c3-126">This view will display a table which lists the Albums in our store with Edit / Details / Delete links, and includes the Album's public properties.</span></span> <span data-ttu-id="196c3-127">Usuniemy pole AlbumArtUrl, ponieważ nie jest to przydatne w przypadku tego ekranu.</span><span class="sxs-lookup"><span data-stu-id="196c3-127">We'll remove the AlbumArtUrl field, as it's not very useful in this display.</span></span> <span data-ttu-id="196c3-128">W &lt;tabeli&gt; sekcji kodu widoku, Usuń &lt;th&gt; i &lt;td&gt; otaczającego odwołania AlbumArtUrl opisane w poniższych wierszach wyróżnionych elementów:</span><span class="sxs-lookup"><span data-stu-id="196c3-128">In &lt;table&gt; section of the view code, remove the &lt;th&gt; and &lt;td&gt; elements surrounding AlbumArtUrl references, as indicated by the highlighted lines below:</span></span>

[!code-cshtml[Main](mvc-music-store-part-5/samples/sample1.cshtml)]

<span data-ttu-id="196c3-129">Kod widoku zmodyfikowany będzie wyglądać następująco:</span><span class="sxs-lookup"><span data-stu-id="196c3-129">The modified view code will appear as follows:</span></span>

[!code-cshtml[Main](mvc-music-store-part-5/samples/sample2.cshtml)]

## <a name="a-first-look-at-the-store-manager"></a><span data-ttu-id="196c3-130">Pierwszy przyjrzeć się Menedżer magazynu</span><span class="sxs-lookup"><span data-stu-id="196c3-130">A first look at the Store Manager</span></span>

<span data-ttu-id="196c3-131">Teraz uruchom aplikację i przejdź do/StoreManager /.</span><span class="sxs-lookup"><span data-stu-id="196c3-131">Now run the application and browse to /StoreManager/.</span></span> <span data-ttu-id="196c3-132">Spowoduje to wyświetlenie możemy po modyfikacji, przedstawiający listę albumów w magazynie wraz z łączami do edytowania szczegółów i usuwania indeksu Menedżera magazynu.</span><span class="sxs-lookup"><span data-stu-id="196c3-132">This displays the Store Manager Index we just modified, showing a list of the albums in the store with links to Edit, Details, and Delete.</span></span>

![](mvc-music-store-part-5/_static/image3.png)

<span data-ttu-id="196c3-133">Kliknięcie łącza edycji wyświetli formularz edycji z polami albumu, łącznie z list rozwijanych dla wykonawcy i rodzaju.</span><span class="sxs-lookup"><span data-stu-id="196c3-133">Clicking the Edit link displays an edit form with fields for the Album, including dropdowns for Genre and Artist.</span></span>

![](mvc-music-store-part-5/_static/image4.png)

<span data-ttu-id="196c3-134">Kliknij łącze "Powrót do listy" u dołu, a następnie kliknij łącze Szczegóły albumu.</span><span class="sxs-lookup"><span data-stu-id="196c3-134">Click the "Back to List" link at the bottom, then click on the Details link for an Album.</span></span> <span data-ttu-id="196c3-135">Spowoduje to wyświetlenie szczegółowych informacji o poszczególnych albumu.</span><span class="sxs-lookup"><span data-stu-id="196c3-135">This displays the detail information for an individual Album.</span></span>

![](mvc-music-store-part-5/_static/image5.png)

<span data-ttu-id="196c3-136">Ponownie kliknij przycisk Wstecz do listy łącza, a następnie kliknij łącze Usuń.</span><span class="sxs-lookup"><span data-stu-id="196c3-136">Again, click the Back to List link, then click on a Delete link.</span></span> <span data-ttu-id="196c3-137">Zostaną wyświetlone okno dialogowe potwierdzenia, pokazywanie szczegółów albumu i pytaniem, czy już wszystko się, że jeśli chcesz go usunąć.</span><span class="sxs-lookup"><span data-stu-id="196c3-137">This displays a confirmation dialog, showing the album details and asking if we're sure we want to delete it.</span></span>

![](mvc-music-store-part-5/_static/image6.png)

<span data-ttu-id="196c3-138">Kliknięcie przycisku Usuń u dołu spowoduje to usunięcie album i powrót do strony indeksu przedstawiono albumu usunięte.</span><span class="sxs-lookup"><span data-stu-id="196c3-138">Clicking the Delete button at the bottom will delete the album and return you to the Index page, which shows the album deleted.</span></span>

<span data-ttu-id="196c3-139">Firma Microsoft nie wszystko gotowe przy użyciu Menedżera magazynu, ale mamy pracy kontrolera i widoku Kod operacji CRUD uruchomić z.</span><span class="sxs-lookup"><span data-stu-id="196c3-139">We're not done with the Store Manager, but we have working controller and view code for the CRUD operations to start from.</span></span>

## <a name="looking-at-the-store-manager-controller-code"></a><span data-ttu-id="196c3-140">Spojrzenie na kod kontrolera Menedżera magazynu</span><span class="sxs-lookup"><span data-stu-id="196c3-140">Looking at the Store Manager Controller code</span></span>

<span data-ttu-id="196c3-141">Kontroler Menedżera magazynu zawiera dobrej ilość kodu.</span><span class="sxs-lookup"><span data-stu-id="196c3-141">The Store Manager Controller contains a good amount of code.</span></span> <span data-ttu-id="196c3-142">Przejdź przez to od góry do dołu.</span><span class="sxs-lookup"><span data-stu-id="196c3-142">Let's go through this from top to bottom.</span></span> <span data-ttu-id="196c3-143">Kontroler zawiera niektóre standardowe przestrzeni nazw dla kontrolera MVC, a także odwołania do przestrzeni nazw naszych modeli.</span><span class="sxs-lookup"><span data-stu-id="196c3-143">The controller includes some standard namespaces for an MVC controller, as well as a reference to our Models namespace.</span></span> <span data-ttu-id="196c3-144">Kontroler ma prywatnej wystąpienie MusicStoreEntities używany przez wszystkie akcje kontrolera dla dostępu do danych.</span><span class="sxs-lookup"><span data-stu-id="196c3-144">The controller has a private instance of MusicStoreEntities, used by each of the controller actions for data access.</span></span>

[!code-csharp[Main](mvc-music-store-part-5/samples/sample3.cs)]

### <a name="store-manager-index-and-details-actions"></a><span data-ttu-id="196c3-145">Przechowywanie działania Menedżera indeksu i szczegóły</span><span class="sxs-lookup"><span data-stu-id="196c3-145">Store Manager Index and Details actions</span></span>

<span data-ttu-id="196c3-146">Widok indeksu powoduje pobranie listy albumy, w tym każdego albumu przywoływanego wykonawcy i Genre, jak widzieliśmy wcześniej podczas pracy w metodzie Przeglądaj magazynu.</span><span class="sxs-lookup"><span data-stu-id="196c3-146">The index view retrieves a list of Albums, including each album's referenced Genre and Artist information, as we previously saw when working on the Store Browse method.</span></span> <span data-ttu-id="196c3-147">Widok indeksu jest po odwołania do obiektów połączonych, dzięki czemu będzie możliwe wyświetlenie każdego albumu Genre nazwy i wykonawcy, więc kontrolera są wydajne i wykonywanie zapytania dla tych informacji w oryginalne żądanie.</span><span class="sxs-lookup"><span data-stu-id="196c3-147">The Index view is following the references to the linked objects so that it can display each album's Genre name and Artist name, so the controller is being efficient and querying for this information in the original request.</span></span>

[!code-csharp[Main](mvc-music-store-part-5/samples/sample4.cs)]

<span data-ttu-id="196c3-148">Akcji kontrolera szczegóły kontrolera StoreManager działa tak samo jak informowaliśmy wcześniej — wysyła zapytanie albumu akcji szczegółów kontrolera magazynu według Identyfikatora przy użyciu metody Find(), zwraca go do widoku.</span><span class="sxs-lookup"><span data-stu-id="196c3-148">The StoreManager Controller's Details controller action works exactly the same as the Store Controller Details action we wrote previously - it queries for the Album by ID using the Find() method, then returns it to the view.</span></span>

[!code-csharp[Main](mvc-music-store-part-5/samples/sample5.cs)]

### <a name="the-create-action-methods"></a><span data-ttu-id="196c3-149">Tworzenie metody akcji</span><span class="sxs-lookup"><span data-stu-id="196c3-149">The Create Action Methods</span></span>

<span data-ttu-id="196c3-150">Tworzenie metody akcji są nieco inne niż te, które firma Microsoft w tym samouczku do tej pory, ponieważ obsługują dane wejściowe formularza.</span><span class="sxs-lookup"><span data-stu-id="196c3-150">The Create action methods are a little different from ones we've seen so far, because they handle form input.</span></span> <span data-ttu-id="196c3-151">Gdy użytkownik odwiedza najpierw /StoreManager/Utwórz/będą one wyświetlane pusty formularz.</span><span class="sxs-lookup"><span data-stu-id="196c3-151">When a user first visits /StoreManager/Create/ they will be shown an empty form.</span></span> <span data-ttu-id="196c3-152">Ta strona HTML będzie zawierać &lt;formularza&gt; element, który zawiera listy rozwijanej i pola tekstowego danych wejściowych elementów, w którym mogą oni wprowadzić szczegóły albumu.</span><span class="sxs-lookup"><span data-stu-id="196c3-152">This HTML page will contain a &lt;form&gt; element that contains dropdown and textbox input elements where they can enter the album's details.</span></span>

<span data-ttu-id="196c3-153">Po użytkownik wypełnia albumu wartości formularza, ich naciśnij przycisk "Zapisz" do przesyłania tych zmian z powrotem do naszej aplikacji do zapisania w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="196c3-153">After the user fills in the Album form values, they can press the "Save" button to submit these changes back to our application to save within the database.</span></span> <span data-ttu-id="196c3-154">Gdy użytkownik naciśnie przycisk "Zapisz" &lt;formularza&gt; przeprowadzi HTTP POST do /StoreManager/Create lub adres URL i przesłać &lt;formularza&gt; wartości jako część POST protokołu HTTP.</span><span class="sxs-lookup"><span data-stu-id="196c3-154">When the user presses the "save" button the &lt;form&gt; will perform an HTTP-POST back to the /StoreManager/Create/ URL and submit the &lt;form&gt; values as part of the HTTP-POST.</span></span>

<span data-ttu-id="196c3-155">ASP.NET MVC pozwala łatwo podzielić przez włączenie firmie Microsoft w celu wykonania dwóch oddzielnych metod akcji "Utwórz" w obrębie klasy Nasze StoreManagerController — jeden do obsługi początkowej Przeglądaj HTTP GET, aby /StoreManager/Create logikę tych dwóch scenariuszy wywołania adresu URL Lub adres URL, a drugi do obsługi protokołu HTTP POST do przesłanych zmian.</span><span class="sxs-lookup"><span data-stu-id="196c3-155">ASP.NET MVC allows us to easily split up the logic of these two URL invocation scenarios by enabling us to implement two separate "Create" action methods within our StoreManagerController class – one to handle the initial HTTP-GET browse to the /StoreManager/Create/ URL, and the other to handle the HTTP-POST of the submitted changes.</span></span>

### <a name="passing-information-to-a-view-using-viewbag"></a><span data-ttu-id="196c3-156">Przekazanie informacji do widoku przy użyciu elementów ViewBag</span><span class="sxs-lookup"><span data-stu-id="196c3-156">Passing information to a View using ViewBag</span></span>

<span data-ttu-id="196c3-157">Firma Microsoft używano wcześniej w tym samouczku obiekt ViewBag, ale nie zawsze mówię większość informacji na ten temat.</span><span class="sxs-lookup"><span data-stu-id="196c3-157">We've used the ViewBag earlier in this tutorial, but haven't talked much about it.</span></span> <span data-ttu-id="196c3-158">Obiekt ViewBag umożliwia firmie Microsoft w celu przekazywania informacji do widoku bez użycia obiektu jednoznacznie modelu.</span><span class="sxs-lookup"><span data-stu-id="196c3-158">The ViewBag allows us to pass information to the view without using a strongly typed model object.</span></span> <span data-ttu-id="196c3-159">W takim przypadku musi naszych akcji kontrolera Edytuj GET protokołu HTTP do przekazywania zarówno listę Genres i artystów do formularza, aby wypełnić listę rozwijaną i zwraca je jako elementy obiekt ViewBag jest najprostszym sposobem, aby to zrobić.</span><span class="sxs-lookup"><span data-stu-id="196c3-159">In this case, our Edit HTTP-GET controller action needs to pass both a list of Genres and Artists to the form to populate the dropdowns, and the simplest way to do that is to return them as ViewBag items.</span></span>

<span data-ttu-id="196c3-160">Obiekt ViewBag jest obiekt dynamiczny, co oznacza, że możesz wpisać ViewBag.Foo lub ViewBag.YourNameHere bez pisania kodu, aby zdefiniować te właściwości.</span><span class="sxs-lookup"><span data-stu-id="196c3-160">The ViewBag is a dynamic object, meaning that you can type ViewBag.Foo or ViewBag.YourNameHere without writing code to define those properties.</span></span> <span data-ttu-id="196c3-161">W tym przypadku kontrolera kodzie użyto ViewBag.GenreId i ViewBag.ArtistId tak, aby wartości listy rozwijanej przesłane za pomocą formularza GenreId i ArtistId, które są właściwości albumu, który będzie można ich ustawienia.</span><span class="sxs-lookup"><span data-stu-id="196c3-161">In this case, the controller code uses ViewBag.GenreId and ViewBag.ArtistId so that the dropdown values submitted with the form will be GenreId and ArtistId, which are the Album properties they will be setting.</span></span>

<span data-ttu-id="196c3-162">Te listy rozwijanej wartości są zwracane do formularza za pomocą obiektu SelectList, który jest przeznaczony tylko dla tego celu.</span><span class="sxs-lookup"><span data-stu-id="196c3-162">These dropdown values are returned to the form using the SelectList object, which is built just for that purpose.</span></span> <span data-ttu-id="196c3-163">Jest to realizowane przy użyciu kodu w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="196c3-163">This is done using code like this:</span></span>

[!code-csharp[Main](mvc-music-store-part-5/samples/sample6.cs)]

<span data-ttu-id="196c3-164">Jak widać z kodu metody akcji, trzy parametry są używany do utworzenia tego obiektu:</span><span class="sxs-lookup"><span data-stu-id="196c3-164">As you can see from the action method code, three parameters are being used to create this object:</span></span>

- <span data-ttu-id="196c3-165">Lista elementów, które będą wyświetlane listy rozwijanej.</span><span class="sxs-lookup"><span data-stu-id="196c3-165">The list of items the dropdown will be displaying.</span></span> <span data-ttu-id="196c3-166">Należy pamiętać, że nie jest to po prostu określonym ciągiem - możemy przechodząc listy gatunkami muzyki.</span><span class="sxs-lookup"><span data-stu-id="196c3-166">Note that this isn't just a string - we're passing a list of Genres.</span></span>
- <span data-ttu-id="196c3-167">Następny parametr przekazywany do SelectList jest wybrana wartość.</span><span class="sxs-lookup"><span data-stu-id="196c3-167">The next parameter being passed to the SelectList is the Selected Value.</span></span> <span data-ttu-id="196c3-168">Ten sposób SelectList wie jak wstępnie wybierz element na liście.</span><span class="sxs-lookup"><span data-stu-id="196c3-168">This how the SelectList knows how to pre-select an item in the list.</span></span> <span data-ttu-id="196c3-169">Są to ułatwia zrozumienie, kiedy dokładnie w formularzu edycji jest bardzo podobne.</span><span class="sxs-lookup"><span data-stu-id="196c3-169">This will be easier to understand when we look at the Edit form, which is pretty similar.</span></span>
- <span data-ttu-id="196c3-170">Ostatni parametr jest właściwość do wyświetlenia.</span><span class="sxs-lookup"><span data-stu-id="196c3-170">The final parameter is the property to be displayed.</span></span> <span data-ttu-id="196c3-171">W takim przypadku tego wskazująca, że właściwość Genre.Name jest co będzie pokazywana użytkownikowi.</span><span class="sxs-lookup"><span data-stu-id="196c3-171">In this case, this is indicating that the Genre.Name property is what will be shown to the user.</span></span>

<span data-ttu-id="196c3-172">Z tym pamiętać następnie utworzyć HTTP GET akcja jest bardzo prosty — dwa SelectLists są dodawane do obiekt ViewBag i żaden obiekt modelu jest przekazywany do formularza (ponieważ go nie został jeszcze utworzony).</span><span class="sxs-lookup"><span data-stu-id="196c3-172">With that in mind, then, the HTTP-GET Create action is pretty simple - two SelectLists are added to the ViewBag, and no model object is passed to the form (since it hasn't been created yet).</span></span>

[!code-csharp[Main](mvc-music-store-part-5/samples/sample7.cs)]

### <a name="html-helpers-to-display-the-drop-downs-in-the-create-view"></a><span data-ttu-id="196c3-173">Pomocników HTML do wyświetlenia szczegółu Drop w tworzenie widoku</span><span class="sxs-lookup"><span data-stu-id="196c3-173">HTML Helpers to display the Drop Downs in the Create View</span></span>

<span data-ttu-id="196c3-174">Ponieważ zajmowaliśmy jak listy rozwijanej wartości są przekazywane do widoku, Spójrzmy szybki w widoku, aby zobaczyć, jak te wartości są wyświetlane.</span><span class="sxs-lookup"><span data-stu-id="196c3-174">Since we've talked about how the drop down values are passed to the view, let's take a quick look at the view to see how those values are displayed.</span></span> <span data-ttu-id="196c3-175">W widoku kodu (/ Views/StoreManager/Create.cshtml), pojawi się następujące wywołanie dotyczące wyświetlania listy Genre w dół.</span><span class="sxs-lookup"><span data-stu-id="196c3-175">In the view code (/Views/StoreManager/Create.cshtml), you'll see the following call is made to display the Genre drop down.</span></span>

[!code-cshtml[Main](mvc-music-store-part-5/samples/sample8.cshtml)]

<span data-ttu-id="196c3-176">Jest to nazywane pomocnika kodu HTML — metodę narzędzia, która wykonuje wspólne zadania widoku.</span><span class="sxs-lookup"><span data-stu-id="196c3-176">This is known as an HTML Helper - a utility method which performs a common view task.</span></span> <span data-ttu-id="196c3-177">Pomocników HTML są przydatne w przypadku aktualizowania naszego kodu widoku zwięzły i do odczytu.</span><span class="sxs-lookup"><span data-stu-id="196c3-177">HTML Helpers are very useful in keeping our view code concise and readable.</span></span> <span data-ttu-id="196c3-178">Pomocnik Html.DropDownList są dostarczane przez program ASP.NET MVC, ale jako zajmiemy się tym później można utworzyć własne elementy pomocnicze dla widoku kodu, który firma Microsoft będzie ponowne użycie w naszej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="196c3-178">The Html.DropDownList helper is provided by ASP.NET MVC, but as we'll see later it's possible to create our own helpers for view code we'll reuse in our application.</span></span>

<span data-ttu-id="196c3-179">Wywołanie Html.DropDownList właśnie musi być polecenie dwie czynności — w przypadku gdy get listy, aby wyświetlić i jakie korzyści (jeśli istnieje), należy wstępnie wybrane.</span><span class="sxs-lookup"><span data-stu-id="196c3-179">The Html.DropDownList call just needs to be told two things - where to get the list to display, and what value (if any) should be pre-selected.</span></span> <span data-ttu-id="196c3-180">Pierwszy parametr GenreId, określa, że lista DropDownList do wyszukania wartość o nazwie GenreId w modelu lub obiekt ViewBag.</span><span class="sxs-lookup"><span data-stu-id="196c3-180">The first parameter, GenreId, tells the DropDownList to look for a value named GenreId in either the model or ViewBag.</span></span> <span data-ttu-id="196c3-181">Drugi parametr jest służy do wskazania wartości, aby wyświetlić, początkowo wybrane na liście rozwijanej.</span><span class="sxs-lookup"><span data-stu-id="196c3-181">The second parameter is used to indicate the value to show as initially selected in the drop down list.</span></span> <span data-ttu-id="196c3-182">Ponieważ ten formularz jest tworzenie formularza, nie mają być instalowane żadnej wartości, a String.Empty jest przekazywana.</span><span class="sxs-lookup"><span data-stu-id="196c3-182">Since this form is a Create form, there's no value to be preselected and String.Empty is passed.</span></span>

### <a name="handling-the-posted-form-values"></a><span data-ttu-id="196c3-183">Obsługa wartości formularza przesłane</span><span class="sxs-lookup"><span data-stu-id="196c3-183">Handling the Posted Form values</span></span>

<span data-ttu-id="196c3-184">Jak wspomniano przed, istnieją dwie metody akcji związane z każdego formularza.</span><span class="sxs-lookup"><span data-stu-id="196c3-184">As we discussed before, there are two action methods associated with each form.</span></span> <span data-ttu-id="196c3-185">Pierwszy obsługuje żądania HTTP GET i zostanie wyświetlony formularz.</span><span class="sxs-lookup"><span data-stu-id="196c3-185">The first handles the HTTP-GET request and displays the form.</span></span> <span data-ttu-id="196c3-186">Drugi obsługuje żądania POST protokołu HTTP, który zawiera wartości przesłanego formularza.</span><span class="sxs-lookup"><span data-stu-id="196c3-186">The second handles the HTTP-POST request, which contains the submitted form values.</span></span> <span data-ttu-id="196c3-187">Należy zauważyć, że kontroler akcji ma atrybut [HttpPost], który poinformuje platformę ASP.NET MVC, że tylko powinien odpowiadać na żądania HTTP POST.</span><span class="sxs-lookup"><span data-stu-id="196c3-187">Notice that controller action has an [HttpPost] attribute, which tells ASP.NET MVC that it should only respond to HTTP-POST requests.</span></span>

[!code-csharp[Main](mvc-music-store-part-5/samples/sample9.cs)]

<span data-ttu-id="196c3-188">Ta akcja ma cztery obowiązki:</span><span class="sxs-lookup"><span data-stu-id="196c3-188">This action has four responsibilities:</span></span>

- 1. <span data-ttu-id="196c3-189">Odczytać wartości formularza</span><span class="sxs-lookup"><span data-stu-id="196c3-189">Read the form values</span></span>
- 2. <span data-ttu-id="196c3-190">Sprawdź, czy wartości formularza przekazać reguł sprawdzania poprawności</span><span class="sxs-lookup"><span data-stu-id="196c3-190">Check if the form values pass any validation rules</span></span>
- 3. <span data-ttu-id="196c3-191">Jeśli przesyłania formularza jest nieprawidłowy, Zapisz dane i wyświetlić zaktualizowaną listę</span><span class="sxs-lookup"><span data-stu-id="196c3-191">If the form submission is valid, save the data and display the updated list</span></span>
- 4. <span data-ttu-id="196c3-192">Jeśli przesłanie formularza nie jest prawidłowy, Wyświetl ponownie formularz błędy sprawdzania poprawności</span><span class="sxs-lookup"><span data-stu-id="196c3-192">If the form submission is not valid, redisplay the form with validation errors</span></span>

#### <a name="reading-form-values-with-model-binding"></a><span data-ttu-id="196c3-193">Odczytywanie wartości formularza z powiązaniem modelu</span><span class="sxs-lookup"><span data-stu-id="196c3-193">Reading Form Values with Model Binding</span></span>

<span data-ttu-id="196c3-194">Przesłanie formularza, która zawiera wartości GenreId i ArtistId (z listy rozwijanej) i wartości tekstowe dla tytułu, ceny i AlbumArtUrl przetwarza akcji kontrolera.</span><span class="sxs-lookup"><span data-stu-id="196c3-194">The controller action is processing a form submission that includes values for GenreId and ArtistId (from the drop down list) and textbox values for Title, Price, and AlbumArtUrl.</span></span> <span data-ttu-id="196c3-195">Użytkownik może uzyskać bezpośredniego dostępu do wartości formularza, lepszym rozwiązaniem jest korzystanie z funkcji powiązania modelu wbudowanych w platformy ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="196c3-195">While it's possible to directly access form values, a better approach is to use the Model Binding capabilities built into ASP.NET MVC.</span></span> <span data-ttu-id="196c3-196">Podczas działania kontrolera przyjmuje jako parametr typu modelu, ASP.NET MVC podejmie próbę wypełnienia obiektu tego typu za pomocą formularza danych wejściowych (a także wartości trasy i querystring).</span><span class="sxs-lookup"><span data-stu-id="196c3-196">When a controller action takes a model type as a parameter, ASP.NET MVC will attempt to populate an object of that type using form inputs (as well as route and querystring values).</span></span> <span data-ttu-id="196c3-197">Dzieje się tak, wyszukując wartości, których nazwy są zgodne np. właściwości obiektu modelu, ustawiając wartość GenreId nowy obiekt albumu wygląda danych wejściowych o nazwie GenreId.</span><span class="sxs-lookup"><span data-stu-id="196c3-197">It does this by looking for values whose names match properties of the model object, e.g. when setting the new Album object's GenreId value, it looks for an input with the name GenreId.</span></span> <span data-ttu-id="196c3-198">Podczas tworzenia widoków w programie ASP.NET MVC przy użyciu standardowych metod formularze będą zawsze być renderowany przy użyciu nazwy właściwości jako nazwy pola wejściowego, tak to nazwy pól będzie po prostu zgodne.</span><span class="sxs-lookup"><span data-stu-id="196c3-198">When you create views using the standard methods in ASP.NET MVC, the forms will always be rendered using property names as input field names, so this the field names will just match up.</span></span>

#### <a name="validating-the-model"></a><span data-ttu-id="196c3-199">Sprawdzanie poprawności modelu</span><span class="sxs-lookup"><span data-stu-id="196c3-199">Validating the Model</span></span>

<span data-ttu-id="196c3-200">Model została zweryfikowana za pomocą prostego wywołania ModelState.IsValid.</span><span class="sxs-lookup"><span data-stu-id="196c3-200">The model is validated with a simple call to ModelState.IsValid.</span></span> <span data-ttu-id="196c3-201">Firma Microsoft nie dodano żadnych reguł sprawdzania poprawności do naszej klasy albumu jeszcze - robimy które bit - teraz tak to sprawdzenie nie ma wiele zrobić.</span><span class="sxs-lookup"><span data-stu-id="196c3-201">We haven't added any validation rules to our Album class yet - we'll do that in a bit - so right now this check doesn't have much to do.</span></span> <span data-ttu-id="196c3-202">Co to jest ważne jest, czy sprawdzanie ModelStat.IsValid będzie dostosować do reguł sprawdzania poprawności, które testujemy w modelu, więc przyszłe zmiany reguł sprawdzania poprawności nie wymagają żadnych aktualizacji do kodu akcji kontrolera.</span><span class="sxs-lookup"><span data-stu-id="196c3-202">What's important is that this ModelStat.IsValid check will adapt to the validation rules we put on our model, so future changes to validation rules won't require any updates to the controller action code.</span></span>

#### <a name="saving-the-submitted-values"></a><span data-ttu-id="196c3-203">Zapisywanie wartości przesłane</span><span class="sxs-lookup"><span data-stu-id="196c3-203">Saving the submitted values</span></span>

<span data-ttu-id="196c3-204">Przesłanie formularza przeszedł sprawdzania poprawności, jest czas, aby zapisać wartości w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="196c3-204">If the form submission passes validation, it's time to save the values to the database.</span></span> <span data-ttu-id="196c3-205">Z programu Entity Framework, który wymaga dodania modelu do kolekcji albumów i wywołanie metody SaveChanges.</span><span class="sxs-lookup"><span data-stu-id="196c3-205">With Entity Framework, that just requires adding the model to the Albums collection and calling SaveChanges.</span></span>

[!code-csharp[Main](mvc-music-store-part-5/samples/sample10.cs)]

<span data-ttu-id="196c3-206">Entity Framework generuje odpowiednich poleceń SQL, aby zachować wartość.</span><span class="sxs-lookup"><span data-stu-id="196c3-206">Entity Framework generates the appropriate SQL commands to persist the value.</span></span> <span data-ttu-id="196c3-207">Po zapisaniu danych, możemy przekierowania powrót do listy albumy, widzimy naszej aktualizacji.</span><span class="sxs-lookup"><span data-stu-id="196c3-207">After saving the data, we redirect back to the list of Albums so we can see our update.</span></span> <span data-ttu-id="196c3-208">Jest to realizowane przez zwrócenie RedirectToAction o nazwie akcji kontrolera, którą chcemy udostępnić wyświetlane.</span><span class="sxs-lookup"><span data-stu-id="196c3-208">This is done by returning RedirectToAction with the name of the controller action we want displayed.</span></span> <span data-ttu-id="196c3-209">W takim przypadku to metoda indeksu.</span><span class="sxs-lookup"><span data-stu-id="196c3-209">In this case, that's the Index method.</span></span>

#### <a name="displaying-invalid-form-submissions-with-validation-errors"></a><span data-ttu-id="196c3-210">Wyświetlanie przesłanych nieprawidłową postać z błędami sprawdzania poprawności</span><span class="sxs-lookup"><span data-stu-id="196c3-210">Displaying invalid form submissions with Validation Errors</span></span>

<span data-ttu-id="196c3-211">W przypadku nieprawidłową postać danych wejściowych wartości listy rozwijanej są dodawane do elementów ViewBag (tak jak w przypadku HTTP GET) i powiązany model wartości są przekazywane z powrotem do widoku do wyświetlenia.</span><span class="sxs-lookup"><span data-stu-id="196c3-211">In the case of invalid form input, the dropdown values are added to the ViewBag (as in the HTTP-GET case) and the bound model values are passed back to the view for display.</span></span> <span data-ttu-id="196c3-212">Błędy sprawdzania poprawności są automatycznie wyświetlane przy użyciu @Html.ValidationMessageFor pomocnika kodu HTML.</span><span class="sxs-lookup"><span data-stu-id="196c3-212">Validation errors are automatically displayed using the @Html.ValidationMessageFor HTML Helper.</span></span>

#### <a name="testing-the-create-form"></a><span data-ttu-id="196c3-213">Testowanie formularza tworzenia</span><span class="sxs-lookup"><span data-stu-id="196c3-213">Testing the Create Form</span></span>

<span data-ttu-id="196c3-214">Aby przetestować ten limit, uruchom aplikację i przejdź do /StoreManager/Utwórz / — spowoduje to wyświetlenie pusty formularz, który został zwrócony przez metodę HTTP GET utworzyć StoreController.</span><span class="sxs-lookup"><span data-stu-id="196c3-214">To test this out, run the application and browse to /StoreManager/Create/ - this will show you the blank form which was returned by the StoreController Create HTTP-GET method.</span></span>

<span data-ttu-id="196c3-215">Wypełnij niektóre wartości, a następnie kliknij przycisk Utwórz, aby przesłać formularza.</span><span class="sxs-lookup"><span data-stu-id="196c3-215">Fill in some values and click the Create button to submit the form.</span></span>

![](mvc-music-store-part-5/_static/image7.png)

![](mvc-music-store-part-5/_static/image8.png)

### <a name="handling-edits"></a><span data-ttu-id="196c3-216">Obsługa zmiany</span><span class="sxs-lookup"><span data-stu-id="196c3-216">Handling Edits</span></span>

<span data-ttu-id="196c3-217">Edytowanie pary akcji (HTTP GET i POST protokołu HTTP) są bardzo podobne do tworzenia metod akcji, którą właśnie analizujemy.</span><span class="sxs-lookup"><span data-stu-id="196c3-217">The Edit action pair (HTTP-GET and HTTP-POST) are very similar to the Create action methods we just looked at.</span></span> <span data-ttu-id="196c3-218">Ponieważ Edycja scenariusz obejmuje pracę z istniejącego albumu, Edytuj HTTP GET metody ładuje albumu na podstawie parametru "id" przekazany za pomocą trasy.</span><span class="sxs-lookup"><span data-stu-id="196c3-218">Since the edit scenario involves working with an existing album, the Edit HTTP-GET method loads the Album based on the "id" parameter, passed in via the route.</span></span> <span data-ttu-id="196c3-219">Ten kod do pobierania albumy AlbumId jest taka sama, jak zostało wcześniej analizujemy w szczegóły akcji kontrolera.</span><span class="sxs-lookup"><span data-stu-id="196c3-219">This code for retrieving an album by AlbumId is the same as we've previously looked at in the Details controller action.</span></span> <span data-ttu-id="196c3-220">W przypadku tworzenia / metody HTTP GET listy rozwijanej wartości są zwracane przez obiekt ViewBag.</span><span class="sxs-lookup"><span data-stu-id="196c3-220">As with the Create / HTTP-GET method, the drop down values are returned via the ViewBag.</span></span> <span data-ttu-id="196c3-221">To pozwala zwrócić albumu naszych obiekt modelu do widoku (który jest silnie typizowaną do klasy albumu) podczas przejścia dodatkowe dane (np. lista gatunkami muzyki) za pośrednictwem obiekt ViewBag.</span><span class="sxs-lookup"><span data-stu-id="196c3-221">This allows us to return an Album as our model object to the view (which is strongly typed to the Album class) while passing additional data (e.g. a list of Genres) via the ViewBag.</span></span>

[!code-csharp[Main](mvc-music-store-part-5/samples/sample11.cs)]

<span data-ttu-id="196c3-222">Akcję POST protokołu HTTP edycji jest bardzo podobny do akcji tworzenia POST protokołu HTTP.</span><span class="sxs-lookup"><span data-stu-id="196c3-222">The Edit HTTP-POST action is very similar to the Create HTTP-POST action.</span></span> <span data-ttu-id="196c3-223">Jedyną różnicą jest to, że zamiast opcji dodawania nowego albumu do bazy danych. Kolekcja albumy, firma Microsoft jest znajdowania bieżące wystąpienie klasy Album przy użyciu bazy danych. Entry(album) i ustawienie stanu Modified.</span><span class="sxs-lookup"><span data-stu-id="196c3-223">The only difference is that instead of adding a new album to the db.Albums collection, we're finding the current instance of the Album using db.Entry(album) and setting its state to Modified.</span></span> <span data-ttu-id="196c3-224">Informuje Entity Framework, modyfikowania istniejącego albumu zamiast tworzenia nowej.</span><span class="sxs-lookup"><span data-stu-id="196c3-224">This tells Entity Framework that we are modifying an existing album as opposed to creating a new one.</span></span>

[!code-csharp[Main](mvc-music-store-part-5/samples/sample12.cs)]

<span data-ttu-id="196c3-225">Firma Microsoft testuje tę możliwość uruchamiania aplikacji i przeglądanie/StoreManger /, a następnie klikając link edycji dla albumu.</span><span class="sxs-lookup"><span data-stu-id="196c3-225">We can test this out by running the application and browsing to /StoreManger/, then clicking the Edit link for an album.</span></span>

![](mvc-music-store-part-5/_static/image9.png)

<span data-ttu-id="196c3-226">Spowoduje to wyświetlenie formularza edycji wyświetlany przez metodę GET HTTP edycji.</span><span class="sxs-lookup"><span data-stu-id="196c3-226">This displays the Edit form shown by the Edit HTTP-GET method.</span></span> <span data-ttu-id="196c3-227">Wypełnij niektóre wartości, a następnie kliknij przycisk Zapisz.</span><span class="sxs-lookup"><span data-stu-id="196c3-227">Fill in some values and click the Save button.</span></span>

![](mvc-music-store-part-5/_static/image10.png)

<span data-ttu-id="196c3-228">Posty na formularzu, zapisuje te wartości i zwraca nam do listy albumy, pokazujący, że wartości zostały zaktualizowane.</span><span class="sxs-lookup"><span data-stu-id="196c3-228">This posts the form, saves the values, and returns us to the Album list, showing that the values were updated.</span></span>

![](mvc-music-store-part-5/_static/image11.png)

### <a name="handling-deletion"></a><span data-ttu-id="196c3-229">Obsługa usuwania</span><span class="sxs-lookup"><span data-stu-id="196c3-229">Handling Deletion</span></span>

<span data-ttu-id="196c3-230">Usuwanie jest zgodny ze wzorcem tej samej edycji i Utwórz przy użyciu jednego kontrolera akcji do wyświetlania formularza potwierdzenia i innej akcji kontrolera do obsługi przesyłania formularza.</span><span class="sxs-lookup"><span data-stu-id="196c3-230">Deletion follows the same pattern as Edit and Create, using one controller action to display the confirmation form, and another controller action to handle the form submission.</span></span>

<span data-ttu-id="196c3-231">Akcja kontrolera HTTP GET usunąć jest dokładnie taka sama naszych poprzedniej akcji kontrolera szczegóły Menedżera magazynu.</span><span class="sxs-lookup"><span data-stu-id="196c3-231">The HTTP-GET Delete controller action is exactly the same as our previous Store Manager Details controller action.</span></span>

[!code-csharp[Main](mvc-music-store-part-5/samples/sample13.cs)]

<span data-ttu-id="196c3-232">Formularz, który jest silnie typizowane wyświetli typu albumy, przy użyciu Delete wyświetlanie zawartości szablonu.</span><span class="sxs-lookup"><span data-stu-id="196c3-232">We display a form that's strongly typed to an Album type, using the Delete view content template.</span></span>

![](mvc-music-store-part-5/_static/image12.png)

<span data-ttu-id="196c3-233">Szablon Usuń pokazuje wszystkie pola dla modelu, ale firma Microsoft może uprościć tego down sobą.</span><span class="sxs-lookup"><span data-stu-id="196c3-233">The Delete template shows all the fields for the model, but we can simplify that down quite a bit.</span></span> <span data-ttu-id="196c3-234">Zmień kod widoku w /Views/StoreManager/Delete.cshtml do następującego.</span><span class="sxs-lookup"><span data-stu-id="196c3-234">Change the view code in /Views/StoreManager/Delete.cshtml to the following.</span></span>

[!code-cshtml[Main](mvc-music-store-part-5/samples/sample14.cshtml)]

<span data-ttu-id="196c3-235">Spowoduje to wyświetlenie uproszczony potwierdzenie usunięcia.</span><span class="sxs-lookup"><span data-stu-id="196c3-235">This displays a simplified Delete confirmation.</span></span>

![](mvc-music-store-part-5/_static/image13.png)

<span data-ttu-id="196c3-236">Kliknięcie przycisku Usuń powoduje, że formularza do ponownego zaksięgowania serwera, który wykonuje akcję DeleteConfirmed.</span><span class="sxs-lookup"><span data-stu-id="196c3-236">Clicking the Delete button causes the form to be posted back to the server, which executes the DeleteConfirmed action.</span></span>

[!code-csharp[Main](mvc-music-store-part-5/samples/sample15.cs)]

<span data-ttu-id="196c3-237">Nasze akcji POST protokołu HTTP do kontrolera usunąć wykonuje następujące akcje:</span><span class="sxs-lookup"><span data-stu-id="196c3-237">Our HTTP-POST Delete Controller Action takes the following actions:</span></span>

- 1. <span data-ttu-id="196c3-238">Ładuje albumu według Identyfikatora</span><span class="sxs-lookup"><span data-stu-id="196c3-238">Loads the Album by ID</span></span>
- 2. <span data-ttu-id="196c3-239">Usuwa album i Zapisz zmiany</span><span class="sxs-lookup"><span data-stu-id="196c3-239">Deletes it the album and save changes</span></span>
- 3. <span data-ttu-id="196c3-240">Przekierowuje do indeksu, pokazujący, że Album został usunięty z listy</span><span class="sxs-lookup"><span data-stu-id="196c3-240">Redirects to the Index, showing that the Album was removed from the list</span></span>

<span data-ttu-id="196c3-241">Aby to sprawdzić, uruchom aplikację i przejdź do /StoreManager.</span><span class="sxs-lookup"><span data-stu-id="196c3-241">To test this, run the application and browse to /StoreManager.</span></span> <span data-ttu-id="196c3-242">Wybierz album z listy i kliknij łącze Usuń.</span><span class="sxs-lookup"><span data-stu-id="196c3-242">Select an album from the list and click the Delete link.</span></span>

![](mvc-music-store-part-5/_static/image14.png)

<span data-ttu-id="196c3-243">Zostanie wyświetlony ekran potwierdzenia naszych Delete.</span><span class="sxs-lookup"><span data-stu-id="196c3-243">This displays our Delete confirmation screen.</span></span>

![](mvc-music-store-part-5/_static/image15.png)

<span data-ttu-id="196c3-244">Kliknięcie przycisku Usuń Usuwa album i zwraca nam do strony indeksu Menedżera magazynu zawiera album został usunięty.</span><span class="sxs-lookup"><span data-stu-id="196c3-244">Clicking the Delete button removes the album and returns us to the Store Manager Index page, which shows that the album has been deleted.</span></span>

![](mvc-music-store-part-5/_static/image16.png)

### <a name="using-a-custom-html-helper-to-truncate-text"></a><span data-ttu-id="196c3-245">Przy użyciu niestandardowych pomocnika kodu HTML do obcięcia tekstu</span><span class="sxs-lookup"><span data-stu-id="196c3-245">Using a custom HTML Helper to truncate text</span></span>

<span data-ttu-id="196c3-246">Mamy jeden potencjalny problem z naszą stronę indeks Menedżera magazynu.</span><span class="sxs-lookup"><span data-stu-id="196c3-246">We've got one potential issue with our Store Manager Index page.</span></span> <span data-ttu-id="196c3-247">Nasze właściwości albumu tytułu i nazwy wykonawcy zarówno można wystarczająco długie, że może wywoływać poza naszych formatowanie tabeli.</span><span class="sxs-lookup"><span data-stu-id="196c3-247">Our Album Title and Artist Name properties can both be long enough that they could throw off our table formatting.</span></span> <span data-ttu-id="196c3-248">Utworzymy niestandardowych pomocnika kodu HTML, aby umożliwić nam łatwo obcięcia tych i innych właściwości w naszym widoków.</span><span class="sxs-lookup"><span data-stu-id="196c3-248">We'll create a custom HTML Helper to allow us to easily truncate these and other properties in our Views.</span></span>

![](mvc-music-store-part-5/_static/image17.png)

<span data-ttu-id="196c3-249">W razor @helper składni wprowadził bardzo łatwo tworzyć własne funkcje pomocnicze do użycia w widoków.</span><span class="sxs-lookup"><span data-stu-id="196c3-249">Razor's @helper syntax has made it pretty easy to create your own helper functions for use in your views.</span></span> <span data-ttu-id="196c3-250">Otwórz widok /Views/StoreManager/Index.cshtml i Dodaj następujący kod bezpośrednio po @model wiersza.</span><span class="sxs-lookup"><span data-stu-id="196c3-250">Open the /Views/StoreManager/Index.cshtml view and add the following code directly after the @model line.</span></span>

[!code-cshtml[Main](mvc-music-store-part-5/samples/sample16.cshtml)]

<span data-ttu-id="196c3-251">Ta metoda pomocnika przyjmuje ciągu i maksymalną długość, aby umożliwić.</span><span class="sxs-lookup"><span data-stu-id="196c3-251">This helper method takes a string and a maximum length to allow.</span></span> <span data-ttu-id="196c3-252">Jeśli podany tekst jest krótszy niż określona długość, pomocnika wyświetla go w formie — jest.</span><span class="sxs-lookup"><span data-stu-id="196c3-252">If the text supplied is shorter than the length specified, the helper outputs it as-is.</span></span> <span data-ttu-id="196c3-253">Jeśli jest on dłuższy, następnie je obcina tekst i renderuje "..." dla pozostałych.</span><span class="sxs-lookup"><span data-stu-id="196c3-253">If it is longer, then it truncates the text and renders "…" for the remainder.</span></span>

<span data-ttu-id="196c3-254">Teraz możemy użyć naszych Truncate pomocnika do zapewnienia, że tytuł i nazwy wykonawcy właściwości są mniej niż 25 znaków.</span><span class="sxs-lookup"><span data-stu-id="196c3-254">Now we can use our Truncate helper to ensure that both the Album Title and Artist Name properties are less than 25 characters.</span></span> <span data-ttu-id="196c3-255">Pełny widok przy użyciu naszego nowego pomocnika Truncate pojawia się poniżej.</span><span class="sxs-lookup"><span data-stu-id="196c3-255">The complete view code using our new Truncate helper appears below.</span></span>

[!code-cshtml[Main](mvc-music-store-part-5/samples/sample17.cshtml)]

<span data-ttu-id="196c3-256">Teraz przeglądających /StoreManager/ adres URL, albumów i tytuły są przechowywane poniżej naszych maksymalną długość.</span><span class="sxs-lookup"><span data-stu-id="196c3-256">Now when we browse the /StoreManager/ URL, the albums and titles are kept below our maximum lengths.</span></span>

![](mvc-music-store-part-5/_static/image18.png)

<span data-ttu-id="196c3-257">Uwaga: Ten pokazuje simple case tworzenia i przy użyciu pomocnika, w jednym widoku.</span><span class="sxs-lookup"><span data-stu-id="196c3-257">Note: This shows the simple case of creating and using a helper in one view.</span></span> <span data-ttu-id="196c3-258">Aby dowiedzieć się więcej o tworzeniu wątków, które są dostępne w całej lokacji, zobacz Moje wpisie w blogu: [http://bit.ly/mvc3-helper-options](http://bit.ly/mvc3-helper-options)</span><span class="sxs-lookup"><span data-stu-id="196c3-258">To learn more about creating helpers that you can use throughout your site, see my blog post: [http://bit.ly/mvc3-helper-options](http://bit.ly/mvc3-helper-options)</span></span>


> [!div class="step-by-step"]
> <span data-ttu-id="196c3-259">[Poprzednie](mvc-music-store-part-4.md)
> [dalej](mvc-music-store-part-6.md)</span><span class="sxs-lookup"><span data-stu-id="196c3-259">[Previous](mvc-music-store-part-4.md)
[Next](mvc-music-store-part-6.md)</span></span>
