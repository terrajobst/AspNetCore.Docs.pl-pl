---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-4
title: 'Część 4: Wyświetlanie listy produktów | Dokumentacja firmy Microsoft'
author: JoeStagner
description: W tej serii samouczków zawiera szczegóły wszystkich kroków kompilacji Przykładowa aplikacja Tailspin Spyworks. Część 4 obejmuje tworzenie listy produktów z zysk GridView...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/21/2010
ms.topic: article
ms.assetid: 4fab47d5-a6ec-4fdc-91f0-651a093a24b9
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-4
msc.type: authoredcontent
ms.openlocfilehash: 929d88d747c25ca7c4f6f991421cb3aa9456aa45
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37368812"
---
<a name="part-4-listing-products"></a><span data-ttu-id="ca167-104">Część 4: Tworzenie listy produktów</span><span class="sxs-lookup"><span data-stu-id="ca167-104">Part 4: Listing Products</span></span>
====================
<span data-ttu-id="ca167-105">przez [Stagner Jan](https://github.com/JoeStagner)</span><span class="sxs-lookup"><span data-stu-id="ca167-105">by [Joe Stagner](https://github.com/JoeStagner)</span></span>

> <span data-ttu-id="ca167-106">Tailspin Spyworks pokazuje, jak bardzo łatwo jest tworzyć zaawansowane, skalowalne aplikacje dla platformy .NET.</span><span class="sxs-lookup"><span data-stu-id="ca167-106">Tailspin Spyworks demonstrates how extraordinarily simple it is to create powerful, scalable applications for the .NET platform.</span></span> <span data-ttu-id="ca167-107">Przedstawia on poza sposób użycia wspaniałych nowych funkcjach w ASP.NET 4 do tworzenia sklep online, m.in. zakupy wyewidencjonowanie i Administracja.</span><span class="sxs-lookup"><span data-stu-id="ca167-107">It shows off how to use the great new features in ASP.NET 4 to build an online store, including shopping, checkout, and administration.</span></span>
> 
> <span data-ttu-id="ca167-108">W tej serii samouczków zawiera szczegóły wszystkich kroków kompilacji Przykładowa aplikacja Tailspin Spyworks.</span><span class="sxs-lookup"><span data-stu-id="ca167-108">This tutorial series details all of the steps taken to build the Tailspin Spyworks sample application.</span></span> <span data-ttu-id="ca167-109">Część 4 obejmuje tworzenie listy produktów za pomocą kontrolki GridView.</span><span class="sxs-lookup"><span data-stu-id="ca167-109">Part 4 covers listing products with the GridView control.</span></span>


## <a id="_Toc260221670"></a>  <span data-ttu-id="ca167-110">Tworzenie listy produktów za pomocą kontrolki GridView</span><span class="sxs-lookup"><span data-stu-id="ca167-110">Listing Products with the GridView Control</span></span>

<span data-ttu-id="ca167-111">Zacznijmy implementacji naszą stronę ProductsList.aspx przez "Kliknięcie prawym przyciskiem myszy" na nasze rozwiązanie i wybierając pozycję "Dodaj" i "Nowy element".</span><span class="sxs-lookup"><span data-stu-id="ca167-111">Let's begin implementing our ProductsList.aspx page by "Right Clicking" on our solution and selecting "Add" and "New Item".</span></span>

![](tailspin-spyworks-part-4/_static/image1.jpg)

<span data-ttu-id="ca167-112">Wybierz pozycję "Formularza przy użyciu wzorca stronę internetową" i wprowadź nazwę strony ProductsList.aspx".</span><span class="sxs-lookup"><span data-stu-id="ca167-112">Choose "Web Form Using Master Page" and enter a page name of ProductsList.aspx".</span></span>

<span data-ttu-id="ca167-113">Kliknij przycisk "Dodaj".</span><span class="sxs-lookup"><span data-stu-id="ca167-113">Click "Add".</span></span>

![](tailspin-spyworks-part-4/_static/image2.jpg)

<span data-ttu-id="ca167-114">Następnie wybierz folder "Style", w którym firma Microsoft umieszczone na stronie Site.Master i wybierz ją z okna "Zawartość folderu".</span><span class="sxs-lookup"><span data-stu-id="ca167-114">Next choose the "Styles" folder where we placed the Site.Master page and select it from the "Contents of folder" window.</span></span>

![](tailspin-spyworks-part-4/_static/image3.jpg)

<span data-ttu-id="ca167-115">Kliknij przycisk "Ok", aby utworzyć stronę.</span><span class="sxs-lookup"><span data-stu-id="ca167-115">Click "Ok" to create the page.</span></span>

<span data-ttu-id="ca167-116">Naszej bazie danych jest wypełniana danymi produktów, jak pokazano poniżej.</span><span class="sxs-lookup"><span data-stu-id="ca167-116">Our database is populated with product data as seen below.</span></span>

![](tailspin-spyworks-part-4/_static/image4.jpg)

<span data-ttu-id="ca167-117">Po naszej stronie ponownie użyjemy źródła danych jednostki do dostępu do tych danych produktu, ale w tym wystąpieniu musimy wybrać jednostki produktu i należy ograniczyć elementy, które są zwracane tylko do tych dla wybranej kategorii.</span><span class="sxs-lookup"><span data-stu-id="ca167-117">After our page is created we'll again use an Entity Data Source to access that product data, but in this instance we need to select the Product Entities and we need to restrict the items that are returned to only those for the selected Category.</span></span>

<span data-ttu-id="ca167-118">W tym celu będzie o EntityDataSource automatycznej klauzuli WHERE i określamy będzie WhereParameter.</span><span class="sxs-lookup"><span data-stu-id="ca167-118">To accomplish this we'll tell the EntityDataSource to Auto Generate the WHERE clause and we'll specify the WhereParameter.</span></span>

<span data-ttu-id="ca167-119">Dowiesz się odwołać, podczas tworzenia elementów Menu w naszym "produktu kategorii Menu" dynamicznie zbudowaliśmy łącze, dodając CatagoryID do ciągu kwerendy dla każdego łącza.</span><span class="sxs-lookup"><span data-stu-id="ca167-119">You'll recall that when we created the Menu Items in our "Product Category Menu" we dynamically built the link by adding the CatagoryID to the QueryString for each link.</span></span> <span data-ttu-id="ca167-120">Firma Microsoft poinformuje źródła danych jednostki do wyprowadzenia parametr gdzie z tego parametru QueryString.</span><span class="sxs-lookup"><span data-stu-id="ca167-120">We will tell the Entity Data Source to derive the WHERE parameter from that QueryString parameter.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-4/samples/sample1.aspx)]

<span data-ttu-id="ca167-121">Następnie skonfigurujemy formantu ListView, aby wyświetlić listę produktów.</span><span class="sxs-lookup"><span data-stu-id="ca167-121">Next, we'll configure the ListView control to display a list of products.</span></span> <span data-ttu-id="ca167-122">Aby utworzyć zapewnienia optymalnego działania zakupów, firma Microsoft będzie compact kilka zwięzłe funkcji do każdego produktu wyświetlane w naszej ListVew.</span><span class="sxs-lookup"><span data-stu-id="ca167-122">To create an optimal shopping experience we'll compact several concise features into each individual product displayed in our ListVew.</span></span>

- <span data-ttu-id="ca167-123">Nazwa produktu będzie łącze do widoku szczegółów produktu.</span><span class="sxs-lookup"><span data-stu-id="ca167-123">The product name will be a link to the product's detail view.</span></span>
- <span data-ttu-id="ca167-124">Zostanie wyświetlona cena produktu.</span><span class="sxs-lookup"><span data-stu-id="ca167-124">The product's price will be displayed.</span></span>
- <span data-ttu-id="ca167-125">Obraz produktu będzie wyświetlany, i dynamicznie wybierzemy obrazu z katalogu katalog obrazów w naszej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="ca167-125">An image of the product will be displayed and we'll dynamically select the image from a catalog images directory in our application.</span></span>
- <span data-ttu-id="ca167-126">Firma Microsoft będzie zawierać link do natychmiast dodać określonego produktu do koszyka.</span><span class="sxs-lookup"><span data-stu-id="ca167-126">We will include a link to immediately add the specific product to the shopping cart.</span></span>

<span data-ttu-id="ca167-127">Oto znaczniki dla naszych wystąpienie kontrolki ListView.</span><span class="sxs-lookup"><span data-stu-id="ca167-127">Here is the markup for our ListView control instance.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-4/samples/sample2.aspx)]

