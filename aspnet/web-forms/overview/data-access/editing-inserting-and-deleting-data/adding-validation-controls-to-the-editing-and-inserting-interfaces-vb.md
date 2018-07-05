---
uid: web-forms/overview/data-access/editing-inserting-and-deleting-data/adding-validation-controls-to-the-editing-and-inserting-interfaces-vb
title: Dodawanie kontrolek weryfikacji do edycji interfejsów i wstawiania (VB) | Dokumentacja firmy Microsoft
author: rick-anderson
description: W tym samouczku zobaczymy, jak łatwo jest dodawanie kontrolek weryfikacji do EditItemTemplate i InsertItemTemplate danych formantu sieci Web, aby zapewnić bardziej...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/17/2006
ms.topic: article
ms.assetid: e3d7028a-7a22-4a4f-babe-d53afc41c0e2
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/data-access/editing-inserting-and-deleting-data/adding-validation-controls-to-the-editing-and-inserting-interfaces-vb
msc.type: authoredcontent
ms.openlocfilehash: a2fc00426022513c6e2adc49b0df30f943403302
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37366232"
---
<a name="adding-validation-controls-to-the-editing-and-inserting-interfaces-vb"></a>Dodawanie kontrolek weryfikacji do edycji interfejsów i wstawiania (VB)
====================
przez [Bento Scott](https://twitter.com/ScottOnWriting)

[Pobierz przykładową aplikację](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_19_VB.exe) lub [Pobierz plik PDF](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/datatutorial19vb1.pdf)

> W tym samouczku zobaczymy, jak łatwo jest dodawanie kontrolek weryfikacji do EditItemTemplate i InsertItemTemplate danych formantu sieci Web, aby zapewnić bardziej niezawodne interfejsu użytkownika.


## <a name="introduction"></a>Wprowadzenie

Kontrolki GridView i DetailsView w przykładach rozważyliśmy już przestrzeni ostatnich trzech samouczków wszystko zostało połączyły BoundFields i CheckBoxFields (typy pól automatycznie dodawane przez program Visual Studio, podczas tworzenia powiązania GridView lub DetailsView ze źródłem danych kontrolowanie za pomocą tagu inteligentnego). W trakcie edytowania wierszy w kontrolce GridView lub DetailsView BoundFields, te, które nie są tylko do odczytu są konwertowane na pola tekstowe, z którego użytkownik końcowy może modyfikować istniejące dane. Podobnie podczas wstawiania nowego rejestrowania w kontrolce DetailsView tych BoundFields którego `InsertVisible` właściwość jest ustawiona na `True` (ustawienie domyślne) są renderowane jako puste pola tekstowe, w którym użytkownik może podać wartości pola nowego rekordu. Podobnie CheckBoxFields, które są wyłączone w interfejsie standardowe, tylko do odczytu, są konwertowane do włączonego pola wyboru w edycji i wstawiania interfejsów.

Wartość domyślna, edycji i wstawiania interfejsów elementu BoundField i CheckBoxField mogą być przydatne, interfejs brakuje dowolny rodzaj sprawdzania poprawności. Jeśli użytkownik popełni błąd danych wpis — takich jak pominięcie `ProductName` pola lub wprowadzając nieprawidłową wartością dla `UnitsInStock` (na przykład -50) będzie zgłaszany wyjątek z w ramach głębokości architektury aplikacji. Gdy ten wyjątek może być bezpiecznie obsługiwany, jak pokazano w [poprzedniego samouczka](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb.md), najlepiej edycji i wstawiania interfejsu użytkownika obejmuje kontrolek weryfikacji, aby uniemożliwić użytkownikowi wprowadzanie takich nieprawidłowe dane w pierwsze miejsce.

Aby podasz niestandardowe edycji lub wstawianie interfejsu, należy zastąpić elementu BoundField lub CheckBoxField TemplateField. Kontrolek TemplateField, które były temat dyskusji w [za pomocą kontrolek TemplateField w kontrolce GridView](../custom-formatting/using-templatefields-in-the-gridview-control-cs.md) i [za pomocą kontrolek TemplateField w kontrolce DetailsView](../custom-formatting/using-templatefields-in-the-detailsview-control-vb.md) samouczki i może składać się z wielu Definiowanie szablonów oddzielnych interfejsów wiersza różne stany. TemplateField `ItemTemplate` służy do podczas renderowania pól tylko do odczytu lub wiersze w kontrolkach DetailsView lub GridView, natomiast `EditItemTemplate` i `InsertItemTemplate` wskazują interfejsy do edycji i wstawiania tryby, odpowiednio.

W tym samouczku zobaczymy, jak łatwo jest dodawanie kontrolek weryfikacji do TemplateField `EditItemTemplate` i `InsertItemTemplate` zapewnia bardziej niezawodna interfejs użytkownika. W szczególności ten samouczek przedstawia przykład utworzone w [badanie zdarzeń skojarzonych z Wstawianie, aktualizowanie i usuwanie](examining-the-events-associated-with-inserting-updating-and-deleting-vb.md) samouczek i rozszerzają edycji i wstawiania interfejsy do uwzględnienia odpowiednie sprawdzania poprawności.

## <a name="step-1-replicating-the-example-fromexamining-the-events-associated-with-inserting-updating-and-deletingexamining-the-events-associated-with-inserting-updating-and-deleting-vbmd"></a>Krok 1: Replikowanie przykładu z[badanie zdarzeń skojarzonych z Wstawianie, aktualizowanie i usuwanie](examining-the-events-associated-with-inserting-updating-and-deleting-vb.md)

W [badanie zdarzeń skojarzonych z Wstawianie, aktualizowanie i usuwanie](examining-the-events-associated-with-inserting-updating-and-deleting-vb.md) samouczka utworzona strona, która wymienione nazwy i ceny produktów w edycji kontrolki GridView. Ponadto strona ujęte DetailsView którego `DefaultMode` właściwość `Insert`, a tym samym zawsze renderowanie w trybie wstawiania. Z tym DetailsView użytkownik może wprowadź nazwę i cena dla nowych produktów, kliknij przycisk Wstaw i jest dodawane do systemu (patrz rysunek 1).


[![Poprzedni przykład umożliwia użytkownikom dodawanie nowych produktów i edytować istniejące zadania](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image2.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image1.png)

**Rysunek 1**: poprzedniego przykładu umożliwia użytkownikom dodawanie nowych produktów i edytować istniejące ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image3.png))


