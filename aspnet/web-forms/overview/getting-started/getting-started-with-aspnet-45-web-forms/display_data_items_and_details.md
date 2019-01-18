---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/display_data_items_and_details
title: Wyświetlanie elementów danych i szczegółowych informacji | Dokumentacja firmy Microsoft
author: Erikre
description: W tej serii samouczków opisano podstawy tworzenia aplikacji formularzy sieci Web ASP.NET za pomocą programu ASP.NET 4.7 i Microsoft Visual Studio 2017
ms.author: riande
ms.date: 1/04/2019
ms.assetid: 64a491a8-0ed6-4c2f-9c1c-412962eb6006
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/display_data_items_and_details
msc.type: authoredcontent
ms.openlocfilehash: acc2f8e78375ef0455d467e2af750ecbee623224
ms.sourcegitcommit: 184ba5b44d1c393076015510ac842b77bc9d4d93
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/18/2019
ms.locfileid: "54396228"
---
<a name="display-data-items-and-details"></a><span data-ttu-id="3c82c-103">Wyświetlanie elementów danych i szczegóły</span><span class="sxs-lookup"><span data-stu-id="3c82c-103">Display data items and details</span></span>
====================
<span data-ttu-id="3c82c-104">przez [Erik Reitan](https://github.com/Erikre)</span><span class="sxs-lookup"><span data-stu-id="3c82c-104">by [Erik Reitan](https://github.com/Erikre)</span></span>

> <span data-ttu-id="3c82c-105">W tej serii samouczków dowiesz się, podstawy tworzenia aplikacji formularzy sieci Web ASP.NET za pomocą programu ASP.NET 4.7 i Microsoft Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="3c82c-105">This tutorial series teaches you the basics of building an ASP.NET Web Forms application with ASP.NET 4.7 and Microsoft Visual Studio 2017.</span></span>

<span data-ttu-id="3c82c-106">W tym samouczku dowiesz się, jak wyświetlać elementy danych i szczegóły elementu danych przy użyciu formularzy sieci Web ASP.NET i Entity Framework Code First.</span><span class="sxs-lookup"><span data-stu-id="3c82c-106">In this tutorial, you'll learn how to display data items and data item details with ASP.NET Web Forms and Entity Framework Code First.</span></span> <span data-ttu-id="3c82c-107">Ten samouczek opiera się na poprzednim samouczku "Interfejsu użytkownika i Nawigacja" jako części serii samouczków o nazwie Wingtip zabawki Store.</span><span class="sxs-lookup"><span data-stu-id="3c82c-107">This tutorial builds on the previous "UI and Navigation" tutorial as part of the Wingtip Toy Store tutorial series.</span></span> <span data-ttu-id="3c82c-108">Po ukończeniu tego samouczka, zobaczysz produktów na *ProductsList.aspx* strony i szczegóły na temat produktu *ProductDetails.aspx* strony.</span><span class="sxs-lookup"><span data-stu-id="3c82c-108">After completing this tutorial, you'll see products on the *ProductsList.aspx* page and a product's details on the *ProductDetails.aspx* page.</span></span>

## <a name="youll-learn-how-to"></a><span data-ttu-id="3c82c-109">Dowiesz się, jak:</span><span class="sxs-lookup"><span data-stu-id="3c82c-109">You'll learn how to:</span></span>

- <span data-ttu-id="3c82c-110">Dodaj formant do wyświetlania produktów z bazy danych</span><span class="sxs-lookup"><span data-stu-id="3c82c-110">Add a data control to display products from the database</span></span>
- <span data-ttu-id="3c82c-111">Połącz kontrolki danych do wybranych danych</span><span class="sxs-lookup"><span data-stu-id="3c82c-111">Connect a data control to the selected data</span></span>
- <span data-ttu-id="3c82c-112">Dodaj formant danych, aby wyświetlić szczegółowe informacje z bazy danych</span><span class="sxs-lookup"><span data-stu-id="3c82c-112">Add a data control to display product details from the database</span></span>
- <span data-ttu-id="3c82c-113">Pobieranie wartości z ciągu zapytania i użyć tej wartości, aby ograniczyć dane, które są pobierane z bazy danych</span><span class="sxs-lookup"><span data-stu-id="3c82c-113">Retrieve a value from the query string and use that value to limit the data that's retrieved from the database</span></span>

### <a name="features-introduced-in-this-tutorial"></a><span data-ttu-id="3c82c-114">Funkcje wprowadzone w tym samouczku:</span><span class="sxs-lookup"><span data-stu-id="3c82c-114">Features introduced in this tutorial:</span></span>

- <span data-ttu-id="3c82c-115">Wiązanie modelu</span><span class="sxs-lookup"><span data-stu-id="3c82c-115">Model binding</span></span>
- <span data-ttu-id="3c82c-116">Dostawców wartości</span><span class="sxs-lookup"><span data-stu-id="3c82c-116">Value providers</span></span>

## <a name="add-a-data-control"></a><span data-ttu-id="3c82c-117">Dodawanie kontrolki danych</span><span class="sxs-lookup"><span data-stu-id="3c82c-117">Add a data control</span></span>

<span data-ttu-id="3c82c-118">Jest kilka różnych opcji można użyć, aby powiązać dane z kontrolką serwera.</span><span class="sxs-lookup"><span data-stu-id="3c82c-118">You can use a few different options to bind data to a server control.</span></span> <span data-ttu-id="3c82c-119">Najbardziej typowe obejmują:</span><span class="sxs-lookup"><span data-stu-id="3c82c-119">The most common include:</span></span>

 * <span data-ttu-id="3c82c-120">Dodawanie kontroli źródła danych</span><span class="sxs-lookup"><span data-stu-id="3c82c-120">Adding a data source control</span></span>
 * <span data-ttu-id="3c82c-121">Ręczne dodawanie kodu</span><span class="sxs-lookup"><span data-stu-id="3c82c-121">Adding code by hand</span></span>
 * <span data-ttu-id="3c82c-122">Przy użyciu wiązania modelu</span><span class="sxs-lookup"><span data-stu-id="3c82c-122">Using model binding</span></span>

### <a name="use-a-data-source-control-to-bind-data"></a><span data-ttu-id="3c82c-123">Użyj kontroli źródła danych, aby powiązać dane</span><span class="sxs-lookup"><span data-stu-id="3c82c-123">Use a data source control to bind data</span></span>

<span data-ttu-id="3c82c-124">Dodawanie kontroli źródła danych umożliwia połączenie kontroli źródła danych do formantu, który wyświetla dane.</span><span class="sxs-lookup"><span data-stu-id="3c82c-124">Adding a data source control allows you to link the data source control to the control that displays the data.</span></span> <span data-ttu-id="3c82c-125">W przypadku tej metody użytkownik może w sposób deklaratywny, a nie programowo, łączenie kontrolek po stronie serwera ze źródłami danych.</span><span class="sxs-lookup"><span data-stu-id="3c82c-125">With this approach, you can declaratively,  rather than programmatically, connect server-side controls to data sources.</span></span>

### <a name="code-by-hand-to-bind-data"></a><span data-ttu-id="3c82c-126">Kod ręcznie, aby powiązać dane</span><span class="sxs-lookup"><span data-stu-id="3c82c-126">Code by hand to bind data</span></span>

<span data-ttu-id="3c82c-127">Kodowanie ręcznie obejmuje:</span><span class="sxs-lookup"><span data-stu-id="3c82c-127">Coding by hand involves:</span></span>

1. <span data-ttu-id="3c82c-128">Odczytywanie wartości</span><span class="sxs-lookup"><span data-stu-id="3c82c-128">Reading a value</span></span>
2. <span data-ttu-id="3c82c-129">Sprawdzanie, jeśli ma wartość null</span><span class="sxs-lookup"><span data-stu-id="3c82c-129">Checking if it's null</span></span>
3. <span data-ttu-id="3c82c-130">Podczas konwertowania go do odpowiedniego typu</span><span class="sxs-lookup"><span data-stu-id="3c82c-130">Converting it to an appropriate type</span></span>
4. <span data-ttu-id="3c82c-131">Sprawdzanie, czy Powodzenie konwersji</span><span class="sxs-lookup"><span data-stu-id="3c82c-131">Checking conversion success</span></span>
5. <span data-ttu-id="3c82c-132">Przy użyciu wartości w zapytaniu</span><span class="sxs-lookup"><span data-stu-id="3c82c-132">Using the value in the query</span></span> 

<span data-ttu-id="3c82c-133">Takie podejście umożliwia pełną kontrolę nad logika dostępu do danych.</span><span class="sxs-lookup"><span data-stu-id="3c82c-133">This approach lets you have full control over your data-access logic.</span></span>

### <a name="use-model-binding-to-bind-data"></a><span data-ttu-id="3c82c-134">Użyj wiązania modelu do powiązania danych</span><span class="sxs-lookup"><span data-stu-id="3c82c-134">Use model binding to bind data</span></span>

<span data-ttu-id="3c82c-135">Wiązanie modelu pozwala powiązać wyniki z znacznie mniejszej ilości kodu i daje możliwość ponownego użycia funkcji w całej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="3c82c-135">Model binding lets you bind results with far less code and gives you the ability to reuse the functionality throughout your application.</span></span> <span data-ttu-id="3c82c-136">Jego upraszcza pracę z logiki skoncentrowane na kodzie dostępu do danych przy jednoczesnym dalszym zapewnianiu framework rozbudowane, powiązanie danych.</span><span class="sxs-lookup"><span data-stu-id="3c82c-136">It simplifies working with code-focused data-access logic while still providing a rich, data-binding framework.</span></span>

## <a name="display-products"></a><span data-ttu-id="3c82c-137">Wyświetl produkty</span><span class="sxs-lookup"><span data-stu-id="3c82c-137">Display products</span></span>

<span data-ttu-id="3c82c-138">W tym samouczku użyjesz wiązania modelu do powiązania danych.</span><span class="sxs-lookup"><span data-stu-id="3c82c-138">In this tutorial, you'll use model binding to bind data.</span></span> <span data-ttu-id="3c82c-139">Aby skonfigurować kontrolkę danych na potrzeby tworzenia powiązania modelu wybierz dane, należy ustawić formantu `SelectMethod` właściwość na nazwę metody w kodzie strony.</span><span class="sxs-lookup"><span data-stu-id="3c82c-139">To configure a data control to use model binding to select data, you set the control's `SelectMethod` property to a method name in the page's code.</span></span> <span data-ttu-id="3c82c-140">Kontrolki danych wywołuje metodę w odpowiednim czasie w cyklu życia strony i automatycznie wiąże zwracanych danych.</span><span class="sxs-lookup"><span data-stu-id="3c82c-140">The data control calls the method at the appropriate time in the page life cycle and automatically binds the returned data.</span></span> <span data-ttu-id="3c82c-141">Nie trzeba jawnie wywołać `DataBind` metody.</span><span class="sxs-lookup"><span data-stu-id="3c82c-141">There's no need to explicitly call the `DataBind` method.</span></span>

1. <span data-ttu-id="3c82c-142">W **Eksploratora rozwiązań**, otwórz *ProductList.aspx*.</span><span class="sxs-lookup"><span data-stu-id="3c82c-142">In **Solution Explorer**, open *ProductList.aspx*.</span></span>
2. <span data-ttu-id="3c82c-143">Zastąp istniejący kod znaczników ten kod znaczników:</span><span class="sxs-lookup"><span data-stu-id="3c82c-143">Replace the existing markup with this markup:</span></span>   

    [!code-aspx-csharp[Main](display_data_items_and_details/samples/sample1.aspx)]

<span data-ttu-id="3c82c-144">Ten kod używa **ListView** formantu o nazwie `productList` do wyświetlania produktów.</span><span class="sxs-lookup"><span data-stu-id="3c82c-144">This code uses a **ListView** control named `productList` to display products.</span></span>

[!code-aspx-csharp[Main](display_data_items_and_details/samples/sample2.aspx)]

<span data-ttu-id="3c82c-145">Za pomocą szablonów i stylów, możesz zdefiniować sposób, w jaki **ListView** kontrolka wyświetla dane.</span><span class="sxs-lookup"><span data-stu-id="3c82c-145">With templates and styles, you define how the **ListView** control displays data.</span></span> <span data-ttu-id="3c82c-146">Jest to przydatne dla danych w każdej powtarzające się struktury.</span><span class="sxs-lookup"><span data-stu-id="3c82c-146">It's useful for data in any repeating structure.</span></span> <span data-ttu-id="3c82c-147">Chociaż to **ListView** przykład po prostu wyświetla dane z bazy danych, można także bez konieczności pisania kodu i umożliwianie użytkownikom edytowanie, wstawianie i usuwanie danych i aby i sortowanie danych strony.</span><span class="sxs-lookup"><span data-stu-id="3c82c-147">Though this **ListView** example simply displays database data, you can also, without code, enable users to edit, insert, and delete data, and to sort and page data.</span></span>

<span data-ttu-id="3c82c-148">Ustawiając `ItemType` właściwości w **ListView** kontrolować wyrażenia wiązania danych `Item` jest dostępny i kontrolki staje się silnie typizowane.</span><span class="sxs-lookup"><span data-stu-id="3c82c-148">By setting the `ItemType` property in the **ListView** control, the data-binding expression `Item` is available and the control becomes strongly typed.</span></span> <span data-ttu-id="3c82c-149">Jak wspomniano w poprzednim samouczku, możesz wybrać element Szczegóły obiektu za pomocą funkcji IntelliSense, takich jak określanie `ProductName`:</span><span class="sxs-lookup"><span data-stu-id="3c82c-149">As mentioned in the previous tutorial, you can select Item object details with IntelliSense, such as specifying the `ProductName`:</span></span>

![Wyświetlanie danych elementów i szczegóły — funkcja IntelliSense](display_data_items_and_details/_static/image1.png)

<span data-ttu-id="3c82c-151">Jest również przy użyciu wiązania modelu, aby określić `SelectMethod` wartość.</span><span class="sxs-lookup"><span data-stu-id="3c82c-151">You're also using model binding to specify a `SelectMethod` value.</span></span> <span data-ttu-id="3c82c-152">Ta wartość (`GetProducts`) odnosi się do metody, należy dodać do kodu opóźnienie do wyświetlania produktów w następnym kroku.</span><span class="sxs-lookup"><span data-stu-id="3c82c-152">This value (`GetProducts`) corresponds to the method you'll add to the code behind to display products in the next step.</span></span>

### <a name="add-code-to-display-products"></a><span data-ttu-id="3c82c-153">Dodaj kod do wyświetlania produktów</span><span class="sxs-lookup"><span data-stu-id="3c82c-153">Add code to display products</span></span>

<span data-ttu-id="3c82c-154">W tym kroku dodasz kod, aby wypełnić **ListView** kontrolki z danymi produktów z bazy danych.</span><span class="sxs-lookup"><span data-stu-id="3c82c-154">In this step, you'll add code to populate the **ListView** control with product data from the database.</span></span> <span data-ttu-id="3c82c-155">Kod obsługuje wyświetlanie wszystkich produktów i produktów poszczególnych kategorii.</span><span class="sxs-lookup"><span data-stu-id="3c82c-155">The code supports showing all products and  individual category products.</span></span>

1. <span data-ttu-id="3c82c-156">W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy *ProductList.aspx* , a następnie wybierz **Wyświetl kod**.</span><span class="sxs-lookup"><span data-stu-id="3c82c-156">In **Solution Explorer**, right-click *ProductList.aspx* and then select **View Code**.</span></span>
2. <span data-ttu-id="3c82c-157">Zastąp istniejący kod w *ProductList.aspx.cs* pliku, w tym:</span><span class="sxs-lookup"><span data-stu-id="3c82c-157">Replace the existing code in the *ProductList.aspx.cs* file with this:</span></span>   

    [!code-csharp[Main](display_data_items_and_details/samples/sample3.cs)]

<span data-ttu-id="3c82c-158">Ten kod zawiera `GetProducts` metody, **ListView** kontrolki `ItemType` odwołuje się do właściwości w *ProductList.aspx* strony.</span><span class="sxs-lookup"><span data-stu-id="3c82c-158">This code shows the `GetProducts` method that the **ListView** control's `ItemType` property references in the *ProductList.aspx* page.</span></span> <span data-ttu-id="3c82c-159">Aby ograniczyć wyniki do kategorii konkretnej bazy danych, ustawia kod `categoryId` wartość wartość ciągu zapytania, które są przekazywane do *ProductList.aspx* gdy *ProductList.aspx* strona to przejście.</span><span class="sxs-lookup"><span data-stu-id="3c82c-159">To limit the results to a specific database category, the code sets the `categoryId` value from the query string value passed to the *ProductList.aspx* page when the *ProductList.aspx* page is navigated to.</span></span> <span data-ttu-id="3c82c-160">`QueryStringAttribute` Klasy w `System.Web.ModelBinding` przestrzeń nazw jest używana do pobrania wartości zmiennej ciągu zapytania `id`.</span><span class="sxs-lookup"><span data-stu-id="3c82c-160">The `QueryStringAttribute` class in the `System.Web.ModelBinding` namespace is used to retrieve the value of the query string variable `id`.</span></span> <span data-ttu-id="3c82c-161">To powoduje, że wiązanie modelu do wypróbowania można powiązać wartości z ciągu zapytania do `categoryId` parametru w czasie wykonywania.</span><span class="sxs-lookup"><span data-stu-id="3c82c-161">This instructs model binding to try to bind a value from the query string to the `categoryId` parameter at run time.</span></span>

<span data-ttu-id="3c82c-162">Jeśli prawidłową kategorię jest przekazywany jako ciąg zapytania do strony, wyniki zapytania są ograniczone do tych produktów w bazie danych, które odpowiadają `categoryId` wartość.</span><span class="sxs-lookup"><span data-stu-id="3c82c-162">When a valid category is passed as a query string to the page, the results of the query are limited to those products in the database that match the `categoryId` value.</span></span> <span data-ttu-id="3c82c-163">Na przykład jeśli *ProductsList.aspx* to jest adres URL strony:</span><span class="sxs-lookup"><span data-stu-id="3c82c-163">For instance, if the *ProductsList.aspx* page URL is this:</span></span>


[!code-console[Main](display_data_items_and_details/samples/sample4.cmd)]

<span data-ttu-id="3c82c-164">Na stronie są wyświetlane tylko produkty gdzie `categoryId` jest równa `1`.</span><span class="sxs-lookup"><span data-stu-id="3c82c-164">The page displays only the products where the `categoryId` equals `1`.</span></span>

<span data-ttu-id="3c82c-165">Wszystkie produkty są wyświetlane, jeśli nie ciągu zapytania jest dołączony, kiedy *ProductList.aspx* nosi nazwę strony.</span><span class="sxs-lookup"><span data-stu-id="3c82c-165">All products are displayed if no query string is included when the *ProductList.aspx* page is called.</span></span>

<span data-ttu-id="3c82c-166">Źródła wartości dla tych metod są określane jako *wartość dostawców* (takie jak *QueryString*), i są określane atrybuty parametrów, wskazujące, której dostawca wartości do użycia jako *wartości atrybutów dostawcy* (takie jak `id`).</span><span class="sxs-lookup"><span data-stu-id="3c82c-166">The sources of values for these methods are referred to as *value providers* (such as *QueryString*), and the parameter attributes that indicate which value provider to use are referred to as *value provider attributes* (such as `id`).</span></span> <span data-ttu-id="3c82c-167">Program ASP.NET zawiera dostawców wartości i odpowiednie atrybuty wszystkie typowe źródła danych wejściowych użytkownika w aplikacji formularzy sieci Web, takich jak ciąg zapytania, plików cookie, wartości formularza, formanty, stan widoku, stan sesji i właściwości profilu.</span><span class="sxs-lookup"><span data-stu-id="3c82c-167">ASP.NET includes value providers and corresponding attributes for all of the typical sources of user input in a Web Forms application such as the query string, cookies, form values, controls, view state, session state, and profile properties.</span></span> <span data-ttu-id="3c82c-168">Można także napisać dostawców wartości niestandardowych.</span><span class="sxs-lookup"><span data-stu-id="3c82c-168">You can also write custom value providers.</span></span>

### <a name="run-the-application"></a><span data-ttu-id="3c82c-169">Uruchamianie aplikacji</span><span class="sxs-lookup"><span data-stu-id="3c82c-169">Run the application</span></span>

<span data-ttu-id="3c82c-170">Uruchom aplikację teraz wyświetlić wszystkie produkty lub produkty z kategorii.</span><span class="sxs-lookup"><span data-stu-id="3c82c-170">Run the application now to view all products or a category's products.</span></span>

1. <span data-ttu-id="3c82c-171">Naciśnij klawisz **F5** podczas gdy w programie Visual Studio, aby uruchomić aplikację.</span><span class="sxs-lookup"><span data-stu-id="3c82c-171">Press **F5** while in Visual Studio to run the application.</span></span>  
   <span data-ttu-id="3c82c-172">Przeglądarki otwiera się i pokazuje *Default.aspx* strony.</span><span class="sxs-lookup"><span data-stu-id="3c82c-172">The browser opens and shows the *Default.aspx* page.</span></span>

2. <span data-ttu-id="3c82c-173">Wybierz **samochodów** menu nawigacji Kategoria produktu.</span><span class="sxs-lookup"><span data-stu-id="3c82c-173">Select **Cars** from the product category navigation menu.</span></span>  
   <span data-ttu-id="3c82c-174">*ProductList.aspx* stronie wyświetlane tylko **samochodów** kategorii produktów.</span><span class="sxs-lookup"><span data-stu-id="3c82c-174">The *ProductList.aspx* page displays showing only **Cars** category products.</span></span> <span data-ttu-id="3c82c-175">W dalszej części tego samouczka wyświetlisz informacje szczegółowe.</span><span class="sxs-lookup"><span data-stu-id="3c82c-175">Later in this tutorial, you'll display product details.</span></span>  

    ![Wyświetlanie danych elementów i szczegóły — samochodów](display_data_items_and_details/_static/image2.png)

3. <span data-ttu-id="3c82c-177">Wybierz **produktów** z menu nawigacji u góry.</span><span class="sxs-lookup"><span data-stu-id="3c82c-177">Select **Products** from the navigation menu at the top.</span></span>  
   <span data-ttu-id="3c82c-178">Ponownie *ProductList.aspx* zostanie wyświetlona strona, jednak tym razem pokazuje całą listę produktów.</span><span class="sxs-lookup"><span data-stu-id="3c82c-178">Again, the *ProductList.aspx* page is displayed, however this time it shows the entire list of products.</span></span>   

    ![Wyświetlanie danych elementów i szczegóły — produkty](display_data_items_and_details/_static/image3.png)

4. <span data-ttu-id="3c82c-180">Zamknij przeglądarkę i wróć do programu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="3c82c-180">Close the browser and return to Visual Studio.</span></span>

### <a name="add-a-data-control-to-display-product-details"></a><span data-ttu-id="3c82c-181">Dodaj formant danych, aby wyświetlić szczegółowe informacje o produkcie</span><span class="sxs-lookup"><span data-stu-id="3c82c-181">Add a data control to display product details</span></span>

<span data-ttu-id="3c82c-182">Następnie zmodyfikujesz kod znaczników w *ProductDetails.aspx* strony, które dodano w poprzednim samouczku, aby wyświetlić informacje o określonym produktem.</span><span class="sxs-lookup"><span data-stu-id="3c82c-182">Next, you'll modify the markup in the *ProductDetails.aspx* page that you added in the previous tutorial to display specific product information.</span></span>

1. <span data-ttu-id="3c82c-183">W **Eksploratora rozwiązań**, otwórz *ProductDetails.aspx*.</span><span class="sxs-lookup"><span data-stu-id="3c82c-183">In **Solution Explorer**, open *ProductDetails.aspx*.</span></span>

2. <span data-ttu-id="3c82c-184">Zastąp istniejący kod znaczników ten kod znaczników:</span><span class="sxs-lookup"><span data-stu-id="3c82c-184">Replace the existing markup with this markup:</span></span>

    [!code-aspx-csharp[Main](display_data_items_and_details/samples/sample5.aspx)] 

    <span data-ttu-id="3c82c-185">Ten kod używa **FormView** formantu, aby wyświetlić szczegóły określonego produktu.</span><span class="sxs-lookup"><span data-stu-id="3c82c-185">This code uses a **FormView** control to display specific product details.</span></span> <span data-ttu-id="3c82c-186">Ten kod znaczników używa metod, takich jak metody używane do wyświetlania danych w *ProductList.aspx* strony.</span><span class="sxs-lookup"><span data-stu-id="3c82c-186">This markup uses methods like the methods used to display data in the *ProductList.aspx* page.</span></span> <span data-ttu-id="3c82c-187">**FormView** formant jest używany do wyświetlania jeden rekord jednocześnie ze źródła danych.</span><span class="sxs-lookup"><span data-stu-id="3c82c-187">The **FormView** control is used to display a single record at a time from a data source.</span></span> <span data-ttu-id="3c82c-188">Kiedy używasz **FormView** kontrolki, tworzenie szablonów, aby wyświetlić i edytować wartości powiązanych z danymi.</span><span class="sxs-lookup"><span data-stu-id="3c82c-188">When you use the **FormView** control, you create templates to display and edit data-bound values.</span></span> <span data-ttu-id="3c82c-189">Te szablony zawierają kontrolek, powiązań wyrażenia i formatowanie zdefiniować wygląd formularza i nowe funkcje.</span><span class="sxs-lookup"><span data-stu-id="3c82c-189">These templates contain controls, binding expressions, and formatting that define the form's look and functionality.</span></span>

<span data-ttu-id="3c82c-190">Połączenie poprzedniego znaczników do bazy danych wymaga dodatkowego kodu.</span><span class="sxs-lookup"><span data-stu-id="3c82c-190">Connecting the previous markup to the database requires additional code.</span></span>

1. <span data-ttu-id="3c82c-191">W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy *ProductDetails.aspx* a następnie kliknij przycisk **Wyświetl kod**.</span><span class="sxs-lookup"><span data-stu-id="3c82c-191">In **Solution Explorer**, right-click *ProductDetails.aspx* and then click **View Code**.</span></span>  
   <span data-ttu-id="3c82c-192">*ProductDetails.aspx.cs* pliku jest wyświetlany.</span><span class="sxs-lookup"><span data-stu-id="3c82c-192">The *ProductDetails.aspx.cs* file is displayed.</span></span>

2. <span data-ttu-id="3c82c-193">Zastąp istniejący kod przy użyciu tego kodu:</span><span class="sxs-lookup"><span data-stu-id="3c82c-193">Replace the existing code with this code:</span></span>   

    [!code-csharp[Main](display_data_items_and_details/samples/sample6.cs)]

<span data-ttu-id="3c82c-194">Ten kod sprawdza, czy "`productID`" wartość ciągu zapytania.</span><span class="sxs-lookup"><span data-stu-id="3c82c-194">This code checks for a "`productID`" query-string value.</span></span> <span data-ttu-id="3c82c-195">Jeśli wartość nieprawidłowy ciąg zapytania zostanie znaleziony, zostanie wyświetlony zgodnego produktu.</span><span class="sxs-lookup"><span data-stu-id="3c82c-195">If a valid query-string value is found, the matching product is displayed.</span></span> <span data-ttu-id="3c82c-196">Jeśli ciąg zapytania nie zostanie odnaleziony lub jego wartość nie jest prawidłowa, jest wyświetlany żaden produkt.</span><span class="sxs-lookup"><span data-stu-id="3c82c-196">If the query-string isn't found, or its value isn't valid, no product is displayed.</span></span>

### <a name="run-the-application"></a><span data-ttu-id="3c82c-197">Uruchamianie aplikacji</span><span class="sxs-lookup"><span data-stu-id="3c82c-197">Run the application</span></span>

<span data-ttu-id="3c82c-198">Teraz można uruchomić aplikacji, aby zobaczyć indywidualnych produktów, wyświetlane oparte na identyfikator produktu.</span><span class="sxs-lookup"><span data-stu-id="3c82c-198">Now you can run the application to see an individual product displayed based on product ID.</span></span>

1. <span data-ttu-id="3c82c-199">Naciśnij klawisz **F5** podczas gdy w programie Visual Studio, aby uruchomić aplikację.</span><span class="sxs-lookup"><span data-stu-id="3c82c-199">Press **F5** while in Visual Studio to run the application.</span></span>  
   <span data-ttu-id="3c82c-200">Przeglądarki otwiera się i pokazuje *Default.aspx* strony.</span><span class="sxs-lookup"><span data-stu-id="3c82c-200">The browser opens and shows the *Default.aspx* page.</span></span>

2. <span data-ttu-id="3c82c-201">Wybierz **łodzi** z menu nawigacji kategorii.</span><span class="sxs-lookup"><span data-stu-id="3c82c-201">Select **Boats** from the category navigation menu.</span></span>  
   <span data-ttu-id="3c82c-202">*ProductList.aspx* zostanie wyświetlona strona.</span><span class="sxs-lookup"><span data-stu-id="3c82c-202">The *ProductList.aspx* page is displayed.</span></span>

3. <span data-ttu-id="3c82c-203">Wybierz **łodzi dokument** na liście produktów.</span><span class="sxs-lookup"><span data-stu-id="3c82c-203">Select **Paper Boat** from the product list.</span></span>
   <span data-ttu-id="3c82c-204">*ProductDetails.aspx* zostanie wyświetlona strona.</span><span class="sxs-lookup"><span data-stu-id="3c82c-204">The *ProductDetails.aspx* page is displayed.</span></span>

    ![Wyświetlanie danych elementów i szczegóły — produkty](display_data_items_and_details/_static/image4.png)
    
4. <span data-ttu-id="3c82c-206">Zamknij przeglądarkę.</span><span class="sxs-lookup"><span data-stu-id="3c82c-206">Close the browser.</span></span>


## <a name="additional-resources"></a><span data-ttu-id="3c82c-207">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="3c82c-207">Additional resources</span></span>

[<span data-ttu-id="3c82c-208">Pobieranie i wyświetlanie danych za pomocą wiązania modelu i formularzy sieci web</span><span class="sxs-lookup"><span data-stu-id="3c82c-208">Retrieving and displaying data with model binding and web forms</span></span>](../../presenting-and-managing-data/model-binding/retrieving-data.md)

## <a name="next-steps"></a><span data-ttu-id="3c82c-209">Następne kroki</span><span class="sxs-lookup"><span data-stu-id="3c82c-209">Next steps</span></span>

<span data-ttu-id="3c82c-210">W tym samouczku dodano znaczników i kodu do wyświetlania produktów oraz szczegóły produktów.</span><span class="sxs-lookup"><span data-stu-id="3c82c-210">In this tutorial, you added markup and code to display products and product details.</span></span> <span data-ttu-id="3c82c-211">Wiesz już o silnie typizowane kontrolki danych, tworzenia powiązania modelu i dostawców wartości.</span><span class="sxs-lookup"><span data-stu-id="3c82c-211">You learned about strongly typed data controls, model binding, and value providers.</span></span> <span data-ttu-id="3c82c-212">W następnym samouczku dodasz koszyka do przykładowej aplikacji Wingtip Toys.</span><span class="sxs-lookup"><span data-stu-id="3c82c-212">In the next tutorial, you'll add a shopping cart to the Wingtip Toys sample application.</span></span> 

> [!div class="step-by-step"]
> <span data-ttu-id="3c82c-213">[Poprzednie](ui_and_navigation.md)
> [dalej](shopping-cart.md)</span><span class="sxs-lookup"><span data-stu-id="3c82c-213">[Previous](ui_and_navigation.md)
[Next](shopping-cart.md)</span></span>
