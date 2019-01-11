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
<a name="display-data-items-and-details"></a>Wyświetlanie elementów danych i szczegóły
====================
przez [Erik Reitan](https://github.com/Erikre)

> W tej serii samouczków dowiesz się, podstawy tworzenia aplikacji formularzy sieci Web ASP.NET za pomocą programu ASP.NET 4.7 i Microsoft Visual Studio Community 2017 dla sieci Web.

W tym samouczku dowiesz się, jak wyświetlać elementy danych i szczegóły elementu danych przy użyciu formularzy sieci Web ASP.NET i Entity Framework Code First. Ten samouczek opiera się na poprzednim samouczku "Interfejsu użytkownika i Nawigacja" jako części serii samouczków o nazwie Wingtip zabawki Store. W tym samouczku ukończone produkty na *ProductsList.aspx* strony i szczegóły na temat produktu *ProductDetails.aspx* strony są wyświetlane.

## <a name="what-you-learn"></a>Zdobytą wiedzę

- Dodaj formant danych do wyświetlania produktów bazy danych.
- Utworzenie połączenia danych formantu do wybranych danych.
- Dodaj formant danych, aby wyświetlić szczegółowe informacje o produkcie.
- Przeanalizować wartości ciągu zapytania i używać go do filtrowania danych pobrane bazy danych.

Funkcje wprowadzone w tym samouczku obejmują wiązania modelu i dostawców wartości.

## <a name="add-a-data-control-to-display-products"></a>Dodawanie kontrolki danych do wyświetlania produktów
 
Masz kilka opcji, aby powiązać dane z kontrolką serwera. Najbardziej typowe obejmują:

 * Dodawanie kontroli źródła danych
 * Ręczne dodawanie kodu
 * Implementowanie wiązanie modelu

### <a name="use-a-data-source-control-to-bind-data"></a>Użyj kontroli źródła danych, aby powiązać dane

Dodawanie kontroli źródła danych łączy do kontroli źródła danych do formantu, który wyświetla dane. W przypadku tej metody użytkownik może w sposób deklaratywny, a nie programowo, łączenie kontrolek po stronie serwera ze źródłami danych.

### <a name="code-by-hand-to-bind-data"></a>Kod ręcznie, aby powiązać dane

Kodowanie ręcznie obejmuje:

1. Odczytywanie wartości
2. Sprawdzanie, jeśli ma wartość null
3. Podczas konwertowania go do odpowiedniego typu
4. Sprawdzanie, czy Powodzenie konwersji
5. Tworzenie zapytania o przekonwertowane wartości 

W przypadku tej metody masz pełną kontrolę nad logika dostępu do danych.

### <a name="use-model-binding-to-bind-data"></a>Użyj wiązania modelu do powiązania danych

Za pomocą wiązania modelu powiązać wyniki z znacznie mniejszej ilości kodu i daje możliwość ponownego użycia funkcji w całej aplikacji. Jego upraszcza pracę z logiki skoncentrowane na kodzie dostępu do danych przy jednoczesnym dalszym zapewnianiu framework rozbudowane, powiązanie danych.

## <a name="display-products"></a>Wyświetl produkty

W tym samouczku użyjesz wiązania modelu do powiązania danych. Aby skonfigurować kontrolkę danych na potrzeby tworzenia powiązania modelu wybierz dane, należy ustawić formantu `SelectMethod` właściwości do metody w kodzie strony. Kontrolki danych wywołuje metodę w odpowiednim czasie w cyklu życia strony i automatycznie wiąże zwracanych danych. Nie trzeba jawnie wywołać `DataBind` metody.

Działa przez następujące kroki, możesz zmodyfikować *ProductList.aspx* znaczników w celu wyświetlania produktów.

1. W **Eksploratora rozwiązań**, otwórz *ProductList.aspx*.

2. Zastąp istniejący kod znaczników następującym kodem: 

    [!code-aspx-csharp[Main](display_data_items_and_details/samples/sample1.aspx)]

Korzysta z poprzednim znaczników **ListView** formantu o nazwie `productList` do wyświetlania produktów.

[!code-aspx-csharp[Main](display_data_items_and_details/samples/sample2.aspx)]

Za pomocą szablonów i stylów, możesz zdefiniować sposób, w jaki **ListView** kontrolka wyświetla dane. Jest to przydatne dla danych w każdej powtarzające się struktury. Chociaż to **ListView** przykład po prostu wyświetla dane z bazy danych, można także bez konieczności pisania kodu i umożliwianie użytkownikom edytowanie, wstawianie i usuwanie danych i aby i sortowanie danych strony.

Po ustawieniu `ItemType` właściwość **ListView** kontrolować wyrażenia wiązania danych `Item` jest dostępny i kontrolki staje się silnie typizowane. Jak wspomniano w poprzednim samouczku, możesz wybrać element Szczegóły obiektu za pomocą funkcji IntelliSense, takich jak określanie `ProductName`:

![Wyświetlanie danych elementów i szczegóły — funkcja IntelliSense](display_data_items_and_details/_static/image1.png)

Za pomocą wiązania modelu określasz `SelectMethod` wartość (`GetProducts`). Jest to metoda dodawania do kodu opóźnienie do wyświetlania produktów w następnym kroku.

### <a name="add-code-to-display-products"></a>Dodaj kod do wyświetlania produktów

W tym kroku dodasz kod, aby wypełnić **ListView** kontroli danych z bazy danych produktu. Kod obsługuje wyświetlanie wszystkich produktów i produktów poszczególnych kategorii.

1. W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy *ProductList.aspx* , a następnie wybierz **Wyświetl kod**.
2. Zastąp istniejący kod w *ProductList.aspx.cs* pliku, w tym:   

    [!code-csharp[Main](display_data_items_and_details/samples/sample3.cs)]

Ten kod zawiera `GetProducts` metody, **ListView** kontrolki `ItemType` odwołuje się do właściwości w *ProductList.aspx*. Aby ograniczyć wyniki do kategorii konkretnej bazy danych, ustawia kod `categoryId` wartości z ciągu zapytania przekazany do *ProductList.aspx*. `QueryStringAttribute` Klasy w `System.Web.ModelBinding` przestrzeń nazw jest używana do pobierania zmiennej ciągu zapytania `id`wartości. To powoduje, że model, w czasie wykonywania, jest powiązane powiązanie wartość ciągu zapytania do `categoryId` parametru.

Gdy prawidłową kategorię (`categoryId`) jest przekazywany, wyniki są ograniczone do tej kategorii produktów, bazy danych. Na przykład jeśli *ProductsList.aspx* to jest adres URL strony:

[!code-console[Main](display_data_items_and_details/samples/sample4.cmd)]

Na stronie są wyświetlane tylko produkty gdzie `categoryId` jest równa `1`.

Wszystkie produkty są wyświetlane, jeśli nie ciągu zapytania jest przekazywany.

Źródła wartości te metody są wywoływane *wartość dostawców* (takie jak `QueryString`), a atrybuty parametrów, wskazujące, której dostawca wartości do użycia, są nazywane *wartości atrybutów dostawcy* ( takie jak `id`). Program ASP.NET zawiera dostawców wartości i atrybuty dla wszystkich typowych formularzy sieci Web aplikacji użytkownika źródeł danych wejściowych. Obejmują one ciągu zapytania, plików cookie, wartości formularza, formanty, stan widoku, stan sesji i właściwości profilu. Można także napisać dostawców wartości niestandardowych.

### <a name="run-the-application"></a>Uruchamianie aplikacji

Uruchom aplikację teraz wyświetlić wszystkie produkty lub produkty z kategorii.

1. W programie Visual Studio, naciśnij klawisz **F5** do uruchomienia aplikacji.
 Przeglądarki otwiera się i pokazuje *Default.aspx* strony.

2. Wybierz z menu Kategoria produktu **samochodów**.

   *ProductList.aspx* strony zostanie wyświetlony tylko produkty z **samochodów** kategorii. W dalszej części tego samouczka możesz wyświetlić szczegółowe informacje o produkcie.

    ![Wyświetlanie danych elementów i szczegóły — samochodów](display_data_items_and_details/_static/image2.png)

3. Wybierz **produktów** z górnego menu.
 *ProductList.aspx* strony wyświetli teraz wszystkie produkty. 

    ![Wyświetlanie danych elementów i szczegóły — produkty](display_data_items_and_details/_static/image3.png)

4. Zamknij przeglądarkę i wróć do programu Visual Studio.

### <a name="add-a-data-control-to-display-product-details"></a>Dodaj formant danych, aby wyświetlić szczegółowe informacje o produkcie

Modyfikowanie *ProductDetails.aspx* znaczników, które dodano w poprzednim samouczku, aby wyświetlić informacje o określonym produktem:

1. W **Eksploratora rozwiązań**, otwórz *ProductDetails.aspx*.

2. Zastąp istniejący kod znaczników ten kod znaczników:

    [!code-aspx-csharp[Main](display_data_items_and_details/samples/sample5.aspx)] 

Ten kod znaczników używa **FormView** formantu, aby wyświetlić szczegóły określonego produktu. Używa metod, takich jak te używane do wyświetlania danych w *ProductList.aspx*. **FormView** formant jest używany do wyświetlania jeden rekord jednocześnie ze źródła danych. Kiedy używasz **FormView** kontrolki, tworzenie szablonów, aby wyświetlić i edytować wartości powiązanych z danymi. Te szablony zawierają kontrolek, powiązań wyrażenia i formatowanie zdefiniować wygląd formularza i nowe funkcje.

Połączenie poprzedniego znaczników do bazy danych wymaga dodatkowego kodu.

1. W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy *ProductDetails.aspx* , a następnie wybierz **Wyświetl kod**.  
   *ProductDetails.aspx.cs* pliku jest wyświetlany.

2. Zastąp istniejący kod w tym:   

    [!code-csharp[Main](display_data_items_and_details/samples/sample6.cs)]

Ten kod sprawdza, czy "`productID`" wartość ciągu zapytania. Jeśli nieprawidłowa wartość zostanie znaleziona, zostanie wyświetlony zgodnego produktu. Jeśli ciąg zapytania nie zostanie odnaleziony lub jego wartość nie jest prawidłowa, jest wyświetlany żaden produkt.

### <a name="run-the-application"></a>Uruchamianie aplikacji

Teraz możesz uruchomić aplikację, aby wyświetlić szczegóły konkretnego produktu na podstawie identyfikatora produktu.

1. W programie Visual Studio, naciśnij klawisz **F5** do uruchomienia aplikacji.  
 Zostanie otwarta przeglądarka *Default.aspx*.

2. Wybierz z menu kategorii **łodzi**.  
 *ProductList.aspx* zostanie wyświetlona strona.

3. Wybierz **papier łodzi**.  
 *ProductDetails.aspx* zostanie wyświetlona strona.   

    ![Wyświetlanie danych elementów i szczegóły — produkty](display_data_items_and_details/_static/image4.png)

W następnym samouczku dodasz koszyka do aplikacji Wingtip Toys.

## <a name="additional-resources"></a>Dodatkowe zasoby

[Pobieranie i wyświetlanie danych za pomocą wiązania modelu i formularzy sieci web](../../presenting-and-managing-data/model-binding/retrieving-data.md)

> [!div class="step-by-step"]
> [Poprzednie](ui_and_navigation.md)
> [dalej](shopping-cart.md)
