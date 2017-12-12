---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-7
title: "Część 7: Dodawanie funkcji | Dokumentacja firmy Microsoft"
author: JoeStagner
description: "Ta seria samouczek zawiera szczegóły dotyczące wszystkich kroków kompilacji Tailspin Spyworks przykładowej aplikacji. Część 7 dodaje dodatkowe funkcje, takie jak konto okienko..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/21/2010
ms.topic: article
ms.assetid: 50223ee9-11b9-4cf3-bca2-e2f10bf471f3
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-7
msc.type: authoredcontent
ms.openlocfilehash: 5280de44b3e75f9d1ae85e0248bc3ef6d5444f6d
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
<a name="part-7-adding-features"></a><span data-ttu-id="3c857-104">Część 7: Dodawanie funkcji</span><span class="sxs-lookup"><span data-stu-id="3c857-104">Part 7: Adding Features</span></span>
====================
<span data-ttu-id="3c857-105">przez [Stagner Jan](https://github.com/JoeStagner)</span><span class="sxs-lookup"><span data-stu-id="3c857-105">by [Joe Stagner](https://github.com/JoeStagner)</span></span>

> <span data-ttu-id="3c857-106">Tailspin Spyworks pokazano, jak bardzo proste jest tworzenie zaawansowanych, skalowalnych aplikacji dla platformy .NET.</span><span class="sxs-lookup"><span data-stu-id="3c857-106">Tailspin Spyworks demonstrates how extraordinarily simple it is to create powerful, scalable applications for the .NET platform.</span></span> <span data-ttu-id="3c857-107">Przedstawia on poza jak nowe, fantastyczne funkcje programu ASP.NET 4 do tworzenia sklepu online, łącznie z zakupów, wyewidencjonowania i administracji.</span><span class="sxs-lookup"><span data-stu-id="3c857-107">It shows off how to use the great new features in ASP.NET 4 to build an online store, including shopping, checkout, and administration.</span></span>
> 
> <span data-ttu-id="3c857-108">Ta seria samouczek zawiera szczegóły dotyczące wszystkich kroków kompilacji Tailspin Spyworks przykładowej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="3c857-108">This tutorial series details all of the steps taken to build the Tailspin Spyworks sample application.</span></span> <span data-ttu-id="3c857-109">Część 7 dodaje dodatkowe funkcje, takie jak konto przeglądu, recenzje produktów i "popularnych elementy" i kontrolek użytkownika "również zakupionych".</span><span class="sxs-lookup"><span data-stu-id="3c857-109">Part 7 adds additional features, such as account review, product reviews, and "popular items" and "also purchased" user controls.</span></span>


## <a id="_Toc260221673"></a><span data-ttu-id="3c857-110">Dodawanie funkcji</span><span class="sxs-lookup"><span data-stu-id="3c857-110">Adding Features</span></span>

<span data-ttu-id="3c857-111">Chociaż użytkownicy mogą przeglądać naszego katalogu, należy umieścić elementów w ich koszyk i ukończyć proces wyewidencjonowania, Brak funkcji obsługi wielu możemy uwzględni zwiększające naszej witrynie.</span><span class="sxs-lookup"><span data-stu-id="3c857-111">Though users can browse our catalog, place items in their shopping cart, and complete the checkout process, there are a number of supporting features that we will include to improve our site.</span></span>

1. <span data-ttu-id="3c857-112">Przegląd konta (Lista zleceń umieszczone oraz szczegóły).</span><span class="sxs-lookup"><span data-stu-id="3c857-112">Account Review (List orders placed and view details.)</span></span>
2. <span data-ttu-id="3c857-113">Dodaj zawartość określonego kontekstu, pierwsza strona.</span><span class="sxs-lookup"><span data-stu-id="3c857-113">Add some context specific content to the front page.</span></span>
3. <span data-ttu-id="3c857-114">Dodawanie funkcji, aby umożliwić użytkownikom przeglądu produktów w katalogu.</span><span class="sxs-lookup"><span data-stu-id="3c857-114">Add a feature to let users Review the products in the catalog.</span></span>
4. <span data-ttu-id="3c857-115">Tworzenie formantu użytkownika do wyświetlania popularnych elementów i miejsce, które kontrolują na stronie witryny.</span><span class="sxs-lookup"><span data-stu-id="3c857-115">Create a User Control to display Popular Items and Place that control on the front page.</span></span>
5. <span data-ttu-id="3c857-116">Tworzenie formantu użytkownika "Zakupu" i dodaj go do strony szczegółów produktu.</span><span class="sxs-lookup"><span data-stu-id="3c857-116">Create an "Also Purchased" user control and add it to the product details page.</span></span>
6. <span data-ttu-id="3c857-117">Dodawanie kontaktu strony.</span><span class="sxs-lookup"><span data-stu-id="3c857-117">Add a Contact Page.</span></span>
7. <span data-ttu-id="3c857-118">Dodaj informacje o stronie.</span><span class="sxs-lookup"><span data-stu-id="3c857-118">Add an About Page.</span></span>
8. <span data-ttu-id="3c857-119">Błąd globalne</span><span class="sxs-lookup"><span data-stu-id="3c857-119">Global Error</span></span>

## <a id="_Toc260221674"></a><span data-ttu-id="3c857-120">Przegląd konta</span><span class="sxs-lookup"><span data-stu-id="3c857-120">Account Review</span></span>

<span data-ttu-id="3c857-121">W folderze "Konto" Utwórz dwie strony .aspx jedną o nazwie OrderList.aspx i innych OrderDetails.aspx o nazwie</span><span class="sxs-lookup"><span data-stu-id="3c857-121">In the "Account" folder create two .aspx pages one named OrderList.aspx and the other named OrderDetails.aspx</span></span>

<span data-ttu-id="3c857-122">OrderList.aspx będzie korzystać kontrolki GridView i EntityDataSoure, ile mamy wcześniej.</span><span class="sxs-lookup"><span data-stu-id="3c857-122">OrderList.aspx will leverage the GridView and EntityDataSoure controls much as we have previously.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample1.aspx)]

