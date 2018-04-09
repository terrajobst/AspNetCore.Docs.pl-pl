---
uid: web-forms/overview/data-access/editing-inserting-and-deleting-data/adding-validation-controls-to-the-editing-and-inserting-interfaces-vb
title: Dodawanie formantów weryfikacji do edycji i wstawianie interfejsów (VB) | Dokumentacja firmy Microsoft
author: rick-anderson
description: W tym samouczku zajmiemy się tym, jak łatwo jest Dodaj formanty walidacji EditItemTemplate i InsertItemTemplate danych formantu sieci Web, aby zapewnić bardziej...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/17/2006
ms.topic: article
ms.assetid: e3d7028a-7a22-4a4f-babe-d53afc41c0e2
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/editing-inserting-and-deleting-data/adding-validation-controls-to-the-editing-and-inserting-interfaces-vb
msc.type: authoredcontent
ms.openlocfilehash: eda86c5481e5739b6cd9a2470889a26144302a3f
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
---
<a name="adding-validation-controls-to-the-editing-and-inserting-interfaces-vb"></a>Dodawanie formantów weryfikacji do edycji i wstawianie interfejsów (VB)
====================
przez [Bento Scott](https://twitter.com/ScottOnWriting)

[Pobierz przykładową aplikację](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_19_VB.exe) lub [pobierania plików PDF](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/datatutorial19vb1.pdf)

> W tym samouczku będziemy zostanie wyświetlone, jak łatwo jest Dodaj formanty walidacji EditItemTemplate i InsertItemTemplate danych formantu sieci Web zapewnia bardziej niezawodne interfejs użytkownika.


## <a name="introduction"></a>Wprowadzenie

Kontrolki GridView i widoku DetailsView w przykładach, możemy zostały przedstawione w ciągu ostatnich trzech samouczki ma wszystkie zostały składa się z BoundFields i CheckBoxFields (typy pól automatycznie dodawane przez program Visual Studio podczas wiązania ze źródłem danych widoku GridView lub widoku DetailsView kontrolowanie za pomocą tagów inteligentnych). Podczas edycji wiersza w widoku GridView lub widoku DetailsView, te BoundFields, które nie są tylko do odczytu są konwertowane na pola tekstowe, z którego użytkownik końcowy może zmodyfikować istniejących danych. Podobnie podczas Wstawianie nowy rekord formantu widoku DetailsView tych BoundFields którego `InsertVisible` właściwość jest ustawiona na `True` (ustawienie domyślne) są renderowane jako puste pola tekstowe, w którym użytkownik może podać wartości pól w nowym rekordzie. Podobnie CheckBoxFields, które są wyłączone w interfejsie standardowe, tylko do odczytu, są konwertowane na włączone pola wyboru w edycji i wstawianie interfejsów.

Domyślnie edycji i wstawianie interfejsów dla elementu BoundField i CheckBoxField mogą być przydatne, interfejs brakuje dowolny rodzaj sprawdzania poprawności. Jeśli użytkownik wprowadzi błąd zapisu danych — takich jak pominięcie `ProductName` pola lub nieprawidłową wartość dla wprowadzania `UnitsInStock` (na przykład -50) zostanie zgłoszony wyjątek z wewnątrz głębokości architektury aplikacji. Podczas tego wyjątku można bezpiecznie obsłużyć jak pokazano w [poprzedniego samouczek](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb.md), najlepiej edycji lub wstawianie interfejsu użytkownika to formanty walidacji, aby uniemożliwić użytkownikowi wprowadzanie takich nieprawidłowe dane w pierwszym miejscu.

Zapewnić dostosowane edycji lub wstawianie interfejsu, musimy elementu BoundField lub CheckBoxField Zamień na pole TemplateField. TemplateFields, które były temat dyskusji w [przy użyciu TemplateFields w kontrolce GridView](../custom-formatting/using-templatefields-in-the-gridview-control-cs.md) i [TemplateFields za pomocą formantu widoku DetailsView](../custom-formatting/using-templatefields-in-the-detailsview-control-vb.md) samouczki, może składać się z wielu Definiowanie szablonów należy oddzielić interfejsów dla stanów innego wiersza. TemplateField `ItemTemplate` służy do podczas renderowania pola tylko do odczytu lub wierszy w widoku DetailsView lub widoku GridView formantów, podczas gdy `EditItemTemplate` i `InsertItemTemplate` wskazać interfejsy służące do edycji i wstawiania tryby, odpowiednio.

W tym samouczku zajmiemy się tym, jak łatwo jest Dodaj formanty walidacji do TemplateField `EditItemTemplate` i `InsertItemTemplate` zapewnia bardziej niezawodne interfejs użytkownika. W szczególności ten samouczek przedstawia przykład utworzone w [badanie zdarzeń skojarzonych z Wstawianie, aktualizowanie i usuwanie](examining-the-events-associated-with-inserting-updating-and-deleting-vb.md) samouczka i wspomaga edycji i wstawianie interfejsy do uwzględnienia odpowiednie sprawdzania poprawności.

## <a name="step-1-replicating-the-example-fromexamining-the-events-associated-with-inserting-updating-and-deletingexamining-the-events-associated-with-inserting-updating-and-deleting-vbmd"></a>Krok 1: Replikowanie w przykładzie z[badanie zdarzenia skojarzonego z Wstawianie, aktualizowanie i usuwanie](examining-the-events-associated-with-inserting-updating-and-deleting-vb.md)

W [badanie zdarzeń skojarzonych z Wstawianie, aktualizowanie i usuwanie](examining-the-events-associated-with-inserting-updating-and-deleting-vb.md) samouczek utworzyliśmy stronę wymienione nazwy i cen produktów w można edytować widoku GridView. Ponadto stronie uwzględniona Element DetailsView o `DefaultMode` ustawiono właściwość `Insert`, a tym samym zawsze renderowanie w trybie wstawiania. Z tego widoku DetailsView, użytkownik może wprowadź nazwę i cen dla nowego produktu, kliknij przycisk Wstaw i został dodany do systemu (zobacz rysunek 1).


[![Poprzedni przykład pozwala użytkownikom na dodawanie nowych produktów i edytować istniejące](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image2.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image1.png)

**Rysunek 1**: poprzedniego przykładu umożliwia użytkownikom dodawanie nowych produktów i edytować istniejące ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image3.png))


