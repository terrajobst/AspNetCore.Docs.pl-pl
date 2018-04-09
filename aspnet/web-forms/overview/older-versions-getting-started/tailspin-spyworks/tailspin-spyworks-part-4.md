---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-4
title: 'Część 4: Wyświetlanie listy produktów | Dokumentacja firmy Microsoft'
author: JoeStagner
description: Ta seria samouczek zawiera szczegóły dotyczące wszystkich kroków kompilacji Tailspin Spyworks przykładowej aplikacji. Wyświetlanie listy produktów z widoku GridView zysk obejmuje część 4.
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/21/2010
ms.topic: article
ms.assetid: 4fab47d5-a6ec-4fdc-91f0-651a093a24b9
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-4
msc.type: authoredcontent
ms.openlocfilehash: 69b26344e6dcdbf27e94da90ad5d6cd79f27ccd3
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
---
<a name="part-4-listing-products"></a><span data-ttu-id="dea1a-104">Część 4: Lista produktów</span><span class="sxs-lookup"><span data-stu-id="dea1a-104">Part 4: Listing Products</span></span>
====================
<span data-ttu-id="dea1a-105">przez [Stagner Jan](https://github.com/JoeStagner)</span><span class="sxs-lookup"><span data-stu-id="dea1a-105">by [Joe Stagner](https://github.com/JoeStagner)</span></span>

> <span data-ttu-id="dea1a-106">Tailspin Spyworks pokazano, jak bardzo proste jest tworzenie zaawansowanych, skalowalnych aplikacji dla platformy .NET.</span><span class="sxs-lookup"><span data-stu-id="dea1a-106">Tailspin Spyworks demonstrates how extraordinarily simple it is to create powerful, scalable applications for the .NET platform.</span></span> <span data-ttu-id="dea1a-107">Przedstawia on poza jak nowe, fantastyczne funkcje programu ASP.NET 4 do tworzenia sklepu online, łącznie z zakupów, wyewidencjonowania i administracji.</span><span class="sxs-lookup"><span data-stu-id="dea1a-107">It shows off how to use the great new features in ASP.NET 4 to build an online store, including shopping, checkout, and administration.</span></span>
> 
> <span data-ttu-id="dea1a-108">Ta seria samouczek zawiera szczegóły dotyczące wszystkich kroków kompilacji Tailspin Spyworks przykładowej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="dea1a-108">This tutorial series details all of the steps taken to build the Tailspin Spyworks sample application.</span></span> <span data-ttu-id="dea1a-109">Część 4 zawiera listę produktów za pomocą formantu widoku GridView.</span><span class="sxs-lookup"><span data-stu-id="dea1a-109">Part 4 covers listing products with the GridView control.</span></span>


## <a id="_Toc260221670"></a>  <span data-ttu-id="dea1a-110">Wyświetlanie listy produktów z formantem widoku GridView</span><span class="sxs-lookup"><span data-stu-id="dea1a-110">Listing Products with the GridView Control</span></span>

<span data-ttu-id="dea1a-111">Zacznijmy Implementowanie naszą stronę ProductsList.aspx "Kliknąć prawym przyciskiem myszy" na naszych rozwiązań i wybierając polecenie "Dodaj" i "Nowy element".</span><span class="sxs-lookup"><span data-stu-id="dea1a-111">Let's begin implementing our ProductsList.aspx page by "Right Clicking" on our solution and selecting "Add" and "New Item".</span></span>

![](tailspin-spyworks-part-4/_static/image1.jpg)

<span data-ttu-id="dea1a-112">Wybierz pozycję "Strony sieci Web formularza za pomocą wzorca" i wprowadź nazwę strony ProductsList.aspx".</span><span class="sxs-lookup"><span data-stu-id="dea1a-112">Choose "Web Form Using Master Page" and enter a page name of ProductsList.aspx".</span></span>

<span data-ttu-id="dea1a-113">Kliknij przycisk "Dodaj".</span><span class="sxs-lookup"><span data-stu-id="dea1a-113">Click "Add".</span></span>

![](tailspin-spyworks-part-4/_static/image2.jpg)

<span data-ttu-id="dea1a-114">Następnie wybierz folder "Style", gdzie możemy umieścić strony Site.Master i wybierz ją z okna "Zawartość folderu".</span><span class="sxs-lookup"><span data-stu-id="dea1a-114">Next choose the "Styles" folder where we placed the Site.Master page and select it from the "Contents of folder" window.</span></span>

![](tailspin-spyworks-part-4/_static/image3.jpg)

<span data-ttu-id="dea1a-115">Kliknij przycisk "Ok", aby utworzyć stronę.</span><span class="sxs-lookup"><span data-stu-id="dea1a-115">Click "Ok" to create the page.</span></span>

<span data-ttu-id="dea1a-116">Naszej bazie danych jest wypełniana danych produktu, jak pokazano poniżej.</span><span class="sxs-lookup"><span data-stu-id="dea1a-116">Our database is populated with product data as seen below.</span></span>

![](tailspin-spyworks-part-4/_static/image4.jpg)

<span data-ttu-id="dea1a-117">Po utworzeniu naszą stronę ponownie użyjemy Entity Data Source dostępu do danych tego produktu, ale w tym wystąpieniu musimy wybierz jednostek produktu i musimy ograniczanie elementów, które są zwracane tylko do tych dla wybranej kategorii.</span><span class="sxs-lookup"><span data-stu-id="dea1a-117">After our page is created we'll again use an Entity Data Source to access that product data, but in this instance we need to select the Product Entities and we need to restrict the items that are returned to only those for the selected Category.</span></span>

<span data-ttu-id="dea1a-118">W tym celu możemy poinformować obiektu EntityDataSource do automatycznego generowania klauzuli WHERE i firma Microsoft będzie określić WhereParameter.</span><span class="sxs-lookup"><span data-stu-id="dea1a-118">To accomplish this we'll tell the EntityDataSource to Auto Generate the WHERE clause and we'll specify the WhereParameter.</span></span>

<span data-ttu-id="dea1a-119">Będziesz się odwołać, że gdy utworzyliśmy elementów Menu w naszym "produktu kategorii Menu" dynamicznie budujemy łącza przez dodanie CatagoryID na ciąg zapytania dla każdego łącza.</span><span class="sxs-lookup"><span data-stu-id="dea1a-119">You'll recall that when we created the Menu Items in our "Product Category Menu" we dynamically built the link by adding the CatagoryID to the QueryString for each link.</span></span> <span data-ttu-id="dea1a-120">Firma Microsoft powiadomi Entity Data Source pochodzić z parametru QueryString parametru WHERE.</span><span class="sxs-lookup"><span data-stu-id="dea1a-120">We will tell the Entity Data Source to derive the WHERE parameter from that QueryString parameter.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-4/samples/sample1.aspx)]

<span data-ttu-id="dea1a-121">Dalej skonfigurujemy formantu ListView, aby wyświetlić listę produktów.</span><span class="sxs-lookup"><span data-stu-id="dea1a-121">Next, we'll configure the ListView control to display a list of products.</span></span> <span data-ttu-id="dea1a-122">Aby utworzyć zapewnienia optymalnego działania zakupów, firma Microsoft będzie compact kilka funkcji zwięzły do każdego produktu poszczególne wyświetlane w naszym ListVew.</span><span class="sxs-lookup"><span data-stu-id="dea1a-122">To create an optimal shopping experience we'll compact several concise features into each individual product displayed in our ListVew.</span></span>

- <span data-ttu-id="dea1a-123">Nazwa produktu będzie link do tego produktu: widok szczegółów.</span><span class="sxs-lookup"><span data-stu-id="dea1a-123">The product name will be a link to the product's detail view.</span></span>
- <span data-ttu-id="dea1a-124">Wyświetlane są ceny produktu.</span><span class="sxs-lookup"><span data-stu-id="dea1a-124">The product's price will be displayed.</span></span>
- <span data-ttu-id="dea1a-125">Obraz produktu będą wyświetlane i dynamicznie wybierzemy obrazu z katalogu katalog obrazów w naszej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="dea1a-125">An image of the product will be displayed and we'll dynamically select the image from a catalog images directory in our application.</span></span>
- <span data-ttu-id="dea1a-126">Firma Microsoft będzie zawierać łącze do natychmiast dodać określonego produktu do koszyka.</span><span class="sxs-lookup"><span data-stu-id="dea1a-126">We will include a link to immediately add the specific product to the shopping cart.</span></span>

<span data-ttu-id="dea1a-127">Oto kod znaczników dla naszych wystąpienia formantu ListView.</span><span class="sxs-lookup"><span data-stu-id="dea1a-127">Here is the markup for our ListView control instance.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-4/samples/sample2.aspx)]