<span data-ttu-id="3c857-123">EntityDataSoure wybiera rekordy z tabeli zamówienia filtrowane wg nazwy użytkownika (zobacz WhereParameter) który mamy ustawiony w zmiennej sesji użytkownikowi logowanie użytkownika.</span><span class="sxs-lookup"><span data-stu-id="3c857-123">The EntityDataSoure selects records from the Orders table filtered on the UserName (see the WhereParameter) which we set in a session variable when the user log's in.</span></span>

<span data-ttu-id="3c857-124">Należy też zauważyć tych parametrów w pole hiperłącza HyperlinkField GridView:</span><span class="sxs-lookup"><span data-stu-id="3c857-124">Note also these parameters in the HyperlinkField of the GridView:</span></span>

[!code-xml[Main](tailspin-spyworks-part-7/samples/sample2.xml)]

<span data-ttu-id="3c857-125">Określają one łącza do każdego produktu, określając pola OrderID jako parametr QueryString na stronie OrderDetails.aspx w widoku szczegółów zamówienia.</span><span class="sxs-lookup"><span data-stu-id="3c857-125">These specify the link to the Order details view for each product specifying the OrderID field as a QueryString parameter to the OrderDetails.aspx page.</span></span>

## <a id="_Toc260221675"></a><span data-ttu-id="3c857-126">OrderDetails.aspx</span><span class="sxs-lookup"><span data-stu-id="3c857-126">OrderDetails.aspx</span></span>

