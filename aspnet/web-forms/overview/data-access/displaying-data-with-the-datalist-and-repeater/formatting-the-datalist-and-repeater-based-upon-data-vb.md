---
uid: web-forms/overview/data-access/displaying-data-with-the-datalist-and-repeater/formatting-the-datalist-and-repeater-based-upon-data-vb
title: Formatowanie elementów DataList i Repeater na podstawie danych (VB) | Dokumentacja firmy Microsoft
author: rick-anderson
description: W tym samouczku będziemy czynności przykładów jak możemy formatowanie wyglądu kontrolki DataList i Repeater, albo za pomocą funkcji formatowania za pomocą...
ms.author: aspnetcontent
ms.date: 09/13/2006
ms.assetid: e2f401ae-37bb-4b19-aa97-d6b385d40f88
msc.legacyurl: /web-forms/overview/data-access/displaying-data-with-the-datalist-and-repeater/formatting-the-datalist-and-repeater-based-upon-data-vb
msc.type: authoredcontent
ms.openlocfilehash: 428438b2bae062c09d13c002f4729c3c394975a1
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/05/2018
ms.locfileid: "37807713"
---
<a name="formatting-the-datalist-and-repeater-based-upon-data-vb"></a>Formatowanie elementów DataList i Repeater na podstawie danych (VB)
====================
przez [Bento Scott](https://twitter.com/ScottOnWriting)

[Pobierz przykładową aplikację](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_30_VB.exe) lub [Pobierz plik PDF](formatting-the-datalist-and-repeater-based-upon-data-vb/_static/datatutorial30vb1.pdf)

> W tym samouczku będziemy czynności przykładów jak możemy formatowanie wyglądu kontrolki DataList i Repeater, za pomocą funkcji formatowania w szablonach lub dzięki obsłudze zdarzeń powiązanych z danymi.


## <a name="introduction"></a>Wprowadzenie

Jak widzieliśmy w poprzednim samouczku kontrolki DataList oferuje szereg właściwości stylu związane, które wpływają na jego wyglądu. W szczególności widzieliśmy jak przypisać domyślnej klasy CSS do DataList s `HeaderStyle`, `ItemStyle`, `AlternatingItemStyle`, i `SelectedItemStyle` właściwości. Oprócz tych czterech właściwości kontrolki DataList obejmuje szereg innych właściwości stylu związane, takie jak `Font`, `ForeColor`, `BackColor`, i `BorderWidth`, kilka. W kontrolce elementu powtarzanego nie zawiera żadnych właściwości stylu związane. Takie ustawienia stylu muszą być wykonane bezpośrednio z poziomu kodu znaczników w szablonach s elementu powtarzanego.

Często jednak sposób formatowania danych zależy od samych danych. Na przykład podczas wyświetlania listy produktów firma Microsoft może mają być wyświetlane informacje o produkcie koloru światła szara czcionka jest ona obsługiwana, czy może chcesz wyróżnić `UnitsInStock` wartość, gdy jest równa 0. Jak widzieliśmy w poprzednich samouczkach GridView DetailsView i FormView oferuje dwa odrębne sposoby formatowania ich występowania na podstawie swoich danych:

- **`DataBound` Zdarzeń** utworzyć program obsługi zdarzeń dla odpowiedniego `DataBound` zdarzenie, które są generowane po danych została powiązana z każdego elementu (dla widoku GridView był `RowDataBound` zdarzenia; DataList i Repeater jest `ItemDataBound`zdarzeń). W takim przypadku program obsługi, po prostu dane powiązane można zbadać i formatowanie decyzje wprowadzone. Zbadaliśmy tej techniki w [niestandardowe formatowanie oparte na danych](../custom-formatting/custom-formatting-based-upon-data-vb.md) samouczka.
- **Formatowanie funkcji w szablonach** po używanie kontrolek TemplateField w DetailsView lub GridView formantów lub szablonu w kontrolce FormView, możemy dodać funkcję formatowania do klasy związane z kodem strony ASP.NET, warstwy logiki biznesowej lub dowolny inne biblioteki klas, który jest dostępny z poziomu aplikacji sieci web. Ta funkcja formatowania może akceptować dowolnej liczby parametrów wejściowych, ale musi zwracać HTML do renderowania w szablonie. Najpierw zostały poddane funkcji formatowania [za pomocą kontrolek TemplateField w kontrolce GridView](../custom-formatting/using-templatefields-in-the-gridview-control-vb.md) samouczka.

Obie te metody formatowania są dostępne za pomocą kontrolek DataList i Repeater. W tym samouczku będziemy czynności przykłady użycia obu tych technik dla obu kontrolek.

## <a name="using-theitemdataboundevent-handler"></a>Za pomocą`ItemDataBound`program obsługi zdarzeń

Gdy powiązania danych z kontrolką DataList z kontroli źródła danych lub za pośrednictwem programowo przypisywania danych do kontrolki s `DataSource` właściwości i wywoływania jego `DataBind()` metody DataList s `DataBinding` wyzwala zdarzenia, źródło danych, wyliczenia, a każdy rekord danych jest powiązany z kontrolki DataList. Dla każdego rekordu w źródle danych tworzy kontrolki DataList [ `DataListItem` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalistitem.aspx) obiekt, a następnie powiązane z bieżącym rekordem. W trakcie tego procesu w elemencie DataList zgłasza dwa zdarzenia:

- **`ItemCreated`** generowane po `DataListItem` została utworzona
- **`ItemDataBound`** generowane po bieżącego rekordu została powiązana z `DataListItem`

Poniższe kroki wchodzą w skład procesu powiązanie danych kontrolki DataList.

1. DataList s [ `DataBinding` zdarzeń](https://msdn.microsoft.com/library/system.web.ui.control.databinding.aspx) generowane
2. Powiązania danych do kontrolki DataList  
  
   Dla każdego rekordu w źródle danych 

    1. Utwórz `DataListItem` obiektu
    2. Ogień [ `ItemCreated` zdarzeń](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalist.itemcreated.aspx)
    3. Rekord do powiązania `DataListItem`
    4. Ogień [ `ItemDataBound` zdarzeń](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalist.itemdatabound.aspx)
    5. Dodaj `DataListItem` do `Items` kolekcji

Powiązanie danych z kontrolką elementu powtarzanego, jej postępu przy użyciu dokładnie tych samych sekwencji kroków. Jedyną różnicą jest to, że zamiast `DataListItem` używa powtarzanego wystąpień tworzonych [ `RepeaterItem` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.repeateritem(VS.80).aspx)s.

> [!NOTE]
> Uważni czytnika, być może zauważono nieznaczne anomalii między sekwencję czynności, które orzeczona podczas DataList i Repeater są powiązane z danymi, a gdy widoku GridView jest powiązana z danymi. Na końcu tail proces wiązania danych, zgłasza widoku GridView `DataBound` zdarzenia; jednak kontrolki DataList ani elementu powtarzanego mają takie zdarzenie. Jest to spowodowane kontrolek DataList i Repeater zostały utworzone w przedziale czasu 1.x ASP.NET, zanim wzorzec procedury obsługi zdarzeń przed i po poziomu stało się typowe.


Tak, jak przy użyciu GridView, jedną z opcji formatowania, na podstawie danych jest utworzenie zdarzenia obsługi dla `ItemDataBound` zdarzeń. Ta procedura obsługi zdarzeń może sprawdzić dane, które było po prostu została powiązana z `DataListItem` lub `RepeaterItem` i wpływają na formatowanie kontrolki, zgodnie z potrzebami.

Kontrolki DataList zmiany formatowania dla całego elementu można zaimplementować przy użyciu `DataListItem` s stylu związane właściwości, które zawierają standardowe `Font`, `ForeColor`, `BackColor`, `CssClass`i tak dalej. Wpływ na formatowanie określonej kontrolki sieci Web w szablonie s DataList, musimy programowo uzyskać dostęp i zmodyfikować styl te kontrolki sieci Web. Widzieliśmy sposób wykonania tego ponownie w *niestandardowe formatowanie oparte na danych* samouczka. Kontrolce elementu powtarzanego, takich jak `RepeaterItem` klasa nie ma stylu związane właściwości; w związku z tym, wszystkie zmiany dotyczące stylu wprowadzone `RepeaterItem` w `ItemDataBound` programu obsługi zdarzeń musi odbywać się przez programowo dostęp i aktualizowanie formantów sieci Web w ciągu szablon.

Ponieważ `ItemDataBound` formatowanie elementów DataList i Repeater są niemal identyczne, nasz przykład koncentruje się na za pomocą kontrolki DataList techniki.

## <a name="step-1-displaying-product-information-in-the-datalist"></a>Krok 1: Wyświetlanie informacji o produkcie w elemencie DataList

Zanim będziemy zajmować formatowania, umożliwiają s najpierw utworzyć stronę, która korzysta z kontrolką DataList do wyświetlania informacji o produkcie. W [poprzedniego samouczka](displaying-data-with-the-datalist-and-repeater-controls-vb.md) utworzyliśmy kontrolką DataList którego `ItemTemplate` wyświetlana każda nazwa produktu s, kategorii, dostawcy, ilość na jednostkę, a cena. Pozwól, s, powtórz tę funkcję, w tym miejscu, w tym samouczku. Aby to osiągnąć, albo można odtworzyć kontrolki DataList i jego ObjectDataSource od podstaw, możesz także skopiować za pośrednictwem tych kontrolek ze strony utworzony w poprzednim samouczku (`Basics.aspx`) i wklej je do strony, w tym samouczku (`Formatting.aspx`).

Gdy zostały zreplikowane funkcji kontrolek DataList i kontrolki ObjectDataSource `Basics.aspx` do `Formatting.aspx`, Poświęć chwilę, aby zmienić DataList s `ID` właściwość `DataList1` na bardziej opisową `ItemDataBoundFormattingExample`. Następnie Wyświetl kontrolki DataList w przeglądarce. Jak pokazano na rysunku 1, formatowania jedyną różnicą między każdego produktu jest, że kolor tła alternatywne.


[![Produkty są wymienione w kontrolce DataList](formatting-the-datalist-and-repeater-based-upon-data-vb/_static/image2.png)](formatting-the-datalist-and-repeater-based-upon-data-vb/_static/image1.png)

**Rysunek 1**: produkty są wymienione w kontrolce DataList ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](formatting-the-datalist-and-repeater-based-upon-data-vb/_static/image3.png))


