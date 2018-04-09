---
uid: web-forms/overview/data-access/custom-formatting/using-templatefields-in-the-gridview-control-cs
title: Używanie TemplateFields w kontrolce GridView (C#) | Dokumentacja firmy Microsoft
author: rick-anderson
description: Zapewnienie elastyczności widoku GridView oferuje TemplateField, która renderuje przy użyciu szablonu. Szablon może zawierać kombinację statycznego HTML, formantów sieci Web i...
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/31/2010
ms.topic: article
ms.assetid: 11de31e8-a78a-4f96-bd75-66e994175902
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/custom-formatting/using-templatefields-in-the-gridview-control-cs
msc.type: authoredcontent
ms.openlocfilehash: 6485cbda50912920808fc0caf41c888493f210dc
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
---
<a name="using-templatefields-in-the-gridview-control-c"></a>Używanie TemplateFields w kontrolce GridView (C#)
====================
przez [Bento Scott](https://twitter.com/ScottOnWriting)

[Pobierz przykładową aplikację](http://download.microsoft.com/download/9/6/9/969e5c94-dfb6-4e47-9570-d6d9e704c3c1/ASPNET_Data_Tutorial_12_CS.exe) lub [pobierania plików PDF](using-templatefields-in-the-gridview-control-cs/_static/datatutorial12cs1.pdf)

> Zapewnienie elastyczności widoku GridView oferuje TemplateField, która renderuje przy użyciu szablonu. Szablon może zawierać zarówno statycznego HTML sieci Web formantów i składnia wiązania z danymi. W tym samouczku zajmiemy się, jak używać TemplateField do osiągnięcia lepszej dostosowywania za pomocą formantu widoku GridView.


## <a name="introduction"></a>Wprowadzenie

Widoku GridView składa się z zestawu pól wskazujące właściwości, które z `DataSource` mają być uwzględniane w renderowanym danych wyjściowych oraz sposób wyświetlania danych. Najprostsza typ pola jest elementu BoundField, który zawiera wartości danych jako tekst. Inne typy pól wyświetlania danych przy użyciu alternatywnych elementów HTML. Pole CheckBoxField, na przykład renderuje jako wyboru, których stan zaznaczenia zależy od wartości pola określone dane; Element ImageField renderuje obraz, którego źródło obrazu jest oparty na pola określone dane. Hiperłącza i przyciski, których stan jest zależna od podstawowej wartości pola danych może być renderowany przy użyciu typy pól pole hiperłącza HyperLinkField i ButtonField.

Gdy typy pól CheckBoxField, element ImageField pole hiperłącza HyperLinkField i ButtonField umożliwić alternatywny widok danych, nadal są dość ograniczony względem formatowania. Pole CheckBoxField mogą być wyświetlane tylko jednego pola wyboru, natomiast element ImageField mogą być wyświetlane tylko jeden obraz. Co zrobić, jeśli określone pole musi wyświetlić tekst pola wyboru, *i* obrazu, wszystkie zależności od wartości pola danych? Czy możemy co w przypadku wyświetlania danych za pomocą formantu sieci Web innego niż pole wyboru, Image, hiperłącze lub przycisk? Ponadto elementu BoundField ogranicza wyświetlanie do pola danych. Co zrobić, jeśli trzeba Pokaż dwóch lub więcej wartości pól danych w jednej kolumnie w widoku GridView?

Aby dostosować ten poziom elastyczności widoku GridView oferuje TemplateField, która renderuje przy użyciu *szablonu*. Szablon może zawierać zarówno statycznego HTML sieci Web formantów i składnia wiązania z danymi. Ponadto TemplateField ma różne szablony, które mogą służyć do dostosowywania renderowania w różnych sytuacjach. Na przykład `ItemTemplate` używany domyślnie do renderowania komórki dla każdego wiersza, ale `EditItemTemplate` szablon może być używany do dostosowania interfejsu podczas edytowania danych.

W tym samouczku zajmiemy się, jak używać TemplateField do osiągnięcia lepszej dostosowywania za pomocą formantu widoku GridView. W [poprzedniego samouczek](custom-formatting-based-upon-data-cs.md) widzieliśmy sposobu dostosowywania oparte na podstawowych danych przy użyciu formatowania `DataBound` i `RowDataBound` procedury obsługi zdarzeń. Innym sposobem dostosowania formatowanie oparte na danych przez wywołanie metody formatuje metody w ramach szablonu. Ta technika przyjrzymy w tym samouczku, jak również.

W tym samouczku używamy TemplateFields do dostosowywania wyglądu listy pracowników. W szczególności firma Microsoft będzie wyświetlić listę wszystkich pracowników, ale będzie wyświetlany pracownika imiona i nazwiska w jednej kolumnie, datę ich zatrudnienia w formancie kalendarza i kolumna stanu, która wskazuje, ile dni już został zatrudnieni w firmie.


[![Trzy TemplateFields są używane, aby dostosować wyświetlanie](using-templatefields-in-the-gridview-control-cs/_static/image2.png)](using-templatefields-in-the-gridview-control-cs/_static/image1.png)

**Rysunek 1**: trzy TemplateFields są używane, aby dostosować wyświetlanie ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](using-templatefields-in-the-gridview-control-cs/_static/image3.png))


## <a name="step-1-binding-the-data-to-the-gridview"></a>Krok 1: Wiązanie danych w widoku GridView

W sytuacji, gdy musisz użyć TemplateFields do dostosowywania wyglądu, można go znaleźć najłatwiej Rozpocznij od utworzenia widoku GridView formant, który zawiera tylko BoundFields najpierw, a następnie dodaj nowe TemplateFields lub przekonwertować istniejącej BoundFields do TemplateFields zgodnie z potrzebami. W związku z tym Zacznijmy w tym samouczku dodając element GridView do strony za pomocą projektanta i powiązania jej z ObjectDataSource, które zwraca listę pracowników. Te czynności spowoduje utworzenie Element GridView z BoundFields dla każdego pola pracowników.

Otwórz `GridViewTemplateField.aspx` strony i przeciągnij element GridView z przybornika do projektanta. Z tagów inteligentnych w widoku GridView możliwość dodania nowej kontrolki ObjectDataSource, który wywołuje `EmployeesBLL` klasy `GetEmployees()` metody.


[![Dodaj nowe kontrolki ObjectDataSource, która wywołuje metodę GetEmployees()](using-templatefields-in-the-gridview-control-cs/_static/image5.png)](using-templatefields-in-the-gridview-control-cs/_static/image4.png)

**Rysunek 2**: Dodaj nowe kontrolki ObjectDataSource tego wywołuje `GetEmployees()` — metoda ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](using-templatefields-in-the-gridview-control-cs/_static/image6.png))


