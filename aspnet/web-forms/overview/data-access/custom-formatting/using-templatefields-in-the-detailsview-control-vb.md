---
uid: web-forms/overview/data-access/custom-formatting/using-templatefields-in-the-detailsview-control-vb
title: Używanie kontrolek TemplateField w kontrolce DetailsView (VB) | Dokumentacja firmy Microsoft
author: rick-anderson
description: Te same możliwości kontrolek TemplateField dostępne w widoku GridView są również dostępne w kontrolce DetailsView. W tym samouczku będziesz wyświetlamy jednego produktu...
ms.author: aspnetcontent
ms.date: 03/31/2010
ms.assetid: 0b91d5f8-127d-4f6a-b204-f2e2b35ef703
msc.legacyurl: /web-forms/overview/data-access/custom-formatting/using-templatefields-in-the-detailsview-control-vb
msc.type: authoredcontent
ms.openlocfilehash: 5a994df097148428779c9e219ed08247d47ea1a5
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/05/2018
ms.locfileid: "37821738"
---
<a name="using-templatefields-in-the-detailsview-control-vb"></a>Używanie kontrolek TemplateField w kontrolce DetailsView (VB)
====================
przez [Bento Scott](https://twitter.com/ScottOnWriting)

[Pobierz przykładową aplikację](http://download.microsoft.com/download/5/7/0/57084608-dfb3-4781-991c-407d086e2adc/ASPNET_Data_Tutorial_13_VB.exe) lub [Pobierz plik PDF](using-templatefields-in-the-detailsview-control-vb/_static/datatutorial13vb1.pdf)

> Te same możliwości kontrolek TemplateField dostępne w widoku GridView są również dostępne w kontrolce DetailsView. W tym samouczku będziesz wyświetlamy jednego produktu za pomocą DetailsView, zawierający kontrolek TemplateField.


## <a name="introduction"></a>Wprowadzenie

TemplateField zapewnia wyższy stopień elastyczności renderowania danych niż elementu BoundField, CheckBoxField, pole hiperłącza HyperLinkField i inne formanty pola danych. W [poprzedniego samouczka](using-templatefields-in-the-gridview-control-vb.md) przyjrzeliśmy się przy użyciu TemplateField w GridView do:

- Wyświetlanie wielu wartości pól danych w jednej kolumnie. W szczególności zarówno `FirstName` i `LastName` pola zostały połączone w jedną kolumnę GridView.
- Umożliwia alternatywny kontrolki sieci Web express wartość pola danych. Widzieliśmy instrukcje pokazują `HiredDate` wartość przy użyciu kontrolki kalendarza.
- Pokaż informacje o stanie oparte na danych bazowych. Gdy `Employees` tabela nie zawiera kolumny, która zwraca liczbę dni, które zostało pracownika w zadaniu, byliśmy w stanie do wyświetlania tych informacji w przykładzie GridView w poprzednim samouczku przy użyciu TemplateField i metody formatowania.

Te same możliwości kontrolek TemplateField dostępne w widoku GridView są również dostępne w kontrolce DetailsView. W tym samouczku będziesz wyświetlamy jednego produktu za pomocą DetailsView, który zawiera dwa kontrolek TemplateField. Pierwszy TemplateField będą łączone `UnitPrice`, `UnitsInStock`, i `UnitsOnOrder` pól danych w jednym wierszu DetailsView. Drugi TemplateField zostanie wyświetlona wartość `Discontinued` pola, ale użyje metody formatowania, aby wyświetlić "YES", jeśli `Discontinued` jest `True`i "NO" inaczej.


[![Dwóch kontrolek TemplateField są używane do dostosowywania wyświetlania](using-templatefields-in-the-detailsview-control-vb/_static/image2.png)](using-templatefields-in-the-detailsview-control-vb/_static/image1.png)

**Rysunek 1**: dwóch kontrolek TemplateField są używane do dostosowywania wyświetlania ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](using-templatefields-in-the-detailsview-control-vb/_static/image3.png))


Zaczynajmy!

## <a name="step-1-binding-the-data-to-the-detailsview"></a>Krok 1: Wiązanie danych do DetailsView

Zgodnie z opisem w poprzednim samouczku, pracując z kontrolek TemplateField jest często łatwiej rozpocząć od utworzenia kontrolce DetailsView, który zawiera tylko BoundFields, a następnie Dodaj nowych kontrolek TemplateField lub przekonwertować istniejącej BoundFields kontrolek TemplateField jako wymagane . W związku z tym można uruchomić w tym samouczku, dodając DetailsView na strony za pomocą projektanta i powiązywanie kontrolki ObjectDataSource, które zwraca listę produktów. Te kroki utworzy DetailsView z BoundFields dla każdego pola wartości inne niż logiczne produktu i CheckBoxField dla jednego pola wartość logiczna (wstrzymane).

