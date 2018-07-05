---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-7
title: 'Część 7: Dodawanie funkcji | Dokumentacja firmy Microsoft'
author: JoeStagner
description: W tej serii samouczków zawiera szczegóły wszystkich kroków kompilacji Przykładowa aplikacja Tailspin Spyworks. Część 7 dodaje dodatkowe funkcje, takie jak okienko konta...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/21/2010
ms.topic: article
ms.assetid: 50223ee9-11b9-4cf3-bca2-e2f10bf471f3
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-7
msc.type: authoredcontent
ms.openlocfilehash: 8cdde10981835877e5ac2f65860010920a68d0a2
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37389179"
---
<a name="part-7-adding-features"></a><span data-ttu-id="4e718-104">Część 7: Dodawanie funkcji</span><span class="sxs-lookup"><span data-stu-id="4e718-104">Part 7: Adding Features</span></span>
====================
<span data-ttu-id="4e718-105">przez [Stagner Jan](https://github.com/JoeStagner)</span><span class="sxs-lookup"><span data-stu-id="4e718-105">by [Joe Stagner](https://github.com/JoeStagner)</span></span>

> <span data-ttu-id="4e718-106">Tailspin Spyworks pokazuje, jak bardzo łatwo jest tworzyć zaawansowane, skalowalne aplikacje dla platformy .NET.</span><span class="sxs-lookup"><span data-stu-id="4e718-106">Tailspin Spyworks demonstrates how extraordinarily simple it is to create powerful, scalable applications for the .NET platform.</span></span> <span data-ttu-id="4e718-107">Przedstawia on poza sposób użycia wspaniałych nowych funkcjach w ASP.NET 4 do tworzenia sklep online, m.in. zakupy wyewidencjonowanie i Administracja.</span><span class="sxs-lookup"><span data-stu-id="4e718-107">It shows off how to use the great new features in ASP.NET 4 to build an online store, including shopping, checkout, and administration.</span></span>
> 
> <span data-ttu-id="4e718-108">W tej serii samouczków zawiera szczegóły wszystkich kroków kompilacji Przykładowa aplikacja Tailspin Spyworks.</span><span class="sxs-lookup"><span data-stu-id="4e718-108">This tutorial series details all of the steps taken to build the Tailspin Spyworks sample application.</span></span> <span data-ttu-id="4e718-109">Część 7 dodaje dodatkowe funkcje, takie jak konta przeglądu, recenzje produktów i "Najpopularniejsze pozycje" i formanty użytkownika "również zakupione".</span><span class="sxs-lookup"><span data-stu-id="4e718-109">Part 7 adds additional features, such as account review, product reviews, and "popular items" and "also purchased" user controls.</span></span>


## <a id="_Toc260221673"></a>  <span data-ttu-id="4e718-110">Dodawanie funkcji</span><span class="sxs-lookup"><span data-stu-id="4e718-110">Adding Features</span></span>

<span data-ttu-id="4e718-111">Chociaż użytkownicy mogą przeglądać naszego wykazu, umieść elementy w swoich koszyka zakupów i ukończenie procesu realizowania zamówienia, istnieje szereg obsługi funkcji, że firma Microsoft dołącza do udoskonalać naszą witrynę.</span><span class="sxs-lookup"><span data-stu-id="4e718-111">Though users can browse our catalog, place items in their shopping cart, and complete the checkout process, there are a number of supporting features that we will include to improve our site.</span></span>

1. <span data-ttu-id="4e718-112">Przegląd konta (Lista zamówień umieszczone i wyświetlić szczegółowe informacje).</span><span class="sxs-lookup"><span data-stu-id="4e718-112">Account Review (List orders placed and view details.)</span></span>
2. <span data-ttu-id="4e718-113">Dodaj część zawartości określonego kontekstu do pierwszej strony.</span><span class="sxs-lookup"><span data-stu-id="4e718-113">Add some context specific content to the front page.</span></span>
3. <span data-ttu-id="4e718-114">Dodaj funkcję, aby umożliwić użytkownikom przeglądanie produktów, w katalogu.</span><span class="sxs-lookup"><span data-stu-id="4e718-114">Add a feature to let users Review the products in the catalog.</span></span>
4. <span data-ttu-id="4e718-115">Tworzenie kontrolki użytkownika do wyświetlania na pierwszej stronie Najpopularniejsze pozycje i miejsce, które kontrolują.</span><span class="sxs-lookup"><span data-stu-id="4e718-115">Create a User Control to display Popular Items and Place that control on the front page.</span></span>
5. <span data-ttu-id="4e718-116">Tworzenie kontrolki użytkownika "Również zakupione" i dodaj go do strony szczegółów produktu.</span><span class="sxs-lookup"><span data-stu-id="4e718-116">Create an "Also Purchased" user control and add it to the product details page.</span></span>
6. <span data-ttu-id="4e718-117">Dodaj kontakt strony.</span><span class="sxs-lookup"><span data-stu-id="4e718-117">Add a Contact Page.</span></span>
7. <span data-ttu-id="4e718-118">Dodaj informacje o stronie.</span><span class="sxs-lookup"><span data-stu-id="4e718-118">Add an About Page.</span></span>
8. <span data-ttu-id="4e718-119">Błąd globalne</span><span class="sxs-lookup"><span data-stu-id="4e718-119">Global Error</span></span>

## <a id="_Toc260221674"></a>  <span data-ttu-id="4e718-120">Przegląd konta</span><span class="sxs-lookup"><span data-stu-id="4e718-120">Account Review</span></span>

<span data-ttu-id="4e718-121">W folderze "Konto" Utwórz dwie strony .aspx, jeden o nazwie OrderList.aspx i nazwanych OrderDetails.aspx</span><span class="sxs-lookup"><span data-stu-id="4e718-121">In the "Account" folder create two .aspx pages one named OrderList.aspx and the other named OrderDetails.aspx</span></span>

<span data-ttu-id="4e718-122">OrderList.aspx będzie korzystać z kontrolki GridView i EntityDataSoure, ile mamy wcześniej.</span><span class="sxs-lookup"><span data-stu-id="4e718-122">OrderList.aspx will leverage the GridView and EntityDataSoure controls much as we have previously.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample1.aspx)]

