---
uid: web-forms/overview/data-access/custom-formatting/using-templatefields-in-the-gridview-control-vb
title: Używanie kontrolek TemplateField w kontrolce GridView (VB) | Dokumentacja firmy Microsoft
author: rick-anderson
description: Aby zapewnić elastyczność, widoku GridView oferuje TemplateField, która renderuje przy użyciu szablonu. Szablon może zawierać kombinację statyczny kod HTML, kontrolki sieci Web i...
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/31/2010
ms.topic: article
ms.assetid: a92cd6ed-609a-4e40-ad23-004b54afd436
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/data-access/custom-formatting/using-templatefields-in-the-gridview-control-vb
msc.type: authoredcontent
ms.openlocfilehash: 845b5b07849420f03e60a9eb243f07641253540f
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37396511"
---
<a name="using-templatefields-in-the-gridview-control-vb"></a>Używanie kontrolek TemplateField w kontrolce GridView (VB)
====================
przez [Bento Scott](https://twitter.com/ScottOnWriting)

[Pobierz przykładową aplikację](http://download.microsoft.com/download/5/7/0/57084608-dfb3-4781-991c-407d086e2adc/ASPNET_Data_Tutorial_12_VB.exe) lub [Pobierz plik PDF](using-templatefields-in-the-gridview-control-vb/_static/datatutorial12vb1.pdf)

> Aby zapewnić elastyczność, widoku GridView oferuje TemplateField, która renderuje przy użyciu szablonu. Szablon może zawierać różne statyczny HTML, internetowej kontrolki i składnia wiązania danych. W tym samouczku zajmiemy się, jak używać TemplateField do osiągnięcia większego stopnia dostosowania za pomocą kontrolki GridView.


## <a name="introduction"></a>Wprowadzenie

Kontrolki GridView składa się z zestawu pól, które wskazują, jakie właściwości z `DataSource` mają być uwzględniane w wyniku renderowania oraz sposób wyświetlania danych. Najprostszy typ pola jest elementu BoundField, która wyświetla wartości danych w postaci tekstu. Inne typy pól wyświetlania danych przy użyciu alternatywnych elementów HTML. CheckBoxField, na przykład renderowany jako pole, którego zaznaczony stan zależy od wartości pola określone dane; ImageField renderuje obraz, którego źródłem obrazu opiera się na pola określone dane. Hiperłącza i przyciski, którego stan jest zależna od podstawowej wartości pola danych może być renderowany przy użyciu typów pól pole hiperłącza HyperLinkField i ButtonField.

Chociaż typy pól CheckBoxField, ImageField, pole hiperłącza HyperLinkField i ButtonField umożliwić alternatywny widok danych, nadal są dość ograniczony w odniesieniu do formatowania. CheckBoxField mogą być wyświetlane tylko jedno pole wyboru, natomiast ImageField mogą być wyświetlane tylko jeden obraz. Co zrobić, jeśli określone pole wymaganych, aby wyświetlić tekst pola wyboru, *i* obrazu, wszystkie na podstawie wartości pól z różnymi danymi? Lub co zrobić, jeśli chcemy wyświetlić dane za pomocą formantu sieci Web innych niż pola wyboru, obraz, hiperłącze lub przycisk? Ponadto elementu BoundField ogranicza jego wyświetlany obraz do potrzeb jedno pole danych. Co zrobić, jeśli chcemy Pokaż co najmniej dwóch wartości pól danych w jednej kolumnie GridView?

Aby uwzględnić ten poziom elastyczności widoku GridView oferuje TemplateField, która renderuje przy użyciu *szablonu*. Szablon może zawierać różne statyczny HTML, internetowej kontrolki i składnia wiązania danych. Ponadto TemplateField ma różne szablony, które mogą służyć do dostosowywania renderowania w różnych sytuacjach. Na przykład `ItemTemplate` używany domyślnie do renderowania komórki dla każdego wiersza, ale `EditItemTemplate` szablon może być używany do dostosowania interfejsu podczas edytowania danych.

W tym samouczku zajmiemy się, jak używać TemplateField do osiągnięcia większego stopnia dostosowania za pomocą kontrolki GridView. W [poprzedni Samouczek](custom-formatting-based-upon-data-vb.md) widzieliśmy, jak dostosować formatowanie oparte na podstawowej dane za pomocą `DataBound` i `RowDataBound` procedury obsługi zdarzeń. Innym sposobem dostosowania formatowanie na podstawie danych podstawowych jest przez wywołanie metody formatowania z w ramach szablonu. Ta technika omówimy w tym samouczku, jak również.

W tym samouczku użyjemy kontrolek TemplateField dostosować wygląd listy pracowników. W szczególności firma Microsoft będzie wyświetlić listę wszystkich pracowników, ale będą wyświetlane pracownika imiona i nazwiska w jednej kolumnie, ich data zatrudnienia formant kalendarza i kolumnę Stan, który wskazuje, ile dni one został zatrudniony w firmie.


[![Trzy kontrolek TemplateField są używane do dostosowywania wyświetlania](using-templatefields-in-the-gridview-control-vb/_static/image2.png)](using-templatefields-in-the-gridview-control-vb/_static/image1.png)

**Rysunek 1**: trzech kontrolek TemplateField są używane do dostosowywania wyświetlania ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](using-templatefields-in-the-gridview-control-vb/_static/image3.png))


