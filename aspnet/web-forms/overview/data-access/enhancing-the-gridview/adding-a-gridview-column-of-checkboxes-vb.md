---
uid: web-forms/overview/data-access/enhancing-the-gridview/adding-a-gridview-column-of-checkboxes-vb
title: Dodawanie kolumny widoku GridView pola wyboru (VB) | Dokumentacja firmy Microsoft
author: rick-anderson
description: "W tym samouczku analizuje jak dodać kolumnę pola wyboru do kontrolki widoku siatki, aby przyznać użytkownikowi intuicyjny sposób wybierania wielu wierszy G..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/06/2007
ms.topic: article
ms.assetid: 39253d05-75c0-41c7-b9d4-a6c58ecf69ce
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/enhancing-the-gridview/adding-a-gridview-column-of-checkboxes-vb
msc.type: authoredcontent
ms.openlocfilehash: 326201f9fe9ba5f482308dc8bfd7d2decb9fbd8f
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
<a name="adding-a-gridview-column-of-checkboxes-vb"></a>Dodawanie kolumny widoku GridView pola wyboru (VB)
====================
przez [Bento Scott](https://twitter.com/ScottOnWriting)

[Pobierz przykładową aplikację](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_52_VB.exe) lub [pobierania plików PDF](adding-a-gridview-column-of-checkboxes-vb/_static/datatutorial52vb1.pdf)

> W tym samouczku analizuje jak dodać kolumnę pola wyboru do kontrolki widoku siatki, aby przyznać użytkownikowi intuicyjny sposób wybierania wielu wierszy w widoku GridView.


## <a name="introduction"></a>Wprowadzenie

W poprzednim samouczek możemy zbadać jak dodać kolumnę przycisków radiowych do na potrzeby wybranie określonego rekordu w widoku GridView. Kolumnę przycisków radiowych jest interfejsem odpowiedniego użytkownika, gdy użytkownik jest ograniczona do wybierania co najwyżej jeden element z siatki. Czasami jednak może chcemy umożliwia użytkownikowi pobranie dowolnej liczby elementów siatki. Klienci sieci Web poczty e-mail, na przykład wyświetlić zwykle listę komunikatów z kolumną pola wyboru. Użytkownik może wybrać dowolną liczbę wiadomości i następnie wykonanie akcji, takich jak przenoszenia wiadomości e-mail do innego folderu lub usuwając je.

W tym samouczku zostanie wyświetlone jak dodać kolumnę pola wyboru oraz sposobu określania, jakie pola wyboru zostały zaznaczone na ogłaszania zwrotnego. W szczególności firma Microsoft będzie kompilacji przykładem ściśle naśladuje interfejsu użytkownika klienta opartego na sieci web. Zawiera element GridView stronicowanych listę produktów w naszym przykładzie `Products` tabeli bazy danych z wyboru w każdym wierszu (zobacz rysunek 1). Usuń wybrane produkty po kliknięciu przycisku, spowoduje usunięcie tych produktów wybranych.


[![Każdy wiersz produktu zawiera pole wyboru](adding-a-gridview-column-of-checkboxes-vb/_static/image1.gif)](adding-a-gridview-column-of-checkboxes-vb/_static/image1.png)

**Rysunek 1**: każdy wiersz produktu zawiera pole wyboru ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](adding-a-gridview-column-of-checkboxes-vb/_static/image2.png))


## <a name="step-1-adding-a-paged-gridview-that-lists-product-information"></a>Krok 1: Dodawanie stronicowanych widoku GridView, która zawiera informacje o produkcie

Zanim firma Microsoft martwić się o dodanie kolumny pola wyboru, umożliwiają s pierwszy fokus na wyświetlanie listy produktów w widoku GridView, który obsługuje stronicowania. Uruchamianie przez otwarcie `CheckBoxField.aspx` strony `EnhancedGridView` folder i przeciągnij element GridView z przybornika do projektanta, ustawienie jej `ID` do `Products`. Następnie wybierz powiązać widoku GridView nowy element ObjectDataSource o nazwie `ProductsDataSource`. Element ObjectDataSource umożliwia konfigurowanie `ProductsBLL` klasy wywoływania `GetProducts()` metody, aby zwrócić dane. Ponieważ tego widoku GridView będzie tylko do odczytu, ustaw listy rozwijane w UPDATE, INSERT i usuwanie kart na (Brak).


[![Utwórz nowy element ObjectDataSource o nazwie ProductsDataSource](adding-a-gridview-column-of-checkboxes-vb/_static/image2.gif)](adding-a-gridview-column-of-checkboxes-vb/_static/image3.png)

**Rysunek 2**: Utwórz nowy składnik o nazwie ObjectDataSource `ProductsDataSource` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](adding-a-gridview-column-of-checkboxes-vb/_static/image4.png))