Powiązanie widoku GridView w ten sposób spowoduje automatyczne dodanie elementu BoundField dla każdej właściwości pracownika: `EmployeeID`, `LastName`, `FirstName`, `Title`, `HireDate`, `ReportsTo`, i `Country`. Dla tego raportu teraz nie odblokowane z wyświetlaniem `EmployeeID`, `ReportsTo`, lub `Country` właściwości. Aby usunąć te BoundFields można wykonywać następujące czynności:

- W systemie kliknij okno dialogowe pola łącza Edytuj kolumny w widoku GridView tagów inteligentnych można wyświetlić tego okna dialogowego. Następnie wybierz BoundFields z lewej dolnej liście i kliknij czerwony znak X przycisk, aby usunąć elementu BoundField.
- Ręcznie Edytuj składni deklaratywnej w widoku GridView z widoku źródła, Usuń `<asp:BoundField>` elementu BoundField chcesz usunąć.

Po usunięciu `EmployeeID`, `ReportsTo`, i `Country` BoundFields, znaczników w widoku GridView powinien wyglądać następująco:


[!code-aspx[Main](using-templatefields-in-the-gridview-control-cs/samples/sample1.aspx)]

Poświęć chwilę, aby wyświetlić postęp naszych w przeglądarce. W tym momencie tabeli z rekordem powinna być widoczna dla każdego pracownika i cztery kolumny: jeden dla pracownika nazwisko, jeden dla ich imię, jeden dla ich tytułu i jeden na datę ich zatrudnienia.