<span data-ttu-id="3c857-127">Używamy obiektu EntityDataSource kontroli dostępu do zamówienia i FormView wyświetlać dane kolejności i innego obiektu EntityDataSource z widoku GridView do wyświetlenia wszystkich kolejność pozycji.</span><span class="sxs-lookup"><span data-stu-id="3c857-127">We will use an EntityDataSource control to access the Orders and a FormView to display the Order data and another EntityDataSource with a GridView to display all the Order's line items.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample3.aspx)]

<span data-ttu-id="3c857-128">W pliku kodu za (OrderDetails.aspx.cs) mamy dwa małego bity celów.</span><span class="sxs-lookup"><span data-stu-id="3c857-128">In the Code Behind file (OrderDetails.aspx.cs) we have two little bits of housekeeping.</span></span>

<span data-ttu-id="3c857-129">Najpierw należy upewnić się, że SzczegółyZamówień zawsze pobiera OrderId.</span><span class="sxs-lookup"><span data-stu-id="3c857-129">First we need to make sure that OrderDetails always gets an OrderId.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample4.cs)]

<span data-ttu-id="3c857-130">Musimy także obliczania i wyświetlania kolejności całkowita z pozycji wiersza.</span><span class="sxs-lookup"><span data-stu-id="3c857-130">We also need to calculate and display the order total from the line items.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample5.cs)]

## <a id="_Toc260221676"></a><span data-ttu-id="3c857-131">Strona główna</span><span class="sxs-lookup"><span data-stu-id="3c857-131">The Home Page</span></span>

<span data-ttu-id="3c857-132">Na stronie Default.aspx Dodajmy niektórych zawartość statyczną.</span><span class="sxs-lookup"><span data-stu-id="3c857-132">Let's add some static content to the Default.aspx page.</span></span>

<span data-ttu-id="3c857-133">Najpierw będzie utworzyć folderu "Content" i jego folderu obrazów (i będzie umieścić obraz ma być używany na stronie głównej.)</span><span class="sxs-lookup"><span data-stu-id="3c857-133">First I'll create a "Content" folder and within it an Images folder (and I'll include an image to be used on the home page.)</span></span>

<span data-ttu-id="3c857-134">W zastępczym dolnej części strony Default.aspx Dodaj następujący kod znaczników.</span><span class="sxs-lookup"><span data-stu-id="3c857-134">Into the bottom placeholder of the Default.aspx page, add the following markup.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample6.aspx)]

## <a id="_Toc260221677"></a><span data-ttu-id="3c857-135">Recenzje produktów</span><span class="sxs-lookup"><span data-stu-id="3c857-135">Product Reviews</span></span>

<span data-ttu-id="3c857-136">Najpierw dodamy przycisk z łączem do formularza, który możemy użyć do wprowadzania przeglądu produktu.</span><span class="sxs-lookup"><span data-stu-id="3c857-136">First we'll add a button with a link to a form that we can use to enter a product review.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample7.aspx)]

![](tailspin-spyworks-part-7/_static/image1.jpg)

<span data-ttu-id="3c857-137">Należy pamiętać, możemy przekazywanie ProductID w ciągu zapytania</span><span class="sxs-lookup"><span data-stu-id="3c857-137">Note that we are passing the ProductID in the query string</span></span>

<span data-ttu-id="3c857-138">Następny Dodajmy stronę o nazwie ReviewAdd.aspx</span><span class="sxs-lookup"><span data-stu-id="3c857-138">Next let's add page named ReviewAdd.aspx</span></span>

