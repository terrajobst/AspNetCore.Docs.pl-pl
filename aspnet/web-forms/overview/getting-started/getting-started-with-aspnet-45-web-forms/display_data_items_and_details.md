---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/display_data_items_and_details
title: Wyświetlanie elementów danych i szczegółowych informacji | Dokumentacja firmy Microsoft
author: Erikre
description: Tej serii samouczków obejmuje podstawy tworzenia aplikacji formularzy sieci Web ASP.NET przy użyciu platformy ASP.NET 4.5 i programu Microsoft Visual Studio Express 2013 for firma Microsoft...
ms.author: aspnetcontent
ms.date: 09/08/2014
ms.assetid: 64a491a8-0ed6-4c2f-9c1c-412962eb6006
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/display_data_items_and_details
msc.type: authoredcontent
ms.openlocfilehash: 083f7182416012c85f05db255fcab4d8e535b52a
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/05/2018
ms.locfileid: "37820350"
---
<a name="display-data-items-and-details"></a>Wyświetlanie elementów danych i szczegółowych informacji
====================
przez [Erik Reitan](https://github.com/Erikre)

[Pobierz Wingtip Toys przykładowego projektu (C#)](http://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) lub [Pobierz książkę elektroniczną (PDF)](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)

> W tej serii samouczków obejmuje podstawy tworzenia aplikacji formularzy sieci Web ASP.NET przy użyciu platformy ASP.NET 4.5 i programu Microsoft Visual Studio Express 2013 for Web. Visual Studio 2013 [projektu za pomocą kodu źródłowego języka C#](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) jest dostępny dla tej serii samouczków towarzyszą.


W tym samouczku opisano sposób wyświetlania elementów danych i szczegóły elementu danych przy użyciu formularzy sieci Web ASP.NET i Entity Framework Code First. Ten samouczek opiera się na poprzednim samouczku "Interfejsu użytkownika i Nawigacja" i jest częścią serii samouczków o nazwie Wingtip zabawki Store. Po ukończeniu tego samouczka będziesz mieć możliwość Zobacz produkty na *ProductsList.aspx* strony i szczegółowe informacje o poszczególnych produktów w *ProductDetails.aspx* strony.

## <a name="what-youll-learn"></a>Zawartość:

- Jak dodać kontrolkę typu danych do wyświetlania produktów z bazy danych.
- Jak połączyć kontrolki danych z wybranych danych.
- Jak dodać kontrolkę typu danych, aby wyświetlić szczegółowe informacje z bazy danych.
- Jak pobrać wartości z ciągu zapytania i użyć tej wartości, aby ograniczyć dane, które są pobierane z bazy danych.

### <a name="these-are-the-features-introduced-in-the-tutorial"></a>Poniżej przedstawiono funkcje wprowadzone w tym samouczku:

- Wiązanie modelu
- Dostawców wartości

## <a name="adding-a-data-control-to-display-products"></a>Dodawanie kontrolki danych do wyświetlania produktów

Podczas tworzenia powiązania danych z formantem serwera, istnieje kilka różnych opcji, których można użyć. Najbardziej typowe opcje obejmują dodawanie kontroli źródła danych, dodając kod ręcznie lub przy użyciu wiązania modelu.

### <a name="using-a-data-source-control-to-bind-data"></a>Aby powiązać dane przy użyciu kontroli źródła danych

Dodawanie kontroli źródła danych umożliwia połączenie kontroli źródła danych do formantu, który wyświetla dane. Takie podejście umożliwia deklaratywne kontrolki serwerowe bezpośrednio połączyć źródeł danych, a nie w sposób programowy.

### <a name="coding-by-hand-to-bind-data"></a>Kodowanie ręcznie, aby powiązać dane

Dodając kod ręcznie obejmuje odczytu wartości, sprawdzanie wartości null, próbując konwertować go do odpowiedniego typu, sprawdzanie, czy konwersja powiodła się i na koniec przy użyciu wartości w zapytaniu. Jeśli chcesz zachować pełną kontrolę nad logika dostępu do danych należy użyć tej metody.

### <a name="using-model-binding-to-bind-data"></a>Przy użyciu modelu powiązania do powiązania danych

Przy użyciu wiązania modelu pozwala powiązać wyniki za pomocą znacznie mniejszej ilości kodu i daje możliwość ponownego użycia funkcji w całej aplikacji. Powiązanie modelu ma na celu uproszczenia pracy z logiką skoncentrowane na kodzie dostępu do danych przy jednoczesnym zachowaniu zalet framework rozbudowane, powiązanie danych.

## <a name="displaying-products"></a>Wyświetlanie produktów

W tym samouczku użyjesz wiązania modelu do powiązania danych. Aby skonfigurować kontrolkę danych na potrzeby tworzenia powiązania modelu wybierz dane, należy ustawić formantu `SelectMethod` właściwość na nazwę metody w kodzie strony. Kontrolki danych wywołuje metodę w odpowiednim czasie w cyklu życia strony i automatycznie wiąże zwracanych danych. Nie trzeba jawnie wywołać `DataBind` metody.

Korzystając z poniższych instrukcji, zmodyfikujesz kod znaczników w *ProductList.aspx* strony, aby wyświetlić strony produktów.

1. W **Eksploratora rozwiązań**, otwórz *ProductList.aspx* strony.
2. Zastąp istniejący kod znaczników następującym kodem:   

    [!code-aspx[Main](display_data_items_and_details/samples/sample1.aspx)]

Ten kod używa **ListView** formantu o nazwie "productList" do wyświetlania produktów.

[!code-aspx[Main](display_data_items_and_details/samples/sample2.aspx)]

**ListView** kontrolka wyświetla dane w formacie, który zdefiniujesz przy użyciu szablonów i stylów. Jest to przydatne dla danych w każdej powtarzające się struktury. To **ListView** przykładzie po prostu pokazuje dane z bazy danych, jednak aby umożliwić użytkownikom edytowanie, wstawianie i usuwanie danych i do sortowania i dane strony, wszystko to bez kodu.

Ustawiając `ItemType` właściwości w **ListView** kontrolować wyrażenia wiązania danych `Item` jest dostępny i kontrolki staje się silnie typizowane. Jak wspomniano w poprzednim samouczku, można wybrać szczegóły obiekt elementu za pomocą funkcji IntelliSense, takich jak określanie `ProductName`:

![Wyświetlanie danych elementów i szczegóły — funkcja IntelliSense](display_data_items_and_details/_static/image1.png)

Ponadto, używają wiązania modelu do określania `SelectMethod` wartość. Ta wartość (`GetProducts`) będą odpowiadać na metody, która doda kodu do wyświetlania produktów w następnym kroku.

### <a name="adding-code-to-display-products"></a>Dodawanie kodu do wyświetlania produktów

W tym kroku dodasz kod, aby wypełnić **ListView** kontrolki z danymi produktów z bazy danych. Ten kod będzie obsługiwać przedstawiający produkty według poszczególnych kategorii, a także wyświetlanie wszystkich produktów.

1. W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy *ProductList.aspx* a następnie kliknij przycisk **Wyświetl kod**.
2. Zastąp istniejący kod w *ProductList.aspx.cs* pliku następującym kodem:   

    [!code-csharp[Main](display_data_items_and_details/samples/sample3.cs)]

Ten kod zawiera `GetProducts` metodę, która odwołuje się do niej `ItemType` właściwość **ListView** w kontrolce *ProductList.aspx* strony. Aby ograniczyć wyniki do określonej kategorii w bazie danych, ustawia kod `categoryId` wartość wartość ciągu zapytania, które są przekazywane do *ProductList.aspx* gdy *ProductList.aspx* strona to przejście. `QueryStringAttribute` Klasy w `System.Web.ModelBinding` przestrzeń nazw jest używana do pobierania wartości identyfikatora zmiennych ciągu zapytania. To powoduje, że wiązanie modelu do wypróbowania można powiązać wartości z ciągu zapytania do `categoryId` parametru w czasie wykonywania.

Jeśli prawidłową kategorię jest przekazywany jako ciąg zapytania do strony, wyniki zapytania są ograniczone do tych produktów w bazie danych, które odpowiadają `categoryId` wartość. Na przykład jeśli adres URL do *ProductsList.aspx* strony jest następująca:

[!code-console[Main](display_data_items_and_details/samples/sample4.cmd)]

Na stronie są wyświetlane tylko produkty gdzie `category` jest równa `1`.

Jeśli nie ciągu zapytania jest dołączony podczas przechodzenia do *ProductList.aspx* strony wyświetli wszystkie produkty.

Źródła wartości dla tych metod są określane jako *wartość dostawców* (takie jak *QueryString*), i są określane atrybuty parametrów, wskazujące, której dostawca wartości do użycia jako wartość Dostawca atrybutów (takie jak "`id`"). Program ASP.NET zawiera dostawców wartości i odpowiednie atrybuty wszystkie typowe źródła danych wejściowych użytkownika w aplikacji formularzy sieci Web, takich jak ciąg zapytania, plików cookie, wartości formularza, formanty, stan widoku, stan sesji i właściwości profilu. Można także napisać dostawców wartości niestandardowych.

### <a name="running-the-application"></a>Uruchamianie aplikacji

Uruchom aplikację teraz aby zobaczyć, jak można wyświetlić wszystkie produkty lub po prostu zestaw ograniczone według kategorii produktów.

1. W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy *Default.aspx* strony i wybierz **Pokaż w przeglądarce**.  
 Spowoduje to otwarcie i Pokaż przeglądarki *Default.aspx* strony.
2. Wybierz **samochodów** menu nawigacji Kategoria produktu.  
 *ProductList.aspx* zostanie wyświetlona strona, pokazujący tylko produkty z kategorii "Samochody". W dalszej części tego samouczka będą wyświetlane szczegóły produktu.  

    ![Wyświetlanie danych elementów i szczegóły — samochodów](display_data_items_and_details/_static/image2.png)
3. Wybierz **produktów** z menu nawigacji u góry.  
 Ponownie *ProductList.aspx* zostanie wyświetlona strona, jednak tym razem pokazuje całą listę produktów.   

    ![Wyświetlanie danych elementów i szczegóły — produkty](display_data_items_and_details/_static/image3.png)
4. Zamknij przeglądarkę i wróć do programu Visual Studio.

### <a name="adding-a-data-control-to-display-product-details"></a>Dodawanie kontrolki danych, aby wyświetlić szczegółowe informacje o produkcie

Następnie zmodyfikujesz kod znaczników w *ProductDetails.aspx* strony dodanej w poprzednim samouczku, aby stronę można wyświetlić informacje o poszczególnych produktów.

1. W **Eksploratora rozwiązań**, otwórz *ProductDetails.aspx* strony.
2. Zastąp istniejący kod znaczników następującym kodem:   

    [!code-aspx[Main](display_data_items_and_details/samples/sample5.aspx)]

Ten kod używa **FormView** formantu, aby wyświetlić szczegóły dotyczące indywidualnych produktów. Ten kod znaczników używa metod, takich jak te, które są używane do wyświetlania danych w *ProductList.aspx* strony. **FormView** formant jest używany do wyświetlania jeden rekord jednocześnie ze źródła danych. Kiedy używasz **FormView** kontrolki, tworzenie szablonów, aby wyświetlić i edytować wartości powiązanych z danymi. Szablony zawierają kontrolki, powiązanie wyrażenia i formatowanie, które definiują wygląd i funkcjonalność formularza.

Aby połączyć z powyższych znaczników do bazy danych, należy dodać dodatkowy kod *ProductDetails.aspx* kodu.

1. W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy *ProductDetails.aspx* a następnie kliknij przycisk **Wyświetl kod**.  
   *ProductDetails.aspx.cs* plik zostanie wyświetlony.
2. Zastąp istniejący kod następującym kodem:   

    [!code-csharp[Main](display_data_items_and_details/samples/sample6.cs)]

Ten kod sprawdza, czy "`productID`" wartość ciągu zapytania. Jeśli wartość nieprawidłowy ciąg zapytania zostanie znaleziony, zostanie wyświetlony zgodnego produktu. Jeśli nie ciąg zapytania zostanie znaleziony, lub wartość ciągu zapytania jest nieprawidłowa, żaden produkt jest wyświetlany na *ProductDetails.aspx* strony.

### <a name="running-the-application"></a>Uruchamianie aplikacji

Teraz można uruchomić aplikacji, aby zobaczyć indywidualnych produktów, wyświetlane na podstawie identyfikatora produktu.

1. Naciśnij klawisz **F5** podczas gdy w programie Visual Studio, aby uruchomić aplikację.  
 Spowoduje to otwarcie i Pokaż przeglądarki *Default.aspx* strony.
2. Wybierz pozycję "Łodzi" z menu nawigacji kategorii.  
 *ProductList.aspx* zostanie wyświetlona strona.
3. Wybierz produkt "Łodzi dokument" na liście produktów.  
 *ProductDetails.aspx* zostanie wyświetlona strona.   

    ![Wyświetlanie danych elementów i szczegóły — produkty](display_data_items_and_details/_static/image4.png)
4. Zamknij przeglądarkę.

## <a name="summary"></a>Podsumowanie

W tym samouczku tej serii ma dodać znaczników i kodu, aby wyświetlić listę produktów i wyświetlić szczegółowe informacje o produkcie. W trakcie tego procesu omawialiśmy silnie typizowane kontrolki danych, tworzenia powiązania modelu i dostawców wartości. W następnym samouczku dodasz koszyka do przykładowej aplikacji Wingtip Toys.

## <a name="additional-resources"></a>Dodatkowe zasoby

[Pobieranie i wyświetlanie danych za pomocą wiązania modelu i formularzy sieci web](../../presenting-and-managing-data/model-binding/retrieving-data.md)

> [!div class="step-by-step"]
> [Poprzednie](ui_and_navigation.md)
> [dalej](shopping-cart.md)
