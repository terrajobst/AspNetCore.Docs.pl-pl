---
uid: web-forms/overview/data-access/custom-formatting/custom-formatting-based-upon-data-vb
title: Niestandardowe formatowanie na podstawie danych (VB) | Dokumentacja firmy Microsoft
author: rick-anderson
description: Dostosowywanie formatu GridView, DetailsView lub FormView w oparciu o dane powiązane z nim można przeprowadzić na wiele sposobów. W tym samouczku będziemy l...
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/31/2010
ms.topic: article
ms.assetid: df5a1525-386f-4632-972c-57b199870bc3
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/data-access/custom-formatting/custom-formatting-based-upon-data-vb
msc.type: authoredcontent
ms.openlocfilehash: 2ed55c181534e18bbbb4447ce9a4e5c19ff3717b
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37366203"
---
<a name="custom-formatting-based-upon-data-vb"></a>Niestandardowe formatowanie na podstawie danych (VB)
====================
przez [Bento Scott](https://twitter.com/ScottOnWriting)

[Pobierz przykładową aplikację](http://download.microsoft.com/download/5/7/0/57084608-dfb3-4781-991c-407d086e2adc/ASPNET_Data_Tutorial_11_VB.exe) lub [Pobierz plik PDF](custom-formatting-based-upon-data-vb/_static/datatutorial11vb1.pdf)

> Dostosowywanie formatu GridView, DetailsView lub FormView w oparciu o dane powiązane z nim można przeprowadzić na wiele sposobów. W tym samouczku omówimy sposób wykonania powiązane formatowanie danych przy użyciu procedur obsługi zdarzeń z danymi i RowDataBound.


## <a name="introduction"></a>Wprowadzenie

Można dostosować wygląd kontrolki GridView DetailsView i FormView przy użyciu różnych właściwości stylu związane. Właściwości, takie jak `CssClass`, `Font`, `BorderWidth`, `BorderStyle`, `BorderColor`, `Width`, i `Height`, między innymi, dyktowania ogólny wygląd formantu renderowany. Właściwości w tym `HeaderStyle`, `RowStyle`, `AlternatingRowStyle`, i inne umożliwiają te same ustawienia stylu mają być stosowane do poszczególnych sekcji. Podobnie można zastosować te ustawienia stylu na poziomie pola.

W wielu scenariuszach, formatowania wymagania zależą od wartości wyświetlanych danych. Na przykład do zwracania uwagi z akcji produktów, raport zawierający listę informacji o produkcie ustawić kolor tła na żółty dla tych produktów, których `UnitsInStock` i `UnitsOnOrder` pola są równe 0. Aby wyróżnić bardziej kosztowne produktów, firma Microsoft chce wyświetlić cen produktów wyceny ponad 75.00 $ pogrubioną czcionką.

Dostosowywanie formatu GridView, DetailsView lub FormView w oparciu o dane powiązane z nim można przeprowadzić na wiele sposobów. W tym samouczku omówimy sposób wykonania powiązane formatowanie danych za pośrednictwem `DataBound` i `RowDataBound` procedury obsługi zdarzeń. W następnym samouczku przedstawimy zagadnienia związane z alternatywnym podejściem.

## <a name="using-the-detailsview-controlsdataboundevent-handler"></a>Za pomocą kontrolce DetailsView`DataBound`program obsługi zdarzeń

Podczas wiązania danych do DetailsView z kontroli źródła danych lub za pośrednictwem programowo przypisywania danych do kontrolki `DataSource` właściwości i wywoływania jego `DataBind()` wystąpić poniższą sekwencję czynności metody:

1. Kontrolka sieci Web danych `DataBinding` generowane zdarzenie.
2. Dane są powiązane z danymi formantu sieci Web.
3. Kontrolka sieci Web danych `DataBound` generowane zdarzenie.

Niestandardowej logiki może zostać wprowadzone bezpośrednio po kroki 1 i 3 za pomocą programu obsługi zdarzeń. Tworząc program obsługi zdarzeń dla `DataBound` zdarzenia, można programowo określić dane, które ma zostać powiązany do kontroli danych w sieci Web i dostosować formatowanie zgodnie z potrzebami. W celu zilustrowania tego Załóżmy tworzenie DetailsView, która zostanie wyświetlona lista ogólne informacje o produkcie, ale będą wyświetlane `UnitPrice` wartość w ***czcionki pogrubienia, kursywy*** w przypadku przekroczenia 75.00 $.

## <a name="step-1-displaying-the-product-information-in-a-detailsview"></a>Krok 1: Wyświetlania informacji o produkcie DetailsView

Otwórz `CustomColors.aspx` strony w `CustomFormatting` folderu, przeciągnij kontrolce DetailsView z przybornika w projektancie, ustaw jego `ID` wartość właściwości `ExpensiveProductsPriceInBoldItalic`i powiązać ją z nowego formantu ObjectDataSource, który wywołuje `ProductsBLL` klasy `GetProducts()` metody. Szczegółowy opis kroków do realizacji tego zadania są pominiętą w tym miejscu dla zwięzłości, ponieważ firma Microsoft ich szczegółowo zbadane w poprzednich samouczkach.

Gdy kontrolki ObjectDataSource został powiązany z DetailsView, Poświęć chwilę na modyfikowanie listy pól. Czy załączania do usunięcia `ProductID`, `SupplierID`, `CategoryID`, `UnitsInStock`, `UnitsOnOrder`, `ReorderLevel`, i `Discontinued` BoundFields przekształcony w pozostałych BoundFields i zmieniono jego nazwę. Czy mogę również wyczyszczone `Width` i `Height` ustawienia. Ponieważ DetailsView jest wyświetlany tylko jeden rekord, musimy Włączanie stronicowania w celu umożliwiają użytkownikom końcowym wyświetlić wszystkie produkty. To zrobić, zaznaczając pole wyboru Włącz stronicowania w DetailsView tagu inteligentnego.


[![Rysunek 1: Włączanie stronicowania zaznacz pole wyboru w tagu inteligentnego DetailsView](custom-formatting-based-upon-data-vb/_static/image2.png)](custom-formatting-based-upon-data-vb/_static/image1.png)

**Rysunek 1**: rysunek 1: Włączanie stronicowania zaznacz pole wyboru w tagu inteligentnego DetailsView ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](custom-formatting-based-upon-data-vb/_static/image3.png))


