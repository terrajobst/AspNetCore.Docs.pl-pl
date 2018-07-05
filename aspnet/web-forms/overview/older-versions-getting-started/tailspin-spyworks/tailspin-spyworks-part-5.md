---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-5
title: 'Część 5: Logika biznesowa | Dokumentacja firmy Microsoft'
author: JoeStagner
description: W tej serii samouczków zawiera szczegóły wszystkich kroków kompilacji Przykładowa aplikacja Tailspin Spyworks. Część 5 dodaje niektóre logiki biznesowej.
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/21/2010
ms.topic: article
ms.assetid: eaef475a-ca91-47ea-a4a7-d074005ed80c
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-5
msc.type: authoredcontent
ms.openlocfilehash: 719d56e0764e2f66b8813c9487119bbc700d738c
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37377989"
---
<a name="part-5-business-logic"></a><span data-ttu-id="0cfa2-104">Część 5: Logika biznesowa</span><span class="sxs-lookup"><span data-stu-id="0cfa2-104">Part 5: Business Logic</span></span>
====================
<span data-ttu-id="0cfa2-105">przez [Stagner Jan](https://github.com/JoeStagner)</span><span class="sxs-lookup"><span data-stu-id="0cfa2-105">by [Joe Stagner](https://github.com/JoeStagner)</span></span>

> <span data-ttu-id="0cfa2-106">Tailspin Spyworks pokazuje, jak bardzo łatwo jest tworzyć zaawansowane, skalowalne aplikacje dla platformy .NET.</span><span class="sxs-lookup"><span data-stu-id="0cfa2-106">Tailspin Spyworks demonstrates how extraordinarily simple it is to create powerful, scalable applications for the .NET platform.</span></span> <span data-ttu-id="0cfa2-107">Przedstawia on poza sposób użycia wspaniałych nowych funkcjach w ASP.NET 4 do tworzenia sklep online, m.in. zakupy wyewidencjonowanie i Administracja.</span><span class="sxs-lookup"><span data-stu-id="0cfa2-107">It shows off how to use the great new features in ASP.NET 4 to build an online store, including shopping, checkout, and administration.</span></span>
> 
> <span data-ttu-id="0cfa2-108">W tej serii samouczków zawiera szczegóły wszystkich kroków kompilacji Przykładowa aplikacja Tailspin Spyworks.</span><span class="sxs-lookup"><span data-stu-id="0cfa2-108">This tutorial series details all of the steps taken to build the Tailspin Spyworks sample application.</span></span> <span data-ttu-id="0cfa2-109">Część 5 dodaje niektóre logiki biznesowej.</span><span class="sxs-lookup"><span data-stu-id="0cfa2-109">Part 5 adds some business logic.</span></span>


## <a id="_Toc260221671"></a>  <span data-ttu-id="0cfa2-110">Dodawanie logiki biznesowej</span><span class="sxs-lookup"><span data-stu-id="0cfa2-110">Adding Some Business Logic</span></span>

<span data-ttu-id="0cfa2-111">Chcemy, aby nasze środowisko zakupów były dostępne w każdym przypadku, gdy ktoś odwiedza witryny sieci web.</span><span class="sxs-lookup"><span data-stu-id="0cfa2-111">We want our shopping experience to be available whenever someone visits our web site.</span></span> <span data-ttu-id="0cfa2-112">Osoby odwiedzające będzie przeglądanie i dodawanie elementów do koszyka, nawet jeśli nie są zarejestrowane lub zalogowany.</span><span class="sxs-lookup"><span data-stu-id="0cfa2-112">Visitors will be able to browse and add items to the shopping cart even if they are not registered or logged in.</span></span> <span data-ttu-id="0cfa2-113">Gdy są one gotowe do wyewidencjonowania otrzymają oni możliwość uwierzytelniania i jeśli nie są jeszcze Członkowie będą mogli utworzyć konto.</span><span class="sxs-lookup"><span data-stu-id="0cfa2-113">When they are ready to check out they will be given the option to authenticate and if they are not yet members they will be able to create an account.</span></span>

<span data-ttu-id="0cfa2-114">Oznacza to, że firma Microsoft będzie należy zaimplementować logikę do przekonwertowania koszyk sklepowy z anonimowych stanu do stanu "Zarejestrowany użytkownik".</span><span class="sxs-lookup"><span data-stu-id="0cfa2-114">This means that we will need to implement the logic to convert the shopping cart from an anonymous state to a "Registered User" state.</span></span>

<span data-ttu-id="0cfa2-115">Teraz Utwórz katalog o nazwie "Klasy", a następnie kliknij prawym przyciskiem myszy w folderze i utworzyć nowy plik "Class" o nazwie MyShoppingCart.cs</span><span class="sxs-lookup"><span data-stu-id="0cfa2-115">Let's create a directory named "Classes" then Right-Click on the folder and create a new "Class" file named MyShoppingCart.cs</span></span>

![](tailspin-spyworks-part-5/_static/image1.jpg)

![](tailspin-spyworks-part-5/_static/image1.png)

<span data-ttu-id="0cfa2-116">Jak wcześniej wspomniano, będziemy rozszerzać klasy, która implementuje stronę MyShoppingCart.aspx i zrobimy to za pomocą. Konstrukcja zaawansowane "klasy częściowej" NET firmy.</span><span class="sxs-lookup"><span data-stu-id="0cfa2-116">As previously mentioned we will be extending the class that implements the MyShoppingCart.aspx page and we will do this using .NET's powerful "Partial Class" construct.</span></span>

<span data-ttu-id="0cfa2-117">Wygenerowany wywołania dla naszego pliku MyShoppingCart.aspx.cf wygląda następująco.</span><span class="sxs-lookup"><span data-stu-id="0cfa2-117">The generated call for our MyShoppingCart.aspx.cf file looks like this.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample1.cs)]