[![LastName, FirstName, tytuł i rekrutacji pola są wyświetlane dla każdego pracownika](using-templatefields-in-the-gridview-control-cs/_static/image8.png)](using-templatefields-in-the-gridview-control-cs/_static/image7.png)

**Rysunek 3**: `LastName`, `FirstName`, `Title`, i `HireDate` pola są wyświetlane dla każdego pracownika ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](using-templatefields-in-the-gridview-control-cs/_static/image9.png))


## <a name="step-2-displaying-the-first-and-last-names-in-a-single-column"></a>Krok 2: Wyświetlanie imiona i nazwiska w jednej kolumnie

Obecnie każdy pracownik imienia i nazwiska są wyświetlane w oddzielnej kolumnie. Może to być dobry połączyć je do jednej kolumny. W tym celu należy użyć TemplateField. Firma Microsoft może albo Dodaj nowe pole TemplateField, Dodaj do niej potrzebne znaczniki i składnia wiązania z danymi, a następnie usuń `FirstName` i `LastName` BoundFields lub firma Microsoft może przekonwertować `FirstName` elementu BoundField na pole TemplateField, Edytuj TemplateField do uwzględnienia `LastName` wartość, a następnie usuń `LastName` elementu BoundField.

W obu przypadkach efekt net takiego samego wyniku, ale osobiście podoba konwertowanie BoundFields TemplateFields, gdy jest to możliwe, ponieważ konwersja automatycznie dodaje `ItemTemplate` i `EditItemTemplate` z formantów sieci Web i składnia wiązania z danymi, aby naśladował wygląd i funkcjonalność elementu BoundField. Korzyścią jest to, że trzeba będzie mniej pracy TemplateField jako proces konwersji będzie wykonali część obciążenia pracą firmie Microsoft.

Aby przekonwertować istniejącego elementu BoundField na pole TemplateField, kliknij łącze Edytowanie kolumn z tagów inteligentnych w widoku GridView, wyświetlania okna dialogowego pól. Wybierz pole BoundField przekonwertować z listy w lewym dolnym rogu, a następnie kliknij łącze "Konwertuj to pole na pole TemplateField" w prawym dolnym rogu.


[![Konwertuj na pole TemplateField z okna dialogowego pól elementu BoundField](using-templatefields-in-the-gridview-control-cs/_static/image11.png)](using-templatefields-in-the-gridview-control-cs/_static/image10.png)

**Rysunek 4**: Konwertowanie elementu BoundField na pole TemplateField z okna dialogowego pola ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](using-templatefields-in-the-gridview-control-cs/_static/image12.png))


Przejdź dalej i przekonwertować `FirstName` elementu BoundField na pole TemplateField. Po tej zmianie należy nie ma różnic perceptive w projektancie. Jest to spowodowane Konwertowanie elementu BoundField na pole TemplateField tworzy TemplateField, zawierającą wygląd i działanie elementu BoundField. Pomimo nie jest visual różnicy w tym momencie w projektancie, ten proces konwersji zastąpił elementu BoundField składni deklaratywnej - `<asp:BoundField DataField="FirstName" HeaderText="FirstName" SortExpression="FirstName" />` — przy użyciu następującej składni TemplateField:


[!code-aspx[Main](using-templatefields-in-the-gridview-control-cs/samples/sample2.aspx)]

Jak widać, TemplateField składa się z dwóch szablonów `ItemTemplate` mający etykietę których `Text` właściwości ustawiono wartość `FirstName` pola danych i `EditItemTemplate` z pola tekstowego kontroli, których `Text` także ustawienia właściwości Aby `FirstName` pola danych. Składnia wiązania z danymi - `<%# Bind("fieldName") %>` -oznacza to, że w polu danych *`fieldName`* jest powiązany z określonej właściwości formantu sieci Web.

Aby dodać `LastName` wartość do tego TemplateField, należy dodać inny formant etykiety w sieci Web w pola danych `ItemTemplate` i powiąż jego `Text` właściwości `LastName`. Można to zrobić ręcznie lub za pomocą projektanta. Aby wykonać to zadanie ręcznie, po prostu Dodaj odpowiedniej składni deklaratywnej do `ItemTemplate`:


[!code-aspx[Main](using-templatefields-in-the-gridview-control-cs/samples/sample3.aspx)]

Aby dodać go za pomocą projektanta, kliknij łącze Edytuj szablony z tagów inteligentnych w widoku GridView. Spowoduje to wyświetlenie interfejsu edycji szablonu w widoku GridView. W tym tagów inteligentnych interfejsu znajduje się lista szablonów w widoku GridView. Ponieważ zatrudniamy tylko jeden TemplateField w tym momencie, tylko szablony wymienione na liście rozwijanej są tych szablonów dla `FirstName` TemplateField wraz z `EmptyDataTemplate` i `PagerTemplate`. `EmptyDataTemplate` Szablonu, jeśli jest określony, używany do renderowania danych wyjściowych w widoku GridView, jeśli nie są wyniki w danych powiązany Element GridView; `PagerTemplate`, jeśli jest określony, używany do renderowania interfejsu stronicowania w widoku GridView, który obsługuje stronicowania.


[![W widoku GridView szablony mogą być edytowane przy użyciu narzędzia Projektant](using-templatefields-in-the-gridview-control-cs/_static/image14.png)](using-templatefields-in-the-gridview-control-cs/_static/image13.png)

**Rysunek 5**: GridView szablonów można można edytowane za pomocą projektanta ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](using-templatefields-in-the-gridview-control-cs/_static/image15.png))


Można również wyświetlić `LastName` w `FirstName` TemplateField przeciągnij formantu etykiety z przybornika do `FirstName` na pole TemplateField `ItemTemplate` w widoku GridView na edytowanie szablonu interfejsu.


[![Dodawanie formantu etykiety sieci Web do ItemTemplate TemplateField imię](using-templatefields-in-the-gridview-control-cs/_static/image17.png)](using-templatefields-in-the-gridview-control-cs/_static/image16.png)

**Rysunek 6**: Dodawanie formantu etykiety sieci Web do `FirstName` ItemTemplate na pole TemplateField ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](using-templatefields-in-the-gridview-control-cs/_static/image18.png))


W tym momencie formant etykiety sieci Web dodany do TemplateField ma jego `Text` właściwość "Label". Potrzebujemy zmienić to ustawienie, aby ta właściwość jest powiązana z wartością `LastName` zamiast tego pola danych. Aby wykonać to kliknięcie tagów inteligentnych formantu etykiety i wybierz opcję Edytuj powiązania danych.


[![Wybierz opcję Edycja powiązania danych z tagów inteligentnych etykiet](using-templatefields-in-the-gridview-control-cs/_static/image20.png)](using-templatefields-in-the-gridview-control-cs/_static/image19.png)

**Rysunek 7**: wybierz opcję Edytuj powiązania DataBindings z tagów inteligentnych etykiet ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](using-templatefields-in-the-gridview-control-cs/_static/image21.png))


Zostanie wyświetlone okno dialogowe powiązania danych. W tym miejscu można wybrać właściwość do uczestnictwa w wiązania z danymi na liście z lewej strony i wybierz pole, aby powiązać dane z listy rozwijanej z prawej strony. Wybierz `Text` właściwości od lewej i `LastName` z prawej strony, a następnie kliknij przycisk OK.


[![Powiązać z właściwością Text w polu danych nazwisko](using-templatefields-in-the-gridview-control-cs/_static/image23.png)](using-templatefields-in-the-gridview-control-cs/_static/image22.png)

**Rysunek 8**: powiązać `Text` właściwości `LastName` pola danych ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](using-templatefields-in-the-gridview-control-cs/_static/image24.png))


> [!NOTE]
> Okno dialogowe powiązania danych można określić, czy ma być przeprowadzane dwukierunkowego wiązania z danymi. Jeśli to pole nie jest zaznaczona, składnia wiązania z danymi `<%# Eval("LastName")%>` będzie używany zamiast `<%# Bind("LastName")%>`. Albo podejście jest poprawne dla tego samouczka. Dwukierunkowego wiązania z danymi staje się ważne przy wstawianiu i edytowanie danych. Po prostu wyświetlania danych, jednak albo metoda będzie działać równie dobrze. Omówiono dwukierunkowego wiązania z danymi szczegółowo w przyszłości samouczki.


