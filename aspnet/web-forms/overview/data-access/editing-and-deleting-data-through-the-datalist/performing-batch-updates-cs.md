---
uid: web-forms/overview/data-access/editing-and-deleting-data-through-the-datalist/performing-batch-updates-cs
title: Wykonywanie wsadowe aktualizacji (C#) | Dokumentacja firmy Microsoft
author: rick-anderson
description: Dowiedz się, jak utworzyć pełni nieedytowalne Edytuj DataList, w którym wszystkie jego elementy znajdują się w tryb i wartości, których można zapisać, klikając przycisk "Aktualizuj wszystkie"...
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/30/2006
ms.topic: article
ms.assetid: 57743ca7-5695-4e07-aed1-44b297f245a9
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/editing-and-deleting-data-through-the-datalist/performing-batch-updates-cs
msc.type: authoredcontent
ms.openlocfilehash: af19104edb1849272773193befe1f5b2c7347683
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
---
<a name="performing-batch-updates-c"></a>Wykonywanie wsadowe aktualizacji (C#)
====================
przez [Bento Scott](https://twitter.com/ScottOnWriting)

[Pobierz przykładową aplikację](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_37_CS.exe) lub [pobierania plików PDF](performing-batch-updates-cs/_static/datatutorial37cs1.pdf)

> Dowiedz się, jak utworzyć pełni nieedytowalne Edytuj DataList, w którym wszystkie jego elementy znajdują się w tryb i wartości, których można zapisać, klikając przycisk "Aktualizuj wszystkie" na stronie.


## <a name="introduction"></a>Wprowadzenie

W [poprzedniego samouczek](an-overview-of-editing-and-deleting-data-in-the-datalist-cs.md) możemy zbadać tworzenie DataList na poziomie pozycji. Jak standardowe GridView można edytować każdy element DataList uwzględnione edycji przycisku, gdy kliknięty, spowodowałoby można edytować elementu. Gdy ten element poziomu edycji działa dobrze w przypadku danych, która jest aktualizowana tylko od czasu do czasu, niektórych scenariuszy przypadków zastosowań wymagają użytkownikowi edytowanie wielu rekordów. Jeśli użytkownik należy do edytowania wielu rekordów i wymusza na kliknij przycisk Edytuj, ich zmiany, a następnie kliknij przycisk Aktualizuj dla każdego z nich, ilość kliknięcie mogą utrudniać jej wydajność. W takich sytuacjach lepszym rozwiązaniem jest zapewnienie DataList pełni edytowalny, gdzie jeden *wszystkie* jego elementy znajdują się w trybie edycji i wartości, których można edytować, klikając przycisk Aktualizuj wszystkie na stronie (zobacz rysunek 1).


[![Każdego elementu w pełni edytowalne DataList może być modyfikowany.](performing-batch-updates-cs/_static/image2.png)](performing-batch-updates-cs/_static/image1.png)

**Rysunek 1**: każdego elementu w pełni DataList edycji można modyfikować ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](performing-batch-updates-cs/_static/image3.png))


W tym samouczku zajmiemy się, jak umożliwić użytkownikom aktualizowania informacji dotyczących adresów dostawcy przy użyciu w pełni można edytować elementu DataList.

## <a name="step-1-create-the-editable-user-interface-in-the-datalist-s-itemtemplate"></a>Krok 1: Tworzenie interfejsu użytkownika można edytować w szablon DataList ItemTemplate s

W poprzednim samouczek, gdzie tworzenie standardowych, poziom elementu DataList można edytować, możemy użyliśmy dwa szablony:

- `ItemTemplate` zawiera interfejs użytkownika tylko do odczytu (formantów etykiet w sieci Web do wyświetlania każdego produktu, Nazwa s i cen).
- `EditItemTemplate` zawiera interfejs użytkownika trybu edycji (dwie kontrolki TextBox w sieci Web).

DataList s `EditItemIndex` właściwość decyduje, jakie `DataListItem` (jeśli istnieje) jest renderowany przy użyciu `EditItemTemplate`. W szczególności `DataListItem` których `ItemIndex` wartość odpowiada DataList s `EditItemIndex` właściwości jest renderowany przy użyciu `EditItemTemplate`. Ten model działa również w przypadku, gdy tylko jeden element można edytować w czasie, ale powróci od siebie przy tworzeniu pełni edytowalnego elementu DataList.

Dla DataList pełni edytowalny, chcemy *wszystkie* z `DataListItem` s do renderowania przy użyciu interfejsu można edytować. Najprostszym sposobem, w tym celu jest określenie interfejsu można edytować w `ItemTemplate`. Modyfikowania informacji adresu dostawcy, można edytować interfejs zawiera nazwę dostawcy jako tekst, a następnie pól tekstowych dla adresu, miasta i wartości kraju.

Uruchamianie przez otwarcie `BatchUpdate.aspx` strony, Dodaj formant DataList i ustaw jej `ID` właściwości `Suppliers`. Z tagów inteligentnych DataList s opt można dodać nowe kontrolki ObjectDataSource o nazwie `SuppliersDataSource`.


[![Utwórz nowy element ObjectDataSource o nazwie SuppliersDataSource](performing-batch-updates-cs/_static/image5.png)](performing-batch-updates-cs/_static/image4.png)

**Rysunek 2**: Utwórz nowy składnik o nazwie ObjectDataSource `SuppliersDataSource` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](performing-batch-updates-cs/_static/image6.png))


