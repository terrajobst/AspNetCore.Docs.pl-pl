---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/display_data_items_and_details
title: Wyświetl dane elementów i szczegóły | Dokumentacja firmy Microsoft
author: Erikre
description: Ten samouczek serii uczy podstawowe informacje dotyczące tworzenia aplikacji formularzy sieci Web ASP.NET przy użyciu platformy ASP.NET 4.5 i programu Microsoft Visual Studio Express 2013 dla możemy...
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/08/2014
ms.topic: article
ms.assetid: 64a491a8-0ed6-4c2f-9c1c-412962eb6006
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/display_data_items_and_details
msc.type: authoredcontent
ms.openlocfilehash: 5fea654aa5116193cb7496c1b9020ed8e25fc06f
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
---
<a name="display-data-items-and-details"></a><span data-ttu-id="9d11a-103">Wyświetl dane elementów i szczegóły</span><span class="sxs-lookup"><span data-stu-id="9d11a-103">Display Data Items and Details</span></span>
====================
<span data-ttu-id="9d11a-104">Przez [Erik Reitan](https://github.com/Erikre)</span><span class="sxs-lookup"><span data-stu-id="9d11a-104">by [Erik Reitan](https://github.com/Erikre)</span></span>

<span data-ttu-id="9d11a-105">[Pobierz Wingtip Toys przykładowy projekt (C#)](http://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) lub [Pobierz E-book (PDF)](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)</span><span class="sxs-lookup"><span data-stu-id="9d11a-105">[Download Wingtip Toys Sample Project (C#)](http://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) or [Download E-book (PDF)](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)</span></span>

> <span data-ttu-id="9d11a-106">Ten samouczek serii nauczyć podstawowe informacje dotyczące tworzenia aplikacji formularzy sieci Web ASP.NET przy użyciu platformy ASP.NET 4.5 i programu Microsoft Visual Studio Express 2013 for Web.</span><span class="sxs-lookup"><span data-stu-id="9d11a-106">This tutorial series will teach you the basics of building an ASP.NET Web Forms application using ASP.NET 4.5 and Microsoft Visual Studio Express 2013 for Web.</span></span> <span data-ttu-id="9d11a-107">Visual Studio 2013 [projektu z kodu źródłowego C#](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) jest dostępna towarzyszące tej serii samouczka.</span><span class="sxs-lookup"><span data-stu-id="9d11a-107">A Visual Studio 2013 [project with C# source code](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) is available to accompany this tutorial series.</span></span>


<span data-ttu-id="9d11a-108">Ten przewodnik opisuje sposób wyświetlania danych elementów i szczegóły elementu danych przy użyciu formularzy sieci Web ASP.NET i Entity Framework Code First.</span><span class="sxs-lookup"><span data-stu-id="9d11a-108">This tutorial describes how to display data items and data item details using ASP.NET Web Forms and Entity Framework Code First.</span></span> <span data-ttu-id="9d11a-109">W tym samouczku jest oparty na poprzednich samouczek "Nawigacji i interfejsu użytkownika" i jest częścią serii samouczek Wingtip zabawka magazynu.</span><span class="sxs-lookup"><span data-stu-id="9d11a-109">This tutorial builds on the previous tutorial "UI and Navigation" and is part of the Wingtip Toy Store tutorial series.</span></span> <span data-ttu-id="9d11a-110">Po zakończeniu tego samouczka będziesz mieć możliwość Zobacz produktów na *ProductsList.aspx* strony i szczegółowe informacje o indywidualnych produktów na *ProductDetails.aspx* strony.</span><span class="sxs-lookup"><span data-stu-id="9d11a-110">When you've completed this tutorial, you'll be able to see products on the *ProductsList.aspx* page and details about an individual product on the *ProductDetails.aspx* page.</span></span>

## <a name="what-youll-learn"></a><span data-ttu-id="9d11a-111">Zawartość:</span><span class="sxs-lookup"><span data-stu-id="9d11a-111">What you'll learn:</span></span>

- <span data-ttu-id="9d11a-112">Jak dodać formantu danych do wyświetlenia produktów z bazy danych.</span><span class="sxs-lookup"><span data-stu-id="9d11a-112">How to add a data control to display products from the database.</span></span>
- <span data-ttu-id="9d11a-113">Jak nawiązać formantu danych wybranych danych.</span><span class="sxs-lookup"><span data-stu-id="9d11a-113">How to connect a data control to the selected data.</span></span>
- <span data-ttu-id="9d11a-114">Jak dodać formantu danych, aby wyświetlić szczegółowe informacje z bazy danych.</span><span class="sxs-lookup"><span data-stu-id="9d11a-114">How to add a data control to display product details from the database.</span></span>
- <span data-ttu-id="9d11a-115">Jak pobrać wartości z ciągu zapytania i użycie tej wartości, aby ograniczyć dane, które są pobierane z bazy danych.</span><span class="sxs-lookup"><span data-stu-id="9d11a-115">How to retrieve a value from the query string and use that value to limit the data that's retrieved from the database.</span></span>

### <a name="these-are-the-features-introduced-in-the-tutorial"></a><span data-ttu-id="9d11a-116">Poniżej przedstawiono funkcje wprowadzone w samouczku:</span><span class="sxs-lookup"><span data-stu-id="9d11a-116">These are the features introduced in the tutorial:</span></span>

- <span data-ttu-id="9d11a-117">Wiązanie modelu</span><span class="sxs-lookup"><span data-stu-id="9d11a-117">Model Binding</span></span>
- <span data-ttu-id="9d11a-118">Dostawców wartości</span><span class="sxs-lookup"><span data-stu-id="9d11a-118">Value providers</span></span>

## <a name="adding-a-data-control-to-display-products"></a><span data-ttu-id="9d11a-119">Dodawanie formantu danych do wyświetlenia produktów</span><span class="sxs-lookup"><span data-stu-id="9d11a-119">Adding a Data Control to Display Products</span></span>

<span data-ttu-id="9d11a-120">Podczas tworzenia wiązania danych formantu serwera, istnieje kilka różnych opcji, których można użyć.</span><span class="sxs-lookup"><span data-stu-id="9d11a-120">When binding data to a server control, there are a few different options you can use.</span></span> <span data-ttu-id="9d11a-121">Najbardziej typowe opcje obejmują dodawanie formantu źródła danych, dodawanie kodu ręcznie lub przy użyciu wiązania modelu.</span><span class="sxs-lookup"><span data-stu-id="9d11a-121">The most common options include adding a data source control, adding code by hand, or using model binding.</span></span>

### <a name="using-a-data-source-control-to-bind-data"></a><span data-ttu-id="9d11a-122">Za pomocą formantu źródła danych do powiązania danych</span><span class="sxs-lookup"><span data-stu-id="9d11a-122">Using a Data Source Control to Bind Data</span></span>

<span data-ttu-id="9d11a-123">Dodawanie formantu źródła danych umożliwia łączenie formantu źródła danych do formantu, który wyświetla dane.</span><span class="sxs-lookup"><span data-stu-id="9d11a-123">Adding a data source control allows you to link the data source control to the control that displays the data.</span></span> <span data-ttu-id="9d11a-124">Takie podejście umożliwia deklaratywnie połączenie bezpośrednio do źródeł danych, a nie w sposób programowy formantów po stronie serwera.</span><span class="sxs-lookup"><span data-stu-id="9d11a-124">This approach allows you to declaratively connect server-side controls directly to data sources, rather than using a programmatic approach.</span></span>

### <a name="coding-by-hand-to-bind-data"></a><span data-ttu-id="9d11a-125">Kodowanie ręcznie do powiązania danych</span><span class="sxs-lookup"><span data-stu-id="9d11a-125">Coding By Hand to Bind Data</span></span>

<span data-ttu-id="9d11a-126">Dodając kod ręcznie obejmuje odczytu wartości, sprawdzanie wartości null próby przekonwertować go do odpowiedniego typu, sprawdzanie, czy konwersja powiodła się i na koniec przy użyciu wartości w zapytaniu.</span><span class="sxs-lookup"><span data-stu-id="9d11a-126">Adding code by hand involves reading the value, checking for a null value, attempting to convert it to the appropriate type, checking whether the conversion was successful, and finally, using the value in the query.</span></span> <span data-ttu-id="9d11a-127">Jeśli chcesz zachować pełną kontrolę nad logika dostępu do danych, czy posłuż się tą metodą.</span><span class="sxs-lookup"><span data-stu-id="9d11a-127">You would use this approach when you need to retain full control over your data-access logic.</span></span>

### <a name="using-model-binding-to-bind-data"></a><span data-ttu-id="9d11a-128">Przy użyciu modelu powiązanie do powiązania danych</span><span class="sxs-lookup"><span data-stu-id="9d11a-128">Using Model Binding to Bind Data</span></span>

<span data-ttu-id="9d11a-129">Przy użyciu wiązania modelu umożliwia tworzenie powiązań wyników za pomocą znacznie mniej kodu i daje możliwość ponownego użycia funkcji w całej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="9d11a-129">Using model binding allows you to bind results using far less code and gives you the ability to reuse the functionality throughout your application.</span></span> <span data-ttu-id="9d11a-130">Powiązanie modelu ma na celu uproszczenie pracy z logiką fokus kodu dostępu do danych przy jednoczesnym zachowaniu zalet framework rozbudowane, powiązanie danych.</span><span class="sxs-lookup"><span data-stu-id="9d11a-130">Model binding aims to simplify working with code-focused data-access logic while still retaining the benefits of a rich, data-binding framework.</span></span>

## <a name="displaying-products"></a><span data-ttu-id="9d11a-131">Wyświetlanie produktów</span><span class="sxs-lookup"><span data-stu-id="9d11a-131">Displaying Products</span></span>

<span data-ttu-id="9d11a-132">W tym samouczku użyjesz wiązania modelu do powiązania danych.</span><span class="sxs-lookup"><span data-stu-id="9d11a-132">In this tutorial, you'll use model binding to bind data.</span></span> <span data-ttu-id="9d11a-133">Aby skonfigurować formantu danych na potrzeby tworzenia powiązania modelu wybierz dane, należy ustawić formantu `SelectMethod` właściwość na nazwę metody w kodzie strony.</span><span class="sxs-lookup"><span data-stu-id="9d11a-133">To configure a data control to use model binding to select data, you set the control's `SelectMethod` property to the name of a method in the page's code.</span></span> <span data-ttu-id="9d11a-134">Formant danych wywołuje metodę we właściwym czasie w cyklu życia strony i automatycznie wiąże zwróconych danych.</span><span class="sxs-lookup"><span data-stu-id="9d11a-134">The data control calls the method at the appropriate time in the page life cycle and automatically binds the returned data.</span></span> <span data-ttu-id="9d11a-135">Nie istnieje potrzeba aby jawnie wywołać `DataBind` metody.</span><span class="sxs-lookup"><span data-stu-id="9d11a-135">There's no need to explicitly call the `DataBind` method.</span></span>

<span data-ttu-id="9d11a-136">Wykonując poniższe kroki, będą wprowadzane zmiany kodu znaczników w *ProductList.aspx* strony, dzięki czemu można wyświetlić strony, produktów.</span><span class="sxs-lookup"><span data-stu-id="9d11a-136">Using the steps below, you'll modify the markup in the *ProductList.aspx* page so that the page can display products.</span></span>

1. <span data-ttu-id="9d11a-137">W **Eksploratora rozwiązań**, otwórz *ProductList.aspx* strony.</span><span class="sxs-lookup"><span data-stu-id="9d11a-137">In **Solution Explorer**, open the *ProductList.aspx* page.</span></span>
2. <span data-ttu-id="9d11a-138">Zastąp istniejący kod znaczników następujący kod:</span><span class="sxs-lookup"><span data-stu-id="9d11a-138">Replace the existing markup with the following markup:</span></span>   

    [!code-aspx[Main](display_data_items_and_details/samples/sample1.aspx)]

<span data-ttu-id="9d11a-139">Ten kod zawiera **ListView** formantu o nazwie "productList", aby wyświetlić te produkty.</span><span class="sxs-lookup"><span data-stu-id="9d11a-139">This code uses a **ListView** control named "productList" to display the products.</span></span>

[!code-aspx[Main](display_data_items_and_details/samples/sample2.aspx)]

<span data-ttu-id="9d11a-140">**ListView** kontroli wyświetla dane w formacie, który można zdefiniować przy użyciu szablonów i style.</span><span class="sxs-lookup"><span data-stu-id="9d11a-140">The **ListView** control displays data in a format that you define by using templates and styles.</span></span> <span data-ttu-id="9d11a-141">Jest to przydatne w przypadku danych w żadnej identycznych struktury.</span><span class="sxs-lookup"><span data-stu-id="9d11a-141">It is useful for data in any repeating structure.</span></span> <span data-ttu-id="9d11a-142">To **ListView** przykładzie pokazano, po prostu dane z bazy danych, jednak umożliwia użytkownikom edytowanie, wstawianie i usuwanie danych i sortowanie i dane strony wszystko to bez kodu.</span><span class="sxs-lookup"><span data-stu-id="9d11a-142">This **ListView** example simply shows data from the database, however you can enable users to edit, insert, and delete data, and to sort and page data, all without code.</span></span>

<span data-ttu-id="9d11a-143">Przez ustawienie `ItemType` właściwości w **ListView** kontrolować wyrażenia wiązania danych `Item` jest dostępny i formantu staje się silnie typizowane.</span><span class="sxs-lookup"><span data-stu-id="9d11a-143">By setting the `ItemType` property in the **ListView** control, the data-binding expression `Item` is available and the control becomes strongly typed.</span></span> <span data-ttu-id="9d11a-144">Jak wspomniano w poprzedniej samouczek, możesz wybrać szczegóły obiektu elementu za pomocą funkcji IntelliSense, takich jak określanie `ProductName`:</span><span class="sxs-lookup"><span data-stu-id="9d11a-144">As mentioned in the previous tutorial, you can select details of the Item object using IntelliSense, such as specifying the `ProductName`:</span></span>

![Wyświetlanie danych elementów i szczegóły — IntelliSense](display_data_items_and_details/_static/image1.png)

<span data-ttu-id="9d11a-146">Ponadto używasz wiązania modelu do określenia `SelectMethod` wartość.</span><span class="sxs-lookup"><span data-stu-id="9d11a-146">In addition, you are using model binding to specify a `SelectMethod` value.</span></span> <span data-ttu-id="9d11a-147">Ta wartość (`GetProducts`) odpowiada metodzie doda kodu do wyświetlenia produktów w następnym kroku.</span><span class="sxs-lookup"><span data-stu-id="9d11a-147">This value (`GetProducts`) will correspond to the method that you will add to the code behind to display products in the next step.</span></span>

### <a name="adding-code-to-display-products"></a><span data-ttu-id="9d11a-148">Dodawanie kodu do wyświetlenia produktów</span><span class="sxs-lookup"><span data-stu-id="9d11a-148">Adding Code to Display Products</span></span>

<span data-ttu-id="9d11a-149">W tym kroku zostanie Dodaj kod, aby wypełnić **ListView** formantu z danymi produktu z bazy danych.</span><span class="sxs-lookup"><span data-stu-id="9d11a-149">In this step, you'll add code to populate the **ListView** control with product data from the database.</span></span> <span data-ttu-id="9d11a-150">Ten kod będzie obsługiwać produktów przedstawiający poszczególnych kategorii, a także wyświetlanie wszystkich produktów.</span><span class="sxs-lookup"><span data-stu-id="9d11a-150">The code will support showing products by individual category, as well as showing all products.</span></span>

1. <span data-ttu-id="9d11a-151">W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy *ProductList.aspx* , a następnie kliknij przycisk **kod widoku**.</span><span class="sxs-lookup"><span data-stu-id="9d11a-151">In **Solution Explorer**, right-click *ProductList.aspx* and then click **View Code**.</span></span>
2. <span data-ttu-id="9d11a-152">Zastąp istniejący kod w *ProductList.aspx.cs* pliku następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="9d11a-152">Replace the existing code in the *ProductList.aspx.cs* file with the following code:</span></span>   

    [!code-csharp[Main](display_data_items_and_details/samples/sample3.cs)]

<span data-ttu-id="9d11a-153">Ten kod zawiera `GetProducts` metodę, która odwołuje się do niego `ItemType` właściwość **ListView** kontroli w *ProductList.aspx* strony.</span><span class="sxs-lookup"><span data-stu-id="9d11a-153">This code shows the `GetProducts` method that's referenced by the `ItemType` property of the **ListView** control in the *ProductList.aspx* page.</span></span> <span data-ttu-id="9d11a-154">Aby ograniczyć wyniki do jednej konkretnej kategorii w bazie danych, ustawia kod `categoryId` niż wartość ciąg zapytania przekazany do *ProductList.aspx* gdy *ProductList.aspx* jest strony przejście.</span><span class="sxs-lookup"><span data-stu-id="9d11a-154">To limit the results to a specific category in the database, the code sets the `categoryId` value from the query string value passed to the *ProductList.aspx* page when the *ProductList.aspx* page is navigated to.</span></span> <span data-ttu-id="9d11a-155">`QueryStringAttribute` Klasy w `System.Web.ModelBinding` przestrzeni nazw służy do pobierania wartości identyfikatora zmiennej ciągu zapytania. To powoduje, że wiązanie modelu próby powiązać wartości z ciągu zapytania do `categoryId` parametru w czasie wykonywania.</span><span class="sxs-lookup"><span data-stu-id="9d11a-155">The `QueryStringAttribute` class in the `System.Web.ModelBinding` namespace is used to retrieve the value of the query string variable id. This instructs model binding to try to bind a value from the query string to the `categoryId` parameter at run time.</span></span>

<span data-ttu-id="9d11a-156">W przypadku prawidłową kategorią jest przekazywany jako ciąg zapytania do strony, wyniki zapytania są ograniczone do tych produktów w bazie danych, które pasują `categoryId` wartość.</span><span class="sxs-lookup"><span data-stu-id="9d11a-156">When a valid category is passed as a query string to the page, the results of the query are limited to those products in the database that match the `categoryId` value.</span></span> <span data-ttu-id="9d11a-157">Na przykład jeśli adres URL, który *ProductsList.aspx* strona jest następujące:</span><span class="sxs-lookup"><span data-stu-id="9d11a-157">For instance, if the URL to the *ProductsList.aspx* page is the following:</span></span>

[!code-console[Main](display_data_items_and_details/samples/sample4.cmd)]

<span data-ttu-id="9d11a-158">Na stronie są wyświetlane tylko te produkty gdzie `category` jest równe `1`.</span><span class="sxs-lookup"><span data-stu-id="9d11a-158">The page displays only the products where the `category` equals `1`.</span></span>

<span data-ttu-id="9d11a-159">Jeśli ciąg zapytania nie jest dołączony podczas nawigowania do *ProductList.aspx* stronę, wszystkie produkty zostaną wyświetlone.</span><span class="sxs-lookup"><span data-stu-id="9d11a-159">If no query string is included when navigating to the *ProductList.aspx* page, all products will be displayed.</span></span>

<span data-ttu-id="9d11a-160">Źródła wartości te metody są określane jako *wartość dostawców* (takich jak *QueryString*), i wskazujący, który dostawca wartości do użycia atrybuty parametru są określane jako wartość atrybuty dostawcy (takich jak "`id`").</span><span class="sxs-lookup"><span data-stu-id="9d11a-160">The sources of values for these methods are referred to as *value providers* (such as *QueryString*), and the parameter attributes that indicate which value provider to use are referred to as value provider attributes (such as "`id`").</span></span> <span data-ttu-id="9d11a-161">Program ASP.NET zawiera dostawców wartości i atrybuty odpowiednie dla wszystkich typowych źródeł danych wejściowych użytkownika w aplikacji formularzy sieci Web, na przykład ciąg zapytania, plików cookie, wartości formularza, kontrolek, stan widoku, stan sesji i właściwości profilu.</span><span class="sxs-lookup"><span data-stu-id="9d11a-161">ASP.NET includes value providers and corresponding attributes for all of the typical sources of user input in a Web Forms application, such as the query string, cookies, form values, controls, view state, session state, and profile properties.</span></span> <span data-ttu-id="9d11a-162">Można również napisać dostawców wartości niestandardowych.</span><span class="sxs-lookup"><span data-stu-id="9d11a-162">You can also write custom value providers.</span></span>

### <a name="running-the-application"></a><span data-ttu-id="9d11a-163">Uruchamianie aplikacji</span><span class="sxs-lookup"><span data-stu-id="9d11a-163">Running the Application</span></span>

<span data-ttu-id="9d11a-164">Uruchom aplikację teraz, aby zobaczyć, jak można wyświetlić wszystkie produkty lub po prostu zestawu ograniczone według kategorii produktów.</span><span class="sxs-lookup"><span data-stu-id="9d11a-164">Run the application now to see how you can view all of the products or just a set of products limited by category.</span></span>

1. <span data-ttu-id="9d11a-165">W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy *Default.aspx* i wybrać opcję **Wyświetl w przeglądarce**.</span><span class="sxs-lookup"><span data-stu-id="9d11a-165">In the **Solution Explorer**, right-click the *Default.aspx* page and select **View in Browser**.</span></span>  
 <span data-ttu-id="9d11a-166">W przeglądarce zostanie otworzyć i wyświetlić *Default.aspx* strony.</span><span class="sxs-lookup"><span data-stu-id="9d11a-166">The browser will open and show the *Default.aspx* page.</span></span>
2. <span data-ttu-id="9d11a-167">Wybierz **samochodów** menu nawigacji Kategoria produktu.</span><span class="sxs-lookup"><span data-stu-id="9d11a-167">Select **Cars** from the product category navigation menu.</span></span>  
 <span data-ttu-id="9d11a-168">*ProductList.aspx* zostanie wyświetlona strona, pokazujący tylko produkty uwzględnione w kategorii "Samochodów".</span><span class="sxs-lookup"><span data-stu-id="9d11a-168">The *ProductList.aspx* page is displayed showing only products included in the "Cars" category.</span></span> <span data-ttu-id="9d11a-169">W dalszej części tego samouczka są wyświetlane takie szczegóły produktu.</span><span class="sxs-lookup"><span data-stu-id="9d11a-169">Later in this tutorial, you will display product details.</span></span>  

    ![Wyświetlanie danych elementów i szczegóły — samochodów](display_data_items_and_details/_static/image2.png)
3. <span data-ttu-id="9d11a-171">Wybierz **produktów** z menu nawigacji u góry.</span><span class="sxs-lookup"><span data-stu-id="9d11a-171">Select **Products** from the navigation menu at the top.</span></span>  
 <span data-ttu-id="9d11a-172">Ponownie *ProductList.aspx* strona jest wyświetlana, ale tym razem zawiera całą listę produktów.</span><span class="sxs-lookup"><span data-stu-id="9d11a-172">Again, the *ProductList.aspx* page is displayed, however this time it shows the entire list of products.</span></span>   

    ![Wyświetlanie danych elementów i szczegóły — produktów](display_data_items_and_details/_static/image3.png)
4. <span data-ttu-id="9d11a-174">Zamknij przeglądarkę i powrócić do programu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="9d11a-174">Close the browser and return to Visual Studio.</span></span>

### <a name="adding-a-data-control-to-display-product-details"></a><span data-ttu-id="9d11a-175">Dodawanie formantu danych, aby wyświetlić szczegółowe informacje o produkcie</span><span class="sxs-lookup"><span data-stu-id="9d11a-175">Adding a Data Control to Display Product Details</span></span>

<span data-ttu-id="9d11a-176">Następnie będzie modyfikowania kodu znaczników w *ProductDetails.aspx* strony dodanego w poprzedniej samouczka, dzięki czemu strony można wyświetlić informacje o indywidualnych produktów.</span><span class="sxs-lookup"><span data-stu-id="9d11a-176">Next, you'll modify the markup in the *ProductDetails.aspx* page that you added in the previous tutorial so that the page can display information about an individual product.</span></span>

1. <span data-ttu-id="9d11a-177">W **Eksploratora rozwiązań**, otwórz *ProductDetails.aspx* strony.</span><span class="sxs-lookup"><span data-stu-id="9d11a-177">In **Solution Explorer**, open the *ProductDetails.aspx* page.</span></span>
2. <span data-ttu-id="9d11a-178">Zastąp istniejący kod znaczników następujący kod:</span><span class="sxs-lookup"><span data-stu-id="9d11a-178">Replace the existing markup with the following markup:</span></span>   

    [!code-aspx[Main](display_data_items_and_details/samples/sample5.aspx)]

<span data-ttu-id="9d11a-179">Ten kod zawiera **FormView** formantu, aby wyświetlić szczegółowe informacje o indywidualnych produktów.</span><span class="sxs-lookup"><span data-stu-id="9d11a-179">This code uses a **FormView** control to display details about an individual product.</span></span> <span data-ttu-id="9d11a-180">Ten kod znaczników używa metody, takie jak te, które są używane do wyświetlania danych w *ProductList.aspx* strony.</span><span class="sxs-lookup"><span data-stu-id="9d11a-180">This markup uses methods like those that are used to display data in the *ProductList.aspx* page.</span></span> <span data-ttu-id="9d11a-181">**FormView** kontroli jest używana do wyświetlania pojedynczego rekordu jednocześnie ze źródła danych.</span><span class="sxs-lookup"><span data-stu-id="9d11a-181">The **FormView** control is used to display a single record at a time from a data source.</span></span> <span data-ttu-id="9d11a-182">Jeśli używasz **FormView** kontroli, utworzyć szablony, aby wyświetlić i edytować wartości powiązane z danymi.</span><span class="sxs-lookup"><span data-stu-id="9d11a-182">When you use the **FormView** control, you create templates to display and edit data-bound values.</span></span> <span data-ttu-id="9d11a-183">Szablony zawierają formanty, wyrażenia powiązania i formatowanie definiujących wygląd i funkcjonalność formularza.</span><span class="sxs-lookup"><span data-stu-id="9d11a-183">The templates contain controls, binding expressions, and formatting that define the look and functionality of the form.</span></span>

<span data-ttu-id="9d11a-184">Aby połączyć powyżej znaczników do bazy danych, należy dodać dodatkowy kod w celu *ProductDetails.aspx* kodu.</span><span class="sxs-lookup"><span data-stu-id="9d11a-184">To connect the above markup to the database, you must add additional code to the *ProductDetails.aspx* code.</span></span>

1. <span data-ttu-id="9d11a-185">W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy *ProductDetails.aspx* , a następnie kliknij przycisk **kod widoku**.</span><span class="sxs-lookup"><span data-stu-id="9d11a-185">In **Solution Explorer**, right-click *ProductDetails.aspx* and then click **View Code**.</span></span>  
   <span data-ttu-id="9d11a-186">*ProductDetails.aspx.cs* plik zostanie wyświetlony.</span><span class="sxs-lookup"><span data-stu-id="9d11a-186">The *ProductDetails.aspx.cs* file will be displayed.</span></span>
2. <span data-ttu-id="9d11a-187">Zastąp istniejący kod następujący kod:</span><span class="sxs-lookup"><span data-stu-id="9d11a-187">Replace the existing code with the following code:</span></span>   

    [!code-csharp[Main](display_data_items_and_details/samples/sample6.cs)]

<span data-ttu-id="9d11a-188">Sprawdza, czy ten kod "`productID`" wartość ciągu zapytania.</span><span class="sxs-lookup"><span data-stu-id="9d11a-188">This code checks for a "`productID`" query-string value.</span></span> <span data-ttu-id="9d11a-189">Jeśli wartość nieprawidłowy ciąg zapytania zostanie znaleziony, zostanie wyświetlony zgodnego produktu.</span><span class="sxs-lookup"><span data-stu-id="9d11a-189">If a valid query-string value is found, the matching product is displayed.</span></span> <span data-ttu-id="9d11a-190">Jeśli nie ciąg zapytania zostanie znaleziony, lub wartość ciągu zapytania jest nieprawidłowa, produktu nie jest wyświetlany na *ProductDetails.aspx* strony.</span><span class="sxs-lookup"><span data-stu-id="9d11a-190">If no query-string is found, or the query-string value is not valid, no product is displayed on the *ProductDetails.aspx* page.</span></span>

### <a name="running-the-application"></a><span data-ttu-id="9d11a-191">Uruchamianie aplikacji</span><span class="sxs-lookup"><span data-stu-id="9d11a-191">Running the Application</span></span>

<span data-ttu-id="9d11a-192">Teraz można uruchomić aplikację, aby zobaczyć indywidualnych produktów, wyświetlane na podstawie identyfikatora produktu.</span><span class="sxs-lookup"><span data-stu-id="9d11a-192">Now you can run the application to see an individual product displayed based on the id of the product.</span></span>

1. <span data-ttu-id="9d11a-193">Naciśnij klawisz **F5** while w programie Visual Studio, aby uruchomić aplikację.</span><span class="sxs-lookup"><span data-stu-id="9d11a-193">Press **F5** while in Visual Studio to run the application.</span></span>  
 <span data-ttu-id="9d11a-194">W przeglądarce zostanie otworzyć i wyświetlić *Default.aspx* strony.</span><span class="sxs-lookup"><span data-stu-id="9d11a-194">The browser will open and show the *Default.aspx* page.</span></span>
2. <span data-ttu-id="9d11a-195">Wybierz "Łodzi" w menu nawigacji kategorii.</span><span class="sxs-lookup"><span data-stu-id="9d11a-195">Select "Boats" from the category navigation menu.</span></span>  
 <span data-ttu-id="9d11a-196">*ProductList.aspx* zostanie wyświetlona strona.</span><span class="sxs-lookup"><span data-stu-id="9d11a-196">The *ProductList.aspx* page is displayed.</span></span>
3. <span data-ttu-id="9d11a-197">Wybierz produkt "Papieru łodzi" na liście produktów.</span><span class="sxs-lookup"><span data-stu-id="9d11a-197">Select the "Paper Boat" product from the product list.</span></span>  
 <span data-ttu-id="9d11a-198">*ProductDetails.aspx* zostanie wyświetlona strona.</span><span class="sxs-lookup"><span data-stu-id="9d11a-198">The *ProductDetails.aspx* page is displayed.</span></span>   

    ![Wyświetlanie danych elementów i szczegóły — produktów](display_data_items_and_details/_static/image4.png)
4. <span data-ttu-id="9d11a-200">Zamknij przeglądarkę.</span><span class="sxs-lookup"><span data-stu-id="9d11a-200">Close the browser.</span></span>

## <a name="summary"></a><span data-ttu-id="9d11a-201">Podsumowanie</span><span class="sxs-lookup"><span data-stu-id="9d11a-201">Summary</span></span>

<span data-ttu-id="9d11a-202">W tym samouczku serii mają Dodawanie znaczników i kodu, aby wyświetlić listę produktów i wyświetlić informacje szczegółowe.</span><span class="sxs-lookup"><span data-stu-id="9d11a-202">In this tutorial of the series you have add markup and code to display a product list and to display product details.</span></span> <span data-ttu-id="9d11a-203">W trakcie tego procesu uzyskanych o jednoznacznie danych kontrolki, wiązania modelu i dostawców wartości.</span><span class="sxs-lookup"><span data-stu-id="9d11a-203">During this process you have learned about strongly typed data controls, model binding, and value providers.</span></span> <span data-ttu-id="9d11a-204">W następnym samouczku zostanie dodana koszyk Wingtip Toys przykładowej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="9d11a-204">In the next tutorial, you'll add a shopping cart to the Wingtip Toys sample application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="9d11a-205">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="9d11a-205">Additional Resources</span></span>

[<span data-ttu-id="9d11a-206">Trwa pobieranie i wyświetlanie danych z wiązania modelu i formularzy sieci web</span><span class="sxs-lookup"><span data-stu-id="9d11a-206">Retrieving and displaying data with model binding and web forms</span></span>](../../presenting-and-managing-data/model-binding/retrieving-data.md)

> [!div class="step-by-step"]
> <span data-ttu-id="9d11a-207">[Poprzednie](ui_and_navigation.md)
> [dalej](shopping-cart.md)</span><span class="sxs-lookup"><span data-stu-id="9d11a-207">[Previous](ui_and_navigation.md)
[Next](shopping-cart.md)</span></span>
