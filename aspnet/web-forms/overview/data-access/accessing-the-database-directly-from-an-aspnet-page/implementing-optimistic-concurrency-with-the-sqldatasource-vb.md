---
uid: web-forms/overview/data-access/accessing-the-database-directly-from-an-aspnet-page/implementing-optimistic-concurrency-with-the-sqldatasource-vb
title: "Implementowanie optymistycznej współbieżności z SqlDataSource (VB) | Dokumentacja firmy Microsoft"
author: rick-anderson
description: "W tym samouczku możemy Przejrzyj podstawowe informacje o optymistyczne sterowanie współbieżnością, a następnie eksplorować implementowania za pomocą formantu SqlDataSource."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2007
ms.topic: article
ms.assetid: a8fa72ee-8328-4854-a419-c1b271772303
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/accessing-the-database-directly-from-an-aspnet-page/implementing-optimistic-concurrency-with-the-sqldatasource-vb
msc.type: authoredcontent
ms.openlocfilehash: 974ea50a0d12aae09107470815214b20068ea553
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/24/2018
---
<a name="implementing-optimistic-concurrency-with-the-sqldatasource-vb"></a>Implementowanie optymistycznej współbieżności z SqlDataSource (VB)
====================
przez [Bento Scott](https://twitter.com/ScottOnWriting)

[Pobierz przykładową aplikację](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_50_VB.exe) lub [pobierania plików PDF](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/datatutorial50vb1.pdf)

> W tym samouczku możemy Przejrzyj podstawowe informacje o optymistyczne sterowanie współbieżnością, a następnie eksplorować implementowania za pomocą formantu SqlDataSource.


## <a name="introduction"></a>Wprowadzenie

W poprzednim samouczek możemy zbadać sposób dodawania Wstawianie, aktualizowanie i usuwanie funkcji w celu sterowania SqlDataSource. Krótko mówiąc, aby zapewnić te funkcje musimy określ odpowiednie `INSERT`, `UPDATE`, lub `DELETE` instrukcji SQL w formancie s `InsertCommand`, `UpdateCommand`, lub `DeleteCommand` właściwości, oraz odpowiedni Parametry w `InsertParameters`, `UpdateParameters`, i `DeleteParameters` kolekcji. Podczas tych właściwości i kolekcji można określić ręcznie, skonfiguruj źródło danych Kreatora s przycisk Zaawansowane oferuje Generuj `INSERT`, `UPDATE`, i `DELETE` na podstawie wyboru instrukcje, która spowoduje automatyczne tworzenie tych instrukcji `SELECT` instrukcji.

Wraz z Generuj `INSERT`, `UPDATE`, i `DELETE` instrukcje wyboru, okno dialogowe Opcje zaawansowane generowania SQL obejmuje użycie opcji optymistycznej współbieżności (zobacz rysunek 1). Po zaznaczeniu tej opcji, `WHERE` klauzule wygenerowana automatycznie `UPDATE` i `DELETE` instrukcje są modyfikowane do wykonywania tylko aktualizacji lub usuwania Jeśli podstawowej t nie zostały danych bazy danych został zmodyfikowany od czasu użytkownika ostatniego załadowania danych w siatce.


![Można dodać obsługę optymistycznej współbieżności z zaawansowanych generowanie kodu SQL, opcje — Okno dialogowe](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image1.gif)

**Rysunek 1**: z zaawansowanych można dodać obsługę optymistycznej współbieżności generowanie kodu SQL, opcje — Okno dialogowe


W [implementacja optymistycznej współbieżności](../editing-inserting-and-deleting-data/implementing-optimistic-concurrency-vb.md) samouczek możemy zbadać podstawy optymistyczne sterowanie współbieżnością i jak dodać go do elementu ObjectDataSource. W tym samouczku będziemy retuszowanie na podstawowe informacje o optymistyczne sterowanie współbieżnością i następnie eksplorować je przy użyciu SqlDataSource implementowania.

## <a name="a-recap-of-optimistic-concurrency"></a>Drużyn optymistycznej współbieżności

Dla aplikacji sieci web, które pozwalają wielu równoczesnych użytkowników, aby edytować lub usunąć tych samych danych, istnieje możliwość, że jeden użytkownik przypadkowo może zastąpić innego zmiany s. W [implementacja optymistycznej współbieżności](../editing-inserting-and-deleting-data/implementing-optimistic-concurrency-vb.md) samouczka I podane w poniższym przykładzie:

Załóżmy, że dwóch użytkowników, Jisun i Sam, zostały zarówno odwiedzania strony w aplikacji, która dozwolone osoby odwiedzające aktualizować i usuwać produktów za pomocą kontrolki widoku siatki. Zarówno kliknij przycisk Edytuj dla Chai zbliżonym do czasu. Jisun zmiany nazwy produktu Zepołowy Chai i klika przycisk Aktualizuj. Wynikiem jest `UPDATE` instrukcji, która jest wysyłane do bazy danych, która ustawia *wszystkie* pól można aktualizować produktu s (nawet jeśli Jisun aktualizowana tylko jedno pole `ProductName`). W tym momencie baza danych ma wartości Zepołowy Chai, kategoria Napoje cieczy egzotycznych dostawcy i tak dalej dla tego konkretnego produktu. Jednak GridView na ekranie s Sam nadal zawiera nazwę produktu w można edytować wiersza elementu GridView jako Chai. Kilka sekund po Jisun s zmiany zostały przekazane, Sam aktualizacje kategorii przyprawy i klika aktualizacji. Powoduje to `UPDATE` instrukcji wysyłane do bazy danych, która ustawia nazwę produktu Chai, `CategoryID` do odpowiednich przyprawy identyfikator kategorii i tak dalej. Zostały zastąpione Jisun s zmiany nazwy produktu.

Rysunek 2 przedstawia to interakcji.


[![Gdy dwóch użytkowników jednocześnie zaktualizowania rekordu, potencjalnych s s jeden użytkownik zmienia się na zastąpienie innych s](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image2.gif)](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image1.png)

**Rysunek 2**: gdy dwóch użytkowników jednocześnie aktualizacja istnieje rekord s potencjalne dla jednego użytkownika s zmiany Zastąp innych s ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image2.png))