Skonfiguruj ObjectDataSource do pobierania danych przy użyciu `SuppliersBLL` klasy s `GetSuppliers()` — metoda (patrz rysunek 3). Podobnie jak w przypadku poprzedniego samouczek, zamiast aktualizowanie informacji o dostawcy przez element ObjectDataSource, firma Microsoft będzie współpracować bezpośrednio z warstwy logiki biznesowej. W związku z tym, ustaw listy rozwijanej (Brak) na karcie aktualizacji (patrz rysunek 4).


[![Pobierz informacje przy użyciu metody GetSuppliers()](performing-batch-updates-cs/_static/image8.png)](performing-batch-updates-cs/_static/image7.png)

**Rysunek 3**: pobrać dostawcy informacji za pomocą `GetSuppliers()` — metoda ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](performing-batch-updates-cs/_static/image9.png))


[![Ustaw listy rozwijanej (Brak) na karcie aktualizacji](performing-batch-updates-cs/_static/image11.png)](performing-batch-updates-cs/_static/image10.png)

**Rysunek 4**: Ustaw na liście rozwijanej na (Brak) na karcie aktualizacji ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](performing-batch-updates-cs/_static/image12.png))


Po zakończeniu działania kreatora programu Visual Studio automatycznie generuje DataList s `ItemTemplate` do wyświetlania każdego pola danych zwróconych przez źródło danych w formancie etykiety w sieci Web. Należy zmodyfikować tego szablonu, aby zamiast tego zapewnia interfejs do edycji. `ItemTemplate` Można dostosować za pomocą projektanta przy użyciu opcji Edytuj szablony z tagów inteligentnych DataList s lub bezpośrednio za pomocą składni deklaratywnej.

Poświęć chwilę, aby utworzyć edycji interfejs, który wyświetla nazwę s dostawcy jako tekst, ale zawiera adres dostawcy s, miasta i kraju wartości pól tekstowych. Po wprowadzeniu tych zmian, strony składni deklaratywnej s powinien wyglądać podobnie do poniższej:


[!code-aspx[Main](performing-batch-updates-cs/samples/sample1.aspx)]

> [!NOTE]
> Jak z poprzednim samouczka DataList w tym samouczku musi mieć swój stan widoku włączone.


