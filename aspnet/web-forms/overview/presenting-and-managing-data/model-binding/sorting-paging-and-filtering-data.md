---
uid: web-forms/overview/presenting-and-managing-data/model-binding/sorting-paging-and-filtering-data
title: "Sortowanie, stronicowania i filtrowanie danych przy użyciu wiązania modelu i formularzy sieci web | Dokumentacja firmy Microsoft"
author: tfitzmac
description: "Ten samouczek serii przedstawiono podstawowe aspekty projektu formularzy sieci Web ASP.NET przy użyciu wiązania modelu. Wiązania modelu sprawia, że dane interakcji więcej proste-..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/27/2014
ms.topic: article
ms.assetid: 266e7866-e327-4687-b29d-627a0925e87d
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/presenting-and-managing-data/model-binding/sorting-paging-and-filtering-data
msc.type: authoredcontent
ms.openlocfilehash: 94fc84533be5fcbcf0612fcdcabea7dee738d89b
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
<a name="sorting-paging-and-filtering-data-with-model-binding-and-web-forms"></a><span data-ttu-id="4ceb2-104">Sortowanie, stronicowania i filtrowanie danych przy użyciu wiązania modelu i formularzy sieci web</span><span class="sxs-lookup"><span data-stu-id="4ceb2-104">Sorting, paging, and filtering data with model binding and web forms</span></span>
====================
<span data-ttu-id="4ceb2-105">przez [FitzMacken niestandardowy](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="4ceb2-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="4ceb2-106">Ten samouczek serii przedstawiono podstawowe aspekty projektu formularzy sieci Web ASP.NET przy użyciu wiązania modelu.</span><span class="sxs-lookup"><span data-stu-id="4ceb2-106">This tutorial series demonstrates basic aspects of using model binding with an ASP.NET Web Forms project.</span></span> <span data-ttu-id="4ceb2-107">Wiązania modelu sprawia, że dane interakcji więcej proste niż dotyczących danych obiekty źródła (na przykład element ObjectDataSource lub SqlDataSource).</span><span class="sxs-lookup"><span data-stu-id="4ceb2-107">Model binding makes data interaction more straight-forward than dealing with data source objects (such as ObjectDataSource or SqlDataSource).</span></span> <span data-ttu-id="4ceb2-108">Ta seria rozpoczyna się od wprowadzające informacje i przechodzi do bardziej zaawansowanych pojęcia w kolejnych samouczkach.</span><span class="sxs-lookup"><span data-stu-id="4ceb2-108">This series starts with introductory material and moves to more advanced concepts in later tutorials.</span></span>
> 
> <span data-ttu-id="4ceb2-109">W tym samouczku przedstawiono sposób dodawania sortowania, stronicowania i filtrowanie danych za pośrednictwem wiązania modelu.</span><span class="sxs-lookup"><span data-stu-id="4ceb2-109">This tutorial shows how to add sorting, paging, and filtering of the data through model binding.</span></span>
> 
> <span data-ttu-id="4ceb2-110">W tym samouczku opiera się na projekcie utworzony w pierwszym [części](retrieving-data.md) serii.</span><span class="sxs-lookup"><span data-stu-id="4ceb2-110">This tutorial builds on the project created in the first [part](retrieving-data.md) of the series.</span></span>
> 
> <span data-ttu-id="4ceb2-111">Możesz [Pobierz](https://go.microsoft.com/fwlink/?LinkId=286116) kompletnego projektu w języku C# lub VB.</span><span class="sxs-lookup"><span data-stu-id="4ceb2-111">You can [download](https://go.microsoft.com/fwlink/?LinkId=286116) the complete project in C# or VB.</span></span> <span data-ttu-id="4ceb2-112">Kod do pobrania współpracuje z programu Visual Studio 2012 lub Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="4ceb2-112">The downloadable code works with either Visual Studio 2012 or Visual Studio 2013.</span></span> <span data-ttu-id="4ceb2-113">Używa szablonu programu Visual Studio 2012, który jest nieco inne niż szablon programu Visual Studio 2013 przedstawiona w tym samouczku.</span><span class="sxs-lookup"><span data-stu-id="4ceb2-113">It uses the Visual Studio 2012 template, which is slightly different than the Visual Studio 2013 template shown in this tutorial.</span></span>


## <a name="what-youll-build"></a><span data-ttu-id="4ceb2-114">Będzie kompilacji</span><span class="sxs-lookup"><span data-stu-id="4ceb2-114">What you'll build</span></span>

<span data-ttu-id="4ceb2-115">W tym samouczku będziesz:</span><span class="sxs-lookup"><span data-stu-id="4ceb2-115">In this tutorial, you'll:</span></span>

1. <span data-ttu-id="4ceb2-116">Włącz sortowanie i stronicowanie danych</span><span class="sxs-lookup"><span data-stu-id="4ceb2-116">Enable sorting and paging of the data</span></span>
2. <span data-ttu-id="4ceb2-117">Włącz filtrowanie danych na podstawie zaznaczenia przez użytkownika</span><span class="sxs-lookup"><span data-stu-id="4ceb2-117">Enable filtering of the data based on a selection by the user</span></span>

## <a name="add-sorting"></a><span data-ttu-id="4ceb2-118">Dodaj sortowanie</span><span class="sxs-lookup"><span data-stu-id="4ceb2-118">Add sorting</span></span>

<span data-ttu-id="4ceb2-119">Włączanie sortowania w widoku GridView jest bardzo proste.</span><span class="sxs-lookup"><span data-stu-id="4ceb2-119">Enabling sorting in the GridView is very easy.</span></span> <span data-ttu-id="4ceb2-120">W pliku Student.aspx wystarczy ustawić **AllowSorting** do **true** w widoku GridView.</span><span class="sxs-lookup"><span data-stu-id="4ceb2-120">In the Student.aspx file, simply set **AllowSorting** to **true** in the GridView.</span></span> <span data-ttu-id="4ceb2-121">Nie należy ustawić **SortExpression** wartość dla każdej kolumny, jak automatycznie służy element DataField.</span><span class="sxs-lookup"><span data-stu-id="4ceb2-121">You do not need to set a **SortExpression** value for each column as the DataField is automatically used.</span></span> <span data-ttu-id="4ceb2-122">Widoku GridView modyfikuje zapytanie zawierało porządkowanie danych przez wybraną wartość.</span><span class="sxs-lookup"><span data-stu-id="4ceb2-122">The GridView modifies the query to include ordering the data by the selected value.</span></span> <span data-ttu-id="4ceb2-123">Wyróżniony kod poniżej pokazano sposób dodawania, które należy podjąć włączyć sortowanie.</span><span class="sxs-lookup"><span data-stu-id="4ceb2-123">The highlighted code below shows the addition you need to make to enable sorting.</span></span>

[!code-aspx[Main](sorting-paging-and-filtering-data/samples/sample1.aspx?highlight=5)]

<span data-ttu-id="4ceb2-124">Uruchamianie aplikacji sieci web i testowanie sortowania rekordów uczniowie według wartości w różnych kolumn.</span><span class="sxs-lookup"><span data-stu-id="4ceb2-124">Run the web application, and test sorting student records by the values in different columns.</span></span>

![Sortowanie uczniów lub studentów](sorting-paging-and-filtering-data/_static/image2.png)

## <a name="add-paging"></a><span data-ttu-id="4ceb2-126">Dodaj stronicowania</span><span class="sxs-lookup"><span data-stu-id="4ceb2-126">Add paging</span></span>

<span data-ttu-id="4ceb2-127">Włączanie stronicowania również jest bardzo proste.</span><span class="sxs-lookup"><span data-stu-id="4ceb2-127">Enabling paging is also very easy.</span></span> <span data-ttu-id="4ceb2-128">W widoku GridView, ustaw **właściwość AllowPaging** właściwości **true** i ustaw **PageSize** właściwości to liczba rekordów, które mają zostać wyświetlone na każdej stronie.</span><span class="sxs-lookup"><span data-stu-id="4ceb2-128">In the GridView, set the **AllowPaging** property to **true** and set the **PageSize** property to the number of records you wish to display on each page.</span></span> <span data-ttu-id="4ceb2-129">W tym samouczku można ustawić go do 4.</span><span class="sxs-lookup"><span data-stu-id="4ceb2-129">In this tutorial, you can set it to 4.</span></span>

[!code-aspx[Main](sorting-paging-and-filtering-data/samples/sample2.aspx?highlight=5)]

<span data-ttu-id="4ceb2-130">Uruchom aplikację sieci web i zwróć uwagę, że teraz rekordy są podzielone na kilka stron z nie więcej niż 4 rekordów wyświetlanych na jednej stronie.</span><span class="sxs-lookup"><span data-stu-id="4ceb2-130">Run the web application, and notice that now the records are divided over multiple pages with no more than 4 records displayed on a single page.</span></span>

![Dodaj stronicowania](sorting-paging-and-filtering-data/_static/image4.png)

<span data-ttu-id="4ceb2-132">Wykonywanie zapytań odroczonych poprawia wydajność aplikacji.</span><span class="sxs-lookup"><span data-stu-id="4ceb2-132">Deferred query execution improves the application efficiency.</span></span> <span data-ttu-id="4ceb2-133">Zamiast pobierania cały zestaw danych, w widoku GridView modyfikuje zapytanie w celu pobrania tylko rekordów dla bieżącej strony.</span><span class="sxs-lookup"><span data-stu-id="4ceb2-133">Instead of retrieving the entire data set, the GridView modifies the query to retrieve only the records for the current page.</span></span>

## <a name="filter-records-by-user-selection"></a><span data-ttu-id="4ceb2-134">Filtrowanie rekordów przez określonego użytkownika</span><span class="sxs-lookup"><span data-stu-id="4ceb2-134">Filter records by user selection</span></span>

<span data-ttu-id="4ceb2-135">Wiązanie modelu dodaje kilka atrybutów, które umożliwiają określanie sposobu wartość dla parametru w metodzie powiązania modelu.</span><span class="sxs-lookup"><span data-stu-id="4ceb2-135">Model binding adds several attributes which enable you to designate how to set the value for a parameter in a model binding method.</span></span> <span data-ttu-id="4ceb2-136">Te atrybuty są w **System.Web.ModelBinding** przestrzeni nazw.</span><span class="sxs-lookup"><span data-stu-id="4ceb2-136">These attributes are in the **System.Web.ModelBinding** namespace.</span></span> <span data-ttu-id="4ceb2-137">Obejmują one:</span><span class="sxs-lookup"><span data-stu-id="4ceb2-137">They include:</span></span>

- <span data-ttu-id="4ceb2-138">Formant</span><span class="sxs-lookup"><span data-stu-id="4ceb2-138">Control</span></span>
- <span data-ttu-id="4ceb2-139">Plik cookie</span><span class="sxs-lookup"><span data-stu-id="4ceb2-139">Cookie</span></span>
- <span data-ttu-id="4ceb2-140">Formularz</span><span class="sxs-lookup"><span data-stu-id="4ceb2-140">Form</span></span>
- <span data-ttu-id="4ceb2-141">Profil</span><span class="sxs-lookup"><span data-stu-id="4ceb2-141">Profile</span></span>
- <span data-ttu-id="4ceb2-142">Ciąg zapytania</span><span class="sxs-lookup"><span data-stu-id="4ceb2-142">QueryString</span></span>
- <span data-ttu-id="4ceb2-143">RouteData</span><span class="sxs-lookup"><span data-stu-id="4ceb2-143">RouteData</span></span>
- <span data-ttu-id="4ceb2-144">Sesja</span><span class="sxs-lookup"><span data-stu-id="4ceb2-144">Session</span></span>
- <span data-ttu-id="4ceb2-145">UserProfile</span><span class="sxs-lookup"><span data-stu-id="4ceb2-145">UserProfile</span></span>
- <span data-ttu-id="4ceb2-146">Stan widoku</span><span class="sxs-lookup"><span data-stu-id="4ceb2-146">ViewState</span></span>

<span data-ttu-id="4ceb2-147">W tym samouczku użyjesz wartość formantu do filtrowania, które rekordy są wyświetlane w widoku GridView.</span><span class="sxs-lookup"><span data-stu-id="4ceb2-147">In this tutorial, you will use a control's value to filter which records are displayed in the GridView.</span></span> <span data-ttu-id="4ceb2-148">Należy dodać **kontroli** atrybut do metody zapytania została utworzona wcześniej.</span><span class="sxs-lookup"><span data-stu-id="4ceb2-148">You will add the **Control** attribute to the query method you had created earlier.</span></span> <span data-ttu-id="4ceb2-149">W [później](using-query-string-values-to-retrieve-data.md) samouczka będą stosowane **QueryString** atrybutu parametru, aby określić, czy wartość parametru pochodzą z wartości ciągu zapytania.</span><span class="sxs-lookup"><span data-stu-id="4ceb2-149">In a [later](using-query-string-values-to-retrieve-data.md) tutorial, you will apply the **QueryString** attribute to a parameter to specify that the parameter value comes from a query string value.</span></span>

<span data-ttu-id="4ceb2-150">Najpierw powyżej ValidationSummary, Dodaj na rozwijanej liście filtrowania studentów, które są wyświetlane.</span><span class="sxs-lookup"><span data-stu-id="4ceb2-150">First, above the ValidationSummary, add a drop down list for filtering which students are shown.</span></span>

[!code-aspx[Main](sorting-paging-and-filtering-data/samples/sample3.aspx?highlight=3-11)]

<span data-ttu-id="4ceb2-151">W pliku CodeBehind zmodyfikuj metody select, aby otrzymać wartość z formantu, a następnie ustaw nazwę parametru Nazwa formantu, który zawiera wartość.</span><span class="sxs-lookup"><span data-stu-id="4ceb2-151">In the code-behind file, modify the select method to receive a value from the control, and set the name of the parameter to the name of the control that provides the value.</span></span>

<span data-ttu-id="4ceb2-152">Należy dodać **przy użyciu** instrukcji dla **System.Web.ModelBinding** przestrzeni nazw, aby rozpoznać atrybut kontrolki.</span><span class="sxs-lookup"><span data-stu-id="4ceb2-152">You must add a **using** statement for the **System.Web.ModelBinding** namespace to resolve the Control attribute.</span></span>

[!code-csharp[Main](sorting-paging-and-filtering-data/samples/sample4.cs)]

<span data-ttu-id="4ceb2-153">Poniższy kod przedstawia metody select zlikwidować do filtrowania danych zwróconych na podstawie wartości z listy rozwijanej.</span><span class="sxs-lookup"><span data-stu-id="4ceb2-153">The following code shows the select method re-worked to filter the returned data based on the value of the drop down list.</span></span> <span data-ttu-id="4ceb2-154">Dodawanie atrybutu kontroli przed parametr określa, że wartość tego parametru pochodzi z formantu o takiej samej nazwie.</span><span class="sxs-lookup"><span data-stu-id="4ceb2-154">Adding a control attribute before a parameter specifies that the value for this parameter comes from a control with the same name.</span></span>

[!code-csharp[Main](sorting-paging-and-filtering-data/samples/sample5.cs)]

<span data-ttu-id="4ceb2-155">Uruchom aplikację sieci web i wybierz różne wartości z listy rozwijanej listy, aby przefiltrować listę uczniów lub studentów.</span><span class="sxs-lookup"><span data-stu-id="4ceb2-155">Run the web application and select different values from the drop down list to filter the list of students.</span></span>

![Filtr uczniów lub studentów](sorting-paging-and-filtering-data/_static/image6.png)

## <a name="conclusion"></a><span data-ttu-id="4ceb2-157">Wniosek</span><span class="sxs-lookup"><span data-stu-id="4ceb2-157">Conclusion</span></span>

<span data-ttu-id="4ceb2-158">W tym samouczku włączone sortowanie i stronicowanie danych.</span><span class="sxs-lookup"><span data-stu-id="4ceb2-158">In this tutorial, you enabled sorting and paging of the data.</span></span> <span data-ttu-id="4ceb2-159">Możesz również włączone filtrowanie danych przez wartość formantu.</span><span class="sxs-lookup"><span data-stu-id="4ceb2-159">You also enabled filtering the data by the value of a control.</span></span>

<span data-ttu-id="4ceb2-160">W następnej [samouczek](integrating-jquery-ui.md) zwiększy interfejsu użytkownika dzięki integracji elementu widget interfejsu użytkownika JQuery w szablonie danych dynamicznych.</span><span class="sxs-lookup"><span data-stu-id="4ceb2-160">In the next [tutorial](integrating-jquery-ui.md) you will enhance the UI by integrating a JQuery UI widget into the dynamic data template.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="4ceb2-161">[Poprzednie](updating-deleting-and-creating-data.md)
[dalej](integrating-jquery-ui.md)</span><span class="sxs-lookup"><span data-stu-id="4ceb2-161">[Previous](updating-deleting-and-creating-data.md)
[Next](integrating-jquery-ui.md)</span></span>
