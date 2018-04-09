---
uid: web-forms/overview/data-access/custom-formatting/using-templatefields-in-the-detailsview-control-vb
title: Używanie TemplateFields w kontrolce widoku DetailsView (VB) | Dokumentacja firmy Microsoft
author: rick-anderson
description: Te same możliwości TemplateFields dostępne w widoku GridView są również dostępne za pomocą formantu widoku DetailsView. W tym samouczku będziesz wyświetli jeden produkt...
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/31/2010
ms.topic: article
ms.assetid: 0b91d5f8-127d-4f6a-b204-f2e2b35ef703
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/custom-formatting/using-templatefields-in-the-detailsview-control-vb
msc.type: authoredcontent
ms.openlocfilehash: 67009460477dcc3d1e966220b446a47d6e5b6f5a
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
---
<a name="using-templatefields-in-the-detailsview-control-vb"></a>Używanie TemplateFields w kontrolce widoku DetailsView (VB)
====================
przez [Bento Scott](https://twitter.com/ScottOnWriting)

[Pobierz przykładową aplikację](http://download.microsoft.com/download/5/7/0/57084608-dfb3-4781-991c-407d086e2adc/ASPNET_Data_Tutorial_13_VB.exe) lub [pobierania plików PDF](using-templatefields-in-the-detailsview-control-vb/_static/datatutorial13vb1.pdf)

> Te same możliwości TemplateFields dostępne w widoku GridView są również dostępne za pomocą formantu widoku DetailsView. W tym samouczku będziesz wyświetli jeden produkt za pomocą widoku DetailsView zawierające TemplateFields.


## <a name="introduction"></a>Wprowadzenie

TemplateField oferuje wyższego poziomu bezpieczeństwa i elastyczność podczas renderowania danych niż elementu BoundField, CheckBoxField pole hiperłącza HyperLinkField i inne formanty pola danych. W [poprzedniego samouczek](using-templatefields-in-the-gridview-control-vb.md) analizujemy przy użyciu TemplateField w widoku GridView do:

- Wyświetlać wiele wartości pola danych w jednej kolumnie. W szczególności zarówno `FirstName` i `LastName` pola zostały połączone w jedną kolumnę do widoku GridView.
- Za pomocą formantu sieci Web alternatywny wyrazić wartości pola danych. Widzieliśmy sposób wyświetlania `HiredDate` wartość za pomocą formantu kalendarza.
- Pokaż informacje o stanie oparte na danych. Gdy `Employees` tabela nie zawiera kolumny, która zwraca liczbę dni została pracownika w zadaniu, udało się wyświetlić takie informacje w przykładzie widoku GridView poprzedniej samouczka z użyciem TemplateField i metodę formatowania.

Te same możliwości TemplateFields dostępne w widoku GridView są również dostępne za pomocą formantu widoku DetailsView. W tym samouczku będziesz wyświetli jeden produkt za pomocą widoku DetailsView, który zawiera dwa TemplateFields. Pierwszy TemplateField zostaną połączone `UnitPrice`, `UnitsInStock`, i `UnitsOnOrder` pola danych w jednym wierszu widoku DetailsView. Drugi TemplateField zostanie wyświetlona wartość `Discontinued` pól, ale użyje metodę formatowania do wyświetlenia "Tak" Jeśli `Discontinued` jest `True`i "NO" inaczej.


[![Dwa TemplateFields są używane, aby dostosować wyświetlanie](using-templatefields-in-the-detailsview-control-vb/_static/image2.png)](using-templatefields-in-the-detailsview-control-vb/_static/image1.png)

**Rysunek 1**: dwa TemplateFields są używane, aby dostosować wyświetlanie ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](using-templatefields-in-the-detailsview-control-vb/_static/image3.png))


Dzieła!

## <a name="step-1-binding-the-data-to-the-detailsview"></a>Krok 1: Wiązanie danych w widoku DetailsView

Zgodnie z opisem w poprzedniej samouczek, podczas pracy z TemplateFields często najłatwiej Rozpocznij od utworzenia formantu widoku DetailsView, który zawiera tylko BoundFields, a następnie dodaj nowe TemplateFields lub przekonwertować istniejącej BoundFields TemplateFields jako jest wymagane . W związku z tym uruchomić tego samouczka, dodając element DetailsView do strony za pomocą projektanta i powiązania jej z ObjectDataSource, które zwraca listę produktów. Te czynności spowoduje utworzenie Element DetailsView z BoundFields dla każdego pola wartości inne niż logiczne produktu i CheckBoxField w jednym polu wartość logiczna (wycofany).