Naszym celem w tym samouczku jest rozszerzyć widoku DetailsView i GridView, aby zapewnić formanty walidacji. W szczególności zostanie logiki sprawdzania poprawności:

- Wymaga podania nazwy podczas wstawiania lub edytowania produktu
- Wymaga podania ceny podczas wstawiania rekordów; Podczas edytowania rekordu, firma Microsoft będzie wymagać wciąż cen, ale będzie używać logika w widoku GridView `RowUpdating` już z wcześniejszych samouczek programu obsługi zdarzeń
- Sprawdź, czy wartość wprowadzona w cenie formacie waluty prawidłowe

Zanim można przyjrzymy rozbudować w poprzednim przykładzie, aby uwzględnić sprawdzania poprawności, najpierw musimy replikacji w przykładzie z `DataModificationEvents.aspx` strony do strony w tym samouczku `UIValidation.aspx`. Potrzebujemy kopiować zarówno w tym celu `DataModificationEvents.aspx` deklaratywne znaczników strony i jego kod źródłowy. Najpierw skopiować na deklaratywne znaczników, wykonując następujące czynności:

1. Otwórz `DataModificationEvents.aspx` strony w programie Visual Studio
2. Przejdź do strony deklaratywne znaczników (kliknij przycisk źródła w dolnej części strony)
3. Skopiuj tekst wewnątrz `<asp:Content>` i `</asp:Content>` tagi (linie 3 – 44), jak pokazano na rysunku 2.


[![Skopiuj tekst w &lt;asp: Content&gt; formantu](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image5.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image4.png)

**Rysunek 2**: skopiować tekst w `<asp:Content>` formantu ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image6.png))


1. Otwórz `UIValidation.aspx` strony
2. Przejdź do strony deklaratywne znaczników
3. Wklej tekst wewnątrz `<asp:Content>` formantu.

Aby skopiować kod źródłowy, otwórz `DataModificationEvents.aspx.vb` zaznacz i skopiuj tylko tekst *w* `EditInsertDelete_DataModificationEvents` klasy. Skopiuj obsługi zdarzeń trzy (`Page_Load`, `GridView1_RowUpdating`, i `ObjectDataSource1_Inserting`), ale **nie** skopiuj deklaracji klasy. Wklej skopiowany tekst *w* `EditInsertDelete_UIValidation` klasy w `UIValidation.aspx.vb`.

Po przeniesieniu za pośrednictwem zawartość i kod z `DataModificationEvents.aspx` do `UIValidation.aspx`, Poświęć chwilę, aby przetestować postęp w przeglądarce. Powinny pojawić się takie same dane wyjściowe i doświadczenia, te same funkcje, w każdym z tych dwóch stronach (dla zrzut ekranu odwołują się do rysunku 1 `DataModificationEvents.aspx` w działaniu).

## <a name="step-2-converting-the-boundfields-into-templatefields"></a>Krok 2: Konwertowanie BoundFields na TemplateFields