<span data-ttu-id="3c857-139">Ta strona będzie używać zestawu ASP.NET AJAX kontroli narzędzi.</span><span class="sxs-lookup"><span data-stu-id="3c857-139">This page will use the ASP.NET AJAX Control Toolkit.</span></span> <span data-ttu-id="3c857-140">Jeśli użytkownik jeszcze nie, aby pobrać go z [DevExpress](http://devexpress.com/act) i wskazówki dotyczące konfigurowania zestawu narzędzi do użytku z programem Visual Studio tutaj [https://www.asp.net/learn/ajax-videos/video-76.aspx](../../../videos/ajax-control-toolkit/how-do-i-get-started-with-the-aspnet-ajax-control-toolkit.md).</span><span class="sxs-lookup"><span data-stu-id="3c857-140">If you have not already done so you can download it from [DevExpress](http://devexpress.com/act) and there is guidance on setting up the toolkit for use with Visual Studio here [https://www.asp.net/learn/ajax-videos/video-76.aspx](../../../videos/ajax-control-toolkit/how-do-i-get-started-with-the-aspnet-ajax-control-toolkit.md).</span></span>

<span data-ttu-id="3c857-141">W trybie projektowania przeciągnij formanty i modułów weryfikacji z przybornika i kompilacji podobny do poniższego formularza.</span><span class="sxs-lookup"><span data-stu-id="3c857-141">In design mode, drag controls and validators from the toolbox and build a form like the one below.</span></span>

![](tailspin-spyworks-part-7/_static/image2.jpg)

<span data-ttu-id="3c857-142">Kod znaczników będzie wyglądać następująco.</span><span class="sxs-lookup"><span data-stu-id="3c857-142">The markup will look something like this.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample8.aspx)]

<span data-ttu-id="3c857-143">Teraz, możemy wprowadzić recenzji, służy do wyświetlania tych przeglądami na stronie produktu.</span><span class="sxs-lookup"><span data-stu-id="3c857-143">Now that we can enter reviews, lets display those reviews on the product page.</span></span>

<span data-ttu-id="3c857-144">Na stronie ProductDetails.aspx, Dodaj ten kod znaczników.</span><span class="sxs-lookup"><span data-stu-id="3c857-144">Add this markup to the ProductDetails.aspx page.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample9.aspx)]

<span data-ttu-id="3c857-145">Uruchomienie teraz naszej aplikacji i przechodząc do produktu zawiera informacje o produkcie tym recenzje klientów.</span><span class="sxs-lookup"><span data-stu-id="3c857-145">Running our application now and navigating to a product shows the product information including customer reviews.</span></span>

![](tailspin-spyworks-part-7/_static/image3.jpg)

## <a id="_Toc260221678"></a><span data-ttu-id="3c857-146">Formant popularnych elementów (Tworzenie kontrolki użytkownika)</span><span class="sxs-lookup"><span data-stu-id="3c857-146">Popular Items Control (Creating User Controls)</span></span>

<span data-ttu-id="3c857-147">W celu zwiększenia sprzedaży w witrynie sieci web dodamy kilka funkcji "sugerujących sprzedaje" produktów popularnych lub pokrewne.</span><span class="sxs-lookup"><span data-stu-id="3c857-147">In order to increase sales on your web site we will add a couple of features to "suggestive sell" popular or related products.</span></span>

<span data-ttu-id="3c857-148">Pierwszy te funkcje będą listę najpopularniejszych produktu w naszym katalogu produktów.</span><span class="sxs-lookup"><span data-stu-id="3c857-148">The first of these features will be a list of the more popular product in our product catalog.</span></span>

<span data-ttu-id="3c857-149">Utworzymy "Kontrola użytkownika" do wyświetlenia sprzedających się elementy na stronie głównej naszej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="3c857-149">We will create a "User Control" to display the top selling items on the home page of our application.</span></span> <span data-ttu-id="3c857-150">Ponieważ będzie to formantu, możemy użyć go na każdej stronie, po prostu przeciągania i upuszczania formantu w projektancie programu Visual Studio na każdej stronie, które firma Microsoft, takich jak.</span><span class="sxs-lookup"><span data-stu-id="3c857-150">Since this will be a control, we can use it on any page by simply dragging and dropping the control in Visual Studio's designer onto any page that we like.</span></span>