Otwórz `DetailsViewTemplateField.aspx` strony i przeciągnij element DetailsView z przybornika do projektanta. Z tagów inteligentnych DetailsView możliwość dodania nowej kontrolki ObjectDataSource, który wywołuje `ProductsBLL` klasy `GetProducts()` metody.


[![Dodaj nowe kontrolki ObjectDataSource, która wywołuje metodę GetProducts()](using-templatefields-in-the-detailsview-control-vb/_static/image5.png)](using-templatefields-in-the-detailsview-control-vb/_static/image4.png)

**Rysunek 2**: Dodaj nowe kontrolki ObjectDataSource tego wywołuje `GetProducts()` — metoda ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](using-templatefields-in-the-detailsview-control-vb/_static/image6.png))


Ten raport można usunąć `ProductID`, `SupplierID`, `CategoryID`, i `ReorderLevel` BoundFields. Następnie należy zmienić kolejność BoundFields, aby `CategoryName` i `SupplierName` BoundFields pojawi się natychmiast po `ProductName` elementu BoundField. Możesz dostosować `HeaderText` potrzeb i właściwości formatowania dla BoundFields jako użytkownik. Podobnie jak z widoku GridView, te zmiany poziomu elementu BoundField można wykonać za pomocą okna dialogowego pola (dostępne, klikając łącze Edytuj pola w widoku DetailsView tagów inteligentnych) lub za pomocą składni deklaratywnej. Ponadto wyczyszczenie DetailsView `Height` i `Width` wartości właściwości, aby umożliwić widoku DetailsView kontroli rozszerzenia na podstawie danych wyświetlane i zaznacz pole wyboru Włącz stronicowanie w tagu.

Po wprowadzeniu tych zmian, znaczników deklaratywne formantu widoku DetailsView powinien wyglądać podobnie do poniższej:


[!code-aspx[Main](using-templatefields-in-the-detailsview-control-vb/samples/sample1.aspx)]

Poświęć chwilę, aby wyświetlić stronę za pośrednictwem przeglądarki. W tym momencie powinny pojawić się jeden produkt wymienione (Chai) z wierszami przedstawiający nazwę produktu, Kategoria dostawcy, cen, jednostki w magazynie, jednostki w kolejności i jego stan wycofane.


[![Są wyświetlane szczegóły tego produktu, używając szeregu BoundFields](using-templatefields-in-the-detailsview-control-vb/_static/image8.png)](using-templatefields-in-the-detailsview-control-vb/_static/image7.png)

**Rysunek 3**: szczegółowe informacje są wyświetlane przy użyciu serii BoundFields ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](using-templatefields-in-the-detailsview-control-vb/_static/image9.png))


## <a name="step-2-combining-the-price-units-in-stock-and-units-on-order-into-one-row"></a>Krok 2: Łączenie cen, jednostki w magazynie i jednostki w kolejności w jeden wiersz

Widoku DetailsView zawiera wiersz dla `UnitPrice`, `UnitsInStock`, i `UnitsOnOrder` pól. Firma Microsoft można połączyć tych pól danych w pojedynczy wiersz z TemplateField, dodając nowe pole TemplateField lub konwertując jeden z istniejących `UnitPrice`, `UnitsInStock`, i `UnitsOnOrder` BoundFields na pole TemplateField. Gdy wolę osobiście Konwertowanie istniejących BoundFields, umożliwia rozwiązaniem, dodając nowe pole TemplateField.

Uruchomić, klikając łącze Edytuj pola w widoku DetailsView tagów inteligentnych można wyświetlić okno dialogowe pola. Następnie dodaj nowe pole TemplateField i ustawić jej `HeaderText` właściwości "Cena i spisu" i Przenieś nowe TemplateField, dzięki czemu jest umieszczana powyżej `UnitPrice` elementu BoundField.


[![Dodaj nowe pole TemplateField do formantu widoku DetailsView](using-templatefields-in-the-detailsview-control-vb/_static/image11.png)](using-templatefields-in-the-detailsview-control-vb/_static/image10.png)

**Rysunek 4**: Dodaj nowe pole TemplateField do formantu widoku DetailsView ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](using-templatefields-in-the-detailsview-control-vb/_static/image12.png))


Ponieważ ten nowy TemplateField będzie zawierać wartości aktualnie wyświetlany w `UnitPrice`, `UnitsInStock`, i `UnitsOnOrder` BoundFields, umożliwia Usuń je.

