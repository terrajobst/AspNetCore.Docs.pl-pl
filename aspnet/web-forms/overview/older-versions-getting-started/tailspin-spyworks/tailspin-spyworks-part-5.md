---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-5
title: 'Część 5: Logika biznesowa | Dokumentacja firmy Microsoft'
author: JoeStagner
description: Ta seria samouczek zawiera szczegóły dotyczące wszystkich kroków kompilacji Tailspin Spyworks przykładowej aplikacji. Część 5 dodaje niektórych logiki biznesowej.
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/21/2010
ms.topic: article
ms.assetid: eaef475a-ca91-47ea-a4a7-d074005ed80c
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-5
msc.type: authoredcontent
ms.openlocfilehash: e4342e634ef8c4bcf4e0085650a28f414ab23736
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
ms.locfileid: "30885084"
---
<a name="part-5-business-logic"></a><span data-ttu-id="f4042-104">Część 5: Logika biznesowa</span><span class="sxs-lookup"><span data-stu-id="f4042-104">Part 5: Business Logic</span></span>
====================
<span data-ttu-id="f4042-105">przez [Stagner Jan](https://github.com/JoeStagner)</span><span class="sxs-lookup"><span data-stu-id="f4042-105">by [Joe Stagner](https://github.com/JoeStagner)</span></span>

> <span data-ttu-id="f4042-106">Tailspin Spyworks pokazano, jak bardzo proste jest tworzenie zaawansowanych, skalowalnych aplikacji dla platformy .NET.</span><span class="sxs-lookup"><span data-stu-id="f4042-106">Tailspin Spyworks demonstrates how extraordinarily simple it is to create powerful, scalable applications for the .NET platform.</span></span> <span data-ttu-id="f4042-107">Przedstawia on poza jak nowe, fantastyczne funkcje programu ASP.NET 4 do tworzenia sklepu online, łącznie z zakupów, wyewidencjonowania i administracji.</span><span class="sxs-lookup"><span data-stu-id="f4042-107">It shows off how to use the great new features in ASP.NET 4 to build an online store, including shopping, checkout, and administration.</span></span>
> 
> <span data-ttu-id="f4042-108">Ta seria samouczek zawiera szczegóły dotyczące wszystkich kroków kompilacji Tailspin Spyworks przykładowej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="f4042-108">This tutorial series details all of the steps taken to build the Tailspin Spyworks sample application.</span></span> <span data-ttu-id="f4042-109">Część 5 dodaje niektórych logiki biznesowej.</span><span class="sxs-lookup"><span data-stu-id="f4042-109">Part 5 adds some business logic.</span></span>


## <a id="_Toc260221671"></a>  <span data-ttu-id="f4042-110">Dodanie niektórych logika biznesowa</span><span class="sxs-lookup"><span data-stu-id="f4042-110">Adding Some Business Logic</span></span>

<span data-ttu-id="f4042-111">Chcemy wiemy z doświadczenia zakupów mają być dostępne, gdy ktoś odwiedzi witryny sieci web.</span><span class="sxs-lookup"><span data-stu-id="f4042-111">We want our shopping experience to be available whenever someone visits our web site.</span></span> <span data-ttu-id="f4042-112">Osoby odwiedzające będą mogli przeglądania i Dodaj elementy do koszyka, nawet jeśli nie są zarejestrowane lub zalogowany.</span><span class="sxs-lookup"><span data-stu-id="f4042-112">Visitors will be able to browse and add items to the shopping cart even if they are not registered or logged in.</span></span> <span data-ttu-id="f4042-113">Gdy są one gotowe do wyewidencjonowania otrzymają oni możliwość uwierzytelnienia i jeśli nie są jeszcze elementy członkowskie będą oni mogli utworzyć konto.</span><span class="sxs-lookup"><span data-stu-id="f4042-113">When they are ready to check out they will be given the option to authenticate and if they are not yet members they will be able to create an account.</span></span>

<span data-ttu-id="f4042-114">Oznacza to, że musimy zaimplementować logiki można przekonwertować koszyka z anonimowego stanu na stan "Zarejestrowany użytkownik".</span><span class="sxs-lookup"><span data-stu-id="f4042-114">This means that we will need to implement the logic to convert the shopping cart from an anonymous state to a "Registered User" state.</span></span>

<span data-ttu-id="f4042-115">Teraz Utwórz katalog o nazwie "Klasy", a następnie kliknij prawym przyciskiem myszy w folderze i utworzyć nowe "Class" pliku o nazwie MyShoppingCart.cs</span><span class="sxs-lookup"><span data-stu-id="f4042-115">Let's create a directory named "Classes" then Right-Click on the folder and create a new "Class" file named MyShoppingCart.cs</span></span>

![](tailspin-spyworks-part-5/_static/image1.jpg)

![](tailspin-spyworks-part-5/_static/image1.png)

<span data-ttu-id="f4042-116">Jak wcześniej wspomniano, firma Microsoft będzie rozszerzenie klasy, która implementuje stronę MyShoppingCart.aspx i firma Microsoft będzie to zrobić przy użyciu. Konstrukcja zaawansowane "klasy częściowej" przez sieć.</span><span class="sxs-lookup"><span data-stu-id="f4042-116">As previously mentioned we will be extending the class that implements the MyShoppingCart.aspx page and we will do this using .NET's powerful "Partial Class" construct.</span></span>

<span data-ttu-id="f4042-117">Wywołanie wygenerowany dla naszych pliku MyShoppingCart.aspx.cf wygląda następująco.</span><span class="sxs-lookup"><span data-stu-id="f4042-117">The generated call for our MyShoppingCart.aspx.cf file looks like this.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample1.cs)]