<span data-ttu-id="dea1a-128">Firma Microsoft są dynamiczne tworzenie kilku łączy dla każdego produktu wyświetlane.</span><span class="sxs-lookup"><span data-stu-id="dea1a-128">We are dynamically building several links for each displayed product.</span></span>

<span data-ttu-id="dea1a-129">Ponadto aby przetestować własnych nowej strony należy utworzyć struktury katalogu dla produktu katalog obrazów w następujący sposób.</span><span class="sxs-lookup"><span data-stu-id="dea1a-129">Also, before we test own new page we need to create the directory structure for the product catalog images as follows.</span></span>

![](tailspin-spyworks-part-4/_static/image1.png)

<span data-ttu-id="dea1a-130">Po zatwierdzeniu dostępne obrazy naszego produktu można było przetestować naszą stronę produktu listy.</span><span class="sxs-lookup"><span data-stu-id="dea1a-130">Once our product images are accessible we can test our product list page.</span></span>

![](tailspin-spyworks-part-4/_static/image5.jpg)

<span data-ttu-id="dea1a-131">Na stronie głównej witryny kliknij jeden z linków listy kategorii.</span><span class="sxs-lookup"><span data-stu-id="dea1a-131">From the site's home page, click on one of the Category List Links.</span></span>

![](tailspin-spyworks-part-4/_static/image6.jpg)

