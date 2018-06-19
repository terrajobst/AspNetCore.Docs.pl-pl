---
uid: web-forms/overview/data-access/displaying-data-with-the-datalist-and-repeater/formatting-the-datalist-and-repeater-based-upon-data-vb
title: Formatowanie DataList i powtarzanego na podstawie danych (VB) | Dokumentacja firmy Microsoft
author: rick-anderson
description: W tym samouczku będziemy czynności w przykłady jak możemy formatować wygląd formantów DataList i powtarzanego, przy użyciu formatowania funkcje za pomocą...
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/13/2006
ms.topic: article
ms.assetid: e2f401ae-37bb-4b19-aa97-d6b385d40f88
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/displaying-data-with-the-datalist-and-repeater/formatting-the-datalist-and-repeater-based-upon-data-vb
msc.type: authoredcontent
ms.openlocfilehash: 174a68cf0785b33c85139d57ede9717ce7e135e0
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
ms.locfileid: "30876595"
---
<a name="formatting-the-datalist-and-repeater-based-upon-data-vb"></a>Formatowanie DataList i powtarzanego na podstawie danych (VB)
====================
przez [Bento Scott](https://twitter.com/ScottOnWriting)

[Pobierz przykładową aplikację](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_30_VB.exe) lub [pobierania plików PDF](formatting-the-datalist-and-repeater-based-upon-data-vb/_static/datatutorial30vb1.pdf)

> W tym samouczku będziemy czynności w przykłady jak możemy formatować wygląd formantów DataList i powtarzanego, za pomocą funkcji formatowania w szablonach lub Obsługa zdarzenia z danymi.


## <a name="introduction"></a>Wprowadzenie

Jako widzieliśmy w poprzednim samouczek elementu DataList oferuje wiele właściwości powiązanych z stylu, które mają wpływ na jego wyglądu. W szczególności widzieliśmy jak przypisać domyślnej klasy CSS do DataList s `HeaderStyle`, `ItemStyle`, `AlternatingItemStyle`, i `SelectedItemStyle` właściwości. Oprócz tych czterech właściwości elementu DataList zawiera wiele innych właściwości związanych z stylu, takich jak `Font`, `ForeColor`, `BackColor`, i `BorderWidth`, kilka. Kontrolce elementu powtarzanego nie zawiera żadnych właściwości związanych z stylu. Takie ustawienia stylu muszą być wprowadzane bezpośrednio z poziomu znacznika w szablonach s elementu powtarzanego.

Często, jak dane powinny być sformatowane zależy od samych danych. Na przykład podczas wyświetlania produktów może chcemy wyświetlania informacji o produktach w kolorze światła czcionki szary, jeśli jest już obsługiwana lub chcemy zaznacz `UnitsInStock` wartość, gdy jest równa 0. Jako widzieliśmy w poprzednim samouczki GridView widoku DetailsView i FormView oferują dwa różne sposoby formatowania ich wygląd, na podstawie ich danych:

- **`DataBound` Zdarzeń** Tworzenie procedury obsługi zdarzeń do właściwego `DataBound` zdarzenie, które są generowane po danych została powiązana z każdego elementu (dla widoku GridView był `RowDataBound` zdarzeń; DataList i powtarzanego jest `ItemDataBound`zdarzeń). W takim przypadku program obsługi, po prostu danymi powiązanymi można zbadać i formatowanie decyzje wprowadzone. Możemy zbadać tej techniki [niestandardowe formatowanie oparte na danych](../custom-formatting/custom-formatting-based-upon-data-vb.md) samouczka.
- **Formatowanie funkcji w szablonach** używając TemplateFields w widoku DetailsView lub widoku GridView formantów lub szablonu w formancie FormView, można dodać funkcji formatowania do klasy związane z kodem strony ASP.NET, warstwy logiki biznesowej lub dowolnej inne biblioteki klas, który jest dostępny z aplikacji sieci web. Ta funkcja formatowania może akceptować dowolnej liczby parametrów wejściowych, ale musi zwracać HTML do renderowania w szablonie. Najpierw zbadano funkcje formatowania w [przy użyciu TemplateFields w kontrolce GridView](../custom-formatting/using-templatefields-in-the-gridview-control-vb.md) samouczka.

Oba te techniki formatowania są dostępne z formantami DataList i elementu powtarzanego. W tym samouczku będziemy czynności w przykłady użycia obie techniki dla obu formantów.

## <a name="using-theitemdataboundevent-handler"></a>Przy użyciu`ItemDataBound`obsługi zdarzeń

Gdy danych jest powiązany z DataList z kontroli źródła danych lub programowo przypisując danych do kontrolki s `DataSource` właściwości i wywołanie jego `DataBind()` metody DataList s `DataBinding` zdarzenia generowane wyliczone, źródła danych a każdy rekord danych jest powiązany z elementu DataList. Dla każdego rekordu w źródle danych, tworzy elementu DataList [ `DataListItem` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalistitem.aspx) obiekt, a następnie powiązany z bieżącym rekordem. W trakcie tego procesu elementu DataList zgłasza dwa zdarzenia:

- **`ItemCreated`** generowane po `DataListItem` został utworzony
- **`ItemDataBound`** generowane po powiązany bieżącego rekordu `DataListItem`

Poniższe kroki wchodzą w skład procesu wiązania danych formant DataList.

1. DataList s [ `DataBinding` zdarzeń](https://msdn.microsoft.com/library/system.web.ui.control.databinding.aspx) uruchamiany
2. Do elementu DataList powiązania danych  
  
   Dla każdego rekordu w źródle danych 

    1. Utwórz `DataListItem` obiektu
    2. Fire [ `ItemCreated` zdarzeń](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalist.itemcreated.aspx)
    3. Powiąż rekordu `DataListItem`
    4. Fire [ `ItemDataBound` zdarzeń](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalist.itemdatabound.aspx)
    5. Dodaj `DataListItem` do `Items` kolekcji

Wiązanie danych w kontrolce elementu powtarzanego, jego postępów za pośrednictwem dokładnie tej samej sekwencji kroków. Jedyną różnicą jest to, że zamiast `DataListItem` używa powtarzanego wystąpień tworzona, [ `RepeaterItem` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.repeateritem(VS.80).aspx)s.

> [!NOTE]
> Czytnik astute zauważyć nieznaczne anomalii między sekwencja kroków, które orzeczona podczas DataList i powtarzanego są związane z danymi i gdy widoku GridView jest powiązany z danymi. Na końcu tail proces wiązania danych, zgłasza widoku GridView `DataBound` zdarzeń; jednak formant DataList ani elementu powtarzanego mieć takiego zdarzenia. Jest to spowodowane formant DataList i powtarzanego zostały utworzone w przedziale czasu 1.x ASP.NET, przed wzorzec programu obsługi zdarzeń przed i po poziomu stała wspólnej.


Jak z widoku GridView, jedną z opcji formatowania na podstawie danych do utworzenia programu obsługi zdarzeń dla `ItemDataBound` zdarzeń. Ten program obsługi zdarzeń może sprawdzić dane, które były została powiązana tylko z `DataListItem` lub `RepeaterItem` i wpływają na formatowanie formantu, zgodnie z potrzebami.

Dla formant DataList zmiany formatowania dla całego elementu można implementować przy użyciu `DataListItem` s powiązane styl właściwości, które obejmują standardowe `Font`, `ForeColor`, `BackColor`, `CssClass`i tak dalej. Wpływ na formatowanie określonego formantów sieci Web wewnątrz elementu DataList szablonu s, musimy programowo modyfikacji i dostępu styl tych formantów sieci Web. Widzieliśmy sposób wykonania tego Wstecz w *niestandardowe formatowanie oparte na danych* samouczka. Kontrolce elementu powtarzanego, takich jak `RepeaterItem` klasa nie ma powiązanych styl właściwości; w związku z tym wszystkie powiązane styl zmiany wprowadzone do `RepeaterItem` w `ItemDataBound` obsługi zdarzeń musi zostać wykonana przy programowane uzyskiwanie dostępu i aktualizowanie formantów sieci Web w szablon.

Ponieważ `ItemDataBound` formatowanie technika DataList i powtarzanego są niemal identyczne, naszym przykładzie koncentruje się na przy użyciu elementu DataList.

## <a name="step-1-displaying-product-information-in-the-datalist"></a>Krok 1: Wyświetlanie informacji o produkcie w elementu DataList

Przed możemy martwić formatowanie, umożliwiają s utworzyć stronę, która używa DataList do wyświetlenia informacji o produkcie. W [poprzedniego samouczek](displaying-data-with-the-datalist-and-repeater-controls-vb.md) utworzyliśmy DataList którego `ItemTemplate` wyświetlane każdego nazwa produktu s, kategorii, dostawca, ilość na jednostkę oraz cenę. Let s, powtórz tę funkcję w tym miejscu, w tym samouczku. Aby to zrobić, albo można odtworzyć elementu DataList i jego ObjectDataSource od początku lub za pośrednictwem tych kontrolek można skopiować ze strony utworzonej w poprzednim samouczku (`Basics.aspx`) i wklej je do strony, w tym samouczku (`Formatting.aspx`).

Po zostały zreplikowane funkcji DataList i ObjectDataSource `Basics.aspx` do `Formatting.aspx`, Poświęć chwilę, aby zmienić DataList s `ID` właściwość z `DataList1` do bardziej opisowe `ItemDataBoundFormattingExample`. Następnie można wyświetlić elementu DataList w przeglądarce. Jak pokazano na rysunku 1, formatowania jedyną różnicą między każdego produktu jest, że przełącza kolor tła.


[![Produkty są wymienione w formant DataList](formatting-the-datalist-and-repeater-based-upon-data-vb/_static/image2.png)](formatting-the-datalist-and-repeater-based-upon-data-vb/_static/image1.png)

**Rysunek 1**: produkty są wymienione w formant DataList ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](formatting-the-datalist-and-repeater-based-upon-data-vb/_static/image3.png))