Poświęć chwilę, aby wyświetlić tą stronę za pośrednictwem przeglądarki. Jak widać, widoku GridView nadal zawiera cztery kolumny; jednak `FirstName` w kolumnie jest teraz wyświetlany *zarówno* `FirstName` i `LastName` wartości pola danych.


[![Imię i nazwisko wartości są wyświetlane w jednej kolumnie](using-templatefields-in-the-gridview-control-cs/_static/image26.png)](using-templatefields-in-the-gridview-control-cs/_static/image25.png)

**Rysunek 9**: zarówno `FirstName` i `LastName` wartości są wyświetlane w jednej kolumnie ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](using-templatefields-in-the-gridview-control-cs/_static/image27.png))


Aby ukończyć ten krok pierwszy, Usuń `LastName` elementu BoundField i zmiana nazwy `FirstName` na pole TemplateField `HeaderText` dla właściwości "Name". Po wprowadzeniu tych zmian deklaratywne znaczników w widoku GridView powinna wyglądać następująco:


[!code-aspx[Main](using-templatefields-in-the-gridview-control-cs/samples/sample4.aspx)]


[![Pierwszy i nazwisk każdego pracownika są wyświetlane w jedną kolumną](using-templatefields-in-the-gridview-control-cs/_static/image29.png)](using-templatefields-in-the-gridview-control-cs/_static/image28.png)

**Na rysunku nr 10**: pierwszy i ostatni nazwy każdy pracownik są wyświetlane w jedną kolumną ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](using-templatefields-in-the-gridview-control-cs/_static/image30.png))


## <a name="step-3-using-the-calendar-control-to-display-thehireddatefield"></a>Krok 3: Za pomocą formantu kalendarza wyświetlania`HiredDate`pola

Wyświetlanie wartości pola danych jako tekst w widoku GridView jest tak proste, jak użyć elementu BoundField. W niektórych scenariuszach jednak danych jest najlepiej wyrazić przy użyciu określonego formantu sieci Web zamiast tylko tekst. Takie dostosowania wyświetlania danych jest możliwe za pomocą TemplateFields. Na przykład zamiast niż wyświetlić datę zatrudnienia pracownika jako tekst, firma Microsoft może pokazać kalendarza (przy użyciu [formant kalendarza](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar(VS.80).aspx)) z ich data zatrudnienia wyróżnione.

W tym celu uruchom konwertując `HiredDate` elementu BoundField na pole TemplateField. Po prostu przejdź do widoku GridView tagów inteligentnych i kliknij łącze edycji kolumn otworzeniem okna dialogowego pól. Wybierz `HiredDate` elementu BoundField i kliknij przycisk "Konwertuj to pole na pole TemplateField."


[![Konwertowanie elementu HiredDate BoundField na pole TemplateField](using-templatefields-in-the-gridview-control-cs/_static/image32.png)](using-templatefields-in-the-gridview-control-cs/_static/image31.png)

**Rysunek 11**: konwertowanie `HiredDate` elementu BoundField na pole TemplateField ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](using-templatefields-in-the-gridview-control-cs/_static/image33.png))


Jak widzieliśmy w kroku 2, to spowoduje zamianę elementu BoundField TemplateField, który zawiera `ItemTemplate` i `EditItemTemplate` z etykiety i pola tekstowego których `Text` właściwości są powiązane z `HiredDate` wartość, przy użyciu składni wiązania z danymi `<%# Bind("HiredDate")%>`.

Aby zastąpić tekst formantu kalendarza, edytowanie szablonu, usuwając etykiety i dodawanie formantu kalendarza. Przy użyciu projektanta, wybierz pozycję Edytuj szablony z tagów inteligentnych w widoku GridView i wybierz polecenie `HireDate` na pole TemplateField `ItemTemplate` z listy rozwijanej. Następnie usuń formantu etykiety i przeciągnij formant kalendarza z przybornika do interfejsu edycji szablonu.


[![Dodawanie do formantu kalendarza rekrutacji ItemTemplate na pole TemplateField](using-templatefields-in-the-gridview-control-cs/_static/image35.png)](using-templatefields-in-the-gridview-control-cs/_static/image34.png)