Aby zapobiec unfolding formularza w tym scenariuszu [sterowania współbieżnością](http://en.wikipedia.org/wiki/Concurrency_control) musi zostać wdrożone. [Optymistycznej współbieżności](http://en.wikipedia.org/wiki/Optimistic_concurrency_control) fokus w tym samouczku działa przy założeniu, że może być konfliktom współbieżności i, gdy przeważająca większość czasu takie konflikty won t wynikają. W związku z tym, w przypadku konfliktu optymistyczne sterowanie współbieżnością po prostu informuje użytkownika zapisania ich zmiany mogą t, ponieważ inny użytkownik zmodyfikował tych samych danych.

> [!NOTE]
> W przypadku aplikacji, w którym zakłada się, że będzie wiele konfliktom współbieżności lub jeśli takie konflikty nie są dopuszczalna następnie sterowania współbieżnością pesymistyczne można zamiast tego. Odwołaj się do [implementacja optymistycznej współbieżności](../editing-inserting-and-deleting-data/implementing-optimistic-concurrency-vb.md) samouczka bardziej szczegółowe omówienie współbieżności pesymistyczne formantu.


Optymistyczne sterowanie współbieżnością działania przez zapewnienie im rekordu są zaktualizowane lub usunięte ma takie same wartości, jak podczas aktualizowania lub usuwania procesu uruchamiania. Na przykład po kliknięciu przycisku edycji w GridView edytowalny, wartości rekordu s są do odczytu z bazy danych i wyświetlane w pola tekstowe i inne formanty sieci Web. Te oryginalne wartości są zapisywane w widoku GridView. Następnie po użytkownik wprowadza swoje zmiany i kliknięciu przycisku Aktualizuj `UPDATE` instrukcją użytą w należy wziąć pod uwagę oryginalnych wartości oraz nowych wartości i aktualizować tylko podstawowy rekordów bazy danych w przypadku oryginalnej wartości, że użytkownik rozpoczął edycję są takie same jak wartości nadal w bazie danych. Rysunek 3 przedstawia następująca sekwencja zdarzeń.


[![Dla aktualizacji lub usunięcia, które było poprawne oryginalnych wartości musi być równa wartości bieżącej bazy danych](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image3.gif)](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image3.png)

**Rysunek 3**: For Update lub Delete pomyślnie oryginalnej wartości musi być równa wartości bieżącej bazy danych ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image4.png))


