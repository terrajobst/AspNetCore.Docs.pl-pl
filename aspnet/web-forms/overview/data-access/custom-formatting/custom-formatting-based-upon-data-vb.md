---
uid: web-forms/overview/data-access/custom-formatting/custom-formatting-based-upon-data-vb
title: Niestandardowe formatowanie na podstawie danych (VB) | Dokumentacja firmy Microsoft
author: rick-anderson
description: Dopasowywanie formatu widoku GridView, widoku DetailsView lub FormView w oparciu o dane powiązane z nim można wykonywać na wiele sposobów. W tym samouczku wyślemy wiadomość l...
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/31/2010
ms.topic: article
ms.assetid: df5a1525-386f-4632-972c-57b199870bc3
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/custom-formatting/custom-formatting-based-upon-data-vb
msc.type: authoredcontent
ms.openlocfilehash: a5c7f99b863697cc49a5bc9831dae861f51e129d
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
---
<a name="custom-formatting-based-upon-data-vb"></a>Niestandardowe formatowanie na podstawie danych (VB)
====================
przez [Bento Scott](https://twitter.com/ScottOnWriting)

[Pobierz przykładową aplikację](http://download.microsoft.com/download/5/7/0/57084608-dfb3-4781-991c-407d086e2adc/ASPNET_Data_Tutorial_11_VB.exe) lub [pobierania plików PDF](custom-formatting-based-upon-data-vb/_static/datatutorial11vb1.pdf)

> Dopasowywanie formatu widoku GridView, widoku DetailsView lub FormView w oparciu o dane powiązane z nim można wykonywać na wiele sposobów. W tym samouczku wyjaśniono, jak wykonywać powiązany formatowanie danych przy użyciu obsługi zdarzeń powiązanych z danymi i RowDataBound.


## <a name="introduction"></a>Wprowadzenie

Za pomocą różnych właściwości powiązanych z styl można dostosować wygląd kontrolki GridView widoku DetailsView i FormView. Właściwości, takie jak `CssClass`, `Font`, `BorderWidth`, `BorderStyle`, `BorderColor`, `Width`, i `Height`, między innymi określać ogólny wygląd formantu renderowany. Właściwości w tym `HeaderStyle`, `RowStyle`, `AlternatingRowStyle`, a inne umożliwiają te same ustawienia styl ma zostać zastosowany do poszczególnych sekcji. Analogicznie można zastosować te ustawienia stylu na poziomie pola.

W wielu scenariuszach, formatowania wymagania zależą od wartości wyświetlanych danych. Na przykład, aby zwrócić uwagę na z akcji produktów, raport zawierający listę informacji o produkcie ustawić kolor tła, który żółty dla tych produktów, których `UnitsInStock` i `UnitsOnOrder` pola są takie same na 0. Aby wyróżnić droższe produktów, firma Microsoft może mają być wyświetlane ceny tych produktów, kosztów więcej niż 75.00 $ pogrubioną czcionką.

Dopasowywanie formatu widoku GridView, widoku DetailsView lub FormView w oparciu o dane powiązane z nim można wykonywać na wiele sposobów. W tym samouczku wyjaśniono, jak wykonywać powiązany formatowanie danych za pośrednictwem `DataBound` i `RowDataBound` procedury obsługi zdarzeń. W następnym samouczku firma Microsoft będzie Poznaj informacje o innym podejściu.

## <a name="using-the-detailsview-controlsdataboundevent-handler"></a>Używanie formantu widoku DetailsView`DataBound`obsługi zdarzeń

Gdy danych jest powiązany Element DetailsView z kontroli źródła danych lub programowo przypisując danych z formantem `DataSource` właściwości i wywołanie jego `DataBind()` występuje następująca sekwencja kroków metody:

1. Formant sieci Web danych `DataBinding` generowane zdarzenie.
2. Dane jest powiązany z danymi formantu sieci Web.
3. Formant sieci Web danych `DataBound` generowane zdarzenie.

Niestandardowa logika mogą zostać dodane natychmiast po wykonaniu kroków 1 i 3, za pomocą programu obsługi zdarzeń. Tworząc programu obsługi zdarzeń dla `DataBound` zdarzeń możemy programowo określić dane, które ma zostać powiązany do kontroli danych w sieci Web i dostosować formatowanie zgodnie z potrzebami. Aby zilustrować ten umożliwia tworzenie widoku DetailsView, który wyświetla ogólne informacje o produkcie, ale będą wyświetlane `UnitPrice` wartość w ***czcionki Pogrubienie, Kursywa*** w razie przekroczenia 75.00 $.

## <a name="step-1-displaying-the-product-information-in-a-detailsview"></a>Krok 1: Wyświetlanie informacji o produkcie, w widoku DetailsView

Otwórz `CustomColors.aspx` strony `CustomFormatting` folderu, przeciągnij formant widoku DetailsView z przybornika do projektanta, ustaw jej `ID` wartości właściwości do `ExpensiveProductsPriceInBoldItalic`i powiązać ją z nowej kontrolki ObjectDataSource, który wywołuje `ProductsBLL` klasy `GetProducts()` metody. Szczegółowy opis kroków realizacji tego zadania zostały pominięte w tym miejscu w celu jego skrócenia ponieważ firma Microsoft ich szczegółowo zbadane w poprzednim samouczki.

Po elemencie ObjectDataSource został powiązany z widoku DetailsView, Poświęć chwilę, aby zmodyfikować listy pól. I została wybrana opcja usunąć `ProductID`, `SupplierID`, `CategoryID`, `UnitsInStock`, `UnitsOnOrder`, `ReorderLevel`, i `Discontinued` BoundFields zmieniona i ponownie sformatowany BoundFields pozostałych. Można również wyczyszczenie `Width` i `Height` ustawienia. Ponieważ w widoku DetailsView jest wyświetlany tylko jeden rekord, należy włączyć stronicowanie, aby umożliwić użytkownikom końcowym wyświetlanie wszystkich produktów. To zrobić, zaznaczając pole wyboru Włącz stronicowanie w widoku DetailsView tagów inteligentnych.


[![Rysunek 1: Pole wyboru Włącz stronicowanie w widoku DetailsView tagów inteligentnych](custom-formatting-based-upon-data-vb/_static/image2.png)](custom-formatting-based-upon-data-vb/_static/image1.png)

**Rysunek 1**: rysunku 1: pole wyboru Włącz stronicowanie w widoku DetailsView tagów inteligentnych ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](custom-formatting-based-upon-data-vb/_static/image3.png))


