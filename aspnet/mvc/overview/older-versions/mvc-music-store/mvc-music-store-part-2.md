---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-2
title: 'Część 2: Kontrolery | Dokumentacja firmy Microsoft'
author: jongalloway
description: W tej serii samouczków szczegółowo opisuje wszystkie etapy, tworzenie przykładowej aplikacji platformy ASP.NET MVC Music Store. Część 2 obejmuje kontrolerów.
ms.author: aspnetcontent
ms.date: 04/21/2011
ms.assetid: 998ce4e1-9d72-435b-8f1c-399a10ae4360
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-2
msc.type: authoredcontent
ms.openlocfilehash: 8824d5d2f5670aee2df6dc6e74767e4a851dd4ae
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/05/2018
ms.locfileid: "37841098"
---
<a name="part-2-controllers"></a><span data-ttu-id="8ce99-104">Część 2: kontrolery</span><span class="sxs-lookup"><span data-stu-id="8ce99-104">Part 2: Controllers</span></span>
====================
<span data-ttu-id="8ce99-105">przez [Galloway'em Jon](https://github.com/jongalloway)</span><span class="sxs-lookup"><span data-stu-id="8ce99-105">by [Jon Galloway](https://github.com/jongalloway)</span></span>

> <span data-ttu-id="8ce99-106">MVC Music Store jest aplikacją z samouczka, który wprowadzono i opisano krok po kroku, jak używać platformy ASP.NET MVC i programu Visual Studio do tworzenia aplikacji internetowych.</span><span class="sxs-lookup"><span data-stu-id="8ce99-106">The MVC Music Store is a tutorial application that introduces and explains step-by-step how to use ASP.NET MVC and Visual Studio for web development.</span></span>  
>   
> <span data-ttu-id="8ce99-107">MVC Music Store jest uproszczone przykładową implementację magazynu sprzedaje utworów muzycznych albumy online, która implementuje podstawowej witryny administracji, logowania użytkownika i funkcje koszyka zakupów.</span><span class="sxs-lookup"><span data-stu-id="8ce99-107">The MVC Music Store is a lightweight sample store implementation which sells music albums online, and implements basic site administration, user sign-in, and shopping cart functionality.</span></span>  
>   
> <span data-ttu-id="8ce99-108">W tej serii samouczków szczegółowo opisuje wszystkie etapy, tworzenie przykładowej aplikacji platformy ASP.NET MVC Music Store.</span><span class="sxs-lookup"><span data-stu-id="8ce99-108">This tutorial series details all of the steps taken to build the ASP.NET MVC Music Store sample application.</span></span> <span data-ttu-id="8ce99-109">Część 2 obejmuje kontrolerów.</span><span class="sxs-lookup"><span data-stu-id="8ce99-109">Part 2 covers Controllers.</span></span>


<span data-ttu-id="8ce99-110">Za pomocą platform sieci web tradycyjnych przychodzących adresów URL są zwykle mapowane na pliki na dysku.</span><span class="sxs-lookup"><span data-stu-id="8ce99-110">With traditional web frameworks, incoming URLs are typically mapped to files on disk.</span></span> <span data-ttu-id="8ce99-111">Na przykład: żądania dla adresu URL, takich jak "/ Products.aspx" lub "/ Products.php" mogą być przetwarzane przez plik "Products.aspx" lub "Products.php".</span><span class="sxs-lookup"><span data-stu-id="8ce99-111">For example: a request for a URL like "/Products.aspx" or "/Products.php" might be processed by a "Products.aspx" or "Products.php" file.</span></span>

<span data-ttu-id="8ce99-112">Oparte na sieci Web platformy MVC adresy URL są mapowane na kod serwera w nieco inny sposób.</span><span class="sxs-lookup"><span data-stu-id="8ce99-112">Web-based MVC frameworks map URLs to server code in a slightly different way.</span></span> <span data-ttu-id="8ce99-113">Zamiast mapowania przychodzących adresów URL do plików, zamiast tego mapowania ich adresy URL do metod w klasach.</span><span class="sxs-lookup"><span data-stu-id="8ce99-113">Instead of mapping incoming URLs to files, they instead map URLs to methods on classes.</span></span> <span data-ttu-id="8ce99-114">Te klasy są nazywane "Kontrolerów" i są one odpowiedzialne za przetwarzanie przychodzących żądań HTTP, Obsługa danych wejściowych użytkownika, pobierania i zapisywania danych i określania odpowiedź do wysłania z powrotem do klienta (wyświetlania kodu HTML, Pobierz plik, przekierowanie na inny Adres URL itp.).</span><span class="sxs-lookup"><span data-stu-id="8ce99-114">These classes are called "Controllers" and they are responsible for processing incoming HTTP requests, handling user input, retrieving and saving data, and determining the response to send back to the client (display HTML, download a file, redirect to a different URL, etc.).</span></span>

## <a name="adding-a-homecontroller"></a><span data-ttu-id="8ce99-115">Dodawanie HomeController</span><span class="sxs-lookup"><span data-stu-id="8ce99-115">Adding a HomeController</span></span>

<span data-ttu-id="8ce99-116">Nasza aplikacja MVC Music Store rozpocznie się przez dodawanie klasy kontrolera, który będzie obsługiwać adresy URL do strony głównej witryny firmy Microsoft.</span><span class="sxs-lookup"><span data-stu-id="8ce99-116">We'll begin our MVC Music Store application by adding a Controller class that will handle URLs to the Home page of our site.</span></span> <span data-ttu-id="8ce99-117">Firma Microsoft będzie zgodne z konwencjami nazewnictwa domyślnego platformy ASP.NET MVC i wywołać go HomeController.</span><span class="sxs-lookup"><span data-stu-id="8ce99-117">We'll follow the default naming conventions of ASP.NET MVC and call it HomeController.</span></span>

<span data-ttu-id="8ce99-118">Kliknij prawym przyciskiem myszy folder "Kontrolerów" w Eksploratorze rozwiązań i wybierz pozycję "Dodaj", a następnie polecenie "Kontrolera...":</span><span class="sxs-lookup"><span data-stu-id="8ce99-118">Right-click the "Controllers" folder within the Solution Explorer and select "Add", and then the "Controller…" command:</span></span>

![](mvc-music-store-part-2/_static/image1.jpg)

<span data-ttu-id="8ce99-119">Zostanie wyświetlone okno dialogowe "Dodaj kontroler".</span><span class="sxs-lookup"><span data-stu-id="8ce99-119">This will bring up the "Add Controller" dialog.</span></span> <span data-ttu-id="8ce99-120">Nazwa kontrolera "HomeController", a następnie naciśnij przycisk Dodaj.</span><span class="sxs-lookup"><span data-stu-id="8ce99-120">Name the controller "HomeController" and press the Add button.</span></span>

![](mvc-music-store-part-2/_static/image1.png)

<span data-ttu-id="8ce99-121">Spowoduje to utworzenie nowego pliku HomeController.cs, używając następującego kodu:</span><span class="sxs-lookup"><span data-stu-id="8ce99-121">This will create a new file, HomeController.cs, with the following code:</span></span>

[!code-csharp[Main](mvc-music-store-part-2/samples/sample1.cs)]

<span data-ttu-id="8ce99-122">Aby rozpocząć ułatwianiu, Przyjrzyjmy zastąpić Index — metoda prostą metodę, która po prostu zwraca ciąg.</span><span class="sxs-lookup"><span data-stu-id="8ce99-122">To start as simply as possible, let's replace the Index method with a simple method that just returns a string.</span></span> <span data-ttu-id="8ce99-123">My sprawiamy, żeby dwie zmiany:</span><span class="sxs-lookup"><span data-stu-id="8ce99-123">We'll make two changes:</span></span>

- <span data-ttu-id="8ce99-124">Zmiana metody w celu zwrócenia ciągu zamiast element ActionResult</span><span class="sxs-lookup"><span data-stu-id="8ce99-124">Change the method to return a string instead of an ActionResult</span></span>
- <span data-ttu-id="8ce99-125">Zmień instrukcję return powrót do "Hello z domu"</span><span class="sxs-lookup"><span data-stu-id="8ce99-125">Change the return statement to return "Hello from Home"</span></span>

<span data-ttu-id="8ce99-126">Metoda powinna teraz wyglądać następująco:</span><span class="sxs-lookup"><span data-stu-id="8ce99-126">The method should now look like this:</span></span>

[!code-csharp[Main](mvc-music-store-part-2/samples/sample2.cs)]

## <a name="running-the-application"></a><span data-ttu-id="8ce99-127">Uruchamianie aplikacji</span><span class="sxs-lookup"><span data-stu-id="8ce99-127">Running the Application</span></span>

<span data-ttu-id="8ce99-128">Teraz uruchommy lokacji.</span><span class="sxs-lookup"><span data-stu-id="8ce99-128">Now let's run the site.</span></span> <span data-ttu-id="8ce99-129">Możemy start Nasz serwer sieci web i wypróbować lokacji przy użyciu dowolnej z następujących czynności:</span><span class="sxs-lookup"><span data-stu-id="8ce99-129">We can start our web-server and try out the site using any of the following::</span></span>

- <span data-ttu-id="8ce99-130">Wybierz element menu Start Debugging ⇨ debugowania</span><span class="sxs-lookup"><span data-stu-id="8ce99-130">Choose the Debug ⇨ Start Debugging menu item</span></span>
- <span data-ttu-id="8ce99-131">Kliknij przycisk zieloną strzałkę na pasku narzędzi ![](mvc-music-store-part-2/_static/image2.jpg)</span><span class="sxs-lookup"><span data-stu-id="8ce99-131">Click the Green arrow button in the toolbar ![](mvc-music-store-part-2/_static/image2.jpg)</span></span>
- <span data-ttu-id="8ce99-132">Użyj skrótu klawiaturowego, F5.</span><span class="sxs-lookup"><span data-stu-id="8ce99-132">Use the keyboard shortcut, F5.</span></span>

<span data-ttu-id="8ce99-133">Przy użyciu dowolnej z powyższych kroków spowoduje kompilacji w projekcie i spowodować, że ASP.NET Development Server, która jest wbudowana w Visual Web Developer można uruchomić.</span><span class="sxs-lookup"><span data-stu-id="8ce99-133">Using any of the above steps will compile our project, and then cause the ASP.NET Development Server that is built-into Visual Web Developer to start.</span></span> <span data-ttu-id="8ce99-134">Powiadomienie pojawi się w dolnym rogu ekranu, aby wskazać, że ASP.NET Development Server została uruchomiona i wyświetli numer portu, że jest on uruchomiony pod.</span><span class="sxs-lookup"><span data-stu-id="8ce99-134">A notification will appear in the bottom corner of the screen to indicate that the ASP.NET Development Server has started up, and will show the port number that it is running under.</span></span>

![](mvc-music-store-part-2/_static/image2.png)

<span data-ttu-id="8ce99-135">Visual Web Developer następnie zostanie automatycznie otwarte okno przeglądarki, w którego adres URL wskazuje na naszym serwerze sieci web.</span><span class="sxs-lookup"><span data-stu-id="8ce99-135">Visual Web Developer will then automatically open a browser window whose URL points to our web-server.</span></span> <span data-ttu-id="8ce99-136">Pozwoli to nam możesz szybko wypróbować usługę naszej aplikacji sieci web:</span><span class="sxs-lookup"><span data-stu-id="8ce99-136">This will allow us to quickly try out our web application:</span></span>

![](mvc-music-store-part-2/_static/image3.png)

<span data-ttu-id="8ce99-137">Porządku, to było całkiem szybkiego — utworzyliśmy nową witrynę sieci Web, dodane trzy śródwierszową i mamy tekstu w przeglądarce.</span><span class="sxs-lookup"><span data-stu-id="8ce99-137">Okay, that was pretty quick – we created a new website, added a three line function, and we've got text in a browser.</span></span> <span data-ttu-id="8ce99-138">Nie okazji do analizy, ale jest rozpoczęcia.</span><span class="sxs-lookup"><span data-stu-id="8ce99-138">Not rocket science, but it's a start.</span></span>

<span data-ttu-id="8ce99-139">*Uwaga: Visual Web Developer obejmuje ASP.NET Development Server, co spowoduje uruchomienie witryny sieci Web na wielu bezpłatnych losowy "port". Na powyższym zrzucie ekranu, witryna działa na `http://localhost:26641/`, więc używa portu 26641. Numer portu będzie różna. Odnosimy /Store/Browse like adresy URL w tym samouczku, które zaczną po numer portu. Zakładając, że numer portu w danych 26641, przechodząc do/Store/przeglądania oznacza przejście do `http://localhost:26641/Store/Browse`.*</span><span class="sxs-lookup"><span data-stu-id="8ce99-139">*Note: Visual Web Developer includes the ASP.NET Development Server, which will run your website on a random free "port" number. In the screenshot above, the site is running at `http://localhost:26641/`, so it's using port 26641. Your port number will be different. When we talk about URL's like /Store/Browse in this tutorial, that will go after the port number. Assuming a port number of 26641, browsing to /Store/Browse will mean browsing to `http://localhost:26641/Store/Browse`.*</span></span>

## <a name="adding-a-storecontroller"></a><span data-ttu-id="8ce99-140">Dodawanie StoreController</span><span class="sxs-lookup"><span data-stu-id="8ce99-140">Adding a StoreController</span></span>

<span data-ttu-id="8ce99-141">Dodaliśmy HomeController prostego, który implementuje strony głównej witryny firmy Microsoft.</span><span class="sxs-lookup"><span data-stu-id="8ce99-141">We added a simple HomeController that implements the Home Page of our site.</span></span> <span data-ttu-id="8ce99-142">Teraz Dodajmy inny kontroler, który użyjemy do implementacji funkcji przeglądania naszego sklepu music.</span><span class="sxs-lookup"><span data-stu-id="8ce99-142">Let's now add another controller that we'll use to implement the browsing functionality of our music store.</span></span> <span data-ttu-id="8ce99-143">Kontrolera magazynu obsługuje trzy scenariusze:</span><span class="sxs-lookup"><span data-stu-id="8ce99-143">Our store controller will support three scenarios:</span></span>

- <span data-ttu-id="8ce99-144">Strona listy gatunki muzyki w naszym Sklepie utworów muzycznych</span><span class="sxs-lookup"><span data-stu-id="8ce99-144">A listing page of the music genres in our music store</span></span>
- <span data-ttu-id="8ce99-145">Strony przeglądania, który wymienia wszystkie albumów muzycznych określonego rodzaju</span><span class="sxs-lookup"><span data-stu-id="8ce99-145">A browse page that lists all of the music albums in a particular genre</span></span>
- <span data-ttu-id="8ce99-146">Strona zawierająca szczegóły informacji na temat albumu określonych utworów muzycznych</span><span class="sxs-lookup"><span data-stu-id="8ce99-146">A details page that shows information about a specific music album</span></span>

<span data-ttu-id="8ce99-147">Rozpoczniemy pracę, dodając nową klasę StoreController...</span><span class="sxs-lookup"><span data-stu-id="8ce99-147">We'll start by adding a new StoreController class..</span></span> <span data-ttu-id="8ce99-148">Nie jest jeszcze można zatrzymać aplikacji przez zamknięcie przeglądarki lub wybierając element menu Zatrzymaj debugowanie ⇨ debugowania.</span><span class="sxs-lookup"><span data-stu-id="8ce99-148">If you haven't already, stop running the application either by closing the browser or selecting the Debug ⇨ Stop Debugging menu item.</span></span>

<span data-ttu-id="8ce99-149">Teraz można dodać nowe StoreController.</span><span class="sxs-lookup"><span data-stu-id="8ce99-149">Now add a new StoreController.</span></span> <span data-ttu-id="8ce99-150">Tak samo, jak postępowanie z HomeController, możemy to zrobić, klikając prawym przyciskiem myszy w folderze "Kontrolerów" w Eksploratorze rozwiązań i wybierając Add -&gt;kontrolera elementu menu.</span><span class="sxs-lookup"><span data-stu-id="8ce99-150">Just like we did with HomeController, we'll do this by right-clicking on the "Controllers" folder within the Solution Explorer and choosing the Add-&gt;Controller menu item</span></span>

![](mvc-music-store-part-2/_static/image4.png)

<span data-ttu-id="8ce99-151">Nasz nowy StoreController ma już metodę "Index".</span><span class="sxs-lookup"><span data-stu-id="8ce99-151">Our new StoreController already has an "Index" method.</span></span> <span data-ttu-id="8ce99-152">Użyjemy tej metody "Index", aby zaimplementować naszą stronę listy, który zawiera listę wszystkich gatunki w naszym music store —.</span><span class="sxs-lookup"><span data-stu-id="8ce99-152">We'll use this "Index" method to implement our listing page that lists all genres in our music store.</span></span> <span data-ttu-id="8ce99-153">Dodamy również dwie dodatkowe metody do zaimplementowania dwa inne scenariusze chcemy, aby nasz StoreController do obsługi: Przeglądaj i szczegółowe informacje.</span><span class="sxs-lookup"><span data-stu-id="8ce99-153">We'll also add two additional methods to implement the two other scenarios we want our StoreController to handle: Browse and Details.</span></span>

<span data-ttu-id="8ce99-154">Te metody (indeks i przeglądania szczegółów) w ramach kontrolera są nazywane "Akcji kontrolera", a jako już przeczytane z metodą akcji HomeController.Index (), swojej pracy ma odpowiadać na żądania adres URL i (ogólnie rzecz biorąc) określić zawartość powinny być wysyłane z powrotem do przeglądarki lub użytkownik, który wywołał adresu URL.</span><span class="sxs-lookup"><span data-stu-id="8ce99-154">These methods (Index, Browse and Details) within our Controller are called "Controller Actions", and as you've already seen with the HomeController.Index()action method, their job is to respond to URL requests and (generally speaking) determine what content should be sent back to the browser or user that invoked the URL.</span></span>

<span data-ttu-id="8ce99-155">Zaczniemy naszej implementacji StoreController, zmieniając theIndex() metoda zwraca ciąg "Hello z Store.Index()" i dodamy podobne metody Browse() i Details():</span><span class="sxs-lookup"><span data-stu-id="8ce99-155">We'll start our StoreController implementation by changing theIndex() method to return the string "Hello from Store.Index()" and we'll add similar methods for Browse() and Details():</span></span>

[!code-csharp[Main](mvc-music-store-part-2/samples/sample3.cs)]

<span data-ttu-id="8ce99-156">Uruchom projekt ponownie i Przeglądaj następujące adresy URL:</span><span class="sxs-lookup"><span data-stu-id="8ce99-156">Run the project again and browse the following URLs:</span></span>

- <span data-ttu-id="8ce99-157">/ Store</span><span class="sxs-lookup"><span data-stu-id="8ce99-157">/Store</span></span>
- <span data-ttu-id="8ce99-158">/ Store/przeglądania</span><span class="sxs-lookup"><span data-stu-id="8ce99-158">/Store/Browse</span></span>
- <span data-ttu-id="8ce99-159">/ Store/Details</span><span class="sxs-lookup"><span data-stu-id="8ce99-159">/Store/Details</span></span>

<span data-ttu-id="8ce99-160">Uzyskiwanie dostępu do tych adresów URL wywołania metody akcji w ramach kontrolera i zwracania odpowiedzi ciąg:</span><span class="sxs-lookup"><span data-stu-id="8ce99-160">Accessing these URLs will invoke the action methods within our Controller and return string responses:</span></span>

![](mvc-music-store-part-2/_static/image5.png)

<span data-ttu-id="8ce99-161">To doskonałe rozwiązanie, ale są tylko stałe ciągi.</span><span class="sxs-lookup"><span data-stu-id="8ce99-161">That's great, but these are just constant strings.</span></span> <span data-ttu-id="8ce99-162">Upewnijmy się, ich dynamiczny, dzięki czemu pobierają informacje z adresu URL i wyświetlania ich w danych wyjściowych strony.</span><span class="sxs-lookup"><span data-stu-id="8ce99-162">Let's make them dynamic, so they take information from the URL and display it in the page output.</span></span>

<span data-ttu-id="8ce99-163">Pierwsze zmienimy metody akcji przeglądania można pobrać wartości querystring w adresie URL.</span><span class="sxs-lookup"><span data-stu-id="8ce99-163">First we'll change the Browse action method to retrieve a querystring value from the URL.</span></span> <span data-ttu-id="8ce99-164">Możemy to zrobić, dodając parametr "gatunku" do naszych metody akcji.</span><span class="sxs-lookup"><span data-stu-id="8ce99-164">We can do this by adding a "genre" parameter to our action method.</span></span> <span data-ttu-id="8ce99-165">W takim przypadku to ASP.NET MVC automatycznie przekazują żadnych parametrów post formularza lub ciągu kwerendy o nazwie "gatunku" do naszych metody akcji, gdy jest wywoływana.</span><span class="sxs-lookup"><span data-stu-id="8ce99-165">When we do this ASP.NET MVC will automatically pass any querystring or form post parameters named "genre" to our action method when it is invoked.</span></span>

[!code-csharp[Main](mvc-music-store-part-2/samples/sample4.cs)]

<span data-ttu-id="8ce99-166">*Uwaga: Firma Microsoft za pomocą metody narzędzie HttpUtility.HtmlEncode oczyszczają danych wejściowych użytkownika. Uniemożliwia to użytkownikom wprowadzanie Javascript do naszych widoku z linkiem, takich jak /Store/Browse? Gatunku =&lt;skryptu&gt;window.location= "http://hackersite.com"&lt;/script&gt;.*</span><span class="sxs-lookup"><span data-stu-id="8ce99-166">*Note: We're using the HttpUtility.HtmlEncode utility method to sanitize the user input. This prevents users from injecting Javascript into our View with a link like /Store/Browse?Genre=&lt;script&gt;window.location='http://hackersite.com'&lt;/script&gt;.*</span></span>

<span data-ttu-id="8ce99-167">Teraz możemy przejść do/Store/przeglądania? Gatunku = Najdywania</span><span class="sxs-lookup"><span data-stu-id="8ce99-167">Now let's browse to /Store/Browse?Genre=Disco</span></span>

![](mvc-music-store-part-2/_static/image6.png)

<span data-ttu-id="8ce99-168">Następnie zmienimy akcji Details do odczytywania i wyświetlić parametr wejściowy identyfikator.</span><span class="sxs-lookup"><span data-stu-id="8ce99-168">Let's next change the Details action to read and display an input parameter named ID.</span></span> <span data-ttu-id="8ce99-169">W odróżnieniu od naszych Poprzednia metoda firma Microsoft nie będzie można osadzanie wartość Identyfikatora jako parametr ciągu kwerendy.</span><span class="sxs-lookup"><span data-stu-id="8ce99-169">Unlike our previous method, we won't be embedding the ID value as a querystring parameter.</span></span> <span data-ttu-id="8ce99-170">Zamiast tego osadzimy, go bezpośrednio z poziomu samego adresu.</span><span class="sxs-lookup"><span data-stu-id="8ce99-170">Instead we'll embed it directly within the URL itself.</span></span> <span data-ttu-id="8ce99-171">Na przykład: /Store/Details/5.</span><span class="sxs-lookup"><span data-stu-id="8ce99-171">For example: /Store/Details/5.</span></span>

<span data-ttu-id="8ce99-172">ASP.NET MVC pozwala nam to łatwo zrobić bez konieczności nic konfigurować.</span><span class="sxs-lookup"><span data-stu-id="8ce99-172">ASP.NET MVC lets us easily do this without having to configure anything.</span></span> <span data-ttu-id="8ce99-173">Konwencji routingu domyślnego platformy ASP.NET MVC jest segment adresu URL po nazwie metody akcji jako parametr o nazwie "ID".</span><span class="sxs-lookup"><span data-stu-id="8ce99-173">ASP.NET MVC's default routing convention is to treat the segment of a URL after the action method name as a parameter named "ID".</span></span> <span data-ttu-id="8ce99-174">Jeśli metoda akcji zawiera parametr o nazwie identyfikator następnie platformy ASP.NET MVC zostanie automatycznie przekazać segment adresu URL dla Ciebie jako parametr.</span><span class="sxs-lookup"><span data-stu-id="8ce99-174">If your action method has a parameter named ID then ASP.NET MVC will automatically pass the URL segment to you as a parameter.</span></span>

[!code-csharp[Main](mvc-music-store-part-2/samples/sample5.cs)]

<span data-ttu-id="8ce99-175">Uruchom aplikację, a następnie przejdź do /Store/Details/5:</span><span class="sxs-lookup"><span data-stu-id="8ce99-175">Run the application and browse to /Store/Details/5:</span></span>

![](mvc-music-store-part-2/_static/image7.png)

<span data-ttu-id="8ce99-176">Przypomnijmy przedstawiające wprowadzone aktualizacje do tej pory:</span><span class="sxs-lookup"><span data-stu-id="8ce99-176">Let's recap what we've done so far:</span></span>

- <span data-ttu-id="8ce99-177">Utworzyliśmy nowy projekt ASP.NET MVC w Visual Web Developer</span><span class="sxs-lookup"><span data-stu-id="8ce99-177">We've created a new ASP.NET MVC project in Visual Web Developer</span></span>
- <span data-ttu-id="8ce99-178">Wiemy już struktury folderów podstawowych aplikacji ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="8ce99-178">We've discussed the basic folder structure of an ASP.NET MVC application</span></span>
- <span data-ttu-id="8ce99-179">Nauczyliśmy się sposobu uruchamiania naszej witryny sieci Web za pomocą programu ASP.NET Development Server</span><span class="sxs-lookup"><span data-stu-id="8ce99-179">We've learned how to run our website using the ASP.NET Development Server</span></span>
- <span data-ttu-id="8ce99-180">Utworzyliśmy dwie klasy kontrolera: HomeController i StoreController</span><span class="sxs-lookup"><span data-stu-id="8ce99-180">We've created two Controller classes: a HomeController and a StoreController</span></span>
- <span data-ttu-id="8ce99-181">Dodaliśmy metod akcji do naszych kontrolery, które odpowiadają na żądania adres URL i zwracają tekst w przeglądarce</span><span class="sxs-lookup"><span data-stu-id="8ce99-181">We've added Action Methods to our controllers which respond to URL requests and return text to the browser</span></span>


> [!div class="step-by-step"]
> <span data-ttu-id="8ce99-182">[Poprzednie](mvc-music-store-part-1.md)
> [dalej](mvc-music-store-part-3.md)</span><span class="sxs-lookup"><span data-stu-id="8ce99-182">[Previous](mvc-music-store-part-1.md)
[Next](mvc-music-store-part-3.md)</span></span>