W tym samouczku umożliwiają s formatowanie kontrolki DataList w taki sposób, że wszystkie produkty z ceną mniejszą niż 20,00 USD za będą mieć zarówno jego nazwy i Jednostka ceny wyróżniony żółtym.

## <a name="step-2-programmatically-determining-the-value-of-the-data-in-the-itemdatabound-event-handler"></a>Krok 2: Programowe określanie wartości danych w obsłudze zdarzeń ItemDataBound

Ponieważ tylko te produkty z ceną w obszarze 20,00 USD za będą mieć niestandardowe formatowanie zastosowane, firma Microsoft musi być możliwe ustalenie, w każdej cenie s produktu. Podczas wiązania danych z kontrolką DataList, kontrolki DataList wylicza rekordy źródła danych i dla każdego rekordu tworzy `DataListItem` wystąpienia rekordu źródła danych do powiązania `DataListItem`. Po określonemu rekordowi s danych została powiązana z bieżącego `DataListItem` obiektu DataList s `ItemDataBound` jest wyzwalane zdarzenie. Można utworzyć program obsługi zdarzeń dla tego zdarzenia sprawdzić wartości danych dla bieżącego `DataListItem` i na podstawie tych wartości, wprowadź niezbędne zmiany formatowania.

Utwórz `ItemDataBound` zdarzenia dla kontrolki DataList i Dodaj następujący kod:


[!code-vb[Main](formatting-the-datalist-and-repeater-based-upon-data-vb/samples/sample1.vb)]

While koncepcji i semantyka za DataList s `ItemDataBound` programu obsługi zdarzeń są takie same jak dla GridView s `RowDataBound` programu obsługi zdarzeń w *niestandardowe formatowanie oparte na danych* różni się składni w samouczku nieco. Gdy `ItemDataBound` generowane zdarzenie `DataListItem` tylko powiązane z danymi jest przekazywana do odpowiedniego programu obsługi zdarzeń za pośrednictwem `e.Item` (zamiast `e.Row`, podobnie jak w przypadku GridView s `RowDataBound` program obsługi zdarzeń). DataList s `ItemDataBound` obsługi zdarzenia generowane dla *każdego* wiersz dodany do DataList, w tym wiersze nagłówka, stopki i separatora wierszy. Jednak informacje o produkcie jest tylko powiązane wiersze danych. W związku z tym korzystając z `ItemDataBound` zdarzenie, aby sprawdzić dane powiązane z kontrolki DataList, musimy najpierw upewnij się, że firma Microsoft odnośnie do pracy z elementu danych. Można to osiągnąć, sprawdzając `DataListItem` s [ `ItemType` właściwość](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalistitem.itemtype.aspx), która może mieć jedną z [następujące wartości osiem](https://msdn.microsoft.com/library/system.web.ui.webcontrols.listitemtype.aspx):

- `AlternatingItem`
- `EditItem`
- `Footer`
- `Header`
- `Item`
- `Pager`
- `SelectedItem`
- `Separator`

Zarówno `Item` i `AlternatingItem``DataListItem` elementy danych korzeń DataList s s. Zakładając, że firma Microsoft odnośnie do pracy z `Item` lub `AlternatingItem`, dostępu do rzeczywistego `ProductsRow` wystąpienia, która została powiązana z bieżącego `DataListItem`. `DataListItem` s [ `DataItem` właściwość](https://msdn.microsoft.com/system.web.ui.webcontrols.datalistitem.dataitem.aspx) zawiera odwołanie do `DataRowView` obiektu, którego `Row` właściwość zawiera odwołanie do rzeczywistego `ProductsRow` obiektu.

Następnie możemy sprawdzić `ProductsRow` wystąpienia s `UnitPrice` właściwości. Od tabeli Produkty s `UnitPrice` pole umożliwia `NULL` wartości przed podjęciem próby uzyskania dostępu `UnitPrice` właściwość możemy powinny najpierw sprawdzić, czy ma `NULL` wartości przy użyciu `IsUnitPriceNull()` metody. Jeśli `UnitPrice` wartość nie jest `NULL`, możemy następnie sprawdź, czy jego s mniejszą niż 20,00 USD za. Jeśli tak jest rzeczywiście w obszarze 20,00 USD za, następnie należy zastosować niestandardowe formatowanie.

## <a name="step-3-highlighting-the-product-s-name-and-price"></a>Krok 3: Wyróżnianie Nazwa s produktu i cenę

Gdy wiemy, że cena produktu s jest mniejsza niż 20,00 USD za pozostaje aby wyróżnić jego nazwa i cena. Aby to osiągnąć, należy najpierw programowo odwołujemy się formantów etykiet w `ItemTemplate` , wyświetlanie, nazwa produktu s i ceny. Następnie należy je wyświetlić żółte tło. Informacje o formatowaniu tego mogą być stosowane przez bezpośrednie zmodyfikowanie etykiet `BackColor` właściwości (`LabelID.BackColor = Color.Yellow`); najlepiej, jeśli jednak wszystkich kwestii związanych z ekranu powinny być wyrażone za pomocą kaskadowych arkuszy stylów. W rzeczywistości mamy już arkusza stylów, zapewniająca żądanego formatowania zdefiniowane w `Styles.css`  -  `AffordablePriceEmphasis`, która została utworzona i omówione w *niestandardowe formatowanie oparte na danych* samouczek.

Aby zastosować formatowanie, wystarczy ustawić dwóch formantów Web etykiety `CssClass` właściwości `AffordablePriceEmphasis`, jak pokazano w poniższym kodzie:


[!code-vb[Main](formatting-the-datalist-and-repeater-based-upon-data-vb/samples/sample2.vb)]

Za pomocą `ItemDataBound` zakończona programu obsługi zdarzeń, ponownie `Formatting.aspx` strony w przeglądarce. Tak jak pokazano na rysunku 2, tych produktów z ceną w obszarze 20,00 USD za ma ich nazwy i ceny wyróżnione.


[![Te produkty mniej niż 20,00 USD za wyróżniono](formatting-the-datalist-and-repeater-based-upon-data-vb/_static/image5.png)](formatting-the-datalist-and-repeater-based-upon-data-vb/_static/image4.png)

**Rysunek 2**: te produkty mniej niż 20,00 USD za wyróżniono ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](formatting-the-datalist-and-repeater-based-upon-data-vb/_static/image6.png))