Po wprowadzeniu tych zmian znaczników widoku DetailsView będą:


[!code-aspx[Main](custom-formatting-based-upon-data-vb/samples/sample1.aspx)]

Poświęć chwilę, aby przetestować tę stronę w przeglądarce.


[![Wyświetla jeden produkt formantu widoku DetailsView](custom-formatting-based-upon-data-vb/_static/image5.png)](custom-formatting-based-upon-data-vb/_static/image4.png)

**Rysunek 2**: widoku DetailsView kontroli Wyświetla jeden produkt jednocześnie ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](custom-formatting-based-upon-data-vb/_static/image6.png))


## <a name="step-2-programmatically-determining-the-value-of-the-data-in-the-databound-event-handler"></a>Krok 2: Programowane określania wartości danych w obsłudze zdarzeń powiązanych z danymi

Aby wyświetlić ceny pogrubienie, kursywa czcionki dla tych produktów których `UnitPrice` $75.00 przekracza wartość, należy najpierw można programowo określić `UnitPrice` wartość. Dla widoku DetailsView, można to zrobić w `DataBound` obsługi zdarzeń. Aby utworzyć zdarzenia programu obsługi kliknij w widoku DetailsView w projektancie, a następnie przejdź do okna właściwości. Naciśnij klawisz F4 w celu nawiązania połączenia, jeśli nie jest widoczne, lub przejdź do menu Widok i wybierz opcję menu okna właściwości. W oknie właściwości kliknij ikonę błyskawicy Aby wyświetlić listę zdarzeń DetailsView. Następnie, albo kliknij dwukrotnie `DataBound` zdarzenia lub wpisz nazwę programu obsługi zdarzeń, które chcesz utworzyć.


![Tworzenie procedury obsługi zdarzeń dla zdarzenia z danymi](custom-formatting-based-upon-data-vb/_static/image7.png)

**Rysunek 3**: Tworzenie procedury obsługi zdarzeń dla `DataBound` zdarzeń


