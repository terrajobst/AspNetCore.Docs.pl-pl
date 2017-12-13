---
uid: web-forms/overview/presenting-and-managing-data/model-binding/using-query-string-values-to-retrieve-data
title: "Przy użyciu wartości ciągu zapytania do filtrowania danych z powiązaniem modelu i sieci web forms | Dokumentacja firmy Microsoft"
author: tfitzmac
description: "Ten samouczek serii przedstawiono podstawowe aspekty projektu formularzy sieci Web ASP.NET przy użyciu wiązania modelu. Wiązania modelu sprawia, że dane interakcji więcej proste-..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/27/2014
ms.topic: article
ms.assetid: b90978bd-795d-4871-9ade-1671caff5730
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/presenting-and-managing-data/model-binding/using-query-string-values-to-retrieve-data
msc.type: authoredcontent
ms.openlocfilehash: 2e5328ccda019462163b984da3661f7322b738df
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
<a name="using-query-string-values-to-filter-data-with-model-binding-and-web-forms"></a><span data-ttu-id="e3573-104">Przy użyciu wartości ciągu zapytania do filtrowania danych z wiązania modelu i formularzy sieci web</span><span class="sxs-lookup"><span data-stu-id="e3573-104">Using query string values to filter data with model binding and web forms</span></span>
====================
<span data-ttu-id="e3573-105">przez [FitzMacken niestandardowy](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="e3573-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="e3573-106">Ten samouczek serii przedstawiono podstawowe aspekty projektu formularzy sieci Web ASP.NET przy użyciu wiązania modelu.</span><span class="sxs-lookup"><span data-stu-id="e3573-106">This tutorial series demonstrates basic aspects of using model binding with an ASP.NET Web Forms project.</span></span> <span data-ttu-id="e3573-107">Wiązania modelu sprawia, że dane interakcji więcej proste niż dotyczących danych obiekty źródła (na przykład element ObjectDataSource lub SqlDataSource).</span><span class="sxs-lookup"><span data-stu-id="e3573-107">Model binding makes data interaction more straight-forward than dealing with data source objects (such as ObjectDataSource or SqlDataSource).</span></span> <span data-ttu-id="e3573-108">Ta seria rozpoczyna się od wprowadzające informacje i przechodzi do bardziej zaawansowanych pojęcia w kolejnych samouczkach.</span><span class="sxs-lookup"><span data-stu-id="e3573-108">This series starts with introductory material and moves to more advanced concepts in later tutorials.</span></span>
> 
> <span data-ttu-id="e3573-109">W tym samouczku pokazano, jak przekazać wartości w ciągu zapytania i użycie tej wartości można pobrać danych za pośrednictwem wiązania modelu.</span><span class="sxs-lookup"><span data-stu-id="e3573-109">This tutorial shows how to pass a value in the query string and use that value to retrieve data through model binding.</span></span>
> 
> <span data-ttu-id="e3573-110">W tym samouczku opiera się na projekcie utworzone w [wcześniej](retrieving-data.md) części serii.</span><span class="sxs-lookup"><span data-stu-id="e3573-110">This tutorial builds on the project created in the [earlier](retrieving-data.md) parts of the series.</span></span>
> 
> <span data-ttu-id="e3573-111">Możesz [Pobierz](https://go.microsoft.com/fwlink/?LinkId=286116) kompletnego projektu w języku C# lub VB.</span><span class="sxs-lookup"><span data-stu-id="e3573-111">You can [download](https://go.microsoft.com/fwlink/?LinkId=286116) the complete project in C# or VB.</span></span> <span data-ttu-id="e3573-112">Kod do pobrania współpracuje z programu Visual Studio 2012 lub Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="e3573-112">The downloadable code works with either Visual Studio 2012 or Visual Studio 2013.</span></span> <span data-ttu-id="e3573-113">Używa szablonu programu Visual Studio 2012, który jest nieco inne niż szablon programu Visual Studio 2013 przedstawiona w tym samouczku.</span><span class="sxs-lookup"><span data-stu-id="e3573-113">It uses the Visual Studio 2012 template, which is slightly different than the Visual Studio 2013 template shown in this tutorial.</span></span>


## <a name="what-youll-build"></a><span data-ttu-id="e3573-114">Będzie kompilacji</span><span class="sxs-lookup"><span data-stu-id="e3573-114">What you'll build</span></span>

<span data-ttu-id="e3573-115">W tym samouczku będziesz:</span><span class="sxs-lookup"><span data-stu-id="e3573-115">In this tutorial, you'll:</span></span>

1. <span data-ttu-id="e3573-116">Dodaj nową stronę do wyświetlenia kursy zarejestrowanych dla użytkowników domowych</span><span class="sxs-lookup"><span data-stu-id="e3573-116">Add a new page to show the enrolled courses for a student</span></span>
2. <span data-ttu-id="e3573-117">Pobieranie szkoleń w zarejestrowany dla wybranych dla użytkowników domowych, na podstawie wartości w ciągu zapytania</span><span class="sxs-lookup"><span data-stu-id="e3573-117">Retrieve the enrolled courses for the selected student based on a value in the query string</span></span>
3. <span data-ttu-id="e3573-118">Dodawanie hiperłącza z wartością ciągu zapytania w widoku siatki do nowej strony</span><span class="sxs-lookup"><span data-stu-id="e3573-118">Add a hyperlink with a query string value from the grid view to the new page</span></span>

<span data-ttu-id="e3573-119">Kroki opisane w tym samouczku są dość podobne do zagadnienia omówione w wcześniej [samouczek](sorting-paging-and-filtering-data.md) do filtrowania wyświetlanych studentów, na podstawie zaznaczenia użytkownika na liście rozwijanej.</span><span class="sxs-lookup"><span data-stu-id="e3573-119">The steps in this tutorial are fairly similar to what you did in the earlier [tutorial](sorting-paging-and-filtering-data.md) to filter the displayed students based on the user selection in a drop down list.</span></span> <span data-ttu-id="e3573-120">W tym samouczku, użyte **kontroli** atrybutu w metodzie select, aby określić, czy wartość parametru pochodzą z formantu.</span><span class="sxs-lookup"><span data-stu-id="e3573-120">In that tutorial, you used the **Control** attribute in the select method to specify that the parameter value comes from a control.</span></span> <span data-ttu-id="e3573-121">W tym samouczku użyjesz **QueryString** atrybutu w metodzie select, aby określić, że wartość parametru pochodzi z ciągu zapytania.</span><span class="sxs-lookup"><span data-stu-id="e3573-121">In this tutorial, you'll use the **QueryString** attribute in the select method to specify that the parameter value comes from the query string.</span></span>

## <a name="add-new-page-for-displaying-a-students-courses"></a><span data-ttu-id="e3573-122">Dodaj nową stronę do wyświetlania szkoleń dla użytkowników domowych</span><span class="sxs-lookup"><span data-stu-id="e3573-122">Add new page for displaying a student's courses</span></span>

<span data-ttu-id="e3573-123">Dodaj nowy formularz sieci web używający strony wzorcowej Site.master i nazwa strony **kursów**.</span><span class="sxs-lookup"><span data-stu-id="e3573-123">Add a new web form that uses the Site.master master page, and name the page **Courses**.</span></span>

<span data-ttu-id="e3573-124">W **Courses.aspx** plików, Dodaj do wyświetlenia kursów dla uczniów wybranego widoku siatki.</span><span class="sxs-lookup"><span data-stu-id="e3573-124">In the **Courses.aspx** file, add a grid view to display the courses for the selected student.</span></span>

[!code-aspx[Main](using-query-string-values-to-retrieve-data/samples/sample1.aspx)]

## <a name="define-the-select-method"></a><span data-ttu-id="e3573-125">Zdefiniuj wybierz — Metoda</span><span class="sxs-lookup"><span data-stu-id="e3573-125">Define the select method</span></span>

<span data-ttu-id="e3573-126">W **Courses.aspx.cs**, wybierz metodę o nazwie określonej w widoku siatki spowoduje dodanie **SelectMethod** właściwości.</span><span class="sxs-lookup"><span data-stu-id="e3573-126">In **Courses.aspx.cs**, you will add the select method with the name you specified in the grid view's **SelectMethod** property.</span></span> <span data-ttu-id="e3573-127">W tej metodzie zostanie Zdefiniuj kwerendę dla pobierania kursy studenta oraz określ, czy parametr pochodzą z wartości ciągu zapytania z taką samą nazwę jak parametr.</span><span class="sxs-lookup"><span data-stu-id="e3573-127">In that method, you'll define the query for retrieving a student's courses, and specify that the parameter comes from a query string value with the same name as the parameter.</span></span>

<span data-ttu-id="e3573-128">Najpierw należy dodać następujące **przy użyciu** instrukcje.</span><span class="sxs-lookup"><span data-stu-id="e3573-128">First, you must add the following **using** statements.</span></span>

[!code-csharp[Main](using-query-string-values-to-retrieve-data/samples/sample2.cs)]

<span data-ttu-id="e3573-129">Następnie dodaj następujący kod do Courses.aspx.cs:</span><span class="sxs-lookup"><span data-stu-id="e3573-129">Then, add the following code to Courses.aspx.cs:</span></span>

[!code-csharp[Main](using-query-string-values-to-retrieve-data/samples/sample3.cs)]

<span data-ttu-id="e3573-130">Atrybutu QueryString oznacza, że wartość ciągu kwerendy o nazwie StudentID zostanie automatycznie przypisany do parametru w ramach tej metody.</span><span class="sxs-lookup"><span data-stu-id="e3573-130">The QueryString attribute means that a query string value named StudentID is automatically assigned to the parameter in this method.</span></span>

## <a name="add-hyperlink-with-query-string-value"></a><span data-ttu-id="e3573-131">Dodawanie hiperłącza z wartością ciągu zapytania</span><span class="sxs-lookup"><span data-stu-id="e3573-131">Add hyperlink with query string value</span></span>

<span data-ttu-id="e3573-132">W widoku siatki w Students.aspx spowoduje dodanie pola hiperłącze, który stanowi łącze do nowej strony szkolenia.</span><span class="sxs-lookup"><span data-stu-id="e3573-132">In the grid view on Students.aspx, you will add a hyperlink field that links to your new Courses page.</span></span> <span data-ttu-id="e3573-133">Hiperłącze będzie zawierać wartość ciągu zapytania o identyfikatorze studenta.</span><span class="sxs-lookup"><span data-stu-id="e3573-133">The hyperlink will include a query string value with the student's id.</span></span>

<span data-ttu-id="e3573-134">W Students.aspx Dodaj następujące pole do kolumny w widoku siatki poniżej pola do całkowitej środków.</span><span class="sxs-lookup"><span data-stu-id="e3573-134">In Students.aspx, add the following field to the grid view columns just below the field for Total Credits.</span></span>

[!code-aspx[Main](using-query-string-values-to-retrieve-data/samples/sample4.aspx?highlight=7-8)]

<span data-ttu-id="e3573-135">Uruchom aplikację i zwróć uwagę, że widoku siatki zawiera teraz łącze szkolenia.</span><span class="sxs-lookup"><span data-stu-id="e3573-135">Run the application and notice that the grid view now includes the Courses link.</span></span>

![Dodawanie hiperłącza](using-query-string-values-to-retrieve-data/_static/image1.png)

<span data-ttu-id="e3573-137">Kliknięcie łącza zobaczysz kursy zarejestrowanych tego studenta.</span><span class="sxs-lookup"><span data-stu-id="e3573-137">When you click one of the links, you'll see that student's enrolled courses.</span></span>

![Pokaż kursy](using-query-string-values-to-retrieve-data/_static/image2.png)

## <a name="conclusion"></a><span data-ttu-id="e3573-139">Wniosek</span><span class="sxs-lookup"><span data-stu-id="e3573-139">Conclusion</span></span>

<span data-ttu-id="e3573-140">W tym samouczku po dodaniu łącza z wartością ciągu zapytania.</span><span class="sxs-lookup"><span data-stu-id="e3573-140">In this tutorial, you added a link with a query string value.</span></span> <span data-ttu-id="e3573-141">Tej wartości ciągu zapytania jest używany dla wartości parametru w metodzie select.</span><span class="sxs-lookup"><span data-stu-id="e3573-141">You used that query string value for the parameter value in the select method.</span></span>

<span data-ttu-id="e3573-142">W następnej [samouczek](adding-business-logic-layer.md), przeniesie kod z kodem — pliki do warstwy logiki biznesowej i warstwy dostępu do danych.</span><span class="sxs-lookup"><span data-stu-id="e3573-142">In the next [tutorial](adding-business-logic-layer.md), you will move the code from the code-behind files into a business logic layer and a data access layer.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="e3573-143">[Poprzednie](integrating-jquery-ui.md)
[dalej](adding-business-logic-layer.md)</span><span class="sxs-lookup"><span data-stu-id="e3573-143">[Previous](integrating-jquery-ui.md)
[Next](adding-business-logic-layer.md)</span></span>
