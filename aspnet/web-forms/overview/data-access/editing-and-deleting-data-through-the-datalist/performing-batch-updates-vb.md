---
uid: web-forms/overview/data-access/editing-and-deleting-data-through-the-datalist/performing-batch-updates-vb
title: Wykonywanie aktualizacji wsadowych (VB) | Dokumentacja firmy Microsoft
author: rick-anderson
description: Dowiedz się, jak utworzyć w pełni edytowalne DataList, gdzie wszystkie jego elementy znajdują się w edycji tryb i którego wartości można zapisać, klikając przycisk "Aktualizuj wszystkie"...
ms.author: aspnetcontent
ms.date: 10/30/2006
ms.assetid: 8dac22a7-91de-4e3b-888f-a4c438b03851
msc.legacyurl: /web-forms/overview/data-access/editing-and-deleting-data-through-the-datalist/performing-batch-updates-vb
msc.type: authoredcontent
ms.openlocfilehash: 6d6575e5f13441c38c5d7c74c8b5136a5206ffa9
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/05/2018
ms.locfileid: "37803898"
---
<a name="performing-batch-updates-vb"></a>Wykonywanie aktualizacji wsadowych (VB)
====================
przez [Bento Scott](https://twitter.com/ScottOnWriting)

[Pobierz przykładową aplikację](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_37_VB.exe) lub [Pobierz plik PDF](performing-batch-updates-vb/_static/datatutorial37vb1.pdf)

> Dowiedz się, jak utworzyć w pełni edytowalne DataList, gdzie wszystkie jego elementy znajdują się w edycji tryb i którego wartości można zapisać, klikając przycisk "Aktualizuj wszystkie" na stronie.


## <a name="introduction"></a>Wprowadzenie

W [poprzedni Samouczek](an-overview-of-editing-and-deleting-data-in-the-datalist-vb.md) zbadaliśmy sposób tworzenia DataList na poziomie elementu. Jak standardowy GridView można edytować każdy element w kontrolce DataList uwzględnione zmiany przycisku, po kliknięciu czyniłyby można edytować elementu. Ten element poziomu edycji działa dobrze w przypadku danych, które są aktualizowane tylko co pewien czas, niektórych scenariuszy przypadków użycia wymagają użytkownikowi edytowanie wielu rekordów. Jeśli użytkownik musi edytowanie wielu rekordów i jest zmuszony do kliknij przycisk Edytuj, ich zmiany, a następnie kliknij przycisk Aktualizuj dla każdego z nich, ilość klikając mogą utrudniać jej wydajność. W takich sytuacjach lepszym rozwiązaniem jest zapewnienie DataList pełni edytowalne, gdzie jeden *wszystkich* jego elementy są w trybie edycji, a których wartości mogą być edytowane, klikając przycisk Aktualizuj wszystkie na stronie (patrz rysunek 1).


[![Każdy element w pełni edytowalne DataList może być modyfikowany.](performing-batch-updates-vb/_static/image2.png)](performing-batch-updates-vb/_static/image1.png)

**Rysunek 1**: można zmodyfikować każdy element w pełni edytowalne DataList ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](performing-batch-updates-vb/_static/image3.png))


W tym samouczku zajmiemy się, jak umożliwić użytkownikom na aktualizowanie informacji o adresie dostawcy przy użyciu w pełni edytowalne DataList.

## <a name="step-1-create-the-editable-user-interface-in-the-datalist-s-itemtemplate"></a>Krok 1: Tworzenie interfejsu użytkownika można edytować w kontrolkach DataList ItemTemplate s

W poprzednim samouczku gdzie tworzenia standardowych, schodzić do poziomu DataList można edytować, firma Microsoft użyte dwa szablony:

- `ItemTemplate` zawiera interfejs użytkownika tylko do odczytu (formantów etykiet w sieci Web do wyświetlania każdej Nazwa s produktu i cenę).
- `EditItemTemplate` zawiera interfejs użytkownika tryb edycji (dwóch formantów sieci Web w polu tekstowym).

