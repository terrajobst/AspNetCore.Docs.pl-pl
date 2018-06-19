---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-3
title: 'Część 3: Układ i Menu kategorii | Dokumentacja firmy Microsoft'
author: JoeStagner
description: Ta seria samouczek zawiera szczegóły dotyczące wszystkich kroków kompilacji Tailspin Spyworks przykładowej aplikacji. Część 3 obejmuje dodawanie układ i menu kategorii.
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/21/2010
ms.topic: article
ms.assetid: 94ea1a70-a9bc-4241-8f36-08366d64bab9
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-3
msc.type: authoredcontent
ms.openlocfilehash: 27a493173b03f813ee3dcbbfafd8bc52fb0b9771
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
ms.locfileid: "30882091"
---
<a name="part-3-layout-and-category-menu"></a><span data-ttu-id="0f615-104">Część 3: Układ i Menu kategorii</span><span class="sxs-lookup"><span data-stu-id="0f615-104">Part 3: Layout and Category Menu</span></span>
====================
<span data-ttu-id="0f615-105">przez [Stagner Jan](https://github.com/JoeStagner)</span><span class="sxs-lookup"><span data-stu-id="0f615-105">by [Joe Stagner](https://github.com/JoeStagner)</span></span>

> <span data-ttu-id="0f615-106">Tailspin Spyworks pokazano, jak bardzo proste jest tworzenie zaawansowanych, skalowalnych aplikacji dla platformy .NET.</span><span class="sxs-lookup"><span data-stu-id="0f615-106">Tailspin Spyworks demonstrates how extraordinarily simple it is to create powerful, scalable applications for the .NET platform.</span></span> <span data-ttu-id="0f615-107">Przedstawia on poza jak nowe, fantastyczne funkcje programu ASP.NET 4 do tworzenia sklepu online, łącznie z zakupów, wyewidencjonowania i administracji.</span><span class="sxs-lookup"><span data-stu-id="0f615-107">It shows off how to use the great new features in ASP.NET 4 to build an online store, including shopping, checkout, and administration.</span></span>
> 
> <span data-ttu-id="0f615-108">Ta seria samouczek zawiera szczegóły dotyczące wszystkich kroków kompilacji Tailspin Spyworks przykładowej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="0f615-108">This tutorial series details all of the steps taken to build the Tailspin Spyworks sample application.</span></span> <span data-ttu-id="0f615-109">Część 3 obejmuje dodawanie układ i menu kategorii.</span><span class="sxs-lookup"><span data-stu-id="0f615-109">Part 3 covers adding layout and a category menu.</span></span>


## <a id="_Toc260221669"></a>  <span data-ttu-id="0f615-110">Dodanie niektórych układ i Menu kategorii</span><span class="sxs-lookup"><span data-stu-id="0f615-110">Adding Some Layout and a Category Menu</span></span>

<span data-ttu-id="0f615-111">W naszym strony wzorcowej witryny dodamy div kolumny po lewej stronie, która będzie zawierać naszych menu Kategoria produktu.</span><span class="sxs-lookup"><span data-stu-id="0f615-111">In our site master page we'll add a div for the left side column that will contain our product category menu.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-3/samples/sample1.aspx)]

<span data-ttu-id="0f615-112">Należy pamiętać, że inne formatowanie i odpowiednie dopasowanie będą udostępniane przez klasy CSS, która dodane do naszych pliku Style.css.</span><span class="sxs-lookup"><span data-stu-id="0f615-112">Note that the desired aligning and other formatting will be provided by the CSS class that we added to our Style.css file.</span></span>

[!code-css[Main](tailspin-spyworks-part-3/samples/sample2.css)]

<span data-ttu-id="0f615-113">Menu Kategoria produktu zostanie dynamicznie utworzony w czasie wykonywania, badając Commerce bazy danych dla istniejącej kategorii produktów i tworzenie elementów menu i odpowiadającego łączy.</span><span class="sxs-lookup"><span data-stu-id="0f615-113">The product category menu will be dynamically created at runtime by querying the Commerce database for existing product categories and creating the menu items and corresponding links.</span></span>

<span data-ttu-id="0f615-114">W tym celu użyjemy dwa ASP. Formanty zaawansowanych danych w sieci.</span><span class="sxs-lookup"><span data-stu-id="0f615-114">To accomplish this we will use two of ASP.NET's powerful data controls.</span></span> <span data-ttu-id="0f615-115">Formantem "Entity Data Source" i "W elemencie ListView".</span><span class="sxs-lookup"><span data-stu-id="0f615-115">The "Entity Data Source" control and the "ListView" control.</span></span>

![](tailspin-spyworks-part-3/_static/image1.jpg)

<span data-ttu-id="0f615-116">Załóżmy Przełącz się do "Projekt" i pomocników umożliwiają konfigurowanie naszych kontrolki.</span><span class="sxs-lookup"><span data-stu-id="0f615-116">Let's switch to "Design View" and use the helpers to configure our controls.</span></span>

![](tailspin-spyworks-part-3/_static/image2.jpg)

<span data-ttu-id="0f615-117">Załóżmy ustawioną właściwość Identyfikatora obiektu EntityDataSource EDS\_kategorii\_Menu i kliknij pozycję "Konfigurowanie źródła danych".</span><span class="sxs-lookup"><span data-stu-id="0f615-117">Let's set the EntityDataSource ID property to EDS\_Category\_Menu and click on "Configure Data Source".</span></span>

![](tailspin-spyworks-part-3/_static/image3.jpg)

<span data-ttu-id="0f615-118">Wybierz połączenie CommerceEntities utworzonym dla nas podczas utworzyliśmy źródłowego modelu danych jednostki dla naszych Commerce bazy danych i kliknij przycisk "Dalej".</span><span class="sxs-lookup"><span data-stu-id="0f615-118">Select the CommerceEntities Connection that was created for us when we created the Entity Data Source Model for our Commerce Database and click "Next".</span></span>

![](tailspin-spyworks-part-3/_static/image4.jpg)

<span data-ttu-id="0f615-119">Wybierz nazwę zestawu jednostek "Kategorie" i pozostaw pozostałe opcje jako domyślny.</span><span class="sxs-lookup"><span data-stu-id="0f615-119">Select the "Categories" Entity set name and leave the rest of the options as default.</span></span> <span data-ttu-id="0f615-120">Kliknij przycisk "Zakończ".</span><span class="sxs-lookup"><span data-stu-id="0f615-120">Click "Finish".</span></span>

<span data-ttu-id="0f615-121">Teraz załóżmy ustawić właściwość Identyfikator wystąpienia formantu ListView, który możemy umieścić na naszą stronę do elementu ListView\_ProductsMenu i Aktywuj jego pomocy.</span><span class="sxs-lookup"><span data-stu-id="0f615-121">Now let's set the ID property of the ListView control instance that we placed on our page to ListView\_ProductsMenu and activate its helper.</span></span>

![](tailspin-spyworks-part-3/_static/image5.jpg)

<span data-ttu-id="0f615-122">Mimo że firma Microsoft można użyć opcji kontroli do formatowania wyświetlania elementu danych i formatowania, wszystkich naszych tworzenie menu będzie wymagać tylko proste znaczników, firma Microsoft będzie wprowadzenie kodu w widoku źródła.</span><span class="sxs-lookup"><span data-stu-id="0f615-122">Though we could use control options to format the data item display and formatting, our menu creation will only require simple markup so we will enter the code in the source view.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-3/samples/sample3.aspx)]