Po wprowadzeniu tych zmian będzie znaczników DetailsView:


[!code-aspx[Main](custom-formatting-based-upon-data-vb/samples/sample1.aspx)]

Poświęć chwilę, w celu przetestowania tej strony w przeglądarce.


[![W kontrolce DetailsView Wyświetla jeden produkt w czasie](custom-formatting-based-upon-data-vb/_static/image5.png)](custom-formatting-based-upon-data-vb/_static/image4.png)

**Rysunek 2**: DetailsView kontrolka Wyświetla jeden produkt w danym momencie ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](custom-formatting-based-upon-data-vb/_static/image6.png))


## <a name="step-2-programmatically-determining-the-value-of-the-data-in-the-databound-event-handler"></a>Krok 2: Programowe określanie wartości danych w obsłudze zdarzeń powiązanych z danymi

Aby wyświetlić ceny czcionką pogrubienia, kursywy, dla tych produktów, których `UnitPrice` wartość przekracza $75.00, musimy najpierw można programowo określić `UnitPrice` wartość. Dla DetailsView, można to zrobić w `DataBound` programu obsługi zdarzeń. Można utworzyć zdarzenia obsługi kliknij DetailsView w projektancie, a następnie przejdź do okna właściwości. Naciśnij klawisz F4, aby wyświetlić go, jeśli nie jest widoczny czy przejdź do menu Widok, a następnie wybierz opcję menu okna właściwości. W oknie dialogowym właściwości kliknij ikonę pioruna na liście DetailsView zdarzenia. Następnie kliknij dwukrotnie `DataBound` zdarzenia lub wpisz nazwę programu obsługi zdarzeń, którą chcesz utworzyć.


![Tworzenie procedury obsługi zdarzeń dla zdarzenia z danymi](custom-formatting-based-upon-data-vb/_static/image7.png)

**Rysunek 3**: Tworzenie procedury obsługi zdarzeń dla `DataBound` zdarzeń


> [!NOTE]
> Można również utworzyć program obsługi zdarzeń z części kodu strony ASP.NET. Można znaleźć dwie listy rozwijanej w górnej części strony. Wybierz obiekt z listy rozwijanej po lewej stronie i zdarzeń, w której chcesz utworzyć program obsługi w prawo rozwijanej listy i programu Visual Studio automatycznie utworzy odpowiednie zdarzenie obsługi.


