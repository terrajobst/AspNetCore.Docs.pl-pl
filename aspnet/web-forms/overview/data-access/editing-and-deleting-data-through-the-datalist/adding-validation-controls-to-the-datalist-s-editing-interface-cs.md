---
uid: web-forms/overview/data-access/editing-and-deleting-data-through-the-datalist/adding-validation-controls-to-the-datalist-s-editing-interface-cs
title: Dodawanie formantów weryfikacji do elementu DataList do edycji interfejsu (C#) | Dokumentacja firmy Microsoft
author: rick-anderson
description: W tym samouczku zajmiemy się tym, jak łatwo jest Dodaj formanty walidacji do EditItemTemplate DataList zapewnić bardziej niezawodne edycji int. użytkownika...
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/30/2006
ms.topic: article
ms.assetid: 3ecc21c5-da0e-40ab-abb4-fac1e47398ad
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/editing-and-deleting-data-through-the-datalist/adding-validation-controls-to-the-datalist-s-editing-interface-cs
msc.type: authoredcontent
ms.openlocfilehash: 13582989aed60ec7949afcd683472546a91d171c
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
ms.locfileid: "30889127"
---
<a name="adding-validation-controls-to-the-datalists-editing-interface-c"></a>Dodawanie formantów weryfikacji DataList edycji interfejsu (C#)
====================
przez [Bento Scott](https://twitter.com/ScottOnWriting)

[Pobierz przykładową aplikację](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_39_CS.exe) lub [pobierania plików PDF](adding-validation-controls-to-the-datalist-s-editing-interface-cs/_static/datatutorial39cs1.pdf)

> W tym samouczku będziemy zostanie wyświetlone, jak łatwo jest Dodaj formanty walidacji do EditItemTemplate DataList zapewnić bardziej niezawodne edycji interfejsu użytkownika.


## <a name="introduction"></a>Wprowadzenie

W DataList dotychczasowych edycji samouczki DataLists edycji interfejsów nie zostały uwzględnione sprawdzania poprawności danych wejściowych żadnych aktywnego użytkownika, nawet, jeśli takie jak brak nazwy produktu lub wyników ceny wyjątek w danych wejściowych nieprawidłowego użytkownika. W [poprzedniego samouczek](handling-bll-and-dal-level-exceptions-cs.md) możemy zbadać, jak dodać kod, aby DataList s obsługi wyjątków `UpdateCommand` obsługi zdarzeń, aby przechwycić i bezpiecznie wyświetlać informacje na temat wszelkie wyjątki, które zostały zgłoszone. Najlepiej, jeśli jednak interfejs edytowania obejmuje formanty walidacji, aby uniemożliwić użytkownikowi wprowadzanie takich nieprawidłowe dane w pierwszej kolejności.

W tym samouczku zajmiemy się tym, jak łatwo jest Dodaj formanty walidacji do DataList s `EditItemTemplate` zapewnić bardziej niezawodne edycji interfejsu użytkownika. W szczególności w tym samouczku oparta na przykładzie utworzonej w poprzednim samouczku i wspomaga edycji interfejsu, który ma zawierać odpowiednie sprawdzania poprawności.

## <a name="step-1-replicating-the-example-fromhandling-bll--and-dal-level-exceptionshandling-bll-and-dal-level-exceptions-csmd"></a>Krok 1: Replikowanie w przykładzie z[obsługi wyjątków na poziomie logiki warstwy Biznesowej i warstwy DAL](handling-bll-and-dal-level-exceptions-cs.md)

W [obsługi logiki warstwy Biznesowej i wyjątków na poziomie warstwy DAL](handling-bll-and-dal-level-exceptions-cs.md) samouczek utworzyliśmy stronę wymienione nazwy i cen produktów w dwie kolumny, można edytować elementu DataList. Naszym celem w tym samouczku jest rozszerzyć interfejsu edycji s DataList, należą formanty walidacji. W szczególności zostanie logiki sprawdzania poprawności:

- Wymagaj, aby podać nazwę produktu s
- Sprawdź, czy wartość wprowadzona w cenie formacie waluty prawidłowe
- Upewnij się, że wartość ceny jest większa lub równa zero, ponieważ ujemny `UnitPrice` wartości jest niedozwolone

Zanim można przyjrzymy rozbudować w poprzednim przykładzie, aby uwzględnić sprawdzania poprawności, najpierw musimy replikacji w przykładzie z `ErrorHandling.aspx` strony `EditDeleteDataList` folder ze stroną, w tym samouczku `UIValidation.aspx`. Potrzebujemy kopiować zarówno w tym `ErrorHandling.aspx` znaczników deklaratywne s i jego kod źródłowy. Najpierw skopiować na deklaratywne znaczników, wykonując następujące czynności:

1. Otwórz `ErrorHandling.aspx` strony w programie Visual Studio
2. Przejdź do strony s znaczników deklaratywne (kliknij przycisk źródła w dolnej części strony)
3. Skopiuj tekst wewnątrz `<asp:Content>` i `</asp:Content>` tagi (linie 3 – 32), jak pokazano na rysunku 1.


[![Skopiuj tekst w &lt;asp: Content&gt; formantu](adding-validation-controls-to-the-datalist-s-editing-interface-cs/_static/image2.png)](adding-validation-controls-to-the-datalist-s-editing-interface-cs/_static/image1.png)

**Rysunek 1**: skopiować tekst w `<asp:Content>` formantu ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](adding-validation-controls-to-the-datalist-s-editing-interface-cs/_static/image3.png))


1. Otwórz `UIValidation.aspx` strony
2. Przejdź do znaczników deklaratywne s
3. Wklej tekst wewnątrz `<asp:Content>` formantu.

Aby skopiować kod źródłowy, otwórz `ErrorHandling.aspx.vb` zaznacz i skopiuj tylko tekst *w* `EditDeleteDataList_ErrorHandling` klasy. Skopiuj obsługi zdarzeń trzy (`Products_EditCommand`, `Products_CancelCommand`, i `Products_UpdateCommand`) wraz z programem `DisplayExceptionDetails` metody, ale czy **nie** skopiuj deklaracji klasy lub `using` instrukcje. Wklej skopiowany tekst *w* `EditDeleteDataList_UIValidation` klasy w `UIValidation.aspx.vb`.

Po przeniesieniu za pośrednictwem zawartość i kod z `ErrorHandling.aspx` do `UIValidation.aspx`, Poświęć chwilę, aby przetestować stron w przeglądarce. Powinni widzieć tych samych danych wyjściowych i środowisko funkcji w każdym z tych dwóch stron (patrz rysunek 2).


[![Na stronie UIValidation.aspx naśladuje funkcji w ErrorHandling.aspx](adding-validation-controls-to-the-datalist-s-editing-interface-cs/_static/image5.png)](adding-validation-controls-to-the-datalist-s-editing-interface-cs/_static/image4.png)

**Rysunek 2**: `UIValidation.aspx` strony naśladuje funkcji w `ErrorHandling.aspx` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](adding-validation-controls-to-the-datalist-s-editing-interface-cs/_static/image6.png))