## <a name="step-1-binding-the-data-to-the-gridview"></a>Krok 1: Wiązanie danych do kontrolki GridView

W sytuacji, gdzie należy użyć kontrolek TemplateField dostosować wygląd, I Najprostszym rozwiązaniem Rozpocznij od utworzenia kontrolki widoku siatki, zawierający tylko BoundFields najpierw a następnie dodać nowych kontrolek TemplateField lub przekonwertować istniejącej BoundFields do Kontrolek TemplateField zgodnie z potrzebami. W związku z tym Zacznijmy w tym samouczku, dodając GridView na strony za pomocą projektanta i powiązywanie kontrolki ObjectDataSource, które zwraca listę wszystkich pracowników. Te kroki utworzy GridView z BoundFields dla każdego pola pracowników.

Otwórz `GridViewTemplateField.aspx` strony, a następnie przeciągnij GridView z przybornika do projektanta. Z tagu inteligentnego GridView wybierz można dodać nowego formantu ObjectDataSource, który wywołuje `EmployeesBLL` klasy `GetEmployees()` metody.


[![Dodawanie nowego formantu ObjectDataSource, który wywołuje metodę GetEmployees()](using-templatefields-in-the-gridview-control-vb/_static/image5.png)](using-templatefields-in-the-gridview-control-vb/_static/image4.png)

**Rysunek 2**: Dodawanie nowego formantu ObjectDataSource tego Invokes `GetEmployees()` — metoda ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](using-templatefields-in-the-gridview-control-vb/_static/image6.png))


Powiązanie widoku GridView w ten sposób spowoduje automatyczne dodanie elementu BoundField dla każdej właściwości pracownika: `EmployeeID`, `LastName`, `FirstName`, `Title`, `HireDate`, `ReportsTo`, i `Country`. Dla tego raportu możemy nie odblokowane z wyświetlaniem `EmployeeID`, `ReportsTo`, lub `Country` właściwości. Aby usunąć te BoundFields możesz wykonywać następujące czynności:

- Użyj kliknij okno dialogowe pola Link Edytowanie kolumn z tagu inteligentnego prvku GridView Aby wyświetlić to okno dialogowe. Wybierz BoundFields w lewym dolnym rogu listy i kliknij czerwony znak X przycisk Dalej, aby usunąć elementu BoundField.
- Ręczne edytowanie składni deklaratywnej GridView z widoku źródła, Usuń `<asp:BoundField>` elementu dla elementu BoundField chcesz usunąć.

Po usunięciu `EmployeeID`, `ReportsTo`, i `Country` BoundFields, znaczników w widoku GridView powinien wyglądać następująco:


[!code-aspx[Main](using-templatefields-in-the-gridview-control-vb/samples/sample1.aspx)]

Poświęć chwilę, aby wyświetlić postępach w przeglądarce. W tym momencie powinien zostać wyświetlony tabeli przy użyciu rekordu dla każdego pracownika i czterech kolumn: jeden dla pracownika nazwisko, jeden dla swojego imienia, jeden dla stanowiska i jeden dla ich data zatrudnienia.


