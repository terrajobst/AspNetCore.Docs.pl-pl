---
uid: web-forms/overview/data-access/enhancing-the-gridview/adding-a-gridview-column-of-checkboxes-vb
title: Dodawanie kolumny pól wyboru (VB) do kontrolki GridView | Dokumentacja firmy Microsoft
author: rick-anderson
description: W tym samouczku pokazano, jak można dodać kolumny pól wyboru do kontrolki GridView, aby przyznać użytkownikowi intuicyjny sposób wybierania wielu wierszy G....
ms.author: riande
ms.date: 03/06/2007
ms.assetid: 39253d05-75c0-41c7-b9d4-a6c58ecf69ce
msc.legacyurl: /web-forms/overview/data-access/enhancing-the-gridview/adding-a-gridview-column-of-checkboxes-vb
msc.type: authoredcontent
ms.openlocfilehash: be7fc4fe93d15e7550f873adffa15e82384d25be
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/16/2018
ms.locfileid: "41755545"
---
<a name="adding-a-gridview-column-of-checkboxes-vb"></a>Dodawanie kolumny pól wyboru (VB) do kontrolki GridView
====================
przez [Bento Scott](https://twitter.com/ScottOnWriting)

[Pobierz przykładową aplikację](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_52_VB.exe) lub [Pobierz plik PDF](adding-a-gridview-column-of-checkboxes-vb/_static/datatutorial52vb1.pdf)

> W tym samouczku pokazano, jak można dodać kolumny pól wyboru do kontrolki widoku siatki, aby przyznać użytkownikowi intuicyjny sposób wybierania wielu wierszy GridView.


## <a name="introduction"></a>Wprowadzenie

W poprzednim samouczku zbadaliśmy sposób dodawania kolumny przycisków radiowych do kontrolki GridView na potrzeby wybranie określonego rekordu. Kolumny przycisków radiowych jest odpowiedni interfejs, gdy użytkownik jest ograniczona do wybierania co najwyżej jeden element z siatki. Czasami jednak warto umożliwić użytkownikowi wybrać dowolną liczbę elementów z siatki. Oparte na sieci Web klientów poczty e-mail, na przykład zazwyczaj wyświetlić listę komunikatów z kolumny pól wyboru. Użytkownik może wybrać dowolną liczbę komunikatów, a następnie wykonaj pewne akcje, takie jak przenoszenie wiadomości e-mail do innego folderu lub usuwając je.

W tym samouczku zobaczymy, jak dodawanie kolumny pól wyboru i jak określić, jakie pola wyboru zostały sprawdzone na zwrot. W szczególności utworzymy przykładowi, który ścisłe naśladowanie interfejsu użytkownika klienta e-mail opartego na sieci web. Naszym przykładzie będzie zawierać stronicowane GridView zawierającą listę produktów w `Products` tabeli bazy danych za pomocą pola wyboru w każdym wierszu (patrz rysunek 1). Usuń wybrane produkty po kliknięciu przycisku, spowoduje to usunięcie tych produktów, wybrane.


[![Każdy wiersz produktu zawiera pola wyboru](adding-a-gridview-column-of-checkboxes-vb/_static/image1.gif)](adding-a-gridview-column-of-checkboxes-vb/_static/image1.png)

**Rysunek 1**: każdy wiersz produktu zawiera pola wyboru ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](adding-a-gridview-column-of-checkboxes-vb/_static/image2.png))


## <a name="step-1-adding-a-paged-gridview-that-lists-product-information"></a>Krok 1: Dodawanie GridView stronicowane, zawierającego informacje o produkcie

Zanim będziemy zajmować Dodawanie kolumny pól wyboru, chętnie s pierwszy skupić się na wyświetlanie listy produktów w widoku GridView, która obsługuje stronicowanie. Zacznij od otwarcia `CheckBoxField.aspx` strony w `EnhancedGridView` folder i przeciągnij GridView z przybornika w projektancie, ustawiając jego `ID` do `Products`. Następnie wybierz powiązać widoku GridView nowe kontrolki ObjectDataSource, o nazwie `ProductsDataSource`. Konfigurowanie kontrolki ObjectDataSource używać `ProductsBLL` klasy wywoływania `GetProducts()` metodę, aby zwrócić dane. Ponieważ tego widoku GridView będzie tylko do odczytu, ustawić list rozwijanych w UPDATE, INSERT i usuwanie kart (Brak).


[![Tworzenie nowego elementu ObjectDataSource, o nazwie ProductsDataSource](adding-a-gridview-column-of-checkboxes-vb/_static/image2.gif)](adding-a-gridview-column-of-checkboxes-vb/_static/image3.png)

**Rysunek 2**: Utwórz nowy o nazwie elementu ObjectDataSource `ProductsDataSource` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](adding-a-gridview-column-of-checkboxes-vb/_static/image4.png))