> [!NOTE]
> Można również utworzyć program obsługi zdarzeń z części kodu strony ASP.NET. Można znaleźć listy rozwijanej w górnej części strony. Wybierz obiekt z listy rozwijanej po lewej stronie i zdarzenia chcesz utworzyć program obsługi z tej listy rozwijanej listy i Visual Studio automatycznie utworzy programu obsługi zdarzeń odpowiednie.


W ten sposób zostanie automatycznie utworzyć programu obsługi zdarzeń i przejście do części kodu gdy zostało ono dodane. W tym momencie zostanie wyświetlony:


[!code-vb[Main](custom-formatting-based-upon-data-vb/samples/sample2.vb)]

Dane powiązane z widoku DetailsView jest możliwy za pośrednictwem `DataItem` właściwości. Odwołaj możemy powiązanie naszych formantów do silnie typizowane DataTable, który składa się z kolekcją jednoznacznie wystąpienia elementu DataRow. Gdy element DataTable jest powiązana z widoku DetailsView, pierwszy element DataRow w elemencie DataTable jest przypisany do DetailsView `DataItem` właściwości. W szczególności `DataItem` przypisano właściwości `DataRowView` obiektu. Możemy użyć `DataRowView`w `Row` właściwości, aby uzyskać dostęp do źródłowego obiektu DataRow jest rzeczywiście `ProductsRow` wystąpienia. Gdy to mamy `ProductsRow` wystąpienia firma Microsoft może wprowadzać naszej decyzji, sprawdzając, czy po prostu wartości właściwości obiektu.

Poniższy kod ilustruje sposób określić, czy `UnitPrice` wartość powiązane z formantem widoku DetailsView jest większa niż $75.00:


[!code-vb[Main](custom-formatting-based-upon-data-vb/samples/sample3.vb)]

> [!NOTE]
> Ponieważ `UnitPrice` może mieć `NULL` wartość w bazie danych, najpierw musimy sprawdzić upewnij się, że firma Microsoft nie jest pracy nad `NULL` wartość przed uzyskaniem dostępu do `ProductsRow`w `UnitPrice` właściwości. To sprawdzenie ważne jest, ponieważ firma Microsoft podejmie próbę uzyskania dostępu `UnitPrice` właściwości, gdy ma `NULL` wartość `ProductsRow` zgłosi obiektu [strongtypingexception — wyjątek](https://msdn.microsoft.com/library/system.data.strongtypingexception.aspx).


## <a name="step-3-formatting-the-unitprice-value-in-the-detailsview"></a>Krok 3: Formatowania wartości UnitPrice w widoku DetailsView

W tym momencie można określić, czy `UnitPrice` wartość powiązany z widoku DetailsView ma wartość, która przekracza $75.00, ale mamy jeszcze i sprawdź, jak programowo dostosowanie widoku DetailsView formatowanie odpowiednio. Aby zmodyfikować formatowanie całego wiersza w widoku DetailsView, programowo dostęp za pomocą wiersza `DetailsViewID.Rows(index)`; Aby zmodyfikować określonej komórki, dostęp do użycia `DetailsViewID.Rows(index).Cells(index)`. Gdy istnieje odwołanie do wiersza lub komórki możemy następnie dostosować wygląd przez ustawienie właściwości związanych z stylu.

Programowane uzyskiwanie dostępu do wiersza wymaga, aby poznać indeks wiersza, który rozpoczyna się od 0. `UnitPrice` Wiersz jest piątego wiersza w widoku DetailsView, nadanie mu indeksu 4 i udostępnienie jej programowo przy użyciu `ExpensiveProductsPriceInBoldItalic.Rows(4)`. Na tym etapie firma Microsoft może mieć zawartości cały wiersz wyświetlone czcionką pogrubienie, kursywa przy użyciu następującego kodu:


[!code-vb[Main](custom-formatting-based-upon-data-vb/samples/sample4.vb)]

Jednak będzie *zarówno* etykietę (cena) i wartość pogrubionego oraz pochylonego. Jeśli chcemy mieć tylko wartość pogrubionego oraz pochylonego musimy zastosowania formatowania do drugiej komórce wiersza, który może być wykonywane przy użyciu następujących:


[!code-vb[Main](custom-formatting-based-upon-data-vb/samples/sample5.vb)]

Ponieważ naszymi samouczkami pory używano arkusze stylów do obsługi czyste rozdzielenie renderowanego kodu znaczników i informacje dotyczące stylu, zamiast właściwości styl zgodnie z powyższym umożliwia zamiast tego użyć klasę CSS. Otwórz `Styles.css` arkusza stylów i Dodaj nową klasę CSS o nazwie `ExpensivePriceEmphasis` z następującej definicji:


[!code-css[Main](custom-formatting-based-upon-data-vb/samples/sample6.css)]

Następnie w `DataBound` program obsługi zdarzeń, ustaw komórki `CssClass` właściwości `ExpensivePriceEmphasis`. Poniższy kod przedstawia `DataBound` obsługi zdarzeń w całości:


[!code-vb[Main](custom-formatting-based-upon-data-vb/samples/sample7.vb)]

Podczas wyświetlania Chai, które koszty mniej niż 75.00 $, ceny jest wyświetlany w normalnym czcionki (patrz rysunek 4). Jednak podczas wyświetlania Niku Kobe Mishi, mającej ceny $97.00 ceny jest wyświetlany w czcionki Pogrubienie, Kursywa (patrz rysunek 5).


[![Ceny mniej niż $75.00 są wyświetlane w Normalna czcionka](custom-formatting-based-upon-data-vb/_static/image9.png)](custom-formatting-based-upon-data-vb/_static/image8.png)

**Rysunek 4**: ceny mniej niż $75.00 są wyświetlane w czcionki normalny ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](custom-formatting-based-upon-data-vb/_static/image10.png))