Otwórz `DetailsViewTemplateField.aspx` strony i przeciągnij element DetailsView z przybornika do projektanta. Z tagu inteligentnego DetailsView wybierz można dodać nowego formantu ObjectDataSource, który wywołuje `ProductsBLL` klasy `GetProducts()` metody.


[![Dodawanie nowego formantu ObjectDataSource, który wywołuje metodę GetProducts()](using-templatefields-in-the-detailsview-control-vb/_static/image5.png)](using-templatefields-in-the-detailsview-control-vb/_static/image4.png)

**Rysunek 2**: Dodawanie nowego formantu ObjectDataSource tego Invokes `GetProducts()` — metoda ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](using-templatefields-in-the-detailsview-control-vb/_static/image6.png))


Ten raport można usunąć `ProductID`, `SupplierID`, `CategoryID`, i `ReorderLevel` BoundFields. Następnie należy zmienić kolejność BoundFields tak, aby `CategoryName` i `SupplierName` BoundFields pojawiają się natychmiast po `ProductName` elementu BoundField. Możesz dostosować `HeaderText` właściwości i właściwości formatowania dla BoundFields jak wedle uznania. Podobnie jak przy użyciu GridView, te operacje edycji elementu BoundField poziomie można wykonać za pomocą okna dialogowego pola (dostępne, klikając link Edytuj pola w tagu inteligentnego DetailsView) lub za pomocą składni deklaratywnej. Na koniec wyczyszczenie DetailsView `Height` i `Width` wartości właściwości w celu umożliwienia DetailsView sterowania, aby rozwinąć w oparciu o dane wyświetlane i zaznacz pole wyboru Włącz stronicowania w tagu inteligentnego.

Po wprowadzeniu tych zmian, swojej kontrolce DetailsView oznaczeniu deklaracyjnym powinien wyglądać podobnie do poniższej:


[!code-aspx[Main](using-templatefields-in-the-detailsview-control-vb/samples/sample1.aspx)]

Poświęć chwilę, aby wyświetlić stronę za pośrednictwem przeglądarki. W tym momencie powinien zostać wyświetlony jeden produkt na liście (Chai) przedstawiający nazwę produktu, kategorii, dostawca, ceny, jednostek w magazynie, jednostki w kolejności i jego stan nieobsługiwane wierszy.


[![Szczegóły produktu są wyświetlane, używając szeregu BoundFields](using-templatefields-in-the-detailsview-control-vb/_static/image8.png)](using-templatefields-in-the-detailsview-control-vb/_static/image7.png)

**Rysunek 3**: szczegółowe informacje są wyświetlane przy użyciu serii BoundFields ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](using-templatefields-in-the-detailsview-control-vb/_static/image9.png))


## <a name="step-2-combining-the-price-units-in-stock-and-units-on-order-into-one-row"></a>Krok 2: Łączenie ceny, stan magazynu i jednostki w kolejności w jednym wierszu

DetailsView ma wiersz `UnitPrice`, `UnitsInStock`, i `UnitsOnOrder` pola. Możemy połączyć te pola danych w pojedynczy wiersz z TemplateField, dodając nowe TemplateField lub przez jedną z istniejącą konwersję `UnitPrice`, `UnitsInStock`, i `UnitsOnOrder` BoundFields do TemplateField. Gdy wolę osobiście, konwertowania istniejących BoundFields, teraz praktyczne, dodając nowe TemplateField.

Rozpocznij, klikając link Edytuj pola w DetailsView tagu inteligentnego, aby wyświetlić okno dialogowe pól. Następnie dodaj nowy TemplateField i ustaw jego `HeaderText` właściwość "Cena i spisu" i Przenieś TemplateField nowe, tak że jest umieszczana powyżej `UnitPrice` elementu BoundField.


[![Dodaj nowe TemplateField na kontrolce DetailsView](using-templatefields-in-the-detailsview-control-vb/_static/image11.png)](using-templatefields-in-the-detailsview-control-vb/_static/image10.png)

**Rysunek 4**: dodawanie nowych TemplateField do kontrolce DetailsView ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](using-templatefields-in-the-detailsview-control-vb/_static/image12.png))


Ponieważ ten nowy TemplateField będzie zawierać wartości aktualnie wyświetlany w `UnitPrice`, `UnitsInStock`, i `UnitsOnOrder` BoundFields, przewodnika usuńmy je.

