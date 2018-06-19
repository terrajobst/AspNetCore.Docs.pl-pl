---
uid: web-forms/overview/data-access/custom-formatting/using-the-formview-s-templates-cs
title: Za pomocą szablonów FormView (C#) | Dokumentacja firmy Microsoft
author: rick-anderson
description: W przeciwieństwie do widoku DetailsView FormView nie składa się z pola. Zamiast tego FormView jest renderowany przy użyciu szablonów. W tym samouczku zajmiemy się przy użyciu F....
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/31/2010
ms.topic: article
ms.assetid: d3f062af-88cf-426d-af44-e41f32c41672
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/custom-formatting/using-the-formview-s-templates-cs
msc.type: authoredcontent
ms.openlocfilehash: 0e1b36f0bfc244e39bb620c1c066b3e2403722cb
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
ms.locfileid: "30875828"
---
<a name="using-the-formviews-templates-c"></a>Za pomocą szablonów FormView (C#)
====================
przez [Bento Scott](https://twitter.com/ScottOnWriting)

[Pobierz przykładową aplikację](http://download.microsoft.com/download/9/6/9/969e5c94-dfb6-4e47-9570-d6d9e704c3c1/ASPNET_Data_Tutorial_14_CS.exe) lub [pobierania plików PDF](using-the-formview-s-templates-cs/_static/datatutorial14cs1.pdf)

> W przeciwieństwie do widoku DetailsView FormView nie składa się z pola. Zamiast tego FormView jest renderowany przy użyciu szablonów. W tym samouczku zajmiemy się do prezentowania mniej sztywne wyświetlania danych przy użyciu formantu FormView.


## <a name="introduction"></a>Wprowadzenie

W samouczkach ostatnich dwóch widzieliśmy jak dostosować za pomocą TemplateFields wyjść kontrolki GridView i widoku DetailsView. TemplateFields Zezwalaj zawartości dla określonego pola można zdecydowanie można dostosować, ale w końcu GridView i widoku DetailsView ma wygląd raczej boxy, siatki. W różnych scenariuszach jest idealne rozwiązanie w układzie siatki, ale w czasie wyświetlania więcej płynnych, mniej sztywne jest wymagany. Podczas wyświetlania pojedynczego rekordu, płynne układ jest możliwe przy użyciu formantu FormView.

W przeciwieństwie do widoku DetailsView FormView nie składa się z pola. Nie można dodać elementu BoundField lub TemplateField FormView. Zamiast tego FormView jest renderowany przy użyciu szablonów. Traktować jako formant widoku DetailsView zawierający pojedynczy TemplateField FormView. FormView obsługuje następujące szablony:

- `ItemTemplate` używany do renderowania określonego rekordu wyświetlane w widoku FormView
- `HeaderTemplate` można określić opcjonalne nagłówki wierszy
- `FooterTemplate` można określić opcjonalny stopki wiersza
- `EmptyDataTemplate` gdy FormView `DataSource` nie ma żadnych rekordów `EmptyDataTemplate` jest używany zamiast `ItemTemplate` do renderowania kodu znaczników formantu
- `PagerTemplate` można użyć do dostosowania interfejsu stronicowania dla FormViews, że włączono stronicowania
- `EditItemTemplate` / `InsertItemTemplate` używane do dostosowania interfejsu edycji lub wstawianie interfejsu dla FormViews, które obsługują takie funkcje

W tym samouczku zajmiemy się za pomocą formantu FormView do prezentowania mniej sztywne wyświetlania produktów. Zamiast pola nazwy, kategorii, dostawcy i tak dalej, FormView w `ItemTemplate` wyświetli te wartości przy użyciu kombinacji element nagłówka i `<table>` (zobacz rysunek 1).


[![FormView dzieli siatki układu w widoku DetailsView](using-the-formview-s-templates-cs/_static/image2.png)](using-the-formview-s-templates-cs/_static/image1.png)

**Rysunek 1**: FormView dzieli poza widoczne układu Grid-Like w widoku DetailsView ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](using-the-formview-s-templates-cs/_static/image3.png))


## <a name="step-1-binding-the-data-to-the-formview"></a>Krok 1: Wiązanie danych w widoku FormView

Otwórz `FormView.aspx` strony i przeciągnij FormView z przybornika do projektanta. Najpierw Dodawanie FormView jest wyświetlana jako szare pole poinstruowanie nam który `ItemTemplate` jest wymagana.


[![FormView nie można wyrenderować w projektancie, dopóki nie podano ItemTemplate](using-the-formview-s-templates-cs/_static/image5.png)](using-the-formview-s-templates-cs/_static/image4.png)

**Rysunek 2**: FormView nie można wyrenderować w Projektancie dopóki `ItemTemplate` podano ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](using-the-formview-s-templates-cs/_static/image6.png))