<span data-ttu-id="0cfa2-118">Zwróć uwagę na użycie "partial" — słowo kluczowe.</span><span class="sxs-lookup"><span data-stu-id="0cfa2-118">Note the use of the "partial" keyword.</span></span>

<span data-ttu-id="0cfa2-119">Plik klasy, które właśnie wygenerowaną wygląda następująco.</span><span class="sxs-lookup"><span data-stu-id="0cfa2-119">The class file that we just generated looks like this.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample2.cs)]

<span data-ttu-id="0cfa2-120">Firma Microsoft spowoduje scalenie naszej implementacji, dodając partial-słowo kluczowe do tego pliku.</span><span class="sxs-lookup"><span data-stu-id="0cfa2-120">We will merge our implementations by adding the partial keyword to this file as well.</span></span>

<span data-ttu-id="0cfa2-121">Nasz nowy plik klasy wygląda teraz następująco.</span><span class="sxs-lookup"><span data-stu-id="0cfa2-121">Our new class file now looks like this.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample3.cs)]

<span data-ttu-id="0cfa2-122">Pierwsza metoda, która zostanie dodany do naszych klasy jest metodą "AddItem".</span><span class="sxs-lookup"><span data-stu-id="0cfa2-122">The first method that we will add to our class is the "AddItem" method.</span></span> <span data-ttu-id="0cfa2-123">Jest to metoda, który ostatecznie zostanie wywołana, gdy użytkownik kliknie w łączach "Dodaj do grafikę" na stronach listy produktów oraz szczegóły produktów.</span><span class="sxs-lookup"><span data-stu-id="0cfa2-123">This is the method that will ultimately be called when the user clicks on the "Add to Art" links on the Product List and Product Details pages.</span></span>

<span data-ttu-id="0cfa2-124">Dołącz następujący ciąg do przy użyciu instrukcji w górnej części strony.</span><span class="sxs-lookup"><span data-stu-id="0cfa2-124">Append the following to the using statements at the top of the page.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample4.cs)]

<span data-ttu-id="0cfa2-125">I Dodaj tę metodę do klasy MyShoppingCart.</span><span class="sxs-lookup"><span data-stu-id="0cfa2-125">And add this method to the MyShoppingCart class.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample5.cs)]

<span data-ttu-id="0cfa2-126">Używamy składnik LINQ to Entities, aby zobaczyć, czy element jest już w koszyku.</span><span class="sxs-lookup"><span data-stu-id="0cfa2-126">We are using LINQ to Entities to see if the item is already in the cart.</span></span> <span data-ttu-id="0cfa2-127">Jeśli tak, możemy zaktualizować ilość zamówienia elementu, w przeciwnym razie firma Microsoft tworzy nowy wpis dla wybranego elementu</span><span class="sxs-lookup"><span data-stu-id="0cfa2-127">If so, we update the order quantity of the item, otherwise we create a new entry for the selected item</span></span>