<span data-ttu-id="3c857-151">W Eksploratorze rozwiązań programu Visual Studio kliknij prawym przyciskiem myszy nazwę rozwiązania i Utwórz nowy katalog o nazwie "Formanty".</span><span class="sxs-lookup"><span data-stu-id="3c857-151">In Visual Studio's solutions explorer, right-click on the solution name and create a new directory named "Controls".</span></span> <span data-ttu-id="3c857-152">Mimo że nie jest konieczne to zrobić, możemy pomóc, Zachowaj naszych projektu, tworząc naszych kontrolek użytkownika w katalogu "Formanty".</span><span class="sxs-lookup"><span data-stu-id="3c857-152">While it is not necessary to do so, we will help keep our project organized by creating all our user controls in the "Controls" directory.</span></span>

<span data-ttu-id="3c857-153">Kliknij prawym przyciskiem myszy w folderze formantów, a następnie wybierz pozycję "Nowy element":</span><span class="sxs-lookup"><span data-stu-id="3c857-153">Right-click on the controls folder and choose "New Item" :</span></span>

![](tailspin-spyworks-part-7/_static/image4.jpg)

<span data-ttu-id="3c857-154">Określ nazwę dla naszych formantu "PopularItems".</span><span class="sxs-lookup"><span data-stu-id="3c857-154">Specify a name for our control of "PopularItems".</span></span> <span data-ttu-id="3c857-155">Należy pamiętać, że rozszerzenie pliku dla formantów użytkownika .ascx nie aspx.</span><span class="sxs-lookup"><span data-stu-id="3c857-155">Note that the file extension for user controls is .ascx not .aspx.</span></span>

<span data-ttu-id="3c857-156">Mamy kontroli popularnych użytkownika elementy zostaną zdefiniowane w następujący sposób.</span><span class="sxs-lookup"><span data-stu-id="3c857-156">Our Popular Items User control will be defined as follows.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample10.aspx)]

<span data-ttu-id="3c857-157">W tym miejscu używamy metody, które firma Microsoft nie zostały jeszcze użyte w tej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="3c857-157">Here we're using a method we have not used yet in this application.</span></span> <span data-ttu-id="3c857-158">Używamy kontrolce elementu powtarzanego i zamiast kontroli źródła danych możemy jest powiązanie kontrolce elementu powtarzanego z wyników zapytania składnika LINQ to Entities.</span><span class="sxs-lookup"><span data-stu-id="3c857-158">We're using the repeater control and instead of using a data source control we're binding the Repeater Control to the results of a LINQ to Entities query.</span></span>

<span data-ttu-id="3c857-159">W kodzie mamy kontroli przejdziemy w następujący sposób.</span><span class="sxs-lookup"><span data-stu-id="3c857-159">In the code behind of our control we do that as follows.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample11.cs)]

<span data-ttu-id="3c857-160">Należy też zauważyć wierszem ważne w górnej części znaczników naszych formantu.</span><span class="sxs-lookup"><span data-stu-id="3c857-160">Note also this important line at the top of our control's markup.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample12.aspx)]

<span data-ttu-id="3c857-161">Ponieważ większość popularnych elementów nie zmiana na podstawie minutę na minutę dodamy bóle dyrektywy poprawić wydajność aplikacji.</span><span class="sxs-lookup"><span data-stu-id="3c857-161">Since the most popular items won't be changing on a minute to minute basis we can add a aching directive to improve the performance of our application.</span></span> <span data-ttu-id="3c857-162">Ta dyrektywa spowoduje, że kod formanty można wykonywać tylko po wygaśnięciu pamięci podręcznej danych wyjściowych formantu.</span><span class="sxs-lookup"><span data-stu-id="3c857-162">This directive will cause the controls code to only be executed when the cached output of the control expires.</span></span> <span data-ttu-id="3c857-163">W przeciwnym razie będzie używać pamięci podręcznej wersji wyjście formantu.</span><span class="sxs-lookup"><span data-stu-id="3c857-163">Otherwise, the cached version of the control's output will be used.</span></span>