Ostatnim zadaniem w tym kroku jest zdefiniowanie `ItemTemplate` kod znaczników dla ceny i TemplateField spisu, który można osiągnąć za pomocą szablonu DetailsView edytowania interfejsu projektanta lub ręcznie za pomocą składni deklaratywnej formantu. Podobnie jak w przypadku GridView interfejsu edycji szablonu DetailsView jest możliwy, klikając link Edytuj szablony w tagu inteligentnego. W tym miejscu można wybrać szablon do edycji z listy rozwijanej, a następnie dodaj wszystkie kontrolki sieci Web z przybornika.

W tym samouczku Rozpocznij, dodając kontrolkę typu etykieta do cen i spisu TemplateField `ItemTemplate`. Następnie kliknij link Edytuj powiązania danych z tag inteligentny formant etykiety w sieci Web i powiązać `Text` właściwości `UnitPrice` pola.


[![Powiąż właściwości tekstu etykiety z polem danych UnitPrice](using-templatefields-in-the-detailsview-control-vb/_static/image14.png)](using-templatefields-in-the-detailsview-control-vb/_static/image13.png)

**Rysunek 5**: powiązywanie etykiety `Text` właściwości `UnitPrice` pola danych ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](using-templatefields-in-the-detailsview-control-vb/_static/image15.png))


## <a name="formatting-the-price-as-a-currency"></a>Formatowanie cena jako walutę

Dodając ten formant etykiety w sieci Web ceny i TemplateField magazynu będą teraz wyświetlane tylko cena dla wybranego produktu. Rysunek 6 przedstawia zrzut ekranu: postępach tej pory podczas wyświetlania za pośrednictwem przeglądarki.


[![Ceny i spisu TemplateField pokazuje cenę](using-templatefields-in-the-detailsview-control-vb/_static/image17.png)](using-templatefields-in-the-detailsview-control-vb/_static/image16.png)

**Rysunek 6**: ceny i spisu TemplateField pokazuje cenę ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](using-templatefields-in-the-detailsview-control-vb/_static/image18.png))


Należy zauważyć, że cena produktu nie jest sformatowany jako walutę. Za pomocą elementu BoundField, formatowanie jest możliwa przez ustawienie `HtmlEncode` właściwości `False` i `DataFormatString` właściwość `{0:formatSpecifier}`. TemplateField jednak wszelkie instrukcje formatowania należy określić w składni wiązania z danymi lub za pomocą metody formatowania zdefiniowane w innym kodem aplikacji (np. strony ASP.NET osobna klasa kodu).

Aby określić formatowanie składni wiązania danych używany w kontrolce etykiety w sieci Web, wróć do okna dialogowego powiązania danych, klikając link Edytuj powiązania danych z tagu etykiet. Można wpisać instrukcje formatowania bezpośrednio z listy rozwijanej Format, lub wybierz jeden z ciągów zdefiniowanym formacie. Za pomocą elementu BoundField `DataFormatString` właściwości formatowania jest określany przy użyciu `{0:formatSpecifier}`.

Aby uzyskać `UnitPrice` Użyj formatowania waluty określonych, wybierając wartość odpowiednie listy rozwijanej lub wpisując w polu `{0:C}` ręcznie.


[![Cena w formacie waluty](using-templatefields-in-the-detailsview-control-vb/_static/image20.png)](using-templatefields-in-the-detailsview-control-vb/_static/image19.png)

**Rysunek 7**: formatowanie cena jako walutę ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](using-templatefields-in-the-detailsview-control-vb/_static/image21.png))


Deklaratywne, specyfikacja formatowania jest oznaczany jako drugiego parametru do `Bind` lub `Eval` metody. Ustawienia wprowadzone przed chwilą za pośrednictwem wyników projektanta w następujące wyrażenie wiążące dane w oznaczeniu deklaracyjnym:


[!code-aspx[Main](using-templatefields-in-the-detailsview-control-vb/samples/sample2.aspx)]

## <a name="adding-the-remaining-data-fields-to-the-templatefield"></a>Dodawanie pozostałych pól danych do TemplateField

W tym momencie została firma Microsoft wyświetlane i sformatowany `UnitPrice` dane pole ceny i TemplateField spisu, ale nadal musisz wyświetlić `UnitsInStock` i `UnitsOnOrder` pola. Możemy wyświetlić je na wiersz poniżej ceny i w nawiasach. Z interfejsu edycji szablonu w Projektancie można dodawać takich znaczników, ustawiając kursor w szablonie i po prostu wpisując tekst, który ma być wyświetlany. Alternatywnie można wprowadzić bezpośrednio w składni deklaratywnej ten kod znaczników.