<span data-ttu-id="0cfa2-128">Aby wywołać tę metodę wdrażamy strony AddToCart.aspx, który nie tylko metody tej klasy, ale następnie wyświetlone bieżące koszyk = po dodaniu elementu.</span><span class="sxs-lookup"><span data-stu-id="0cfa2-128">In order to call this method we will implement an AddToCart.aspx page that not only class this method but then displayed the current shopping a=cart after the item has been added.</span></span>

<span data-ttu-id="0cfa2-129">Kliknij prawym przyciskiem myszy nazwę rozwiązania w Eksploratorze rozwiązań i Dodaj i nową stronę o nazwie AddToCart.aspx, jak firma Microsoft wcześniej robione.</span><span class="sxs-lookup"><span data-stu-id="0cfa2-129">Right-Click on the solution name in the solution explorer and add and new page named AddToCart.aspx as we have done previously.</span></span>

<span data-ttu-id="0cfa2-130">Gdy firma Microsoft może używać tej strony do wyświetlenia wyników pośrednich, takich jak niska podstawowych problemów itp., w naszej implementacji, strony bez faktycznego renderowania, ale raczej wywoła logiki "Add" i przekierowania.</span><span class="sxs-lookup"><span data-stu-id="0cfa2-130">While we could use this page to display interim results like low stock issues, etc, in our implementation, the page will not actually render, but rather call the "Add" logic and redirect.</span></span>

<span data-ttu-id="0cfa2-131">W tym celu dodamy poniższy kod do strony\_załadowane zdarzenie.</span><span class="sxs-lookup"><span data-stu-id="0cfa2-131">To accomplish this we'll add the following code to the Page\_Load event.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample6.cs)]

<span data-ttu-id="0cfa2-132">Należy pamiętać, że trwa pobieranie produktu do dodania do koszyka parametr QueryString i wywołanie metody AddItem klasy Nasze.</span><span class="sxs-lookup"><span data-stu-id="0cfa2-132">Note that we are retrieving the product to add to the shopping cart from a QueryString parameter and calling the AddItem method of our class.</span></span>

<span data-ttu-id="0cfa2-133">Zakładając, że żadne błędy nie zostaną napotkane kontrola jest przekazywana do strony SHoppingCart.aspx, który będzie w pełni wdrażamy obok.</span><span class="sxs-lookup"><span data-stu-id="0cfa2-133">Assuming no errors are encountered control is passed to the SHoppingCart.aspx page which we will fully implement next.</span></span> <span data-ttu-id="0cfa2-134">Jeśli ma to być błąd możemy zgłosić wyjątek.</span><span class="sxs-lookup"><span data-stu-id="0cfa2-134">If there should be an error we throw an exception.</span></span>

<span data-ttu-id="0cfa2-135">Obecnie mamy jeszcze nie zaimplementowano globalna procedura obsługi błędów, więc będzie nieobsłużony wyjątek przez naszą aplikację, ale firma Microsoft będzie rozwiązać ten problem wkrótce.</span><span class="sxs-lookup"><span data-stu-id="0cfa2-135">Currently we have not yet implemented a global error handler so this exception would go unhandled by our application but we will remedy this shortly.</span></span>