Naszym celem na potrzeby tego samouczka jest rozszerzyć DetailsView i GridView, aby zapewnić kontrolkami walidacji. W szczególności naszych logikę weryfikacji wykonują następujące czynności:

- Wymaga podania nazwy podczas wstawiania lub edytowania produktu
- Wymaga podania cena przy wstawianiu rekordu Podczas edytowania rekordu, firma Microsoft nadal będzie wymagać ceny, ale będzie użyć logiki programowej w GridView `RowUpdating` już istnieje z wcześniejszych samouczka program obsługi zdarzeń
- Upewnij się, że wartość wprowadzona w cenie formacie waluty prawidłowe

Zanim można przyjrzymy się rozszerzając poprzedniego przykładu, aby uwzględnić sprawdzania poprawności, najpierw trzeba replikować przykładu z `DataModificationEvents.aspx` strony do strony, w tym samouczku `UIValidation.aspx`. W tym potrzebujemy do skopiowania zarówno `DataModificationEvents.aspx` oznaczeniu deklaracyjnym strony i jego kod źródłowy. Najpierw skopiuj oznaczeniu deklaracyjnym, wykonując następujące czynności:

1. Otwórz `DataModificationEvents.aspx` strony w programie Visual Studio
2. Przejdź do strony oznaczeniu deklaracyjnym (kliknij przycisk źródło w dolnej części strony)
3. Kopiuj tekst w obrębie `<asp:Content>` i `</asp:Content>` tagi (linie 3 – 44), jak pokazano na rysunku 2.


[![Kopiuj tekst w &lt;asp: Content&gt; kontroli](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image5.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image4.png)

**Rysunek 2**: Skopiuj tekst w `<asp:Content>` kontroli ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image6.png))


1. Otwórz `UIValidation.aspx` strony
2. Przejdź do strony oznaczeniu deklaracyjnym
3. Wklej tekst w obrębie `<asp:Content>` kontroli.

Aby skopiować kod źródłowy, otwórz `DataModificationEvents.aspx.vb` strony i skopiować właśnie tekst *w ramach* `EditInsertDelete_DataModificationEvents` klasy. Skopiuj obsługi trzech zdarzeń (`Page_Load`, `GridView1_RowUpdating`, i `ObjectDataSource1_Inserting`), jednak **nie** skopiuj deklaracji klasy. Wklej skopiowany tekst *w ramach* `EditInsertDelete_UIValidation` klasy w `UIValidation.aspx.vb`.

Po przeniesieniu za pośrednictwem treści i kodu z `DataModificationEvents.aspx` do `UIValidation.aspx`, Poświęć chwilę, w celu przetestowania swoje postępy w przeglądarce. Powinien zostać wyświetlony takie same dane wyjściowe i środowisko funkcji w każdym z tych dwóch stronach (odnoszą się do rysunku 1 dla zrzut ekranu `DataModificationEvents.aspx` w działaniu).

## <a name="step-2-converting-the-boundfields-into-templatefields"></a>Krok 2: Konwersji BoundFields do kontrolek TemplateField