<span data-ttu-id="f4042-118">Zwróć uwagę na użycie słowa kluczowego "częściowej".</span><span class="sxs-lookup"><span data-stu-id="f4042-118">Note the use of the "partial" keyword.</span></span>

<span data-ttu-id="f4042-119">Plik klasy, który mamy wygenerował wygląda następująco.</span><span class="sxs-lookup"><span data-stu-id="f4042-119">The class file that we just generated looks like this.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample2.cs)]

<span data-ttu-id="f4042-120">Firma Microsoft zostaną scalone naszych implementacje przez dodanie do tego pliku, a także partial — słowo kluczowe.</span><span class="sxs-lookup"><span data-stu-id="f4042-120">We will merge our implementations by adding the partial keyword to this file as well.</span></span>

<span data-ttu-id="f4042-121">Naszego nowego pliku klasy teraz wygląda następująco.</span><span class="sxs-lookup"><span data-stu-id="f4042-121">Our new class file now looks like this.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample3.cs)]

<span data-ttu-id="f4042-122">Pierwsza metoda, który zostanie dodany do naszej klasy jest metoda "AddItem".</span><span class="sxs-lookup"><span data-stu-id="f4042-122">The first method that we will add to our class is the "AddItem" method.</span></span> <span data-ttu-id="f4042-123">Jest to metoda, który ostatecznie zostanie wywołany, gdy użytkownik kliknie łącza "Dodaj do grafik" strony Lista produktów i szczegółowe informacje.</span><span class="sxs-lookup"><span data-stu-id="f4042-123">This is the method that will ultimately be called when the user clicks on the "Add to Art" links on the Product List and Product Details pages.</span></span>

<span data-ttu-id="f4042-124">Dołącz do przy użyciu następujących instrukcji w górnej części strony.</span><span class="sxs-lookup"><span data-stu-id="f4042-124">Append the following to the using statements at the top of the page.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample4.cs)]

<span data-ttu-id="f4042-125">I Dodaj tę metodę do klasy MyShoppingCart.</span><span class="sxs-lookup"><span data-stu-id="f4042-125">And add this method to the MyShoppingCart class.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample5.cs)]

<span data-ttu-id="f4042-126">Aby zobaczyć, czy element jest już koszyka korzystamy LINQ to Entities.</span><span class="sxs-lookup"><span data-stu-id="f4042-126">We are using LINQ to Entities to see if the item is already in the cart.</span></span> <span data-ttu-id="f4042-127">Jeśli tak, możemy zaktualizować ilość zamówienia elementu, w przeciwnym razie utworzymy nowy wpis dla wybranego elementu</span><span class="sxs-lookup"><span data-stu-id="f4042-127">If so, we update the order quantity of the item, otherwise we create a new entry for the selected item</span></span>

