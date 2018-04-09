---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-8
title: 'Część 8: Koszyka z aktualizacje interfejsu Ajax | Dokumentacja firmy Microsoft'
author: jongalloway
description: Ten samouczek serii zawiera szczegóły dotyczące wszystkich kroków tworzenia przykładowej aplikacji ASP.NET MVC utworów muzycznych magazynu. Część 8 obejmuje koszyk z aktualizacje interfejsu Ajax.
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/21/2011
ms.topic: article
ms.assetid: 26b2f55e-ed42-4277-89b0-c941eb754145
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-8
msc.type: authoredcontent
ms.openlocfilehash: 195c01ff0d71b2bfd0c00e71244d47a166330921
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
---
<a name="part-8-shopping-cart-with-ajax-updates"></a><span data-ttu-id="c3f09-104">Część 8: Koszyku z aktualizacje interfejsu Ajax</span><span class="sxs-lookup"><span data-stu-id="c3f09-104">Part 8: Shopping Cart with Ajax Updates</span></span>
====================
<span data-ttu-id="c3f09-105">przez [Galloway Jan](https://github.com/jongalloway)</span><span class="sxs-lookup"><span data-stu-id="c3f09-105">by [Jon Galloway](https://github.com/jongalloway)</span></span>

> <span data-ttu-id="c3f09-106">Magazyn utworów muzycznych MVC jest samouczek aplikacji, które wprowadzono i opisano krok po kroku, jak używać do tworzenia aplikacji sieci web platformy ASP.NET MVC i Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="c3f09-106">The MVC Music Store is a tutorial application that introduces and explains step-by-step how to use ASP.NET MVC and Visual Studio for web development.</span></span>  
>   
> <span data-ttu-id="c3f09-107">Magazyn utworów muzycznych MVC jest implementacja magazynu lekkie próbki, co sprzedaje albumów muzycznych w trybie online i implementuje podstawowej witryny administracji, logowania użytkownika i funkcje koszyka zakupów.</span><span class="sxs-lookup"><span data-stu-id="c3f09-107">The MVC Music Store is a lightweight sample store implementation which sells music albums online, and implements basic site administration, user sign-in, and shopping cart functionality.</span></span>  
>   
> <span data-ttu-id="c3f09-108">Ten samouczek serii zawiera szczegóły dotyczące wszystkich kroków tworzenia przykładowej aplikacji ASP.NET MVC utworów muzycznych magazynu.</span><span class="sxs-lookup"><span data-stu-id="c3f09-108">This tutorial series details all of the steps taken to build the ASP.NET MVC Music Store sample application.</span></span> <span data-ttu-id="c3f09-109">Część 8 obejmuje koszyk z aktualizacje interfejsu Ajax.</span><span class="sxs-lookup"><span data-stu-id="c3f09-109">Part 8 covers Shopping Cart with Ajax Updates.</span></span>


<span data-ttu-id="c3f09-110">Firma Microsoft będzie umożliwiają użytkownikom umieścić albumów w ich koszyka bez rejestrowania, ale będzie trzeba zarejestrować jako gości, aby zakończyć proces realizacji transakcji.</span><span class="sxs-lookup"><span data-stu-id="c3f09-110">We'll allow users to place albums in their cart without registering, but they'll need to register as guests to complete checkout.</span></span> <span data-ttu-id="c3f09-111">Proces kupowania i wyewidencjonowania spowoduje podzielić na dwa kontrolery: kontroler ShoppingCart, dzięki czemu anonimowo Dodawanie elementów do koszyka i kontrolera wyewidencjonowania, który obsługuje realizacji.</span><span class="sxs-lookup"><span data-stu-id="c3f09-111">The shopping and checkout process will be separated into two controllers: a ShoppingCart Controller which allows anonymously adding items to a cart, and a Checkout Controller which handles the checkout process.</span></span> <span data-ttu-id="c3f09-112">Firma Microsoft będzie rozpoczynać się od koszyka, w tej sekcji, a następnie proces wyewidencjonowania w poniższej sekcji kompilacji.</span><span class="sxs-lookup"><span data-stu-id="c3f09-112">We'll start with the Shopping Cart in this section, then build the Checkout process in the following section.</span></span>

## <a name="adding-the-cart-order-and-orderdetail-model-classes"></a><span data-ttu-id="c3f09-113">Dodawanie klas modelu koszyka, kolejność i OrderDetail</span><span class="sxs-lookup"><span data-stu-id="c3f09-113">Adding the Cart, Order, and OrderDetail model classes</span></span>

<span data-ttu-id="c3f09-114">Procesów naszego koszyka i wyewidencjonowania spowoduje, że korzystanie z niektórych nowych klas.</span><span class="sxs-lookup"><span data-stu-id="c3f09-114">Our Shopping Cart and Checkout processes will make use of some new classes.</span></span> <span data-ttu-id="c3f09-115">Kliknij prawym przyciskiem myszy folder modeli i dodać klasę koszyka (Cart.cs) z następującym kodem.</span><span class="sxs-lookup"><span data-stu-id="c3f09-115">Right-click the Models folder and add a Cart class (Cart.cs) with the following code.</span></span>

[!code-csharp[Main](mvc-music-store-part-8/samples/sample1.cs)]

<span data-ttu-id="c3f09-116">Ta klasa jest bardzo podobne do innych osób, możemy używany wykonanej do tej pory, z wyjątkiem atrybutu [klucz] właściwości RecordId.</span><span class="sxs-lookup"><span data-stu-id="c3f09-116">This class is pretty similar to others we've used so far, with the exception of the [Key] attribute for the RecordId property.</span></span> <span data-ttu-id="c3f09-117">Elementy naszego koszyka będzie mieć identyfikator ciągu o nazwie CartID umożliwia anonimowego zakupów, ale w tabeli przedstawiono całkowitą klucza podstawowego o nazwie RecordId.</span><span class="sxs-lookup"><span data-stu-id="c3f09-117">Our Cart items will have a string identifier named CartID to allow anonymous shopping, but the table includes an integer primary key named RecordId.</span></span> <span data-ttu-id="c3f09-118">Konwencja pierwszy kod Entity Framework oczekuje klucz podstawowy dla tabeli o nazwie koszyka będzie CartId lub identyfikator, że firma Microsoft może łatwo zastępuj za pomocą adnotacji lub kod Jeśli chcemy.</span><span class="sxs-lookup"><span data-stu-id="c3f09-118">By convention, Entity Framework Code-First expects that the primary key for a table named Cart will be either CartId or ID, but we can easily override that via annotations or code if we want.</span></span> <span data-ttu-id="c3f09-119">To jest przykładowy sposób możemy użyć prostego konwencje w Entity Framework kod pierwszego po ich własnych nam, ale firma Microsoft jest nie ograniczone przez nich podczas nie.</span><span class="sxs-lookup"><span data-stu-id="c3f09-119">This is an example of how we can use the simple conventions in Entity Framework Code-First when they suit us, but we're not constrained by them when they don't.</span></span>

<span data-ttu-id="c3f09-120">Następnie Dodaj klasę zamówienia (Order.cs) z następującym kodem.</span><span class="sxs-lookup"><span data-stu-id="c3f09-120">Next, add an Order class (Order.cs) with the following code.</span></span>

[!code-csharp[Main](mvc-music-store-part-8/samples/sample2.cs)]

<span data-ttu-id="c3f09-121">Ta klasa śledzi informacje o kolejności Podsumowanie i dostarczania.</span><span class="sxs-lookup"><span data-stu-id="c3f09-121">This class tracks summary and delivery information for an order.</span></span> <span data-ttu-id="c3f09-122">**Nie będzie jeszcze skompilować**, ponieważ ma on właściwości nawigacji SzczegółyZamówień, która jest zależna od klasy nie utworzono jeszcze.</span><span class="sxs-lookup"><span data-stu-id="c3f09-122">**It won't compile yet**, because it has an OrderDetails navigation property which depends on a class we haven't created yet.</span></span> <span data-ttu-id="c3f09-123">Załóżmy ustalić, czy obecnie przez dodanie klasy o nazwie OrderDetail.cs, dodając następujący kod.</span><span class="sxs-lookup"><span data-stu-id="c3f09-123">Let's fix that now by adding a class named OrderDetail.cs, adding the following code.</span></span>

[!code-csharp[Main](mvc-music-store-part-8/samples/sample3.cs)]

<span data-ttu-id="c3f09-124">Wybierzemy jeden ostatniej aktualizacji do naszej klasy MusicStoreEntities uwzględnienie DbSets, który ujawnia te nowe klasy modelu, w tym również klasę DbSet&lt;wykonawcy&gt;.</span><span class="sxs-lookup"><span data-stu-id="c3f09-124">We'll make one last update to our MusicStoreEntities class to include DbSets which expose those new Model classes, also including a DbSet&lt;Artist&gt;.</span></span> <span data-ttu-id="c3f09-125">Zaktualizowano klasy MusicStoreEntities pojawia się jako poniżej.</span><span class="sxs-lookup"><span data-stu-id="c3f09-125">The updated MusicStoreEntities class appears as below.</span></span>

[!code-csharp[Main](mvc-music-store-part-8/samples/sample4.cs)]

## <a name="managing-the-shopping-cart-business-logic"></a><span data-ttu-id="c3f09-126">Zarządzanie koszyku logika biznesowa</span><span class="sxs-lookup"><span data-stu-id="c3f09-126">Managing the Shopping Cart business logic</span></span>

<span data-ttu-id="c3f09-127">Następnie utworzymy klasy ShoppingCart w folderze modeli.</span><span class="sxs-lookup"><span data-stu-id="c3f09-127">Next, we'll create the ShoppingCart class in the Models folder.</span></span> <span data-ttu-id="c3f09-128">ShoppingCart model obsługuje dostęp do danych do tabeli koszyka.</span><span class="sxs-lookup"><span data-stu-id="c3f09-128">The ShoppingCart model handles data access to the Cart table.</span></span> <span data-ttu-id="c3f09-129">Ponadto obsłuży logiki biznesowej na dodawanie i usuwanie elementów z koszyka zakupów.</span><span class="sxs-lookup"><span data-stu-id="c3f09-129">Additionally, it will handle the business logic to for adding and removing items from the shopping cart.</span></span>

<span data-ttu-id="c3f09-130">Ponieważ nie chcemy wymagać od użytkowników zalogowania się do konta tak dodać elementy do ich koszyk, firma Microsoft będzie przypisywać użytkowników tymczasowego Unikatowy identyfikator (przy użyciu identyfikatora GUID lub Unikatowy identyfikator globalny) podczas uzyskiwania dostępu do koszyka.</span><span class="sxs-lookup"><span data-stu-id="c3f09-130">Since we don't want to require users to sign up for an account just to add items to their shopping cart, we will assign users a temporary unique identifier (using a GUID, or globally unique identifier) when they access the shopping cart.</span></span> <span data-ttu-id="c3f09-131">Ten identyfikator przy użyciu klasy sesji ASP.NET będzie przechowywane.</span><span class="sxs-lookup"><span data-stu-id="c3f09-131">We'll store this ID using the ASP.NET Session class.</span></span>

<span data-ttu-id="c3f09-132">*Uwaga: Sesja programu ASP.NET jest wygodne miejsce do przechowywania informacji o użytkowniku, która wygaśnie po ich działania lokacji. Natomiast nadużycia stanu sesji może mieć wpływ na wydajność w witrynach większy, używanie firmę światła będzie działać również w celach demonstracyjnych.*</span><span class="sxs-lookup"><span data-stu-id="c3f09-132">*Note: The ASP.NET Session is a convenient place to store user-specific information which will expire after they leave the site. While misuse of session state can have performance implications on larger sites, our light use will work well for demonstration purposes.*</span></span>

<span data-ttu-id="c3f09-133">Klasa ShoppingCart udostępnia następujące metody:</span><span class="sxs-lookup"><span data-stu-id="c3f09-133">The ShoppingCart class exposes the following methods:</span></span>

<span data-ttu-id="c3f09-134">**AddToCart** przyjmuje jako parametr albumu i dodaje go do koszyka użytkownika.</span><span class="sxs-lookup"><span data-stu-id="c3f09-134">**AddToCart** takes an Album as a parameter and adds it to the user's cart.</span></span> <span data-ttu-id="c3f09-135">Ponieważ w tabeli koszyka śledzi ilości dla każdego albumu, zawiera logikę do utworzenia nowego wiersza, w razie potrzeby lub po prostu zwiększyć ilość, jeśli użytkownik ma już uporządkowane jedną kopię albumu.</span><span class="sxs-lookup"><span data-stu-id="c3f09-135">Since the Cart table tracks quantity for each album, it includes logic to create a new row if needed or just increment the quantity if the user has already ordered one copy of the album.</span></span>

<span data-ttu-id="c3f09-136">**RemoveFromCart** ma identyfikator albumu i usuwa go z koszyka użytkownika.</span><span class="sxs-lookup"><span data-stu-id="c3f09-136">**RemoveFromCart** takes an Album ID and removes it from the user's cart.</span></span> <span data-ttu-id="c3f09-137">Jeśli użytkownik ma tylko jedną kopię album na ich koszyka, wiersza zostanie usunięty.</span><span class="sxs-lookup"><span data-stu-id="c3f09-137">If the user only had one copy of the album in their cart, the row is removed.</span></span>

<span data-ttu-id="c3f09-138">**EmptyCart** usuwa wszystkie elementy z koszyka użytkownika.</span><span class="sxs-lookup"><span data-stu-id="c3f09-138">**EmptyCart** removes all items from a user's shopping cart.</span></span>

<span data-ttu-id="c3f09-139">**GetCartItems** pobiera listę CartItems wyświetlania lub przetwarzania.</span><span class="sxs-lookup"><span data-stu-id="c3f09-139">**GetCartItems** retrieves a list of CartItems for display or processing.</span></span>

<span data-ttu-id="c3f09-140">**GetCount** pobiera całkowitą liczbę użytkownik ma w ich koszyku albumy.</span><span class="sxs-lookup"><span data-stu-id="c3f09-140">**GetCount** retrieves a the total number of albums a user has in their shopping cart.</span></span>

<span data-ttu-id="c3f09-141">**GetTotal** oblicza całkowity koszt wszystkich elementów w koszyka.</span><span class="sxs-lookup"><span data-stu-id="c3f09-141">**GetTotal** calculates the total cost of all items in the cart.</span></span>

<span data-ttu-id="c3f09-142">**CreateOrder** konwertuje koszyka kolejności w fazie realizacji transakcji.</span><span class="sxs-lookup"><span data-stu-id="c3f09-142">**CreateOrder** converts the shopping cart to an order during the checkout phase.</span></span>

<span data-ttu-id="c3f09-143">**GetCart** jest metodą statyczną, która pozwala na uzyskanie obiektu koszyka naszych kontrolerów.</span><span class="sxs-lookup"><span data-stu-id="c3f09-143">**GetCart** is a static method which allows our controllers to obtain a cart object.</span></span> <span data-ttu-id="c3f09-144">Używa **GetCartId** metodę, aby obsłużyć odczyt CartId z sesji użytkownika.</span><span class="sxs-lookup"><span data-stu-id="c3f09-144">It uses the **GetCartId** method to handle reading the CartId from the user's session.</span></span> <span data-ttu-id="c3f09-145">Metoda GetCartId wymaga element HttpContextBase tak, aby go przeczytać CartId użytkownika z sesji użytkownika.</span><span class="sxs-lookup"><span data-stu-id="c3f09-145">The GetCartId method requires the HttpContextBase so that it can read the user's CartId from user's session.</span></span>

<span data-ttu-id="c3f09-146">Poniżej przedstawiono pełną **ShoppingCart klasy**:</span><span class="sxs-lookup"><span data-stu-id="c3f09-146">Here's the complete **ShoppingCart class**:</span></span>

[!code-csharp[Main](mvc-music-store-part-8/samples/sample5.cs)]

## <a name="viewmodels"></a><span data-ttu-id="c3f09-147">ViewModels</span><span class="sxs-lookup"><span data-stu-id="c3f09-147">ViewModels</span></span>

<span data-ttu-id="c3f09-148">Kontrolera koszyka zakupów należy do komunikowania się niektórych złożonych informacji do jego widoków, które nie prawidłowo mapowania na naszych obiekty modelu.</span><span class="sxs-lookup"><span data-stu-id="c3f09-148">Our Shopping Cart Controller will need to communicate some complex information to its views which doesn't map cleanly to our Model objects.</span></span> <span data-ttu-id="c3f09-149">Nie chcemy zmodyfikować naszych modeli do własnych naszych widoków; Klasy modeli powinno reprezentować naszych domeny, a nie w interfejsie użytkownika.</span><span class="sxs-lookup"><span data-stu-id="c3f09-149">We don't want to modify our Models to suit our views; Model classes should represent our domain, not the user interface.</span></span> <span data-ttu-id="c3f09-150">Jedno rozwiązanie byłoby przekazywania informacji do naszej widoków przy użyciu klasy obiekt ViewBag robiliśmy przy użyciu informacji z listy rozwijanej Menedżer magazynu, ale przekazywanie wiele informacji za pomocą elementów ViewBag pobiera trudne do zarządzania.</span><span class="sxs-lookup"><span data-stu-id="c3f09-150">One solution would be to pass the information to our Views using the ViewBag class, as we did with the Store Manager dropdown information, but passing a lot of information via ViewBag gets hard to manage.</span></span>

<span data-ttu-id="c3f09-151">To rozwiązanie jest użycie *ViewModel* wzorca.</span><span class="sxs-lookup"><span data-stu-id="c3f09-151">A solution to this is to use the *ViewModel* pattern.</span></span> <span data-ttu-id="c3f09-152">Korzystając z tego wzorca utworzymy jednoznacznie klas, które są zoptymalizowane pod kątem scenariuszami określony widok, a które udostępniają właściwości dla zawartości dynamicznej, wartości/wymagane przez naszym szablony widoku.</span><span class="sxs-lookup"><span data-stu-id="c3f09-152">When using this pattern we create strongly-typed classes that are optimized for our specific view scenarios, and which expose properties for the dynamic values/content needed by our view templates.</span></span> <span data-ttu-id="c3f09-153">Nasze klasy kontrolera można wypełnić i przekazać te klasy zoptymalizowanych pod kątem widoku do naszej Wyświetl szablon do użycia.</span><span class="sxs-lookup"><span data-stu-id="c3f09-153">Our controller classes can then populate and pass these view-optimized classes to our view template to use.</span></span> <span data-ttu-id="c3f09-154">Umożliwia to zabezpieczenie typów, sprawdzanie kompilacji i Edytor IntelliSense w widoku szablonów.</span><span class="sxs-lookup"><span data-stu-id="c3f09-154">This enables type-safety, compile-time checking, and editor IntelliSense within view templates.</span></span>

<span data-ttu-id="c3f09-155">Utwórz dwa modele widok do użycia w naszym kontrolera koszyk: ShoppingCartViewModel będzie przechowywać zawartość koszyk użytkownika i ShoppingCartRemoveViewModel będzie używany do wyświetlania informacji potwierdzenie, gdy użytkownik usuwa element z ich koszyka.</span><span class="sxs-lookup"><span data-stu-id="c3f09-155">We'll create two View Models for use in our Shopping Cart controller: the ShoppingCartViewModel will hold the contents of the user's shopping cart, and the ShoppingCartRemoveViewModel will be used to display confirmation information when a user removes something from their cart.</span></span>

<span data-ttu-id="c3f09-156">Teraz Utwórz nowy folder ViewModels w folderze głównym naszych projektu w celu zachowania rzeczy organizowane.</span><span class="sxs-lookup"><span data-stu-id="c3f09-156">Let's create a new ViewModels folder in the root of our project to keep things organized.</span></span> <span data-ttu-id="c3f09-157">Kliknij prawym przyciskiem myszy projekt, wybierz opcję Dodaj / nowego folderu.</span><span class="sxs-lookup"><span data-stu-id="c3f09-157">Right-click the project, select Add / New Folder.</span></span>

![](mvc-music-store-part-8/_static/image1.jpg)

<span data-ttu-id="c3f09-158">Nazwa folderu ViewModels.</span><span class="sxs-lookup"><span data-stu-id="c3f09-158">Name the folder ViewModels.</span></span>

![](mvc-music-store-part-8/_static/image1.png)

<span data-ttu-id="c3f09-159">Następnie Dodaj klasę ShoppingCartViewModel w folderze ViewModels.</span><span class="sxs-lookup"><span data-stu-id="c3f09-159">Next, add the ShoppingCartViewModel class in the ViewModels folder.</span></span> <span data-ttu-id="c3f09-160">Ma dwie właściwości: lista elementów z koszyka, a wartość dziesiętną do przechowywania całkowitej cen dla wszystkich elementów w koszyka.</span><span class="sxs-lookup"><span data-stu-id="c3f09-160">It has two properties: a list of Cart items, and a decimal value to hold the total price for all items in the cart.</span></span>

[!code-csharp[Main](mvc-music-store-part-8/samples/sample6.cs)]

<span data-ttu-id="c3f09-161">Teraz Dodaj ShoppingCartRemoveViewModel do folderu ViewModels z następujących czterech właściwości.</span><span class="sxs-lookup"><span data-stu-id="c3f09-161">Now add the ShoppingCartRemoveViewModel to the ViewModels folder, with the following four properties.</span></span>

[!code-csharp[Main](mvc-music-store-part-8/samples/sample7.cs)]

## <a name="the-shopping-cart-controller"></a><span data-ttu-id="c3f09-162">Kontroler koszyka zakupów</span><span class="sxs-lookup"><span data-stu-id="c3f09-162">The Shopping Cart Controller</span></span>

<span data-ttu-id="c3f09-163">Kontroler koszyku ma trzy główne cele: Dodawanie elementów do koszyka, usuwanie elementów z koszyka i wyświetlania elementów w koszyka.</span><span class="sxs-lookup"><span data-stu-id="c3f09-163">The Shopping Cart controller has three main purposes: adding items to a cart, removing items from the cart, and viewing items in the cart.</span></span> <span data-ttu-id="c3f09-164">Spowoduje to, że użycie trzech klas możemy właśnie utworzony: ShoppingCartViewModel, ShoppingCartRemoveViewModel i ShoppingCart.</span><span class="sxs-lookup"><span data-stu-id="c3f09-164">It will make use of the three classes we just created: ShoppingCartViewModel, ShoppingCartRemoveViewModel, and ShoppingCart.</span></span> <span data-ttu-id="c3f09-165">Jak StoreController i StoreManagerController dodamy pola do przechowywania wystąpienia MusicStoreEntities.</span><span class="sxs-lookup"><span data-stu-id="c3f09-165">As in the StoreController and StoreManagerController, we'll add a field to hold an instance of MusicStoreEntities.</span></span>

<span data-ttu-id="c3f09-166">Dodaj nowy kontroler koszyku do projektu przy użyciu szablonu pusty kontroler.</span><span class="sxs-lookup"><span data-stu-id="c3f09-166">Add a new Shopping Cart controller to the project using the Empty controller template.</span></span>

![](mvc-music-store-part-8/_static/image2.png)

<span data-ttu-id="c3f09-167">W tym miejscu jest pełną kontroler ShoppingCart.</span><span class="sxs-lookup"><span data-stu-id="c3f09-167">Here's the complete ShoppingCart Controller.</span></span> <span data-ttu-id="c3f09-168">Indeks i Dodaj kontroler akcji powinna wyglądać znajomo bardzo.</span><span class="sxs-lookup"><span data-stu-id="c3f09-168">The Index and Add Controller actions should look very familiar.</span></span> <span data-ttu-id="c3f09-169">Akcji kontrolera Usuń i CartSummary obsługi dwóch szczególnych przypadkach, które omówiono w następnej sekcji.</span><span class="sxs-lookup"><span data-stu-id="c3f09-169">The Remove and CartSummary controller actions handle two special cases, which we'll discuss in the following section.</span></span>

[!code-csharp[Main](mvc-music-store-part-8/samples/sample8.cs)]

## <a name="ajax-updates-with-jquery"></a><span data-ttu-id="c3f09-170">Aktualizacje interfejsu AJAX z jQuery</span><span class="sxs-lookup"><span data-stu-id="c3f09-170">Ajax Updates with jQuery</span></span>

<span data-ttu-id="c3f09-171">Następnie utworzymy strona indeksu koszyka zakupów, która jest silnie typizowaną do ShoppingCartViewModel i używa szablonu widoku listy przy użyciu tej samej metody co przed.</span><span class="sxs-lookup"><span data-stu-id="c3f09-171">We'll next create a Shopping Cart Index page that is strongly typed to the ShoppingCartViewModel and uses the List View template using the same method as before.</span></span>

![](mvc-music-store-part-8/_static/image3.png)

<span data-ttu-id="c3f09-172">Jednak zamiast Html.ActionLink usunięcie elementów z koszyka, użyjemy jQuery do "okablować się" zdarzenie click dla wszystkich łączy w tym widoku, których klasa HTML biznesowych — właścicieli.</span><span class="sxs-lookup"><span data-stu-id="c3f09-172">However, instead of using an Html.ActionLink to remove items from the cart, we'll use jQuery to "wire up" the click event for all links in this view which have the HTML class RemoveLink.</span></span> <span data-ttu-id="c3f09-173">Zamiast przesyłanie formularza, ten program obsługi zdarzeń kliknij właśnie wprowadzi wywołania zwrotnego AJAX do naszej RemoveFromCart akcji kontrolera.</span><span class="sxs-lookup"><span data-stu-id="c3f09-173">Rather than posting the form, this click event handler will just make an AJAX callback to our RemoveFromCart controller action.</span></span> <span data-ttu-id="c3f09-174">RemoveFromCart zwraca wynik Zserializowany do postaci JSON, który naszych wywołania zwrotnego jQuery następnie analizuje i wykonuje cztery szybkie aktualizacje do strony przy użyciu technologii jQuery:</span><span class="sxs-lookup"><span data-stu-id="c3f09-174">The RemoveFromCart returns a JSON serialized result, which our jQuery callback then parses and performs four quick updates to the page using jQuery:</span></span>

- 1. <span data-ttu-id="c3f09-175">Usuwa albumu usuniętych z listy</span><span class="sxs-lookup"><span data-stu-id="c3f09-175">Removes the deleted album from the list</span></span>
- 2. <span data-ttu-id="c3f09-176">Aktualizuje liczba koszyka w nagłówku</span><span class="sxs-lookup"><span data-stu-id="c3f09-176">Updates the cart count in the header</span></span>
- 3. <span data-ttu-id="c3f09-177">Wyświetla komunikat o aktualizacji dla użytkownika</span><span class="sxs-lookup"><span data-stu-id="c3f09-177">Displays an update message to the user</span></span>
- 4. <span data-ttu-id="c3f09-178">Aktualizuje cena razem koszyka</span><span class="sxs-lookup"><span data-stu-id="c3f09-178">Updates the cart total price</span></span>

<span data-ttu-id="c3f09-179">Ponieważ scenariusz Usuń jest obsługiwane przez wywołania zwrotnego Ajax w widoku indeksu, potrzebujemy nie dodatkowy widok RemoveFromCart akcji.</span><span class="sxs-lookup"><span data-stu-id="c3f09-179">Since the remove scenario is being handled by an Ajax callback within the Index view, we don't need an additional view for RemoveFromCart action.</span></span> <span data-ttu-id="c3f09-180">W tym miejscu jest kompletny kod dla widoku /ShoppingCart/Index:</span><span class="sxs-lookup"><span data-stu-id="c3f09-180">Here is the complete code for the /ShoppingCart/Index view:</span></span>

[!code-cshtml[Main](mvc-music-store-part-8/samples/sample9.cshtml)]

<span data-ttu-id="c3f09-181">Aby przetestować tę możliwość, musimy można dodać elementy do naszego koszyka sklepowego.</span><span class="sxs-lookup"><span data-stu-id="c3f09-181">In order to test this out, we need to be able to add items to our shopping cart.</span></span> <span data-ttu-id="c3f09-182">Będziemy informować naszych **szczegóły magazynu** widok ma zawierać przycisk "Dodaj do koszyka".</span><span class="sxs-lookup"><span data-stu-id="c3f09-182">We'll update our **Store Details** view to include an "Add to cart" button.</span></span> <span data-ttu-id="c3f09-183">Są nam go, firma Microsoft może zawierać części albumu dodatkowe informacje, które dodano od momentu ostatniej aktualizacji możemy ten widok: Genre, wykonawcy ceny i albumu.</span><span class="sxs-lookup"><span data-stu-id="c3f09-183">While we're at it, we can include some of the Album additional information which we've added since we last updated this view: Genre, Artist, Price, and Album Art.</span></span> <span data-ttu-id="c3f09-184">Zaktualizowany kod widoku Szczegóły magazynu pojawia się, jak pokazano poniżej.</span><span class="sxs-lookup"><span data-stu-id="c3f09-184">The updated Store Details view code appears as shown below.</span></span>

[!code-cshtml[Main](mvc-music-store-part-8/samples/sample10.cshtml)]

<span data-ttu-id="c3f09-185">Teraz możemy kliknij za pośrednictwem sklepu i przetestować Dodawanie i usuwanie albumów do i z naszego koszyka sklepowego.</span><span class="sxs-lookup"><span data-stu-id="c3f09-185">Now we can click through the store and test adding and removing Albums to and from our shopping cart.</span></span> <span data-ttu-id="c3f09-186">Uruchom aplikację, a następnie przejdź do indeksu magazynu.</span><span class="sxs-lookup"><span data-stu-id="c3f09-186">Run the application and browse to the Store Index.</span></span>

![](mvc-music-store-part-8/_static/image4.png)

<span data-ttu-id="c3f09-187">Następnie kliknij Genre, aby wyświetlić listę albumów.</span><span class="sxs-lookup"><span data-stu-id="c3f09-187">Next, click on a Genre to view a list of albums.</span></span>

![](mvc-music-store-part-8/_static/image5.png)

<span data-ttu-id="c3f09-188">Klikając tytuł teraz pokazuje naszych zaktualizowane widoku Szczegóły albumu, w tym przycisku "Dodaj do koszyka".</span><span class="sxs-lookup"><span data-stu-id="c3f09-188">Clicking on an Album title now shows our updated Album Details view, including the "Add to cart" button.</span></span>

![](mvc-music-store-part-8/_static/image6.png)

<span data-ttu-id="c3f09-189">Kliknięcie przycisku "Dodaj do koszyka" przedstawiono naszych indeksu koszyka zakupów z lista podsumowania koszyka zakupów.</span><span class="sxs-lookup"><span data-stu-id="c3f09-189">Clicking the "Add to cart" button shows our Shopping Cart Index view with the shopping cart summary list.</span></span>

![](mvc-music-store-part-8/_static/image7.png)

<span data-ttu-id="c3f09-190">Po załadowaniu zapasowych koszyku, możesz kliknąć Usuń z koszyka łącza w celu wyświetlenia aktualizacji interfejsu Ajax do koszyka.</span><span class="sxs-lookup"><span data-stu-id="c3f09-190">After loading up your shopping cart, you can click on the Remove from cart link to see the Ajax update to your shopping cart.</span></span>

![](mvc-music-store-part-8/_static/image8.png)

<span data-ttu-id="c3f09-191">Firma Microsoft powstanie limit działającego koszyka, dzięki czemu użytkownicy wyrejestrować dodać elementy do ich koszyka.</span><span class="sxs-lookup"><span data-stu-id="c3f09-191">We've built out a working shopping cart which allows unregistered users to add items to their cart.</span></span> <span data-ttu-id="c3f09-192">W poniższej sekcji firma Microsoft będzie zezwolić im na rejestrowanie i ukończyć proces wyewidencjonowania.</span><span class="sxs-lookup"><span data-stu-id="c3f09-192">In the following section, we'll allow them to register and complete the checkout process.</span></span>


> [!div class="step-by-step"]
> <span data-ttu-id="c3f09-193">[Poprzednie](mvc-music-store-part-7.md)
> [dalej](mvc-music-store-part-9.md)</span><span class="sxs-lookup"><span data-stu-id="c3f09-193">[Previous](mvc-music-store-part-7.md)
[Next](mvc-music-store-part-9.md)</span></span>