> [!NOTE]
> Ponieważ kontrolki DataList jest renderowany jako kod HTML `<table>`, jego `DataListItem` wystąpienia mają właściwości stylu związane, które można ustawić dotyczą konkretnego stylu cały element. Załóżmy, że Chcieliśmy, aby wyróżnić *całego* elementu żółty, gdy jej miesięczna cena był mniejszy niż 20,00 USD za, firma Microsoft może zastąpiono kod, do której odwołanie etykiety i ustaw ich `CssClass` właściwości za pomocą następującego kodu: `e.Item.CssClass = "AffordablePriceEmphasis"` (patrz rysunek 3).


`RepeaterItem` S, wchodzące w skład kontrolce elementu powtarzanego jednak don t oferują takie właściwości stylu. W związku z tym stosując formatowanie niestandardowe do powtarzanego wymaga od aplikacji ponownego obliczenia właściwości stylu kontrolki sieci Web w szablonach Repeater s, tak samo, jak zrobiliśmy na rysunku 2.


[![Cały element produktu jest wyróżniona na produkty w ramach 20,00 USD za](formatting-the-datalist-and-repeater-based-upon-data-vb/_static/image8.png)](formatting-the-datalist-and-repeater-based-upon-data-vb/_static/image7.png)

**Rysunek 3**: cały element produktu jest wyróżniona na produkty w ramach 20,00 USD za ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](formatting-the-datalist-and-repeater-based-upon-data-vb/_static/image9.png))