[![LastName, FirstName, tytuł i rekrutacji pola są wyświetlane dla każdego pracownika](using-templatefields-in-the-gridview-control-vb/_static/image8.png)](using-templatefields-in-the-gridview-control-vb/_static/image7.png)

**Rysunek 3**: `LastName`, `FirstName`, `Title`, i `HireDate` pola są wyświetlane dla każdego pracownika ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](using-templatefields-in-the-gridview-control-vb/_static/image9.png))


## <a name="step-2-displaying-the-first-and-last-names-in-a-single-column"></a>Krok 2: Wyświetlanie imiona i nazwiska w jednej kolumnie

Aktualnie każdemu pracownikowi imienia i nazwiska są wyświetlane w oddzielnych kolumnach. Może to być dobre rozwiązanie połączyć je w jedną kolumnę. W tym celu należy użyć TemplateField. Firma Microsoft może albo dodawać nowe TemplateField, Dodaj do niego wymagane znaczniki i składnia wiązania danych, a następnie usuń `FirstName` i `LastName` BoundFields lub firma Microsoft może przekonwertować `FirstName` elementu BoundField do TemplateField, Edytuj TemplateField do uwzględnienia `LastName` wartości, a następnie usuń `LastName` elementu BoundField.

Oba podejścia net ten sam wynik, ale osobiście podoba mi się konwertowanie BoundFields kontrolek TemplateField, gdy jest to możliwe, ponieważ konwersja automatycznie dodaje `ItemTemplate` i `EditItemTemplate` za pomocą kontrolki sieci Web i składnia wiązania danych do naśladowania wyglądu i funkcje elementu BoundField. Korzyścią jest to, że firma Microsoft będzie trzeba przeprowadzić mniej pracy TemplateField jako proces konwersji będzie wykonano część obciążenia pracą dla nas.

Aby przekonwertować istniejącego elementu BoundField TemplateField, kliknij link Edytuj kolumny z GridView tagu inteligentnego wyświetlania okna dialogowego pól. Wybieranie elementu BoundField konwersji z listy w lewym dolnym rogu, a następnie kliknij link "Convert to pole do TemplateField" w prawym dolnym rogu.


[![Konwertowanie elementu BoundField TemplateField z okna dialogowego pola](using-templatefields-in-the-gridview-control-vb/_static/image11.png)](using-templatefields-in-the-gridview-control-vb/_static/image10.png)

**Rysunek 4**: Konwertowanie elementu BoundField do TemplateField z okna dialogowego pola ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](using-templatefields-in-the-gridview-control-vb/_static/image12.png))


Przejdź dalej i przekonwertować `FirstName` elementu BoundField do TemplateField. Po tej zmianie nie ma perceptive różnic w projektancie. Jest to spowodowane Konwertowanie elementu BoundField na TemplateField tworzy TemplateField, która utrzymuje wyglądu i działania elementu BoundField. Pomimo nie jest visual różnicy w tym momencie w projektancie, ten proces konwersji została zastąpiona składni deklaratywnej elementu BoundField - `<asp:BoundField DataField="FirstName" HeaderText="FirstName" SortExpression="FirstName" />` — przy użyciu następującej składni TemplateField:


[!code-aspx[Main](using-templatefields-in-the-gridview-control-vb/samples/sample2.aspx)]

Jak widać, TemplateField składa się z dwóch szablonów `ItemTemplate` , ma etykietę o `Text` właściwość jest ustawiona na wartość `FirstName` pola danych i `EditItemTemplate` za pomocą TextBox formant, którego `Text` właściwość jest również ustawiona Aby `FirstName` pola danych. Składnia wiązania danych - `<%# Bind("fieldName") %>` — wskazuje, że pole danych *`fieldName`* jest powiązany z określoną właściwość formantu sieci Web.

Aby dodać `LastName` wartość do tego TemplateField musimy dodać inny formant etykiety w sieci Web w pole danych `ItemTemplate` i powiąż jego `Text` właściwość `LastName`. Można to zrobić ręcznie lub za pomocą projektanta. Aby ręcznie wykonać to zadanie, po prostu Dodaj odpowiedniej składni deklaratywnej do `ItemTemplate`:


[!code-aspx[Main](using-templatefields-in-the-gridview-control-vb/samples/sample3.aspx)]

Aby dodać go za pomocą projektanta, kliknij link Edytuj szablony z GridView tagu inteligentnego. Spowoduje to wyświetlenie interfejsu edycji szablonu kontrolki GridView. W tym tagu inteligentnego interfejsu znajduje się lista szablonów w widoku GridView. Ponieważ mamy jeden TemplateField w tym momencie, tylko szablony wymienione na liście rozwijanej są te szablony dla `FirstName` TemplateField wraz z `EmptyDataTemplate` i `PagerTemplate`. `EmptyDataTemplate` Szablonu, jeśli zostanie określony, używany do renderowania GridView dane wyjściowe, jeśli brak wyników w danych powiązany z GridView; `PagerTemplate`, jeśli określony, używany do renderowania interfejsu stronicowania dla widoku GridView, która obsługuje stronicowanie.


[![Można edytować za pomocą projektanta szablonów GridView](using-templatefields-in-the-gridview-control-vb/_static/image14.png)](using-templatefields-in-the-gridview-control-vb/_static/image13.png)

**Rysunek 5**: GridView szablony można można edytować za pomocą projektanta ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](using-templatefields-in-the-gridview-control-vb/_static/image15.png))


Można również wyświetlić `LastName` w `FirstName` TemplateField przeciągnij formant etykiety z przybornika do `FirstName` firmy TemplateField `ItemTemplate` w widoku GridView przez edycję szablonu interfejsu.


[![Dodaj kontrolkę typu etykieta w sieci Web do właściwości ItemTemplate TemplateField imię](using-templatefields-in-the-gridview-control-vb/_static/image17.png)](using-templatefields-in-the-gridview-control-vb/_static/image16.png)

**Rysunek 6**: Dodawanie formantu sieci Web etykiety, aby `FirstName` ItemTemplate TemplateField firmy ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](using-templatefields-in-the-gridview-control-vb/_static/image18.png))


W tym momencie formant etykiety w sieci Web dodany do TemplateField ma jego `Text` właściwość ustawioną na "Label". Potrzebujemy zmienić to ustawienie, aby ta właściwość jest powiązana wartość `LastName` zamiast tego pola danych. Aby osiągnąć ten kliknij tag inteligentny formant etykiety i wybierz opcję Edytuj powiązania danych.


[![Wybierz opcję Edytuj powiązania danych z tagu etykiet](using-templatefields-in-the-gridview-control-vb/_static/image20.png)](using-templatefields-in-the-gridview-control-vb/_static/image19.png)

**Rysunek 7**: wybierz opcję Edytuj powiązania danych z tagu etykiet ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](using-templatefields-in-the-gridview-control-vb/_static/image21.png))


Zostanie wyświetlone okno dialogowe powiązania danych. W tym miejscu można wybrać właściwość do uczestnictwa w powiązanie danych z listy po lewej stronie i wybierz pole, aby powiązać dane z listy rozwijanej po prawej stronie. Wybierz `Text` właściwości z lewej strony i `LastName` pola z prawej strony, a następnie kliknij przycisk OK.


[![Powiąż właściwość tekst pola Nazwisko danych](using-templatefields-in-the-gridview-control-vb/_static/image23.png)](using-templatefields-in-the-gridview-control-vb/_static/image22.png)

**Rysunek 8**: powiązywanie `Text` właściwości `LastName` pola danych ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](using-templatefields-in-the-gridview-control-vb/_static/image24.png))


> [!NOTE]
> Okno dialogowe powiązania danych można określić, czy ma być przeprowadzane dwukierunkowego wiązania danych. Jeśli pole to nie jest zaznaczone, składnia wiązania danych `<%# Eval("LastName")%>` będą używane zamiast `<%# Bind("LastName")%>`. Każda z tych metod jest dobrym rozwiązaniem dla tego samouczka. Dwukierunkowego wiązania danych staje się ważne, podczas wstawiania i edytowanie danych. Po prostu wyświetlania danych, jednak każda z tych metod będzie działać równie dobrze. Omówimy dwukierunkowego wiązania danych szczegółowo w przyszłości samouczków.