<span data-ttu-id="0f615-123">Należy pamiętać, instrukcji "Eval": &lt;% # % Eval("CategoryName")&gt;</span><span class="sxs-lookup"><span data-stu-id="0f615-123">Please note the "Eval" statement : &lt;%# Eval("CategoryName") %&gt;</span></span>

<span data-ttu-id="0f615-124">Składnia ASP.NET &lt;% # %&gt; Konwencji skrótowa, która sprawia, że środowisko uruchomieniowe można wykonać, jest zawarty w i zapisuje wyniki "w wierszu".</span><span class="sxs-lookup"><span data-stu-id="0f615-124">The ASP.NET syntax &lt;%# %&gt; is a shorthand convention that instructs the runtime to execute whatever is contained within and output the results "in Line".</span></span>

<span data-ttu-id="0f615-125">Instrukcja Eval("CategoryName") nakazuje, dla bieżącego wpisu w powiązanej kolekcji elementów danych, Pobierz wartość nazwy elementów modelu jednostki "CatagoryName".</span><span class="sxs-lookup"><span data-stu-id="0f615-125">The statement Eval("CategoryName") instructs that, for the current entry in the bound collection of data items, fetch the value of the Entity Model item names "CatagoryName".</span></span> <span data-ttu-id="0f615-126">To krótkie składnię bardzo zaawansowaną funkcją.</span><span class="sxs-lookup"><span data-stu-id="0f615-126">This is concise syntax for a very powerful feature.</span></span>

<span data-ttu-id="0f615-127">Umożliwia uruchamianie aplikacji teraz.</span><span class="sxs-lookup"><span data-stu-id="0f615-127">Lets run the application now.</span></span>

![](tailspin-spyworks-part-3/_static/image6.jpg)

<span data-ttu-id="0f615-128">Nasze menu Kategoria produktu jest teraz wyświetlany i po możemy umieść kursor nad jednym z elementów menu kategorii widzimy punktów menu element link do strony mamy jeszcze do zaimplementowania o nazwie ProductsList.aspx i że nawiązaliśmy argument ciągu zapytania dynamicznego który zawiera  Identyfikator kategorii.</span><span class="sxs-lookup"><span data-stu-id="0f615-128">Note that our product category menu is now displayed and when we hover over one of the category menu items we can see the menu item link points to a page we have yet to implement named ProductsList.aspx and that we have built a dynamic query string argument that contains the category id.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="0f615-129">[Poprzednie](tailspin-spyworks-part-2.md)
> [dalej](tailspin-spyworks-part-4.md)</span><span class="sxs-lookup"><span data-stu-id="0f615-129">[Previous](tailspin-spyworks-part-2.md)
[Next](tailspin-spyworks-part-4.md)</span></span>
