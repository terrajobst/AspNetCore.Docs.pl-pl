---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-9
title: "Część 9: Rejestracja i wyewidencjonowania | Dokumentacja firmy Microsoft"
author: jongalloway
description: "Ten samouczek serii zawiera szczegóły dotyczące wszystkich kroków tworzenia przykładowej aplikacji ASP.NET MVC utworów muzycznych magazynu. Część 9 obejmuje rejestracji i płatności."
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/21/2011
ms.topic: article
ms.assetid: d65c5c2b-a039-463f-ad29-25cf9fb7a1ba
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-9
msc.type: authoredcontent
ms.openlocfilehash: 1caf836f8c92cbc9ab95e0aa990f81493e577a27
ms.sourcegitcommit: 016f4d58663bcd442930227022de23fb3abee0b3
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/12/2018
---
<a name="part-9-registration-and-checkout"></a><span data-ttu-id="180b9-104">Część 9: Rejestracja i wyewidencjonowania</span><span class="sxs-lookup"><span data-stu-id="180b9-104">Part 9: Registration and Checkout</span></span>
====================
<span data-ttu-id="180b9-105">przez [Galloway Jan](https://github.com/jongalloway)</span><span class="sxs-lookup"><span data-stu-id="180b9-105">by [Jon Galloway](https://github.com/jongalloway)</span></span>

> <span data-ttu-id="180b9-106">Magazyn utworów muzycznych MVC jest samouczek aplikacji, które wprowadzono i opisano krok po kroku, jak używać do tworzenia aplikacji sieci web platformy ASP.NET MVC i Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="180b9-106">The MVC Music Store is a tutorial application that introduces and explains step-by-step how to use ASP.NET MVC and Visual Studio for web development.</span></span>  
>   
> <span data-ttu-id="180b9-107">Magazyn utworów muzycznych MVC jest implementacja magazynu lekkie próbki, co sprzedaje albumów muzycznych w trybie online i implementuje podstawowej witryny administracji, logowania użytkownika i funkcje koszyka zakupów.</span><span class="sxs-lookup"><span data-stu-id="180b9-107">The MVC Music Store is a lightweight sample store implementation which sells music albums online, and implements basic site administration, user sign-in, and shopping cart functionality.</span></span>  
>   
> <span data-ttu-id="180b9-108">Ten samouczek serii zawiera szczegóły dotyczące wszystkich kroków tworzenia przykładowej aplikacji ASP.NET MVC utworów muzycznych magazynu.</span><span class="sxs-lookup"><span data-stu-id="180b9-108">This tutorial series details all of the steps taken to build the ASP.NET MVC Music Store sample application.</span></span> <span data-ttu-id="180b9-109">Część 9 obejmuje rejestracji i płatności.</span><span class="sxs-lookup"><span data-stu-id="180b9-109">Part 9 covers Registration and Checkout.</span></span>


<span data-ttu-id="180b9-110">W tej sekcji możemy zostanie utworzony CheckoutController, który będzie zbierać dana osoba adresu i informacji o płatności.</span><span class="sxs-lookup"><span data-stu-id="180b9-110">In this section, we will be creating a CheckoutController which will collect the shopper's address and payment information.</span></span> <span data-ttu-id="180b9-111">Firma Microsoft będzie wymagać od użytkowników do Zarejestruj się w naszej witrynie przed wyewidencjonowania, aby ten kontroler wymaga autoryzacji.</span><span class="sxs-lookup"><span data-stu-id="180b9-111">We will require users to register with our site prior to checking out, so this controller will require authorization.</span></span>

<span data-ttu-id="180b9-112">Użytkownicy będą przejdź do realizacji z ich koszyk przez kliknięcie przycisku "Wyewidencjonowania".</span><span class="sxs-lookup"><span data-stu-id="180b9-112">Users will navigate to the checkout process from their shopping cart by clicking the "Checkout" button.</span></span>

![](mvc-music-store-part-9/_static/image1.jpg)

<span data-ttu-id="180b9-113">Jeśli użytkownik nie jest zalogowany, zostanie wyświetlony monit do.</span><span class="sxs-lookup"><span data-stu-id="180b9-113">If the user is not logged in, they will be prompted to.</span></span>

![](mvc-music-store-part-9/_static/image1.png)

<span data-ttu-id="180b9-114">Po pomyślnym logowaniu użytkownika będzie wyświetlana widoku adres i płatności.</span><span class="sxs-lookup"><span data-stu-id="180b9-114">Upon successful login, the user is then shown the Address and Payment view.</span></span>

![](mvc-music-store-part-9/_static/image2.png)

<span data-ttu-id="180b9-115">Po ich wprowadzeniu formularza i przesłać kolejności, będą one wyświetlane ekran potwierdzenia zamówienia.</span><span class="sxs-lookup"><span data-stu-id="180b9-115">Once they have filled the form and submitted the order, they will be shown the order confirmation screen.</span></span>

![](mvc-music-store-part-9/_static/image3.png)

<span data-ttu-id="180b9-116">Próba wyświetlenia kolejności nieistniejącą lub kolejność, która nie należy do Ciebie zostaną wyświetlone w widoku błędów.</span><span class="sxs-lookup"><span data-stu-id="180b9-116">Attempting to view either a non-existent order or an order that doesn't belong to you will show the Error view.</span></span>

![](mvc-music-store-part-9/_static/image4.png)

## <a name="migrating-the-shopping-cart"></a><span data-ttu-id="180b9-117">Migrowanie koszyka</span><span class="sxs-lookup"><span data-stu-id="180b9-117">Migrating the Shopping Cart</span></span>

<span data-ttu-id="180b9-118">Podczas procesu zakupów anonimowe, gdy użytkownik kliknie przycisk wyewidencjonowania, one będą musieli zarejestrować i logowania.</span><span class="sxs-lookup"><span data-stu-id="180b9-118">While the shopping process is anonymous, when the user clicks on the Checkout button, they will be required to register and login.</span></span> <span data-ttu-id="180b9-119">Użytkownicy będą oczekują zachowamy ich zakupów informacji koszyka między wizyt, więc musimy kojarzy informacje koszyka zakupów z użytkownikiem, po ich zakończeniu rejestracji lub logowania.</span><span class="sxs-lookup"><span data-stu-id="180b9-119">Users will expect that we will maintain their shopping cart information between visits, so we will need to associate the shopping cart information with a user when they complete registration or login.</span></span>

<span data-ttu-id="180b9-120">Jest to rzeczywiście bardzo proste, w celu, ponieważ klasa nasze ShoppingCart już ma metodę, która zostanie skojarzony wszystkich elementów na bieżącej koszyka z nazwą użytkownika.</span><span class="sxs-lookup"><span data-stu-id="180b9-120">This is actually very simple to do, as our ShoppingCart class already has a method which will associate all the items in the current cart with a username.</span></span> <span data-ttu-id="180b9-121">Po prostu musimy ta metoda jest wywoływana po zakończeniu użytkownika rejestracji lub logowania.</span><span class="sxs-lookup"><span data-stu-id="180b9-121">We will just need to call this method when a user completes registration or login.</span></span>

<span data-ttu-id="180b9-122">Otwórz **elementu AccountController** klasy, która dodaliśmy możemy zostały konfigurowania członkostwa i autoryzacji.</span><span class="sxs-lookup"><span data-stu-id="180b9-122">Open the **AccountController** class that we added when we were setting up Membership and Authorization.</span></span> <span data-ttu-id="180b9-123">Dodaj następnie za pomocą instrukcji odwołujące się do MvcMusicStore.Models, dodaj następującą metodę MigrateShoppingCart:</span><span class="sxs-lookup"><span data-stu-id="180b9-123">Add a using statement referencing MvcMusicStore.Models, then add the following MigrateShoppingCart method:</span></span>

[!code-csharp[Main](mvc-music-store-part-9/samples/sample1.cs)]

<span data-ttu-id="180b9-124">Następnie należy zmodyfikować akcji po logowaniu do wywołania MigrateShoppingCart po zweryfikowaniu użytkownika, jak pokazano poniżej:</span><span class="sxs-lookup"><span data-stu-id="180b9-124">Next, modify the LogOn post action to call MigrateShoppingCart after the user has been validated, as shown below:</span></span>

[!code-csharp[Main](mvc-music-store-part-9/samples/sample2.cs)]

<span data-ttu-id="180b9-125">Wprowadzić do rejestru post akcji, natychmiast po pomyślnym utworzeniu konta użytkownika:</span><span class="sxs-lookup"><span data-stu-id="180b9-125">Make the same change to the Register post action, immediately after the user account is successfully created:</span></span>

[!code-csharp[Main](mvc-music-store-part-9/samples/sample3.cs)]

<span data-ttu-id="180b9-126">To wszystko — teraz anonimowe koszyk zostanie automatycznie przeniesiona do konta użytkownika po pomyślnej rejestracji lub logowania.</span><span class="sxs-lookup"><span data-stu-id="180b9-126">That's it - now an anonymous shopping cart will be automatically transferred to a user account upon successful registration or login.</span></span>

## <a name="creating-the-checkoutcontroller"></a><span data-ttu-id="180b9-127">Tworzenie CheckoutController</span><span class="sxs-lookup"><span data-stu-id="180b9-127">Creating the CheckoutController</span></span>

<span data-ttu-id="180b9-128">Kliknij prawym przyciskiem myszy folder kontrolery i dodania nowego kontrolera do projektu o nazwie CheckoutController przy użyciu szablonu pusty kontroler.</span><span class="sxs-lookup"><span data-stu-id="180b9-128">Right-click on the Controllers folder and add a new Controller to the project named CheckoutController using the Empty controller template.</span></span>

![](mvc-music-store-part-9/_static/image5.png)

<span data-ttu-id="180b9-129">Najpierw dodaj atrybut autoryzacji powyżej deklaracji klasy kontrolera, aby wymagać od użytkowników zarejestrować przed wyewidencjonowania:</span><span class="sxs-lookup"><span data-stu-id="180b9-129">First, add the Authorize attribute above the Controller class declaration to require users to register before checkout:</span></span>

[!code-csharp[Main](mvc-music-store-part-9/samples/sample4.cs)]

<span data-ttu-id="180b9-130">*Uwaga: Jest to podobne do zmiany, które wprowadziliśmy wcześniej do StoreManagerController, ale w takim przypadku wymagany atrybut autoryzacji, czy użytkownik jest w roli administratora. W kontrolerze wyewidencjonowania, firma Microsoft jest wymaganie, użytkownik jest zalogowany, ale nie są wymagające, aby były Administratorzy.*</span><span class="sxs-lookup"><span data-stu-id="180b9-130">*Note: This is similar to the change we previously made to the StoreManagerController, but in that case the Authorize attribute required that the user be in an Administrator role. In the Checkout Controller, we're requiring the user be logged in but aren't requiring that they be administrators.*</span></span>

<span data-ttu-id="180b9-131">Dla uproszczenia firma Microsoft nie będzie można zajmujących się informacje o płatności w tym samouczku.</span><span class="sxs-lookup"><span data-stu-id="180b9-131">For the sake of simplicity, we won't be dealing with payment information in this tutorial.</span></span> <span data-ttu-id="180b9-132">Zamiast tego jest więcej użytkowników do wyewidencjonowania, przy użyciu kod promocyjny.</span><span class="sxs-lookup"><span data-stu-id="180b9-132">Instead, we are allowing users to check out using a promotional code.</span></span> <span data-ttu-id="180b9-133">Ten kod promocyjny przy użyciu stałej o nazwie PromoCode będzie przechowywane.</span><span class="sxs-lookup"><span data-stu-id="180b9-133">We will store this promotional code using a constant named PromoCode.</span></span>

<span data-ttu-id="180b9-134">Jak StoreController firma Microsoft będzie zadeklarować pole do przechowywania wystąpienia klasy MusicStoreEntities o nazwie storeDB.</span><span class="sxs-lookup"><span data-stu-id="180b9-134">As in the StoreController, we'll declare a field to hold an instance of the MusicStoreEntities class, named storeDB.</span></span> <span data-ttu-id="180b9-135">Aby używać klasy MusicStoreEntities, musimy dodać za pomocą instrukcji MvcMusicStore.Models przestrzeni nazw.</span><span class="sxs-lookup"><span data-stu-id="180b9-135">In order to make use of the MusicStoreEntities class, we will need to add a using statement for the MvcMusicStore.Models namespace.</span></span> <span data-ttu-id="180b9-136">Na początku kontrolera wyewidencjonowania pojawia się poniżej.</span><span class="sxs-lookup"><span data-stu-id="180b9-136">The top of our Checkout controller appears below.</span></span>

[!code-csharp[Main](mvc-music-store-part-9/samples/sample5.cs)]

<span data-ttu-id="180b9-137">CheckoutController mają następujące akcje kontrolera:</span><span class="sxs-lookup"><span data-stu-id="180b9-137">The CheckoutController will have the following controller actions:</span></span>

<span data-ttu-id="180b9-138">**AddressAndPayment (metoda GET)** spowoduje wyświetlenie formularza, aby umożliwić użytkownikom wprowadzanie informacji o ich.</span><span class="sxs-lookup"><span data-stu-id="180b9-138">**AddressAndPayment (GET method)** will display a form to allow the user to enter their information.</span></span>

<span data-ttu-id="180b9-139">**AddressAndPayment (metody POST)** będzie sprawdzanie poprawności danych wejściowych i przetwarzania zamówienia.</span><span class="sxs-lookup"><span data-stu-id="180b9-139">**AddressAndPayment (POST method)** will validate the input and process the order.</span></span>

<span data-ttu-id="180b9-140">**Zakończenie** będą wyświetlane, gdy użytkownik pomyślnie zakończy proces realizacji transakcji.</span><span class="sxs-lookup"><span data-stu-id="180b9-140">**Complete** will be shown after a user has successfully finished the checkout process.</span></span> <span data-ttu-id="180b9-141">Ten widok będzie zawierać liczbę porządkową użytkownika, jako potwierdzenia.</span><span class="sxs-lookup"><span data-stu-id="180b9-141">This view will include the user's order number, as confirmation.</span></span>

<span data-ttu-id="180b9-142">Po pierwsze teraz Zmień nazwę akcji kontrolera indeksu (który został wygenerowany, gdy utworzono kontrolera) na AddressAndPayment.</span><span class="sxs-lookup"><span data-stu-id="180b9-142">First, let's rename the Index controller action (which was generated when we created the controller) to AddressAndPayment.</span></span> <span data-ttu-id="180b9-143">Ta akcja kontrolera wyświetli formularz wyewidencjonowania, więc nie wymaga żadnych informacji o modelu.</span><span class="sxs-lookup"><span data-stu-id="180b9-143">This controller action just displays the checkout form, so it doesn't require any model information.</span></span>

[!code-csharp[Main](mvc-music-store-part-9/samples/sample6.cs)]

<span data-ttu-id="180b9-144">Nasze metody AddressAndPayment POST będzie wykonują te same czynności, które zostały użyte podczas StoreManagerController: spróbuje zaakceptować przesyłania formularza i ukończyć kolejność i ponownie spowoduje wyświetlenie formularza, jeśli działanie nie powiodło się.</span><span class="sxs-lookup"><span data-stu-id="180b9-144">Our AddressAndPayment POST method will follow the same pattern we used in the StoreManagerController: it will try to accept the form submission and complete the order, and will re-display the form if it fails.</span></span>

<span data-ttu-id="180b9-145">Po sprawdzanie poprawności danych wejściowych z formularza spełnia wymagania weryfikacji naszych zamówienia, firma Microsoft będzie sprawdzać PromoCode wartości formularza bezpośrednio.</span><span class="sxs-lookup"><span data-stu-id="180b9-145">After validating the form input meets our validation requirements for an Order, we will check the PromoCode form value directly.</span></span> <span data-ttu-id="180b9-146">Zakładając, że wszystkie ustawienia są poprawne, możemy zaktualizowane informacje zostaną zapisane w kolejności, poinformuj obiektu ShoppingCart, aby ukończyć proces kolejności i Przekierowanie do ukończenia akcji.</span><span class="sxs-lookup"><span data-stu-id="180b9-146">Assuming everything is correct, we will save the updated information with the order, tell the ShoppingCart object to complete the order process, and redirect to the Complete action.</span></span>

[!code-csharp[Main](mvc-music-store-part-9/samples/sample7.cs)]

<span data-ttu-id="180b9-147">Po pomyślnym zakończeniu procesu realizacji transakcji użytkowników zostanie przekierowany do akcji kontrolera ukończone.</span><span class="sxs-lookup"><span data-stu-id="180b9-147">Upon successful completion of the checkout process, users will be redirected to the Complete controller action.</span></span> <span data-ttu-id="180b9-148">Ta akcja wykona prostego wyboru, aby zweryfikować, że kolejność w rzeczywistości należy do zalogowanego użytkownika przed pokazaniem numer zamówienia jako potwierdzenie.</span><span class="sxs-lookup"><span data-stu-id="180b9-148">This action will perform a simple check to validate that the order does indeed belong to the logged-in user before showing the order number as a confirmation.</span></span>

[!code-csharp[Main](mvc-music-store-part-9/samples/sample8.cs)]

<span data-ttu-id="180b9-149">*Uwaga: Wystąpił błąd podczas widoku został utworzony automatycznie firmie Microsoft w folderze /Views/Shared momencie mamy rozpoczęcia projektu.*</span><span class="sxs-lookup"><span data-stu-id="180b9-149">*Note: The Error view was automatically created for us in the /Views/Shared folder when we began the project.*</span></span>

<span data-ttu-id="180b9-150">Kompletny kod CheckoutController wygląda następująco:</span><span class="sxs-lookup"><span data-stu-id="180b9-150">The complete CheckoutController code is as follows:</span></span>

[!code-csharp[Main](mvc-music-store-part-9/samples/sample9.cs)]

## <a name="adding-the-addressandpayment-view"></a><span data-ttu-id="180b9-151">Dodawanie widoku AddressAndPayment</span><span class="sxs-lookup"><span data-stu-id="180b9-151">Adding the AddressAndPayment view</span></span>

<span data-ttu-id="180b9-152">Teraz Utwórzmy AddressAndPayment widoku.</span><span class="sxs-lookup"><span data-stu-id="180b9-152">Now, let's create the AddressAndPayment view.</span></span> <span data-ttu-id="180b9-153">Kliknij prawym przyciskiem myszy na jednym z AddressAndPayment akcji kontrolera, a następnie Dodaj widok o nazwie AddressAndPayment jest silnie typizowane jako kolejność, która używa Edytuj szablon, jak pokazano poniżej.</span><span class="sxs-lookup"><span data-stu-id="180b9-153">Right-click on one of the AddressAndPayment controller actions and add a view named AddressAndPayment which is strongly typed as an Order and uses the Edit template, as shown below.</span></span>

![](mvc-music-store-part-9/_static/image6.png)

<span data-ttu-id="180b9-154">Ten widok spowoduje, że użycie dwóch metod analizujemy podczas tworzenia widoku StoreManagerEdit:</span><span class="sxs-lookup"><span data-stu-id="180b9-154">This view will make use of two of the techniques we looked at while building the StoreManagerEdit view:</span></span>

- <span data-ttu-id="180b9-155">Używamy Html.EditorForModel() do wyświetlania pól formularza dla modelu kolejności</span><span class="sxs-lookup"><span data-stu-id="180b9-155">We will use Html.EditorForModel() to display form fields for the Order model</span></span>
- <span data-ttu-id="180b9-156">Firma Microsoft będzie korzystać z atrybutów sprawdzania poprawności przy użyciu klasy kolejność reguł sprawdzania poprawności</span><span class="sxs-lookup"><span data-stu-id="180b9-156">We will leverage validation rules using an Order class with validation attributes</span></span>

<span data-ttu-id="180b9-157">Zaczniemy aktualizując kod formularza, aby użyć Html.EditorForModel() następuje dodatkowe pole tekstowe dla ten kod promocyjny.</span><span class="sxs-lookup"><span data-stu-id="180b9-157">We'll start by updating the form code to use Html.EditorForModel(), followed by an additional textbox for the Promo Code.</span></span> <span data-ttu-id="180b9-158">Kompletny kod dla widoku AddressAndPayment przedstawiono poniżej.</span><span class="sxs-lookup"><span data-stu-id="180b9-158">The complete code for the AddressAndPayment view is shown below.</span></span>

[!code-cshtml[Main](mvc-music-store-part-9/samples/sample10.cshtml)]

## <a name="defining-validation-rules-for-the-order"></a><span data-ttu-id="180b9-159">Definiowanie reguł sprawdzania poprawności dla zlecenia</span><span class="sxs-lookup"><span data-stu-id="180b9-159">Defining validation rules for the Order</span></span>

<span data-ttu-id="180b9-160">Teraz, kiedy naszych widoku jest skonfigurowane, możemy ustawi reguł sprawdzania poprawności dla modelu kolejności jak robiliśmy wcześniej dla modelu albumu.</span><span class="sxs-lookup"><span data-stu-id="180b9-160">Now that our view is set up, we will set up the validation rules for our Order model as we did previously for the Album model.</span></span> <span data-ttu-id="180b9-161">Kliknij prawym przyciskiem folder modeli i dodać klasę o nazwie kolejności.</span><span class="sxs-lookup"><span data-stu-id="180b9-161">Right-click on the Models folder and add a class named Order.</span></span> <span data-ttu-id="180b9-162">Oprócz atrybuty weryfikacji, które wcześniej użyliśmy albumu firma Microsoft będzie także przy użyciu wyrażenia regularnego Sprawdź poprawność adresu e-mail użytkownika.</span><span class="sxs-lookup"><span data-stu-id="180b9-162">In addition to the validation attributes we used previously for the Album, we will also be using a Regular Expression to validate the user's email address.</span></span>

[!code-csharp[Main](mvc-music-store-part-9/samples/sample11.cs)]

<span data-ttu-id="180b9-163">Podjęto próbę przesłać formularza z brakującym lub nieprawidłowe informacje teraz wyświetli komunikat o błędzie przy użyciu weryfikacji po stronie klienta.</span><span class="sxs-lookup"><span data-stu-id="180b9-163">Attempting to submit the form with missing or invalid information will now show error message using client-side validation.</span></span>

![](mvc-music-store-part-9/_static/image7.png)

<span data-ttu-id="180b9-164">Zgoda zostało wykonane większość pracy twardych dla realizacji; po prostu mamy kilka prawdopodobieństwo i kończy na zakończenie.</span><span class="sxs-lookup"><span data-stu-id="180b9-164">Okay, we've done most of the hard work for the checkout process; we just have a few odds and ends to finish.</span></span> <span data-ttu-id="180b9-165">Należy dodać dwa widoki proste i musimy zajmie się oddanie informacji koszyka w procesie logowania.</span><span class="sxs-lookup"><span data-stu-id="180b9-165">We need to add two simple views, and we need to take care of the handoff of the cart information during the login process.</span></span>

## <a name="adding-the-checkout-complete-view"></a><span data-ttu-id="180b9-166">Dodawanie wyewidencjonowania pełnego widoku</span><span class="sxs-lookup"><span data-stu-id="180b9-166">Adding the Checkout Complete view</span></span>

<span data-ttu-id="180b9-167">Wyewidencjonowanie pełnego widoku jest dość proste, ponieważ wymaga ona jedynie do wyświetlenia identyfikatora kolejności.</span><span class="sxs-lookup"><span data-stu-id="180b9-167">The Checkout Complete view is pretty simple, as it just needs to display the Order ID.</span></span> <span data-ttu-id="180b9-168">Kliknij prawym przyciskiem akcji kontrolera pełne i Dodaj widok o nazwie Complete, który jest silnie typizowane jako int.</span><span class="sxs-lookup"><span data-stu-id="180b9-168">Right-click on the Complete controller action and add a view named Complete which is strongly typed as an int.</span></span>

![](mvc-music-store-part-9/_static/image8.png)

<span data-ttu-id="180b9-169">Teraz modyfikacjom kod widoku do wyświetlenia Identyfikatora kolejności, jak pokazano poniżej.</span><span class="sxs-lookup"><span data-stu-id="180b9-169">Now we will update the view code to display the Order ID, as shown below.</span></span>

[!code-cshtml[Main](mvc-music-store-part-9/samples/sample12.cshtml)]

## <a name="updating-the-error-view"></a><span data-ttu-id="180b9-170">Trwa aktualizowanie widoku błędów</span><span class="sxs-lookup"><span data-stu-id="180b9-170">Updating The Error view</span></span>

<span data-ttu-id="180b9-171">Szablon domyślny zawiera widoku błędów w folderze Widoki udostępnione, dzięki czemu może być ponownie używane w innym miejscu w lokacji.</span><span class="sxs-lookup"><span data-stu-id="180b9-171">The default template includes an Error view in the Shared views folder so that it can be re-used elsewhere in the site.</span></span> <span data-ttu-id="180b9-172">Ten widok błąd zawiera błąd bardzo proste i nie używa naszej witrynie układu, więc będziemy informować go.</span><span class="sxs-lookup"><span data-stu-id="180b9-172">This Error view contains a very simple error and doesn't use our site Layout, so we'll update it.</span></span>

<span data-ttu-id="180b9-173">Ponieważ jest to strona rodzajowy komunikat o błędzie, zawartość jest bardzo proste.</span><span class="sxs-lookup"><span data-stu-id="180b9-173">Since this is a generic error page, the content is very simple.</span></span> <span data-ttu-id="180b9-174">Firma Microsoft będzie zawierać wiadomości, a łącze, aby przejść do poprzedniej strony w historii, jeśli użytkownik chce ponownie spróbuj wykonać akcję ich.</span><span class="sxs-lookup"><span data-stu-id="180b9-174">We'll include a message and a link to navigate to the previous page in history if the user wants to re-try their action.</span></span>

[!code-cshtml[Main](mvc-music-store-part-9/samples/sample13.cshtml)]


>[!div class="step-by-step"]
<span data-ttu-id="180b9-175">[Poprzednie](mvc-music-store-part-8.md)
[dalej](mvc-music-store-part-10.md)</span><span class="sxs-lookup"><span data-stu-id="180b9-175">[Previous](mvc-music-store-part-8.md)
[Next](mvc-music-store-part-10.md)</span></span>
