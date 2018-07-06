---
uid: web-forms/overview/data-access/custom-formatting/using-the-formview-s-templates-vb
title: Przy użyciu szablony widoku FormView (VB) | Dokumentacja firmy Microsoft
author: rick-anderson
description: W odróżnieniu od DetailsView FormView nie składa się z pól. Zamiast tego widoku FormView jest renderowany przy użyciu szablonów. W tym samouczku zajmiemy się przy użyciu F....
ms.author: aspnetcontent
ms.date: 03/31/2010
ms.assetid: 67b25f4c-2823-42b6-b07d-1d650b3fd711
msc.legacyurl: /web-forms/overview/data-access/custom-formatting/using-the-formview-s-templates-vb
msc.type: authoredcontent
ms.openlocfilehash: bb613158343e6845e583ed823e2a2c16f958b720
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/05/2018
ms.locfileid: "37838157"
---
<a name="using-the-formviews-templates-vb"></a>Przy użyciu szablony widoku FormView (VB)
====================
przez [Bento Scott](https://twitter.com/ScottOnWriting)

[Pobierz przykładową aplikację](http://download.microsoft.com/download/5/7/0/57084608-dfb3-4781-991c-407d086e2adc/ASPNET_Data_Tutorial_14_VB.exe) lub [Pobierz plik PDF](using-the-formview-s-templates-vb/_static/datatutorial14vb1.pdf)

> W odróżnieniu od DetailsView FormView nie składa się z pól. Zamiast tego widoku FormView jest renderowany przy użyciu szablonów. W tym samouczku zajmiemy się przy użyciu kontrolki FormView do przedstawienia mniej sztywne widoku danych.


## <a name="introduction"></a>Wprowadzenie

W samouczkach ostatnich dwóch widzieliśmy jak dostosować dane wyjściowe kontrolki GridView i DetailsView używanie kontrolek TemplateField. Kontrolek TemplateField zezwala na zawartość dla określonego pola, aby być wysoce dostosowane, ale w końcu GridView i DetailsView ma wygląd zamiast boxy, podobne do siatki. Umożliwia obsługę wielu scenariuszy układu siatki jest idealnym rozwiązaniem, ale czasami wyświetlania bardziej płynne, mniej sztywne jest wymagany. Podczas wyświetlania pojedynczy rekord, płynny układ jest możliwe przy użyciu kontrolki FormView.

W odróżnieniu od DetailsView FormView nie składa się z pól. Nie można dodać elementu BoundField lub TemplateField na kontrolce FormView. Zamiast tego widoku FormView jest renderowany przy użyciu szablonów. FormView należy traktować jako zawierający pojedynczy TemplateField kontrolki widoku szczegółów. FormView obsługuje następujące szablony:

- `ItemTemplate` używany do renderowania określonemu rekordowi wyświetlane w widoku FormView
- `HeaderTemplate` można określić opcjonalny nagłówek
- `FooterTemplate` można określić wiersz opcjonalne stopki
- `EmptyDataTemplate` gdy FormView `DataSource` nie ma żadnych rekordów `EmptyDataTemplate` jest używana zamiast `ItemTemplate` renderowania kodu znaczników kontrolki
- `PagerTemplate` może służyć do dostosowania interfejsu stronicowania dla FormViews, że włączono stronicowania
- `EditItemTemplate` / `InsertItemTemplate` Umożliwia dostosowywanie interfejsu edycji lub wstawianie interfejsu dla FormViews, który obsługuje takie funkcje

W tym samouczku zajmiemy się przy użyciu kontrolki FormView do przedstawienia mniej sztywne wyświetlania produktów. Zamiast pola nazwy, kategorii, dostawcy i tak dalej, FormView firmy `ItemTemplate` wyświetli te wartości przy użyciu kombinacji header element i `<table>` (patrz rysunek 1).


[![FormView dzieli układu siatki w DetailsView](using-the-formview-s-templates-vb/_static/image2.png)](using-the-formview-s-templates-vb/_static/image1.png)

**Rysunek 1**: FormView podziały poza widoczne Grid-Like układu w DetailsView ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](using-the-formview-s-templates-vb/_static/image3.png))


## <a name="step-1-binding-the-data-to-the-formview"></a>Krok 1: Powiązanie danych z widoku FormView