**Rysunek 12**: Dodawanie formantu kalendarza `HireDate` na pole TemplateField `ItemTemplate` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](using-templatefields-in-the-gridview-control-cs/_static/image36.png))


W tym momencie każdego wiersza w widoku GridView będzie zawierać formant kalendarza w jego `HiredDate` TemplateField. Jednak rzeczywista przez pracownika `HiredDate` nie określono wartości dowolne miejsce w formancie kalendarza, powodując każdego formantu kalendarza domyślnie wyświetla bieżącego miesiąca oraz daty. Aby rozwiązać ten problem, należy przypisać każdego pracownika `HiredDate` do formantu kalendarza [SelectedDate](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.selecteddate(VS.80).aspx) i [VisibleDate](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.visibledate(VS.80).aspx) właściwości.

Tag inteligentny formant kalendarza wybierz Edytuj powiązania danych. Następnie powiązać zarówno `SelectedDate` i `VisibleDate` właściwości `HiredDate` pola danych.


[![Właściwości VisibleDate i SelectedDate należy powiązać pole HiredDate danych](using-templatefields-in-the-gridview-control-cs/_static/image38.png)](using-templatefields-in-the-gridview-control-cs/_static/image37.png)

**Rysunek 13**: powiązać `SelectedDate` i `VisibleDate` właściwości `HiredDate` pola danych ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](using-templatefields-in-the-gridview-control-cs/_static/image39.png))


> [!NOTE]
> Formant kalendarza wybrana data nie musi być widoczny. Na przykład kalendarz może mieć 1 sierpnia<sup>st</sup>, 1999 jako wybranej daty, ale być widoczne w bieżącym miesiącu i roku. Wybrana data i Data widoczne są określane przez formant kalendarza `SelectedDate` i `VisibleDate` właściwości. Ponieważ chcemy zarówno wybierz pracownika `HiredDate` i upewnij się, że jest ona wyświetlana należy do obu tych właściwości do powiązania `HireDate` pola danych.


Podczas wyświetlania strony w przeglądarce, kalendarz teraz pokazuje miesiąc od daty zatrudnionego pracownika i wybiera w określonym dniu.


[![HiredDate pracownika jest wyświetlany w formancie kalendarza](using-templatefields-in-the-gridview-control-cs/_static/image41.png)](using-templatefields-in-the-gridview-control-cs/_static/image40.png)

**Rysunek 14**: pracownika `HiredDate` jest wyświetlany w formancie kalendarza ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](using-templatefields-in-the-gridview-control-cs/_static/image42.png))


> [!NOTE]
> Sprzecznie wszystkie przykłady możemy dotychczasowych przedstawiono w tym samouczku robiliśmy *nie* ustawić `EnableViewState` właściwości `false` dla tego widoku GridView. Powodem tej decyzji jest ponieważ kliknięcie daty formant kalendarza powoduje odświeżenie strony, ustawienie kalendarza wybranej daty na datę właśnie kliknięto. Jeśli stan widoku GridView jest wyłączone, jednak każdej strony danych w widoku GridView jest odbitych do jego źródle danych, co powoduje, że wybrana data kalendarza ma zostać ustawiona *ponownie* do pracownika `HireDate`, zastępowania Data, wybierany przez użytkownika.


W tym samouczku jest moot dyskusji ponieważ użytkownik nie będzie mógł zaktualizować pracownika `HireDate`. Prawdopodobnie będzie najlepsza skonfigurować formant kalendarza, tak aby nie są można wybierać dat jej. Niezależnie od tego w tym samouczku przedstawiono, w niektórych sytuacjach stan widoku można włączyć w celu zapewnienia niektórych funkcji.

## <a name="step-4-showing-the-number-of-days-the-employee-has-worked-for-the-company"></a>Krok 4: Wyświetlana jest liczba dni korzystania z pracownika działał dla firmy

Do tej pory zaobserwowano dwie aplikacje TemplateFields:

- Łączenie dwóch lub więcej wartości pól danych w jednej kolumnie, i
- Wyrażenie wartości pola danych za pomocą formantu sieci Web zamiast tekstu