Ostatnie zadanie dla tego kroku jest określenie `ItemTemplate` znaczników ceny i TemplateField spisu, który może być realizowane za pośrednictwem szablonu DetailsView edycji interfejsu w Projektancie lub ręcznie za pomocą składni deklaratywnej formantu. Podobnie jak w przypadku widoku GridView, interfejs edytowania szablonu DetailsView jest możliwy, klikając łącze Edytuj szablony w tagu. W tym miejscu można wybrać szablon do edycji z listy rozwijanej, a następnie dodaj wszystkie formanty sieci Web z przybornika.

W tym samouczku, Rozpocznij od dodania formantu etykiety ceny i spisu TemplateField `ItemTemplate`. Następnie kliknij łącze Edytuj powiązania DataBindings z tagu formantu etykiety w sieci Web i powiązać `Text` właściwości `UnitPrice` pola.


[![Właściwość tekstu etykiety należy powiązać pole UnitPrice danych](using-templatefields-in-the-detailsview-control-vb/_static/image14.png)](using-templatefields-in-the-detailsview-control-vb/_static/image13.png)

**Rysunek 5**: powiązać etykiety `Text` właściwości `UnitPrice` pola danych ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](using-templatefields-in-the-detailsview-control-vb/_static/image15.png))


## <a name="formatting-the-price-as-a-currency"></a>Formatowanie cena jako walutę

Z tym dodatkiem Web etykiety kontroli ceny i TemplateField spisu wyświetli tylko cen dla wybranej produktu. Rysunek 6 przedstawia zrzut ekranu: naszych postępu dotychczasowych widzianego za pośrednictwem przeglądarki.


[![Ceny i TemplateField magazynu zawiera cenę](using-templatefields-in-the-detailsview-control-vb/_static/image17.png)](using-templatefields-in-the-detailsview-control-vb/_static/image16.png)

**Rysunek 6**: ceny i TemplateField magazynu zawiera cenę ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](using-templatefields-in-the-detailsview-control-vb/_static/image18.png))


Należy pamiętać, ceny produktu nie jest sformatowany jako walutę. Z elementu BoundField formatowania jest możliwe przez ustawienie `HtmlEncode` właściwości `False` i `DataFormatString` właściwości `{0:formatSpecifier}`. Na pole TemplateField jednak żadnych instrukcji formatowania należy określić w składni wiązania z danymi lub za pomocą metody formatowania zdefiniowanych w innym kodem aplikacji (na przykład w klasie związanej z kodem strony ASP.NET).

Aby określić formatowanie składnia wiązania danych używanych w sieci Web etykiety, wróć do okna dialogowego powiązania danych, klikając łącze Edytuj powiązania DataBindings z tagów inteligentnych etykiet. Można wpisać instrukcje formatowania bezpośrednio z listy rozwijanej Format lub wybierz jeden z ciągów zdefiniowany format. Z elementu BoundField, takich jak `DataFormatString` właściwości, formatowanie zostanie określona przy użyciu `{0:formatSpecifier}`.

Aby uzyskać `UnitPrice` pola użytkowania formatowanie waluty określonych, wybierając odpowiednie listy rozwijanej lub wpisując w `{0:C}` ręcznie.


[![Cena w formacie waluty](using-templatefields-in-the-detailsview-control-vb/_static/image20.png)](using-templatefields-in-the-detailsview-control-vb/_static/image19.png)

**Rysunek 7**: sformatować cena jako walutę ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](using-templatefields-in-the-detailsview-control-vb/_static/image21.png))


Deklaratywnie, formatowania specyfikacji jest oznaczony jako drugi parametr do `Bind` lub `Eval` metody. Ustawienia tylko za pośrednictwem wyników projektanta w następującym wyrażeniem wiązania z danymi w znaczniku deklaratywne:


[!code-aspx[Main](using-templatefields-in-the-detailsview-control-vb/samples/sample2.aspx)]

## <a name="adding-the-remaining-data-fields-to-the-templatefield"></a>Dodawanie pozostałych pól danych do TemplateField

W tym momencie wprowadzeniu wyświetlane i sformatowany `UnitPrice` danych ceny i spisu TemplateField, ale nadal konieczne jest wyświetlenie `UnitsInStock` i `UnitsOnOrder` pól. Umożliwia wyświetlanie je na wiersz poniżej ceny i w nawiasach. Z szablonu interfejsu edycji w Projektancie można dodać takie znaczników umieszczając kursor w szablonie i po prostu wpisywanie tekstu do wyświetlenia. Alternatywnie można wpisać bezpośrednio w składni deklaratywnej tego znacznika.