W tym samouczku umożliwiają format elementu DataList w taki sposób, że wszystkie produkty z cen mniej niż 20,00 $ ma zarówno jego nazwy i Jednostka ceny wyróżnione żółty s.

## <a name="step-2-programmatically-determining-the-value-of-the-data-in-the-itemdatabound-event-handler"></a>Krok 2: Programowane określania wartości danych w obsłudze zdarzeń ItemDataBound

Ponieważ tylko produkty cen w obszarze będzie $20,00 mają niestandardowe formatowanie zastosowane, firma Microsoft musi być możliwe ustalenie cena s każdego produktu. Podczas tworzenia wiązania danych DataList, DataList wylicza rekordów źródła danych i dla każdego rekordu, tworzy `DataListItem` wystąpienie powiązania rekordzie źródła danych `DataListItem`. Po określonego rekordu s danych została powiązana z bieżącą `DataListItem` obiekt DataList s `ItemDataBound` zdarzenie jest wywoływane. Można utworzyć program obsługi zdarzeń dla tego zdarzenia sprawdzić wartości danych dla bieżącej `DataListItem` i oparte na tych wartości, wprowadź niezbędne zmiany formatowania.

Utwórz `ItemDataBound` zdarzeń dla elementu DataList i Dodaj następujący kod:


[!code-vb[Main](formatting-the-datalist-and-repeater-based-upon-data-vb/samples/sample1.vb)]

Podczas koncepcji i semantyki za DataList s `ItemDataBound` obsługi zdarzeń są takie same, jak w widoku GridView s `RowDataBound` obsługi zdarzeń w *niestandardowe formatowanie oparte na danych* samouczka składnia różni się nieznacznie. Gdy `ItemDataBound` generowane zdarzenie `DataListItem` tylko powiązany z danych są przekazywane do odpowiedniego programu obsługi zdarzeń za pomocą `e.Item` (zamiast `e.Row`, podobnie jak w przypadku GridView s `RowDataBound` obsługi zdarzeń). DataList s `ItemDataBound` obsługi zdarzenia generowane dla *każdego* wiersz dodany do elementu DataList, w tym wiersze nagłówka, stopki i separatora wierszy. Jednak informacji o produkcie jest powiązany tylko z wierszy danych. W związku z tym korzystając z `ItemDataBound` zdarzenie, aby sprawdzić dane, powiązany z elementu DataList, należy najpierw upewnij się, że firma Microsoft odnośnie do pracy z elementu danych. Można to osiągnąć poprzez sprawdzenie `DataListItem` s [ `ItemType` właściwości](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalistitem.itemtype.aspx), który może mieć jeden z [następujące wartości osiem](https://msdn.microsoft.com/library/system.web.ui.webcontrols.listitemtype.aspx):

- `AlternatingItem`
- `EditItem`
- `Footer`
- `Header`
- `Item`
- `Pager`
- `SelectedItem`
- `Separator`

Zarówno `Item` i `AlternatingItem``DataListItem` elementy danych w skład DataList s s. Zakładając, że firma Microsoft odnośnie do pracy z `Item` lub `AlternatingItem`, możemy dostępu rzeczywiste `ProductsRow` wystąpienia, która została powiązana z bieżącą `DataListItem`. `DataListItem` s [ `DataItem` właściwości](https://msdn.microsoft.com/system.web.ui.webcontrols.datalistitem.dataitem.aspx) zawiera odwołanie do `DataRowView` obiektu, którego `Row` właściwość zawiera odwołanie do rzeczywistego `ProductsRow` obiektu.

Następnie sprawdzamy `ProductsRow` wystąpienia s `UnitPrice` właściwości. Od tabeli Produkty s `UnitPrice` pole umożliwia `NULL` wartości, przed podjęciem próby uzyskania dostępu `UnitPrice` właściwości możemy najpierw sprawdź, czy ma `NULL` wartości przy użyciu `IsUnitPriceNull()` metody. Jeśli `UnitPrice` wartość nie jest `NULL`, możemy następnie sprawdź, czy jest ona mniejsza niż 20,00 $ s. Jeśli tak jest rzeczywiście w obszarze $20,00, następnie należy zastosować niestandardowe formatowanie.

## <a name="step-3-highlighting-the-product-s-name-and-price"></a>Krok 3: Wyróżnianie Nazwa s produktu i cen

Wiemy, że cena produktu s jest mniejsza niż 20,00 $, czy pozostaje po aby wyróżnić jego nazwa i cenę. W tym celu możemy musi najpierw programowo odwoływać formantów etykiet w `ItemTemplate` wyświetlający nazwa produktu s i cenę. Następnie należy je wyświetlić żółty tło. Te informacje formatowania mogą być stosowane przez bezpośrednie modyfikowanie etykiet `BackColor` właściwości (`LabelID.BackColor = Color.Yellow`); najlepiej, jeśli jednak wszystkich zagadnień związanych z wyświetlania powinien zostać przedstawiony przy użyciu kaskadowych arkuszy stylów. W rzeczywistości mamy już arkusza stylów, która udostępnia żądanego formatowania zdefiniowane w `Styles.css`  -  `AffordablePriceEmphasis`, który został utworzony i omówione w *niestandardowe formatowanie oparte na danych* samouczka.

Aby zastosować formatowanie, wystarczy ustawić dwóch formantów sieci Web etykiety `CssClass` właściwości `AffordablePriceEmphasis`, jak pokazano w poniższym kodzie:


[!code-vb[Main](formatting-the-datalist-and-repeater-based-upon-data-vb/samples/sample2.vb)]

Z `ItemDataBound` obsługi zdarzeń została zakończona, ponownie `Formatting.aspx` strony w przeglądarce. Jak pokazano na rysunku 2, produkty z cen w obszarze $20,00 ma zarówno nazwy użytkownika i cen wyróżnione.


[![Te produkty mniej niż $20,00 wyróżniono](formatting-the-datalist-and-repeater-based-upon-data-vb/_static/image5.png)](formatting-the-datalist-and-repeater-based-upon-data-vb/_static/image4.png)

**Rysunek 2**: te produkty mniej niż $20,00 wyróżniono ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](formatting-the-datalist-and-repeater-based-upon-data-vb/_static/image6.png))


