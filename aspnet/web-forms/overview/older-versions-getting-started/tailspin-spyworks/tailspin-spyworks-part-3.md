---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-3
title: 'Część 3: Układ i Menu kategorii | Dokumentacja firmy Microsoft'
author: JoeStagner
description: W tej serii samouczków zawiera szczegóły wszystkich kroków kompilacji Przykładowa aplikacja Tailspin Spyworks. Część 3 obejmuje dodawanie układ i menu kategorii.
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/21/2010
ms.topic: article
ms.assetid: 94ea1a70-a9bc-4241-8f36-08366d64bab9
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-3
msc.type: authoredcontent
ms.openlocfilehash: 72642610c203127b431e03214b2f1bc85a85b5fb
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37366650"
---
<a name="part-3-layout-and-category-menu"></a><span data-ttu-id="4b56e-104">Część 3: Układ i Menu kategorii</span><span class="sxs-lookup"><span data-stu-id="4b56e-104">Part 3: Layout and Category Menu</span></span>
====================
<span data-ttu-id="4b56e-105">przez [Stagner Jan](https://github.com/JoeStagner)</span><span class="sxs-lookup"><span data-stu-id="4b56e-105">by [Joe Stagner](https://github.com/JoeStagner)</span></span>

> <span data-ttu-id="4b56e-106">Tailspin Spyworks pokazuje, jak bardzo łatwo jest tworzyć zaawansowane, skalowalne aplikacje dla platformy .NET.</span><span class="sxs-lookup"><span data-stu-id="4b56e-106">Tailspin Spyworks demonstrates how extraordinarily simple it is to create powerful, scalable applications for the .NET platform.</span></span> <span data-ttu-id="4b56e-107">Przedstawia on poza sposób użycia wspaniałych nowych funkcjach w ASP.NET 4 do tworzenia sklep online, m.in. zakupy wyewidencjonowanie i Administracja.</span><span class="sxs-lookup"><span data-stu-id="4b56e-107">It shows off how to use the great new features in ASP.NET 4 to build an online store, including shopping, checkout, and administration.</span></span>
> 
> <span data-ttu-id="4b56e-108">W tej serii samouczków zawiera szczegóły wszystkich kroków kompilacji Przykładowa aplikacja Tailspin Spyworks.</span><span class="sxs-lookup"><span data-stu-id="4b56e-108">This tutorial series details all of the steps taken to build the Tailspin Spyworks sample application.</span></span> <span data-ttu-id="4b56e-109">Część 3 obejmuje dodawanie układ i menu kategorii.</span><span class="sxs-lookup"><span data-stu-id="4b56e-109">Part 3 covers adding layout and a category menu.</span></span>


## <a id="_Toc260221669"></a>  <span data-ttu-id="4b56e-110">Dodanie niektórych układ i Menu kategorii</span><span class="sxs-lookup"><span data-stu-id="4b56e-110">Adding Some Layout and a Category Menu</span></span>

<span data-ttu-id="4b56e-111">Strona wzorcowa naszej witryny polega na dodaniu div kolumny po lewej stronie, która będzie zawierać nasze menu kategorii produktów.</span><span class="sxs-lookup"><span data-stu-id="4b56e-111">In our site master page we'll add a div for the left side column that will contain our product category menu.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-3/samples/sample1.aspx)]

<span data-ttu-id="4b56e-112">Należy pamiętać, że żądane wyrównanie i inne elementy formatowania będzie świadczona przez klasy CSS, która dodaliśmy do naszego pliku Style.css.</span><span class="sxs-lookup"><span data-stu-id="4b56e-112">Note that the desired aligning and other formatting will be provided by the CSS class that we added to our Style.css file.</span></span>

[!code-css[Main](tailspin-spyworks-part-3/samples/sample2.css)]

<span data-ttu-id="4b56e-113">Menu kategorii produktu zostanie dynamicznie utworzony w czasie wykonywania, badając Commerce bazy danych dla istniejącej kategorii produktów oraz tworzenie elementów menu i odpowiadające łączy.</span><span class="sxs-lookup"><span data-stu-id="4b56e-113">The product category menu will be dynamically created at runtime by querying the Commerce database for existing product categories and creating the menu items and corresponding links.</span></span>

<span data-ttu-id="4b56e-114">W tym celu użyjemy dwóch ASP. Formanty zaawansowanych danych przez sieć.</span><span class="sxs-lookup"><span data-stu-id="4b56e-114">To accomplish this we will use two of ASP.NET's powerful data controls.</span></span> <span data-ttu-id="4b56e-115">Formantem "Źródła danych jednostki" i "ListView".</span><span class="sxs-lookup"><span data-stu-id="4b56e-115">The "Entity Data Source" control and the "ListView" control.</span></span>

![](tailspin-spyworks-part-3/_static/image1.jpg)

<span data-ttu-id="4b56e-116">Teraz przejdź do "W widoku projektu" i umożliwia konfigurowanie kontrolek naszych pomocników.</span><span class="sxs-lookup"><span data-stu-id="4b56e-116">Let's switch to "Design View" and use the helpers to configure our controls.</span></span>

![](tailspin-spyworks-part-3/_static/image2.jpg)

<span data-ttu-id="4b56e-117">Teraz ustaw właściwość Identyfikatora EntityDataSource do zewnętrznego źródła danych\_kategorii\_Menu i kliknij przycisk "Konfiguruj źródła danych".</span><span class="sxs-lookup"><span data-stu-id="4b56e-117">Let's set the EntityDataSource ID property to EDS\_Category\_Menu and click on "Configure Data Source".</span></span>

![](tailspin-spyworks-part-3/_static/image3.jpg)

<span data-ttu-id="4b56e-118">Wybierz połączenie CommerceEntities, który został utworzony dla nas podczas tworzenia modelu źródła danych jednostki dla naszych Commerce bazy danych, a następnie kliknij przycisk "Dalej".</span><span class="sxs-lookup"><span data-stu-id="4b56e-118">Select the CommerceEntities Connection that was created for us when we created the Entity Data Source Model for our Commerce Database and click "Next".</span></span>

![](tailspin-spyworks-part-3/_static/image4.jpg)

<span data-ttu-id="4b56e-119">Wybierz jednostki "Kategorie" Nazwa zestawu i pozostaw resztę opcji jako domyślny.</span><span class="sxs-lookup"><span data-stu-id="4b56e-119">Select the "Categories" Entity set name and leave the rest of the options as default.</span></span> <span data-ttu-id="4b56e-120">Kliknij przycisk "Zakończ".</span><span class="sxs-lookup"><span data-stu-id="4b56e-120">Click "Finish".</span></span>

<span data-ttu-id="4b56e-121">Teraz możemy ustawić właściwość identyfikator wystąpienie kontrolki ListView, który możemy umieścić na naszej stronie ListView\_ProductsMenu i uaktywnić jego pomocy.</span><span class="sxs-lookup"><span data-stu-id="4b56e-121">Now let's set the ID property of the ListView control instance that we placed on our page to ListView\_ProductsMenu and activate its helper.</span></span>

![](tailspin-spyworks-part-3/_static/image5.jpg)

<span data-ttu-id="4b56e-122">Mimo że moglibyśmy użyć różnych opcji kontroli do formatowania wyświetlania elementu danych i formatowania, nasze menu tworzenia będzie wymagać tylko proste znaczników, dzięki czemu możemy wprowadzić kod w widoku źródła.</span><span class="sxs-lookup"><span data-stu-id="4b56e-122">Though we could use control options to format the data item display and formatting, our menu creation will only require simple markup so we will enter the code in the source view.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-3/samples/sample3.aspx)]