Dodawanie znaczników statyczne formantów sieci Web etykiety i składnia wiązania z danymi, aby ceny i TemplateField magazynu zawiera informacje dotyczące cen i spisu w następujący sposób:

*UnitPrice*  
(**w magazynie / w kolejności:** *StanMagazynu* / *JednostkiZamówione*)

Po wykonaniu tego zadania znaczników deklaratywne z widoku DetailsView powinien wyglądać podobnie do poniższej:


[!code-aspx[Main](using-templatefields-in-the-detailsview-control-vb/samples/sample3.aspx)]

Wprowadzone zmiany zostały możemy skonsolidowane informacje dotyczące cen i spisu w pojedynczy wiersz widoku DetailsView.


[![Ceny i informacje spisu są wyświetlane w pojedynczy wiersz tabeli](using-templatefields-in-the-detailsview-control-vb/_static/image23.png)](using-templatefields-in-the-detailsview-control-vb/_static/image22.png)

**Rysunek 8**: ceny i informacje spisu są wyświetlane w pojedynczy wiersz tabeli ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](using-templatefields-in-the-detailsview-control-vb/_static/image24.png))


## <a name="step-3-customizing-the-discontinued-field-information"></a>Krok 3: Dostosowywanie wycofane pola

`Products` Tabeli `Discontinued` kolumna jest wartość bitowego, która wskazuje, czy produkt nie jest obsługiwany. Podczas tworzenia wiązania widoku DetailsView (lub widoku GridView) do kontroli źródła danych, takich jak pola wartość logiczna `Discontinued`, są zaimplementowane jako CheckBoxFields, pola wartości inne niż logiczne, jak `ProductID`, `ProductName`i tak dalej są zaimplementowane jako BoundFields. Renderuje polu CheckBoxField jako wyłączone pole wyboru jest zaznaczone, jeśli wartość w polu danych ma wartość True i niezaznaczone inaczej.

Zamiast wyświetlania polu CheckBoxField firma Microsoft może być wyświetlany tekst wskazującą, czy produkt jest już obsługiwana. W tym celu firma Microsoft może usunąć polu CheckBoxField z widoku DetailsView a następnie dodaj elementu BoundField których `DataField` ustawiono właściwość `Discontinued`. Poświęć chwilę, aby to zrobić. Po tej zmianie widoku DetailsView zawiera tekst "True" dla produktów wycofane i "False" dla produktów, które są nadal aktywne.


[![Ciągi wartość PRAWDA i FAŁSZ są używane do wyświetlania stanu wycofane](using-templatefields-in-the-detailsview-control-vb/_static/image26.png)](using-templatefields-in-the-detailsview-control-vb/_static/image25.png)

**Rysunek 9**: ciągi True i False służą do wyświetlania stanu wycofany ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](using-templatefields-in-the-detailsview-control-vb/_static/image27.png))


Załóżmy, że nie chcemy ciągi "True" lub "False" mają być używane, ale "Tak" i "Nie", zamiast tego. Takie dostosowania można wykonać za pomocą TemplateField, a także metodę formatowania. Metoda formatowania można wykonać w dowolnej liczbie parametrów wejściowych, ale musi zwracać HTML (jako ciąg) do dodania do szablonu.

Dodaj metodę formatowania `DetailsViewTemplateField.aspx` strony związane z kodem klasę o nazwie `DisplayDiscontinuedAsYESorNO` która akceptuje `Northwind.ProductsRow` obiektu jako parametr wejściowy i zwraca wartość typu ciąg. Zgodnie z opisem w poprzedniej samouczek, ta metoda *musi* oznaczone jako `Protected` lub `Public` aby były dostępne z szablonu.


[!code-vb[Main](using-templatefields-in-the-detailsview-control-vb/samples/sample4.vb)]

Ta metoda sprawdza parametr wejściowy (`discontinued`) i zwraca wartość "Tak", jeśli jest `True`"NO" w przeciwnym razie wartość.

> [!NOTE]
> W metodzie formatowania w poprzedniej samouczek odwołania, który mamy zostały przekazanie pola danych, który może zawierać `NULL` s i dlatego trzeba sprawdzić, czy pracownika `HiredDate` bazy danych ma wartość właściwości `NULL` wartość przed Uzyskiwanie dostępu do `EmployeesRow`w `HiredDate` właściwości. Takie wyboru nie jest tu potrzebne od `Discontinued` kolumna nigdy nie może mieć bazy danych `NULL` przypisanych wartości. Ponadto to, dlaczego metoda może zaakceptować, wprowadź wartość logiczną parametru zamiast do akceptowania `ProductsRow` wystąpienia lub parametrem typu `Object`.