Aby dodać kontrolek weryfikacji do interfejsów edycji i wstawianie, BoundFields używany przez formanty DetailsView i GridView muszą zostać przekonwertowane do kontrolek TemplateField. Aby to osiągnąć, kliknij linki Edytuj kolumny i Edytuj pola w kontrolkami GridView i DetailsView tagów inteligentnych, odpowiednio. Istnieje, zaznacz wszystkie BoundFields, a następnie kliknij link "Konwertuj to pole na TemplateField".


[![Konwertowanie każdego BoundFields DetailsView i GridView kontrolek TemplateField](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image8.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image7.png)

**Rysunek 3**: Konwertuj każdego DetailsView i GridView BoundFields do kontrolek TemplateField ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image9.png))


Konwertowanie elementu BoundField na TemplateField za pomocą okna dialogowego pola generuje TemplateField, która wykazuje te same interfejsy, tylko do odczytu, edycji i wstawianie elementu BoundField, sam. Następujący kod przedstawia składni deklaratywnej dla `ProductName` pole DetailsView po został przekonwertowany na TemplateField:

[!code-aspx[Main](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/samples/sample1.aspx)]

Należy pamiętać, że ta TemplateField miał trzy szablony tworzone automatycznie `ItemTemplate`, `EditItemTemplate`, i `InsertItemTemplate`. `ItemTemplate` Wyświetla wartość pola danych jednego (`ProductName`) za pomocą formantu sieci Web etykiety, podczas gdy `EditItemTemplate` i `InsertItemTemplate` przedstawiają wartości pola danych w kontrolce TextBox w sieci Web, która kojarzy pola danych za pomocą pole tekstowe `Text` Właściwość przy użyciu dwukierunkowego wiązania danych. Ponieważ tylko używamy DetailsView na tej stronie do wrzucania, możesz usunąć `ItemTemplate` i `EditItemTemplate` z dwóch kontrolek TemplateField, mimo że nie przynosi żadnych szkód w pozostawiania je.

Ponieważ widoku GridView nie obsługuje wbudowanej Wstawianie funkcji DetailsView, konwertowania GridView `ProductName` pole TemplateField powoduje tylko `ItemTemplate` i `EditItemTemplate`:

[!code-aspx[Main](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/samples/sample2.aspx)]

Klikając pozycję "Convert to pole do TemplateField", programu Visual Studio został utworzony TemplateField, w których szablonów naśladować interfejs użytkownika elementu przekonwertowanego BoundField. Można to sprawdzić, przechodząc do tej strony za pośrednictwem przeglądarki. Przekonasz się, że wygląd i zachowanie kontrolek TemplateField jest taka sama jak środowisko podczas BoundFields były używane zamiast tego.

> [!NOTE]
> Możesz dostosować edycji interfejsów w szablonach, zgodnie z potrzebami. Na przykład warto mieć w polu tekstowym w `UnitPrice` kontrolek TemplateField renderowane jako pole tekstowe z mniejszych niż `ProductName` pola tekstowego. W tym celu można ustawić pole tekstowe `Columns` właściwość do odpowiedniej wartości lub podać bezwzględne szerokość za pośrednictwem `Width` właściwości. W następnym samouczku zobaczymy całkowicie Dostosowywanie interfejsu edycji, zastępując wprowadzania danych alternatywny kontrolki sieci Web w polu tekstowym.


## <a name="step-3-adding-the-validation-controls-to-the-gridviewsedititemtemplate-s"></a>Krok 3: Dodawanie kontrolek weryfikacji do GridView`EditItemTemplate` s

Podczas tworzenia formularzy wprowadzania danych, ważne jest użytkownikom wprowadzanie wszystkich wymaganych pól oraz czy wszystkie podane dane wejściowe są wartości prawne, niepoprawnie sformatowany. Aby upewnić się, że dane wejściowe użytkownika są prawidłowe, ASP.NET zawiera pięć formantów wbudowanych sprawdzania poprawności, które są przeznaczone do użycia w celu sprawdzenia wartości pojedynczej kontrolki wprowadzania:

- [RequiredFieldValidator](https://msdn.microsoft.com/library/5hbw267h(VS.80).aspx) gwarantuje, że podać wartość
- [CompareValidator](https://msdn.microsoft.com/library/db330ayw(VS.80).aspx) sprawdza poprawność wartości z inną wartością formantu sieci Web lub wartości stałej lub gwarantuje, że format wartości jest dozwolony dla określonego typu danych
- [RangeValidator](https://msdn.microsoft.com/library/f70d09xt.aspx) gwarantuje, że wartość znajduje się w zakresie wartości
- [RegularExpressionValidator](https://msdn.microsoft.com/library/eahwtc9e.aspx) weryfikuje czy wartość [wyrażeń regularnych](http://en.wikipedia.org/wiki/Regular_expression)
- [CustomValidator](https://msdn.microsoft.com/library/9eee01cx(VS.80).aspx) weryfikuje czy wartość niestandardowych, zdefiniowanych przez użytkownika — metoda

Aby uzyskać więcej informacji na temat tych pięć formantów, zapoznaj się [sekcji kontrolkami walidacji](https://quickstarts.asp.net/quickstartv20/aspnet/doc/ctrlref/validation/default.aspx) z [samouczki Szybki Start platformy ASP.NET](https://asp.net/QuickStart/aspnet/).

W naszym samouczku musimy użyć RequiredFieldValidator w DetailsView i GridView `ProductName` kontrolek TemplateField i RequiredFieldValidator w DetailsView `UnitPrice` TemplateField. Ponadto należy dodać CompareValidator do obie kontrolki `UnitPrice` kontrolek TemplateField, który gwarantuje, że wprowadzona cena ma wartość większą niż lub równa 0 i są prezentowane w formacie waluty prawidłowe.

> [!NOTE]
> Podczas gdy ASP.NET 1.x miał tych tych samych kontrolek pięć sprawdzania poprawności, ASP.NET 2.0 dodał szereg ulepszeń, głównym, dwa skryptu po stronie klienta jest obsługa przeglądarek innych niż Internet Explorer i możliwość partycji sprawdzania poprawności formantów na stronie do Sprawdzanie poprawności grupy. Aby uzyskać więcej informacji na temat nowych funkcji kontroli sprawdzania poprawności w wersji 2.0, zobacz [analiza formanty sprawdzania poprawności w programie ASP.NET 2.0](http://aspnet.4guysfromrolla.com/articles/112305-1.aspx).


Zacznijmy od Dodawanie kontrolek weryfikacji niezbędne, aby `EditItemTemplate` s w kontrolek TemplateField w kontrolce GridView. Aby to zrobić, kliknij link Edytuj szablony z GridView tagu inteligentnego, aby wyświetlić interfejs edytowania szablonu. W tym miejscu można wybrać szablon, który do edycji z listy rozwijanej. Ponieważ chcemy rozszerzyć interfejs edytowania, musimy dodać kontrolek weryfikacji do `ProductName` i `UnitPrice`firmy `EditItemTemplate` s.


[![Należy rozszerzyć właściwościami ProductName i EditItemTemplates UnitPrice firmy](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image11.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image10.png)

**Rysunek 4**: musimy Rozszerz `ProductName` i `UnitPrice`firmy `EditItemTemplate` s ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image12.png))


W `ProductName` `EditItemTemplate`, Dodaj RequiredFieldValidator, przeciągając go z przybornika do interfejsu edycji szablonu, umieszczając za pole tekstowe.


[![Dodaj RequiredFieldValidator do ProductName EditItemTemplate](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image14.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image13.png)

**Rysunek 5**: Dodaj RequiredFieldValidator do `ProductName` `EditItemTemplate` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image15.png))


Wszystkie formanty sprawdzania poprawności działa, sprawdzając poprawność danych wejściowych z jednego formantu sieci Web platformy ASP.NET. W związku z tym, należy wskazać, że RequiredFieldValidator właśnie dodaliśmy, należy przeprowadzić walidacji względem pola tekstowego w `EditItemTemplate`; jest to realizowane przez ustawienie formant sprawdzania poprawności [Właściwość ControlToValidate elementu](https://msdn.microsoft.com/library/system.web.ui.webcontrols.basevalidator.controltovalidate(VS.80).aspx) do `ID` właściwej kontroli sieci Web. Pole tekstowe ma obecnie zamiast nieopisane `ID` z `TextBox1`, ale możemy zmienić ją na coś bardziej odpowiedniego. Kliknij w polu tekstowym w szablonie, a następnie w oknie Właściwości zmień `ID` z `TextBox1` do `EditProductName`.


[![Zmiana Identyfikatora pole tekstowe do EditProductName](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image17.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image16.png)

**Rysunek 6**: Zmień pole tekstowe `ID` do `EditProductName` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image18.png))


Następnym etapem jest skonfigurowanie RequiredFieldValidator `ControlToValidate` właściwość `EditProductName`. Wreszcie, ustaw [komunikat o błędzie właściwości](https://msdn.microsoft.com/library/system.web.ui.webcontrols.basevalidator.errormessage(VS.80).aspx) do "Należy podać nazwę produktu" i [właściwości Text](https://msdn.microsoft.com/library/system.web.ui.webcontrols.basevalidator.text(VS.80).aspx) do "\*". `Text` Wartość właściwości podana, czy tekst, który jest wyświetlany przez kontrolkę sprawdzania poprawności w przypadku niepowodzenia weryfikacji. `ErrorMessage` Wartości właściwości, która jest wymagana, jest używana przez kontrolki podsumowania walidacji; Jeśli `Text` wartość właściwości jest pominięty, `ErrorMessage` wartość właściwości jest również tekstu wyświetlanego przez formant sprawdzania poprawności na nieprawidłowe dane wejściowe.

Po ustawieniu tych trzech właściwości RequiredFieldValidator, ekran powinien wyglądać podobnie jak rysunek 7.


[![Ustawianie właściwości tekstu, komunikat o błędzie i ControlToValidate elementu RequiredFieldValidator](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image20.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image19.png)

**Rysunek 7**: Ustaw RequiredFieldValidator `ControlToValidate`, `ErrorMessage`, i `Text` właściwości ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image21.png))


Za pomocą RequiredFieldValidator dodane do `ProductName` `EditItemTemplate`, wszystkie, że pozostaje tylko niezbędne sprawdzania poprawności, aby dodać `UnitPrice` `EditItemTemplate`. Ponieważ firma Microsoft decydujesz, dla tej strony `UnitPrice` jest opcjonalne, jeśli edytowania rekordu, nie musimy dodać RequiredFieldValidator. Jednak zrobić należy dodać CompareValidator, aby upewnić się, że `UnitPrice`, jeśli zostanie podany, jest poprawnie sformatowany jako walutę i jest większa lub równa 0.

Zanim dodamy CompareValidator do `UnitPrice` `EditItemTemplate`, zmienimy najpierw identyfikator formantu sieci Web w polu tekstowym z `TextBox2` do `EditUnitPrice`. Po wprowadzeniu tej zmiany należy dodać CompareValidator, ustawiając jego `ControlToValidate` właściwości `EditUnitPrice`, jego `ErrorMessage` właściwość "cena musi być większa lub równa zero i nie może zawierać symbolu waluty" i jego `Text` właściwości "\*".

Aby wskazać, że `UnitPrice` wartość musi być większa lub równa 0, ustaw CompareValidator [właściwości Operator](https://msdn.microsoft.com/library/system.web.ui.webcontrols.comparevalidator.operator(VS.80).aspx) do `GreaterThanEqual`, jego [właściwość ValueToCompare](https://msdn.microsoft.com/library/system.web.ui.webcontrols.comparevalidator.valuetocompare(VS.80).aspx) "0" i jego [ Właściwość typu](https://msdn.microsoft.com/library/system.web.ui.webcontrols.basecomparevalidator.type.aspx) do `Currency`. Poniższej składni deklaratywnej `UnitPrice` firmy TemplateField `EditItemTemplate` po tych zmian:

[!code-aspx[Main](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/samples/sample3.aspx)]

Po wprowadzeniu tych zmian, otwórz stronę w przeglądarce. Jeśli użytkownik spróbuje pominąć nazwę lub wprowadź wartość Nieprawidłowa cena podczas edytowania produktu, gwiazdkę pojawia się obok pola tekstowego. Jak pokazano na rysunku 8, która zawiera symbol waluty, takich jak cenie od 19,95 USD wartość cena jest uznawane za nieprawidłowe. CompareValidator `Currency` `Type` umożliwia separatory cyfr (na przykład kropki, w zależności od ustawień kultury i przecinki) i wiodący znak plus lub minus, ale *nie* zezwala na symbol waluty. To zachowanie może perplex użytkowników interfejsu edycji obecnie renderowanie `UnitPrice` przy użyciu formatu waluty.

> [!NOTE]
> Pamiętaj, że w *wydarzeń związanych z Wstawianie, aktualizowanie i usuwanie* samouczku ustawimy elementu BoundField `DataFormatString` właściwość `{0:c}` Aby sformatować je jako walutę. Ponadto możemy ustawić `ApplyFormatInEditMode` właściwości na wartość true, powodując widoku GridView przez edytowanie interfejsu do sformatowania `UnitPrice` jako walutę. Podczas konwertowania elementu BoundField na TemplateField w Visual Studio wymienionych te ustawienia i sformatowane pole tekstowe `Text` właściwość jako walutę przy użyciu składni wiązania danych `<%# Bind("UnitPrice", "{0:c}") %>`.


[![Gwiazdka pojawia się obok pola tekstowe z nieprawidłowe dane wejściowe](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image23.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image22.png)

**Rysunek 8**: gwiazdką pojawi się obok pola tekstowe z nieprawidłowe dane wejściowe ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image24.png))


Podczas sprawdzania poprawności działania — jest, użytkownik będzie musiał ręcznie usunąć symbol waluty, podczas edytowania rekordu, który nie jest dopuszczalna. Aby rozwiązać ten problem, mamy trzy opcje:

1. Konfigurowanie `EditItemTemplate` tak, aby `UnitPrice` wartość nie jest sformatowany jako walutę.
2. Zezwalaj użytkownikowi wprowadzanie symbolu waluty, usuwając CompareValidator i zamianę RegularExpressionValidator, który prawidłowo sprawdza, czy wartość waluty poprawnie sformatowane. Ten problem, w tym miejscu jest wyrażenie regularne, aby sprawdzić wartość waluty nie jest łatwa i wymagałoby pisanie kodu, jeśli chcemy dołączyć ustawienia kultury.
3. Całkowicie usunąć formant sprawdzania poprawności i zależą od logiki weryfikacji po stronie serwera w GridView `RowUpdating` programu obsługi zdarzeń.

Użyjmy opcja #1 na potrzeby tego ćwiczenia. Obecnie `UnitPrice` są sformatowane jako walutę z powodu wyrażenia wiązania danych dla pola tekstowego w `EditItemTemplate`: `<%# Bind("UnitPrice", "{0:c}") %>`. Zmień instrukcję powiązania `Bind("UnitPrice", "{0:n2}")`, która formatuje wynik jako liczbę z dwoma cyframi precyzji. Można to zrobić bezpośrednio za pomocą składni deklaratywnej lub klikając łącze edycji powiązania danych z `EditUnitPrice` polu tekstowym w `UnitPrice` firmy TemplateField `EditItemTemplate` (patrz rysunki 9 i 10).


[![Kliknij link Edytuj powiązania danych, pole tekstowe](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image26.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image25.png)

**Rysunek 9**: kliknij link Edytuj powiązania danych w polu tekstowym ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image27.png))


[![Określ specyfikatora formatu w instrukcji powiązania](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image29.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image28.png)

**Na rysunku nr 10**: Określ specyfikatora formatu w `Bind` — instrukcja ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image30.png))


Dzięki tej zmianie sformatowane ceny w interfejsie edycji zawiera przecinki jako separator grup i kropki jako separatora dziesiętnego, ale pozostawia off symbol waluty.

> [!NOTE]
> `UnitPrice` `EditItemTemplate` Nie obejmuje RequiredFieldValidator, umożliwiając odświeżenie strony, aby zaktualizować logikę w celu rozpoczęcia. Jednak `RowUpdating` skopiowanych z programu obsługi zdarzeń *badanie zdarzeń skojarzonych z Wstawianie, aktualizowanie i usuwanie* samouczek obejmuje programowe Sprawdź, czy masz pewność, że `UnitPrice` podano. Możesz usunąć tę logikę, należy pozostawić ją jako — jest lub dodać RequiredFieldValidator do `UnitPrice` `EditItemTemplate`.


## <a name="step-4-summarizing-data-entry-problems"></a>Krok 4: Podsumowanie problemów zapis danych

Oprócz pięciu sprawdzania poprawności formantów, program ASP.NET zawiera [kontrolki podsumowania walidacji](https://msdn.microsoft.com/library/f9h59855(VS.80).aspx), który wyświetla `ErrorMessage` s te mechanizmy sprawdzania poprawności wykryto nieprawidłowe dane. Te dane podsumowania mogą być wyświetlane w postaci tekstu na stronie sieci web lub za pośrednictwem messagebox modalne, po stronie klienta. Umożliwia zwiększenie tego samouczka, aby uwzględnić messagebox po stronie klienta, podsumowanie problemów weryfikacji.

Aby to zrobić, przeciągnij kontrolki podsumowania walidacji z przybornika do projektanta. Położenie formantu sprawdzania poprawności nie ma znaczenia, ponieważ będziemy skonfigurować tak, aby tylko wyświetlić podsumowanie jako komunikat messagebox. Po dodaniu kontrolki, ustaw jego [właściwość ShowSummary](https://msdn.microsoft.com/library/system.web.ui.webcontrols.validationsummary.showsummary(VS.80).aspx) do `False` i jego [właściwość ShowMessageBox](https://msdn.microsoft.com/library/system.web.ui.webcontrols.validationsummary.showmessagebox(VS.80).aspx) do `True`. Dodając ten wszelkie błędy sprawdzania poprawności są podsumowane w messagebox po stronie klienta.


[![Błędy sprawdzania poprawności są podsumowane w Messagebox po stronie klienta](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image32.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image31.png)

**Rysunek 11**: błędy sprawdzania poprawności są podsumowane w Messagebox po stronie klienta ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image33.png))


## <a name="step-5-adding-the-validation-controls-to-the-detailsviewsinsertitemtemplate"></a>Krok 5: Dodawanie kontrolek weryfikacji do DetailsView`InsertItemTemplate`

W tym samouczku pozostaje na dodawanie kontrolek weryfikacji do interfejsu Wstawianie DetailsView. Dodawanie kontrolek weryfikacji do szablonów DetailsView proces jest identyczna jak sprawdzane w kroku 3; w związku z tym firma Microsoft będzie breeze za pośrednictwem zadań w tym kroku. Ile My mieliśmy przy użyciu GridView `EditItemTemplate` s, zachęcam Cię do zmiany nazwy `ID` s pola tekstowe z nieopisane `TextBox1` i `TextBox2` do `InsertProductName` i `InsertUnitPrice`.

Dodaj RequiredFieldValidator do `ProductName` `InsertItemTemplate`. Ustaw `ControlToValidate` do `ID` pola tekstowego w szablonie, jego `Text` właściwość "\*" i jego `ErrorMessage` właściwość "Należy podać nazwę produktu".

Ponieważ `UnitPrice` jest wymagany dla tej strony, podczas dodawania nowego rekordu, Dodaj RequiredFieldValidator do `UnitPrice` `InsertItemTemplate`, ustawiając jego `ControlToValidate`, `Text`, i `ErrorMessage` właściwości odpowiednio. Na koniec należy dodać CompareValidator do `UnitPrice` `InsertItemTemplate` jak również konfigurowanie jej `ControlToValidate`, `Text`, `ErrorMessage`, `Type`, `Operator`, i `ValueToCompare` właściwości tak samo, jak postępowanie z `UnitPrice`firmy CompareValidator w GridView `EditItemTemplate`.

Po dodaniu tych kontrolek walidacji, nowy produkt nie dodawane do systemu, jeśli nie podano nazwy lub jej miesięczna cena jest liczbą ujemną lub nielegalny sformatowany.


[![Dodano logikę walidacji interfejsu Wstawianie DetailsView](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image35.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image34.png)

**Rysunek 12**: logikę weryfikacji została dodana do interfejsu Wstawianie DetailsView ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image36.png))


## <a name="step-6-partitioning-the-validation-controls-into-validation-groups"></a>Krok 6: Partycjonowania kontrolek weryfikacji do sprawdzania poprawności grupy

Naszą stronę składa się z dwóch zestawów logicznie różnych kontrolek weryfikacji: te, które odnoszą się do widoku GridView przez edytowanie interfejsu i te, które odpowiadają DetailsView użytkownika wstawiania interfejsu. Domyślnie, gdy wystąpi ogłaszania zwrotnego *wszystkich* sprawdzania poprawności formantów na stronie są sprawdzane. Jednak podczas edytowania rekordu nie chcemy DetailsView interfejsu Wstawianie kontrolek weryfikacji do sprawdzania poprawności. Rysunek 13 przedstawiono nasze bieżące dilemma po użytkownik edytuje produktu przy użyciu wartości doskonale prawne, klikając aktualizacja powoduje błąd sprawdzania poprawności, ponieważ wartości nazwy i ceny, wstawianie interfejsu są puste.


[![Aktualizowanie produktu powoduje, że interfejs Wstawianie kontrolek weryfikacji do uruchomienia](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image38.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image37.png)

**Rysunek 13**: Trwa aktualizowanie produktu powoduje, że interfejs Wstawianie kontrolek weryfikacji do uruchomienia ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image39.png))


Formanty sprawdzania poprawności w programie ASP.NET 2.0 można podzielić na partycje do sprawdzania poprawności grupy za pomocą ich `ValidationGroup` właściwości. Aby skojarzyć zestaw kontrolek weryfikacji do grupy, wystarczy ustawić ich `ValidationGroup` właściwość taką samą wartość. W naszym samouczku ustaw `ValidationGroup` właściwości kontrolki GridView kontrolek TemplateField w celu z weryfikacji `EditValidationControls` i `ValidationGroup` właściwości DetailsView kontrolek TemplateField w celu `InsertValidationControls`. Te zmiany może odbywać się bezpośrednio w oznaczeniu deklaracyjnym lub w oknie właściwości, korzystając z projektanta Edytuj szablon interfejsu.

Oprócz sprawdzania poprawności formantów, przycisk i formanty przycisku w programie ASP.NET 2.0 również obejmować `ValidationGroup` właściwości. Grupy sprawdzania poprawności modułów sprawdzania poprawności są sprawdzane ważności tylko, gdy ogłaszania zwrotnego wywołane przez przycisk, który ma taką samą `ValidationGroup` ustawienie właściwości. Na przykład, w kolejności DetailsView przycisk Wstaw wyzwolić `InsertValidationControls` grupy sprawdzania poprawności, musimy CommandField `ValidationGroup` właściwość `InsertValidationControls` (zobacz rysunek 14). Ponadto, ustawić GridView firmy CommandField `ValidationGroup` właściwość `EditValidationControls`.


[![Zestaw DetailsView przez właściwość ValidationGroup firmy CommandField InsertValidationControls](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image41.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image40.png)

**Rysunek 14**: Ustaw DetailsView firmy CommandField `ValidationGroup` właściwości `InsertValidationControls` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image42.png))


Po wprowadzeniu tych zmian DetailsView GridView kontrolek TemplateField i CommandFields powinien wyglądać podobnie do następującego:

Kontrolek TemplateField i CommandField DetailsView

[!code-aspx[Main](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/samples/sample4.aspx)]

CommandField i kontrolek TemplateField w widoku GridView

[!code-aspx[Main](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/samples/sample5.aspx)]

W tym momencie formanty sprawdzania poprawności specyficznych dla edycji ognia tylko po kliknięciu przycisku aktualizacji GridView i fire formanty sprawdzania poprawności specyficznych dla insert tylko wtedy, gdy kliknięto przycisk Wstaw DetailsView rozwiązywania problemu wyróżniony przez rysunek 13. Jednak dzięki tej zmianie naszych kontrolki podsumowania walidacji nie jest już wyświetlany podczas wprowadzania nieprawidłowe dane. Zawiera także kontrolki podsumowania walidacji `ValidationGroup` właściwość i tylko pokazuje informacje podsumowujące dla tych kontrolek walidacji w jego grupie sprawdzania poprawności. Dlatego musimy mieć dwóch kontrolek walidacji na tej stronie, jeden dla `InsertValidationControls` sprawdzania poprawności grupy, a drugi dla `EditValidationControls`.

[!code-aspx[Main](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/samples/sample6.aspx)]

W przypadku to dodawanie Nasz samouczek dotyczący jest pełny.

## <a name="summary"></a>Podsumowanie

Gdy BoundFields zapewniają zarówno interfejs Wstawianie i edycji, interfejs nie jest możliwe do dostosowania. Często chcemy Dodawanie kontrolek weryfikacji do edycji i wstawiania interfejsu, aby upewnić się, że użytkownik musi wprowadzić wymagane dane wejściowe w formacie prawnych. W tym celu możemy przekonwertować BoundFields kontrolek TemplateField i dodawanie kontrolek weryfikacji do odpowiednie szablony. W tym samouczku będziemy rozszerzone przykładu z *badanie zdarzeń skojarzonych z Wstawianie, aktualizowanie i usuwanie* samouczek, dodawanie kontrolek weryfikacji do obu DetailsView przez wstawianie interfejsu i w widoku GridView Edytowanie interfejsu. Ponadto widzieliśmy sposób wyświetlania informacji podsumowania weryfikacji za pomocą kontrolki podsumowania walidacji i sposobie partycjonowania formanty sprawdzania poprawności na stronie w odrębnych sprawdzania poprawności grupy.

Jak widzieliśmy w tym samouczku kontrolek TemplateField umożliwiają interfejsów edycji i wstawianie uzupełnić obliczonymi obejmujący kontrolkami walidacji. Kontrolek TemplateField można również rozszerzać uwzględnienie dodatkowych danych wejściowych kontrolki sieci Web, umożliwiając pole tekstowe, które mają zostać zastąpione przez bardziej odpowiedni formant sieci Web. W naszym samouczku dalej zobaczymy, jak zastąpić formant pola tekstowego przy użyciu kontrolki DropDownList powiązanych z danymi, co jest idealnym rozwiązaniem podczas edytowania klucz obcy (takie jak `CategoryID` lub `SupplierID` w `Products` tabeli).

Wszystkiego najlepszego programowania!

## <a name="about-the-author"></a>Informacje o autorze

[Scott Bento](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor siedem ASP/ASP.NET książek i założycielem [4GuysFromRolla.com](http://www.4guysfromrolla.com), pracował nad przy użyciu technologii Microsoft Web od 1998 r. Scott działa jako niezależny Konsultant, trainer i składnika zapisywania. Jego najnowszą książkę Stephena [ *Sams uczyć się ASP.NET 2.0 w ciągu 24 godzin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). ADAM można z Tobą skontaktować w [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) lub za pośrednictwem jego blogu, który znajduje się w temacie [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Specjalne podziękowania dla

W tej serii samouczków został zrecenzowany przez wielu recenzentów pomocne. Wiodące osób dokonujących przeglądu, w tym samouczku zostały Liz Shulok i Nowak Zack. Zainteresowani zapoznaniem Moje kolejnych artykułów MSDN? Jeśli tak, Porzuć mnie linii w [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Poprzednie](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb.md)
> [dalej](customizing-the-data-modification-interface-vb.md)
