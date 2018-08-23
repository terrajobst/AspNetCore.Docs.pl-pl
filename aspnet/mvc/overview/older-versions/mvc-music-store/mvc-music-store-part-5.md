---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-5
title: 'Część 5: Edycja formularzy i tworzenie szablonów | Dokumentacja firmy Microsoft'
author: jongalloway
description: W tej serii samouczków szczegółowo opisuje wszystkie etapy, tworzenie przykładowej aplikacji platformy ASP.NET MVC Music Store. Część 5 obejmuje Edycja formularzy i szablonów.
ms.author: riande
ms.date: 04/21/2011
ms.assetid: 6b09413a-6d6a-425a-87c9-629f91b91b28
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-5
msc.type: authoredcontent
ms.openlocfilehash: c1065dcb45b6d28672edba32b95c7fc476c8b944
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/16/2018
ms.locfileid: "41753924"
---
<a name="part-5-edit-forms-and-templating"></a><span data-ttu-id="bdf73-104">Część 5: Edycja formularzy i tworzenie szablonów</span><span class="sxs-lookup"><span data-stu-id="bdf73-104">Part 5: Edit Forms and Templating</span></span>
====================
<span data-ttu-id="bdf73-105">przez [Galloway'em Jon](https://github.com/jongalloway)</span><span class="sxs-lookup"><span data-stu-id="bdf73-105">by [Jon Galloway](https://github.com/jongalloway)</span></span>

> <span data-ttu-id="bdf73-106">MVC Music Store jest aplikacją z samouczka, który wprowadzono i opisano krok po kroku, jak używać platformy ASP.NET MVC i programu Visual Studio do tworzenia aplikacji internetowych.</span><span class="sxs-lookup"><span data-stu-id="bdf73-106">The MVC Music Store is a tutorial application that introduces and explains step-by-step how to use ASP.NET MVC and Visual Studio for web development.</span></span>  
>   
> <span data-ttu-id="bdf73-107">MVC Music Store jest uproszczone przykładową implementację magazynu sprzedaje utworów muzycznych albumy online, która implementuje podstawowej witryny administracji, logowania użytkownika i funkcje koszyka zakupów.</span><span class="sxs-lookup"><span data-stu-id="bdf73-107">The MVC Music Store is a lightweight sample store implementation which sells music albums online, and implements basic site administration, user sign-in, and shopping cart functionality.</span></span>
> 
> <span data-ttu-id="bdf73-108">W tej serii samouczków szczegółowo opisuje wszystkie etapy, tworzenie przykładowej aplikacji platformy ASP.NET MVC Music Store.</span><span class="sxs-lookup"><span data-stu-id="bdf73-108">This tutorial series details all of the steps taken to build the ASP.NET MVC Music Store sample application.</span></span> <span data-ttu-id="bdf73-109">Część 5 obejmuje Edycja formularzy i szablonów.</span><span class="sxs-lookup"><span data-stu-id="bdf73-109">Part 5 covers Edit Forms and Templating.</span></span>


<span data-ttu-id="bdf73-110">W ciągu ostatnich rozdziale firma Microsoft była podczas ładowania danych z naszej bazie danych i wyświetlanie ich.</span><span class="sxs-lookup"><span data-stu-id="bdf73-110">In the past chapter, we were loading data from our database and displaying it.</span></span> <span data-ttu-id="bdf73-111">W tym rozdziale będzie pozwalamy również edycji danych.</span><span class="sxs-lookup"><span data-stu-id="bdf73-111">In this chapter, we'll also enable editing the data.</span></span>

## <a name="creating-the-storemanagercontroller"></a><span data-ttu-id="bdf73-112">Tworzenie StoreManagerController</span><span class="sxs-lookup"><span data-stu-id="bdf73-112">Creating the StoreManagerController</span></span>

<span data-ttu-id="bdf73-113">Firma Microsoft rozpocznie się, tworząc nowy kontroler o nazwie **StoreManagerController**.</span><span class="sxs-lookup"><span data-stu-id="bdf73-113">We'll begin by creating a new controller called **StoreManagerController**.</span></span> <span data-ttu-id="bdf73-114">Dla tego kontrolera firma Microsoft będzie się korzystanie z funkcji tworzenia szkieletu ASP.NET MVC 3 Tools Update.</span><span class="sxs-lookup"><span data-stu-id="bdf73-114">For this controller, we will be taking advantage of the Scaffolding features available in the ASP.NET MVC 3 Tools Update.</span></span> <span data-ttu-id="bdf73-115">Ustaw opcje dla okna dialogowego Dodaj kontroler, jak pokazano poniżej.</span><span class="sxs-lookup"><span data-stu-id="bdf73-115">Set the options for the Add Controller dialog as shown below.</span></span>

![](mvc-music-store-part-5/_static/image1.png)

<span data-ttu-id="bdf73-116">Po kliknięciu przycisku Dodaj, zobaczysz, że mechanizm tworzenia szkieletu ASP.NET MVC 3 nie dobre ilość pracy za Ciebie:</span><span class="sxs-lookup"><span data-stu-id="bdf73-116">When you click the Add button, you'll see that the ASP.NET MVC 3 scaffolding mechanism does a good amount of work for you:</span></span>

- <span data-ttu-id="bdf73-117">Tworzy nowy StoreManagerController za pomocą zmiennej lokalnej platformy Entity Framework</span><span class="sxs-lookup"><span data-stu-id="bdf73-117">It creates the new StoreManagerController with a local Entity Framework variable</span></span>
- <span data-ttu-id="bdf73-118">Dodaje StoreManager folder do folderu widoków projektu</span><span class="sxs-lookup"><span data-stu-id="bdf73-118">It adds a StoreManager folder to the project's Views folder</span></span>
- <span data-ttu-id="bdf73-119">Dodaje widok Create.cshtml Delete.cshtml, Details.cshtml, Edit.cshtml i Index.cshtml silnie typizowaną do klasy albumu</span><span class="sxs-lookup"><span data-stu-id="bdf73-119">It adds Create.cshtml, Delete.cshtml, Details.cshtml, Edit.cshtml, and Index.cshtml view, strongly typed to the Album class</span></span>

![](mvc-music-store-part-5/_static/image2.png)

<span data-ttu-id="bdf73-120">Nowa klasa kontrolera StoreManager obejmuje CRUD (Tworzenie, odczytywanie, aktualizowanie, usuwanie) akcji kontrolera, które wiedzą, jak pracować z Album klasa modelu i kontekstu nasze narzędzia Entity Framework na użytek dostępu do bazy danych.</span><span class="sxs-lookup"><span data-stu-id="bdf73-120">The new StoreManager controller class includes CRUD (create, read, update, delete) controller actions which know how to work with the Album model class and use our Entity Framework context for database access.</span></span>

## <a name="modifying-a-scaffolded-view"></a><span data-ttu-id="bdf73-121">Modyfikowanie szkieletu widoku</span><span class="sxs-lookup"><span data-stu-id="bdf73-121">Modifying a Scaffolded View</span></span>

<span data-ttu-id="bdf73-122">Należy pamiętać, że chociaż ten kod został wygenerowany dla nas, jest standardowy kod ASP.NET MVC, tak samo, jak został możemy pisania w tym samouczku.</span><span class="sxs-lookup"><span data-stu-id="bdf73-122">It's important to remember that, while this code was generated for us, it's standard ASP.NET MVC code, just like we've been writing throughout this tutorial.</span></span> <span data-ttu-id="bdf73-123">Jest ona przeznaczona do zaoszczędzić czas, możesz poświęcić na pisaniu kodu kontrolera standardowy i ręczne tworzenie silnie typizowane widoki, ale nie jest to typ wygenerowanego kodu można było zaobserwować poprzedzone znakiem dire ostrzeżeń w komentarze na temat sposobu nie może zmienić Kod.</span><span class="sxs-lookup"><span data-stu-id="bdf73-123">It's intended to save you the time you'd spend on writing boilerplate controller code and creating the strongly typed views manually, but this isn't the kind of generated code you may have seen prefaced with dire warnings in comments about how you mustn't change the code.</span></span> <span data-ttu-id="bdf73-124">Jest to Twój kod i oczekiwaniami go zmienić.</span><span class="sxs-lookup"><span data-stu-id="bdf73-124">This is your code, and you're expected to change it.</span></span>

<span data-ttu-id="bdf73-125">Tak Zacznijmy od szybka edycja do widoku indeksu StoreManager (/ Views/StoreManager/Index.cshtml).</span><span class="sxs-lookup"><span data-stu-id="bdf73-125">So, let's start with a quick edit to the StoreManager Index view (/Views/StoreManager/Index.cshtml).</span></span> <span data-ttu-id="bdf73-126">Ten widok przedstawia tabeli, w którym jest wyświetlana lista albumów w naszym Sklepie za pomocą edycji / szczegóły / Usuń linki i zawiera właściwości publiczne albumu.</span><span class="sxs-lookup"><span data-stu-id="bdf73-126">This view will display a table which lists the Albums in our store with Edit / Details / Delete links, and includes the Album's public properties.</span></span> <span data-ttu-id="bdf73-127">Usuniemy pole AlbumArtUrl, ponieważ nie jest to przydatne w przypadku tego ekranu.</span><span class="sxs-lookup"><span data-stu-id="bdf73-127">We'll remove the AlbumArtUrl field, as it's not very useful in this display.</span></span> <span data-ttu-id="bdf73-128">W &lt;tabeli&gt; sekcji widoku kodu, należy usunąć &lt;th&gt; i &lt;td&gt; elementy otaczającego AlbumArtUrl odwołań, wskazane przez wyróżnione wiersze poniżej:</span><span class="sxs-lookup"><span data-stu-id="bdf73-128">In &lt;table&gt; section of the view code, remove the &lt;th&gt; and &lt;td&gt; elements surrounding AlbumArtUrl references, as indicated by the highlighted lines below:</span></span>

[!code-cshtml[Main](mvc-music-store-part-5/samples/sample1.cshtml)]

<span data-ttu-id="bdf73-129">Wyświetl zmodyfikowany kod będzie wyglądać następująco:</span><span class="sxs-lookup"><span data-stu-id="bdf73-129">The modified view code will appear as follows:</span></span>

[!code-cshtml[Main](mvc-music-store-part-5/samples/sample2.cshtml)]

## <a name="a-first-look-at-the-store-manager"></a><span data-ttu-id="bdf73-130">Pierwsze spojrzenie na Menedżera Store</span><span class="sxs-lookup"><span data-stu-id="bdf73-130">A first look at the Store Manager</span></span>

<span data-ttu-id="bdf73-131">Teraz uruchom aplikację i przejdź do/StoreManager /.</span><span class="sxs-lookup"><span data-stu-id="bdf73-131">Now run the application and browse to /StoreManager/.</span></span> <span data-ttu-id="bdf73-132">Spowoduje to wyświetlenie tylko zmodyfikowany, wyświetlanie listy albumów w magazynie wraz z łączami do edytowania szczegółów i usuwania indeksu Menedżera Store.</span><span class="sxs-lookup"><span data-stu-id="bdf73-132">This displays the Store Manager Index we just modified, showing a list of the albums in the store with links to Edit, Details, and Delete.</span></span>

![](mvc-music-store-part-5/_static/image3.png)

<span data-ttu-id="bdf73-133">Kliknij link Edytuj przedstawia formularz edycji z polami Album, w tym menu rozwijanych dla wykonawcy i gatunku.</span><span class="sxs-lookup"><span data-stu-id="bdf73-133">Clicking the Edit link displays an edit form with fields for the Album, including dropdowns for Genre and Artist.</span></span>

![](mvc-music-store-part-5/_static/image4.png)

<span data-ttu-id="bdf73-134">Kliknij link "Powrót do listy" u dołu, a następnie kliknij łącze szczegółowych dla albumu.</span><span class="sxs-lookup"><span data-stu-id="bdf73-134">Click the "Back to List" link at the bottom, then click on the Details link for an Album.</span></span> <span data-ttu-id="bdf73-135">Zostaną wyświetlone szczegółowe informacje dotyczące poszczególnych albumu.</span><span class="sxs-lookup"><span data-stu-id="bdf73-135">This displays the detail information for an individual Album.</span></span>

![](mvc-music-store-part-5/_static/image5.png)

<span data-ttu-id="bdf73-136">Ponownie kliknij przycisk Wstecz do listy łącza, a następnie kliknij łącze Usuń.</span><span class="sxs-lookup"><span data-stu-id="bdf73-136">Again, click the Back to List link, then click on a Delete link.</span></span> <span data-ttu-id="bdf73-137">Spowoduje to wyświetlenie okna dialogowego potwierdzenia, wyświetlania szczegółów albumu ani zastanawiać się, jeśli firma Microsoft upewnieniu się, że chcemy go usunąć.</span><span class="sxs-lookup"><span data-stu-id="bdf73-137">This displays a confirmation dialog, showing the album details and asking if we're sure we want to delete it.</span></span>

![](mvc-music-store-part-5/_static/image6.png)

<span data-ttu-id="bdf73-138">Kliknięcie przycisku Usuń u dołu Usuń album i powrót do strony indeksu przedstawiono albumu usunięte.</span><span class="sxs-lookup"><span data-stu-id="bdf73-138">Clicking the Delete button at the bottom will delete the album and return you to the Index page, which shows the album deleted.</span></span>

<span data-ttu-id="bdf73-139">Jeszcze nie skończyliśmy przy użyciu Menedżera Store, ogranicza NAS jednak praca kontrolera i Wyświetl kod dla operacji CRUD rozpocząć od.</span><span class="sxs-lookup"><span data-stu-id="bdf73-139">We're not done with the Store Manager, but we have working controller and view code for the CRUD operations to start from.</span></span>

## <a name="looking-at-the-store-manager-controller-code"></a><span data-ttu-id="bdf73-140">Patrząc kodu Store Menedżera kontrolera</span><span class="sxs-lookup"><span data-stu-id="bdf73-140">Looking at the Store Manager Controller code</span></span>

<span data-ttu-id="bdf73-141">Kontroler Menedżera Store zawiera dobre ilości kodu.</span><span class="sxs-lookup"><span data-stu-id="bdf73-141">The Store Manager Controller contains a good amount of code.</span></span> <span data-ttu-id="bdf73-142">Przejdźmy przez to od góry do dołu.</span><span class="sxs-lookup"><span data-stu-id="bdf73-142">Let's go through this from top to bottom.</span></span> <span data-ttu-id="bdf73-143">Kontroler obejmuje niektóre standardowe przestrzenie nazw dla kontrolera MVC, a także odwołania do przestrzeni nazw naszych modeli.</span><span class="sxs-lookup"><span data-stu-id="bdf73-143">The controller includes some standard namespaces for an MVC controller, as well as a reference to our Models namespace.</span></span> <span data-ttu-id="bdf73-144">Kontroler ma prywatnej wystąpienie MusicStoreEntities, używanego przez poszczególne akcji kontrolera, aby uzyskać dostęp do danych.</span><span class="sxs-lookup"><span data-stu-id="bdf73-144">The controller has a private instance of MusicStoreEntities, used by each of the controller actions for data access.</span></span>

[!code-csharp[Main](mvc-music-store-part-5/samples/sample3.cs)]

### <a name="store-manager-index-and-details-actions"></a><span data-ttu-id="bdf73-145">Store Menedżera indeksu oraz szczegóły akcji</span><span class="sxs-lookup"><span data-stu-id="bdf73-145">Store Manager Index and Details actions</span></span>

<span data-ttu-id="bdf73-146">Widok indeksu umożliwia pobranie listy ze zdjęciami, w tym każdego albumu odwołania wykonawcy i gatunku, jak widzieliśmy wcześniej podczas pracy w metodzie Przeglądaj Store.</span><span class="sxs-lookup"><span data-stu-id="bdf73-146">The index view retrieves a list of Albums, including each album's referenced Genre and Artist information, as we previously saw when working on the Store Browse method.</span></span> <span data-ttu-id="bdf73-147">Widok indeksu obserwowanych odwołania do obiektów połączonych, dzięki czemu może on zawierać każdego albumu gatunku nazwy i nazwy wykonawcy, więc kontrolera jest wydajny i wykonujących zapytania o te informacje w oryginalne żądanie.</span><span class="sxs-lookup"><span data-stu-id="bdf73-147">The Index view is following the references to the linked objects so that it can display each album's Genre name and Artist name, so the controller is being efficient and querying for this information in the original request.</span></span>

[!code-csharp[Main](mvc-music-store-part-5/samples/sample4.cs)]

<span data-ttu-id="bdf73-148">Akcji kontrolera szczegóły kontrolera StoreManager działa tak samo jak informowaliśmy wcześniej — wysyła zapytanie o Album akcji szczegółów kontrolera Store za pomocą Identyfikatora przy użyciu metody Find(), zwraca go do widoku.</span><span class="sxs-lookup"><span data-stu-id="bdf73-148">The StoreManager Controller's Details controller action works exactly the same as the Store Controller Details action we wrote previously - it queries for the Album by ID using the Find() method, then returns it to the view.</span></span>

[!code-csharp[Main](mvc-music-store-part-5/samples/sample5.cs)]

### <a name="the-create-action-methods"></a><span data-ttu-id="bdf73-149">Tworzenie metod akcji</span><span class="sxs-lookup"><span data-stu-id="bdf73-149">The Create Action Methods</span></span>

<span data-ttu-id="bdf73-150">Tworzenie metod akcji są nieco inne niż te, które widzieliśmy w do tej pory, ponieważ obsługują dane wejściowe formularza.</span><span class="sxs-lookup"><span data-stu-id="bdf73-150">The Create action methods are a little different from ones we've seen so far, because they handle form input.</span></span> <span data-ttu-id="bdf73-151">Gdy użytkownik najpierw odwiedzi /StoreManager/tworzenie/zobaczy pusty formularz.</span><span class="sxs-lookup"><span data-stu-id="bdf73-151">When a user first visits /StoreManager/Create/ they will be shown an empty form.</span></span> <span data-ttu-id="bdf73-152">Ta strona HTML będzie zawierał &lt;formularza&gt; element, który zawiera lista rozwijana i pole tekstowe dane wejściowe elementów, gdzie można wprowadzić szczegóły albumu.</span><span class="sxs-lookup"><span data-stu-id="bdf73-152">This HTML page will contain a &lt;form&gt; element that contains dropdown and textbox input elements where they can enter the album's details.</span></span>

<span data-ttu-id="bdf73-153">Po użytkownik wypełnia wartości formularza fotograficzne, ich naciśnij przycisk "Zapisz", aby przesłać te zmiany z powrotem do naszej aplikacji do zapisania w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="bdf73-153">After the user fills in the Album form values, they can press the "Save" button to submit these changes back to our application to save within the database.</span></span> <span data-ttu-id="bdf73-154">Gdy użytkownik naciśnie przycisk "Zapisz" &lt;formularza&gt; będą wykonywać akcję POST protokołu HTTP, do /StoreManager/Tworzenie/adresu URL i Prześlij &lt;formularza&gt; wartości jako część POST protokołu HTTP.</span><span class="sxs-lookup"><span data-stu-id="bdf73-154">When the user presses the "save" button the &lt;form&gt; will perform an HTTP-POST back to the /StoreManager/Create/ URL and submit the &lt;form&gt; values as part of the HTTP-POST.</span></span>

<span data-ttu-id="bdf73-155">ASP.NET MVC pozwala łatwo podzielić logikę te dwa scenariusze wywołania adresu URL, co pozwala na implementowanie dwóch oddzielnych metod akcji "Utwórz" w ramach naszych klasy StoreManagerController — jeden do obsługi HTTP GET początkowej wskaż /StoreManager/Create / Adres URL, a druga do obsługi protokołu HTTP-POST przesłanych zmian.</span><span class="sxs-lookup"><span data-stu-id="bdf73-155">ASP.NET MVC allows us to easily split up the logic of these two URL invocation scenarios by enabling us to implement two separate "Create" action methods within our StoreManagerController class – one to handle the initial HTTP-GET browse to the /StoreManager/Create/ URL, and the other to handle the HTTP-POST of the submitted changes.</span></span>

### <a name="passing-information-to-a-view-using-viewbag"></a><span data-ttu-id="bdf73-156">Przekazanie informacji do widoku, używając elementów ViewBag</span><span class="sxs-lookup"><span data-stu-id="bdf73-156">Passing information to a View using ViewBag</span></span>

<span data-ttu-id="bdf73-157">Firma Microsoft była używana obiekt ViewBag wcześniej w tym samouczku, ale nie jeszcze wiele o nim rozmawialiśmy.</span><span class="sxs-lookup"><span data-stu-id="bdf73-157">We've used the ViewBag earlier in this tutorial, but haven't talked much about it.</span></span> <span data-ttu-id="bdf73-158">Obiekt ViewBag pozwala nam do przekazywania informacji do widoku bez użycia obiektu silnie typizowany model.</span><span class="sxs-lookup"><span data-stu-id="bdf73-158">The ViewBag allows us to pass information to the view without using a strongly typed model object.</span></span> <span data-ttu-id="bdf73-159">W tym przypadku nasze akcji kontrolera HTTP-GET Edytuj musi podawać zarówno listę gatunki i artystów do formularza w celu wypełniania list rozwijanych i zwraca je jako elementy obiekt ViewBag jest najprostszym sposobem wykonania tego zadania.</span><span class="sxs-lookup"><span data-stu-id="bdf73-159">In this case, our Edit HTTP-GET controller action needs to pass both a list of Genres and Artists to the form to populate the dropdowns, and the simplest way to do that is to return them as ViewBag items.</span></span>

<span data-ttu-id="bdf73-160">Obiekt ViewBag jest obiekt dynamiczny, co oznacza, że można wpisać ViewBag.Foo lub ViewBag.YourNameHere bez konieczności pisania kodu, aby zdefiniować te właściwości.</span><span class="sxs-lookup"><span data-stu-id="bdf73-160">The ViewBag is a dynamic object, meaning that you can type ViewBag.Foo or ViewBag.YourNameHere without writing code to define those properties.</span></span> <span data-ttu-id="bdf73-161">W tym przypadku kontrolera kod używa ViewBag.GenreId i ViewBag.ArtistId tak, aby wartości list rozwijanych przesłane za pomocą formularza GenreId i ArtistId, które nie są właściwości fotograficzne, które będzie można ich ustawienia.</span><span class="sxs-lookup"><span data-stu-id="bdf73-161">In this case, the controller code uses ViewBag.GenreId and ViewBag.ArtistId so that the dropdown values submitted with the form will be GenreId and ArtistId, which are the Album properties they will be setting.</span></span>

<span data-ttu-id="bdf73-162">Te wartości list rozwijanych są zwracane do formularza przy użyciu obiektu SelectList, której podstawą jest tylko do tego celu.</span><span class="sxs-lookup"><span data-stu-id="bdf73-162">These dropdown values are returned to the form using the SelectList object, which is built just for that purpose.</span></span> <span data-ttu-id="bdf73-163">Odbywa się przy użyciu kodu w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="bdf73-163">This is done using code like this:</span></span>

[!code-csharp[Main](mvc-music-store-part-5/samples/sample6.cs)]

<span data-ttu-id="bdf73-164">Jak widać w kodzie metody akcji, trzy parametry są używane do utworzenia tego obiektu:</span><span class="sxs-lookup"><span data-stu-id="bdf73-164">As you can see from the action method code, three parameters are being used to create this object:</span></span>

- <span data-ttu-id="bdf73-165">Lista elementów, które będą wyświetlane listy rozwijanej.</span><span class="sxs-lookup"><span data-stu-id="bdf73-165">The list of items the dropdown will be displaying.</span></span> <span data-ttu-id="bdf73-166">Należy pamiętać, że nie jest to po prostu określonym ciągiem — firma Microsoft jest przekazanie listę gatunki.</span><span class="sxs-lookup"><span data-stu-id="bdf73-166">Note that this isn't just a string - we're passing a list of Genres.</span></span>
- <span data-ttu-id="bdf73-167">Następny parametr przekazywany do SelectList jest wybrana wartość.</span><span class="sxs-lookup"><span data-stu-id="bdf73-167">The next parameter being passed to the SelectList is the Selected Value.</span></span> <span data-ttu-id="bdf73-168">Ten sposób SelectList wie, jak należy wstępnie wybrać element na liście.</span><span class="sxs-lookup"><span data-stu-id="bdf73-168">This how the SelectList knows how to pre-select an item in the list.</span></span> <span data-ttu-id="bdf73-169">Są to łatwiejsze do zrozumienia, gdy spojrzymy na formularz edycji, co jest bardzo podobne.</span><span class="sxs-lookup"><span data-stu-id="bdf73-169">This will be easier to understand when we look at the Edit form, which is pretty similar.</span></span>
- <span data-ttu-id="bdf73-170">Ostatni parametr jest właściwość do wyświetlenia.</span><span class="sxs-lookup"><span data-stu-id="bdf73-170">The final parameter is the property to be displayed.</span></span> <span data-ttu-id="bdf73-171">W tym przypadku to wskazujący, że właściwość Genre.Name jest, co będzie widoczna dla użytkownika.</span><span class="sxs-lookup"><span data-stu-id="bdf73-171">In this case, this is indicating that the Genre.Name property is what will be shown to the user.</span></span>

<span data-ttu-id="bdf73-172">Mając to na uwadze następnie Akcja Utwórz HTTP GET jest całkiem proste — dwa SelectLists są dodawane do obiekt ViewBag i żaden obiekt modelu jest przekazywany do formularza (ponieważ jest ona nie został jeszcze utworzony).</span><span class="sxs-lookup"><span data-stu-id="bdf73-172">With that in mind, then, the HTTP-GET Create action is pretty simple - two SelectLists are added to the ViewBag, and no model object is passed to the form (since it hasn't been created yet).</span></span>

[!code-csharp[Main](mvc-music-store-part-5/samples/sample7.cs)]

### <a name="html-helpers-to-display-the-drop-downs-in-the-create-view"></a><span data-ttu-id="bdf73-173">Pomocników HTML do wyświetlenia szczegółu Drop w tworzenie widoku</span><span class="sxs-lookup"><span data-stu-id="bdf73-173">HTML Helpers to display the Drop Downs in the Create View</span></span>

<span data-ttu-id="bdf73-174">Ponieważ Rozmawialiśmy o jak listy rozwijanej wartości są przekazywane do widoku, Przyjrzyjmy szybki widok, aby zobaczyć, jak te wartości są wyświetlane.</span><span class="sxs-lookup"><span data-stu-id="bdf73-174">Since we've talked about how the drop down values are passed to the view, let's take a quick look at the view to see how those values are displayed.</span></span> <span data-ttu-id="bdf73-175">W kodzie widoku (/ Views/StoreManager/Create.cshtml), zostaną wyświetlone następujące wywołanie do wyświetlania listy gatunku w dół.</span><span class="sxs-lookup"><span data-stu-id="bdf73-175">In the view code (/Views/StoreManager/Create.cshtml), you'll see the following call is made to display the Genre drop down.</span></span>

[!code-cshtml[Main](mvc-music-store-part-5/samples/sample8.cshtml)]

<span data-ttu-id="bdf73-176">Jest to nazywane pomocnika kodu HTML — metodę narzędzia, która wykonuje wspólne zadania widoku.</span><span class="sxs-lookup"><span data-stu-id="bdf73-176">This is known as an HTML Helper - a utility method which performs a common view task.</span></span> <span data-ttu-id="bdf73-177">Pomocników HTML są bardzo przydatne w zachowaniu naszego kodu widoku zwięzłe i czytelne.</span><span class="sxs-lookup"><span data-stu-id="bdf73-177">HTML Helpers are very useful in keeping our view code concise and readable.</span></span> <span data-ttu-id="bdf73-178">Pomocnik Html.DropDownList znajduje się przez platformę ASP.NET MVC, ale jak w dalszej części można utworzyć własne elementy pomocnicze dla widoku kodu, które firma Microsoft zostanie ponownie użyty w naszej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="bdf73-178">The Html.DropDownList helper is provided by ASP.NET MVC, but as we'll see later it's possible to create our own helpers for view code we'll reuse in our application.</span></span>

<span data-ttu-id="bdf73-179">Po prostu wywołanie Html.DropDownList musi dowiedzieć się dwie rzeczy — gdzie należy get listy, aby wyświetlić i jakie wartości (jeśli istnieją) powinna być wstępnie wybrane.</span><span class="sxs-lookup"><span data-stu-id="bdf73-179">The Html.DropDownList call just needs to be told two things - where to get the list to display, and what value (if any) should be pre-selected.</span></span> <span data-ttu-id="bdf73-180">Pierwszy parametr, GenreId, informuje DropDownList do wyszukania wartość o nazwie GenreId w modelu lub obiekt ViewBag.</span><span class="sxs-lookup"><span data-stu-id="bdf73-180">The first parameter, GenreId, tells the DropDownList to look for a value named GenreId in either the model or ViewBag.</span></span> <span data-ttu-id="bdf73-181">Drugi parametr służy do wskazania wartość, aby pokazać, jak początkowo zaznaczone na liście rozwijanej.</span><span class="sxs-lookup"><span data-stu-id="bdf73-181">The second parameter is used to indicate the value to show as initially selected in the drop down list.</span></span> <span data-ttu-id="bdf73-182">Ponieważ ta forma jest tworzenie formularza, nie ma wartości być instalowane i String.Empty jest przekazywany.</span><span class="sxs-lookup"><span data-stu-id="bdf73-182">Since this form is a Create form, there's no value to be preselected and String.Empty is passed.</span></span>

### <a name="handling-the-posted-form-values"></a><span data-ttu-id="bdf73-183">Obsługa wartości formularza opublikowane</span><span class="sxs-lookup"><span data-stu-id="bdf73-183">Handling the Posted Form values</span></span>

<span data-ttu-id="bdf73-184">Tak jak Omówiliśmy to przed, istnieją dwie metody akcji skojarzonych z każdym formularzu.</span><span class="sxs-lookup"><span data-stu-id="bdf73-184">As we discussed before, there are two action methods associated with each form.</span></span> <span data-ttu-id="bdf73-185">Pierwszy obsługuje żądania HTTP GET i zostanie wyświetlony formularz.</span><span class="sxs-lookup"><span data-stu-id="bdf73-185">The first handles the HTTP-GET request and displays the form.</span></span> <span data-ttu-id="bdf73-186">Drugi obsługuje żądania POST protokołu HTTP, który zawiera wartości przesłanego formularza.</span><span class="sxs-lookup"><span data-stu-id="bdf73-186">The second handles the HTTP-POST request, which contains the submitted form values.</span></span> <span data-ttu-id="bdf73-187">Należy zauważyć, że akcja kontrolera ma atrybut [HttpPost], który poinformuje platformę ASP.NET MVC, czy tylko powinien odpowiadać na żądania HTTP POST.</span><span class="sxs-lookup"><span data-stu-id="bdf73-187">Notice that controller action has an [HttpPost] attribute, which tells ASP.NET MVC that it should only respond to HTTP-POST requests.</span></span>

[!code-csharp[Main](mvc-music-store-part-5/samples/sample9.cs)]

<span data-ttu-id="bdf73-188">Ta akcja ma cztery obowiązki:</span><span class="sxs-lookup"><span data-stu-id="bdf73-188">This action has four responsibilities:</span></span>

- 1. <span data-ttu-id="bdf73-189">Odczyt wartości formularza</span><span class="sxs-lookup"><span data-stu-id="bdf73-189">Read the form values</span></span>
- 2. <span data-ttu-id="bdf73-190">Sprawdź, w przypadku wartości formularza przekazać reguł sprawdzania poprawności</span><span class="sxs-lookup"><span data-stu-id="bdf73-190">Check if the form values pass any validation rules</span></span>
- 3. <span data-ttu-id="bdf73-191">Jeśli przesłanie formularza jest prawidłowy, Zapisz dane i wyświetlić zaktualizowaną listę</span><span class="sxs-lookup"><span data-stu-id="bdf73-191">If the form submission is valid, save the data and display the updated list</span></span>
- 4. <span data-ttu-id="bdf73-192">Jeśli przesłanie formularza nie jest prawidłowy, Wyświetl ponownie formularz z błędami walidacji</span><span class="sxs-lookup"><span data-stu-id="bdf73-192">If the form submission is not valid, redisplay the form with validation errors</span></span>

#### <a name="reading-form-values-with-model-binding"></a><span data-ttu-id="bdf73-193">Odczytywanie wartości formularza za pomocą wiązania modelu</span><span class="sxs-lookup"><span data-stu-id="bdf73-193">Reading Form Values with Model Binding</span></span>

<span data-ttu-id="bdf73-194">Przesłanie formularza, który zawiera wartości GenreId i ArtistId (z listy rozwijanej) i pole tekstowe wartości dla tytułu, ceny i AlbumArtUrl przetwarzania akcji kontrolera.</span><span class="sxs-lookup"><span data-stu-id="bdf73-194">The controller action is processing a form submission that includes values for GenreId and ArtistId (from the drop down list) and textbox values for Title, Price, and AlbumArtUrl.</span></span> <span data-ttu-id="bdf73-195">Chociaż istnieje możliwość uzyskania bezpośredniego dostępu do wartości formularza, lepszym rozwiązaniem jest korzystanie z wbudowanych w platformy ASP.NET MVC możliwości wiązania modelu.</span><span class="sxs-lookup"><span data-stu-id="bdf73-195">While it's possible to directly access form values, a better approach is to use the Model Binding capabilities built into ASP.NET MVC.</span></span> <span data-ttu-id="bdf73-196">W przypadku akcji kontrolera przyjmuje typ modelu jako parametru, ASP.NET MVC będzie podejmować próby wypełnienia obiektu tego typu za pomocą formularza danych wejściowych (a także wartości trasy i querystring).</span><span class="sxs-lookup"><span data-stu-id="bdf73-196">When a controller action takes a model type as a parameter, ASP.NET MVC will attempt to populate an object of that type using form inputs (as well as route and querystring values).</span></span> <span data-ttu-id="bdf73-197">Jest to realizowane przez wyszukiwanie wartości, których nazwy odpowiadają właściwości obiektu modelu, np. podczas ustawiania wartości GenreId nowy obiekt fotograficzne, szuka danych wejściowych o nazwie GenreId.</span><span class="sxs-lookup"><span data-stu-id="bdf73-197">It does this by looking for values whose names match properties of the model object, e.g. when setting the new Album object's GenreId value, it looks for an input with the name GenreId.</span></span> <span data-ttu-id="bdf73-198">Podczas tworzenia widoków przy użyciu standardowych metod we wzorcu ASP.NET MVC, formularze będą zawsze być renderowany przy użyciu nazwy właściwości jako nazwy pola wejściowego, tak to nazwy pól będą po prostu zgodne.</span><span class="sxs-lookup"><span data-stu-id="bdf73-198">When you create views using the standard methods in ASP.NET MVC, the forms will always be rendered using property names as input field names, so this the field names will just match up.</span></span>

#### <a name="validating-the-model"></a><span data-ttu-id="bdf73-199">Sprawdzanie poprawności modelu</span><span class="sxs-lookup"><span data-stu-id="bdf73-199">Validating the Model</span></span>

<span data-ttu-id="bdf73-200">Model została zweryfikowana za pomocą prostego wywołania do ModelState.IsValid.</span><span class="sxs-lookup"><span data-stu-id="bdf73-200">The model is validated with a simple call to ModelState.IsValid.</span></span> <span data-ttu-id="bdf73-201">Firma Microsoft nie dodano reguł sprawdzania poprawności do naszych klasy albumu jeszcze - wykonamy, które w znacznej — więc ten test nie ma wiele do zrobienia.</span><span class="sxs-lookup"><span data-stu-id="bdf73-201">We haven't added any validation rules to our Album class yet - we'll do that in a bit - so right now this check doesn't have much to do.</span></span> <span data-ttu-id="bdf73-202">Ważne jest, to sprawdzenie ModelStat.IsValid dostosuje się do reguł sprawdzania poprawności, które umieściliśmy na nasz model, dzięki czemu przyszłe zmiany reguł sprawdzania poprawności nie wymaga żadnych aktualizacji kodu akcji kontrolera.</span><span class="sxs-lookup"><span data-stu-id="bdf73-202">What's important is that this ModelStat.IsValid check will adapt to the validation rules we put on our model, so future changes to validation rules won't require any updates to the controller action code.</span></span>

#### <a name="saving-the-submitted-values"></a><span data-ttu-id="bdf73-203">Zapisywanie wartości przesłane</span><span class="sxs-lookup"><span data-stu-id="bdf73-203">Saving the submitted values</span></span>

<span data-ttu-id="bdf73-204">W przypadku przesyłania formularza pozytywnie przejdą weryfikację, nadszedł czas na zapisać wartości w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="bdf73-204">If the form submission passes validation, it's time to save the values to the database.</span></span> <span data-ttu-id="bdf73-205">Z platformą Entity Framework, która wymaga dodania modelu do kolekcji ze zdjęciami i wywoływać metodę SaveChanges.</span><span class="sxs-lookup"><span data-stu-id="bdf73-205">With Entity Framework, that just requires adding the model to the Albums collection and calling SaveChanges.</span></span>

[!code-csharp[Main](mvc-music-store-part-5/samples/sample10.cs)]

<span data-ttu-id="bdf73-206">Entity Framework generuje odpowiednie polecenia SQL, aby zachować wartość.</span><span class="sxs-lookup"><span data-stu-id="bdf73-206">Entity Framework generates the appropriate SQL commands to persist the value.</span></span> <span data-ttu-id="bdf73-207">Po zapisaniu danych, organów powrót do listy ze zdjęciami, dzięki czemu możemy zobaczyć naszej aktualizacji.</span><span class="sxs-lookup"><span data-stu-id="bdf73-207">After saving the data, we redirect back to the list of Albums so we can see our update.</span></span> <span data-ttu-id="bdf73-208">Polega to na zwracanie RedirectToAction nazwą akcji kontrolera, które mają być wyświetlane, firma Microsoft.</span><span class="sxs-lookup"><span data-stu-id="bdf73-208">This is done by returning RedirectToAction with the name of the controller action we want displayed.</span></span> <span data-ttu-id="bdf73-209">W tym przypadku to metoda indeksu.</span><span class="sxs-lookup"><span data-stu-id="bdf73-209">In this case, that's the Index method.</span></span>

#### <a name="displaying-invalid-form-submissions-with-validation-errors"></a><span data-ttu-id="bdf73-210">Wyświetlanie nieprawidłową postać zgłoszenia błędów sprawdzania poprawności</span><span class="sxs-lookup"><span data-stu-id="bdf73-210">Displaying invalid form submissions with Validation Errors</span></span>

<span data-ttu-id="bdf73-211">W przypadku wprowadzania nieprawidłową postać wartości list rozwijanych są dodawane do elementów ViewBag (tak jak w przypadku protokołu HTTP GET) i powiązany model wartości są przekazywane do widoku do wyświetlenia.</span><span class="sxs-lookup"><span data-stu-id="bdf73-211">In the case of invalid form input, the dropdown values are added to the ViewBag (as in the HTTP-GET case) and the bound model values are passed back to the view for display.</span></span> <span data-ttu-id="bdf73-212">Błędy sprawdzania poprawności są automatycznie wyświetlane przy użyciu @Html.ValidationMessageFor pomocnika kodu HTML.</span><span class="sxs-lookup"><span data-stu-id="bdf73-212">Validation errors are automatically displayed using the @Html.ValidationMessageFor HTML Helper.</span></span>

#### <a name="testing-the-create-form"></a><span data-ttu-id="bdf73-213">Testowanie Utwórz formularz</span><span class="sxs-lookup"><span data-stu-id="bdf73-213">Testing the Create Form</span></span>

<span data-ttu-id="bdf73-214">Do przetestowania tego, uruchom aplikację i przejdź do /StoreManager/tworzenie / — spowoduje to wyświetlenie pusty formularz, który został zwrócony przez metodę HTTP GET tworzenie StoreController.</span><span class="sxs-lookup"><span data-stu-id="bdf73-214">To test this out, run the application and browse to /StoreManager/Create/ - this will show you the blank form which was returned by the StoreController Create HTTP-GET method.</span></span>

<span data-ttu-id="bdf73-215">Wypełnij niektóre wartości, a następnie kliknij przycisk Utwórz, można przesłać formularza.</span><span class="sxs-lookup"><span data-stu-id="bdf73-215">Fill in some values and click the Create button to submit the form.</span></span>

![](mvc-music-store-part-5/_static/image7.png)

![](mvc-music-store-part-5/_static/image8.png)

### <a name="handling-edits"></a><span data-ttu-id="bdf73-216">Obsługa zmiany</span><span class="sxs-lookup"><span data-stu-id="bdf73-216">Handling Edits</span></span>

<span data-ttu-id="bdf73-217">Edytowanie pary akcji (HTTP GET i POST protokołu HTTP) są bardzo podobne do tworzenia metod akcji, którą właśnie przyjrzeliśmy się.</span><span class="sxs-lookup"><span data-stu-id="bdf73-217">The Edit action pair (HTTP-GET and HTTP-POST) are very similar to the Create action methods we just looked at.</span></span> <span data-ttu-id="bdf73-218">Ponieważ scenariusz edycji wymaga pracy z istniejącego albumu, Edytuj HTTP-GET metoda ładuje albumu na podstawie parametru "id" przekazaną za pomocą trasy.</span><span class="sxs-lookup"><span data-stu-id="bdf73-218">Since the edit scenario involves working with an existing album, the Edit HTTP-GET method loads the Album based on the "id" parameter, passed in via the route.</span></span> <span data-ttu-id="bdf73-219">Ten kod do pobierania albumy AlbumId jest taka sama, jak wcześniej Opisaliśmy w szczegóły akcji kontrolera.</span><span class="sxs-lookup"><span data-stu-id="bdf73-219">This code for retrieving an album by AlbumId is the same as we've previously looked at in the Details controller action.</span></span> <span data-ttu-id="bdf73-220">Podobnie jak w przypadku tworzenia / metody GET protokołu HTTP, listy rozwijanej wartości są zwracane przez obiekt ViewBag.</span><span class="sxs-lookup"><span data-stu-id="bdf73-220">As with the Create / HTTP-GET method, the drop down values are returned via the ViewBag.</span></span> <span data-ttu-id="bdf73-221">Dzięki temu do zwrócenia albumu naszych obiekt modelu do widoku (zdecydowanie jest wpisane w klasie albumu) podczas przekazywania dodatkowe dane (np. lista gatunki) za pośrednictwem obiekt ViewBag.</span><span class="sxs-lookup"><span data-stu-id="bdf73-221">This allows us to return an Album as our model object to the view (which is strongly typed to the Album class) while passing additional data (e.g. a list of Genres) via the ViewBag.</span></span>

[!code-csharp[Main](mvc-music-store-part-5/samples/sample11.cs)]

<span data-ttu-id="bdf73-222">Akcja POST protokołu HTTP edycji jest bardzo podobny do akcji tworzenia POST protokołu HTTP.</span><span class="sxs-lookup"><span data-stu-id="bdf73-222">The Edit HTTP-POST action is very similar to the Create HTTP-POST action.</span></span> <span data-ttu-id="bdf73-223">Jedyną różnicą jest to, że zamiast opcji dodawania nowych album z bazą danych. Kolekcja ze zdjęciami, firma Microsoft jest znajdowania bieżące wystąpienie Album przy użyciu bazy danych. Entry(album) i ustawianie jego stanu Modified.</span><span class="sxs-lookup"><span data-stu-id="bdf73-223">The only difference is that instead of adding a new album to the db.Albums collection, we're finding the current instance of the Album using db.Entry(album) and setting its state to Modified.</span></span> <span data-ttu-id="bdf73-224">Informuje Entity Framework, modyfikowania istniejącego albumu zamiast tworzenia nowej.</span><span class="sxs-lookup"><span data-stu-id="bdf73-224">This tells Entity Framework that we are modifying an existing album as opposed to creating a new one.</span></span>

[!code-csharp[Main](mvc-music-store-part-5/samples/sample12.cs)]

<span data-ttu-id="bdf73-225">Firma Microsoft tę możliwość testowania działania aplikacji i przechodząc do/StoreManger /, a następnie klikając link edycji dla albumu.</span><span class="sxs-lookup"><span data-stu-id="bdf73-225">We can test this out by running the application and browsing to /StoreManger/, then clicking the Edit link for an album.</span></span>

![](mvc-music-store-part-5/_static/image9.png)

<span data-ttu-id="bdf73-226">Spowoduje to wyświetlenie formularz edycji wyświetlane przez metodę GET HTTP edycji.</span><span class="sxs-lookup"><span data-stu-id="bdf73-226">This displays the Edit form shown by the Edit HTTP-GET method.</span></span> <span data-ttu-id="bdf73-227">Wypełnij niektóre wartości, a następnie kliknij przycisk Zapisz.</span><span class="sxs-lookup"><span data-stu-id="bdf73-227">Fill in some values and click the Save button.</span></span>

![](mvc-music-store-part-5/_static/image10.png)

<span data-ttu-id="bdf73-228">Publikuje formularza, zapisuje wartości i zwraca nam do listy fotograficzne, pokazujący, że wartości zostały zaktualizowane.</span><span class="sxs-lookup"><span data-stu-id="bdf73-228">This posts the form, saves the values, and returns us to the Album list, showing that the values were updated.</span></span>

![](mvc-music-store-part-5/_static/image11.png)

### <a name="handling-deletion"></a><span data-ttu-id="bdf73-229">Usuwanie obsługi</span><span class="sxs-lookup"><span data-stu-id="bdf73-229">Handling Deletion</span></span>

<span data-ttu-id="bdf73-230">Usuwanie następuje tego samego wzorca jako Edit and Create, za pomocą jednego kontrolera akcji w celu wyświetlenia formularza potwierdzenia i innej akcji kontrolera, do obsługi przesyłania formularza.</span><span class="sxs-lookup"><span data-stu-id="bdf73-230">Deletion follows the same pattern as Edit and Create, using one controller action to display the confirmation form, and another controller action to handle the form submission.</span></span>

<span data-ttu-id="bdf73-231">Akcji kontrolera HTTP GET, Usuń dokładnie jest taka sama jak nasze poprzedniej akcji w szczegółach Menedżera Store kontrolera.</span><span class="sxs-lookup"><span data-stu-id="bdf73-231">The HTTP-GET Delete controller action is exactly the same as our previous Store Manager Details controller action.</span></span>

[!code-csharp[Main](mvc-music-store-part-5/samples/sample13.cs)]

<span data-ttu-id="bdf73-232">Możemy wyświetlić formularz, który jest silnie typizowane typu fotograficzne, przy użyciu szablonu zawartości widoku Delete.</span><span class="sxs-lookup"><span data-stu-id="bdf73-232">We display a form that's strongly typed to an Album type, using the Delete view content template.</span></span>

![](mvc-music-store-part-5/_static/image12.png)

<span data-ttu-id="bdf73-233">Szablon Usuń pokazuje wszystkie pola dla modelu, ale ułatwimy że jeszcze chwilę.</span><span class="sxs-lookup"><span data-stu-id="bdf73-233">The Delete template shows all the fields for the model, but we can simplify that down quite a bit.</span></span> <span data-ttu-id="bdf73-234">Zmień kod widok w /Views/StoreManager/Delete.cshtml do następujących.</span><span class="sxs-lookup"><span data-stu-id="bdf73-234">Change the view code in /Views/StoreManager/Delete.cshtml to the following.</span></span>

[!code-cshtml[Main](mvc-music-store-part-5/samples/sample14.cshtml)]

<span data-ttu-id="bdf73-235">Spowoduje to wyświetlenie uproszczone potwierdzenie usunięcia.</span><span class="sxs-lookup"><span data-stu-id="bdf73-235">This displays a simplified Delete confirmation.</span></span>

![](mvc-music-store-part-5/_static/image13.png)

<span data-ttu-id="bdf73-236">Kliknięcie przycisku Usuń powoduje, że formularz, aby zaksięgowania na serwerze, który wykonuje akcję DeleteConfirmed.</span><span class="sxs-lookup"><span data-stu-id="bdf73-236">Clicking the Delete button causes the form to be posted back to the server, which executes the DeleteConfirmed action.</span></span>

[!code-csharp[Main](mvc-music-store-part-5/samples/sample15.cs)]

<span data-ttu-id="bdf73-237">Nasze akcji POST protokołu HTTP do kontrolera Usuń wykonuje następujące akcje:</span><span class="sxs-lookup"><span data-stu-id="bdf73-237">Our HTTP-POST Delete Controller Action takes the following actions:</span></span>

- 1. <span data-ttu-id="bdf73-238">Ładuje albumu według Identyfikatora</span><span class="sxs-lookup"><span data-stu-id="bdf73-238">Loads the Album by ID</span></span>
- 2. <span data-ttu-id="bdf73-239">Usuwa je album i Zapisz zmiany</span><span class="sxs-lookup"><span data-stu-id="bdf73-239">Deletes it the album and save changes</span></span>
- 3. <span data-ttu-id="bdf73-240">Przekierowuje do indeksu, pokazujący, że Album został usunięty z listy</span><span class="sxs-lookup"><span data-stu-id="bdf73-240">Redirects to the Index, showing that the Album was removed from the list</span></span>

<span data-ttu-id="bdf73-241">Aby to sprawdzić, uruchom aplikację i przejdź do /StoreManager.</span><span class="sxs-lookup"><span data-stu-id="bdf73-241">To test this, run the application and browse to /StoreManager.</span></span> <span data-ttu-id="bdf73-242">Wybierz album z listy i kliknij łącze Usuń.</span><span class="sxs-lookup"><span data-stu-id="bdf73-242">Select an album from the list and click the Delete link.</span></span>

![](mvc-music-store-part-5/_static/image14.png)

<span data-ttu-id="bdf73-243">Spowoduje to wyświetlenie naszych ekran potwierdzenia usunięcia.</span><span class="sxs-lookup"><span data-stu-id="bdf73-243">This displays our Delete confirmation screen.</span></span>

![](mvc-music-store-part-5/_static/image15.png)

<span data-ttu-id="bdf73-244">Kliknięcie przycisku Usuń Usuwa album i zwraca nam do strony indeksu Menedżera Store, który pokazuje, że album został usunięty.</span><span class="sxs-lookup"><span data-stu-id="bdf73-244">Clicking the Delete button removes the album and returns us to the Store Manager Index page, which shows that the album has been deleted.</span></span>

![](mvc-music-store-part-5/_static/image16.png)

### <a name="using-a-custom-html-helper-to-truncate-text"></a><span data-ttu-id="bdf73-245">Obetnij tekstu przy użyciu niestandardowego pomocnika HTML</span><span class="sxs-lookup"><span data-stu-id="bdf73-245">Using a custom HTML Helper to truncate text</span></span>

<span data-ttu-id="bdf73-246">Mamy jeden potencjalny problem z naszą stronę indeksu Menedżera Store.</span><span class="sxs-lookup"><span data-stu-id="bdf73-246">We've got one potential issue with our Store Manager Index page.</span></span> <span data-ttu-id="bdf73-247">Nasze właściwości tytuł i nazwy wykonawcy zarówno można wystarczająco długi, że może wywoływać poza naszym formatowanie tabeli.</span><span class="sxs-lookup"><span data-stu-id="bdf73-247">Our Album Title and Artist Name properties can both be long enough that they could throw off our table formatting.</span></span> <span data-ttu-id="bdf73-248">Utworzymy niestandardowego pomocnika kodu HTML do pozwalają łatwo obciąć tych i innych właściwości w naszym widokach.</span><span class="sxs-lookup"><span data-stu-id="bdf73-248">We'll create a custom HTML Helper to allow us to easily truncate these and other properties in our Views.</span></span>

![](mvc-music-store-part-5/_static/image17.png)

<span data-ttu-id="bdf73-249">Firmy razor @helper składni wprowadził dość łatwo jest tworzyć swoje własne funkcje pomocnicze do użytku w widoków.</span><span class="sxs-lookup"><span data-stu-id="bdf73-249">Razor's @helper syntax has made it pretty easy to create your own helper functions for use in your views.</span></span> <span data-ttu-id="bdf73-250">Powoduje ono otwarcie widoku /Views/StoreManager/Index.cshtml i Dodaj poniższy kod bezpośrednio po @model wiersza.</span><span class="sxs-lookup"><span data-stu-id="bdf73-250">Open the /Views/StoreManager/Index.cshtml view and add the following code directly after the @model line.</span></span>

[!code-cshtml[Main](mvc-music-store-part-5/samples/sample16.cshtml)]

<span data-ttu-id="bdf73-251">Ta metoda pomocnika przyjmuje ciąg i maksymalna długość, aby umożliwić.</span><span class="sxs-lookup"><span data-stu-id="bdf73-251">This helper method takes a string and a maximum length to allow.</span></span> <span data-ttu-id="bdf73-252">Jeśli tekst podany jest krótszy niż długość określona, pomocnika wyświetla go w formie-to.</span><span class="sxs-lookup"><span data-stu-id="bdf73-252">If the text supplied is shorter than the length specified, the helper outputs it as-is.</span></span> <span data-ttu-id="bdf73-253">Jeśli jest on dłuższy, następnie go obcina tekst i renderuje "..." dla pozostałych.</span><span class="sxs-lookup"><span data-stu-id="bdf73-253">If it is longer, then it truncates the text and renders "…" for the remainder.</span></span>

<span data-ttu-id="bdf73-254">Teraz możemy użyć naszego Truncate pomocnika, aby upewnić się, że obie tytuł wykonawcy nazwy właściwości i mniej niż 25 znaków.</span><span class="sxs-lookup"><span data-stu-id="bdf73-254">Now we can use our Truncate helper to ensure that both the Album Title and Artist Name properties are less than 25 characters.</span></span> <span data-ttu-id="bdf73-255">Pełny przegląd kodu za pomocą naszego nowego pomocnika Truncate pojawia się poniżej.</span><span class="sxs-lookup"><span data-stu-id="bdf73-255">The complete view code using our new Truncate helper appears below.</span></span>

[!code-cshtml[Main](mvc-music-store-part-5/samples/sample17.cshtml)]

<span data-ttu-id="bdf73-256">Teraz przeglądających /StoreManager/ adres URL, albumy i tytuły są utrzymywane na poniżej naszej maksymalnej długości.</span><span class="sxs-lookup"><span data-stu-id="bdf73-256">Now when we browse the /StoreManager/ URL, the albums and titles are kept below our maximum lengths.</span></span>

![](mvc-music-store-part-5/_static/image18.png)

<span data-ttu-id="bdf73-257">Uwaga: Spowoduje to pokazanie tak proste tworzenie i używanie pomocnika w jednym widoku.</span><span class="sxs-lookup"><span data-stu-id="bdf73-257">Note: This shows the simple case of creating and using a helper in one view.</span></span> <span data-ttu-id="bdf73-258">Aby dowiedzieć się więcej na temat tworzenia wątków, które można użyć w całej lokacji, zobacz wpis w blogu moje: [http://bit.ly/mvc3-helper-options](http://bit.ly/mvc3-helper-options)</span><span class="sxs-lookup"><span data-stu-id="bdf73-258">To learn more about creating helpers that you can use throughout your site, see my blog post: [http://bit.ly/mvc3-helper-options](http://bit.ly/mvc3-helper-options)</span></span>


> [!div class="step-by-step"]
> <span data-ttu-id="bdf73-259">[Poprzednie](mvc-music-store-part-4.md)
> [dalej](mvc-music-store-part-6.md)</span><span class="sxs-lookup"><span data-stu-id="bdf73-259">[Previous](mvc-music-store-part-4.md)
[Next](mvc-music-store-part-6.md)</span></span>