<span data-ttu-id="4e718-123">EntityDataSoure wybiera rekordy z tabeli Orders filtrowane według nazwy użytkownika (zobacz WhereParameter), które możemy ustawić w zmiennej sesji użytkownikowi logowanie użytkownika.</span><span class="sxs-lookup"><span data-stu-id="4e718-123">The EntityDataSoure selects records from the Orders table filtered on the UserName (see the WhereParameter) which we set in a session variable when the user log's in.</span></span>

<span data-ttu-id="4e718-124">Należy pamiętać, również tych parametrów w pole hiperłącza HyperlinkField GridView:</span><span class="sxs-lookup"><span data-stu-id="4e718-124">Note also these parameters in the HyperlinkField of the GridView:</span></span>

[!code-xml[Main](tailspin-spyworks-part-7/samples/sample2.xml)]

<span data-ttu-id="4e718-125">Określają one łącze do widoku szczegółów zamówienia dla każdego produktu, określając pole OrderID jako parametr QueryString na stronie OrderDetails.aspx.</span><span class="sxs-lookup"><span data-stu-id="4e718-125">These specify the link to the Order details view for each product specifying the OrderID field as a QueryString parameter to the OrderDetails.aspx page.</span></span>

## <a id="_Toc260221675"></a>  <span data-ttu-id="4e718-126">OrderDetails.aspx</span><span class="sxs-lookup"><span data-stu-id="4e718-126">OrderDetails.aspx</span></span>

