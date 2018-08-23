---
uid: web-forms/overview/data-access/editing-and-deleting-data-through-the-datalist/adding-validation-controls-to-the-datalist-s-editing-interface-vb
title: Dodawanie kontrolek weryfikacji do elementu DataList jego edytowania interfejsu (VB) | Dokumentacja firmy Microsoft
author: rick-anderson
description: W tym samouczku zobaczymy, jak łatwo jest dodawanie kontrolek weryfikacji do DataList EditItemTemplate zapewnić bardziej niezawodne edycji int. użytkownika...
ms.author: riande
ms.date: 10/30/2006
ms.assetid: 6b073fc6-524d-453d-be7c-0c30986de391
msc.legacyurl: /web-forms/overview/data-access/editing-and-deleting-data-through-the-datalist/adding-validation-controls-to-the-datalist-s-editing-interface-vb
msc.type: authoredcontent
ms.openlocfilehash: 6fe5fcba322f3d3a37b862f0a85810d8b4dda5f4
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/16/2018
ms.locfileid: "41756458"
---
<a name="adding-validation-controls-to-the-datalists-editing-interface-vb"></a>Dodawanie kontrolek weryfikacji do interfejsu edycji DataList (VB)
====================
przez [Bento Scott](https://twitter.com/ScottOnWriting)

[Pobierz przykładową aplikację](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_39_VB.exe) lub [Pobierz plik PDF](adding-validation-controls-to-the-datalist-s-editing-interface-vb/_static/datatutorial39vb1.pdf)

> W tym samouczku zobaczymy, jak łatwo jest dodawanie kontrolek weryfikacji do DataList EditItemTemplate zapewnić bardziej niezawodne edycji interfejsu użytkownika.


## <a name="introduction"></a>Wprowadzenie

W elemencie DataList samouczki do tej pory edycji DataLists edycji interfejsów nie zostały uwzględnione wszelkie walidacji danych wejściowych użytkownika aktywnego, mimo że nieprawidłowego użytkownika dane wejściowe np. Brak nazwy produktu lub ceny powoduje wyjątek. W [poprzedni Samouczek](handling-bll-and-dal-level-exceptions-vb.md) zbadaliśmy sposób dodawania kodu do DataList s obsługi wyjątków `UpdateCommand` program obsługi zdarzeń, aby przechwycić i bez problemu zmieniała wyświetlać informacje na temat wyjątków, które zostały zgłoszone. W idealnym przypadku jednak interfejs edytowania obejmuje kontrolek weryfikacji, aby uniemożliwić użytkownikowi wprowadzanie takich nieprawidłowe dane w pierwszej kolejności.

W tym samouczku zobaczymy, jak łatwo jest dodawanie kontrolek weryfikacji do DataList s `EditItemTemplate` zapewnić bardziej niezawodne edycji interfejsu użytkownika. W szczególności w tym samouczku przyjmuje przykład utworzonej w poprzednim samouczku i rozszerzają interfejs edytowania obejmujący odpowiednie sprawdzania poprawności.

## <a name="step-1-replicating-the-example-fromhandling-bll--and-dal-level-exceptionshandling-bll-and-dal-level-exceptions-vbmd"></a>Krok 1: Replikowanie przykładu z[Obsługa wyjątków na poziomie warstwy DAL i LOGIKI](handling-bll-and-dal-level-exceptions-vb.md)

W [obsługi LOGIKI i wyjątków na poziomie warstwy DAL](handling-bll-and-dal-level-exceptions-vb.md) samouczka utworzona strona, która wymienione nazwy i ceny produktów w DataList dwie kolumny, można edytować. Naszym celem na potrzeby tego samouczka jest rozszerzyć interfejsu edycji s DataList obejmujący kontrolkami walidacji. W szczególności naszych logikę weryfikacji wykonują następujące czynności:

- Wymaga podania nazwy produktu s
- Upewnij się, że wartość wprowadzona w cenie formacie waluty prawidłowe
- Upewnij się, że wartość wprowadzona dla cena jest większa lub równa zero, ponieważ ujemnych `UnitPrice` wartość jest niedozwolona

Zanim można przyjrzymy się rozszerzając poprzedniego przykładu, aby uwzględnić sprawdzania poprawności, najpierw trzeba replikować przykładu z `ErrorHandling.aspx` strony w `EditDeleteDataList` folderu do strony, w tym samouczku `UIValidation.aspx`. W tym potrzebujemy do skopiowania zarówno `ErrorHandling.aspx` strony oznaczeniu deklaracyjnym s i jego kod źródłowy. Najpierw skopiuj oznaczeniu deklaracyjnym, wykonując następujące czynności:

1. Otwórz `ErrorHandling.aspx` strony w programie Visual Studio
2. Przejdź do strony s oznaczeniu deklaracyjnym (kliknij przycisk źródło w dolnej części strony)
3. Kopiuj tekst w obrębie `<asp:Content>` i `</asp:Content>` tagi (linie 3 – 32), jak pokazano na rysunku 1.


[![Kopiuj tekst w &lt;asp: Content&gt; kontroli](adding-validation-controls-to-the-datalist-s-editing-interface-vb/_static/image2.png)](adding-validation-controls-to-the-datalist-s-editing-interface-vb/_static/image1.png)

**Rysunek 2**: Skopiuj tekst w `<asp:Content>` kontroli ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](adding-validation-controls-to-the-datalist-s-editing-interface-vb/_static/image3.png))