W `ItemTemplate` I m przy użyciu dwóch nowych klas CSS `SupplierPropertyLabel` i `SupplierPropertyValue`, które zostały dodane do `Styles.css` klasy i skonfigurowana do korzystania z tych samych ustawień stylu `ProductPropertyLabel` i `ProductPropertyValue` klas CSS.


[!code-css[Main](performing-batch-updates-cs/samples/sample2.css)]

Po wprowadzeniu tych zmian, odwiedź stronę tej strony za pośrednictwem przeglądarki. Jak pokazano na rysunku nr 5, każdy element DataList Wyświetla nazwę dostawcy jako tekst i używa pola tekstowe, aby wyświetlić adres, miasta i kraju.


[![Każdego z dostawców elementu DataList jest edytowalna](performing-batch-updates-cs/_static/image14.png)](performing-batch-updates-cs/_static/image13.png)

**Rysunek 5**: każdy dostawca w elementu DataList jest edytowalna ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](performing-batch-updates-cs/_static/image15.png))


## <a name="step-2-adding-an-update-all-button"></a>Krok 2: Dodawanie aktualizacji wszystkich przycisku

Podczas każdego z dostawców na rysunku 5 ma jego adres, miasta i wyświetlane w polu tekstowym pola, obecnie nie ma przycisku aktualizacji dostępnej. Zamiast przycisku Aktualizuj na element z pełni edytowalne DataLists jest zwykle jeden przycisk Aktualizuj wszystkie na stronie, po kliknięciu aktualizacji *wszystkie* rekordów elementu DataList. W tym samouczku umożliwiają s Dodaj dwa przyciski Aktualizuj wszystkie — jeden w górnej części strony, a drugi na dole (chociaż albo przycisku będzie miał ten sam efekt).

Start, dodając kontrolkę przycisku Web powyżej DataList i ustaw jej `ID` właściwości `UpdateAll1`. Następnie dodać drugi kontrolki przycisku w sieci Web pod DataList, ustawienie jej `ID` do `UpdateAll2`. Ustaw `Text` właściwości dla dwóch przycisków do wszystkich aktualizacji. Ponadto tworzenie obsługi zdarzeń dla obu przycisków `Click` zdarzenia. Zamiast duplikowania logiki aktualizacji w każdym z obsługi zdarzeń, umożliwiają s Refaktoryzuj tej logiki na metodę trzeci `UpdateAllSupplierAddresses`, o obsługi zdarzeń, po prostu wywołanie tej metody trzecich.


[!code-csharp[Main](performing-batch-updates-cs/samples/sample3.cs)]

Rysunek 6 przedstawia strony po dodaniu wszystkich aktualizacji przycisków.


[![Dwa przyciski wszystkich aktualizacji zostały dodane do strony](performing-batch-updates-cs/_static/image17.png)](performing-batch-updates-cs/_static/image16.png)

**Rysunek 6**: dwa przyciski wszystkich aktualizacji zostały dodane do strony ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](performing-batch-updates-cs/_static/image18.png))


## <a name="step-3-updating-all-of-the-suppliers-address-information"></a>Krok 3: Aktualizowanie wszystkie informacje o adresie dostawcy

Z wszystkich elementów s DataList wyświetlania interfejsu edycji i dodając przyciski Aktualizuj wszystkie całkowicie pozostaje zapisywanie kod, aby wykonać aktualizację partii. W szczególności należy pętli elementy DataList s i wywołanie `SuppliersBLL` klasy s `UpdateSupplierAddress` metody dla każdej z nich.

Kolekcja `DataListItem` wystąpień tego w skład elementu DataList jest możliwy za pośrednictwem DataList s [ `Items` właściwości](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalist.items.aspx). Z odwołaniem do `DataListItem`, możemy chwycić odpowiadającego `SupplierID` z `DataKeys` kolekcji i programowo odwołania sieci Web pola tekstowego kontrolki w `ItemTemplate` jak pokazano w następującym kodem:


[!code-csharp[Main](performing-batch-updates-cs/samples/sample4.cs)]