<span data-ttu-id="4e718-127">Użyjemy EntityDataSource kontroli dostępu do zamówienia i FormView wyświetlane dane zamówień i innym EntityDataSource przy użyciu GridView do wyświetlenia wszystkich kolejności pozycji.</span><span class="sxs-lookup"><span data-stu-id="4e718-127">We will use an EntityDataSource control to access the Orders and a FormView to display the Order data and another EntityDataSource with a GridView to display all the Order's line items.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample3.aspx)]

<span data-ttu-id="4e718-128">W pliku kodu za (OrderDetails.aspx.cs) mamy dwa bity nieco celów.</span><span class="sxs-lookup"><span data-stu-id="4e718-128">In the Code Behind file (OrderDetails.aspx.cs) we have two little bits of housekeeping.</span></span>

<span data-ttu-id="4e718-129">Najpierw musimy upewnić się, że OrderDetails zawsze pobiera OrderId.</span><span class="sxs-lookup"><span data-stu-id="4e718-129">First we need to make sure that OrderDetails always gets an OrderId.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample4.cs)]

<span data-ttu-id="4e718-130">Musimy również obliczają i wyświetlają kolejności całkowitej z elementów wiersza.</span><span class="sxs-lookup"><span data-stu-id="4e718-130">We also need to calculate and display the order total from the line items.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample5.cs)]

## <a id="_Toc260221676"></a>  <span data-ttu-id="4e718-131">Strona główna</span><span class="sxs-lookup"><span data-stu-id="4e718-131">The Home Page</span></span>

<span data-ttu-id="4e718-132">Dodajmy zawartości statycznej do strony Default.aspx.</span><span class="sxs-lookup"><span data-stu-id="4e718-132">Let's add some static content to the Default.aspx page.</span></span>

<span data-ttu-id="4e718-133">Najpierw będzie utworzyć folderu "Zawartość" i wewnątrz tego folderu obrazów (i będzie umieścić obraz ma być używany na stronie głównej)</span><span class="sxs-lookup"><span data-stu-id="4e718-133">First I'll create a "Content" folder and within it an Images folder (and I'll include an image to be used on the home page.)</span></span>

<span data-ttu-id="4e718-134">Do symbolu zastępczego dolnej części strony Default.aspx Dodaj następujący kod znaczników.</span><span class="sxs-lookup"><span data-stu-id="4e718-134">Into the bottom placeholder of the Default.aspx page, add the following markup.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample6.aspx)]

## <a id="_Toc260221677"></a>  <span data-ttu-id="4e718-135">Recenzje produktów</span><span class="sxs-lookup"><span data-stu-id="4e718-135">Product Reviews</span></span>

<span data-ttu-id="4e718-136">Najpierw dodamy przycisk za pomocą łącza do formularza, które firma Microsoft może służyć do wprowadzania Recenzja produktu.</span><span class="sxs-lookup"><span data-stu-id="4e718-136">First we'll add a button with a link to a form that we can use to enter a product review.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample7.aspx)]

![](tailspin-spyworks-part-7/_static/image1.jpg)

<span data-ttu-id="4e718-137">Należy pamiętać, firma Microsoft przekazywanie ProductID w ciągu zapytania</span><span class="sxs-lookup"><span data-stu-id="4e718-137">Note that we are passing the ProductID in the query string</span></span>

<span data-ttu-id="4e718-138">Dalej Dodajmy stronę o nazwie ReviewAdd.aspx</span><span class="sxs-lookup"><span data-stu-id="4e718-138">Next let's add page named ReviewAdd.aspx</span></span>