Aby dodać formanty walidacji do edycji i wstawianie interfejsów, BoundFields używany przez formant widoku DetailsView i GridView muszą zostać przekonwertowane do TemplateFields. Można to osiągnąć, kliknij odpowiednio łącza edycji kolumn i Edytuj pola w widoku GridView i DetailsView tagów inteligentnych. , Zaznacz wszystkie BoundFields, a następnie kliknij łącze "Konwertuj to pole na pole TemplateField".


[![Konwertowanie każdego DetailsView i w widoku GridView BoundFields TemplateFields](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image8.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image7.png)

**Rysunek 3**: konwertowanie każdego DetailsView i w widoku GridView BoundFields do TemplateFields ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image9.png))


Konwertowanie elementu BoundField na pole TemplateField za pomocą okna dialogowego pola generuje TemplateField, który wykazuje tych samych interfejsów tylko do odczytu, edytowania i wstawianie jako elementu BoundField, do samej siebie. Następujący kod przedstawia składnię deklaratywne `ProductName` w widoku DetailsView po został przekonwertowany na pole TemplateField:

[!code-aspx[Main](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/samples/sample1.aspx)]

Należy pamiętać, że ta TemplateField ma trzy szablony tworzone automatycznie `ItemTemplate`, `EditItemTemplate`, i `InsertItemTemplate`. `ItemTemplate` Wyświetla wartość pola danych jednego (`ProductName`) za pomocą formantu etykiety w sieci Web, podczas gdy `EditItemTemplate` i `InsertItemTemplate` przedstawić wartości pola danych w formancie TextBox sieci Web, który kojarzy w polu danych z pole tekstowe `Text` Właściwość przy użyciu dwukierunkowego wiązania z danymi. Ponieważ w widoku DetailsView jest używany tylko na tej stronie do wrzucania, można usunąć `ItemTemplate` i `EditItemTemplate` z dwóch TemplateFields, chociaż nie powoduje żadnych problemów w pozostawienie ich.

Ponieważ w widoku GridView nie obsługuje wbudowanej funkcji DetailsView Wstawianie, konwertowanie w widoku GridView `ProductName` pole na pole TemplateField powoduje tylko `ItemTemplate` i `EditItemTemplate`:

[!code-aspx[Main](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/samples/sample2.aspx)]

Klikając przycisk "Konwertuj to pole na pole TemplateField", Visual Studio został utworzony TemplateField, w których szablony naśladować interfejsu użytkownika przekonwertowanego elementu BoundField. Można to sprawdzić, przechodząc na stronę tej strony za pośrednictwem przeglądarki. Znajdziesz się, że wygląd i zachowanie TemplateFields jest identyczne środowisko podczas BoundFields użyto zamiast tego.

> [!NOTE]
> Możesz dostosować edycji interfejsów w szablonach, zgodnie z potrzebami. Na przykład może chcemy mieć w polu tekstowym `UnitPrice` TemplateFields renderowane jako pole tekstowe mniejsze niż `ProductName` pola tekstowego. W tym celu można ustawić pole tekstowe `Columns` właściwości do odpowiedniej wartości lub podaj bezwzględny szerokość za pośrednictwem `Width` właściwości. W następnym samouczku przedstawiono będzie całkowicie dostosowywania interfejsu edycji przez zastąpienie pole tekstowe z wpisem danych formantu sieci Web.


## <a name="step-3-adding-the-validation-controls-to-the-gridviewsedititemtemplate-s"></a>Krok 3: Dodawanie formantów weryfikacji do widoku GridView`EditItemTemplate` s

Podczas tworzenia formularzy wprowadzania danych, ważne jest wprowadzania wszystkich wymaganych pól i że wszystkie podane dane wejściowe są wartości prawnych, niepoprawnie sformatowany. Aby upewnić się, że dane wejściowe użytkownika są prawidłowe, program ASP.NET udostępnia pięć formantów wbudowanych sprawdzania poprawności, które są przeznaczone do użycia w celu sprawdzenia wartości kontrolki wprowadzania w formie jednej:

- [RequiredFieldValidator](https://msdn.microsoft.com/library/5hbw267h(VS.80).aspx) gwarantuje, że wartość została podana
- [CompareValidator](https://msdn.microsoft.com/library/db330ayw(VS.80).aspx) sprawdza poprawność wartości z inną wartością formantu sieci Web lub wartość stałą lub zapewnia, że format wartości jest niedozwolona dla określonego typu danych
- [RangeValidator](https://msdn.microsoft.com/library/f70d09xt.aspx) gwarantuje, że wartość znajduje się w zakresie wartości
- [RegularExpressionValidator](https://msdn.microsoft.com/library/eahwtc9e.aspx) weryfikuje wartość względem [wyrażeń regularnych](http://en.wikipedia.org/wiki/Regular_expression)
- [CustomValidator](https://msdn.microsoft.com/library/9eee01cx(VS.80).aspx) weryfikuje wartość z niestandardowych, definiowanych przez użytkownika — metoda

Aby uzyskać więcej informacji o tych kontrolek pięć, zapoznaj się [sekcji Formanty walidacji](https://quickstarts.asp.net/quickstartv20/aspnet/doc/ctrlref/validation/default.aspx) z [samouczków szybkiego startu ASP.NET](https://asp.net/QuickStart/aspnet/).

W naszym samouczku trzeba będzie użyć RequiredFieldValidator w widoku DetailsView i w widoku GridView `ProductName` TemplateFields i RequiredFieldValidator w widoku DetailsView `UnitPrice` TemplateField. Ponadto musisz dodać CompareValidator do obu formantów `UnitPrice` TemplateFields, który zapewnia, że wprowadzona cena ma wartość większą niż lub równa 0 i jest podana w formacie waluty prawidłowe.

> [!NOTE]
> Podczas ASP.NET 1.x miały te takich samych kontroli pięć sprawdzania poprawności, ASP.NET 2.0 dodał szereg ulepszeń, głównym dwa skryptu po stronie klienta jest obsługa przeglądarek innych niż program Internet Explorer i możliwość formantów sprawdzania poprawności partycji na stronie do Sprawdzanie poprawności grupy. Aby uzyskać więcej informacji na temat nowych funkcji kontroli weryfikacji w 2.0 dotyczą [pincety formanty walidacji w programie ASP.NET 2.0](http://aspnet.4guysfromrolla.com/articles/112305-1.aspx).


Zacznijmy od dodania formanty walidacji niezbędne `EditItemTemplate` s w TemplateFields w widoku GridView. Aby to zrobić, kliknij łącze Edytuj szablony z widoku GridView tagów inteligentnych można wyświetlić interfejsu edycji szablonu. W tym miejscu można wybrać szablon, który można edytować na liście rozwijanej. Ponieważ chcemy, aby rozszerzyć interfejs edytowania, musimy Dodaj formanty walidacji do `ProductName` i `UnitPrice`w `EditItemTemplate` s.


[![Należy rozszerzyć ProductName i jego UnitPrice EditItemTemplates](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image11.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image10.png)

**Rysunek 4**: musimy Rozszerz `ProductName` i `UnitPrice`w `EditItemTemplate` s ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image12.png))


W `ProductName` `EditItemTemplate`, Dodaj RequiredFieldValidator przeciągając je z przybornika do interfejsu edycji szablonu, umieszczając za pole tekstowe.


[![Dodaj RequiredFieldValidator do ProductName EditItemTemplate](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image14.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image13.png)

**Rysunek 5**: Dodaj RequiredFieldValidator do `ProductName` `EditItemTemplate` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image15.png))


Wszystkie formanty walidacji pracę sprawdzanie poprawności danych wejściowych jeden formant sieci Web ASP.NET. W związku z tym należy wskazać, że RequiredFieldValidator właśnie dodaliśmy powinien sprawdzania poprawności pole tekstowe w `EditItemTemplate`; odbywa się przez ustawienie sterowania weryfikacji [Właściwość ControlToValidate](https://msdn.microsoft.com/library/system.web.ui.webcontrols.basevalidator.controltovalidate(VS.80).aspx) do `ID` odpowiednie formantu sieci Web. Pole tekstowe ma obecnie raczej nieopisane `ID` z `TextBox1`, ale teraz zmienić na bardziej odpowiednią. Kliknij w polu tekstowym w szablonie, a następnie w oknie Właściwości zmień `ID` z `TextBox1` do `EditProductName`.


[![Zmień identyfikator pole tekstowe EditProductName](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image17.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image16.png)

**Rysunek 6**: Zmień pole tekstowe `ID` do `EditProductName` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image18.png))


Następnie należy ustawić RequiredFieldValidator `ControlToValidate` właściwości `EditProductName`. Wreszcie, ustaw [właściwości ErrorMessage](https://msdn.microsoft.com/library/system.web.ui.webcontrols.basevalidator.errormessage(VS.80).aspx) do "Należy podać nazwę produktu" i [właściwości Text](https://msdn.microsoft.com/library/system.web.ui.webcontrols.basevalidator.text(VS.80).aspx) do "\*". `Text` Wartość właściwości, jeśli zostanie podana, jest tekst, który jest wyświetlany przez formant weryfikacji w przypadku niepowodzenia weryfikacji. `ErrorMessage` Wartość właściwości, która jest wymagana, jest używany przez formant ValidationSummary; Jeśli `Text` pominięcia będzie używana wartość właściwości `ErrorMessage` wartość właściwości jest również tekstu wyświetlanego przez formant sprawdzania poprawności na nieprawidłowe dane wejściowe.

Ustawienie tych trzech właściwości RequiredFieldValidator, po ekranie powinien wyglądać podobnie do rysunku 7.


[![Ustawianie właściwości tekstu, komunikat o błędzie i ControlToValidate RequiredFieldValidator](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image20.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image19.png)

**Rysunek 7**: Ustaw RequiredFieldValidator `ControlToValidate`, `ErrorMessage`, i `Text` właściwości ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image21.png))


Z RequiredFieldValidator dodane do `ProductName` `EditItemTemplate`, wszystkie, czy pozostaje jest dodanie niezbędne sprawdzania poprawności `UnitPrice` `EditItemTemplate`. Ponieważ firma Microsoft, decydujesz się na tej stronie `UnitPrice` jest opcjonalny w przypadku edytowania rekordu, nie należy dodawać RequiredFieldValidator. Jednak należy dodać CompareValidator, aby upewnić się, że `UnitPrice`, jeśli podany, jest poprawnie sformatowana jako walutę i jest większa lub równa 0.

Zanim dodamy CompareValidator do `UnitPrice` `EditItemTemplate`, najpierw Zmieńmy identyfikator formantu TextBox w sieci Web z `TextBox2` do `EditUnitPrice`. Po wprowadzeniu tej zmiany należy dodać CompareValidator, ustawienie jej `ControlToValidate` właściwości `EditUnitPrice`, jego `ErrorMessage` dla właściwości "cena musi być większa lub równa zero i nie może zawierać symbol waluty," i jego `Text` właściwości "\*".

Aby wskazać, że `UnitPrice` wartość musi być większa lub równa 0, ustaw CompareValidator [właściwości Operator](https://msdn.microsoft.com/library/system.web.ui.webcontrols.comparevalidator.operator(VS.80).aspx) do `GreaterThanEqual`, jego [właściwości ValueToCompare](https://msdn.microsoft.com/library/system.web.ui.webcontrols.comparevalidator.valuetocompare(VS.80).aspx) "0" i jego [ Typ właściwości](https://msdn.microsoft.com/library/system.web.ui.webcontrols.basecomparevalidator.type.aspx) do `Currency`. Poniżej przedstawiono składni deklaratywnej `UnitPrice` na pole TemplateField `EditItemTemplate` po dokonaniu zmiany:

[!code-aspx[Main](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/samples/sample3.aspx)]

Po wprowadzeniu tych zmian, otwórz stronę w przeglądarce. Przy próbie pominąć nazwę lub wprowadź wartość Nieprawidłowa cena podczas edytowania produktu gwiazdka pojawia się obok pola tekstowego. Jak pokazano na rysunku 8, cenę, która zawiera symbol waluty, takich jak $ cenie od 19,95 jest uznawane za nieprawidłowe. CompareValidator `Currency` `Type` umożliwia separatory cyfr (na przykład przecinków lub okresów, w zależności od ustawienia kultury) i wiodące plus lub minus, ale *nie* zezwolenie na symbol waluty. To zachowanie może perplex użytkowników, jak interfejs edytowania obecnie renderuje `UnitPrice` w formacie waluty.

> [!NOTE]
> Odwołaj się, że w *zdarzeń skojarzonych z Wstawianie, aktualizowanie i usuwanie* samouczek ustawiliśmy elementu BoundField `DataFormatString` właściwości `{0:c}` celu formacie waluty. Ponadto możemy ustawić `ApplyFormatInEditMode` właściwości na wartość true, powodując widoku GridView do edycji interfejs do formatowania `UnitPrice` jako walutę. Podczas konwertowania elementu BoundField na pole TemplateField, Visual Studio zauważyć te ustawienia i sformatowany pole tekstowe `Text` właściwość jako walutę przy użyciu składni wiązania z danymi `<%# Bind("UnitPrice", "{0:c}") %>`.


[![Gwiazdka pojawia się obok pola tekstowe z nieprawidłowe dane wejściowe](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image23.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image22.png)

**Rysunek 8**: gwiazdki pojawia się dalej, aby pola tekstowe z nieprawidłowe dane wejściowe ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image24.png))


Podczas sprawdzania poprawności działa jako — jest, użytkownik będzie musiał ręcznie usunąć symbol waluty podczas edytowania rekordu, który nie jest akceptowane. Aby rozwiązać ten problem, firma Microsoft dostępne są trzy opcje:

1. Skonfiguruj `EditItemTemplate` , aby `UnitPrice` wartość nie jest sformatowany jako walutę.
2. Zezwalaj użytkownikowi na wprowadzenie symbolu waluty przez usunięcie CompareValidator i zastępowanie RegularExpressionValidator, który prawidłowo sprawdza, czy wartość waluty prawidłowo sformatowane. W tym miejscu problem jest wyrażenie regularne do sprawdzania poprawności wartości waluty nie jest łatwa i będzie wymagać pisanie kodu, jeśli trzeba zastosować ustawienia kultury.
3. Całkowicie usunąć formant sprawdzania poprawności i zależne od logikę weryfikacji po stronie serwera w widoku GridView `RowUpdating` obsługi zdarzeń.

Przejdźmy z opcją #1 w tym ćwiczeniu. Obecnie `UnitPrice` jest w formacie waluty z powodu wyrażenia wiązania danych dla pola tekstowego w `EditItemTemplate`: `<%# Bind("UnitPrice", "{0:c}") %>`. Zmień instrukcję Bind `Bind("UnitPrice", "{0:n2}")`, które formatuje wynik jako liczba z dwóch cyfr precyzji. Można to zrobić bezpośrednio za pomocą składni deklaratywnej lub klikając łącze Edytuj powiązania DataBindings z `EditUnitPrice` TextBox w `UnitPrice` na pole TemplateField `EditItemTemplate` (zobacz rysunku 9 i 10).


[![Kliknij łącze Edytuj powiązania DataBindings pole tekstowe](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image26.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image25.png)

**Rysunek 9**: kliknij łącze Edytuj powiązania danych w polu tekstowym ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image27.png))


[![Określ specyfikatora formatu w instrukcji Bind](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image29.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image28.png)

**Na rysunku nr 10**: Określ specyfikatora formatu w `Bind` instrukcji ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image30.png))


Dzięki tej zmianie sformatowany cen w interfejsie edycji obejmuje przecinkami jako separator grupy i okres jako separator dziesiętny, ale pozostawia poza symbol waluty.

> [!NOTE]
> `UnitPrice` `EditItemTemplate` Nie zawiera RequiredFieldValidator, pozwalając odświeżenie strony, aby i aktualizowania logiki do rozpoczęcia. Jednak `RowUpdating` kopiowane z obsługi zdarzeń *badanie zdarzeń skojarzonych z Wstawianie, aktualizowanie i usuwanie* samouczek obejmuje programowe Sprawdź, czy upewnia się, że `UnitPrice` jest dostępne. Możesz także usunąć tę logikę, pozostaw go jako-, albo Dodaj RequiredFieldValidator do `UnitPrice` `EditItemTemplate`.


## <a name="step-4-summarizing-data-entry-problems"></a>Krok 4: Podsumowanie problemów zapis danych

Oprócz formantów pięć weryfikacji platformy ASP.NET zawiera [formant ValidationSummary](https://msdn.microsoft.com/library/f9h59855(VS.80).aspx), który wyświetla `ErrorMessage` s tych kontrolek sprawdzania poprawności, które wykryto nieprawidłowe dane. To podsumowanie danych mogą być wyświetlane jako tekstu na stronie sieci web lub za pośrednictwem messagebox modalne, po stronie klienta. Umożliwia także poprawiania komfortu tego samouczka do dołączenia po stronie klienta element messagebox podsumowania sprawdzania poprawności problemów.

Aby to zrobić, przeciągnij formant ValidationSummary z przybornika do projektanta. Lokalizacja formantu weryfikacji naprawdę nie ma znaczenia, ponieważ chcemy skonfiguruj go, aby wyświetlić podsumowanie tylko jako element messagebox. Po dodaniu formantu, ustaw jej [właściwości ShowSummary](https://msdn.microsoft.com/library/system.web.ui.webcontrols.validationsummary.showsummary(VS.80).aspx) do `False` i jego [właściwości ShowMessageBox](https://msdn.microsoft.com/library/system.web.ui.webcontrols.validationsummary.showmessagebox(VS.80).aspx) do `True`. Z tym dodatkiem jakieś błędy sprawdzania poprawności zostały podsumowane w messagebox po stronie klienta.


[![Błędy sprawdzania poprawności zostały podsumowane w Messagebox po stronie klienta](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image32.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image31.png)

**Rysunek 11**: błędy sprawdzania poprawności zostały podsumowane w Messagebox po stronie klienta ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image33.png))


## <a name="step-5-adding-the-validation-controls-to-the-detailsviewsinsertitemtemplate"></a>Krok 5: Dodawanie formantów weryfikacji do widoku DetailsView`InsertItemTemplate`

Wszystkie opcje, które pozostaje w tym samouczku jest Dodaj formanty walidacji interfejsu Wstawianie DetailsView. Proces dodawania formanty walidacji w szablonach DetailsView jest identyczna jak zbadać w kroku 3; Dlatego firma Microsoft będzie breeze za pomocą zadania w tym kroku. Jak robiliśmy z widoku GridView `EditItemTemplate` s, I zachęca do zmiany nazwy `ID` s pola tekstowe z nieopisane `TextBox1` i `TextBox2` do `InsertProductName` i `InsertUnitPrice`.

Dodaj RequiredFieldValidator do `ProductName` `InsertItemTemplate`. Ustaw `ControlToValidate` do `ID` pola tekstowego w szablonie, jego `Text` dla właściwości "\*" i jego `ErrorMessage` dla właściwości "Należy podać nazwę produktu".

Ponieważ `UnitPrice` jest wymagany dla tej strony, podczas dodawania nowego rekordu, Dodaj RequiredFieldValidator do `UnitPrice` `InsertItemTemplate`, ustawienie jej `ControlToValidate`, `Text`, i `ErrorMessage` właściwości odpowiednio. Na koniec należy dodać CompareValidator do `UnitPrice` `InsertItemTemplate` jak również konfigurowanie jego `ControlToValidate`, `Text`, `ErrorMessage`, `Type`, `Operator`, i `ValueToCompare` właściwości tak samo, jak robiliśmy z `UnitPrice`w CompareValidator w widoku GridView `EditItemTemplate`.

Po dodaniu tych kontrolek sprawdzania poprawności, nowego produktu nie może być dodany do systemu, jeśli nie podano nazwy lub jego cena jest liczbą ujemną ani sformatowany w niedozwolony sposób.


[![Dodano logikę weryfikacji DetailsView Wstawianie interfejsu](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image35.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image34.png)

**Rysunek 12**: logikę weryfikacji został dodany do interfejsu Wstawianie DetailsView ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image36.png))


## <a name="step-6-partitioning-the-validation-controls-into-validation-groups"></a>Krok 6: Partycjonowanie formanty walidacji w grupach sprawdzania poprawności

Naszą stronę składa się z dwóch różnych logicznie zestawów formanty walidacji: te, które odpowiadają widoku GridView do edycji interfejsu i tych, które odpowiadają widoku DetailsView do wstawiania interfejsu. Domyślnie podczas odświeżania strony *wszystkie* formanty walidacji na tej stronie są sprawdzane. Jednak podczas edytowania rekordu nie chcemy formanty walidacji DetailsView Wstawianie interfejs do sprawdzania poprawności. Rysunek 13 ilustruje nasze bieżące dilemma, gdy użytkownik edytuje produkt o wartości doskonale prawne, kliknięcie przycisku Aktualizacja powoduje błąd sprawdzania poprawności, ponieważ wartości nazwy i cen w interfejsie Wstawianie są puste.


[![Aktualizowanie produktu spowoduje, że interfejs wstawianie formantów weryfikacji Fire](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image38.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image37.png)

**Rysunek 13**: aktualizowanie produktu spowoduje, że interfejs wstawianie formantów weryfikacji Fire ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image39.png))


Formanty walidacji w programie ASP.NET 2.0 mogą być podzielone na grupy weryfikacji za pomocą ich `ValidationGroup` właściwości. Aby skojarzyć zbiór formanty walidacji w grupie, wystarczy ustawić ich `ValidationGroup` właściwości na tę samą wartość. W naszym samouczku ustawić `ValidationGroup` właściwości formanty walidacji w widoku GridView TemplateFields do `EditValidationControls` i `ValidationGroup` właściwości TemplateFields DetailsView do `InsertValidationControls`. Te zmiany może odbywać się bezpośrednio w deklaratywne znaczników lub w oknie właściwości, gdy przy użyciu narzędzia Projektant Edytuj szablon interfejsu.

Oprócz sprawdzania poprawności kontrolki, przycisk i formanty w programie ASP.NET 2.0 przycisk również obejmować `ValidationGroup` właściwości. Grupy sprawdzania poprawności modułów sprawdzania poprawności są sprawdzane pod kątem poprawności tylko podczas odświeżania strony jest wywołanych przycisku, który ma taką samą `ValidationGroup` ustawienie właściwości. Na przykład w kolejności dla przycisku Wstaw DetailsView do wyzwolenia `InsertValidationControls` grupy sprawdzania poprawności należy ustawić w elemencie CommandField `ValidationGroup` właściwości `InsertValidationControls` (patrz rysunek 14). Ponadto należy ustawić w widoku GridView w elemencie CommandField `ValidationGroup` właściwości `EditValidationControls`.


[![Właściwości w elemencie CommandField ValidationGroup InsertValidationControls przez zestaw widoku DetailsView](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image41.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image40.png)

**Rysunek 14**: Ustaw DetailsView w elemencie CommandField `ValidationGroup` właściwości `InsertValidationControls` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image42.png))


Po wprowadzeniu tych zmian widoku DetailsView i w widoku GridView TemplateFields i CommandFields powinien wyglądać podobnie do następującego:

TemplateFields i CommandField DetailsView

[!code-aspx[Main](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/samples/sample4.aspx)]

CommandField i TemplateFields w widoku GridView

[!code-aspx[Main](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/samples/sample5.aspx)]

W tym momencie formanty walidacji specyficzne dla edycji wyzwalać tylko po kliknięciu przycisku Aktualizuj w widoku GridView i sprawdzania poprawności specyficznych dla wstawiania fire formantów tylko wtedy, gdy zostanie kliknięty przycisk Wstaw DetailsView rozwiązywania problemów zaznaczone rysunku 13. Jednak z tą zmianą naszych formant ValidationSummary nie jest już wyświetlany podczas wprowadzania nieprawidłowe dane. Zawiera także formant ValidationSummary `ValidationGroup` właściwości i tylko przedstawiono podsumowanie tych formanty walidacji w jego grupie sprawdzania poprawności. W związku z tym musimy mieć dwa formanty walidacji na tej stronie, jeden dla `InsertValidationControls` grupy sprawdzania poprawności i jeden dla `EditValidationControls`.

[!code-aspx[Main](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/samples/sample6.aspx)]

Z tym dodatkiem z naszym samouczkiem jest zakończona

## <a name="summary"></a>Podsumowanie

Gdy BoundFields zapewniają zarówno interfejs Wstawianie i edytowania, interfejs nie jest można dostosowywać. Zazwyczaj chcemy Dodaj formanty walidacji do edycji i wstawianie interfejs, aby upewnić się, że użytkownik wprowadza wymagają danych wejściowych w formacie prawnych. W tym celu możemy przekonwertować BoundFields TemplateFields i Dodaj formanty walidacji do odpowiednie szablony. W tym samouczku będziemy rozszerzony przykład z *badanie zdarzeń skojarzonych z Wstawianie, aktualizowanie i usuwanie* samouczek, dodawanie formantów weryfikacji do obu widoku DetailsView na wstawianie interfejsu i w widoku GridView Edytowanie interfejsu. Ponadto widzieliśmy sposób wyświetlania informacji podsumowania weryfikacji przy użyciu formant ValidationSummary i partycji formanty walidacji na stronie w odrębnych sprawdzania poprawności grupy.

Jak widzieliśmy w tym samouczku TemplateFields Zezwalaj edycji i wstawianie interfejsów zostać rozszerzony do należą formanty walidacji. TemplateFields można również rozszerzać uwzględnienie dodatkowych wejściowych formantów sieci Web, włączanie pole tekstowe, które mają zostać zastąpione przez bardziej odpowiedni formant sieci Web. W naszym samouczku dalej zajmiemy się tym, jak zastąpić formantu TextBox formant DropDownList powiązane z danymi, który jest idealny edytowania klucz obcy (takich jak `CategoryID` lub `SupplierID` w `Products` tabeli).

Programowanie przyjemność!

## <a name="about-the-author"></a>Informacje o autorze

[Scott Bento](http://www.4guysfromrolla.com/ScottMitchell.shtml), autora siedmiu książek ASP/ASP.NET i twórcę z [4GuysFromRolla.com](http://www.4guysfromrolla.com), pracuje z technologii Microsoft Web od 1998. Scott działa jako niezależnego konsultanta trainer i składnika zapisywania. Jest jego najnowszej książki [ *Sams nauczyć się ASP.NET 2.0 w ciągu 24 godzin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Piotr można uzyskać pod adresem [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) lub za pośrednictwem jego blog, który znajduje się w temacie [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Specjalne podziękowania dla

Ten samouczek serii zostało sprawdzone przez wiele recenzentów przydatne. Prowadzić osób dokonujących przeglądu, w tym samouczku zostały Liz Shulok i Zack Nowak. Zainteresowani recenzowania Moje nadchodzących artykuły MSDN? Jeśli tak, Porzuć mnie linii w [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Poprzednie](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb.md)
> [dalej](customizing-the-data-modification-interface-vb.md)