<span data-ttu-id="3c857-164">Teraz wszystko, co mamy zrobić to objąć naszą stronę Default.aspc naszego nowego formantu.</span><span class="sxs-lookup"><span data-stu-id="3c857-164">Now all we have to do is include our new control in our Default.aspc page.</span></span>

<span data-ttu-id="3c857-165">Użyj przeciągnij i upuść można umieścić w kolumnie Otwórz naszych domyślnego formularza wystąpienia formantu.</span><span class="sxs-lookup"><span data-stu-id="3c857-165">Use drag and drop to place an instance of the control in the open column of our Default form.</span></span>

![](tailspin-spyworks-part-7/_static/image5.jpg)

<span data-ttu-id="3c857-166">Teraz możemy uruchamiania aplikacji na stronę główną Wyświetla najpopularniejszych elementy.</span><span class="sxs-lookup"><span data-stu-id="3c857-166">Now when we run our application the home page displays the most popular items.</span></span>

![](tailspin-spyworks-part-7/_static/image6.jpg)

## <a id="_Toc260221679"></a><span data-ttu-id="3c857-167">"Zakupu" kontroli (kontrolek użytkownika z parametrami)</span><span class="sxs-lookup"><span data-stu-id="3c857-167">"Also Purchased" Control (User Controls with Parameters)</span></span>

<span data-ttu-id="3c857-168">Drugi formant użytkownika, który utworzymy potrwa sugerujących, sprzedaży na następny poziom, dodając kontekstu szczegółowością.</span><span class="sxs-lookup"><span data-stu-id="3c857-168">The second User Control that we'll create will take suggestive selling to the next level by adding context specificity.</span></span>

<span data-ttu-id="3c857-169">Logika obliczyć pierwszych elementów "Zakupu" jest nieuproszczony.</span><span class="sxs-lookup"><span data-stu-id="3c857-169">The logic to calculate the top "Also Purchased" items is non-trivial.</span></span>

<span data-ttu-id="3c857-170">Mamy kontroli "Zakupu" Wybierz rekordy SzczegółyZamówień (wcześniej kupił) dla aktualnie wybranego ProductID, a następnie pobrania OrderIDs dla każdego zlecenia unikatowy, która znajduje się.</span><span class="sxs-lookup"><span data-stu-id="3c857-170">Our "Also Purchased" control will select the OrderDetails records (previously purchased) for the currently selected ProductID and grab the OrderIDs for each unique order that is found.</span></span>

<span data-ttu-id="3c857-171">Następnie firma Microsoft będzie wybierać al produktów z tych zleceń i Suma zakupione ilości.</span><span class="sxs-lookup"><span data-stu-id="3c857-171">Then we will select al the products from all those Orders and sum the quantities purchased.</span></span> <span data-ttu-id="3c857-172">Firma Microsoft będzie sortować produkty Suma ilość i Wyświetl 5 pierwszych elementów.</span><span class="sxs-lookup"><span data-stu-id="3c857-172">We'll sort the products by that quantity sum and display the top five items.</span></span>

<span data-ttu-id="3c857-173">Biorąc pod uwagę złożoność tę logikę, wprowadzimy tego algorytmu jako procedury składowanej.</span><span class="sxs-lookup"><span data-stu-id="3c857-173">Given the complexity of this logic, we will implement this algorithm as a stored procedure.</span></span>

<span data-ttu-id="3c857-174">T-SQL dla procedury składowanej ma następującą składnię.</span><span class="sxs-lookup"><span data-stu-id="3c857-174">The T-SQL for the stored procedure is as follows.</span></span>

[!code-sql[Main](tailspin-spyworks-part-7/samples/sample13.sql)]