<span data-ttu-id="4e718-139">Ta strona będzie używać ASP.NET AJAX Control Toolkit.</span><span class="sxs-lookup"><span data-stu-id="4e718-139">This page will use the ASP.NET AJAX Control Toolkit.</span></span> <span data-ttu-id="4e718-140">Jeśli użytkownik jeszcze tego nie zrobiono, aby pobrać go z [DevExpress](http://devexpress.com/act) i wskazówki dotyczące konfigurowania zestawu narzędzi do użytku z programem Visual Studio tutaj [ https://www.asp.net/learn/ajax-videos/video-76.aspx ](../../../videos/ajax-control-toolkit/how-do-i-get-started-with-the-aspnet-ajax-control-toolkit.md).</span><span class="sxs-lookup"><span data-stu-id="4e718-140">If you have not already done so you can download it from [DevExpress](http://devexpress.com/act) and there is guidance on setting up the toolkit for use with Visual Studio here [https://www.asp.net/learn/ajax-videos/video-76.aspx](../../../videos/ajax-control-toolkit/how-do-i-get-started-with-the-aspnet-ajax-control-toolkit.md).</span></span>

<span data-ttu-id="4e718-141">W trybie projektowania przeciągnij formanty i modułów weryfikacji z przybornika i budowania formularza podobny do poniższego.</span><span class="sxs-lookup"><span data-stu-id="4e718-141">In design mode, drag controls and validators from the toolbox and build a form like the one below.</span></span>

![](tailspin-spyworks-part-7/_static/image2.jpg)

<span data-ttu-id="4e718-142">Znaczniki będą wyglądać mniej więcej tak.</span><span class="sxs-lookup"><span data-stu-id="4e718-142">The markup will look something like this.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample8.aspx)]

<span data-ttu-id="4e718-143">Teraz, że możemy wprowadzić recenzje, pozwala wyświetlać te przeglądy na stronie produktu.</span><span class="sxs-lookup"><span data-stu-id="4e718-143">Now that we can enter reviews, lets display those reviews on the product page.</span></span>

<span data-ttu-id="4e718-144">Na stronie ProductDetails.aspx, Dodaj ten kod znaczników.</span><span class="sxs-lookup"><span data-stu-id="4e718-144">Add this markup to the ProductDetails.aspx page.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample9.aspx)]

<span data-ttu-id="4e718-145">Uruchomione obecnie nasza aplikacja i przechodząc do produktu zawiera informacje o produkcie, łącznie z przeglądy wykonywane przez klientów.</span><span class="sxs-lookup"><span data-stu-id="4e718-145">Running our application now and navigating to a product shows the product information including customer reviews.</span></span>

![](tailspin-spyworks-part-7/_static/image3.jpg)

## <a id="_Toc260221678"></a>  <span data-ttu-id="4e718-146">Najpopularniejsze pozycje kontroli (Tworzenie kontrolki użytkownika)</span><span class="sxs-lookup"><span data-stu-id="4e718-146">Popular Items Control (Creating User Controls)</span></span>

<span data-ttu-id="4e718-147">Aby zwiększyć sprzedaż w witrynie sieci web dodamy kilka funkcji "dwuznaczne sell" popularnych lub powiązanych produktów.</span><span class="sxs-lookup"><span data-stu-id="4e718-147">In order to increase sales on your web site we will add a couple of features to "suggestive sell" popular or related products.</span></span>

<span data-ttu-id="4e718-148">Pierwsza z tych funkcji będzie lista więcej popularnych produktów w naszym katalogu produktów.</span><span class="sxs-lookup"><span data-stu-id="4e718-148">The first of these features will be a list of the more popular product in our product catalog.</span></span>

<span data-ttu-id="4e718-149">Utworzymy "Kontrolkę użytkownika" Aby wyświetlić najlepiej sprzedające elementów na stronie głównej naszej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="4e718-149">We will create a "User Control" to display the top selling items on the home page of our application.</span></span> <span data-ttu-id="4e718-150">Ponieważ są to kontrolki, możemy użyć go na dowolnej stronie usługi, po prostu przeciąganie i upuszczanie formantu w projektancie programu Visual Studio na każdej stronie, które firma Microsoft, takich jak.</span><span class="sxs-lookup"><span data-stu-id="4e718-150">Since this will be a control, we can use it on any page by simply dragging and dropping the control in Visual Studio's designer onto any page that we like.</span></span>