Gdy użytkownik kliknie jeden z przycisków Aktualizuj wszystkie `UpdateAllSupplierAddresses` iteruje po każdej metody `DataListItem` w `Suppliers` DataList i wywołania `SuppliersBLL` klasy s `UpdateSupplierAddress` metody, przekazując odpowiednie wartości. Wartość nie wprowadzono adres, miasta lub kraju przekazuje jest wartością `Nothing` do `UpdateSupplierAddress` (zamiast pustego ciągu), które powoduje w bazie danych `NULL` dla pól rekordu s podstawowych.

> [!NOTE]
> Jako ulepszeniem możesz dodać stan formantu etykiety w sieci Web do strony, która zawiera komunikat z potwierdzeniem po wykonaniu aktualizacji wsadowych.


## <a name="updating-only-those-addresses-that-have-been-modified"></a>Aktualizowanie tylko adresy, które zostały zmodyfikowane

Algorytm aktualizacji wsadowych używany dla tego samouczka wywołań `UpdateSupplierAddress` metodę *co* dostawcą DataList, niezależnie od tego, czy informacje o ich adresów został zmieniony. Gdy blind takie aktualizacje nie są zazwyczaj problem z wydajnością, może prowadzić do rekordów zbędny w przypadku której inspekcja zmiany do tabeli bazy danych. Na przykład, jeśli używasz wyzwalaczy do rejestrowania wszystkich `UPDATE` s, aby `Suppliers` do tabeli inspekcji, za każdym razem, gdy użytkownik kliknie przycisk Aktualizuj wszystkie zostanie utworzony nowy rekord inspekcji dla każdego dostawcy systemu, niezależnie od tego, czy użytkownik wszelkie wprowadzone zmiany.