> [!NOTE]
> Ponieważ elementu DataList jest renderowane jako kodu HTML `<table>`, jego `DataListItem` wystąpienia mają właściwości związanych z stylu, które można ustawić do zastosowania określonego stylu do całego elementu. Na przykład możemy wyróżnić *cały* elementu żółty, gdy jego cena była mniejsza niż 20,00 $, firma Microsoft może zastąpiła kod, który odwołuje się do etykiety i ustawić ich `CssClass` właściwości, korzystając z poniższego kodu: `e.Item.CssClass = "AffordablePriceEmphasis"` (patrz rysunek 3).


`RepeaterItem` , Które składają się na kontrolce elementu powtarzanego jednak t Jan oferują takich właściwości stylu poziomie. W związku z tym formatowania niestandardowych do powtarzanego wymaga stosowania właściwości stylu dla formantów sieci Web w szablonów elementu powtarzanego s, tak samo, jak robiliśmy na rysunku 2.


[![Cały element produktu zostanie wyróżniona dla produktów w obszarze 20,00 $](formatting-the-datalist-and-repeater-based-upon-data-vb/_static/image8.png)](formatting-the-datalist-and-repeater-based-upon-data-vb/_static/image7.png)

**Rysunek 3**: cały element produktu zostanie wyróżniona dla produktów w obszarze 20,00 $ ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](formatting-the-datalist-and-repeater-based-upon-data-vb/_static/image9.png))