To spowoduje automatycznie utworzyć procedury obsługi zdarzeń i przejście do fragment kodu których został dodany. W tym momencie zostanie wyświetlony:


[!code-vb[Main](custom-formatting-based-upon-data-vb/samples/sample2.vb)]

Dane powiązane z DetailsView jest możliwy za pośrednictwem `DataItem` właściwości. Odwołaj, firma Microsoft powiązanie naszych formanty do silnie typizowane DataTable, który składa się z kolekcji wystąpień klasy DataRow silnie typizowane. Gdy DataTable jest powiązana z DetailsView, pierwszy wiersz danych w tabeli DataTable jest przypisany do DetailsView `DataItem` właściwości. W szczególności `DataItem` właściwość jest przypisana `DataRowView` obiektu. Możemy użyć `DataRowView`firmy `Row` właściwości, aby uzyskać dostęp do bazowego obiektu DataRow, który jest faktycznie `ProductsRow` wystąpienia. Gdy będziemy już mieć to `ProductsRow` wystąpienia, firma Microsoft ułatwia podjęcie decyzji, po prostu sprawdzając wartości właściwości obiektu.

Poniższy kod ilustruje sposób określić, czy `UnitPrice` powiązany z kontrolą DetailsView wartość jest większa niż $75.00:


[!code-vb[Main](custom-formatting-based-upon-data-vb/samples/sample3.vb)]

> [!NOTE]
> Ponieważ `UnitPrice` może mieć `NULL` wartość w bazie danych, najpierw zapoznaj się upewnić, że firma Microsoft nie są brane z `NULL` wartość przed uzyskaniem dostępu do `ProductsRow`firmy `UnitPrice` właściwości. Ważne jest to sprawdzenie, ponieważ jeśli firma Microsoft spróbuje uzyskać dostęp do `UnitPrice` właściwości, gdy ma ona `NULL` wartość `ProductsRow` zgłosi obiektu [strongtypingexception — wyjątek](https://msdn.microsoft.com/library/system.data.strongtypingexception.aspx).


## <a name="step-3-formatting-the-unitprice-value-in-the-detailsview"></a>Krok 3: Formatowanie wartość UnitPrice DetailsView

W tym momencie można określić czy `UnitPrice` wartość granicy do DetailsView ma wartość, która przekracza $75.00, ale zostały wykonane następujące kroki, ale aby zobaczyć, jak programowo ustawić DetailsView formatowanie odpowiednio. Aby zmodyfikować formatowanie całego wiersza w widoku DetailsView, programowego dostępu do modułu za pomocą wiersza polecenia `DetailsViewID.Rows(index)`; Aby zmodyfikować określonej komórki, dostęp do użycia `DetailsViewID.Rows(index).Cells(index)`. Gdy będziemy już mieć odwołanie do wiersza lub komórki możemy następnie dostosuj jego wygląd poprzez ustawienie jego właściwości stylu związane.

Programowe uzyskiwanie dostępu do wiersza wymaga, że znasz indeks wiersza, który zaczyna się od 0. `UnitPrice` Wiersz jest piąty wiersz DetailsView, nadając mu indeksu 4 i oznaczania go jako dostępny programowo przy użyciu `ExpensiveProductsPriceInBoldItalic.Rows(4)`. Na tym etapie firma Microsoft może mieć zawartości cały wiersz wyświetlana czcionką pogrubienia, kursywy, używając następującego kodu:


[!code-vb[Main](custom-formatting-based-upon-data-vb/samples/sample4.vb)]

Jednak dzięki temu *zarówno* etykietę (cena) i wartość pogrubiony i kursywę. Jeśli chcemy mieć tylko wartość pogrubiony i kursywę należy zastosować te odpowiadający ustawieniom lokalnym drugiej komórce wiersz, który można osiągnąć za pomocą następujących:


[!code-vb[Main](custom-formatting-based-upon-data-vb/samples/sample5.vb)]

Ponieważ naszych samouczków dotychczasowych korzystano arkuszy stylów do utrzymania czystą separacji między renderowanego kodu znaczników i informacje dotyczące stylu, zamiast ustawienie właściwości konkretnego stylu, jak pokazano powyżej Przyjrzyjmy zamiast tego użyj klasę CSS. Otwórz `Styles.css` arkusza stylów i Dodaj nową klasę CSS o nazwie `ExpensivePriceEmphasis` przy użyciu następujących definicji:


[!code-css[Main](custom-formatting-based-upon-data-vb/samples/sample6.css)]

Następnie w `DataBound` procedura obsługi zdarzeń, ustawianie komórki `CssClass` właściwość `ExpensivePriceEmphasis`. Poniższy kod przedstawia `DataBound` program obsługi zdarzeń w całości:


[!code-vb[Main](custom-formatting-based-upon-data-vb/samples/sample7.vb)]

Podczas wyświetlania Chai, której koszt mniej niż 75.00 $, cena jest wyświetlany czcionką normalną (zobacz rysunek 4). Jednak podczas wyświetlania Niku Kobe Mishi, mającej cenie $97.00 cena jest wyświetlana w czcionki pogrubienia, kursywy (zobacz rysunek 5).


[![Ceny opóźnienia mniejsze niż $75.00 są wyświetlane w czcionki normalny](custom-formatting-based-upon-data-vb/_static/image9.png)](custom-formatting-based-upon-data-vb/_static/image8.png)

**Rysunek 4**: ceny opóźnienia mniejsze niż $75.00 są wyświetlane w czcionki normalny ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](custom-formatting-based-upon-data-vb/_static/image10.png))