<span data-ttu-id="4e718-151">W Eksploratorze rozwiązań programu Visual Studio kliknij prawym przyciskiem myszy nazwę rozwiązania, a następnie utwórz nowy katalog o nazwie "Controls".</span><span class="sxs-lookup"><span data-stu-id="4e718-151">In Visual Studio's solutions explorer, right-click on the solution name and create a new directory named "Controls".</span></span> <span data-ttu-id="4e718-152">Mimo że nie jest to konieczne to zrobić, firma Microsoft ułatwi naszego projektu, tworząc naszych kontrolki użytkownika w katalogu "Controls".</span><span class="sxs-lookup"><span data-stu-id="4e718-152">While it is not necessary to do so, we will help keep our project organized by creating all our user controls in the "Controls" directory.</span></span>

<span data-ttu-id="4e718-153">Kliknij prawym przyciskiem myszy w folderze kontroli i wybierz pozycję "Nowy element":</span><span class="sxs-lookup"><span data-stu-id="4e718-153">Right-click on the controls folder and choose "New Item" :</span></span>

![](tailspin-spyworks-part-7/_static/image4.jpg)

<span data-ttu-id="4e718-154">Określ nazwę dla naszych formantu "PopularItems".</span><span class="sxs-lookup"><span data-stu-id="4e718-154">Specify a name for our control of "PopularItems".</span></span> <span data-ttu-id="4e718-155">Należy pamiętać, że rozszerzenie pliku, w przypadku kontrolek użytkownika .ascx nie .aspx.</span><span class="sxs-lookup"><span data-stu-id="4e718-155">Note that the file extension for user controls is .ascx not .aspx.</span></span>

<span data-ttu-id="4e718-156">Nasze kontrolki popularne użytkownika elementy będą zdefiniowane w następujący sposób.</span><span class="sxs-lookup"><span data-stu-id="4e718-156">Our Popular Items User control will be defined as follows.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample10.aspx)]

<span data-ttu-id="4e718-157">Tutaj używamy metody, które firma Microsoft nie ma jeszcze używane w tej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="4e718-157">Here we're using a method we have not used yet in this application.</span></span> <span data-ttu-id="4e718-158">Używamy kontrolce elementu powtarzanego i zamiast korzystać z kontroli źródła danych możemy konieczności utworzenia powiązania w kontrolce elementu powtarzanego z wynikami zapytaniu składnika LINQ to Entities.</span><span class="sxs-lookup"><span data-stu-id="4e718-158">We're using the repeater control and instead of using a data source control we're binding the Repeater Control to the results of a LINQ to Entities query.</span></span>

<span data-ttu-id="4e718-159">W kodzie naszej kontroli czynność tę możemy wykonać w następujący sposób.</span><span class="sxs-lookup"><span data-stu-id="4e718-159">In the code behind of our control we do that as follows.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample11.cs)]

<span data-ttu-id="4e718-160">Należy też zauważyć ten wiersz ważne w górnej części naszych kontroli znaczników.</span><span class="sxs-lookup"><span data-stu-id="4e718-160">Note also this important line at the top of our control's markup.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample12.aspx)]

<span data-ttu-id="4e718-161">Ponieważ najpopularniejszych elementów, nie będzie zmiana na podstawie minutę na minutę możemy dodać dyrektywę bóle, aby poprawić wydajność aplikacji.</span><span class="sxs-lookup"><span data-stu-id="4e718-161">Since the most popular items won't be changing on a minute to minute basis we can add a aching directive to improve the performance of our application.</span></span> <span data-ttu-id="4e718-162">Ta dyrektywa spowoduje, że kod formantów, aby wykonać tylko po wygaśnięciu pamięci podręcznej danych wyjściowych formantu.</span><span class="sxs-lookup"><span data-stu-id="4e718-162">This directive will cause the controls code to only be executed when the cached output of the control expires.</span></span> <span data-ttu-id="4e718-163">W przeciwnym razie będą używane w pamięci podręcznej wersji wyjście formantu.</span><span class="sxs-lookup"><span data-stu-id="4e718-163">Otherwise, the cached version of the control's output will be used.</span></span>