[![Ceny kosztowne produktów są wyświetlane w pogrubienie, kursywa czcionki](custom-formatting-based-upon-data-vb/_static/image12.png)](custom-formatting-based-upon-data-vb/_static/image11.png)

**Rysunek 5**: ceny kosztowne produktów są wyświetlane w pogrubienie, kursywa czcionki ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](custom-formatting-based-upon-data-vb/_static/image13.png))


## <a name="using-the-formview-controlsdataboundevent-handler"></a>Używanie formantu FormView`DataBound`obsługi zdarzeń

Kroki określania danych powiązany FormView są identyczne jak Utwórz element DetailsView `DataBound` program obsługi zdarzeń, rzutowania `DataItem` właściwości odpowiedni typ obiektu powiązanego z formantem i określić sposób kontynuować. FormView i widoku DetailsView różnią się jednak, w sposób aktualizowania wyglądu w swoim interfejsie użytkownika.

FormView nie zawiera żadnych BoundFields i dlatego nie ma `Rows` kolekcji. Zamiast tego FormView składa się z szablonów, które mogą zawierać kombinację Statycznych, formantów sieci Web i składnia wiązania z danymi. Dostosowywanie styl FormView zwykle obejmuje dostosowywania styl co najmniej jednego formantów sieci Web w szablonach FormView.

Na przykład załóżmy Użyj FormView do listy produktów, takich jak w poprzednim przykładzie, ale tym razem umożliwia wyświetlanie tylko nazwa produktu i jednostki w magazynie z jednostki w magazynie wyświetlana czerwona czcionki, jeśli jest mniejsza lub równa 10.

## <a name="step-4-displaying-the-product-information-in-a-formview"></a>Krok 4: Wyświetlanie informacji o produkcie w widoku FormView

Dodaj FormView do `CustomColors.aspx` strony poniżej widoku DetailsView i ustaw jej `ID` właściwości `LowStockedProductsInRed`. Powiązać FormView kontrolki ObjectDataSource utworzony w poprzednim kroku. Spowoduje to utworzenie `ItemTemplate`, `EditItemTemplate`, i `InsertItemTemplate` dla widoku FormView. Usuń `EditItemTemplate` i `InsertItemTemplate` i uproszczenia `ItemTemplate` uwzględnienie tylko `ProductName` i `UnitsInStock` wartości każdej z nich w ich własnych formantów etykiet odpowiednio o nazwie. Podobnie jak w przypadku widoku DetailsView z wcześniejszego przykładu również pole wyboru Włącz stronicowanie w tagu FormView.

Po te operacje edycji znaczników sieci FormView powinien wyglądać podobny do następującego:


[!code-aspx[Main](custom-formatting-based-upon-data-vb/samples/sample8.aspx)]

Należy pamiętać, że `ItemTemplate` zawiera:

- **Statycznych** tekst "produktu:" i "jednostki w magazynie:" wraz z `<br />` i `<b>` elementy.
- **Formanty sieci Web** dwóch formantów etykiet, `ProductNameLabel` i `UnitsInStockLabel`.
- **Składnia wiązania z danymi** `<%# Bind("ProductName") %>` i `<%# Bind("UnitsInStock") %>` składni przypisuje wartości z tych pól do formantów etykiet `Text` właściwości.

## <a name="step-5-programmatically-determining-the-value-of-the-data-in-the-databound-event-handler"></a>Krok 5: Programowane określania wartości danych w obsłudze zdarzeń powiązanych z danymi

Adiustację FormView pełną, następnym krokiem jest programowo Ustal, czy `UnitsInStock` wartość jest mniejsza niż 10. Jest to osiągane dokładnie tak samo jak z FormView jak z widoku DetailsView. Rozpocznij od utworzenia programu obsługi zdarzeń dla FormView `DataBound` zdarzeń.


![Tworzenie obsługi zdarzeń powiązanych z danymi](custom-formatting-based-upon-data-vb/_static/image14.png)

**Rysunek 6**: tworzenie `DataBound` obsługi zdarzeń


W przypadku obsługi rzutowania FormView `DataItem` właściwości `ProductsRow` wystąpienia i określić czy `UnitsInPrice` wartość jest taka, że potrzebujemy go wyświetlić w czerwonym czcionki.


[!code-vb[Main](custom-formatting-based-upon-data-vb/samples/sample9.vb)]

## <a name="step-6-formatting-the-unitsinstocklabel-label-control-in-the-formviews-itemtemplate"></a>Krok 6: Formatowanie formantu etykiety UnitsInStockLabel w FormView ItemTemplate

Ostatnim krokiem jest format wyświetlone `UnitsInStock` wartość w red czcionki, jeśli wartość wynosi 10 lub mniej. Potrzebujemy programowy dostęp w tym celu `UnitsInStockLabel` kontroli w `ItemTemplate` i ustawienia swoich właściwości stylu, dzięki czemu jego tekst jest wyświetlany na czerwono. Aby uzyskać dostęp do sieci Web formantu w szablonie, należy użyć `FindControl("controlID")` metody następująco:


[!code-vb[Main](custom-formatting-based-upon-data-vb/samples/sample10.vb)]

W naszym przykładzie chcemy uzyskać dostępu do etykiety kontroli, których `ID` wartość jest `UnitsInStockLabel`, dlatego firma Microsoft użyje:


[!code-vb[Main](custom-formatting-based-upon-data-vb/samples/sample11.vb)]

Po mamy programowe odwołanie do formantu sieci Web, firma Microsoft odpowiednio zmienić właściwości związanych z stylu. Zgodnie z poprzedniego przykładu, został utworzony klasę CSS w `Styles.css` o nazwie `LowUnitsInStockEmphasis`. Aby zastosować ten styl formantu etykiety w sieci Web, ustaw jej `CssClass` właściwości odpowiednio.


[!code-vb[Main](custom-formatting-based-upon-data-vb/samples/sample12.vb)]

> [!NOTE]
> Składnia formatowanie szablonu programowane uzyskiwanie dostępu za pomocą formantu sieci Web `FindControl("controlID")` , a następnie ustawienie właściwości związanych z styl można również przy użyciu [TemplateFields](https://msdn.microsoft.com/library/system.web.ui.webcontrols.templatefield(VS.80).aspx) w widoku DetailsView lub widoku GridView formanty. W naszym samouczku dalej zajmiemy się TemplateFields.


7 ilustracji przedstawiono FormView podczas wyświetlania produktu którego `UnitsInStock` wartość jest większa niż 10, a jego wartość mniejsza niż 10 produktu na rysunku 8.


[![W przypadku produktów z wystarczająco duże jednostki w magazynie jest stosowana bez formatowania niestandardowych](custom-formatting-based-upon-data-vb/_static/image16.png)](custom-formatting-based-upon-data-vb/_static/image15.png)

**Rysunek 7**: dla produktów z wystarczająco duże jednostki w magazynie, formatowanie niestandardowe nie są stosowane ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](custom-formatting-based-upon-data-vb/_static/image17.png))