<span data-ttu-id="3c857-175">Należy pamiętać, że tej procedury składowanej (SelectPurchasedWithProducts) istniał w bazie danych, gdy firma Microsoft uwzględnione w naszej aplikacji i firma Microsoft wygenerowany modelu danych jednostki, możemy określić oprócz tabele i widoki, które firma Microsoft potrzeby modelu danych jednostki powinna zawierać tę procedurę składowaną.</span><span class="sxs-lookup"><span data-stu-id="3c857-175">Note that this stored procedure (SelectPurchasedWithProducts) existed in the database when we included it in our application and when we generated the Entity Data Model we specified that, in addition to the Tables and Views that we needed, the Entity Data Model should include this stored procedure.</span></span>

<span data-ttu-id="3c857-176">Dostęp do procedury składowanej z modelu danych jednostki, należy zaimportować funkcji.</span><span class="sxs-lookup"><span data-stu-id="3c857-176">To access the stored procedure from the Entity Data Model we need to import the function.</span></span>

<span data-ttu-id="3c857-177">Kliknij dwukrotnie modelu danych jednostki w Eksploratorze rozwiązań, aby otworzyć go w Projektancie i Otwórz przeglądarkę z modelu, a następnie kliknij prawym przyciskiem myszy w Projektancie i wybierz opcję "Dodaj importu funkcji".</span><span class="sxs-lookup"><span data-stu-id="3c857-177">Double Click on the Entity Data Model in the Solutions Explorer to open it in the designer and open the Model Browser, then right-click in the designer and select "Add Function Import".</span></span>

![](tailspin-spyworks-part-7/_static/image1.png)

<span data-ttu-id="3c857-178">W ten sposób spowoduje otwarcie okna dialogowego.</span><span class="sxs-lookup"><span data-stu-id="3c857-178">Doing so will open this dialog.</span></span>

![](tailspin-spyworks-part-7/_static/image2.png)

<span data-ttu-id="3c857-179">Wypełnij pola, jak pokazano powyżej, wybierając "SelectPurchasedWithProducts" i użyj nazwy procedury nazwę naszych importowanych funkcji.</span><span class="sxs-lookup"><span data-stu-id="3c857-179">Fill out the fields as you see above, selecting the "SelectPurchasedWithProducts" and use the procedure name for the name of our imported function.</span></span>

<span data-ttu-id="3c857-180">Kliknij przycisk "Ok".</span><span class="sxs-lookup"><span data-stu-id="3c857-180">Click "Ok".</span></span>

<span data-ttu-id="3c857-181">To to, które firma Microsoft może po prostu program względem procedury składowanej jak firma Microsoft może innego elementu w modelu.</span><span class="sxs-lookup"><span data-stu-id="3c857-181">Having done this we can simply program against the stored procedure as we might any other item in the model.</span></span>

<span data-ttu-id="3c857-182">Tak w folderze naszych "Formanty" Utwórz kontrolkę użytkownika o nazwie AlsoPurchased.ascx.</span><span class="sxs-lookup"><span data-stu-id="3c857-182">So, in our "Controls" folder create a new user control named AlsoPurchased.ascx.</span></span>

<span data-ttu-id="3c857-183">Kod znaczników dla tego formantu wyglądają bardzo dostosowanym do sterowania PopularItems.</span><span class="sxs-lookup"><span data-stu-id="3c857-183">The markup for this control will look very familiar to the PopularItems control.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample14.aspx)]

<span data-ttu-id="3c857-184">Znacząca różnica jest który są nie pamięci podręcznej danych wyjściowych, ponieważ do renderowania elementu będą się różnić od produktu.</span><span class="sxs-lookup"><span data-stu-id="3c857-184">The notable difference is that are not caching the output since the item's to be rendered will differ by product.</span></span>

<span data-ttu-id="3c857-185">ProductId będzie "property" do formantu.</span><span class="sxs-lookup"><span data-stu-id="3c857-185">The ProductId will be a "property" to the control.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample15.cs)]

<span data-ttu-id="3c857-186">W obsłudze zdarzeń PreRender formantu możemy bkość, aby wykonać trzy czynności.</span><span class="sxs-lookup"><span data-stu-id="3c857-186">In the control's PreRender event handler we eed to do three things.</span></span>