Istnieją różne podejścia do wdrażania optymistycznej współbieżności (zobacz [Bromberg A. Peterowi](http://peterbromberg.net/) s [logikę aktualizowania współbieżności Optmistic](http://www.eggheadcafe.com/articles/20050719.asp) dla krótki opis wiele opcji). Wspomaga zastosowanych SqlDataSource (oraz ADO.NET wpisanych zestawów danych użytych w naszym Warstwa dostępu do danych) `WHERE` klauzulę porównanie wszystkich oryginalnych wartości. Następujące `UPDATE` instrukcji, na przykład aktualizacje nazwy i cena produktu tylko wtedy, gdy bieżące wartości bazy danych są takie same wartości, które pierwotnie zostały pobrane podczas aktualizacji rekordu w widoku GridView. `@ProductName` i `@UnitPrice` parametrów zawiera nowe wartości wprowadzonej przez użytkownika, podczas gdy `@original_ProductName` i `@original_UnitPrice` zawierają wartości, które pierwotnie zostały załadowane w widoku GridView, gdy kliknięto przycisk Edytuj:


[!code-sql[Main](implementing-optimistic-concurrency-with-the-sqldatasource-vb/samples/sample1.sql)]

Jak zajmiemy się tym, w tym samouczku, włączanie optymistyczne sterowanie współbieżnością z SqlDataSource jest tak proste, jak sprawdzanie pola wyboru.

## <a name="step-1-creating-a-sqldatasource-that-supports-optimistic-concurrency"></a>Krok 1: Tworzenie SqlDataSource obsługującego optymistycznej współbieżności

Uruchamianie przez otwarcie `OptimisticConcurrency.aspx` strony z `SqlDataSource` folderu. Przeciągnij z przybornika do projektanta, ustawienia kontroli SqlDataSource jego `ID` właściwości `ProductsDataSourceWithOptimisticConcurrency`. Następnie kliknij łącze Konfigurowanie źródła danych z tagów inteligentnych s formantu. Na pierwszym ekranie kreatora wybierz do pracy z `NORTHWINDConnectionString` i kliknij przycisk Dalej.


[![Wybierz do pracy z NORTHWINDConnectionString](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image4.gif)](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image5.png)

**Rysunek 4**: Wybierz pracować z `NORTHWINDConnectionString` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image6.png))


W tym przykładzie, które będą dodawane GridView, która umożliwia użytkownikom edytowanie `Products` tabeli. W związku z tym z konfiguracji ekranu instrukcji Select wybierz `Products` tabeli z listy rozwijanej i wybierz `ProductID`, `ProductName`, `UnitPrice`, i `Discontinued` kolumny, jak pokazano na rysunku 5.


[![Z tabeli Produkty zwrócić ProductID, ProductName, UnitPrice i wycofane kolumn](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image5.gif)](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image7.png)

**Rysunek 5**: Z `Products` tabeli, aby uzyskać `ProductID`, `ProductName`, `UnitPrice`, i `Discontinued` kolumn ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image8.png))


Po pobraniu kolumny, kliknij przycisk Zaawansowane, aby wyświetlić okno dialogowe Opcje zaawansowane generowania SQL. Sprawdź generowanie `INSERT`, `UPDATE`, i `DELETE` instrukcje i użyj pól wyboru optymistycznej współbieżności i kliknij przycisk OK (odwołują się do rysunku 1 dla zrzut ekranu). Ukończ pracę kreatora, klikając przycisk Dalej, a następnie Zakończ.

Po zakończeniu pracy Kreatora konfigurowania źródła danych, Poświęć chwilę, aby zbadać powstałe w ten sposób `DeleteCommand` i `UpdateCommand` właściwości i `DeleteParameters` i `UpdateParameters` kolekcji. W tym celu najłatwiej kliknij w lewym dolnym rogu, aby wyświetlić stronę składni deklaratywnej s na karcie Źródło. Można znaleźć `UpdateCommand` wartość:


[!code-sql[Main](implementing-optimistic-concurrency-with-the-sqldatasource-vb/samples/sample2.sql)]

Z parametrami siedmiu w `UpdateParameters` kolekcji:


[!code-aspx[Main](implementing-optimistic-concurrency-with-the-sqldatasource-vb/samples/sample3.aspx)]

Podobnie `DeleteCommand` właściwości i `DeleteParameters` kolekcji powinna wyglądać następująco:


[!code-sql[Main](implementing-optimistic-concurrency-with-the-sqldatasource-vb/samples/sample4.sql)]


[!code-aspx[Main](implementing-optimistic-concurrency-with-the-sqldatasource-vb/samples/sample5.aspx)]

Oprócz rozbudować `WHERE` klauzul `UpdateCommand` i `DeleteCommand` właściwości (i dodanie dodatkowych parametrów do kolekcji odpowiednich parametrów), wybierając optymistycznej współbieżności opcja dostosowuje dwie inne użycia Właściwości:

- Zmiany [ `ConflictDetection` właściwości](https://msdn.microsoft.com/library/system.web.ui.webcontrols.sqldatasource.conflictdetection.aspx) z `OverwriteChanges` (ustawienie domyślne) do`CompareAllValues`
- Zmiany [ `OldValuesParameterFormatString` właściwości](https://msdn.microsoft.com/library/system.web.ui.webcontrols.sqldatasource.oldvaluesparameterformatstring.aspx) z {0} (ustawienie domyślne) z oryginalną\_{0}.

Gdy dane kontrolka sieci Web wywołuje SqlDataSource s `Update()` lub `Delete()` metody przekazaniem w oryginalnych wartości. Jeśli SqlDataSource s `ConflictDetection` właściwość jest ustawiona na `CompareAllValues`, te oryginalne wartości są dodawane do polecenia. `OldValuesParameterFormatString` Dostarcza wzorzec nazewnictwa używane dla oryginalnej wartości parametrów. Kreator konfigurowania źródła danych przy użyciu oryginalnego\_{0} oraz nazwy każdego parametru oryginalnego `UpdateCommand` i `DeleteCommand` właściwości i `UpdateParameters` i `DeleteParameters` kolekcji odpowiednio.

> [!NOTE]
> Ponieważ firma Microsoft re nie używa s kontroli SqlDataSource Wstawianie możliwości, możesz usunąć `InsertCommand` właściwość i jej `InsertParameters` kolekcji.


## <a name="correctly-handlingnullvalues"></a>Obsługa poprawnie`NULL`wartości

Niestety, zwiększonej `UPDATE` i `DELETE` czy automatycznie instrukcje wygenerowana przez Kreatora konfigurowania źródła danych, używając optymistycznej współbieżności *nie* pracować z rekordów, które zawierają `NULL` wartości. Aby zobaczyć przyczynę, należy wziąć pod uwagę naszych s SqlDataSource `UpdateCommand`:


[!code-sql[Main](implementing-optimistic-concurrency-with-the-sqldatasource-vb/samples/sample6.sql)]

`UnitPrice` Kolumny w `Products` tabela może mieć `NULL` wartości. Jeśli został określony rekord `NULL` wartość `UnitPrice`, `WHERE` części klauzuli `[UnitPrice] = @original_UnitPrice` będzie *zawsze* obliczyć wartość false, ponieważ `NULL = NULL` zawsze zwraca wartość False. W związku z tym rekordy zawierających `NULL` wartości nie można edytować ani usunąć, jako `UPDATE` i `DELETE` instrukcje `WHERE` klauzule won powrotu t wszystkie wiersze, aby zaktualizować lub usunąć.

> [!NOTE]
> Ta usterka najpierw zostało zgłoszone do firmy Microsoft w czerwca 2004 w [SqlDataSource generuje niepoprawne instrukcji SQL](https://connect.microsoft.com/VisualStudio/feedback/ViewFeedback.aspx?FeedbackID=93937) i powinno zostanie rozwiązany w następnej wersji platformy ASP.NET.


Aby rozwiązać ten problem, konieczne jest ręczne uaktualnienie `WHERE` klauzule zarówno `UpdateCommand` i `DeleteCommand` właściwości **wszystkie** kolumn, które mogą zostać `NULL` wartości. Ogólnie rzecz biorąc, zmień `[ColumnName] = @original_ColumnName` do:


[!code-sql[Main](implementing-optimistic-concurrency-with-the-sqldatasource-vb/samples/sample7.sql)]

Ta modyfikacja może się bezpośrednio za pomocą deklaratywne znaczników, za pomocą opcji UpdateQuery lub DeleteQuery z okna właściwości lub aktualizacji i usuwanie kart Określ niestandardową instrukcję SQL lub procedurę składowaną opcję Konfigurowanie danych Kreator źródła. Ponownie, należy przewidzieć modyfikacji *co* kolumny w `UpdateCommand` i `DeleteCommand` s `WHERE` klauzuli, która może zawierać `NULL` wartości.

Powoduje to zastosowanie do naszym przykładzie zmodyfikować następujące `UpdateCommand` i `DeleteCommand` wartości:


[!code-sql[Main](implementing-optimistic-concurrency-with-the-sqldatasource-vb/samples/sample8.sql)]

## <a name="step-2-adding-a-gridview-with-edit-and-delete-options"></a>Krok 2: Dodawanie Element GridView z właściwością edytowania i usuwania opcji

Z SqlDataSource skonfigurowany do obsługi optymistycznej współbieżności czy pozostaje jest dodanie danych formantu sieci Web do strony, która korzysta z tego formantu współbieżności. W tym samouczku umożliwiają s dodawania widoku GridView, zapewniająca zarówno edycji i usuwania funkcji. Aby to zrobić, przeciągnij element GridView z przybornika do projektanta i zestaw jej `ID` do `Products`. Z tagów inteligentnych s GridView powiązać `ProductsDataSourceWithOptimisticConcurrency` kontroli SqlDataSource dodany w kroku 1. Ponadto sprawdź opcje Włącz edytowanie i usuwanie Włącz z tagów inteligentnych.


[![Powiązać widoku GridView SqlDataSource i Włącz edytowanie i usuwanie](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image6.gif)](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image9.png)

**Rysunek 6**: powiązać widoku GridView SqlDataSource i Włącz edytowanie i usuwanie ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image10.png))


Po dodaniu widoku GridView, skonfiguruj jego wygląd przez usunięcie `ProductID` elementu BoundField, zmiana `ProductName` s elementu BoundField `HeaderText` właściwości produktu i aktualizowanie `UnitPrice` elementu BoundField, aby jego `HeaderText` jest właściwość po prostu cena. W idealnym przypadku d zwiększania edycji interfejsu, który ma zawierać RequiredFieldValidator dla `ProductName` wartość i CompareValidator dla `UnitPrice` wartość (aby upewnić się, że jest poprawnie sformatowana wartość liczbowa s). Zapoznaj się [Dostosowywanie interfejs modyfikacji danych](../editing-inserting-and-deleting-data/customizing-the-data-modification-interface-vb.md) samouczka na pełniejsze przyjrzeć się Dostosowywanie s GridView edycji interfejsu.

> [!NOTE]
> GridView, który musi być włączony stan widoku s, ponieważ oryginalne wartości przekazane z widoku GridView do SqlDataSource są przechowywane w widoku stanu.


Po wprowadzeniu tych zmian do widoku GridView, znaczników deklaratywne GridView i SqlDataSource powinien wyglądać podobny do następującego:


[!code-aspx[Main](implementing-optimistic-concurrency-with-the-sqldatasource-vb/samples/sample9.aspx)]

Aby zobaczyć kontrolki optymistycznej współbieżności na akcję, Otwórz dwa okna przeglądarki i załadować `OptimisticConcurrency.aspx` strony w obu. Kliknij przyciski edycji dla pierwszego produktu w obu przeglądarkach. W jednej przeglądarki Zmień nazwę produktu, a następnie kliknij przycisk Aktualizuj. Przeglądarka będzie ogłaszanie i widoku GridView powróci do trybu edycji wstępnie przedstawiający nową nazwę produktu dla rekordu właśnie edytowany.

W oknie przeglądarki drugi zmiany ceny (ale pozostaw nazwę produktu jako wartości oryginalnej), a następnie kliknij przycisk Aktualizuj. Odświeżania strony siatki powróci do trybu edycji wstępnie, ale zmiany ceny nie została zarejestrowana. Drugi przeglądarki zawiera tę samą wartość jak pierwszy nazwę produktu z stara cena. Zmiany wprowadzone w oknie przeglądarki drugi zostały utracone. Ponadto zmiany zostały utracone raczej ciche, ponieważ nie było żadnych wyjątków lub komunikat wskazujący, czy naruszenie współbieżności właśnie wystąpił.


[![Zmiany w oknie przeglądarki drugi zostały utracone w trybie dyskretnym](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image7.gif)](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image11.png)

**Rysunek 7**: zmiany w drugim przeglądarki okna zostały dyskretnie utraty ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image12.png))