DataList s `EditItemIndex` właściwości połączenia z opisywanym co `DataListItem` (jeśli istnieje) jest renderowany przy użyciu `EditItemTemplate`. W szczególności `DataListItem` którego `ItemIndex` wartość odpowiada DataList s `EditItemIndex` właściwość jest renderowany przy użyciu `EditItemTemplate`. Ten model działa dobrze w przypadku, gdy tylko jeden element można edytować w czasie, ale od siebie wypada podczas tworzenia w pełni edytowalne DataList.

Dla DataList pełni edytowalne, chcemy *wszystkich* z `DataListItem` s do renderowania przy użyciu interfejsu można edytować. Najprostszym sposobem, aby osiągnąć ten cel jest zdefiniowanie interfejsu można edytować w `ItemTemplate`. Do modyfikowania informacje o adresie dostawcy, można edytować interfejs zawiera nazwę dostawcy jako tekst, a następnie pola tekstowe adres, miasto i kraj wartości.

Zacznij od otwarcia `BatchUpdate.aspx` strony, Dodaj kontrolki DataList i ustaw jego `ID` właściwość `Suppliers`. Za pomocą kontrolek DataList s tagu inteligentnego, zoptymalizowany pod kątem można dodać nowego formantu ObjectDataSource o nazwie `SuppliersDataSource`.


[![Tworzenie nowego elementu ObjectDataSource, o nazwie SuppliersDataSource](performing-batch-updates-vb/_static/image5.png)](performing-batch-updates-vb/_static/image4.png)

**Rysunek 2**: Utwórz nowy o nazwie elementu ObjectDataSource `SuppliersDataSource` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](performing-batch-updates-vb/_static/image6.png))


Konfigurowanie kontrolki ObjectDataSource do pobierania danych przy użyciu `SuppliersBLL` klasy s `GetSuppliers()` — metoda (zobacz rysunek 3). Podobnie jak w poprzednim samouczku, zamiast aktualizowania informacji o dostawcy za pomocą kontrolki ObjectDataSource, firma Microsoft będzie pracować bezpośrednio warstwy logiki biznesowej. W związku z tym, zmień wartość na liście rozwijanej (Brak) na karcie aktualizacji (zobacz rysunek 4).


[![Pobieranie informacji o dostawcy przy użyciu metody GetSuppliers()](performing-batch-updates-vb/_static/image8.png)](performing-batch-updates-vb/_static/image7.png)

**Rysunek 3**: pobieranie dostawcy informacje przy użyciu `GetSuppliers()` — metoda ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](performing-batch-updates-vb/_static/image9.png))


[![Zmień wartość na liście rozwijanej na (Brak) na karcie aktualizacji](performing-batch-updates-vb/_static/image11.png)](performing-batch-updates-vb/_static/image10.png)

**Rysunek 4**: Zmień wartość na liście rozwijanej na (Brak) na karcie aktualizacji ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](performing-batch-updates-vb/_static/image12.png))


Po zakończeniu działania kreatora programu Visual Studio automatycznie generuje DataList s `ItemTemplate` do wyświetlenia każdego pola danych zwróconych przez źródło danych w kontrolce etykiety w sieci Web. Należy zmodyfikować tego szablonu, tak, aby zamiast tego zapewnia interfejs edytowania. `ItemTemplate` Można dostosować za pomocą projektanta za pomocą opcji Edytuj szablony z tagu inteligentnego DataList s lub bezpośrednio za pomocą składni deklaratywnej.

Poświęć chwilę, aby utworzyć interfejs edycji, który zawiera nazwę dostawcy s jako tekst, ale zawiera pola tekstowe dla dostawcy s adres, miasto i wartości kraju. Po wprowadzeniu tych zmian, strona składni deklaratywnej s powinien wyglądać podobnie do poniższej:


[!code-aspx[Main](performing-batch-updates-vb/samples/sample1.aspx)]

> [!NOTE]
> Jak w poprzednim samouczku DataList w tym samouczku musi mieć swój stan widoku, włączone.


