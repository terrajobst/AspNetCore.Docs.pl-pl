---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/shopping-cart
title: Koszyk | Dokumentacja firmy Microsoft
author: Erikre
description: Tej serii samouczków obejmuje podstawy tworzenia aplikacji formularzy sieci Web ASP.NET przy użyciu platformy ASP.NET 4.5 i programu Microsoft Visual Studio Express 2013 for firma Microsoft...
ms.author: riande
ms.date: 09/08/2014
ms.assetid: 6898c601-6c31-432f-8388-e6843f8a17cb
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/shopping-cart
msc.type: authoredcontent
ms.openlocfilehash: 2823970144e6dce4a3a9f4d28dabfabe9fe053d6
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/16/2018
ms.locfileid: "41756321"
---
<a name="shopping-cart"></a>Koszyk
====================
przez [Erik Reitan](https://github.com/Erikre)

[Pobierz Wingtip Toys przykładowego projektu (C#)](http://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) lub [Pobierz książkę elektroniczną (PDF)](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)

> W tej serii samouczków obejmuje podstawy tworzenia aplikacji formularzy sieci Web ASP.NET przy użyciu platformy ASP.NET 4.5 i programu Microsoft Visual Studio Express 2013 for Web. Visual Studio 2013 [projektu za pomocą kodu źródłowego języka C#](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) jest dostępny dla tej serii samouczków towarzyszą.


W tym samouczku opisano logikę biznesową, wymagane do dodania koszyka do przykładowej aplikacji ASP.NET Web Forms Wingtip Toys. Ten samouczek opiera się na poprzednim samouczku "Wyświetlanie danych i szczegółów elementów" i jest częścią serii samouczków o nazwie Wingtip zabawki Store. Po ukończeniu tego samouczka użytkowników przykładowej aplikacji będzie do dodawania, usuwania i modyfikowania produktów w ich koszyk sklepowy.

## <a name="what-youll-learn"></a>Zawartość:

1. Jak utworzyć koszyka zakupów dla aplikacji sieci web.
2. Jak umożliwić użytkownikom dodawanie elementów do koszyka.
3. Jak dodać [GridView](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview(v=vs.110).aspx#introduction) formantu, aby wyświetlić szczegółów koszyka zakupów.
4. Jak obliczają i wyświetlają sumy zamówienia.
5. Jak usunąć i aktualizować elementy na koszyk sklepowy.
6. Jak dołączyć licznik koszyka zakupów.

## <a name="code-features-in-this-tutorial"></a>Kod funkcji w ramach tego samouczka:

1. Entity Framework Code pierwszy
2. Adnotacje danych
3. Silnie typizowane kontrolki danych
4. Wiązanie modelu

## <a name="creating-a-shopping-cart"></a>Tworzenie koszyka

We wcześniejszej części tej serii samouczków dodać strony i kodem, aby wyświetlić dane produktów z bazy danych. W tym samouczku utworzysz koszyka do zarządzania produktami użytkownicy są zainteresowani zakupu. Użytkownicy będą mogli przeglądać i dodawanie elementów do koszyka, nawet jeśli nie są zarejestrowane lub zarejestrowane w. Aby zarządzać koszyk sklepowy dostępu, będą przypisywane użytkownikom unikatową `ID` przy użyciu Unikatowy identyfikator globalny (GUID), gdy użytkownik uzyskuje dostęp do sklepu koszyka po raz pierwszy. Można będzie zapisać `ID` przy użyciu stanu sesji platformy ASP.NET.

> [!NOTE] 
> 
> Stan sesji platformy ASP.NET jest wygodne miejsce do przechowywania informacji o użytkowniku, która wygaśnie po użytkownik opuści lokacji. Gdy nieuprawnione użycie stanu sesji mogą mieć wpływ na wydajność w witrynach większe, jasny korzystanie z sesji, stan działa również w celach demonstracyjnych. Przykładowy projekt o nazwie Wingtip Toys przedstawiono sposób używania stanu sesji bez zewnętrznego dostawcy, których stan sesji jest przechowywane w procesie na serwerze sieci web hosta witryny. Dla większych witryny, które zapewniają wiele wystąpień aplikacji lub witryn, które uruchomić wiele wystąpień aplikacji na różnych serwerach, należy wziąć pod uwagę przy użyciu **Windows Azure Cache Service**. Ta usługa pamięci podręcznej udostępnia usługę buforowania rozproszonego, jest zewnętrzne w stosunku do witryny sieci web, która rozwiązuje problem przy użyciu stanu sesji w procesie. Aby uzyskać więcej informacji, zobacz [jak stanu sesji ASP.NET przy użyciu Windows Azure Web Sites](https://docs.microsoft.com/azure/redis-cache/cache-aspnet-session-state-provider).


### <a name="add-cartitem-as-a-model-class"></a>Dodaj CartItem jako klasy modelu

We wcześniejszej części tej serii samouczków, definicja schematu dla kategorii, a dane produktów, tworząc `Category` i `Product` klas w *modeli* folderu. Teraz Dodaj nową klasę do definiowania schematu do koszyka. W dalszej części tego samouczka dodasz klasy do obsługi dostępu do danych `CartItem` tabeli. Ta klasa dostarcza logikę biznesową do dodawania, usuwania i aktualizowania elementów w koszyku.

1. Kliknij prawym przyciskiem myszy *modeli* i wybierz polecenie **Dodaj**  - &gt; **nowy element**. 

    ![Koszyk sklepowy - nowy element](shopping-cart/_static/image1.png)
2. **Dodaj nowy element** zostanie wyświetlone okno dialogowe. Wybierz **kodu**, a następnie wybierz pozycję **klasy**. 

    ![Koszyk — okno dialogowe nowego elementu Dodawanie](shopping-cart/_static/image2.png)
3. Nazwa ta nowa klasa *CartItem.cs*.
4. Kliknij przycisk **Dodaj**.  
   Plik nowej klasy jest wyświetlany w edytorze.
5. Zastąp domyślny kod następującym kodem:   

    [!code-csharp[Main](shopping-cart/samples/sample1.cs)]

`CartItem` Klasy zawiera schemat, definiujące każdego produktu, użytkownik dodaje do koszyka. Ta klasa jest podobne do innych klas schematu, utworzonej we wcześniejszej części tej serii samouczków. Zgodnie z Konwencją Entity Framework Code First spodziewa się, że klucz podstawowy dla `CartItem` tabeli będzie `CartItemId` lub `ID`. Jednak kod zastępuje domyślne zachowanie przy użyciu adnotacji danych `[Key]` atrybutu. `Key` Atrybut właściwości identyfikator elementu Określa, że `ItemID` właściwość jest kluczem podstawowym.

`CartId` Właściwość określa `ID` użytkownika, który jest skojarzony z elementem do zakupu. Następnie dodasz kod do utworzenia tego użytkownika `ID` gdy użytkownik uzyskuje dostęp do koszyka. To `ID` będzie przechowywany jako zmienną sesji programu ASP.NET.

### <a name="update-the-product-context"></a>Aktualizowanie kontekstu produktu

Oprócz dodawania `CartItem` klasy, należy zaktualizować klasy kontekstu bazy danych, który zarządza klas jednostek i który zapewnia dostęp do danych w bazie danych. Aby to zrobić, dodasz nowo utworzony `CartItem` modelu klasy `ProductContext` klasy.

1. W **Eksploratora rozwiązań**, znajdowanie i otwieranie *ProductContext.cs* w pliku *modeli* folderu.
2. Dodaj wyróżniony kod do *ProductContext.cs* pliku w następujący sposób:  

    [!code-csharp[Main](shopping-cart/samples/sample2.cs?highlight=14)]

Jak wspomniano wcześniej w tej serii samouczków, kod w *ProductContext.cs* dodaje plik `System.Data.Entity` przestrzeni nazw, aby mieć dostęp do wszystkich funkcji programu Entity Framework core. Ta funkcja obejmuje możliwość zapytania, wstawiania, aktualizacji i usuwania danych dzięki współpracy z silnie typizowanych obiektów. `ProductContext` Klasa dodaje dostępu do nowo dodanych `CartItem` klasa modelu.

### <a name="managing-the-shopping-cart-business-logic"></a>Zarządzanie zakupów logikę biznesową koszyka

Następnie utworzysz `ShoppingCart` klas w nowym *logiki* folderu. `ShoppingCart` Klasa obsługuje dostęp do danych `CartItem` tabeli. Klasa zawiera również logikę biznesową do dodawania, usuwania i aktualizowania elementów w koszyku.

Logiki koszyka zakupów, który zostanie dodany będzie zawierać funkcje umożliwiające zarządzanie następujące akcje:

1. Dodawanie towarów do koszyka
2. Usuwanie elementów z koszyka
3. Uzyskiwanie Identyfikatora koszyka zakupów
4. Pobieranie elementów z koszyka
5. Sumowanie ilość wszystkie elementy koszyka zakupów
6. Aktualizowanie danych koszyka zakupów

Stronie koszyka (*ShoppingCart.aspx*) i klasę koszyka zakupów będzie można używać razem dostępu do zakupów danych koszyka. Na stronie koszyka będą wyświetlane wszystkie elementy, które użytkownik dodaje do koszyka. Oprócz koszyka strony i klasy, utworzysz stronę (*AddToCart.aspx*) aby dodać produkty do koszyka. Możesz również dodać kod, aby *ProductList.aspx* strony i *ProductDetails.aspx* strona, która będzie udostępniać link do *AddToCart.aspx* strony, czemu użytkownik może dodać produkty do koszyka.

Na poniższym diagramie przedstawiono proces podstawowy, który występuje, gdy użytkownik dodaje produkt do koszyka.

![Koszyk — dodawanie do koszyka](shopping-cart/_static/image3.png)

Kiedy użytkownik kliknie **Dodaj do koszyka** łącze w dowolnym *ProductList.aspx* strony lub *ProductDetails.aspx* stronie aplikacji spowoduje przejście do *AddToCart.aspx* strony i następnie automatycznie *ShoppingCart.aspx* strony. *AddToCart.aspx* strony doda wybierz produkt do koszyka, wywołując metodę w klasie ShoppingCart. *ShoppingCart.aspx* strony wyświetli produktów, które zostały dodane do koszyka.

#### <a name="creating-the-shopping-cart-class"></a>Tworzenie klasy koszyka zakupów

`ShoppingCart` Klasy zostaną dodane do oddzielnego folderu w aplikacji, dzięki czemu będą wyraźne rozróżnienie między modelu (folderu modeli), strony (folder główny) i Logic Apps (logiki folder).

1. W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy **WingtipToys**projektu, a następnie wybierz **Dodaj**-&gt;**nowy Folder**. Nadaj nazwę nowego folderu *logiki*.
2. Kliknij prawym przyciskiem myszy *logiki* folder, a następnie wybierz **Dodaj**  - &gt; **nowy element**.
3. Dodaj plik klasy o nazwie *ShoppingCartActions.cs*.
4. Zastąp domyślny kod następującym kodem:   

    [!code-csharp[Main](shopping-cart/samples/sample3.cs)]

`AddToCart` Metoda umożliwia poszczególnych produktów, które mają zostać uwzględnione w koszyka opartych na produkcie `ID`. Produkt jest dodawana do koszyka, lub jeśli koszyka zawiera już element dla tego produktu, ilość jest zwiększany.

`GetCartId` Metoda zwraca koszyka `ID` dla użytkownika. Koszyka `ID` służy do śledzenia elementów, które użytkownik ma w ich koszyk sklepowy. Jeśli użytkownik nie ma istniejących koszyka `ID`, nowe koszyka `ID` jest dla niego tworzone. Jeśli użytkownik jest zalogowany jako zarejestrowanego użytkownika koszyka `ID` jest ustawiony do ich nazw użytkowników. Jednak jeśli użytkownik nie jest podpisany w koszyku `ID` jest ustawiona na unikatową wartość (GUID). Identyfikator GUID gwarantuje, że w tym tylko jeden koszyka jest tworzony dla każdego użytkownika, oparte na sesji.

`GetCartItems` Metoda zwraca listę towary w koszyku dla użytkownika. W dalszej części tego samouczka, zobaczysz, że wiązanie modelu służy do wyświetlania elementów koszyka zakupów przy użyciu koszyka `GetCartItems` metody.

### <a name="creating-the-add-to-cart-functionality"></a>Tworzenie funkcji Dodaj do koszyka

Jak wspomniano wcześniej, utworzysz stronę przetwarzania o nazwie *AddToCart.aspx* który będzie używany do dodawania nowych produktów do koszyka użytkownika. Ta strona będzie wywoływać `AddToCart` method in Class metoda `ShoppingCart` klasy, który został utworzony. *AddToCart.aspx* strony będą oczekiwać, że produkt `ID` jest przekazywany do niego. Ten produkt `ID` będzie używany podczas wywoływania `AddToCart` method in Class metoda `ShoppingCart` klasy.

> [!NOTE] 
> 
> Modyfikuje kodu powiązanego (*AddToCart.aspx.cs*) na tej stronie nie strony interfejsu użytkownika (*AddToCart.aspx*).


#### <a name="to-create-the-add-to-cart-functionality"></a>Aby utworzyć Dodaj na do koszyka funkcji:

1. W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy **WingtipToys**projektu, kliknij przycisk **Dodaj**  - &gt; **nowy element**.  
   **Dodaj nowy element** zostanie wyświetlone okno dialogowe.
2. Dodawanie standardowych nowej strony (formularz sieci Web) do aplikacji o nazwie *AddToCart.aspx*. 

    ![Koszyk — Dodaj formularz sieci Web](shopping-cart/_static/image4.png)
3. W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy *AddToCart.aspx* strony, a następnie kliknij przycisk **Wyświetl kod**. *AddToCart.aspx.cs* pliku związanego z kodem jest otwarty w edytorze.
4. Zastąp istniejący kod w *AddToCart.aspx.cs* związanym z kodem następującym kodem:   

    [!code-csharp[Main](shopping-cart/samples/sample4.cs)]

Gdy *AddToCart.aspx* strona została załadowana, produkt `ID` jest pobierana z ciągu zapytania. Następnie tworzone i używane do wywoływania wystąpienia klasy koszyka zakupów `AddToCart` metoda dodaną wcześniej w tym samouczku. `AddToCart` Metody zawarte w *ShoppingCartActions.cs* plików, zawiera logikę do dodania wybrany produkt do koszyka lub zwiększ ilość produktu wybranego produktu. Jeśli nie został dodany produkt do koszyka, produktu jest dodawany do `CartItem` tabeli bazy danych. Jeśli produkt został już dodany do koszyka, a użytkownik dodaje dodatkowy element tego samego produktu, ilość produktów jest zwiększany w `CartItem` tabeli. Na koniec wróć do przekierowania na stronie *ShoppingCart.aspx* strony, które należy dodać w następnym kroku, w którym użytkownik widzi zaktualizowaną listę elementów w koszyku.

Jak wcześniej wspomniano, użytkownik `ID` służy do identyfikowania produktów, które są skojarzone z określonym użytkownikiem. To `ID` zostanie dodany do wiersza w `CartItem` tabeli każdorazowo użytkownik dodaje produkt do koszyka.

### <a name="creating-the-shopping-cart-ui"></a>Tworzenie koszyka interfejsu użytkownika

*ShoppingCart.aspx* strony wyświetli produktów, które użytkownik został dodany do ich koszyk sklepowy. Będzie ona również możliwość dodawania, usuwania i aktualizowania elementów w koszyku.

1. W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy **WingtipToys**, kliknij przycisk **Dodaj**  - &gt; **nowy element**.  
   **Dodaj nowy element** zostanie wyświetlone okno dialogowe.
2. Dodaj nową stronę (formularz sieci Web), która zawiera stronę wzorcową, wybierając **formularz sieci Web używający strony wzorcowej**. Nadaj nowej stronie *ShoppingCart.aspx*.
3. Wybierz **Site.Master** można dołączyć strony wzorcowej do nowo utworzonego *.aspx* strony.
4. W *ShoppingCart.aspx* strony, Zastąp istniejący kod znaczników następującym kodem:   

    [!code-aspx[Main](shopping-cart/samples/sample5.aspx)]

*ShoppingCart.aspx* strona zawiera **GridView** formantu o nazwie `CartList`. Ta kontrola korzysta wiązania modelu do powiązania danych koszyka zakupów z bazy danych w **GridView** kontroli. Po ustawieniu `ItemType` właściwość **GridView** kontrolować wyrażenia wiązania danych `Item` jest dostępna w kodu znaczników kontrolki i kontroli staje się silnie typizowane. Jak wspomniano we wcześniejszej części tej serii samouczków, możesz wybrać szczegóły `Item` obiekt, za pomocą funkcji IntelliSense. Aby skonfigurować kontrolkę danych na potrzeby tworzenia powiązania modelu wybierz dane, należy ustawić `SelectMethod` właściwości formantu. W znacznikach powyżej, ustawić `SelectMethod` przy użyciu metody GetShoppingCartItems, która zwraca listę `CartItem` obiektów. **GridView** kontrola nad danymi wywołuje metodę w odpowiednim czasie w cyklu życia strony i automatycznie wiąże zwracanych danych. `GetShoppingCartItems` Metoda nadal musi zostać dodany.

#### <a name="retrieving-the-shopping-cart-items"></a>Trwa pobieranie elementów koszyka zakupów

Następnie dodaj kod, aby *ShoppingCart.aspx.cs* kodu powiązanego do pobrania i wypełnić koszyk na zakupy interfejsu użytkownika.

1. W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy *ShoppingCart.aspx* strony, a następnie kliknij przycisk **Wyświetl kod**. *ShoppingCart.aspx.cs* pliku związanego z kodem jest otwarty w edytorze.
2. Zastąp istniejący kod następujących czynności:  

    [!code-csharp[Main](shopping-cart/samples/sample6.cs)]

Jak wspomniano powyżej, `GridView` danych kontrolować wywołania `GetShoppingCartItems` metody w odpowiednim czasie życia strony cyklu i automatycznie wiąże zwracanych danych. `GetShoppingCartItems` Metoda tworzy wystąpienie `ShoppingCartActions` obiektu. Następnie kod używa tego wystąpienia, aby przywrócić elementów w koszyku przez wywołanie metody `GetCartItems` metody.

### <a name="adding-products-to-the-shopping-cart"></a>Dodawanie produktów do koszyka

Gdy albo *ProductList.aspx* lub *ProductDetails.aspx* zostanie wyświetlona strona, użytkownik będzie mógł dodać produkt do koszyka, za pomocą linku. Po kliknięciu łącza, aplikacja przejdzie do strony przetwarzania o nazwie *AddToCart.aspx*. *AddToCart.aspx* wywoła strony `AddToCart` method in Class metoda `ShoppingCart` klasy dodaną wcześniej w tym samouczku.

Teraz dodasz **Dodaj do koszyka** łącze do obu *ProductList.aspx* strony i *ProductDetails.aspx* strony. Ten link będzie zawierać produktu `ID` , są pobierane z bazy danych.

1. W **Eksploratora rozwiązań**, Znajdź i Otwórz stronę o nazwie *ProductList.aspx*.
2. Dodawanie znaczników wyróżniony na żółto do *ProductList.aspx* stronie tak, aby cała strona wygląda następująco:  

    [!code-aspx[Main](shopping-cart/samples/sample7.aspx?highlight=50-54)]

### <a name="testing-the-shopping-cart"></a>Testowanie koszyka

Uruchom aplikację, aby zobaczyć, jak dodać produkty do koszyka.

1. Naciśnij klawisz **F5** do uruchomienia aplikacji.  
 Po projekcie odtwarza bazy danych, przeglądarce otworzy się i Pokaż *Default.aspx* strony.
2. Wybierz **samochodów** z menu nawigacji kategorii.  
 *ProductList.aspx* zostanie wyświetlona strona, pokazujący tylko produkty z kategorii "Samochody". 

    ![Koszyk sklepowy - samochodów](shopping-cart/_static/image5.png)
3. Kliknij przycisk **Dodaj do koszyka** łącze obok produktów pierwsze na liście (samochód konwertowany).   
 *ShoppingCart.aspx* zostanie wyświetlona strona, przedstawiający zaznaczenie w koszyku. 

    ![Koszyk sklepowy - koszyka](shopping-cart/_static/image6.png)
4. Wyświetl dodatkowe produkty, wybierając **płaszczyzn** z menu nawigacji kategorii.
5. Kliknij przycisk **Dodaj do koszyka** łącze obok pierwszy produkt na liście.  
 *ShoppingCart.aspx* zostanie wyświetlona strona z elementem dodatkowe.
6. Zamknij przeglądarkę.

### <a name="calculating-and-displaying-the-order-total"></a>Obliczanie i wyświetlanie sumy zamówienia

Oprócz dodawania produktów do koszyka, należy dodać `GetTotal` metody `ShoppingCart` klasy i wyświetlić łączna kwota zamówienia na stronie Koszyk sklepowy.

1. W **Eksploratora rozwiązań**, otwórz *ShoppingCartActions.cs* w pliku *logiki* folderu.
2. Dodaj następujący kod `GetTotal` wyróżniony na żółto do metody `ShoppingCart` klasy, tak aby klasy wygląda następująco:   

    [!code-csharp[Main](shopping-cart/samples/sample8.cs?highlight=85-97)]

Po pierwsze, `GetTotal` metoda pobiera identyfikator koszyka zakupów dla użytkownika. Następnie metoda pobiera koszyka całkowitą przez pomnożenie cena produktu przez ilość produktu dla każdego produktu na liście w koszyku.

> [!NOTE] 
> 
> Powyższy kod używa typu dopuszczającego wartość null "`int?`". Typy dopuszczające wartość null może reprezentować wszystkie wartości typu podstawowego, a także jako wartość null. Aby uzyskać więcej informacji, zobacz [przy użyciu typów dopuszczających wartości zerowe](https://msdn.microsoft.com/library/2cf62fcy(v=vs.110).aspx).


### <a name="modify-the-shopping-cart-display"></a>Zmodyfikować sposób wyświetlania koszyka zakupów

Następnie zmodyfikujesz kod *ShoppingCart.aspx* strony, aby wywołać `GetTotal` metody i sposobu wyświetlania łącznie na *ShoppingCart.aspx* stronie podczas ładowania strony.

1. W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy *ShoppingCart.aspx* strony i wybierz **Wyświetl kod**.
2. W *ShoppingCart.aspx.cs* pliku, zaktualizuj `Page_Load` program obsługi, dodając następujący kod wyróżniony na żółto:   

    [!code-csharp[Main](shopping-cart/samples/sample9.cs?highlight=16-31)]

Gdy *ShoppingCart.aspx* strona ładuje się, ładuje obiekt koszyka zakupów, a następnie pobiera łączną koszyka zakupów, wywołując `GetTotal` metody `ShoppingCart` klasy. Jeśli koszyka jest pusta, w tym celu jest wyświetlony komunikat.

### <a name="testing-the-shopping-cart-total"></a>Testowanie Suma koszyka zakupów

Uruchom aplikację teraz aby zobaczyć, jak można nie tylko dodać produkt do koszyka, ale możesz zobaczyć łączną koszyka zakupów.

1. Naciśnij klawisz **F5** do uruchomienia aplikacji.  
 Spowoduje to otwarcie i Pokaż przeglądarki *Default.aspx* strony.
2. Wybierz **samochodów** z menu nawigacji kategorii.
3. Kliknij przycisk **Dodaj do koszyka** łącze obok pierwszego produktu.   
 *ShoppingCart.aspx* zostanie wyświetlona strona z sumy zamówienia. 

    ![Koszyk — łącznie z koszyka](shopping-cart/_static/image7.png)
4. Niektóre inne produkty (na przykład płaszczyzna) należy dodać do koszyka.
5. *ShoppingCart.aspx* zostanie wyświetlona strona z sumą zaktualizowane dla wszystkich produktów, które zostały dodane. 

    ![Koszyk sklepowy — wielu produktów](shopping-cart/_static/image8.png)
6. Zatrzymaj uruchomionej aplikacji przez zamknięcie okna przeglądarki.

### <a name="adding-update-and-checkout-buttons-to-the-shopping-cart"></a>Dodawanie aktualizacji i wyewidencjonowania przycisków do koszyka

Aby zezwolić użytkownikom na modyfikowanie koszyka, należy dodać **aktualizacji** przycisku i **wyewidencjonowania** przycisk, aby na stronie koszyka. **Wyewidencjonowania** przycisk nie jest używany do momentu w dalszej części tej serii samouczków.

1. W **Eksploratora rozwiązań**, otwórz *ShoppingCart.aspx* strony w folderze głównym projektu aplikacji sieci web.
2. Aby dodać **aktualizacji** przycisku i **wyewidencjonowania** przycisk, aby *ShoppingCart.aspx* strony, Dodaj znaczniki wyróżniony na żółto istniejących znaczników, jak pokazano w Poniższy kod:   

    [!code-aspx[Main](shopping-cart/samples/sample10.aspx?highlight=36-45)]

Kiedy użytkownik kliknie **aktualizacji** przycisku `UpdateBtn_Click` zostanie wywołana procedura obsługi zdarzeń. Ta procedura obsługi zdarzeń wywołuje kod, który należy dodać w następnym kroku.

Następnie należy zaktualizować kod zawarty w *ShoppingCart.aspx.cs* pliku pętli elementów do koszyka i wywołania `RemoveItem` i `UpdateItem` metody.

1. W **Eksploratora rozwiązań**, otwórz *ShoppingCart.aspx.cs* plik w folderze głównym projektu aplikacji sieci web.
2. Dodaj poniższe sekcje kodu wyróżniony na żółto do *ShoppingCart.aspx.cs* pliku:   

    [!code-csharp[Main](shopping-cart/samples/sample11.cs?highlight=9-11,33,44-89)]

Kiedy użytkownik kliknie **aktualizacji** znajdujący się na *ShoppingCart.aspx* strony, wywoływana jest metoda UpdateCartItems. Metoda UpdateCartItems pobiera zaktualizowane wartości dla każdego elementu w koszyku. Następnie wywołuje metodę UpdateCartItems `UpdateShoppingCartDatabase` — metoda (dodane i wyjaśnione w następnym kroku) Dodawanie lub usuwanie elementów z koszyka. Gdy baza danych została zaktualizowana w celu odzwierciedlenia aktualizacji do koszyka, **GridView** kontroli aktualizacji na stronie koszyka, wywołując `DataBind` metodę **GridView**. Ponadto łączna kwota zamówienia na stronie Koszyk sklepowy aktualizowany w celu odzwierciedlenia zaktualizowaną listę elementów.

### <a name="updating-and-removing-shopping-cart-items"></a>Aktualizacja i usuwanie elementów z koszyka zakupów

Na *ShoppingCart.aspx* stronie widać, zostały dodane formanty aktualizowania ilość elementów i usunięcie elementu. Teraz Dodaj kod, który spowoduje, że te formanty pracy.

1. W **Eksploratora rozwiązań**, otwórz *ShoppingCartActions.cs* w pliku *logiki* folderu.
2. Dodaj następujący kod wyróżniony na żółto do *ShoppingCartActions.cs* pliku klasy:   

    [!code-csharp[Main](shopping-cart/samples/sample12.cs?highlight=99-213)]

`UpdateShoppingCartDatabase` Metody wywoływane z `UpdateCartItems` metody *ShoppingCart.aspx.cs* stronie, zawiera logikę, aktualizowanie lub usuwanie elementów z koszyka. `UpdateShoppingCartDatabase` Metoda wykonuje iterację przez wszystkie wiersze w obrębie listy koszyka zakupów. Jeśli element do koszyka zakupów została oznaczona do usunięcia lub ilość jest mniejsza niż jedna `RemoveItem` metoda jest wywoływana. W przeciwnym razie element do koszyka zakupów są sprawdzane pod kątem aktualizacji, gdy `UpdateItem` metoda jest wywoływana. Po element do koszyka zakupów została usunięta lub zaktualizowana, zmiany w bazie danych są zapisywane.

`ShoppingCartUpdates` Struktura jest używana do przechowywania wszystkich elementów koszyka zakupów. `UpdateShoppingCartDatabase` Metoda używa `ShoppingCartUpdates` struktury, aby określić, jeśli jakiekolwiek elementy muszą zostać zaktualizowane lub usunięte.

W następnym samouczku użyjesz `EmptyCart` metodę, aby wyczyścić służącej do obsługi koszyka po zakupieniu produktów. Ale na razie użyjesz `GetCount` metodę, która właśnie został dodany do *ShoppingCartActions.cs* plik, aby określić, ile elementów znajdują się w koszyku.

### <a name="adding-a-shopping-cart-counter"></a>Dodawanie licznik koszyka zakupów

Aby umożliwić użytkownikom przeglądanie całkowitą liczbę elementów w koszyku, dodasz licznik do *Site.Master* strony. Ten licznik będzie również działać jako link do koszyka.

1. W **Eksploratora rozwiązań**, otwórz *Site.Master* strony.
2. Zmodyfikuj znaczniki, dodając zakupów łącze licznika koszyka, jak pokazano w kolorze żółtym do sekcji nawigacji, więc pojawia się ono w następujący sposób:  

    [!code-html[Main](shopping-cart/samples/sample13.html?highlight=6)]
3. Następnie zaktualizuj związanym kodzie *Site.Master.cs* pliku, dodając kod wyróżniony na żółto w następujący sposób:  

    [!code-csharp[Main](shopping-cart/samples/sample14.cs?highlight=11,77-84)]

Przed wyświetleniem strony jako kod HTML, `Page_PreRender` zdarzenie jest wywoływane. W `Page_PreRender` obsługi, łączna liczba koszyka jest określana przez wywołanie metody `GetCount` metody. Zwrócona wartość jest dodawana do `cartCount` zakres uwzględniony w znaczniku elementu *Site.Master* strony. `<span>` Tagów umożliwia wewnętrzne elementy mógł być poprawnie renderowany. Po wyświetleniu dowolnej stronie witryny będą wyświetlane sumy koszyka zakupów. Użytkownik może także kliknąć łącznie koszyka zakupów, aby wyświetlić koszyk sklepowy.

## <a name="testing-the-completed-shopping-cart"></a>Testy zakończone koszyka

Można uruchomić aplikację teraz, aby zobaczyć sposób dodawania, usuwania i aktualizacji elementów w koszyku. Suma koszyka zakupów, zostanie naliczona łączny koszt wszystkich elementów w koszyku.

1. Naciśnij klawisz **F5** do uruchomienia aplikacji.  
 Przeglądarki otwiera się i pokazuje *Default.aspx* strony.
2. Wybierz **samochodów** z menu nawigacji kategorii.
3. Kliknij przycisk **Dodaj do koszyka** łącze obok pierwszego produktu.   
 *ShoppingCart.aspx* zostanie wyświetlona strona z sumy zamówienia.
4. Wybierz **płaszczyzn** z menu nawigacji kategorii.
5. Kliknij przycisk **Dodaj do koszyka** łącze obok pierwszego produktu.
6. Ustaw wartość pierwszego elementu w koszyku na 3 i wybierz **Usuń element** pole wyboru drugiego elementu.<a id="a"></a>
7. Kliknij przycisk **aktualizacji** przycisk, aby zaktualizować na stronie koszyka i wyświetlić nowe sumy zamówienia. 

    ![Koszyk sklepowy — aktualizacja koszyka](shopping-cart/_static/image9.png)

## <a name="summary"></a>Podsumowanie

W tym samouczku utworzono koszyka zakupów dla przykładowej aplikacji Wingtip Toys Web Forms. W tym samouczku użyto programu Entity Framework Code First, adnotacje danych na silnie typizowane kontrolki danych i tworzenia powiązania modelu.

Moduł koszyka zakupów obsługuje dodawanie, usuwanie i aktualizowanie elementów wybranych do zakupu. Oprócz wykonywania funkcji koszyka zakupów, wiesz sposób wyświetlania elementów koszyka zakupów **GridView** kontroli i obliczać sumy zamówienia.

## <a name="addition-information"></a>Informacje dodatkowe

[Przegląd stanu sesji platformy ASP.NET](https://msdn.microsoft.com/library/ms178581.aspx)

> [!div class="step-by-step"]
> [Poprzednie](display_data_items_and_details.md)
> [dalej](checkout-and-payment-with-paypal.md)