<span data-ttu-id="f4042-128">Aby można było wywołać tę metodę wprowadzimy strony AddToCart.aspx nie tylko klasy tej metody, ale następnie wyświetlane bieżące koszyka = po dodaniu elementu.</span><span class="sxs-lookup"><span data-stu-id="f4042-128">In order to call this method we will implement an AddToCart.aspx page that not only class this method but then displayed the current shopping a=cart after the item has been added.</span></span>

<span data-ttu-id="f4042-129">Kliknij prawym przyciskiem myszy nazwę rozwiązania w Eksploratorze rozwiązań i Dodaj i nową stronę o nazwie AddToCart.aspx, jak firma Microsoft wcześniej zrobione.</span><span class="sxs-lookup"><span data-stu-id="f4042-129">Right-Click on the solution name in the solution explorer and add and new page named AddToCart.aspx as we have done previously.</span></span>

<span data-ttu-id="f4042-130">Gdy ta strona może służy do wyświetlania wyników pośrednich niski problemów standardowych itd., w naszym implementacji, strony rzeczywiście renderowania, ale raczej wywoła logiki "Dodaj" i przekierowania.</span><span class="sxs-lookup"><span data-stu-id="f4042-130">While we could use this page to display interim results like low stock issues, etc, in our implementation, the page will not actually render, but rather call the "Add" logic and redirect.</span></span>

<span data-ttu-id="f4042-131">W tym celu dodamy poniższy kod do strony\_zdarzeń obciążenia.</span><span class="sxs-lookup"><span data-stu-id="f4042-131">To accomplish this we'll add the following code to the Page\_Load event.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample6.cs)]

<span data-ttu-id="f4042-132">Należy pamiętać, że trwa pobieranie produktu do dodania do koszyka parametr QueryString i wywołanie metody AddItem klasy Nasze.</span><span class="sxs-lookup"><span data-stu-id="f4042-132">Note that we are retrieving the product to add to the shopping cart from a QueryString parameter and calling the AddItem method of our class.</span></span>

<span data-ttu-id="f4042-133">Zakładając, że żadne błędy nie zostaną napotkane kontrola jest przekazywana do strony SHoppingCart.aspx, która pełni wprowadzimy w polu.</span><span class="sxs-lookup"><span data-stu-id="f4042-133">Assuming no errors are encountered control is passed to the SHoppingCart.aspx page which we will fully implement next.</span></span> <span data-ttu-id="f4042-134">Jeśli ma to być błąd możemy Zgłoś wyjątek.</span><span class="sxs-lookup"><span data-stu-id="f4042-134">If there should be an error we throw an exception.</span></span>

<span data-ttu-id="f4042-135">Obecnie firma Microsoft ma nie zaimplementowano jeszcze obsługi błędów ogólnych tego wyjątku przejdzie nieobsługiwany przez naszą aplikację, ale firma Microsoft będzie wkrótce to rozwiązać.</span><span class="sxs-lookup"><span data-stu-id="f4042-135">Currently we have not yet implemented a global error handler so this exception would go unhandled by our application but we will remedy this shortly.</span></span>