## <a name="using-formatting-functions-from-within-the-template"></a>Za pomocą funkcji formatowania w szablonie

W *za pomocą kontrolek TemplateField w kontrolce GridView* samouczek widzieliśmy jak zastosować niestandardowe formatowanie na podstawie danych za pomocą funkcji formatowania w kontrolce GridView TemplateField powiązanych z wierszami s GridView. Funkcja formatowania jest metodą, która może być wywołana z szablonu i zwraca kod HTML, który był emitowany w jego miejscu. Funkcji formatowania może znajdować się w klasie CodeBehind strony s ASP.NET lub być scentralizowane w plikach klas w `App_Code` folderze lub w osobnym projekcie biblioteki klas. Przenoszenie funkcji formatowania poza klasy CodeBehind s strony ASP.NET jest idealnym rozwiązaniem, jeśli zamierzasz korzystać z tej samej funkcji formatowania na wielu stronach ASP.NET lub w innych aplikacjach sieci web platformy ASP.NET.

Aby zademonstrować funkcje formatowania, umożliwiają s ma informacje o produkcie, które zawierają tekst [WYCOFANY] obok nazwy produktu s, jeśli jego s wycofane. Umożliwiają s są także jeśli żółty wyróżnione cena go s mniejszą niż 20,00 USD za (jak robiliśmy `ItemDataBound` przykład program obsługi zdarzeń); Jeśli cena jest 20,00 USD za lub nowszej, który pozwala s nie wyświetlają rzeczywista cena, ale zamiast tego tekstu, należy wywołać dla oferty ceny. Rysunek 4 przedstawiono zrzut ekranu produktów, wyświetlanie za te reguły formatowania, które zostały zastosowane.


[![Kosztowne produktów cena zostanie zastąpiony tekst, wywołaj oferty cen](formatting-the-datalist-and-repeater-based-upon-data-vb/_static/image11.png)](formatting-the-datalist-and-repeater-based-upon-data-vb/_static/image10.png)

**Rysunek 4**: kosztowne produktów, cena zostanie zastąpiony tekst, wywołaj oferty cen ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](formatting-the-datalist-and-repeater-based-upon-data-vb/_static/image12.png))


## <a name="step-1-create-the-formatting-functions"></a>Krok 1: Tworzenie funkcji formatowania