<span data-ttu-id="dea1a-132">Teraz musimy zaimplementować stronę ProductDetials.apsx oraz funkcje AddToCart.</span><span class="sxs-lookup"><span data-stu-id="dea1a-132">Now we need to implement the ProductDetials.apsx page and the AddToCart functionality.</span></span>

<span data-ttu-id="dea1a-133">Użyj pliku -&gt;nowy, aby utworzyć nazwę strony ProductDetails.aspx przy użyciu witryny strony wzorcowej, jak robiliśmy wcześniej.</span><span class="sxs-lookup"><span data-stu-id="dea1a-133">Use File-&gt;New to create a page name ProductDetails.aspx using the site Master Page as we did previously.</span></span>

<span data-ttu-id="dea1a-134">Ponownie używamy formantu obiektu EntityDataSource uzyskać dostęp do określonego produktu rekordu w bazie danych i użyjemy kontrolkę ASP.NET FormView do wyświetlania danych produktu w następujący sposób.</span><span class="sxs-lookup"><span data-stu-id="dea1a-134">We will again use an EntityDataSource control to access the specific product record in the database and we will use an ASP.NET FormView control to display the product data as follows.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-4/samples/sample3.aspx)]

<span data-ttu-id="dea1a-135">Nie martw się, jeśli formatowanie wygląda nieco Rady do Ciebie.</span><span class="sxs-lookup"><span data-stu-id="dea1a-135">Don't worry if the formatting looks a bit funny to you.</span></span> <span data-ttu-id="dea1a-136">Kod znaczników powyżej pozostawia miejsce w układzie wyświetlania z kilku funkcji, które będą później wprowadzania.</span><span class="sxs-lookup"><span data-stu-id="dea1a-136">The markup above leaves room in the display layout for a couple of features we'll implement later on.</span></span>

<span data-ttu-id="dea1a-137">Koszyku będzie reprezentować bardziej złożonej logice w naszej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="dea1a-137">The Shopping Cart will represent the more complex logic in our application.</span></span> <span data-ttu-id="dea1a-138">Aby rozpocząć pracę, należy użyć pliku -&gt;nowy, aby utworzyć stronę o nazwie MyShoppingCart.aspx.</span><span class="sxs-lookup"><span data-stu-id="dea1a-138">To get started, use File-&gt;New to create a page called MyShoppingCart.aspx.</span></span>

<span data-ttu-id="dea1a-139">Należy pamiętać, że firma Microsoft nie wybierania nazwy ShoppingCart.aspx.</span><span class="sxs-lookup"><span data-stu-id="dea1a-139">Note that we are not choosing the name ShoppingCart.aspx.</span></span>

<span data-ttu-id="dea1a-140">Nasze baza danych zawiera tabelę o nazwie "ShoppingCart".</span><span class="sxs-lookup"><span data-stu-id="dea1a-140">Our database contains a table named "ShoppingCart".</span></span> <span data-ttu-id="dea1a-141">Gdy firma Microsoft wygenerowany modelu Entity Data Model klasy został utworzony dla każdej tabeli w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="dea1a-141">When we generated an Entity Data Model a class was created for each table in the database.</span></span> <span data-ttu-id="dea1a-142">W związku z tym modelu Entity Data Model wygenerowane klasy jednostki o nazwie "ShoppingCart".</span><span class="sxs-lookup"><span data-stu-id="dea1a-142">Therefore, the Entity Data Model generated an Entity Class named "ShoppingCart".</span></span> <span data-ttu-id="dea1a-143">Firma Microsoft może edytować model, tak, aby firma Microsoft może używać tej nazwy naszych stosowania koszyka zakupów lub jej rozszerzenia na naszych potrzeb, ale firma Microsoft będzie wybierze zamiast tego po prostu wybierz nazwę, która pozwoli uniknąć konfliktu.</span><span class="sxs-lookup"><span data-stu-id="dea1a-143">We could edit the model so that we could use that name for our shopping cart implementation or extend it for our needs, but we will opt instead to simply slect a name that will avoid the conflict.</span></span>

<span data-ttu-id="dea1a-144">Warto również zauważyć, że firma Microsoft będzie Tworzenie prostego koszyk i osadzanie logiki koszyka zakupów z ekranem koszyka zakupów.</span><span class="sxs-lookup"><span data-stu-id="dea1a-144">It's also worth noting that we will be creating a simple shopping cart and embedding the shopping cart logic with the shopping cart display.</span></span> <span data-ttu-id="dea1a-145">Firma Microsoft może też do zaimplementowania naszego koszyka sklepowego całkowicie osobnej warstwie Business.</span><span class="sxs-lookup"><span data-stu-id="dea1a-145">We might also choose to implement our shopping cart in a completely separate Business Layer.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="dea1a-146">[Poprzednie](tailspin-spyworks-part-3.md)
> [dalej](tailspin-spyworks-part-5.md)</span><span class="sxs-lookup"><span data-stu-id="dea1a-146">[Previous](tailspin-spyworks-part-3.md)
[Next](tailspin-spyworks-part-5.md)</span></span>