## <a name="using-formatting-functions-from-within-the-template"></a>Przy użyciu funkcji formatowania w szablonie

W *przy użyciu TemplateFields w kontrolce GridView* samouczek widzieliśmy sposób użycia funkcji formatowania w widoku GridView TemplateField do formatowania niestandardowych na podstawie danych powiązanych z wierszami s widoku GridView. Funkcja formatowania to metoda, która może być wywoływany z szablonu i zwraca HTML, aby emitować w jego miejscu. Funkcje formatowania mogą znajdować się w klasie związanej z kodem strony ASP.NET lub być scentralizowane plikami klasy w `App_Code` folderu lub w oddzielnym projektu biblioteki klas. Przenoszenie formatowania funkcja poza klasę CodeBehind s strony ASP.NET jest odpowiedni, jeśli planujesz używanie tej samej funkcji formatowania w wielu stron ASP.NET lub innych aplikacji sieci web ASP.NET.

Aby zademonstrować funkcje formatowania, umożliwiają s ma informacji o produkcie zawierają tekst [WYCOFANY] obok nazwy produktu s, jeśli ją przerwało s. Let s są także obszar cen wyróżnione żółty, jeśli go s mniej niż 20,00 $ (jak robiliśmy `ItemDataBound` przykład program obsługi zdarzeń); Jeśli ceny $20,00 lub nowszej, który pozwala s nie wyświetla rzeczywiste cen, ale zamiast tego tekstu, należy wywołać oferty ceny. Na rysunku 4 przedstawiono zrzut ekranu wyświetlania przy użyciu tych reguł formatowania zastosować produktów.


[![Dla produktów kosztowne ceny jest zastępowany tekstu, wywołaj oferty cen](formatting-the-datalist-and-repeater-based-upon-data-vb/_static/image11.png)](formatting-the-datalist-and-repeater-based-upon-data-vb/_static/image10.png)

**Rysunek 4**: produktów kosztowne, ceny jest zastępowany tekstu, wywołaj oferty cen ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](formatting-the-datalist-and-repeater-based-upon-data-vb/_static/image12.png))


## <a name="step-1-create-the-formatting-functions"></a>Krok 1: Tworzenie funkcji formatowania

