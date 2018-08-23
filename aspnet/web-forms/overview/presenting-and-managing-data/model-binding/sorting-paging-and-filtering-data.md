---
uid: web-forms/overview/presenting-and-managing-data/model-binding/sorting-paging-and-filtering-data
title: Sortowanie, stronicowanie i filtrowanie danych za pomocą wiązania modelu i formularzy sieci web | Dokumentacja firmy Microsoft
author: tfitzmac
description: W tej serii samouczków pokazano podstawowych aspektów projektu formularzy sieci Web ASP.NET przy użyciu wiązania modelu. Wiązanie modelu sprawia, że dane interakcji więcej proste —...
ms.author: riande
ms.date: 02/27/2014
ms.assetid: 266e7866-e327-4687-b29d-627a0925e87d
msc.legacyurl: /web-forms/overview/presenting-and-managing-data/model-binding/sorting-paging-and-filtering-data
msc.type: authoredcontent
ms.openlocfilehash: 86ddedb68b8d18057cd2f7d68e795efda33734b1
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/16/2018
ms.locfileid: "41757363"
---
<a name="sorting-paging-and-filtering-data-with-model-binding-and-web-forms"></a><span data-ttu-id="fc21d-104">Sortowanie, stronicowanie i filtrowanie danych za pomocą wiązania modelu i formularzy sieci web</span><span class="sxs-lookup"><span data-stu-id="fc21d-104">Sorting, paging, and filtering data with model binding and web forms</span></span>
====================
<span data-ttu-id="fc21d-105">przez [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="fc21d-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="fc21d-106">W tej serii samouczków pokazano podstawowych aspektów projektu formularzy sieci Web ASP.NET przy użyciu wiązania modelu.</span><span class="sxs-lookup"><span data-stu-id="fc21d-106">This tutorial series demonstrates basic aspects of using model binding with an ASP.NET Web Forms project.</span></span> <span data-ttu-id="fc21d-107">Wiązanie modelu sprawia, że dane interakcji prostszą niż rozwiązywania problemów związanych z danymi obiektów źródła (takich jak kontrolki ObjectDataSource lub SqlDataSource).</span><span class="sxs-lookup"><span data-stu-id="fc21d-107">Model binding makes data interaction more straight-forward than dealing with data source objects (such as ObjectDataSource or SqlDataSource).</span></span> <span data-ttu-id="fc21d-108">Ta seria rozpoczyna się od wprowadzające informacje i przenosi do bardziej zaawansowanych pojęciach w kolejnych samouczkach.</span><span class="sxs-lookup"><span data-stu-id="fc21d-108">This series starts with introductory material and moves to more advanced concepts in later tutorials.</span></span>
> 
> <span data-ttu-id="fc21d-109">W tym samouczku przedstawiono sposób dodawania, sortowanie, stronicowanie i filtrowanie danych za pomocą wiązania modelu.</span><span class="sxs-lookup"><span data-stu-id="fc21d-109">This tutorial shows how to add sorting, paging, and filtering of the data through model binding.</span></span>
> 
> <span data-ttu-id="fc21d-110">Ten samouczek opiera się na projekt utworzony w pierwszym [część](retrieving-data.md) serii.</span><span class="sxs-lookup"><span data-stu-id="fc21d-110">This tutorial builds on the project created in the first [part](retrieving-data.md) of the series.</span></span>
> 
> <span data-ttu-id="fc21d-111">Możesz [Pobierz](https://go.microsoft.com/fwlink/?LinkId=286116) kompletnego projektu w języku C# lub VB.</span><span class="sxs-lookup"><span data-stu-id="fc21d-111">You can [download](https://go.microsoft.com/fwlink/?LinkId=286116) the complete project in C# or VB.</span></span> <span data-ttu-id="fc21d-112">Kod do pobrania w programach Visual Studio 2012 lub Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="fc21d-112">The downloadable code works with either Visual Studio 2012 or Visual Studio 2013.</span></span> <span data-ttu-id="fc21d-113">Używa szablonu programu Visual Studio 2012, który różni się nieco od szablonu programu Visual Studio 2013, przedstawione w tym samouczku.</span><span class="sxs-lookup"><span data-stu-id="fc21d-113">It uses the Visual Studio 2012 template, which is slightly different than the Visual Studio 2013 template shown in this tutorial.</span></span>


## <a name="what-youll-build"></a><span data-ttu-id="fc21d-114">Będziesz tworzyć</span><span class="sxs-lookup"><span data-stu-id="fc21d-114">What you'll build</span></span>

<span data-ttu-id="fc21d-115">W ramach tego samouczka należy:</span><span class="sxs-lookup"><span data-stu-id="fc21d-115">In this tutorial, you'll:</span></span>

1. <span data-ttu-id="fc21d-116">Włączyć sortowanie i stronicowanie danych</span><span class="sxs-lookup"><span data-stu-id="fc21d-116">Enable sorting and paging of the data</span></span>
2. <span data-ttu-id="fc21d-117">Włącz filtrowanie danych na podstawie wybranych przez użytkownika</span><span class="sxs-lookup"><span data-stu-id="fc21d-117">Enable filtering of the data based on a selection by the user</span></span>

## <a name="add-sorting"></a><span data-ttu-id="fc21d-118">Dodaj sortowanie</span><span class="sxs-lookup"><span data-stu-id="fc21d-118">Add sorting</span></span>

<span data-ttu-id="fc21d-119">Włączanie sortowania w widoku GridView jest bardzo proste.</span><span class="sxs-lookup"><span data-stu-id="fc21d-119">Enabling sorting in the GridView is very easy.</span></span> <span data-ttu-id="fc21d-120">W pliku Student.aspx, po prostu ustaw **AllowSorting** do **true** w widoku GridView.</span><span class="sxs-lookup"><span data-stu-id="fc21d-120">In the Student.aspx file, simply set **AllowSorting** to **true** in the GridView.</span></span> <span data-ttu-id="fc21d-121">Nie należy ustawić **SortExpression** wartość dla każdej kolumny, jak DataField jest stosowana automatycznie.</span><span class="sxs-lookup"><span data-stu-id="fc21d-121">You do not need to set a **SortExpression** value for each column as the DataField is automatically used.</span></span> <span data-ttu-id="fc21d-122">Kontrolki GridView modyfikuje zapytanie, aby uwzględnić porządkowanie danych przez wybraną wartość.</span><span class="sxs-lookup"><span data-stu-id="fc21d-122">The GridView modifies the query to include ordering the data by the selected value.</span></span> <span data-ttu-id="fc21d-123">Wyróżniony kod poniżej przedstawiono dodanie należy wprowadzić włączyć sortowanie.</span><span class="sxs-lookup"><span data-stu-id="fc21d-123">The highlighted code below shows the addition you need to make to enable sorting.</span></span>

[!code-aspx[Main](sorting-paging-and-filtering-data/samples/sample1.aspx?highlight=5)]

<span data-ttu-id="fc21d-124">Uruchamianie aplikacji sieci web i testowanie sortowania dla uczniów rekordy według wartości w różnych kolumnach.</span><span class="sxs-lookup"><span data-stu-id="fc21d-124">Run the web application, and test sorting student records by the values in different columns.</span></span>

![Studenci sortowania](sorting-paging-and-filtering-data/_static/image2.png)

## <a name="add-paging"></a><span data-ttu-id="fc21d-126">Dodawanie stronicowania</span><span class="sxs-lookup"><span data-stu-id="fc21d-126">Add paging</span></span>

<span data-ttu-id="fc21d-127">Włączanie stronicowania również jest bardzo proste.</span><span class="sxs-lookup"><span data-stu-id="fc21d-127">Enabling paging is also very easy.</span></span> <span data-ttu-id="fc21d-128">W widoku GridView, ustaw **właściwość AllowPaging** właściwości **true** i ustaw **PageSize** liczbę rekordów, które mają zostać wyświetlone na każdej stronie.</span><span class="sxs-lookup"><span data-stu-id="fc21d-128">In the GridView, set the **AllowPaging** property to **true** and set the **PageSize** property to the number of records you wish to display on each page.</span></span> <span data-ttu-id="fc21d-129">W tym samouczku można ustawić go do 4.</span><span class="sxs-lookup"><span data-stu-id="fc21d-129">In this tutorial, you can set it to 4.</span></span>

[!code-aspx[Main](sorting-paging-and-filtering-data/samples/sample2.aspx?highlight=5)]

<span data-ttu-id="fc21d-130">Uruchom aplikację sieci web i zwróć uwagę, że teraz rekordy są podzielone na kilka stron z nie więcej niż 4 wyświetlanych na jednej stronie rekordów.</span><span class="sxs-lookup"><span data-stu-id="fc21d-130">Run the web application, and notice that now the records are divided over multiple pages with no more than 4 records displayed on a single page.</span></span>

![Dodawanie stronicowania](sorting-paging-and-filtering-data/_static/image4.png)

<span data-ttu-id="fc21d-132">Wykonanie odroczone zapytanie poprawia wydajność aplikacji.</span><span class="sxs-lookup"><span data-stu-id="fc21d-132">Deferred query execution improves the application efficiency.</span></span> <span data-ttu-id="fc21d-133">Zamiast pobierania całego zestawu danych, widoku GridView modyfikuje zapytanie, aby pobrać tylko te rekordy, dla bieżącej strony.</span><span class="sxs-lookup"><span data-stu-id="fc21d-133">Instead of retrieving the entire data set, the GridView modifies the query to retrieve only the records for the current page.</span></span>

## <a name="filter-records-by-user-selection"></a><span data-ttu-id="fc21d-134">Filtrowanie rekordów na podstawie wyboru użytkownik</span><span class="sxs-lookup"><span data-stu-id="fc21d-134">Filter records by user selection</span></span>

<span data-ttu-id="fc21d-135">Wiązanie modelu dodaje kilka atrybutów, które pozwalają na określenie sposobu ustawiania wartości dla parametru metody wiązania modelu.</span><span class="sxs-lookup"><span data-stu-id="fc21d-135">Model binding adds several attributes which enable you to designate how to set the value for a parameter in a model binding method.</span></span> <span data-ttu-id="fc21d-136">Te atrybuty **System.Web.ModelBinding** przestrzeni nazw.</span><span class="sxs-lookup"><span data-stu-id="fc21d-136">These attributes are in the **System.Web.ModelBinding** namespace.</span></span> <span data-ttu-id="fc21d-137">Obejmują one:</span><span class="sxs-lookup"><span data-stu-id="fc21d-137">They include:</span></span>

- <span data-ttu-id="fc21d-138">Formant</span><span class="sxs-lookup"><span data-stu-id="fc21d-138">Control</span></span>
- <span data-ttu-id="fc21d-139">Plik cookie</span><span class="sxs-lookup"><span data-stu-id="fc21d-139">Cookie</span></span>
- <span data-ttu-id="fc21d-140">Formularz</span><span class="sxs-lookup"><span data-stu-id="fc21d-140">Form</span></span>
- <span data-ttu-id="fc21d-141">Profil</span><span class="sxs-lookup"><span data-stu-id="fc21d-141">Profile</span></span>
- <span data-ttu-id="fc21d-142">Ciąg zapytania</span><span class="sxs-lookup"><span data-stu-id="fc21d-142">QueryString</span></span>
- <span data-ttu-id="fc21d-143">RouteData</span><span class="sxs-lookup"><span data-stu-id="fc21d-143">RouteData</span></span>
- <span data-ttu-id="fc21d-144">Sesja</span><span class="sxs-lookup"><span data-stu-id="fc21d-144">Session</span></span>
- <span data-ttu-id="fc21d-145">UserProfile</span><span class="sxs-lookup"><span data-stu-id="fc21d-145">UserProfile</span></span>
- <span data-ttu-id="fc21d-146">Stan widoku</span><span class="sxs-lookup"><span data-stu-id="fc21d-146">ViewState</span></span>

<span data-ttu-id="fc21d-147">W tym samouczku wartość kontrolki zostanie użyta, aby filtrować rekordy, które są wyświetlane w widoku GridView.</span><span class="sxs-lookup"><span data-stu-id="fc21d-147">In this tutorial, you will use a control's value to filter which records are displayed in the GridView.</span></span> <span data-ttu-id="fc21d-148">Należy dodać **kontroli** atrybutu do metody zapytania została utworzona wcześniej.</span><span class="sxs-lookup"><span data-stu-id="fc21d-148">You will add the **Control** attribute to the query method you had created earlier.</span></span> <span data-ttu-id="fc21d-149">W [później](using-query-string-values-to-retrieve-data.md) samouczek, zostaną zastosowane **QueryString** atrybutu do parametru do określenia, czy wartość parametru pochodzą od wartości ciągu zapytania.</span><span class="sxs-lookup"><span data-stu-id="fc21d-149">In a [later](using-query-string-values-to-retrieve-data.md) tutorial, you will apply the **QueryString** attribute to a parameter to specify that the parameter value comes from a query string value.</span></span>

<span data-ttu-id="fc21d-150">Po pierwsze powyżej podsumowania walidacji, dodać listy rozwijanej w celu filtrowania listy, studentów, które są wyświetlane.</span><span class="sxs-lookup"><span data-stu-id="fc21d-150">First, above the ValidationSummary, add a drop down list for filtering which students are shown.</span></span>

[!code-aspx[Main](sorting-paging-and-filtering-data/samples/sample3.aspx?highlight=3-11)]

<span data-ttu-id="fc21d-151">W pliku związanym z kodem zmodyfikować wybierz metodę, aby otrzymać wartość z kontrolki, a następnie ustaw nazwę parametru nazwę kontrolki, która zawiera wartość.</span><span class="sxs-lookup"><span data-stu-id="fc21d-151">In the code-behind file, modify the select method to receive a value from the control, and set the name of the parameter to the name of the control that provides the value.</span></span>

<span data-ttu-id="fc21d-152">Należy dodać **przy użyciu** poufności informacji dotyczące **System.Web.ModelBinding** przestrzeni nazw, aby rozwiązać atrybut kontrolki.</span><span class="sxs-lookup"><span data-stu-id="fc21d-152">You must add a **using** statement for the **System.Web.ModelBinding** namespace to resolve the Control attribute.</span></span>

[!code-csharp[Main](sorting-paging-and-filtering-data/samples/sample4.cs)]

<span data-ttu-id="fc21d-153">Poniższy kod przedstawia metody select zlikwidować filtrującą dane zwrócone dane na podstawie wartości na liście rozwijanej.</span><span class="sxs-lookup"><span data-stu-id="fc21d-153">The following code shows the select method re-worked to filter the returned data based on the value of the drop down list.</span></span> <span data-ttu-id="fc21d-154">Dodawanie atrybutu kontroli, zanim parametr określa, że wartość tego parametru pochodzi z formantu o takiej samej nazwie.</span><span class="sxs-lookup"><span data-stu-id="fc21d-154">Adding a control attribute before a parameter specifies that the value for this parameter comes from a control with the same name.</span></span>

[!code-csharp[Main](sorting-paging-and-filtering-data/samples/sample5.cs)]

<span data-ttu-id="fc21d-155">Uruchom aplikację sieci web i wybierz różne wartości z listy rozwijanej listy, aby filtrować listę uczniów.</span><span class="sxs-lookup"><span data-stu-id="fc21d-155">Run the web application and select different values from the drop down list to filter the list of students.</span></span>

![Studenci filtru](sorting-paging-and-filtering-data/_static/image6.png)

## <a name="conclusion"></a><span data-ttu-id="fc21d-157">Wniosek</span><span class="sxs-lookup"><span data-stu-id="fc21d-157">Conclusion</span></span>

<span data-ttu-id="fc21d-158">W tym samouczku włączone sortowanie i stronicowanie danych.</span><span class="sxs-lookup"><span data-stu-id="fc21d-158">In this tutorial, you enabled sorting and paging of the data.</span></span> <span data-ttu-id="fc21d-159">Możesz również włączone filtrowania danych według wartości kontrolki.</span><span class="sxs-lookup"><span data-stu-id="fc21d-159">You also enabled filtering the data by the value of a control.</span></span>

<span data-ttu-id="fc21d-160">W ciągu następnych [samouczek](integrating-jquery-ui.md) ulepszenie interfejsu użytkownika przez integrowanie widżetów interfejsu użytkownika JQuery szablonu dynamicznego danych.</span><span class="sxs-lookup"><span data-stu-id="fc21d-160">In the next [tutorial](integrating-jquery-ui.md) you will enhance the UI by integrating a JQuery UI widget into the dynamic data template.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="fc21d-161">[Poprzednie](updating-deleting-and-creating-data.md)
> [dalej](integrating-jquery-ui.md)</span><span class="sxs-lookup"><span data-stu-id="fc21d-161">[Previous](updating-deleting-and-creating-data.md)
[Next](integrating-jquery-ui.md)</span></span>