Poświęć chwilę, aby wyświetlić tą stronę za pośrednictwem przeglądarki. Jak widać, widoku GridView wciąż zawiera cztery kolumny; jednak `FirstName` wymienia teraz kolumny *zarówno* `FirstName` i `LastName` wartości pola danych.


[![Imię i nazwisko wartości są wyświetlane w jednej kolumnie](using-templatefields-in-the-gridview-control-vb/_static/image26.png)](using-templatefields-in-the-gridview-control-vb/_static/image25.png)

**Rysunek 9**: zarówno `FirstName` i `LastName` wartości są wyświetlane w jednej kolumnie ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](using-templatefields-in-the-gridview-control-vb/_static/image27.png))


Aby ukończyć ten pierwszy krok, Usuń `LastName` elementu BoundField i Zmień nazwę `FirstName` firmy TemplateField `HeaderText` właściwość "Name". Po wprowadzeniu tych zmian oznaczeniu deklaracyjnym GridView powinien wyglądać następująco:


[!code-aspx[Main](using-templatefields-in-the-gridview-control-vb/samples/sample4.aspx)]


[![Pierwszy i ostatni nazwy każdego pracownika są wyświetlane w jednej kolumnie](using-templatefields-in-the-gridview-control-vb/_static/image29.png)](using-templatefields-in-the-gridview-control-vb/_static/image28.png)

**Na rysunku nr 10**: pierwszy i ostatni nazwy każdego pracownika są wyświetlane w jedną kolumnę ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](using-templatefields-in-the-gridview-control-vb/_static/image30.png))


## <a name="step-3-using-the-calendar-control-to-display-thehireddatefield"></a>Krok 3: Przy użyciu kontrolki kalendarza do wyświetlenia`HiredDate`pola

Wyświetlanie wartości pola danych jako tekst w kontrolce GridView jest tak proste, jak za pomocą elementu BoundField. W niektórych scenariuszach jednak dane jest najlepiej wyrażona za pomocą określonego formantu sieci Web zamiast tylko tekst. Takie Dostosowywanie widoku danych jest możliwe za pomocą kontrolek TemplateField. Na przykład, zamiast niż data zatrudnienia pracownika wyświetlane jako tekst, firma Microsoft można pokazać kalendarza (przy użyciu [kontrolki kalendarza](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar(VS.80).aspx)) przy użyciu ich data zatrudnienia wyróżnione.

W tym celu uruchom za pomocą konwersji `HiredDate` elementu BoundField do TemplateField. Po prostu przejdź do tagu inteligentnego GridView i kliknij link Edytuj kolumny, wyświetlania okna dialogowego pól. Wybierz `HiredDate` elementu BoundField, a następnie kliknij przycisk "Konwertowanie to pole TemplateField."


[![Konwertowanie elementu HiredDate BoundField TemplateField](using-templatefields-in-the-gridview-control-vb/_static/image32.png)](using-templatefields-in-the-gridview-control-vb/_static/image31.png)

**Rysunek 11**: Konwertuj `HiredDate` elementu BoundField do TemplateField ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](using-templatefields-in-the-gridview-control-vb/_static/image33.png))


Jak widzieliśmy w kroku 2, to spowoduje zastąpienie elementu BoundField TemplateField, który zawiera `ItemTemplate` i `EditItemTemplate` etykiety i pole tekstowe którego `Text` właściwości są powiązane z `HiredDate` wartości przy użyciu składni wiązania danych `<%# Bind("HiredDate")%>`.

Aby zamienić tekst przy użyciu kontrolki kalendarza, należy edytować szablon, usuwając etykietę i dodając formant kalendarza. Przy użyciu projektanta, wybierz pozycję Edytuj szablony z GridView tagu inteligentnego, a następnie wybierz `HireDate` firmy TemplateField `ItemTemplate` z listy rozwijanej. Następnie usuń formant etykiety i przeciągnij formant kalendarza z przybornika do interfejsu edycji szablonu.


[![Dodaj formant kalendarza do rekrutacji ItemTemplate TemplateField firmy](using-templatefields-in-the-gridview-control-vb/_static/image35.png)](using-templatefields-in-the-gridview-control-vb/_static/image34.png)