1. <span data-ttu-id="3c857-187">Upewnij się, że ustawiono ProductID.</span><span class="sxs-lookup"><span data-stu-id="3c857-187">Make sure the ProductID is set.</span></span>
2. <span data-ttu-id="3c857-188">Zobacz, czy wszystkie produkty, które zostały zakupione z bieżącej.</span><span class="sxs-lookup"><span data-stu-id="3c857-188">See if there are any products that have been purchased with the current one.</span></span>
3. <span data-ttu-id="3c857-189">Dane wyjściowe niektóre elementy, jak określono w #2.</span><span class="sxs-lookup"><span data-stu-id="3c857-189">Output some items as determined in #2.</span></span>

<span data-ttu-id="3c857-190">Należy zwrócić uwagę, jak łatwo jest wywołać procedurę składowaną za pośrednictwem modelu.</span><span class="sxs-lookup"><span data-stu-id="3c857-190">Note how easy it is to call the stored procedure through the model.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample16.cs)]

<span data-ttu-id="3c857-191">Po ustaleniu, które są "również zakupione" możemy po prostu powiązać powtarzanego wyników zwróconych przez kwerendę.</span><span class="sxs-lookup"><span data-stu-id="3c857-191">After determining that there ARE "also purchased" we can simply bind the repeater to the results returned by the query.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample17.cs)]

<span data-ttu-id="3c857-192">Jakby nie było żadnych elementów "zakupu" będzie wystarczy wyświetlić innych popularnych elementów z naszego katalogu.</span><span class="sxs-lookup"><span data-stu-id="3c857-192">If there were not any "also purchased" items we'll simply display other popular items from our catalog.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample18.cs)]

<span data-ttu-id="3c857-193">Aby wyświetlić elementy "Zakupu", otwórz stronę ProductDetails.aspx i przeciągnij formant AlsoPurchased w Eksploratorze rozwiązań, aby był on wyświetlany w tym miejscu w znaczniku.</span><span class="sxs-lookup"><span data-stu-id="3c857-193">To view the "Also Purchased" items, open the ProductDetails.aspx page and drag the AlsoPurchased control from the Solutions Explorer so that it appears in this position in the markup.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample19.aspx)]

<span data-ttu-id="3c857-194">W ten sposób utworzyć odwołanie do formantu w górnej części strony ProductDetails.</span><span class="sxs-lookup"><span data-stu-id="3c857-194">Doing so will create a reference to the control at the top of the ProductDetails page.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample20.aspx)]

<span data-ttu-id="3c857-195">Ponieważ kontrola użytkownika AlsoPurchased wymaga podania liczby ProductId możemy ustawi właściwość ProductID naszych formantu przy użyciu instrukcji Eval względem bieżącego elementu modelu danych strony.</span><span class="sxs-lookup"><span data-stu-id="3c857-195">Since the AlsoPurchased user control requires a ProductId number we will set the ProductID property of our control by using an Eval statement against the current data model item of the page.</span></span>

![](tailspin-spyworks-part-7/_static/image3.png)

<span data-ttu-id="3c857-196">Gdy firma Microsoft kompilacji i uruchomić teraz i przejdź do produktu widzimy elementów "Zakupu".</span><span class="sxs-lookup"><span data-stu-id="3c857-196">When we build and run now and browse to a product we see the "Also Purchased" items.</span></span>

![](tailspin-spyworks-part-7/_static/image7.jpg)

>[!div class="step-by-step"]
<span data-ttu-id="3c857-197">[Poprzednie](tailspin-spyworks-part-6.md)
[dalej](tailspin-spyworks-part-8.md)</span><span class="sxs-lookup"><span data-stu-id="3c857-197">[Previous](tailspin-spyworks-part-6.md)
[Next](tailspin-spyworks-part-8.md)</span></span>