[![Ceny kosztowne produktów są wyświetlane w pogrubienie, kursywa czcionki](custom-formatting-based-upon-data-vb/_static/image12.png)](custom-formatting-based-upon-data-vb/_static/image11.png)

**Rysunek 5**: kosztowne produktów ceny są wyświetlane w pogrubienie, kursywa czcionki ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](custom-formatting-based-upon-data-vb/_static/image13.png))


## <a name="using-the-formview-controlsdataboundevent-handler"></a>Używanie formantu FormView`DataBound`program obsługi zdarzeń

Kroki oznaczania danych bazowych, powiązany z kontrolce FormView są identyczne z tymi, Utwórz element DetailsView `DataBound` rzutowania programu obsługi zdarzeń `DataItem` właściwość do typu odpowiedniego obiektu powiązanego z formantem i opracowaniu. FormView i DetailsView różnią się jednak, w sposób aktualizowania wygląd ich interfejsu użytkownika.

FormView nie zawiera żadnych BoundFields i dlatego nie ma `Rows` kolekcji. Zamiast tego kontrolce FormView składa się z szablonów, które mogą zawierać kombinację statyczny kod HTML, kontrolki sieci Web i składnia wiązania danych. Dostosowywanie stylu kontrolce FormView zwykle obejmuje dostosowywanie stylu co najmniej jeden z formantów sieci Web w obrębie szablony widoku FormView.

Na przykład Przyjrzyjmy Użyj FormView listę produktów, takich jak w poprzednim przykładzie, ale tym razem możemy wyświetlić tylko nazwę produktu i jednostek w magazynie przy użyciu jednostek w magazynie wyświetlana czcionką czerwony, jeśli jest on mniejszy niż lub równy 10.

## <a name="step-4-displaying-the-product-information-in-a-formview"></a>Krok 4: Wyświetlanie informacji o produkcie w kontrolce FormView

Dodaj FormView do `CustomColors.aspx` strony pod DetailsView i ustaw jego `ID` właściwość `LowStockedProductsInRed`. Powiąż FormView kontrolka ObjectDataSource, utworzony w poprzednim kroku. Spowoduje to utworzenie `ItemTemplate`, `EditItemTemplate`, i `InsertItemTemplate` dla widoku FormView. Usuń `EditItemTemplate` i `InsertItemTemplate` i uprościć `ItemTemplate` obejmujący tylko `ProductName` i `UnitsInStock` wartości, każde we własnych odpowiednio o silnych nazwach formantów etykiet. Podobnie jak w przypadku DetailsView z wcześniejszego przykładu, należy również sprawdzić pole wyboru Włącz stronicowania w tagu inteligentnego FormView.

Po edycji tych znaczników swoje FormView powinien wyglądać podobny do następującego:


[!code-aspx[Main](custom-formatting-based-upon-data-vb/samples/sample8.aspx)]

Należy pamiętać, że `ItemTemplate` zawiera:

- **Statyczny HTML** tekst "produkt:" i "jednostek w magazynie:" wraz z `<br />` i `<b>` elementów.
- **Formanty składników Web** dwóch formantów etykiet `ProductNameLabel` i `UnitsInStockLabel`.
- **Składnia wiązania danych** `<%# Bind("ProductName") %>` i `<%# Bind("UnitsInStock") %>` składni, która przypisuje wartości z tych pól do formantów etykiet `Text` właściwości.

## <a name="step-5-programmatically-determining-the-value-of-the-data-in-the-databound-event-handler"></a>Krok 5: Programowe określanie wartości danych w obsłudze zdarzeń powiązanych z danymi

Za pomocą znaczników FormView jest pełna, następnym krokiem jest programowo określić, czy `UnitsInStock` wartość jest mniejsza niż 10. Jest to realizowane w dokładnie taki sam sposób, przy użyciu widoku FormView, ponieważ był za pomocą DetailsView. Rozpocznij od utworzenia programu obsługi zdarzeń dla FormView `DataBound` zdarzeń.


![Utwórz procedurę obsługi zdarzeń z danymi](custom-formatting-based-upon-data-vb/_static/image14.png)

**Rysunek 6**: tworzenie `DataBound` program obsługi zdarzeń


W przypadku obsługi rzutowania FormView `DataItem` właściwości `ProductsRow` wystąpienia i określić, czy `UnitsInPrice` wartość jest taka, że potrzebujemy mogą być wyświetlane dla czerwonych czcionki.


[!code-vb[Main](custom-formatting-based-upon-data-vb/samples/sample9.vb)]

## <a name="step-6-formatting-the-unitsinstocklabel-label-control-in-the-formviews-itemtemplate"></a>Krok 6: Formatowanie formantu etykiety UnitsInStockLabel w ItemTemplate FormView

Ostatnim krokiem jest do formatowania wyświetlany `UnitsInStock` wartość czcionką czerwony, jeśli wartość wynosi 10 lub mniej. W tym potrzebujemy do uzyskania programowego dostępu `UnitsInStockLabel` w kontrolce `ItemTemplate` i ustaw jego właściwości stylu, tak aby jego tekst jest wyświetlany w kolorze czerwonym. Aby uzyskać dostęp do formantu sieci Web w taki sposób, w szablonie, należy użyć `FindControl("controlID")` metoda następująco:


[!code-vb[Main](custom-formatting-based-upon-data-vb/samples/sample10.vb)]

W tym przykładzie chcemy uzyskać dostęp etykietę formant, którego `ID` wartość `UnitsInStockLabel`, więc należy użyć:


[!code-vb[Main](custom-formatting-based-upon-data-vb/samples/sample11.vb)]

Gdy będziemy już mieć programowe odwołanie do formantu sieci Web, firma Microsoft można zmodyfikować jego właściwości stylu związane zgodnie z potrzebami. Zgodnie z wcześniejszym przykładzie utworzono klasę CSS z `Styles.css` o nazwie `LowUnitsInStockEmphasis`. Aby zastosować ten styl kontrolki sieci Web etykietę, ustaw jego `CssClass` właściwość odpowiednio.


[!code-vb[Main](custom-formatting-based-upon-data-vb/samples/sample12.vb)]

> [!NOTE]
> Składnia służąca do formatowania szablonu programowe uzyskiwanie dostępu za pomocą formantu sieci Web `FindControl("controlID")` oraz ustawienie jego właściwości stylu związane można również przy użyciu [kontrolek TemplateField](https://msdn.microsoft.com/library/system.web.ui.webcontrols.templatefield(VS.80).aspx) DetailsView lub widoku GridView kontrolki. Zajmiemy się kontrolek TemplateField w naszym samouczku dalej.


Rysunki 7 przedstawia FormView, podczas wyświetlania produktu którego `UnitsInStock` wartość jest większa niż 10, w przypadku, gdy produkt na rysunku 8 ma wartość mniejsza niż 10.


[![W przypadku produktów z wystarczająco duże jednostki w magazynie formatowanie niestandardowe nie jest stosowany](custom-formatting-based-upon-data-vb/_static/image16.png)](custom-formatting-based-upon-data-vb/_static/image15.png)

**Rysunek 7**: dla produktów z wystarczająco dużą jednostek w magazynie, formatowanie niestandardowe nie są stosowane ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](custom-formatting-based-upon-data-vb/_static/image17.png))