[![Konfigurowanie kontrolki ObjectDataSource do pobierania danych przy użyciu metody GetProducts()](adding-a-gridview-column-of-checkboxes-vb/_static/image3.gif)](adding-a-gridview-column-of-checkboxes-vb/_static/image5.png)

**Rysunek 3**: Konfigurowanie kontrolki ObjectDataSource pobierania danych przy użyciu `GetProducts()` — metoda ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](adding-a-gridview-column-of-checkboxes-vb/_static/image6.png))


[![Ustaw list rozwijanych w UPDATE, INSERT i usuwanie kart (Brak)](adding-a-gridview-column-of-checkboxes-vb/_static/image4.gif)](adding-a-gridview-column-of-checkboxes-vb/_static/image7.png)

**Rysunek 4**: Ustaw listy rozwijane w aktualizacji, WSTAWIANIA i usuwania karty (Brak) ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](adding-a-gridview-column-of-checkboxes-vb/_static/image8.png))


Po zakończeniu pracy kreatora Konfigurowanie źródła danych, program Visual Studio automatycznie utworzy BoundColumns i CheckBoxColumn dla pól danych dotyczących produktu. Jak Przeprowadziliśmy w poprzednim samouczku, Usuń wszystkie elementy oprócz `ProductName`, `CategoryName`, i `UnitPrice` BoundFields i zmień `HeaderText` właściwości, aby produkt, kategoria i ceny. Konfigurowanie `UnitPrice` elementu BoundField tak, aby jego wartość jest formatowana jako walutę. Również skonfigurować GridView do obsługi stronicowania, zaznaczając pole wyboru Włącz stronicowania z tagu inteligentnego.

Pozwól s również dodać interfejsu użytkownika dla usuwania wybranych produktów. Dodawanie kontrolki przycisku w sieci Web pod GridView, ustawiając jego `ID` do `DeleteSelectedProducts` i jego `Text` właściwość, aby usunąć wybrane produkty. Zamiast faktycznego usuwania produktów z bazy danych, w tym przykładzie firma Microsoft będzie po prostu wyświetlony komunikat informujący o produktów, które będą usunięte. Aby to umożliwić, należy dodać formant etykiety w sieci Web, poniżej przycisku. Ustawić jej identyfikator na `DeleteResults`, wyczyść się jego `Text` właściwości, a następnie ustaw jego `Visible` i `EnableViewState` właściwości `False`.

Po wprowadzeniu tych zmian, GridView, ObjectDataSource, przycisk i etykietę s oznaczeniu deklaracyjnym powinien podobny do następującego:


[!code-aspx[Main](adding-a-gridview-column-of-checkboxes-vb/samples/sample1.aspx)]

Poświęć chwilę, aby wyświetlić stronę w przeglądarce (zobacz rysunek 5). W tym momencie powinien zostać wyświetlony, nazwa, kategoria i ceny produktów pierwszych dziesięciu.


[![Nazwa, kategoria i ceny produktów pierwsze dziesięć są wyświetlane.](adding-a-gridview-column-of-checkboxes-vb/_static/image5.gif)](adding-a-gridview-column-of-checkboxes-vb/_static/image9.png)

**Rysunek 5**: nazwa, kategoria i ceny produktów pierwsze dziesięć są wyświetlane ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](adding-a-gridview-column-of-checkboxes-vb/_static/image10.png))


## <a name="step-2-adding-a-column-of-checkboxes"></a>Krok 2: Dodawanie kolumny pól wyboru

Ponieważ ASP.NET 2.0 obejmuje CheckBoxField, jeden wydaje się, może służyć do dodawania kolumny pól wyboru w kontrolce GridView. Niestety, który nie jest tak, ponieważ CheckBoxField jest przeznaczona do pracy z polem danych logicznych. Oznacza to aby można było używać CheckBoxField możemy określić podstawowej pola danych, którego wartość konsultacji w celu określenia, czy renderowanych pole wyboru jest zaznaczone. Firma Microsoft nie można użyć CheckBoxField po prostu dołączyć kolumnę nie zaznaczone pola wyboru.

Zamiast tego, możemy dodać TemplateField i dodać kontrolkę pola wyboru w sieci Web do jej `ItemTemplate`. Przejdź dalej i Dodaj TemplateField do `Products` GridView, co pierwsze pole (lewej). Za pomocą tagu inteligentnego s GridView kliknij link Edytuj szablony, a następnie przeciągnij formant pola wyboru w sieci Web z przybornika do `ItemTemplate`. Ustaw to pole wyboru s `ID` właściwość `ProductSelector`.