[![Jednostki w magazynie jest zaznaczone na czerwono dla tych produktów z wartości 10 lub mniej](custom-formatting-based-upon-data-vb/_static/image19.png)](custom-formatting-based-upon-data-vb/_static/image18.png)

**Rysunek 8**: jednostki w magazynie jest zaznaczone na czerwono dla tych produktów z wartości 10 lub mniej ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](custom-formatting-based-upon-data-vb/_static/image20.png))


## <a name="formatting-with-the-gridviewsrowdataboundevent"></a>Formatowanie w widoku GridView`RowDataBound`zdarzeń

Wcześniej możemy zbadać sekwencja kroków widoku DetailsView i FormView kontrolki postępu za pośrednictwem podczas wiązania z danymi. Oto za pośrednictwem tych kroków ponownie jako odświeżacz.

1. Formant sieci Web danych `DataBinding` generowane zdarzenie.
2. Dane jest powiązany z danymi formantu sieci Web.
3. Formant sieci Web danych `DataBound` generowane zdarzenie.

Te trzy proste kroki są wystarczające dla widoku DetailsView i FormView, ponieważ są one wyświetlane tylko jeden rekord. Dla widoku GridView, który zawiera *wszystkie* powiązane rekordy do niego (nie tylko w pierwszym), jest nieco bardziej skomplikowane krok 2.

W kroku 2 widoku GridView wylicza źródła danych i dla każdego rekordu, tworzy `GridViewRow` wystąpienia i wiąże bieżącego rekordu. Dla każdego `GridViewRow` wywoływane dodawane do widoku GridView, dwa zdarzenia:

- **`RowCreated`** generowane po `GridViewRow` został utworzony
- **`RowDataBound`** generowane po bieżącego rekordu została powiązana z `GridViewRow`.

Dla widoku GridView następnie powiązanie danych jest dokładniej opisanego przez następująca sekwencja kroków:

1. W widoku GridView `DataBinding` generowane zdarzenie.
2. Dane widoku GridView jest powiązane.   
  
   Dla każdego rekordu w źródle danych 

    1. Utwórz `GridViewRow` obiektu
    2. Fire `RowCreated` zdarzeń
    3. Powiąż rekordu `GridViewRow`
    4. Fire `RowDataBound` zdarzeń
    5. Dodaj `GridViewRow` do `Rows` kolekcji
3. W widoku GridView `DataBound` generowane zdarzenie.

Aby dostosować format poszczególne rekordy w widoku GridView, następnie należy utworzyć programu obsługi zdarzeń dla `RowDataBound` zdarzeń. Na przykład Dodajmy GridView do `CustomColors.aspx` strony, która zawiera nazwę, kategorii i ceny dla każdego produktu wyróżnianie te produkty, których cena jest mniejsza niż 10,00 $ kolorem żółtym tle.

## <a name="step-7-displaying-product-information-in-a-gridview"></a>Krok 7: Wyświetlanie informacji o produkcie w widoku GridView

Dodaj element GridView poniżej FormView z poprzedniego przykładu i ustaw jej `ID` właściwości `HighlightCheapProducts`. Ponieważ mamy już ObjectDataSource, która zwraca wszystkich produktów na stronie, w tym należy powiązać widoku GridView. Na koniec edytować w widoku GridView BoundFields w celu uwzględnienia tylko te produkty nazwy kategorii i ceny. Po te zmiany w widoku GridView znaczników powinien wyglądać następująco:


[!code-aspx[Main](custom-formatting-based-upon-data-vb/samples/sample13.aspx)]

Na rysunku nr 9 przedstawiono naszych postęp w tym punkcie podczas wyświetlania za pośrednictwem przeglądarki.


[![Widoku GridView wyświetlane nazwa, kategoria i ceny dla każdego produktu](custom-formatting-based-upon-data-vb/_static/image22.png)](custom-formatting-based-upon-data-vb/_static/image21.png)

**Rysunek 9**: widoku GridView wyświetlane nazwa, kategoria i ceny dla każdego produktu ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](custom-formatting-based-upon-data-vb/_static/image23.png))


## <a name="step-8-programmatically-determining-the-value-of-the-data-in-the-rowdatabound-event-handler"></a>Krok 8: Programowane określania wartości danych w obsłudze zdarzeń RowDataBound

