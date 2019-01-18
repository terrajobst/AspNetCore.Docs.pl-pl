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
<a name="display-data-items-and-details"></a>Wyświetlanie elementów danych i szczegóły
====================
przez [Erik Reitan](https://github.com/Erikre)

> W tej serii samouczków dowiesz się, podstawy tworzenia aplikacji formularzy sieci Web ASP.NET za pomocą programu ASP.NET 4.7 i Microsoft Visual Studio 2017.

W tym samouczku dowiesz się, jak wyświetlać elementy danych i szczegóły elementu danych przy użyciu formularzy sieci Web ASP.NET i Entity Framework Code First. Ten samouczek opiera się na poprzednim samouczku "Interfejsu użytkownika i Nawigacja" jako części serii samouczków o nazwie Wingtip zabawki Store. Po ukończeniu tego samouczka, zobaczysz produktów na *ProductsList.aspx* strony i szczegóły na temat produktu *ProductDetails.aspx* strony.

## <a name="youll-learn-how-to"></a>Dowiesz się, jak:

- Dodaj formant do wyświetlania produktów z bazy danych
- Połącz kontrolki danych do wybranych danych
- Dodaj formant danych, aby wyświetlić szczegółowe informacje z bazy danych
- Pobieranie wartości z ciągu zapytania i użyć tej wartości, aby ograniczyć dane, które są pobierane z bazy danych

### <a name="features-introduced-in-this-tutorial"></a>Funkcje wprowadzone w tym samouczku:

- Wiązanie modelu
- Dostawców wartości

## <a name="add-a-data-control"></a>Dodawanie kontrolki danych

Jest kilka różnych opcji można użyć, aby powiązać dane z kontrolką serwera. Najbardziej typowe obejmują:

 * Dodawanie kontroli źródła danych
 * Ręczne dodawanie kodu
 * Przy użyciu wiązania modelu

### <a name="use-a-data-source-control-to-bind-data"></a>Użyj kontroli źródła danych, aby powiązać dane

Dodawanie kontroli źródła danych umożliwia połączenie kontroli źródła danych do formantu, który wyświetla dane. W przypadku tej metody użytkownik może w sposób deklaratywny, a nie programowo, łączenie kontrolek po stronie serwera ze źródłami danych.

### <a name="code-by-hand-to-bind-data"></a>Kod ręcznie, aby powiązać dane

Kodowanie ręcznie obejmuje:

1. Odczytywanie wartości
2. Sprawdzanie, jeśli ma wartość null
3. Podczas konwertowania go do odpowiedniego typu
4. Sprawdzanie, czy Powodzenie konwersji
5. Przy użyciu wartości w zapytaniu 

Takie podejście umożliwia pełną kontrolę nad logika dostępu do danych.

### <a name="use-model-binding-to-bind-data"></a>Użyj wiązania modelu do powiązania danych

Wiązanie modelu pozwala powiązać wyniki z znacznie mniejszej ilości kodu i daje możliwość ponownego użycia funkcji w całej aplikacji. Jego upraszcza pracę z logiki skoncentrowane na kodzie dostępu do danych przy jednoczesnym dalszym zapewnianiu framework rozbudowane, powiązanie danych.

## <a name="display-products"></a>Wyświetl produkty

W tym samouczku użyjesz wiązania modelu do powiązania danych. Aby skonfigurować kontrolkę danych na potrzeby tworzenia powiązania modelu wybierz dane, należy ustawić formantu `SelectMethod` właściwość na nazwę metody w kodzie strony. Kontrolki danych wywołuje metodę w odpowiednim czasie w cyklu życia strony i automatycznie wiąże zwracanych danych. Nie trzeba jawnie wywołać `DataBind` metody.

1. W **Eksploratora rozwiązań**, otwórz *ProductList.aspx*.
2. Zastąp istniejący kod znaczników ten kod znaczników:   

    [!code-aspx-csharp[Main](display_data_items_and_details/samples/sample1.aspx)]

Ten kod używa **ListView** formantu o nazwie `productList` do wyświetlania produktów.

[!code-aspx-csharp[Main](display_data_items_and_details/samples/sample2.aspx)]

Za pomocą szablonów i stylów, możesz zdefiniować sposób, w jaki **ListView** kontrolka wyświetla dane. Jest to przydatne dla danych w każdej powtarzające się struktury. Chociaż to **ListView** przykład po prostu wyświetla dane z bazy danych, można także bez konieczności pisania kodu i umożliwianie użytkownikom edytowanie, wstawianie i usuwanie danych i aby i sortowanie danych strony.

Ustawiając `ItemType` właściwości w **ListView** kontrolować wyrażenia wiązania danych `Item` jest dostępny i kontrolki staje się silnie typizowane. Jak wspomniano w poprzednim samouczku, możesz wybrać element Szczegóły obiektu za pomocą funkcji IntelliSense, takich jak określanie `ProductName`:

![Wyświetlanie danych elementów i szczegóły — funkcja IntelliSense](display_data_items_and_details/_static/image1.png)

Jest również przy użyciu wiązania modelu, aby określić `SelectMethod` wartość. Ta wartość (`GetProducts`) odnosi się do metody, należy dodać do kodu opóźnienie do wyświetlania produktów w następnym kroku.

### <a name="add-code-to-display-products"></a>Dodaj kod do wyświetlania produktów

W tym kroku dodasz kod, aby wypełnić **ListView** kontrolki z danymi produktów z bazy danych. Kod obsługuje wyświetlanie wszystkich produktów i produktów poszczególnych kategorii.

1. W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy *ProductList.aspx* , a następnie wybierz **Wyświetl kod**.
2. Zastąp istniejący kod w *ProductList.aspx.cs* pliku, w tym:   

    [!code-csharp[Main](display_data_items_and_details/samples/sample3.cs)]

Ten kod zawiera `GetProducts` metody, **ListView** kontrolki `ItemType` odwołuje się do właściwości w *ProductList.aspx* strony. Aby ograniczyć wyniki do kategorii konkretnej bazy danych, ustawia kod `categoryId` wartość wartość ciągu zapytania, które są przekazywane do *ProductList.aspx* gdy *ProductList.aspx* strona to przejście. `QueryStringAttribute` Klasy w `System.Web.ModelBinding` przestrzeń nazw jest używana do pobrania wartości zmiennej ciągu zapytania `id`. To powoduje, że wiązanie modelu do wypróbowania można powiązać wartości z ciągu zapytania do `categoryId` parametru w czasie wykonywania.

Jeśli prawidłową kategorię jest przekazywany jako ciąg zapytania do strony, wyniki zapytania są ograniczone do tych produktów w bazie danych, które odpowiadają `categoryId` wartość. Na przykład jeśli *ProductsList.aspx* to jest adres URL strony:


[!code-console[Main](display_data_items_and_details/samples/sample4.cmd)]

Na stronie są wyświetlane tylko produkty gdzie `categoryId` jest równa `1`.

Wszystkie produkty są wyświetlane, jeśli nie ciągu zapytania jest dołączony, kiedy *ProductList.aspx* nosi nazwę strony.

Źródła wartości dla tych metod są określane jako *wartość dostawców* (takie jak *QueryString*), i są określane atrybuty parametrów, wskazujące, której dostawca wartości do użycia jako *wartości atrybutów dostawcy* (takie jak `id`). Program ASP.NET zawiera dostawców wartości i odpowiednie atrybuty wszystkie typowe źródła danych wejściowych użytkownika w aplikacji formularzy sieci Web, takich jak ciąg zapytania, plików cookie, wartości formularza, formanty, stan widoku, stan sesji i właściwości profilu. Można także napisać dostawców wartości niestandardowych.

### <a name="run-the-application"></a>Uruchamianie aplikacji

Uruchom aplikację teraz wyświetlić wszystkie produkty lub produkty z kategorii.

1. Naciśnij klawisz **F5** podczas gdy w programie Visual Studio, aby uruchomić aplikację.  
   Przeglądarki otwiera się i pokazuje *Default.aspx* strony.

2. Wybierz **samochodów** menu nawigacji Kategoria produktu.  
   *ProductList.aspx* stronie wyświetlane tylko **samochodów** kategorii produktów. W dalszej części tego samouczka wyświetlisz informacje szczegółowe.  

    ![Wyświetlanie danych elementów i szczegóły — samochodów](display_data_items_and_details/_static/image2.png)

3. Wybierz **produktów** z menu nawigacji u góry.  
   Ponownie *ProductList.aspx* zostanie wyświetlona strona, jednak tym razem pokazuje całą listę produktów.   

    ![Wyświetlanie danych elementów i szczegóły — produkty](display_data_items_and_details/_static/image3.png)

4. Zamknij przeglądarkę i wróć do programu Visual Studio.

### <a name="add-a-data-control-to-display-product-details"></a>Dodaj formant danych, aby wyświetlić szczegółowe informacje o produkcie

Następnie zmodyfikujesz kod znaczników w *ProductDetails.aspx* strony, które dodano w poprzednim samouczku, aby wyświetlić informacje o określonym produktem.

1. W **Eksploratora rozwiązań**, otwórz *ProductDetails.aspx*.

2. Zastąp istniejący kod znaczników ten kod znaczników:

    [!code-aspx-csharp[Main](display_data_items_and_details/samples/sample5.aspx)] 

    Ten kod używa **FormView** formantu, aby wyświetlić szczegóły określonego produktu. Ten kod znaczników używa metod, takich jak metody używane do wyświetlania danych w *ProductList.aspx* strony. **FormView** formant jest używany do wyświetlania jeden rekord jednocześnie ze źródła danych. Kiedy używasz **FormView** kontrolki, tworzenie szablonów, aby wyświetlić i edytować wartości powiązanych z danymi. Te szablony zawierają kontrolek, powiązań wyrażenia i formatowanie zdefiniować wygląd formularza i nowe funkcje.

Połączenie poprzedniego znaczników do bazy danych wymaga dodatkowego kodu.

1. W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy *ProductDetails.aspx* a następnie kliknij przycisk **Wyświetl kod**.  
   *ProductDetails.aspx.cs* pliku jest wyświetlany.

2. Zastąp istniejący kod przy użyciu tego kodu:   

    [!code-csharp[Main](display_data_items_and_details/samples/sample6.cs)]

Ten kod sprawdza, czy "`productID`" wartość ciągu zapytania. Jeśli wartość nieprawidłowy ciąg zapytania zostanie znaleziony, zostanie wyświetlony zgodnego produktu. Jeśli ciąg zapytania nie zostanie odnaleziony lub jego wartość nie jest prawidłowa, jest wyświetlany żaden produkt.

### <a name="run-the-application"></a>Uruchamianie aplikacji

Teraz można uruchomić aplikacji, aby zobaczyć indywidualnych produktów, wyświetlane oparte na identyfikator produktu.

1. Naciśnij klawisz **F5** podczas gdy w programie Visual Studio, aby uruchomić aplikację.  
   Przeglądarki otwiera się i pokazuje *Default.aspx* strony.

2. Wybierz **łodzi** z menu nawigacji kategorii.  
   *ProductList.aspx* zostanie wyświetlona strona.

3. Wybierz **łodzi dokument** na liście produktów.
   *ProductDetails.aspx* zostanie wyświetlona strona.

    ![Wyświetlanie danych elementów i szczegóły — produkty](display_data_items_and_details/_static/image4.png)
    
4. Zamknij przeglądarkę.


## <a name="additional-resources"></a>Dodatkowe zasoby

[Pobieranie i wyświetlanie danych za pomocą wiązania modelu i formularzy sieci web](../../presenting-and-managing-data/model-binding/retrieving-data.md)

## <a name="next-steps"></a>Następne kroki

W tym samouczku dodano znaczników i kodu do wyświetlania produktów oraz szczegóły produktów. Wiesz już o silnie typizowane kontrolki danych, tworzenia powiązania modelu i dostawców wartości. W następnym samouczku dodasz koszyka do przykładowej aplikacji Wingtip Toys. 

> [!div class="step-by-step"]
> [Poprzednie](ui_and_navigation.md)
> [dalej](shopping-cart.md)