Otwórz `FormView.aspx` strony, a następnie przeciągnij kontrolce FormView z przybornika do projektanta. Najpierw dodaj FormView jest wyświetlana jako szarego pola, które poinstruowanie nam, `ItemTemplate` jest wymagana.


[![Nie można renderować widoku FormView w projektancie, dopóki nie podano właściwości ItemTemplate](using-the-formview-s-templates-vb/_static/image5.png)](using-the-formview-s-templates-vb/_static/image4.png)

**Rysunek 2**: FormView nie może być renderowany w Projektancie aż `ItemTemplate` podano ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](using-the-formview-s-templates-vb/_static/image6.png))


`ItemTemplate` Można tworzyć ręcznie (za pomocą składni deklaratywnej) lub mogą być tworzone automatycznie przez powiązanie FormView do kontroli źródła danych, za pomocą projektanta. To utworzony automatycznie `ItemTemplate` zawiera kod HTML, wyświetla nazwę każdego pola oraz etykiety formant, którego `Text` właściwość jest powiązana z wartością pola. Takie podejście również automatycznie — tworzy `InsertItemTemplate` i `EditItemTemplate`, które są wypełniane przy użyciu kontrolki wejściowe dla każdego pola danych zwracany przez kontrolę źródła danych.

Jeśli chcesz automatycznie utworzyć szablon, w tagu inteligentnego FormView dodać nowego formantu ObjectDataSource, który wywołuje `ProductsBLL` klasy `GetProducts()` metody. Spowoduje to utworzenie FormView z `ItemTemplate`, `InsertItemTemplate`, i `EditItemTemplate`. W widoku źródła Usuń `InsertItemTemplate` i `EditItemTemplate` , ponieważ nie jesteśmy zainteresowani tworzenie kontrolce FormView obsługującego edycji lub jeszcze wstawiania. Następnie czyści znaczników w ramach `ItemTemplate` tak, że mamy pustego do pracy z.

Jeśli czy raczej zbudowania `ItemTemplate` ręcznie, można dodawać i konfigurować kontrolki ObjectDataSource, przeciągając go z przybornika do projektanta. Jednak nie należy ustawiać FormView źródła danych przy użyciu projektanta. Zamiast tego należy przejść do widoku źródła i ręcznie ustawić FormView `DataSourceID` właściwości `ID` wartość kontrolki ObjectDataSource. Następnie należy ręcznie dodać `ItemTemplate`.

Niezależnie od tego, jakie podejście możesz skupię się na obszarze, w tym momencie usługi FormView oznaczeniu deklaracyjnym wyglądu:


[!code-aspx[Main](using-the-formview-s-templates-vb/samples/sample1.aspx)]

Poświęć chwilę, aby zaznaczyć pole wyboru Włącz stronicowania w tagu inteligentnego FormView; Spowoduje to dodanie `AllowPaging="True"` atrybutu składni deklaratywnej FormView. Ponadto należy ustawić `EnableViewState` wartość False dla właściwości.

## <a name="step-2-defining-theitemtemplates-markup"></a>Krok 2: Definiowanie`ItemTemplate`firmy znaczników

Za pomocą FormView powiązany z formantu ObjectDataSource i skonfigurowane do obsługi stronicowania jesteśmy gotowi do określania zawartości `ItemTemplate`. W tym samouczku, Przyjrzyjmy się nazwę produktu, wyświetlana w `<h3>` nagłówka. Poniżej użyjemy HTML `<table>` do wyświetlenia pozostałych właściwości produktu w tabeli cztery kolumny, gdzie pierwszy i trzeci kolumn listy nazw właściwości, a drugiego i czwartego ich wartości listy.

Ten kod znaczników można wprowadzać w za pośrednictwem interfejsu edycji szablonu FormView w Projektancie lub wprowadzić ręcznie, za pomocą składni deklaratywnej. Podczas pracy z szablonami I zazwyczaj szybszym rozwiązaniem współpracować bezpośrednio ze składni deklaratywnej, ale możesz skorzystać, niezależnie od techniki potrafisz najbardziej się.

Następujący kod przedstawia FormView oznaczeniu deklaracyjnym po `ItemTemplate`firmy struktury zostało ukończone:


[!code-aspx[Main](using-the-formview-s-templates-vb/samples/sample2.aspx)]