W tym przykładzie potrzebne dwie funkcje formatowania, który wyświetla nazwę produktu, wraz z tekstu [WYCOFANY], w razie potrzeby i inny wyświetlający wyróżnione cen, jeśli jego s mniejsza niż $20,00 lub tekstu, wywołaj dla oferty cen w inny sposób. Let s, Utwórz te funkcje w klasie związanej z kodem strony ASP.NET i nazwij je `DisplayProductNameAndDiscontinuedStatus` i `DisplayPrice`. Obie metody muszą zwracać HTML do renderowania jako ciąg, a oba muszą być oznaczone `Protected` (lub `Public`) w celu wywołania z części składni deklaratywnej strony ASP.NET. Następujący kod dla tych dwóch metod:


[!code-vb[Main](formatting-the-datalist-and-repeater-based-upon-data-vb/samples/sample3.vb)]

Należy pamiętać, że `DisplayProductNameAndDiscontinuedStatus` metody akceptuje wartości `productName` i `discontinued` danych pola jako wartości skalarne, podczas gdy `DisplayPrice` metoda przyjmuje `ProductsRow` wystąpienia (zamiast `unitPrice` wartość skalarną). Każda metoda będzie działać; Jednak jeśli funkcja formatowania działa z wartości skalarnych, które mogą zawierać bazy danych `NULL` wartości (takie jak `UnitPrice`; ani `ProductName` ani `Discontinued` Zezwalaj `NULL` wartości), specjalne należy zachować ostrożność podczas obsługi tych dane wejściowe skalarne.

W szczególności parametr wejściowy musi być typu `Object` ponieważ przychodzące wartość może być `DBNull` wystąpienia zamiast oczekiwany typ danych. Ponadto należy sprawdzić, aby określić, czy wartość przychodzącego jest bazą danych `NULL` wartość. Oznacza to czy możemy `DisplayPrice` metodę, aby zaakceptować ceny jako wartość skalarną, możemy d trzeba użyć poniższego kodu:


[!code-vb[Main](formatting-the-datalist-and-repeater-based-upon-data-vb/samples/sample4.vb)]

Należy pamiętać, że `unitPrice` parametr wejściowy jest typu `Object` oraz że instrukcji warunkowej został zmodyfikowany w celu upewnienia się, jeśli `unitPrice` jest `DBNull` lub nie. Ponadto, ponieważ `unitPrice` parametru wejściowego jest przekazywany jako `Object`, musi być rzutowane na wartość dziesiętną.

## <a name="step-2-calling-the-formatting-function-from-the-datalist-s-itemtemplate"></a>Krok 2: Wywoływanie funkcji formatowania z ItemTemplate DataList s

Formatowanie funkcje dodane do naszych klasie związanej z kodem strony ASP.NET liście nie zostanie do wywołania tych funkcji z DataList s formatowania `ItemTemplate`. Wywoływanie funkcji formatowania z szablonu, Umieść wywołanie funkcji w składni wiązania z danymi:


[!code-aspx[Main](formatting-the-datalist-and-repeater-based-upon-data-vb/samples/sample5.aspx)]

W DataList s `ItemTemplate` `ProductNameLabel` formantu etykiety Web obecnie Wyświetla nazwę produktu s, przypisując jego `Text` właściwości wynik z `<%# Eval("ProductName") %>`. W celu ich wyświetlania nazwę oraz tekstu [WYCOFANY], jeśli to konieczne, zaktualizuj składni deklaratywnej tak, aby zamiast tego przypisuje `Text` właściwości wartość elementu `DisplayProductNameAndDiscontinuedStatus` metody. Po tej czynności musi przekazywana nazwa produktu s i wycofane wartości przy użyciu `Eval("columnName")` składni. `Eval` Zwraca wartość typu `Object`, ale `DisplayProductNameAndDiscontinuedStatus` metoda oczekuje parametrów wejściowych typu `String` i `Boolean`; w związku z tym możemy rzutować wartości zwracanych przez `Eval` metody dla typów oczekiwanego parametru wejściowego, w następujący sposób:


[!code-aspx[Main](formatting-the-datalist-and-repeater-based-upon-data-vb/samples/sample6.aspx)]

W celu wyświetlenia ceny, firma Microsoft może po prostu ustawić `UnitPriceLabel` etykiety s `Text` właściwości do wartości zwracanej przez `DisplayPrice` — metoda, podobnie jak możemy została ona wyświetlana nazwa produktu s i [WYCOFANE] tekstu. Jednak zamiast przekazywanie `UnitPrice` jako parametr wejściowy skalarne, zamiast tego jest przekazywana w całej `ProductsRow` wystąpienie:


[!code-aspx[Main](formatting-the-datalist-and-repeater-based-upon-data-vb/samples/sample7.aspx)]

Wywołania funkcji formatowania w miejscu Poświęć chwilę, aby wyświetlić postęp naszych w przeglądarce. Na ekranie powinien wyglądać podobnie do rysunek 5 z produktami wycofane, łącznie z tekstem [WYCOFANY] i tych produktów, kosztów więcej niż $20,00 o ich ceny zastąpione tekst należy wywołania oferty cen.


[![Dla produktów kosztowne ceny jest zastępowany tekstu, wywołaj oferty cen](formatting-the-datalist-and-repeater-based-upon-data-vb/_static/image14.png)](formatting-the-datalist-and-repeater-based-upon-data-vb/_static/image13.png)

**Rysunek 5**: produktów kosztowne, ceny jest zastępowany tekstu, wywołaj oferty cen ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](formatting-the-datalist-and-repeater-based-upon-data-vb/_static/image15.png))


## <a name="summary"></a>Podsumowanie

Formatowanie zawartości formantu DataList lub elementu powtarzanego na podstawie danych można wykonywać przy użyciu dwóch metod. Pierwszy technika jest utworzenie programu obsługi zdarzeń dla `ItemDataBound` zdarzenie, które uruchamia się, ponieważ każdy rekord w źródle danych jest powiązany z nową `DataListItem` lub `RepeaterItem`. W `ItemDataBound` program obsługi zdarzeń, bieżący element s danych można zbadać i formatowanie, mogą być stosowane do zawartości szablonu lub, w przypadku `DataListItem` s do całego elementu.

Alternatywnie formatowania niestandardowych można uzyskać za pośrednictwem funkcji formatowania. Funkcja formatowania jest metoda, która może zostać wywołana z elementu DataList lub elementu powtarzanego s szablonów, które zwraca kod HTML, aby emitować w jego miejscu. Często zwracane przez funkcję formatowania kodu HTML jest określany przez wartości jest powiązany z bieżącego elementu. Wartości te mogą zostać przekazane do funkcji formatowania jako wartości skalarne lub przez przekazanie całego obiektu jest powiązany z elementem (takich jak `ProductsRow` wystąpienie).

Programowanie przyjemność!

## <a name="about-the-author"></a>Informacje o autorze

[Scott Bento](http://www.4guysfromrolla.com/ScottMitchell.shtml), autora siedmiu książek ASP/ASP.NET i twórcę z [4GuysFromRolla.com](http://www.4guysfromrolla.com), pracuje z technologii Microsoft Web od 1998. Scott działa jako niezależnego konsultanta trainer i składnika zapisywania. Jest jego najnowszej książki [ *Sams nauczyć się ASP.NET 2.0 w ciągu 24 godzin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Piotr można uzyskać pod adresem [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) lub za pośrednictwem jego blog, który znajduje się w temacie [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Specjalne podziękowania dla

Ten samouczek serii zostało sprawdzone przez wiele recenzentów przydatne. Prowadzić osób dokonujących przeglądu, w tym samouczku zostały Yaakov Ellis Randy Schmidt i Liz Shulok. Zainteresowani recenzowania Moje nadchodzących artykuły MSDN? Jeśli tak, Porzuć mnie linii w [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Poprzednie](displaying-data-with-the-datalist-and-repeater-controls-vb.md)
> [dalej](showing-multiple-records-per-row-with-the-datalist-control-vb.md)