Został przyczyny, dlaczego drugi zmiany przeglądarce s nie zostały zatwierdzone, ponieważ `UPDATE` instrukcja s `WHERE` klauzul odfiltrowane wszystkie rekordy i w związku z tym, nie wpływa na żadne wiersze. Let s przyjrzeć się `UPDATE` ponownie instrukcji:


[!code-sql[Main](implementing-optimistic-concurrency-with-the-sqldatasource-vb/samples/sample10.sql)]

Po drugie okno przeglądarki aktualizacji rekordu, oryginalną nazwę produktu określone w `WHERE` klauzuli są zgodne się z istniejącą nazwą produktu (ponieważ został zmieniony przez przeglądarkę pierwszy). W związku z tym instrukcji `[ProductName] = @original_ProductName` zwraca wartość False, a `UPDATE` nie ma wpływu na rekordy.

> [!NOTE]
> Usuń działa w taki sam sposób. Dwa okna przeglądarki Otwórz rozpocząć edycji danego produktu z jednym, a następnie zapisz jego zmiany. Po zapisaniu zmian w jednej przeglądarki, kliknij przycisk Usuń dla tego samego produktu w innym. Ponieważ oryginalne ADAM wartości t są zgodne w `DELETE` instrukcja s `WHERE` klauzuli delete dyskretnie nie powiedzie się.