**Rysunek 12**: Dodawanie formantu kalendarza `HireDate` firmy TemplateField `ItemTemplate` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](using-templatefields-in-the-gridview-control-vb/_static/image36.png))


W tym momencie każdego wiersza w widoku GridView będzie zawierać formant kalendarza w jego `HiredDate` TemplateField. Jednak rzeczywista przez pracownika `HiredDate` nie ustawiono wartości dowolne miejsce w kontrolce kalendarza, powodując każdej kontrolki kalendarza, aby domyślnie wyświetla bieżący miesiąc i datę. Aby rozwiązać ten problem, należy przypisać każdemu pracownikowi `HiredDate` do kontrolki kalendarza [SelectedDate](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.selecteddate(VS.80).aspx) i [VisibleDate](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.visibledate(VS.80).aspx) właściwości.

Z tagu inteligentnego kontrolki kalendarza wybierz pozycję Edytuj powiązania danych. Następnie Powiąż zarówno `SelectedDate` i `VisibleDate` właściwości `HiredDate` pola danych.


[![Powiąż właściwości VisibleDate i SelectedDate z polem danych HiredDate](using-templatefields-in-the-gridview-control-vb/_static/image38.png)](using-templatefields-in-the-gridview-control-vb/_static/image37.png)

**Rysunek 13**: powiązywanie `SelectedDate` i `VisibleDate` właściwości `HiredDate` pola danych ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](using-templatefields-in-the-gridview-control-vb/_static/image39.png))


> [!NOTE]
> Wybrana data kontrolki kalendarza nie musi znajdować się widoczny. Na przykład może mieć 1 sierpnia kalendarza<sup>st</sup>, 1999 jako wybranej daty, ale być widoczny w bieżącym miesiącu i roku. Wybranej daty i Data widoczne są określone przez kontrolkę kalendarza `SelectedDate` i `VisibleDate` właściwości. Ponieważ chcemy, aby obie wybierz pracownika `HiredDate` i upewnij się, że jest on wyświetlany należy do obu tych właściwości do powiązania `HireDate` pola danych.


Podczas wyświetlania strony w przeglądarce, kalendarz teraz Wyświetla miesiąc Data zatrudnienia pracownika i wybiera określonym dniu.


[![HiredDate pracownika jest wyświetlany w formancie kalendarza](using-templatefields-in-the-gridview-control-vb/_static/image41.png)](using-templatefields-in-the-gridview-control-vb/_static/image40.png)

**Rysunek 14**: pracownik `HiredDate` jest wyświetlany w formancie kalendarza ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](using-templatefields-in-the-gridview-control-vb/_static/image42.png))


> [!NOTE]
> Sprzecznie wszystkie przykłady widzieliśmy w tym samym zakresie, w tym samouczku zrobiliśmy *nie* ustaw `EnableViewState` właściwość `False` dla tego widoku GridView. Przyczyną tej decyzji jest ponieważ daty kontrolki kalendarza kliknięcie powoduje odświeżenie strony, ustawienie daty wybranego kalendarza do wartości typu date właśnie kliknięto. Jeśli stan widoku GridView jest wyłączona, jednak każdego odświeżania danych w widoku GridView jest odbitych do jego źródle danych, co powoduje, że wybrana data kalendarza, należy ustawić *ponownie* do pracownika `HireDate`, zastępowania Data, wybierany przez użytkownika.


W tym samouczku jest to moot dyskusji, ponieważ użytkownik nie jest w stanie zaktualizować pracownika `HireDate`. Prawdopodobnie będzie najlepiej skonfigurować kontrolki kalendarza, tak, aby jego daty nie są można wybierać. Niezależnie od tego w tym samouczku pokazano, że w niektórych sytuacjach stan widoku musi zostać włączona w celu zapewnienia określonych funkcji.

## <a name="step-4-showing-the-number-of-days-the-employee-has-worked-for-the-company"></a>Krok 4: Pracował przedstawiający liczbę dni, pracownik firmy

Do tej pory skrócił się dwie aplikacje kontrolek TemplateField:

- Łączenie dwóch lub więcej wartości pól danych w jednej kolumnie, a
- Wyrażenie pola wartości danych za pomocą formantu sieci Web zamiast tekstu