[![Skonfiguruj ObjectDataSource do pobierania danych przy użyciu metody GetProducts()](adding-a-gridview-column-of-checkboxes-vb/_static/image3.gif)](adding-a-gridview-column-of-checkboxes-vb/_static/image5.png)

**Rysunek 3**: Konfigurowanie ObjectDataSource do pobierania danych przy użyciu `GetProducts()` — metoda ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](adding-a-gridview-column-of-checkboxes-vb/_static/image6.png))


[![Ustawianie list rozwijanych w UPDATE, INSERT i usuwanie kart na (Brak)](adding-a-gridview-column-of-checkboxes-vb/_static/image4.gif)](adding-a-gridview-column-of-checkboxes-vb/_static/image7.png)

**Rysunek 4**: wartość listy rozwijane w AKTUALIZOWANIA, WSTAWIANIA i usuwanie kart (None) ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](adding-a-gridview-column-of-checkboxes-vb/_static/image8.png))


Po zakończeniu pracy Kreatora konfigurowania źródła danych programu Visual Studio automatycznie utworzy BoundColumns i CheckBoxColumn dla pól danych dotyczących produktu. Jak opisano w poprzedniej samouczek, Usuń wszystkie elementy oprócz `ProductName`, `CategoryName`, i `UnitPrice` BoundFields i zmień `HeaderText` właściwości do produktu, kategorii i cenę. Skonfiguruj `UnitPrice` elementu BoundField tak, aby jego wartość jest w formacie waluty. Również skonfigurować do obsługi stronicowania, zaznaczając pole wyboru Włącz stronicowania z tagów inteligentnych widoku GridView.

Umożliwiają również dodawanie interfejsu użytkownika dla usuwania zaznaczonych produktów s. Dodawanie formantu przycisku Web poniżej widoku GridView ustawienie jej `ID` do `DeleteSelectedProducts` i jego `Text` właściwość, aby usunąć wybrane produkty. Zamiast faktycznie ich nie usuwać produkty z bazy danych, w tym przykładzie firma Microsoft będzie po prostu Wyświetl komunikat z informacją o produktów, które będą usunięte. Aby to umożliwić, Dodaj formant etykiety w sieci Web, poniżej przycisku. Ustawić jej identyfikator na `DeleteResults`, wyczyść się jego `Text` właściwości i ustaw jego `Visible` i `EnableViewState` właściwości `False`.

Po wprowadzeniu tych zmian, znaczników deklaratywne s GridView, ObjectDataSource przycisk i etykiety powinna podobny do następującego:


[!code-aspx[Main](adding-a-gridview-column-of-checkboxes-vb/samples/sample1.aspx)]

Poświęć chwilę, aby wyświetlić stronę w przeglądarce (patrz rysunek 5). W tym momencie powinny pojawić się nazwa, kategoria i ceny produktów pierwszych dziesięciu.


[![Wyświetlane są nazwa, kategoria i cen pierwszy 10 produktów](adding-a-gridview-column-of-checkboxes-vb/_static/image5.gif)](adding-a-gridview-column-of-checkboxes-vb/_static/image9.png)

**Rysunek 5**: Name, kategorii i cen pierwszy 10 produktów są wyświetlane ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](adding-a-gridview-column-of-checkboxes-vb/_static/image10.png))


## <a name="step-2-adding-a-column-of-checkboxes"></a>Krok 2: Dodawanie kolumny pola wyboru

Ponieważ ASP.NET 2.0 obejmuje CheckBoxField, jeden może wziąć pod uwagę że może służyć do dodać kolumnę pola wyboru do widoku GridView. Niestety, który nie jest to, jak w polu CheckBoxField jest przeznaczona do pracy z polem danych logicznych. Oznacza to aby użyć polu CheckBoxField możemy określić podstawowej pola danych, którego wartość jest konsultacji ustalenie, czy zaznaczono pole wyboru renderowany. Nie można użyć polu CheckBoxField uwzględnienie tylko kolumnę usunięciu zaznaczenia pola wyboru.

Zamiast tego, możemy dodać pole TemplateField i dodać kontrolkę pola wyboru sieci Web do jego `ItemTemplate`. Przejdź dalej i Dodaj pole TemplateField do `Products` GridView i przekształcenie go w pierwszym polu (daleko od lewej). Z widoku GridView s tagu, kliknij łącze Edytuj szablony, a następnie przeciągnij formant wyboru sieci Web z przybornika do `ItemTemplate`. Ustaw to pole wyboru s `ID` właściwości `ProductSelector`.