`ItemTemplate` Można tworzyć ręcznie (za pomocą składni deklaratywnej) lub mogą być tworzone automatycznie przez powiązanie FormView z formantem źródła danych za pomocą projektanta. To utworzony automatycznie `ItemTemplate` zawiera kod HTML, który wyświetla nazwę każdego pola i etykiety formantu którego `Text` właściwość jest powiązana wartość pola. Takie podejście auto tworzy także `InsertItemTemplate` i `EditItemTemplate`, które są wypełniane przy użyciu kontrolki wejściowe dla każdego pola danych zwróconych przez formant źródła danych.

Jeśli chcesz automatycznie utworzyć szablon, w tagu FormView dodać nowe kontrolki ObjectDataSource, który wywołuje `ProductsBLL` klasy `GetProducts()` metody. Spowoduje to utworzenie FormView z `ItemTemplate`, `InsertItemTemplate`, i `EditItemTemplate`. Z widoku źródła usunąć `InsertItemTemplate` i `EditItemTemplate` , ponieważ nie jesteśmy planuje się tworzenie FormView obsługującego edycji lub wstawianie jeszcze. Następnie czyści znaczników w `ItemTemplate` , dzięki czemu będziemy mieć pustego do pracy z.

Jeśli musisz utworzyć `ItemTemplate` ręcznie, możesz dodać i skonfigurować ObjectDataSource przeciągając je z przybornika do projektanta. Jednak nie należy ustawiać źródła danych FormView przy użyciu projektanta. Zamiast tego, przejdź do widoku źródłowego i ręcznie ustawić FormView `DataSourceID` właściwości `ID` wartość elementu ObjectDataSource. Następnie należy ręcznie dodać `ItemTemplate`.

Niezależnie od tego, jakie rozwiązanie możesz podjęcia decyzji, w tym momencie znaczników deklaratywne Twojej FormView wyglądu:


[!code-aspx[Main](using-the-formview-s-templates-cs/samples/sample1.aspx)]

Poświęć chwilę, aby zaznaczyć pole wyboru Włącz stronicowanie w widoku FormView tagów inteligentnych; Spowoduje to dodanie `AllowPaging="True"` atrybutu składni deklaratywnej FormView. Ponadto należy ustawić `EnableViewState` wartość False dla właściwości.

## <a name="step-2-defining-theitemtemplates-markup"></a>Krok 2: Definiowanie`ItemTemplate`tego znacznika

Z FormView powiązany z kontrolki ObjectDataSource i skonfigurowane do obsługi stronicowania jesteśmy gotowi umożliwia określenie zawartości do `ItemTemplate`. W tym samouczku, ma teraz nazwę produktu wyświetlany w `<h3>` nagłówka. Następujące, można użyć kodu HTML `<table>` do wyświetlenia pozostałych właściwości produktu w tabeli cztery kolumny, gdzie kolumny pierwszy i trzeci listę nazw właściwości, a drugim i czwartym listy wartości.

Ten kod znaczników można wprowadzać w za pomocą interfejsu FormView szablon do edycji w Projektancie lub wprowadzić ręcznie za pomocą składni deklaratywnej. Podczas pracy z szablonami I zwykle szybszym rozwiązaniem pracować bezpośrednio z składni deklaratywnej, ale możesz użyć dowolnego technika jest najbardziej Ci.

Następujący kod przedstawia znaczników deklaratywne FormView po `ItemTemplate`przez zakończył struktury:


[!code-aspx[Main](using-the-formview-s-templates-cs/samples/sample2.aspx)]

