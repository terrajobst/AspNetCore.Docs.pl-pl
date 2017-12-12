---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-10
title: "Część 10: Końcowe aktualizacje do nawigacji i lokacja, zawarcia | Dokumentacja firmy Microsoft"
author: jongalloway
description: "Ten samouczek serii zawiera szczegóły dotyczące wszystkich kroków tworzenia przykładowej aplikacji ASP.NET MVC utworów muzycznych magazynu. Część 10 obejmuje końcowe aktualizacje do nawigacji i S..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/21/2011
ms.topic: article
ms.assetid: 0c6e4c2f-fcdb-4978-9656-1990c6f15727
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-10
msc.type: authoredcontent
ms.openlocfilehash: af08039de2d810948b9ab64974111b0346c7fa0f
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
<a name="part-10-final-updates-to-navigation-and-site-design-conclusion"></a><span data-ttu-id="cb362-104">Część 10: Końcowe aktualizacje nawigacji i lokacja, zawarcia</span><span class="sxs-lookup"><span data-stu-id="cb362-104">Part 10: Final Updates to Navigation and Site Design, Conclusion</span></span>
====================
<span data-ttu-id="cb362-105">przez [Galloway Jan](https://github.com/jongalloway)</span><span class="sxs-lookup"><span data-stu-id="cb362-105">by [Jon Galloway](https://github.com/jongalloway)</span></span>

> <span data-ttu-id="cb362-106">Magazyn utworów muzycznych MVC jest samouczek aplikacji, które wprowadzono i opisano krok po kroku, jak używać do tworzenia aplikacji sieci web platformy ASP.NET MVC i Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="cb362-106">The MVC Music Store is a tutorial application that introduces and explains step-by-step how to use ASP.NET MVC and Visual Studio for web development.</span></span>  
>   
> <span data-ttu-id="cb362-107">Magazyn utworów muzycznych MVC jest implementacja magazynu lekkie próbki, co sprzedaje albumów muzycznych w trybie online i implementuje podstawowej witryny administracji, logowania użytkownika i funkcje koszyka zakupów.</span><span class="sxs-lookup"><span data-stu-id="cb362-107">The MVC Music Store is a lightweight sample store implementation which sells music albums online, and implements basic site administration, user sign-in, and shopping cart functionality.</span></span>  
>   
> <span data-ttu-id="cb362-108">Ten samouczek serii zawiera szczegóły dotyczące wszystkich kroków tworzenia przykładowej aplikacji ASP.NET MVC utworów muzycznych magazynu.</span><span class="sxs-lookup"><span data-stu-id="cb362-108">This tutorial series details all of the steps taken to build the ASP.NET MVC Music Store sample application.</span></span> <span data-ttu-id="cb362-109">Część 10 obejmuje końcowe aktualizacje do nawigacji i lokacja, zawarcia.</span><span class="sxs-lookup"><span data-stu-id="cb362-109">Part 10 covers Final Updates to Navigation and Site Design, Conclusion.</span></span>


<span data-ttu-id="cb362-110">Najważniejsze funkcje została ukończona dla witryny, ale wciąż istnieje niektóre funkcje do dodania do nawigacji w witrynie, strony głównej i strony Przeglądaj magazynu.</span><span class="sxs-lookup"><span data-stu-id="cb362-110">We've completed all the major functionality for our site, but we still have some features to add to the site navigation, the home page, and the Store Browse page.</span></span>

## <a name="creating-the-shopping-cart-summary-partial-view"></a><span data-ttu-id="cb362-111">Tworzenie zakupów Podsumowanie widoku częściowego koszyka</span><span class="sxs-lookup"><span data-stu-id="cb362-111">Creating the Shopping Cart Summary Partial View</span></span>

<span data-ttu-id="cb362-112">Chcemy ujawniać liczba elementów w koszyku użytkownika w całej lokacji.</span><span class="sxs-lookup"><span data-stu-id="cb362-112">We want to expose the number of items in the user's shopping cart across the entire site.</span></span>

![](mvc-music-store-part-10/_static/image1.png)

<span data-ttu-id="cb362-113">Firma Microsoft to łatwo zaimplementować przez utworzenie widoku częściowego, która jest dodawana do naszej Site.master.</span><span class="sxs-lookup"><span data-stu-id="cb362-113">We can easily implement this by creating a partial view which is added to our Site.master.</span></span>

<span data-ttu-id="cb362-114">Jak pokazano wcześniej, kontrolera ShoppingCart obejmuje CartSummary metody akcji, która zwraca widok częściowy:</span><span class="sxs-lookup"><span data-stu-id="cb362-114">As shown previously, the ShoppingCart controller includes a CartSummary action method which returns a partial view:</span></span>

[!code-csharp[Main](mvc-music-store-part-10/samples/sample1.cs)]

<span data-ttu-id="cb362-115">Aby utworzyć widok częściowy CartSummary, kliknij prawym przyciskiem folder widoków/ShoppingCart i wybierz polecenie Dodaj widok.</span><span class="sxs-lookup"><span data-stu-id="cb362-115">To create the CartSummary partial view, right-click on the Views/ShoppingCart folder and select Add View.</span></span> <span data-ttu-id="cb362-116">Nazwa widoku CartSummary i zaznacz pole wyboru "Utwórz widok częściowy", jak pokazano poniżej.</span><span class="sxs-lookup"><span data-stu-id="cb362-116">Name the view CartSummary and check the "Create a partial view" checkbox as shown below.</span></span>

![](mvc-music-store-part-10/_static/image2.png)

<span data-ttu-id="cb362-117">Widok częściowy CartSummary jest naprawdę proste — jest tylko łącze do widoku ShoppingCart indeksu, który zawiera liczbę elementów w koszyka.</span><span class="sxs-lookup"><span data-stu-id="cb362-117">The CartSummary partial view is really simple - it's just a link to the ShoppingCart Index view which shows the number of items in the cart.</span></span> <span data-ttu-id="cb362-118">Kompletny kod dla CartSummary.cshtml wygląda następująco:</span><span class="sxs-lookup"><span data-stu-id="cb362-118">The complete code for CartSummary.cshtml is as follows:</span></span>

[!code-cshtml[Main](mvc-music-store-part-10/samples/sample2.cshtml)]

<span data-ttu-id="cb362-119">Firma Microsoft może zawierać widoku częściowego w dowolnej strony w witrynie, w tym wzorcu lokacji przy użyciu metody Html.RenderAction.</span><span class="sxs-lookup"><span data-stu-id="cb362-119">We can include a partial view in any page in the site, including the Site master, by using the Html.RenderAction method.</span></span> <span data-ttu-id="cb362-120">RenderAction wymaga firmie Microsoft w celu określenia nazwy akcji ("CartSummary") i nazwy kontrolera ("ShoppingCart") jako poniżej.</span><span class="sxs-lookup"><span data-stu-id="cb362-120">RenderAction requires us to specify the Action Name ("CartSummary") and the Controller Name ("ShoppingCart") as below.</span></span>

[!code-cshtml[Main](mvc-music-store-part-10/samples/sample3.cshtml)]

<span data-ttu-id="cb362-121">Przed dodaniem to do lokacji układu, zostaną również utworzone Genre Menu, firma Microsoft może wprowadzać wszystkich naszych Site.master aktualizacji w tym samym czasie.</span><span class="sxs-lookup"><span data-stu-id="cb362-121">Before adding this to the site Layout, we will also create the Genre Menu so we can make all of our Site.master updates at one time.</span></span>

## <a name="creating-the-genre-menu-partial-view"></a><span data-ttu-id="cb362-122">Tworzenie widoku częściowego Menu Genre</span><span class="sxs-lookup"><span data-stu-id="cb362-122">Creating the Genre Menu Partial View</span></span>

<span data-ttu-id="cb362-123">Firma Microsoft może ułatwić znacznie naszych użytkowników poruszać się po magazynie przez dodanie Menu Genre, w której znajduje się lista wszystkich gatunki dostępne w naszym magazynie.</span><span class="sxs-lookup"><span data-stu-id="cb362-123">We can make it a lot easier for our users to navigate through the store by adding a Genre Menu which lists all the Genres available in our store.</span></span>

![](mvc-music-store-part-10/_static/image3.png)

<span data-ttu-id="cb362-124">Firma Microsoft będzie taka sama wykonaj kroki również utworzyć GenreMenu widoku częściowego, a następnie możemy Dodaj oba te do poziomu głównego witryny.</span><span class="sxs-lookup"><span data-stu-id="cb362-124">We will follow the same steps also create a GenreMenu partial view, and then we can add them both to the Site master.</span></span> <span data-ttu-id="cb362-125">Najpierw dodaj następujące akcji kontrolera GenreMenu do StoreController:</span><span class="sxs-lookup"><span data-stu-id="cb362-125">First, add the following GenreMenu controller action to the StoreController:</span></span>

[!code-csharp[Main](mvc-music-store-part-10/samples/sample4.cs)]

<span data-ttu-id="cb362-126">Ta akcja zwraca listę gatunkami muzyki, które będą wyświetlane przez widok częściowy, który zostanie utworzony obok.</span><span class="sxs-lookup"><span data-stu-id="cb362-126">This action returns a list of Genres which will be displayed by the partial view, which we will create next.</span></span>

<span data-ttu-id="cb362-127">*Uwaga: Dodano atrybut [ChildActionOnly] do tej akcji kontrolera, co oznacza, że chcemy tylko tę akcję do użycia w widoku częściowego. Ten atrybut uniemożliwi wstrzymywane przechodząc do /Store/GenreMenu akcji kontrolera. Nie jest to wymagane dla widoków częściowych, ale jest dobrym rozwiązaniem, ponieważ chcemy upewnić się, że nasze akcji kontrolera są używane jako mamy zamierzają. Możemy również zwróconego PartialView zamiast widoku, który umożliwia aparat widoku wiedzieć, że nie należy używać układu dla tego widoku, jest on dołączony w innych widokach.*</span><span class="sxs-lookup"><span data-stu-id="cb362-127">*Note: We have added the [ChildActionOnly] attribute to this controller action, which indicates that we only want this action to be used from a Partial View. This attribute will prevent the controller action from being executed by browsing to /Store/GenreMenu. This isn't required for partial views, but it is a good practice, since we want to make sure our controller actions are used as we intend. We are also returning PartialView rather than View, which lets the view engine know that it shouldn't use the Layout for this view, as it is being included in other views.*</span></span>

<span data-ttu-id="cb362-128">Kliknij prawym przyciskiem myszy na GenreMenu akcji kontrolera oraz tworzenia widoku częściowego o nazwie GenreMenu, który jest silnie typizowane za pomocą klasy Genre widoku danych, jak pokazano poniżej.</span><span class="sxs-lookup"><span data-stu-id="cb362-128">Right-click on the GenreMenu controller action and create a partial view named GenreMenu which is strongly typed using the Genre view data class as shown below.</span></span>

![](mvc-music-store-part-10/_static/image4.png)

<span data-ttu-id="cb362-129">Zaktualizuj kod widok GenreMenu widoku częściowego do wyświetlenia elementów przy użyciu nieuporządkowaną listę w następujący sposób.</span><span class="sxs-lookup"><span data-stu-id="cb362-129">Update the view code for the GenreMenu partial view to display the items using an unordered list as follows.</span></span>

[!code-cshtml[Main](mvc-music-store-part-10/samples/sample5.cshtml)]

## <a name="updating-site-layout-to-display-our-partial-views"></a><span data-ttu-id="cb362-130">Aktualizowanie lokacji układ do wyświetlenia naszych widoki częściowe</span><span class="sxs-lookup"><span data-stu-id="cb362-130">Updating Site Layout to display our Partial Views</span></span>

<span data-ttu-id="cb362-131">W układzie lokacji można dodać nasze widoki częściowe (/widoki/Shared/\_Layout.cshtml) przez wywołanie metody Html.RenderAction().</span><span class="sxs-lookup"><span data-stu-id="cb362-131">We can add our partial views to the Site Layout (/Views/Shared/\_Layout.cshtml) by calling Html.RenderAction().</span></span> <span data-ttu-id="cb362-132">Dodamy je w, a także pewne dodatkowe znaczników do wyświetlania, jak pokazano poniżej:</span><span class="sxs-lookup"><span data-stu-id="cb362-132">We'll add them both in, as well as some additional markup to display them, as shown below:</span></span>

[!code-cshtml[Main](mvc-music-store-part-10/samples/sample6.cshtml)]

<span data-ttu-id="cb362-133">Teraz uruchamianych aplikacji, zostanie wyświetlone Genre w obszarze nawigacji po lewej stronie i Podsumowanie koszyka u góry.</span><span class="sxs-lookup"><span data-stu-id="cb362-133">Now when we run the application, we will see the Genre in the left navigation area and the Cart Summary at the top.</span></span>

## <a name="update-to-the-store-browse-page"></a><span data-ttu-id="cb362-134">Aktualizowanie do magazynu Przeglądaj strony</span><span class="sxs-lookup"><span data-stu-id="cb362-134">Update to the Store Browse page</span></span>

<span data-ttu-id="cb362-135">Strona magazynu Przeglądaj będzie działać, ale wygląda bardzo dobre.</span><span class="sxs-lookup"><span data-stu-id="cb362-135">The Store Browse page is functional, but doesn't look very good.</span></span> <span data-ttu-id="cb362-136">Firma Microsoft może zaktualizować strony, aby pokazać albumów w układzie lepsze, aktualizując widoku kodu (znajdujący się w /Views/Store/Browse.cshtml) w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="cb362-136">We can update the page to show the albums in a better layout by updating the view code (found in /Views/Store/Browse.cshtml) as follows:</span></span>

[!code-cshtml[Main](mvc-music-store-part-10/samples/sample7.cshtml)]

<span data-ttu-id="cb362-137">W tym miejscu są czynione Użyj Url.Action zamiast Html.ActionLink, dzięki czemu można zastosować, specjalne formatowanie do łącza obejmują kompozycji albumu.</span><span class="sxs-lookup"><span data-stu-id="cb362-137">Here we are making use of Url.Action rather than Html.ActionLink so that we can apply special formatting to the link to include the album artwork.</span></span>

<span data-ttu-id="cb362-138">*Uwaga: Firma Microsoft wyświetlanie tytułowych albumu ogólnych, dla tych albumów. Te informacje są przechowywane w bazie danych i można edytować za pomocą Menedżera magazynu. Jesteś Zapraszamy dodać własne kompozycji.*</span><span class="sxs-lookup"><span data-stu-id="cb362-138">*Note: We are displaying a generic album cover for these albums. This information is stored in the database and is editable via the Store Manager. You are welcome to add your own artwork.*</span></span>

<span data-ttu-id="cb362-139">Teraz, gdy firma Microsoft przejdź do określonego rodzaju, zostanie wyświetlone albumów siatką z kompozycji albumu.</span><span class="sxs-lookup"><span data-stu-id="cb362-139">Now when we browse to a Genre, we will see the albums shown in a grid with the album artwork.</span></span>

![](mvc-music-store-part-10/_static/image5.png)

## <a name="updating-the-home-page-to-show-top-selling-albums"></a><span data-ttu-id="cb362-140">Aktualizowanie strony głównej pokazanie Top albumów sprzedaży</span><span class="sxs-lookup"><span data-stu-id="cb362-140">Updating the Home Page to show Top Selling Albums</span></span>

<span data-ttu-id="cb362-141">Chcemy funkcji naszych sprzedających albumów na stronie głównej zwiększenie sprzedaży.</span><span class="sxs-lookup"><span data-stu-id="cb362-141">We want to feature our top selling albums on the home page to increase sales.</span></span> <span data-ttu-id="cb362-142">Wybierzemy niektórych aktualizacji do naszej HomeController dojście, którego i dodać niektóre dodatkowe grafiki.</span><span class="sxs-lookup"><span data-stu-id="cb362-142">We'll make some updates to our HomeController to handle that, and add in some additional graphics as well.</span></span>

<span data-ttu-id="cb362-143">Najpierw dodamy właściwości nawigacji do naszej klasy albumu tak, aby EntityFramework wie, że są powiązane.</span><span class="sxs-lookup"><span data-stu-id="cb362-143">First, we'll add a navigation property to our Album class so that EntityFramework knows that they're associated.</span></span> <span data-ttu-id="cb362-144">Kilka ostatnich wierszy z naszych **albumu** klasa powinna wyglądać następująco:</span><span class="sxs-lookup"><span data-stu-id="cb362-144">The last few lines of our **Album** class should now look like this:</span></span>

[!code-csharp[Main](mvc-music-store-part-10/samples/sample8.cs)]

<span data-ttu-id="cb362-145">*Uwaga: Wymaga to dodawanie przy użyciu instrukcji przenieść system.Collections.Generic — przestrzeń nazw.*</span><span class="sxs-lookup"><span data-stu-id="cb362-145">*Note: This will require adding a using statement to bring in the System.Collections.Generic namespace.*</span></span>

<span data-ttu-id="cb362-146">Najpierw dodamy pola storeDB i MvcMusicStore.Models przy użyciu instrukcje, jak naszym kontrolerów.</span><span class="sxs-lookup"><span data-stu-id="cb362-146">First, we'll add a storeDB field and the MvcMusicStore.Models using statements, as in our other controllers.</span></span> <span data-ttu-id="cb362-147">Następnie dodamy następującą metodę do HomeController, który wysyła zapytanie do naszej bazie danych można znaleźć górny albumów sprzedaży według SzczegółyZamówień.</span><span class="sxs-lookup"><span data-stu-id="cb362-147">Next, we'll add the following method to the HomeController which queries our database to find top selling albums according to OrderDetails.</span></span>

[!code-csharp[Main](mvc-music-store-part-10/samples/sample9.cs)]

<span data-ttu-id="cb362-148">Jest to metoda prywatna, ponieważ nie chcemy udostępnić go jako akcji kontrolera.</span><span class="sxs-lookup"><span data-stu-id="cb362-148">This is a private method, since we don't want to make it available as a controller action.</span></span> <span data-ttu-id="cb362-149">Firma Microsoft zawierają w HomeController dla uproszczenia, ale zaleca się przenieść logiki biznesowej do klasy oddzielne usługi zgodnie z potrzebami.</span><span class="sxs-lookup"><span data-stu-id="cb362-149">We are including it in the HomeController for simplicity, but you are encouraged to move your business logic into separate service classes as appropriate.</span></span>

<span data-ttu-id="cb362-150">Z tym w miejscu możemy zaktualizować akcji kontrolera indeksu wyszukiwania 5 najlepiej sprzedających albumów i zwraca je do widoku.</span><span class="sxs-lookup"><span data-stu-id="cb362-150">With that in place, we can update the Index controller action to query the top 5 selling albums and return them to the view.</span></span>

[!code-csharp[Main](mvc-music-store-part-10/samples/sample10.cs)]

<span data-ttu-id="cb362-151">Kompletny kod dla zaktualizowanej HomeController jest, jak pokazano poniżej.</span><span class="sxs-lookup"><span data-stu-id="cb362-151">The complete code for the updated HomeController is as shown below.</span></span>

[!code-csharp[Main](mvc-music-store-part-10/samples/sample11.cs)]

<span data-ttu-id="cb362-152">Na koniec musimy zaktualizować naszych widoku indeksu Narzędzia główne, tak, aby go wyświetlić listę albumów aktualizowanie typu modelu i dodawania listy albumu w dół.</span><span class="sxs-lookup"><span data-stu-id="cb362-152">Finally, we'll need to update our Home Index view so that it can display a list of albums by updating the Model type and adding the album list to the bottom.</span></span> <span data-ttu-id="cb362-153">Teraz nastąpi przekierowanie do tej możliwości, aby również dodać nagłówek i sekcja podwyższanie poziomu do strony.</span><span class="sxs-lookup"><span data-stu-id="cb362-153">We will take this opportunity to also add a heading and a promotion section to the page.</span></span>

[!code-cshtml[Main](mvc-music-store-part-10/samples/sample12.cshtml)]

<span data-ttu-id="cb362-154">Uruchamianych aplikacji, firma Microsoft będzie widoczny naszych zaktualizowanej strony głównej z najwyższym albumów sprzedaży i promocyjne wiadomości.</span><span class="sxs-lookup"><span data-stu-id="cb362-154">Now when we run the application, we'll see our updated home page with top selling albums and our promotional message.</span></span>

![](mvc-music-store-part-10/_static/image1.jpg)

## <a name="conclusion"></a><span data-ttu-id="cb362-155">Wniosek</span><span class="sxs-lookup"><span data-stu-id="cb362-155">Conclusion</span></span>

<span data-ttu-id="cb362-156">Firma Microsoft w tym samouczku czy tej platformy ASP.NET MVC ułatwia można tworzyć zaawansowane witryny sieci Web z dostępem do bazy danych, członkostwa, AJAX, itp.</span><span class="sxs-lookup"><span data-stu-id="cb362-156">We've seen that that ASP.NET MVC makes it easy to create a sophisticated website with database access, membership, AJAX, etc.</span></span> <span data-ttu-id="cb362-157">bardzo szybko.</span><span class="sxs-lookup"><span data-stu-id="cb362-157">pretty quickly.</span></span> <span data-ttu-id="cb362-158">Miejmy nadzieję, że w tym samouczku przyznał Ci narzędzia, które należy rozpocząć tworzenie własnych ASP.NET MVC aplikacji!</span><span class="sxs-lookup"><span data-stu-id="cb362-158">Hopefully this tutorial has given you the tools you need to get started building your own ASP.NET MVC applications!</span></span>


>[!div class="step-by-step"]
[<span data-ttu-id="cb362-159">Poprzednie</span><span class="sxs-lookup"><span data-stu-id="cb362-159">Previous</span></span>](mvc-music-store-part-9.md)
