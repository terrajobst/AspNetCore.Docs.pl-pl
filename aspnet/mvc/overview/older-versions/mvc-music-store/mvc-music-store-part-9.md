---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-9
title: 'Część 9: Rejestracja i finalizacja zakupu | Dokumentacja firmy Microsoft'
author: jongalloway
description: W tej serii samouczków szczegółowo opisuje wszystkie etapy, tworzenie przykładowej aplikacji platformy ASP.NET MVC Music Store. Część 9 obejmuje Rejestracja i finalizacja zakupu.
ms.author: aspnetcontent
ms.date: 04/21/2011
ms.assetid: d65c5c2b-a039-463f-ad29-25cf9fb7a1ba
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-9
msc.type: authoredcontent
ms.openlocfilehash: e521e30cb820d834d57c87538b869861a536657b
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/05/2018
ms.locfileid: "37812876"
---
<a name="part-9-registration-and-checkout"></a><span data-ttu-id="9353c-104">Część 9: Rejestracja i finalizacja zakupu</span><span class="sxs-lookup"><span data-stu-id="9353c-104">Part 9: Registration and Checkout</span></span>
====================
<span data-ttu-id="9353c-105">przez [Galloway'em Jon](https://github.com/jongalloway)</span><span class="sxs-lookup"><span data-stu-id="9353c-105">by [Jon Galloway](https://github.com/jongalloway)</span></span>

> <span data-ttu-id="9353c-106">MVC Music Store jest aplikacją z samouczka, który wprowadzono i opisano krok po kroku, jak używać platformy ASP.NET MVC i programu Visual Studio do tworzenia aplikacji internetowych.</span><span class="sxs-lookup"><span data-stu-id="9353c-106">The MVC Music Store is a tutorial application that introduces and explains step-by-step how to use ASP.NET MVC and Visual Studio for web development.</span></span>  
>   
> <span data-ttu-id="9353c-107">MVC Music Store jest uproszczone przykładową implementację magazynu sprzedaje utworów muzycznych albumy online, która implementuje podstawowej witryny administracji, logowania użytkownika i funkcje koszyka zakupów.</span><span class="sxs-lookup"><span data-stu-id="9353c-107">The MVC Music Store is a lightweight sample store implementation which sells music albums online, and implements basic site administration, user sign-in, and shopping cart functionality.</span></span>  
>   
> <span data-ttu-id="9353c-108">W tej serii samouczków szczegółowo opisuje wszystkie etapy, tworzenie przykładowej aplikacji platformy ASP.NET MVC Music Store.</span><span class="sxs-lookup"><span data-stu-id="9353c-108">This tutorial series details all of the steps taken to build the ASP.NET MVC Music Store sample application.</span></span> <span data-ttu-id="9353c-109">Część 9 obejmuje Rejestracja i finalizacja zakupu.</span><span class="sxs-lookup"><span data-stu-id="9353c-109">Part 9 covers Registration and Checkout.</span></span>


<span data-ttu-id="9353c-110">W tej sekcji utworzymy CheckoutController, który będzie zbierać dobierana adresu i informacji o płatności.</span><span class="sxs-lookup"><span data-stu-id="9353c-110">In this section, we will be creating a CheckoutController which will collect the shopper's address and payment information.</span></span> <span data-ttu-id="9353c-111">Firma Microsoft będzie wymagać od użytkowników do rejestracji w usłudze naszą witrynę przed wyewidencjonowywanie, więc ten kontroler wymaga autoryzacji.</span><span class="sxs-lookup"><span data-stu-id="9353c-111">We will require users to register with our site prior to checking out, so this controller will require authorization.</span></span>

<span data-ttu-id="9353c-112">Użytkownicy spowoduje przejście do rozpoczęcie procesu realizowania zamówienia z ich koszyka, klikając przycisk "Wyewidencjonowania".</span><span class="sxs-lookup"><span data-stu-id="9353c-112">Users will navigate to the checkout process from their shopping cart by clicking the "Checkout" button.</span></span>

![](mvc-music-store-part-9/_static/image1.jpg)

<span data-ttu-id="9353c-113">Jeśli użytkownik nie jest zalogowany, ich zostanie wyświetlony monit.</span><span class="sxs-lookup"><span data-stu-id="9353c-113">If the user is not logged in, they will be prompted to.</span></span>

![](mvc-music-store-part-9/_static/image1.png)

<span data-ttu-id="9353c-114">Po pomyślnym logowaniu użytkownika będzie wyświetlana w widoku adresu i płatności.</span><span class="sxs-lookup"><span data-stu-id="9353c-114">Upon successful login, the user is then shown the Address and Payment view.</span></span>

![](mvc-music-store-part-9/_static/image2.png)

<span data-ttu-id="9353c-115">Po ich wprowadzeniu formularza i złożeniem zamówienia, będą one wyświetlane ekranie potwierdzenia zamówienia.</span><span class="sxs-lookup"><span data-stu-id="9353c-115">Once they have filled the form and submitted the order, they will be shown the order confirmation screen.</span></span>

![](mvc-music-store-part-9/_static/image3.png)

<span data-ttu-id="9353c-116">Podjęto próbę wyświetlić kolejność nieistniejącej lub zamówienie, która nie należy do Ciebie pokaże widoku błędów.</span><span class="sxs-lookup"><span data-stu-id="9353c-116">Attempting to view either a non-existent order or an order that doesn't belong to you will show the Error view.</span></span>

![](mvc-music-store-part-9/_static/image4.png)

## <a name="migrating-the-shopping-cart"></a><span data-ttu-id="9353c-117">Migrowanie koszyka</span><span class="sxs-lookup"><span data-stu-id="9353c-117">Migrating the Shopping Cart</span></span>

<span data-ttu-id="9353c-118">Chociaż proces zakupów anonimowe, gdy użytkownik kliknie przycisk wyewidencjonowania, ich będą musieli zarejestrować i zaloguj się.</span><span class="sxs-lookup"><span data-stu-id="9353c-118">While the shopping process is anonymous, when the user clicks on the Checkout button, they will be required to register and login.</span></span> <span data-ttu-id="9353c-119">Użytkownicy oczekują, że firma Microsoft zachowa informacjami koszyka zakupów między wizyt, dzięki czemu firma Microsoft będzie należy skojarzyć je koszyka zakupów z użytkownikiem, po ich zakończeniu rejestracji lub logowania.</span><span class="sxs-lookup"><span data-stu-id="9353c-119">Users will expect that we will maintain their shopping cart information between visits, so we will need to associate the shopping cart information with a user when they complete registration or login.</span></span>

<span data-ttu-id="9353c-120">Jest to naprawdę bardzo proste, w celu klasy Nasze ShoppingCart ma już metody, która zostanie skojarzony z wszystkich elementów w koszyku bieżącego z nazwą użytkownika.</span><span class="sxs-lookup"><span data-stu-id="9353c-120">This is actually very simple to do, as our ShoppingCart class already has a method which will associate all the items in the current cart with a username.</span></span> <span data-ttu-id="9353c-121">Po prostu konieczne będzie wywołanie tej metody, gdy użytkownik kończy rejestracji lub logowania.</span><span class="sxs-lookup"><span data-stu-id="9353c-121">We will just need to call this method when a user completes registration or login.</span></span>

<span data-ttu-id="9353c-122">Otwórz **elementu AccountController** klasę, która dodaliśmy przygotowując możemy zostały członkostwo i autoryzacja.</span><span class="sxs-lookup"><span data-stu-id="9353c-122">Open the **AccountController** class that we added when we were setting up Membership and Authorization.</span></span> <span data-ttu-id="9353c-123">Dodaj następnie za pomocą instrukcji odwołujące się do MvcMusicStore.Models, dodaj następującą metodę MigrateShoppingCart:</span><span class="sxs-lookup"><span data-stu-id="9353c-123">Add a using statement referencing MvcMusicStore.Models, then add the following MigrateShoppingCart method:</span></span>

[!code-csharp[Main](mvc-music-store-part-9/samples/sample1.cs)]

<span data-ttu-id="9353c-124">Następnie zmodyfikuj akcji po logowania, aby wywołać MigrateShoppingCart po sprawdzeniu poprawności użytkownika, jak pokazano poniżej:</span><span class="sxs-lookup"><span data-stu-id="9353c-124">Next, modify the LogOn post action to call MigrateShoppingCart after the user has been validated, as shown below:</span></span>

[!code-csharp[Main](mvc-music-store-part-9/samples/sample2.cs)]

<span data-ttu-id="9353c-125">Należy tej samej zmiany do rejestru wpis akcji, natychmiast, po pomyślnym utworzeniu konta użytkownika:</span><span class="sxs-lookup"><span data-stu-id="9353c-125">Make the same change to the Register post action, immediately after the user account is successfully created:</span></span>

[!code-csharp[Main](mvc-music-store-part-9/samples/sample3.cs)]

<span data-ttu-id="9353c-126">To wszystko — teraz anonimowe koszyka będą automatycznie przekazywane do konta użytkownika po pomyślnej rejestracji lub logowania.</span><span class="sxs-lookup"><span data-stu-id="9353c-126">That's it - now an anonymous shopping cart will be automatically transferred to a user account upon successful registration or login.</span></span>

## <a name="creating-the-checkoutcontroller"></a><span data-ttu-id="9353c-127">Tworzenie CheckoutController</span><span class="sxs-lookup"><span data-stu-id="9353c-127">Creating the CheckoutController</span></span>

<span data-ttu-id="9353c-128">Kliknij prawym przyciskiem myszy w folderze kontrolerów i Dodaj nowy kontroler do projektu o nazwie CheckoutController przy użyciu szablonu pusty kontroler.</span><span class="sxs-lookup"><span data-stu-id="9353c-128">Right-click on the Controllers folder and add a new Controller to the project named CheckoutController using the Empty controller template.</span></span>

![](mvc-music-store-part-9/_static/image5.png)

<span data-ttu-id="9353c-129">Najpierw dodaj atrybut Autoryzuj nad deklaracją klasy kontrolera, aby wymagać od użytkowników zarejestrowania przed wyewidencjonowania:</span><span class="sxs-lookup"><span data-stu-id="9353c-129">First, add the Authorize attribute above the Controller class declaration to require users to register before checkout:</span></span>

[!code-csharp[Main](mvc-music-store-part-9/samples/sample4.cs)]

<span data-ttu-id="9353c-130">*Uwaga: Jest to podobne do zmiany, które wcześniej wprowadziliśmy StoreManagerController, ale w tym przypadku atrybut autoryzacji wymagane, aby użytkownik był w roli administratora. W kontrolerze wyewidencjonowania, firma Microsoft jest wymaganie, użytkownik jest zalogowany, ale nie wymaga, aby były one administratorów.*</span><span class="sxs-lookup"><span data-stu-id="9353c-130">*Note: This is similar to the change we previously made to the StoreManagerController, but in that case the Authorize attribute required that the user be in an Administrator role. In the Checkout Controller, we're requiring the user be logged in but aren't requiring that they be administrators.*</span></span>

<span data-ttu-id="9353c-131">Dla uproszczenia firma Microsoft nie będzie można zajmujących się informacje o płatności w ramach tego samouczka.</span><span class="sxs-lookup"><span data-stu-id="9353c-131">For the sake of simplicity, we won't be dealing with payment information in this tutorial.</span></span> <span data-ttu-id="9353c-132">Zamiast tego jest więcej użytkowników do wyewidencjonowania, przy użyciu kodu promocyjnego.</span><span class="sxs-lookup"><span data-stu-id="9353c-132">Instead, we are allowing users to check out using a promotional code.</span></span> <span data-ttu-id="9353c-133">Ten kod promocyjny przy użyciu stałą o nazwie PromoCode będą przechowywane.</span><span class="sxs-lookup"><span data-stu-id="9353c-133">We will store this promotional code using a constant named PromoCode.</span></span>

<span data-ttu-id="9353c-134">Jak StoreController firma Microsoft będzie zadeklarować pole do przechowywania wystąpienia klasy MusicStoreEntities o nazwie storeDB.</span><span class="sxs-lookup"><span data-stu-id="9353c-134">As in the StoreController, we'll declare a field to hold an instance of the MusicStoreEntities class, named storeDB.</span></span> <span data-ttu-id="9353c-135">Aby można było wprowadzić korzystać z klasy MusicStoreEntities, firma Microsoft będzie należy dodać, przy użyciu instrukcji dla przestrzeni nazw MvcMusicStore.Models.</span><span class="sxs-lookup"><span data-stu-id="9353c-135">In order to make use of the MusicStoreEntities class, we will need to add a using statement for the MvcMusicStore.Models namespace.</span></span> <span data-ttu-id="9353c-136">Górnej kontrolera wyewidencjonowania pojawia się poniżej.</span><span class="sxs-lookup"><span data-stu-id="9353c-136">The top of our Checkout controller appears below.</span></span>

[!code-csharp[Main](mvc-music-store-part-9/samples/sample5.cs)]

<span data-ttu-id="9353c-137">CheckoutController będzie miała następujące akcje kontrolera:</span><span class="sxs-lookup"><span data-stu-id="9353c-137">The CheckoutController will have the following controller actions:</span></span>

<span data-ttu-id="9353c-138">**AddressAndPayment (metoda GET)** spowoduje wyświetlenie formularza, aby umożliwić użytkownikowi wprowadzić informacje o ich.</span><span class="sxs-lookup"><span data-stu-id="9353c-138">**AddressAndPayment (GET method)** will display a form to allow the user to enter their information.</span></span>

<span data-ttu-id="9353c-139">**AddressAndPayment (metody POST)** będzie sprawdzanie poprawności danych wejściowych i przetworzenia zamówienia.</span><span class="sxs-lookup"><span data-stu-id="9353c-139">**AddressAndPayment (POST method)** will validate the input and process the order.</span></span>

<span data-ttu-id="9353c-140">**Pełne** zostanie wyświetlone po użytkownik zostało zakończone pomyślnie rozpoczęcie procesu realizowania zamówienia.</span><span class="sxs-lookup"><span data-stu-id="9353c-140">**Complete** will be shown after a user has successfully finished the checkout process.</span></span> <span data-ttu-id="9353c-141">Ten widok będzie zawierać numer zamówienia przez użytkownika, jako potwierdzenie.</span><span class="sxs-lookup"><span data-stu-id="9353c-141">This view will include the user's order number, as confirmation.</span></span>

<span data-ttu-id="9353c-142">Po pierwsze możemy zmienić nazwy indeksu akcji kontrolera, (który został wygenerowany podczas tworzenia kontrolera) na AddressAndPayment.</span><span class="sxs-lookup"><span data-stu-id="9353c-142">First, let's rename the Index controller action (which was generated when we created the controller) to AddressAndPayment.</span></span> <span data-ttu-id="9353c-143">Ta akcja kontrolera po prostu wyświetla formularz wyewidencjonowania, więc nie wymaga żadnych informacji o modelu.</span><span class="sxs-lookup"><span data-stu-id="9353c-143">This controller action just displays the checkout form, so it doesn't require any model information.</span></span>

[!code-csharp[Main](mvc-music-store-part-9/samples/sample6.cs)]

<span data-ttu-id="9353c-144">Nasze metody AddressAndPayment POST będzie zgodna z tego samego wzorca użytymi w StoreManagerController: spróbuje zaakceptować przesyłania formularza i wypełnij zamówienie i ponownie spowoduje wyświetlenie formularza, jeśli zakończy się niepowodzeniem.</span><span class="sxs-lookup"><span data-stu-id="9353c-144">Our AddressAndPayment POST method will follow the same pattern we used in the StoreManagerController: it will try to accept the form submission and complete the order, and will re-display the form if it fails.</span></span>

<span data-ttu-id="9353c-145">Po weryfikacji danych wejściowych z formularza spełnia naszych wymagań w zakresie sprawdzania poprawności dla zamówienia, firma Microsoft będzie sprawdzać wartości formularza PromoCode bezpośrednio.</span><span class="sxs-lookup"><span data-stu-id="9353c-145">After validating the form input meets our validation requirements for an Order, we will check the PromoCode form value directly.</span></span> <span data-ttu-id="9353c-146">Przy założeniu, że wszystko jest poprawna, firma Microsoft będzie zapisywać zaktualizowane informacje o kolejności, powiedzieć obiektowi ShoppingCart, aby ukończyć proces kolejności i Przekieruj do Zakończenie akcji.</span><span class="sxs-lookup"><span data-stu-id="9353c-146">Assuming everything is correct, we will save the updated information with the order, tell the ShoppingCart object to complete the order process, and redirect to the Complete action.</span></span>

[!code-csharp[Main](mvc-music-store-part-9/samples/sample7.cs)]

<span data-ttu-id="9353c-147">Po pomyślnym zakończeniu procesu realizowania zamówienia nastąpi przekierowanie do akcji kontrolera pełną użytkowników.</span><span class="sxs-lookup"><span data-stu-id="9353c-147">Upon successful completion of the checkout process, users will be redirected to the Complete controller action.</span></span> <span data-ttu-id="9353c-148">Ta akcja wykona prostą kontrolę, aby zweryfikować, że kolejność faktycznie należą do zalogowanego użytkownika przed wyświetleniem numer zamówienia jako potwierdzenie.</span><span class="sxs-lookup"><span data-stu-id="9353c-148">This action will perform a simple check to validate that the order does indeed belong to the logged-in user before showing the order number as a confirmation.</span></span>

[!code-csharp[Main](mvc-music-store-part-9/samples/sample8.cs)]

<span data-ttu-id="9353c-149">*Uwaga: Widok błąd został automatycznie utworzony dla nas w folderze /Views/Shared zaczynaliśmy korzystać z projektu.*</span><span class="sxs-lookup"><span data-stu-id="9353c-149">*Note: The Error view was automatically created for us in the /Views/Shared folder when we began the project.*</span></span>

<span data-ttu-id="9353c-150">Kompletny kod CheckoutController jest następująca:</span><span class="sxs-lookup"><span data-stu-id="9353c-150">The complete CheckoutController code is as follows:</span></span>

[!code-csharp[Main](mvc-music-store-part-9/samples/sample9.cs)]

## <a name="adding-the-addressandpayment-view"></a><span data-ttu-id="9353c-151">Dodawanie widoku AddressAndPayment</span><span class="sxs-lookup"><span data-stu-id="9353c-151">Adding the AddressAndPayment view</span></span>

<span data-ttu-id="9353c-152">Teraz Utwórzmy widoku AddressAndPayment.</span><span class="sxs-lookup"><span data-stu-id="9353c-152">Now, let's create the AddressAndPayment view.</span></span> <span data-ttu-id="9353c-153">Kliknij prawym przyciskiem myszy na jednym z AddressAndPayment akcji kontrolera, a następnie Dodaj widok o nazwie AddressAndPayment jest silnie typizowane jako zamówienie, która używa tego szablonu edycji, jak pokazano poniżej.</span><span class="sxs-lookup"><span data-stu-id="9353c-153">Right-click on one of the AddressAndPayment controller actions and add a view named AddressAndPayment which is strongly typed as an Order and uses the Edit template, as shown below.</span></span>

![](mvc-music-store-part-9/_static/image6.png)

<span data-ttu-id="9353c-154">W tym widoku spowoduje, że korzystanie z dwóch technik przyjrzeliśmy się podczas tworzenia widoku StoreManagerEdit:</span><span class="sxs-lookup"><span data-stu-id="9353c-154">This view will make use of two of the techniques we looked at while building the StoreManagerEdit view:</span></span>

- <span data-ttu-id="9353c-155">Firma Microsoft użyje Html.EditorForModel() do wyświetlania pól formularza dla modelu zamówienia</span><span class="sxs-lookup"><span data-stu-id="9353c-155">We will use Html.EditorForModel() to display form fields for the Order model</span></span>
- <span data-ttu-id="9353c-156">Firma Microsoft będzie korzystać z reguł sprawdzania poprawności, za pomocą klasy kolejności przy użyciu atrybutów sprawdzania poprawności</span><span class="sxs-lookup"><span data-stu-id="9353c-156">We will leverage validation rules using an Order class with validation attributes</span></span>

<span data-ttu-id="9353c-157">Zaczniemy, aktualizując kod formularza, aby użyć Html.EditorForModel(), następuje dodatkowe pole tekstowe dla kodu promocyjnego.</span><span class="sxs-lookup"><span data-stu-id="9353c-157">We'll start by updating the form code to use Html.EditorForModel(), followed by an additional textbox for the Promo Code.</span></span> <span data-ttu-id="9353c-158">Kompletny kod dla widoku AddressAndPayment znajdują się poniżej.</span><span class="sxs-lookup"><span data-stu-id="9353c-158">The complete code for the AddressAndPayment view is shown below.</span></span>

[!code-cshtml[Main](mvc-music-store-part-9/samples/sample10.cshtml)]

## <a name="defining-validation-rules-for-the-order"></a><span data-ttu-id="9353c-159">Definiowanie reguł sprawdzania poprawności dla zamówienia</span><span class="sxs-lookup"><span data-stu-id="9353c-159">Defining validation rules for the Order</span></span>

<span data-ttu-id="9353c-160">Teraz, gdy skonfigurowano naszych widoku firma Microsoft ustawi reguł sprawdzania poprawności dla modelu kolejności ile My mieliśmy wcześniej dla modelu albumu.</span><span class="sxs-lookup"><span data-stu-id="9353c-160">Now that our view is set up, we will set up the validation rules for our Order model as we did previously for the Album model.</span></span> <span data-ttu-id="9353c-161">Kliknij prawym przyciskiem myszy w folderze modeli i Dodaj klasę o nazwie zamówienia.</span><span class="sxs-lookup"><span data-stu-id="9353c-161">Right-click on the Models folder and add a class named Order.</span></span> <span data-ttu-id="9353c-162">Oprócz atrybutów sprawdzania poprawności, która była wcześniej używana albumu również użyjemy wyrażeń regularnych do sprawdzania poprawności adresu e-mail użytkownika.</span><span class="sxs-lookup"><span data-stu-id="9353c-162">In addition to the validation attributes we used previously for the Album, we will also be using a Regular Expression to validate the user's email address.</span></span>

[!code-csharp[Main](mvc-music-store-part-9/samples/sample11.cs)]

<span data-ttu-id="9353c-163">Podjęto próbę Prześlij formularz z brakującymi lub nieprawidłowe informacje będą teraz pokazywać komunikat o błędzie, za pomocą weryfikacji po stronie klienta.</span><span class="sxs-lookup"><span data-stu-id="9353c-163">Attempting to submit the form with missing or invalid information will now show error message using client-side validation.</span></span>

![](mvc-music-store-part-9/_static/image7.png)

<span data-ttu-id="9353c-164">To wszystko firma Microsoft wykonane większość trudną pracę na rozpoczęcie procesu realizowania zamówienia; Wystarczy kilka kończy się i kolizję na zakończenie.</span><span class="sxs-lookup"><span data-stu-id="9353c-164">Okay, we've done most of the hard work for the checkout process; we just have a few odds and ends to finish.</span></span> <span data-ttu-id="9353c-165">Należy dodać dwa widoki proste i musimy zadbać o przekazywanie informacji koszyka w procesie logowania.</span><span class="sxs-lookup"><span data-stu-id="9353c-165">We need to add two simple views, and we need to take care of the handoff of the cart information during the login process.</span></span>

## <a name="adding-the-checkout-complete-view"></a><span data-ttu-id="9353c-166">Dodawanie wyewidencjonowania pełny przegląd</span><span class="sxs-lookup"><span data-stu-id="9353c-166">Adding the Checkout Complete view</span></span>

<span data-ttu-id="9353c-167">Wyewidencjonowanie pełnego widoku jest całkiem proste, po prostu potrzeba do wyświetlenia identyfikatora zamówienia.</span><span class="sxs-lookup"><span data-stu-id="9353c-167">The Checkout Complete view is pretty simple, as it just needs to display the Order ID.</span></span> <span data-ttu-id="9353c-168">Kliknij prawym przyciskiem myszy na akcję pełną kontrolera i dodać widok o nazwie zakończone, w którym zdecydowanie jest wpisane w formie typu int.</span><span class="sxs-lookup"><span data-stu-id="9353c-168">Right-click on the Complete controller action and add a view named Complete which is strongly typed as an int.</span></span>

![](mvc-music-store-part-9/_static/image8.png)

<span data-ttu-id="9353c-169">Teraz zaktualizujemy kod widok, aby wyświetlić identyfikator zamówienia, jak pokazano poniżej.</span><span class="sxs-lookup"><span data-stu-id="9353c-169">Now we will update the view code to display the Order ID, as shown below.</span></span>

[!code-cshtml[Main](mvc-music-store-part-9/samples/sample12.cshtml)]

## <a name="updating-the-error-view"></a><span data-ttu-id="9353c-170">Trwa aktualizowanie widoku błędów</span><span class="sxs-lookup"><span data-stu-id="9353c-170">Updating The Error view</span></span>

<span data-ttu-id="9353c-171">Szablon domyślny zawiera widoku błędów w folderze Widoki udostępnione, dzięki czemu może być ponownie używane gdzie indziej w lokacji.</span><span class="sxs-lookup"><span data-stu-id="9353c-171">The default template includes an Error view in the Shared views folder so that it can be re-used elsewhere in the site.</span></span> <span data-ttu-id="9353c-172">Ten widok błąd zawiera bardzo prosty błąd i nie używa naszą stronę układu, dlatego zaktualizujemy go.</span><span class="sxs-lookup"><span data-stu-id="9353c-172">This Error view contains a very simple error and doesn't use our site Layout, so we'll update it.</span></span>

<span data-ttu-id="9353c-173">Ponieważ jest to strona błąd rodzajowy, zawartość jest bardzo proste.</span><span class="sxs-lookup"><span data-stu-id="9353c-173">Since this is a generic error page, the content is very simple.</span></span> <span data-ttu-id="9353c-174">Firma Microsoft będzie obejmują wiadomość i łącze, aby przejść do poprzedniej strony w historii, jeśli użytkownik chce ponowić próbę ich działania.</span><span class="sxs-lookup"><span data-stu-id="9353c-174">We'll include a message and a link to navigate to the previous page in history if the user wants to re-try their action.</span></span>

[!code-cshtml[Main](mvc-music-store-part-9/samples/sample13.cshtml)]


> [!div class="step-by-step"]
> <span data-ttu-id="9353c-175">[Poprzednie](mvc-music-store-part-8.md)
> [dalej](mvc-music-store-part-10.md)</span><span class="sxs-lookup"><span data-stu-id="9353c-175">[Previous](mvc-music-store-part-8.md)
[Next](mvc-music-store-part-10.md)</span></span>