Gdy `ProductsDataTable` jest powiązany z widoku GridView jego `ProductsRow` wystąpienia są wyliczone i dla każdego `ProductsRow` `GridViewRow` jest tworzony. `GridViewRow`w `DataItem` właściwości jest przypisany do określonej `ProductRow`, po którym w widoku GridView `RowDataBound` jest uruchamiany program obsługi zdarzeń. Aby określić `UnitPrice` wartość dla każdego produktu powiązany z widoku GridView, a następnie, należy utworzyć program obsługi zdarzeń dla w widoku GridView `RowDataBound` zdarzeń. W tej obsłudze zdarzeń możemy sprawdzić `UnitPrice` wartość dla bieżącego `GridViewRow` i podejmowania decyzji formatowania dla danego wiersza.

Ten program obsługi zdarzeń mogą być tworzone przy użyciu tej samej serii kroków z FormView i widoku DetailsView.


![Tworzenie procedury obsługi zdarzeń dla zdarzenia RowDataBound w widoku GridView](custom-formatting-based-upon-data-vb/_static/image24.png)

**Na rysunku nr 10**: Tworzenie procedury obsługi zdarzeń dla w widoku GridView `RowDataBound` zdarzeń


Tworzenie programu obsługi zdarzeń w ten sposób spowoduje, że następujący kod, aby automatycznie dodany do strony ASP.NET części kodu:


[!code-vb[Main](custom-formatting-based-upon-data-vb/samples/sample14.vb)]

Gdy `RowDataBound` generowane zdarzenie programu obsługi zdarzeń jest przekazywany jako drugi parametr typu obiektu `GridViewRowEventArgs`, który zawiera właściwości o nazwie `Row`. Ta właściwość zwraca odwołanie do `GridViewRow` który został właśnie powiązane z danymi. Aby uzyskać dostęp do `ProductsRow` powiązane wystąpienie `GridViewRow` używamy `DataItem` właściwości w następujący sposób:


[!code-vb[Main](custom-formatting-based-upon-data-vb/samples/sample15.vb)]

Podczas pracy z `RowDataBound` należy pamiętać, że widoku GridView składa się z różnych typów wierszy oraz że to zdarzenie jest generowane dla programu obsługi zdarzeń *wszystkie* wiersz typów. A `GridViewRow`tego typu można ustalić przy jego `RowType` właściwości i może mieć jedną z możliwych wartości:

- `DataRow` wiersz, który jest powiązany z rekordu w widoku GridView `DataSource`
- `EmptyDataRow` wiersz wyświetlana, jeśli w widoku GridView `DataSource` jest pusty
- `Footer` Wiersz stopki; Jeśli wyświetlane w widoku GridView `ShowFooter` ma ustawioną wartość właściwości `True`
- `Header` wiersz nagłówka; wyświetlany, jeśli właściwość ShowHeader w widoku GridView jest ustawiona na `True` (ustawienie domyślne)
- `Pager` w widoku GridView implementują stronicowanie, wiersza, który wyświetla interfejs stronicowania
- `Separator` nie jest używana dla widoku GridView, ale używany przez `RowType` właściwości DataList i powtarzanego steruje dwóch danych omówiono w przyszłości samouczki formantów sieci Web

Ponieważ `EmptyDataRow`, `Header`, `Footer`, i `Pager` wierszy nie są skojarzone z `DataSource` rekordów, będzie zawsze mają wartość `Nothing` dla ich `DataItem` właściwości. Z tego powodu przed przystąpieniem do pracy z bieżącym `GridViewRow`w `DataItem` właściwości, możemy najpierw należy się upewnić, że firma Microsoft jest zajmujących się `DataRow`. Można to osiągnąć poprzez sprawdzenie `GridViewRow`w `RowType` właściwości w następujący sposób:


[!code-vb[Main](custom-formatting-based-upon-data-vb/samples/sample16.vb)]

## <a name="step-9-highlighting-the-row-yellow-when-the-unitprice-value-is-less-than-1000"></a>Krok 9: Wyróżnianie żółty po UnitPrice wartość wiersza jest mniejsza niż 10,00 $