[![Dodaj formant wyboru sieci Web o nazwie ProductSelector do TemplateField s ItemTemplate](adding-a-gridview-column-of-checkboxes-vb/_static/image6.gif)](adding-a-gridview-column-of-checkboxes-vb/_static/image11.png)

**Rysunek 6**: Dodaj pola wyboru sieci Web formantu o nazwie `ProductSelector` s TemplateField `ItemTemplate` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](adding-a-gridview-column-of-checkboxes-vb/_static/image12.png))


Z formantem TemplateField i pole wyboru w sieci Web dodany każdy wiersz zawiera pole wyboru. Rysunek nr 7 przedstawia tę stronę, podczas wyświetlania za pośrednictwem przeglądarki, po dodaniu TemplateField i pól wyboru.


[![Każdy wiersz produktu zawiera teraz Checkbox](adding-a-gridview-column-of-checkboxes-vb/_static/image7.gif)](adding-a-gridview-column-of-checkboxes-vb/_static/image13.png)

**Rysunek 7**: każdy wiersz produktu zawiera teraz Checkbox ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](adding-a-gridview-column-of-checkboxes-vb/_static/image14.png))


## <a name="step-3-determining-what-checkboxes-were-checked-on-postback"></a>Krok 3: Określanie, jakie pola wyboru zostały sprawdzone odświeżania strony

W tym momencie mamy kolumny pola wyboru, ale nie można określić, jakie pola wyboru zostały zaznaczone na ogłaszania zwrotnego. Po kliknięciu przycisku Usuń wybrane produkty, musimy wiedzieć, jakie pola wyboru zostały zaznaczone w celu usunięcia tych produktów.

GridView s [ `Rows` właściwość](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.gridview.rows.aspx) zapewnia dostęp do wierszy danych w widoku GridView. Firma Microsoft iterację te wiersze uzyskania programowego dostępu do formantu wyboru, a następnie zapoznaj się jego `Checked` właściwości w celu określenia, czy pole wyboru zostało zaznaczone.

Tworzenie procedury obsługi zdarzeń dla `DeleteSelectedProducts` kontrolki przycisku w sieci Web s `Click` zdarzeń i Dodaj następujący kod:


[!code-vb[Main](adding-a-gridview-column-of-checkboxes-vb/samples/sample2.vb)]

`Rows` Właściwość zwraca kolekcję `GridViewRow` wystąpień w skład tego wiersze danych widoku GridView s. `For Each` Pętli tutaj wylicza tej kolekcji. Dla każdego `GridViewRow` obiekt wiersza s wyboru programowo jest dostępny przy użyciu `row.FindControl("controlID")`. Jeśli pole wyboru jest zaznaczone, odpowiedni wiersz s `ProductID` wartość jest pobierana z `DataKeys` kolekcji. W tym ćwiczeniu, po prostu wyświetli komunikat informacyjny w `DeleteResults` etykietę, mimo że w działającą aplikację możemy d zamiast wywoływania `ProductsBLL` klasy s `DeleteProduct(productID)` metody.

Dodając ten program obsługi zdarzeń, kliknięcie przycisku Usuń wybrane produkty teraz wyświetla `ProductID` s zaznaczonych produktów.


[![Po kliknięciu przycisku produktów usunąć wybrane ProductIDs wybrane produkty są wyświetlane](adding-a-gridview-column-of-checkboxes-vb/_static/image8.gif)](adding-a-gridview-column-of-checkboxes-vb/_static/image15.png)

**Rysunek 8**: gdy usunąć wybrane produkty przycisku wybrane produkty `ProductID` s są wyświetlane ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](adding-a-gridview-column-of-checkboxes-vb/_static/image16.png))


## <a name="step-4-adding-check-all-and-uncheck-all-buttons"></a>Krok 4: Dodawanie zaznacz wszystkie i usuń zaznaczenie wszystkich przycisków

Jeśli użytkownik chce, aby usunąć wszystkie produkty na bieżącej stronie, musi sprawdzić każdy dziesięć pól wyboru. Możemy pomóc przyspieszyć ten proces przez dodanie wszystkich Sprawdź przycisku, po kliknięciu zaznaczenie wszystkich pól wyboru w siatce. Przycisk Usuń zaznaczenie wszystkich będą pomocne w równym stopniu.

Dodaj dwa kontrolki przycisku w sieci Web ze stroną, umieszczenia ich powyżej widoku GridView. Ustaw najpierw jeden s `ID` do `CheckAll` i jego `Text` właściwość, aby sprawdzić wszystkie; Ustaw drugi jeden s `ID` do `UncheckAll` i jego `Text` Usuń zaznaczenie wszystkich właściwości.


[!code-aspx[Main](adding-a-gridview-column-of-checkboxes-vb/samples/sample3.aspx)]

