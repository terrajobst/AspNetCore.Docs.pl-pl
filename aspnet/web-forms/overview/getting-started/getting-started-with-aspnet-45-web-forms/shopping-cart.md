---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/shopping-cart
title: Koszyka | Dokumentacja firmy Microsoft
author: Erikre
description: "Ten samouczek serii uczy podstawowe informacje dotyczące tworzenia aplikacji formularzy sieci Web ASP.NET przy użyciu platformy ASP.NET 4.5 i programu Microsoft Visual Studio Express 2013 dla możemy..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/08/2014
ms.topic: article
ms.assetid: 6898c601-6c31-432f-8388-e6843f8a17cb
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/shopping-cart
msc.type: authoredcontent
ms.openlocfilehash: 5c0e16df7d60b944c96f8d5510225fff321124d1
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
<a name="shopping-cart"></a>Koszyk
====================
Przez [Erik Reitan](https://github.com/Erikre)

[Pobierz Wingtip Toys przykładowy projekt (C#)](http://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) lub [Pobierz E-book (PDF)](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)

> Ten samouczek serii nauczyć podstawowe informacje dotyczące tworzenia aplikacji formularzy sieci Web ASP.NET przy użyciu platformy ASP.NET 4.5 i programu Microsoft Visual Studio Express 2013 for Web. Visual Studio 2013 [projektu z kodu źródłowego C#](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) jest dostępna towarzyszące tej serii samouczka.


Ten przewodnik opisuje wymagane do dodania do przykładowej aplikacji składnika ASP.NET Web Forms Wingtip Toys koszyk logiki biznesowej. W tym samouczku jest oparty na poprzednich samouczek "Wyświetlania elementów i szczegóły danych" i jest częścią serii samouczek Wingtip zabawka magazynu. Po zakończeniu tego samouczka użytkowników aplikacji przykładowej będzie możliwość dodawania, usuwania i modyfikowania produktów w ich koszyk.

## <a name="what-youll-learn"></a>Zawartość:

1. Jak utworzyć koszyk dla aplikacji sieci web.
2. Jak umożliwić użytkownikom na dodawanie elementów do koszyka.
3. Jak dodać [GridView](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview(v=vs.110).aspx#introduction) formantu, aby wyświetlić szczegóły koszyka zakupów.
4. Sposób obliczania i wyświetlania sumy zamówienia.
5. Jak usunąć i aktualizowanie elementów w koszyku.
6. Jak dołączyć licznik koszyka zakupów.

## <a name="code-features-in-this-tutorial"></a>Funkcje kodu w tym samouczku:

1. Entity Framework Code First
2. Adnotacji danych
3. Silnie typizowane formanty danych
4. Wiązanie modelu

## <a name="creating-a-shopping-cart"></a>Tworzenie koszyk

We wcześniejszej części tego samouczka serii dodać stron i kod, aby wyświetlić dane produktu z bazy danych. W tym samouczku utworzysz koszyk do zarządzania produktów użytkownicy są zainteresowani zakupu. Użytkownicy będą mogli przeglądania i Dodaj elementy do koszyka, nawet jeśli nie są zarejestrowane lub zalogowany. Aby zarządzać dostępu koszyka zakupów, zostanie przypisana użytkowników unikatowego `ID` przy użyciu Unikatowy identyfikator globalny (GUID), gdy użytkownik uzyskuje dostęp do zakupów koszyka po raz pierwszy. Będą to przechowywane `ID` przy użyciu stanu sesji ASP.NET.

> [!NOTE] 
> 
> Stan sesji ASP.NET jest wygodne miejsce do przechowywania informacji o użytkowniku, która wygaśnie po użytkownik opuści lokacji. Natomiast nadużycia stanu sesji może mieć wpływ na wydajność w witrynach większy, jasny korzystanie z sesji stanu działa dobrze w celach demonstracyjnych. Przykładowy projekt Wingtip Toys przedstawia sposób użycia stanu sesji bez zewnętrznego dostawcy, gdy stan sesji jest przechowywane w procesie na serwerze sieci web hosta witryny. Większe witryny, które zapewniają wiele wystąpień aplikacji lub witryn, które uruchomić wiele wystąpień aplikacji na różnych serwerach, należy rozważyć użycie **usługi pamięć podręczna systemu Windows Azure**. Ta usługa pamięci podręcznej zapewnia buforowania usługami rozproszonymi zewnętrznej witryny sieci web i rozwiązuje problem przy użyciu stanu sesji w procesie. Aby uzyskać więcej informacji, zobacz [jak stanu sesji ASP.NET z systemu Windows Azure Web Sites](https://docs.microsoft.com/azure/redis-cache/cache-aspnet-session-state-provider).


### <a name="add-cartitem-as-a-model-class"></a>Dodaj CartItem jako klasę modelu

We wcześniejszej części tego samouczka serii schematu dla kategorii i produktu danych zdefiniowany przez utworzenie `Category` i `Product` klas w *modele* folderu. Teraz Dodaj nową klasę do definiowania schematu koszyk. W dalszej części tego samouczka, spowoduje dodanie klasy do obsługi dostępu do danych `CartItem` tabeli. Ta klasa dostarcza logiki biznesowej do dodawania, usuwania i aktualizowania elementów w koszyku.

1. Kliknij prawym przyciskiem myszy *modele* i wybierz polecenie **Dodaj**  - &gt; **nowy element**. 

    ![Koszyk — nowy element](shopping-cart/_static/image1.png)
2. **Dodaj nowy element** zostanie wyświetlone okno dialogowe. Wybierz **kod**, a następnie wybierz **klasy**. 

    ![Koszyka — okno dialogowe nowego elementu do dodania](shopping-cart/_static/image2.png)
3. Nazwa ta nowa klasa *CartItem.cs*.
4. Kliknij przycisk **Dodaj**.  
 Nowy plik klasy jest wyświetlany w edytorze.
5. Zastąp następujący kod w kodzie domyślnym:   

    [!code-csharp[Main](shopping-cart/samples/sample1.cs)]

`CartItem` Klasa zawiera schemat, który będzie definiował każdego produktu użytkownik dodaje do koszyka. Ta klasa jest podobny do innych schematu klasy, z którymi został utworzony we wcześniejszej części tego samouczka serii. Według Konwencji Entity Framework Code First spodziewa się, że klucz podstawowy dla `CartItem` tabeli będzie `CartItemId` lub `ID`. Jednak kod zastępuje domyślne zachowanie przy użyciu adnotacji danych `[Key]` atrybutu. `Key` Atrybutu właściwości ItemId Określa, że `ItemID` właściwość jest kluczem podstawowym.

`CartId` Właściwość określa `ID` użytkownika, który jest skojarzony z elementem zakupu. Dodasz kod w celu utworzenia tego użytkownika `ID` gdy użytkownik uzyskuje dostęp do koszyka. To `ID` będzie przechowywany jako zmienną sesji programu ASP.NET.

### <a name="update-the-product-context"></a>Aktualizacja kontekstu produktu

Oprócz dodania `CartItem` klasy, należy zaktualizować klasy kontekstu bazy danych, która zarządza klas jednostek i który zapewnia dostęp do danych w bazie danych. Aby to zrobić, spowoduje dodanie nowo utworzony `CartItem` klasa do modelu `ProductContext` klasy.

1. W **Eksploratora rozwiązań**, znajdowanie i otwieranie *ProductContext.cs* w pliku *modele* folderu.
2. Dodaj wyróżniony kod, aby *ProductContext.cs* plików w następujący sposób:  

    [!code-csharp[Main](shopping-cart/samples/sample2.cs?highlight=14)]

Jak już wspomniano w tym samouczku, kod w *ProductContext.cs* dodaje plik `System.Data.Entity` przestrzeni nazw, aby mieć dostęp do wszystkich podstawowych funkcji programu Entity Framework. Ta funkcja obejmuje możliwość zapytania, wstawiania, aktualizowania i usuwania danych Praca z silnie typizowanych obiektów. `ProductContext` Klasa dodaje dostępu do nowo dodanego `CartItem` klasa modelu.

### <a name="managing-the-shopping-cart-business-logic"></a>Zarządzanie logiką biznesową koszyka zakupów

Następnie utworzysz `ShoppingCart` klasy w nowym *logiki* folderu. `ShoppingCart` Klasa obsługuje dostępu do danych `CartItem` tabeli. Klasa zawiera również logiki biznesowej do dodawania, usuwania i aktualizowania elementów w koszyku.

Logika koszyka zakupów, który zostanie dodany będzie zawierać funkcji do zarządzania następujące akcje:

1. Dodawanie elementów do koszyka
2. Usuwanie elementów z koszyka
3. Pobieranie Identyfikatora koszyka zakupów
4. Pobieranie elementów z koszyka
5. Sumowanie ilość wszystkie elementy koszyka zakupów
6. Aktualizowanie danych koszyka zakupów

Stronie koszyka (*ShoppingCart.aspx*) i klasę koszyka zakupów będzie można używać razem dostępu do danych koszyka zakupów. Na stronie koszyka spowoduje wyświetlenie wszystkich elementów, które użytkownik dodaje do koszyka. Oprócz wygaśnięciu koszyka strony i klasy, utworzysz strony (*AddToCart.aspx*) można dodać produktów do koszyka. Zostanie również dodać kod, aby *ProductList.aspx* strony i *ProductDetails.aspx* strony, która udostępnia łącza do *AddToCart.aspx* strony, aby dodać użytkownika produkty do koszyka.

Na poniższym diagramie przedstawiono proces basic, który występuje, gdy użytkownik dodaje produktu do koszyka.

![Koszyk — dodawanie do koszyka](shopping-cart/_static/image3.png)

Po kliknięciu przez użytkownika **Dodaj do koszyka** łącze albo *ProductList.aspx* strony lub *ProductDetails.aspx* strony, aplikacji spowoduje przejście do *AddToCart.aspx* strony, a następnie automatycznie *ShoppingCart.aspx* strony. *AddToCart.aspx* strony doda wybierz produktu do koszyka przez wywołanie metody w klasie ShoppingCart. *ShoppingCart.aspx* będą wyświetlane produkty, które zostały dodane do koszyka.

#### <a name="creating-the-shopping-cart-class"></a>Tworzenie klasy koszyka zakupów

`ShoppingCart` Klasy zostaną dodane do oddzielnego folderu w aplikacji, dzięki czemu będzie jasnego rozróżnienia między modelu (folderu modeli), stron (folder główny) i logiki (folder logiki).

1. W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy **WingtipToys**projekt i wybierz **Dodaj**-&gt;**nowy Folder**. Nazwa nowego folderu *logiki*.
2. Kliknij prawym przyciskiem myszy *logiki* folder, a następnie wybierz **Dodaj**  - &gt; **nowy element**.
3. Dodaj nowy plik klasy o nazwie *ShoppingCartActions.cs*.
4. Zastąp następujący kod w kodzie domyślnym:   

    [!code-csharp[Main](shopping-cart/samples/sample3.cs)]

`AddToCart` Metoda umożliwia poszczególnych produktów, które mają zostać uwzględnione w koszyku produktu w oparciu `ID`. Produkt jest dodawana do koszyka, lub jeśli koszyka zawiera już element dla tego produktu, ilość jest zwiększany.

`GetCartId` Metoda zwraca koszyka `ID` dla użytkownika. Koszyka `ID` służy do śledzenia elementów, których użytkownik ma w ich koszyk. Jeśli użytkownik nie ma istniejących koszyka `ID`, nowe koszyka `ID` jest tworzony dla nich. Jeśli użytkownik jest zalogowany jako zarejestrowanego użytkownika, koszyka `ID` ma ustawioną wartość nazwy użytkownika. Jednak jeśli użytkownik nie jest zalogowany, koszyka `ID` ma ustawioną wartość unikatowy (GUID). Identyfikator GUID gwarantuje, że tego tylko jeden koszyka jest tworzony dla każdego użytkownika, oparte na sesji.

`GetCartItems` Metoda zwraca listę zakupy elementów z koszyka dla użytkownika. W dalszej części tego samouczka, zobaczysz, że wiązanie modelu jest używany do wyświetlania elementów koszyka zakupów przy użyciu koszyka `GetCartItems` metody.

### <a name="creating-the-add-to-cart-functionality"></a>Tworzenie funkcji Dodaj do koszyka

Jak wspomniano wcześniej, zostanie utworzona strona przetwarzania o nazwie *AddToCart.aspx* który będzie używany do dodawania nowych produktów do koszyka użytkownika. Ta strona będzie wywoływać `AddToCart` metoda `ShoppingCart` klasy, który został właśnie utworzony. *AddToCart.aspx* strony będzie oczekuje, że produkt `ID` została przekazana do niej. Ten produkt `ID` będzie używana przy wywoływaniu `AddToCart` metoda `ShoppingCart` klasy.

> [!NOTE] 
> 
> Modyfikuje kodu powiązanego (*AddToCart.aspx.cs*) na tej stronie nie strony interfejsu użytkownika (*AddToCart.aspx*).


#### <a name="to-create-the-add-to-cart-functionality"></a>Aby utworzyć Dodaj na do koszyka funkcji:

1. W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy **WingtipToys**projektu, kliknij przycisk **Dodaj**  - &gt; **nowy element**.  
 **Dodaj nowy element** zostanie wyświetlone okno dialogowe.
2. Dodaj nową stronę standardowe (formularza sieci Web) do aplikacji o nazwie *AddToCart.aspx*. 

    ![Koszyka — Dodawanie formularza sieci Web](shopping-cart/_static/image4.png)
3. W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy *AddToCart.aspx* a następnie kliknij przycisk **kod widoku**. *AddToCart.aspx.cs* plik CodeBehind jest otwarty w edytorze.
4. Zastąp istniejący kod w *AddToCart.aspx.cs* kodem następującym kodem:   

    [!code-csharp[Main](shopping-cart/samples/sample4.cs)]

Gdy *AddToCart.aspx* załadowanej strony, produktu `ID` są pobierane z ciągu zapytania. Następnie wystąpienia klasy koszyka zakupów są tworzone i używane do wywoływania `AddToCart` metody dodanego wcześniej w tym samouczku. `AddToCart` Metody zawarte w *ShoppingCartActions.cs* plików, zawiera logikę, aby dodać wybrane produktu do koszyka lub zwiększyć ilość produktu zaznaczonego produktu. Jeśli produkt nie został dodany do koszyka, produkt jest dodawany do `CartItem` tabeli bazy danych. Jeśli użytkownik dodaje dodatkowy element tego samego produktu produktu został już dodany do koszyka, ilość produktu jest zwiększany w `CartItem` tabeli. Na koniec strony przekierowuje do *ShoppingCart.aspx* strona, która zostanie dodana w następnym kroku, w którym użytkownik widzi zaktualizowaną listę elementów w koszyka.

Jak wcześniej wspomniano, użytkownik `ID` służy do identyfikowania produktów, które są skojarzone z określonym użytkownikiem. To `ID` zostanie dodany do wiersza w `CartItem` tabeli każdym razem, użytkownik dodaje produktu do koszyka.

### <a name="creating-the-shopping-cart-ui"></a>Tworzenie koszyk interfejsu użytkownika

*ShoppingCart.aspx* będą wyświetlane produkty, które użytkownik został dodany do ich koszyk. Będzie on również zawierał możliwość dodawania, usuwania i aktualizowania elementów w koszyku.

1. W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy **WingtipToys**, kliknij przycisk **Dodaj**  - &gt; **nowy element**.  
 **Dodaj nowy element** zostanie wyświetlone okno dialogowe.
2. Dodawanie nowej strony (formularza sieci Web), która zawiera stronę wzorcową wybierając **formularza sieci Web używający strony wzorcowej**. Nazwa nowej strony *ShoppingCart.aspx*.
3. Wybierz **Site.Master** dołączyć strony wzorcowej do nowo utworzony *.aspx* strony.
4. W *ShoppingCart.aspx* strony, Zamień istniejący kod znaczników następujący kod:   

    [!code-aspx[Main](shopping-cart/samples/sample5.aspx)]

*ShoppingCart.aspx* strona zawiera **GridView** formantu o nazwie `CartList`. Ten formant jest używane powiązanie modelu do powiązania danych koszyka zakupów z bazy danych do **GridView** formantu. Podczas ustawiania `ItemType` właściwość **GridView** kontrolować wyrażenia wiązania danych `Item` jest dostępna w znaczniku formantem i staje się silnie typizowane. Jak wspomniano wcześniej w tym samouczku, można wybrać szczegóły `Item` przy użyciu funkcji IntelliSense. W celu skonfigurowania formantu danych na potrzeby tworzenia powiązania modelu wybierz dane, należy ustawić `SelectMethod` właściwości formantu. W znaczniku powyżej, można ustawić `SelectMethod` przy użyciu metody GetShoppingCartItems, która zwraca listę `CartItem` obiektów. **GridView** formantu danych wywołuje metodę we właściwym czasie w cyklu życia strony i automatycznie wiąże zwróconych danych. `GetShoppingCartItems` Metody nadal musi zostać dodany.

#### <a name="retrieving-the-shopping-cart-items"></a>Pobieranie elementów koszyka zakupów

Następnie dodaj kod, aby *ShoppingCart.aspx.cs* kodem pobrać i umieścić w Interfejsie koszyka zakupów.

1. W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy *ShoppingCart.aspx* a następnie kliknij przycisk **kod widoku**. *ShoppingCart.aspx.cs* plik CodeBehind jest otwarty w edytorze.
2. Zastąp istniejący kod poniżej:  

    [!code-csharp[Main](shopping-cart/samples/sample6.cs)]

Jak wspomniano powyżej, `GridView` danych kontrolować wywołania `GetShoppingCartItems` metody w odpowiednim czasie życia strony cyklu i automatycznie wiąże zwróconych danych. `GetShoppingCartItems` Metoda tworzy wystąpienie `ShoppingCartActions` obiektu. Następnie kod korzysta to wystąpienie zostanie zwrócony elementów z koszyka, wywołując `GetCartItems` metody.

### <a name="adding-products-to-the-shopping-cart"></a>Dodawanie produktów do koszyka

Gdy albo *ProductList.aspx* lub *ProductDetails.aspx* zostanie wyświetlona strona, użytkownik będzie mogła dodawać produktu do koszyka zakupów przy użyciu łącza. Po kliknięciu łącza aplikacji powoduje przejście do strony przetwarzania o nazwie *AddToCart.aspx*. *AddToCart.aspx* wywoła strony `AddToCart` metoda `ShoppingCart` klasy dodanego wcześniej w tym samouczku.

Teraz, należy dodać **Dodaj do koszyka** łącze do obu *ProductList.aspx* strony i *ProductDetails.aspx* strony. Link ten będzie zawierać produktu `ID` które są pobierane z bazy danych.

1. W **Eksploratora rozwiązań**, znajdowanie i otwieranie stronę o nazwie *ProductList.aspx*.
2. Dodawanie znaczników wyróżnione kolorem żółtym do *ProductList.aspx* strony, tak aby całą stronę wygląda następująco:  

    [!code-aspx[Main](shopping-cart/samples/sample7.aspx?highlight=50-54)]

### <a name="testing-the-shopping-cart"></a>Testowanie koszyka

Uruchom aplikację, aby zobaczyć, jak dodać produktów do koszyka.

1. Naciśnij klawisz **F5** do uruchomienia aplikacji.  
 Po projektu odtwarza bazy danych, w przeglądarce zostanie otworzyć i Pokaż *Default.aspx* strony.
2. Wybierz **samochodów** z menu nawigacji kategorii.  
 *ProductList.aspx* zostanie wyświetlona strona, pokazujący tylko produkty uwzględnione w kategorii "Samochodów". 

    ![Koszyk - samochodów](shopping-cart/_static/image5.png)
3. Kliknij przycisk **Dodaj do koszyka** link obok pierwszego produktu wymienionych (przekonwertowania samochodów).   
 *ShoppingCart.aspx* strony zostanie wyświetlone zaznaczenie w koszyku. 

    ![Koszyk - koszyka](shopping-cart/_static/image6.png)
4. Wyświetl dodatkowe produkty, wybierając **płaszczyzn** z menu nawigacji kategorii.
5. Kliknij przycisk **Dodaj do koszyka** link obok pierwszy produkt na liście.  
 *ShoppingCart.aspx* zostanie wyświetlona strona z elementem dodatkowe.
6. Zamknij przeglądarkę.

### <a name="calculating-and-displaying-the-order-total"></a>Obliczanie i wyświetlanie sumy zamówienia

Oprócz dodania produktów do koszyka, należy dodać `GetTotal` metody `ShoppingCart` klasy i wyświetla wartość zamówienia na liście na stronie koszyka.

1. W **Eksploratora rozwiązań**, otwórz *ShoppingCartActions.cs* w pliku *logiki* folderu.
2. Dodaj następujące `GetTotal` wyróżnione kolorem żółtym do metody `ShoppingCart` klasy, dzięki czemu klasy wygląda następująco:   

    [!code-csharp[Main](shopping-cart/samples/sample8.cs?highlight=85-97)]

Najpierw `GetTotal` metoda pobiera identyfikator koszyka zakupów dla użytkownika. Następnie metoda pobiera koszyka całkowita przez pomnożenie cena produktu przez ilość produktu dla każdego produktu w koszyku na liście.

> [!NOTE] 
> 
> Powyższy kod używa wartości null typu "`int?`". Typy dopuszczające wartości zerowe może reprezentować wszystkich wartości typu podstawowego, a także jako wartość null. Aby uzyskać więcej informacji, zobacz [przy użyciu typów dopuszczających wartości zerowe](https://msdn.microsoft.com/library/2cf62fcy(v=vs.110).aspx).


### <a name="modify-the-shopping-cart-display"></a>Modyfikowanie wyświetlanie koszyka zakupów

Następnie będzie zmodyfikować kod *ShoppingCart.aspx* stronę, aby wywołać `GetTotal` — metoda i wyświetlania, które razem w *ShoppingCart.aspx* strony podczas ładowania strony.

1. W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy *ShoppingCart.aspx* i wybrać opcję **kod widoku**.
2. W *ShoppingCart.aspx.cs* plików, zaktualizuj `Page_Load` obsługi przez dodanie poniższego kodu wyróżnione kolorem żółtym:   

    [!code-csharp[Main](shopping-cart/samples/sample9.cs?highlight=16-31)]

Gdy *ShoppingCart.aspx* ładowania strony, ładuje obiektu koszyka zakupów, a następnie pobiera łączną koszyka zakupów przez wywołanie metody `GetTotal` metody `ShoppingCart` klasy. Jeśli koszyk jest pusty, w tym celu zostanie wyświetlony komunikat.

### <a name="testing-the-shopping-cart-total"></a>Testowanie łączną koszyka zakupów

Uruchom aplikację teraz, aby zobaczyć, jak nie można tylko dodać produktu do koszyka, ale można zobaczyć łączną koszyka zakupów.

1. Naciśnij klawisz **F5** do uruchomienia aplikacji.  
 W przeglądarce zostanie otworzyć i wyświetlić *Default.aspx* strony.
2. Wybierz **samochodów** z menu nawigacji kategorii.
3. Kliknij przycisk **Dodaj do koszyka** link obok pierwszego produktu.   
 *ShoppingCart.aspx* zostanie wyświetlona strona z sumy zamówienia. 

    ![Koszyka — łącznie z koszyka](shopping-cart/_static/image7.png)
4. Niektóre inne produkty (na przykład płaszczyzna) Dodaj do koszyka.
5. *ShoppingCart.aspx* zostanie wyświetlona strona z zaktualizowana Suma wszystkich produktów, które zostały dodane. 

    ![Koszyk — wiele produktów](shopping-cart/_static/image8.png)
6. Zamknięcie okna przeglądarki, należy zatrzymać uruchomionej aplikacji.

### <a name="adding-update-and-checkout-buttons-to-the-shopping-cart"></a>Dodawanie do koszyka aktualizacji i przyciski wyewidencjonowania

Aby zezwolić użytkownikom na modyfikowanie koszyka, zostanie dodana **aktualizacji** przycisk i **wyewidencjonowania** przycisk, aby na stronie koszyka. **Wyewidencjonowania** przycisk nie jest używany do momentu w dalszej części tego samouczka serii.

1. W **Eksploratora rozwiązań**, otwórz *ShoppingCart.aspx* strony w folderze głównym projektu aplikacji sieci web.
2. Aby dodać **aktualizacji** przycisk i **wyewidencjonowania** przycisk, aby *ShoppingCart.aspx* Dodaj znaczników wyróżnione kolorem żółtym istniejących znaczników, jak pokazano w następujący kod:   

    [!code-aspx[Main](shopping-cart/samples/sample10.aspx?highlight=36-45)]

Po kliknięciu przez użytkownika **aktualizacji** przycisku `UpdateBtn_Click` zostanie wywołany program obsługi zdarzeń. Ten program obsługi zdarzeń będzie wywoływać kod, który zostanie dodana w następnym kroku.

Można następnie zaktualizuj kod źródłowy znajdujący się *ShoppingCart.aspx.cs* pliku pętli elementy koszyka i wywołanie `RemoveItem` i `UpdateItem` metody.

1. W **Eksploratora rozwiązań**, otwórz *ShoppingCart.aspx.cs* w katalogu głównym projektu aplikacji sieci web.
2. Dodaj następujące sekcje kodu wyróżnione kolorem żółtym do *ShoppingCart.aspx.cs* pliku:   

    [!code-csharp[Main](shopping-cart/samples/sample11.cs?highlight=9-11,33,44-89)]

Po kliknięciu przez użytkownika **aktualizacji** znajdującego się na *ShoppingCart.aspx* strony, wywoływana jest metoda UpdateCartItems. Metoda UpdateCartItems pobiera zaktualizowane wartości dla każdego elementu w koszyku. Następnie wywołuje metodę UpdateCartItems `UpdateShoppingCartDatabase` — metoda (dodany i wyjaśniono w następnym kroku) do dodania lub usunięcia elementów z koszyka zakupów. Po bazy danych została zaktualizowana w celu uwzględnienia aktualizacji do koszyka, **GridView** aktualizacji formantu na stronie koszyka przez wywołanie metody `DataBind` metodę **GridView**. Ponadto łączna kwota zamówienia na stronie koszyka zostało zaktualizowane do uwzględnienia zaktualizowaną listę elementów.

### <a name="updating-and-removing-shopping-cart-items"></a>Aktualizacja i usuwanie elementów z koszyka zakupów

Na *ShoppingCart.aspx* strony widać formanty dodano aktualizowania ilość elementów i usunięcie elementu. Teraz Dodaj kod, aby ułatwić tych kontrolek pracy.

1. W **Eksploratora rozwiązań**, otwórz *ShoppingCartActions.cs* w pliku *logiki* folderu.
2. Dodaj następujący kod wyróżnione kolorem żółtym do *ShoppingCartActions.cs* pliku klasy:   

    [!code-csharp[Main](shopping-cart/samples/sample12.cs?highlight=99-213)]

`UpdateShoppingCartDatabase` — Metoda, wywoływana z `UpdateCartItems` metoda *ShoppingCart.aspx.cs* pozycję zawiera logikę, aktualizowanie lub usuwanie elementów z koszyka. `UpdateShoppingCartDatabase` Metody iteruje wszystkie wiersze w liście koszyka zakupów. Jeśli element koszyka zakupów została oznaczona do usunięcia lub ilość jest mniejsza niż jedna `RemoveItem` metoda jest wywoływana. W przeciwnym razie element koszyka zakupów jest sprawdzany pod kątem aktualizacji podczas `UpdateItem` metoda jest wywoływana. Po elemencie koszyka zakupów został usunięty lub zaktualizowany, zmian w bazie danych są zapisywane.

`ShoppingCartUpdates` Struktura jest używana do przechowywania wszystkich elementów koszyka zakupów. `UpdateShoppingCartDatabase` Używa metody `ShoppingCartUpdates` struktury w celu określenia, czy elementy muszą zostać zaktualizowane lub usunięte.

W następnym samouczku użyjesz `EmptyCart` metodę, aby wyczyścić zakupy koszyka po zakupie produktów. Jednak obecnie korzystasz z `GetCount` metody, który właśnie został dodany do *ShoppingCartActions.cs* plik, aby określić, ile elementów znajdują się w koszyku.

### <a name="adding-a-shopping-cart-counter"></a>Dodawanie licznik koszyka zakupów

Umożliwia użytkownikowi przeglądanie całkowitą liczbę elementów w koszyku doda licznik do *Site.Master* strony. Ten licznik zostanie również działać jako łącze do koszyka.

1. W **Eksploratora rozwiązań**, otwórz *Site.Master* strony.
2. Zmodyfikować kod znaczników przez dodanie łącze licznika koszyka zakupów, jak pokazano w żółty do sekcji nawigacji, pojawi się w następujący sposób:  

    [!code-html[Main](shopping-cart/samples/sample13.html?highlight=6)]
3. Następnie zaktualizuj kodem z *Site.Master.cs* pliku przez dodanie kodu wyróżnione kolorem żółtym w następujący sposób:  

    [!code-csharp[Main](shopping-cart/samples/sample14.cs?highlight=11,77-84)]

Przed wyświetleniem strony HTML, `Page_PreRender` zdarzenia. W `Page_PreRender` obsługi, łączna liczba koszyk jest określana przez wywołanie metody `GetCount` metody. Zwrócona wartość jest dodawana do `cartCount` zakres zawarte w znaczniku danego *Site.Master* strony. `<span>` Tagi umożliwia wewnętrzne elementy, aby być renderowane poprawnie. Po wyświetleniu dowolnej strony witryny pojawi się łączną koszyka zakupów. Użytkownik może również kliknąć łączną koszyka zakupów do wyświetlenia koszyk.

## <a name="testing-the-completed-shopping-cart"></a>Testowanie ukończone koszyk

Można uruchomić aplikacji teraz, aby zobaczyć sposób dodawania, usuwania i aktualizacji elementów w koszyku. Łączna wartość koszyka zakupów będzie odzwierciedlać całkowity koszt wszystkich elementów w koszyku.

1. Naciśnij klawisz **F5** do uruchomienia aplikacji.  
 W przeglądarce zostanie otwarty i przedstawia *Default.aspx* strony.
2. Wybierz **samochodów** z menu nawigacji kategorii.
3. Kliknij przycisk **Dodaj do koszyka** link obok pierwszego produktu.   
 *ShoppingCart.aspx* zostanie wyświetlona strona z sumy zamówienia.
4. Wybierz **płaszczyzn** z menu nawigacji kategorii.
5. Kliknij przycisk **Dodaj do koszyka** link obok pierwszego produktu.
6. Ilość pierwszego elementu w koszyku 3 i wybraniu **Usuń element** pola wyboru drugiego elementu.<a id="a"></a>
7. Kliknij przycisk **aktualizacji** przycisk, aby zaktualizować na stronie koszyka i wyświetlić nowy sumy zamówienia. 

    ![Koszyk - koszyka aktualizacji](shopping-cart/_static/image9.png)

## <a name="summary"></a>Podsumowanie

W tym samouczku został utworzony koszyk przykładowej aplikacji formularzy sieci Web Wingtip Toys. W tym samouczku użyto Entity Framework Code First, adnotacji danych kontrolki jednoznacznie danych i wiązania modelu.

Moduł koszyka zakupów obsługuje dodawanie, usuwanie i aktualizowanie elementów wybranych do zakupu. Oprócz wykonania funkcji koszyka zakupów, kiedy znasz już sposób wyświetlania elementów koszyka zakupów **GridView** kontroli oraz obliczenia sumy zamówienia.

## <a name="addition-information"></a>Dodatkowych informacji

[Przegląd stanu sesji ASP.NET](https://msdn.microsoft.com/en-us/library/ms178581.aspx)

>[!div class="step-by-step"]
[Poprzednie](display_data_items_and_details.md)
[dalej](checkout-and-payment-with-paypal.md)
