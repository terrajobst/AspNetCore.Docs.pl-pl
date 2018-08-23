---
uid: web-forms/overview/presenting-and-managing-data/model-binding/using-query-string-values-to-retrieve-data
title: Przy użyciu wartości ciągu zapytania do filtrowania danych przy użyciu wiązania modelu web forms | Dokumentacja firmy Microsoft
author: tfitzmac
description: W tej serii samouczków pokazano podstawowych aspektów projektu formularzy sieci Web ASP.NET przy użyciu wiązania modelu. Wiązanie modelu sprawia, że dane interakcji więcej proste —...
ms.author: riande
ms.date: 02/27/2014
ms.assetid: b90978bd-795d-4871-9ade-1671caff5730
msc.legacyurl: /web-forms/overview/presenting-and-managing-data/model-binding/using-query-string-values-to-retrieve-data
msc.type: authoredcontent
ms.openlocfilehash: fc4ec64cf583f25eca6795f7ff9215f025170054
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/16/2018
ms.locfileid: "41756591"
---
<a name="using-query-string-values-to-filter-data-with-model-binding-and-web-forms"></a><span data-ttu-id="99aed-104">Za pomocą wartości ciągu zapytania do filtrowania danych z wiązania modelu i formularzy sieci web</span><span class="sxs-lookup"><span data-stu-id="99aed-104">Using query string values to filter data with model binding and web forms</span></span>
====================
<span data-ttu-id="99aed-105">przez [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="99aed-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="99aed-106">W tej serii samouczków pokazano podstawowych aspektów projektu formularzy sieci Web ASP.NET przy użyciu wiązania modelu.</span><span class="sxs-lookup"><span data-stu-id="99aed-106">This tutorial series demonstrates basic aspects of using model binding with an ASP.NET Web Forms project.</span></span> <span data-ttu-id="99aed-107">Wiązanie modelu sprawia, że dane interakcji prostszą niż rozwiązywania problemów związanych z danymi obiektów źródła (takich jak kontrolki ObjectDataSource lub SqlDataSource).</span><span class="sxs-lookup"><span data-stu-id="99aed-107">Model binding makes data interaction more straight-forward than dealing with data source objects (such as ObjectDataSource or SqlDataSource).</span></span> <span data-ttu-id="99aed-108">Ta seria rozpoczyna się od wprowadzające informacje i przenosi do bardziej zaawansowanych pojęciach w kolejnych samouczkach.</span><span class="sxs-lookup"><span data-stu-id="99aed-108">This series starts with introductory material and moves to more advanced concepts in later tutorials.</span></span>
> 
> <span data-ttu-id="99aed-109">W tym samouczku pokazano, jak przekazać wartości ciągu zapytania i wartości można używać do pobierania danych przy użyciu wiązania modelu.</span><span class="sxs-lookup"><span data-stu-id="99aed-109">This tutorial shows how to pass a value in the query string and use that value to retrieve data through model binding.</span></span>
> 
> <span data-ttu-id="99aed-110">Ten samouczek opiera się na projekt utworzony w [wcześniej](retrieving-data.md) części tej serii.</span><span class="sxs-lookup"><span data-stu-id="99aed-110">This tutorial builds on the project created in the [earlier](retrieving-data.md) parts of the series.</span></span>
> 
> <span data-ttu-id="99aed-111">Możesz [Pobierz](https://go.microsoft.com/fwlink/?LinkId=286116) kompletnego projektu w języku C# lub VB.</span><span class="sxs-lookup"><span data-stu-id="99aed-111">You can [download](https://go.microsoft.com/fwlink/?LinkId=286116) the complete project in C# or VB.</span></span> <span data-ttu-id="99aed-112">Kod do pobrania w programach Visual Studio 2012 lub Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="99aed-112">The downloadable code works with either Visual Studio 2012 or Visual Studio 2013.</span></span> <span data-ttu-id="99aed-113">Używa szablonu programu Visual Studio 2012, który różni się nieco od szablonu programu Visual Studio 2013, przedstawione w tym samouczku.</span><span class="sxs-lookup"><span data-stu-id="99aed-113">It uses the Visual Studio 2012 template, which is slightly different than the Visual Studio 2013 template shown in this tutorial.</span></span>


## <a name="what-youll-build"></a><span data-ttu-id="99aed-114">Będziesz tworzyć</span><span class="sxs-lookup"><span data-stu-id="99aed-114">What you'll build</span></span>

<span data-ttu-id="99aed-115">W ramach tego samouczka należy:</span><span class="sxs-lookup"><span data-stu-id="99aed-115">In this tutorial, you'll:</span></span>

1. <span data-ttu-id="99aed-116">Dodaj nową stronę, aby wyświetlić zarejestrowane kursy dla uczniów lub studentów</span><span class="sxs-lookup"><span data-stu-id="99aed-116">Add a new page to show the enrolled courses for a student</span></span>
2. <span data-ttu-id="99aed-117">Pobieranie zarejestrowanych kursy dla wybranej uczniów lub studentów, na podstawie wartości w ciągu zapytania</span><span class="sxs-lookup"><span data-stu-id="99aed-117">Retrieve the enrolled courses for the selected student based on a value in the query string</span></span>
3. <span data-ttu-id="99aed-118">Dodać hiperłącze z wartością ciągu zapytania z widoku siatki do nowej strony</span><span class="sxs-lookup"><span data-stu-id="99aed-118">Add a hyperlink with a query string value from the grid view to the new page</span></span>

<span data-ttu-id="99aed-119">Kroki opisane w tym samouczku są podobne do demonstrującego we wcześniejszych przykładach [samouczek](sorting-paging-and-filtering-data.md) celu przefiltrowania uczniów wyświetlane, w oparciu o wybór użytkownika na liście rozwijanej.</span><span class="sxs-lookup"><span data-stu-id="99aed-119">The steps in this tutorial are fairly similar to what you did in the earlier [tutorial](sorting-paging-and-filtering-data.md) to filter the displayed students based on the user selection in a drop down list.</span></span> <span data-ttu-id="99aed-120">W tym samouczku użyto **kontroli** atrybutu w metodzie wybierz, aby określić, że wartość parametru pochodzi z formantu.</span><span class="sxs-lookup"><span data-stu-id="99aed-120">In that tutorial, you used the **Control** attribute in the select method to specify that the parameter value comes from a control.</span></span> <span data-ttu-id="99aed-121">W tym samouczku użyjesz **QueryString** atrybutu w metodzie select, aby określić, że wartość parametru pochodzi z ciągu zapytania.</span><span class="sxs-lookup"><span data-stu-id="99aed-121">In this tutorial, you'll use the **QueryString** attribute in the select method to specify that the parameter value comes from the query string.</span></span>

## <a name="add-new-page-for-displaying-a-students-courses"></a><span data-ttu-id="99aed-122">Dodaj nową stronę do wyświetlania kursy studenta</span><span class="sxs-lookup"><span data-stu-id="99aed-122">Add new page for displaying a student's courses</span></span>

<span data-ttu-id="99aed-123">Dodaj nowy formularz sieci web używający strony wzorcowej Site.master i nazwij stronę **kursów**.</span><span class="sxs-lookup"><span data-stu-id="99aed-123">Add a new web form that uses the Site.master master page, and name the page **Courses**.</span></span>

<span data-ttu-id="99aed-124">W **Courses.aspx** plików, Dodawanie widoku siatki, aby wyświetlić kursy dla wybranej uczniów lub studentów.</span><span class="sxs-lookup"><span data-stu-id="99aed-124">In the **Courses.aspx** file, add a grid view to display the courses for the selected student.</span></span>

[!code-aspx[Main](using-query-string-values-to-retrieve-data/samples/sample1.aspx)]

## <a name="define-the-select-method"></a><span data-ttu-id="99aed-125">Definiowanie metody select</span><span class="sxs-lookup"><span data-stu-id="99aed-125">Define the select method</span></span>

<span data-ttu-id="99aed-126">W **Courses.aspx.cs**, dodasz wybierz metodę o nazwie określonej w widoku siatki **metody SelectMethod** właściwości.</span><span class="sxs-lookup"><span data-stu-id="99aed-126">In **Courses.aspx.cs**, you will add the select method with the name you specified in the grid view's **SelectMethod** property.</span></span> <span data-ttu-id="99aed-127">W tej metodzie będzie zdefiniować zapytanie do pobierania kursy studenta i określić, że parametr pochodzi z ciągiem zapytania z taką samą nazwę jak parametr.</span><span class="sxs-lookup"><span data-stu-id="99aed-127">In that method, you'll define the query for retrieving a student's courses, and specify that the parameter comes from a query string value with the same name as the parameter.</span></span>

<span data-ttu-id="99aed-128">Najpierw należy dodać następujące **przy użyciu** instrukcji.</span><span class="sxs-lookup"><span data-stu-id="99aed-128">First, you must add the following **using** statements.</span></span>

[!code-csharp[Main](using-query-string-values-to-retrieve-data/samples/sample2.cs)]

<span data-ttu-id="99aed-129">Następnie dodaj następujący kod do Courses.aspx.cs:</span><span class="sxs-lookup"><span data-stu-id="99aed-129">Then, add the following code to Courses.aspx.cs:</span></span>

[!code-csharp[Main](using-query-string-values-to-retrieve-data/samples/sample3.cs)]

<span data-ttu-id="99aed-130">Atrybut QueryString oznacza, że wartość ciągu kwerendy o nazwie StudentID jest automatycznie przypisywana do parametru w przypadku tej metody.</span><span class="sxs-lookup"><span data-stu-id="99aed-130">The QueryString attribute means that a query string value named StudentID is automatically assigned to the parameter in this method.</span></span>

## <a name="add-hyperlink-with-query-string-value"></a><span data-ttu-id="99aed-131">Dodawanie hiperlinku z wartością ciągu zapytania</span><span class="sxs-lookup"><span data-stu-id="99aed-131">Add hyperlink with query string value</span></span>

<span data-ttu-id="99aed-132">W widoku siatki na Students.aspx zostaną dodane pola hiperłącze, który stanowi łącze do nowej strony kursów.</span><span class="sxs-lookup"><span data-stu-id="99aed-132">In the grid view on Students.aspx, you will add a hyperlink field that links to your new Courses page.</span></span> <span data-ttu-id="99aed-133">Hiperłącze będzie zawierać wartość ciągu zapytania w identyfikatorze studenta.</span><span class="sxs-lookup"><span data-stu-id="99aed-133">The hyperlink will include a query string value with the student's id.</span></span>

<span data-ttu-id="99aed-134">W Students.aspx Dodaj następujące pola do kolumny w widoku siatki, tuż poniżej pola, łączna liczba kredytów systemu.</span><span class="sxs-lookup"><span data-stu-id="99aed-134">In Students.aspx, add the following field to the grid view columns just below the field for Total Credits.</span></span>

[!code-aspx[Main](using-query-string-values-to-retrieve-data/samples/sample4.aspx?highlight=7-8)]

<span data-ttu-id="99aed-135">Uruchom aplikację i zwróć uwagę, że w widoku siatki teraz link kursów.</span><span class="sxs-lookup"><span data-stu-id="99aed-135">Run the application and notice that the grid view now includes the Courses link.</span></span>

![Dodawanie hiperlinku](using-query-string-values-to-retrieve-data/_static/image1.png)

<span data-ttu-id="99aed-137">Po kliknięciu jednego z linków, zobaczysz ten uczniów zarejestrowane kursów.</span><span class="sxs-lookup"><span data-stu-id="99aed-137">When you click one of the links, you'll see that student's enrolled courses.</span></span>

![Pokaż kursy](using-query-string-values-to-retrieve-data/_static/image2.png)

## <a name="conclusion"></a><span data-ttu-id="99aed-139">Wniosek</span><span class="sxs-lookup"><span data-stu-id="99aed-139">Conclusion</span></span>

<span data-ttu-id="99aed-140">W tym samouczku dodano łącze z wartością ciągu zapytania.</span><span class="sxs-lookup"><span data-stu-id="99aed-140">In this tutorial, you added a link with a query string value.</span></span> <span data-ttu-id="99aed-141">Ta wartość ciągu zapytania jest używany dla wartości parametru w metodzie select.</span><span class="sxs-lookup"><span data-stu-id="99aed-141">You used that query string value for the parameter value in the select method.</span></span>

<span data-ttu-id="99aed-142">W ciągu następnych [samouczek](adding-business-logic-layer.md), kod zostaną przeniesione z plików z kodem, warstwy logiki biznesowej i warstwy dostępu do danych.</span><span class="sxs-lookup"><span data-stu-id="99aed-142">In the next [tutorial](adding-business-logic-layer.md), you will move the code from the code-behind files into a business logic layer and a data access layer.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="99aed-143">[Poprzednie](integrating-jquery-ui.md)
> [dalej](adding-business-logic-layer.md)</span><span class="sxs-lookup"><span data-stu-id="99aed-143">[Previous](integrating-jquery-ui.md)
[Next](adding-business-logic-layer.md)</span></span>