Ta metoda formatowania pełną, liście nie zostanie celu wywołania go z TemplateField `ItemTemplate`. Aby utworzyć TemplateField Usuń `Discontinued` elementu BoundField i dodać nowe pole TemplateField lub przekonwertować `Discontinued` elementu BoundField na pole TemplateField. Następnie, w widoku znaczników deklaratywne edytować TemplateField tak, aby zawierał tylko ItemTemplate, który wywołuje `DisplayDiscontinuedAsYESorNO` metody, przekazując wartość bieżącej klasy `ProductRow` wystąpienia `Discontinued` właściwości. To jest możliwy za pośrednictwem `Eval` metody. W szczególności znaczników TemplateField powinien wyglądać następująco:


[!code-aspx[Main](using-templatefields-in-the-detailsview-control-vb/samples/sample5.aspx)]

Spowoduje to `DisplayDiscontinuedAsYESorNO` metodę można wywołać podczas renderowania widoku DetailsView, przekazując `ProductRow` wystąpienia `Discontinued` wartość. Ponieważ `Eval` metoda zwraca wartość typu `Object`, ale `DisplayDiscontinuedAsYESorNO` metoda oczekuje parametru wejściowego typu `Boolean`, możemy rzutowania `Eval` metody zwracają wartość `Boolean`. `DisplayDiscontinuedAsYESorNO` Metody będą zwracać "Tak" lub "NO" w zależności od wartości odbiera. Zwrócona wartość jest wyświetlanych w tym widoku DetailsView wiersz (zobacz rysunek 10).


[![Wartości Tak lub nie są teraz wyświetlane w wierszu wycofany](using-templatefields-in-the-detailsview-control-vb/_static/image29.png)](using-templatefields-in-the-detailsview-control-vb/_static/image28.png)

**Na rysunku nr 10**: wartości Tak lub nie są teraz wyświetlane w wierszu wycofany ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](using-templatefields-in-the-detailsview-control-vb/_static/image30.png))


## <a name="summary"></a>Podsumowanie

Umożliwia TemplateField w kontrolce widoku DetailsView wyższego poziomu bezpieczeństwa i elastyczność w wyświetlaniu danych niż jest dostępne inne formanty pola i idealnie nadają się do sytuacji, w których:

- Wiele pól danych muszą być widoczny w widoku GridView
- Dane najlepiej jest wyrażona za pomocą formantu sieci Web zamiast zwykłego tekstu
- Dane wyjściowe zależy od danych, takich jak wyświetlanie metadanych lub ponowne formatowanie danych

Gdy TemplateFields umożliwić wysoką elastyczność w czasie renderowania danych podstawowych DetailsView, dane wyjściowe widoku DetailsView nadal tak nieco boxy jako każde pole jest renderowane jako wiersz w formacie HTML `<table>`.

Formant FormView oferuje wysoką elastyczność w konfigurowaniu przetworzonych wyników. FormView nie zawiera pola, ale raczej właśnie serii szablonów (`ItemTemplate`, `EditItemTemplate`, `HeaderTemplate`i tak dalej). Będzie przedstawiono sposób użycia FormView osiągnąć większą kontrolę nad renderowanym układzie w naszym samouczku dalej.

Programowanie przyjemność!

## <a name="about-the-author"></a>Informacje o autorze

[Scott Bento](http://www.4guysfromrolla.com/ScottMitchell.shtml), autora siedmiu książek ASP/ASP.NET i twórcę z [4GuysFromRolla.com](http://www.4guysfromrolla.com), pracuje z technologii Microsoft Web od 1998. Scott działa jako niezależnego konsultanta trainer i składnika zapisywania. Jest jego najnowszej książki [ *Sams nauczyć się ASP.NET 2.0 w ciągu 24 godzin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Piotr można uzyskać pod adresem [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) lub za pośrednictwem jego blog, który znajduje się w temacie [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Specjalne podziękowania dla

Ten samouczek serii zostało sprawdzone przez wiele recenzentów przydatne. Recenzenta realizacji w tym samouczku został Dan Jagers. Zainteresowani recenzowania Moje nadchodzących artykuły MSDN? Jeśli tak, Porzuć mnie linii w [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Poprzednie](using-templatefields-in-the-gridview-control-vb.md)
> [dalej](using-the-formview-s-templates-vb.md)