<span data-ttu-id="4e718-164">Teraz musimy to zrobić będzie zawierać naszej nowej kontrolki na naszej stronie Default.aspc.</span><span class="sxs-lookup"><span data-stu-id="4e718-164">Now all we have to do is include our new control in our Default.aspc page.</span></span>

<span data-ttu-id="4e718-165">Użyj przeciągnij i upuść do umieszczenia wystąpienie formantu w kolumnie Otwórz naszych domyślnego formularza.</span><span class="sxs-lookup"><span data-stu-id="4e718-165">Use drag and drop to place an instance of the control in the open column of our Default form.</span></span>

![](tailspin-spyworks-part-7/_static/image5.jpg)

<span data-ttu-id="4e718-166">Firma Microsoft uruchamiania naszej aplikacji na stronie głównej wyświetli teraz najpopularniejszych elementów.</span><span class="sxs-lookup"><span data-stu-id="4e718-166">Now when we run our application the home page displays the most popular items.</span></span>

![](tailspin-spyworks-part-7/_static/image6.jpg)

## <a id="_Toc260221679"></a>  <span data-ttu-id="4e718-167">"Również zakupione" sterowania (kontrolek użytkownika za pomocą parametrów)</span><span class="sxs-lookup"><span data-stu-id="4e718-167">"Also Purchased" Control (User Controls with Parameters)</span></span>

<span data-ttu-id="4e718-168">Drugi formant użytkownika, który utworzymy potrwa dwuznaczne sprzedaży na wyższy poziom, dodając kontekstu szczegółowością.</span><span class="sxs-lookup"><span data-stu-id="4e718-168">The second User Control that we'll create will take suggestive selling to the next level by adding context specificity.</span></span>

<span data-ttu-id="4e718-169">Logiki można obliczyć pierwszych elementów "Również zakupione" jest trywialny.</span><span class="sxs-lookup"><span data-stu-id="4e718-169">The logic to calculate the top "Also Purchased" items is non-trivial.</span></span>

<span data-ttu-id="4e718-170">Mamy kontroli "Również zakupione" będzie wybrać rekordy OrderDetails (wcześniej kupił) dla aktualnie wybranego elementu ProductID i Pobierz OrderIDs dla każdego zamówienia unikatowy, która znajduje się.</span><span class="sxs-lookup"><span data-stu-id="4e718-170">Our "Also Purchased" control will select the OrderDetails records (previously purchased) for the currently selected ProductID and grab the OrderIDs for each unique order that is found.</span></span>

<span data-ttu-id="4e718-171">Następnie firma Microsoft będzie wybrać al produkty z tych zamówień i sumę ilości zakupu.</span><span class="sxs-lookup"><span data-stu-id="4e718-171">Then we will select al the products from all those Orders and sum the quantities purchased.</span></span> <span data-ttu-id="4e718-172">Utworzymy posortować produkty według tej sumy liczby i wyświetlić pięć pierwszych elementów.</span><span class="sxs-lookup"><span data-stu-id="4e718-172">We'll sort the products by that quantity sum and display the top five items.</span></span>

<span data-ttu-id="4e718-173">Biorąc pod uwagę złożoność tę logikę, wprowadzimy ten algorytm jako procedurę przechowywaną.</span><span class="sxs-lookup"><span data-stu-id="4e718-173">Given the complexity of this logic, we will implement this algorithm as a stored procedure.</span></span>

<span data-ttu-id="4e718-174">T-SQL dla procedury składowanej jest w następujący sposób.</span><span class="sxs-lookup"><span data-stu-id="4e718-174">The T-SQL for the stored procedure is as follows.</span></span>

[!code-sql[Main](tailspin-spyworks-part-7/samples/sample13.sql)]