Z perspektywy użytkownika końcowego s w drugim oknie przeglądarki po kliknięciu przycisku Aktualizuj siatki powraca do trybu edycji wstępnie, ale ich zmiany zostały utracone. Jednak tam s nie wizualny, który trzymaj t ich zmiany. Najlepiej Jeśli zmiany użytkowników s zostaną utracone na naruszenie współbieżności, możemy d powiadamiać użytkowników i czasem zachować siatki w trybie edycji. Let s, zobacz, jak to zrobić.

## <a name="step-3-determining-when-a-concurrency-violation-has-occurred"></a>Krok 3: Określanie wystąpienia Naruszenie współbieżności

Ponieważ Naruszenie współbieżności odrzuci jedną wprowadził zmiany, byłoby nieuprzywilejowany alertu użytkownika, jeśli nastąpiło naruszenie współbieżności. Aby ostrzec użytkownika, umożliwiają s Dodawanie formantu etykiety w sieci Web do górnej części strony o nazwie `ConcurrencyViolationMessage` których `Text` właściwości wyświetli następujący komunikat: próbowano zaktualizować lub usunąć rekord, który jednocześnie został zaktualizowany przez innego użytkownika. Należy Przejrzyj zmiany wprowadzone przez użytkownika, a następnie wykonaj ponownie aktualizację lub Usuń. Ustawianie formantu etykiety s `CssClass` właściwości na ostrzeżenie, czyli klasę CSS zdefiniowanych w `Styles.css` wyświetlającym tekst w czcionki czerwony, kursywa bold i duże. Wreszcie, ustaw etykiety s `Visible` i `EnableViewState` właściwości `False`. Spowoduje to ukrycie etykiety z wyjątkiem tylko ogłaszania, te zwrotnego którym możemy jawnie ustawić jej `Visible` właściwości `True`.