W tym przykładzie potrzebujemy dwóch funkcji formatowania, taki, który wyświetla nazwę produktu, wraz z tekstu [WYCOFANY], jeśli to konieczne i innym wyświetlającą wyróżnione ceny, jeśli jego s mniejszej niż 20,00 USD za lub tekstu, wywołaj oferty cen, w przeciwnym razie. Pozwól s tworzenia tych funkcji w klasie CodeBehind strony s ASP.NET i nazwij je `DisplayProductNameAndDiscontinuedStatus` i `DisplayPrice`. Obie metody musi zwracać HTML do renderowania jako ciąg i oba muszą być oznaczone `Protected` (lub `Public`) w celu można wywołać z część składni deklaratywnej strony ASP.NET. Następujący kod dla tych dwóch metod:


[!code-vb[Main](formatting-the-datalist-and-repeater-based-upon-data-vb/samples/sample3.vb)]

Należy pamiętać, że `DisplayProductNameAndDiscontinuedStatus` metoda akceptuje wartości `productName` i `discontinued` danych pól jako wartości skalarnych, natomiast `DisplayPrice` metoda przyjmuje `ProductsRow` wystąpienia (zamiast `unitPrice` wartość skalarna). Każda z tych metod będzie działać; Jednak jeśli przy użyciu wartości skalarnych, które mogą zawierać bazy danych działa funkcja formatowania `NULL` wartości (takie jak `UnitPrice`; ani `ProductName` ani `Discontinued` Zezwalaj na `NULL` wartości), specjalne należy zachować ostrożność podczas obsługi tych dane wejściowe skalarne.

W szczególności, parametr wejściowy musi być typu `Object` ponieważ przychodzące wartość może być `DBNull` wystąpieniem, a nie oczekiwany typ danych. Ponadto należy sprawdzić, aby ustalić, czy wartość przychodzącego jest bazą danych `NULL` wartość. Oznacza to jeśli Chcieliśmy `DisplayPrice` metodę, aby akceptować cena jako wartość skalarną, możemy d trzeba użyć następującego kodu:


[!code-vb[Main](formatting-the-datalist-and-repeater-based-upon-data-vb/samples/sample4.vb)]

Należy pamiętać, że `unitPrice` parametr wejściowy jest typu `Object` oraz że instrukcji warunkowej została zmodyfikowana, aby upewnić się, jeśli `unitPrice` jest `DBNull` czy nie. Ponadto, ponieważ `unitPrice` parametr wejściowy jest przekazywany jako `Object`, należy zrzutować na wartość dziesiętną.

## <a name="step-2-calling-the-formatting-function-from-the-datalist-s-itemtemplate"></a>Krok 2: Wywoływanie funkcji formatowania z właściwości ItemTemplate DataList s

Za pomocą funkcji formatowania, dodany do naszych ASP.NET strony s osobna klasa kodu pozostaje tylko do wywołania funkcji z DataList s formatowania `ItemTemplate`. Aby wywołać funkcję formatowania z szablonu, Umieść wywołanie funkcji w ramach składnia wiązania z danymi:


[!code-aspx[Main](formatting-the-datalist-and-repeater-based-upon-data-vb/samples/sample5.aspx)]

W elemencie DataList s `ItemTemplate` `ProductNameLabel` formant etykiety w sieci Web obecnie Wyświetla nazwę produktu s, przypisując jej `Text` właściwości wyniku z `<%# Eval("ProductName") %>`. W celu jej służy do wyświetlania nazwy oraz tekst [WYCOFANY], jeśli to konieczne, zaktualizuj składni deklaratywnej tak, aby zamiast tego przypisuje `Text` wartości właściwości z `DisplayProductNameAndDiscontinuedStatus` metody. Po wykonaniu tej czynności firma Microsoft musi przekazać nazwę produktu s i nieobsługiwane wartości za pomocą `Eval("columnName")` składni. `Eval` Zwraca wartość typu `Object`, ale `DisplayProductNameAndDiscontinuedStatus` metoda oczekuje parametrów wejściowych typu `String` i `Boolean`; w związku z tym, firma Microsoft rzutować wartości zwracanych przez `Eval` metodę, aby typy oczekiwanym parametrem wejściowym, w następujący sposób:


[!code-aspx[Main](formatting-the-datalist-and-repeater-based-upon-data-vb/samples/sample6.aspx)]

Aby wyświetlić ceny, możemy po prostu ustaw `UnitPriceLabel` etykiety s `Text` właściwości do wartości zwracanej przez `DisplayPrice` metodę, tak samo, jak firma Microsoft została do wyświetlania nazwy produktu s i [WYCOFANE] tekstu. Jednak zamiast w `UnitPrice` jako skalarna parametr wejściowy, zamiast tego przekazanie całego `ProductsRow` wystąpienie:


[!code-aspx[Main](formatting-the-datalist-and-repeater-based-upon-data-vb/samples/sample7.aspx)]

Przy użyciu wywołania do funkcji formatowania w miejscu Poświęć chwilę, aby wyświetlić postępach w przeglądarce. Ekran powinien wyglądać podobnie jak rysunek 5 za pomocą uwzględniałyby produkty, łącznie z tekstem [WYCOFANY] i tych produktów wyceny ponad 20,00 USD za posiadające ich ceny zastąpione tekstem, wywołanie oferty ceny.


[![Kosztowne produktów cena zostanie zastąpiony tekst, wywołaj oferty cen](formatting-the-datalist-and-repeater-based-upon-data-vb/_static/image14.png)](formatting-the-datalist-and-repeater-based-upon-data-vb/_static/image13.png)

**Rysunek 5**: kosztowne produktów, cena zostanie zastąpiony tekst, wywołaj oferty cen ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](formatting-the-datalist-and-repeater-based-upon-data-vb/_static/image15.png))


## <a name="summary"></a>Podsumowanie

Formatowanie zawartości w kontrolce DataList lub Repeater na podstawie danych można wykonywać przy użyciu dwóch technik. Pierwszą techniką jest utworzenie zdarzenia obsługi dla `ItemDataBound` zdarzenie, które uruchamia się, ponieważ każdy rekord w źródle danych jest powiązany z nową `DataListItem` lub `RepeaterItem`. W `ItemDataBound` procedura obsługi zdarzeń, można sprawdzić bieżące dane element s i formatowanie, mogą być stosowane do zawartości, szablonu lub, w przypadku `DataListItem` s danego całego elementu.

Alternatywnie niestandardowe formatowanie można realizować za pomocą funkcji formatowania. Funkcja formatowania jest metodą, która może być wywoływany w kontrolce DataList lub Repeater s szablonów, które zwraca kod HTML do emitowania w tym miejscu. Często zwrócona przez funkcję formatowania kodu HTML jest ustalany wartości związania z bieżącego elementu. Te wartości mogą być przekazywane do funkcji formatowania, jako wartości skalarne lub przez przekazanie całego obiektu, który jest powiązany z elementem (takie jak `ProductsRow` wystąpienie).

Wszystkiego najlepszego programowania!

## <a name="about-the-author"></a>Informacje o autorze

[Scott Bento](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor siedem ASP/ASP.NET książek i założycielem [4GuysFromRolla.com](http://www.4guysfromrolla.com), pracował nad przy użyciu technologii Microsoft Web od 1998 r. Scott działa jako niezależny Konsultant, trainer i składnika zapisywania. Jego najnowszą książkę Stephena [ *Sams uczyć się ASP.NET 2.0 w ciągu 24 godzin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). ADAM można z Tobą skontaktować w [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) lub za pośrednictwem jego blogu, który znajduje się w temacie [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Specjalne podziękowania dla

W tej serii samouczków został zrecenzowany przez wielu recenzentów pomocne. Wiodące osób dokonujących przeglądu, w tym samouczku zostały Yaakov Ellis Randy Schmidt i Liz Shulok. Zainteresowani zapoznaniem Moje kolejnych artykułów MSDN? Jeśli tak, Porzuć mnie linii w [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Poprzednie](displaying-data-with-the-datalist-and-repeater-controls-vb.md)
> [dalej](showing-multiple-records-per-row-with-the-datalist-control-vb.md)