<span data-ttu-id="4e718-175">Należy pamiętać, że tej procedury składowanej (SelectPurchasedWithProducts) istniał w bazie danych podczas dołączyliśmy w naszej aplikacji i możemy generowane określonej, oprócz tabele i widoki, które Musieliśmy, modelu Entity Data Model modelu Entity Data Model powinien zawierać tę procedurę składowaną.</span><span class="sxs-lookup"><span data-stu-id="4e718-175">Note that this stored procedure (SelectPurchasedWithProducts) existed in the database when we included it in our application and when we generated the Entity Data Model we specified that, in addition to the Tables and Views that we needed, the Entity Data Model should include this stored procedure.</span></span>

<span data-ttu-id="4e718-176">Dostęp do procedury składowanej z modelu Entity Data Model należy zaimportować funkcji.</span><span class="sxs-lookup"><span data-stu-id="4e718-176">To access the stored procedure from the Entity Data Model we need to import the function.</span></span>

<span data-ttu-id="4e718-177">Kliknij dwukrotnie modelu Entity Data Model w Eksploratorze rozwiązań, aby otworzyć go w Projektancie i Otwórz przeglądarkę modelu, a następnie prawym przyciskiem myszy projektanta i wybierz pozycję "Dodaj funkcję Import".</span><span class="sxs-lookup"><span data-stu-id="4e718-177">Double Click on the Entity Data Model in the Solutions Explorer to open it in the designer and open the Model Browser, then right-click in the designer and select "Add Function Import".</span></span>

![](tailspin-spyworks-part-7/_static/image1.png)

<span data-ttu-id="4e718-178">To spowoduje otwarcie tego okna dialogowego.</span><span class="sxs-lookup"><span data-stu-id="4e718-178">Doing so will open this dialog.</span></span>

![](tailspin-spyworks-part-7/_static/image2.png)

<span data-ttu-id="4e718-179">Wypełnij pola jak pokazano powyżej, wybierając "SelectPurchasedWithProducts", a następnie użyj nazwy procedury nazwa naszej funkcji zaimportowane.</span><span class="sxs-lookup"><span data-stu-id="4e718-179">Fill out the fields as you see above, selecting the "SelectPurchasedWithProducts" and use the procedure name for the name of our imported function.</span></span>

<span data-ttu-id="4e718-180">Kliknij przycisk "Ok".</span><span class="sxs-lookup"><span data-stu-id="4e718-180">Click "Ok".</span></span>

<span data-ttu-id="4e718-181">To to, którą firma Microsoft może po prostu programować przy użyciu procedury składowanej, ponieważ firma Microsoft może być inny element w modelu.</span><span class="sxs-lookup"><span data-stu-id="4e718-181">Having done this we can simply program against the stored procedure as we might any other item in the model.</span></span>

<span data-ttu-id="4e718-182">Tak w folderze naszych "Controls" Utwórz nową kontrolkę użytkownika o nazwie AlsoPurchased.ascx.</span><span class="sxs-lookup"><span data-stu-id="4e718-182">So, in our "Controls" folder create a new user control named AlsoPurchased.ascx.</span></span>

<span data-ttu-id="4e718-183">Kod znaczników dla tej kontrolki będzie wyglądać bardzo podobnie do kontroli PopularItems.</span><span class="sxs-lookup"><span data-stu-id="4e718-183">The markup for this control will look very familiar to the PopularItems control.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample14.aspx)]

<span data-ttu-id="4e718-184">Godne uwagi różnica polega na tym, które są nie pamięci podręcznej danych wyjściowych od czasu do renderowania elementu różnią się według produktu.</span><span class="sxs-lookup"><span data-stu-id="4e718-184">The notable difference is that are not caching the output since the item's to be rendered will differ by product.</span></span>

<span data-ttu-id="4e718-185">Identyfikator produktu będzie "property" do formantu.</span><span class="sxs-lookup"><span data-stu-id="4e718-185">The ProductId will be a "property" to the control.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample15.cs)]

<span data-ttu-id="4e718-186">W obsłudze zdarzenie PreRender kontrolki firma Microsoft eed, aby wykonać trzy czynności.</span><span class="sxs-lookup"><span data-stu-id="4e718-186">In the control's PreRender event handler we eed to do three things.</span></span>