W `ItemTemplate` I m przy użyciu dwóch nowych klas CSS `SupplierPropertyLabel` i `SupplierPropertyValue`, zostały dodane do `Styles.css` klasy i skonfigurowany do korzystania z tych samych ustawień stylu `ProductPropertyLabel` i `ProductPropertyValue` klas CSS.


[!code-css[Main](performing-batch-updates-vb/samples/sample2.css)]

Po wprowadzeniu tych zmian, odwiedź tę stronę za pośrednictwem przeglądarki. Jak pokazano na rysunku 5, każdy element DataList Wyświetla nazwę dostawcy jako tekst i użyto pola tekstowe, aby wyświetlić adres, miasto i kraj.


[![Każdy dostawca w elemencie DataList jest edytowalna](performing-batch-updates-vb/_static/image14.png)](performing-batch-updates-vb/_static/image13.png)

**Rysunek 5**: każdy dostawca w elemencie DataList jest edytowalna ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](performing-batch-updates-vb/_static/image15.png))


## <a name="step-2-adding-an-update-all-button"></a>Krok 2: Dodawanie aktualizację wszystkich przycisku

Podczas każdego dostawcy na rysunku 5 zawiera jego adres, miasto i kraj pola wyświetlane w polu tekstowym, obecnie nie ma aktualizacji przycisku dostępne. Zamiast przycisku aktualizowania na element, za pomocą w pełni edytowalne DataLists przeważnie jest jeden przycisk Aktualizuj wszystkie na tej stronie, po kliknięciu, aktualizuje *wszystkie* rekordów w elemencie DataList. Na potrzeby tego samouczka należy zezwolić s dodaje dwa przyciski wszystkich aktualizacji — jeden w górnej części strony i jeden w dolnej części, (chociaż albo przycisk będzie miał ten sam efekt).

Rozpocznij od Dodawanie kontrolki przycisku w sieci Web powyżej DataList i ustaw jego `ID` właściwość `UpdateAll1`. Następnie dodać drugi formant przycisku w sieci Web pod DataList, ustawiając jego `ID` do `UpdateAll2`. Ustaw `Text` właściwości dla dwóch przycisków do wszystkich aktualizacji. Ponadto utworzyć procedury obsługi zdarzeń dla obu przycisków `Click` zdarzenia. A nie powiela logika aktualizacji we wszystkich procedur obsługi zdarzeń, umożliwiają s Refaktoryzuj tej logiki Trzecia metoda `UpdateAllSupplierAddresses`, posiadające procedury obsługi zdarzeń, po prostu wywoływanie tej metody trzeci.


[!code-vb[Main](performing-batch-updates-vb/samples/sample3.vb)]

Rysunek 6 przedstawia stronę po aktualizacji wszystkie przyciski zostały dodane.


[![Dwa przyciski wszystkich aktualizacji zostały dodane do strony](performing-batch-updates-vb/_static/image17.png)](performing-batch-updates-vb/_static/image16.png)

**Rysunek 6**: dwa przyciski wszystkich aktualizacji zostały dodane do strony ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](performing-batch-updates-vb/_static/image18.png))


## <a name="step-3-updating-all-of-the-suppliers-address-information"></a>Krok 3: Aktualizowanie wszystkie informacje o adresie dostawcy

Przy użyciu wszystkich elementów s DataList wyświetlania interfejsu edycji i dodając zaktualizować wszystkie przyciski wszystko to to, że pozostaje pisania kodu do przeprowadzenia aktualizacji usługi batch. W szczególności należy w pętli poprzez elementów DataList s i wywołania `SuppliersBLL` klasy s `UpdateSupplierAddress` metody dla każdego z nich.

Kolekcja `DataListItem` wystąpień tego korzeń kontrolki DataList można uzyskać dostęp za pomocą kontrolek DataList s [ `Items` właściwość](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalist.items.aspx). Z odwołaniem do `DataListItem`, firma Microsoft może Pobierz odpowiedni `SupplierID` z `DataKeys` zbierania i programowo odwołania sieci Web pola tekstowego kontrolki w ramach `ItemTemplate` tak jak pokazano w poniższym kodzie:


[!code-vb[Main](performing-batch-updates-vb/samples/sample4.vb)]

Gdy użytkownik kliknie jeden z przycisków Aktualizuj wszystkie `UpdateAllSupplierAddresses` metoda iteruje przez każdy `DataListItem` w `Suppliers` DataList i wywołania `SuppliersBLL` klasy s `UpdateSupplierAddress` metody, przekazując odpowiednie wartości. Wartość — wprowadzono adres, miasta lub kraju przebiegów jest wartością `Nothing` do `UpdateSupplierAddress` (zamiast pustego ciągu), co powoduje w bazie danych `NULL` dla pól rekordu s podstawowych.

> [!NOTE]
> Jako rozszerzenie można dodać stan formantu etykiety w sieci Web do strony, który zawiera komunikat z potwierdzeniem po wykonaniu aktualizacji usługi batch.


## <a name="updating-only-those-addresses-that-have-been-modified"></a>Aktualizowanie tych adresów, które zostały zmodyfikowane

Algorytm aktualizacji usługi batch, używany dla tego samouczka wywołań `UpdateSupplierAddress` metodę *co* dostawcy w elemencie DataList, niezależnie od tego, czy ich informacje o adresie został zmieniony. Chociaż takie blind aktualizacje nie są zazwyczaj problem z wydajnością, której jest przeprowadzana inspekcja zmiany do tabeli bazy danych może prowadzić do zbędny rekordów. Na przykład, jeśli używasz wyzwalacze do rejestrowania wszystkich `UPDATE` s `Suppliers` do tabeli inspekcji, za każdym razem, gdy użytkownik kliknie przycisk Aktualizuj wszystkie, zostanie utworzony nowy rekord inspekcji dla każdego dostawcy w systemie, niezależnie od tego, czy użytkownik wprowadzone zmiany.