[![Jednostki w magazynie numer jest wyświetlany w kolorze czerwonym dla tych produktów za pomocą wartości 10 lub mniejsza](custom-formatting-based-upon-data-vb/_static/image19.png)](custom-formatting-based-upon-data-vb/_static/image18.png)

**Rysunek 8**: jednostki w magazynie numer jest wyświetlany w kolorze czerwonym dla tych produktów za pomocą wartości 10 lub mniejsza ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](custom-formatting-based-upon-data-vb/_static/image20.png))


## <a name="formatting-with-the-gridviewsrowdataboundevent"></a>GridView — mechanizm formatowania`RowDataBound`zdarzeń

Wcześniej zbadaliśmy sekwencji kroków DetailsView i FormView kontroluje postęp podczas wiązania z danymi. Przyjrzyjmy się za pośrednictwem tych kroków ponownie jako odświeżacz.

1. Kontrolka sieci Web danych `DataBinding` generowane zdarzenie.
2. Dane są powiązane z danymi formantu sieci Web.
3. Kontrolka sieci Web danych `DataBound` generowane zdarzenie.

Te trzy proste kroki są wystarczające dla DetailsView i FormView, ponieważ są one wyświetlane tylko jeden rekord. Dla widoku GridView, powoduje wyświetlenie *wszystkich* powiązane rekordy do niego (nie tylko w pierwszym) kroku 2 jest nieco bardziej skomplikowane.

W kroku 2 widoku GridView wylicza źródła danych i dla każdego rekordu tworzy `GridViewRow` wystąpienia i wiąże bieżącego rekordu. Dla każdego `GridViewRow` dodawane do widoku GridView, dwa zdarzenia są wywoływane:

- **`RowCreated`** generowane po `GridViewRow` została utworzona
- **`RowDataBound`** generowane po bieżącego rekordu została powiązana z `GridViewRow`.

Dla widoku GridView następnie wiązanie danych jest dokładniej opisana przez poniższą sekwencję czynności:

1. W widoku GridView `DataBinding` generowane zdarzenie.
2. Powiązania danych w kontrolce GridView.   
  
   Dla każdego rekordu w źródle danych 

    1. Utwórz `GridViewRow` obiektu
    2. Ogień `RowCreated` zdarzeń
    3. Rekord do powiązania `GridViewRow`
    4. Ogień `RowDataBound` zdarzeń
    5. Dodaj `GridViewRow` do `Rows` kolekcji
3. W widoku GridView `DataBound` generowane zdarzenie.

Aby dostosować format GridView pojedynczych rekordów, następnie należy utworzyć program obsługi zdarzeń dla `RowDataBound` zdarzeń. Na przykład możemy dodać GridView do `CustomColors.aspx` strona, która zawiera nazwy, kategorii i ceny dla każdego produktu, wyróżnianie tych produktów, których cena jest mniejsza niż 10,00 zł kolorem żółtym tłem.

## <a name="step-7-displaying-product-information-in-a-gridview"></a>Krok 7: Wyświetlanie informacji o produkcie w widoku GridView

Dodaj GridView poniżej FormView z poprzedniego przykładu, a następnie ustaw jego `ID` właściwość `HighlightCheapProducts`. Ponieważ kontrolki ObjectDataSource, które zwraca wszystkie produkty na stronie mamy już powiązać widoku GridView. Wreszcie edytować BoundFields GridView obejmujący tylko produktów nazwy, kategorii i ceny. Po edycji tych znaczników GridView powinien wyglądać:


[!code-aspx[Main](custom-formatting-based-upon-data-vb/samples/sample13.aspx)]

Rysunek 9 pokazuje nasz postęp do tego momentu podczas wyświetlania za pośrednictwem przeglądarki.


[![Kontrolki GridView Wyświetla nazwy, kategorii i ceny dla każdego produktu](custom-formatting-based-upon-data-vb/_static/image22.png)](custom-formatting-based-upon-data-vb/_static/image21.png)

**Rysunek 9**: widoku GridView Wyświetla nazwy, kategorii i ceny dla każdego produktu ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](custom-formatting-based-upon-data-vb/_static/image23.png))


## <a name="step-8-programmatically-determining-the-value-of-the-data-in-the-rowdatabound-event-handler"></a>Krok 8: Programowe określanie wartości danych w obsłudze zdarzeń RowDataBound