<span data-ttu-id="4b56e-123">Należy pamiętać, instrukcji "Eval": &lt;% # Eval("CategoryName") %&gt;</span><span class="sxs-lookup"><span data-stu-id="4b56e-123">Please note the "Eval" statement : &lt;%# Eval("CategoryName") %&gt;</span></span>

<span data-ttu-id="4b56e-124">Składnia ASP.NET &lt;% # %&gt; jest Konwencja skrót, który powoduje, że środowisko uruchomieniowe do wykonania, niezależnie od rodzaju znajduje się w obrębie i zapisuje wyniki "w wierszu".</span><span class="sxs-lookup"><span data-stu-id="4b56e-124">The ASP.NET syntax &lt;%# %&gt; is a shorthand convention that instructs the runtime to execute whatever is contained within and output the results "in Line".</span></span>

<span data-ttu-id="4b56e-125">Instrukcja Eval("CategoryName") powoduje, że, bieżącego wpisu w powiązanej kolekcji elementów danych, pobrać wartości nazw elementów modelu Entity "CatagoryName".</span><span class="sxs-lookup"><span data-stu-id="4b56e-125">The statement Eval("CategoryName") instructs that, for the current entry in the bound collection of data items, fetch the value of the Entity Model item names "CatagoryName".</span></span> <span data-ttu-id="4b56e-126">Jest to zwarta składnia bardzo zaawansowaną funkcją.</span><span class="sxs-lookup"><span data-stu-id="4b56e-126">This is concise syntax for a very powerful feature.</span></span>

<span data-ttu-id="4b56e-127">Uruchomimy aplikację teraz.</span><span class="sxs-lookup"><span data-stu-id="4b56e-127">Lets run the application now.</span></span>

![](tailspin-spyworks-part-3/_static/image6.jpg)

<span data-ttu-id="4b56e-128">Pamiętaj, że nasze menu kategorii produktu są wyświetlane i kiedy jednak umieścimy kursor nad jednym z elementów menu kategorii widać prowadzi link element menu na stronie mamy jeszcze do wdrożenia o nazwie ProductsList.aspx a, stworzyliśmy argumentu ciągu zapytania dynamicznego, zawiera  Identyfikator kategorii.</span><span class="sxs-lookup"><span data-stu-id="4b56e-128">Note that our product category menu is now displayed and when we hover over one of the category menu items we can see the menu item link points to a page we have yet to implement named ProductsList.aspx and that we have built a dynamic query string argument that contains the category id.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="4b56e-129">[Poprzednie](tailspin-spyworks-part-2.md)
> [dalej](tailspin-spyworks-part-4.md)</span><span class="sxs-lookup"><span data-stu-id="4b56e-129">[Previous](tailspin-spyworks-part-2.md)
[Next](tailspin-spyworks-part-4.md)</span></span>