<span data-ttu-id="0cfa2-136">Ponadto Zwróć uwagę na użycie instrukcji Debug.Fail() (dostępne za pośrednictwem `using System.Diagnostics;)`</span><span class="sxs-lookup"><span data-stu-id="0cfa2-136">Note also the use of the statement Debug.Fail() (available via `using System.Diagnostics;)`</span></span>

<span data-ttu-id="0cfa2-137">To aplikacja jest uruchomiona w debugerze, ta metoda spowoduje wyświetlenie okna dialogowego szczegółowe informacje na temat stanu aplikacji oraz komunikat o błędzie, który określamy.</span><span class="sxs-lookup"><span data-stu-id="0cfa2-137">Is the application is running inside the debugger, this method will display a detailed dialog with information about the applications state along with the error message that we specify.</span></span>

<span data-ttu-id="0cfa2-138">Podczas uruchamiania w środowisku produkcyjnym instrukcji Debug.Fail() jest ignorowany.</span><span class="sxs-lookup"><span data-stu-id="0cfa2-138">When running in production the Debug.Fail() statement is ignored.</span></span>

<span data-ttu-id="0cfa2-139">Można zauważyć w kodzie powyżej wywołanie metody w naszym zakupów nazw klas koszyka "GetShoppingCartId".</span><span class="sxs-lookup"><span data-stu-id="0cfa2-139">You will note in the code above a call to a method in our shopping cart class names "GetShoppingCartId".</span></span>

<span data-ttu-id="0cfa2-140">Dodaj kod, aby wdrożyć metodę w następujący sposób.</span><span class="sxs-lookup"><span data-stu-id="0cfa2-140">Add the code to implement the method as follows.</span></span>

<span data-ttu-id="0cfa2-141">Należy pamiętać, że dodaliśmy również aktualizacji i wyewidencjonowywania przycisków i etykiet, gdzie możemy wyświetlić koszyk "łącznie".</span><span class="sxs-lookup"><span data-stu-id="0cfa2-141">Note that we've also added update and checkout buttons and a label where we can display the cart "total".</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample7.cs)]

<span data-ttu-id="0cfa2-142">Teraz możemy dodać elementy do naszego koszyka, ale nie zaimplementowano rozwiązania tlp logikę do wyświetlenia w koszyku po dodaniu produktu.</span><span class="sxs-lookup"><span data-stu-id="0cfa2-142">We can now add items to our shopping cart but we have not implemented the logic to display the cart after a product has been added.</span></span>

<span data-ttu-id="0cfa2-143">Tak na stronie MyShoppingCart.aspx dodamy kontrolkę EntityDataSource i kontroli GridVire w następujący sposób.</span><span class="sxs-lookup"><span data-stu-id="0cfa2-143">So, in the MyShoppingCart.aspx page we'll add an EntityDataSource control and a GridVire control as follows.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-5/samples/sample8.aspx)]

<span data-ttu-id="0cfa2-144">Wywołaj formularza w projektancie, dzięki czemu można dwukrotnie kliknij przycisk Aktualizuj koszyk i wygenerować program obsługi zdarzeń kliknięcie, który jest określony w deklaracji w znaczniku.</span><span class="sxs-lookup"><span data-stu-id="0cfa2-144">Call up the form in the designer so that you can double click on the Update Cart button and generate the click event handler that is specified in the declaration in the markup.</span></span>

<span data-ttu-id="0cfa2-145">Szczegółowe informacje będą później wdrażamy, ale w ten sposób będzie nam skompilować i uruchomić naszą aplikację bez błędów.</span><span class="sxs-lookup"><span data-stu-id="0cfa2-145">We'll implement the details later but doing this will let us build and run our application without errors.</span></span>

<span data-ttu-id="0cfa2-146">Po uruchomieniu aplikacji i Dodaj element do koszyka zobaczysz następujący.</span><span class="sxs-lookup"><span data-stu-id="0cfa2-146">When you run the application and add an item to the shopping cart you will see this.</span></span>

![](tailspin-spyworks-part-5/_static/image2.jpg)

<span data-ttu-id="0cfa2-147">Należy pamiętać, że firma Microsoft odpowiadają regułom z widoku siatki "domyślna" poprzez implementację trzy kolumny niestandardowej.</span><span class="sxs-lookup"><span data-stu-id="0cfa2-147">Note that we have deviated from the "default" grid display by implementing three custom columns.</span></span>

<span data-ttu-id="0cfa2-148">Pierwszy jest edytowalna, pole "Związane" ilość:</span><span class="sxs-lookup"><span data-stu-id="0cfa2-148">The first is an Editable, "Bound" field for the Quantity:</span></span>

[!code-aspx[Main](tailspin-spyworks-part-5/samples/sample9.aspx)]

<span data-ttu-id="0cfa2-149">Następne jest kolumnę "obliczeniową", która powoduje wyświetlenie elementu wiersz całkowitej (element kosztów razy ilość do zamówienia):</span><span class="sxs-lookup"><span data-stu-id="0cfa2-149">The next is a "calculated" column that displays the line item total (the item cost times the quantity to be ordered):</span></span>

[!code-aspx[Main](tailspin-spyworks-part-5/samples/sample10.aspx)]

<span data-ttu-id="0cfa2-150">Na koniec mamy kolumnę niestandardową, która zawiera formant pola wyboru, który użytkownik będzie używany do wskazania, że element powinny zostać usunięte z wykresu zakupów.</span><span class="sxs-lookup"><span data-stu-id="0cfa2-150">Lastly we have a custom column that contains a CheckBox control that the user will use to indicate that the item should be removed from the shopping chart.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-5/samples/sample11.aspx)]