Należy zauważyć, że składnia wiązania danych - `<%# Eval("ProductName") %>`, na przykład można wstrzyknięte bezpośrednio do tego szablonu w danych wyjściowych. Oznacza to, że jej muszą nie można przypisać do kontrolkę typu etykieta `Text` właściwości. Na przykład, mamy `ProductName` wartość wyświetlana w `<h3>` elementu za pomocą `<h3><%# Eval("ProductName") %></h3>`, którego produktu Chai będą renderowane jako `<h3>Chai</h3>`.

`ProductPropertyLabel` i `ProductPropertyValue` klas CSS służą do określania stylu produktu nazw właściwości i wartości w `<table>`. Te klasy CSS są zdefiniowane w `Styles.css` i spowodować, że nazwy właściwości pogrubiony i wyrównany do prawej i Dodaj po prawej stronie, wypełnienie wartości właściwości.

Ponieważ istnieją CheckBoxFields nie udostępniono FormView, aby pokazać `Discontinued` wartości jako pole wyboru możemy dodać własną formant pola wyboru. `Enabled` Właściwość jest ustawiona na wartość False, dzięki czemu tylko do odczytu, a pole wyboru `Checked` właściwość jest powiązana wartość `Discontinued` pola danych.

Za pomocą `ItemTemplate` kompletne, informacje o produkcie jest wyświetlany w sposób bardziej płynny. Porównaj dane wyjściowe DetailsView ostatni samouczek (rysunek 3) z danych wyjściowych generowanych przez FormView w tym samouczku (rysunek 4).


[![Sztywnych dane wyjściowe DetailsView](using-the-formview-s-templates-vb/_static/image8.png)](using-the-formview-s-templates-vb/_static/image7.png)

**Rysunek 3**: sztywne dane wyjściowe DetailsView ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](using-the-formview-s-templates-vb/_static/image9.png))


[![Płynne FormView danych wyjściowych](using-the-formview-s-templates-vb/_static/image11.png)](using-the-formview-s-templates-vb/_static/image10.png)

**Rysunek 4**: dane wyjściowe FormView dynamiki ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](using-the-formview-s-templates-vb/_static/image12.png))


## <a name="summary"></a>Podsumowanie

Kontrolki GridView i DetailsView może mieć swoje dane wyjściowe dostosowane używanie kontrolek TemplateField, jednocześnie nadal powodować swoje dane w formacie siatki, boxy. W takich sytuacjach, gdy pojedynczy rekord potrzebuje mają być wyświetlane w układzie mniej sztywne FormView jest idealnym wyborem. Jak DetailsView FormView renderuje pojedynczy rekord z jego `DataSource`, ale w przeciwieństwie do DetailsView składa się tylko z szablonów, a nie obsługuje pola.

Jak widzieliśmy w tym samouczku FormView umożliwia bardziej elastycznym układzie przy wyświetlaniu pojedynczy rekord. W przyszłości samouczki zajmiemy się elementów DataList i Repeater mechanizmów kontrolnych, które oferują taki sam poziom elastyczności jako FormsView, ale będą mogli wyświetlić wiele rekordów (na przykład GridView).

Wszystkiego najlepszego programowania!

## <a name="about-the-author"></a>Informacje o autorze

[Scott Bento](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor siedem ASP/ASP.NET książek i założycielem [4GuysFromRolla.com](http://www.4guysfromrolla.com), pracował nad przy użyciu technologii Microsoft Web od 1998 r. Scott działa jako niezależny Konsultant, trainer i składnika zapisywania. Jego najnowszą książkę Stephena [ *Sams uczyć się ASP.NET 2.0 w ciągu 24 godzin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). ADAM można z Tobą skontaktować w [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) lub za pośrednictwem jego blogu, który znajduje się w temacie [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Specjalne podziękowania dla

W tej serii samouczków został zrecenzowany przez wielu recenzentów pomocne. Weryfikacja potencjalnych klientów w ramach tego samouczka został E.R. Gilmore. Zainteresowani zapoznaniem Moje kolejnych artykułów MSDN? Jeśli tak, Porzuć mnie linii w [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Poprzednie](using-templatefields-in-the-detailsview-control-vb.md)
> [dalej](displaying-summary-information-in-the-gridview-s-footer-vb.md)