<span data-ttu-id="f4042-136">Należy też zauważyć, użyj instrukcji Debug.Fail() (dostępne za pośrednictwem `using System.Diagnostics;)`</span><span class="sxs-lookup"><span data-stu-id="f4042-136">Note also the use of the statement Debug.Fail() (available via `using System.Diagnostics;)`</span></span>

<span data-ttu-id="f4042-137">Jest aplikacja jest uruchomiona w debugerze, ta metoda wyświetli okno dialogowe szczegółowe informacje o stanie aplikacji oraz komunikat o błędzie, który jest określona.</span><span class="sxs-lookup"><span data-stu-id="f4042-137">Is the application is running inside the debugger, this method will display a detailed dialog with information about the applications state along with the error message that we specify.</span></span>

<span data-ttu-id="f4042-138">Podczas pracy w środowisku produkcyjnym instrukcji Debug.Fail() jest ignorowana.</span><span class="sxs-lookup"><span data-stu-id="f4042-138">When running in production the Debug.Fail() statement is ignored.</span></span>

<span data-ttu-id="f4042-139">Można zauważyć w kodzie powyżej wywołanie do metody w naszym zakupów nazw klas koszyka "GetShoppingCartId".</span><span class="sxs-lookup"><span data-stu-id="f4042-139">You will note in the code above a call to a method in our shopping cart class names "GetShoppingCartId".</span></span>

<span data-ttu-id="f4042-140">Dodaj kod, aby zaimplementować metodę w następujący sposób.</span><span class="sxs-lookup"><span data-stu-id="f4042-140">Add the code to implement the method as follows.</span></span>

<span data-ttu-id="f4042-141">Należy pamiętać, że również dodaliśmy przyciski aktualizacji i wyewidencjonowania i etykiety, której można wyświetlić koszyka "Suma".</span><span class="sxs-lookup"><span data-stu-id="f4042-141">Note that we've also added update and checkout buttons and a label where we can display the cart "total".</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample7.cs)]

<span data-ttu-id="f4042-142">Można teraz dodać elementy do naszego koszyka, ale nie wdrożonych logikę do wyświetlenia koszyka, po dodaniu produktu.</span><span class="sxs-lookup"><span data-stu-id="f4042-142">We can now add items to our shopping cart but we have not implemented the logic to display the cart after a product has been added.</span></span>

<span data-ttu-id="f4042-143">Tak na stronie MyShoppingCart.aspx dodamy formantem obiektu EntityDataSource i GridVire w następujący sposób.</span><span class="sxs-lookup"><span data-stu-id="f4042-143">So, in the MyShoppingCart.aspx page we'll add an EntityDataSource control and a GridVire control as follows.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-5/samples/sample8.aspx)]

<span data-ttu-id="f4042-144">Wywołanie formularza w projektancie, dzięki czemu można dwukrotnie kliknij przycisk Aktualizuj koszyk i generowanie obsługi zdarzeń kliknięcia, który określono w deklaracji w znaczniku.</span><span class="sxs-lookup"><span data-stu-id="f4042-144">Call up the form in the designer so that you can double click on the Update Cart button and generate the click event handler that is specified in the declaration in the markup.</span></span>

<span data-ttu-id="f4042-145">Firma Microsoft będzie później zaimplementować szczegóły, ale w ten sposób zostanie Daj nam kompilowanie i uruchamianie aplikacji bez błędów.</span><span class="sxs-lookup"><span data-stu-id="f4042-145">We'll implement the details later but doing this will let us build and run our application without errors.</span></span>

<span data-ttu-id="f4042-146">Po uruchomieniu aplikacji i Dodaj element do koszyka zobaczysz to.</span><span class="sxs-lookup"><span data-stu-id="f4042-146">When you run the application and add an item to the shopping cart you will see this.</span></span>

![](tailspin-spyworks-part-5/_static/image2.jpg)

<span data-ttu-id="f4042-147">Należy pamiętać, że firma Microsoft odpowiadają regułom z widoku siatki "domyślne" zaimplementowanie trzy kolumny niestandardowe.</span><span class="sxs-lookup"><span data-stu-id="f4042-147">Note that we have deviated from the "default" grid display by implementing three custom columns.</span></span>

<span data-ttu-id="f4042-148">Pierwsza to edytowalna, pole "Powiązane" ilość:</span><span class="sxs-lookup"><span data-stu-id="f4042-148">The first is an Editable, "Bound" field for the Quantity:</span></span>

[!code-aspx[Main](tailspin-spyworks-part-5/samples/sample9.aspx)]

<span data-ttu-id="f4042-149">Następne jest kolumnę "obliczeniową", która wyświetla całkowitą element wiersza (element koszt razy może zostać określona ilość):</span><span class="sxs-lookup"><span data-stu-id="f4042-149">The next is a "calculated" column that displays the line item total (the item cost times the quantity to be ordered):</span></span>

[!code-aspx[Main](tailspin-spyworks-part-5/samples/sample10.aspx)]

<span data-ttu-id="f4042-150">Na koniec mamy niestandardowych kolumny, która zawiera formant wyboru, który użytkownik zostanie służy do wskazywania, czy można usunąć elementu z planu zakupów.</span><span class="sxs-lookup"><span data-stu-id="f4042-150">Lastly we have a custom column that contains a CheckBox control that the user will use to indicate that the item should be removed from the shopping chart.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-5/samples/sample11.aspx)]

