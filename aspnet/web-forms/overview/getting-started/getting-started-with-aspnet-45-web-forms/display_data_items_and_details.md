---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/display_data_items_and_details
title: Wyświetlanie elementów danych i szczegółowych informacji | Dokumentacja firmy Microsoft
author: Erikre
description: W tej serii samouczków obejmuje podstawy tworzenia aplikacji formularzy sieci Web ASP.NET za pomocą programu ASP.NET 4.7 i Microsoft Visual Studio Community 2017 dla sieci Web
ms.author: riande
ms.date: 1/09/2019
ms.assetid: 64a491a8-0ed6-4c2f-9c1c-412962eb6006
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/display_data_items_and_details
msc.type: authoredcontent
ms.openlocfilehash: 73ae1660f5d6e3e28c1c155e745a62936e3502df
ms.sourcegitcommit: cec77d5ad8a0cedb1ecbec32834111492afd0cd2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/10/2019
ms.locfileid: "54207437"
---
<a name="display-data-items-and-details"></a><span data-ttu-id="137e2-103">Wyświetlanie elementów danych i szczegóły</span><span class="sxs-lookup"><span data-stu-id="137e2-103">Display data items and details</span></span>
====================
<span data-ttu-id="137e2-104">przez [Erik Reitan](https://github.com/Erikre)</span><span class="sxs-lookup"><span data-stu-id="137e2-104">by [Erik Reitan](https://github.com/Erikre)</span></span>

> <span data-ttu-id="137e2-105">W tej serii samouczków dowiesz się, podstawy tworzenia aplikacji formularzy sieci Web ASP.NET za pomocą programu ASP.NET 4.7 i Microsoft Visual Studio Community 2017 dla sieci Web.</span><span class="sxs-lookup"><span data-stu-id="137e2-105">This tutorial series teaches you the basics of building an ASP.NET Web Forms application with ASP.NET 4.7 and Microsoft Visual Studio Community 2017 for the Web.</span></span>

<span data-ttu-id="137e2-106">W tym samouczku dowiesz się, jak wyświetlać elementy danych i szczegóły elementu danych przy użyciu formularzy sieci Web ASP.NET i Entity Framework Code First.</span><span class="sxs-lookup"><span data-stu-id="137e2-106">In this tutorial, you learn how to display data items and data item details with ASP.NET Web Forms and Entity Framework Code First.</span></span> <span data-ttu-id="137e2-107">Ten samouczek opiera się na poprzednim samouczku "Interfejsu użytkownika i Nawigacja" jako części serii samouczków o nazwie Wingtip zabawki Store.</span><span class="sxs-lookup"><span data-stu-id="137e2-107">This tutorial builds on the previous "UI and Navigation" tutorial as part of the Wingtip Toy Store tutorial series.</span></span> <span data-ttu-id="137e2-108">W tym samouczku ukończone produkty na *ProductsList.aspx* strony i szczegóły na temat produktu *ProductDetails.aspx* strony są wyświetlane.</span><span class="sxs-lookup"><span data-stu-id="137e2-108">In the completed tutorial, products on the *ProductsList.aspx* page and a product's details on the *ProductDetails.aspx* page are displayed.</span></span>

## <a name="what-you-learn"></a><span data-ttu-id="137e2-109">Zdobytą wiedzę</span><span class="sxs-lookup"><span data-stu-id="137e2-109">What you learn</span></span>

- <span data-ttu-id="137e2-110">Dodaj formant danych do wyświetlania produktów bazy danych.</span><span class="sxs-lookup"><span data-stu-id="137e2-110">Add a data control to display database products.</span></span>
- <span data-ttu-id="137e2-111">Utworzenie połączenia danych formantu do wybranych danych.</span><span class="sxs-lookup"><span data-stu-id="137e2-111">Connect a data control to selected data.</span></span>
- <span data-ttu-id="137e2-112">Dodaj formant danych, aby wyświetlić szczegółowe informacje o produkcie.</span><span class="sxs-lookup"><span data-stu-id="137e2-112">Add a data control to display product details.</span></span>
- <span data-ttu-id="137e2-113">Przeanalizować wartości ciągu zapytania i używać go do filtrowania danych pobrane bazy danych.</span><span class="sxs-lookup"><span data-stu-id="137e2-113">Parse a query string value and use it to filter retrieved database data.</span></span>

<span data-ttu-id="137e2-114">Funkcje wprowadzone w tym samouczku obejmują wiązania modelu i dostawców wartości.</span><span class="sxs-lookup"><span data-stu-id="137e2-114">Features introduced in this tutorial include model binding and value providers.</span></span>

## <a name="add-a-data-control-to-display-products"></a><span data-ttu-id="137e2-115">Dodawanie kontrolki danych do wyświetlania produktów</span><span class="sxs-lookup"><span data-stu-id="137e2-115">Add a data control to display products</span></span>
 
<span data-ttu-id="137e2-116">Masz kilka opcji, aby powiązać dane z kontrolką serwera.</span><span class="sxs-lookup"><span data-stu-id="137e2-116">You have a few options to bind data to a server control.</span></span> <span data-ttu-id="137e2-117">Najbardziej typowe obejmują:</span><span class="sxs-lookup"><span data-stu-id="137e2-117">The most common include:</span></span>

 * <span data-ttu-id="137e2-118">Dodawanie kontroli źródła danych</span><span class="sxs-lookup"><span data-stu-id="137e2-118">Adding a data source control</span></span>
 * <span data-ttu-id="137e2-119">Ręczne dodawanie kodu</span><span class="sxs-lookup"><span data-stu-id="137e2-119">Adding code by hand</span></span>
 * <span data-ttu-id="137e2-120">Implementowanie wiązanie modelu</span><span class="sxs-lookup"><span data-stu-id="137e2-120">Implementing model binding</span></span>

### <a name="use-a-data-source-control-to-bind-data"></a><span data-ttu-id="137e2-121">Użyj kontroli źródła danych, aby powiązać dane</span><span class="sxs-lookup"><span data-stu-id="137e2-121">Use a data source control to bind data</span></span>

<span data-ttu-id="137e2-122">Dodawanie kontroli źródła danych łączy do kontroli źródła danych do formantu, który wyświetla dane.</span><span class="sxs-lookup"><span data-stu-id="137e2-122">Adding a data source control links the data source control to the control that displays the data.</span></span> <span data-ttu-id="137e2-123">W przypadku tej metody użytkownik może w sposób deklaratywny, a nie programowo, łączenie kontrolek po stronie serwera ze źródłami danych.</span><span class="sxs-lookup"><span data-stu-id="137e2-123">With this approach, you can declaratively, rather than programmatically, connect server-side controls to data sources.</span></span>

### <a name="code-by-hand-to-bind-data"></a><span data-ttu-id="137e2-124">Kod ręcznie, aby powiązać dane</span><span class="sxs-lookup"><span data-stu-id="137e2-124">Code by hand to bind data</span></span>

<span data-ttu-id="137e2-125">Kodowanie ręcznie obejmuje:</span><span class="sxs-lookup"><span data-stu-id="137e2-125">Coding by hand involves:</span></span>

1. <span data-ttu-id="137e2-126">Odczytywanie wartości</span><span class="sxs-lookup"><span data-stu-id="137e2-126">Reading a value</span></span>
2. <span data-ttu-id="137e2-127">Sprawdzanie, jeśli ma wartość null</span><span class="sxs-lookup"><span data-stu-id="137e2-127">Checking if it's null</span></span>
3. <span data-ttu-id="137e2-128">Podczas konwertowania go do odpowiedniego typu</span><span class="sxs-lookup"><span data-stu-id="137e2-128">Converting it to an appropriate type</span></span>
4. <span data-ttu-id="137e2-129">Sprawdzanie, czy Powodzenie konwersji</span><span class="sxs-lookup"><span data-stu-id="137e2-129">Checking conversion success</span></span>
5. <span data-ttu-id="137e2-130">Tworzenie zapytania o przekonwertowane wartości</span><span class="sxs-lookup"><span data-stu-id="137e2-130">Making a query with the converted value</span></span> 

<span data-ttu-id="137e2-131">W przypadku tej metody masz pełną kontrolę nad logika dostępu do danych.</span><span class="sxs-lookup"><span data-stu-id="137e2-131">With this approach, you have full control over your data-access logic.</span></span>

### <a name="use-model-binding-to-bind-data"></a><span data-ttu-id="137e2-132">Użyj wiązania modelu do powiązania danych</span><span class="sxs-lookup"><span data-stu-id="137e2-132">Use model binding to bind data</span></span>

<span data-ttu-id="137e2-133">Za pomocą wiązania modelu powiązać wyniki z znacznie mniejszej ilości kodu i daje możliwość ponownego użycia funkcji w całej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="137e2-133">With model binding, you bind results with far less code and it gives you the ability to reuse the functionality throughout your application.</span></span> <span data-ttu-id="137e2-134">Jego upraszcza pracę z logiki skoncentrowane na kodzie dostępu do danych przy jednoczesnym dalszym zapewnianiu framework rozbudowane, powiązanie danych.</span><span class="sxs-lookup"><span data-stu-id="137e2-134">It simplifies working with code-focused data-access logic while still providing a rich, data-binding framework.</span></span>

## <a name="display-products"></a><span data-ttu-id="137e2-135">Wyświetl produkty</span><span class="sxs-lookup"><span data-stu-id="137e2-135">Display products</span></span>

<span data-ttu-id="137e2-136">W tym samouczku użyjesz wiązania modelu do powiązania danych.</span><span class="sxs-lookup"><span data-stu-id="137e2-136">In this tutorial, you use model binding to bind data.</span></span> <span data-ttu-id="137e2-137">Aby skonfigurować kontrolkę danych na potrzeby tworzenia powiązania modelu wybierz dane, należy ustawić formantu `SelectMethod` właściwości do metody w kodzie strony.</span><span class="sxs-lookup"><span data-stu-id="137e2-137">To configure a data control to use model binding to select data, you set the control's `SelectMethod` property to a method in the page's code.</span></span> <span data-ttu-id="137e2-138">Kontrolki danych wywołuje metodę w odpowiednim czasie w cyklu życia strony i automatycznie wiąże zwracanych danych.</span><span class="sxs-lookup"><span data-stu-id="137e2-138">The data control calls the method at the appropriate time in the page life cycle and automatically binds the returned data.</span></span> <span data-ttu-id="137e2-139">Nie trzeba jawnie wywołać `DataBind` metody.</span><span class="sxs-lookup"><span data-stu-id="137e2-139">There's no need to explicitly call the `DataBind` method.</span></span>

<span data-ttu-id="137e2-140">Działa przez następujące kroki, możesz zmodyfikować *ProductList.aspx* znaczników w celu wyświetlania produktów.</span><span class="sxs-lookup"><span data-stu-id="137e2-140">Working through the following steps, you modify *ProductList.aspx* markup to display products.</span></span>

1. <span data-ttu-id="137e2-141">W **Eksploratora rozwiązań**, otwórz *ProductList.aspx*.</span><span class="sxs-lookup"><span data-stu-id="137e2-141">In **Solution Explorer**, open *ProductList.aspx*.</span></span>

2. <span data-ttu-id="137e2-142">Zastąp istniejący kod znaczników następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="137e2-142">Replace the existing markup with the following markup:</span></span> 

    [!code-aspx-csharp[Main](display_data_items_and_details/samples/sample1.aspx)]

<span data-ttu-id="137e2-143">Korzysta z poprzednim znaczników **ListView** formantu o nazwie `productList` do wyświetlania produktów.</span><span class="sxs-lookup"><span data-stu-id="137e2-143">The preceding markup uses a **ListView** control named `productList` to display products.</span></span>

[!code-aspx-csharp[Main](display_data_items_and_details/samples/sample2.aspx)]

<span data-ttu-id="137e2-144">Za pomocą szablonów i stylów, możesz zdefiniować sposób, w jaki **ListView** kontrolka wyświetla dane.</span><span class="sxs-lookup"><span data-stu-id="137e2-144">With templates and styles, you define how the **ListView** control displays data.</span></span> <span data-ttu-id="137e2-145">Jest to przydatne dla danych w każdej powtarzające się struktury.</span><span class="sxs-lookup"><span data-stu-id="137e2-145">It's useful for data in any repeating structure.</span></span> <span data-ttu-id="137e2-146">Chociaż to **ListView** przykład po prostu wyświetla dane z bazy danych, można także bez konieczności pisania kodu i umożliwianie użytkownikom edytowanie, wstawianie i usuwanie danych i aby i sortowanie danych strony.</span><span class="sxs-lookup"><span data-stu-id="137e2-146">Though this **ListView** example simply displays database data, you can also, without code, enable users to edit, insert, and delete data, and to sort and page data.</span></span>

<span data-ttu-id="137e2-147">Po ustawieniu `ItemType` właściwość **ListView** kontrolować wyrażenia wiązania danych `Item` jest dostępny i kontrolki staje się silnie typizowane.</span><span class="sxs-lookup"><span data-stu-id="137e2-147">When you set the `ItemType` property in the **ListView** control, the data-binding expression `Item` is available and the control becomes strongly typed.</span></span> <span data-ttu-id="137e2-148">Jak wspomniano w poprzednim samouczku, możesz wybrać element Szczegóły obiektu za pomocą funkcji IntelliSense, takich jak określanie `ProductName`:</span><span class="sxs-lookup"><span data-stu-id="137e2-148">As mentioned in the previous tutorial, you can select Item object details with IntelliSense, such as specifying the `ProductName`:</span></span>

![Wyświetlanie danych elementów i szczegóły — funkcja IntelliSense](display_data_items_and_details/_static/image1.png)

<span data-ttu-id="137e2-150">Za pomocą wiązania modelu określasz `SelectMethod` wartość (`GetProducts`).</span><span class="sxs-lookup"><span data-stu-id="137e2-150">With model binding, you're specifying a `SelectMethod` value (`GetProducts`).</span></span> <span data-ttu-id="137e2-151">Jest to metoda dodawania do kodu opóźnienie do wyświetlania produktów w następnym kroku.</span><span class="sxs-lookup"><span data-stu-id="137e2-151">This is the method you add to the code behind to display products in the next step.</span></span>

### <a name="add-code-to-display-products"></a><span data-ttu-id="137e2-152">Dodaj kod do wyświetlania produktów</span><span class="sxs-lookup"><span data-stu-id="137e2-152">Add code to display products</span></span>

<span data-ttu-id="137e2-153">W tym kroku dodasz kod, aby wypełnić **ListView** kontroli danych z bazy danych produktu.</span><span class="sxs-lookup"><span data-stu-id="137e2-153">In this step, you add code to populate the **ListView** control with database product data.</span></span> <span data-ttu-id="137e2-154">Kod obsługuje wyświetlanie wszystkich produktów i produktów poszczególnych kategorii.</span><span class="sxs-lookup"><span data-stu-id="137e2-154">The code supports showing all products and individual category products.</span></span>

1. <span data-ttu-id="137e2-155">W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy *ProductList.aspx* , a następnie wybierz **Wyświetl kod**.</span><span class="sxs-lookup"><span data-stu-id="137e2-155">In **Solution Explorer**, right-click *ProductList.aspx* and then select **View Code**.</span></span>
2. <span data-ttu-id="137e2-156">Zastąp istniejący kod w *ProductList.aspx.cs* pliku, w tym:</span><span class="sxs-lookup"><span data-stu-id="137e2-156">Replace the existing code in the *ProductList.aspx.cs* file with this:</span></span>   

    [!code-csharp[Main](display_data_items_and_details/samples/sample3.cs)]

<span data-ttu-id="137e2-157">Ten kod zawiera `GetProducts` metody, **ListView** kontrolki `ItemType` odwołuje się do właściwości w *ProductList.aspx*.</span><span class="sxs-lookup"><span data-stu-id="137e2-157">This code shows the `GetProducts` method that the **ListView** control's `ItemType` property references in *ProductList.aspx*.</span></span> <span data-ttu-id="137e2-158">Aby ograniczyć wyniki do kategorii konkretnej bazy danych, ustawia kod `categoryId` wartości z ciągu zapytania przekazany do *ProductList.aspx*.</span><span class="sxs-lookup"><span data-stu-id="137e2-158">To limit the results to a specific database category, the code sets the `categoryId` value from the query string passed to *ProductList.aspx*.</span></span> <span data-ttu-id="137e2-159">`QueryStringAttribute` Klasy w `System.Web.ModelBinding` przestrzeń nazw jest używana do pobierania zmiennej ciągu zapytania `id`wartości.</span><span class="sxs-lookup"><span data-stu-id="137e2-159">The `QueryStringAttribute` class in the `System.Web.ModelBinding` namespace is used to retrieve the query string variable `id`'s value.</span></span> <span data-ttu-id="137e2-160">To powoduje, że model, w czasie wykonywania, jest powiązane powiązanie wartość ciągu zapytania do `categoryId` parametru.</span><span class="sxs-lookup"><span data-stu-id="137e2-160">This instructs model binding to, at run time, bind a query string value to the `categoryId` parameter.</span></span>

<span data-ttu-id="137e2-161">Gdy prawidłową kategorię (`categoryId`) jest przekazywany, wyniki są ograniczone do tej kategorii produktów, bazy danych.</span><span class="sxs-lookup"><span data-stu-id="137e2-161">When a valid category (`categoryId`) is passed, the results are limited to that category's database products.</span></span> <span data-ttu-id="137e2-162">Na przykład jeśli *ProductsList.aspx* to jest adres URL strony:</span><span class="sxs-lookup"><span data-stu-id="137e2-162">For instance, if the *ProductsList.aspx* page URL is this:</span></span>

[!code-console[Main](display_data_items_and_details/samples/sample4.cmd)]

<span data-ttu-id="137e2-163">Na stronie są wyświetlane tylko produkty gdzie `categoryId` jest równa `1`.</span><span class="sxs-lookup"><span data-stu-id="137e2-163">The page displays only the products where the `categoryId` equals `1`.</span></span>

<span data-ttu-id="137e2-164">Wszystkie produkty są wyświetlane, jeśli nie ciągu zapytania jest przekazywany.</span><span class="sxs-lookup"><span data-stu-id="137e2-164">All products are displayed if no query string is passed.</span></span>

<span data-ttu-id="137e2-165">Źródła wartości te metody są wywoływane *wartość dostawców* (takie jak `QueryString`), a atrybuty parametrów, wskazujące, której dostawca wartości do użycia, są nazywane *wartości atrybutów dostawcy* ( takie jak `id`).</span><span class="sxs-lookup"><span data-stu-id="137e2-165">The value sources for these methods are called *value providers* (such as `QueryString`), and the parameter attributes that indicate which value provider to use are called *value provider attributes* (such as `id`).</span></span> <span data-ttu-id="137e2-166">Program ASP.NET zawiera dostawców wartości i atrybuty dla wszystkich typowych formularzy sieci Web aplikacji użytkownika źródeł danych wejściowych.</span><span class="sxs-lookup"><span data-stu-id="137e2-166">ASP.NET includes value providers and attributes for all typical Web Forms application user input sources.</span></span> <span data-ttu-id="137e2-167">Obejmują one ciągu zapytania, plików cookie, wartości formularza, formanty, stan widoku, stan sesji i właściwości profilu.</span><span class="sxs-lookup"><span data-stu-id="137e2-167">These include the query string, cookies, form values, controls, view state, session state, and profile properties.</span></span> <span data-ttu-id="137e2-168">Można także napisać dostawców wartości niestandardowych.</span><span class="sxs-lookup"><span data-stu-id="137e2-168">You can also write custom value providers.</span></span>

### <a name="run-the-application"></a><span data-ttu-id="137e2-169">Uruchamianie aplikacji</span><span class="sxs-lookup"><span data-stu-id="137e2-169">Run the application</span></span>

<span data-ttu-id="137e2-170">Uruchom aplikację teraz wyświetlić wszystkie produkty lub produkty z kategorii.</span><span class="sxs-lookup"><span data-stu-id="137e2-170">Run the application now to view all products or a category's products.</span></span>

1. <span data-ttu-id="137e2-171">W programie Visual Studio, naciśnij klawisz **F5** do uruchomienia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="137e2-171">In Visual Studio, press **F5** to run the application.</span></span>
 <span data-ttu-id="137e2-172">Przeglądarki otwiera się i pokazuje *Default.aspx* strony.</span><span class="sxs-lookup"><span data-stu-id="137e2-172">The browser opens and shows the *Default.aspx* page.</span></span>

2. <span data-ttu-id="137e2-173">Wybierz z menu Kategoria produktu **samochodów**.</span><span class="sxs-lookup"><span data-stu-id="137e2-173">From the product category menu, select **Cars**.</span></span>

   <span data-ttu-id="137e2-174">*ProductList.aspx* strony zostanie wyświetlony tylko produkty z **samochodów** kategorii.</span><span class="sxs-lookup"><span data-stu-id="137e2-174">The *ProductList.aspx* page appears, showing only products from the **Cars** category.</span></span> <span data-ttu-id="137e2-175">W dalszej części tego samouczka możesz wyświetlić szczegółowe informacje o produkcie.</span><span class="sxs-lookup"><span data-stu-id="137e2-175">Later in this tutorial, you display product details.</span></span>

    ![Wyświetlanie danych elementów i szczegóły — samochodów](display_data_items_and_details/_static/image2.png)

3. <span data-ttu-id="137e2-177">Wybierz **produktów** z górnego menu.</span><span class="sxs-lookup"><span data-stu-id="137e2-177">Select **Products** from the top menu.</span></span>
 <span data-ttu-id="137e2-178">*ProductList.aspx* strony wyświetli teraz wszystkie produkty.</span><span class="sxs-lookup"><span data-stu-id="137e2-178">The *ProductList.aspx* page now displays all products.</span></span> 

    ![Wyświetlanie danych elementów i szczegóły — produkty](display_data_items_and_details/_static/image3.png)

4. <span data-ttu-id="137e2-180">Zamknij przeglądarkę i wróć do programu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="137e2-180">Close the browser and return to Visual Studio.</span></span>

### <a name="add-a-data-control-to-display-product-details"></a><span data-ttu-id="137e2-181">Dodaj formant danych, aby wyświetlić szczegółowe informacje o produkcie</span><span class="sxs-lookup"><span data-stu-id="137e2-181">Add a Data Control to display product details</span></span>

<span data-ttu-id="137e2-182">Modyfikowanie *ProductDetails.aspx* znaczników, które dodano w poprzednim samouczku, aby wyświetlić informacje o określonym produktem:</span><span class="sxs-lookup"><span data-stu-id="137e2-182">Modify the *ProductDetails.aspx* markup that you added in the previous tutorial to display specific product information:</span></span>

1. <span data-ttu-id="137e2-183">W **Eksploratora rozwiązań**, otwórz *ProductDetails.aspx*.</span><span class="sxs-lookup"><span data-stu-id="137e2-183">In **Solution Explorer**, open *ProductDetails.aspx*.</span></span>

2. <span data-ttu-id="137e2-184">Zastąp istniejący kod znaczników ten kod znaczników:</span><span class="sxs-lookup"><span data-stu-id="137e2-184">Replace the existing markup with this markup:</span></span>

    [!code-aspx-csharp[Main](display_data_items_and_details/samples/sample5.aspx)] 

<span data-ttu-id="137e2-185">Ten kod znaczników używa **FormView** formantu, aby wyświetlić szczegóły określonego produktu.</span><span class="sxs-lookup"><span data-stu-id="137e2-185">This markup uses a **FormView** control to display specific product details.</span></span> <span data-ttu-id="137e2-186">Używa metod, takich jak te używane do wyświetlania danych w *ProductList.aspx*.</span><span class="sxs-lookup"><span data-stu-id="137e2-186">It uses methods like those used to display data in *ProductList.aspx*.</span></span> <span data-ttu-id="137e2-187">**FormView** formant jest używany do wyświetlania jeden rekord jednocześnie ze źródła danych.</span><span class="sxs-lookup"><span data-stu-id="137e2-187">The **FormView** control is used to display a single record at a time from a data source.</span></span> <span data-ttu-id="137e2-188">Kiedy używasz **FormView** kontrolki, tworzenie szablonów, aby wyświetlić i edytować wartości powiązanych z danymi.</span><span class="sxs-lookup"><span data-stu-id="137e2-188">When you use the **FormView** control, you create templates to display and edit data-bound values.</span></span> <span data-ttu-id="137e2-189">Te szablony zawierają kontrolek, powiązań wyrażenia i formatowanie zdefiniować wygląd formularza i nowe funkcje.</span><span class="sxs-lookup"><span data-stu-id="137e2-189">These templates contain controls, binding expressions, and formatting that define the form's look and functionality.</span></span>

<span data-ttu-id="137e2-190">Połączenie poprzedniego znaczników do bazy danych wymaga dodatkowego kodu.</span><span class="sxs-lookup"><span data-stu-id="137e2-190">Connecting the previous markup to the database requires additional code.</span></span>

1. <span data-ttu-id="137e2-191">W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy *ProductDetails.aspx* , a następnie wybierz **Wyświetl kod**.</span><span class="sxs-lookup"><span data-stu-id="137e2-191">In **Solution Explorer**, right-click *ProductDetails.aspx* and then select **View Code**.</span></span>  
   <span data-ttu-id="137e2-192">*ProductDetails.aspx.cs* pliku jest wyświetlany.</span><span class="sxs-lookup"><span data-stu-id="137e2-192">The *ProductDetails.aspx.cs* file is displayed.</span></span>

2. <span data-ttu-id="137e2-193">Zastąp istniejący kod w tym:</span><span class="sxs-lookup"><span data-stu-id="137e2-193">Replace the existing code with this:</span></span>   

    [!code-csharp[Main](display_data_items_and_details/samples/sample6.cs)]

<span data-ttu-id="137e2-194">Ten kod sprawdza, czy "`productID`" wartość ciągu zapytania.</span><span class="sxs-lookup"><span data-stu-id="137e2-194">This code checks for a "`productID`" query string value.</span></span> <span data-ttu-id="137e2-195">Jeśli nieprawidłowa wartość zostanie znaleziona, zostanie wyświetlony zgodnego produktu.</span><span class="sxs-lookup"><span data-stu-id="137e2-195">If a valid value is found, the matching product is displayed.</span></span> <span data-ttu-id="137e2-196">Jeśli ciąg zapytania nie zostanie odnaleziony lub jego wartość nie jest prawidłowa, jest wyświetlany żaden produkt.</span><span class="sxs-lookup"><span data-stu-id="137e2-196">If the query string isn't found, or its value isn't valid, no product is displayed.</span></span>

### <a name="run-the-application"></a><span data-ttu-id="137e2-197">Uruchamianie aplikacji</span><span class="sxs-lookup"><span data-stu-id="137e2-197">Run the application</span></span>

<span data-ttu-id="137e2-198">Teraz możesz uruchomić aplikację, aby wyświetlić szczegóły konkretnego produktu na podstawie identyfikatora produktu.</span><span class="sxs-lookup"><span data-stu-id="137e2-198">Now you can run the application to see specific product details based on product ID.</span></span>

1. <span data-ttu-id="137e2-199">W programie Visual Studio, naciśnij klawisz **F5** do uruchomienia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="137e2-199">In Visual Studio, press **F5** to run the application.</span></span>  
 <span data-ttu-id="137e2-200">Zostanie otwarta przeglądarka *Default.aspx*.</span><span class="sxs-lookup"><span data-stu-id="137e2-200">The browser opens to *Default.aspx*.</span></span>

2. <span data-ttu-id="137e2-201">Wybierz z menu kategorii **łodzi**.</span><span class="sxs-lookup"><span data-stu-id="137e2-201">From the category menu, select **Boats**.</span></span>  
 <span data-ttu-id="137e2-202">*ProductList.aspx* zostanie wyświetlona strona.</span><span class="sxs-lookup"><span data-stu-id="137e2-202">The *ProductList.aspx* page is displayed.</span></span>

3. <span data-ttu-id="137e2-203">Wybierz **papier łodzi**.</span><span class="sxs-lookup"><span data-stu-id="137e2-203">Select **Paper Boat**.</span></span>  
 <span data-ttu-id="137e2-204">*ProductDetails.aspx* zostanie wyświetlona strona.</span><span class="sxs-lookup"><span data-stu-id="137e2-204">The *ProductDetails.aspx* page is displayed.</span></span>   

    ![Wyświetlanie danych elementów i szczegóły — produkty](display_data_items_and_details/_static/image4.png)

<span data-ttu-id="137e2-206">W następnym samouczku dodasz koszyka do aplikacji Wingtip Toys.</span><span class="sxs-lookup"><span data-stu-id="137e2-206">In the next tutorial, you add a shopping cart to the Wingtip Toys application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="137e2-207">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="137e2-207">Additional resources</span></span>

[<span data-ttu-id="137e2-208">Pobieranie i wyświetlanie danych za pomocą wiązania modelu i formularzy sieci web</span><span class="sxs-lookup"><span data-stu-id="137e2-208">Retrieving and displaying data with model binding and web forms</span></span>](../../presenting-and-managing-data/model-binding/retrieving-data.md)

> [!div class="step-by-step"]
> <span data-ttu-id="137e2-209">[Poprzednie](ui_and_navigation.md)
> [dalej](shopping-cart.md)</span><span class="sxs-lookup"><span data-stu-id="137e2-209">[Previous](ui_and_navigation.md)
[Next](shopping-cart.md)</span></span>