Klasy ADO.NET DataTable i DataAdapter są przeznaczone do obsługi aktualizacji wsadowych, których wyniki tylko zmodyfikowanych, usuniętych i nowych rekordów w jakiejkolwiek korespondencji bazy danych. Każdy wiersz w tabeli DataTable ma [ `RowState` właściwość](https://msdn.microsoft.com/library/system.data.datarow.rowstate.aspx) wskazuje, czy wiersz został dodany do elementu DataTable, usunąć z niego zmodyfikowany, czy pozostaje bez zmian. Gdy DataTable początkowo jest wypełniony, wszystkie wiersze są oznaczone bez zmian. Zmiana wartości kolumny wiersza s oznacza wiersza, jako zmodyfikowane.

W `SuppliersBLL` klasy, firma Microsoft aktualizuje informacje o adresie określonego dostawcy s przez wczytanie pierwszego rekordu jednego dostawcy do `SuppliersDataTable` , a następnie ustaw `Address`, `City`, i `Country` wartości kolumn, używając następującego kodu:


[!code-vb[Main](performing-batch-updates-vb/samples/sample5.vb)]

Ten kod przypisuje naively przekazany adres, miasto i wartości kraju `SuppliersRow` w `SuppliersDataTable` niezależnie od tego, czy wartości zostały zmienione. Zmiany powodują `SuppliersRow` s `RowState` właściwość oznaczona jako zmodyfikowana. Gdy s warstwy dostępu do danych `Update` metoda jest wywoływana, widzi, że `SupplierRow` została zmodyfikowana i w związku z tym wysyła `UPDATE` polecenia w bazie danych.

Wyobraź sobie, że dodaliśmy kod do tej metody, aby tylko przypisać przekazany w adres, miasto i wartości kraju, jeśli będą się różnić od `SuppliersRow` s istniejące wartości. W przypadku, gdy adres, miasto i kraj są takie same jak w przypadku istniejących danych, nie zostaną wprowadzone nie zmiany i `SupplierRow` s `RowState` zostanie oznaczona jako niezmienionym. Wynikiem jest to, że gdy DAL s `Update` metoda jest wywoływana, wywołanie bazy danych, nie zostanie nawiązane, ponieważ `SuppliersRow` nie został zmodyfikowany.

Wprowadzenie tej zmiany, Zastąp instrukcji, które ślepo przypisać przekazany adres, miasto i wartości kraju z następującym kodem:


[!code-vb[Main](performing-batch-updates-vb/samples/sample6.vb)]

Dzięki temu dodano kod, DAL s `Update` metoda wysyła `UPDATE` instrukcji do bazy danych tylko te rekordy, w których wartości związanych z adresem zostały zmienione.

Alternatywnie można czy ma żadnych różnic pola adresu w przekazanym i danych w bazie danych i, jeśli brak to none, po prostu pominąć wywołanie DAL s `Update` metody. To podejście sprawdza się dobrze, jeśli jest to metoda, której jest przeprowadzana przy użyciu bazy danych bezpośrednia ponieważ przekazywany t nie jest metody bezpośredniej DB `SuppliersRow` wystąpienia, którego `RowState` może zostać sprawdzone w celu ustalenia, czy wywołanie bazy danych jest faktycznie potrzebny.

> [!NOTE]
> Każdorazowo `UpdateSupplierAddress` metoda jest wywoływana, wykonywane jest wywołanie do bazy danych, aby pobrać informacje o zaktualizowanym rekordem. Następnie w przypadku zmiany w danych, inne połączenie z bazą danych wykonano można zaktualizować wiersza tabeli. Ten przepływ pracy może być zoptymalizowany, tworząc `UpdateSupplierAddress` przeciążenia metody, która akceptuje `EmployeesDataTable` wystąpienia, która ma *wszystkich* zmian z `BatchUpdate.aspx` strony. Następnie można wprowadzić jedno wywołanie do bazy danych do wszystkich rekordów z `Suppliers` tabeli. Następnie można wyliczyć dwa zestawy wyników, i można go zaktualizować tylko te rekordy, w których nastąpiły zmiany.


## <a name="summary"></a>Podsumowanie

W tym samouczku widzieliśmy sposób tworzenia DataList pełni edytowalne, pozwalając użytkownikom na szybko zmodyfikować informacje o adresie wielu dostawców. Rozpoczęliśmy przez definiowanie interfejsu edycji kontrolki TextBox w sieci Web dostawcy s adres, miasto i wartości kraju w elemencie DataList s `ItemTemplate`. Następnie dodaliśmy Aktualizuj wszystkie przyciski powyżej i poniżej kontrolki DataList. Po utworzeniu użytkownika ma swoje zmiany i kliknięciu jeden z przycisków Aktualizuj wszystkie `DataListItem` s są wyliczane i wywołania `SuppliersBLL` klasy s `UpdateSupplierAddress` wykonano metody.

Wszystkiego najlepszego programowania!

## <a name="about-the-author"></a>Informacje o autorze

[Scott Bento](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor siedem ASP/ASP.NET książek i założycielem [4GuysFromRolla.com](http://www.4guysfromrolla.com), pracował nad przy użyciu technologii Microsoft Web od 1998 r. Scott działa jako niezależny Konsultant, trainer i składnika zapisywania. Jego najnowszą książkę Stephena [ *Sams uczyć się ASP.NET 2.0 w ciągu 24 godzin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). ADAM można z Tobą skontaktować w [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) lub za pośrednictwem jego blogu, który znajduje się w temacie [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Specjalne podziękowania dla

W tej serii samouczków został zrecenzowany przez wielu recenzentów pomocne. Wiodące osób dokonujących przeglądu, w tym samouczku zostały Zack Jones i Krzysztof Pespisa. Zainteresowani zapoznaniem Moje kolejnych artykułów MSDN? Jeśli tak, Porzuć mnie linii w [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Poprzednie](an-overview-of-editing-and-deleting-data-in-the-datalist-vb.md)
> [dalej](handling-bll-and-dal-level-exceptions-vb.md)