Trzeci stosowania TemplateFields jest w trakcie wyświetlania metadanych dotyczących w widoku GridView podstawowych danych. Oprócz wyświetlanie dat pracowników, na przykład może być również chcemy mieć kolumnę wyświetlającą ile łączną liczbę dni one w zadaniu.

Jeszcze inną stosowania TemplateFields powstaje w scenariuszach, gdy danych musi być inaczej wyświetlane w raporcie strony sieci web niż w formacie są przechowywane w bazie danych. Załóżmy, że `Employees` miał tabeli `Gender` pola, które są przechowywane znak `M` lub `F` wskaż, płeć pracownika. Jeśli te informacje są wyświetlane na stronie sieci web może chęć pokazania płeć, jako "Męskiego" lub "Żeńskiego", a nie tylko "M" lub "F".

Oba te scenariusze są obsługiwane przez tworzenie *formatowania — metoda* w klasie związanej z kodem strony ASP.NET (lub w bibliotece osobnej klasy zaimplementowane jako `static` metody) który jest wywoływany z szablonu. Metoda formatowania jest wywoływany z szablonu przy użyciu tej samej składni wiązania z danymi widoczne wcześniej. Metoda formatowania można wykonać w dowolnej liczbie parametrów, ale musi zwracać ciąg. Ten ciąg zwrócony jest kodu HTML, które są wstrzykiwane do szablonu.

W celu zilustrowania koncepcji, ta funkcja pozwala rozszerzyć naszym samouczkiem, aby pokazać kolumnę zawierającą sumę dni, przez które pracownik był w zadaniu. Ta metoda formatowania potrwa `Northwind.EmployeesRow` obiektu i zwraca liczbę dni, które pracownik ma zatrudnienia jako ciąg. Tej metody można dodać do klasy związane z kodem strony ASP.NET, ale *musi* oznaczone jako `protected` lub `public` aby były dostępne z szablonu.


[!code-csharp[Main](using-templatefields-in-the-gridview-control-cs/samples/sample5.cs)]

Ponieważ `HiredDate` pole może zawierać `NULL` bazy danych wartości firma Microsoft musi najpierw upewnij się, że wartość nie jest `NULL` przed wykonaniem obliczeń. Jeśli `HiredDate` wartość jest `NULL`, po prostu zostanie zwrócona wartość "Unknown"; Jeśli nie jest `NULL`, możemy obliczenia różnicy między bieżącym czasem i `HiredDate` wartości i zwraca liczbę dni.

Aby korzystać z tej metody należy wywołać go z TemplateField w widoku GridView przy użyciu składni wiązania z danymi. Rozpocznij, dodając nowe pole TemplateField do widoku GridView, klikając łącze Edytuj kolumny w widoku GridView tagów inteligentnych i dodać nowe pole TemplateField.


[![Dodaj nowe pole TemplateField do widoku GridView](using-templatefields-in-the-gridview-control-cs/_static/image44.png)](using-templatefields-in-the-gridview-control-cs/_static/image43.png)

**Rysunek 15**: Dodaj nowe pole TemplateField do widoku GridView ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](using-templatefields-in-the-gridview-control-cs/_static/image45.png))


Ustaw ten nowy TemplateField `HeaderText` dla właściwości "Dni na zadanie" i jego `ItemStyle`w `HorizontalAlign` właściwości `Center`. Aby wywołać `DisplayDaysOnJob` metody z szablonu, Dodaj `ItemTemplate` i należy użyć następującej składni wiązania z danymi:


[!code-aspx[Main](using-templatefields-in-the-gridview-control-cs/samples/sample6.aspx)]

`Container.DataItem` Zwraca `DataRowView` obiekt odpowiadający `DataSource` rekord powiązany z `GridViewRow`. Jego `Row` właściwość zwraca jednoznacznie `Northwind.EmployeesRow`, który jest przekazywany do `DisplayDaysOnJob` metody. Ta składnia wiązania z danymi mogą występować bezpośrednio w `ItemTemplate` (jak pokazano w poniższej składni deklaratywnej) lub może zostać przypisane do `Text` właściwości formantu etykiety w sieci Web.