1. <span data-ttu-id="4e718-187">Upewnij się, że ustawiono ProductID.</span><span class="sxs-lookup"><span data-stu-id="4e718-187">Make sure the ProductID is set.</span></span>
2. <span data-ttu-id="4e718-188">Zobacz, czy wszystkie produkty, w których została zakupiona z bieżącej subskrypcji.</span><span class="sxs-lookup"><span data-stu-id="4e718-188">See if there are any products that have been purchased with the current one.</span></span>
3. <span data-ttu-id="4e718-189">Dane wyjściowe niektóre elementy, jak określono w #2.</span><span class="sxs-lookup"><span data-stu-id="4e718-189">Output some items as determined in #2.</span></span>

<span data-ttu-id="4e718-190">Należy pamiętać o tym, jak łatwo jest wywołanie procedury składowanej za pomocą modelu.</span><span class="sxs-lookup"><span data-stu-id="4e718-190">Note how easy it is to call the stored procedure through the model.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample16.cs)]

<span data-ttu-id="4e718-191">Po ustaleniu, które są "również zakupione" możemy po prostu powiązać powtarzanego wyników zwróconych przez zapytanie.</span><span class="sxs-lookup"><span data-stu-id="4e718-191">After determining that there ARE "also purchased" we can simply bind the repeater to the results returned by the query.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample17.cs)]

<span data-ttu-id="4e718-192">Jakby nie było żadnych elementów "również zakupione" będzie wyświetlana po prostu innych popularnych elementów z naszego wykazu.</span><span class="sxs-lookup"><span data-stu-id="4e718-192">If there were not any "also purchased" items we'll simply display other popular items from our catalog.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample18.cs)]

<span data-ttu-id="4e718-193">Aby wyświetlić elementy "Również zakupione", otwórz stronę ProductDetails.aspx, a następnie przeciągnij formant AlsoPurchased z poziomu Eksploratora rozwiązań, aby była ona wyświetlana w tym miejscu w znaczniku.</span><span class="sxs-lookup"><span data-stu-id="4e718-193">To view the "Also Purchased" items, open the ProductDetails.aspx page and drag the AlsoPurchased control from the Solutions Explorer so that it appears in this position in the markup.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample19.aspx)]

<span data-ttu-id="4e718-194">Ten sposób utworzyć odwołanie do formantu w górnej części strony ProductDetails.</span><span class="sxs-lookup"><span data-stu-id="4e718-194">Doing so will create a reference to the control at the top of the ProductDetails page.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample20.aspx)]

<span data-ttu-id="4e718-195">Ponieważ AlsoPurchased kontrolki użytkownika musi zawierać cyfry ProductId, firma Microsoft ustawi właściwości ProductID naszych formantu przy użyciu instrukcji Eval względem bieżącego elementu modelu danych strony.</span><span class="sxs-lookup"><span data-stu-id="4e718-195">Since the AlsoPurchased user control requires a ProductId number we will set the ProductID property of our control by using an Eval statement against the current data model item of the page.</span></span>

![](tailspin-spyworks-part-7/_static/image3.png)

<span data-ttu-id="4e718-196">Gdy do skompilowania i uruchom teraz i przejdź do produktu widzimy elementów "Również zakupione".</span><span class="sxs-lookup"><span data-stu-id="4e718-196">When we build and run now and browse to a product we see the "Also Purchased" items.</span></span>

![](tailspin-spyworks-part-7/_static/image7.jpg)

> [!div class="step-by-step"]
> <span data-ttu-id="4e718-197">[Poprzednie](tailspin-spyworks-part-6.md)
> [dalej](tailspin-spyworks-part-8.md)</span><span class="sxs-lookup"><span data-stu-id="4e718-197">[Previous](tailspin-spyworks-part-6.md)
[Next](tailspin-spyworks-part-8.md)</span></span>