![](tailspin-spyworks-part-5/_static/image3.jpg)

<span data-ttu-id="0cfa2-151">Jak widać, zamówienia, więc wiersz podsumowania jest pusty dodać logikę do obliczania całkowitego zamówienia.</span><span class="sxs-lookup"><span data-stu-id="0cfa2-151">As you can see, the Order Total line is empty so let's add some logic to calculate the Order Total.</span></span>

<span data-ttu-id="0cfa2-152">Firma Microsoft będzie najpierw zaimplementować metodę "GetTotal" do naszych MyShoppingCart klasy.</span><span class="sxs-lookup"><span data-stu-id="0cfa2-152">We'll first implement a "GetTotal" method to our MyShoppingCart Class.</span></span>

<span data-ttu-id="0cfa2-153">W pliku MyShoppingCart.cs Dodaj następujący kod.</span><span class="sxs-lookup"><span data-stu-id="0cfa2-153">In the MyShoppingCart.cs file add the following code.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample12.cs)]

<span data-ttu-id="0cfa2-154">Następnie na stronie\_firma Microsoft będzie metodę można wywołać nasz GetTotal program obsługi zdarzeń obciążenia.</span><span class="sxs-lookup"><span data-stu-id="0cfa2-154">Then in the Page\_Load event handler we'll can call our GetTotal method.</span></span> <span data-ttu-id="0cfa2-155">W tym samym czasie dodamy test, aby sprawdzić, czy koszyka jest pusta i odpowiednio dostosować ekran, jeśli jest.</span><span class="sxs-lookup"><span data-stu-id="0cfa2-155">At the same time we'll add a test to see if the shopping cart is empty and adjust the display accordingly if it is.</span></span>

<span data-ttu-id="0cfa2-156">Teraz Jeśli koszyka jest pusta uzyskujemy to:</span><span class="sxs-lookup"><span data-stu-id="0cfa2-156">Now if the shopping cart is empty we get this:</span></span>

![](tailspin-spyworks-part-5/_static/image4.jpg)

<span data-ttu-id="0cfa2-157">A jeśli nie, widzimy naszych łącznie.</span><span class="sxs-lookup"><span data-stu-id="0cfa2-157">And if not, we see our total.</span></span>

![](tailspin-spyworks-part-5/_static/image5.jpg)

<span data-ttu-id="0cfa2-158">Jednak ta strona nie jest jeszcze ukończone.</span><span class="sxs-lookup"><span data-stu-id="0cfa2-158">However, this page is not yet complete.</span></span>

<span data-ttu-id="0cfa2-159">Musimy dodatkowej logiki, aby ponownie obliczyć koszyka, usuwając elementy, które zostały oznaczone do usunięcia i określając nowe wartości ilości, ponieważ niektóre mogły zostać zmienione w siatce przez użytkownika.</span><span class="sxs-lookup"><span data-stu-id="0cfa2-159">We will need additional logic to recalculate the shopping cart by removing items marked for removal and by determining new quantity values as some may have been changed in the grid by the user.</span></span>

<span data-ttu-id="0cfa2-160">Umożliwia, Dodaj metodę "RemoveItem" do klasy Nasze koszyka zakupów w MyShoppingCart.cs, aby obsłużyć przypadek, gdy użytkownik oznaczenie elementu do usunięcia.</span><span class="sxs-lookup"><span data-stu-id="0cfa2-160">Lets add a "RemoveItem" method to our shopping cart class in MyShoppingCart.cs to handle the case when a user marks an item for removal.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample13.cs)]

<span data-ttu-id="0cfa2-161">Teraz sklonujemy ad metodę, aby obsłużyć sytuacji, gdy użytkownik po prostu zmienia jakości do zamówienia w widoku GridView.</span><span class="sxs-lookup"><span data-stu-id="0cfa2-161">Now let's ad a method to handle the circumstance when a user simply changes the quality to be ordered in the GridView.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample14.cs)]

<span data-ttu-id="0cfa2-162">Za pomocą podstawowych funkcji Remove i aktualizację w miejscu można zaimplementować logikę, która faktycznie aktualizuje koszyka zakupów w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="0cfa2-162">With the basic Remove and Update features in place we can implement the logic that actually updates the shopping cart in the database.</span></span> <span data-ttu-id="0cfa2-163">(In MyShoppingCart.cs)</span><span class="sxs-lookup"><span data-stu-id="0cfa2-163">(In MyShoppingCart.cs)</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample15.cs)]