Zwróć uwagę, że składnia wiązania z danymi - `<%# Eval("ProductName") %>`, na przykład mogą zostać dodane bezpośrednio do danych wyjściowych szablonu. Oznacza to, że jej muszą nie można przypisać do formantu etykiety `Text` właściwości. Na przykład mamy `ProductName` wartość wyświetlana w `<h3>` przy użyciu elementu `<h3><%# Eval("ProductName") %></h3>`, który produktu Chai spowoduje, że jako `<h3>Chai</h3>`.

`ProductPropertyLabel` i `ProductPropertyValue` klas CSS służą do określania stylu produktu nazw właściwości i wartości w `<table>`. Te klasy CSS zdefiniowanych w `Styles.css` i spowodować, że nazwy właściwości czcionką pogrubioną i wyrównany do prawej, a następnie dodaj prawo dopełnienie wartości właściwości.

Ponieważ nie ma żadnych CheckBoxFields dostępne w widoku FormView, aby pokazać `Discontinued` wartość jako pole wyboru możemy dodać własne kontrolki wyboru. `Enabled` Właściwość jest ustawiona na False, dzięki czemu tylko do odczytu i pole wyboru `Checked` właściwość jest powiązana wartość `Discontinued` pola danych.

Z `ItemTemplate` zakończeniu informacji o produkcie jest wyświetlany w sposób bardziej płynne. Porównaj dane wyjściowe widoku DetailsView z ostatnich samouczek (rysunek 3) z danych wyjściowych wygenerowanych przez FormView w tym samouczku (4 rysunek).


[![Sztywnych dane wyjściowe w widoku DetailsView](using-the-formview-s-templates-cs/_static/image8.png)](using-the-formview-s-templates-cs/_static/image7.png)

**Rysunek 3**: sztywne dane wyjściowe z widoku DetailsView ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](using-the-formview-s-templates-cs/_static/image9.png))


[![Dane wyjściowe płynne FormView](using-the-formview-s-templates-cs/_static/image11.png)](using-the-formview-s-templates-cs/_static/image10.png)

**Rysunek 4**: dane wyjściowe FormView płynu ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](using-the-formview-s-templates-cs/_static/image12.png))


## <a name="summary"></a>Podsumowanie

Formanty widoku GridView i widoku DetailsView mogą mieć ich dane wyjściowe dostosowane przy użyciu TemplateFields, jednocześnie nadal ona swoje dane w formacie siatki, boxy. W takich sytuacjach, gdy jeden rekord potrzebuje mają być wyświetlane w układzie mniej sztywne FormView jest idealnym wyborem. Podobnie jak DetailsView FormView renderuje pojedynczy rekord z jego `DataSource`, jednak w przeciwieństwie do widoku DetailsView składa się tylko z szablonów i nie obsługuje pól.

Jak widzieliśmy w tym samouczku FormView umożliwia bardziej elastyczne układu, podczas wyświetlania pojedynczego rekordu. W przyszłości samouczki zajmiemy się formanty DataList i powtarzanego, które zapewniają ten sam poziom elastyczności jako FormsView, ale można wyświetlić wiele rekordów (na przykład w widoku GridView).

Programowanie przyjemność!

## <a name="about-the-author"></a>Informacje o autorze

[Scott Bento](http://www.4guysfromrolla.com/ScottMitchell.shtml), autora siedmiu książek ASP/ASP.NET i twórcę z [4GuysFromRolla.com](http://www.4guysfromrolla.com), pracuje z technologii Microsoft Web od 1998. Scott działa jako niezależnego konsultanta trainer i składnika zapisywania. Jest jego najnowszej książki [ *Sams nauczyć się ASP.NET 2.0 w ciągu 24 godzin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Piotr można uzyskać pod adresem [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) lub za pośrednictwem jego blog, który znajduje się w temacie [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Specjalne podziękowania dla

Ten samouczek serii zostało sprawdzone przez wiele recenzentów przydatne. Recenzenta realizacji w tym samouczku został E.R. Gilmore. Zainteresowani recenzowania Moje nadchodzących artykuły MSDN? Jeśli tak, Porzuć mnie linii w [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Poprzednie](using-templatefields-in-the-detailsview-control-cs.md)
> [dalej](displaying-summary-information-in-the-gridview-s-footer-cs.md)