1. Otwórz `UIValidation.aspx` strony
2. Przejdź do strony s oznaczeniu deklaracyjnym
3. Wklej tekst w obrębie `<asp:Content>` kontroli.

Aby skopiować kod źródłowy, otwórz `ErrorHandling.aspx.vb` strony i skopiować właśnie tekst *w ramach* `EditDeleteDataList_ErrorHandling` klasy. Skopiuj obsługi trzech zdarzeń (`Products_EditCommand`, `Products_CancelCommand`, i `Products_UpdateCommand`) wraz z `DisplayExceptionDetails` metody, ale czy **nie** skopiuj deklaracji klasy lub `using` instrukcji. Wklej skopiowany tekst *w ramach* `EditDeleteDataList_UIValidation` klasy w `UIValidation.aspx.vb`.

Po przeniesieniu za pośrednictwem treści i kodu z `ErrorHandling.aspx` do `UIValidation.aspx`, Poświęć chwilę, w celu przetestowania stron w przeglądarce. Należy wyświetlić ten sam wynik i środowisko funkcji w każdym z tych dwóch stronach (patrz rysunek 2).


[![Na stronie UIValidation.aspx naśladuje funkcjonalność ErrorHandling.aspx](adding-validation-controls-to-the-datalist-s-editing-interface-vb/_static/image5.png)](adding-validation-controls-to-the-datalist-s-editing-interface-vb/_static/image4.png)

**Rysunek 2**: `UIValidation.aspx` strony naśladuje funkcjonalność `ErrorHandling.aspx` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](adding-validation-controls-to-the-datalist-s-editing-interface-vb/_static/image6.png))


## <a name="step-2-adding-the-validation-controls-to-the-datalist-s-edititemtemplate"></a>Krok 2: Dodawanie kontrolek weryfikacji do DataList s EditItemTemplate

Podczas tworzenia formularzy wprowadzania danych, ważne jest użytkownikom wprowadzanie wszystkie wymagane pola, a wszystkie ich podane dane wejściowe są wartości prawne, niepoprawnie sformatowany. Aby upewnić się, że s danych wprowadzonych przez użytkownika są prawidłowe, ASP.NET zawiera pięć formantów wbudowanych sprawdzania poprawności, które są przeznaczone do sprawdzenia wartości pojedynczej kontrolki wprowadzania w sieci Web:

- [RequiredFieldValidator](https://msdn.microsoft.com/library/5hbw267h(VS.80).aspx) gwarantuje, że podać wartość
- [CompareValidator](https://msdn.microsoft.com/library/db330ayw(VS.80).aspx) sprawdza poprawność wartości z inną wartością formantu sieci Web lub wartości stałej lub gwarantuje, że format wartości s jest dozwolony dla określonego typu danych
- [RangeValidator](https://msdn.microsoft.com/library/f70d09xt.aspx) gwarantuje, że wartość znajduje się w zakresie wartości
- [RegularExpressionValidator](https://msdn.microsoft.com/library/eahwtc9e.aspx) weryfikuje czy wartość [wyrażeń regularnych](http://en.wikipedia.org/wiki/Regular_expression)
- [CustomValidator](https://msdn.microsoft.com/library/9eee01cx(VS.80).aspx) weryfikuje czy wartość niestandardowych, zdefiniowanych przez użytkownika — metoda

Aby uzyskać więcej informacji na temat tych pięć formantów odnoszą się do [Dodawanie kontrolek weryfikacji do edycji i wstawiania interfejsy](../editing-inserting-and-deleting-data/adding-validation-controls-to-the-editing-and-inserting-interfaces-vb.md) samouczka lub zapoznaj się [sekcji kontrolkami walidacji](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/ctrlref/validation/default.aspx) z [Samouczki Szybki Start platformy ASP.NET](https://quickstarts.asp.net).

W naszym samouczku musimy użyć RequiredFieldValidator, aby upewnić się, że została podana wartość dla nazwy produktu i CompareValidator, aby upewnić się, czy wprowadzona cena ma wartość większą niż lub równa 0 i są prezentowane w formacie waluty prawidłowe.

> [!NOTE]
> Podczas gdy ASP.NET 1.x miał tych tych samych kontrolek pięć sprawdzania poprawności, ASP.NET 2.0 dodał szereg ulepszeń, głównym, dwa skryptu po stronie klienta jest obsługa przeglądarek, oprócz programu Internet Explorer i możliwość partycji sprawdzania poprawności formantów na stronie do Sprawdzanie poprawności grupy. Aby uzyskać więcej informacji na temat nowych funkcji kontroli sprawdzania poprawności w wersji 2.0, zobacz [analiza formanty sprawdzania poprawności w programie ASP.NET 2.0](http://aspnet.4guysfromrolla.com/articles/112305-1.aspx).


Pozwól s początek Dodawanie kontrolek weryfikacji niezbędne do DataList s `EditItemTemplate`. To zadanie można wykonać za pomocą projektanta, klikając link Edytuj szablony z tagu inteligentnego s DataList lub za pomocą składni deklaratywnej. Pozwól s kroku przez proces, korzystając z opcji Edytuj szablony z widoku Projekt. Po wybraniu do edycji kontrolki DataList s `EditItemTemplate`, Dodaj RequiredFieldValidator, przeciągając go z przybornika do interfejsu edycji szablonu, umieszczając go po `ProductName` pola tekstowego.


[![Dodaj RequiredFieldValidator do EditItemTemplate po ProductName pole tekstowe](adding-validation-controls-to-the-datalist-s-editing-interface-vb/_static/image8.png)](adding-validation-controls-to-the-datalist-s-editing-interface-vb/_static/image7.png)

**Rysunek 3**: Dodaj RequiredFieldValidator do `EditItemTemplate After` `ProductName` pola tekstowego ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](adding-validation-controls-to-the-datalist-s-editing-interface-vb/_static/image9.png))


Wszystkie formanty sprawdzania poprawności działa, sprawdzając poprawność danych wejściowych z jednego formantu sieci Web platformy ASP.NET. W związku z tym, należy wskazać, że RequiredFieldValidator właśnie dodaliśmy, należy przeprowadzić walidacji względem `ProductName` pole tekstowe; odbywa się przez ustawienie sprawdzania poprawności s [ `ControlToValidate` właściwość](https://msdn.microsoft.com/library/system.web.ui.webcontrols.basevalidator.controltovalidate(VS.80).aspx) do `ID` programu odpowiednie kontrolki sieci Web (`ProductName`, w tym wystąpieniu). Następnym etapem jest skonfigurowanie [ `ErrorMessage` właściwość](https://msdn.microsoft.com/library/system.web.ui.webcontrols.basevalidator.errormessage(VS.80).aspx) do należy podać nazwę produktu s i [ `Text` właściwość](https://msdn.microsoft.com/library/system.web.ui.webcontrols.basevalidator.text(VS.80).aspx) do \*. `Text` Wartość właściwości podana, czy tekst, który jest wyświetlany przez kontrolkę sprawdzania poprawności w przypadku niepowodzenia weryfikacji. `ErrorMessage` Wartości właściwości, która jest wymagana, jest używana przez kontrolki podsumowania walidacji; Jeśli `Text` wartość właściwości jest pominięty, `ErrorMessage` wartość właściwości jest wyświetlany formantu sprawdzania poprawności na nieprawidłowe dane wejściowe.

Po ustawieniu tych trzech właściwości RequiredFieldValidator, ekran powinien wyglądać podobnie jak rysunek 4.


[![Ustaw ControlToValidate RequiredFieldValidator s obiektu, komunikat o błędzie i właściwości tekstu](adding-validation-controls-to-the-datalist-s-editing-interface-vb/_static/image11.png)](adding-validation-controls-to-the-datalist-s-editing-interface-vb/_static/image10.png)

**Rysunek 4**: Set RequiredFieldValidator s `ControlToValidate`, `ErrorMessage`, i `Text` właściwości ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](adding-validation-controls-to-the-datalist-s-editing-interface-vb/_static/image12.png))


Za pomocą RequiredFieldValidator dodane do `EditItemTemplate`, wszystkie, że pozostaje tylko dodać niezbędne sprawdzanie poprawności dla cena produktu s pola tekstowego. Ponieważ `UnitPrice` jest opcjonalny podczas edytowania rekordu, firma Microsoft don t trzeba dodać RequiredFieldValidator. Jednak zrobić należy dodać CompareValidator, aby upewnić się, że `UnitPrice`, jeśli zostanie podany, jest poprawnie sformatowany jako walutę i jest większa lub równa 0.

Dodaj CompareValidator do `EditItemTemplate` i ustaw jego `ControlToValidate` właściwości `UnitPrice`, jego `ErrorMessage` właściwość cena musi być większa lub równa zero i nie może zawierać symbolu waluty i jego `Text` właściwości \*. Aby wskazać, że `UnitPrice` wartość musi być większa lub równa 0, ustaw CompareValidator s [ `Operator` właściwość](https://msdn.microsoft.com/library/system.web.ui.webcontrols.comparevalidator.operator(VS.80).aspx) do `GreaterThanEqual`, jego [ `ValueToCompare` właściwość](https://msdn.microsoft.com/library/system.web.ui.webcontrols.comparevalidator.valuetocompare(VS.80).aspx) 0, i jego [ `Type` właściwość](https://msdn.microsoft.com/library/system.web.ui.webcontrols.basecomparevalidator.type.aspx) do `Currency`.

Po dodaniu tych dwóch weryfikacji kontrolek DataList s `EditItemTemplate` składni deklaratywnej s powinien wyglądać podobnie do poniższego:


[!code-aspx[Main](adding-validation-controls-to-the-datalist-s-editing-interface-vb/samples/sample1.aspx)]

Po wprowadzeniu tych zmian, otwórz stronę w przeglądarce. Jeśli użytkownik spróbuje pominąć nazwę lub wprowadź wartość Nieprawidłowa cena podczas edytowania produktu, gwiazdkę pojawia się obok pola tekstowego. Jak pokazano na rysunku 5, która zawiera symbol waluty, takich jak cenie od 19,95 USD wartość cena jest uznawane za nieprawidłowe. CompareValidator s `Currency` `Type` umożliwia separatory cyfr (na przykład kropki, w zależności od ustawień kultury i przecinki) i wiodący znak plus lub minus, ale *nie* zezwala na symbol waluty. To zachowanie może perplex użytkowników interfejsu edycji obecnie renderowanie `UnitPrice` przy użyciu formatu waluty.


[![Gwiazdka pojawia się obok pola tekstowe z nieprawidłowe dane wejściowe](adding-validation-controls-to-the-datalist-s-editing-interface-vb/_static/image14.png)](adding-validation-controls-to-the-datalist-s-editing-interface-vb/_static/image13.png)

**Rysunek 5**: gwiazdką pojawi się obok pola tekstowe z nieprawidłowe dane wejściowe ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](adding-validation-controls-to-the-datalist-s-editing-interface-vb/_static/image15.png))


Podczas sprawdzania poprawności działania — jest, użytkownik będzie musiał ręcznie usunąć symbol waluty, podczas edytowania rekordu, który nie jest dopuszczalna. Ponadto istnieją nieprawidłowe dane wejściowe, edycji interfejs żadna aktualizacja ani Anuluj przyciski, po kliknięciu wywoła ogłaszania zwrotnego. W idealnym przypadku przycisku Anuluj zwróci kontrolki DataList stan wstępnie edycji niezależnie od ważności danych s wprowadzonych przez użytkownika. Ponadto należy upewnić się, że dane strony s są prawidłowe przed zaktualizowaniem informacje o produkcie w elemencie DataList s `UpdateCommand` program obsługi zdarzeń jako logiki po stronie klienta, co może prowadzić przez użytkowników, których przeglądarki albo don obsługuje język JavaScript, masz formanty sprawdzania poprawności jej obsługa jest wyłączona.

## <a name="removing-the-currency-symbol-from-the-edititemtemplate-s-unitprice-textbox"></a>Usuwanie symbolu waluty z EditItemTemplate s UnitPrice TextBox

Korzystając z CompareValidator s `Currency``Type`, sprawdzają poprawność danych wejściowych nie może zawierać dowolny symbol waluty. Obecność takich symboli powoduje, że CompareValidator do oznaczania danych wejściowych jako nieprawidłowy. Jednak interfejsu edycji obecnie zawiera symbol waluty w `UnitPrice` pola tekstowego, co oznacza, użytkownik musi jawnie usunąć symbol waluty przed zapisaniem zmian. Aby rozwiązać ten problem mamy trzy opcje:

1. Konfigurowanie `EditItemTemplate` tak, aby `UnitPrice` wartość pola tekstowego jest sformatowana jako walutę.
2. Zezwalaj użytkownikowi wprowadzanie symbolu waluty, usuwając CompareValidator i zamianę RegularExpressionValidator, która sprawdza, czy wartość waluty poprawnie sformatowana. Żądanie, w tym miejscu jest wyrażenie regularne, aby sprawdzić wartość waluty nie jest tak proste jak CompareValidator i wymagałoby pisanie kodu, jeśli chcemy dołączyć ustawienia kultury.
3. Całkowicie usunąć formant sprawdzania poprawności i polegają na logikę niestandardowego sprawdzania poprawności po stronie serwera w GridView s `RowUpdating` programu obsługi zdarzeń.

Pozwól s zawsze pod ręką opcja 1 na potrzeby tego samouczka. Obecnie `UnitPrice` są sformatowane jako wartość waluty z powodu wyrażenia wiązania danych dla pola tekstowego w `EditItemTemplate`: `<%# Eval("UnitPrice", "{0:c}") %>`. Zmiana `Eval` instrukcję, aby `Eval("UnitPrice", "{0:n2}")`, która formatuje wynik jako liczbę z dwoma cyframi precyzji. Można to zrobić bezpośrednio za pomocą składni deklaratywnej lub klikając łącze edycji powiązania danych z `UnitPrice` pole tekstowe w elemencie DataList s `EditItemTemplate`.

Dzięki tej zmianie sformatowane ceny w interfejsie edycji zawiera przecinki jako separator grup i kropki jako separatora dziesiętnego, ale pozostawia off symbol waluty.

> [!NOTE]
> Podczas usuwania format waluty z interfejsu, które można edytować, czy Zajmuję się umieścić symbol waluty jako tekst poza pole tekstowe. Stanowi wskazówkę użytkownika, który nie ma potrzeby zapewnienie symbol waluty.


## <a name="fixing-the-cancel-button"></a>Naprawianie przycisk Anuluj

Domyślnie formanty sprawdzania poprawności w sieci Web emitować JavaScript do wykonywania sprawdzania poprawności po stronie klienta. Po kliknięciu przycisku, element LinkButton lub ImageButton, formanty sprawdzania poprawności na stronie są sprawdzane po stronie klienta, zanim nastąpi odświeżenie strony. W przypadku wszelkich nieprawidłowych danych zwrotu zostało anulowane. W przypadku niektórych przycisków jednak ważności danych mogą być nieistotne; w takim przypadku zwrotu anulowana z powodu nieprawidłowych danych jest uciążliwy.

Przykładem takiego znajduje się w przycisk Anuluj. Wyobraź sobie, czy użytkownik wprowadzi nieprawidłowe dane, takie jak nazwa produktu s, pomijając decyduje o nagrywa t chcesz przecież zapisanie produktu i trafienia przycisk Anuluj. Obecnie przycisku Anuluj wyzwala formanty sprawdzania poprawności na stronie, które wysyłają raporty, nazwa produktu brakujących elementów i zapobiec zwrotu. Nasze użytkownik będzie musiał wpisać jakiś tekst do `ProductName` TextBox tylko do anulujesz proces edycji.

Na szczęście mają Button, element LinkButton i ImageButton [ `CausesValidation` właściwość](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.causesvalidation.aspx) , można wskazać, czy kliknięcie przycisku powinno zainicjować logikę weryfikacji (wartość domyślna to `True`). Ustaw s przycisku Anuluj `CausesValidation` właściwość `False`.

## <a name="ensuring-the-inputs-are-valid-in-the-updatecommand-event-handler"></a>Zapewniając dane wejściowe są prawidłowe w elementu UpdateCommand program obsługi zdarzeń

Ze względu na skrypt po stronie klienta, wyemitowane przez formanty sprawdzania poprawności, jeśli użytkownik wprowadzi nieprawidłowe dane wejściowe formanty sprawdzania poprawności anulować wszelkie ogłaszania zwrotnego inicjowane przez przycisk LinkButton, lub ImageButton kontrolki, których `CausesValidation` właściwości są `True` ( ustawienie domyślne). Jednak jeśli użytkownik odwiedził z antiquated przeglądarki lub których obsługa języka JavaScript została wyłączona, nie będą wykonywane sprawdzanie poprawności po stronie klienta.

Wszystkie formanty weryfikacji platformy ASP.NET Powtórz logikę weryfikacji natychmiast po ogłaszania zwrotnego i raportowania ogólnej poprawność danych wejściowych strony s za pośrednictwem [ `Page.IsValid` właściwość](https://msdn.microsoft.com/library/system.web.ui.page.isvalid.aspx). Jednak przepływ strona nie zostanie przerwana lub zatrzymany w dowolny sposób na podstawie wartości `Page.IsValid`. Jako programiści, odpowiada za naszych upewnij się, że `Page.IsValid` właściwość ma wartość `True` przed kontynuowaniem kod, który zakłada prawidłowe dane wejściowe.

Jeśli użytkownik ma Obsługa skryptów JavaScript wyłączona, odwiedzić naszą stronę, edytuje produktu, wprowadza wartość ceny zbyt kosztowne i kliknięciu przycisku Aktualizuj weryfikacji po stronie klienta zostanie pominięta, a następnie nastąpi odświeżenie strony. Na odświeżenie strony, ASP.NET, strona s `UpdateCommand` wykonuje program obsługi zdarzeń i zgłaszany jest wyjątek podczas próby przeprowadzenia analizy zbyt kosztowne `Decimal`. Ponieważ mamy obsługi wyjątków, bez problemu zmieniała obsługi takiego wyjątku, ale firma Microsoft może uniemożliwić nieprawidłowe dane poślizg w pierwszej kolejności za pomocą tylko kontynuowanie przy użyciu `UpdateCommand` programu obsługi zdarzeń Jeśli `Page.IsValid` ma wartość `True`.

Dodaj następujący kod na początku `UpdateCommand` programu obsługi zdarzeń bezpośrednio przed `Try` bloku:


[!code-vb[Main](adding-validation-controls-to-the-datalist-s-editing-interface-vb/samples/sample2.vb)]

Dodając ten produkt będzie podejmować próby można zaktualizować tylko wtedy, gdy przesłane dane są prawidłowe. Większość użytkowników, których t mieć możliwość ogłaszanie zwrotne nieprawidłowych danych z powodu skrypty po stronie klienta formanty sprawdzania poprawności, ale użytkowników, których przeglądarek don obsługuje języka JavaScript lub obsługi języka JavaScript, które mają wyłączony, można pominąć sprawdzanie po stronie klienta i przesyłać nieprawidłowe dane.

> [!NOTE]
> Uważni czytnik będzie pamiętamy podczas aktualizowania danych przy użyciu GridView, firma Microsoft trzeba jawnie sprawdziła `Page.IsValid` właściwości w klasie CodeBehind nasze strony s. Jest to spowodowane konsultować się z widoku GridView `Page.IsValid` właściwości dla nas i tylko przychody z aktualizacją update tylko wtedy, gdy zwraca wartość `True`.


## <a name="step-3-summarizing-data-entry-problems"></a>Krok 3: Podsumowanie problemów zapis danych

Oprócz pięciu sprawdzania poprawności formantów, program ASP.NET zawiera [kontrolki podsumowania walidacji](https://msdn.microsoft.com/library/f9h59855(VS.80).aspx), który wyświetla `ErrorMessage` s te mechanizmy sprawdzania poprawności wykryto nieprawidłowe dane. Te dane podsumowania mogą być wyświetlane w postaci tekstu na stronie sieci web lub za pośrednictwem messagebox modalne, po stronie klienta. Pozwól s zwiększenia tego samouczka, aby uwzględnić messagebox po stronie klienta, podsumowanie problemów weryfikacji.

Aby to zrobić, przeciągnij kontrolki podsumowania walidacji z przybornika do projektanta. Lokalizacja t kontrolki podsumowania walidacji naprawdę są ważne, ponieważ możemy ponownie w celu skonfigurowania go, aby tylko wyświetlić podsumowanie jako komunikat messagebox. Po dodaniu kontrolki, ustaw jego [ `ShowSummary` właściwość](https://msdn.microsoft.com/library/system.web.ui.webcontrols.validationsummary.showsummary(VS.80).aspx) do `False` i jego [ `ShowMessageBox` właściwość](https://msdn.microsoft.com/library/system.web.ui.webcontrols.validationsummary.showmessagebox(VS.80).aspx) do `True`. Dodając ten wszelkie błędy sprawdzania poprawności są podsumowane w messagebox po stronie klienta (patrz rysunek 6).


[![Błędy sprawdzania poprawności są podsumowane w Messagebox po stronie klienta](adding-validation-controls-to-the-datalist-s-editing-interface-vb/_static/image17.png)](adding-validation-controls-to-the-datalist-s-editing-interface-vb/_static/image16.png)

**Rysunek 6**: błędy sprawdzania poprawności są podsumowane w Messagebox po stronie klienta ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](adding-validation-controls-to-the-datalist-s-editing-interface-vb/_static/image18.png))


## <a name="summary"></a>Podsumowanie

W tym samouczku widzieliśmy jak zmniejszyć prawdopodobieństwo wyjątków za pomocą kontrolek weryfikacji do aktywnego upewnij się, że dane wejściowe naszych użytkowników prawidłowy przed podjęciem próby ich używać w przepływie pracy aktualizacji. Program ASP.NET zapewnia pięć formantów sieci Web sprawdzania poprawności, które są przeznaczone do wglądu namierzenie internetowego kontrolować dane wejściowe s i raportować o ważności s danych wejściowych. W ramach tego samouczka użyliśmy dwa z tych pięć formantów RequiredFieldValidator i CompareValidator zapewnienie podano nazwę produktu s i że cena miał format waluty z wartością większą niż lub równą zero.

Dodawanie kontrolek weryfikacji do s DataList edytowanie interfejsu wystarczy przeciągnąć je na `EditItemTemplate` z przybornika i ustawienie niewielki podzbiór właściwości. Domyślnie formanty sprawdzania poprawności automatycznie emitują skrypt weryfikacji po stronie klienta; zapewniają także weryfikacji po stronie serwera na odświeżenie strony, przechowywania zbiorczą wynik w `Page.IsValid` właściwości. Aby pominąć weryfikację po stronie klienta, po kliknięciu przycisku, element LinkButton lub ImageButton, ustaw s `CausesValidation` właściwość `False`. Ponadto, przed przystąpieniem do wykonywania wszystkich zadań z danymi na odświeżenie strony, upewnij się, że `Page.IsValid` właściwość zwraca `True`.

Wszystkie samouczki edycji kontrolki DataList możemy ve badania do tej pory były bardzo proste interfejsy edycji pole tekstowe nazwy produktu s i inny wpis dla ceny. Edytowanie interfejsu może jednak zawierać kombinację różnych formantów sieci Web, takich jak kontrolek DropDownList, kalendarze, przyciski radiowe, pola wyboru i tak dalej. W naszym następnym samouczku Zapoznamy się kompilowanie interfejs, który korzysta z rozmaitych kontrolki sieci Web.

Wszystkiego najlepszego programowania!

## <a name="about-the-author"></a>Informacje o autorze

[Scott Bento](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor siedem ASP/ASP.NET książek i założycielem [4GuysFromRolla.com](http://www.4guysfromrolla.com), pracował nad przy użyciu technologii Microsoft Web od 1998 r. Scott działa jako niezależny Konsultant, trainer i składnika zapisywania. Jego najnowszą książkę Stephena [ *Sams uczyć się ASP.NET 2.0 w ciągu 24 godzin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). ADAM można z Tobą skontaktować w [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) lub za pośrednictwem jego blogu, który znajduje się w temacie [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Specjalne podziękowania dla

W tej serii samouczków został zrecenzowany przez wielu recenzentów pomocne. Wiodące osób dokonujących przeglądu, w tym samouczku zostały Dennis Patterson, Krzysztof Pespisa i Liz Shulok. Zainteresowani zapoznaniem Moje kolejnych artykułów MSDN? Jeśli tak, Porzuć mnie linii w [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Poprzednie](handling-bll-and-dal-level-exceptions-vb.md)
> [dalej](customizing-the-datalist-s-editing-interface-vb.md)