Następnie należy utworzyć metody w klasie związanej z kodem o nazwie `ToggleCheckState(checkState)` , gdy została wywołana, wylicza `Products` s widoku GridView `Rows` kolekcji i ustawia s każdego wyboru `Checked` właściwości do wartości przekazanych w *checkState*  parametru.


[!code-vb[Main](adding-a-gridview-column-of-checkboxes-vb/samples/sample4.vb)]

Następnie należy utworzyć `Click` obsługi zdarzeń `CheckAll` i `UncheckAll` przycisków. W `CheckAll` s obsługi zdarzeń, po prostu wywołanie `ToggleCheckState(True)`; na liście `UncheckAll`, wywołaj `ToggleCheckState(False)`.


[!code-vb[Main](adding-a-gridview-column-of-checkboxes-vb/samples/sample5.vb)]

Z tego kodu kliknięcie przycisku Sprawdź wszystkie powoduje odświeżenie strony i sprawdza wszystkie pola wyboru w widoku GridView. Podobnie klikając polecenie Usuń zaznaczenie wszystkich usuwa wszystkie pola wyboru. Po sprawdzeniu przycisk Sprawdź wszystkie rysunku nr 9 przedstawiono ekranu.


[![Kliknij przycisk wszystkie sprawdzenia zaznaczyć wszystkie pola wyboru](adding-a-gridview-column-of-checkboxes-vb/_static/image9.gif)](adding-a-gridview-column-of-checkboxes-vb/_static/image17.png)

**Rysunek 9**: kliknięcie przycisku Sprawdź wszystkie przycisk wybiera wszystkie pola wyboru ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](adding-a-gridview-column-of-checkboxes-vb/_static/image18.png))


> [!NOTE]
> Podczas wyświetlania kolumny pola wyboru, jednym z podejść zaznaczania lub unselecting wszystkie pola wyboru jest pole wyboru w wierszu nagłówka. Ponadto bieżącego zaznacz wszystkie / Usuń zaznaczenie wszystkich wdrożenie wymaga odświeżania strony. Pola wyboru można zaznaczać lub usuwać zaznaczenia, jednak całkowicie za pomocą skryptu po stronie klienta, zapewniając snappier środowisko użytkownika. Aby zapoznać się z za pomocą wyboru wiersz nagłówka dla Zaznacz wszystko i usuń zaznaczenie wszystkich szczegółowo, wraz z dyskusji przy użyciu technik po stronie klienta, zapoznaj się [sprawdzanie wszystkie pola wyboru w widoku GridView skryptu po stronie klienta przy użyciu i wszystkie pola wyboru Sprawdź](http://aspnet.4guysfromrolla.com/articles/053106-1.aspx).


## <a name="summary"></a>Podsumowanie

W przypadkach, w których należy powiadomić użytkowników, wybierz dowolną liczbę wierszy w widoku GridView przed kontynuowaniem Dodawanie kolumny pola wyboru jest jedną z opcji. Jak widzieliśmy w tym samouczku, włącznie z kolumną zawierającą pola wyboru w widoku GridView pociąga za sobą Dodawanie TemplateField za pomocą formantu CheckBox w sieci Web. Za pomocą formantu sieci Web (w przeciwieństwie wstrzyknięcie znaczników bezpośrednio do szablonu, jak robiliśmy w poprzednich instrukcji) ASP.NET pamięta automatycznie co pola wyboru zostały i nie zostały zaznaczone w ogłaszania zwrotnego. Firma Microsoft może również uzyskania programowego dostępu do pola wyboru w kodzie Aby ustalić, czy zaznaczono pole wyboru danego lub zmienić stan zaznaczenia.

W tym samouczku i ostatnią przeglądał dodanie selektora kolumnę do widoku GridView. W naszym samouczku dalej zajmiemy się, jak to zrobić, z bitowego pracy, można dodać możliwości wstawianie do widoku GridView.

Programowanie przyjemność!

## <a name="about-the-author"></a>Informacje o autorze

[Scott Bento](http://www.4guysfromrolla.com/ScottMitchell.shtml), autora siedmiu książek ASP/ASP.NET i twórcę z [4GuysFromRolla.com](http://www.4guysfromrolla.com), pracuje z technologii Microsoft Web od 1998. Scott działa jako niezależnego konsultanta trainer i składnika zapisywania. Jest jego najnowszej książki [ *Sams nauczyć się ASP.NET 2.0 w ciągu 24 godzin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Piotr można uzyskać pod adresem [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) lub za pośrednictwem jego blog, który znajduje się w temacie [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

>[!div class="step-by-step"]
[Poprzednie](adding-a-gridview-column-of-radio-buttons-vb.md)
[dalej](inserting-a-new-record-from-the-gridview-s-footer-vb.md)