[![Dodawanie formantu etykiety do strony, aby wyświetlić ostrzeżenia](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image8.gif)](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image13.png)

**Rysunek 8**: Dodawanie formantu etykiety do strony, aby wyświetlić ostrzeżenia ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image14.png))


Podczas przeprowadzania aktualizacji lub usuwania, GridView s `RowUpdated` i `RowDeleted` uruchomienie procedury obsługi zdarzeń po jego kontroli źródła danych wykonał żądanej update lub delete. Można określić liczbę wierszy objętych operacji te programy obsługi zdarzeń. Jeśli zero wierszy została zmieniona, chcemy wyświetlić `ConcurrencyViolationMessage` etykiety.

Tworzenie procedury obsługi zdarzeń dla obu `RowUpdated` i `RowDeleted` zdarzeń i Dodaj następujący kod:


[!code-vb[Main](implementing-optimistic-concurrency-with-the-sqldatasource-vb/samples/sample11.vb)]

W obu programów obsługi zdarzeń sprawdzamy `e.AffectedRows` właściwości i, jeśli ma on wartość równą 0, ustaw `ConcurrencyViolationMessage` etykiety s `Visible` właściwości `True`. W `RowUpdated` program obsługi zdarzeń, możemy także poinstruować GridView pozostanie w trybie edycji przez ustawienie `KeepInEditMode` właściwości na wartość true. W ten sposób, musimy ponownie powiązać dane do siatki, dzięki czemu inne dane użytkownika s jest ładowany do edycji interfejsu. Jest to osiągane przez wywołanie GridView s `DataBind()` metody.

Jak pokazano na rysunku nr 9, z tych dwóch zdarzenia, zostanie wyświetlony komunikat bardzo istotne przy każdym wystąpieniu Naruszenie współbieżności.


[![Komunikat jest wyświetlany w wypadku Naruszenie współbieżności](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image9.gif)](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image15.png)

**Rysunek 9**: komunikat jest wyświetlany w wypadku Naruszenie współbieżności ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](implementing-optimistic-concurrency-with-the-sqldatasource-vb/_static/image16.png))


## <a name="summary"></a>Podsumowanie

Podczas tworzenia aplikacji sieci web gdzie wiele równoczesnych użytkowników może edytować tych samych danych, należy rozważyć opcje sterowania współbieżności. Domyślnie dane programu ASP.NET, które kontroluje sieci Web i kontrolki źródła danych nie stosować żadnego formantu współbieżności. Jak widzieliśmy w tym samouczku, implementacja optymistyczne sterowanie współbieżnością z SqlDataSource jest stosunkowo szybkie i łatwe. SqlDataSource obsługuje większość legwork dla Twojego Dodawanie rozszerzony `WHERE` klauzule wygenerowana automatycznie `UPDATE` i `DELETE` instrukcji, ale są kilka precyzyjnie w obsłudze `NULL` wartości kolumn, zgodnie z opisem w Obsługa poprawnie `NULL` wartości sekcji.

Ten samouczek zawiera naszych badania SqlDataSource. Naszymi samouczkami, pozostałe powróci do pracy z danymi przy użyciu elementu ObjectDataSource i architektura warstwowego.

Programowanie przyjemność!

## <a name="about-the-author"></a>Informacje o autorze

[Scott Bento](http://www.4guysfromrolla.com/ScottMitchell.shtml), autora siedmiu książek ASP/ASP.NET i twórcę z [4GuysFromRolla.com](http://www.4guysfromrolla.com), pracuje z technologii Microsoft Web od 1998. Scott działa jako niezależnego konsultanta trainer i składnika zapisywania. Jest jego najnowszej książki [ *Sams nauczyć się ASP.NET 2.0 w ciągu 24 godzin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Piotr można uzyskać pod adresem [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) lub za pośrednictwem jego blog, który znajduje się w temacie [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

>[!div class="step-by-step"]
[Poprzednie](inserting-updating-and-deleting-data-with-the-sqldatasource-vb.md)