![](tailspin-spyworks-part-5/_static/image3.jpg)

<span data-ttu-id="f4042-151">Jak widać, zamówienia, więc warto całkowita wiersz jest pusty Dodaj logikę można obliczyć całkowitą kolejności.</span><span class="sxs-lookup"><span data-stu-id="f4042-151">As you can see, the Order Total line is empty so let's add some logic to calculate the Order Total.</span></span>

<span data-ttu-id="f4042-152">Firma Microsoft będzie najpierw implementacji metody "GetTotal" do naszej klasy MyShoppingCart.</span><span class="sxs-lookup"><span data-stu-id="f4042-152">We'll first implement a "GetTotal" method to our MyShoppingCart Class.</span></span>

<span data-ttu-id="f4042-153">W pliku MyShoppingCart.cs Dodaj poniższy kod.</span><span class="sxs-lookup"><span data-stu-id="f4042-153">In the MyShoppingCart.cs file add the following code.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample12.cs)]

<span data-ttu-id="f4042-154">Następnie na stronie\_można Zadzwonimy naszych GetTotal — metoda obsługi zdarzeń obciążenia.</span><span class="sxs-lookup"><span data-stu-id="f4042-154">Then in the Page\_Load event handler we'll can call our GetTotal method.</span></span> <span data-ttu-id="f4042-155">W tym samym czasie dodamy test, aby sprawdzić, czy koszyk jest pusty i odpowiednio wyświetlania, jeśli jest.</span><span class="sxs-lookup"><span data-stu-id="f4042-155">At the same time we'll add a test to see if the shopping cart is empty and adjust the display accordingly if it is.</span></span>

<span data-ttu-id="f4042-156">Obecnie Jeśli koszyk jest pusty uzyskujemy to:</span><span class="sxs-lookup"><span data-stu-id="f4042-156">Now if the shopping cart is empty we get this:</span></span>

![](tailspin-spyworks-part-5/_static/image4.jpg)

<span data-ttu-id="f4042-157">A jeśli nie, widzimy naszych razem.</span><span class="sxs-lookup"><span data-stu-id="f4042-157">And if not, we see our total.</span></span>

![](tailspin-spyworks-part-5/_static/image5.jpg)

<span data-ttu-id="f4042-158">Jednak ta strona nie jest jeszcze ukończone.</span><span class="sxs-lookup"><span data-stu-id="f4042-158">However, this page is not yet complete.</span></span>

<span data-ttu-id="f4042-159">Potrzebujemy dodatkową logikę ponownego obliczenia koszyka przez usunięcie elementów, które zostały oznaczone do usunięcia i określając nowe wartości ilości jako część mogły zostać zmienione w siatce przez użytkownika.</span><span class="sxs-lookup"><span data-stu-id="f4042-159">We will need additional logic to recalculate the shopping cart by removing items marked for removal and by determining new quantity values as some may have been changed in the grid by the user.</span></span>

<span data-ttu-id="f4042-160">Umożliwia dodawanie metody "RemoveItem" do klasy Nasze koszyka zakupów w MyShoppingCart.cs do obsługi w przypadku, gdy użytkownik oznacza element do usunięcia.</span><span class="sxs-lookup"><span data-stu-id="f4042-160">Lets add a "RemoveItem" method to our shopping cart class in MyShoppingCart.cs to handle the case when a user marks an item for removal.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample13.cs)]

<span data-ttu-id="f4042-161">Teraz załóżmy ad metody obsługi sytuacji, gdy użytkownik po prostu zmienia jakości może zostać określona w widoku GridView.</span><span class="sxs-lookup"><span data-stu-id="f4042-161">Now let's ad a method to handle the circumstance when a user simply changes the quality to be ordered in the GridView.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample14.cs)]

<span data-ttu-id="f4042-162">Z podstawowymi funkcjami aktualizacja i usuwanie w miejscu możemy wdrożyć logikę, która faktycznie aktualizuje koszyk w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="f4042-162">With the basic Remove and Update features in place we can implement the logic that actually updates the shopping cart in the database.</span></span> <span data-ttu-id="f4042-163">(In MyShoppingCart.cs)</span><span class="sxs-lookup"><span data-stu-id="f4042-163">(In MyShoppingCart.cs)</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample15.cs)]

