---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-2
title: 'Część 2: Kontrolerów | Dokumentacja firmy Microsoft'
author: jongalloway
description: Ten samouczek serii zawiera szczegóły dotyczące wszystkich kroków tworzenia przykładowej aplikacji ASP.NET MVC utworów muzycznych magazynu. Część 2 obejmuje kontrolerów.
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/21/2011
ms.topic: article
ms.assetid: 998ce4e1-9d72-435b-8f1c-399a10ae4360
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-2
msc.type: authoredcontent
ms.openlocfilehash: 680cdea388d9b01961bd626643c0fd91c9205ed7
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
---
<a name="part-2-controllers"></a><span data-ttu-id="c6618-104">Część 2: kontrolerów</span><span class="sxs-lookup"><span data-stu-id="c6618-104">Part 2: Controllers</span></span>
====================
<span data-ttu-id="c6618-105">przez [Galloway Jan](https://github.com/jongalloway)</span><span class="sxs-lookup"><span data-stu-id="c6618-105">by [Jon Galloway](https://github.com/jongalloway)</span></span>

> <span data-ttu-id="c6618-106">Magazyn utworów muzycznych MVC jest samouczek aplikacji, które wprowadzono i opisano krok po kroku, jak używać do tworzenia aplikacji sieci web platformy ASP.NET MVC i Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="c6618-106">The MVC Music Store is a tutorial application that introduces and explains step-by-step how to use ASP.NET MVC and Visual Studio for web development.</span></span>  
>   
> <span data-ttu-id="c6618-107">Magazyn utworów muzycznych MVC jest implementacja magazynu lekkie próbki, co sprzedaje albumów muzycznych w trybie online i implementuje podstawowej witryny administracji, logowania użytkownika i funkcje koszyka zakupów.</span><span class="sxs-lookup"><span data-stu-id="c6618-107">The MVC Music Store is a lightweight sample store implementation which sells music albums online, and implements basic site administration, user sign-in, and shopping cart functionality.</span></span>  
>   
> <span data-ttu-id="c6618-108">Ten samouczek serii zawiera szczegóły dotyczące wszystkich kroków tworzenia przykładowej aplikacji ASP.NET MVC utworów muzycznych magazynu.</span><span class="sxs-lookup"><span data-stu-id="c6618-108">This tutorial series details all of the steps taken to build the ASP.NET MVC Music Store sample application.</span></span> <span data-ttu-id="c6618-109">Część 2 obejmuje kontrolerów.</span><span class="sxs-lookup"><span data-stu-id="c6618-109">Part 2 covers Controllers.</span></span>


<span data-ttu-id="c6618-110">Web tradycyjnych struktur przychodzących adresów URL są zwykle mapowane do plików na dysku.</span><span class="sxs-lookup"><span data-stu-id="c6618-110">With traditional web frameworks, incoming URLs are typically mapped to files on disk.</span></span> <span data-ttu-id="c6618-111">Na przykład: żądanie dla danego adresu URL, takich jak "/ Products.aspx" lub "/ Products.php" mogą być przetwarzane przez plik "Products.aspx" lub "Products.php".</span><span class="sxs-lookup"><span data-stu-id="c6618-111">For example: a request for a URL like "/Products.aspx" or "/Products.php" might be processed by a "Products.aspx" or "Products.php" file.</span></span>

<span data-ttu-id="c6618-112">Oparte na sieci Web platformy MVC adresy URL są mapowane na kod serwera w nieco inny sposób.</span><span class="sxs-lookup"><span data-stu-id="c6618-112">Web-based MVC frameworks map URLs to server code in a slightly different way.</span></span> <span data-ttu-id="c6618-113">Zamiast mapowania przychodzących adresów URL do plików, zamiast tego mapują adresy URL do metody klasy.</span><span class="sxs-lookup"><span data-stu-id="c6618-113">Instead of mapping incoming URLs to files, they instead map URLs to methods on classes.</span></span> <span data-ttu-id="c6618-114">Klasy te są nazywane "Kontrolerów" i są one odpowiedzialna za przetwarzanie przychodzących żądań HTTP w celu obsługi danych wejściowych użytkownika, pobieranie i zapisywanie danych i określania odpowiedź do wysłania z powrotem do klienta (wyświetlania kodu HTML, Pobierz plik, przekierowanie na inny Adres URL itp.).</span><span class="sxs-lookup"><span data-stu-id="c6618-114">These classes are called "Controllers" and they are responsible for processing incoming HTTP requests, handling user input, retrieving and saving data, and determining the response to send back to the client (display HTML, download a file, redirect to a different URL, etc.).</span></span>

## <a name="adding-a-homecontroller"></a><span data-ttu-id="c6618-115">Dodawanie HomeController</span><span class="sxs-lookup"><span data-stu-id="c6618-115">Adding a HomeController</span></span>

<span data-ttu-id="c6618-116">Naszej aplikacji MVC utworów muzycznych magazynu rozpocznie się przez dodanie klasy kontrolera, który będzie obsługiwać adresy URL do strony głównej witryny firmy Microsoft.</span><span class="sxs-lookup"><span data-stu-id="c6618-116">We'll begin our MVC Music Store application by adding a Controller class that will handle URLs to the Home page of our site.</span></span> <span data-ttu-id="c6618-117">Firma Microsoft będzie wykonaj domyślnych konwencji nazewnictwa platformy ASP.NET MVC i wywołać go HomeController.</span><span class="sxs-lookup"><span data-stu-id="c6618-117">We'll follow the default naming conventions of ASP.NET MVC and call it HomeController.</span></span>

<span data-ttu-id="c6618-118">Kliknij prawym przyciskiem myszy folder "Kontrolerów" w Eksploratorze rozwiązań i wybierz opcję "Dodaj" i "Kontrolera..."</span><span class="sxs-lookup"><span data-stu-id="c6618-118">Right-click the "Controllers" folder within the Solution Explorer and select "Add", and then the "Controller…"</span></span> <span data-ttu-id="c6618-119">polecenie:</span><span class="sxs-lookup"><span data-stu-id="c6618-119">command:</span></span>

![](mvc-music-store-part-2/_static/image1.jpg)

<span data-ttu-id="c6618-120">Zostanie wyświetlone okno dialogowe "Dodaj kontroler".</span><span class="sxs-lookup"><span data-stu-id="c6618-120">This will bring up the "Add Controller" dialog.</span></span> <span data-ttu-id="c6618-121">Nazwa kontrolera "HomeController" i naciśnij przycisk Dodaj.</span><span class="sxs-lookup"><span data-stu-id="c6618-121">Name the controller "HomeController" and press the Add button.</span></span>

![](mvc-music-store-part-2/_static/image1.png)

<span data-ttu-id="c6618-122">Spowoduje to utworzenie nowego pliku HomeController.cs, z następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="c6618-122">This will create a new file, HomeController.cs, with the following code:</span></span>

[!code-csharp[Main](mvc-music-store-part-2/samples/sample1.cs)]

<span data-ttu-id="c6618-123">Aby uruchomić ułatwianiu, umożliwia Zamień metody indeksu prosta metoda, która po prostu zwraca ciąg.</span><span class="sxs-lookup"><span data-stu-id="c6618-123">To start as simply as possible, let's replace the Index method with a simple method that just returns a string.</span></span> <span data-ttu-id="c6618-124">Wybierzemy dwie zmiany:</span><span class="sxs-lookup"><span data-stu-id="c6618-124">We'll make two changes:</span></span>

- <span data-ttu-id="c6618-125">Zmień metodę zwraca ciąg zamiast element ActionResult</span><span class="sxs-lookup"><span data-stu-id="c6618-125">Change the method to return a string instead of an ActionResult</span></span>
- <span data-ttu-id="c6618-126">Zmień instrukcję return powrót do "Hello z domu"</span><span class="sxs-lookup"><span data-stu-id="c6618-126">Change the return statement to return "Hello from Home"</span></span>

<span data-ttu-id="c6618-127">Metoda powinna wyglądać następująco:</span><span class="sxs-lookup"><span data-stu-id="c6618-127">The method should now look like this:</span></span>

[!code-csharp[Main](mvc-music-store-part-2/samples/sample2.cs)]

## <a name="running-the-application"></a><span data-ttu-id="c6618-128">Uruchamianie aplikacji</span><span class="sxs-lookup"><span data-stu-id="c6618-128">Running the Application</span></span>

<span data-ttu-id="c6618-129">Teraz załóżmy uruchamiania witryny.</span><span class="sxs-lookup"><span data-stu-id="c6618-129">Now let's run the site.</span></span> <span data-ttu-id="c6618-130">Możemy uruchomić naszych serwera sieci web i wypróbować lokacji przy użyciu następujących::</span><span class="sxs-lookup"><span data-stu-id="c6618-130">We can start our web-server and try out the site using any of the following::</span></span>

- <span data-ttu-id="c6618-131">Wybierz element menu Rozpocznij debugowanie ⇨ debugowania</span><span class="sxs-lookup"><span data-stu-id="c6618-131">Choose the Debug ⇨ Start Debugging menu item</span></span>
- <span data-ttu-id="c6618-132">Kliknij przycisk zieloną strzałkę na pasku narzędzi ![](mvc-music-store-part-2/_static/image2.jpg)</span><span class="sxs-lookup"><span data-stu-id="c6618-132">Click the Green arrow button in the toolbar ![](mvc-music-store-part-2/_static/image2.jpg)</span></span>
- <span data-ttu-id="c6618-133">Użyj skrótu klawiaturowego, F5.</span><span class="sxs-lookup"><span data-stu-id="c6618-133">Use the keyboard shortcut, F5.</span></span>

<span data-ttu-id="c6618-134">Przy użyciu dowolnego z powyższych kroków będzie skompilować naszych projektu, a następnie spowodować ASP.NET Development Server jest wbudowane Visual Web Developer można uruchomić.</span><span class="sxs-lookup"><span data-stu-id="c6618-134">Using any of the above steps will compile our project, and then cause the ASP.NET Development Server that is built-into Visual Web Developer to start.</span></span> <span data-ttu-id="c6618-135">Powiadomienia będą wyświetlane w dolnym rogu ekranu, aby wskazać, że ASP.NET Development Server rozpoczęła i będzie wyświetlany numer portu, czy działa w ramach.</span><span class="sxs-lookup"><span data-stu-id="c6618-135">A notification will appear in the bottom corner of the screen to indicate that the ASP.NET Development Server has started up, and will show the port number that it is running under.</span></span>

![](mvc-music-store-part-2/_static/image2.png)

<span data-ttu-id="c6618-136">Visual Web Developer następnie zostanie automatycznie Otwórz okno przeglądarki, w których adres URL wskazuje naszych serwera sieci web.</span><span class="sxs-lookup"><span data-stu-id="c6618-136">Visual Web Developer will then automatically open a browser window whose URL points to our web-server.</span></span> <span data-ttu-id="c6618-137">Pozwoli to nam możesz szybko wypróbować usługę naszej aplikacji sieci web:</span><span class="sxs-lookup"><span data-stu-id="c6618-137">This will allow us to quickly try out our web application:</span></span>

![](mvc-music-store-part-2/_static/image3.png)

<span data-ttu-id="c6618-138">OK, który był dość szybki — utworzyliśmy nową witrynę sieci Web, dodać funkcję trzech linii i mamy tekstu w przeglądarce.</span><span class="sxs-lookup"><span data-stu-id="c6618-138">Okay, that was pretty quick – we created a new website, added a three line function, and we've got text in a browser.</span></span> <span data-ttu-id="c6618-139">Nie okazji nauki, ale jest rozpoczęcia.</span><span class="sxs-lookup"><span data-stu-id="c6618-139">Not rocket science, but it's a start.</span></span>

<span data-ttu-id="c6618-140">*Uwaga: Visual Web Developer zawiera ASP.NET Development Server, co spowoduje uruchomienie witryny sieci Web na liczby losowe wolnego "port". Na zrzucie ekranu powyżej, witryna jest uruchomiona na `http://localhost:26641/`, dlatego używa portu 26641. Numer portu będzie różna. Przy omawianiu /Store/Browse podobnego adresy URL, w tym samouczku, które przechodzą po numer portu. Zakładając, że numer portu z 26641, przechodząc do sklepu/Przeglądaj oznacza przeglądania `http://localhost:26641/Store/Browse`.*</span><span class="sxs-lookup"><span data-stu-id="c6618-140">*Note: Visual Web Developer includes the ASP.NET Development Server, which will run your website on a random free "port" number. In the screenshot above, the site is running at `http://localhost:26641/`, so it's using port 26641. Your port number will be different. When we talk about URL's like /Store/Browse in this tutorial, that will go after the port number. Assuming a port number of 26641, browsing to /Store/Browse will mean browsing to `http://localhost:26641/Store/Browse`.*</span></span>

## <a name="adding-a-storecontroller"></a><span data-ttu-id="c6618-141">Dodawanie StoreController</span><span class="sxs-lookup"><span data-stu-id="c6618-141">Adding a StoreController</span></span>

<span data-ttu-id="c6618-142">Dodaliśmy HomeController proste, który implementuje strony głównej witryny.</span><span class="sxs-lookup"><span data-stu-id="c6618-142">We added a simple HomeController that implements the Home Page of our site.</span></span> <span data-ttu-id="c6618-143">Teraz Dodajmy innego kontrolera, które będą używane do implementowania przeglądania naszych magazynu utworów muzycznych.</span><span class="sxs-lookup"><span data-stu-id="c6618-143">Let's now add another controller that we'll use to implement the browsing functionality of our music store.</span></span> <span data-ttu-id="c6618-144">Kontrolera magazynu obsługuje trzy scenariusze:</span><span class="sxs-lookup"><span data-stu-id="c6618-144">Our store controller will support three scenarios:</span></span>

- <span data-ttu-id="c6618-145">Strony listę gatunkami muzyki utworów muzycznych w naszym magazynie utworów muzycznych</span><span class="sxs-lookup"><span data-stu-id="c6618-145">A listing page of the music genres in our music store</span></span>
- <span data-ttu-id="c6618-146">Strona przeglądania, która zawiera listę wszystkich albumów muzycznych określonego rodzaju</span><span class="sxs-lookup"><span data-stu-id="c6618-146">A browse page that lists all of the music albums in a particular genre</span></span>
- <span data-ttu-id="c6618-147">Strona szczegółów, która zawiera informacje o albumu określonych utworów muzycznych</span><span class="sxs-lookup"><span data-stu-id="c6618-147">A details page that shows information about a specific music album</span></span>

<span data-ttu-id="c6618-148">Zaczniemy przez dodanie nowych klas StoreController...</span><span class="sxs-lookup"><span data-stu-id="c6618-148">We'll start by adding a new StoreController class..</span></span> <span data-ttu-id="c6618-149">Jeśli nie jest jeszcze zatrzymać uruchomienie aplikacji przez zamknięcie przeglądarki lub wybranie ⇨ debugowania Zatrzymaj debugowanie elementu menu.</span><span class="sxs-lookup"><span data-stu-id="c6618-149">If you haven't already, stop running the application either by closing the browser or selecting the Debug ⇨ Stop Debugging menu item.</span></span>

<span data-ttu-id="c6618-150">Teraz Dodaj nowe StoreController.</span><span class="sxs-lookup"><span data-stu-id="c6618-150">Now add a new StoreController.</span></span> <span data-ttu-id="c6618-151">Tak samo, jak robiliśmy z HomeController, firma Microsoft będzie to zrobić przez kliknięcie prawym przyciskiem myszy folder "Kontrolerów" w Eksploratorze rozwiązań i wybierając Add -&gt;kontrolera elementu menu</span><span class="sxs-lookup"><span data-stu-id="c6618-151">Just like we did with HomeController, we'll do this by right-clicking on the "Controllers" folder within the Solution Explorer and choosing the Add-&gt;Controller menu item</span></span>

![](mvc-music-store-part-2/_static/image4.png)

<span data-ttu-id="c6618-152">Naszej nowej StoreController już ma metodę "Index".</span><span class="sxs-lookup"><span data-stu-id="c6618-152">Our new StoreController already has an "Index" method.</span></span> <span data-ttu-id="c6618-153">Aby zaimplementować stronę dotyczącą listę zawierającą wszystkie genres w naszym magazynie utworów muzycznych użyjemy tej metody "Index".</span><span class="sxs-lookup"><span data-stu-id="c6618-153">We'll use this "Index" method to implement our listing page that lists all genres in our music store.</span></span> <span data-ttu-id="c6618-154">Dodamy również dwie dodatkowe metody służące do implementacji dwa inne scenariusze chcemy naszych StoreController do obsługi: przeglądania i szczegóły.</span><span class="sxs-lookup"><span data-stu-id="c6618-154">We'll also add two additional methods to implement the two other scenarios we want our StoreController to handle: Browse and Details.</span></span>

<span data-ttu-id="c6618-155">Te metody (indeks, przeglądania i szczegóły) w ramach kontrolera są nazywane "Akcji kontrolera" i jako już zapoznaniu się z metodą akcji HomeController.Index (), ich zadanie ma odpowiadać na żądania adresu URL i (ogólnie rzecz biorąc) określenie zawartości ma zostać odesłana do przeglądarki lub użytkownik, który wywołał adres URL.</span><span class="sxs-lookup"><span data-stu-id="c6618-155">These methods (Index, Browse and Details) within our Controller are called "Controller Actions", and as you've already seen with the HomeController.Index()action method, their job is to respond to URL requests and (generally speaking) determine what content should be sent back to the browser or user that invoked the URL.</span></span>

<span data-ttu-id="c6618-156">Zaczniemy implementacja naszej StoreController zmieniając theIndex() metoda zwraca ciąg "Hello z Store.Index()" i Browse() i Details() dodamy podobnych metod:</span><span class="sxs-lookup"><span data-stu-id="c6618-156">We'll start our StoreController implementation by changing theIndex() method to return the string "Hello from Store.Index()" and we'll add similar methods for Browse() and Details():</span></span>

[!code-csharp[Main](mvc-music-store-part-2/samples/sample3.cs)]

<span data-ttu-id="c6618-157">Uruchom ponownie projekt, a następnie przejdź do następujących adresów URL:</span><span class="sxs-lookup"><span data-stu-id="c6618-157">Run the project again and browse the following URLs:</span></span>

- <span data-ttu-id="c6618-158">/ Store</span><span class="sxs-lookup"><span data-stu-id="c6618-158">/Store</span></span>
- <span data-ttu-id="c6618-159">/ Magazynu/przeglądania</span><span class="sxs-lookup"><span data-stu-id="c6618-159">/Store/Browse</span></span>
- <span data-ttu-id="c6618-160">/ / Szczegóły magazynu</span><span class="sxs-lookup"><span data-stu-id="c6618-160">/Store/Details</span></span>

<span data-ttu-id="c6618-161">Uzyskiwanie dostępu do tych adresów URL wywołania metody akcji w obrębie kontrolera i zwraca ciąg odpowiedzi:</span><span class="sxs-lookup"><span data-stu-id="c6618-161">Accessing these URLs will invoke the action methods within our Controller and return string responses:</span></span>

![](mvc-music-store-part-2/_static/image5.png)

<span data-ttu-id="c6618-162">To świetnie, ale są tylko stałe ciągów.</span><span class="sxs-lookup"><span data-stu-id="c6618-162">That's great, but these are just constant strings.</span></span> <span data-ttu-id="c6618-163">Można wprowadzić je dynamiczny, aby pobrać informacje z adresu URL i wyświetl ją w danych wyjściowych strony.</span><span class="sxs-lookup"><span data-stu-id="c6618-163">Let's make them dynamic, so they take information from the URL and display it in the page output.</span></span>

<span data-ttu-id="c6618-164">Najpierw zmienimy metody akcji przeglądania można pobrać wartości querystring w adresie URL.</span><span class="sxs-lookup"><span data-stu-id="c6618-164">First we'll change the Browse action method to retrieve a querystring value from the URL.</span></span> <span data-ttu-id="c6618-165">Firma Microsoft można to zrobić przez dodanie parametru "rodzaju" do naszej metody akcji.</span><span class="sxs-lookup"><span data-stu-id="c6618-165">We can do this by adding a "genre" parameter to our action method.</span></span> <span data-ttu-id="c6618-166">W takim przypadku to ASP.NET MVC automatycznie przejdzie żadnych parametrów post formularza lub ciągu kwerendy o nazwie "rodzaju" do naszej metody akcji, gdy jest wywoływana.</span><span class="sxs-lookup"><span data-stu-id="c6618-166">When we do this ASP.NET MVC will automatically pass any querystring or form post parameters named "genre" to our action method when it is invoked.</span></span>

[!code-csharp[Main](mvc-music-store-part-2/samples/sample4.cs)]

<span data-ttu-id="c6618-167">*Uwaga: Firma Microsoft korzysta z metody narzędzie HttpUtility.HtmlEncode do oczyszczenia danych wejściowych użytkownika. Zapobiega to wstrzykiwania Javascript do naszej widoku z łączem, takich jak /Store/Browse użytkowników? Genre =&lt;skryptu&gt;window.location= "http://hackersite.com"&lt;/script&gt;.*</span><span class="sxs-lookup"><span data-stu-id="c6618-167">*Note: We're using the HttpUtility.HtmlEncode utility method to sanitize the user input. This prevents users from injecting Javascript into our View with a link like /Store/Browse?Genre=&lt;script&gt;window.location='http://hackersite.com'&lt;/script&gt;.*</span></span>

<span data-ttu-id="c6618-168">Teraz załóżmy przejdź do sklepu/Przeglądaj? Genre = Disco</span><span class="sxs-lookup"><span data-stu-id="c6618-168">Now let's browse to /Store/Browse?Genre=Disco</span></span>

![](mvc-music-store-part-2/_static/image6.png)

<span data-ttu-id="c6618-169">Zmieńmy obok akcji Details do odczytu i wyświetlanie parametru wejściowego o nazwie identyfikatora.</span><span class="sxs-lookup"><span data-stu-id="c6618-169">Let's next change the Details action to read and display an input parameter named ID.</span></span> <span data-ttu-id="c6618-170">W przeciwieństwie do naszej poprzedniej — metoda firma Microsoft nie będzie można osadzanie wartość Identyfikatora jako parametr querystring.</span><span class="sxs-lookup"><span data-stu-id="c6618-170">Unlike our previous method, we won't be embedding the ID value as a querystring parameter.</span></span> <span data-ttu-id="c6618-171">Zamiast tego firma Microsoft będzie osadź go bezpośrednio w ramach samego adresu.</span><span class="sxs-lookup"><span data-stu-id="c6618-171">Instead we'll embed it directly within the URL itself.</span></span> <span data-ttu-id="c6618-172">Na przykład: /Store/Details/5.</span><span class="sxs-lookup"><span data-stu-id="c6618-172">For example: /Store/Details/5.</span></span>

<span data-ttu-id="c6618-173">ASP.NET MVC umożliwia nam to łatwo zrobić bez konieczności konfigurowania.</span><span class="sxs-lookup"><span data-stu-id="c6618-173">ASP.NET MVC lets us easily do this without having to configure anything.</span></span> <span data-ttu-id="c6618-174">ASP.NET MVC domyślnej konwencji routingu jest Traktuj segment adresu URL po nazwy metody akcji, jako parametr o nazwie "ID".</span><span class="sxs-lookup"><span data-stu-id="c6618-174">ASP.NET MVC's default routing convention is to treat the segment of a URL after the action method name as a parameter named "ID".</span></span> <span data-ttu-id="c6618-175">Jeśli stosowana metoda akcji ma parametr o nazwie identyfikator platformy ASP.NET MVC zostanie automatycznie przekazuj segment adresu URL do użytkownika jako parametr.</span><span class="sxs-lookup"><span data-stu-id="c6618-175">If your action method has a parameter named ID then ASP.NET MVC will automatically pass the URL segment to you as a parameter.</span></span>

[!code-csharp[Main](mvc-music-store-part-2/samples/sample5.cs)]

<span data-ttu-id="c6618-176">Uruchom aplikację i przejdź do /Store/Details/5:</span><span class="sxs-lookup"><span data-stu-id="c6618-176">Run the application and browse to /Store/Details/5:</span></span>

![](mvc-music-store-part-2/_static/image7.png)

<span data-ttu-id="c6618-177">Załóżmy recap, co możemy wykonane wykonanej do tej pory:</span><span class="sxs-lookup"><span data-stu-id="c6618-177">Let's recap what we've done so far:</span></span>

- <span data-ttu-id="c6618-178">W programie Visual Web Developer utworzyliśmy nowy projekt ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="c6618-178">We've created a new ASP.NET MVC project in Visual Web Developer</span></span>
- <span data-ttu-id="c6618-179">Zostały omówione struktury folderów podstawowe aplikacji platformy ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="c6618-179">We've discussed the basic folder structure of an ASP.NET MVC application</span></span>
- <span data-ttu-id="c6618-180">Zostały dowiedzieliśmy się, jak uruchomić naszą witrynę sieci Web za pomocą programu ASP.NET Development Server</span><span class="sxs-lookup"><span data-stu-id="c6618-180">We've learned how to run our website using the ASP.NET Development Server</span></span>
- <span data-ttu-id="c6618-181">Utworzyliśmy dwa klasy kontrolera: HomeController i StoreController</span><span class="sxs-lookup"><span data-stu-id="c6618-181">We've created two Controller classes: a HomeController and a StoreController</span></span>
- <span data-ttu-id="c6618-182">Dodaliśmy metod akcji do naszej kontrolerów, które odpowiadają na żądania adresu URL i zwrócić tekst do przeglądarki</span><span class="sxs-lookup"><span data-stu-id="c6618-182">We've added Action Methods to our controllers which respond to URL requests and return text to the browser</span></span>


> [!div class="step-by-step"]
> <span data-ttu-id="c6618-183">[Poprzednie](mvc-music-store-part-1.md)
> [dalej](mvc-music-store-part-3.md)</span><span class="sxs-lookup"><span data-stu-id="c6618-183">[Previous](mvc-music-store-part-1.md)
[Next](mvc-music-store-part-3.md)</span></span>