Gdy `ProductsDataTable` jest powiązany z kontrolki GridView jego `ProductsRow` wystąpienia są wyliczane i dla każdego `ProductsRow` `GridViewRow` zostanie utworzony. `GridViewRow`Firmy `DataItem` właściwość jest przypisany do określonych `ProductRow`, po upływie którego GridView `RowDataBound` program obsługi zdarzeń jest wywoływane. Aby określić `UnitPrice` widoku GridView powiązana wartość dla każdego produktu, a następnie, musimy utworzyć program obsługi zdarzeń dla GridView `RowDataBound` zdarzeń. W tej obsługi zdarzeń możemy sprawdzić `UnitPrice` wartość dla bieżącego `GridViewRow` i formatowania decyzji dla tego wiersza.

Ta procedura obsługi zdarzeń mogą być tworzone przy użyciu tego samego serię kroków, jak za pomocą FormView i DetailsView.


![Tworzenie procedury obsługi zdarzeń dla zdarzenia RowDataBound GridView](custom-formatting-based-upon-data-vb/_static/image24.png)

**Na rysunku nr 10**: Tworzenie procedury obsługi zdarzeń dla GridView `RowDataBound` zdarzeń


Tworzenie obsługi zdarzeń w ten sposób spowoduje, że następujący kod, aby były automatycznie dodawane do strony ASP.NET, fragment kodu:


[!code-vb[Main](custom-formatting-based-upon-data-vb/samples/sample14.vb)]

Gdy `RowDataBound` generowane zdarzenie programu obsługi zdarzeń jest przekazywany jako drugi parametr obiektu typu `GridViewRowEventArgs`, która ma właściwość o nazwie `Row`. Ta właściwość zwraca odwołanie do `GridViewRow` było po prostu powiązane z danymi. Aby uzyskać dostęp do `ProductsRow` wystąpienia jest powiązana z `GridViewRow` używamy `DataItem` właściwości w następujący sposób:


[!code-vb[Main](custom-formatting-based-upon-data-vb/samples/sample15.vb)]

Podczas pracy z `RowDataBound` ważne jest, aby pamiętać, że widoku GridView składa się z różnego rodzaju wierszy, a to zdarzenie jest generowane dla programu obsługi zdarzeń *wszystkich* wiersz typów. A `GridViewRow`firmy można określić typu przez jego `RowType` właściwości i może mieć jedną z możliwych wartości:

- `DataRow` wiersz, który jest powiązany z rekordem z GridView `DataSource`
- `EmptyDataRow` wiersz wyświetlany, gdy GridView `DataSource` jest pusty
- `Footer` Wiersz stopki; Jeśli pokazywane w widoku GridView `ShowFooter` właściwość jest ustawiona na `True`
- `Header` wiersz nagłówka; wyświetlane, jeśli GridView ShowHeader zostaje ustalona `True` (ustawienie domyślne)
- `Pager` dla GridView implementują stronicowania wiersza, który umożliwia wyświetlanie interfejsu stronicowania
- `Separator` nie jest używany dla widoku GridView, ale używany przez `RowType` właściwości kontrolek DataList i Repeater kontroluje danych dwóch formantów sieci Web, które omówimy w przyszłości samouczki

Ponieważ `EmptyDataRow`, `Header`, `Footer`, i `Pager` wierszy nie są skojarzone z `DataSource` rekordu, będzie zawsze mają wartość `Nothing` dla ich `DataItem` właściwości. Z tego powodu przed przystąpieniem do pracy z bieżącego `GridViewRow`firmy `DataItem` właściwości, firma Microsoft najpierw należy się upewnić, że firma Microsoft jest zajmujących się `DataRow`. Można to osiągnąć, sprawdzając `GridViewRow`firmy `RowType` właściwości w następujący sposób:


[!code-vb[Main](custom-formatting-based-upon-data-vb/samples/sample16.vb)]

## <a name="step-9-highlighting-the-row-yellow-when-the-unitprice-value-is-less-than-1000"></a>Krok 9: Wyróżnianie żółty po UnitPrice wartość wiersza jest mniejsza niż 10,00 zł