Klasy ADO.NET DataTable i element DataAdapter zostały zaprojektowane do obsługi aktualizacje wsadowe, gdy tylko zmodyfikowanych, usuniętych i nowych rekordów powoduje każdy komunikat bazy danych. Każdy wiersz w tabeli DataTable ma [ `RowState` właściwości](https://msdn.microsoft.com/library/system.data.datarow.rowstate.aspx) wskazuje, czy wiersz został dodany do elementu DataTable, usunąć z niego zmodyfikowane, czy pozostaje niezmieniona. Gdy element DataTable jest wstępnie wypełniony, wszystkie wiersze zostały oznaczone bez zmian. Zmiana wartości kolumny wiersza s oznacza wiersza, jako zmodyfikowane.

W `SuppliersBLL` modyfikacjom danych adresu określonego dostawcy s przez pierwszy Odczyt w rekordzie jednego dostawcy do klasy `SuppliersDataTable` , a następnie ustaw `Address`, `City`, i `Country` wartości kolumn, używając następującego kodu:


[!code-csharp[Main](performing-batch-updates-cs/samples/sample5.cs)]

Ten kod przypisuje naively przekazany adres, miasta i kraju wartości `SuppliersRow` w `SuppliersDataTable` niezależnie od tego, czy wartości zostały zmienione. Zmiany powodują `SuppliersRow` s `RowState` właściwość oznaczona jako zmodyfikowana. Gdy s Warstwa dostępu do danych `Update` metoda jest wywoływana, stwierdzeniu, że `SupplierRow` została zmodyfikowana i w związku z tym wysyła `UPDATE` polecenia w bazie danych.

Załóżmy, że dodano kod do tej metody można przypisać tylko kraju wartości przekazywane w adresu, miasta i jeśli są one różne od `SuppliersRow` s istniejące wartości. W przypadku których adres, miasta i kraju są takie same jak istniejące dane nie zostaną wprowadzone nie zmiany i `SupplierRow` s `RowState` zostanie oznaczona jako niezmienionym. Wynikiem jest to, że gdy DAL s `Update` metoda jest wywoływana, Brak wywołania bazy danych zostanie nawiązane, ponieważ `SuppliersRow` nie został zmodyfikowany.

Wprowadzenie tej zmiany, należy zastąpić instrukcji, które ślepo przypisać przekazany adres, miasta i kraju wartości z następującym kodem:


[!code-csharp[Main](performing-batch-updates-cs/samples/sample6.cs)]

Z tym należy dodać kodu, DAL s `Update` wysyła metody `UPDATE` instrukcji do bazy danych tylko te rekordy, w których wartości związanych z adresem zostały zmienione.

Alternatywnie firma Microsoft może śledzić czy istnieją różnice między polami przekazany adres i danych w bazie danych i, jeśli nie ma żadnych, po prostu pominąć wywołanie DAL s `Update` metody. Ta metoda sprawdza się, gdy metoda, której przy użyciu bazy danych bezpośrednia ponieważ przekazany t nie jest metoda bezpośrednia DB `SuppliersRow` wystąpienie, którego `RowState` można sprawdzić, określ, czy wywołanie bazy danych jest rzeczywiście potrzebne.

> [!NOTE]
> Zawsze `UpdateSupplierAddress` wywołania metody, wywołanie jest wprowadzone w bazie danych można pobrać informacji o zaktualizowanych rekordów. Następnie jeśli nie ma żadnych zmian danych, inne połączenie z bazą danych zostanie przeprowadzona można zaktualizować wiersza tabeli. Ten przepływ pracy może zostać zoptymalizowana przez utworzenie `UpdateSupplierAddress` przeciążenie metody, która akceptuje `EmployeesDataTable` wystąpienia, które ma *wszystkie* zmian z `BatchUpdate.aspx` strony. Następnie można dokonaj jednego wywołania bazy danych można pobrać wszystkie rekordy z `Suppliers` tabeli. Następnie można wyliczyć dwóch zestawów wyników i tylko te rekordy, w których nastąpiły zmiany można go zaktualizować.


## <a name="summary"></a>Podsumowanie

W tym samouczku widzieliśmy tworzenie DataList pełni można edytować, umożliwiając użytkownikowi szybkie modyfikowanie informacji o adresie dla wielu dostawców. Firma Microsoft uruchomione przez definiowanie interfejsu edycji formantu TextBox w sieci Web dla adresu s dostawcy, miasta i wartości kraju w DataList s `ItemTemplate`. Następnie dodaliśmy przyciski Aktualizuj wszystkie powyżej i poniżej elementu DataList. Po utworzeniu użytkownika ma swoje zmiany i kliknięciu jeden z przycisków Aktualizuj wszystkie `DataListItem` s są wyliczane i wywołanie `SuppliersBLL` klasy s `UpdateSupplierAddress` staje się metody.

Programowanie przyjemność!

## <a name="about-the-author"></a>Informacje o autorze

[Scott Bento](http://www.4guysfromrolla.com/ScottMitchell.shtml), autora siedmiu książek ASP/ASP.NET i twórcę z [4GuysFromRolla.com](http://www.4guysfromrolla.com), pracuje z technologii Microsoft Web od 1998. Scott działa jako niezależnego konsultanta trainer i składnika zapisywania. Jest jego najnowszej książki [ *Sams nauczyć się ASP.NET 2.0 w ciągu 24 godzin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Piotr można uzyskać pod adresem [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) lub za pośrednictwem jego blog, który znajduje się w temacie [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Specjalne podziękowania dla

Ten samouczek serii zostało sprawdzone przez wiele recenzentów przydatne. Prowadzić osób dokonujących przeglądu, w tym samouczku zostały Kowalski Zack i Krzysztof Pespisa. Zainteresowani recenzowania Moje nadchodzących artykuły MSDN? Jeśli tak, Porzuć mnie linii w [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Poprzednie](an-overview-of-editing-and-deleting-data-in-the-datalist-cs.md)
> [dalej](handling-bll-and-dal-level-exceptions-cs.md)