## <a name="step-2-adding-the-validation-controls-to-the-datalist-s-edititemtemplate"></a>Krok 2: Dodawanie formantów weryfikacji do elementu DataList EditItemTemplate s

Podczas tworzenia formularzy wprowadzania danych, ważne jest wprowadzania wszystkich wymaganych pól i że wszystkie ich podane dane wejściowe są wartości prawnych, niepoprawnie sformatowany. Aby upewnić się, że s danych wprowadzonych przez użytkownika są prawidłowe, program ASP.NET zapewnia pięć wbudowanych sprawdzania kontroluje, opracowane w celu sprawdzenia wartości pojedynczego kontrolki wprowadzania w formie sieci Web:

- [RequiredFieldValidator](https://msdn.microsoft.com/library/5hbw267h(VS.80).aspx) gwarantuje, że wartość została podana
- [CompareValidator](https://msdn.microsoft.com/library/db330ayw(VS.80).aspx) sprawdza poprawność wartości z inną wartością formantu sieci Web lub wartość stałą lub zapewnia, że format wartości s jest niedozwolona dla określonego typu danych
- [RangeValidator](https://msdn.microsoft.com/library/f70d09xt.aspx) gwarantuje, że wartość znajduje się w zakresie wartości
- [RegularExpressionValidator](https://msdn.microsoft.com/library/eahwtc9e.aspx) weryfikuje wartość względem [wyrażeń regularnych](http://en.wikipedia.org/wiki/Regular_expression)
- [CustomValidator](https://msdn.microsoft.com/library/9eee01cx(VS.80).aspx) weryfikuje wartość z niestandardowych, definiowanych przez użytkownika — metoda

Więcej informacji o tych kontrolek pięć odwołują się do [dodawanie formantów weryfikacji do edycji i wstawianie interfejsy](../editing-inserting-and-deleting-data/adding-validation-controls-to-the-editing-and-inserting-interfaces-cs.md) samouczek lub wyewidencjonowanie [sekcji Formanty walidacji](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/ctrlref/validation/default.aspx) z [Samouczków szybkiego startu ASP.NET](https://quickstarts.asp.net).

W naszym samouczku potrzebujemy RequiredFieldValidator, aby upewnić się, że zostały podane wartości dla nazwy produktu i CompareValidator do Sprawdź, czy wprowadzona cena ma wartość większą niż lub równa 0 i jest podana w formacie waluty prawidłowe.

> [!NOTE]
> Podczas ASP.NET 1.x miały te takich samych kontroli pięć sprawdzania poprawności, ASP.NET 2.0 dodał szereg ulepszeń, głównym dwa skryptu po stronie klienta jest obsługa przeglądarek oprócz programu Internet Explorer i możliwość formantów sprawdzania poprawności partycji na stronie do Sprawdzanie poprawności grupy. Aby uzyskać więcej informacji na temat nowych funkcji kontroli weryfikacji w 2.0 dotyczą [pincety formanty walidacji w programie ASP.NET 2.0](http://aspnet.4guysfromrolla.com/articles/112305-1.aspx).


Let s, najpierw dodaj formanty walidacji niezbędne do DataList s `EditItemTemplate`. To zadanie można wykonać za pomocą projektanta, klikając łącze Edytuj szablony z tagów inteligentnych s DataList lub za pomocą składni deklaratywnej. Let s kroków procesu przy użyciu opcji Edytuj szablony w widoku Projekt. Po wybraniu do edycji DataList s `EditItemTemplate`, Dodaj RequiredFieldValidator przeciągając je z przybornika w interfejsie edycji szablonu umieszczenia go po `ProductName` pola tekstowego.


[![Dodaj RequiredFieldValidator do EditItemTemplate po pole tekstowe ProductName](adding-validation-controls-to-the-datalist-s-editing-interface-cs/_static/image8.png)](adding-validation-controls-to-the-datalist-s-editing-interface-cs/_static/image7.png)

**Rysunek 3**: Dodaj RequiredFieldValidator do `EditItemTemplate After` `ProductName` pola tekstowego ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](adding-validation-controls-to-the-datalist-s-editing-interface-cs/_static/image9.png))


Wszystkie formanty walidacji pracę sprawdzanie poprawności danych wejściowych jeden formant sieci Web ASP.NET. W związku z tym należy wskazać, że RequiredFieldValidator właśnie dodaliśmy powinien sprawdzania poprawności `ProductName` pole tekstowe; odbywa się przez ustawienie weryfikacji s [ `ControlToValidate` właściwości](https://msdn.microsoft.com/library/system.web.ui.webcontrols.basevalidator.controltovalidate(VS.80).aspx) do `ID` z odpowiednie kontrolka sieci Web (`ProductName`, w tym wystąpieniu). Następnie należy ustawić [ `ErrorMessage` właściwości](https://msdn.microsoft.com/library/system.web.ui.webcontrols.basevalidator.errormessage(VS.80).aspx) do należy podać nazwę produktu s i [ `Text` właściwości](https://msdn.microsoft.com/library/system.web.ui.webcontrols.basevalidator.text(VS.80).aspx) do \*. `Text` Wartość właściwości, jeśli zostanie podana, jest tekst, który jest wyświetlany przez formant weryfikacji w przypadku niepowodzenia weryfikacji. `ErrorMessage` Wartość właściwości, która jest wymagana, jest używany przez formant ValidationSummary; Jeśli `Text` pominięcia będzie używana wartość właściwości `ErrorMessage` wartość właściwości jest wyświetlany przez formant sprawdzania poprawności na nieprawidłowe dane wejściowe.

Ustawienie tych trzech właściwości RequiredFieldValidator, po ekranie powinien wyglądać podobnie do rysunek 4.


[![Ustawianie RequiredFieldValidator s ControlToValidate, komunikat o błędzie i właściwości tekstu](adding-validation-controls-to-the-datalist-s-editing-interface-cs/_static/image11.png)](adding-validation-controls-to-the-datalist-s-editing-interface-cs/_static/image10.png)

**Rysunek 4**: Ustaw RequiredFieldValidator s `ControlToValidate`, `ErrorMessage`, i `Text` właściwości ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](adding-validation-controls-to-the-datalist-s-editing-interface-cs/_static/image12.png))


Z RequiredFieldValidator dodane do `EditItemTemplate`, wszystkie, czy pozostaje jest dodanie niezbędne weryfikacji w cenie produktu s pola tekstowego. Ponieważ `UnitPrice` jest opcjonalny podczas edytowania rekordu, możemy ADAM trzeba dodać RequiredFieldValidator t. Jednak należy dodać CompareValidator, aby upewnić się, że `UnitPrice`, jeśli podany, jest poprawnie sformatowana jako walutę i jest większa lub równa 0.

Dodaj CompareValidator do `EditItemTemplate` i ustaw jej `ControlToValidate` właściwości `UnitPrice`, jego `ErrorMessage` właściwości ceny musi być większa lub równa zero i nie może zawierać symbol waluty i jego `Text` właściwości \*. Aby wskazać, że `UnitPrice` wartość musi być większa lub równa 0, ustaw CompareValidator s [ `Operator` właściwości](https://msdn.microsoft.com/library/system.web.ui.webcontrols.comparevalidator.operator(VS.80).aspx) do `GreaterThanEqual`, jego [ `ValueToCompare` właściwości](https://msdn.microsoft.com/library/system.web.ui.webcontrols.comparevalidator.valuetocompare(VS.80).aspx) 0 i jego [ `Type` właściwości](https://msdn.microsoft.com/library/system.web.ui.webcontrols.basecomparevalidator.type.aspx) do `Currency`.

Po dodaniu tych dwóch weryfikacji zabezpieczeń DataList s `EditItemTemplate` składni deklaratywnej s powinien wyglądać podobnie do następującego:


[!code-aspx[Main](adding-validation-controls-to-the-datalist-s-editing-interface-cs/samples/sample1.aspx)]

Po wprowadzeniu tych zmian, otwórz stronę w przeglądarce. Przy próbie pominąć nazwę lub wprowadź wartość Nieprawidłowa cena podczas edytowania produktu gwiazdka pojawia się obok pola tekstowego. Jak pokazano na rysunku nr 5, cenę, która zawiera symbol waluty, takich jak $ cenie od 19,95 jest uznawane za nieprawidłowe. CompareValidator s `Currency` `Type` umożliwia separatory cyfr (na przykład przecinków lub okresów, w zależności od ustawienia kultury) i wiodące plus lub minus, ale *nie* zezwolenie na symbol waluty. To zachowanie może perplex użytkowników, jak interfejs edytowania obecnie renderuje `UnitPrice` w formacie waluty.


[![Gwiazdka pojawia się obok pola tekstowe z nieprawidłowe dane wejściowe](adding-validation-controls-to-the-datalist-s-editing-interface-cs/_static/image14.png)](adding-validation-controls-to-the-datalist-s-editing-interface-cs/_static/image13.png)

**Rysunek 5**: gwiazdki pojawia się dalej, aby pola tekstowe z nieprawidłowe dane wejściowe ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](adding-validation-controls-to-the-datalist-s-editing-interface-cs/_static/image15.png))


Podczas sprawdzania poprawności działa jako — jest, użytkownik będzie musiał ręcznie usunąć symbol waluty podczas edytowania rekordu, który nie jest akceptowane. Ponadto są nieprawidłowe dane wejściowe w edycji interfejs ani aktualizacja ani Anuluj przyciski, po kliknięciu, wywoła odświeżania strony. W idealnym przypadku przycisku Anuluj do stanu wstępnie edycji, niezależnie od ważności danych wejściowych użytkownika s zwróci elementu DataList. Należy również upewnić się, że dane strony są prawidłowe przed aktualizacją produktu informacje w DataList s `UpdateCommand` program obsługi zdarzeń, jako formanty walidacji logiki po stronie klienta można pominąć, przez użytkowników, których przeglądarki, ADAM nie obsługuje języka JavaScript albo mieć jego obsługa jest wyłączona.

## <a name="removing-the-currency-symbol-from-the-edititemtemplate-s-unitprice-textbox"></a>Usuwanie symbolu waluty w polu tekstowym UnitPrice s EditItemTemplate

Korzystając z CompareValidator s `Currency``Type`, sprawdzania poprawności danych wejściowych nie może zawierać żadnych symboli waluty. Obecność takie symbole powoduje, że CompareValidator można oznaczyć jako niepoprawne dane wejściowe. Jednak naszych interfejs edytowania obecnie zawiera symbol waluty w `UnitPrice` pole tekstowe, co oznacza, użytkownik musi jawnie usunąć symbol waluty przed zapisaniem zmian. Aby rozwiązać ten problem, będziemy mieć trzy opcje:

1. Skonfiguruj `EditItemTemplate` , aby `UnitPrice` wartości tekstowe nie jest sformatowany jako walutę.
2. Zezwalaj użytkownikowi na wprowadzenie symbolu waluty przez usunięcie CompareValidator i zastępowanie RegularExpressionValidator, która sprawdza, czy wartość waluty prawidłowo sformatowane. Żądania, w tym miejscu jest wyrażenie regularne do sprawdzania poprawności wartości waluty nie jest równie proste jak CompareValidator i będzie wymagać pisanie kodu, jeśli trzeba zastosować ustawienia kultury.
3. Całkowicie usunąć formant sprawdzania poprawności i zależne od logiki niestandardowej weryfikacji po stronie serwera w widoku GridView s `RowUpdating` program obsługi zdarzeń.

Let s przejść z opcją 1 na potrzeby tego samouczka. Obecnie `UnitPrice` jest formatowana jako wartość walutową z powodu wyrażenia wiązania danych dla pola tekstowego w `EditItemTemplate`: `<%# Eval("UnitPrice", "{0:c}") %>`. Zmień `Eval` instrukcji `Eval("UnitPrice", "{0:n2}")`, które formatuje wynik jako liczba z dwóch cyfr precyzji. Można to zrobić bezpośrednio za pomocą składni deklaratywnej lub klikając łącze Edytuj powiązania DataBindings z `UnitPrice` TextBox w DataList s `EditItemTemplate`.

Dzięki tej zmianie sformatowany cen w interfejsie edycji obejmuje przecinkami jako separator grupy i okres jako separator dziesiętny, ale pozostawia poza symbol waluty.

> [!NOTE]
> Podczas usuwania formacie waluty w interfejsie można edytować, I celowe umieścić symbol waluty jako tekst poza pole tekstowe. Stanowi wskazówkę dla użytkownika, który niepotrzebne Podaj symbol waluty.


## <a name="fixing-the-cancel-button"></a>Ustalania przycisku Anuluj

Domyślnie formantów sieci Web weryfikacji Emituj obsługę języka JavaScript do sprawdzania poprawności po stronie klienta. Po kliknięciu przycisku, LinkButton lub ImageButton formanty walidacji na stronie są sprawdzane po stronie klienta, zanim nastąpi ogłaszania zwrotnego. W przypadku wszelkich nieprawidłowych danych, ogłaszania zwrotnego zostało anulowane. W przypadku niektórych przycisków jednak ważności danych może być nieistotne; w takim przypadku niedogodności jest posiadanie ogłaszania zwrotnego anulowane z powodu nieprawidłowych danych.

Takie przykładem jest przycisk Anuluj. Załóżmy, że użytkownik wprowadzi nieprawidłowe dane, takie jak nazwa produktu s, pomijając decyduje o tych t chcesz zapisać produkt po wszystkich i trafienia przycisku Anuluj. Obecnie przycisku Anuluj wyzwala formanty walidacji na stronie, których raport, czy nazwa produktu Brak i zapobiec ogłaszania zwrotnego. Naszych użytkownik będzie musiał wpisać tekst do `ProductName` pole tekstowe tylko do anulujesz procesu edycji.

Na szczęście przycisku, LinkButton i ImageButton mają [ `CausesValidation` właściwości](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.causesvalidation.aspx) który można wskazać, czy kliknięcie przycisku należy zainicjować logikę weryfikacji (domyślnie `True`). Ustaw s przycisku Anuluj `CausesValidation` właściwości `False`.

## <a name="ensuring-the-inputs-are-valid-in-the-updatecommand-event-handler"></a>Zapewnienie dane wejściowe są prawidłowe w obsłudze zdarzeń UpdateCommand

Z powodu emitowane przez formanty walidacji skryptu po stronie klienta, jeśli użytkownik wprowadzi nieprawidłowe dane wejściowe formanty walidacji anulować wszelkie ogłaszania zwrotnego inicjowane przez przycisk, LinkButton, lub ImageButton kontrolki, których `CausesValidation` właściwości są `True` ( ustawienie domyślne). Jednak jeśli użytkownik odwiedzający jest za antiquated przeglądarki lub jednej obsługi języka JavaScript została wyłączona, sprawdzanie poprawności po stronie klienta nie zostanie wykonany.

Wszystkie formanty weryfikacji platformy ASP.NET Powtórz ich logikę weryfikacji natychmiast po ogłaszania zwrotnego i zgłoś ogólną ważności wejść strony s za pośrednictwem [ `Page.IsValid` właściwości](https://msdn.microsoft.com/library/system.web.ui.page.isvalid.aspx). Jednak przepływ strony nie zostanie przerwana lub zatrzymać w dowolnym sposób na podstawie wartości z `Page.IsValid`. Jako deweloperów odpowiada naszych upewnij się, że `Page.IsValid` właściwość ma wartość `True` przed kontynuowaniem kodu, który przyjmuje prawidłowe dane wejściowe.

Jeśli użytkownik ma wyłączone JavaScript, odwiedza naszą stronę, edytuje produktu, wprowadza wartość ceny za kosztowne i kliknięciu przycisku Aktualizuj weryfikacji po stronie klienta będzie pomijana i nastąpi odświeżenie strony. Strony, strony ASP.NET `UpdateCommand` wykonuje program obsługi zdarzeń i wystąpił wyjątek podczas próby przeprowadzenia analizy zbyt drogie w `Decimal`. Ponieważ mamy obsługi wyjątków, bezpiecznie obsługi takiego wyjątku, ale firma Microsoft może uniemożliwić nieprawidłowe dane poślizgiem za pośrednictwem w pierwszej kolejności przez tylko kontynuowanie `UpdateCommand` obsługi zdarzeń Jeśli `Page.IsValid` ma wartość `True`.

Dodaj następujący kod do początku `UpdateCommand` program obsługi zdarzeń, bezpośrednio przed `Try` bloku:


[!code-csharp[Main](adding-validation-controls-to-the-datalist-s-editing-interface-cs/samples/sample2.cs)]

Z tym dodatkiem produktu będzie podejmować próby można zaktualizować tylko wtedy, gdy zostało przesłane dane są prawidłowe. Większość użytkowników won t może odświeżania nieprawidłowych danych z powodu skrypty po stronie klienta formanty sprawdzania poprawności, ale użytkownicy, których przeglądarki ADAM nie obsługuje języka JavaScript lub obsługi języka JavaScript, która ma wyłączone, można pominąć sprawdzanie po stronie klienta i przesłać nieprawidłowe dane.

> [!NOTE]
> Czytnik astute będzie odwołania, który podczas aktualizowania danych z widoku GridView, możemy t trzeba jawnie sprawdziła `Page.IsValid` właściwości w klasie związanej z kodem naszej strony s. Jest to spowodowane sprawdza widoku GridView `Page.IsValid` właściwość nam i tylko będzie kontynuowane przy użyciu aktualizacji tylko wtedy, gdy zwraca wartość `True`.


## <a name="step-3-summarizing-data-entry-problems"></a>Krok 3: Podsumowanie problemów zapis danych

Oprócz formantów pięć weryfikacji platformy ASP.NET zawiera [formant ValidationSummary](https://msdn.microsoft.com/library/f9h59855(VS.80).aspx), który wyświetla `ErrorMessage` s tych kontrolek sprawdzania poprawności, które wykryto nieprawidłowe dane. To podsumowanie danych mogą być wyświetlane jako tekstu na stronie sieci web lub za pośrednictwem messagebox modalne, po stronie klienta. Let s zwiększenia tego samouczka do dołączenia po stronie klienta element messagebox podsumowania sprawdzania poprawności problemów.

Aby to zrobić, przeciągnij formant ValidationSummary z przybornika do projektanta. Lokalizacja t formant ValidationSummary naprawdę znaczenia, ponieważ firma Microsoft re w celu skonfigurowania go, aby wyświetlić podsumowanie tylko jako element messagebox. Po dodaniu formantu, ustaw jej [ `ShowSummary` właściwości](https://msdn.microsoft.com/library/system.web.ui.webcontrols.validationsummary.showsummary(VS.80).aspx) do `False` i jego [ `ShowMessageBox` właściwości](https://msdn.microsoft.com/library/system.web.ui.webcontrols.validationsummary.showmessagebox(VS.80).aspx) do `True`. Z tym dodatkiem jakieś błędy sprawdzania poprawności zostały podsumowane w messagebox po stronie klienta (patrz rysunek 6).


[![Błędy sprawdzania poprawności zostały podsumowane w Messagebox po stronie klienta](adding-validation-controls-to-the-datalist-s-editing-interface-cs/_static/image17.png)](adding-validation-controls-to-the-datalist-s-editing-interface-cs/_static/image16.png)

**Rysunek 6**: błędy sprawdzania poprawności zostały podsumowane w Messagebox po stronie klienta ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](adding-validation-controls-to-the-datalist-s-editing-interface-cs/_static/image18.png))


## <a name="summary"></a>Podsumowanie

W tym samouczku widzieliśmy sposób zmniejszyć prawdopodobieństwo wyjątków za pomocą formantów weryfikacji do aktywnego upewnij się, że naszych użytkowników dane wejściowe są prawidłowe, przed podjęciem próby użycia ich w aktualizacji przepływu pracy. Program ASP.NET udostępnia pięć weryfikacji formantów sieci Web przeznaczonych do zbadania określonej sieci Web kontroli danych wejściowych s i raportować o ważności wejściowych s. W tym samouczku użyliśmy dwa z tych kontrolek pięć RequiredFieldValidator i CompareValidator do upewnij się, że podano nazwę produktu s i że ceny miał formacie waluty o wartości większe niż lub równa zero.

Dodawanie formantów weryfikacji s DataList edycji interfejsu jest tak proste, jak przeciągając je na `EditItemTemplate` z przybornika i ustawiania niewielki podzbiór właściwości. Domyślnie przez formanty walidacji automatycznie Emituj skrypt weryfikacji po stronie klienta; zapewniają także weryfikacji po stronie serwera na odświeżenie strony, przechowywanie zbiorczą wynik w `Page.IsValid` właściwości. Aby pominąć weryfikacji po stronie klienta, gdy zostanie kliknięty przycisk, LinkButton lub ImageButton, Ustaw przycisk s `CausesValidation` właściwości `False`. Ponadto przed wykonaniem zadań z danych przesyłanych na ogłaszania zwrotnego, upewnij się, że `Page.IsValid` zwraca właściwość `True`.

Wszystkie DataList edycji samouczki możemy stawienia zbadać wykonanej do tej pory miały bardzo proste interfejsy edycji pole tekstowe nazwy produktu s, a druga w cenie. Interfejs edycji, może jednak zawierać kombinację inne formanty sieci Web, takich jak DropDownLists, kalendarzy, RadioButton, pola wyboru i tak dalej. W naszym samouczku dalej przyjrzymy tworzenia interfejs, który używa różnorodnych narzędzi formantów sieci Web.

Programowanie przyjemność!

## <a name="about-the-author"></a>Informacje o autorze

[Scott Bento](http://www.4guysfromrolla.com/ScottMitchell.shtml), autora siedmiu książek ASP/ASP.NET i twórcę z [4GuysFromRolla.com](http://www.4guysfromrolla.com), pracuje z technologii Microsoft Web od 1998. Scott działa jako niezależnego konsultanta trainer i składnika zapisywania. Jest jego najnowszej książki [ *Sams nauczyć się ASP.NET 2.0 w ciągu 24 godzin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Piotr można uzyskać pod adresem [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) lub za pośrednictwem jego blog, który znajduje się w temacie [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Specjalne podziękowania dla

Ten samouczek serii zostało sprawdzone przez wiele recenzentów przydatne. Prowadzić osób dokonujących przeglądu, w tym samouczku zostały firmy Dennis Patterson, Krzysztof Pespisa i Liz Shulok. Zainteresowani recenzowania Moje nadchodzących artykuły MSDN? Jeśli tak, Porzuć mnie linii w [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Poprzednie](handling-bll-and-dal-level-exceptions-cs.md)
> [dalej](customizing-the-datalist-s-editing-interface-cs.md)