<span data-ttu-id="0cfa2-164">Należy zauważyć, że że ta metoda oczekuje dwóch parametrów.</span><span class="sxs-lookup"><span data-stu-id="0cfa2-164">You'll note that this method expects two parameters.</span></span> <span data-ttu-id="0cfa2-165">Jeden jest koszyka Id, a drugi jest Tablica obiektów typu zdefiniowanego przez użytkownika.</span><span class="sxs-lookup"><span data-stu-id="0cfa2-165">One is the shopping cart Id and the other is an array of objects of user defined type.</span></span>

<span data-ttu-id="0cfa2-166">Aby zminimalizować zależności naszych logiki szczegółowych interfejsu użytkownika, zdefiniowaliśmy struktury danych, które możemy użyć, aby przekazać elementy koszyka zakupów do naszego kodu bez naszych metoda konieczności uzyskania bezpośredniego dostępu do kontrolki GridView.</span><span class="sxs-lookup"><span data-stu-id="0cfa2-166">So as to minimize the dependency of our logic on user interface specifics, we've defined a data structure that we can use to pass the shopping cart items to our code without our method needing to directly access the GridView control.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample16.cs)]

<span data-ttu-id="0cfa2-167">W naszym pliku MyShoppingCart.aspx.cs firma Microsoft może używać tej struktury w naszej obsługi zdarzeń kliknij przycisk aktualizacji w następujący sposób.</span><span class="sxs-lookup"><span data-stu-id="0cfa2-167">In our MyShoppingCart.aspx.cs file we can use this structure in our Update Button Click Event handler as follows.</span></span> <span data-ttu-id="0cfa2-168">Należy pamiętać, że oprócz aktualizowanie koszyka zaktualizujemy również Suma koszyka.</span><span class="sxs-lookup"><span data-stu-id="0cfa2-168">Note that in addition to updating the cart we will update the cart total as well.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample17.cs)]

<span data-ttu-id="0cfa2-169">Należy zwrócić uwagę przy użyciu szczególne znaczenie w odniesieniu ten wiersz kodu:</span><span class="sxs-lookup"><span data-stu-id="0cfa2-169">Note with particular interest this line of code:</span></span>

[!code-javascript[Main](tailspin-spyworks-part-5/samples/sample18.js)]

<span data-ttu-id="0cfa2-170">GetValues() to funkcja pomocnika specjalne, który wprowadzimy w MyShoppingCart.aspx.cs w następujący sposób.</span><span class="sxs-lookup"><span data-stu-id="0cfa2-170">GetValues() is a special helper function that we will implement in MyShoppingCart.aspx.cs as follows.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample19.cs)]

<span data-ttu-id="0cfa2-171">Zapewnia to czyste możliwość dostępu do wartości elementów powiązania w naszym kontrolki GridView.</span><span class="sxs-lookup"><span data-stu-id="0cfa2-171">This provides a clean way to access the values of the bound elements in our GridView control.</span></span> <span data-ttu-id="0cfa2-172">Ponieważ naszych formant pola wyboru "Usuń element" nie jest powiązana firma Microsoft będzie do niego dostęp za pomocą metody FindControl().</span><span class="sxs-lookup"><span data-stu-id="0cfa2-172">Since our "Remove Item" CheckBox Control is not bound we'll access it via the FindControl() method.</span></span>

<span data-ttu-id="0cfa2-173">Na tym etapie, podczas tworzenia projektu przygotowujemy się do zaimplementowania rozpoczęcie procesu realizowania zamówienia.</span><span class="sxs-lookup"><span data-stu-id="0cfa2-173">At this stage in your project's development we are getting ready to implement the checkout process.</span></span>

<span data-ttu-id="0cfa2-174">Przed ten sposób możemy należy użyć programu Visual Studio, aby wygenerować członkowskiej bazie danych, a następnie dodanie użytkownika do repozytorium członkostwa.</span><span class="sxs-lookup"><span data-stu-id="0cfa2-174">Before doing so let's use Visual Studio to generate the membership database and add a user to the membership repository.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="0cfa2-175">[Poprzednie](tailspin-spyworks-part-4.md)
> [dalej](tailspin-spyworks-part-6.md)</span><span class="sxs-lookup"><span data-stu-id="0cfa2-175">[Previous](tailspin-spyworks-part-4.md)
[Next](tailspin-spyworks-part-6.md)</span></span>