Ostatnim krokiem jest programowe podświetlić całą `GridViewRow` Jeśli `UnitPrice` wartość dla tego wiersza jest mniejsza niż 10,00 zł. Składnia służąca do uzyskiwania dostępu do wierszy w widoku GridView lub komórek jest taka sama, jak w przypadku DetailsView `GridViewID.Rows(index)` na dostęp do całego wiersza `GridViewID.Rows(index).Cells(index)` do uzyskania dostępu do konkretnych komórek. Jednak, gdy `RowDataBound` programu obsługi zdarzeń uruchamia powiązane z danych `GridViewRow` nie został jeszcze dodany do GridView `Rows` kolekcji. W związku z tym nie masz dostępu do bieżącego `GridViewRow` wystąpienia z `RowDataBound` obsługę zdarzeń z użyciem kolekcji wierszy.

Zamiast `GridViewID.Rows(index)`, firma Microsoft może odwoływać się do bieżącego `GridViewRow` wystąpienia w `RowDataBound` za pomocą programu obsługi zdarzeń `e.Row`. Oznacza to, w celu wyróżnienia bieżącego `GridViewRow` wystąpienia z `RowDataBound` program obsługi zdarzeń, należy użyć:


[!code-vb[Main](custom-formatting-based-upon-data-vb/samples/sample17.vb)]

Zamiast `GridViewRow`firmy `BackColor` właściwości bezpośrednio, możemy trzymaj się przy użyciu klas CSS. Utworzono klasę CSS o nazwie `AffordablePriceEmphasis` , ustawia kolor tła na żółty. Gotowy `RowDataBound` obsługi zdarzeń:


[!code-vb[Main](custom-formatting-based-upon-data-vb/samples/sample18.vb)]


[![Najbardziej niedrogie produkty są wyróżnione żółty](custom-formatting-based-upon-data-vb/_static/image26.png)](custom-formatting-based-upon-data-vb/_static/image25.png)

**Rysunek 11**: większość niedrogie produkty są wyróżniony żółtym ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](custom-formatting-based-upon-data-vb/_static/image27.png))


## <a name="summary"></a>Podsumowanie

W tym samouczku widzieliśmy sposób formatowania GridView DetailsView i FormView na podstawie danych powiązane z formantem. W tym utworzyliśmy program obsługi zdarzeń dla `DataBound` lub `RowDataBound` zdarzeń, gdzie dane bazowe zbadano wraz z zmiany formatowania, jeśli to konieczne. Dostępu do danych powiązany z DetailsView lub FormView, używamy `DataItem` właściwość `DataBound` programu obsługi zdarzeń; GridView, każdy `GridViewRow` wystąpienia `DataItem` właściwość zawiera dane powiązane z tym wierszu, który jest dostępny w `RowDataBound` programu obsługi zdarzeń.

Składnia programowo Dostosowywanie formatowania danych kontrolki sieci Web zależy od tego, kontrolki sieci Web i sposób wyświetlania danych do sformatowania. DetailsView i GridView kontrolki, wierszy i komórek jest możliwy przez numer porządkowy indeksu. Dla FormView, która korzysta z szablonów `FindControl("controlID")` metoda jest często używana do lokalizowania formantu sieci Web z w ramach szablonu.

W następnym samouczku omówimy sposób użycia szablonów z kontrolkami GridView i DetailsView. Ponadto zobaczymy inna technika Dostosowywanie formatowania, oparte na danych bazowych.

Wszystkiego najlepszego programowania!

## <a name="about-the-author"></a>Informacje o autorze

[Scott Bento](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor siedem ASP/ASP.NET książek i założycielem [4GuysFromRolla.com](http://www.4guysfromrolla.com), pracował nad przy użyciu technologii Microsoft Web od 1998 r. Scott działa jako niezależny Konsultant, trainer i składnika zapisywania. Jego najnowszą książkę Stephena [ *Sams uczyć się ASP.NET 2.0 w ciągu 24 godzin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). ADAM można z Tobą skontaktować w [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) lub za pośrednictwem jego blogu, który znajduje się w temacie [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Specjalne podziękowania dla

W tej serii samouczków został zrecenzowany przez wielu recenzentów pomocne. Wiodące osób dokonujących przeglądu, w tym samouczku zostały E.R. Gilmore, Dennis Patterson i Dan Jagers. Zainteresowani zapoznaniem Moje kolejnych artykułów MSDN? Jeśli tak, Porzuć mnie linii w [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Poprzednie](displaying-summary-information-in-the-gridview-s-footer-cs.md)
> [dalej](using-templatefields-in-the-gridview-control-vb.md)