[![Dodaj kontrolkę pola wyboru w sieci Web o nazwie ProductSelector do właściwości ItemTemplate TemplateField s](adding-a-gridview-column-of-checkboxes-vb/_static/image6.gif)](adding-a-gridview-column-of-checkboxes-vb/_static/image11.png)

**Rysunek 6**: Dodaj nazwę formantu sieci Web zaznacz pole wyboru `ProductSelector` s TemplateField `ItemTemplate` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](adding-a-gridview-column-of-checkboxes-vb/_static/image12.png))


Za pomocą kontrolki TemplateField i zaznacz pole wyboru w sieci Web dodany każdy wiersz zawiera teraz pola wyboru. Rysunek nr 7 przedstawia tę stronę, podczas wyświetlania za pośrednictwem przeglądarki, po dodaniu TemplateField i pól wyboru.


[![Każdy wiersz produktu zawiera teraz pola wyboru](adding-a-gridview-column-of-checkboxes-vb/_static/image7.gif)](adding-a-gridview-column-of-checkboxes-vb/_static/image13.png)

**Rysunek 7**: każdy wiersz produktu zawiera teraz pola wyboru ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](adding-a-gridview-column-of-checkboxes-vb/_static/image14.png))


## <a name="step-3-determining-what-checkboxes-were-checked-on-postback"></a>Krok 3: Określanie, jakie pola wyboru zostały sprawdzone na odświeżenie strony

W tym momencie mamy kolumny pól wyboru, ale nie sposobem ustalenia, jakie pola wyboru zostały sprawdzone na zwrot. Po kliknięciu przycisku Usuń wybrane produkty, musimy wiedzieć, jakie pola wyboru zostały zaewidencjonowane w celu usunięcia tych produktów.

GridView s [ `Rows` właściwość](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.rows.aspx) zapewnia dostęp do wierszy danych w widoku GridView. Firma Microsoft może wykonać iterację te wiersze programowego dostępu do kontrolki pola wyboru, a następnie zapoznaj się z jego `Checked` właściwości w celu określenia, czy pole wyboru zostało zaznaczone.

Utwórz procedurę obsługi zdarzeń dla `DeleteSelectedProducts` formant przycisku w sieci Web s `Click` zdarzeń i Dodaj następujący kod:


[!code-vb[Main](adding-a-gridview-column-of-checkboxes-vb/samples/sample2.vb)]

`Rows` Właściwość zwraca kolekcję `GridViewRow` wystąpień tego korzeń wiersze danych s GridView. `For Each` Pętli tutaj wylicza tej kolekcji. Dla każdego `GridViewRow` obiektu wiersz s pole wyboru jest programowo uzyskać dostęp przy użyciu `row.FindControl("controlID")`. Jeśli pole wyboru jest zaznaczone, odpowiedni wiersz s `ProductID` wartość jest pobierana z `DataKeys` kolekcji. W tym ćwiczeniu możemy po prostu wyświetlić komunikat informacyjny w `DeleteResults` etykiety, mimo że w działającej aplikacji możemy d zamiast wywoływania `ProductsBLL` klasy s `DeleteProduct(productID)` metody.

Wyświetla teraz kliknięcie przycisku Usuń wybrane produkty dodając ten program obsługi zdarzeń `ProductID` s wybranych produktów.


[![Po kliknięciu przycisku Usuń produktów wybrane ProductIDs wybrane produkty są wyświetlane](adding-a-gridview-column-of-checkboxes-vb/_static/image8.gif)](adding-a-gridview-column-of-checkboxes-vb/_static/image15.png)

**Rysunek 8**: podczas usuwania wybrane produkty przycisku wybrane produkty `ProductID` należą s ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](adding-a-gridview-column-of-checkboxes-vb/_static/image16.png))


## <a name="step-4-adding-check-all-and-uncheck-all-buttons"></a>Krok 4: Dodanie Zaznacz wszystko i usuń zaznaczenie wszystkich przycisków

Jeśli użytkownik chce, aby usunąć wszystkie produkty na bieżącej stronie, musisz sprawdzić każdy dziesięć pola wyboru. Możemy pomóc przyspieszyć ten proces, dodając wszystkie zaznacz przycisk, który, po kliknięciu zaznaczenie wszystkich pól wyboru w siatce. Usuń zaznaczenie wszystkich przycisk będzie równie przydatne.

Dodaj dwie kontrolki przycisku w sieci Web do strony, umieszczając je powyżej widoku GridView. Ustaw najpierw jeden s `ID` do `CheckAll` i jego `Text` właściwość, aby sprawdzić wszystkie; Ustaw drugi jeden s `ID` do `UncheckAll` i jego `Text` Usuń zaznaczenie wszystkich właściwości.


[!code-aspx[Main](adding-a-gridview-column-of-checkboxes-vb/samples/sample3.aspx)]