> [!NOTE]
> Możesz też zamiast przekazywanie `EmployeesRow` wystąpienia, firma Microsoft może po prostu Przekaż w `HireDate` wartości przy użyciu `<%# DisplayDaysOnJob(Eval("HireDate")) %>`. Jednak `Eval` metoda zwraca `object`, więc mamy zmienić naszych `DisplayDaysOnJob` podpis metody, aby zaakceptować parametru wejściowego typu `object`, zamiast tego. Firma Microsoft nie można rzutować ślepo `Eval("HireDate")` wywołanie `DateTime` ponieważ `HireDate` kolumny w `Employees` tabela może zawierać `NULL` wartości. W związku z tym, czy należy zaakceptować `object` jako parametru wejściowego dla `DisplayDaysOnJob` metoda, sprawdź, czy miała bazy danych `NULL` wartość (co można wykonać przy użyciu `Convert.IsDBNull(objectToCheck)`), a następnie przejdź w związku z tym.


Z powodu tych precyzyjnie I została wybrana opcja umożliwia przekazywanie całego `EmployeesRow` wystąpienia. W następnym samouczku zajmiemy się tym więcej przykład dopasowywania dla przy użyciu `Eval("columnName")` składni parametr wejściowy może być przekazywany do metody formatowania.

Poniżej pokazano składni deklaratywnej naszych GridView, po dodaniu TemplateField i `DisplayDaysOnJob` metoda wywoływana z `ItemTemplate`:


[!code-aspx[Main](using-templatefields-in-the-gridview-control-cs/samples/sample7.aspx)]

16 rysunek pokazuje samouczek ukończone, podczas wyświetlania za pośrednictwem przeglądarki.


[![Liczba dni, przez które pracownik był w zadaniu jest wyświetlany.](using-templatefields-in-the-gridview-control-cs/_static/image47.png)](using-templatefields-in-the-gridview-control-cs/_static/image46.png)

**Rysunek 16**: liczbę dni pracownik był w zadaniu jest wyświetlany ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](using-templatefields-in-the-gridview-control-cs/_static/image48.png))


## <a name="summary"></a>Podsumowanie

Umożliwia wyższego poziomu bezpieczeństwa i elastyczność w wyświetlaniu danych niż jest dostępne inne formanty pole TemplateField w kontrolce GridView. TemplateFields idealnie nadają się do sytuacji, w których:

- Wiele pól danych muszą być widoczny w widoku GridView
- Dane najlepiej jest wyrażona za pomocą formantu sieci Web zamiast zwykłego tekstu
- Dane wyjściowe zależy od danych, takich jak wyświetlanie metadanych lub ponowne formatowanie danych

Oprócz dostosowywania widoku danych, TemplateFields są również używane do dostosowywania interfejsów użytkownika używane do edycji i wstawiania danych, ponieważ zajmiemy się w przyszłości samouczki.

Następne dwa samouczki nadal eksploracji szablony, począwszy od przyjrzeć się przy użyciu TemplateFields w widoku DetailsView. Następujące, firma Microsoft będzie przejdź do widoku FormView, zapewniają większą elastyczność w układ i struktury danych przy użyciu szablonów zamiast pól.

Programowanie przyjemność!

## <a name="about-the-author"></a>Informacje o autorze

[Scott Bento](http://www.4guysfromrolla.com/ScottMitchell.shtml), autora siedmiu książek ASP/ASP.NET i twórcę z [4GuysFromRolla.com](http://www.4guysfromrolla.com), pracuje z technologii Microsoft Web od 1998. Scott działa jako niezależnego konsultanta trainer i składnika zapisywania. Jest jego najnowszej książki [ *Sams nauczyć się ASP.NET 2.0 w ciągu 24 godzin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Piotr można uzyskać pod adresem [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) lub za pośrednictwem jego blog, który znajduje się w temacie [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Specjalne podziękowania dla

Ten samouczek serii zostało sprawdzone przez wiele recenzentów przydatne. Recenzenta realizacji w tym samouczku został Dan Jagers. Zainteresowani recenzowania Moje nadchodzących artykuły MSDN? Jeśli tak, Porzuć mnie linii w [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Poprzednie](custom-formatting-based-upon-data-cs.md)
> [dalej](using-templatefields-in-the-detailsview-control-cs.md)