Ostatnim krokiem jest programowo Podświetl cały `GridViewRow` Jeśli `UnitPrice` wartość dla tego wiersza jest mniejsza niż 10,00 $. Składnia do uzyskiwania dostępu do komórek lub wierszy w widoku GridView jest taka sama, jak w widoku DetailsView `GridViewID.Rows(index)` dostępu cały wiersz do `GridViewID.Rows(index).Cells(index)` można uzyskać dostępu do określonej komórki. Jednakże, gdy `RowDataBound` obsługi zdarzenia generowane powiązane z danych `GridViewRow` jeszcze nie można dodać do widoku GridView `Rows` kolekcji. W związku z tym nie masz dostępu do bieżącego `GridViewRow` wystąpienia z `RowDataBound` obsługi zdarzeń przy użyciu kolekcji wierszy.

Zamiast `GridViewID.Rows(index)`, firma Microsoft może się odwoływać bieżącego `GridViewRow` wystąpienia w `RowDataBound` przy użyciu programu obsługi zdarzeń `e.Row`. Oznacza to, aby wyróżnić bieżącego `GridViewRow` wystąpienia z `RowDataBound` możemy użyć obsługi zdarzenia:


[!code-vb[Main](custom-formatting-based-upon-data-vb/samples/sample17.vb)]

Zamiast `GridViewRow`w `BackColor` właściwości bezpośrednio, umożliwia przestrzegaj przy użyciu klas CSS. Utworzono klasę CSS o nazwie `AffordablePriceEmphasis` żółty który ustawia kolor tła. Wypełniony `RowDataBound` sposób obsługi zdarzeń:


[!code-vb[Main](custom-formatting-based-upon-data-vb/samples/sample18.vb)]


[![Najbardziej ekonomiczny produkty są wyróżnione żółty](custom-formatting-based-upon-data-vb/_static/image26.png)](custom-formatting-based-upon-data-vb/_static/image25.png)

**Rysunek 11**: najbardziej ekonomiczny produkty są wyróżnione żółty ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](custom-formatting-based-upon-data-vb/_static/image27.png))


## <a name="summary"></a>Podsumowanie

W tym samouczku widzieliśmy sposób formatowania GridView widoku DetailsView i FormView w oparciu o dane powiązane z formantem. W tym utworzyliśmy programu obsługi zdarzeń dla `DataBound` lub `RowDataBound` zdarzenia, w którym danych zbadano oraz zmiany formatowania, jeśli to konieczne. Uzyskiwanie dostępu do danych powiązanych z widoku DetailsView ani FormView, używamy `DataItem` właściwości w `DataBound` obsługi zdarzeń; Element GridView każdego `GridViewRow` wystąpienia `DataItem` właściwość zawiera dane powiązane z tego wiersza, który jest dostępny w `RowDataBound` obsługi zdarzeń.

Składnia programowo Dostosowywanie formatowania danych formantu sieci Web zależy od formantu sieci Web i sposób wyświetlania danych do sformatowania. Element DetailsView i GridView kontrolki, wierszy i komórek mogą uzyskiwać indeksem. Dla FormView, który korzysta z szablonów `FindControl("controlID")` metoda jest często używana do lokalizowania formantu sieci Web w szablonie.

W następnym samouczku wyjaśniono, jak używać szablonów z widoku GridView i widoku DetailsView. Ponadto firma Microsoft zostanie wyświetlone innej techniki dostosowywania formatowanie oparte na danych.

Programowanie przyjemność!

## <a name="about-the-author"></a>Informacje o autorze

[Scott Bento](http://www.4guysfromrolla.com/ScottMitchell.shtml), autora siedmiu książek ASP/ASP.NET i twórcę z [4GuysFromRolla.com](http://www.4guysfromrolla.com), pracuje z technologii Microsoft Web od 1998. Scott działa jako niezależnego konsultanta trainer i składnika zapisywania. Jest jego najnowszej książki [ *Sams nauczyć się ASP.NET 2.0 w ciągu 24 godzin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Piotr można uzyskać pod adresem [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) lub za pośrednictwem jego blog, który znajduje się w temacie [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Specjalne podziękowania dla

Ten samouczek serii zostało sprawdzone przez wiele recenzentów przydatne. Prowadzić osób dokonujących przeglądu, w tym samouczku zostały E.R. Gilmore, firmy Dennis Patterson i Dan Jagers. Zainteresowani recenzowania Moje nadchodzących artykuły MSDN? Jeśli tak, Porzuć mnie linii w [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Poprzednie](displaying-summary-information-in-the-gridview-s-footer-cs.md)
> [dalej](using-templatefields-in-the-gridview-control-vb.md)