<span data-ttu-id="ca167-128">Dynamicznie tworzymy kilka linków dla każdego produktu wyświetlane.</span><span class="sxs-lookup"><span data-stu-id="ca167-128">We are dynamically building several links for each displayed product.</span></span>

<span data-ttu-id="ca167-129">Ponadto przed przetestowanie własnych nową stronę musimy utworzyć katalog obrazów struktury katalogów dla produktu w następujący sposób.</span><span class="sxs-lookup"><span data-stu-id="ca167-129">Also, before we test own new page we need to create the directory structure for the product catalog images as follows.</span></span>

![](tailspin-spyworks-part-4/_static/image1.png)

<span data-ttu-id="ca167-130">Po nasze obrazy produktów są dostępne możemy przetestować naszą stronę listy produktu.</span><span class="sxs-lookup"><span data-stu-id="ca167-130">Once our product images are accessible we can test our product list page.</span></span>

![](tailspin-spyworks-part-4/_static/image5.jpg)

<span data-ttu-id="ca167-131">Na stronie głównej witryny kliknij jeden z linków listy kategorii.</span><span class="sxs-lookup"><span data-stu-id="ca167-131">From the site's home page, click on one of the Category List Links.</span></span>

![](tailspin-spyworks-part-4/_static/image6.jpg)