Dodawanie znaczników statyczne formanty etykiet w sieci Web i składnia wiązania danych tak, aby ceny i TemplateField magazynu zawiera informacje dotyczące cen i spisu w następujący sposób:

*Cena jednostkowa*  
(**w magazynie / w kolejności:** *UnitsInStock* / *JednostkiZamówione*)

Po wykonaniu tego zadania usługi DetailsView oznaczeniu deklaracyjnym powinien wyglądać podobnie do poniższej:


[!code-aspx[Main](using-templatefields-in-the-detailsview-control-vb/samples/sample3.aspx)]

Za pomocą tych zmian skonsolidowaliśmy informacji o cenach i spisu w pojedynczy wiersz DetailsView.


[![Ceny i informacje dotyczące spisu są wyświetlane w pojedynczy wiersz tabeli](using-templatefields-in-the-detailsview-control-vb/_static/image23.png)](using-templatefields-in-the-detailsview-control-vb/_static/image22.png)

**Rysunek 8**: ceny i informacje dotyczące spisu są wyświetlane w pojedynczy wiersz tabeli ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](using-templatefields-in-the-detailsview-control-vb/_static/image24.png))


## <a name="step-3-customizing-the-discontinued-field-information"></a>Krok 3: Dostosowywanie informacji nieobsługiwane pola

`Products` Tabeli `Discontinued` kolumna jest wartością bitową, która wskazuje, czy produkt został wycofany. Podczas tworzenia powiązania DetailsView (lub GridView) do kontroli źródła danych, takich jak pola wartość logiczna `Discontinued`, są implementowane jako CheckBoxFields, pola wartości inne niż logiczne, jak `ProductID`, `ProductName`i tak dalej są implementowane jako BoundFields. CheckBoxField renderowany jako wyłączone pole wyboru jest zaznaczone, jeśli wartość pola danych jest w przeciwnym razie PRAWDA i niezaznaczone.

Zamiast wyświetlania CheckBoxField, warto zamiast tego wyświetli się tekst wskazującą, czy produkt jest obsługiwany. W tym celu można usunąć CheckBoxField z DetailsView i następnie dodać elementu BoundField którego `DataField` właściwość `Discontinued`. Poświęć chwilę, aby to zrobić. Po tej zmianie DetailsView zawiera tekst "True" dla uwzględniałyby produkty wycofane i "False" dla produktów, które są nadal aktywne.


[![Ciągi wartości True i False są używane do wyświetlania stanu nieobsługiwane](using-templatefields-in-the-detailsview-control-vb/_static/image26.png)](using-templatefields-in-the-detailsview-control-vb/_static/image25.png)

**Rysunek 9**: ciągi True i False są używane do wyświetlania stanu wycofany ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](using-templatefields-in-the-detailsview-control-vb/_static/image27.png))


Wyobraź sobie, że nie Chcieliśmy ciągi "True" lub "False", ma być używany, ale "YES" i "NO", zamiast tego. Takie dostosowania można wykonać za pomocą TemplateField i metody formatowania. Metody formatowania można wykonać w dowolnej liczbie parametrów wejściowych, ale musi zwracać kod HTML (jako ciąg) do dodania do szablonu.

Dodaj metodę formatowania `DetailsViewTemplateField.aspx` strony związanym z kodem klasę o nazwie `DisplayDiscontinuedAsYESorNO` akceptujący `Northwind.ProductsRow` obiekt jako parametr wejściowy i zwraca ciąg. Zgodnie z opisem w poprzednim samouczku ta metoda *musi* można oznaczyć jako `Protected` lub `Public` aby były dostępne z szablonu.


[!code-vb[Main](using-templatefields-in-the-detailsview-control-vb/samples/sample4.vb)]

Ta metoda sprawdza, czy parametr wejściowy (`discontinued`) i zwraca wartość "YES", jeśli jest `True`"NO" w przeciwnym razie wartość.

> [!NOTE]
> W metodzie formatowania w poprzedniego samouczka odwołania, który możemy zostały przekazując pola danych, który może zawierać `NULL` s i dlatego trzeba sprawdzić, czy pracownika `HiredDate` wartość właściwości ma bazę danych `NULL` wartość przed Uzyskiwanie dostępu do `EmployeesRow`firmy `HiredDate` właściwości. Takie wyboru nie jest tu potrzebne od `Discontinued` kolumna nigdy nie może mieć bazy danych `NULL` wartości przypisane. Ponadto, to Dlaczego metoda może zaakceptować wartość logiczną wprowadzania parametrów, niż musieć go zaakceptować `ProductsRow` wystąpienie lub parametr typu `Object`.