Trzeci używanie kontrolek TemplateField jest w trakcie wyświetlania metadanych dotyczących GridView dane bazowe. Oprócz wyświetlania dat pracowników, na przykład może być również chcemy mieć kolumnę wyświetlającą ile łączną liczbę dni, które zostały w zadaniu.

Jeszcze używanie kontrolek TemplateField innego powstaje w scenariuszach, gdy dane powinny być inaczej wyświetlane w raporcie strony sieci web niż w formacie są przechowywane w bazie danych. Załóżmy, że `Employees` miał tabeli `Gender` pola, które są przechowywane znak `M` lub `F` do wskazania płeć pracownika. Podczas wyświetlania tych informacji na stronie sieci web może chcemy wyświetlić płeć "Mężczyzna" lub "Żeńskiego", a nie po prostu "M" lub "F".

Oba te scenariusze są obsługiwane przez tworzenie *metoda formatowania* w klasie CodeBehind strony ASP.NET (lub w bibliotece osobnej klasy zaimplementowane jako `Shared` metoda), gdy jest wywoływana z szablonu. Metody formatowania jest wywoływany z szablonu przy użyciu tej samej składni databinding widoczne wcześniej. Metoda formatowania można wykonać w dowolnej liczbie parametrów, ale musi zwracać ciąg. Ten ciąg zwracany jest kod HTML, które są wstrzykiwane do szablonu.

Aby zilustrować to pojęcie, możemy rozszerzyć naszego samouczka, aby pokazać kolumnę, zawierającego całkowita liczba dni, przez które pracownik został na nim. Ta metoda formatowania będzie wykonywał w `Northwind.EmployeesRow` obiektu i zwraca liczbę dni, pracownik został zatrudniony jako ciąg. Tej metody mogą być dodawane do klasy CodeBehind strony ASP.NET, ale *musi* można oznaczyć jako `Protected` lub `Public` aby były dostępne z szablonu.


[!code-vb[Main](using-templatefields-in-the-gridview-control-vb/samples/sample5.vb)]

Ponieważ `HiredDate` pole może zawierać `NULL` bazy danych wartości firma Microsoft musi najpierw upewnij się, że wartości nie `NULL` przed wykonaniem obliczeń. Jeśli `HiredDate` wartość `NULL`, po prostu przekierujemy ciąg "Nieznane"; Jeśli nie jest `NULL`, firma Microsoft obliczenia różnicy między bieżącym czasem i `HiredDate` wartości i zwróć liczbę dni.

Korzystanie z tej metody należy wywołać go z TemplateField w widoku GridView przy użyciu składni wiązania danych. Rozpocznij, dodając nowe TemplateField do kontrolki GridView, klikając łącze Edytowanie kolumn w tagu inteligentnego GridView i dodawania nowych TemplateField.


[![Dodaj nowe TemplateField do widoku GridView](using-templatefields-in-the-gridview-control-vb/_static/image44.png)](using-templatefields-in-the-gridview-control-vb/_static/image43.png)

**Rysunek 15**: dodawanie nowych TemplateField do kontrolki GridView ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](using-templatefields-in-the-gridview-control-vb/_static/image45.png))


Ustaw ten nowy TemplateField `HeaderText` właściwość "Dni na zadanie" i jego `ItemStyle`firmy `HorizontalAlign` właściwość `Center`. Do wywołania `DisplayDaysOnJob` metody z szablonu, Dodaj `ItemTemplate` i należy użyć następującej składni wiązania z danymi:


[!code-aspx[Main](using-templatefields-in-the-gridview-control-vb/samples/sample6.aspx)]

`Container.DataItem` Zwraca `DataRowView` obiekt odpowiadający `DataSource` rekord powiązany z `GridViewRow`. Jego `Row` właściwość zwraca, że wpisane pogrubieniem `Northwind.EmployeesRow`, która jest przekazywana do `DisplayDaysOnJob` metody. Ta składnia wiązania danych mogą być wyświetlane bezpośrednio w `ItemTemplate` (jak pokazano w poniższej składni deklaratywnej) lub mogą być przypisane do `Text` właściwości formantu etykiety w sieci Web.