<span data-ttu-id="ca167-132">Teraz musimy zaimplementować stronę ProductDetials.apsx i funkcjonalność AddToCart.</span><span class="sxs-lookup"><span data-stu-id="ca167-132">Now we need to implement the ProductDetials.apsx page and the AddToCart functionality.</span></span>

<span data-ttu-id="ca167-133">Użyj pliku -&gt;nowy, aby utworzyć nazwę strony ProductDetails.aspx przy użyciu witryny stronę wzorcową, ile My mieliśmy wcześniej.</span><span class="sxs-lookup"><span data-stu-id="ca167-133">Use File-&gt;New to create a page name ProductDetails.aspx using the site Master Page as we did previously.</span></span>

<span data-ttu-id="ca167-134">Ponownie użyjemy kontrolkę EntityDataSource uzyskać dostęp do rekordu określonego produktu w bazie danych, a następnie użyjemy kontrolkę ASP.NET FormView do wyświetlania danych produktu w następujący sposób.</span><span class="sxs-lookup"><span data-stu-id="ca167-134">We will again use an EntityDataSource control to access the specific product record in the database and we will use an ASP.NET FormView control to display the product data as follows.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-4/samples/sample3.aspx)]

<span data-ttu-id="ca167-135">Nie martw się, jeśli formatowanie wygląda nieco Rady do Ciebie.</span><span class="sxs-lookup"><span data-stu-id="ca167-135">Don't worry if the formatting looks a bit funny to you.</span></span> <span data-ttu-id="ca167-136">Powyższe znaczników pozostawia miejsce w układzie wyświetlana przez kilka funkcji, które firma Microsoft będzie implementowana w późniejszym czasie na.</span><span class="sxs-lookup"><span data-stu-id="ca167-136">The markup above leaves room in the display layout for a couple of features we'll implement later on.</span></span>

<span data-ttu-id="ca167-137">Zawartość koszyka będzie reprezentować bardziej złożonej logiki w naszej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="ca167-137">The Shopping Cart will represent the more complex logic in our application.</span></span> <span data-ttu-id="ca167-138">Aby rozpocząć pracę, należy użyć pliku -&gt;nowy, aby utworzyć stronę o nazwie MyShoppingCart.aspx.</span><span class="sxs-lookup"><span data-stu-id="ca167-138">To get started, use File-&gt;New to create a page called MyShoppingCart.aspx.</span></span>

<span data-ttu-id="ca167-139">Należy pamiętać, że firma Microsoft nie wybierają nazwę ShoppingCart.aspx.</span><span class="sxs-lookup"><span data-stu-id="ca167-139">Note that we are not choosing the name ShoppingCart.aspx.</span></span>

<span data-ttu-id="ca167-140">Nasze baza danych zawiera tabelę o nazwie "ShoppingCart".</span><span class="sxs-lookup"><span data-stu-id="ca167-140">Our database contains a table named "ShoppingCart".</span></span> <span data-ttu-id="ca167-141">Gdy firma Microsoft wygenerowany model Entity Data Model klasy został utworzony dla każdej tabeli w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="ca167-141">When we generated an Entity Data Model a class was created for each table in the database.</span></span> <span data-ttu-id="ca167-142">W związku z tym modelu Entity Data Model wygenerować klasę jednostki o nazwie "ShoppingCart".</span><span class="sxs-lookup"><span data-stu-id="ca167-142">Therefore, the Entity Data Model generated an Entity Class named "ShoppingCart".</span></span> <span data-ttu-id="ca167-143">Firma Microsoft można edytować model, dzięki czemu firma Microsoft może używać tej nazwy dla naszej zakupów implementacji koszyka lub rozszerzania jej do naszych potrzeb, ale firma Microsoft będzie zoptymalizowany pod kątem zamiast tego można po prostu wybierz nazwę, która pozwoli uniknąć konfliktu.</span><span class="sxs-lookup"><span data-stu-id="ca167-143">We could edit the model so that we could use that name for our shopping cart implementation or extend it for our needs, but we will opt instead to simply slect a name that will avoid the conflict.</span></span>

<span data-ttu-id="ca167-144">Warto również zauważyć, że utworzymy prostą koszyka zakupów i osadzanie logikę koszyka zakupów z wyświetlaniem koszyka zakupów.</span><span class="sxs-lookup"><span data-stu-id="ca167-144">It's also worth noting that we will be creating a simple shopping cart and embedding the shopping cart logic with the shopping cart display.</span></span> <span data-ttu-id="ca167-145">Firma Microsoft może też zaimplementować naszego koszyka w oddzielną warstwy biznesowej.</span><span class="sxs-lookup"><span data-stu-id="ca167-145">We might also choose to implement our shopping cart in a completely separate Business Layer.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="ca167-146">[Poprzednie](tailspin-spyworks-part-3.md)
> [dalej](tailspin-spyworks-part-5.md)</span><span class="sxs-lookup"><span data-stu-id="ca167-146">[Previous](tailspin-spyworks-part-3.md)
[Next](tailspin-spyworks-part-5.md)</span></span>
