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
ms.locfileid: "30892572"
---
<a name="display-data-items-and-details"></a>Wyświetl dane elementów i szczegóły
====================
Przez [Erik Reitan](https://github.com/Erikre)

[Pobierz Wingtip Toys przykładowy projekt (C#)](http://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) lub [Pobierz E-book (PDF)](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)

> Ten samouczek serii nauczyć podstawowe informacje dotyczące tworzenia aplikacji formularzy sieci Web ASP.NET przy użyciu platformy ASP.NET 4.5 i programu Microsoft Visual Studio Express 2013 for Web. Visual Studio 2013 [projektu z kodu źródłowego C#](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) jest dostępna towarzyszące tej serii samouczka.


Ten przewodnik opisuje sposób wyświetlania danych elementów i szczegóły elementu danych przy użyciu formularzy sieci Web ASP.NET i Entity Framework Code First. W tym samouczku jest oparty na poprzednich samouczek "Nawigacji i interfejsu użytkownika" i jest częścią serii samouczek Wingtip zabawka magazynu. Po zakończeniu tego samouczka będziesz mieć możliwość Zobacz produktów na *ProductsList.aspx* strony i szczegółowe informacje o indywidualnych produktów na *ProductDetails.aspx* strony.

## <a name="what-youll-learn"></a>Zawartość:

- Jak dodać formantu danych do wyświetlenia produktów z bazy danych.
- Jak nawiązać formantu danych wybranych danych.
- Jak dodać formantu danych, aby wyświetlić szczegółowe informacje z bazy danych.
- Jak pobrać wartości z ciągu zapytania i użycie tej wartości, aby ograniczyć dane, które są pobierane z bazy danych.

### <a name="these-are-the-features-introduced-in-the-tutorial"></a>Poniżej przedstawiono funkcje wprowadzone w samouczku:

- Wiązanie modelu
- Dostawców wartości

## <a name="adding-a-data-control-to-display-products"></a>Dodawanie formantu danych do wyświetlenia produktów

Podczas tworzenia wiązania danych formantu serwera, istnieje kilka różnych opcji, których można użyć. Najbardziej typowe opcje obejmują dodawanie formantu źródła danych, dodawanie kodu ręcznie lub przy użyciu wiązania modelu.

### <a name="using-a-data-source-control-to-bind-data"></a>Za pomocą formantu źródła danych do powiązania danych

Dodawanie formantu źródła danych umożliwia łączenie formantu źródła danych do formantu, który wyświetla dane. Takie podejście umożliwia deklaratywnie połączenie bezpośrednio do źródeł danych, a nie w sposób programowy formantów po stronie serwera.

### <a name="coding-by-hand-to-bind-data"></a>Kodowanie ręcznie do powiązania danych

Dodając kod ręcznie obejmuje odczytu wartości, sprawdzanie wartości null próby przekonwertować go do odpowiedniego typu, sprawdzanie, czy konwersja powiodła się i na koniec przy użyciu wartości w zapytaniu. Jeśli chcesz zachować pełną kontrolę nad logika dostępu do danych, czy posłuż się tą metodą.

### <a name="using-model-binding-to-bind-data"></a>Przy użyciu modelu powiązanie do powiązania danych

Przy użyciu wiązania modelu umożliwia tworzenie powiązań wyników za pomocą znacznie mniej kodu i daje możliwość ponownego użycia funkcji w całej aplikacji. Powiązanie modelu ma na celu uproszczenie pracy z logiką fokus kodu dostępu do danych przy jednoczesnym zachowaniu zalet framework rozbudowane, powiązanie danych.

## <a name="displaying-products"></a>Wyświetlanie produktów

W tym samouczku użyjesz wiązania modelu do powiązania danych. Aby skonfigurować formantu danych na potrzeby tworzenia powiązania modelu wybierz dane, należy ustawić formantu `SelectMethod` właściwość na nazwę metody w kodzie strony. Formant danych wywołuje metodę we właściwym czasie w cyklu życia strony i automatycznie wiąże zwróconych danych. Nie istnieje potrzeba aby jawnie wywołać `DataBind` metody.

Wykonując poniższe kroki, będą wprowadzane zmiany kodu znaczników w *ProductList.aspx* strony, dzięki czemu można wyświetlić strony, produktów.

1. W **Eksploratora rozwiązań**, otwórz *ProductList.aspx* strony.
2. Zastąp istniejący kod znaczników następujący kod:   

    [!code-aspx[Main](display_data_items_and_details/samples/sample1.aspx)]

Ten kod zawiera **ListView** formantu o nazwie "productList", aby wyświetlić te produkty.

[!code-aspx[Main](display_data_items_and_details/samples/sample2.aspx)]

**ListView** kontroli wyświetla dane w formacie, który można zdefiniować przy użyciu szablonów i style. Jest to przydatne w przypadku danych w żadnej identycznych struktury. To **ListView** przykładzie pokazano, po prostu dane z bazy danych, jednak umożliwia użytkownikom edytowanie, wstawianie i usuwanie danych i sortowanie i dane strony wszystko to bez kodu.

Przez ustawienie `ItemType` właściwości w **ListView** kontrolować wyrażenia wiązania danych `Item` jest dostępny i formantu staje się silnie typizowane. Jak wspomniano w poprzedniej samouczek, możesz wybrać szczegóły obiektu elementu za pomocą funkcji IntelliSense, takich jak określanie `ProductName`:

![Wyświetlanie danych elementów i szczegóły — IntelliSense](display_data_items_and_details/_static/image1.png)

Ponadto używasz wiązania modelu do określenia `SelectMethod` wartość. Ta wartość (`GetProducts`) odpowiada metodzie doda kodu do wyświetlenia produktów w następnym kroku.

### <a name="adding-code-to-display-products"></a>Dodawanie kodu do wyświetlenia produktów

W tym kroku zostanie Dodaj kod, aby wypełnić **ListView** formantu z danymi produktu z bazy danych. Ten kod będzie obsługiwać produktów przedstawiający poszczególnych kategorii, a także wyświetlanie wszystkich produktów.

1. W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy *ProductList.aspx* , a następnie kliknij przycisk **kod widoku**.
2. Zastąp istniejący kod w *ProductList.aspx.cs* pliku następującym kodem:   

    [!code-csharp[Main](display_data_items_and_details/samples/sample3.cs)]

Ten kod zawiera `GetProducts` metodę, która odwołuje się do niego `ItemType` właściwość **ListView** kontroli w *ProductList.aspx* strony. Aby ograniczyć wyniki do jednej konkretnej kategorii w bazie danych, ustawia kod `categoryId` niż wartość ciąg zapytania przekazany do *ProductList.aspx* gdy *ProductList.aspx* jest strony przejście. `QueryStringAttribute` Klasy w `System.Web.ModelBinding` przestrzeni nazw służy do pobierania wartości identyfikatora zmiennej ciągu zapytania. To powoduje, że wiązanie modelu próby powiązać wartości z ciągu zapytania do `categoryId` parametru w czasie wykonywania.

W przypadku prawidłową kategorią jest przekazywany jako ciąg zapytania do strony, wyniki zapytania są ograniczone do tych produktów w bazie danych, które pasują `categoryId` wartość. Na przykład jeśli adres URL, który *ProductsList.aspx* strona jest następujące:

[!code-console[Main](display_data_items_and_details/samples/sample4.cmd)]

Na stronie są wyświetlane tylko te produkty gdzie `category` jest równe `1`.

Jeśli ciąg zapytania nie jest dołączony podczas nawigowania do *ProductList.aspx* stronę, wszystkie produkty zostaną wyświetlone.

Źródła wartości te metody są określane jako *wartość dostawców* (takich jak *QueryString*), i wskazujący, który dostawca wartości do użycia atrybuty parametru są określane jako wartość atrybuty dostawcy (takich jak "`id`"). Program ASP.NET zawiera dostawców wartości i atrybuty odpowiednie dla wszystkich typowych źródeł danych wejściowych użytkownika w aplikacji formularzy sieci Web, na przykład ciąg zapytania, plików cookie, wartości formularza, kontrolek, stan widoku, stan sesji i właściwości profilu. Można również napisać dostawców wartości niestandardowych.

### <a name="running-the-application"></a>Uruchamianie aplikacji

Uruchom aplikację teraz, aby zobaczyć, jak można wyświetlić wszystkie produkty lub po prostu zestawu ograniczone według kategorii produktów.

1. W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy *Default.aspx* i wybrać opcję **Wyświetl w przeglądarce**.  
 W przeglądarce zostanie otworzyć i wyświetlić *Default.aspx* strony.
2. Wybierz **samochodów** menu nawigacji Kategoria produktu.  
 *ProductList.aspx* zostanie wyświetlona strona, pokazujący tylko produkty uwzględnione w kategorii "Samochodów". W dalszej części tego samouczka są wyświetlane takie szczegóły produktu.  

    ![Wyświetlanie danych elementów i szczegóły — samochodów](display_data_items_and_details/_static/image2.png)
3. Wybierz **produktów** z menu nawigacji u góry.  
 Ponownie *ProductList.aspx* strona jest wyświetlana, ale tym razem zawiera całą listę produktów.   

    ![Wyświetlanie danych elementów i szczegóły — produktów](display_data_items_and_details/_static/image3.png)
4. Zamknij przeglądarkę i powrócić do programu Visual Studio.

### <a name="adding-a-data-control-to-display-product-details"></a>Dodawanie formantu danych, aby wyświetlić szczegółowe informacje o produkcie

Następnie będzie modyfikowania kodu znaczników w *ProductDetails.aspx* strony dodanego w poprzedniej samouczka, dzięki czemu strony można wyświetlić informacje o indywidualnych produktów.

1. W **Eksploratora rozwiązań**, otwórz *ProductDetails.aspx* strony.
2. Zastąp istniejący kod znaczników następujący kod:   

    [!code-aspx[Main](display_data_items_and_details/samples/sample5.aspx)]

Ten kod zawiera **FormView** formantu, aby wyświetlić szczegółowe informacje o indywidualnych produktów. Ten kod znaczników używa metody, takie jak te, które są używane do wyświetlania danych w *ProductList.aspx* strony. **FormView** kontroli jest używana do wyświetlania pojedynczego rekordu jednocześnie ze źródła danych. Jeśli używasz **FormView** kontroli, utworzyć szablony, aby wyświetlić i edytować wartości powiązane z danymi. Szablony zawierają formanty, wyrażenia powiązania i formatowanie definiujących wygląd i funkcjonalność formularza.

Aby połączyć powyżej znaczników do bazy danych, należy dodać dodatkowy kod w celu *ProductDetails.aspx* kodu.

1. W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy *ProductDetails.aspx* , a następnie kliknij przycisk **kod widoku**.  
   *ProductDetails.aspx.cs* plik zostanie wyświetlony.
2. Zastąp istniejący kod następujący kod:   

    [!code-csharp[Main](display_data_items_and_details/samples/sample6.cs)]

Sprawdza, czy ten kod "`productID`" wartość ciągu zapytania. Jeśli wartość nieprawidłowy ciąg zapytania zostanie znaleziony, zostanie wyświetlony zgodnego produktu. Jeśli nie ciąg zapytania zostanie znaleziony, lub wartość ciągu zapytania jest nieprawidłowa, produktu nie jest wyświetlany na *ProductDetails.aspx* strony.

### <a name="running-the-application"></a>Uruchamianie aplikacji

Teraz można uruchomić aplikację, aby zobaczyć indywidualnych produktów, wyświetlane na podstawie identyfikatora produktu.

1. Naciśnij klawisz **F5** while w programie Visual Studio, aby uruchomić aplikację.  
 W przeglądarce zostanie otworzyć i wyświetlić *Default.aspx* strony.
2. Wybierz "Łodzi" w menu nawigacji kategorii.  
 *ProductList.aspx* zostanie wyświetlona strona.
3. Wybierz produkt "Papieru łodzi" na liście produktów.  
 *ProductDetails.aspx* zostanie wyświetlona strona.   

    ![Wyświetlanie danych elementów i szczegóły — produktów](display_data_items_and_details/_static/image4.png)
4. Zamknij przeglądarkę.

## <a name="summary"></a>Podsumowanie

W tym samouczku serii mają Dodawanie znaczników i kodu, aby wyświetlić listę produktów i wyświetlić informacje szczegółowe. W trakcie tego procesu uzyskanych o jednoznacznie danych kontrolki, wiązania modelu i dostawców wartości. W następnym samouczku zostanie dodana koszyk Wingtip Toys przykładowej aplikacji.

## <a name="additional-resources"></a>Dodatkowe zasoby

[Trwa pobieranie i wyświetlanie danych z wiązania modelu i formularzy sieci web](../../presenting-and-managing-data/model-binding/retrieving-data.md)

> [!div class="step-by-step"]
> [Poprzednie](ui_and_navigation.md)
> [dalej](shopping-cart.md)