<span data-ttu-id="f4042-164">Będzie należy pamiętać, że ta metoda oczekuje dwóch parametrów.</span><span class="sxs-lookup"><span data-stu-id="f4042-164">You'll note that this method expects two parameters.</span></span> <span data-ttu-id="f4042-165">Jeden element jest koszyk Id, a drugi jest Tablica obiektów typu zdefiniowanego przez użytkownika.</span><span class="sxs-lookup"><span data-stu-id="f4042-165">One is the shopping cart Id and the other is an array of objects of user defined type.</span></span>

<span data-ttu-id="f4042-166">Aby zminimalizować zależności logiki szczegółowych interfejsu użytkownika, firma Microsoft zdefiniowaniu struktury danych, które firma Microsoft może używać do przekazywania elementy koszyka zakupów do naszego kodu bez naszych metody konieczności bezpośredni dostęp do kontrolki widoku siatki.</span><span class="sxs-lookup"><span data-stu-id="f4042-166">So as to minimize the dependency of our logic on user interface specifics, we've defined a data structure that we can use to pass the shopping cart items to our code without our method needing to directly access the GridView control.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample16.cs)]

<span data-ttu-id="f4042-167">W naszym pliku MyShoppingCart.aspx.cs możemy użyć tej struktury w naszym obsługi zdarzeń kliknij przycisk aktualizacji w następujący sposób.</span><span class="sxs-lookup"><span data-stu-id="f4042-167">In our MyShoppingCart.aspx.cs file we can use this structure in our Update Button Click Event handler as follows.</span></span> <span data-ttu-id="f4042-168">Należy pamiętać, że oprócz aktualizowanie koszyka przeprowadzimy aktualizację do całości koszyka.</span><span class="sxs-lookup"><span data-stu-id="f4042-168">Note that in addition to updating the cart we will update the cart total as well.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample17.cs)]

<span data-ttu-id="f4042-169">Należy pamiętać o szczególne znaczenie ten wiersz kodu:</span><span class="sxs-lookup"><span data-stu-id="f4042-169">Note with particular interest this line of code:</span></span>

[!code-javascript[Main](tailspin-spyworks-part-5/samples/sample18.js)]

<span data-ttu-id="f4042-170">GetValues() jest funkcja pomocnika specjalne wprowadzimy w MyShoppingCart.aspx.cs w następujący sposób.</span><span class="sxs-lookup"><span data-stu-id="f4042-170">GetValues() is a special helper function that we will implement in MyShoppingCart.aspx.cs as follows.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample19.cs)]

<span data-ttu-id="f4042-171">To zapewnia czystą sposób uzyskać dostęp do wartości elementów powiązania w naszym kontrolki widoku siatki.</span><span class="sxs-lookup"><span data-stu-id="f4042-171">This provides a clean way to access the values of the bound elements in our GridView control.</span></span> <span data-ttu-id="f4042-172">Ponieważ naszych formant wyboru "Usuń element" nie jest powiązany firma Microsoft będzie do niego dostęp za pomocą metody FindControl().</span><span class="sxs-lookup"><span data-stu-id="f4042-172">Since our "Remove Item" CheckBox Control is not bound we'll access it via the FindControl() method.</span></span>

<span data-ttu-id="f4042-173">Na tym etapie tworzenia projektu przygotowujemy się do realizacji procesu realizacji transakcji.</span><span class="sxs-lookup"><span data-stu-id="f4042-173">At this stage in your project's development we are getting ready to implement the checkout process.</span></span>

<span data-ttu-id="f4042-174">Przed dokonaniem teraz użyć programu Visual Studio do generowania bazy danych członkostwa i dodać użytkownika do repozytorium członkostwa.</span><span class="sxs-lookup"><span data-stu-id="f4042-174">Before doing so let's use Visual Studio to generate the membership database and add a user to the membership repository.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="f4042-175">[Poprzednie](tailspin-spyworks-part-4.md)
> [dalej](tailspin-spyworks-part-6.md)</span><span class="sxs-lookup"><span data-stu-id="f4042-175">[Previous](tailspin-spyworks-part-4.md)
[Next](tailspin-spyworks-part-6.md)</span></span>