Następnie należy utworzyć metodę w klasie CodeBehind o nazwie `ToggleCheckState(checkState)` , po wywołaniu, wylicza `Products` GridView s `Rows` kolekcji i ustawia s każdego pola wyboru `Checked` właściwości do wartości przekazanych w *wartość checkState*  parametru.


[!code-vb[Main](adding-a-gridview-column-of-checkboxes-vb/samples/sample4.vb)]

Następnie należy utworzyć `Click` programy obsługi zdarzeń dla `CheckAll` i `UncheckAll` przycisków. W `CheckAll` s programu obsługi zdarzeń po prostu wywołanie `ToggleCheckState(True)`; w `UncheckAll`, wywołaj `ToggleCheckState(False)`.


[!code-vb[Main](adding-a-gridview-column-of-checkboxes-vb/samples/sample5.vb)]

Przy użyciu tego kodu klikając przycisk Zaznacz wszystko powoduje odświeżenie strony i sprawdza wszystkie pola wyboru w widoku GridView. Podobnie klikając polecenie Usuń zaznaczenie wszystkich usuwa wszystkie pola wyboru. Po sprawdzeniu przycisk Sprawdź wszystkie rysunku nr 9 przedstawiono ekranu.


[![Kliknięcie test, który przycisk wszystkich spowoduje zaznaczenie wszystkich pól wyboru](adding-a-gridview-column-of-checkboxes-vb/_static/image9.gif)](adding-a-gridview-column-of-checkboxes-vb/_static/image17.png)

**Rysunek 9**: kliknięcie przycisku Sprawdź wszystkie przycisk wybiera wszystkie pola wyboru ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](adding-a-gridview-column-of-checkboxes-vb/_static/image18.png))


> [!NOTE]
> Podczas wyświetlania kolumny pól wyboru, jednym z podejść zaznaczania lub unselecting wszystkie pola wyboru jest za pośrednictwem pola wyboru w wierszu nagłówka. Ponadto bieżące Zaznacz wszystko / Usuń zaznaczenie wszystkich wdrożenie wymaga ogłaszania zwrotnego. Zaznacz pola wyboru może być zaznaczony lub niezaznaczony, jednak całkowicie za pomocą skryptu po stronie klienta, zapewniając w ten sposób skrócenie środowisko użytkownika. Aby eksplorować wyboru wiersz nagłówka Zaznacz wszystko i usuń zaznaczenie wszystkich szczegółowo, wraz z dyskusję na temat korzystania z technik po stronie klienta, zapoznaj się z [sprawdzanie wszystkich pól wyboru skryptu po stronie klienta przy użyciu GridView i wszystkie pola wyboru Sprawdź](http://aspnet.4guysfromrolla.com/articles/053106-1.aspx).


## <a name="summary"></a>Podsumowanie

W przypadkach, w których należy powiadomić użytkowników, wybierz dowolną liczbę wierszy z GridView przed kontynuowaniem Dodawanie kolumny pól wyboru jest jedną z opcji. Jak widzieliśmy w ramach tego samouczka, w tym kolumny pól wyboru w widoku GridView pociąga za sobą Dodawanie TemplateField za pomocą kontrolki CheckBox w sieci Web. Za pomocą formantu sieci Web (a nie zostanie wstrzyknięta znaczników bezpośrednio w szablonie, ile My mieliśmy w poprzednim samouczku) ASP.NET pamięta automatycznie co pola wyboru były i nie zostały zaznaczone w zwrotu. Możemy też programowo uzyskać dostęp do pola wyboru w kodzie do określenia, czy zaznaczono opcję danego pola wyboru lub zmienić stan zaznaczenia.

W tym samouczku oraz ostatnią przyjrzano się dodanie selektora kolumnę do widoku GridView. W naszym następnym samouczku zajmiemy się, jak to zrobić, bit pracy, możemy dodać możliwości wstawianie do kontrolki GridView.

Wszystkiego najlepszego programowania!

## <a name="about-the-author"></a>Informacje o autorze

[Scott Bento](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor siedem ASP/ASP.NET książek i założycielem [4GuysFromRolla.com](http://www.4guysfromrolla.com), pracował nad przy użyciu technologii Microsoft Web od 1998 r. Scott działa jako niezależny Konsultant, trainer i składnika zapisywania. Jego najnowszą książkę Stephena [ *Sams uczyć się ASP.NET 2.0 w ciągu 24 godzin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). ADAM można z Tobą skontaktować w [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) lub za pośrednictwem jego blogu, który znajduje się w temacie [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

> [!div class="step-by-step"]
> [Poprzednie](adding-a-gridview-column-of-radio-buttons-vb.md)
> [dalej](inserting-a-new-record-from-the-gridview-s-footer-vb.md)