Przy użyciu tej metody formatowania pełną, pozostaje tylko do wywoływania go z TemplateField `ItemTemplate`. Aby utworzyć TemplateField usunąć `Discontinued` elementu BoundField i dodać nowe TemplateField lub przekonwertować `Discontinued` elementu BoundField do TemplateField. Następnie, w widoku oznaczeniu deklaracyjnym edytować TemplateField tak, aby zawierała tylko właściwości ItemTemplate, który wywołuje `DisplayDiscontinuedAsYESorNO` metody, przekazując wartość bieżącej `ProductRow` wystąpienia `Discontinued` właściwości. To jest możliwy za pośrednictwem `Eval` metody. W szczególności powinny wyglądać TemplateField znaczników:


[!code-aspx[Main](using-templatefields-in-the-detailsview-control-vb/samples/sample5.aspx)]

Spowoduje to `DisplayDiscontinuedAsYESorNO` metoda do wywołania podczas renderowania DetailsView, przekazując `ProductRow` wystąpienia `Discontinued` wartość. Ponieważ `Eval` metoda zwraca wartość typu `Object`, ale `DisplayDiscontinuedAsYESorNO` metoda oczekuje parametru wejściowego typu `Boolean`, firma Microsoft rzutowania `Eval` metody zwracają wartość `Boolean`. `DisplayDiscontinuedAsYESorNO` Metody będą zwracać "YES" lub "NO" w zależności od wartości odbierze. Zwrócona wartość jest wyświetlanych w tym DetailsView wiersz (zobacz rysunek 10).


[![Wartości Tak lub nie są teraz wyświetlane w wierszu wstrzymane](using-templatefields-in-the-detailsview-control-vb/_static/image29.png)](using-templatefields-in-the-detailsview-control-vb/_static/image28.png)

**Na rysunku nr 10**: YES lub NO wartości są teraz wyświetlane w wierszu wycofany ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](using-templatefields-in-the-detailsview-control-vb/_static/image30.png))


## <a name="summary"></a>Podsumowanie

Umożliwia TemplateField w kontrolce DetailsView wyższy stopień elastyczności w wyświetlaniu danych niż jest dostępne z innymi formantami pola i doskonale nadają się do sytuacji, w których:

- Wiele pól danych muszą być wyświetlane w jedną kolumnę GridView
- Dane najlepiej jest wyrażany za pomocą formantu sieci Web zamiast zwykłego tekstu
- Dane wyjściowe jest zależny od danych bazowych, takich jak wyświetlanie metadanych lub ponowne formatowanie danych

Gdy kontrolek TemplateField pozwalają na większy stopień elastyczności w czasie renderowania danych bazowych DetailsView, dane wyjściowe DetailsView nadal działa nieco boxy jak każde pole jest renderowane jako wiersz w kodzie HTML `<table>`.

Formant widoku FormView oferuje pewną większą elastyczność w konfigurowaniu wyniku renderowania. FormView nie zawiera pola, ale raczej po prostu szereg szablonów (`ItemTemplate`, `EditItemTemplate`, `HeaderTemplate`i tak dalej). Zobaczymy, jak używać FormView osiągnąć większą kontrolę nad renderowanym układzie w naszym samouczku dalej.

Wszystkiego najlepszego programowania!

## <a name="about-the-author"></a>Informacje o autorze

[Scott Bento](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor siedem ASP/ASP.NET książek i założycielem [4GuysFromRolla.com](http://www.4guysfromrolla.com), pracował nad przy użyciu technologii Microsoft Web od 1998 r. Scott działa jako niezależny Konsultant, trainer i składnika zapisywania. Jego najnowszą książkę Stephena [ *Sams uczyć się ASP.NET 2.0 w ciągu 24 godzin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). ADAM można z Tobą skontaktować w [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) lub za pośrednictwem jego blogu, który znajduje się w temacie [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Specjalne podziękowania dla

W tej serii samouczków został zrecenzowany przez wielu recenzentów pomocne. Weryfikacja potencjalnych klientów w ramach tego samouczka został Dan Jagers. Zainteresowani zapoznaniem Moje kolejnych artykułów MSDN? Jeśli tak, Porzuć mnie linii w [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Poprzednie](using-templatefields-in-the-gridview-control-vb.md)
> [dalej](using-the-formview-s-templates-vb.md)