> [!NOTE]
> Alternatywnie zamiast w `EmployeesRow` wypadku firma Microsoft może po prostu Przekaż `HireDate` wartości przy użyciu `<%# DisplayDaysOnJob(Eval("HireDate")) %>`. Jednak `Eval` metoda zwraca `Object`, więc mamy zmienić naszych `DisplayDaysOnJob` podpis metody, aby zaakceptować parametr wejściowy typu `Object`, a zamiast tego. Firma Microsoft nie można rzutować bezrefleksyjne `Eval("HireDate")` wywołanie `DateTime` ponieważ `HireDate` kolumny w `Employees` tabela może zawierać `NULL` wartości. W związku z tym, czy należy zaakceptować `Object` jako parametr wejściowy `DisplayDaysOnJob` metody, sprawdź, gdyby miał on bazę danych `NULL` wartość (który można osiągnąć za pomocą `Convert.IsDBNull(objectToCheck)`), a następnie kontynuuj odpowiednio.


Z powodu tych precyzyjnie załączania czy mogę przenieść całą `EmployeesRow` wystąpienia. W następnym samouczku zobaczymy więcej dopasowywania przykład za pomocą `Eval("columnName")` składni przekazywania parametru wejściowego do metody formatowania.

Poniżej pokazano składni deklaratywnej naszych GridView, po dodaniu TemplateField i `DisplayDaysOnJob` metodę o nazwie z `ItemTemplate`:


[!code-aspx[Main](using-templatefields-in-the-gridview-control-vb/samples/sample7.aspx)]

Rysunek 16 zawiera samouczek ukończone, podczas wyświetlania za pośrednictwem przeglądarki.


[![Liczba dni, które pracownik był w zadaniu jest wyświetlany.](using-templatefields-in-the-gridview-control-vb/_static/image47.png)](using-templatefields-in-the-gridview-control-vb/_static/image46.png)

**Rysunek 16**: liczbę dni pracownik był w zadaniu jest wyświetlany ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](using-templatefields-in-the-gridview-control-vb/_static/image48.png))


## <a name="summary"></a>Podsumowanie

TemplateField w kontrolce GridView umożliwia wyższy stopień elastyczności w wyświetlaniu danych niż jest dostępne z innymi formantami pola. Kontrolek TemplateField świetnie sprawdzają się w sytuacjach, gdzie:

- Wiele pól danych muszą być wyświetlane w jedną kolumnę GridView
- Dane najlepiej jest wyrażany za pomocą formantu sieci Web zamiast zwykłego tekstu
- Dane wyjściowe jest zależny od danych bazowych, takich jak wyświetlanie metadanych lub ponowne formatowanie danych

Oprócz Dostosowywanie widoku danych, kontrolek TemplateField są również używane do dostosowywania interfejsu użytkownika używane do edycji i wstawiania danych, ponieważ zajmiemy się w przyszłości samouczki.

W dwóch następnych samouczków Kontynuuj Eksplorowanie szablonów, począwszy od przyjrzeć się używanie kontrolek TemplateField w DetailsView. Poniżej firma Microsoft będzie przejdź do widoku FormView, aby zapewnić większą elastyczność w układ i struktury danych przy użyciu szablonów audytów pola.

Wszystkiego najlepszego programowania!

## <a name="about-the-author"></a>Informacje o autorze

[Scott Bento](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor siedem ASP/ASP.NET książek i założycielem [4GuysFromRolla.com](http://www.4guysfromrolla.com), pracował nad przy użyciu technologii Microsoft Web od 1998 r. Scott działa jako niezależny Konsultant, trainer i składnika zapisywania. Jego najnowszą książkę Stephena [ *Sams uczyć się ASP.NET 2.0 w ciągu 24 godzin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). ADAM można z Tobą skontaktować w [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) lub za pośrednictwem jego blogu, który znajduje się w temacie [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Specjalne podziękowania dla

W tej serii samouczków został zrecenzowany przez wielu recenzentów pomocne. Weryfikacja potencjalnych klientów w ramach tego samouczka został Dan Jagers. Zainteresowani zapoznaniem Moje kolejnych artykułów MSDN? Jeśli tak, Porzuć mnie linii w [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Poprzednie](custom-formatting-based-upon-data-vb.md)
> [dalej](using-templatefields-in-the-detailsview-control-vb.md)
